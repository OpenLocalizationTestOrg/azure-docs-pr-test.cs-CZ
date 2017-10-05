---
title: "Škálovatelné vědecké zpracování dat pomocí Azure Data Lake: návod začátku do konce | Microsoft Docs"
description: "Jak používat Azure Data Lake udělat zkoumání a binární klasifikace úlohy dat na datovou sadu."
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
ms.openlocfilehash: 34fbe99572b4a6cee73de6ae5412a0ec09dd1ccc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="scalable-data-science-with-azure-data-lake-an-end-to-end-walkthrough"></a><span data-ttu-id="37455-103">Škálovatelné vědecké zpracování dat pomocí Azure Data Lake: návod začátku do konce</span><span class="sxs-lookup"><span data-stu-id="37455-103">Scalable Data Science with Azure Data Lake: An end-to-end Walkthrough</span></span>
<span data-ttu-id="37455-104">Tento návod ukazuje, jak používat Azure Data Lake a zkoumání dat a binární klasifikace úlohy na ukázku cesty taxíkem NYC jízdenky datová sada k předvídání, zda budeme tip platit tarif.</span><span class="sxs-lookup"><span data-stu-id="37455-104">This walkthrough shows how to use Azure Data Lake to do data exploration and binary classification tasks on a sample of the NYC taxi trip and fare dataset to predict whether or not a tip will be paid by a fare.</span></span> <span data-ttu-id="37455-105">Provede vás provede postupem [proces vědecké účely dat Team](http://aka.ms/datascienceprocess), klient server, získávání dat pro modelování školení a pak do nasazení webové služby, který publikuje modelu.</span><span class="sxs-lookup"><span data-stu-id="37455-105">It walks you through the steps of the [Team Data Science Process](http://aka.ms/datascienceprocess), end-to-end, from data acquisition to model training, and then to the deployment of a web service that publishes the model.</span></span>

### <a name="azure-data-lake-analytics"></a><span data-ttu-id="37455-106">Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="37455-106">Azure Data Lake Analytics</span></span>
<span data-ttu-id="37455-107">[Microsoft Azure Data Lake](https://azure.microsoft.com/solutions/data-lake/) má všechny možnosti potřeba usnadňují datových vědců k ukládání dat všech velikost, tvar a rychlost a ke zpracování dat, pokročilé analýzy a modelování s vysokou strojové učení škálovatelnost nákladově efektivní způsobem.</span><span class="sxs-lookup"><span data-stu-id="37455-107">The [Microsoft Azure Data Lake](https://azure.microsoft.com/solutions/data-lake/) has all the capabilities required to make it easy for data scientists to store data of any size, shape and speed, and to conduct data processing, advanced analytics, and machine learning modeling with high scalability in a cost-effective way.</span></span>   <span data-ttu-id="37455-108">Platíte na základě na úlohu jenom v případě, že data ve skutečnosti probíhá zpracování.</span><span class="sxs-lookup"><span data-stu-id="37455-108">You pay on a per-job basis, only when data is actually being processed.</span></span> <span data-ttu-id="37455-109">Azure Data Lake Analytics obsahuje U-SQL, jazyk, který smíchá deklarativní charakter jazyka SQL o výrazovou sílu jazyka C# k poskytování škálovatelných distribuovaných možnost dotazu.</span><span class="sxs-lookup"><span data-stu-id="37455-109">Azure Data Lake Analytics includes U-SQL, a language that blends the declarative nature of SQL with the expressive power of C# to provide scalable distributed query capability.</span></span> <span data-ttu-id="37455-110">Umožňuje zpracování nestrukturovaných dat s použitím schématu na čtení, vložení vlastní logiky a uživatelem definované funkce (UDF) a zahrnuje rozšíření povolit podrobné kontrolu nad postup provést ve velkém měřítku.</span><span class="sxs-lookup"><span data-stu-id="37455-110">It enables you to process unstructured data by applying schema on read, insert custom logic and user defined functions (UDFs), and includes extensibility to enable fine grained control over how to execute at scale.</span></span> <span data-ttu-id="37455-111">Další informace o filosofie návrhu tříd za U-SQL najdete v tématu [Visual Studio příspěvku na blogu](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).</span><span class="sxs-lookup"><span data-stu-id="37455-111">To learn more about the design philosophy behind U-SQL, see [Visual Studio blog post](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).</span></span>

<span data-ttu-id="37455-112">Služba Data Lake Analytics je také klíčovou součástí Cortana Analytics Suite a spolupracuje se službami Azure SQL Data Warehouse, Power BI a Data Factory.</span><span class="sxs-lookup"><span data-stu-id="37455-112">Data Lake Analytics is also a key part of Cortana Analytics Suite and works with Azure SQL Data Warehouse, Power BI, and Data Factory.</span></span> <span data-ttu-id="37455-113">To vám dává kompletní cloud velkých objemů dat a platformy pokročilou analýzu.</span><span class="sxs-lookup"><span data-stu-id="37455-113">This gives you a complete cloud big data and advanced analytics platform.</span></span>

<span data-ttu-id="37455-114">Tento názorný postup začíná popisující požadavky a prostředky, které jsou nutné k dokončení úlohy s Data Lake Analytics, která tvoří proces vědecké účely dat a jak je nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="37455-114">This walkthrough begins by describing the prerequisites and resources that are needed to complete the tasks with Data Lake Analytics that form the data science process and how to install them.</span></span> <span data-ttu-id="37455-115">Pak se popisuje postup zpracování dat pomocí U-SQL a ukazuje, jak používat Python a Hive se ukončí s Azure Machine Learning Studio vytvářet a nasazovat prediktivní modely.</span><span class="sxs-lookup"><span data-stu-id="37455-115">Then it outlines the data processing steps using U-SQL and concludes by showing how to use Python and Hive with Azure Machine Learning Studio to build and deploy the predictive models.</span></span> 

### <a name="u-sql-and-visual-studio"></a><span data-ttu-id="37455-116">U-SQL Server a Visual Studio</span><span class="sxs-lookup"><span data-stu-id="37455-116">U-SQL and Visual Studio</span></span>
<span data-ttu-id="37455-117">Tento názorný postup se doporučuje pomocí sady Visual Studio upravit skriptů U-SQL zpracovat datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="37455-117">This walkthrough recommends using Visual Studio to edit U-SQL scripts to process the dataset.</span></span> <span data-ttu-id="37455-118">Skriptů U-SQL jsou zde popsané a poskytuje v samostatném souboru.</span><span class="sxs-lookup"><span data-stu-id="37455-118">The U-SQL scripts are described here and provided in a separate file.</span></span> <span data-ttu-id="37455-119">Proces zahrnuje příjem, zkoumat a vzorkování data.</span><span class="sxs-lookup"><span data-stu-id="37455-119">The process includes ingesting, exploring, and sampling the data.</span></span> <span data-ttu-id="37455-120">Také ukazuje, jak spustit úlohu skripty U-SQL z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="37455-120">It also shows how to run a U-SQL scripted job from the Azure portal.</span></span> <span data-ttu-id="37455-121">Pro data v clusteru HDInsight přidružené k usnadnění vytváření a nasazení modelu binární klasifikace v Azure Machine Learning Studio se vytvoří tabulek Hive.</span><span class="sxs-lookup"><span data-stu-id="37455-121">Hive tables are created for the data in an associated HDInsight cluster to facilitate the building and deployment of a binary classification model in Azure Machine Learning Studio.</span></span>  

### <a name="python"></a><span data-ttu-id="37455-122">Python</span><span class="sxs-lookup"><span data-stu-id="37455-122">Python</span></span>
<span data-ttu-id="37455-123">Tento názorný postup obsahuje také oddíl, který ukazuje, jak vytvářet a nasazovat prediktivní model pomocí Azure Machine Learning Studio Python.</span><span class="sxs-lookup"><span data-stu-id="37455-123">This walkthrough also contains a section that shows how to build and deploy a predictive model using Python with Azure Machine Learning Studio.</span></span>  <span data-ttu-id="37455-124">Poskytujeme poznámkového bloku Jupyter pomocí skriptů Python pro tyto kroky v tomto procesu.</span><span class="sxs-lookup"><span data-stu-id="37455-124">We provide a Jupyter notebook with the Python scripts for these steps in this process.</span></span> <span data-ttu-id="37455-125">Poznámkový blok obsahuje kód pro některé další funkce engineering kroky a modely konstrukce například více třídami klasifikace a regresní modelování kromě modelu binární klasifikace podle zde uvedeného.</span><span class="sxs-lookup"><span data-stu-id="37455-125">The notebook includes code for some additional feature engineering steps and models construction such as multiclass classification and regression modeling in addition to the binary classification model outlined here.</span></span> <span data-ttu-id="37455-126">Tato úloha regrese je k předvídání množství tip podle dalších funkcí tip.</span><span class="sxs-lookup"><span data-stu-id="37455-126">The regression task is to predict the amount of the tip based on other tip features.</span></span> 

### <a name="azure-machine-learning"></a><span data-ttu-id="37455-127">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="37455-127">Azure Machine Learning</span></span>
<span data-ttu-id="37455-128">Azure Machine Learning Studio je umožňuje vytvářet a nasazovat prediktivní modely.</span><span class="sxs-lookup"><span data-stu-id="37455-128">Azure Machine Learning Studio is used to build and deploy the predictive models.</span></span> <span data-ttu-id="37455-129">To se provádí pomocí dva přístupy: nejdřív se skriptů Pythonu a potom se tabulek Hive v clusteru HDInsight (Hadoop).</span><span class="sxs-lookup"><span data-stu-id="37455-129">This is done using two approaches: first with Python scripts and then with Hive tables on an HDInsight (Hadoop) cluster.</span></span>

### <a name="scripts"></a><span data-ttu-id="37455-130">Skripty</span><span class="sxs-lookup"><span data-stu-id="37455-130">Scripts</span></span>
<span data-ttu-id="37455-131">V tomto názorném postupu jsou uvedeny pouze hlavní kroky.</span><span class="sxs-lookup"><span data-stu-id="37455-131">Only the principal steps are outlined in this walkthrough.</span></span> <span data-ttu-id="37455-132">Si můžete stáhnout kompletní **skript U-SQL** a **Poznámkový blok Jupyter** z [Githubu](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).</span><span class="sxs-lookup"><span data-stu-id="37455-132">You can download the full **U-SQL script** and **Jupyter Notebook** from [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="37455-133">Požadavky</span><span class="sxs-lookup"><span data-stu-id="37455-133">Prerequisites</span></span>
<span data-ttu-id="37455-134">Před zahájením těchto témat, musíte mít následující:</span><span class="sxs-lookup"><span data-stu-id="37455-134">Before you begin these topics, you must have the following:</span></span>

* <span data-ttu-id="37455-135">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="37455-135">An Azure subscription.</span></span> <span data-ttu-id="37455-136">Pokud není již nemáte, přečtěte si téma [získání bezplatné zkušební verze Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="37455-136">If you do not already have one, see [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="37455-137">[Doporučeno] Visual Studio 2013 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="37455-137">[Recommended] Visual Studio 2013 or later.</span></span> <span data-ttu-id="37455-138">Pokud jste ještě není jedním z těchto verzí, můžete stáhnout bezplatnou verzi komunity z [Visual Studio Community](https://www.visualstudio.com/vs/community/).</span><span class="sxs-lookup"><span data-stu-id="37455-138">If you do not already have one of these versions installed, you can download a free Community version from [Visual Studio Community](https://www.visualstudio.com/vs/community/).</span></span>

> [!NOTE]
> <span data-ttu-id="37455-139">Místo Visual Studio můžete také použít portál Azure k odesílání dotazů Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="37455-139">Instead of Visual Studio, you can also use the Azure Portal to submit Azure Data Lake queries.</span></span> <span data-ttu-id="37455-140">Poskytujeme pokyny o tom, jak udělat, tak i pomocí sady Visual Studio a na portálu v části s názvem **zpracování dat pomocí U-SQL**.</span><span class="sxs-lookup"><span data-stu-id="37455-140">We will provide instructions on how to do so both with Visual Studio and on the portal in the section titled **Process data with U-SQL**.</span></span> 
> 
> 


## <a name="prepare-data-science-environment-for-azure-data-lake"></a><span data-ttu-id="37455-141">Příprava dat vědecké účely prostředí pro Azure Data Lake</span><span class="sxs-lookup"><span data-stu-id="37455-141">Prepare data science environment for Azure Data Lake</span></span>
<span data-ttu-id="37455-142">Příprava prostředí vědecké účely data v tomto návodu, vytvořte v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="37455-142">To prepare the data science environment for this walkthrough, create the following resources:</span></span>

* <span data-ttu-id="37455-143">Azure Data Lake Store (ADLS)</span><span class="sxs-lookup"><span data-stu-id="37455-143">Azure Data Lake Store (ADLS)</span></span> 
* <span data-ttu-id="37455-144">Azure Data Lake Analytics (ADLA)</span><span class="sxs-lookup"><span data-stu-id="37455-144">Azure Data Lake Analytics (ADLA)</span></span>
* <span data-ttu-id="37455-145">Účet služby Azure Blob storage</span><span class="sxs-lookup"><span data-stu-id="37455-145">Azure Blob storage account</span></span>
* <span data-ttu-id="37455-146">Účet Azure Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="37455-146">Azure Machine Learning Studio account</span></span>
* <span data-ttu-id="37455-147">Nástroje Azure Data Lake pro Visual Studio (doporučeno)</span><span class="sxs-lookup"><span data-stu-id="37455-147">Azure Data Lake Tools for Visual Studio (Recommended)</span></span>

<span data-ttu-id="37455-148">Tato část obsahuje pokyny k vytvoření každý z těchto prostředků.</span><span class="sxs-lookup"><span data-stu-id="37455-148">This section provides instructions on how to create each of these resources.</span></span> <span data-ttu-id="37455-149">Pokud chcete použít tabulek Hive pomocí Azure Machine Learning, místo Python, k vytvoření modelu, také musíte se ke zřízení clusteru služby HDInsight (Hadoop).</span><span class="sxs-lookup"><span data-stu-id="37455-149">If you choose to use Hive tables with Azure Machine Learning, instead of Python, to build a model,you will also need to provision an HDInsight (Hadoop) cluster.</span></span> <span data-ttu-id="37455-150">Tento alternativní postup v popsané v následující části.</span><span class="sxs-lookup"><span data-stu-id="37455-150">This alternative procedure in described in the appropriate section below.</span></span>


> [!NOTE]
> <span data-ttu-id="37455-151">**Azure Data Lake Store** je možné vytvořit buď samostatně nebo při vytváření **Azure Data Lake Analytics** jako výchozí úložiště.</span><span class="sxs-lookup"><span data-stu-id="37455-151">The **Azure Data Lake Store** can be created either separately or when you create the **Azure Data Lake Analytics** as the default storage.</span></span> <span data-ttu-id="37455-152">Odkazují pokyny pro vytvoření každý z těchto prostředků samostatně níže, ale nemusí být samostatně vytvoření účtu úložiště Data Lake.</span><span class="sxs-lookup"><span data-stu-id="37455-152">Instructions are referenced for creating each of these resources separately below, but the Data Lake storage account need not be created separately.</span></span>
>
> 

### <a name="create-an-azure-data-lake-store"></a><span data-ttu-id="37455-153">Vytvoření Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="37455-153">Create an Azure Data Lake Store</span></span>


<span data-ttu-id="37455-154">Vytvoření ADLS z [portál Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="37455-154">Create an ADLS from the [Azure Portal](http://portal.azure.com).</span></span> <span data-ttu-id="37455-155">Podrobnosti najdete v tématu [vytvoření clusteru HDInsight s Data Lake Store pomocí portálu Azure](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="37455-155">For details, see [Create an HDInsight cluster with Data Lake Store using Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span> <span data-ttu-id="37455-156">Nezapomeňte nastavit identita AAD clusteru v **DataSource** okno **volitelné konfiguraci** okna popsané existuje.</span><span class="sxs-lookup"><span data-stu-id="37455-156">Be sure to set up the Cluster AAD Identity in the **DataSource** blade of the **Optional Configuration** blade described there.</span></span> 

 ![3](./media/machine-learning-data-science-process-data-lake-walkthrough/3-create-ADLS.PNG)

### <a name="create-an-azure-data-lake-analytics-account"></a><span data-ttu-id="37455-158">Vytvoření účtu Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="37455-158">Create an Azure Data Lake Analytics account</span></span>
<span data-ttu-id="37455-159">Vytvoření účtu ADLA z [portálu Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="37455-159">Create an ADLA account from the [Azure Portal](http://portal.azure.com).</span></span> <span data-ttu-id="37455-160">Podrobnosti najdete v tématu [kurz: Začínáme s Azure Data Lake Analytics pomocí portálu Azure](../data-lake-analytics/data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="37455-160">For details, see [Tutorial: get started with Azure Data Lake Analytics using Azure Portal](../data-lake-analytics/data-lake-analytics-get-started-portal.md).</span></span> 

 ![4](./media/machine-learning-data-science-process-data-lake-walkthrough/4-create-ADLA-new.PNG)

### <a name="create-an-azure-blob-storage-account"></a><span data-ttu-id="37455-162">Vytvoření účtu úložiště objektů Blob v Azure</span><span class="sxs-lookup"><span data-stu-id="37455-162">Create an Azure Blob storage account</span></span>
<span data-ttu-id="37455-163">Vytvoření účtu úložiště objektů Blob v Azure z [portálu Azure](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="37455-163">Create an Azure Blob storage account from the [Azure Portal](http://portal.azure.com).</span></span> <span data-ttu-id="37455-164">Podrobnosti najdete v tématu vytvořením účtu úložiště v tématu v [účty Azure storage](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="37455-164">For details, see the Create a storage account section in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>

 ![5](./media/machine-learning-data-science-process-data-lake-walkthrough/5-Create-Azure-Blob.PNG)

### <a name="set-up-an-azure-machine-learning-studio-account"></a><span data-ttu-id="37455-166">Nastavit účet Azure Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="37455-166">Set up an Azure Machine Learning Studio account</span></span>
<span data-ttu-id="37455-167">Přihlášení/do Azure Machine Learning Studio z [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) stránky.</span><span class="sxs-lookup"><span data-stu-id="37455-167">Sign up/into Azure Machine Learning Studio from the [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) page.</span></span> <span data-ttu-id="37455-168">Klikněte na **začít nyní** tlačítko a pak zvolte "Volného prostoru" nebo "Standardní pracovní prostor".</span><span class="sxs-lookup"><span data-stu-id="37455-168">Click on the **Get started now** button and then choose a "Free Workspace" or "Standard Workspace".</span></span> <span data-ttu-id="37455-169">Potom bude moct vytvářet experimenty v Azure ML Studio.</span><span class="sxs-lookup"><span data-stu-id="37455-169">After this you will be able to create experiments in Azure ML Studio.</span></span>  

### <a name="install-azure-data-lake-tools-recommended"></a><span data-ttu-id="37455-170">Instalace nástroje Azure Data Lake [doporučeno]</span><span class="sxs-lookup"><span data-stu-id="37455-170">Install Azure Data Lake Tools [Recommended]</span></span>
<span data-ttu-id="37455-171">Instalace nástroje Azure Data Lake pro vaši verzi sady Visual Studio z [nástrojů Azure Data Lake pro Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span><span class="sxs-lookup"><span data-stu-id="37455-171">Install Azure Data Lake Tools for your version of Visual Studio from [Azure Data Lake Tools for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span></span>

 ![6](./media/machine-learning-data-science-process-data-lake-walkthrough/6-install-ADL-tools-VS.PNG)

<span data-ttu-id="37455-173">Po úspěšném dokončení instalace, otevřete Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="37455-173">After the installation finishes successfully, open up Visual Studio.</span></span> <span data-ttu-id="37455-174">Měli byste vidět Data Lake, klikněte v nabídce v horní části.</span><span class="sxs-lookup"><span data-stu-id="37455-174">You should see the Data Lake tab the menu at the top.</span></span> <span data-ttu-id="37455-175">Vašich prostředků Azure mají objevit v levém panelu, když se přihlásíte ke svému účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="37455-175">Your Azure resources should appear in the left panel when you sign into your Azure account.</span></span>

 ![7](./media/machine-learning-data-science-process-data-lake-walkthrough/7-install-ADL-tools-VS-done.PNG)

## <a name="the-nyc-taxi-trips-dataset"></a><span data-ttu-id="37455-177">Datová sada NYC taxíkem cest</span><span class="sxs-lookup"><span data-stu-id="37455-177">The NYC Taxi Trips dataset</span></span>
<span data-ttu-id="37455-178">Sada dat jsme použili v tomto poli je veřejně dostupné datové sady – [datovou sadu cest taxíkem NYC](http://www.andresmh.com/nyctaxitrips/).</span><span class="sxs-lookup"><span data-stu-id="37455-178">The data set we used here is a publicly available dataset -- the [NYC Taxi Trips dataset](http://www.andresmh.com/nyctaxitrips/).</span></span> <span data-ttu-id="37455-179">Data NYC taxíkem cesty se skládá z přibližně 20GB komprimovaných souborů CSV (nekomprimovaným ~ 48GB), zaznamenávání 173 milionů jednotlivých cest a tarify placené pro každou cestu.</span><span class="sxs-lookup"><span data-stu-id="37455-179">The NYC Taxi Trip data consists of about 20GB of compressed CSV files (~48GB uncompressed), recording more than 173 million individual trips and the fares paid for each trip.</span></span> <span data-ttu-id="37455-180">Každý záznam cestě zahrnuje vyzvednutí a odkládací umístění a časy, anonymizovaná hackerský (ovladač) číslo licence a číslo Medailon (taxi na jedinečné id).</span><span class="sxs-lookup"><span data-stu-id="37455-180">Each trip record includes the pickup and drop-off locations and times, anonymized hack (driver's) license number, and the medallion (taxi’s unique id) number.</span></span> <span data-ttu-id="37455-181">Data obsahuje všechny služebních cest v roku 2013 a je dostupné pro každý měsíc následující dvě datové sady:</span><span class="sxs-lookup"><span data-stu-id="37455-181">The data covers all trips in the year 2013 and is provided in the following two datasets for each month:</span></span>

* <span data-ttu-id="37455-182">'trip_data' CSV obsahuje podrobnosti o cestě, například na počtu cestujících, vyzvednutí a dropoff body, doba trvání cesty a délka cesty.</span><span class="sxs-lookup"><span data-stu-id="37455-182">The 'trip_data' CSV contains trip details, such as number of passengers, pickup and dropoff points, trip duration, and trip length.</span></span> <span data-ttu-id="37455-183">Tady je několik ukázkových záznamů:</span><span class="sxs-lookup"><span data-stu-id="37455-183">Here are a few sample records:</span></span>
  
       medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count, trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
       89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
* <span data-ttu-id="37455-184">Trip_fare CSV obsahující podrobnosti o tarif placené pro každou cestu, například typ platby, velikost tarif, příplatek a daně, tipy a mýtné, a celkovou velikost placené.</span><span class="sxs-lookup"><span data-stu-id="37455-184">The 'trip_fare' CSV contains details of the fare paid for each trip, such as payment type, fare amount, surcharge and taxes, tips and tolls, and the total amount paid.</span></span> <span data-ttu-id="37455-185">Tady je několik ukázkových záznamů:</span><span class="sxs-lookup"><span data-stu-id="37455-185">Here are a few sample records:</span></span>
  
       medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
       89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

<span data-ttu-id="37455-186">Jedinečný klíč pro připojení k cestě\_dat a cesty\_tarif se skládá z následující tři pole: medailonu, hackerský\_licence a vyzvednutí\_data a času.</span><span class="sxs-lookup"><span data-stu-id="37455-186">The unique key to join trip\_data and trip\_fare is composed of the following three fields: medallion, hack\_license and pickup\_datetime.</span></span> <span data-ttu-id="37455-187">Nezpracované soubory CSV jsou přístupné z veřejného úložiště Azure blob.</span><span class="sxs-lookup"><span data-stu-id="37455-187">The raw CSV files can be accessed from a public Azure storage blob.</span></span> <span data-ttu-id="37455-188">Skript U-SQL pro toto připojení je v [připojení k cestě a tarif tabulky](#join) části.</span><span class="sxs-lookup"><span data-stu-id="37455-188">The U-SQL script for this join is in the [Join trip and fare tables](#join) section.</span></span>

## <a name="process-data-with-u-sql"></a><span data-ttu-id="37455-189">Zpracování dat pomocí U-SQL</span><span class="sxs-lookup"><span data-stu-id="37455-189">Process data with U-SQL</span></span>
<span data-ttu-id="37455-190">Úlohy zpracování dat v této části zahrnují příjem, kontrolu kvality, prohlížení a vzorkování data.</span><span class="sxs-lookup"><span data-stu-id="37455-190">The data processing tasks illustrated in this section include ingesting, checking quality, exploring, and sampling the data.</span></span> <span data-ttu-id="37455-191">Také jsme ukazují, jak připojit k cestě a tarif tabulky.</span><span class="sxs-lookup"><span data-stu-id="37455-191">We also show how to join trip and fare tables.</span></span> <span data-ttu-id="37455-192">Koncová část ukazuje spuštění úlohy pomocí skriptu U-SQL z portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="37455-192">The final section shows run a U-SQL scripted job from the Azure portal.</span></span> <span data-ttu-id="37455-193">Tady jsou odkazy na každou část:</span><span class="sxs-lookup"><span data-stu-id="37455-193">Here are links to each subsection:</span></span>

* [<span data-ttu-id="37455-194">Přijímání dat: přečíst data z veřejného objektu blob</span><span class="sxs-lookup"><span data-stu-id="37455-194">Data ingestion: read in data from public blob</span></span>](#ingest)
* [<span data-ttu-id="37455-195">Kontrolu kvality dat</span><span class="sxs-lookup"><span data-stu-id="37455-195">Data quality checks</span></span>](#quality)
* [<span data-ttu-id="37455-196">Zkoumání dat</span><span class="sxs-lookup"><span data-stu-id="37455-196">Data exploration</span></span>](#explore)
* [<span data-ttu-id="37455-197">Připojení k cestě a tarif tabulky</span><span class="sxs-lookup"><span data-stu-id="37455-197">Join trip and fare tables</span></span>](#join)
* [<span data-ttu-id="37455-198">Vzorkování dat</span><span class="sxs-lookup"><span data-stu-id="37455-198">Data sampling</span></span>](#sample)
* [<span data-ttu-id="37455-199">Spuštění úloh U-SQL</span><span class="sxs-lookup"><span data-stu-id="37455-199">Run U-SQL jobs</span></span>](#run)

<span data-ttu-id="37455-200">Skriptů U-SQL jsou zde popsané a poskytuje v samostatném souboru.</span><span class="sxs-lookup"><span data-stu-id="37455-200">The U-SQL scripts are described here and provided in a separate file.</span></span> <span data-ttu-id="37455-201">Si můžete stáhnout kompletní **skriptů U-SQL** z [Githubu](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).</span><span class="sxs-lookup"><span data-stu-id="37455-201">You can download the full **U-SQL scripts** from [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).</span></span>

<span data-ttu-id="37455-202">Spuštění U-SQL, otevřete Visual Studio, klikněte na tlačítko **souboru--> Nový Projekt-->**, zvolte **projekt U-SQL**, název a uložte je do složky.</span><span class="sxs-lookup"><span data-stu-id="37455-202">To execute U-SQL, Open Visual Studio, click **File --> New --> Project**, choose **U-SQL Project**, name and save it to a folder.</span></span>

![8](./media/machine-learning-data-science-process-data-lake-walkthrough/8-create-USQL-project.PNG)

> [!NOTE]
> <span data-ttu-id="37455-204">Je možné pomocí webu Azure Portal spuštění U-SQL místo sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="37455-204">It is possible to use the Azure Portal to execute U-SQL instead of Visual Studio.</span></span> <span data-ttu-id="37455-205">Můžete přejít k prostředku Azure Data Lake Analytics na portálu a odesílání dotazů přímo jako ilustrované na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="37455-205">You can navigate to the Azure Data Lake Analytics resource on the portal and submit queries directly as illustrated in the following figure.</span></span>
> 
> 

![9](./media/machine-learning-data-science-process-data-lake-walkthrough/9-portal-submit-job.PNG)

### <span data-ttu-id="37455-207"><a name="ingest"></a>Přijímání dat: Přečíst data z veřejného objektu blob</span><span class="sxs-lookup"><span data-stu-id="37455-207"><a name="ingest"></a>Data Ingestion: Read in data from public blob</span></span>
<span data-ttu-id="37455-208">Umístění dat v Azure blob je odkazováno jako  **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name**  a mohou být extrahovány pomocí **Extractors.Csv()**.</span><span class="sxs-lookup"><span data-stu-id="37455-208">The location of the data in the Azure blob is referenced as **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name** and can be extracted using **Extractors.Csv()**.</span></span> <span data-ttu-id="37455-209">Nahraďte váš vlastní název kontejneru a název účtu úložiště v následujících skriptů pro container_name@blob_storage_account_name wasb adresu.</span><span class="sxs-lookup"><span data-stu-id="37455-209">Substitute your own container name and storage account name in following scripts for container_name@blob_storage_account_name in the wasb address.</span></span> <span data-ttu-id="37455-210">Vzhledem k tomu, že názvy souborů jsou ve stejném formátu, můžeme použít **cestě\_data_ {\*\}.csv** ke čtení v všechny soubory 12 cesty.</span><span class="sxs-lookup"><span data-stu-id="37455-210">Since the file names are in same format, we can use **trip\_data_{\*\}.csv** to read in all 12 trip files.</span></span> 

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

<span data-ttu-id="37455-211">Vzhledem k tomu, že jsou v první řádek záhlaví, je potřeba odebrat hlavičky a změnit typy sloupců do odpovídající těm, které jsou.</span><span class="sxs-lookup"><span data-stu-id="37455-211">Since there are headers in the first row, we need to remove the headers and change column types into appropriate ones.</span></span> <span data-ttu-id="37455-212">Buď uložení zpracovaná data pomocí Azure Data Lake Storage můžeme **swebhdfs://data_lake_storage_name.azuredatalakestorage.net/folder_name/file_name**_ nebo pomocí účtu úložiště objektů Blob v Azure  **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name**.</span><span class="sxs-lookup"><span data-stu-id="37455-212">We can either save the processed data to Azure Data Lake Storage using **swebhdfs://data_lake_storage_name.azuredatalakestorage.net/folder_name/file_name**_ or to Azure Blob storage account using  **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name**.</span></span> 

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

    ////output data to ADL
    OUTPUT @trip   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_trip.csv"
    USING Outputters.Csv(); 

    ////Output data to blob
    OUTPUT @trip   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_trip.csv"
    USING Outputters.Csv();  

<span data-ttu-id="37455-213">Podobně jsme můžete přečíst tarif datových sad.</span><span class="sxs-lookup"><span data-stu-id="37455-213">Similarly we can read in the fare data sets.</span></span> <span data-ttu-id="37455-214">Klikněte pravým tlačítkem na Azure Data Lake Store, můžete se podívat na vaše data v **portálu Azure--> Průzkumníku dat** nebo **Průzkumníka souborů** v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="37455-214">Right click Azure Data Lake Store, you can choose to look at your data in **Azure Portal --> Data Explorer** or **File Explorer** within Visual Studio.</span></span> 

 ![10](./media/machine-learning-data-science-process-data-lake-walkthrough/10-data-in-ADL-VS.PNG)

 ![11](./media/machine-learning-data-science-process-data-lake-walkthrough/11-data-in-ADL.PNG)

### <span data-ttu-id="37455-217"><a name="quality"></a>Kontrolu kvality dat</span><span class="sxs-lookup"><span data-stu-id="37455-217"><a name="quality"></a>Data quality checks</span></span>
<span data-ttu-id="37455-218">Po cestě a tarif tabulky byly načteny v, kontrolu kvality dat lze provést následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="37455-218">After trip and fare tables have been read in, data quality checks can be done in the following way.</span></span> <span data-ttu-id="37455-219">Výsledné soubory sdíleného svazku clusteru může být výstup do úložiště objektů Blob v Azure nebo Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="37455-219">The resulting CSV files can be output to Azure Blob storage or Azure Data Lake Store.</span></span> 

<span data-ttu-id="37455-220">Získat počet medallions a jedinečné číslo medallions:</span><span class="sxs-lookup"><span data-stu-id="37455-220">Find the number of medallions and unique number of medallions:</span></span>

    ///check the number of medallions and unique number of medallions
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
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_1.csv"
    USING Outputters.Csv(); 

<span data-ttu-id="37455-221">Najděte tyto medallions, kterým má více než 100 odezvy:</span><span class="sxs-lookup"><span data-stu-id="37455-221">Find those medallions that had more than 100 trips:</span></span>

    ///find those medallions that had more than 100 trips
    @ex_2 =
        SELECT medallion,
               COUNT(medallion) AS cnt_medallion
        FROM @trip2
        //where pickup_datetime >= "2013-01-01t00:00:00.0000000" and pickup_datetime <= "2013-04-01t00:00:00.0000000"
        GROUP BY medallion
        HAVING COUNT(medallion) > 100;
        OUTPUT @ex_2   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_2.csv"
    USING Outputters.Csv(); 

<span data-ttu-id="37455-222">Najděte tyto neplatné záznamy z hlediska pickup_longitude:</span><span class="sxs-lookup"><span data-stu-id="37455-222">Find those invalid records in terms of pickup_longitude:</span></span>

    ///find those invalid records in terms of pickup_longitude
    @ex_3 =
        SELECT COUNT(medallion) AS cnt_invalid_pickup_longitude
        FROM @trip
        WHERE
        pickup_longitude <- 90 OR pickup_longitude > 90;
        OUTPUT @ex_3   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_3.csv"
    USING Outputters.Csv(); 

<span data-ttu-id="37455-223">Najdete chybějící hodnoty pro některé proměnné:</span><span class="sxs-lookup"><span data-stu-id="37455-223">Find missing values for some variables:</span></span>

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
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_16.csv"
    USING Outputters.Csv();



### <span data-ttu-id="37455-224"><a name="explore"></a>Zkoumání dat</span><span class="sxs-lookup"><span data-stu-id="37455-224"><a name="explore"></a>Data exploration</span></span>
<span data-ttu-id="37455-225">Provedeme některé zkoumání dat pro lepší pochopení dat.</span><span class="sxs-lookup"><span data-stu-id="37455-225">We can do some data exploration to get a better understanding of the data.</span></span>

<span data-ttu-id="37455-226">Najděte distribuci šikmý a vysypávány služebních cest:</span><span class="sxs-lookup"><span data-stu-id="37455-226">Find the distribution of tipped and non-tipped trips:</span></span>

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
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_4.csv"
    USING Outputters.Csv(); 

<span data-ttu-id="37455-227">Najít distribuci tip velikost s hodnotami oříznutím: 0,5,10 a 20 odběru.</span><span class="sxs-lookup"><span data-stu-id="37455-227">Find the distribution of tip amount with cut-off values: 0,5,10,and 20 dollars.</span></span>

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
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_5.csv"
    USING Outputters.Csv(); 

<span data-ttu-id="37455-228">Najděte základní statistické údaje o cestě vzdálenost:</span><span class="sxs-lookup"><span data-stu-id="37455-228">Find basic statistics of trip distance:</span></span>

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
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_14.csv"
    USING Outputters.Csv();

<span data-ttu-id="37455-229">Najděte percentily vzdálenost cesty:</span><span class="sxs-lookup"><span data-stu-id="37455-229">Find the percentiles of trip distance:</span></span>

    // find percentiles of trip_distance
    @trip_summary3 =
        SELECT DISTINCT vendor_id AS vendor,
                        PERCENTILE_DISC(0.25) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.5) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.75) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc
        FROM @trip;
       // group by vendor_id;
    OUTPUT @trip_summary3
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_13.csv"
    USING Outputters.Csv(); 


### <span data-ttu-id="37455-230"><a name="join"></a>Připojení k cestě a tarif tabulky</span><span class="sxs-lookup"><span data-stu-id="37455-230"><a name="join"></a>Join trip and fare tables</span></span>
<span data-ttu-id="37455-231">Služební cestě a tarif tabulek je možné připojit Medailon, hack_license a pickup_time.</span><span class="sxs-lookup"><span data-stu-id="37455-231">Trip and fare tables can be joined by medallion, hack_license, and pickup_time.</span></span>

    //join trip and fare table

    @model_data_full =
    SELECT t.*, 
    f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
    (f.tip_amount > 0 ? 1: 0) AS tipped,
    (f.tip_amount >20? 4: (f.tip_amount >10? 3:(f.tip_amount >5 ? 2:(f.tip_amount > 0 ? 1: 0)))) AS tip_class
    FROM @trip AS t JOIN  @fare AS f
    ON   (t.medallion == f.medallion AND t.hack_license == f.hack_license AND t.pickup_datetime == f.pickup_datetime)
    WHERE   (pickup_longitude != 0 AND dropoff_longitude != 0 );

    //// output to blob
    OUTPUT @model_data_full   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 

    ////output data to ADL
    OUTPUT @model_data_full   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 


<span data-ttu-id="37455-232">Pro každou úroveň počtu osobní vypočte se počet záznamů, tip pro průměrnou dobu, odchylku tip velikost, procento šikmý služebních cest.</span><span class="sxs-lookup"><span data-stu-id="37455-232">For each level of passenger count, calculate the number of records, average tip amount, variance of tip amount, percentage of tipped trips.</span></span>

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
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_17.csv"
    USING Outputters.Csv();


### <span data-ttu-id="37455-233"><a name="sample"></a>Vzorkování dat</span><span class="sxs-lookup"><span data-stu-id="37455-233"><a name="sample"></a>Data sampling</span></span>
<span data-ttu-id="37455-234">Nejprve jsme náhodně zvolí 0,1 % data z připojeného k tabulce:</span><span class="sxs-lookup"><span data-stu-id="37455-234">First we randomly select 0.1% of the data from the joined table:</span></span>

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
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_random_1_1000.csv"
    USING Outputters.Csv(); 

<span data-ttu-id="37455-235">Potom jsme vrstveného vzorkování provedete binární proměnné tip_class:</span><span class="sxs-lookup"><span data-stu-id="37455-235">Then we do stratified sampling by binary variable tip_class:</span></span>

    //stratified random select 1/1000 data for modeling purpose
    @addrownumberres_stratifiedsample =
    SELECT *,
            ROW_NUMBER() OVER(PARTITION BY tip_class) AS rownum
    FROM @model_data_full;

    @model_data_stratified_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_stratifiedsample
    WHERE rownum % 1000 == 0;
    //// output to blob
    OUTPUT @model_data_stratified_sample_1_1000   
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 
    ////output data to ADL
    OUTPUT @model_data_stratified_sample_1_1000   
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 


### <span data-ttu-id="37455-236"><a name="run"></a>Spuštění úloh U-SQL</span><span class="sxs-lookup"><span data-stu-id="37455-236"><a name="run"></a>Run U-SQL jobs</span></span>
<span data-ttu-id="37455-237">Po dokončení úprav skriptů U-SQL, je možné odeslat na server pomocí účtu Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="37455-237">When you finish editing U-SQL scripts, you can submit them to the server using your Azure Data Lake Analytics account.</span></span> <span data-ttu-id="37455-238">Klikněte na tlačítko **Data Lake**, **odeslat úlohu**, vyberte vaše **účet Analytics**, zvolte **paralelismus**a klikněte na tlačítko **odeslání**  tlačítko.</span><span class="sxs-lookup"><span data-stu-id="37455-238">Click **Data Lake**, **Submit Job**, select your **Analytics Account**, choose **Parallelism**, and click **Submit** button.</span></span>  

 ![12](./media/machine-learning-data-science-process-data-lake-walkthrough/12-submit-USQL.PNG)

<span data-ttu-id="37455-240">Když úloha se úspěšně splněny, stav úlohy se zobrazí v sadě Visual Studio monitorování.</span><span class="sxs-lookup"><span data-stu-id="37455-240">When the job is complied successfully, the status of your job will be displayed in Visual Studio for monitoring.</span></span> <span data-ttu-id="37455-241">Po dokončení úlohy můžete i přehráním proces spuštění úlohy a zjistěte problémové místo kroky ke zlepšení efektivity úlohy.</span><span class="sxs-lookup"><span data-stu-id="37455-241">After the job finishes running, you can even replay the job execution process and find out the bottleneck steps to improve your job efficiency.</span></span> <span data-ttu-id="37455-242">Můžete také přejít na portál Azure a zkontrolujte stav úloh U-SQL.</span><span class="sxs-lookup"><span data-stu-id="37455-242">You can also go to Azure Portal to check the status of your U-SQL jobs.</span></span>

 ![13](./media/machine-learning-data-science-process-data-lake-walkthrough/13-USQL-running-v2.PNG)

 ![14](./media/machine-learning-data-science-process-data-lake-walkthrough/14-USQL-jobs-portal.PNG)

<span data-ttu-id="37455-245">Nyní můžete zkontrolovat výstupní soubory v Azure Blob storage nebo portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="37455-245">Now you can check the output files in either Azure Blob storage or Azure Portal.</span></span> <span data-ttu-id="37455-246">Pro naše modelování v dalším kroku použijeme vrstveného ukázková data.</span><span class="sxs-lookup"><span data-stu-id="37455-246">We will use the stratified sample data for our modeling in the next step.</span></span>

 ![15](./media/machine-learning-data-science-process-data-lake-walkthrough/15-U-SQL-output-csv.PNG)

 ![16](./media/machine-learning-data-science-process-data-lake-walkthrough/16-U-SQL-output-csv-portal.PNG)

## <a name="build-and-deploy-models-in-azure-machine-learning"></a><span data-ttu-id="37455-249">Vytváření a nasazování modelů v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="37455-249">Build and deploy models in Azure Machine Learning</span></span>
<span data-ttu-id="37455-250">Ukážeme dvě možnosti k dispozici pro vás k získání dat do Azure Machine Learning k sestavení a</span><span class="sxs-lookup"><span data-stu-id="37455-250">We demonstrate two options available for you to pull data into Azure Machine Learning to build and</span></span> 

* <span data-ttu-id="37455-251">V první možnosti, můžete použít jen Vzorkovaná data, která byla zapsána do objektu Blob Azure (v **dat vzorkování** předchozí krok) a použít Python pro sestavení a nasazení modely z Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="37455-251">In the first option, you use the sampled data that has been written to an Azure Blob (in the **Data sampling** step above) and use Python to build and deploy models from Azure Machine Learning.</span></span> 
* <span data-ttu-id="37455-252">V druhé možnosti je dotaz na data v Azure Data Lake přímo pomocí dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="37455-252">In the second option, you query the data in Azure Data Lake directly using a Hive query.</span></span> <span data-ttu-id="37455-253">Tato možnost vyžaduje vytvoření nového clusteru HDInsight nebo použít stávající cluster HDInsight, kde tabulek Hive příkaz NY taxíkem data v Azure Data Lake Storage.</span><span class="sxs-lookup"><span data-stu-id="37455-253">This option requires that you create a new HDInsight cluster or use an existing HDInsight cluster where the Hive tables point to the NY Taxi data in Azure Data Lake Storage.</span></span>  <span data-ttu-id="37455-254">Probereme obě tyto možnosti dole.</span><span class="sxs-lookup"><span data-stu-id="37455-254">We discuss both these options below.</span></span> 

## <a name="option-1-use-python-to-build-and-deploy-machine-learning-models"></a><span data-ttu-id="37455-255">Možnost 1: Použití Python k vytváření a nasazování počítače learning modely</span><span class="sxs-lookup"><span data-stu-id="37455-255">Option 1: Use Python to build and deploy machine learning models</span></span>
<span data-ttu-id="37455-256">K vytváření a nasazování modelů machine learning používá Python, vytvořte poznámkového bloku Jupyter na místním počítači nebo v Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="37455-256">To build and deploy machine learning models using Python, create a Jupyter Notebook on your local machine or in Azure Machine Learning Studio.</span></span> <span data-ttu-id="37455-257">Poznámkového bloku Jupyter k dispozici na [Githubu](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough) obsahuje kód úplné chcete prozkoumat, vizualizovat data, funkce technikům, modelování a nasazení.</span><span class="sxs-lookup"><span data-stu-id="37455-257">The Jupyter Notebook  provided on [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough) contains the full code to explore, visualize data, feature engineering, modeling and deployment.</span></span> <span data-ttu-id="37455-258">V tomto článku jsme zobrazit právě modelování a nasazení.</span><span class="sxs-lookup"><span data-stu-id="37455-258">In this article, we show just the modeling and deployment.</span></span> 

### <a name="import-python-libraries"></a><span data-ttu-id="37455-259">Importovat knihovny Python</span><span class="sxs-lookup"><span data-stu-id="37455-259">Import Python libraries</span></span>
<span data-ttu-id="37455-260">Aby bylo možné spustit ukázku soubor, následující Python, balíčky jsou nutné skriptu Poznámkový blok Jupyter nebo Python.</span><span class="sxs-lookup"><span data-stu-id="37455-260">In order to run the sample Jupyter Notebook or the Python script file, the following Python packages are needed.</span></span> <span data-ttu-id="37455-261">Pokud používáte službu AzureML Poznámkový blok, byly tyto balíčky předem nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="37455-261">If you are using the AzureML Notebook service, these packages have been pre-installed.</span></span>

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


### <a name="read-in-the-data-from-blob"></a><span data-ttu-id="37455-262">Pro čtení dat z objektu blob</span><span class="sxs-lookup"><span data-stu-id="37455-262">Read in the data from blob</span></span>
* <span data-ttu-id="37455-263">Připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="37455-263">Connection String</span></span>   
  
        CONTAINERNAME = 'test1'
        STORAGEACCOUNTNAME = 'XXXXXXXXX'
        STORAGEACCOUNTKEY = 'YYYYYYYYYYYYYYYYYYYYYYYYYYYY'
        BLOBNAME = 'demo_ex_9_stratified_1_1000_copy.csv'
        blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
* <span data-ttu-id="37455-264">Přečíst jako text</span><span class="sxs-lookup"><span data-stu-id="37455-264">Read in as text</span></span>
  
        t1 = time.time()
        data = blob_service.get_blob_to_text(CONTAINERNAME,BLOBNAME).split("\n")
        t2 = time.time()
        print(("It takes %s seconds to read in "+BLOBNAME) % (t2 - t1))
  
  ![17](./media/machine-learning-data-science-process-data-lake-walkthrough/17-python_readin_csv.PNG)    
* <span data-ttu-id="37455-266">Přidat názvy sloupců a jednotlivé sloupce</span><span class="sxs-lookup"><span data-stu-id="37455-266">Add column names and separate columns</span></span>
  
        colnames = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime',
        'passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'payment_type', 'fare_amount', 'surcharge', 'mta_tax', 'tolls_amount',  'total_amount', 'tip_amount', 'tipped', 'tip_class', 'rownum']
        df1 = pd.DataFrame([sub.split(",") for sub in data], columns = colnames)
* <span data-ttu-id="37455-267">Změnit některé sloupce na číselné</span><span class="sxs-lookup"><span data-stu-id="37455-267">Change some columns to numeric</span></span>
  
        cols_2_float = ['trip_time_in_secs','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'fare_amount', 'surcharge','mta_tax','tolls_amount','total_amount','tip_amount', 'passenger_count','trip_distance'
        ,'tipped','tip_class','rownum']
        for col in cols_2_float:
            df1[col] = df1[col].astype(float)

### <a name="build-machine-learning-models"></a><span data-ttu-id="37455-268">Vytvářet modely machine learning</span><span class="sxs-lookup"><span data-stu-id="37455-268">Build machine learning models</span></span>
<span data-ttu-id="37455-269">Zde jsme sestavení modelu binární klasifikace předpovědět, zda je vysypávány cesty, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="37455-269">Here we build a binary classification model to predict whether a trip is tipped or not.</span></span> <span data-ttu-id="37455-270">V poznámkovém bloku Jupyter můžete najít další dva modely: více třídami klasifikace a modely regrese.</span><span class="sxs-lookup"><span data-stu-id="37455-270">In the Jupyter Notebook you can find other two models: multiclass classification, and regression models.</span></span>

* <span data-ttu-id="37455-271">Nejprve musíme vytvořit fiktivní proměnné, které lze použít v scikit-další modely</span><span class="sxs-lookup"><span data-stu-id="37455-271">First we need to create dummy variables that can be used in scikit-learn models</span></span>
  
        df1_payment_type_dummy = pd.get_dummies(df1['payment_type'], prefix='payment_type_dummy')
        df1_vendor_id_dummy = pd.get_dummies(df1['vendor_id'], prefix='vendor_id_dummy')
* <span data-ttu-id="37455-272">Vytvořit data snímku pro modelování</span><span class="sxs-lookup"><span data-stu-id="37455-272">Create data frame for the modeling</span></span>
  
        cols_to_keep = ['tipped', 'trip_distance', 'passenger_count']
        data = df1[cols_to_keep].join([df1_payment_type_dummy,df1_vendor_id_dummy])
  
        X = data.iloc[:,1:]
        Y = data.tipped
* <span data-ttu-id="37455-273">Trénování a testování 60 40 rozdělení</span><span class="sxs-lookup"><span data-stu-id="37455-273">Training and testing 60-40 split</span></span>
  
        X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.4, random_state=0)
* <span data-ttu-id="37455-274">Logistic Regression v sadě školení</span><span class="sxs-lookup"><span data-stu-id="37455-274">Logistic Regression in training set</span></span>
  
        model = LogisticRegression()
        logit_fit = model.fit(X_train, Y_train)
        print ('Coefficients: \n', logit_fit.coef_)
        Y_train_pred = logit_fit.predict(X_train)
  
       ![c1](./media/machine-learning-data-science-process-data-lake-walkthrough/c1-py-logit-coefficient.PNG)
* <span data-ttu-id="37455-275">Stanovení skóre testování datové sady</span><span class="sxs-lookup"><span data-stu-id="37455-275">Score testing data set</span></span>
  
        Y_test_pred = logit_fit.predict(X_test)
* <span data-ttu-id="37455-276">Výpočet hodnocení metriky</span><span class="sxs-lookup"><span data-stu-id="37455-276">Calculate Evaluation metrics</span></span>
  
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

### <a name="build-web-service-api-and-consume-it-in-python"></a><span data-ttu-id="37455-277">Sestavení webového rozhraní API služby a využívat v Pythonu</span><span class="sxs-lookup"><span data-stu-id="37455-277">Build Web Service API and consume it in Python</span></span>
<span data-ttu-id="37455-278">Chceme zprovoznit strojového učení modelu, jakmile byl vytvořen.</span><span class="sxs-lookup"><span data-stu-id="37455-278">We want to operationalize the machine learning model after it has been built.</span></span> <span data-ttu-id="37455-279">Tady používáme binární logistic model jako příklad.</span><span class="sxs-lookup"><span data-stu-id="37455-279">Here we use the binary logistic model as an example.</span></span> <span data-ttu-id="37455-280">Zajistěte, aby scikit-další 0.15.1 je verze v místním počítači.</span><span class="sxs-lookup"><span data-stu-id="37455-280">Make sure the scikit-learn version in your local machine is 0.15.1.</span></span> <span data-ttu-id="37455-281">Nemusíte si dělat starosti o to, pokud používáte službu Azure ML studio.</span><span class="sxs-lookup"><span data-stu-id="37455-281">You don't have to worry about this if you use Azure ML studio service.</span></span>

* <span data-ttu-id="37455-282">Najít z Azure ML studio nastavení přihlašovacích údajů pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="37455-282">Find your workspace credentials from Azure ML studio settings.</span></span> <span data-ttu-id="37455-283">V nástroji Azure Machine Learning Studio, klikněte na tlačítko **nastavení** --> **název** --> **autorizace tokeny**.</span><span class="sxs-lookup"><span data-stu-id="37455-283">In Azure Machine Learning Studio, click **Settings** --> **Name** --> **Authorization Tokens**.</span></span> 
  
    ![C3](./media/machine-learning-data-science-process-data-lake-walkthrough/c3-workspace-id.PNG)

        workspaceid = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'
        auth_token = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'

* <span data-ttu-id="37455-285">Vytvoření webové služby</span><span class="sxs-lookup"><span data-stu-id="37455-285">Create Web Service</span></span>
  
        @services.publish(workspaceid, auth_token) 
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float, payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(int) #0, or 1
        def predictNYCTAXI(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            inputArray = [trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH, payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS]
            return logit_fit.predict(inputArray)
* <span data-ttu-id="37455-286">Získat přihlašovací údaje webové služby</span><span class="sxs-lookup"><span data-stu-id="37455-286">Get web service credentials</span></span>
  
        url = predictNYCTAXI.service.url
        api_key =  predictNYCTAXI.service.api_key
  
        print url
        print api_key
  
        @services.service(url, api_key)
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float,payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(float)
        def NYCTAXIPredictor(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            pass
* <span data-ttu-id="37455-287">Volání rozhraní API webové služby.</span><span class="sxs-lookup"><span data-stu-id="37455-287">Call Web service API.</span></span> <span data-ttu-id="37455-288">Budete muset počkejte 5 až 10 sekund po předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="37455-288">You have to wait 5-10 seconds after the previous step.</span></span>
  
        NYCTAXIPredictor(1,2,1,0,0,0,0,0,1)
  
       ![c4](./media/machine-learning-data-science-process-data-lake-walkthrough/c4-call-API.PNG)

## <a name="option-2-create-and-deploy-models-directly-in-azure-machine-learning"></a><span data-ttu-id="37455-289">Možnost 2: Vytvoření a nasazení modely přímo v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="37455-289">Option 2: Create and deploy models directly in Azure Machine Learning</span></span>
<span data-ttu-id="37455-290">Azure Machine Learning Studio můžete číst data přímo z Azure Data Lake Store a pak použije k vytvoření a nasazení modelů.</span><span class="sxs-lookup"><span data-stu-id="37455-290">Azure Machine Learning Studio can read data directly from Azure Data Lake Store and then be used to create and deploy models.</span></span> <span data-ttu-id="37455-291">Tento postup používá tabulku Hive, která odkazuje na Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="37455-291">This approach uses a Hive table that points at the Azure Data Lake Store.</span></span> <span data-ttu-id="37455-292">To vyžaduje, aby zřídit samostatný cluster Azure HDInsight, na který se vytvoří tabulku Hive.</span><span class="sxs-lookup"><span data-stu-id="37455-292">This requires that a separate Azure HDInsight cluster be provisioned, on which the Hive table is created.</span></span> <span data-ttu-id="37455-293">Následující části vysvětlují, jak to provést.</span><span class="sxs-lookup"><span data-stu-id="37455-293">The following sections show how to do this.</span></span> 

### <a name="create-an-hdinsight-linux-cluster"></a><span data-ttu-id="37455-294">Vytvoření clusteru HDInsight Linux</span><span class="sxs-lookup"><span data-stu-id="37455-294">Create an HDInsight Linux Cluster</span></span>
<span data-ttu-id="37455-295">Vytvoření clusteru HDInsight (Linux) z [portál Azure](http://portal.azure.com). Podrobnosti najdete v tématu **vytvoření clusteru HDInsight s přístupem k Azure Data Lake Store** kapitoly [vytvoření clusteru HDInsight s Data Lake Store pomocí portálu Azure](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="37455-295">Create an HDInsight Cluster (Linux) from the [Azure Portal](http://portal.azure.com).For details, see the **Create an HDInsight cluster with access to Azure Data Lake Store** section in [Create an HDInsight cluster with Data Lake Store using Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

 ![18](./media/machine-learning-data-science-process-data-lake-walkthrough/18-create_HDI_cluster.PNG)

### <a name="create-hive-table-in-hdinsight"></a><span data-ttu-id="37455-297">Vytvoří tabulku Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="37455-297">Create Hive table in HDInsight</span></span>
<span data-ttu-id="37455-298">Nyní vytvoříme tabulek Hive, který se má použít v nástroji Azure Machine Learning Studio v clusteru HDInsight pomocí dat uložených v Azure Data Lake Store v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="37455-298">Now we create Hive tables to be used in Azure Machine Learning Studio in the HDInsight cluster using the data stored in Azure Data Lake Store in the previous step.</span></span> <span data-ttu-id="37455-299">Přejděte ke clusteru HDInsight se právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="37455-299">Go to the HDInsight cluster just created.</span></span> <span data-ttu-id="37455-300">Klikněte na tlačítko **nastavení** --> **vlastnosti** --> **identita AAD clusteru** --> **přístupu ADLS**, Ujistěte se, že váš účet Azure Data Lake Store je přidaný do seznamu čtení, zápisu a oprávnění pro spouštění.</span><span class="sxs-lookup"><span data-stu-id="37455-300">Click **Settings** --> **Properties** --> **Cluster AAD Identity** --> **ADLS Access**, make sure your Azure Data Lake Store account is added in the list with read, write and execute rights.</span></span> 

 ![19](./media/machine-learning-data-science-process-data-lake-walkthrough/19-HDI-cluster-add-ADLS.PNG)

<span data-ttu-id="37455-302">Pak klikněte na tlačítko **řídicí panel** vedle **nastavení** tlačítko a okna, objeví se.</span><span class="sxs-lookup"><span data-stu-id="37455-302">Then click **Dashboard** next to the **Settings** button and a window will pop up.</span></span> <span data-ttu-id="37455-303">Klikněte na tlačítko **zobrazení Hive** se zobrazí v pravém horním rohu stránky a můžete **Editor dotazů**.</span><span class="sxs-lookup"><span data-stu-id="37455-303">Click **Hive View** in the upper right corner of the page and you will see the **Query Editor**.</span></span>

 ![20](./media/machine-learning-data-science-process-data-lake-walkthrough/20-HDI-dashboard.PNG)

 ![21](./media/machine-learning-data-science-process-data-lake-walkthrough/21-Hive-Query-Editor-v2.PNG)

<span data-ttu-id="37455-306">Vložte následující skripty Hive a vytvořte tabulku.</span><span class="sxs-lookup"><span data-stu-id="37455-306">Paste in the following Hive scripts to create a table.</span></span> <span data-ttu-id="37455-307">Umístění zdroje dat je v Azure Data Lake Store odkaz tímto způsobem: **adl://data_lake_store_name.azuredatalakestore.net:443/název_složky/název_souboru**.</span><span class="sxs-lookup"><span data-stu-id="37455-307">The location of data source is in Azure Data Lake Store reference in this way: **adl://data_lake_store_name.azuredatalakestore.net:443/folder_name/file_name**.</span></span>

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


<span data-ttu-id="37455-308">Po dokončení dotazu, zobrazí se výsledky takto:</span><span class="sxs-lookup"><span data-stu-id="37455-308">When the query finishes running, you will see the results like this:</span></span>

 ![22](./media/machine-learning-data-science-process-data-lake-walkthrough/22-Hive-Query-results.PNG)

### <a name="build-and-deploy-models-in-azure-machine-learning-studio"></a><span data-ttu-id="37455-310">Vytváření a nasazování modely v Azure Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="37455-310">Build and deploy models in Azure Machine Learning Studio</span></span>
<span data-ttu-id="37455-311">Nyní jsme připraveni k sestavení a nasazení model, který předpovídá, zda je tip placené pomocí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="37455-311">We are now ready to build and deploy a model that predicts whether or not a tip is paid with Azure Machine Learning.</span></span> <span data-ttu-id="37455-312">Je připravená k použití v této binární klasifikace vrstveného ukázková data (tip nebo ne) problém.</span><span class="sxs-lookup"><span data-stu-id="37455-312">The stratified sample data is ready to be used in this binary classification (tip or not) problem.</span></span> <span data-ttu-id="37455-313">Prediktivní modely pomocí více třídami klasifikace (tip_class) a regrese (tip_amount) můžete také být vytvořené a nasazené se službou Azure Machine Learning Studio, ale zde jsme pouze ukazují, jak zpracovávat tento případ použití modelu binární klasifikace.</span><span class="sxs-lookup"><span data-stu-id="37455-313">The predictive models using multiclass classification (tip_class) and regression (tip_amount) can also be built and deployed with Azure Machine Learning Studio, but here we only show how to handle the case using the binary classification model.</span></span>

1. <span data-ttu-id="37455-314">Získat data do aplikace pomocí Azure ML **importovat Data** modulu, k dispozici v **vstupu a výstupu dat** části.</span><span class="sxs-lookup"><span data-stu-id="37455-314">Get the data into Azure ML using the **Import Data** module, available in the **Data Input and Output** section.</span></span> <span data-ttu-id="37455-315">Další informace najdete v tématu [importovat Data modulu](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) stránka s referencemi k.</span><span class="sxs-lookup"><span data-stu-id="37455-315">For more information, see the [Import Data module](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) reference page.</span></span>
2. <span data-ttu-id="37455-316">Vyberte **dotaz Hive** jako **zdroj dat** v **vlastnosti** panelu.</span><span class="sxs-lookup"><span data-stu-id="37455-316">Select **Hive Query** as the **Data source** in the **Properties** panel.</span></span>
3. <span data-ttu-id="37455-317">Vložte následující skript Hive v **databázový dotaz Hive** editoru</span><span class="sxs-lookup"><span data-stu-id="37455-317">Paste the following Hive script in the **Hive database query** editor</span></span>
   
        select * from nyc_stratified_sample;
4. <span data-ttu-id="37455-318">Zadejte identifikátor URI HDInsight clusteru (lze najít na portálu Azure), přihlašovací údaje Hadoop, umístění výstupních dat a název název / / kontejner klíče účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="37455-318">Enter the URI of HDInsight cluster (this can be found in Azure Portal), Hadoop credentials, location of output data, and Azure storage account name/key/container name.</span></span>
   
   ![23](./media/machine-learning-data-science-process-data-lake-walkthrough/23-reader-module-v3.PNG)  

<span data-ttu-id="37455-320">Na obrázku níže je uveden příklad binární klasifikace pokusu se čtení dat z tabulky Hive.</span><span class="sxs-lookup"><span data-stu-id="37455-320">An example of a binary classification experiment reading data from Hive table is shown in the figure below.</span></span>

 ![24](./media/machine-learning-data-science-process-data-lake-walkthrough/24-AML-exp.PNG)

<span data-ttu-id="37455-322">Po vytvoření experimentu klikněte na tlačítko **nastavit webové služby** --> **prediktivní webové služby**</span><span class="sxs-lookup"><span data-stu-id="37455-322">After the experiment is created, click  **Set Up Web Service** --> **Predictive Web Service**</span></span>

 ![25](./media/machine-learning-data-science-process-data-lake-walkthrough/25-AML-exp-deploy.PNG)

<span data-ttu-id="37455-324">Spustit automaticky vytvořený vyhodnocování experiment, po dokončení, klikněte na tlačítko **nasazení webové služby**</span><span class="sxs-lookup"><span data-stu-id="37455-324">Run the automatically created scoring experiment, when it finishes, click **Deploy Web Service**</span></span>

 ![26](./media/machine-learning-data-science-process-data-lake-walkthrough/26-AML-exp-deploy-web.PNG)

<span data-ttu-id="37455-326">Řídicí panel webové služby se zobrazí po chvíli:</span><span class="sxs-lookup"><span data-stu-id="37455-326">The web service dashboard will be displayed shortly:</span></span>

 ![27](./media/machine-learning-data-science-process-data-lake-walkthrough/27-AML-web-api.PNG)

## <a name="summary"></a><span data-ttu-id="37455-328">Souhrn</span><span class="sxs-lookup"><span data-stu-id="37455-328">Summary</span></span>
<span data-ttu-id="37455-329">Provedením tohoto návodu jste vytvořili data vědecké účely prostředí pro vytváření škálovatelných začátku do konce řešení v Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="37455-329">By completing this walkthrough you have created a data science environment for building scalable end-to-end solutions in Azure Data Lake.</span></span> <span data-ttu-id="37455-330">Toto prostředí byl použit k analýze velkých veřejné datové sady, který ji provede kanonický kroky procesu vědecké účely dat, z získávání dat přes model školení a následně do nasazení model jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="37455-330">This environment was used to analyze a large public dataset, taking it through the canonical steps of the Data Science Process, from data acquisition through model training, and then to the deployment of the model as a web service.</span></span> <span data-ttu-id="37455-331">U-SQL byl použit ke zpracování, prozkoumejte a ukázková data.</span><span class="sxs-lookup"><span data-stu-id="37455-331">U-SQL was used to process, explore and sample the data.</span></span> <span data-ttu-id="37455-332">Python a Hive se použít s Azure Machine Learning Studio vytvářet a nasazovat prediktivní modely.</span><span class="sxs-lookup"><span data-stu-id="37455-332">Python and Hive were used with Azure Machine Learning Studio to build and deploy predictive models.</span></span>

## <a name="whats-next"></a><span data-ttu-id="37455-333">Co dále?</span><span class="sxs-lookup"><span data-stu-id="37455-333">What's next?</span></span>
<span data-ttu-id="37455-334">Studijní postup pro [tým datové vědy procesu (TDSP)](http://aka.ms/datascienceprocess) obsahuje odkazy na témata s popisem jednotlivých kroků v procesu pokročilou analýzu.</span><span class="sxs-lookup"><span data-stu-id="37455-334">The learning path for the [Team Data Science Process (TDSP)](http://aka.ms/datascienceprocess) provides links to topics describing each step in the advanced analytics process.</span></span> <span data-ttu-id="37455-335">Existuje řada návodů uvedeno na [proces vědecké účely dat Team návody](data-science-process-walkthroughs.md) stránky, který předvádí použití služby a prostředky v různých scénářích prediktivní analýzy:</span><span class="sxs-lookup"><span data-stu-id="37455-335">There are a series of walkthroughs itemized on the [Team Data Science Process walkthroughs](data-science-process-walkthroughs.md) page that showcase how to use resources and services in various predictive analytics scenarios:</span></span>

* [<span data-ttu-id="37455-336">Proces Team dat. vědecké účely v akci: pomocí SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="37455-336">The Team Data Science Process in action: using SQL Data Warehouse</span></span>](machine-learning-data-science-process-sqldw-walkthrough.md)
* [<span data-ttu-id="37455-337">Proces Team dat. vědecké účely v akci: pomocí clusterů systému HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="37455-337">The Team Data Science Process in action: using HDInsight Hadoop clusters</span></span>](machine-learning-data-science-process-hive-walkthrough.md)
* [<span data-ttu-id="37455-338">Procesu vědecké účely Team dat: SQL Server pomocí</span><span class="sxs-lookup"><span data-stu-id="37455-338">The Team Data Science Process: using SQL Server</span></span>](machine-learning-data-science-process-sql-walkthrough.md)
* [<span data-ttu-id="37455-339">Přehled procesu vědecké účely dat pomocí Spark v Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="37455-339">Overview of the Data Science Process using Spark on Azure HDInsight</span></span>](machine-learning-data-science-spark-overview.md)

