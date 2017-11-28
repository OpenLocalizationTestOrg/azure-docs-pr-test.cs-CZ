---
title: "aaaExplore data v Hadoop clusteru a vytváření modelů v Azure Machine Learning | Microsoft Docs"
description: "Pomocí hello proces vědecké účely dat Team začátku do konce scénář nasazení HDInsight Hadoop clusteru toobuild a nasadit model použití veřejně dostupné datové sady."
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
ms.openlocfilehash: a371032e356ffc366af0d6fbe364af281b6efd19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="hello-team-data-science-process-in-action-use-azure-hdinsight-hadoop-clusters"></a><span data-ttu-id="3c967-103">Hello proces vědecké účely Team dat v akci: použití Azure HDInsight Hadoop clusterů</span><span class="sxs-lookup"><span data-stu-id="3c967-103">hello Team Data Science Process in action: Use Azure HDInsight Hadoop clusters</span></span>
<span data-ttu-id="3c967-104">V tomto návodu použijeme hello [tým datové vědy procesu (TDSP)](data-science-process-overview.md) začátku do konce scénář pomocí [clusteru Azure HDInsight Hadoop](https://azure.microsoft.com/services/hdinsight/) toostore, prozkoumejte a veřejně funkci pracovníka data z hello k dispozici [NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/) datovou sadu a toodown vzorová hello data.</span><span class="sxs-lookup"><span data-stu-id="3c967-104">In this walkthrough, we use hello [Team Data Science Process (TDSP)](data-science-process-overview.md) in an end-to-end scenario using an [Azure HDInsight Hadoop cluster](https://azure.microsoft.com/services/hdinsight/) toostore, explore and feature engineer data from hello publicly available [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset, and toodown sample hello data.</span></span> <span data-ttu-id="3c967-105">Modely hello dat jsou integrované s Azure Machine Learning toohandle binární a více třídami klasifikace a regrese prediktivní úlohy.</span><span class="sxs-lookup"><span data-stu-id="3c967-105">Models of hello data are built with Azure Machine Learning toohandle binary and multiclass classification and regression predictive tasks.</span></span>

<span data-ttu-id="3c967-106">Návod, který ukazuje, jak toohandle větší datovou sadu (1 terabajt) o podobném scénáři pomocí HDInsight Hadoop clusterů pro zpracování dat najdete v tématu [Team Data vědecké účely proces – pomocí Azure HDInsight Hadoop clusterů v 1 TB datovou sadu](machine-learning-data-science-process-hive-criteo-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="3c967-106">For a walkthrough that shows how toohandle a larger (1 terabyte) dataset for a similar scenario using HDInsight Hadoop clusters for data processing, see [Team Data Science Process - Using Azure HDInsight Hadoop Clusters on a 1 TB dataset](machine-learning-data-science-process-hive-criteo-walkthrough.md).</span></span>

<span data-ttu-id="3c967-107">Je také možné toouse IPython poznámkového bloku tooaccomplish hello úloh vidění hello návod použití hello 1 TB datové sady.</span><span class="sxs-lookup"><span data-stu-id="3c967-107">It is also possible toouse an IPython notebook tooaccomplish hello tasks presented hello walkthrough using hello 1 TB dataset.</span></span> <span data-ttu-id="3c967-108">Uživatelé, kteří by jako tento postup by se měli obrátit tootry hello [Criteo návod pomocí připojení Hive ODBC](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) tématu.</span><span class="sxs-lookup"><span data-stu-id="3c967-108">Users who would like tootry this approach should consult hello [Criteo walkthrough using a Hive ODBC connection](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) topic.</span></span>

## <span data-ttu-id="3c967-109"><a name="dataset"></a>Popis NYC taxíkem služebních cest datové sady</span><span class="sxs-lookup"><span data-stu-id="3c967-109"><a name="dataset"></a>NYC Taxi Trips Dataset description</span></span>
<span data-ttu-id="3c967-110">Hello NYC taxíkem cestě dat je přibližně 20GB komprimované hodnot oddělených čárkami (CSV) souborů (nekomprimovaným ~ 48GB), která obsahuje více než 173 milionů jednotlivých cest a hello tarify placené pro každou cestu.</span><span class="sxs-lookup"><span data-stu-id="3c967-110">hello NYC Taxi Trip data is about 20GB of compressed comma-separated values (CSV) files (~48GB uncompressed), comprising more than 173 million individual trips and hello fares paid for each trip.</span></span> <span data-ttu-id="3c967-111">Každý záznam cestě zahrnuje hello vyzvednutí a odkládací umístění a čas, číslo licence anonymizovaná hackerský (ovladač) a číslo Medailon (taxi na jedinečné id).</span><span class="sxs-lookup"><span data-stu-id="3c967-111">Each trip record includes hello pickup and drop-off location and time, anonymized hack (driver's) license number and medallion (taxi’s unique id) number.</span></span> <span data-ttu-id="3c967-112">Hello dat obsahuje všechny služebních cest v hello roku 2013 a je součástí hello následující dvě datové sady pro každý měsíc:</span><span class="sxs-lookup"><span data-stu-id="3c967-112">hello data covers all trips in hello year 2013 and is provided in hello following two datasets for each month:</span></span>

1. <span data-ttu-id="3c967-113">Hello 'trip_data' CSV soubory obsahují podrobnosti o cestě, například na počtu cestujících, vyzvednutí a dropoff body, doba trvání cesty a délka cesty.</span><span class="sxs-lookup"><span data-stu-id="3c967-113">hello 'trip_data' CSV files contain trip details, such as number of passengers, pickup and dropoff points, trip duration, and trip length.</span></span> <span data-ttu-id="3c967-114">Tady je několik ukázkových záznamů:</span><span class="sxs-lookup"><span data-stu-id="3c967-114">Here are a few sample records:</span></span>
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. <span data-ttu-id="3c967-115">Hello 'trip_fare' CSV soubory obsahují podrobnosti o tarif hello placené pro každou cestu, například typ platby, velikost tarif, příplatek a daně, tipy a mýtné a celkovou velikost hello placené.</span><span class="sxs-lookup"><span data-stu-id="3c967-115">hello 'trip_fare' CSV files contain details of hello fare paid for each trip, such as payment type, fare amount, surcharge and taxes, tips and tolls, and hello total amount paid.</span></span> <span data-ttu-id="3c967-116">Tady je několik ukázkových záznamů:</span><span class="sxs-lookup"><span data-stu-id="3c967-116">Here are a few sample records:</span></span>
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

<span data-ttu-id="3c967-117">Hello jedinečné klíče toojoin cestě\_dat a cesty\_tarif se skládá z pole hello: medailonu, hackerský\_licence a vyzvednutí\_data a času.</span><span class="sxs-lookup"><span data-stu-id="3c967-117">hello unique key toojoin trip\_data and trip\_fare is composed of hello fields: medallion, hack\_licence and pickup\_datetime.</span></span>

<span data-ttu-id="3c967-118">všechny tooget hello podrobnosti relevantní tooa konkrétní cesty, je dostatečná toojoin tři klíče: hello "medailonu", "zabezpečení\_licence" a "vyzvednutí\_data a času".</span><span class="sxs-lookup"><span data-stu-id="3c967-118">tooget all of hello details relevant tooa particular trip, it is sufficient toojoin with three keys: hello "medallion", "hack\_license" and "pickup\_datetime".</span></span>

<span data-ttu-id="3c967-119">Jsme popisují některé další podrobné informace o datech hello, když jsme jejich uložení do tabulek Hive za chvíli.</span><span class="sxs-lookup"><span data-stu-id="3c967-119">We describe some more details of hello data when we store them into Hive tables shortly.</span></span>

## <span data-ttu-id="3c967-120"><a name="mltasks"></a>Příklady předpovědi úlohy</span><span class="sxs-lookup"><span data-stu-id="3c967-120"><a name="mltasks"></a>Examples of prediction tasks</span></span>
<span data-ttu-id="3c967-121">Když se blíží dat, určení hello druh předpovědi, že chcete toomake na základě jeho analýzy pomáhá vysvětlení hello úlohy, budete potřebovat tooinclude v procesu.</span><span class="sxs-lookup"><span data-stu-id="3c967-121">When approaching data, determining hello kind of predictions you want toomake based on its analysis helps clarify hello tasks that you will need tooinclude in your process.</span></span>
<span data-ttu-id="3c967-122">Tady jsou tři příklady předpovědi problémy, které jsme adresy v tomto návodu, jejichž formulování je založena na hello *tip\_velikost*:</span><span class="sxs-lookup"><span data-stu-id="3c967-122">Here are three examples of prediction problems that we address in this walkthrough whose formulation is based on hello *tip\_amount*:</span></span>

1. <span data-ttu-id="3c967-123">**Binární klasifikace**: předpovědět, zda byl tip placené cesty, tj. *tip\_velikost* větší než $0 je pozitivní příklad, při *tip\_velikost* $ 0 je záporný příklad.</span><span class="sxs-lookup"><span data-stu-id="3c967-123">**Binary classification**: Predict whether or not a tip was paid for a trip, i.e. a *tip\_amount* that is greater than $0 is a positive example, while a *tip\_amount* of $0 is a negative example.</span></span>
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0
2. <span data-ttu-id="3c967-124">**Více třídami klasifikace**: rozsah hello toopredict tip objemu zaplacení hello cesty.</span><span class="sxs-lookup"><span data-stu-id="3c967-124">**Multiclass classification**: toopredict hello range of tip amounts paid for hello trip.</span></span> <span data-ttu-id="3c967-125">Jsme rozdělit hello *tip\_velikost* do pěti přihrádek nebo třídy:</span><span class="sxs-lookup"><span data-stu-id="3c967-125">We divide hello *tip\_amount* into five bins or classes:</span></span>
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. <span data-ttu-id="3c967-126">**Úloha regrese**: toopredict hello množství hello tip placené cesty.</span><span class="sxs-lookup"><span data-stu-id="3c967-126">**Regression task**: toopredict hello amount of hello tip paid for a trip.</span></span>  

## <span data-ttu-id="3c967-127"><a name="setup"></a>Nastavení clusteru HDInsight Hadoop pro pokročilou analýzu</span><span class="sxs-lookup"><span data-stu-id="3c967-127"><a name="setup"></a>Set up an HDInsight Hadoop cluster for advanced analytics</span></span>
> [!NOTE]
> <span data-ttu-id="3c967-128">Obvykle se jedná **správce** úloh.</span><span class="sxs-lookup"><span data-stu-id="3c967-128">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="3c967-129">Můžete nastavit prostředí Azure pro pokročilou analýzu využívajícího clusteru služby HDInsight v tři kroky:</span><span class="sxs-lookup"><span data-stu-id="3c967-129">You can set up an Azure environment for advanced analytics that employs an HDInsight cluster in three steps:</span></span>

1. <span data-ttu-id="3c967-130">[Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md): Tento účet úložiště se používá pro ukládání dat v Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="3c967-130">[Create a storage account](../storage/common/storage-create-storage-account.md): This storage account is used for storing data in Azure Blob Storage.</span></span> <span data-ttu-id="3c967-131">Hello data použitá v clusterech HDInsight se také nachází zde.</span><span class="sxs-lookup"><span data-stu-id="3c967-131">hello data used in HDInsight clusters also resides here.</span></span>
2. <span data-ttu-id="3c967-132">[Přizpůsobit Azure HDInsight Hadoop clusterů pro hello pokročilé analýzy proces a technologie](machine-learning-data-science-customize-hadoop-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="3c967-132">[Customize Azure HDInsight Hadoop clusters for hello Advanced Analytics Process and Technology](machine-learning-data-science-customize-hadoop-cluster.md).</span></span> <span data-ttu-id="3c967-133">Tento krok vytvoří cluster s 64-bit Anaconda Python 2.7 nainstalované na všech uzlech služby Azure HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="3c967-133">This step creates an Azure HDInsight Hadoop cluster with 64-bit Anaconda Python 2.7 installed on all nodes.</span></span> <span data-ttu-id="3c967-134">Existují dvě tooremember důležité kroky při přizpůsobení clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3c967-134">There are two important steps tooremember while customizing your HDInsight cluster.</span></span>
   
   * <span data-ttu-id="3c967-135">Mějte na paměti, toolink hello úložiště účet vytvořený v kroku 1 k vašemu clusteru HDInsight při jejím vytváření.</span><span class="sxs-lookup"><span data-stu-id="3c967-135">Remember toolink hello storage account created in step 1 with your HDInsight cluster when creating it.</span></span> <span data-ttu-id="3c967-136">Tento účet úložiště je použité tooaccess data, která je zpracovat v rámci clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="3c967-136">This storage account is used tooaccess data that is processed within hello cluster.</span></span>
   * <span data-ttu-id="3c967-137">Po vytvoření clusteru hello povolte vzdálený přístup toohello hlavního uzlu clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="3c967-137">After hello cluster is created, enable Remote Access toohello head node of hello cluster.</span></span> <span data-ttu-id="3c967-138">Přejděte toohello **konfigurace** a klikněte na **povolit vzdálené**.</span><span class="sxs-lookup"><span data-stu-id="3c967-138">Navigate toohello **Configuration** tab and click **Enable Remote**.</span></span> <span data-ttu-id="3c967-139">Tento krok určuje hello uživatelská pověření použitá pro vzdálené přihlášení.</span><span class="sxs-lookup"><span data-stu-id="3c967-139">This step specifies hello user credentials used for remote login.</span></span>
3. <span data-ttu-id="3c967-140">[Vytvořit pracovní prostor služby Azure Machine Learning](machine-learning-create-workspace.md): Tento Azure Machine Learning pracovního prostoru není modely použité toobuild strojové učení.</span><span class="sxs-lookup"><span data-stu-id="3c967-140">[Create an Azure Machine Learning workspace](machine-learning-create-workspace.md): This Azure Machine Learning workspace is used toobuild machine learning models.</span></span> <span data-ttu-id="3c967-141">Tato úloha je určeno po dokončení počáteční data zkoumání a dolů vzorkování pomocí hello clusteru HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3c967-141">This task is addressed after completing an initial data exploration and down sampling using hello HDInsight cluster.</span></span>

## <span data-ttu-id="3c967-142"><a name="getdata"></a>Získat hello data z veřejné zdroje</span><span class="sxs-lookup"><span data-stu-id="3c967-142"><a name="getdata"></a>Get hello data from a public source</span></span>
> [!NOTE]
> <span data-ttu-id="3c967-143">Obvykle se jedná **správce** úloh.</span><span class="sxs-lookup"><span data-stu-id="3c967-143">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="3c967-144">tooget hello [NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/) datové sady z veřejného umístění, můžete použít některou z metod hello popsané v [tooand přesun dat z Azure Blob Storage](machine-learning-data-science-move-azure-blob.md) toocopy hello data tooyour počítače.</span><span class="sxs-lookup"><span data-stu-id="3c967-144">tooget hello [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset from its public location, you may use any of hello methods described in [Move Data tooand from Azure Blob Storage](machine-learning-data-science-move-azure-blob.md) toocopy hello data tooyour machine.</span></span>

<span data-ttu-id="3c967-145">Zde jsme popisují, jak pomocí nástroje AzCopy tootransfer hello soubory obsahující data.</span><span class="sxs-lookup"><span data-stu-id="3c967-145">Here we describe how use AzCopy tootransfer hello files containing data.</span></span> <span data-ttu-id="3c967-146">toodownload a nainstalujte AzCopy postupujte podle pokynů hello v [Začínáme s hello příkazového řádku Azcopy](../storage/common/storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="3c967-146">toodownload and install AzCopy follow hello instructions at [Getting Started with hello AzCopy Command-Line Utility](../storage/common/storage-use-azcopy.md).</span></span>

1. <span data-ttu-id="3c967-147">Z okna příkazového řádku, vydávání hello následující příkazy AzCopy, nahraďte *< path_to_data_folder >* s cílem požadované hello:</span><span class="sxs-lookup"><span data-stu-id="3c967-147">From a Command Prompt window, issue hello following AzCopy commands, replacing *<path_to_data_folder>* with hello desired destination:</span></span>

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

1. <span data-ttu-id="3c967-148">Po dokončení kopírování hello celkem 24 komprimované soubory jsou ve složce data hello vybrali.</span><span class="sxs-lookup"><span data-stu-id="3c967-148">When hello copy completes, a total of 24 zipped files are in hello data folder chosen.</span></span> <span data-ttu-id="3c967-149">Rozbalte hello stažené soubory toohello stejný adresář na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="3c967-149">Unzip hello downloaded files toohello same directory on your local machine.</span></span> <span data-ttu-id="3c967-150">Poznamenejte si hello složky, kde jsou uloženy soubory hello nekomprimovaným.</span><span class="sxs-lookup"><span data-stu-id="3c967-150">Make a note of hello folder where hello uncompressed files reside.</span></span> <span data-ttu-id="3c967-151">Tato složka bude hello odkazované tooas *< cesta\_k\_unzipped_data\_soubory\>*  je následující.</span><span class="sxs-lookup"><span data-stu-id="3c967-151">This folder will be referred tooas hello *<path\_to\_unzipped_data\_files\>* is what follows.</span></span>

## <span data-ttu-id="3c967-152"><a name="upload"></a>Nahrát hello toohello výchozí kontejner dat clusteru Azure HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="3c967-152"><a name="upload"></a>Upload hello data toohello default container of Azure HDInsight Hadoop cluster</span></span>
> [!NOTE]
> <span data-ttu-id="3c967-153">Obvykle se jedná **správce** úloh.</span><span class="sxs-lookup"><span data-stu-id="3c967-153">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="3c967-154">Následující příkazy AzCopy, nahraďte v hello hello následující parametry s hello skutečné hodnoty, které jste zadali při vytváření clusteru Hadoop hello a rozbalení hello datových souborů.</span><span class="sxs-lookup"><span data-stu-id="3c967-154">In hello following AzCopy commands, replace hello following parameters with hello actual values that you specified when creating hello Hadoop cluster and unzipping hello data files.</span></span>

* <span data-ttu-id="3c967-155">***& č. 60; path_to_data_folder >*** directory hello (spolu s cesta) na počítači, které obsahují hello rozbalené datových souborů</span><span class="sxs-lookup"><span data-stu-id="3c967-155">***&#60;path_to_data_folder>*** hello directory (along with path) on your machine that contain hello unzipped data files</span></span>  
* <span data-ttu-id="3c967-156">***& č. 60; název účtu úložiště clusteru Hadoop >*** hello účtu úložiště přidruženého k vašemu clusteru HDInsight</span><span class="sxs-lookup"><span data-stu-id="3c967-156">***&#60;storage account name of Hadoop cluster>*** hello storage account associated with your HDInsight cluster</span></span>
* <span data-ttu-id="3c967-157">***& č. 60; výchozí kontejner clusteru Hadoop >*** výchozí kontejner hello používá váš cluster.</span><span class="sxs-lookup"><span data-stu-id="3c967-157">***&#60;default container of Hadoop cluster>*** hello default container used by your cluster.</span></span> <span data-ttu-id="3c967-158">Všimněte si, že název hello hello výchozího kontejneru je obvykle hello stejný název jako samotný cluster hello.</span><span class="sxs-lookup"><span data-stu-id="3c967-158">Note that hello name of hello default container is usually hello same name as hello cluster itself.</span></span> <span data-ttu-id="3c967-159">Například pokud hello clusteru se nazývá "abc123.azurehdinsight.net", je výchozí kontejner hello abc123.</span><span class="sxs-lookup"><span data-stu-id="3c967-159">For example, if hello cluster is called "abc123.azurehdinsight.net", hello default container is abc123.</span></span>
* <span data-ttu-id="3c967-160">***& č. 60; klíč účtu úložiště >*** hello klíč pro účet úložiště hello používá váš cluster</span><span class="sxs-lookup"><span data-stu-id="3c967-160">***&#60;storage account key>*** hello key for hello storage account used by your cluster</span></span>

<span data-ttu-id="3c967-161">Z příkazového řádku nebo okno prostředí Windows PowerShell v počítači spusťte následující dva příkazy AzCopy hello.</span><span class="sxs-lookup"><span data-stu-id="3c967-161">From a Command Prompt or a Windows PowerShell window in your machine, run hello following two AzCopy commands.</span></span>

<span data-ttu-id="3c967-162">Tento příkaz odešle data cestě hello příliš***nyctaxitripraw*** adresář v kontejneru výchozí hello clusteru Hadoop hello.</span><span class="sxs-lookup"><span data-stu-id="3c967-162">This command uploads hello trip data too***nyctaxitripraw*** directory in hello default container of hello Hadoop cluster.</span></span>

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

<span data-ttu-id="3c967-163">Tento příkaz odešle datový tarif hello příliš***nyctaxifareraw*** adresář v kontejneru výchozí hello clusteru Hadoop hello.</span><span class="sxs-lookup"><span data-stu-id="3c967-163">This command uploads hello fare data too***nyctaxifareraw*** directory in hello default container of hello Hadoop cluster.</span></span>

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

<span data-ttu-id="3c967-164">Hello data by teď v Azure Blob Storage a připravena toobe využívat v rámci clusteru HDInsight hello.</span><span class="sxs-lookup"><span data-stu-id="3c967-164">hello data should now in Azure Blob Storage and ready toobe consumed within hello HDInsight cluster.</span></span>

## <span data-ttu-id="3c967-165"><a name="#download-hql-files"></a>Přihlaste se k hlavnímu uzlu clusteru Hadoop hello a a připravit pro analýzu nahodilého dat</span><span class="sxs-lookup"><span data-stu-id="3c967-165"><a name="#download-hql-files"></a>Log into hello head node of Hadoop cluster and and prepare for exploratory data analysis</span></span>
> [!NOTE]
> <span data-ttu-id="3c967-166">Obvykle se jedná **správce** úloh.</span><span class="sxs-lookup"><span data-stu-id="3c967-166">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="3c967-167">tooaccess hello hlavního uzlu clusteru hello pro analýzu nahodilého dat a dolů vzorkování hello dat, postupujte podle uvedených v postupu hello [přístup hello uzlu clusteru Hadoop Head](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span><span class="sxs-lookup"><span data-stu-id="3c967-167">tooaccess hello head node of hello cluster for exploratory data analysis and down sampling of hello data, follow hello procedure outlined in [Access hello Head Node of Hadoop Cluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span></span>

<span data-ttu-id="3c967-168">V tomto návodu budeme používat hlavně dotazy, které jsou napsané v [Hive](https://hive.apache.org/), jako SQL dotazovací jazyk, explorations tooperform předběžné data.</span><span class="sxs-lookup"><span data-stu-id="3c967-168">In this walkthrough, we primarily use queries written in [Hive](https://hive.apache.org/), a SQL-like query language, tooperform preliminary data explorations.</span></span> <span data-ttu-id="3c967-169">dotazy Hive Hello jsou uložené v souborech .hql.</span><span class="sxs-lookup"><span data-stu-id="3c967-169">hello Hive queries are stored in .hql files.</span></span> <span data-ttu-id="3c967-170">Jsme pak dolů ukázkové toobe tato data použít Azure Machine Learning pro vytváření modelů.</span><span class="sxs-lookup"><span data-stu-id="3c967-170">We then down sample this data toobe used within Azure Machine Learning for building models.</span></span>

<span data-ttu-id="3c967-171">tooprepare hello clusteru pro analýzu dat nahodilého, jsme stáhnout soubory .hql hello obsahující hello relevantní Hive skripty z [githubu](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) tooa místního adresáře (C:\temp) hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="3c967-171">tooprepare hello cluster for exploratory data analysis, we download hello .hql files containing hello relevant Hive scripts from [github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) tooa local directory (C:\temp) on hello head node.</span></span> <span data-ttu-id="3c967-172">toodo se otevřete hello **příkazového řádku** uvnitř hello hlavního uzlu v clusteru a problém hello hello následující dva příkazy:</span><span class="sxs-lookup"><span data-stu-id="3c967-172">toodo this, open hello **Command Prompt** from within hello head node of hello cluster and issue hello following two commands:</span></span>

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

<span data-ttu-id="3c967-173">Tyto dva příkazy stáhne všechny soubory .hql potřebné v tomto návodu toohello místním adresáři ***C:\temp &#92;*** v hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="3c967-173">These two commands will download all .hql files needed in this walkthrough toohello local directory ***C:\temp&#92;*** in hello head node.</span></span>

## <span data-ttu-id="3c967-174"><a name="#hive-db-tables"></a>Vytvoření databáze Hive a tabulky rozdělena na oddíly po měsících</span><span class="sxs-lookup"><span data-stu-id="3c967-174"><a name="#hive-db-tables"></a>Create Hive database and tables partitioned by month</span></span>
> [!NOTE]
> <span data-ttu-id="3c967-175">Obvykle se jedná **správce** úloh.</span><span class="sxs-lookup"><span data-stu-id="3c967-175">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="3c967-176">Snažíme se teď tabulek Hive připraven toocreate pro naší datové sadě taxíkem NYC.</span><span class="sxs-lookup"><span data-stu-id="3c967-176">We are now ready toocreate Hive tables for our NYC taxi dataset.</span></span>
<span data-ttu-id="3c967-177">V hello hlavního uzlu v clusteru Hadoop hello, otevřete hello ***Hadoop příkazového řádku*** hello plochy hello hlavního uzlu a zadejte adresář Hive hello zadáním příkazu hello</span><span class="sxs-lookup"><span data-stu-id="3c967-177">In hello head node of hello Hadoop cluster, open hello ***Hadoop Command Line*** on hello desktop of hello head node, and enter hello Hive directory by entering hello command</span></span>

    cd %hive_home%\bin

> [!NOTE]
> <span data-ttu-id="3c967-178">**Spusťte všechny příkazy Hive v tomto návodu z hello výše Hive bin / directory řádku. To se postará o všech problémech, cesta automaticky. Používáme podmínky hello "Hive directory výzva", "Hive bin / directory řádku" a "Hadoop příkazový řádek" zcela zaměnitelným významem v tomto návodu.**</span><span class="sxs-lookup"><span data-stu-id="3c967-178">**Run all Hive commands in this walkthrough from hello above Hive bin/ directory prompt. This will take care of any path issues automatically. We use hello terms "Hive directory prompt", "Hive bin/ directory prompt",  and "Hadoop Command Line" interchangeably in this walkthrough.**</span></span>
> 
> 

<span data-ttu-id="3c967-179">Hello Hive directory řádku zadejte následující příkaz v Hadoop příkazového řádku hello hlavního uzlu toosubmit hello Hive dotaz toocreate Hive databáze a tabulky hello:</span><span class="sxs-lookup"><span data-stu-id="3c967-179">From hello Hive directory prompt, enter hello following command in Hadoop Command Line of hello head node toosubmit hello Hive query toocreate Hive database and tables:</span></span>

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

<span data-ttu-id="3c967-180">Tady je hello obsah hello ***C:\temp\sample\_hive\_vytvořit\_db\_a\_tables.hql*** souboru, který vytvoří databázi Hive ***nyctaxidb *** a tabulky ***cestě*** a ***tarif***.</span><span class="sxs-lookup"><span data-stu-id="3c967-180">Here is hello content of hello ***C:\temp\sample\_hive\_create\_db\_and\_tables.hql*** file which creates Hive database ***nyctaxidb*** and tables ***trip*** and ***fare***.</span></span>

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

<span data-ttu-id="3c967-181">Tento skript Hive vytvoří dvě tabulky:</span><span class="sxs-lookup"><span data-stu-id="3c967-181">This Hive script creates two tables:</span></span>

* <span data-ttu-id="3c967-182">Hello "cesty" tabulka obsahuje podrobnosti o cestě z každé pravé (podrobnosti o ovladači, výstupní čas, vzdálenost cesty a časy)</span><span class="sxs-lookup"><span data-stu-id="3c967-182">hello "trip" table contains trip details of each ride (driver details, pickup time, trip distance and times)</span></span>
* <span data-ttu-id="3c967-183">Hello "tarif" tabulka obsahuje podrobnosti o tarif (tarif částka tip, mýtné a příplatky).</span><span class="sxs-lookup"><span data-stu-id="3c967-183">hello "fare" table contains fare details (fare amount, tip amount, tolls and surcharges).</span></span>

<span data-ttu-id="3c967-184">Potřebujete-li žádné další pomoc s tyto postupy nebo chcete tooinvestigate alternativní těch, najdete v tématu hello [dotazů odeslání Hive přímo z hello Hadoop příkazového řádku ](machine-learning-data-science-move-hive-tables.md#submit).</span><span class="sxs-lookup"><span data-stu-id="3c967-184">If you need any additional assistance with these procedures or want tooinvestigate alternative ones, see hello section [Submit Hive queries directly from hello Hadoop Command Line ](machine-learning-data-science-move-hive-tables.md#submit).</span></span>

## <span data-ttu-id="3c967-185"><a name="#load-data"></a>Načíst Data tabulky tooHive podle oddílů</span><span class="sxs-lookup"><span data-stu-id="3c967-185"><a name="#load-data"></a>Load Data tooHive tables by partitions</span></span>
> [!NOTE]
> <span data-ttu-id="3c967-186">Obvykle se jedná **správce** úloh.</span><span class="sxs-lookup"><span data-stu-id="3c967-186">This is typically an **Admin** task.</span></span>
> 
> 

<span data-ttu-id="3c967-187">Datová sada taxíkem Hello NYC má přirozené dělení po měsících, které používáme tooenable zpracování a dotazů rychlejší.</span><span class="sxs-lookup"><span data-stu-id="3c967-187">hello NYC taxi dataset has a natural partitioning by month, which we use tooenable faster processing and query times.</span></span> <span data-ttu-id="3c967-188">Hello dole uvedené příkazy Powershellu (vystavené adresář hello Hive pomocí hello **Hadoop příkazového řádku**) načíst data toohello "cesty" a "tarif" tabulek Hive rozdělena na oddíly po měsících.</span><span class="sxs-lookup"><span data-stu-id="3c967-188">hello PowerShell commands below (issued from hello Hive directory using hello **Hadoop Command Line**) load data toohello "trip" and "fare" Hive tables partitioned by month.</span></span>

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

<span data-ttu-id="3c967-189">Hello *ukázka\_hive\_načíst\_data\_podle\_partitions.hql* soubor obsahuje následující hello **NAČÍST** příkazy.</span><span class="sxs-lookup"><span data-stu-id="3c967-189">hello *sample\_hive\_load\_data\_by\_partitions.hql* file contains hello following **LOAD** commands.</span></span>

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

<span data-ttu-id="3c967-190">Všimněte si, že počet dotazů Hive, kterou tady používáme v procesu zkoumání hello zahrnují vyhledávání na právě jeden oddíl nebo na pouze několik oddílů.</span><span class="sxs-lookup"><span data-stu-id="3c967-190">Note that a number of Hive queries we use here in hello exploration process involve looking at just a single partition or at only a couple of partitions.</span></span> <span data-ttu-id="3c967-191">Ale tyto dotazy by bylo možné spustit v rámci celého datového hello.</span><span class="sxs-lookup"><span data-stu-id="3c967-191">But these queries could be run across hello entire data.</span></span>

### <span data-ttu-id="3c967-192"><a name="#show-db"></a>Zobrazit databází v hello clusteru HDInsight Hadoop</span><span class="sxs-lookup"><span data-stu-id="3c967-192"><a name="#show-db"></a>Show databases in hello HDInsight Hadoop cluster</span></span>
<span data-ttu-id="3c967-193">tooshow hello databáze vytvořené v clusteru HDInsight Hadoop v okně hello Hadoop příkazového řádku spusťte následující příkaz v Hadoop příkazového řádku hello:</span><span class="sxs-lookup"><span data-stu-id="3c967-193">tooshow hello databases created in HDInsight Hadoop cluster inside hello Hadoop Command Line window, run hello following command in Hadoop Command Line:</span></span>

    hive -e "show databases;"

### <span data-ttu-id="3c967-194"><a name="#show-tables"></a>Zobrazení tabulek Hive hello v databázi nyctaxidb hello</span><span class="sxs-lookup"><span data-stu-id="3c967-194"><a name="#show-tables"></a>Show hello Hive tables in hello nyctaxidb database</span></span>
<span data-ttu-id="3c967-195">tooshow hello tabulky v databázi nyctaxidb hello, spusťte následující příkaz v Hadoop příkazového řádku hello:</span><span class="sxs-lookup"><span data-stu-id="3c967-195">tooshow hello tables in hello nyctaxidb database, run hello following command in Hadoop Command Line:</span></span>

    hive -e "show tables in nyctaxidb;"

<span data-ttu-id="3c967-196">Může potvrdit, že hello tabulky jsou rozdělena na oddíly vydáním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="3c967-196">We can confirm that hello tables are partitioned by issuing hello command below:</span></span>

    hive -e "show partitions nyctaxidb.trip;"

<span data-ttu-id="3c967-197">Hello očekává, že výstupní jsou uvedeny níže:</span><span class="sxs-lookup"><span data-stu-id="3c967-197">hello expected output is shown below:</span></span>

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

<span data-ttu-id="3c967-198">Podobně jsme můžete zajistit, že tento hello tarif tabulka je rozdělena na oddíly vydáním níže uvedeného příkazu hello:</span><span class="sxs-lookup"><span data-stu-id="3c967-198">Similarly, we can ensure that hello fare table is partitioned by issuing hello command below:</span></span>

    hive -e "show partitions nyctaxidb.fare;"

<span data-ttu-id="3c967-199">Hello očekává, že výstupní jsou uvedeny níže:</span><span class="sxs-lookup"><span data-stu-id="3c967-199">hello expected output is shown below:</span></span>

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

## <span data-ttu-id="3c967-200"><a name="#explore-hive"></a>Zkoumání dat a funkce analýzy v Hive</span><span class="sxs-lookup"><span data-stu-id="3c967-200"><a name="#explore-hive"></a>Data exploration and feature engineering in Hive</span></span>
> [!NOTE]
> <span data-ttu-id="3c967-201">To je obvykle **vědecký pracovník Data** úloh.</span><span class="sxs-lookup"><span data-stu-id="3c967-201">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="3c967-202">Hello zkoumání dat a funkce engineering úlohy pro hello data načtená do tabulek Hive hello můžete udělat pomocí dotazů Hive.</span><span class="sxs-lookup"><span data-stu-id="3c967-202">hello data exploration and feature engineering tasks for hello data loaded into hello Hive tables can be accomplished using Hive queries.</span></span> <span data-ttu-id="3c967-203">Zde jsou příklady takových úloh, že jsme vás provede procesem v této části:</span><span class="sxs-lookup"><span data-stu-id="3c967-203">Here are examples of such tasks that we walk you through in this section:</span></span>

* <span data-ttu-id="3c967-204">Zobrazte hello top 10 záznamů v obou tabulkách.</span><span class="sxs-lookup"><span data-stu-id="3c967-204">View hello top 10 records in both tables.</span></span>
* <span data-ttu-id="3c967-205">Prozkoumejte data distribuce několik polí v různých časových oken.</span><span class="sxs-lookup"><span data-stu-id="3c967-205">Explore data distributions of a few fields in varying time windows.</span></span>
* <span data-ttu-id="3c967-206">Prozkoumejte data quality hello zeměpisné šířky a délky polí.</span><span class="sxs-lookup"><span data-stu-id="3c967-206">Investigate data quality of hello longitude and latitude fields.</span></span>
* <span data-ttu-id="3c967-207">Generovat binární a více třídami klasifikační štítky podle hello **tip\_velikost**.</span><span class="sxs-lookup"><span data-stu-id="3c967-207">Generate binary and multiclass classification labels based on hello **tip\_amount**.</span></span>
* <span data-ttu-id="3c967-208">Vygenerujte funkce computing hello přímé cestě vzdálenosti.</span><span class="sxs-lookup"><span data-stu-id="3c967-208">Generate features by computing hello direct trip distances.</span></span>

### <a name="exploration-view-hello-top-10-records-in-table-trip"></a><span data-ttu-id="3c967-209">Zkoumání: Zobrazení hello top 10 záznamy v cestě tabulky</span><span class="sxs-lookup"><span data-stu-id="3c967-209">Exploration: View hello top 10 records in table trip</span></span>
> [!NOTE]
> <span data-ttu-id="3c967-210">To je obvykle **vědecký pracovník Data** úloh.</span><span class="sxs-lookup"><span data-stu-id="3c967-210">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="3c967-211">jaká data hello vypadá jako toosee, jsme zkontrolujte 10 záznamy ze všech tabulek.</span><span class="sxs-lookup"><span data-stu-id="3c967-211">toosee what hello data looks like, we examine 10 records from each table.</span></span> <span data-ttu-id="3c967-212">Spusťte hello samostatně následující dva dotazy z řádku directory hello Hive v hello Hadoop příkazového řádku konzoly tooinspect hello záznamy.</span><span class="sxs-lookup"><span data-stu-id="3c967-212">Run hello following two queries separately from hello Hive directory prompt in hello Hadoop Command Line console tooinspect hello records.</span></span>

<span data-ttu-id="3c967-213">tooget hello top 10 záznamy v tabulce hello "cesty" od hello první měsíc:</span><span class="sxs-lookup"><span data-stu-id="3c967-213">tooget hello top 10 records in hello table "trip" from hello first month:</span></span>

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

<span data-ttu-id="3c967-214">tooget hello top 10 záznamy v tabulce hello "tarif" z hello první měsíc:</span><span class="sxs-lookup"><span data-stu-id="3c967-214">tooget hello top 10 records in hello table "fare" from hello first month:</span></span>

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

<span data-ttu-id="3c967-215">Je často užitečné toosave hello záznamy tooa soubor pro pohodlný zobrazení.</span><span class="sxs-lookup"><span data-stu-id="3c967-215">It is often useful toosave hello records tooa file for convenient viewing.</span></span> <span data-ttu-id="3c967-216">Malé změny toohello výše dotazu toho dosahuje:</span><span class="sxs-lookup"><span data-stu-id="3c967-216">A small change toohello above query accomplishes this:</span></span>

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-hello-number-of-records-in-each-of-hello-12-partitions"></a><span data-ttu-id="3c967-217">Zkoumání: Zobrazit hello počet záznamů v každé hello 12 oddílů</span><span class="sxs-lookup"><span data-stu-id="3c967-217">Exploration: View hello number of records in each of hello 12 partitions</span></span>
> [!NOTE]
> <span data-ttu-id="3c967-218">To je obvykle **vědecký pracovník Data** úloh.</span><span class="sxs-lookup"><span data-stu-id="3c967-218">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="3c967-219">Zájmu je hello, jak se liší hello počet cest hello kalendářním roce.</span><span class="sxs-lookup"><span data-stu-id="3c967-219">Of interest is hello how hello number of trips varies during hello calendar year.</span></span> <span data-ttu-id="3c967-220">Seskupení podle měsíce umožňuje toosee této distribuce služebních cest, které bude vypadat takto.</span><span class="sxs-lookup"><span data-stu-id="3c967-220">Grouping by month allows us toosee what this distribution of trips looks like.</span></span>

    hive -e "select month, count(*) from nyctaxidb.trip group by month;"

<span data-ttu-id="3c967-221">To nám dává výstup hello:</span><span class="sxs-lookup"><span data-stu-id="3c967-221">This gives us hello output :</span></span>

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

<span data-ttu-id="3c967-222">Zde hello první sloupec je hello měsíc a hello druhou je hello počet cest pro daný měsíc.</span><span class="sxs-lookup"><span data-stu-id="3c967-222">Here, hello first column is hello month and hello second is hello number of trips for that month.</span></span>

<span data-ttu-id="3c967-223">Také jsme můžete počet hello celkový počet záznamů v naší datové sadě cestě vydáním hello následující příkazového řádku adresář hello Hive.</span><span class="sxs-lookup"><span data-stu-id="3c967-223">We can also count hello total number of records in our trip data set by issuing hello following command at hello Hive directory prompt.</span></span>

    hive -e "select count(*) from nyctaxidb.trip;"

<span data-ttu-id="3c967-224">Dostaneme:</span><span class="sxs-lookup"><span data-stu-id="3c967-224">This yields:</span></span>

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

<span data-ttu-id="3c967-225">Pomocí příkazů podobné toothose pro hello cestě datové sady, jsme vydávat dotazy Hive z hello Hive directory řádku pro hello tarif datové sady toovalidate hello počet záznamů.</span><span class="sxs-lookup"><span data-stu-id="3c967-225">Using commands similar toothose shown for hello trip data set, we can issue Hive queries from hello Hive directory prompt for hello fare data set toovalidate hello number of records.</span></span>

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

<span data-ttu-id="3c967-226">To nám dává výstup hello:</span><span class="sxs-lookup"><span data-stu-id="3c967-226">This gives us hello output:</span></span>

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

<span data-ttu-id="3c967-227">Všimněte si, že pro obě sady dat se vrátí hello přesně stejný počet služebních cest za měsíc.</span><span class="sxs-lookup"><span data-stu-id="3c967-227">Note that hello exact same number of trips per month is returned for both data sets.</span></span> <span data-ttu-id="3c967-228">To poskytuje hello první ověření, že hello byla data načtena správně.</span><span class="sxs-lookup"><span data-stu-id="3c967-228">This provides hello first validation that hello data has been loaded correctly.</span></span>

<span data-ttu-id="3c967-229">Počítání hello celkový počet záznamů v sadě dat tarif hello lze provést pomocí příkazu hello níže hello Hive directory řádku:</span><span class="sxs-lookup"><span data-stu-id="3c967-229">Counting hello total number of records in hello fare data set can be done using hello command below from hello Hive directory prompt:</span></span>

    hive -e "select count(*) from nyctaxidb.fare;"

<span data-ttu-id="3c967-230">Dostaneme:</span><span class="sxs-lookup"><span data-stu-id="3c967-230">This yields :</span></span>

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

<span data-ttu-id="3c967-231">Celkový počet záznamů v obou tabulkách Hello je také hello stejné.</span><span class="sxs-lookup"><span data-stu-id="3c967-231">hello total number of records in both tables is also hello same.</span></span> <span data-ttu-id="3c967-232">To poskytuje druhý ověření, že hello byla data načtena správně.</span><span class="sxs-lookup"><span data-stu-id="3c967-232">This provides a second validation that hello data has been loaded correctly.</span></span>

### <a name="exploration-trip-distribution-by-medallion"></a><span data-ttu-id="3c967-233">Zkoumání: Cestě distribuce podle Medailon</span><span class="sxs-lookup"><span data-stu-id="3c967-233">Exploration: Trip distribution by medallion</span></span>
> [!NOTE]
> <span data-ttu-id="3c967-234">To je obvykle **vědecký pracovník Data** úloh.</span><span class="sxs-lookup"><span data-stu-id="3c967-234">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="3c967-235">Tento příklad identifikuje Medailon hello (taxi čísla) s více než 100 služebních cest v daném časovém období.</span><span class="sxs-lookup"><span data-stu-id="3c967-235">This example identifies hello medallion (taxi numbers) with more than 100 trips within a given time period.</span></span> <span data-ttu-id="3c967-236">výhody Hello dotazu z hello oddíly přístup k tabulce vzhledem k tomu, že je podmíněno proměnnou oddílu hello **měsíc**.</span><span class="sxs-lookup"><span data-stu-id="3c967-236">hello query benefits from hello partitioned table access since it is conditioned by hello partition variable **month**.</span></span> <span data-ttu-id="3c967-237">Hello výsledky dotazu jsou zapsány tooa místního souboru queryoutput.tsv v `C:\temp` hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="3c967-237">hello query results are written tooa local file queryoutput.tsv in `C:\temp` on hello head node.</span></span>

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

<span data-ttu-id="3c967-238">Tady je obsah hello *ukázka\_hive\_cestě\_počet\_podle\_medallion.hql* souboru kontroly.</span><span class="sxs-lookup"><span data-stu-id="3c967-238">Here is hello content of *sample\_hive\_trip\_count\_by\_medallion.hql* file for inspection.</span></span>

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

<span data-ttu-id="3c967-239">Hello Medailon v sadě dat taxíkem NYC hello identifikuje jedinečný soubor cab.</span><span class="sxs-lookup"><span data-stu-id="3c967-239">hello medallion in hello NYC taxi data set identifies a unique cab.</span></span> <span data-ttu-id="3c967-240">Abychom mohli identifikovat, které soubory CAB jsou "zaneprázdněný" žádostí, které z nich větší než počet cest provedené v konkrétním časovém období.</span><span class="sxs-lookup"><span data-stu-id="3c967-240">We can identify which cabs are "busy" by asking which ones made more than a certain number of trips in a particular time period.</span></span> <span data-ttu-id="3c967-241">Hello následující příklad určuje soubory CAB, které provádí víc než sto cest v hello první tři měsíce a uloží hello dotazu výsledky tooa místního souboru C:\temp\queryoutput.tsv.</span><span class="sxs-lookup"><span data-stu-id="3c967-241">hello following example identifies cabs that made more than a hundred trips in hello first three months, and saves hello query results tooa local file, C:\temp\queryoutput.tsv.</span></span>

<span data-ttu-id="3c967-242">Tady je obsah hello *ukázka\_hive\_cestě\_počet\_podle\_medallion.hql* souboru kontroly.</span><span class="sxs-lookup"><span data-stu-id="3c967-242">Here is hello content of *sample\_hive\_trip\_count\_by\_medallion.hql* file for inspection.</span></span>

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

<span data-ttu-id="3c967-243">Z hello podregistru directory řádku následující příkaz hello problému:</span><span class="sxs-lookup"><span data-stu-id="3c967-243">From hello Hive directory prompt, issue hello command below :</span></span>

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a><span data-ttu-id="3c967-244">Zkoumání: Cestě distribuce podle Medailon a hack_license</span><span class="sxs-lookup"><span data-stu-id="3c967-244">Exploration: Trip distribution by medallion and hack_license</span></span>
> [!NOTE]
> <span data-ttu-id="3c967-245">To je obvykle **vědecký pracovník Data** úloh.</span><span class="sxs-lookup"><span data-stu-id="3c967-245">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="3c967-246">Při prohlížení datové sady, chceme často tooexamine hello počet výskytů co skupin hodnot.</span><span class="sxs-lookup"><span data-stu-id="3c967-246">When exploring a dataset, we frequently want tooexamine hello number of co-occurences of groups of values.</span></span> <span data-ttu-id="3c967-247">Tato část uveďte příklad, jak toodo to pro soubory CAB a ovladače.</span><span class="sxs-lookup"><span data-stu-id="3c967-247">This section provide an example of how toodo this for cabs and drivers.</span></span>

<span data-ttu-id="3c967-248">Hello *ukázka\_hive\_cestě\_počet\_podle\_Medailon\_license.hql* skupin souborů datový tarif hello nastavena na "Medailon" a "hack_license" a vrátí počet každé kombinaci.</span><span class="sxs-lookup"><span data-stu-id="3c967-248">hello *sample\_hive\_trip\_count\_by\_medallion\_license.hql* file groups hello fare data set on "medallion" and "hack_license" and returns counts of each combination.</span></span> <span data-ttu-id="3c967-249">Níže jsou jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="3c967-249">Below are its contents.</span></span>

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

<span data-ttu-id="3c967-250">Tento dotaz vrací určitý ovladač kombinací seřazené podle sestupné počet cest a souborů cab.</span><span class="sxs-lookup"><span data-stu-id="3c967-250">This query returns cab and particular driver combinations ordered by descending number of trips.</span></span>

<span data-ttu-id="3c967-251">Z hello Hive directory řádku, spusťte:</span><span class="sxs-lookup"><span data-stu-id="3c967-251">From hello Hive directory prompt, run :</span></span>

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

<span data-ttu-id="3c967-252">výsledky dotazu Hello se zapisují tooa místního souboru C:\temp\queryoutput.tsv.</span><span class="sxs-lookup"><span data-stu-id="3c967-252">hello query results are written tooa local file C:\temp\queryoutput.tsv.</span></span>

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitudelatitude-records"></a><span data-ttu-id="3c967-253">Zkoumání: Vyhodnocování kvality dat. kontrolou neplatné délky nebo zeměpisnou šířku záznamů</span><span class="sxs-lookup"><span data-stu-id="3c967-253">Exploration: Assessing data quality by checking for invalid longitude/latitude records</span></span>
> [!NOTE]
> <span data-ttu-id="3c967-254">To je obvykle **vědecký pracovník Data** úloh.</span><span class="sxs-lookup"><span data-stu-id="3c967-254">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="3c967-255">Běžné cíl analýzy nahodilého dat je tooweed se neplatný nebo chybných záznamů.</span><span class="sxs-lookup"><span data-stu-id="3c967-255">A common objective of exploratory data analysis is tooweed out invalid or bad records.</span></span> <span data-ttu-id="3c967-256">Hello příklad v této části určuje, zda buď hello zeměpisné šířky nebo délky pole obsahovat hodnotu daleko mimo hello NYC oblasti.</span><span class="sxs-lookup"><span data-stu-id="3c967-256">hello example in this section determines whether either hello longitude or latitude fields contain a value far outside hello NYC area.</span></span> <span data-ttu-id="3c967-257">Vzhledem k tomu, že je pravděpodobné, že tyto záznamy mají hodnoty chybné zeměpisné šířky, chceme tooeliminate je z všechna data, která je toobe používá pro modelování.</span><span class="sxs-lookup"><span data-stu-id="3c967-257">Since it is likely that such records have an erroneous longitude-latitude values, we want tooeliminate them from any data that is toobe used for modeling.</span></span>

<span data-ttu-id="3c967-258">Tady je obsah hello *ukázka\_hive\_kvality\_assessment.hql* souboru kontroly.</span><span class="sxs-lookup"><span data-stu-id="3c967-258">Here is hello content of *sample\_hive\_quality\_assessment.hql* file for inspection.</span></span>

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


<span data-ttu-id="3c967-259">Z hello Hive directory řádku, spusťte:</span><span class="sxs-lookup"><span data-stu-id="3c967-259">From hello Hive directory prompt, run :</span></span>

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

<span data-ttu-id="3c967-260">Hello *-S* argument zahrnuté v tomto příkazu potlačí hello stav obrazovky výtisk úloh Hive mapy nebo snižte hello.</span><span class="sxs-lookup"><span data-stu-id="3c967-260">hello *-S* argument included in this command suppresses hello status screen printout of hello Hive Map/Reduce jobs.</span></span> <span data-ttu-id="3c967-261">To je užitečné, protože umožňuje hello obrazovky tisk hello Hive dotaz výstup čitelnější.</span><span class="sxs-lookup"><span data-stu-id="3c967-261">This is useful because it makes hello screen print of hello Hive query output more readable.</span></span>

### <a name="exploration-binary-class-distributions-of-trip-tips"></a><span data-ttu-id="3c967-262">Zkoumání: Binární třída distribuce cestě tipy</span><span class="sxs-lookup"><span data-stu-id="3c967-262">Exploration: Binary class distributions of trip tips</span></span>
> [!NOTE]
> <span data-ttu-id="3c967-263">To je obvykle **vědecký pracovník Data** úloh.</span><span class="sxs-lookup"><span data-stu-id="3c967-263">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="3c967-264">Pro problém binární klasifikace hello uvedených v hello [příklady úloh předpovědi](machine-learning-data-science-process-hive-walkthrough.md#mltasks) části, je užitečné tooknow, zda byl zadán tip, nebo ne.</span><span class="sxs-lookup"><span data-stu-id="3c967-264">For hello binary classification problem outlined in hello [Examples of prediction tasks](machine-learning-data-science-process-hive-walkthrough.md#mltasks) section, it is useful tooknow whether a tip was given or not.</span></span> <span data-ttu-id="3c967-265">Toto rozdělení tipy je binární:</span><span class="sxs-lookup"><span data-stu-id="3c967-265">This distribution of tips is binary:</span></span>

* <span data-ttu-id="3c967-266">Zadaný Tip (třída 1, tip\_částka > $0)</span><span class="sxs-lookup"><span data-stu-id="3c967-266">tip given(Class 1, tip\_amount > $0)</span></span>  
* <span data-ttu-id="3c967-267">žádné tip (třída 0, tip\_velikost = 0).</span><span class="sxs-lookup"><span data-stu-id="3c967-267">no tip (Class 0, tip\_amount = $0).</span></span>

<span data-ttu-id="3c967-268">Hello *ukázka\_hive\_vysypávány\_frequencies.hql* tomu souboru vidíte níže.</span><span class="sxs-lookup"><span data-stu-id="3c967-268">hello *sample\_hive\_tipped\_frequencies.hql* file shown below does this.</span></span>

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

<span data-ttu-id="3c967-269">Z hello Hive directory řádku, spusťte:</span><span class="sxs-lookup"><span data-stu-id="3c967-269">From hello Hive directory prompt, run:</span></span>

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-hello-multiclass-setting"></a><span data-ttu-id="3c967-270">Zkoumání: Třída distribuce v nastavení více třídami hello</span><span class="sxs-lookup"><span data-stu-id="3c967-270">Exploration: Class distributions in hello multiclass setting</span></span>
> [!NOTE]
> <span data-ttu-id="3c967-271">To je obvykle **vědecký pracovník Data** úloh.</span><span class="sxs-lookup"><span data-stu-id="3c967-271">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="3c967-272">Pro problém více třídami klasifikace hello uvedených v hello [příklady úloh předpovědi](machine-learning-data-science-process-hive-walkthrough.md#mltasks) tato datová sada také poskytuje vlastní tooa přirozené klasifikace, kde rádi bychom znali toopredict hello množství hello tipy pro zadaný oddíl.</span><span class="sxs-lookup"><span data-stu-id="3c967-272">For hello multiclass classification problem outlined in hello [Examples of prediction tasks](machine-learning-data-science-process-hive-walkthrough.md#mltasks) section this data set also lends itself tooa natural classification where we would like toopredict hello amount of hello tips given.</span></span> <span data-ttu-id="3c967-273">Můžeme použít přihrádek toodefine tip rozsahy v dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="3c967-273">We can use bins toodefine tip ranges in hello query.</span></span> <span data-ttu-id="3c967-274">tooget hello třída distribuce pro hello různých rozsahů tip, používáme hello *ukázka\_hive\_tip\_rozsah\_frequencies.hql* souboru.</span><span class="sxs-lookup"><span data-stu-id="3c967-274">tooget hello class distributions for hello various tip ranges, we use hello *sample\_hive\_tip\_range\_frequencies.hql* file.</span></span> <span data-ttu-id="3c967-275">Níže jsou jeho obsah.</span><span class="sxs-lookup"><span data-stu-id="3c967-275">Below are its contents.</span></span>

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

<span data-ttu-id="3c967-276">Spusťte následující příkaz z příkazového řádku Hadoop konzoly hello:</span><span class="sxs-lookup"><span data-stu-id="3c967-276">Run hello following command from Hadoop Command Line console:</span></span>

    hive -f "C:\temp\sample_hive_tip_range_frequencies.hql"

### <a name="exploration-compute-direct-distance-between-two-longitude-latitude-locations"></a><span data-ttu-id="3c967-277">Zkoumání: Výpočetní přímé vzdálenost mezi dvěma umístěními zeměpisné šířky</span><span class="sxs-lookup"><span data-stu-id="3c967-277">Exploration: Compute Direct Distance Between Two Longitude-Latitude Locations</span></span>
> [!NOTE]
> <span data-ttu-id="3c967-278">To je obvykle **vědecký pracovník Data** úloh.</span><span class="sxs-lookup"><span data-stu-id="3c967-278">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="3c967-279">Míru přímé vzdálenost hello umožní nám toofind out hello nesoulad mezi ním hello skutečné dojít vzdálenost.</span><span class="sxs-lookup"><span data-stu-id="3c967-279">Having a measure of hello direct distance allows us toofind out hello discrepancy between it and hello actual trip distance.</span></span> <span data-ttu-id="3c967-280">Tato funkce jsme stimulují tak, že odkazuje osobní může být menší pravděpodobně tootip, pokud jejich rozmyslete si, že ovladač hello má záměrně provedenou je mnohem delší trasu.</span><span class="sxs-lookup"><span data-stu-id="3c967-280">We motivate this feature by pointing out that a passenger might be less likely tootip if they figure out that hello driver has intentionally taken them by a much longer route.</span></span>

<span data-ttu-id="3c967-281">porovnání hello toosee vzdálenost skutečné cesty a hello [Haversine vzdálenost](http://en.wikipedia.org/wiki/Haversine_formula) mezi dvěma body zeměpisné šířky (vzdálenost hello "dobře kruh"), používáme hello trigonometrické funkce dostupné v rámci Hive, tedy:</span><span class="sxs-lookup"><span data-stu-id="3c967-281">toosee hello comparison between actual trip distance and hello [Haversine distance](http://en.wikipedia.org/wiki/Haversine_formula) between two longitude-latitude points (hello "great circle" distance), we use hello trigonometric functions available within Hive, thus :</span></span>

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

<span data-ttu-id="3c967-282">V dotazu hello výše R je hello úhlu hello země v miles a platformy je převeden tooradians.</span><span class="sxs-lookup"><span data-stu-id="3c967-282">In hello query above, R is hello radius of hello Earth in miles, and pi is converted tooradians.</span></span> <span data-ttu-id="3c967-283">Upozorňujeme, že jsou body zeměpisné šířky hello "filtrované" tooremove hodnoty, které jsou daleko od oblasti NYC hello.</span><span class="sxs-lookup"><span data-stu-id="3c967-283">Note that hello longitude-latitude points are "filtered" tooremove values that are far from hello NYC area.</span></span>

<span data-ttu-id="3c967-284">V takovém případě zápisu jsme naše výsledky tooa adresář s názvem "queryoutputdir".</span><span class="sxs-lookup"><span data-stu-id="3c967-284">In this case, we write our results tooa directory called "queryoutputdir".</span></span> <span data-ttu-id="3c967-285">Hello sekvence příkazů ukazuje následující obrázek nejprve vytvoří tento výstup adresář a potom spustí příkaz Hive hello.</span><span class="sxs-lookup"><span data-stu-id="3c967-285">hello sequence of commands shown below first creates this output directory, and then runs hello Hive command.</span></span>

<span data-ttu-id="3c967-286">Z hello Hive directory řádku, spusťte:</span><span class="sxs-lookup"><span data-stu-id="3c967-286">From hello Hive directory prompt, run:</span></span>

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


<span data-ttu-id="3c967-287">Hello výsledky dotazu jsou zapsány objektů BLOB too9 Azure ***queryoutputdir/000000\_0*** příliš ***queryoutputdir/000008\_0*** v kontejneru výchozí hello clusteru Hadoop hello.</span><span class="sxs-lookup"><span data-stu-id="3c967-287">hello query results are written too9 Azure blobs ***queryoutputdir/000000\_0*** too ***queryoutputdir/000008\_0*** under hello default container of hello Hadoop cluster.</span></span>

<span data-ttu-id="3c967-288">velikost hello toosee hello jednotlivé objekty BLOB, spustíme hello hello Hive directory řádku následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="3c967-288">toosee hello size of hello individual blobs, we run hello following command from hello Hive directory prompt :</span></span>

    hdfs dfs -ls wasb:///queryoutputdir

<span data-ttu-id="3c967-289">toosee hello obsah daného souboru vyslovení 000000\_0, používáme na Hadoop `copyToLocal` příkaz proto.</span><span class="sxs-lookup"><span data-stu-id="3c967-289">toosee hello contents of a given file, say 000000\_0, we use Hadoop's `copyToLocal` command, thus.</span></span>

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [!WARNING]
> <span data-ttu-id="3c967-290">`copyToLocal`může být velmi pomalé pro velké soubory a nedoporučuje se používat pro použití s nimi.</span><span class="sxs-lookup"><span data-stu-id="3c967-290">`copyToLocal` can be very slow for large files, and is not recommended for use with them.</span></span>  
> 
> 

<span data-ttu-id="3c967-291">Hlavní výhodou, že máte tato data jsou umístěny v objektu blob Azure je, že jsme může zkoumat data hello v rámci Azure Machine Learning pomocí hello [importovat Data] [ import-data] modulu.</span><span class="sxs-lookup"><span data-stu-id="3c967-291">A key advantage of having this data reside in an Azure blob is that we may explore hello data within Azure Machine Learning using hello [Import Data][import-data] module.</span></span>

## <span data-ttu-id="3c967-292"><a name="#downsample"></a>Dolů modely ukázkových dat a sestavení v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="3c967-292"><a name="#downsample"></a>Down sample data and build models in Azure Machine Learning</span></span>
> [!NOTE]
> <span data-ttu-id="3c967-293">To je obvykle **vědecký pracovník Data** úloh.</span><span class="sxs-lookup"><span data-stu-id="3c967-293">This is typically a **Data Scientist** task.</span></span>
> 
> 

<span data-ttu-id="3c967-294">Po hello nahodilého dat analytické fáze jsou jsme teď připravena toodown ukázková hello data pro vytváření modelů v Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="3c967-294">After hello exploratory data analysis phase, we are now ready toodown sample hello data for building models in Azure Machine Learning.</span></span> <span data-ttu-id="3c967-295">V této části ukážeme, jak toouse podregistru dotaz toodown ukázka hello data, která je potom k němu přistupovat z hello [importovat Data] [ import-data] modulu v Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="3c967-295">In this section, we show how toouse a Hive query toodown sample hello data, which is then accessed from hello [Import Data][import-data] module in Azure Machine Learning.</span></span>

### <a name="down-sampling-hello-data"></a><span data-ttu-id="3c967-296">Dolů vzorkování dat hello</span><span class="sxs-lookup"><span data-stu-id="3c967-296">Down sampling hello data</span></span>
<span data-ttu-id="3c967-297">Existují dva kroky v tomto postupu.</span><span class="sxs-lookup"><span data-stu-id="3c967-297">There are two steps in this procedure.</span></span> <span data-ttu-id="3c967-298">Nejdřív jsme připojit hello **nyctaxidb.trip** a **nyctaxidb.fare** tabulky na tři klíče, které jsou k dispozici ve všech záznamech: "medailonu", "zabezpečení\_licence", a "vyzvednutí\_data a času".</span><span class="sxs-lookup"><span data-stu-id="3c967-298">First we join hello **nyctaxidb.trip** and **nyctaxidb.fare** tables on three keys that are present in all records : "medallion", "hack\_license", and "pickup\_datetime".</span></span> <span data-ttu-id="3c967-299">Pak se vygeneruje štítek binární klasifikace **vysypávány** a klasifikaci s více třída popisek **tip\_– třída**.</span><span class="sxs-lookup"><span data-stu-id="3c967-299">We then generate a binary classification label **tipped** and a multi-class classification label **tip\_class**.</span></span>

<span data-ttu-id="3c967-300">hello možné toouse toobe dolů jen Vzorkovaná data přímo z hello [importovat Data] [ import-data] modulu v Azure Machine Learning, je nutné toostore hello výsledky hello výše dotazu tooan interní tabulku Hive.</span><span class="sxs-lookup"><span data-stu-id="3c967-300">toobe able toouse hello down sampled data directly from hello [Import Data][import-data] module in Azure Machine Learning, it is necessary toostore hello results of hello above query tooan internal Hive table.</span></span> <span data-ttu-id="3c967-301">V co následuje můžeme vytvořit vnitřní tabulku Hive a naplnit její obsah se hello připojený a dolů jen Vzorkovaná data.</span><span class="sxs-lookup"><span data-stu-id="3c967-301">In what follows, we create an internal Hive table and populate its contents with hello joined and down sampled data.</span></span>

<span data-ttu-id="3c967-302">Hello dotazu použije standardní funkce Hive přímo hodinu hello toogenerate den, týden roku, den v týdnu (1 zastupuje pondělí a 7 zastupuje neděle) z hello "vyzvednutí\_data a času" pole a hello přímé vzdálenost mezi hello vyzvednutí a dropoff umístění.</span><span class="sxs-lookup"><span data-stu-id="3c967-302">hello query applies standard Hive functions directly toogenerate hello hour of day, week of year, weekday (1 stands for Monday, and 7 stands for Sunday) from hello "pickup\_datetime" field,  and hello direct distance between hello pickup and dropoff locations.</span></span> <span data-ttu-id="3c967-303">Uživatelé mohou odkazovat příliš[LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) pro úplný seznam takové funkce.</span><span class="sxs-lookup"><span data-stu-id="3c967-303">Users can refer too[LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) for a complete list of such functions.</span></span>

<span data-ttu-id="3c967-304">Hello dotazu pak dolů ukázky hello dat tak, aby výsledky dotazu hello můžete začlenit do hello Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="3c967-304">hello query then down samples hello data so that hello query results can fit into hello Azure Machine Learning Studio.</span></span> <span data-ttu-id="3c967-305">Pouze asi 1 % hello původní datové sady je importovat do hello Studio.</span><span class="sxs-lookup"><span data-stu-id="3c967-305">Only about 1% of hello original dataset is imported into hello Studio.</span></span>

<span data-ttu-id="3c967-306">Níže jsou hello obsah *ukázka\_hive\_Příprava\_pro\_aml\_full.hql* soubor, který připraví dat pro model vytváření v Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="3c967-306">Below are hello contents of *sample\_hive\_prepare\_for\_aml\_full.hql* file that prepares data for model building in Azure Machine Learning.</span></span>

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

        --- now insert contents of hello join into hello above internal table

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

<span data-ttu-id="3c967-307">toorun vyzvat tento dotaz z adresáře Hive hello:</span><span class="sxs-lookup"><span data-stu-id="3c967-307">toorun this query, from hello Hive directory prompt :</span></span>

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

<span data-ttu-id="3c967-308">Nyní je interní tabulku "nyctaxidb.nyctaxi_downsampled_dataset", který je přístupný pomocí hello [importovat Data] [ import-data] modulu z Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="3c967-308">We now have an internal table "nyctaxidb.nyctaxi_downsampled_dataset" which can be accessed using hello [Import Data][import-data] module from Azure Machine Learning.</span></span> <span data-ttu-id="3c967-309">Kromě toho můžeme použít tuto datovou sadu pro vytváření modelů Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="3c967-309">Furthermore, we may use this dataset for building Machine Learning models.</span></span>  

### <a name="use-hello-import-data-module-in-azure-machine-learning-tooaccess-hello-down-sampled-data"></a><span data-ttu-id="3c967-310">Používejte v Azure Machine Learning tooaccess hello dolů jen Vzorkovaná data modul importovat Data hello</span><span class="sxs-lookup"><span data-stu-id="3c967-310">Use hello Import Data module in Azure Machine Learning tooaccess hello down sampled data</span></span>
<span data-ttu-id="3c967-311">Jako předpoklady pro vydávání dotazů Hive v hello [importovat Data] [ import-data] modul Azure Machine Learning, budeme potřebovat přístup k tooan Azure Machine Learning prostoru a k toohello pověření hello cluster a jeho přidružený účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="3c967-311">As prerequisites for issuing Hive queries in hello [Import Data][import-data] module of Azure Machine Learning, we need access tooan Azure Machine Learning workspace and access toohello credentials of hello cluster and its associated storage account.</span></span>

<span data-ttu-id="3c967-312">Některé podrobnosti o hello [importovat Data] [ import-data] modul a hello tooinput parametry:</span><span class="sxs-lookup"><span data-stu-id="3c967-312">Some details on hello [Import Data][import-data] module and hello parameters tooinput :</span></span>

<span data-ttu-id="3c967-313">**Identifikátor URI serveru HCatalog**: Pokud je název clusteru hello abc123, pak toto je jednoduše: https://abc123.azurehdinsight.net</span><span class="sxs-lookup"><span data-stu-id="3c967-313">**HCatalog server URI**: If hello cluster name is abc123, then this is simply : https://abc123.azurehdinsight.net</span></span>

<span data-ttu-id="3c967-314">**Název uživatelského účtu Hadoop** : zvolené pro hello cluster hello uživatelské jméno (**není** hello RAS uživatelské jméno)</span><span class="sxs-lookup"><span data-stu-id="3c967-314">**Hadoop user account name** : hello user name chosen for hello cluster (**not** hello remote access user name)</span></span>

<span data-ttu-id="3c967-315">**Heslo účtu pož Hadoop** : heslo hello zvolené pro hello cluster (**není** heslo hello vzdáleného přístupu)</span><span class="sxs-lookup"><span data-stu-id="3c967-315">**Hadoop ser account password** : hello password chosen for hello cluster (**not** hello remote access password)</span></span>

<span data-ttu-id="3c967-316">**Umístění výstupu dat** : to je zvolen toobe Azure.</span><span class="sxs-lookup"><span data-stu-id="3c967-316">**Location of output data** : This is chosen toobe Azure.</span></span>

<span data-ttu-id="3c967-317">**Název účtu úložiště Azure** : název účtu úložiště výchozí hello přidruženého k hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="3c967-317">**Azure storage account name** : Name of hello default storage account associated with hello cluster.</span></span>

<span data-ttu-id="3c967-318">**Název kontejneru Azure** : Toto je název kontejneru výchozí hello hello clusteru a je obvykle hello stejný jako název clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="3c967-318">**Azure container name** : This is hello default container name for hello cluster, and is typically hello same as hello cluster name.</span></span> <span data-ttu-id="3c967-319">Pro cluster s názvem "abc123" jde jenom abc123.</span><span class="sxs-lookup"><span data-stu-id="3c967-319">For a cluster called "abc123", this is just abc123.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3c967-320">**Všechny tabulky jsme chcete tooquery pomocí hello [importovat Data] [ import-data] modulu v Azure Machine Learning musí být interní tabulku.**</span><span class="sxs-lookup"><span data-stu-id="3c967-320">**Any table we wish tooquery using hello [Import Data][import-data] module in Azure Machine Learning must be an internal table.**</span></span> <span data-ttu-id="3c967-321">Tip pro určení toho, jestli tabulka T v databázi D.db interní tabulku je následující.</span><span class="sxs-lookup"><span data-stu-id="3c967-321">A tip for determining if a table T in a database D.db is an internal table is as follows.</span></span>
> 
> 

<span data-ttu-id="3c967-322">Z hello podregistru directory řádku problém hello příkaz:</span><span class="sxs-lookup"><span data-stu-id="3c967-322">From hello Hive directory prompt, issue hello command :</span></span>

    hdfs dfs -ls wasb:///D.db/T

<span data-ttu-id="3c967-323">Pokud tabulka hello je interní tabulku a je naplněna, musí zde zobrazí její obsah.</span><span class="sxs-lookup"><span data-stu-id="3c967-323">If hello table is an internal table and it is populated, its contents must show here.</span></span> <span data-ttu-id="3c967-324">Jiný způsob toodetermine, zda tabulka je interní tabulku je toouse hello Azure Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="3c967-324">Another way toodetermine whether a table is an internal table is toouse hello Azure Storage Explorer.</span></span> <span data-ttu-id="3c967-325">Pomocí něho toonavigate toohello výchozí název kontejneru hello clusteru a potom filtrovat podle názvu tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="3c967-325">Use it toonavigate toohello default container name of hello cluster, and then filter by hello table name.</span></span> <span data-ttu-id="3c967-326">Pokud hello tabulka a její obsah objeví, potvrdíte, že je interní tabulku.</span><span class="sxs-lookup"><span data-stu-id="3c967-326">If hello table and its contents show up, this confirms that it is an internal table.</span></span>

<span data-ttu-id="3c967-327">Zde je snímek hello dotaz Hive a hello [importovat Data] [ import-data] modul:</span><span class="sxs-lookup"><span data-stu-id="3c967-327">Here is a snapshot of hello Hive query and hello [Import Data][import-data] module:</span></span>

![Dotaz Hive pro Import dat modul](./media/machine-learning-data-science-process-hive-walkthrough/1eTYf52.png)

<span data-ttu-id="3c967-329">Poznámka: od našich seznamu jen Vzorkovaná data nachází v kontejneru výchozí hello hello výsledný dotaz Hive z Azure Machine Learning je velmi jednoduchý a je jen "vybrat * z nyctaxidb.nyctaxi\_po převzorkování dolů\_data".</span><span class="sxs-lookup"><span data-stu-id="3c967-329">Note that since our down sampled data resides in hello default container, hello resulting Hive query from Azure Machine Learning is very simple and is just a "SELECT * FROM nyctaxidb.nyctaxi\_downsampled\_data".</span></span>

<span data-ttu-id="3c967-330">datovou sadu Hello nyní slouží jako výchozí bod pro vytváření modelů Machine Learning hello.</span><span class="sxs-lookup"><span data-stu-id="3c967-330">hello dataset may now be used as hello starting point for building Machine Learning models.</span></span>

### <span data-ttu-id="3c967-331"><a name="mlmodel"></a>Vytvářet modely v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="3c967-331"><a name="mlmodel"></a>Build models in Azure Machine Learning</span></span>
<span data-ttu-id="3c967-332">Snažíme se teď může tooproceed toomodel sestavení a nasazení modelu v [Azure Machine Learning](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="3c967-332">We are now able tooproceed toomodel building and model deployment in [Azure Machine Learning](https://studio.azureml.net).</span></span> <span data-ttu-id="3c967-333">Hello data jsou připravena pro nás toouse v adresování identifikované výše hello předpovědi problémy:</span><span class="sxs-lookup"><span data-stu-id="3c967-333">hello data is ready for us toouse in addressing hello prediction problems identified above:</span></span>

<span data-ttu-id="3c967-334">**1. Binární klasifikace**: toopredict, jestli byl tip placené cesty.</span><span class="sxs-lookup"><span data-stu-id="3c967-334">**1. Binary classification**: toopredict whether or not a tip was paid for a trip.</span></span>

<span data-ttu-id="3c967-335">**Použít student:** logistic regression Two-class</span><span class="sxs-lookup"><span data-stu-id="3c967-335">**Learner used:** Two-class logistic regression</span></span>

<span data-ttu-id="3c967-336">a.</span><span class="sxs-lookup"><span data-stu-id="3c967-336">a.</span></span> <span data-ttu-id="3c967-337">U tohoto problému našeho štítku cíl (nebo třída) je "vysypávány".</span><span class="sxs-lookup"><span data-stu-id="3c967-337">For this problem, our target (or class) label is "tipped".</span></span> <span data-ttu-id="3c967-338">Naše původní datové sady vzorků nižší má několik sloupců, které jsou nevracení cíl tohoto experimentu klasifikace.</span><span class="sxs-lookup"><span data-stu-id="3c967-338">Our original down-sampled dataset has a few columns that are target leaks for this classification experiment.</span></span> <span data-ttu-id="3c967-339">Konkrétně: tip\_třídy, tip\_velikostí a celkový počet\_částka zobrazení informace o hello cíl štítek, který není k dispozici na testování čas.</span><span class="sxs-lookup"><span data-stu-id="3c967-339">In particular : tip\_class, tip\_amount, and total\_amount reveal information about hello target label that is not available at testing time.</span></span> <span data-ttu-id="3c967-340">Jsme odebrat tyto sloupce brány v úvahu pomocí hello [výběr sloupců v datové sadě] [ select-columns] modulu.</span><span class="sxs-lookup"><span data-stu-id="3c967-340">We remove these columns from consideration using hello [Select Columns in Dataset][select-columns] module.</span></span>

<span data-ttu-id="3c967-341">Hello snímku níže ukazuje naše toopredict experiment, jestli byl placené tip pro danou cestu.</span><span class="sxs-lookup"><span data-stu-id="3c967-341">hello snapshot below shows our experiment toopredict whether or not a tip was paid for a given trip.</span></span>

![Experiment snímku](./media/machine-learning-data-science-process-hive-walkthrough/QGxRz5A.png)

<span data-ttu-id="3c967-343">b.</span><span class="sxs-lookup"><span data-stu-id="3c967-343">b.</span></span> <span data-ttu-id="3c967-344">Pro tento experiment naše distribuce popisek cíl byly přibližně 1:1.</span><span class="sxs-lookup"><span data-stu-id="3c967-344">For this experiment, our target label distributions were roughly 1:1.</span></span>

<span data-ttu-id="3c967-345">Hello snímku níže ukazuje hello distribuce popisky třída tip pro problém binární klasifikace hello.</span><span class="sxs-lookup"><span data-stu-id="3c967-345">hello snapshot below shows hello distribution of tip class labels for hello binary classification problem.</span></span>

![Distribuce popisků tip – třída](./media/machine-learning-data-science-process-hive-walkthrough/9mM4jlD.png)

<span data-ttu-id="3c967-347">V důsledku toho jsme získat AUC 0.987, jak je znázorněno na následujícím obrázku hello.</span><span class="sxs-lookup"><span data-stu-id="3c967-347">As a result, we obtain an AUC of 0.987 as shown in hello figure below.</span></span>

![Hodnota AUC](./media/machine-learning-data-science-process-hive-walkthrough/8JDT0F8.png)

<span data-ttu-id="3c967-349">**2. Více třídami klasifikace**: rozsah hello toopredict tip objemu zaplacení hello na cestu, pomocí hello dříve definované třídy.</span><span class="sxs-lookup"><span data-stu-id="3c967-349">**2. Multiclass classification**: toopredict hello range of tip amounts paid for hello trip, using hello previously defined classes.</span></span>

<span data-ttu-id="3c967-350">**Použít student:** více třídami logistic regression</span><span class="sxs-lookup"><span data-stu-id="3c967-350">**Learner used:** Multiclass logistic regression</span></span>

<span data-ttu-id="3c967-351">a.</span><span class="sxs-lookup"><span data-stu-id="3c967-351">a.</span></span> <span data-ttu-id="3c967-352">Pro tento problém popisek naše cíle (nebo třída) je "tip\_třída" které můžete provést jednu z pěti hodnot (0,1,2,3,4).</span><span class="sxs-lookup"><span data-stu-id="3c967-352">For this problem, our target (or class) label is "tip\_class" which can take one of five values (0,1,2,3,4).</span></span> <span data-ttu-id="3c967-353">Jako v případě binární klasifikace hello máme několik sloupců, které jsou nevracení cíl tohoto experimentu.</span><span class="sxs-lookup"><span data-stu-id="3c967-353">As in hello binary classification case, we have a few columns that are target leaks for this experiment.</span></span> <span data-ttu-id="3c967-354">Konkrétně: vysypávány, tip\_velikost, celkový počet\_částka zobrazení informace o hello cíl štítek, který není k dispozici na testování čas.</span><span class="sxs-lookup"><span data-stu-id="3c967-354">In particular : tipped, tip\_amount, total\_amount reveal information about hello target label that is not available at testing time.</span></span> <span data-ttu-id="3c967-355">Jsme odebrat tyto sloupce pomocí hello [výběr sloupců v datové sadě] [ select-columns] modulu.</span><span class="sxs-lookup"><span data-stu-id="3c967-355">We remove these columns using hello [Select Columns in Dataset][select-columns] module.</span></span>

<span data-ttu-id="3c967-356">snímek Hello níže znázorňuje naše toopredict experiment, ve které bin tip je pravděpodobně toofall (třída 0: tip = $0, 1 – třída: tip > $0 a tip < = 5, 2 – třída: tip > $5 a tip < = 10, třídy 3: tip > $10 a tip < = 20 Třída 4: tip > 20)</span><span class="sxs-lookup"><span data-stu-id="3c967-356">hello snapshot below shows our experiment toopredict in which bin a tip is likely toofall ( Class 0: tip = $0, class 1 : tip > $0 and tip <= $5, Class 2 : tip > $5 and tip <= $10, Class 3 : tip > $10 and tip <= $20, Class 4 : tip > $20)</span></span>

![Experiment snímku](./media/machine-learning-data-science-process-hive-walkthrough/5ztv0n0.png)

<span data-ttu-id="3c967-358">Nyní ukážeme, jak vypadá naše skutečné testovací třída distribuční.</span><span class="sxs-lookup"><span data-stu-id="3c967-358">We now show what our actual test class distribution looks like.</span></span> <span data-ttu-id="3c967-359">Vidíte, že třída 0 a 1 třídy jsou běžně se vyskytujícím, hello ostatní třídy vyskytují jen vzácně.</span><span class="sxs-lookup"><span data-stu-id="3c967-359">We see that while Class 0 and Class 1 are prevalent, hello other classes are rare.</span></span>

![Testování distribuční – třída](./media/machine-learning-data-science-process-hive-walkthrough/Vy1FUKa.png)

<span data-ttu-id="3c967-361">b.</span><span class="sxs-lookup"><span data-stu-id="3c967-361">b.</span></span> <span data-ttu-id="3c967-362">Pro tento experiment používáme záměně matice toolook v našem přesnostmi předpovědi.</span><span class="sxs-lookup"><span data-stu-id="3c967-362">For this experiment, we use a confusion matrix toolook at our prediction accuracies.</span></span> <span data-ttu-id="3c967-363">Příklad najdete níž.</span><span class="sxs-lookup"><span data-stu-id="3c967-363">This is shown below.</span></span>

![Matice nedorozuměním](./media/machine-learning-data-science-process-hive-walkthrough/cxFmErM.png)

<span data-ttu-id="3c967-365">Všimněte si, že naše třída přesnostmi na třídách běžně se vyskytujícím hello je poměrně vhodný, hello modelu neprovádí dobrou práci "vzdělávání" na hello vzácnějších třídy.</span><span class="sxs-lookup"><span data-stu-id="3c967-365">Note that while our class accuracies on hello prevalent classes is quite good, hello model does not do a good job of "learning" on hello rarer classes.</span></span>

<span data-ttu-id="3c967-366">**3. Úloha regrese**: toopredict hello množství tip placené cesty.</span><span class="sxs-lookup"><span data-stu-id="3c967-366">**3. Regression task**: toopredict hello amount of tip paid for a trip.</span></span>

<span data-ttu-id="3c967-367">**Použít student:** Boosted rozhodovací strom</span><span class="sxs-lookup"><span data-stu-id="3c967-367">**Learner used:** Boosted decision tree</span></span>

<span data-ttu-id="3c967-368">a.</span><span class="sxs-lookup"><span data-stu-id="3c967-368">a.</span></span> <span data-ttu-id="3c967-369">Pro tento problém popisek naše cíle (nebo třída) je "tip\_velikost".</span><span class="sxs-lookup"><span data-stu-id="3c967-369">For this problem, our target (or class) label is "tip\_amount".</span></span> <span data-ttu-id="3c967-370">Naše nevracení cíl v tomto případě jsou: vysypávány, tip\_třída, celkový počet\_velikost; tyto proměnné odhalit informace o hello tip množství, které je obvykle dostupná na testování čas.</span><span class="sxs-lookup"><span data-stu-id="3c967-370">Our target leaks in this case are : tipped, tip\_class, total\_amount ; all these variables reveal information about hello tip amount that is typically unavailable at testing time.</span></span> <span data-ttu-id="3c967-371">Jsme odebrat tyto sloupce pomocí hello [výběr sloupců v datové sadě] [ select-columns] modulu.</span><span class="sxs-lookup"><span data-stu-id="3c967-371">We remove these columns using hello [Select Columns in Dataset][select-columns] module.</span></span>

<span data-ttu-id="3c967-372">Hello snímku belows ukazuje naše experimentu toopredict hello množství hello zadaný tip pro.</span><span class="sxs-lookup"><span data-stu-id="3c967-372">hello snapshot belows shows our experiment toopredict hello amount of hello given tip.</span></span>

![Experiment snímku](./media/machine-learning-data-science-process-hive-walkthrough/11TZWgV.png)

<span data-ttu-id="3c967-374">b.</span><span class="sxs-lookup"><span data-stu-id="3c967-374">b.</span></span> <span data-ttu-id="3c967-375">V případě problémů regrese měřit hello přesnostmi naše předpovědí prohlížením hello spolehlivosti došlo k chybě v hello předpovědi, hello koeficient spolehlivosti a hello jako.</span><span class="sxs-lookup"><span data-stu-id="3c967-375">For regression problems, we measure hello accuracies of our prediction by looking at hello squared error in hello predictions, hello coefficient of determination, and hello like.</span></span> <span data-ttu-id="3c967-376">Ukážeme tyto níže.</span><span class="sxs-lookup"><span data-stu-id="3c967-376">We show these below.</span></span>

![Předpověď statistiky](./media/machine-learning-data-science-process-hive-walkthrough/Jat9mrz.png)

<span data-ttu-id="3c967-378">Vidíme, že o hello je koeficient spolehlivosti 0.709, zdání přibližně 71 % odchylky hello se vysvětluje naše koeficienty modelu.</span><span class="sxs-lookup"><span data-stu-id="3c967-378">We see that about hello coefficient of determination is 0.709, implying about 71% of hello variance is explained by our model coefficients.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3c967-379">Další informace o Azure Machine Learning toolearn a jak tooaccess a používat ji, vyhledejte příliš[co je Machine Learning?](machine-learning-what-is-machine-learning.md).</span><span class="sxs-lookup"><span data-stu-id="3c967-379">toolearn more about Azure Machine Learning and how tooaccess and use it, please refer too[What's Machine Learning?](machine-learning-what-is-machine-learning.md).</span></span> <span data-ttu-id="3c967-380">Je velmi užitečná prostředku pro přehrávání s spoustu experimenty Machine Learning v Azure Machine Learning hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/).</span><span class="sxs-lookup"><span data-stu-id="3c967-380">A very useful resource for playing with a bunch of Machine Learning experiments on Azure Machine Learning is hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/).</span></span> <span data-ttu-id="3c967-381">Galerie Hello vysvětluje škálu experimenty a poskytuje důkladné Úvod do hello řadu funkcí služby Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="3c967-381">hello Gallery covers a gamut of experiments and provides a thorough introduction into hello range of capabilities of Azure Machine Learning.</span></span>
> 
> 

## <a name="license-information"></a><span data-ttu-id="3c967-382">Informace o licenci</span><span class="sxs-lookup"><span data-stu-id="3c967-382">License Information</span></span>
<span data-ttu-id="3c967-383">Tento ukázkový postup a jeho doprovodné skriptů jsou sdíleny společností Microsoft v rámci licencí MIT hello.</span><span class="sxs-lookup"><span data-stu-id="3c967-383">This sample walkthrough and its accompanying scripts are shared by Microsoft under hello MIT license.</span></span> <span data-ttu-id="3c967-384">Zkontrolujte prosím soubor LICENSE.txt hello v adresáři hello hello ukázkový kód na Githubu další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="3c967-384">Please check hello LICENSE.txt file in in hello directory of hello sample code on GitHub for more details.</span></span>

## <a name="references"></a><span data-ttu-id="3c967-385">Odkazy</span><span class="sxs-lookup"><span data-stu-id="3c967-385">References</span></span>
<span data-ttu-id="3c967-386">• [Stránce pro stažení Andrés Monroy NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/)</span><span class="sxs-lookup"><span data-stu-id="3c967-386">•    [Andrés Monroy NYC Taxi Trips Download Page](http://www.andresmh.com/nyctaxitrips/)</span></span>  
<span data-ttu-id="3c967-387">• [FOILing NYC Taxi cestě dat podle Whong Jan](http://chriswhong.com/open-data/foil_nyc_taxi/) </span><span class="sxs-lookup"><span data-stu-id="3c967-387">•    [FOILing NYC’s Taxi Trip Data by Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/) </span></span>  
<span data-ttu-id="3c967-388">• [NYC taxíkem a Limousine Komise výzkum a statistiky](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span><span class="sxs-lookup"><span data-stu-id="3c967-388">•    [NYC Taxi and Limousine Commission Research and Statistics](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span></span>

[2]: ./media/machine-learning-data-science-process-hive-walkthrough/output-hive-results-3.png
[11]: ./media/machine-learning-data-science-process-hive-walkthrough/hive-reader-properties.png
[12]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-training.png
[13]: ./media/machine-learning-data-science-process-hive-walkthrough/create-scoring-experiment.png
[14]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-scoring.png
[15]: ./media/machine-learning-data-science-process-hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
