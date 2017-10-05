---
title: "Prozkoumejte data v clusteru Hadoop a vytváření modelů v Azure Machine Learning | Microsoft Docs"
description: "Pomocí procesu Team dat vědecké účely začátku do konce scénář nasazení clusteru služby HDInsight Hadoop pro sestavení a nasazení modelu použití veřejně dostupné datové sady."
services: machine-learning,hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: e9e76c91-d0f6-483d-bae7-2d3157b86aa0
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: e48d59ca467e3e7fd772389e6e48a2d81726f859
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="the-team-data-science-process-in-action-use-azure-hdinsight-hadoop-clusters"></a><span data-ttu-id="92ff8-103">Proces Team dat. vědecké účely v akci: použití Azure HDInsight Hadoop clusterů</span><span class="sxs-lookup"><span data-stu-id="92ff8-103">The Team Data Science Process in action: Use Azure HDInsight Hadoop clusters</span></span>
<span data-ttu-id="92ff8-104">V tomto návodu použijeme [tým datové vědy procesu (TDSP)](data-science-process-overview.md) začátku do konce scénář pomocí [clusteru Azure HDInsight Hadoop](https://azure.microsoft.com/services/hdinsight/) k uložení, prozkoumejte a funkci pracovníka data z veřejně k dispozici [NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/) datové sady a dolů ukázková data.</span><span class="sxs-lookup"><span data-stu-id="92ff8-104">In this walkthrough, we use the [Team Data Science Process (TDSP)](data-science-process-overview.md) in an end-to-end scenario using an [Azure HDInsight Hadoop cluster](https://azure.microsoft.com/services/hdinsight/) to store, explore and feature engineer data from the publicly available [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset, and to down sample the data.</span></span> <span data-ttu-id="92ff8-105">Modely dat jsou integrované s Azure Machine Learning pro zpracování více třídami a binární klasifikace a regrese prediktivní úlohy.</span><span class="sxs-lookup"><span data-stu-id="92ff8-105">Models of the data are built with Azure Machine Learning to handle binary and multiclass classification and regression predictive tasks.</span></span>

<span data-ttu-id="92ff8-106">Návod, který ukazuje způsob zpracování větší datovou sadu (1 terabajt) pro podobné scénáře pomocí clusterů HDInsight Hadoop pro zpracování dat najdete v tématu [Team Data vědecké účely proces – pomocí Azure HDInsight Hadoop clusterů v datové sadě 1 TB](machine-learning-data-science-process-hive-criteo-walkthrough.md) .</span><span class="sxs-lookup"><span data-stu-id="92ff8-106">For a walkthrough that shows how to handle a larger (1 terabyte) dataset for a similar scenario using HDInsight Hadoop clusters for data processing, see [Team Data Science Process - Using Azure HDInsight Hadoop Clusters on a 1 TB dataset](machine-learning-data-science-process-hive-criteo-walkthrough.md).</span></span>

<span data-ttu-id="92ff8-107">Je také možné použít IPython poznámkového bloku k provádění úloh uvedené návod použití datové sady 1 TB.</span><span class="sxs-lookup"><span data-stu-id="92ff8-107">It is also possible to use an IPython notebook to accomplish the tasks presented the walkthrough using the 1 TB dataset.</span></span> <span data-ttu-id="92ff8-108">Uživatelé, kteří by chtěli opakujte tento postup by se měli obrátit [Criteo návod pomocí připojení Hive ODBC](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) tématu.</span><span class="sxs-lookup"><span data-stu-id="92ff8-108">Users who would like to try this approach should consult the [Criteo walkthrough using a Hive ODBC connection](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) topic.</span></span>

## <span data-ttu-id="92ff8-109"><a name="dataset"></a>Popis NYC taxíkem služebních cest datové sady</span><span class="sxs-lookup"><span data-stu-id="92ff8-109"><a name="dataset"></a>NYC Taxi Trips Dataset description</span></span>
<span data-ttu-id="92ff8-110">Data NYC taxíkem cesty je přibližně 20GB komprimované hodnot oddělených čárkami (CSV) souborů (nekomprimovaným ~ 48GB), skládající se z více než 173 milionů jednotlivých cest a tarify placené pro každou cestu.</span><span class="sxs-lookup"><span data-stu-id="92ff8-110">The NYC Taxi Trip data is about 20GB of compressed comma-separated values (CSV) files (~48GB uncompressed), comprising more than 173 million individual trips and the fares paid for each trip.</span></span> <span data-ttu-id="92ff8-111">Každý záznam cestě zahrnuje číslo licence vyzvednutí a odkládací umístění a čas, anonymizovaná hackerský (ovladač) a číslo Medailon (taxi na jedinečné id).</span><span class="sxs-lookup"><span data-stu-id="92ff8-111">Each trip record includes the pickup and drop-off location and time, anonymized hack (driver's) license number and medallion (taxi’s unique id) number.</span></span> <span data-ttu-id="92ff8-112">Data obsahuje všechny služebních cest v roku 2013 a je dostupné pro každý měsíc následující dvě datové sady:</span><span class="sxs-lookup"><span data-stu-id="92ff8-112">The data covers all trips in the year 2013 and is provided in the following two datasets for each month:</span></span>

1. <span data-ttu-id="92ff8-113">CSV soubory 'trip_data' obsahují podrobnosti o cestě, například na počtu cestujících, vyzvednutí a dropoff body, doba trvání cesty a délka cesty.</span><span class="sxs-lookup"><span data-stu-id="92ff8-113">The 'trip_data' CSV files contain trip details, such as number of passengers, pickup and dropoff points, trip duration, and trip length.</span></span> <span data-ttu-id="92ff8-114">Tady je několik ukázkových záznamů:</span><span class="sxs-lookup"><span data-stu-id="92ff8-114">Here are a few sample records:</span></span>
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. <span data-ttu-id="92ff8-115">CSV soubory 'trip_fare' obsahují podrobnosti o tarif placené pro každou cestu, například typ platby, velikost tarif, příplatek a daně, tipy a mýtné a celkovou velikost placené.</span><span class="sxs-lookup"><span data-stu-id="92ff8-115">The 'trip_fare' CSV files contain details of the fare paid for each trip, such as payment type, fare amount, surcharge and taxes, tips and tolls, and the total amount paid.</span></span> <span data-ttu-id="92ff8-116">Tady je několik ukázkových záznamů:</span><span class="sxs-lookup"><span data-stu-id="92ff8-116">Here are a few sample records:</span></span>
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

<span data-ttu-id="92ff8-117">Jedinečný klíč pro připojení k cestě\_dat a cesty\_tarif se skládá z pole: medailonu, hackerský\_licence a vyzvednutí\_data a času.</span><span class="sxs-lookup"><span data-stu-id="92ff8-117">The unique key to join trip\_data and trip\_fare is composed of the fields: medallion, hack\_licence and pickup\_datetime.</span></span>

<span data-ttu-id="92ff8-118">Chcete-li získat všechny podrobnosti relevantní pro konkrétní služební cestě, je dostatečná spojení s tři klíče: "medailonu", "zabezpečení\_licenční" a "vyzvednutí\_data a času".</span><span class="sxs-lookup"><span data-stu-id="92ff8-118">To get all of the details relevant to a particular trip, it is sufficient to join with three keys: the "medallion", "hack\_license" and "pickup\_datetime".</span></span>

<span data-ttu-id="92ff8-119">Jsme popisují některé další podrobnosti dat, když jsme jejich uložení do tabulek Hive za chvíli.</span><span class="sxs-lookup"><span data-stu-id="92ff8-119">We describe some more details of the data when we store them into Hive tables shortly.</span></span>

## <span data-ttu-id="92ff8-120"><a name="mltasks"></a>Příklady předpovědi úlohy</span><span class="sxs-lookup"><span data-stu-id="92ff8-120"><a name="mltasks"></a>Examples of prediction tasks</span></span>
<span data-ttu-id="92ff8-121">Když blíží data, určení druh předpovědi, které chcete, aby na základě jeho analýzy pomáhá vysvětlení úloh, které je potřeba zahrnout do procesu.</span><span class="sxs-lookup"><span data-stu-id="92ff8-121">When approaching data, determining the kind of predictions you want to make based on its analysis helps clarify the tasks that you will need to include in your process.</span></span>
<span data-ttu-id="92ff8-122">Tady jsou tři příklady předpovědi problémy, které jsme adresy v tomto návodu, jejichž formulování je založena na *tip\_velikost*:</span><span class="sxs-lookup"><span data-stu-id="92ff8-122">Here are three examples of prediction problems that we address in this walkthrough whose formulation is based on the *tip\_amount*:</span></span>

1. <span data-ttu-id="92ff8-123">**Binární klasifikace**: předpovědět, zda byl tip placené cesty, tj. *tip\_velikost* větší než $0 je pozitivní příklad, při *tip\_velikost* $ 0 je záporný příklad.</span><span class="sxs-lookup"><span data-stu-id="92ff8-123">**Binary classification**: Predict whether or not a tip was paid for a trip, i.e. a *tip\_amount* that is greater than $0 is a positive example, while a *tip\_amount* of $0 is a negative example.</span></span>
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0
2. <span data-ttu-id="92ff8-124">**Více třídami klasifikace**: K předpovědi rozsahu tip objemu placené pro cestu.</span><span class="sxs-lookup"><span data-stu-id="92ff8-124">**Multiclass classification**: To predict the range of tip amounts paid for the trip.</span></span> <span data-ttu-id="92ff8-125">Jsme rozdělit *tip\_velikost* do pěti přihrádek nebo třídy:</span><span class="sxs-lookup"><span data-stu-id="92ff8-125">We divide the *tip\_amount* into five bins or classes:</span></span>
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. <span data-ttu-id="92ff8-126">**Úloha regrese**: K předvídání množství tip placené cesty.</span><span class="sxs-lookup"><span data-stu-id="92ff8-126">**Regression task**: To predict the amount of the tip paid for a trip.</span></span>  

## <span data-ttu-id="92ff8-127"><a name="setup"></a>Nastavení clusteru HDInsight Hadoop pro pokročilou analýzu</span><span class="sxs-lookup"><span data-stu-id="92ff8-127"><a name="setup"></a>Set up an HDInsight Hadoop cluster for advanced analytics</span></span>
> [!NOTE]
> <span data-ttu-id="92ff8-128">Obvykle se jedná **správce** úloh.</span><span class="sxs-lookup"><span data-stu-id="92ff8-128">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="92ff8-129">Můžete nastavit prostředí Azure pro pokročilou analýzu využívajícího clusteru služby HDInsight v tři kroky:</span><span class="sxs-lookup"><span data-stu-id="92ff8-129">You can set up an Azure environment for advanced analytics that employs an HDInsight cluster in three steps:</span></span>

1. <span data-ttu-id="92ff8-130">[Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md): Tento účet úložiště se používá pro ukládání dat v Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="92ff8-130">[Create a storage account](../storage/common/storage-create-storage-account.md): This storage account is used for storing data in Azure Blob Storage.</span></span> <span data-ttu-id="92ff8-131">Data použitá v clusterech HDInsight se také nachází zde.</span><span class="sxs-lookup"><span data-stu-id="92ff8-131">The data used in HDInsight clusters also resides here.</span></span>
2. <span data-ttu-id="92ff8-132">[Přizpůsobení clusterů systému Azure HDInsight Hadoop pro pokročilé analýzy proces a technologie](machine-learning-data-science-customize-hadoop-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="92ff8-132">[Customize Azure HDInsight Hadoop clusters for the Advanced Analytics Process and Technology](machine-learning-data-science-customize-hadoop-cluster.md).</span></span> <span data-ttu-id="92ff8-133">Tento krok vytvoří cluster s 64-bit Anaconda Python 2.7 nainstalované na všech uzlech služby Azure HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="92ff8-133">This step creates an Azure HDInsight Hadoop cluster with 64-bit Anaconda Python 2.7 installed on all nodes.</span></span> <span data-ttu-id="92ff8-134">Existují dva důležité kroky pamatovat při přizpůsobení clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="92ff8-134">There are two important steps to remember while customizing your HDInsight cluster.</span></span>
   
   * <span data-ttu-id="92ff8-135">Mějte na paměti propojení účtu úložiště vytvořeném v kroku 1 k vašemu clusteru HDInsight při jejím vytváření.</span><span class="sxs-lookup"><span data-stu-id="92ff8-135">Remember to link the storage account created in step 1 with your HDInsight cluster when creating it.</span></span> <span data-ttu-id="92ff8-136">Tento účet úložiště se používá pro přístup k datům, která je zpracovat v rámci clusteru.</span><span class="sxs-lookup"><span data-stu-id="92ff8-136">This storage account is used to access data that is processed within the cluster.</span></span>
   * <span data-ttu-id="92ff8-137">Po vytvoření clusteru, povolte vzdálený přístup k hlavnímu uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="92ff8-137">After the cluster is created, enable Remote Access to the head node of the cluster.</span></span> <span data-ttu-id="92ff8-138">Přejděte na **konfigurace** a klikněte na **povolit vzdálené**.</span><span class="sxs-lookup"><span data-stu-id="92ff8-138">Navigate to the **Configuration** tab and click **Enable Remote**.</span></span> <span data-ttu-id="92ff8-139">Tento krok určuje uživatelská pověření použitá pro vzdálené přihlášení.</span><span class="sxs-lookup"><span data-stu-id="92ff8-139">This step specifies the user credentials used for remote login.</span></span>
3. <span data-ttu-id="92ff8-140">[Vytvořit pracovní prostor služby Azure Machine Learning](machine-learning-create-workspace.md): Tento Azure Machine Learning prostoru slouží k vytvoření modely machine learning.</span><span class="sxs-lookup"><span data-stu-id="92ff8-140">[Create an Azure Machine Learning workspace](machine-learning-create-workspace.md): This Azure Machine Learning workspace is used to build machine learning models.</span></span> <span data-ttu-id="92ff8-141">Tato úloha je určeno po dokončení počáteční data zkoumání a dolů vzorkování pomocí clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="92ff8-141">This task is addressed after completing an initial data exploration and down sampling using the HDInsight cluster.</span></span>

## <span data-ttu-id="92ff8-142"><a name="getdata"></a>Získat data z veřejné zdroje</span><span class="sxs-lookup"><span data-stu-id="92ff8-142"><a name="getdata"></a>Get the data from a public source</span></span>
> [!NOTE]
> <span data-ttu-id="92ff8-143">Obvykle se jedná **správce** úloh.</span><span class="sxs-lookup"><span data-stu-id="92ff8-143">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="92ff8-144">Chcete-li získat [NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/) datové sady z veřejného umístění, můžete použít některou z metod popsaných v [přesun dat do a z Azure Blob Storage](machine-learning-data-science-move-azure-blob.md) kopírovat data do vašeho počítače.</span><span class="sxs-lookup"><span data-stu-id="92ff8-144">To get the [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset from its public location, you may use any of the methods described in [Move Data to and from Azure Blob Storage](machine-learning-data-science-move-azure-blob.md) to copy the data to your machine.</span></span>

<span data-ttu-id="92ff8-145">Zde jsme popisují, jak pomocí nástroje AzCopy pro přenos souborů, který obsahuje data.</span><span class="sxs-lookup"><span data-stu-id="92ff8-145">Here we describe how use AzCopy to transfer the files containing data.</span></span> <span data-ttu-id="92ff8-146">Stáhněte a nainstalujte AzCopy postupujte podle pokynů v [Začínáme pomocí příkazového řádku Azcopy](../storage/common/storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="92ff8-146">To download and install AzCopy follow the instructions at [Getting Started with the AzCopy Command-Line Utility](../storage/common/storage-use-azcopy.md).</span></span>

1. <span data-ttu-id="92ff8-147">Z okna příkazového řádku, vydávání následující příkazy AzCopy nahrazení *< path_to_data_folder >* s požadované cílové umístění:</span><span class="sxs-lookup"><span data-stu-id="92ff8-147">From a Command Prompt window, issue the following AzCopy commands, replacing *<path_to_data_folder>* with the desired destination:</span></span>

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

1. <span data-ttu-id="92ff8-148">Po dokončení kopírování celkem 24 komprimované soubory jsou ve složce data vybrali.</span><span class="sxs-lookup"><span data-stu-id="92ff8-148">When the copy completes, a total of 24 zipped files are in the data folder chosen.</span></span> <span data-ttu-id="92ff8-149">Rozbalte stažené soubory do stejného adresáře na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="92ff8-149">Unzip the downloaded files to the same directory on your local machine.</span></span> <span data-ttu-id="92ff8-150">Poznamenejte si složky, kde jsou umístěny nekomprimovaných souborů.</span><span class="sxs-lookup"><span data-stu-id="92ff8-150">Make a note of the folder where the uncompressed files reside.</span></span> <span data-ttu-id="92ff8-151">Tato složka se označuje jako *< cesta\_k\_unzipped_data\_soubory\>*  je následující.</span><span class="sxs-lookup"><span data-stu-id="92ff8-151">This folder will be referred to as the *<path\_to\_unzipped_data\_files\>* is what follows.</span></span>

## <span data-ttu-id="92ff8-152"><a name="upload"></a>Nahrát data do výchozího kontejneru clusteru Azure HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="92ff8-152"><a name="upload"></a>Upload the data to the default container of Azure HDInsight Hadoop cluster</span></span>
> [!NOTE]
> <span data-ttu-id="92ff8-153">Obvykle se jedná **správce** úloh.</span><span class="sxs-lookup"><span data-stu-id="92ff8-153">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="92ff8-154">V následujících příkazech AzCopy, nahraďte následující parametry skutečné hodnoty, které jste zadali při vytváření clusteru Hadoop a rozbalení datové soubory.</span><span class="sxs-lookup"><span data-stu-id="92ff8-154">In the following AzCopy commands, replace the following parameters with the actual values that you specified when creating the Hadoop cluster and unzipping the data files.</span></span>

* <span data-ttu-id="92ff8-155">***& č. 60; path_to_data_folder >*** adresáři (spolu s cesta) na počítači, které obsahují rozbalené datové soubory</span><span class="sxs-lookup"><span data-stu-id="92ff8-155">***&#60;path_to_data_folder>*** the directory (along with path) on your machine that contain the unzipped data files</span></span>  
* <span data-ttu-id="92ff8-156">***& č. 60; název účtu úložiště clusteru Hadoop >*** účtu úložiště přidruženého k vašemu clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="92ff8-156">***&#60;storage account name of Hadoop cluster>*** the storage account associated with your HDInsight cluster</span></span>
* <span data-ttu-id="92ff8-157">***& č. 60; výchozí kontejner clusteru Hadoop >*** výchozí kontejner používá váš cluster.</span><span class="sxs-lookup"><span data-stu-id="92ff8-157">***&#60;default container of Hadoop cluster>*** the default container used by your cluster.</span></span> <span data-ttu-id="92ff8-158">Všimněte si, zda je název kontejneru výchozí obvykle stejný název jako samotného clusteru.</span><span class="sxs-lookup"><span data-stu-id="92ff8-158">Note that the name of the default container is usually the same name as the cluster itself.</span></span> <span data-ttu-id="92ff8-159">Například pokud clusteru se nazývá "abc123.azurehdinsight.net", je výchozí kontejner abc123.</span><span class="sxs-lookup"><span data-stu-id="92ff8-159">For example, if the cluster is called "abc123.azurehdinsight.net", the default container is abc123.</span></span>
* <span data-ttu-id="92ff8-160">***& č. 60; klíč účtu úložiště >*** klíč pro účet úložiště používá váš cluster</span><span class="sxs-lookup"><span data-stu-id="92ff8-160">***&#60;storage account key>*** the key for the storage account used by your cluster</span></span>

<span data-ttu-id="92ff8-161">Z příkazového řádku nebo okno prostředí Windows PowerShell v počítači spusťte následující dva příkazy AzCopy.</span><span class="sxs-lookup"><span data-stu-id="92ff8-161">From a Command Prompt or a Windows PowerShell window in your machine, run the following two AzCopy commands.</span></span>

<span data-ttu-id="92ff8-162">Tento příkaz odesílá data na cestě ***nyctaxitripraw*** adresář ve výchozím kontejneru clusteru Hadoop.</span><span class="sxs-lookup"><span data-stu-id="92ff8-162">This command uploads the trip data to ***nyctaxitripraw*** directory in the default container of the Hadoop cluster.</span></span>

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

<span data-ttu-id="92ff8-163">Tento příkaz odešle data tarif ***nyctaxifareraw*** adresář ve výchozím kontejneru clusteru Hadoop.</span><span class="sxs-lookup"><span data-stu-id="92ff8-163">This command uploads the fare data to ***nyctaxifareraw*** directory in the default container of the Hadoop cluster.</span></span>

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

<span data-ttu-id="92ff8-164">Data by teď ve službě Azure Blob Storage a připravené k využívat v rámci clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="92ff8-164">The data should now in Azure Blob Storage and ready to be consumed within the HDInsight cluster.</span></span>

## <span data-ttu-id="92ff8-165"><a name="#download-hql-files"></a>Přihlaste se k hlavnímu uzlu clusteru Hadoop a a připravit pro analýzu nahodilého dat</span><span class="sxs-lookup"><span data-stu-id="92ff8-165"><a name="#download-hql-files"></a>Log into the head node of Hadoop cluster and and prepare for exploratory data analysis</span></span>
> [!NOTE]
> <span data-ttu-id="92ff8-166">Obvykle se jedná **správce** úloh.</span><span class="sxs-lookup"><span data-stu-id="92ff8-166">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="92ff8-167">Pro přístup k hlavnímu uzlu clusteru pro analýzu nahodilého dat a dolů vzorkování dat, postupujte podle pokynů uvedených v [přístup hlavního uzlu Hadoop clusteru](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span><span class="sxs-lookup"><span data-stu-id="92ff8-167">To access the head node of the cluster for exploratory data analysis and down sampling of the data, follow the procedure outlined in [Access the Head Node of Hadoop Cluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span></span>

<span data-ttu-id="92ff8-168">V tomto návodu budeme používat hlavně dotazy, které jsou napsané v [Hive](https://hive.apache.org/), jako SQL dotazovací jazyk, k provedení explorations předběžné data.</span><span class="sxs-lookup"><span data-stu-id="92ff8-168">In this walkthrough, we primarily use queries written in [Hive](https://hive.apache.org/), a SQL-like query language, to perform preliminary data explorations.</span></span> <span data-ttu-id="92ff8-169">Dotazy Hive jsou uložené v souborech .hql.</span><span class="sxs-lookup"><span data-stu-id="92ff8-169">The Hive queries are stored in .hql files.</span></span> <span data-ttu-id="92ff8-170">Jsme pak dolů ukázkové tato data, která má být použit v rámci Azure Machine Learning pro vytváření modelů.</span><span class="sxs-lookup"><span data-stu-id="92ff8-170">We then down sample this data to be used within Azure Machine Learning for building models.</span></span>

<span data-ttu-id="92ff8-171">Příprava clusteru pro analýzu nahodilého dat, můžeme stáhnout soubory .hql obsahuje relevantní skripty Hive z [githubu](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) do místního adresáře (C:\temp) z hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="92ff8-171">To prepare the cluster for exploratory data analysis, we download the .hql files containing the relevant Hive scripts from [github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) to a local directory (C:\temp) on the head node.</span></span> <span data-ttu-id="92ff8-172">Chcete-li to provést, otevřete **příkazového řádku** z v rámci hlavního uzlu clusteru a vydejte následující dva příkazy:</span><span class="sxs-lookup"><span data-stu-id="92ff8-172">To do this, open the **Command Prompt** from within the head node of the cluster and issue the following two commands:</span></span>

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

<span data-ttu-id="92ff8-173">Tyto dva příkazy stáhne všechny soubory .hql potřebné v tomto návodu k místnímu adresáři ***C:\temp &#92;*** v hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="92ff8-173">These two commands will download all .hql files needed in this walkthrough to the local directory ***C:\temp&#92;*** in the head node.</span></span>

## <span data-ttu-id="92ff8-174"><a name="#hive-db-tables"></a>Vytvoření databáze Hive a tabulky rozdělena na oddíly po měsících</span><span class="sxs-lookup"><span data-stu-id="92ff8-174"><a name="#hive-db-tables"></a>Create Hive database and tables partitioned by month</span></span>
> [!NOTE]
> <span data-ttu-id="92ff8-175">Obvykle se jedná **správce** úloh.</span><span class="sxs-lookup"><span data-stu-id="92ff8-175">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="92ff8-176">Nyní jsme připraveni k vytváření tabulek Hive pro naší datové sadě taxíkem NYC.</span><span class="sxs-lookup"><span data-stu-id="92ff8-176">We are now ready to create Hive tables for our NYC taxi dataset.</span></span>
<span data-ttu-id="92ff8-177">V hlavního uzlu clusteru Hadoop, otevřete ***Hadoop příkazového řádku*** na ploše hlavního uzlu a zadejte adresář Hive zadáním příkazu</span><span class="sxs-lookup"><span data-stu-id="92ff8-177">In the head node of the Hadoop cluster, open the ***Hadoop Command Line*** on the desktop of the head node, and enter the Hive directory by entering the command</span></span>

    cd %hive_home%\bin

> [!NOTE]
> <span data-ttu-id="92ff8-178">**Spusťte všechny příkazy Hive v tomto návodu z výše uvedených Hive bin / directory řádku. To se postará o všech problémech, cesta automaticky. Používáme podmínky "Hive directory výzva", "Hive bin / directory řádku" a "Hadoop příkazový řádek" zcela zaměnitelným významem v tomto návodu.**</span><span class="sxs-lookup"><span data-stu-id="92ff8-178">**Run all Hive commands in this walkthrough from the above Hive bin/ directory prompt. This will take care of any path issues automatically. We use the terms "Hive directory prompt", "Hive bin/ directory prompt",  and "Hadoop Command Line" interchangeably in this walkthrough.**</span></span>
> 
> 

<span data-ttu-id="92ff8-179">Z adresáře řádku Hive zadejte následující příkaz v Hadoop příkazový řádek z hlavního uzlu odeslat dotaz Hive k vytvoření databáze Hive a tabulek:</span><span class="sxs-lookup"><span data-stu-id="92ff8-179">From the Hive directory prompt, enter the following command in Hadoop Command Line of the head node to submit the Hive query to create Hive database and tables:</span></span>

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

<span data-ttu-id="92ff8-180">Tady je obsah ***C:\temp\sample\_hive\_vytvořit\_db\_a\_tables.hql*** souboru, který vytvoří databázi Hive ***nyctaxidb*** a tabulky ***cestě*** a ***tarif***.</span><span class="sxs-lookup"><span data-stu-id="92ff8-180">Here is the content of the ***C:\temp\sample\_hive\_create\_db\_and\_tables.hql*** file which creates Hive database ***nyctaxidb*** and tables ***trip*** and ***fare***.</span></span>

    create database if not exists nyctaxidb;

    create external table if not exists nyctaxidb.trip
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double)  
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');

    create external table if not exists nyctaxidb.fare
    (
        medallion string,
        hack_license string,
        vendor_id string,
        pickup_datetime string,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double)
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');

<span data-ttu-id="92ff8-181">Tento skript Hive vytvoří dvě tabulky:</span><span class="sxs-lookup"><span data-stu-id="92ff8-181">This Hive script creates two tables:</span></span>

* <span data-ttu-id="92ff8-182">"cesty" tabulka obsahuje podrobnosti o cestě každý pravé (podrobnosti o ovladači, výstupní čas, vzdálenost cesty a časy)</span><span class="sxs-lookup"><span data-stu-id="92ff8-182">the "trip" table contains trip details of each ride (driver details, pickup time, trip distance and times)</span></span>
* <span data-ttu-id="92ff8-183">"tarif" tabulka obsahuje podrobnosti o tarif (tarif částka tip, mýtné a příplatky).</span><span class="sxs-lookup"><span data-stu-id="92ff8-183">the "fare" table contains fare details (fare amount, tip amount, tolls and surcharges).</span></span>

<span data-ttu-id="92ff8-184">Pokud potřebujete žádné další pomoc s tyto postupy nebo chcete prozkoumat alternativní těch, najdete v části [dotazů odeslání Hive přímo z Hadoop příkazového řádku ](machine-learning-data-science-move-hive-tables.md#submit).</span><span class="sxs-lookup"><span data-stu-id="92ff8-184">If you need any additional assistance with these procedures or want to investigate alternative ones, see the section [Submit Hive queries directly from the Hadoop Command Line ](machine-learning-data-science-move-hive-tables.md#submit).</span></span>

## <span data-ttu-id="92ff8-185"><a name="#load-data"></a>Načtení dat do tabulek Hive podle oddílů</span><span class="sxs-lookup"><span data-stu-id="92ff8-185"><a name="#load-data"></a>Load Data to Hive tables by partitions</span></span>
> [!NOTE]
> <span data-ttu-id="92ff8-186">Obvykle se jedná **správce** úloh.</span><span class="sxs-lookup"><span data-stu-id="92ff8-186">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="92ff8-187">Datová sada taxíkem NYC má přirozené dělení po měsících, které jsou používány za účelem povolení rychlejší zpracování a dotazů.</span><span class="sxs-lookup"><span data-stu-id="92ff8-187">The NYC taxi dataset has a natural partitioning by month, which we use to enable faster processing and query times.</span></span> <span data-ttu-id="92ff8-188">Dole uvedené příkazy Powershellu (vystavené Hive directory pomocí **Hadoop příkazového řádku**) načíst data do tabulek Hive "cesty" a "tarif" rozdělena na oddíly po měsících.</span><span class="sxs-lookup"><span data-stu-id="92ff8-188">The PowerShell commands below (issued from the Hive directory using the **Hadoop Command Line**) load data to the "trip" and "fare" Hive tables partitioned by month.</span></span>

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

<span data-ttu-id="92ff8-189">*Ukázka\_hive\_načíst\_data\_podle\_partitions.hql* soubor obsahuje následující **NAČÍST** příkazy.</span><span class="sxs-lookup"><span data-stu-id="92ff8-189">The *sample\_hive\_load\_data\_by\_partitions.hql* file contains the following **LOAD** commands.</span></span>

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

<span data-ttu-id="92ff8-190">Všimněte si, že počet dotazů Hive, kterou tady používáme v procesu zkoumání zahrnují vyhledávání na právě jeden oddíl nebo na pouze několik oddílů.</span><span class="sxs-lookup"><span data-stu-id="92ff8-190">Note that a number of Hive queries we use here in the exploration process involve looking at just a single partition or at only a couple of partitions.</span></span> <span data-ttu-id="92ff8-191">Ale tyto dotazy, které by bylo možné spustit napříč celou daty.</span><span class="sxs-lookup"><span data-stu-id="92ff8-191">But these queries could be run across the entire data.</span></span>

### <span data-ttu-id="92ff8-192"><a name="#show-db"></a>Zobrazit databází v clusteru HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="92ff8-192"><a name="#show-db"></a>Show databases in the HDInsight Hadoop cluster</span></span>
<span data-ttu-id="92ff8-193">Chcete-li zobrazit databáze vytvořené v clusteru HDInsight Hadoop v okně příkazového řádku Hadoop, spusťte následující příkaz v Hadoop příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="92ff8-193">To show the databases created in HDInsight Hadoop cluster inside the Hadoop Command Line window, run the following command in Hadoop Command Line:</span></span>

    hive -e "show databases;"

### <span data-ttu-id="92ff8-194"><a name="#show-tables"></a>Zobrazení tabulek Hive v databázi nyctaxidb</span><span class="sxs-lookup"><span data-stu-id="92ff8-194"><a name="#show-tables"></a>Show the Hive tables in the nyctaxidb database</span></span>
<span data-ttu-id="92ff8-195">Pokud chcete zobrazit tabulky v databázi nyctaxidb, spusťte následující příkaz v Hadoop příkazového řádku:</span><span class="sxs-lookup"><span data-stu-id="92ff8-195">To show the tables in the nyctaxidb database, run the following command in Hadoop Command Line:</span></span>

    hive -e "show tables in nyctaxidb;"

<span data-ttu-id="92ff8-196">Může potvrdit, že v tabulkách jsou rozdělena na oddíly pomocí příkazu níže:</span><span class="sxs-lookup"><span data-stu-id="92ff8-196">We can confirm that the tables are partitioned by issuing the command below:</span></span>

    hive -e "show partitions nyctaxidb.trip;"

<span data-ttu-id="92ff8-197">Očekávaný výstup je zobrazena níže:</span><span class="sxs-lookup"><span data-stu-id="92ff8-197">The expected output is shown below:</span></span>

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 2.075 seconds, Fetched: 12 row(s)

<span data-ttu-id="92ff8-198">Podobně je zajištěno, že tarif tabulka je rozdělena na oddíly pomocí příkazu níže:</span><span class="sxs-lookup"><span data-stu-id="92ff8-198">Similarly, we can ensure that the fare table is partitioned by issuing the command below:</span></span>

    hive -e "show partitions nyctaxidb.fare;"

<span data-ttu-id="92ff8-199">Očekávaný výstup je zobrazena níže:</span><span class="sxs-lookup"><span data-stu-id="92ff8-199">The expected output is shown below:</span></span>

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 1.887 seconds, Fetched: 12 row(s)

## <span data-ttu-id="92ff8-200"><a name="#explore-hive"></a>Zkoumání dat a funkce analýzy v Hive</span><span class="sxs-lookup"><span data-stu-id="92ff8-200"><a name="#explore-hive"></a>Data exploration and feature engineering in Hive</span></span>
> [!NOTE]
> <span data-ttu-id="92ff8-201">To je obvykle **vědecký pracovník Data** úloh.</span><span class="sxs-lookup"><span data-stu-id="92ff8-201">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="92ff8-202">Zkoumání dat a funkce technici úlohy pro data načtena do tabulek Hive můžete udělat pomocí dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="92ff8-202">The data exploration and feature engineering tasks for the data loaded into the Hive tables can be accomplished using Hive queries.</span></span> <span data-ttu-id="92ff8-203">Zde jsou příklady takových úloh, že jsme vás provede procesem v této části:</span><span class="sxs-lookup"><span data-stu-id="92ff8-203">Here are examples of such tasks that we walk you through in this section:</span></span>

* <span data-ttu-id="92ff8-204">Zobrazte prvních 10 záznamů v obou tabulkách.</span><span class="sxs-lookup"><span data-stu-id="92ff8-204">View the top 10 records in both tables.</span></span>
* <span data-ttu-id="92ff8-205">Prozkoumejte data distribuce několik polí v různých časových oken.</span><span class="sxs-lookup"><span data-stu-id="92ff8-205">Explore data distributions of a few fields in varying time windows.</span></span>
* <span data-ttu-id="92ff8-206">Prozkoumejte data quality polí zeměpisné šířky a délky.</span><span class="sxs-lookup"><span data-stu-id="92ff8-206">Investigate data quality of the longitude and latitude fields.</span></span>
* <span data-ttu-id="92ff8-207">Generovat binární a více třídami klasifikační štítky na základě **tip\_velikost**.</span><span class="sxs-lookup"><span data-stu-id="92ff8-207">Generate binary and multiclass classification labels based on the **tip\_amount**.</span></span>
* <span data-ttu-id="92ff8-208">Vygenerujte funkce computing vzdálenosti přímé cesty.</span><span class="sxs-lookup"><span data-stu-id="92ff8-208">Generate features by computing the direct trip distances.</span></span>

### <a name="exploration-view-the-top-10-records-in-table-trip"></a><span data-ttu-id="92ff8-209">Zkoumání: Zobrazte prvních 10 záznamy v cestě tabulky</span><span class="sxs-lookup"><span data-stu-id="92ff8-209">Exploration: View the top 10 records in table trip</span></span>
> [!NOTE]
> <span data-ttu-id="92ff8-210">To je obvykle **vědecký pracovník Data** úloh.</span><span class="sxs-lookup"><span data-stu-id="92ff8-210">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="92ff8-211">Pokud chcete zobrazit, jak vypadá data, jsme zkontrolujte 10 záznamy ze všech tabulek.</span><span class="sxs-lookup"><span data-stu-id="92ff8-211">To see what the data looks like, we examine 10 records from each table.</span></span> <span data-ttu-id="92ff8-212">Spusťte odděleně následující dva dotazy z příkazového řádku adresář Hive v konzole nástroje příkazového řádku Hadoop kontrola záznamy.</span><span class="sxs-lookup"><span data-stu-id="92ff8-212">Run the following two queries separately from the Hive directory prompt in the Hadoop Command Line console to inspect the records.</span></span>

<span data-ttu-id="92ff8-213">Pokud chcete získat prvních 10 záznamy v tabulce "cesty" z první měsíc:</span><span class="sxs-lookup"><span data-stu-id="92ff8-213">To get the top 10 records in the table "trip" from the first month:</span></span>

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

<span data-ttu-id="92ff8-214">Pokud chcete získat "tarif" top 10 záznamy v tabulce z první měsíc:</span><span class="sxs-lookup"><span data-stu-id="92ff8-214">To get the top 10 records in the table "fare" from the first month:</span></span>

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

<span data-ttu-id="92ff8-215">Je často užitečné záznamy uložíte do souboru pro pohodlný zobrazení.</span><span class="sxs-lookup"><span data-stu-id="92ff8-215">It is often useful to save the records to a file for convenient viewing.</span></span> <span data-ttu-id="92ff8-216">Malé změny na výše uvedeném dotazu toho dosahuje:</span><span class="sxs-lookup"><span data-stu-id="92ff8-216">A small change to the above query accomplishes this:</span></span>

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-the-number-of-records-in-each-of-the-12-partitions"></a><span data-ttu-id="92ff8-217">Zkoumání: Zobrazte počet záznamů v každé 12 oddílů</span><span class="sxs-lookup"><span data-stu-id="92ff8-217">Exploration: View the number of records in each of the 12 partitions</span></span>
> [!NOTE]
> <span data-ttu-id="92ff8-218">To je obvykle **vědecký pracovník Data** úloh.</span><span class="sxs-lookup"><span data-stu-id="92ff8-218">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="92ff8-219">Týkající se pokynů počet cest se liší v kalendářním roce.</span><span class="sxs-lookup"><span data-stu-id="92ff8-219">Of interest is the how the number of trips varies during the calendar year.</span></span> <span data-ttu-id="92ff8-220">Seskupení podle měsíce umožňuje zobrazit, jak vypadá této distribuce služebních cest.</span><span class="sxs-lookup"><span data-stu-id="92ff8-220">Grouping by month allows us to see what this distribution of trips looks like.</span></span>

    hive -e "select month, count(*) from nyctaxidb.trip group by month;"

<span data-ttu-id="92ff8-221">To nám dává výstup:</span><span class="sxs-lookup"><span data-stu-id="92ff8-221">This gives us the output :</span></span>

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 283.406 seconds, Fetched: 12 row(s)

<span data-ttu-id="92ff8-222">Zde je první sloupec v měsíci a druhá počet cest pro daný měsíc.</span><span class="sxs-lookup"><span data-stu-id="92ff8-222">Here, the first column is the month and the second is the number of trips for that month.</span></span>

<span data-ttu-id="92ff8-223">Celkový počet záznamů jsme také můžete počítat v naší datové sadě cestě po vydání následujícího příkazu příkazového řádku adresář Hive.</span><span class="sxs-lookup"><span data-stu-id="92ff8-223">We can also count the total number of records in our trip data set by issuing the following command at the Hive directory prompt.</span></span>

    hive -e "select count(*) from nyctaxidb.trip;"

<span data-ttu-id="92ff8-224">Dostaneme:</span><span class="sxs-lookup"><span data-stu-id="92ff8-224">This yields:</span></span>

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

<span data-ttu-id="92ff8-225">Pomocí příkazů, které jsou podobné těm, které jsou pro sadu dat služební cestě, jsme vydávat dotazy Hive z příkazového řádku Hive adresář pro datovou sadu tarif ověření počet záznamů.</span><span class="sxs-lookup"><span data-stu-id="92ff8-225">Using commands similar to those shown for the trip data set, we can issue Hive queries from the Hive directory prompt for the fare data set to validate the number of records.</span></span>

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

<span data-ttu-id="92ff8-226">To nám dává výstup:</span><span class="sxs-lookup"><span data-stu-id="92ff8-226">This gives us the output:</span></span>

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 253.955 seconds, Fetched: 12 row(s)

<span data-ttu-id="92ff8-227">Všimněte si, že pro obě sady dat se vrátí přesně stejný počet služebních cest za měsíc.</span><span class="sxs-lookup"><span data-stu-id="92ff8-227">Note that the exact same number of trips per month is returned for both data sets.</span></span> <span data-ttu-id="92ff8-228">To poskytuje první ověření správně načíst data.</span><span class="sxs-lookup"><span data-stu-id="92ff8-228">This provides the first validation that the data has been loaded correctly.</span></span>

<span data-ttu-id="92ff8-229">Počítání celkový počet záznamů v sadě dat tarif lze provést pomocí příkazu z příkazového řádku adresář Hive níže:</span><span class="sxs-lookup"><span data-stu-id="92ff8-229">Counting the total number of records in the fare data set can be done using the command below from the Hive directory prompt:</span></span>

    hive -e "select count(*) from nyctaxidb.fare;"

<span data-ttu-id="92ff8-230">Dostaneme:</span><span class="sxs-lookup"><span data-stu-id="92ff8-230">This yields :</span></span>

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

<span data-ttu-id="92ff8-231">Celkový počet záznamů v obou tabulkách je také stejné.</span><span class="sxs-lookup"><span data-stu-id="92ff8-231">The total number of records in both tables is also the same.</span></span> <span data-ttu-id="92ff8-232">To poskytuje druhé ověření správně načíst data.</span><span class="sxs-lookup"><span data-stu-id="92ff8-232">This provides a second validation that the data has been loaded correctly.</span></span>

### <a name="exploration-trip-distribution-by-medallion"></a><span data-ttu-id="92ff8-233">Zkoumání: Cestě distribuce podle Medailon</span><span class="sxs-lookup"><span data-stu-id="92ff8-233">Exploration: Trip distribution by medallion</span></span>
> [!NOTE]
> <span data-ttu-id="92ff8-234">To je obvykle **vědecký pracovník Data** úloh.</span><span class="sxs-lookup"><span data-stu-id="92ff8-234">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="92ff8-235">Tento příklad identifikuje Medailon (taxi čísla) s více než 100 služebních cest v daném časovém období.</span><span class="sxs-lookup"><span data-stu-id="92ff8-235">This example identifies the medallion (taxi numbers) with more than 100 trips within a given time period.</span></span> <span data-ttu-id="92ff8-236">Dotaz výhody před přístupem dělenou tabulku vzhledem k tomu, že je podmíněno proměnnou oddílu **měsíc**.</span><span class="sxs-lookup"><span data-stu-id="92ff8-236">The query benefits from the partitioned table access since it is conditioned by the partition variable **month**.</span></span> <span data-ttu-id="92ff8-237">Výsledky dotazu se zapisují do místního souboru queryoutput.tsv v `C:\temp` z hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="92ff8-237">The query results are written to a local file queryoutput.tsv in `C:\temp` on the head node.</span></span>

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

<span data-ttu-id="92ff8-238">Tady je obsah *ukázka\_hive\_cestě\_počet\_podle\_medallion.hql* souboru kontroly.</span><span class="sxs-lookup"><span data-stu-id="92ff8-238">Here is the content of *sample\_hive\_trip\_count\_by\_medallion.hql* file for inspection.</span></span>

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

<span data-ttu-id="92ff8-239">Medailon v datové sadě taxíkem NYC identifikuje jedinečný soubor cab.</span><span class="sxs-lookup"><span data-stu-id="92ff8-239">The medallion in the NYC taxi data set identifies a unique cab.</span></span> <span data-ttu-id="92ff8-240">Abychom mohli identifikovat, které soubory CAB jsou "zaneprázdněný" žádostí, které z nich větší než počet cest provedené v konkrétním časovém období.</span><span class="sxs-lookup"><span data-stu-id="92ff8-240">We can identify which cabs are "busy" by asking which ones made more than a certain number of trips in a particular time period.</span></span> <span data-ttu-id="92ff8-241">Následující příklad určuje soubory CAB, provedené víc než sto cest v první tři měsíce a uloží do místního souboru C:\temp\queryoutput.tsv výsledky dotazu.</span><span class="sxs-lookup"><span data-stu-id="92ff8-241">The following example identifies cabs that made more than a hundred trips in the first three months, and saves the query results to a local file, C:\temp\queryoutput.tsv.</span></span>

<span data-ttu-id="92ff8-242">Tady je obsah *ukázka\_hive\_cestě\_počet\_podle\_medallion.hql* souboru kontroly.</span><span class="sxs-lookup"><span data-stu-id="92ff8-242">Here is the content of *sample\_hive\_trip\_count\_by\_medallion.hql* file for inspection.</span></span>

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

<span data-ttu-id="92ff8-243">Na řádku adresář Hive vydejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="92ff8-243">From the Hive directory prompt, issue the command below :</span></span>

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a><span data-ttu-id="92ff8-244">Zkoumání: Cestě distribuce podle Medailon a hack_license</span><span class="sxs-lookup"><span data-stu-id="92ff8-244">Exploration: Trip distribution by medallion and hack_license</span></span>
> [!NOTE]
> <span data-ttu-id="92ff8-245">To je obvykle **vědecký pracovník Data** úloh.</span><span class="sxs-lookup"><span data-stu-id="92ff8-245">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="92ff8-246">Při prohlížení datové sady, často chceme Zkontrolujte počet výskytů co skupin hodnot.</span><span class="sxs-lookup"><span data-stu-id="92ff8-246">When exploring a dataset, we frequently want to examine the number of co-occurences of groups of values.</span></span> <span data-ttu-id="92ff8-247">Tato část uveďte příklad toho, jak to udělat pro soubory CAB a ovladače.</span><span class="sxs-lookup"><span data-stu-id="92ff8-247">This section provide an example of how to do this for cabs and drivers.</span></span>

<span data-ttu-id="92ff8-248">*Ukázka\_hive\_cestě\_počet\_podle\_Medailon\_license.hql* skupin souborů tarif datové sady na "Medailon" a "hack_license" a Vrátí počet každé kombinaci.</span><span class="sxs-lookup"><span data-stu-id="92ff8-248">The *sample\_hive\_trip\_count\_by\_medallion\_license.hql* file groups the fare data set on "medallion" and "hack_license" and returns counts of each combination.</span></span> <span data-ttu-id="92ff8-249">Níže jsou jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="92ff8-249">Below are its contents.</span></span>

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

<span data-ttu-id="92ff8-250">Tento dotaz vrací určitý ovladač kombinací seřazené podle sestupné počet cest a souborů cab.</span><span class="sxs-lookup"><span data-stu-id="92ff8-250">This query returns cab and particular driver combinations ordered by descending number of trips.</span></span>

<span data-ttu-id="92ff8-251">Z adresáře řádku Hive spusťte příkaz:</span><span class="sxs-lookup"><span data-stu-id="92ff8-251">From the Hive directory prompt, run :</span></span>

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

<span data-ttu-id="92ff8-252">Výsledky dotazu jsou zapisovány do místního souboru C:\temp\queryoutput.tsv.</span><span class="sxs-lookup"><span data-stu-id="92ff8-252">The query results are written to a local file C:\temp\queryoutput.tsv.</span></span>

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitudelatitude-records"></a><span data-ttu-id="92ff8-253">Zkoumání: Vyhodnocování kvality dat. kontrolou neplatné délky nebo zeměpisnou šířku záznamů</span><span class="sxs-lookup"><span data-stu-id="92ff8-253">Exploration: Assessing data quality by checking for invalid longitude/latitude records</span></span>
> [!NOTE]
> <span data-ttu-id="92ff8-254">To je obvykle **vědecký pracovník Data** úloh.</span><span class="sxs-lookup"><span data-stu-id="92ff8-254">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="92ff8-255">Běžné cílem analýzy nahodilého dat je odstraňování plevele se neplatný nebo chybných záznamů.</span><span class="sxs-lookup"><span data-stu-id="92ff8-255">A common objective of exploratory data analysis is to weed out invalid or bad records.</span></span> <span data-ttu-id="92ff8-256">Příklad v této části určuje, zda buď zeměpisné šířky nebo délky pole obsahovat hodnotu daleko mimo oblast NYC.</span><span class="sxs-lookup"><span data-stu-id="92ff8-256">The example in this section determines whether either the longitude or latitude fields contain a value far outside the NYC area.</span></span> <span data-ttu-id="92ff8-257">Vzhledem k tomu, že je pravděpodobné, že tyto záznamy mají hodnoty chybné zeměpisné šířky, chceme je vyloučit z všechna data, která se má použít pro modelování.</span><span class="sxs-lookup"><span data-stu-id="92ff8-257">Since it is likely that such records have an erroneous longitude-latitude values, we want to eliminate them from any data that is to be used for modeling.</span></span>

<span data-ttu-id="92ff8-258">Tady je obsah *ukázka\_hive\_kvality\_assessment.hql* souboru kontroly.</span><span class="sxs-lookup"><span data-stu-id="92ff8-258">Here is the content of *sample\_hive\_quality\_assessment.hql* file for inspection.</span></span>

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


<span data-ttu-id="92ff8-259">Z adresáře řádku Hive spusťte příkaz:</span><span class="sxs-lookup"><span data-stu-id="92ff8-259">From the Hive directory prompt, run :</span></span>

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

<span data-ttu-id="92ff8-260">*-S* argument zahrnuté v tomto příkazu potlačí výtisk obrazovky stav úloh Hive mapy nebo snižte.</span><span class="sxs-lookup"><span data-stu-id="92ff8-260">The *-S* argument included in this command suppresses the status screen printout of the Hive Map/Reduce jobs.</span></span> <span data-ttu-id="92ff8-261">To je užitečné, protože umožňuje na obrazovce tiskové dotazu výstupu podregistru srozumitelnější.</span><span class="sxs-lookup"><span data-stu-id="92ff8-261">This is useful because it makes the screen print of the Hive query output more readable.</span></span>

### <a name="exploration-binary-class-distributions-of-trip-tips"></a><span data-ttu-id="92ff8-262">Zkoumání: Binární třída distribuce cestě tipy</span><span class="sxs-lookup"><span data-stu-id="92ff8-262">Exploration: Binary class distributions of trip tips</span></span>
> [!NOTE]
> <span data-ttu-id="92ff8-263">To je obvykle **vědecký pracovník Data** úloh.</span><span class="sxs-lookup"><span data-stu-id="92ff8-263">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="92ff8-264">Pro problém binární klasifikace uvedených v [příklady úloh předpovědi](machine-learning-data-science-process-hive-walkthrough.md#mltasks) části, je užitečné vědět, zda byl zadán tip, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="92ff8-264">For the binary classification problem outlined in the [Examples of prediction tasks](machine-learning-data-science-process-hive-walkthrough.md#mltasks) section, it is useful to know whether a tip was given or not.</span></span> <span data-ttu-id="92ff8-265">Toto rozdělení tipy je binární:</span><span class="sxs-lookup"><span data-stu-id="92ff8-265">This distribution of tips is binary:</span></span>

* <span data-ttu-id="92ff8-266">Zadaný Tip (třída 1, tip\_částka > $0)</span><span class="sxs-lookup"><span data-stu-id="92ff8-266">tip given(Class 1, tip\_amount > $0)</span></span>  
* <span data-ttu-id="92ff8-267">žádné tip (třída 0, tip\_velikost = 0).</span><span class="sxs-lookup"><span data-stu-id="92ff8-267">no tip (Class 0, tip\_amount = $0).</span></span>

<span data-ttu-id="92ff8-268">*Ukázka\_hive\_vysypávány\_frequencies.hql* tomu souboru vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="92ff8-268">The *sample\_hive\_tipped\_frequencies.hql* file shown below does this.</span></span>

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

<span data-ttu-id="92ff8-269">Z adresáře řádku Hive spusťte příkaz:</span><span class="sxs-lookup"><span data-stu-id="92ff8-269">From the Hive directory prompt, run:</span></span>

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-the-multiclass-setting"></a><span data-ttu-id="92ff8-270">Zkoumání: Třída distribuce do více třídami nastavení</span><span class="sxs-lookup"><span data-stu-id="92ff8-270">Exploration: Class distributions in the multiclass setting</span></span>
> [!NOTE]
> <span data-ttu-id="92ff8-271">To je obvykle **vědecký pracovník Data** úloh.</span><span class="sxs-lookup"><span data-stu-id="92ff8-271">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="92ff8-272">Pro více třídami klasifikace problému uvedených v [příklady úloh předpovědi](machine-learning-data-science-process-hive-walkthrough.md#mltasks) části tato datová sada také slouží k přirozené klasifikace, kde bychom rádi předpovědi množství tipy zadána.</span><span class="sxs-lookup"><span data-stu-id="92ff8-272">For the multiclass classification problem outlined in the [Examples of prediction tasks](machine-learning-data-science-process-hive-walkthrough.md#mltasks) section this data set also lends itself to a natural classification where we would like to predict the amount of the tips given.</span></span> <span data-ttu-id="92ff8-273">Přihrádek jsme můžete použít k definování rozsahy tip v dotazu.</span><span class="sxs-lookup"><span data-stu-id="92ff8-273">We can use bins to define tip ranges in the query.</span></span> <span data-ttu-id="92ff8-274">Třída distribuce pro různé tip rozsahy získáte používáme *ukázka\_hive\_tip\_rozsah\_frequencies.hql* souboru.</span><span class="sxs-lookup"><span data-stu-id="92ff8-274">To get the class distributions for the various tip ranges, we use the *sample\_hive\_tip\_range\_frequencies.hql* file.</span></span> <span data-ttu-id="92ff8-275">Níže jsou jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="92ff8-275">Below are its contents.</span></span>

    SELECT tip_class, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount=0, 0,
            if(tip_amount>0 and tip_amount<=5, 1,
            if(tip_amount>5 and tip_amount<=10, 2,
            if(tip_amount>10 and tip_amount<=20, 3, 4)))) as tip_class, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tip_class;

<span data-ttu-id="92ff8-276">Z konzoly Hadoop příkazového řádku spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="92ff8-276">Run the following command from Hadoop Command Line console:</span></span>

    hive -f "C:\temp\sample_hive_tip_range_frequencies.hql"

### <a name="exploration-compute-direct-distance-between-two-longitude-latitude-locations"></a><span data-ttu-id="92ff8-277">Zkoumání: Výpočetní přímé vzdálenost mezi dvěma umístěními zeměpisné šířky</span><span class="sxs-lookup"><span data-stu-id="92ff8-277">Exploration: Compute Direct Distance Between Two Longitude-Latitude Locations</span></span>
> [!NOTE]
> <span data-ttu-id="92ff8-278">To je obvykle **vědecký pracovník Data** úloh.</span><span class="sxs-lookup"><span data-stu-id="92ff8-278">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="92ff8-279">Míru přímé vzdálenost umožní nám zjistit nesoulad mezi ním vzdálenost skutečné cesty.</span><span class="sxs-lookup"><span data-stu-id="92ff8-279">Having a measure of the direct distance allows us to find out the discrepancy between it and the actual trip distance.</span></span> <span data-ttu-id="92ff8-280">Tato funkce jsme stimulují tak, že odkazuje osobní může být méně pravděpodobné, že tip, pokud jejich rozmyslete si, že ovladač má záměrně provedenou je mnohem delší trasy.</span><span class="sxs-lookup"><span data-stu-id="92ff8-280">We motivate this feature by pointing out that a passenger might be less likely to tip if they figure out that the driver has intentionally taken them by a much longer route.</span></span>

<span data-ttu-id="92ff8-281">Zobrazíte porovnání mezi vzdálenost skutečné cesty a [Haversine vzdálenost](http://en.wikipedia.org/wiki/Haversine_formula) mezi dvěma body zeměpisné šířky (vzdálenost "dobře kruh"), používáme trigonometrické funkce dostupné v rámci Hive, tedy:</span><span class="sxs-lookup"><span data-stu-id="92ff8-281">To see the comparison between actual trip distance and the [Haversine distance](http://en.wikipedia.org/wiki/Haversine_formula) between two longitude-latitude points (the "great circle" distance), we use the trigonometric functions available within Hive, thus :</span></span>

    set R=3959;
    set pi=radians(180);

    insert overwrite directory 'wasb:///queryoutputdir'

    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
    ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
     *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
     *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
     /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
     +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
     pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
    from nyctaxidb.trip
    where month=1
    and pickup_longitude between -90 and -30
    and pickup_latitude between 30 and 90
    and dropoff_longitude between -90 and -30
    and dropoff_latitude between 30 and 90;

<span data-ttu-id="92ff8-282">Ve výše uvedené dotazu R je radius zemském povrchu v miles a platformy je převést na radiánech.</span><span class="sxs-lookup"><span data-stu-id="92ff8-282">In the query above, R is the radius of the Earth in miles, and pi is converted to radians.</span></span> <span data-ttu-id="92ff8-283">Všimněte si, že body zeměpisné šířky jsou "filtrovány" odebrat hodnoty, které jsou daleko od oblasti NYC.</span><span class="sxs-lookup"><span data-stu-id="92ff8-283">Note that the longitude-latitude points are "filtered" to remove values that are far from the NYC area.</span></span>

<span data-ttu-id="92ff8-284">V takovém případě jsme naše výsledky zapisovat do adresář s názvem "queryoutputdir".</span><span class="sxs-lookup"><span data-stu-id="92ff8-284">In this case, we write our results to a directory called "queryoutputdir".</span></span> <span data-ttu-id="92ff8-285">Sekvence příkazů ukazuje následující obrázek nejprve vytvoří tento výstup adresář a potom spustí příkaz Hive.</span><span class="sxs-lookup"><span data-stu-id="92ff8-285">The sequence of commands shown below first creates this output directory, and then runs the Hive command.</span></span>

<span data-ttu-id="92ff8-286">Z adresáře řádku Hive spusťte příkaz:</span><span class="sxs-lookup"><span data-stu-id="92ff8-286">From the Hive directory prompt, run:</span></span>

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


<span data-ttu-id="92ff8-287">Výsledky dotazu se zapisují do 9 Azure blob ***queryoutputdir/000000\_0*** k ***queryoutputdir/000008\_0*** v kontejneru výchozí clusteru Hadoop.</span><span class="sxs-lookup"><span data-stu-id="92ff8-287">The query results are written to 9 Azure blobs ***queryoutputdir/000000\_0*** to  ***queryoutputdir/000008\_0*** under the default container of the Hadoop cluster.</span></span>

<span data-ttu-id="92ff8-288">Pokud chcete zobrazit velikost jednotlivých objektů BLOB, jsme spusťte následující příkaz z příkazového řádku adresář Hive:</span><span class="sxs-lookup"><span data-stu-id="92ff8-288">To see the size of the individual blobs, we run the following command from the Hive directory prompt :</span></span>

    hdfs dfs -ls wasb:///queryoutputdir

<span data-ttu-id="92ff8-289">Chcete-li zobrazit obsah daného souboru, vyslovte 000000\_0, používáme na Hadoop `copyToLocal` příkaz proto.</span><span class="sxs-lookup"><span data-stu-id="92ff8-289">To see the contents of a given file, say 000000\_0, we use Hadoop's `copyToLocal` command, thus.</span></span>

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [!WARNING]
> <span data-ttu-id="92ff8-290">`copyToLocal`může být velmi pomalé pro velké soubory a nedoporučuje se používat pro použití s nimi.</span><span class="sxs-lookup"><span data-stu-id="92ff8-290">`copyToLocal` can be very slow for large files, and is not recommended for use with them.</span></span>  
> 
> 

<span data-ttu-id="92ff8-291">Hlavní výhodou, že máte tato data jsou umístěny v objektu blob Azure je, že jsme může zkoumat data v rámci pomocí Azure Machine Learning [importovat Data] [ import-data] modulu.</span><span class="sxs-lookup"><span data-stu-id="92ff8-291">A key advantage of having this data reside in an Azure blob is that we may explore the data within Azure Machine Learning using the [Import Data][import-data] module.</span></span>

## <span data-ttu-id="92ff8-292"><a name="#downsample"></a>Dolů modely ukázkových dat a sestavení v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="92ff8-292"><a name="#downsample"></a>Down sample data and build models in Azure Machine Learning</span></span>
> [!NOTE]
> <span data-ttu-id="92ff8-293">To je obvykle **vědecký pracovník Data** úloh.</span><span class="sxs-lookup"><span data-stu-id="92ff8-293">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="92ff8-294">Po z analytické fáze nahodilého dat jsme nyní připraveni na dolů ukázková data pro vytváření modelů v Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="92ff8-294">After the exploratory data analysis phase, we are now ready to down sample the data for building models in Azure Machine Learning.</span></span> <span data-ttu-id="92ff8-295">V této části ukážeme, jak použít dotaz Hive na dolů ukázková data, která je potom k němu přistupovat z [importovat Data] [ import-data] modulu v Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="92ff8-295">In this section, we show how to use a Hive query to down sample the data, which is then accessed from the [Import Data][import-data] module in Azure Machine Learning.</span></span>

### <a name="down-sampling-the-data"></a><span data-ttu-id="92ff8-296">Dolů vzorkování dat</span><span class="sxs-lookup"><span data-stu-id="92ff8-296">Down sampling the data</span></span>
<span data-ttu-id="92ff8-297">Existují dva kroky v tomto postupu.</span><span class="sxs-lookup"><span data-stu-id="92ff8-297">There are two steps in this procedure.</span></span> <span data-ttu-id="92ff8-298">Nejdřív jsme připojit **nyctaxidb.trip** a **nyctaxidb.fare** tabulky na tři klíče, které jsou k dispozici ve všech záznamech: "medailonu", "zabezpečení\_licence", a "vyzvednutí\_data a času".</span><span class="sxs-lookup"><span data-stu-id="92ff8-298">First we join the **nyctaxidb.trip** and **nyctaxidb.fare** tables on three keys that are present in all records : "medallion", "hack\_license", and "pickup\_datetime".</span></span> <span data-ttu-id="92ff8-299">Pak se vygeneruje štítek binární klasifikace **vysypávány** a klasifikaci s více třída popisek **tip\_– třída**.</span><span class="sxs-lookup"><span data-stu-id="92ff8-299">We then generate a binary classification label **tipped** and a multi-class classification label **tip\_class**.</span></span>

<span data-ttu-id="92ff8-300">Abyste mohli použít na nabídku vzorků dat přímo z [importovat Data] [ import-data] modulu v Azure Machine Learning, je nutné ukládání výsledků dotazu na výše uvedené na interní tabulku Hive.</span><span class="sxs-lookup"><span data-stu-id="92ff8-300">To be able to use the down sampled data directly from the [Import Data][import-data] module in Azure Machine Learning, it is necessary to store the results of the above query to an internal Hive table.</span></span> <span data-ttu-id="92ff8-301">V co následuje můžeme vytvořit vnitřní tabulku Hive a naplnit její obsah se připojené k a dolů jen Vzorkovaná data.</span><span class="sxs-lookup"><span data-stu-id="92ff8-301">In what follows, we create an internal Hive table and populate its contents with the joined and down sampled data.</span></span>

<span data-ttu-id="92ff8-302">Dotaz se použije standardní funkce Hive přímo ke generování hodinu, den, týden roku, den v týdnu (1 zastupuje pondělí a 7 zastupuje neděle) od "vyzvednutí\_data a času" pole a přímé vzdálenost mezi vyzvednutí a dropoff umístění.</span><span class="sxs-lookup"><span data-stu-id="92ff8-302">The query applies standard Hive functions directly to generate the hour of day, week of year, weekday (1 stands for Monday, and 7 stands for Sunday) from the "pickup\_datetime" field,  and the direct distance between the pickup and dropoff locations.</span></span> <span data-ttu-id="92ff8-303">Uživatelé mohou odkazovat na [LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) pro úplný seznam takové funkce.</span><span class="sxs-lookup"><span data-stu-id="92ff8-303">Users can refer to [LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) for a complete list of such functions.</span></span>

<span data-ttu-id="92ff8-304">Dotaz pak dolů ukázky data tak, aby výsledky dotazu, můžete začlenit do Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="92ff8-304">The query then down samples the data so that the query results can fit into the Azure Machine Learning Studio.</span></span> <span data-ttu-id="92ff8-305">Pouze asi 1 % původní datové sady je importovat do nástroje Studio.</span><span class="sxs-lookup"><span data-stu-id="92ff8-305">Only about 1% of the original dataset is imported into the Studio.</span></span>

<span data-ttu-id="92ff8-306">Níže jsou obsah *ukázka\_hive\_Příprava\_pro\_aml\_full.hql* soubor, který připraví dat pro model vytváření v Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="92ff8-306">Below are the contents of *sample\_hive\_prepare\_for\_aml\_full.hql* file that prepares data for model building in Azure Machine Learning.</span></span>

        set R = 3959;
        set pi=radians(180);

        create table if not exists nyctaxidb.nyctaxi_downsampled_dataset (

        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\n'
        stored as textfile;

        --- now insert contents of the join into the above internal table

        insert overwrite table nyctaxidb.nyctaxi_downsampled_dataset
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class

        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
        *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
        +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance,
        rand() as sample_key

        from nyctaxidb.trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from nyctaxidb.fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01

<span data-ttu-id="92ff8-307">Spuštění tohoto dotazu z příkazového řádku adresář Hive:</span><span class="sxs-lookup"><span data-stu-id="92ff8-307">To run this query, from the Hive directory prompt :</span></span>

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

<span data-ttu-id="92ff8-308">Nyní je k dispozici vnitřní tabulku "nyctaxidb.nyctaxi_downsampled_dataset", který je přístupný pomocí [importovat Data] [ import-data] modulu z Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="92ff8-308">We now have an internal table "nyctaxidb.nyctaxi_downsampled_dataset" which can be accessed using the [Import Data][import-data] module from Azure Machine Learning.</span></span> <span data-ttu-id="92ff8-309">Kromě toho můžeme použít tuto datovou sadu pro vytváření modelů Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="92ff8-309">Furthermore, we may use this dataset for building Machine Learning models.</span></span>  

### <a name="use-the-import-data-module-in-azure-machine-learning-to-access-the-down-sampled-data"></a><span data-ttu-id="92ff8-310">Použít modul importovat Data v Azure Machine Learning pro přístup k dolů jen Vzorkovaná data</span><span class="sxs-lookup"><span data-stu-id="92ff8-310">Use the Import Data module in Azure Machine Learning to access the down sampled data</span></span>
<span data-ttu-id="92ff8-311">Jako požadavky na vystavování Hive dotazy v [importovat Data] [ import-data] modul Azure Machine Learning, potřebujeme přístup do Azure Machine Learning prostoru a přístup k přihlašovacím údajům clusteru a jeho přidružený účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="92ff8-311">As prerequisites for issuing Hive queries in the [Import Data][import-data] module of Azure Machine Learning, we need access to an Azure Machine Learning workspace and access to the credentials of the cluster and its associated storage account.</span></span>

<span data-ttu-id="92ff8-312">Některé podrobnosti o [importovat Data] [ import-data] modulu a parametry pro vstup:</span><span class="sxs-lookup"><span data-stu-id="92ff8-312">Some details on the [Import Data][import-data] module and the parameters to input :</span></span>

<span data-ttu-id="92ff8-313">**Identifikátor URI serveru HCatalog**: Pokud je název clusteru abc123, pak toto je jednoduše: https://abc123.azurehdinsight.net</span><span class="sxs-lookup"><span data-stu-id="92ff8-313">**HCatalog server URI**: If the cluster name is abc123, then this is simply : https://abc123.azurehdinsight.net</span></span>

<span data-ttu-id="92ff8-314">**Název uživatelského účtu Hadoop** : uživatelské jméno zvolené pro cluster (**není** uživatelské jméno vzdáleného přístupu)</span><span class="sxs-lookup"><span data-stu-id="92ff8-314">**Hadoop user account name** : The user name chosen for the cluster (**not** the remote access user name)</span></span>

<span data-ttu-id="92ff8-315">**Heslo účtu pož Hadoop** : heslo zvolené pro cluster (**není** heslo vzdáleného přístupu)</span><span class="sxs-lookup"><span data-stu-id="92ff8-315">**Hadoop ser account password** : The password chosen for the cluster (**not** the remote access password)</span></span>

<span data-ttu-id="92ff8-316">**Umístění výstupu dat** : to je zvolen jako Azure.</span><span class="sxs-lookup"><span data-stu-id="92ff8-316">**Location of output data** : This is chosen to be Azure.</span></span>

<span data-ttu-id="92ff8-317">**Název účtu úložiště Azure** : název výchozí účet úložiště, které jsou přidruženy ke clusteru.</span><span class="sxs-lookup"><span data-stu-id="92ff8-317">**Azure storage account name** : Name of the default storage account associated with the cluster.</span></span>

<span data-ttu-id="92ff8-318">**Název kontejneru Azure** : Toto je výchozí název kontejneru pro cluster a je obvykle stejný jako název clusteru.</span><span class="sxs-lookup"><span data-stu-id="92ff8-318">**Azure container name** : This is the default container name for the cluster, and is typically the same as the cluster name.</span></span> <span data-ttu-id="92ff8-319">Pro cluster s názvem "abc123" jde jenom abc123.</span><span class="sxs-lookup"><span data-stu-id="92ff8-319">For a cluster called "abc123", this is just abc123.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="92ff8-320">**Všechny tabulky chceme dotazování pomocí [importovat Data] [ import-data] modulu v Azure Machine Learning musí být interní tabulku.**</span><span class="sxs-lookup"><span data-stu-id="92ff8-320">**Any table we wish to query using the [Import Data][import-data] module in Azure Machine Learning must be an internal table.**</span></span> <span data-ttu-id="92ff8-321">Tip pro určení toho, jestli tabulka T v databázi D.db interní tabulku je následující.</span><span class="sxs-lookup"><span data-stu-id="92ff8-321">A tip for determining if a table T in a database D.db is an internal table is as follows.</span></span>
> 
> 

<span data-ttu-id="92ff8-322">Na řádku adresář Hive vydejte příkaz:</span><span class="sxs-lookup"><span data-stu-id="92ff8-322">From the Hive directory prompt, issue the command :</span></span>

    hdfs dfs -ls wasb:///D.db/T

<span data-ttu-id="92ff8-323">Pokud v tabulce je interní tabulku a je naplněna, musí zde zobrazí její obsah.</span><span class="sxs-lookup"><span data-stu-id="92ff8-323">If the table is an internal table and it is populated, its contents must show here.</span></span> <span data-ttu-id="92ff8-324">Zjistit, zda tabulka je interní tabulku je pomocí Průzkumníka úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="92ff8-324">Another way to determine whether a table is an internal table is to use the Azure Storage Explorer.</span></span> <span data-ttu-id="92ff8-325">Použije ji k přejděte do kontejneru výchozí název clusteru a potom filtrovat podle názvu tabulky.</span><span class="sxs-lookup"><span data-stu-id="92ff8-325">Use it to navigate to the default container name of the cluster, and then filter by the table name.</span></span> <span data-ttu-id="92ff8-326">Pokud tabulka a její obsah objeví, potvrdíte, že je interní tabulku.</span><span class="sxs-lookup"><span data-stu-id="92ff8-326">If the table and its contents show up, this confirms that it is an internal table.</span></span>

<span data-ttu-id="92ff8-327">Zde je snímek dotaz Hive a [importovat Data] [ import-data] modul:</span><span class="sxs-lookup"><span data-stu-id="92ff8-327">Here is a snapshot of the Hive query and the [Import Data][import-data] module:</span></span>

![Dotaz Hive pro Import dat modul](./media/machine-learning-data-science-process-hive-walkthrough/1eTYf52.png)

<span data-ttu-id="92ff8-329">Poznámka: od našich nižší jen Vzorkovaná data nachází ve výchozím kontejneru, výsledný dotaz Hive z Azure Machine Learning je velmi jednoduchý a je jen "vybrat * z nyctaxidb.nyctaxi\_po převzorkování dolů\_data".</span><span class="sxs-lookup"><span data-stu-id="92ff8-329">Note that since our down sampled data resides in the default container, the resulting Hive query from Azure Machine Learning is very simple and is just a "SELECT * FROM nyctaxidb.nyctaxi\_downsampled\_data".</span></span>

<span data-ttu-id="92ff8-330">Datová sada může nyní použije jako výchozí bod pro vytváření modelů Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="92ff8-330">The dataset may now be used as the starting point for building Machine Learning models.</span></span>

### <span data-ttu-id="92ff8-331"><a name="mlmodel"></a>Vytvářet modely v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="92ff8-331"><a name="mlmodel"></a>Build models in Azure Machine Learning</span></span>
<span data-ttu-id="92ff8-332">Jsme je nyní možné přejít k vytváření modelů a modelu nasazení v [Azure Machine Learning](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="92ff8-332">We are now able to proceed to model building and model deployment in [Azure Machine Learning](https://studio.azureml.net).</span></span> <span data-ttu-id="92ff8-333">Data jsou připravena k nám pro použití v adresách identifikované výše předpovědi problémy:</span><span class="sxs-lookup"><span data-stu-id="92ff8-333">The data is ready for us to use in addressing the prediction problems identified above:</span></span>

<span data-ttu-id="92ff8-334">**1. Binární klasifikace**: K předvídání, jestli tip byl placené cesty.</span><span class="sxs-lookup"><span data-stu-id="92ff8-334">**1. Binary classification**: To predict whether or not a tip was paid for a trip.</span></span>

<span data-ttu-id="92ff8-335">**Použít student:** logistic regression Two-class</span><span class="sxs-lookup"><span data-stu-id="92ff8-335">**Learner used:** Two-class logistic regression</span></span>

<span data-ttu-id="92ff8-336">a.</span><span class="sxs-lookup"><span data-stu-id="92ff8-336">a.</span></span> <span data-ttu-id="92ff8-337">U tohoto problému našeho štítku cíl (nebo třída) je "vysypávány".</span><span class="sxs-lookup"><span data-stu-id="92ff8-337">For this problem, our target (or class) label is "tipped".</span></span> <span data-ttu-id="92ff8-338">Naše původní datové sady vzorků nižší má několik sloupců, které jsou nevracení cíl tohoto experimentu klasifikace.</span><span class="sxs-lookup"><span data-stu-id="92ff8-338">Our original down-sampled dataset has a few columns that are target leaks for this classification experiment.</span></span> <span data-ttu-id="92ff8-339">Konkrétně: tip\_třídy, tip\_velikostí a celkový počet\_částka zobrazení informace o cílové štítek, který není k dispozici na testování čas.</span><span class="sxs-lookup"><span data-stu-id="92ff8-339">In particular : tip\_class, tip\_amount, and total\_amount reveal information about the target label that is not available at testing time.</span></span> <span data-ttu-id="92ff8-340">Jsme tyto sloupce odebrat z aspekt pomocí [výběr sloupců v datové sadě] [ select-columns] modulu.</span><span class="sxs-lookup"><span data-stu-id="92ff8-340">We remove these columns from consideration using the [Select Columns in Dataset][select-columns] module.</span></span>

<span data-ttu-id="92ff8-341">Snímek níže ukazuje naše experimentu předpovědět, zda byl placené tip pro danou cestu.</span><span class="sxs-lookup"><span data-stu-id="92ff8-341">The snapshot below shows our experiment to predict whether or not a tip was paid for a given trip.</span></span>

![Experiment snímku](./media/machine-learning-data-science-process-hive-walkthrough/QGxRz5A.png)

<span data-ttu-id="92ff8-343">b.</span><span class="sxs-lookup"><span data-stu-id="92ff8-343">b.</span></span> <span data-ttu-id="92ff8-344">Pro tento experiment naše distribuce popisek cíl byly přibližně 1:1.</span><span class="sxs-lookup"><span data-stu-id="92ff8-344">For this experiment, our target label distributions were roughly 1:1.</span></span>

<span data-ttu-id="92ff8-345">Snímek níže znázorňuje rozdělení tipu popisky třídy pro problém binární klasifikace.</span><span class="sxs-lookup"><span data-stu-id="92ff8-345">The snapshot below shows the distribution of tip class labels for the binary classification problem.</span></span>

![Distribuce popisků tip – třída](./media/machine-learning-data-science-process-hive-walkthrough/9mM4jlD.png)

<span data-ttu-id="92ff8-347">V důsledku toho jsme získat AUC 0.987, jak je znázorněno na obrázku níže.</span><span class="sxs-lookup"><span data-stu-id="92ff8-347">As a result, we obtain an AUC of 0.987 as shown in the figure below.</span></span>

![Hodnota AUC](./media/machine-learning-data-science-process-hive-walkthrough/8JDT0F8.png)

<span data-ttu-id="92ff8-349">**2. Více třídami klasifikace**: K předpovědi rozsahu tip objemu placené pro cestu pomocí dříve definovaných tříd.</span><span class="sxs-lookup"><span data-stu-id="92ff8-349">**2. Multiclass classification**: To predict the range of tip amounts paid for the trip, using the previously defined classes.</span></span>

<span data-ttu-id="92ff8-350">**Použít student:** více třídami logistic regression</span><span class="sxs-lookup"><span data-stu-id="92ff8-350">**Learner used:** Multiclass logistic regression</span></span>

<span data-ttu-id="92ff8-351">a.</span><span class="sxs-lookup"><span data-stu-id="92ff8-351">a.</span></span> <span data-ttu-id="92ff8-352">Pro tento problém popisek naše cíle (nebo třída) je "tip\_třída" které můžete provést jednu z pěti hodnot (0,1,2,3,4).</span><span class="sxs-lookup"><span data-stu-id="92ff8-352">For this problem, our target (or class) label is "tip\_class" which can take one of five values (0,1,2,3,4).</span></span> <span data-ttu-id="92ff8-353">Jako v případě binární klasifikace máme několik sloupců, které jsou nevracení cíl tohoto experimentu.</span><span class="sxs-lookup"><span data-stu-id="92ff8-353">As in the binary classification case, we have a few columns that are target leaks for this experiment.</span></span> <span data-ttu-id="92ff8-354">Konkrétně: vysypávány, tip\_velikost, celkový počet\_částka zobrazení informace o cílové štítek, který není k dispozici na testování čas.</span><span class="sxs-lookup"><span data-stu-id="92ff8-354">In particular : tipped, tip\_amount, total\_amount reveal information about the target label that is not available at testing time.</span></span> <span data-ttu-id="92ff8-355">Jsme odebrat tyto sloupce pomocí [výběr sloupců v datové sadě] [ select-columns] modulu.</span><span class="sxs-lookup"><span data-stu-id="92ff8-355">We remove these columns using the [Select Columns in Dataset][select-columns] module.</span></span>

<span data-ttu-id="92ff8-356">Snímek níže ukazuje naše experimentu k předpovědi, které koš tip je pravděpodobné, že tak, aby spadal (třída 0: tip = $0, 1 – třída: tip > $0 a tip < = 5, 2 – třída: tip > $5 a tip < = 10, třídy 3: tip > $10 a tip < = 20 Třída 4: tip > 20)</span><span class="sxs-lookup"><span data-stu-id="92ff8-356">The snapshot below shows our experiment to predict in which bin a tip is likely to fall ( Class 0: tip = $0, class 1 : tip > $0 and tip <= $5, Class 2 : tip > $5 and tip <= $10, Class 3 : tip > $10 and tip <= $20, Class 4 : tip > $20)</span></span>

![Experiment snímku](./media/machine-learning-data-science-process-hive-walkthrough/5ztv0n0.png)

<span data-ttu-id="92ff8-358">Nyní ukážeme, jak vypadá naše skutečné testovací třída distribuční.</span><span class="sxs-lookup"><span data-stu-id="92ff8-358">We now show what our actual test class distribution looks like.</span></span> <span data-ttu-id="92ff8-359">Vidíte, že třída 0 a 1 třídy jsou běžně se vyskytujícím, ostatní třídy vyskytují jen vzácně.</span><span class="sxs-lookup"><span data-stu-id="92ff8-359">We see that while Class 0 and Class 1 are prevalent, the other classes are rare.</span></span>

![Testování distribuční – třída](./media/machine-learning-data-science-process-hive-walkthrough/Vy1FUKa.png)

<span data-ttu-id="92ff8-361">b.</span><span class="sxs-lookup"><span data-stu-id="92ff8-361">b.</span></span> <span data-ttu-id="92ff8-362">Pro tento experiment používáme nedorozuměním matice podívat se na naše přesnostmi předpovědi.</span><span class="sxs-lookup"><span data-stu-id="92ff8-362">For this experiment, we use a confusion matrix to look at our prediction accuracies.</span></span> <span data-ttu-id="92ff8-363">Příklad najdete níž.</span><span class="sxs-lookup"><span data-stu-id="92ff8-363">This is shown below.</span></span>

![Matice nedorozuměním](./media/machine-learning-data-science-process-hive-walkthrough/cxFmErM.png)

<span data-ttu-id="92ff8-365">Všimněte si, že naše třída přesnostmi na běžně se vyskytujícím třídy je poměrně vhodný, modelu neprovádí dobrou práci "vzdělávání" na vzácnějších třídy.</span><span class="sxs-lookup"><span data-stu-id="92ff8-365">Note that while our class accuracies on the prevalent classes is quite good, the model does not do a good job of "learning" on the rarer classes.</span></span>

<span data-ttu-id="92ff8-366">**3. Úloha regrese**: K předvídání množství tip placené cesty.</span><span class="sxs-lookup"><span data-stu-id="92ff8-366">**3. Regression task**: To predict the amount of tip paid for a trip.</span></span>

<span data-ttu-id="92ff8-367">**Použít student:** Boosted rozhodovací strom</span><span class="sxs-lookup"><span data-stu-id="92ff8-367">**Learner used:** Boosted decision tree</span></span>

<span data-ttu-id="92ff8-368">a.</span><span class="sxs-lookup"><span data-stu-id="92ff8-368">a.</span></span> <span data-ttu-id="92ff8-369">Pro tento problém popisek naše cíle (nebo třída) je "tip\_velikost".</span><span class="sxs-lookup"><span data-stu-id="92ff8-369">For this problem, our target (or class) label is "tip\_amount".</span></span> <span data-ttu-id="92ff8-370">Naše nevracení cíl v tomto případě jsou: vysypávány, tip\_třída, celkový počet\_velikost; tyto proměnné odhalit informace o velikosti tip pro, který je obvykle dostupná na testování čas.</span><span class="sxs-lookup"><span data-stu-id="92ff8-370">Our target leaks in this case are : tipped, tip\_class, total\_amount ; all these variables reveal information about the tip amount that is typically unavailable at testing time.</span></span> <span data-ttu-id="92ff8-371">Jsme odebrat tyto sloupce pomocí [výběr sloupců v datové sadě] [ select-columns] modulu.</span><span class="sxs-lookup"><span data-stu-id="92ff8-371">We remove these columns using the [Select Columns in Dataset][select-columns] module.</span></span>

<span data-ttu-id="92ff8-372">Belows snímku ukazuje naše experimentu k předvídání množství dané špičky.</span><span class="sxs-lookup"><span data-stu-id="92ff8-372">The snapshot belows shows our experiment to predict the amount of the given tip.</span></span>

![Experiment snímku](./media/machine-learning-data-science-process-hive-walkthrough/11TZWgV.png)

<span data-ttu-id="92ff8-374">b.</span><span class="sxs-lookup"><span data-stu-id="92ff8-374">b.</span></span> <span data-ttu-id="92ff8-375">V případě problémů regrese měříme přesnostmi naše předpovědí prohlížením kvadratická chyba v jsou předpovědi, koeficient spolehlivosti a podobně.</span><span class="sxs-lookup"><span data-stu-id="92ff8-375">For regression problems, we measure the accuracies of our prediction by looking at the squared error in the predictions, the coefficient of determination, and the like.</span></span> <span data-ttu-id="92ff8-376">Ukážeme tyto níže.</span><span class="sxs-lookup"><span data-stu-id="92ff8-376">We show these below.</span></span>

![Předpověď statistiky](./media/machine-learning-data-science-process-hive-walkthrough/Jat9mrz.png)

<span data-ttu-id="92ff8-378">Vidíme, že o koeficient spolehlivosti je 0.709, zdání přibližně 71 % odchylku vysvětluje naše koeficienty modelu.</span><span class="sxs-lookup"><span data-stu-id="92ff8-378">We see that about the coefficient of determination is 0.709, implying about 71% of the variance is explained by our model coefficients.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="92ff8-379">Můžete najít další informace o Azure Machine Learning a jak přístup a použít jej [co je Machine Learning?](machine-learning-what-is-machine-learning.md).</span><span class="sxs-lookup"><span data-stu-id="92ff8-379">To learn more about Azure Machine Learning and how to access and use it, please refer to [What's Machine Learning?](machine-learning-what-is-machine-learning.md).</span></span> <span data-ttu-id="92ff8-380">Je velmi užitečná prostředku pro přehrávání s spoustu experimenty Machine Learning v Azure Machine Learning [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/).</span><span class="sxs-lookup"><span data-stu-id="92ff8-380">A very useful resource for playing with a bunch of Machine Learning experiments on Azure Machine Learning is the [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/).</span></span> <span data-ttu-id="92ff8-381">Galerie vysvětluje škálu experimenty a poskytuje důkladné Úvod do řadu funkcí služby Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="92ff8-381">The Gallery covers a gamut of experiments and provides a thorough introduction into the range of capabilities of Azure Machine Learning.</span></span>
> 
> 

## <a name="license-information"></a><span data-ttu-id="92ff8-382">Informace o licenci</span><span class="sxs-lookup"><span data-stu-id="92ff8-382">License Information</span></span>
<span data-ttu-id="92ff8-383">Tento ukázkový postup a jeho doprovodné skriptů jsou sdíleny společností Microsoft v rámci licencí MIT.</span><span class="sxs-lookup"><span data-stu-id="92ff8-383">This sample walkthrough and its accompanying scripts are shared by Microsoft under the MIT license.</span></span> <span data-ttu-id="92ff8-384">Zkontrolujte prosím soubor LICENSE.txt v adresáři ukázkový kód na Githubu další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="92ff8-384">Please check the LICENSE.txt file in in the directory of the sample code on GitHub for more details.</span></span>

## <a name="references"></a><span data-ttu-id="92ff8-385">Odkazy</span><span class="sxs-lookup"><span data-stu-id="92ff8-385">References</span></span>
<span data-ttu-id="92ff8-386">• [Stránce pro stažení Andrés Monroy NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/)</span><span class="sxs-lookup"><span data-stu-id="92ff8-386">•    [Andrés Monroy NYC Taxi Trips Download Page](http://www.andresmh.com/nyctaxitrips/)</span></span>  
<span data-ttu-id="92ff8-387">• [FOILing NYC Taxi cestě dat podle Whong Jan](http://chriswhong.com/open-data/foil_nyc_taxi/) </span><span class="sxs-lookup"><span data-stu-id="92ff8-387">•    [FOILing NYC’s Taxi Trip Data by Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/) </span></span>  
<span data-ttu-id="92ff8-388">• [NYC taxíkem a Limousine Komise výzkum a statistiky](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span><span class="sxs-lookup"><span data-stu-id="92ff8-388">•    [NYC Taxi and Limousine Commission Research and Statistics](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span></span>

[2]: ./media/machine-learning-data-science-process-hive-walkthrough/output-hive-results-3.png
[11]: ./media/machine-learning-data-science-process-hive-walkthrough/hive-reader-properties.png
[12]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-training.png
[13]: ./media/machine-learning-data-science-process-hive-walkthrough/create-scoring-experiment.png
[14]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-scoring.png
[15]: ./media/machine-learning-data-science-process-hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
