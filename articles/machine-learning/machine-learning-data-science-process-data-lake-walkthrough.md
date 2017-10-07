---
title: "Škálovatelné vědecké zpracování dat pomocí Azure Data Lake: návod začátku do konce | Microsoft Docs"
description: "Jak toouse Azure Data Lake toodo data zkoumání a binární klasifikace úlohy na datovou sadu."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 91a8207f-1e57-4570-b7fc-7c5fa858ffeb
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: bradsev;weig
ms.openlocfilehash: 8b05457ae7045a7aaed350a7502469f2247161e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scalable-data-science-with-azure-data-lake-an-end-to-end-walkthrough"></a><span data-ttu-id="ac964-103">Škálovatelné vědecké zpracování dat pomocí Azure Data Lake: návod začátku do konce</span><span class="sxs-lookup"><span data-stu-id="ac964-103">Scalable Data Science with Azure Data Lake: An end-to-end Walkthrough</span></span>
<span data-ttu-id="ac964-104">Tento návod ukazuje, jak zkoumání dat toodo toouse Azure Data Lake a binární klasifikace úlohy na ukázku hello NYC taxi služební cestě a tarif toopredict datovou sadu, zda tip budeme platit tarif.</span><span class="sxs-lookup"><span data-stu-id="ac964-104">This walkthrough shows how toouse Azure Data Lake toodo data exploration and binary classification tasks on a sample of hello NYC taxi trip and fare dataset toopredict whether or not a tip will be paid by a fare.</span></span> <span data-ttu-id="ac964-105">Ho vás provede kroky hello hello [proces vědecké účely dat Team](http://aka.ms/datascienceprocess), klient server, z dat pořízení toomodel školení a toohello nasazení webové služby, který publikuje hello modelu.</span><span class="sxs-lookup"><span data-stu-id="ac964-105">It walks you through hello steps of hello [Team Data Science Process](http://aka.ms/datascienceprocess), end-to-end, from data acquisition toomodel training, and then toohello deployment of a web service that publishes hello model.</span></span>

### <a name="azure-data-lake-analytics"></a><span data-ttu-id="ac964-106">Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="ac964-106">Azure Data Lake Analytics</span></span>
<span data-ttu-id="ac964-107">Hello [Microsoft Azure Data Lake](https://azure.microsoft.com/solutions/data-lake/) má všechny požadované toomake možnosti hello snadno data vědců toostore data libovolné velikosti, tvar a rychlost a tooconduct zpracování dat, pokročilé analýzy a modelování machine learning s vysokou škálovatelnost nákladově efektivní způsobem.</span><span class="sxs-lookup"><span data-stu-id="ac964-107">hello [Microsoft Azure Data Lake](https://azure.microsoft.com/solutions/data-lake/) has all hello capabilities required toomake it easy for data scientists toostore data of any size, shape and speed, and tooconduct data processing, advanced analytics, and machine learning modeling with high scalability in a cost-effective way.</span></span>   <span data-ttu-id="ac964-108">Platíte na základě na úlohu jenom v případě, že data ve skutečnosti probíhá zpracování.</span><span class="sxs-lookup"><span data-stu-id="ac964-108">You pay on a per-job basis, only when data is actually being processed.</span></span> <span data-ttu-id="ac964-109">Azure Data Lake Analytics obsahuje U-SQL, jazyk, prolnutí hello deklarativní charakter jazyka SQL s hello výrazovou sílu jazyka C# tooprovide škálovatelných distribuovaných možnost dotazu.</span><span class="sxs-lookup"><span data-stu-id="ac964-109">Azure Data Lake Analytics includes U-SQL, a language that blends hello declarative nature of SQL with hello expressive power of C# tooprovide scalable distributed query capability.</span></span> <span data-ttu-id="ac964-110">Umožňuje vám tooprocess nestrukturovaných dat s použitím schématu na čtení, vložení vlastní logiky a uživatelsky definované funkce (UDF) a zahrnuje rozšíření tooenable jemné možnosti řízení jak tooexecute ve velkém měřítku.</span><span class="sxs-lookup"><span data-stu-id="ac964-110">It enables you tooprocess unstructured data by applying schema on read, insert custom logic and user defined functions (UDFs), and includes extensibility tooenable fine grained control over how tooexecute at scale.</span></span> <span data-ttu-id="ac964-111">toolearn Další informace o filosofie návrhu hello za U-SQL, najdete v části [Visual Studio příspěvku na blogu](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).</span><span class="sxs-lookup"><span data-stu-id="ac964-111">toolearn more about hello design philosophy behind U-SQL, see [Visual Studio blog post](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).</span></span>

<span data-ttu-id="ac964-112">Služba Data Lake Analytics je také klíčovou součástí Cortana Analytics Suite a spolupracuje se službami Azure SQL Data Warehouse, Power BI a Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ac964-112">Data Lake Analytics is also a key part of Cortana Analytics Suite and works with Azure SQL Data Warehouse, Power BI, and Data Factory.</span></span> <span data-ttu-id="ac964-113">To vám dává kompletní cloud velkých objemů dat a platformy pokročilou analýzu.</span><span class="sxs-lookup"><span data-stu-id="ac964-113">This gives you a complete cloud big data and advanced analytics platform.</span></span>

<span data-ttu-id="ac964-114">Tento názorný postup začne prostřednictvím popisu hello požadavky a prostředky, které jsou potřeba toocomplete hello úlohy s Data Lake Analytics, který tvoří proces vědecké účely hello dat a jak tooinstall je.</span><span class="sxs-lookup"><span data-stu-id="ac964-114">This walkthrough begins by describing hello prerequisites and resources that are needed toocomplete hello tasks with Data Lake Analytics that form hello data science process and how tooinstall them.</span></span> <span data-ttu-id="ac964-115">Potom ji popisuje postup hello zpracování dat pomocí U-SQL a ukončí zobrazením jak toouse Python a Hive s Azure Machine Learning Studio toobuild a nasazovat prediktivní modely hello.</span><span class="sxs-lookup"><span data-stu-id="ac964-115">Then it outlines hello data processing steps using U-SQL and concludes by showing how toouse Python and Hive with Azure Machine Learning Studio toobuild and deploy hello predictive models.</span></span> 

### <a name="u-sql-and-visual-studio"></a><span data-ttu-id="ac964-116">U-SQL Server a Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ac964-116">U-SQL and Visual Studio</span></span>
<span data-ttu-id="ac964-117">Tento názorný postup se doporučuje použití datové sady Visual Studio tooedit U-SQL skriptů tooprocess hello.</span><span class="sxs-lookup"><span data-stu-id="ac964-117">This walkthrough recommends using Visual Studio tooedit U-SQL scripts tooprocess hello dataset.</span></span> <span data-ttu-id="ac964-118">Hello skriptů U-SQL jsou zde popsané a poskytuje v samostatném souboru.</span><span class="sxs-lookup"><span data-stu-id="ac964-118">hello U-SQL scripts are described here and provided in a separate file.</span></span> <span data-ttu-id="ac964-119">Hello proces zahrnuje příjem, zkoumat a vzorkování dat hello.</span><span class="sxs-lookup"><span data-stu-id="ac964-119">hello process includes ingesting, exploring, and sampling hello data.</span></span> <span data-ttu-id="ac964-120">Také ukazuje, jak toorun U-SQL skripty úlohu, která z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ac964-120">It also shows how toorun a U-SQL scripted job from hello Azure portal.</span></span> <span data-ttu-id="ac964-121">Pro data hello do přidružených HDInsight clusteru toofacilitate hello sestavení a nasazení modelu binární klasifikace v Azure Machine Learning Studio se vytvoří tabulek Hive.</span><span class="sxs-lookup"><span data-stu-id="ac964-121">Hive tables are created for hello data in an associated HDInsight cluster toofacilitate hello building and deployment of a binary classification model in Azure Machine Learning Studio.</span></span>  

### <a name="python"></a><span data-ttu-id="ac964-122">Python</span><span class="sxs-lookup"><span data-stu-id="ac964-122">Python</span></span>
<span data-ttu-id="ac964-123">Tento názorný postup obsahuje také oddíl, který ukazuje, jak toobuild a nasazení prediktivního modelu Python pomocí Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="ac964-123">This walkthrough also contains a section that shows how toobuild and deploy a predictive model using Python with Azure Machine Learning Studio.</span></span>  <span data-ttu-id="ac964-124">Poskytujeme poznámkového bloku Jupyter pomocí skriptů Python hello tyto kroky v tomto procesu.</span><span class="sxs-lookup"><span data-stu-id="ac964-124">We provide a Jupyter notebook with hello Python scripts for these steps in this process.</span></span> <span data-ttu-id="ac964-125">Poznámkový blok Hello obsahuje kód pro některé další funkce engineering kroky a vytváření modelů například více třídami klasifikace a regresní modelování kromě modelu toohello binární klasifikace podle zde uvedeného.</span><span class="sxs-lookup"><span data-stu-id="ac964-125">hello notebook includes code for some additional feature engineering steps and models construction such as multiclass classification and regression modeling in addition toohello binary classification model outlined here.</span></span> <span data-ttu-id="ac964-126">Úloha regrese Hello je toopredict hello množství hello tip podle dalších funkcí tip.</span><span class="sxs-lookup"><span data-stu-id="ac964-126">hello regression task is toopredict hello amount of hello tip based on other tip features.</span></span> 

### <a name="azure-machine-learning"></a><span data-ttu-id="ac964-127">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="ac964-127">Azure Machine Learning</span></span>
<span data-ttu-id="ac964-128">Azure Machine Learning Studio je použité toobuild a nasazovat prediktivní modely hello.</span><span class="sxs-lookup"><span data-stu-id="ac964-128">Azure Machine Learning Studio is used toobuild and deploy hello predictive models.</span></span> <span data-ttu-id="ac964-129">To se provádí pomocí dva přístupy: nejdřív se skriptů Pythonu a potom se tabulek Hive v clusteru HDInsight (Hadoop).</span><span class="sxs-lookup"><span data-stu-id="ac964-129">This is done using two approaches: first with Python scripts and then with Hive tables on an HDInsight (Hadoop) cluster.</span></span>

### <a name="scripts"></a><span data-ttu-id="ac964-130">Skripty</span><span class="sxs-lookup"><span data-stu-id="ac964-130">Scripts</span></span>
<span data-ttu-id="ac964-131">V tomto názorném postupu jsou uvedeny pouze hello hlavní kroky.</span><span class="sxs-lookup"><span data-stu-id="ac964-131">Only hello principal steps are outlined in this walkthrough.</span></span> <span data-ttu-id="ac964-132">Můžete si stáhnout hello úplné **skript U-SQL** a **Poznámkový blok Jupyter** z [Githubu](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).</span><span class="sxs-lookup"><span data-stu-id="ac964-132">You can download hello full **U-SQL script** and **Jupyter Notebook** from [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ac964-133">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ac964-133">Prerequisites</span></span>
<span data-ttu-id="ac964-134">Před zahájením těchto témat, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="ac964-134">Before you begin these topics, you must have hello following:</span></span>

* <span data-ttu-id="ac964-135">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="ac964-135">An Azure subscription.</span></span> <span data-ttu-id="ac964-136">Pokud není již nemáte, přečtěte si téma [získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="ac964-136">If you do not already have one, see [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="ac964-137">[Doporučeno] Visual Studio 2013 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ac964-137">[Recommended] Visual Studio 2013 or later.</span></span> <span data-ttu-id="ac964-138">Pokud jste ještě není jedním z těchto verzí, můžete stáhnout bezplatnou verzi komunity z [Visual Studio Community](https://www.visualstudio.com/vs/community/).</span><span class="sxs-lookup"><span data-stu-id="ac964-138">If you do not already have one of these versions installed, you can download a free Community version from [Visual Studio Community](https://www.visualstudio.com/vs/community/).</span></span>

> [!NOTE]
> <span data-ttu-id="ac964-139">Místo Visual Studio můžete také použít hello portálu Azure toosubmit Azure Data Lake dotazy.</span><span class="sxs-lookup"><span data-stu-id="ac964-139">Instead of Visual Studio, you can also use hello Azure Portal toosubmit Azure Data Lake queries.</span></span> <span data-ttu-id="ac964-140">Poskytujeme pokyny na tom, jak toodo tak pomocí sady Visual Studio i na portálu hello v hello části s názvem **zpracování dat pomocí U-SQL**.</span><span class="sxs-lookup"><span data-stu-id="ac964-140">We will provide instructions on how toodo so both with Visual Studio and on hello portal in hello section titled **Process data with U-SQL**.</span></span> 
> 
> 


## <a name="prepare-data-science-environment-for-azure-data-lake"></a><span data-ttu-id="ac964-141">Příprava dat vědecké účely prostředí pro Azure Data Lake</span><span class="sxs-lookup"><span data-stu-id="ac964-141">Prepare data science environment for Azure Data Lake</span></span>
<span data-ttu-id="ac964-142">tooprepare hello datové vědy prostředí pro účely tohoto postupu vytvořit hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="ac964-142">tooprepare hello data science environment for this walkthrough, create hello following resources:</span></span>

* <span data-ttu-id="ac964-143">Azure Data Lake Store (ADLS)</span><span class="sxs-lookup"><span data-stu-id="ac964-143">Azure Data Lake Store (ADLS)</span></span> 
* <span data-ttu-id="ac964-144">Azure Data Lake Analytics (ADLA)</span><span class="sxs-lookup"><span data-stu-id="ac964-144">Azure Data Lake Analytics (ADLA)</span></span>
* <span data-ttu-id="ac964-145">Účet služby Azure Blob storage</span><span class="sxs-lookup"><span data-stu-id="ac964-145">Azure Blob storage account</span></span>
* <span data-ttu-id="ac964-146">Účet Azure Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="ac964-146">Azure Machine Learning Studio account</span></span>
* <span data-ttu-id="ac964-147">Nástroje Azure Data Lake pro Visual Studio (doporučeno)</span><span class="sxs-lookup"><span data-stu-id="ac964-147">Azure Data Lake Tools for Visual Studio (Recommended)</span></span>

<span data-ttu-id="ac964-148">Tato část obsahuje pokyny, jak toocreate těchto prostředků.</span><span class="sxs-lookup"><span data-stu-id="ac964-148">This section provides instructions on how toocreate each of these resources.</span></span> <span data-ttu-id="ac964-149">Pokud se rozhodnete toouse tabulek Hive pomocí Azure Machine Learning, místo Python, toobuild modelu, musíte se také tooprovision clusteru služby HDInsight (Hadoop).</span><span class="sxs-lookup"><span data-stu-id="ac964-149">If you choose toouse Hive tables with Azure Machine Learning, instead of Python, toobuild a model,you will also need tooprovision an HDInsight (Hadoop) cluster.</span></span> <span data-ttu-id="ac964-150">Tento alternativní postup v popsané v příslušné části hello níže.</span><span class="sxs-lookup"><span data-stu-id="ac964-150">This alternative procedure in described in hello appropriate section below.</span></span>


> [!NOTE]
> <span data-ttu-id="ac964-151">Hello **Azure Data Lake Store** je možné vytvořit buď samostatně nebo při vytváření hello **Azure Data Lake Analytics** jako hello výchozí úložiště.</span><span class="sxs-lookup"><span data-stu-id="ac964-151">hello **Azure Data Lake Store** can be created either separately or when you create hello **Azure Data Lake Analytics** as hello default storage.</span></span> <span data-ttu-id="ac964-152">Odkazují pokyny pro vytvoření každý z těchto prostředků samostatně níže, ale nemusí být hello účet úložiště Data Lake vytvářejí zvlášť.</span><span class="sxs-lookup"><span data-stu-id="ac964-152">Instructions are referenced for creating each of these resources separately below, but hello Data Lake storage account need not be created separately.</span></span>
>
> 

### <a name="create-an-azure-data-lake-store"></a><span data-ttu-id="ac964-153">Vytvoření Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="ac964-153">Create an Azure Data Lake Store</span></span>


<span data-ttu-id="ac964-154">Vytvoření ADLS z hello [portálu Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ac964-154">Create an ADLS from hello [Azure Portal](http://portal.azure.com).</span></span> <span data-ttu-id="ac964-155">Podrobnosti najdete v tématu [vytvoření clusteru HDInsight s Data Lake Store pomocí portálu Azure](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ac964-155">For details, see [Create an HDInsight cluster with Data Lake Store using Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span> <span data-ttu-id="ac964-156">Být jisti tooset až hello identita AAD clusteru v hello **DataSource** okno hello **volitelné konfiguraci** okna popsané existuje.</span><span class="sxs-lookup"><span data-stu-id="ac964-156">Be sure tooset up hello Cluster AAD Identity in hello **DataSource** blade of hello **Optional Configuration** blade described there.</span></span> 

 ![3](./media/machine-learning-data-science-process-data-lake-walkthrough/3-create-ADLS.PNG)

### <a name="create-an-azure-data-lake-analytics-account"></a><span data-ttu-id="ac964-158">Vytvoření účtu Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="ac964-158">Create an Azure Data Lake Analytics account</span></span>
<span data-ttu-id="ac964-159">Vytvoření účtu ADLA z hello [portálu Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ac964-159">Create an ADLA account from hello [Azure Portal](http://portal.azure.com).</span></span> <span data-ttu-id="ac964-160">Podrobnosti najdete v tématu [kurz: Začínáme s Azure Data Lake Analytics pomocí portálu Azure](../data-lake-analytics/data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ac964-160">For details, see [Tutorial: get started with Azure Data Lake Analytics using Azure Portal](../data-lake-analytics/data-lake-analytics-get-started-portal.md).</span></span> 

 ![4](./media/machine-learning-data-science-process-data-lake-walkthrough/4-create-ADLA-new.PNG)

### <a name="create-an-azure-blob-storage-account"></a><span data-ttu-id="ac964-162">Vytvoření účtu úložiště objektů Blob v Azure</span><span class="sxs-lookup"><span data-stu-id="ac964-162">Create an Azure Blob storage account</span></span>
<span data-ttu-id="ac964-163">Vytvoření účtu úložiště objektů Blob v Azure z hello [portálu Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ac964-163">Create an Azure Blob storage account from hello [Azure Portal](http://portal.azure.com).</span></span> <span data-ttu-id="ac964-164">Podrobnosti najdete v tématu hello vytvořit účet úložiště v tématu [účty Azure storage](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="ac964-164">For details, see hello Create a storage account section in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>

 ![5](./media/machine-learning-data-science-process-data-lake-walkthrough/5-Create-Azure-Blob.PNG)

### <a name="set-up-an-azure-machine-learning-studio-account"></a><span data-ttu-id="ac964-166">Nastavit účet Azure Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="ac964-166">Set up an Azure Machine Learning Studio account</span></span>
<span data-ttu-id="ac964-167">Přihlášení/do Azure Machine Learning Studio z hello [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) stránky.</span><span class="sxs-lookup"><span data-stu-id="ac964-167">Sign up/into Azure Machine Learning Studio from hello [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) page.</span></span> <span data-ttu-id="ac964-168">Klikněte na hello **začít nyní** tlačítko a pak zvolte "Volného prostoru" nebo "Standardní pracovní prostor".</span><span class="sxs-lookup"><span data-stu-id="ac964-168">Click on hello **Get started now** button and then choose a "Free Workspace" or "Standard Workspace".</span></span> <span data-ttu-id="ac964-169">Potom bude možné toocreate experimenty v Azure ML Studio.</span><span class="sxs-lookup"><span data-stu-id="ac964-169">After this you will be able toocreate experiments in Azure ML Studio.</span></span>  

### <a name="install-azure-data-lake-tools-recommended"></a><span data-ttu-id="ac964-170">Instalace nástroje Azure Data Lake [doporučeno]</span><span class="sxs-lookup"><span data-stu-id="ac964-170">Install Azure Data Lake Tools [Recommended]</span></span>
<span data-ttu-id="ac964-171">Instalace nástroje Azure Data Lake pro vaši verzi sady Visual Studio z [nástrojů Azure Data Lake pro Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span><span class="sxs-lookup"><span data-stu-id="ac964-171">Install Azure Data Lake Tools for your version of Visual Studio from [Azure Data Lake Tools for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span></span>

 ![6](./media/machine-learning-data-science-process-data-lake-walkthrough/6-install-ADL-tools-VS.PNG)

<span data-ttu-id="ac964-173">Po instalaci hello skončí úspěšně, otevřete Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ac964-173">After hello installation finishes successfully, open up Visual Studio.</span></span> <span data-ttu-id="ac964-174">Měli byste vidět hello Data Lake karta hello nabídce v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="ac964-174">You should see hello Data Lake tab hello menu at hello top.</span></span> <span data-ttu-id="ac964-175">V levém panelu hello měla vypadat vašich prostředků Azure, když se přihlásíte ke svému účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="ac964-175">Your Azure resources should appear in hello left panel when you sign into your Azure account.</span></span>

 ![7](./media/machine-learning-data-science-process-data-lake-walkthrough/7-install-ADL-tools-VS-done.PNG)

## <a name="hello-nyc-taxi-trips-dataset"></a><span data-ttu-id="ac964-177">datovou sadu cest taxíkem NYC Hello</span><span class="sxs-lookup"><span data-stu-id="ac964-177">hello NYC Taxi Trips dataset</span></span>
<span data-ttu-id="ac964-178">Hello sady dat jsme použili v tomto poli je veřejně dostupné datová sada--hello [datovou sadu cest taxíkem NYC](http://www.andresmh.com/nyctaxitrips/).</span><span class="sxs-lookup"><span data-stu-id="ac964-178">hello data set we used here is a publicly available dataset -- hello [NYC Taxi Trips dataset](http://www.andresmh.com/nyctaxitrips/).</span></span> <span data-ttu-id="ac964-179">Hello NYC taxíkem cestě dat se skládá z přibližně 20GB komprimovaných souborů CSV (nekomprimovaným ~ 48GB), zaznamenávání 173 milionů jednotlivých cest a hello tarify placené pro každou cestu.</span><span class="sxs-lookup"><span data-stu-id="ac964-179">hello NYC Taxi Trip data consists of about 20GB of compressed CSV files (~48GB uncompressed), recording more than 173 million individual trips and hello fares paid for each trip.</span></span> <span data-ttu-id="ac964-180">Každý záznam cestě zahrnuje hello vyzvednutí a odkládací umístění a časy, anonymní zabezpečení číslo licence (ovladač) a hello číslo Medailon (taxi na jedinečné id).</span><span class="sxs-lookup"><span data-stu-id="ac964-180">Each trip record includes hello pickup and drop-off locations and times, anonymized hack (driver's) license number, and hello medallion (taxi’s unique id) number.</span></span> <span data-ttu-id="ac964-181">Hello dat obsahuje všechny služebních cest v hello roku 2013 a je součástí hello následující dvě datové sady pro každý měsíc:</span><span class="sxs-lookup"><span data-stu-id="ac964-181">hello data covers all trips in hello year 2013 and is provided in hello following two datasets for each month:</span></span>

* <span data-ttu-id="ac964-182">Hello 'trip_data' CSV obsahuje podrobnosti o cestě, například na počtu cestujících, vyzvednutí a dropoff body, doba trvání cesty a délka cesty.</span><span class="sxs-lookup"><span data-stu-id="ac964-182">hello 'trip_data' CSV contains trip details, such as number of passengers, pickup and dropoff points, trip duration, and trip length.</span></span> <span data-ttu-id="ac964-183">Tady je několik ukázkových záznamů:</span><span class="sxs-lookup"><span data-stu-id="ac964-183">Here are a few sample records:</span></span>
  
       medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count, trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
       89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
* <span data-ttu-id="ac964-184">Hello CSV obsahuje podrobnosti o tarif hello placené pro každou cestu, například typ platby, velikost tarif, příplatek a daně, tipy a mýtné a celkovou velikost hello placené trip_fare.</span><span class="sxs-lookup"><span data-stu-id="ac964-184">hello 'trip_fare' CSV contains details of hello fare paid for each trip, such as payment type, fare amount, surcharge and taxes, tips and tolls, and hello total amount paid.</span></span> <span data-ttu-id="ac964-185">Tady je několik ukázkových záznamů:</span><span class="sxs-lookup"><span data-stu-id="ac964-185">Here are a few sample records:</span></span>
  
       medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
       89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

<span data-ttu-id="ac964-186">Hello jedinečné klíče toojoin cestě\_dat a cesty\_tarif se skládá z následujících tří polí hello: medailonu, hackerský\_licence a vyzvednutí\_data a času.</span><span class="sxs-lookup"><span data-stu-id="ac964-186">hello unique key toojoin trip\_data and trip\_fare is composed of hello following three fields: medallion, hack\_license and pickup\_datetime.</span></span> <span data-ttu-id="ac964-187">CSV soubory raw Hello jsou přístupné z veřejného úložiště Azure blob.</span><span class="sxs-lookup"><span data-stu-id="ac964-187">hello raw CSV files can be accessed from a public Azure storage blob.</span></span> <span data-ttu-id="ac964-188">Hello skript U-SQL pro toto připojení je v hello [připojení k cestě a tarif tabulky](#join) části.</span><span class="sxs-lookup"><span data-stu-id="ac964-188">hello U-SQL script for this join is in hello [Join trip and fare tables](#join) section.</span></span>

## <a name="process-data-with-u-sql"></a><span data-ttu-id="ac964-189">Zpracování dat pomocí U-SQL</span><span class="sxs-lookup"><span data-stu-id="ac964-189">Process data with U-SQL</span></span>
<span data-ttu-id="ac964-190">úlohy zpracování dat Hello v této části zahrnují příjem, kontrolu kvality, prohlížení a vzorkování dat hello.</span><span class="sxs-lookup"><span data-stu-id="ac964-190">hello data processing tasks illustrated in this section include ingesting, checking quality, exploring, and sampling hello data.</span></span> <span data-ttu-id="ac964-191">Také ukážeme, jak toojoin služební cestě a tarif tabulky.</span><span class="sxs-lookup"><span data-stu-id="ac964-191">We also show how toojoin trip and fare tables.</span></span> <span data-ttu-id="ac964-192">Koncová část Hello ukazuje spuštění úlohy pomocí skriptu U-SQL z hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ac964-192">hello final section shows run a U-SQL scripted job from hello Azure portal.</span></span> <span data-ttu-id="ac964-193">Tady jsou odkazy tooeach část:</span><span class="sxs-lookup"><span data-stu-id="ac964-193">Here are links tooeach subsection:</span></span>

* [<span data-ttu-id="ac964-194">Přijímání dat: přečíst data z veřejného objektu blob</span><span class="sxs-lookup"><span data-stu-id="ac964-194">Data ingestion: read in data from public blob</span></span>](#ingest)
* [<span data-ttu-id="ac964-195">Kontrolu kvality dat</span><span class="sxs-lookup"><span data-stu-id="ac964-195">Data quality checks</span></span>](#quality)
* [<span data-ttu-id="ac964-196">Zkoumání dat</span><span class="sxs-lookup"><span data-stu-id="ac964-196">Data exploration</span></span>](#explore)
* [<span data-ttu-id="ac964-197">Připojení k cestě a tarif tabulky</span><span class="sxs-lookup"><span data-stu-id="ac964-197">Join trip and fare tables</span></span>](#join)
* [<span data-ttu-id="ac964-198">Vzorkování dat</span><span class="sxs-lookup"><span data-stu-id="ac964-198">Data sampling</span></span>](#sample)
* [<span data-ttu-id="ac964-199">Spuštění úloh U-SQL</span><span class="sxs-lookup"><span data-stu-id="ac964-199">Run U-SQL jobs</span></span>](#run)

<span data-ttu-id="ac964-200">Hello skriptů U-SQL jsou zde popsané a poskytuje v samostatném souboru.</span><span class="sxs-lookup"><span data-stu-id="ac964-200">hello U-SQL scripts are described here and provided in a separate file.</span></span> <span data-ttu-id="ac964-201">Můžete si stáhnout hello úplné **skriptů U-SQL** z [Githubu](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).</span><span class="sxs-lookup"><span data-stu-id="ac964-201">You can download hello full **U-SQL scripts** from [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).</span></span>

<span data-ttu-id="ac964-202">tooexecute U-SQL, otevřete Visual Studio, klikněte na tlačítko **souboru--> New--> projekt**, zvolte **projekt U-SQL**, název a uložte ho tooa složky.</span><span class="sxs-lookup"><span data-stu-id="ac964-202">tooexecute U-SQL, Open Visual Studio, click **File --> New --> Project**, choose **U-SQL Project**, name and save it tooa folder.</span></span>

![8](./media/machine-learning-data-science-process-data-lake-walkthrough/8-create-USQL-project.PNG)

> [!NOTE]
> <span data-ttu-id="ac964-204">Je možné toouse hello portálu Azure tooexecute U-SQL místo sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ac964-204">It is possible toouse hello Azure Portal tooexecute U-SQL instead of Visual Studio.</span></span> <span data-ttu-id="ac964-205">Můžete přejít toohello prostředků Azure Data Lake Analytics na portálu hello a odesílání dotazů přímo jako ukázáno v hello následující obrázek.</span><span class="sxs-lookup"><span data-stu-id="ac964-205">You can navigate toohello Azure Data Lake Analytics resource on hello portal and submit queries directly as illustrated in hello following figure.</span></span>
> 
> 

![9](./media/machine-learning-data-science-process-data-lake-walkthrough/9-portal-submit-job.PNG)

### <span data-ttu-id="ac964-207"><a name="ingest"></a>Přijímání dat: Přečíst data z veřejného objektu blob</span><span class="sxs-lookup"><span data-stu-id="ac964-207"><a name="ingest"></a>Data Ingestion: Read in data from public blob</span></span>
<span data-ttu-id="ac964-208">umístění Hello hello dat v hello objektů blob v Azure je odkazováno jako  **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name**  a mohou být extrahovány pomocí **Extractors.Csv()**.</span><span class="sxs-lookup"><span data-stu-id="ac964-208">hello location of hello data in hello Azure blob is referenced as **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name** and can be extracted using **Extractors.Csv()**.</span></span> <span data-ttu-id="ac964-209">Nahraďte váš vlastní název kontejneru a název účtu úložiště v následujících skriptů pro container_name@blob_storage_account_name hello wasb adresu.</span><span class="sxs-lookup"><span data-stu-id="ac964-209">Substitute your own container name and storage account name in following scripts for container_name@blob_storage_account_name in hello wasb address.</span></span> <span data-ttu-id="ac964-210">Vzhledem k tomu, že názvy souborů hello jsou ve stejném formátu, můžeme použít **cestě\_data_ {\*\}.csv** tooread v všechny soubory 12 cesty.</span><span class="sxs-lookup"><span data-stu-id="ac964-210">Since hello file names are in same format, we can use **trip\_data_{\*\}.csv** tooread in all 12 trip files.</span></span> 

    ///Read in Trip data
    @trip0 =
        EXTRACT 
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string
    // This is reading 12 trip data from blob
    FROM "wasb://container_name@blob_storage_account_name.blob.core.windows.net/nyctaxitrip/trip_data_{*}.csv"
    USING Extractors.Csv();

<span data-ttu-id="ac964-211">Vzhledem k tomu, že jsou v hello první řádek záhlaví, budeme potřebovat tooremove hello hlavičky a změnit typy sloupců do odpovídající těm, které jsou.</span><span class="sxs-lookup"><span data-stu-id="ac964-211">Since there are headers in hello first row, we need tooremove hello headers and change column types into appropriate ones.</span></span> <span data-ttu-id="ac964-212">Můžeme buď uložit hello zpracování dat tooAzure Data Lake Storage pomocí **swebhdfs://data_lake_storage_name.azuredatalakestorage.net/folder_name/file_name**_ nebo tooAzure úložiště objektů Blob v účtu pomocí  **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name** .</span><span class="sxs-lookup"><span data-stu-id="ac964-212">We can either save hello processed data tooAzure Data Lake Storage using **swebhdfs://data_lake_storage_name.azuredatalakestorage.net/folder_name/file_name**_ or tooAzure Blob storage account using  **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name**.</span></span> 

    // change data types
    @trip =
        SELECT 
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        DateTime.Parse(pickup_datetime) AS pickup_datetime,
        DateTime.Parse(dropoff_datetime) AS dropoff_datetime,
        Int32.Parse(passenger_count) AS passenger_count,
        Double.Parse(trip_time_in_secs) AS trip_time_in_secs,
        Double.Parse(trip_distance) AS trip_distance,
        (pickup_longitude==string.Empty ? 0: float.Parse(pickup_longitude)) AS pickup_longitude,
        (pickup_latitude==string.Empty ? 0: float.Parse(pickup_latitude)) AS pickup_latitude,
        (dropoff_longitude==string.Empty ? 0: float.Parse(dropoff_longitude)) AS dropoff_longitude,
        (dropoff_latitude==string.Empty ? 0: float.Parse(dropoff_latitude)) AS dropoff_latitude
    FROM @trip0
    WHERE medallion != "medallion";

    ////output data tooADL
    OUTPUT @trip   
    too"swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_trip.csv"
    USING Outputters.Csv(); 

    ////Output data tooblob
    OUTPUT @trip   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_trip.csv"
    USING Outputters.Csv();  

<span data-ttu-id="ac964-213">Podobně jsme může číst hello tarif datových sad.</span><span class="sxs-lookup"><span data-stu-id="ac964-213">Similarly we can read in hello fare data sets.</span></span> <span data-ttu-id="ac964-214">Klikněte pravým tlačítkem na Azure Data Lake Store, můžete toolook na vaše data v **portálu Azure--> Průzkumníku dat** nebo **Průzkumníka souborů** v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ac964-214">Right click Azure Data Lake Store, you can choose toolook at your data in **Azure Portal --> Data Explorer** or **File Explorer** within Visual Studio.</span></span> 

 ![10](./media/machine-learning-data-science-process-data-lake-walkthrough/10-data-in-ADL-VS.PNG)

 ![11](./media/machine-learning-data-science-process-data-lake-walkthrough/11-data-in-ADL.PNG)

### <span data-ttu-id="ac964-217"><a name="quality"></a>Kontrolu kvality dat</span><span class="sxs-lookup"><span data-stu-id="ac964-217"><a name="quality"></a>Data quality checks</span></span>
<span data-ttu-id="ac964-218">Po cestě a tarif tabulky byly načteny v, lze provést kontrolu kvality dat v hello následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="ac964-218">After trip and fare tables have been read in, data quality checks can be done in hello following way.</span></span> <span data-ttu-id="ac964-219">Hello výsledná soubory sdíleného svazku clusteru může být výstup tooAzure Blob storage nebo Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="ac964-219">hello resulting CSV files can be output tooAzure Blob storage or Azure Data Lake Store.</span></span> 

<span data-ttu-id="ac964-220">Najít hello počet medallions a jedinečné číslo medallions:</span><span class="sxs-lookup"><span data-stu-id="ac964-220">Find hello number of medallions and unique number of medallions:</span></span>

    ///check hello number of medallions and unique number of medallions
    @trip2 =
        SELECT
        medallion,
        vendor_id,
        pickup_datetime.Month AS pickup_month
        FROM @trip;

    @ex_1 =
        SELECT
        pickup_month, 
        COUNT(medallion) AS cnt_medallion,
        COUNT(DISTINCT(medallion)) AS unique_medallion
        FROM @trip2
        GROUP BY pickup_month;
        OUTPUT @ex_1   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_1.csv"
    USING Outputters.Csv(); 

<span data-ttu-id="ac964-221">Najděte tyto medallions, kterým má více než 100 odezvy:</span><span class="sxs-lookup"><span data-stu-id="ac964-221">Find those medallions that had more than 100 trips:</span></span>

    ///find those medallions that had more than 100 trips
    @ex_2 =
        SELECT medallion,
               COUNT(medallion) AS cnt_medallion
        FROM @trip2
        //where pickup_datetime >= "2013-01-01t00:00:00.0000000" and pickup_datetime <= "2013-04-01t00:00:00.0000000"
        GROUP BY medallion
        HAVING COUNT(medallion) > 100;
        OUTPUT @ex_2   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_2.csv"
    USING Outputters.Csv(); 

<span data-ttu-id="ac964-222">Najděte tyto neplatné záznamy z hlediska pickup_longitude:</span><span class="sxs-lookup"><span data-stu-id="ac964-222">Find those invalid records in terms of pickup_longitude:</span></span>

    ///find those invalid records in terms of pickup_longitude
    @ex_3 =
        SELECT COUNT(medallion) AS cnt_invalid_pickup_longitude
        FROM @trip
        WHERE
        pickup_longitude <- 90 OR pickup_longitude > 90;
        OUTPUT @ex_3   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_3.csv"
    USING Outputters.Csv(); 

<span data-ttu-id="ac964-223">Najdete chybějící hodnoty pro některé proměnné:</span><span class="sxs-lookup"><span data-stu-id="ac964-223">Find missing values for some variables:</span></span>

    //check missing values
    @res =
        SELECT *,
               (medallion == null? 1 : 0) AS missing_medallion
        FROM @trip;

    @trip_summary6 =
        SELECT 
            vendor_id,
        SUM(missing_medallion) AS medallion_empty, 
        COUNT(medallion) AS medallion_total,
        COUNT(DISTINCT(medallion)) AS medallion_total_unique  
        FROM @res
        GROUP BY vendor_id;
    OUTPUT @trip_summary6
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_16.csv"
    USING Outputters.Csv();



### <span data-ttu-id="ac964-224"><a name="explore"></a>Zkoumání dat</span><span class="sxs-lookup"><span data-stu-id="ac964-224"><a name="explore"></a>Data exploration</span></span>
<span data-ttu-id="ac964-225">Jsme můžete provést některé tooget zkoumání dat lépe porozumět hello data.</span><span class="sxs-lookup"><span data-stu-id="ac964-225">We can do some data exploration tooget a better understanding of hello data.</span></span>

<span data-ttu-id="ac964-226">Najděte hello distribuce šikmý a vysypávány služebních cest:</span><span class="sxs-lookup"><span data-stu-id="ac964-226">Find hello distribution of tipped and non-tipped trips:</span></span>

    ///tipped vs. not tipped distribution
    @tip_or_not =
        SELECT *,
               (tip_amount > 0 ? 1: 0) AS tipped
        FROM @fare;

    @ex_4 =
        SELECT tipped,
               COUNT(*) AS tip_freq
        FROM @tip_or_not
        GROUP BY tipped;
        OUTPUT @ex_4   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_4.csv"
    USING Outputters.Csv(); 

<span data-ttu-id="ac964-227">Najít hello distribuce tip velikost s hodnotami oříznutím: 0,5,10 a 20 odběru.</span><span class="sxs-lookup"><span data-stu-id="ac964-227">Find hello distribution of tip amount with cut-off values: 0,5,10,and 20 dollars.</span></span>

    //tip class/range distribution
    @tip_class =
        SELECT *,
               (tip_amount >20? 4: (tip_amount >10? 3:(tip_amount >5 ? 2:(tip_amount > 0 ? 1: 0)))) AS tip_class
        FROM @fare;
    @ex_5 =
        SELECT tip_class,
               COUNT(*) AS tip_freq
        FROM @tip_class
        GROUP BY tip_class;
        OUTPUT @ex_5   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_5.csv"
    USING Outputters.Csv(); 

<span data-ttu-id="ac964-228">Najděte základní statistické údaje o cestě vzdálenost:</span><span class="sxs-lookup"><span data-stu-id="ac964-228">Find basic statistics of trip distance:</span></span>

    // find basic statistics for trip_distance
    @trip_summary4 =
        SELECT 
            vendor_id,
            COUNT(*) AS cnt_row,
            MIN(trip_distance) AS min_trip_distance,
            MAX(trip_distance) AS max_trip_distance,
            AVG(trip_distance) AS avg_trip_distance 
        FROM @trip
        GROUP BY vendor_id;
    OUTPUT @trip_summary4
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_14.csv"
    USING Outputters.Csv();

<span data-ttu-id="ac964-229">Najděte hello percentily vzdálenosti cesty:</span><span class="sxs-lookup"><span data-stu-id="ac964-229">Find hello percentiles of trip distance:</span></span>

    // find percentiles of trip_distance
    @trip_summary3 =
        SELECT DISTINCT vendor_id AS vendor,
                        PERCENTILE_DISC(0.25) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.5) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.75) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc
        FROM @trip;
       // group by vendor_id;
    OUTPUT @trip_summary3
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_13.csv"
    USING Outputters.Csv(); 


### <span data-ttu-id="ac964-230"><a name="join"></a>Připojení k cestě a tarif tabulky</span><span class="sxs-lookup"><span data-stu-id="ac964-230"><a name="join"></a>Join trip and fare tables</span></span>
<span data-ttu-id="ac964-231">Služební cestě a tarif tabulek je možné připojit Medailon, hack_license a pickup_time.</span><span class="sxs-lookup"><span data-stu-id="ac964-231">Trip and fare tables can be joined by medallion, hack_license, and pickup_time.</span></span>

    //join trip and fare table

    @model_data_full =
    SELECT t.*, 
    f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
    (f.tip_amount > 0 ? 1: 0) AS tipped,
    (f.tip_amount >20? 4: (f.tip_amount >10? 3:(f.tip_amount >5 ? 2:(f.tip_amount > 0 ? 1: 0)))) AS tip_class
    FROM @trip AS t JOIN  @fare AS f
    ON   (t.medallion == f.medallion AND t.hack_license == f.hack_license AND t.pickup_datetime == f.pickup_datetime)
    WHERE   (pickup_longitude != 0 AND dropoff_longitude != 0 );

    //// output tooblob
    OUTPUT @model_data_full   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 

    ////output data tooADL
    OUTPUT @model_data_full   
    too"swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 


<span data-ttu-id="ac964-232">Pro každou úroveň počtu osobní vypočítejte hello počet záznamů, tip pro průměrnou dobu, odchylku tip velikost, procento šikmý služebních cest.</span><span class="sxs-lookup"><span data-stu-id="ac964-232">For each level of passenger count, calculate hello number of records, average tip amount, variance of tip amount, percentage of tipped trips.</span></span>

    // contigency table
    @trip_summary8 =
        SELECT passenger_count,
               COUNT(*) AS cnt,
               AVG(tip_amount) AS avg_tip_amount,
               VAR(tip_amount) AS var_tip_amount,
               SUM(tipped) AS cnt_tipped,
               (float)SUM(tipped)/COUNT(*) AS pct_tipped
        FROM @model_data_full
        GROUP BY passenger_count;
        OUTPUT @trip_summary8
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_17.csv"
    USING Outputters.Csv();


### <span data-ttu-id="ac964-233"><a name="sample"></a>Vzorkování dat</span><span class="sxs-lookup"><span data-stu-id="ac964-233"><a name="sample"></a>Data sampling</span></span>
<span data-ttu-id="ac964-234">Nejprve jsme náhodně zvolí 0,1 % hello dat z tabulky připojené k hello:</span><span class="sxs-lookup"><span data-stu-id="ac964-234">First we randomly select 0.1% of hello data from hello joined table:</span></span>

    //random select 1/1000 data for modeling purpose
    @addrownumberres_randomsample =
    SELECT *,
            ROW_NUMBER() OVER() AS rownum
    FROM @model_data_full;

    @model_data_random_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_randomsample
    WHERE rownum % 1000 == 0;

    OUTPUT @model_data_random_sample_1_1000   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_random_1_1000.csv"
    USING Outputters.Csv(); 

<span data-ttu-id="ac964-235">Potom jsme vrstveného vzorkování provedete binární proměnné tip_class:</span><span class="sxs-lookup"><span data-stu-id="ac964-235">Then we do stratified sampling by binary variable tip_class:</span></span>

    //stratified random select 1/1000 data for modeling purpose
    @addrownumberres_stratifiedsample =
    SELECT *,
            ROW_NUMBER() OVER(PARTITION BY tip_class) AS rownum
    FROM @model_data_full;

    @model_data_stratified_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_stratifiedsample
    WHERE rownum % 1000 == 0;
    //// output tooblob
    OUTPUT @model_data_stratified_sample_1_1000   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 
    ////output data tooADL
    OUTPUT @model_data_stratified_sample_1_1000   
    too"swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 


### <span data-ttu-id="ac964-236"><a name="run"></a>Spuštění úloh U-SQL</span><span class="sxs-lookup"><span data-stu-id="ac964-236"><a name="run"></a>Run U-SQL jobs</span></span>
<span data-ttu-id="ac964-237">Po dokončení úprav skriptů U-SQL, můžete je odeslat toohello serveru pomocí účtu Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="ac964-237">When you finish editing U-SQL scripts, you can submit them toohello server using your Azure Data Lake Analytics account.</span></span> <span data-ttu-id="ac964-238">Klikněte na tlačítko **Data Lake**, **odeslat úlohu**, vyberte vaše **účet Analytics**, zvolte **paralelismus**a klikněte na tlačítko **odeslání**  tlačítko.</span><span class="sxs-lookup"><span data-stu-id="ac964-238">Click **Data Lake**, **Submit Job**, select your **Analytics Account**, choose **Parallelism**, and click **Submit** button.</span></span>  

 ![12](./media/machine-learning-data-science-process-data-lake-walkthrough/12-submit-USQL.PNG)

<span data-ttu-id="ac964-240">Když hello úlohy je úspěšně splněny, hello stav úlohy se zobrazí v sadě Visual Studio pro monitorování.</span><span class="sxs-lookup"><span data-stu-id="ac964-240">When hello job is complied successfully, hello status of your job will be displayed in Visual Studio for monitoring.</span></span> <span data-ttu-id="ac964-241">Po dokončení úlohy hello můžete i opětovného přehrání hello úlohy spuštění procesu a zjistěte hello bottleneck kroky tooimprove efektivity úlohy.</span><span class="sxs-lookup"><span data-stu-id="ac964-241">After hello job finishes running, you can even replay hello job execution process and find out hello bottleneck steps tooimprove your job efficiency.</span></span> <span data-ttu-id="ac964-242">Můžete také přejít tooAzure portál toocheck hello stav úloh U-SQL.</span><span class="sxs-lookup"><span data-stu-id="ac964-242">You can also go tooAzure Portal toocheck hello status of your U-SQL jobs.</span></span>

 ![13](./media/machine-learning-data-science-process-data-lake-walkthrough/13-USQL-running-v2.PNG)

 ![14](./media/machine-learning-data-science-process-data-lake-walkthrough/14-USQL-jobs-portal.PNG)

<span data-ttu-id="ac964-245">Nyní můžete zkontrolovat hello výstupní soubory v Azure Blob storage nebo portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="ac964-245">Now you can check hello output files in either Azure Blob storage or Azure Portal.</span></span> <span data-ttu-id="ac964-246">Pro naše modelování v dalším kroku hello použijeme hello si ukázková data.</span><span class="sxs-lookup"><span data-stu-id="ac964-246">We will use hello stratified sample data for our modeling in hello next step.</span></span>

 ![15](./media/machine-learning-data-science-process-data-lake-walkthrough/15-U-SQL-output-csv.PNG)

 ![16](./media/machine-learning-data-science-process-data-lake-walkthrough/16-U-SQL-output-csv-portal.PNG)

## <a name="build-and-deploy-models-in-azure-machine-learning"></a><span data-ttu-id="ac964-249">Vytváření a nasazování modelů v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="ac964-249">Build and deploy models in Azure Machine Learning</span></span>
<span data-ttu-id="ac964-250">Ukážeme dvě možnosti k dispozici pro vás toopull data do Azure Machine Learning toobuild a</span><span class="sxs-lookup"><span data-stu-id="ac964-250">We demonstrate two options available for you toopull data into Azure Machine Learning toobuild and</span></span> 

* <span data-ttu-id="ac964-251">V první možnosti hello použijete hello vzorků dat, která byla zapsána tooan objektů Blob v Azure (v hello **dat vzorkování** předchozí krok) a použít Python toobuild a nasazovat modely z Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="ac964-251">In hello first option, you use hello sampled data that has been written tooan Azure Blob (in hello **Data sampling** step above) and use Python toobuild and deploy models from Azure Machine Learning.</span></span> 
* <span data-ttu-id="ac964-252">V druhé možnosti hello dotazování hello dat v Azure Data Lake přímo pomocí dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="ac964-252">In hello second option, you query hello data in Azure Data Lake directly using a Hive query.</span></span> <span data-ttu-id="ac964-253">Tato možnost vyžaduje vytvoření nového clusteru HDInsight nebo použít stávající cluster HDInsight tabulky, kde hello Hive bodu toohello NY taxíkem data v Azure Data Lake Storage.</span><span class="sxs-lookup"><span data-stu-id="ac964-253">This option requires that you create a new HDInsight cluster or use an existing HDInsight cluster where hello Hive tables point toohello NY Taxi data in Azure Data Lake Storage.</span></span>  <span data-ttu-id="ac964-254">Probereme obě tyto možnosti dole.</span><span class="sxs-lookup"><span data-stu-id="ac964-254">We discuss both these options below.</span></span> 

## <a name="option-1-use-python-toobuild-and-deploy-machine-learning-models"></a><span data-ttu-id="ac964-255">Možnost 1: Použití Python toobuild a nasazovat modely machine learning</span><span class="sxs-lookup"><span data-stu-id="ac964-255">Option 1: Use Python toobuild and deploy machine learning models</span></span>
<span data-ttu-id="ac964-256">toobuild a nasadit modely machine learning používá Python, vytvořte poznámkového bloku Jupyter na místním počítači nebo v Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="ac964-256">toobuild and deploy machine learning models using Python, create a Jupyter Notebook on your local machine or in Azure Machine Learning Studio.</span></span> <span data-ttu-id="ac964-257">Hello Poznámkový blok Jupyter k dispozici na [Githubu](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough) obsahuje hello tooexplore úplného kódu, vizualizovat data, funkce technikům, modelování a nasazení.</span><span class="sxs-lookup"><span data-stu-id="ac964-257">hello Jupyter Notebook  provided on [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough) contains hello full code tooexplore, visualize data, feature engineering, modeling and deployment.</span></span> <span data-ttu-id="ac964-258">V tomto článku jsme zobrazit právě hello modelování a nasazení.</span><span class="sxs-lookup"><span data-stu-id="ac964-258">In this article, we show just hello modeling and deployment.</span></span> 

### <a name="import-python-libraries"></a><span data-ttu-id="ac964-259">Importovat knihovny Python</span><span class="sxs-lookup"><span data-stu-id="ac964-259">Import Python libraries</span></span>
<span data-ttu-id="ac964-260">V pořadí toorun hello ukázkové Poznámkový blok Jupyter nebo hello soubor skriptu jazyka Python, hello následující balíčky Python, které jsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="ac964-260">In order toorun hello sample Jupyter Notebook or hello Python script file, hello following Python packages are needed.</span></span> <span data-ttu-id="ac964-261">Pokud používáte hello služby AzureML Poznámkový blok, byly tyto balíčky předem nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="ac964-261">If you are using hello AzureML Notebook service, these packages have been pre-installed.</span></span>

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random
    import sklearn
    from sklearn.linear_model import LogisticRegression
    from sklearn.cross_validation import train_test_split
    from sklearn import metrics
    from __future__ import division
    from sklearn import linear_model
    from azureml import services


### <a name="read-in-hello-data-from-blob"></a><span data-ttu-id="ac964-262">Přečtěte si hello dat z objektu blob</span><span class="sxs-lookup"><span data-stu-id="ac964-262">Read in hello data from blob</span></span>
* <span data-ttu-id="ac964-263">Připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="ac964-263">Connection String</span></span>   
  
        CONTAINERNAME = 'test1'
        STORAGEACCOUNTNAME = 'XXXXXXXXX'
        STORAGEACCOUNTKEY = 'YYYYYYYYYYYYYYYYYYYYYYYYYYYY'
        BLOBNAME = 'demo_ex_9_stratified_1_1000_copy.csv'
        blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
* <span data-ttu-id="ac964-264">Přečíst jako text</span><span class="sxs-lookup"><span data-stu-id="ac964-264">Read in as text</span></span>
  
        t1 = time.time()
        data = blob_service.get_blob_to_text(CONTAINERNAME,BLOBNAME).split("\n")
        t2 = time.time()
        print(("It takes %s seconds tooread in "+BLOBNAME) % (t2 - t1))
  
  ![17](./media/machine-learning-data-science-process-data-lake-walkthrough/17-python_readin_csv.PNG)    
* <span data-ttu-id="ac964-266">Přidat názvy sloupců a jednotlivé sloupce</span><span class="sxs-lookup"><span data-stu-id="ac964-266">Add column names and separate columns</span></span>
  
        colnames = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime',
        'passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'payment_type', 'fare_amount', 'surcharge', 'mta_tax', 'tolls_amount',  'total_amount', 'tip_amount', 'tipped', 'tip_class', 'rownum']
        df1 = pd.DataFrame([sub.split(",") for sub in data], columns = colnames)
* <span data-ttu-id="ac964-267">Změnit některé sloupce toonumeric</span><span class="sxs-lookup"><span data-stu-id="ac964-267">Change some columns toonumeric</span></span>
  
        cols_2_float = ['trip_time_in_secs','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'fare_amount', 'surcharge','mta_tax','tolls_amount','total_amount','tip_amount', 'passenger_count','trip_distance'
        ,'tipped','tip_class','rownum']
        for col in cols_2_float:
            df1[col] = df1[col].astype(float)

### <a name="build-machine-learning-models"></a><span data-ttu-id="ac964-268">Vytvářet modely machine learning</span><span class="sxs-lookup"><span data-stu-id="ac964-268">Build machine learning models</span></span>
<span data-ttu-id="ac964-269">Zde jsme sestavení toopredict modelu binární klasifikace, zda cesty je vysypávány nebo ne.</span><span class="sxs-lookup"><span data-stu-id="ac964-269">Here we build a binary classification model toopredict whether a trip is tipped or not.</span></span> <span data-ttu-id="ac964-270">V hello Poznámkový blok Jupyter můžete najít další dva modely: více třídami klasifikace a modely regrese.</span><span class="sxs-lookup"><span data-stu-id="ac964-270">In hello Jupyter Notebook you can find other two models: multiclass classification, and regression models.</span></span>

* <span data-ttu-id="ac964-271">Nejdřív potřebujeme toocreate fiktivní proměnné, které mohou být používány scikit-další modely</span><span class="sxs-lookup"><span data-stu-id="ac964-271">First we need toocreate dummy variables that can be used in scikit-learn models</span></span>
  
        df1_payment_type_dummy = pd.get_dummies(df1['payment_type'], prefix='payment_type_dummy')
        df1_vendor_id_dummy = pd.get_dummies(df1['vendor_id'], prefix='vendor_id_dummy')
* <span data-ttu-id="ac964-272">Vytvořit data snímku pro modelování hello</span><span class="sxs-lookup"><span data-stu-id="ac964-272">Create data frame for hello modeling</span></span>
  
        cols_to_keep = ['tipped', 'trip_distance', 'passenger_count']
        data = df1[cols_to_keep].join([df1_payment_type_dummy,df1_vendor_id_dummy])
  
        X = data.iloc[:,1:]
        Y = data.tipped
* <span data-ttu-id="ac964-273">Trénování a testování 60 40 rozdělení</span><span class="sxs-lookup"><span data-stu-id="ac964-273">Training and testing 60-40 split</span></span>
  
        X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.4, random_state=0)
* <span data-ttu-id="ac964-274">Logistic Regression v sadě školení</span><span class="sxs-lookup"><span data-stu-id="ac964-274">Logistic Regression in training set</span></span>
  
        model = LogisticRegression()
        logit_fit = model.fit(X_train, Y_train)
        print ('Coefficients: \n', logit_fit.coef_)
        Y_train_pred = logit_fit.predict(X_train)
  
       ![c1](./media/machine-learning-data-science-process-data-lake-walkthrough/c1-py-logit-coefficient.PNG)
* <span data-ttu-id="ac964-275">Stanovení skóre testování datové sady</span><span class="sxs-lookup"><span data-stu-id="ac964-275">Score testing data set</span></span>
  
        Y_test_pred = logit_fit.predict(X_test)
* <span data-ttu-id="ac964-276">Výpočet hodnocení metriky</span><span class="sxs-lookup"><span data-stu-id="ac964-276">Calculate Evaluation metrics</span></span>
  
        fpr_train, tpr_train, thresholds_train = metrics.roc_curve(Y_train, Y_train_pred)
        print fpr_train, tpr_train, thresholds_train
  
        fpr_test, tpr_test, thresholds_test = metrics.roc_curve(Y_test, Y_test_pred) 
        print fpr_test, tpr_test, thresholds_test
  
        #AUC
        print metrics.auc(fpr_train,tpr_train)
        print metrics.auc(fpr_test,tpr_test)
  
        #Confusion Matrix
        print metrics.confusion_matrix(Y_train,Y_train_pred)
        print metrics.confusion_matrix(Y_test,Y_test_pred)
  
       ![c2](./media/machine-learning-data-science-process-data-lake-walkthrough/c2-py-logit-evaluation.PNG)

### <a name="build-web-service-api-and-consume-it-in-python"></a><span data-ttu-id="ac964-277">Sestavení webového rozhraní API služby a využívat v Pythonu</span><span class="sxs-lookup"><span data-stu-id="ac964-277">Build Web Service API and consume it in Python</span></span>
<span data-ttu-id="ac964-278">Chceme toooperationalize hello strojového učení modelu, jakmile byl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="ac964-278">We want toooperationalize hello machine learning model after it has been built.</span></span> <span data-ttu-id="ac964-279">Tady používáme hello binární logistic model jako příklad.</span><span class="sxs-lookup"><span data-stu-id="ac964-279">Here we use hello binary logistic model as an example.</span></span> <span data-ttu-id="ac964-280">Ujistěte se, zda text hello scikit-další 0.15.1 je verze v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="ac964-280">Make sure hello scikit-learn version in your local machine is 0.15.1.</span></span> <span data-ttu-id="ac964-281">Pokud používáte službu Azure ML studio nemáte tooworry o to.</span><span class="sxs-lookup"><span data-stu-id="ac964-281">You don't have tooworry about this if you use Azure ML studio service.</span></span>

* <span data-ttu-id="ac964-282">Najít z Azure ML studio nastavení přihlašovacích údajů pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="ac964-282">Find your workspace credentials from Azure ML studio settings.</span></span> <span data-ttu-id="ac964-283">V nástroji Azure Machine Learning Studio, klikněte na tlačítko **nastavení** --> **název** --> **autorizace tokeny**.</span><span class="sxs-lookup"><span data-stu-id="ac964-283">In Azure Machine Learning Studio, click **Settings** --> **Name** --> **Authorization Tokens**.</span></span> 
  
    ![C3](./media/machine-learning-data-science-process-data-lake-walkthrough/c3-workspace-id.PNG)

        workspaceid = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'
        auth_token = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'

* <span data-ttu-id="ac964-285">Vytvoření webové služby</span><span class="sxs-lookup"><span data-stu-id="ac964-285">Create Web Service</span></span>
  
        @services.publish(workspaceid, auth_token) 
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float, payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(int) #0, or 1
        def predictNYCTAXI(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            inputArray = [trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH, payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS]
            return logit_fit.predict(inputArray)
* <span data-ttu-id="ac964-286">Získat přihlašovací údaje webové služby</span><span class="sxs-lookup"><span data-stu-id="ac964-286">Get web service credentials</span></span>
  
        url = predictNYCTAXI.service.url
        api_key =  predictNYCTAXI.service.api_key
  
        print url
        print api_key
  
        @services.service(url, api_key)
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float,payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(float)
        def NYCTAXIPredictor(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            pass
* <span data-ttu-id="ac964-287">Volání rozhraní API webové služby.</span><span class="sxs-lookup"><span data-stu-id="ac964-287">Call Web service API.</span></span> <span data-ttu-id="ac964-288">Máte toowait 5-10 sekund po hello předchozího kroku.</span><span class="sxs-lookup"><span data-stu-id="ac964-288">You have toowait 5-10 seconds after hello previous step.</span></span>
  
        NYCTAXIPredictor(1,2,1,0,0,0,0,0,1)
  
       ![c4](./media/machine-learning-data-science-process-data-lake-walkthrough/c4-call-API.PNG)

## <a name="option-2-create-and-deploy-models-directly-in-azure-machine-learning"></a><span data-ttu-id="ac964-289">Možnost 2: Vytvoření a nasazení modely přímo v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="ac964-289">Option 2: Create and deploy models directly in Azure Machine Learning</span></span>
<span data-ttu-id="ac964-290">Azure Machine Learning Studio můžete číst data přímo z Azure Data Lake Store a pak být použité toocreate a modely nasazení.</span><span class="sxs-lookup"><span data-stu-id="ac964-290">Azure Machine Learning Studio can read data directly from Azure Data Lake Store and then be used toocreate and deploy models.</span></span> <span data-ttu-id="ac964-291">Tento postup používá tabulku Hive, která odkazuje na hello Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="ac964-291">This approach uses a Hive table that points at hello Azure Data Lake Store.</span></span> <span data-ttu-id="ac964-292">To vyžaduje zřídit samostatný cluster Azure HDInsight, na které hello Hive k vytvoření tabulky.</span><span class="sxs-lookup"><span data-stu-id="ac964-292">This requires that a separate Azure HDInsight cluster be provisioned, on which hello Hive table is created.</span></span> <span data-ttu-id="ac964-293">Dobrý den, následující části zobrazit jak toodo to.</span><span class="sxs-lookup"><span data-stu-id="ac964-293">hello following sections show how toodo this.</span></span> 

### <a name="create-an-hdinsight-linux-cluster"></a><span data-ttu-id="ac964-294">Vytvoření clusteru HDInsight Linux</span><span class="sxs-lookup"><span data-stu-id="ac964-294">Create an HDInsight Linux Cluster</span></span>
<span data-ttu-id="ac964-295">Vytvoření clusteru HDInsight (Linux) z hello [portálu Azure](http://portal.azure.com). Podrobnosti najdete v tématu hello **vytvoření clusteru HDInsight s přístup tooAzure Data Lake Store** kapitoly [vytvoření clusteru HDInsight s Data Lake Store pomocí portálu Azure](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ac964-295">Create an HDInsight Cluster (Linux) from hello [Azure Portal](http://portal.azure.com).For details, see hello **Create an HDInsight cluster with access tooAzure Data Lake Store** section in [Create an HDInsight cluster with Data Lake Store using Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

 ![18](./media/machine-learning-data-science-process-data-lake-walkthrough/18-create_HDI_cluster.PNG)

### <a name="create-hive-table-in-hdinsight"></a><span data-ttu-id="ac964-297">Vytvoří tabulku Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="ac964-297">Create Hive table in HDInsight</span></span>
<span data-ttu-id="ac964-298">Nyní vytvoříme toobe tabulek Hive v clusteru HDInsight hello pomocí hello data uložená v Azure Data Lake Store v předchozím kroku hello používá v Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="ac964-298">Now we create Hive tables toobe used in Azure Machine Learning Studio in hello HDInsight cluster using hello data stored in Azure Data Lake Store in hello previous step.</span></span> <span data-ttu-id="ac964-299">Přejděte toohello clusteru HDInsight se právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="ac964-299">Go toohello HDInsight cluster just created.</span></span> <span data-ttu-id="ac964-300">Klikněte na tlačítko **nastavení** --> **vlastnosti** --> **identita AAD clusteru** --> **přístupu ADLS**, Ujistěte se, že váš účet Azure Data Lake Store je přidaný do seznamu hello čtení, zápisu a oprávnění pro spouštění.</span><span class="sxs-lookup"><span data-stu-id="ac964-300">Click **Settings** --> **Properties** --> **Cluster AAD Identity** --> **ADLS Access**, make sure your Azure Data Lake Store account is added in hello list with read, write and execute rights.</span></span> 

 ![19](./media/machine-learning-data-science-process-data-lake-walkthrough/19-HDI-cluster-add-ADLS.PNG)

<span data-ttu-id="ac964-302">Pak klikněte na tlačítko **řídicí panel** další toohello **nastavení** tlačítko a okna, objeví se.</span><span class="sxs-lookup"><span data-stu-id="ac964-302">Then click **Dashboard** next toohello **Settings** button and a window will pop up.</span></span> <span data-ttu-id="ac964-303">Klikněte na tlačítko **zobrazení Hive** v hello pravém horním rohu stránky hello a uvidí hello **Editor dotazů**.</span><span class="sxs-lookup"><span data-stu-id="ac964-303">Click **Hive View** in hello upper right corner of hello page and you will see hello **Query Editor**.</span></span>

 ![20](./media/machine-learning-data-science-process-data-lake-walkthrough/20-HDI-dashboard.PNG)

 ![21](./media/machine-learning-data-science-process-data-lake-walkthrough/21-Hive-Query-Editor-v2.PNG)

<span data-ttu-id="ac964-306">Vložte následující Hive skripty toocreate hello tabulku.</span><span class="sxs-lookup"><span data-stu-id="ac964-306">Paste in hello following Hive scripts toocreate a table.</span></span> <span data-ttu-id="ac964-307">Hello umístění zdroje dat je v Azure Data Lake Store odkaz tímto způsobem: **adl://data_lake_store_name.azuredatalakestore.net:443/název_složky/název_souboru**.</span><span class="sxs-lookup"><span data-stu-id="ac964-307">hello location of data source is in Azure Data Lake Store reference in this way: **adl://data_lake_store_name.azuredatalakestore.net:443/folder_name/file_name**.</span></span>

    CREATE EXTERNAL TABLE nyc_stratified_sample
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string,
      payment_type string,
      fare_amount string,
      surcharge string,
      mta_tax string,
      tolls_amount string,
      total_amount string,
      tip_amount string,
      tipped string,
      tip_class string,
      rownum string
      )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    LOCATION 'adl://data_lake_storage_name.azuredatalakestore.net:443/nyctaxi_folder/demo_ex_9_stratified_1_1000_copy.csv';


<span data-ttu-id="ac964-308">Po dokončení spuštění dotazu hello uvidíte výsledky hello takto:</span><span class="sxs-lookup"><span data-stu-id="ac964-308">When hello query finishes running, you will see hello results like this:</span></span>

 ![22](./media/machine-learning-data-science-process-data-lake-walkthrough/22-Hive-Query-results.PNG)

### <a name="build-and-deploy-models-in-azure-machine-learning-studio"></a><span data-ttu-id="ac964-310">Vytváření a nasazování modely v Azure Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="ac964-310">Build and deploy models in Azure Machine Learning Studio</span></span>
<span data-ttu-id="ac964-311">Jsme jsou teď připravena toobuild a nasadit model, který předpovídá, zda je tip placené pomocí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="ac964-311">We are now ready toobuild and deploy a model that predicts whether or not a tip is paid with Azure Machine Learning.</span></span> <span data-ttu-id="ac964-312">Hello vrstveného ukázková data jsou připravena toobe použít v této binární klasifikace (tip nebo ne) problém.</span><span class="sxs-lookup"><span data-stu-id="ac964-312">hello stratified sample data is ready toobe used in this binary classification (tip or not) problem.</span></span> <span data-ttu-id="ac964-313">Hello prediktivní modely pomocí více třídami klasifikace (tip_class) a regrese (tip_amount) můžete také být vytvořené a nasazené se službou Azure Machine Learning Studio, ale tady jenom ukážeme, jak toohandle hello případu použití hello binární klasifikace modelu.</span><span class="sxs-lookup"><span data-stu-id="ac964-313">hello predictive models using multiclass classification (tip_class) and regression (tip_amount) can also be built and deployed with Azure Machine Learning Studio, but here we only show how toohandle hello case using hello binary classification model.</span></span>

1. <span data-ttu-id="ac964-314">Získat hello data do Azure ML pomocí hello **importovat Data** modulu, k dispozici v hello **vstupu a výstupu dat** části.</span><span class="sxs-lookup"><span data-stu-id="ac964-314">Get hello data into Azure ML using hello **Import Data** module, available in hello **Data Input and Output** section.</span></span> <span data-ttu-id="ac964-315">Další informace najdete v tématu hello [importovat Data modulu](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) stránka s referencemi k.</span><span class="sxs-lookup"><span data-stu-id="ac964-315">For more information, see hello [Import Data module](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) reference page.</span></span>
2. <span data-ttu-id="ac964-316">Vyberte **dotaz Hive** jako hello **zdroj dat** v hello **vlastnosti** panelu.</span><span class="sxs-lookup"><span data-stu-id="ac964-316">Select **Hive Query** as hello **Data source** in hello **Properties** panel.</span></span>
3. <span data-ttu-id="ac964-317">Hello vložte následující skript Hive v hello **databázový dotaz Hive** editoru</span><span class="sxs-lookup"><span data-stu-id="ac964-317">Paste hello following Hive script in hello **Hive database query** editor</span></span>
   
        select * from nyc_stratified_sample;
4. <span data-ttu-id="ac964-318">Zadejte cluster URI HDInsight hello (lze najít na portálu Azure), přihlašovací údaje Hadoop, umístění výstupních dat a název název / / kontejner klíče účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="ac964-318">Enter hello URI of HDInsight cluster (this can be found in Azure Portal), Hadoop credentials, location of output data, and Azure storage account name/key/container name.</span></span>
   
   ![23](./media/machine-learning-data-science-process-data-lake-walkthrough/23-reader-module-v3.PNG)  

<span data-ttu-id="ac964-320">Obrázek hello je uveden příklad binární klasifikace pokusu se čtení dat z tabulky Hive.</span><span class="sxs-lookup"><span data-stu-id="ac964-320">An example of a binary classification experiment reading data from Hive table is shown in hello figure below.</span></span>

 ![24](./media/machine-learning-data-science-process-data-lake-walkthrough/24-AML-exp.PNG)

<span data-ttu-id="ac964-322">Po vytvoření hello experimentu klikněte na tlačítko **nastavit webové služby** --> **prediktivní webové služby**</span><span class="sxs-lookup"><span data-stu-id="ac964-322">After hello experiment is created, click  **Set Up Web Service** --> **Predictive Web Service**</span></span>

 ![25](./media/machine-learning-data-science-process-data-lake-walkthrough/25-AML-exp-deploy.PNG)

<span data-ttu-id="ac964-324">Vyhodnocování experiment, po dokončení klikněte na tlačítko Spustit hello automaticky vytvoří **nasazení webové služby**</span><span class="sxs-lookup"><span data-stu-id="ac964-324">Run hello automatically created scoring experiment, when it finishes, click **Deploy Web Service**</span></span>

 ![26](./media/machine-learning-data-science-process-data-lake-walkthrough/26-AML-exp-deploy-web.PNG)

<span data-ttu-id="ac964-326">řídicí panel Hello webové služby se zobrazí po chvíli:</span><span class="sxs-lookup"><span data-stu-id="ac964-326">hello web service dashboard will be displayed shortly:</span></span>

 ![27](./media/machine-learning-data-science-process-data-lake-walkthrough/27-AML-web-api.PNG)

## <a name="summary"></a><span data-ttu-id="ac964-328">Souhrn</span><span class="sxs-lookup"><span data-stu-id="ac964-328">Summary</span></span>
<span data-ttu-id="ac964-329">Provedením tohoto návodu jste vytvořili data vědecké účely prostředí pro vytváření škálovatelných začátku do konce řešení v Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="ac964-329">By completing this walkthrough you have created a data science environment for building scalable end-to-end solutions in Azure Data Lake.</span></span> <span data-ttu-id="ac964-330">Toto prostředí bylo použité tooanalyze velké veřejné datové sady, trvá prostřednictvím hello kanonický kroky hello proces vědecké účely dat, z získávání dat prostřednictvím modelu školení, a potom toohello nasazení hello model jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="ac964-330">This environment was used tooanalyze a large public dataset, taking it through hello canonical steps of hello Data Science Process, from data acquisition through model training, and then toohello deployment of hello model as a web service.</span></span> <span data-ttu-id="ac964-331">U-SQL bylo použité tooprocess, prozkoumejte a vzorová hello data.</span><span class="sxs-lookup"><span data-stu-id="ac964-331">U-SQL was used tooprocess, explore and sample hello data.</span></span> <span data-ttu-id="ac964-332">Python a Hive se použít s Azure Machine Learning Studio toobuild a nasazovat prediktivní modely.</span><span class="sxs-lookup"><span data-stu-id="ac964-332">Python and Hive were used with Azure Machine Learning Studio toobuild and deploy predictive models.</span></span>

## <a name="whats-next"></a><span data-ttu-id="ac964-333">Co dále?</span><span class="sxs-lookup"><span data-stu-id="ac964-333">What's next?</span></span>
<span data-ttu-id="ac964-334">Hello kurzů pro [tým datové vědy procesu (TDSP)](http://aka.ms/datascienceprocess) poskytuje tootopics odkazy, které popisují každou krok v hello pokročilé analýzy procesu.</span><span class="sxs-lookup"><span data-stu-id="ac964-334">hello learning path for the [Team Data Science Process (TDSP)](http://aka.ms/datascienceprocess) provides links tootopics describing each step in hello advanced analytics process.</span></span> <span data-ttu-id="ac964-335">Existuje řada návodů uvedeno na hello [proces vědecké účely dat Team návody](data-science-process-walkthroughs.md) stránce tohoto představením jak toouse služby v různé scénáře prediktivní analýzy a prostředky:</span><span class="sxs-lookup"><span data-stu-id="ac964-335">There are a series of walkthroughs itemized on hello [Team Data Science Process walkthroughs](data-science-process-walkthroughs.md) page that showcase how toouse resources and services in various predictive analytics scenarios:</span></span>

* [<span data-ttu-id="ac964-336">Hello proces vědecké účely Team dat v akci: pomocí SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="ac964-336">hello Team Data Science Process in action: using SQL Data Warehouse</span></span>](machine-learning-data-science-process-sqldw-walkthrough.md)
* [<span data-ttu-id="ac964-337">Hello proces vědecké účely Team dat v akci: pomocí clusterů systému HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="ac964-337">hello Team Data Science Process in action: using HDInsight Hadoop clusters</span></span>](machine-learning-data-science-process-hive-walkthrough.md)
* [<span data-ttu-id="ac964-338">Hello proces vědecké účely dat Team: pomocí SQL serveru</span><span class="sxs-lookup"><span data-stu-id="ac964-338">hello Team Data Science Process: using SQL Server</span></span>](machine-learning-data-science-process-sql-walkthrough.md)
* [<span data-ttu-id="ac964-339">Přehled hello proces vědecké účely dat pomocí Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="ac964-339">Overview of hello Data Science Process using Spark on Azure HDInsight</span></span>](machine-learning-data-science-spark-overview.md)

