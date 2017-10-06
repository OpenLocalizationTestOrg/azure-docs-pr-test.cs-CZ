---
title: "aaaBuild a nasazení modelu strojového učení pomocí systému SQL Server na virtuálním počítači Azure | Microsoft Docs"
description: "Proces pokročilou analýzu a technologie v akci"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 6066b083-262c-4453-a712-a5c05acc3df8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: fashah;bradsev
ms.openlocfilehash: 30ba9a9e3cf65f75015e13f9c7876dcbccc5bc47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="hello-team-data-science-process-in-action-using-sql-server"></a><span data-ttu-id="9ec63-103">Hello proces vědecké účely Team dat v akci: pomocí SQL serveru</span><span class="sxs-lookup"><span data-stu-id="9ec63-103">hello Team Data Science Process in action: using SQL Server</span></span>
<span data-ttu-id="9ec63-104">V tomto kurzu jste provede hello proces vytváření a nasazování model strojového učení pomocí systému SQL Server a veřejně dostupné datové sady – hello [NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/) datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="9ec63-104">In this tutorial, you walk through hello process of building and deploying a machine learning model using SQL Server and a publicly available dataset -- hello [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset.</span></span> <span data-ttu-id="9ec63-105">Postup Hello dodržovat standardní data vědecké účely pracovního postupu: ingestování a zkoumat hello data, analyzovat funkce toofacilitate učení se pak sestavení a nasazení modelu.</span><span class="sxs-lookup"><span data-stu-id="9ec63-105">hello procedure follows a standard data science workflow: ingest and explore hello data, engineer features toofacilitate learning, then build and deploy a model.</span></span>

## <span data-ttu-id="9ec63-106"><a name="dataset"></a>NYC taxíkem cest v popis datové sady</span><span class="sxs-lookup"><span data-stu-id="9ec63-106"><a name="dataset"></a>NYC Taxi Trips Dataset Description</span></span>
<span data-ttu-id="9ec63-107">Hello NYC taxíkem cestě dat je přibližně 20GB komprimovaných souborů CSV (nekomprimovaným ~ 48GB), která obsahuje více než 173 milionů jednotlivých cest a hello tarify placené pro každou cestu.</span><span class="sxs-lookup"><span data-stu-id="9ec63-107">hello NYC Taxi Trip data is about 20GB of compressed CSV files (~48GB uncompressed), comprising more than 173 million individual trips and hello fares paid for each trip.</span></span> <span data-ttu-id="9ec63-108">Každý záznam cestě zahrnuje hello vyzvednutí a odkládací umístění a čas, číslo licence anonymizovaná hackerský (ovladač) a číslo Medailon (taxi na jedinečné id).</span><span class="sxs-lookup"><span data-stu-id="9ec63-108">Each trip record includes hello pickup and drop-off location and time, anonymized hack (driver's) license number and medallion (taxi’s unique id) number.</span></span> <span data-ttu-id="9ec63-109">Hello dat obsahuje všechny služebních cest v hello roku 2013 a je součástí hello následující dvě datové sady pro každý měsíc:</span><span class="sxs-lookup"><span data-stu-id="9ec63-109">hello data covers all trips in hello year 2013 and is provided in hello following two datasets for each month:</span></span>

1. <span data-ttu-id="9ec63-110">Hello 'trip_data' CSV obsahuje podrobnosti o cestě, například na počtu cestujících, vyzvednutí a dropoff body, doba trvání cesty a délka cesty.</span><span class="sxs-lookup"><span data-stu-id="9ec63-110">hello 'trip_data' CSV contains trip details, such as number of passengers, pickup and dropoff points, trip duration, and trip length.</span></span> <span data-ttu-id="9ec63-111">Tady je několik ukázkových záznamů:</span><span class="sxs-lookup"><span data-stu-id="9ec63-111">Here are a few sample records:</span></span>
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. <span data-ttu-id="9ec63-112">Hello CSV obsahuje podrobnosti o tarif hello placené pro každou cestu, například typ platby, velikost tarif, příplatek a daně, tipy a mýtné a celkovou velikost hello placené trip_fare.</span><span class="sxs-lookup"><span data-stu-id="9ec63-112">hello 'trip_fare' CSV contains details of hello fare paid for each trip, such as payment type, fare amount, surcharge and taxes, tips and tolls, and hello total amount paid.</span></span> <span data-ttu-id="9ec63-113">Tady je několik ukázkových záznamů:</span><span class="sxs-lookup"><span data-stu-id="9ec63-113">Here are a few sample records:</span></span>
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

<span data-ttu-id="9ec63-114">Hello jedinečné klíče toojoin cestě\_dat a cesty\_tarif se skládá z pole hello: medailonu, hackerský\_licence a vyzvednutí\_data a času.</span><span class="sxs-lookup"><span data-stu-id="9ec63-114">hello unique key toojoin trip\_data and trip\_fare is composed of hello fields: medallion, hack\_licence and pickup\_datetime.</span></span>

## <span data-ttu-id="9ec63-115"><a name="mltasks"></a>Příklady předpovědi úlohy</span><span class="sxs-lookup"><span data-stu-id="9ec63-115"><a name="mltasks"></a>Examples of Prediction Tasks</span></span>
<span data-ttu-id="9ec63-116">Jsme se formulovali tři předpovědi problémy podle hello *tip\_velikost*, konkrétně:</span><span class="sxs-lookup"><span data-stu-id="9ec63-116">We will formulate three prediction problems based on hello *tip\_amount*, namely:</span></span>

1. <span data-ttu-id="9ec63-117">Binární klasifikace: předpovědět, zda byl tip placené cesty, tj. *tip\_velikost* větší než $0 je pozitivní příklad, při *tip\_velikost* $ 0 je Příklad záporné.</span><span class="sxs-lookup"><span data-stu-id="9ec63-117">Binary classification: Predict whether or not a tip was paid for a trip, i.e. a *tip\_amount* that is greater than $0 is a positive example, while a *tip\_amount* of $0 is a negative example.</span></span>
2. <span data-ttu-id="9ec63-118">Více třídami klasifikace: rozsah hello toopredict tipu zaplacení hello cesty.</span><span class="sxs-lookup"><span data-stu-id="9ec63-118">Multiclass classification: toopredict hello range of tip paid for hello trip.</span></span> <span data-ttu-id="9ec63-119">Jsme rozdělit hello *tip\_velikost* do pěti přihrádek nebo třídy:</span><span class="sxs-lookup"><span data-stu-id="9ec63-119">We divide hello *tip\_amount* into five bins or classes:</span></span>
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. <span data-ttu-id="9ec63-120">Úloha regrese: toopredict hello množství tip placené cesty.</span><span class="sxs-lookup"><span data-stu-id="9ec63-120">Regression task: toopredict hello amount of tip paid for a trip.</span></span>  

## <span data-ttu-id="9ec63-121"><a name="setup"></a>Nastavení se hello dat Azure vědecké účely prostředí pro pokročilou analýzu</span><span class="sxs-lookup"><span data-stu-id="9ec63-121"><a name="setup"></a>Setting Up hello Azure data science environment for advanced analytics</span></span>
<span data-ttu-id="9ec63-122">Jak vidíte z hello [plánování vašeho prostředí](machine-learning-data-science-plan-your-environment.md) průvodce, existuje několik možností toowork s datovou sadu cest taxíkem NYC hello v Azure:</span><span class="sxs-lookup"><span data-stu-id="9ec63-122">As you can see from hello [Plan Your Environment](machine-learning-data-science-plan-your-environment.md) guide, there are several options toowork with hello NYC Taxi Trips dataset in Azure:</span></span>

* <span data-ttu-id="9ec63-123">Práce s hello data do objektů BLOB služby Azure a modelu v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="9ec63-123">Work with hello data in Azure blobs then model in Azure Machine Learning</span></span>
* <span data-ttu-id="9ec63-124">Načtení dat hello do databáze systému SQL Server a modelu v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="9ec63-124">Load hello data into a SQL Server database then model in Azure Machine Learning</span></span>

<span data-ttu-id="9ec63-125">V tomto kurzu si předvedeme paralelní hromadným importem tooa hello dat systému SQL Server, zkoumání dat, funkce technici a dolů vzorkování pomocí SQL Server Management Studio a také pomocí IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="9ec63-125">In this tutorial we will demonstrate parallel bulk import of hello data tooa SQL Server, data exploration, feature engineering and down sampling using SQL Server Management Studio as well as using IPython Notebook.</span></span> <span data-ttu-id="9ec63-126">[Ukázkové skripty](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) a [poznámkových bloků IPython](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) jsou sdíleny v Githubu.</span><span class="sxs-lookup"><span data-stu-id="9ec63-126">[Sample scripts](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) and [IPython notebooks](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) are shared in GitHub.</span></span> <span data-ttu-id="9ec63-127">Ukázkové IPython poznámkového bloku toowork s daty hello v Azure blob je také dostupná v hello stejné umístění.</span><span class="sxs-lookup"><span data-stu-id="9ec63-127">A sample IPython notebook toowork with hello data in Azure blobs is also available in hello same location.</span></span>

<span data-ttu-id="9ec63-128">tooset prostředí vědecké zpracování dat Azure:</span><span class="sxs-lookup"><span data-stu-id="9ec63-128">tooset up your Azure Data Science environment:</span></span>

1. [<span data-ttu-id="9ec63-129">Vytvoření účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="9ec63-129">Create a storage account</span></span>](../storage/common/storage-create-storage-account.md)
2. [<span data-ttu-id="9ec63-130">Vytvořit pracovní prostor služby Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="9ec63-130">Create an Azure Machine Learning workspace</span></span>](machine-learning-create-workspace.md)
3. <span data-ttu-id="9ec63-131">[Zřízení virtuálního počítače s datové vědy](machine-learning-data-science-setup-sql-server-virtual-machine.md), který poskytuje systému SQL Server a server služby IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="9ec63-131">[Provision a Data Science Virtual Machine](machine-learning-data-science-setup-sql-server-virtual-machine.md), which provides a SQL Server and an IPython Notebook server.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="9ec63-132">Hello ukázkové skripty a IPython poznámkových bloků se stažené tooyour vědecké zpracování dat virtuálního počítače během procesu instalace hello.</span><span class="sxs-lookup"><span data-stu-id="9ec63-132">hello sample scripts and IPython notebooks will be downloaded tooyour Data Science virtual machine during hello setup process.</span></span> <span data-ttu-id="9ec63-133">Po dokončení hello virtuálního počítače po instalaci skriptu hello ukázky bude v knihovně dokumentů Virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="9ec63-133">When hello VM post-installation script completes, hello samples will be in your VM's Documents library:</span></span>  
   > 
   > * <span data-ttu-id="9ec63-134">Ukázku skripty:`C:\Users\<user_name>\Documents\Data Science Scripts`</span><span class="sxs-lookup"><span data-stu-id="9ec63-134">Sample Scripts: `C:\Users\<user_name>\Documents\Data Science Scripts`</span></span>  
   > * <span data-ttu-id="9ec63-135">Ukázka IPython poznámkových bloků:`C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`</span><span class="sxs-lookup"><span data-stu-id="9ec63-135">Sample IPython Notebooks: `C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`</span></span>  
   >   <span data-ttu-id="9ec63-136">kde `<user_name>` je váš virtuální počítač Windows přihlašovací jméno.</span><span class="sxs-lookup"><span data-stu-id="9ec63-136">where `<user_name>` is your VM's Windows login name.</span></span> <span data-ttu-id="9ec63-137">Označujeme toohello ukázkové složky jako **ukázkové skripty** a **poznámkových bloků IPython ukázka**.</span><span class="sxs-lookup"><span data-stu-id="9ec63-137">We will refer toohello sample folders as **Sample Scripts** and **Sample IPython Notebooks**.</span></span>
   > 
   > 

<span data-ttu-id="9ec63-138">Na základě hello velikost datové sady, umístění zdroje dat a hello vybrané Azure cílové prostředí, tento scénář je podobné příliš[scénář \#5: velké datové sady v místních souborů cíle systému SQL Server ve virtuálním počítači Azure](machine-learning-data-science-plan-sample-scenarios.md#largelocaltodb).</span><span class="sxs-lookup"><span data-stu-id="9ec63-138">Based on hello dataset size, data source location, and hello selected Azure target environment, this scenario is similar too[Scenario \#5: Large dataset in a local files, target SQL Server in Azure VM](machine-learning-data-science-plan-sample-scenarios.md#largelocaltodb).</span></span>

## <span data-ttu-id="9ec63-139"><a name="getdata"></a>Získat hello Data z veřejné zdroje</span><span class="sxs-lookup"><span data-stu-id="9ec63-139"><a name="getdata"></a>Get hello Data from Public Source</span></span>
<span data-ttu-id="9ec63-140">tooget hello [NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/) datové sady z veřejného umístění, můžete použít některou z metod hello popsané v [tooand přesun dat z Azure Blob Storage](machine-learning-data-science-move-azure-blob.md) toocopy hello data tooyour nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="9ec63-140">tooget hello [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset from its public location, you may use any of hello methods described in [Move Data tooand from Azure Blob Storage](machine-learning-data-science-move-azure-blob.md) toocopy hello data tooyour new virtual machine.</span></span>

<span data-ttu-id="9ec63-141">pomocí AzCopy data hello toocopy:</span><span class="sxs-lookup"><span data-stu-id="9ec63-141">toocopy hello data using AzCopy:</span></span>

1. <span data-ttu-id="9ec63-142">Přihlaste se tooyour virtuální počítač (VM)</span><span class="sxs-lookup"><span data-stu-id="9ec63-142">Log in tooyour virtual machine (VM)</span></span>
2. <span data-ttu-id="9ec63-143">Vytvořte nový adresář v datový disk hello Virtuálního počítače (Poznámka: Nepoužívejte hello dočasné Disk, který se dodává s hello virtuální počítač jako datový Disk).</span><span class="sxs-lookup"><span data-stu-id="9ec63-143">Create a new directory in hello VM's data disk (Note: Do not use hello Temporary Disk which comes with hello VM as a Data Disk).</span></span>
3. <span data-ttu-id="9ec63-144">V okně příkazového řádku spusťte hello následující příkazového řádku Azcopy, nahraďte < path_to_data_folder > vaše data složky vytvořené v (2):</span><span class="sxs-lookup"><span data-stu-id="9ec63-144">In a Command Prompt window, run hello following Azcopy command line, replacing <path_to_data_folder> with your data folder created in (2):</span></span>
   
        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S
   
    <span data-ttu-id="9ec63-145">Po dokončení hello AzCopy CSV soubory ZIP celkem 24 (12 pro cestu\_dat a 12 pro cestu\_tarif) by měl být ve složce data hello.</span><span class="sxs-lookup"><span data-stu-id="9ec63-145">When hello AzCopy completes, a total of 24 zipped CSV files (12 for trip\_data and 12 for trip\_fare) should be in hello data folder.</span></span>
4. <span data-ttu-id="9ec63-146">Rozbalte hello stažené soubory.</span><span class="sxs-lookup"><span data-stu-id="9ec63-146">Unzip hello downloaded files.</span></span> <span data-ttu-id="9ec63-147">Všimněte si hello složku, kde jsou uloženy soubory hello nekomprimovaným.</span><span class="sxs-lookup"><span data-stu-id="9ec63-147">Note hello folder where hello uncompressed files reside.</span></span> <span data-ttu-id="9ec63-148">Tato složka bude odkazované tooas hello < cesta\_k\_data\_soubory\>.</span><span class="sxs-lookup"><span data-stu-id="9ec63-148">This folder will be referred tooas hello <path\_to\_data\_files\>.</span></span>

## <span data-ttu-id="9ec63-149"><a name="dbload"></a>Hromadný Import dat do databáze serveru SQL</span><span class="sxs-lookup"><span data-stu-id="9ec63-149"><a name="dbload"></a>Bulk Import Data into SQL Server Database</span></span>
<span data-ttu-id="9ec63-150">Hello výkon načítání nebo přenos velkých objemů dat tooan SQL database a následné dotazy lze zvýšit pomocí *rozdělena na oddíly tabulky a zobrazení*.</span><span class="sxs-lookup"><span data-stu-id="9ec63-150">hello performance of loading/transferring large amounts of data tooan SQL database and subsequent queries can be improved by using *Partitioned Tables and Views*.</span></span> <span data-ttu-id="9ec63-151">V této části jsme postupujte podle pokynů hello v tématu [paralelní hromadný Import pomocí SQL oddílu tabulky dat](machine-learning-data-science-parallel-load-sql-partitioned-tables.md) toocreate nová data hello databáze a zatížení do dělené tabulky paralelně.</span><span class="sxs-lookup"><span data-stu-id="9ec63-151">In this section, we will follow hello instructions described in [Parallel Bulk Data Import Using SQL Partition Tables](machine-learning-data-science-parallel-load-sql-partitioned-tables.md) toocreate a new database and load hello data into partitioned tables in parallel.</span></span>

1. <span data-ttu-id="9ec63-152">Během přihlášení tooyour virtuálního počítače, spusťte **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="9ec63-152">While logged in tooyour VM, start **SQL Server Management Studio**.</span></span>
2. <span data-ttu-id="9ec63-153">Připojte pomocí ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="9ec63-153">Connect using Windows Authentication.</span></span>
   
    ![Připojení přes SSMS][12]
3. <span data-ttu-id="9ec63-155">Pokud máte ještě změnit režim ověřování systému SQL Server hello a vytvořit nového uživatele SQL přihlášení, otevřete soubor skriptu hello s názvem **změnit\_auth.sql** v hello **ukázkové skripty** složky.</span><span class="sxs-lookup"><span data-stu-id="9ec63-155">If you have not yet changed hello SQL Server authentication mode and created a new SQL login user, open hello script file named **change\_auth.sql** in hello **Sample Scripts** folder.</span></span> <span data-ttu-id="9ec63-156">Změňte hello výchozí uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="9ec63-156">Change hello  default user name and password.</span></span> <span data-ttu-id="9ec63-157">Klikněte na tlačítko **! Spuštění** ve hello nástrojů toorun hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="9ec63-157">Click **!Execute** in hello toolbar toorun hello script.</span></span>
   
    ![Spuštění skriptu][13]
4. <span data-ttu-id="9ec63-159">Ověřte nebo změňte hello výchozí databáze a protokolu složky tooensure nově vytvořené databáze bude uložen v datový Disk systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9ec63-159">Verify and/or change hello SQL Server default database and log folders tooensure that newly created databases will be stored in a Data Disk.</span></span> <span data-ttu-id="9ec63-160">bitovou kopii Hello virtuální počítač SQL Server, která je optimalizovaná pro datawarehousing zatížení je předem nakonfigurovaný s disky protokolu a data.</span><span class="sxs-lookup"><span data-stu-id="9ec63-160">hello SQL Server VM image that is optimized for datawarehousing loads is pre-configured with data and log disks.</span></span> <span data-ttu-id="9ec63-161">Pokud virtuální počítač neobsahuje datový Disk a přidat nové virtuální pevné disky během hello procesu instalace virtuálních počítačů, změňte výchozí složky hello takto:</span><span class="sxs-lookup"><span data-stu-id="9ec63-161">If your VM did not include a Data Disk and you added new virtual hard disks during hello VM setup process, change hello default folders as follows:</span></span>
   
   * <span data-ttu-id="9ec63-162">Klikněte pravým tlačítkem na název serveru SQL Server hello v hello zbývajících panelu a klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="9ec63-162">Right-click hello SQL Server name in hello left panel and click **Properties**.</span></span>
     
       ![Vlastnosti serveru SQL][14]
   * <span data-ttu-id="9ec63-164">Vyberte **nastavení databáze** z hello **vybrat stránku** seznamu toohello doleva.</span><span class="sxs-lookup"><span data-stu-id="9ec63-164">Select **Database Settings** from hello **Select a page** list toohello left.</span></span>
   * <span data-ttu-id="9ec63-165">Ověřte nebo změňte hello **databáze výchozí umístění** toohello **datový Disk** umístění podle vaší volby.</span><span class="sxs-lookup"><span data-stu-id="9ec63-165">Verify and/or change hello **Database default locations** toohello **Data Disk** locations of your choice.</span></span> <span data-ttu-id="9ec63-166">Toto je, kde jsou umístěny nové databáze, pokud vytvořen s hello výchozí umístění nastavení.</span><span class="sxs-lookup"><span data-stu-id="9ec63-166">This is where new databases reside if created with hello default location settings.</span></span>
     
       ![Výchozí nastavení databáze SQL][15]  
5. <span data-ttu-id="9ec63-168">toocreate novou databázi a sadu skupin souborů toohold hello dělené tabulky, otevřete hello ukázkový skript **vytvořit\_db\_default.sql**.</span><span class="sxs-lookup"><span data-stu-id="9ec63-168">toocreate a new database and a set of filegroups toohold hello partitioned tables, open hello sample script **create\_db\_default.sql**.</span></span> <span data-ttu-id="9ec63-169">skript Hello vytvoří novou databázi s názvem **TaxiNYC** a 12 skupiny souborů v umístění dat výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="9ec63-169">hello script will create a new database named **TaxiNYC** and 12 filegroups in hello default data location.</span></span> <span data-ttu-id="9ec63-170">Každou skupinu souborů bude obsahovat jeden měsíc cesty\_dat a cesty\_jízdenky data.</span><span class="sxs-lookup"><span data-stu-id="9ec63-170">Each filegroup will hold one month of trip\_data and trip\_fare data.</span></span> <span data-ttu-id="9ec63-171">Upravte hello název databáze, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="9ec63-171">Modify hello database name, if desired.</span></span> <span data-ttu-id="9ec63-172">Klikněte na tlačítko **! Spuštění** toorun hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="9ec63-172">Click **!Execute** toorun hello script.</span></span>
6. <span data-ttu-id="9ec63-173">Dále vytvořte dvě tabulky oddílů, jeden pro cestu hello\_dat a druhý pro cestu hello\_tarif.</span><span class="sxs-lookup"><span data-stu-id="9ec63-173">Next, create two partition tables, one for hello trip\_data and another for hello trip\_fare.</span></span> <span data-ttu-id="9ec63-174">Otevřete hello ukázkový skript **vytvořit\_oddílů\_table.sql**, který bude:</span><span class="sxs-lookup"><span data-stu-id="9ec63-174">Open hello sample script **create\_partitioned\_table.sql**, which will:</span></span>
   
   * <span data-ttu-id="9ec63-175">Vytvoření oddílu funkce toosplit hello dat po měsících.</span><span class="sxs-lookup"><span data-stu-id="9ec63-175">Create a partition function toosplit hello data by month.</span></span>
   * <span data-ttu-id="9ec63-176">Vytvořte toomap schéma oddílu každý měsíc dat tooa různých souborů.</span><span class="sxs-lookup"><span data-stu-id="9ec63-176">Create a partition scheme toomap each month's data tooa different filegroup.</span></span>
   * <span data-ttu-id="9ec63-177">Vytvořte dva schéma oddílu namapované toohello dělené tabulky: **nyctaxi\_cestě** bude obsahovat hello cestě\_dat a **nyctaxi\_tarif** bude obsahovat hello cesty \_jízdenky data.</span><span class="sxs-lookup"><span data-stu-id="9ec63-177">Create two partitioned tables mapped toohello partition scheme: **nyctaxi\_trip** will hold hello trip\_data and **nyctaxi\_fare** will hold hello trip\_fare data.</span></span>
     
     <span data-ttu-id="9ec63-178">Klikněte na tlačítko **! Spuštění** toorun hello skriptu a vytvářet tabulky hello rozdělena na oddíly.</span><span class="sxs-lookup"><span data-stu-id="9ec63-178">Click **!Execute** toorun hello script and create hello partitioned tables.</span></span>
7. <span data-ttu-id="9ec63-179">V hello **ukázkové skripty** složky, existují dva ukázkové skripty prostředí PowerShell zadat toodemonstrate paralelní hromadné importy dat tooSQL serveru tabulek.</span><span class="sxs-lookup"><span data-stu-id="9ec63-179">In hello **Sample Scripts** folder, there are two sample PowerShell scripts provided toodemonstrate parallel bulk imports of data tooSQL Server tables.</span></span>
   
   * <span data-ttu-id="9ec63-180">**BCP\_paralelní\_generic.ps1** je obecný skript tooparallel hromadného importu dat do tabulky.</span><span class="sxs-lookup"><span data-stu-id="9ec63-180">**bcp\_parallel\_generic.ps1** is a generic script tooparallel bulk import data into a table.</span></span> <span data-ttu-id="9ec63-181">Umožňuje změnit tento skript tooset hello vstup a cíle proměnné, které je uvedené v řádcích hello komentáře ve skriptu hello.</span><span class="sxs-lookup"><span data-stu-id="9ec63-181">Modify this script tooset hello input and target variables as indicated in hello comment lines in hello script.</span></span>
   * <span data-ttu-id="9ec63-182">**BCP\_paralelní\_nyctaxi.ps1** je předem nakonfigurovaný verze hello obecný skript a může být použité tootooload obě tabulky pro data NYC taxíkem cest hello.</span><span class="sxs-lookup"><span data-stu-id="9ec63-182">**bcp\_parallel\_nyctaxi.ps1** is a pre-configured version of hello generic script and can be used tootooload both tables for hello NYC Taxi Trips data.</span></span>  
8. <span data-ttu-id="9ec63-183">Klikněte pravým tlačítkem na hello **bcp\_paralelní\_nyctaxi.ps1** název skriptu a klikněte na tlačítko **upravit** tooopen v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9ec63-183">Right-click hello **bcp\_parallel\_nyctaxi.ps1** script name and click **Edit** tooopen it in PowerShell.</span></span> <span data-ttu-id="9ec63-184">Zkontrolujte hello předvolby proměnné a úpravám podle názvu tooyour vybrané databáze, složka vstupních dat, cílové složce protokolu a cesty toohello ukázkových formát souborů **nyctaxi_trip.xml** a **nyctaxi\_ fare.xml** (součástí hello **ukázkové skripty** složku).</span><span class="sxs-lookup"><span data-stu-id="9ec63-184">Review hello preset variables and modify according tooyour selected database name, input data folder, target log folder, and paths toohello  sample format files **nyctaxi_trip.xml** and **nyctaxi\_fare.xml** (provided in hello **Sample Scripts** folder).</span></span>
   
    ![Hromadný Import dat][16]
   
    <span data-ttu-id="9ec63-186">Můžete také vybrat režim ověřování hello, výchozí hodnota je ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="9ec63-186">You may also select hello authentication mode, default is Windows Authentication.</span></span> <span data-ttu-id="9ec63-187">Klikněte na tlačítko hello zelenou šipku v panelu nástrojů toorun hello.</span><span class="sxs-lookup"><span data-stu-id="9ec63-187">Click hello green arrow in hello toolbar toorun.</span></span> <span data-ttu-id="9ec63-188">Hello skript se spustí 24 operace hromadného importu v paralelní, 12 pro každou tabulku oddílů.</span><span class="sxs-lookup"><span data-stu-id="9ec63-188">hello script will launch 24 bulk import operations in parallel, 12 for each partitioned table.</span></span> <span data-ttu-id="9ec63-189">Probíhá import dat hello může sledovat otevřením složky dat výchozí systému SQL Server hello výše.</span><span class="sxs-lookup"><span data-stu-id="9ec63-189">You may monitor hello data import progress by opening hello SQL Server default data folder as set above.</span></span>
9. <span data-ttu-id="9ec63-190">sestavy skript prostředí PowerShell Hello Dobrý den, počáteční a koncový čas.</span><span class="sxs-lookup"><span data-stu-id="9ec63-190">hello PowerShell script reports hello starting and ending times.</span></span> <span data-ttu-id="9ec63-191">Pokud všechny hromadně importy dokončení, uvede se hello čas dokončení.</span><span class="sxs-lookup"><span data-stu-id="9ec63-191">When all bulk imports complete, hello ending time is reported.</span></span> <span data-ttu-id="9ec63-192">Žádné chyby hlášené hello do cílové složky protokolů zkontrolujte hello cíl protokolu složky tooverify, který hello hromadný import byl úspěšný, tj.</span><span class="sxs-lookup"><span data-stu-id="9ec63-192">Check hello target log folder tooverify that hello bulk imports were successful, i.e., no errors reported in hello target log folder.</span></span>
10. <span data-ttu-id="9ec63-193">Vaše databáze je nyní připraven pro zkoumání, technici funkce a další operace podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="9ec63-193">Your database is now ready for exploration, feature engineering, and other operations as desired.</span></span> <span data-ttu-id="9ec63-194">Vzhledem k tomu, že hello tabulky jsou rozděleny do oddílů podle toohello **vyzvednutí\_data a času** pole, dotazy, které zahrnují **vyzvednutí\_data a času** podmínky v hello  **KDE** klauzule výhod schéma oddílu hello.</span><span class="sxs-lookup"><span data-stu-id="9ec63-194">Since hello tables are partitioned according toohello **pickup\_datetime** field, queries which include **pickup\_datetime** conditions in hello **WHERE** clause will benefit from hello partition scheme.</span></span>
11. <span data-ttu-id="9ec63-195">V **SQL Server Management Studio**, prozkoumejte hello poskytuje ukázkový skript **ukázka\_queries.sql**.</span><span class="sxs-lookup"><span data-stu-id="9ec63-195">In **SQL Server Management Studio**, explore hello provided sample script **sample\_queries.sql**.</span></span> <span data-ttu-id="9ec63-196">toorun žádné hello ukázkové dotazy, zvýraznění hello dotaz na řádky a pak klikněte na **! Spuštění** v panelu nástrojů hello.</span><span class="sxs-lookup"><span data-stu-id="9ec63-196">toorun any of hello sample queries, highlight hello query lines then click **!Execute** in hello toolbar.</span></span>
12. <span data-ttu-id="9ec63-197">Hello NYC taxíkem cest dat je načten do dvou samostatných tabulkách.</span><span class="sxs-lookup"><span data-stu-id="9ec63-197">hello NYC Taxi Trips data is loaded in two separate tables.</span></span> <span data-ttu-id="9ec63-198">operace spojení tooimprove, důrazně doporučujeme tooindex hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="9ec63-198">tooimprove join operations, it is highly recommended tooindex hello tables.</span></span> <span data-ttu-id="9ec63-199">Hello ukázkový skript **vytvořit\_oddílů\_index.sql** vytváří indexy oddílů hello spojení složený klíč **medailonu, hackerský\_licence a vyzvednutí\_data a času**.</span><span class="sxs-lookup"><span data-stu-id="9ec63-199">hello sample script **create\_partitioned\_index.sql** creates partitioned indexes on hello composite join key **medallion, hack\_license, and pickup\_datetime**.</span></span>

## <span data-ttu-id="9ec63-200"><a name="dbexplore"></a>Zkoumání dat a konstruování funkce v systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="9ec63-200"><a name="dbexplore"></a>Data Exploration and Feature Engineering in SQL Server</span></span>
<span data-ttu-id="9ec63-201">V této části, provedeme zkoumání a funkce generování dat spuštěním dotazů SQL přímo v hello **SQL Server Management Studio** použití databáze systému SQL Server hello vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="9ec63-201">In this section, we will perform data exploration and feature generation by running SQL queries directly in hello **SQL Server Management Studio** using hello SQL Server database created earlier.</span></span> <span data-ttu-id="9ec63-202">Ukázkový skript s názvem **ukázka\_queries.sql** je součástí hello **ukázkové skripty** složky.</span><span class="sxs-lookup"><span data-stu-id="9ec63-202">A sample script named **sample\_queries.sql** is provided in hello **Sample Scripts** folder.</span></span> <span data-ttu-id="9ec63-203">Upravit název hello skriptu toochange hello databáze, pokud se liší od výchozí hello: **TaxiNYC**.</span><span class="sxs-lookup"><span data-stu-id="9ec63-203">Modify hello script toochange hello database name, if it is different from hello default: **TaxiNYC**.</span></span>

<span data-ttu-id="9ec63-204">V tomto cvičení provedeme následující:</span><span class="sxs-lookup"><span data-stu-id="9ec63-204">In this exercise, we will:</span></span>

* <span data-ttu-id="9ec63-205">Připojit příliš**SQL Server Management Studio** pomocí buď ověřování systému Windows nebo pomocí ověřování SQL a hello SQL přihlašovací jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="9ec63-205">Connect too**SQL Server Management Studio** using either Windows Authentication or using SQL Authentication and hello SQL login name and password.</span></span>
* <span data-ttu-id="9ec63-206">Prozkoumejte data distribuce několik polí v různých časových oken.</span><span class="sxs-lookup"><span data-stu-id="9ec63-206">Explore data distributions of a few fields in varying time windows.</span></span>
* <span data-ttu-id="9ec63-207">Prozkoumejte data quality hello zeměpisné šířky a délky polí.</span><span class="sxs-lookup"><span data-stu-id="9ec63-207">Investigate data quality of hello longitude and latitude fields.</span></span>
* <span data-ttu-id="9ec63-208">Generovat binární a více třídami klasifikační štítky podle hello **tip\_velikost**.</span><span class="sxs-lookup"><span data-stu-id="9ec63-208">Generate binary and multiclass classification labels based on hello **tip\_amount**.</span></span>
* <span data-ttu-id="9ec63-209">Generovat funkce a výpočetní nebo porovnat cestě vzdálenosti.</span><span class="sxs-lookup"><span data-stu-id="9ec63-209">Generate features and compute/compare trip distances.</span></span>
* <span data-ttu-id="9ec63-210">Připojení k hello dvou tabulek a extrahovat z náhodného vzorku, který bude použité toobuild modely.</span><span class="sxs-lookup"><span data-stu-id="9ec63-210">Join hello two tables and extract a random sample that will be used toobuild models.</span></span>

<span data-ttu-id="9ec63-211">Pokud jste připravené tooproceed tooAzure Machine Learning, můžete se buď:</span><span class="sxs-lookup"><span data-stu-id="9ec63-211">When you are ready tooproceed tooAzure Machine Learning, you may either:</span></span>  

1. <span data-ttu-id="9ec63-212">Uložit hello konečné SQL dotaz tooextract a ukázkové hello dat a způsobené kopírováním a vkládáním hello dotaz přímo do [importovat Data] [ import-data] modulu v Azure Machine Learning, nebo</span><span class="sxs-lookup"><span data-stu-id="9ec63-212">Save hello final SQL query tooextract and sample hello data and copy-paste hello query directly into a [Import Data][import-data] module in Azure Machine Learning, or</span></span>
2. <span data-ttu-id="9ec63-213">Zachovat hello vzorků a inženýrství dat, které máte v plánu toouse pro model sestavování v nové databáze tabulky a používání hello novou tabulku v hello [importovat Data] [ import-data] modulu v Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="9ec63-213">Persist hello sampled and engineered data you plan toouse for model building in a new database table and use hello new table in hello [Import Data][import-data] module in Azure Machine Learning.</span></span>

<span data-ttu-id="9ec63-214">V této části jsme se hello poslední dotaz tooextract a ukázkové hello data uložit.</span><span class="sxs-lookup"><span data-stu-id="9ec63-214">In this section we will save hello final query tooextract and sample hello data.</span></span> <span data-ttu-id="9ec63-215">druhý metoda Hello je znázorněn v hello [zkoumání dat a funkce Engineering v poznámkovém bloku IPython](#ipnb) části.</span><span class="sxs-lookup"><span data-stu-id="9ec63-215">hello second method is demonstrated in hello [Data Exploration and Feature Engineering in IPython Notebook](#ipnb) section.</span></span>

<span data-ttu-id="9ec63-216">Pro rychlou kontrolu hello počtu řádků a sloupců v hello tabulky vyplněny dříve paralelní hromadného importu</span><span class="sxs-lookup"><span data-stu-id="9ec63-216">For a quick verification of hello number of rows and columns in hello tables populated earlier using parallel bulk import,</span></span>

    -- Report number of rows in table nyctaxi_trip without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('nyctaxi_trip')

    -- Report number of columns in table nyctaxi_trip
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = 'nyctaxi_trip'

#### <a name="exploration-trip-distribution-by-medallion"></a><span data-ttu-id="9ec63-217">Zkoumání: Cestě distribuce podle Medailon</span><span class="sxs-lookup"><span data-stu-id="9ec63-217">Exploration: Trip distribution by medallion</span></span>
<span data-ttu-id="9ec63-218">Tento příklad identifikuje Medailon hello (taxi čísla) s více než 100 služebních cest v daném časovém období.</span><span class="sxs-lookup"><span data-stu-id="9ec63-218">This example identifies hello medallion (taxi numbers) with more than 100 trips within a given time period.</span></span> <span data-ttu-id="9ec63-219">Hello dotazu by využívat přístup k tabulce hello rozdělena na oddíly, vzhledem k tomu, že je podmíněno tím schéma oddílů hello **vyzvednutí\_data a času**.</span><span class="sxs-lookup"><span data-stu-id="9ec63-219">hello query would benefit from hello partitioned table access since it is conditioned by hello partition scheme of **pickup\_datetime**.</span></span> <span data-ttu-id="9ec63-220">Dotazování hello úplnou datovou sadu se ujistěte se taky použít hello oddílů tabulky nebo indexu kontroly.</span><span class="sxs-lookup"><span data-stu-id="9ec63-220">Querying hello full dataset will also make use of hello partitioned table and/or index scan.</span></span>

    SELECT medallion, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

#### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a><span data-ttu-id="9ec63-221">Zkoumání: Cestě distribuce podle Medailon a hack_license</span><span class="sxs-lookup"><span data-stu-id="9ec63-221">Exploration: Trip distribution by medallion and hack_license</span></span>
    SELECT medallion, hack_license, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

#### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a><span data-ttu-id="9ec63-222">Hodnocení kvality dat: Ověřte záznamy s nesprávné délky a šířky</span><span class="sxs-lookup"><span data-stu-id="9ec63-222">Data Quality Assessment: Verify records with incorrect longitude and/or latitude</span></span>
<span data-ttu-id="9ec63-223">Tento příklad prověří, pokud jakýkoli hello zeměpisné délky a šířky polí buď obsahuje neplatnou hodnotu (radián stupňů musí být mezi -90 a 90), nebo (0, 0) souřadnice.</span><span class="sxs-lookup"><span data-stu-id="9ec63-223">This example investigates if any of hello longitude and/or latitude fields either contain an invalid value (radian degrees should be between -90 and 90), or have (0, 0) coordinates.</span></span>

    SELECT COUNT(*) FROM nyctaxi_trip
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

#### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a><span data-ttu-id="9ec63-224">Zkoumání: Šikmý vs. Není vysypávány služebních cest distribučního</span><span class="sxs-lookup"><span data-stu-id="9ec63-224">Exploration: Tipped vs. Not Tipped Trips distribution</span></span>
<span data-ttu-id="9ec63-225">Tento příklad vyhledá hello počet cest, které byly vysypávány oproti není vysypávány v daném časovém období (nebo hello úplnou datovou sadu Pokud pokrývající celý rok hello).</span><span class="sxs-lookup"><span data-stu-id="9ec63-225">This example finds hello number of trips that were tipped vs. not tipped in a given time period (or in hello full dataset if covering hello full year).</span></span> <span data-ttu-id="9ec63-226">Toto rozdělení odráží toobe distribuční binární popisek hello později použita k modelování binární klasifikace.</span><span class="sxs-lookup"><span data-stu-id="9ec63-226">This distribution reflects hello binary label distribution toobe later used for binary classification modeling.</span></span>

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM nyctaxi_fare
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

#### <a name="exploration-tip-classrange-distribution"></a><span data-ttu-id="9ec63-227">Zkoumání: Třída nebo rozsah distribuční Tip</span><span class="sxs-lookup"><span data-stu-id="9ec63-227">Exploration: Tip Class/Range Distribution</span></span>
<span data-ttu-id="9ec63-228">Tento příklad vypočítá distribuci hello rozsahy tip v daném časovém období (nebo hello úplnou datovou sadu Pokud pokrývající celý rok hello).</span><span class="sxs-lookup"><span data-stu-id="9ec63-228">This example computes hello distribution of tip ranges in a given time period (or in hello full dataset if covering hello full year).</span></span> <span data-ttu-id="9ec63-229">Toto je hello distribuční hello popisek tříd, které se použijí později pro modelování více třídami klasifikace.</span><span class="sxs-lookup"><span data-stu-id="9ec63-229">This is hello distribution of hello label classes that will be used later for multiclass classification modeling.</span></span>

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

#### <a name="exploration-compute-and-compare-trip-distance"></a><span data-ttu-id="9ec63-230">Zkoumání: Výpočetní a porovnání vzdálenost cesty</span><span class="sxs-lookup"><span data-stu-id="9ec63-230">Exploration: Compute and Compare Trip Distance</span></span>
<span data-ttu-id="9ec63-231">Tento příklad převede hello vyzvednutí a odkládací zeměpisné délky a šířky tooSQL geography body, vypočítá vzdálenost cestě hello pomocí SQL geography body rozdíl a vrátí z náhodného vzorku hello výsledky pro porovnání.</span><span class="sxs-lookup"><span data-stu-id="9ec63-231">This example converts hello pickup and drop-off longitude and latitude tooSQL geography points, computes hello trip distance using SQL geography points difference, and returns a random sample of hello results for comparison.</span></span> <span data-ttu-id="9ec63-232">Příklad Hello omezí výsledky hello toovalid koordinuje jenom pomocí hello kvality assessment dotaz na data popsané výše.</span><span class="sxs-lookup"><span data-stu-id="9ec63-232">hello example limits hello results toovalid coordinates only using hello data quality assessment query covered earlier.</span></span>

    SELECT
    pickup_location=geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326)
    ,dropoff_location=geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326)
    ,trip_distance
    ,computedist=round(geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326).STDistance(geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326))/1000, 2)
    FROM nyctaxi_trip
    tablesample(0.01 percent)
    WHERE CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND   CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

#### <a name="feature-engineering-in-sql-queries"></a><span data-ttu-id="9ec63-233">Funkce analýzy v dotazy SQL</span><span class="sxs-lookup"><span data-stu-id="9ec63-233">Feature Engineering in SQL Queries</span></span>
<span data-ttu-id="9ec63-234">Hello popisek generování geography převod zkoumání dotazy a může být také použít toogenerate popisky nebo funkcích odebráním hello počítání část.</span><span class="sxs-lookup"><span data-stu-id="9ec63-234">hello label generation and geography conversion exploration queries can also be used toogenerate labels/features by removing hello counting part.</span></span> <span data-ttu-id="9ec63-235">Další funkce engineering SQL příklady jsou uvedeny v hello [zkoumání dat a funkce Engineering v poznámkovém bloku IPython](#ipnb) části.</span><span class="sxs-lookup"><span data-stu-id="9ec63-235">Additional feature engineering SQL examples are provided in hello [Data Exploration and Feature Engineering in IPython Notebook](#ipnb) section.</span></span> <span data-ttu-id="9ec63-236">Je efektivnější toorun hello funkce generování dotazy na úplnou datovou sadu hello nebo velké podmnožinu pomocí dotazů SQL, které se spustit přímo na instanci databáze systému SQL Server hello.</span><span class="sxs-lookup"><span data-stu-id="9ec63-236">It is more efficient toorun hello feature generation queries on hello full dataset or a large subset of it using SQL queries which run directly on hello SQL Server database instance.</span></span> <span data-ttu-id="9ec63-237">Hello dotazy může být spuštěna v **SQL Server Management Studio**, IPython Poznámkový blok nebo všechny vývojové nástroje/prostředí který má přístup k databázi hello místně nebo vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="9ec63-237">hello queries may be executed in **SQL Server Management Studio**, IPython Notebook or any development tool/environment which can access hello database locally or remotely.</span></span>

#### <a name="preparing-data-for-model-building"></a><span data-ttu-id="9ec63-238">Připravují se Data k vytváření modelů</span><span class="sxs-lookup"><span data-stu-id="9ec63-238">Preparing Data for Model Building</span></span>
<span data-ttu-id="9ec63-239">Hello následující dotaz spojení hello **nyctaxi\_cestě** a **nyctaxi\_tarif** tabulky, vygeneruje štítek binární klasifikace **vysypávány**, Popisek více třída klasifikace **tip\_třída**a extrahuje z náhodného vzorku 1 % z hello úplné připojené k datové sadě.</span><span class="sxs-lookup"><span data-stu-id="9ec63-239">hello following query joins hello **nyctaxi\_trip** and **nyctaxi\_fare** tables, generates a binary classification label **tipped**, a multi-class classification label **tip\_class**, and extracts a 1% random sample from hello full joined dataset.</span></span> <span data-ttu-id="9ec63-240">Tento dotaz můžete zkopírovat a vložit přímo v hello [Azure Machine Learning Studio](https://studio.azureml.net) [importovat Data] [ import-data] modul pro přijímání přímé dat z databáze serveru SQL Server hello instance v Azure.</span><span class="sxs-lookup"><span data-stu-id="9ec63-240">This query can be copied then pasted directly in hello [Azure Machine Learning Studio](https://studio.azureml.net) [Import Data][import-data] module for direct data ingestion from hello SQL Server database instance in Azure.</span></span> <span data-ttu-id="9ec63-241">dotaz Hello vyloučí záznamy s nesprávnou (0, 0) souřadnice.</span><span class="sxs-lookup"><span data-stu-id="9ec63-241">hello query excludes records with incorrect (0, 0) coordinates.</span></span>

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,     f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_trip t, nyctaxi_fare f
    TABLESAMPLE (1 percent)
    WHERE t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'


## <span data-ttu-id="9ec63-242"><a name="ipnb"></a>Zkoumání dat a funkce technikům v poznámkovém bloku IPython</span><span class="sxs-lookup"><span data-stu-id="9ec63-242"><a name="ipnb"></a>Data Exploration and Feature Engineering in IPython Notebook</span></span>
<span data-ttu-id="9ec63-243">V této části provedeme zkoumání dat a funkce generování pomocí jazyka Python a SQL dotazů na databázi systému SQL Server hello vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="9ec63-243">In this section, we will perform data exploration and feature generation using both Python and SQL queries against hello SQL Server database created earlier.</span></span> <span data-ttu-id="9ec63-244">Ukázka IPython Poznámkový blok s názvem **machine-Learning-data-science-process-sql-story.ipynb** je součástí hello **poznámkových bloků IPython ukázka** složky.</span><span class="sxs-lookup"><span data-stu-id="9ec63-244">A sample IPython notebook named **machine-Learning-data-science-process-sql-story.ipynb** is provided in hello **Sample IPython Notebooks** folder.</span></span> <span data-ttu-id="9ec63-245">Tento poznámkový blok je také k dispozici na [Githubu](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks).</span><span class="sxs-lookup"><span data-stu-id="9ec63-245">This notebook is also available on [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks).</span></span>

<span data-ttu-id="9ec63-246">Hello Doporučené pořadí při práci s velkým objemem dat je hello následující:</span><span class="sxs-lookup"><span data-stu-id="9ec63-246">hello recommended sequence when working with big data is hello following:</span></span>

* <span data-ttu-id="9ec63-247">Přečtěte si v malé ukázkové hello dat do data v paměti rámečku.</span><span class="sxs-lookup"><span data-stu-id="9ec63-247">Read in a small sample of hello data into an in-memory data frame.</span></span>
* <span data-ttu-id="9ec63-248">Provést některé vizualizace a explorations pomocí hello jen Vzorkovaná data.</span><span class="sxs-lookup"><span data-stu-id="9ec63-248">Perform some visualizations and explorations using hello sampled data.</span></span>
* <span data-ttu-id="9ec63-249">Experimentů pomocí funkce inženýrství pomocí hello Vzorkovat data.</span><span class="sxs-lookup"><span data-stu-id="9ec63-249">Experiment with feature engineering using hello sampled data.</span></span>
* <span data-ttu-id="9ec63-250">Pro větší zkoumání dat manipulaci s daty a funkce technologie, použijte dotazy SQL tooissue Python přímo pro databázi systému SQL Server hello v hello virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="9ec63-250">For larger data exploration, data manipulation and feature engineering, use Python tooissue SQL Queries directly against hello SQL Server database in hello Azure VM.</span></span>
* <span data-ttu-id="9ec63-251">Rozhodněte, hello ukázka velikost toouse pro vytváření modelů Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="9ec63-251">Decide hello sample size toouse for Azure Machine Learning model building.</span></span>

<span data-ttu-id="9ec63-252">Jakmile budete připraveni tooproceed tooAzure Machine Learning, můžete buď může:</span><span class="sxs-lookup"><span data-stu-id="9ec63-252">When ready tooproceed tooAzure Machine Learning, you may either:</span></span>  

1. <span data-ttu-id="9ec63-253">Uložit hello konečné SQL dotaz tooextract a ukázkové hello dat a způsobené kopírováním a vkládáním hello dotaz přímo do [importovat Data] [ import-data] modulu v Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="9ec63-253">Save hello final SQL query tooextract and sample hello data and copy-paste hello query directly into a [Import Data][import-data] module in Azure Machine Learning.</span></span> <span data-ttu-id="9ec63-254">Tato metoda je znázorněn v hello [vytváření modelů v Azure Machine Learning](#mlmodel) části.</span><span class="sxs-lookup"><span data-stu-id="9ec63-254">This method is demonstrated in hello [Building Models in Azure Machine Learning](#mlmodel) section.</span></span>    
2. <span data-ttu-id="9ec63-255">Zachování hello vzorků a inženýrství dat, které máte v plánu toouse pro model sestavování v nové databázové tabulky, potom použijte novou tabulku hello v hello [importovat Data] [ import-data] modulu.</span><span class="sxs-lookup"><span data-stu-id="9ec63-255">Persist hello sampled and engineered data you plan toouse for model building in a new database table, then use hello new table in hello [Import Data][import-data] module.</span></span>

<span data-ttu-id="9ec63-256">Hello následují několik zkoumání dat, vizualizace dat a funkce technici příklady.</span><span class="sxs-lookup"><span data-stu-id="9ec63-256">hello following are a few data exploration, data visualization, and feature engineering examples.</span></span> <span data-ttu-id="9ec63-257">Další příklady najdete v tématu hello ukázkový SQL IPython Poznámkový blok v hello **poznámkových bloků IPython ukázka** složky.</span><span class="sxs-lookup"><span data-stu-id="9ec63-257">For more examples, see hello sample SQL IPython notebook in hello **Sample IPython Notebooks** folder.</span></span>

#### <a name="initialize-database-credentials"></a><span data-ttu-id="9ec63-258">Inicializace přihlašovací údaje databáze</span><span class="sxs-lookup"><span data-stu-id="9ec63-258">Initialize Database Credentials</span></span>
<span data-ttu-id="9ec63-259">Inicializace nastavení připojení k databázi v hello následující proměnné:</span><span class="sxs-lookup"><span data-stu-id="9ec63-259">Initialize your database connection settings in hello following variables:</span></span>

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database server>

#### <a name="create-database-connection"></a><span data-ttu-id="9ec63-260">Vytvoření připojení k databázi</span><span class="sxs-lookup"><span data-stu-id="9ec63-260">Create Database Connection</span></span>
    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

#### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a><span data-ttu-id="9ec63-261">Sestava počtu řádků a sloupců v tabulce nyctaxi_trip</span><span class="sxs-lookup"><span data-stu-id="9ec63-261">Report number of rows and columns in table nyctaxi_trip</span></span>
    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('nyctaxi_trip')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('nyctaxi_trip')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

* <span data-ttu-id="9ec63-262">Celkový počet řádků = 173179759</span><span class="sxs-lookup"><span data-stu-id="9ec63-262">Total number of rows = 173179759</span></span>  
* <span data-ttu-id="9ec63-263">Celkový počet sloupců = 14</span><span class="sxs-lookup"><span data-stu-id="9ec63-263">Total number of columns = 14</span></span>

#### <a name="read-in-a-small-data-sample-from-hello-sql-server-database"></a><span data-ttu-id="9ec63-264">Pro čtení ve vzorku malá data z databáze systému SQL Server hello</span><span class="sxs-lookup"><span data-stu-id="9ec63-264">Read-in a small data sample from hello SQL Server Database</span></span>
    t0 = time.time()

    query = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (0.05 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time tooread hello sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

<span data-ttu-id="9ec63-265">Čas tooread hello ukázková tabulka je 6.492000 sekund</span><span class="sxs-lookup"><span data-stu-id="9ec63-265">Time tooread hello sample table is 6.492000 seconds</span></span>  
<span data-ttu-id="9ec63-266">Počet řádků a sloupců načíst = (84952, 21)</span><span class="sxs-lookup"><span data-stu-id="9ec63-266">Number of rows and columns retrieved = (84952, 21)</span></span>

#### <a name="descriptive-statistics"></a><span data-ttu-id="9ec63-267">Popisný statistiky</span><span class="sxs-lookup"><span data-stu-id="9ec63-267">Descriptive Statistics</span></span>
<span data-ttu-id="9ec63-268">Nyní jsou data připravena tooexplore hello vzorků.</span><span class="sxs-lookup"><span data-stu-id="9ec63-268">Now are ready tooexplore hello sampled data.</span></span> <span data-ttu-id="9ec63-269">Začneme s prohlížení popisný statistiku pro hello **cestě\_vzdálenost** (nebo libovolný jiný) polí:</span><span class="sxs-lookup"><span data-stu-id="9ec63-269">We start with looking at descriptive statistics for hello **trip\_distance** (or any other) field(s):</span></span>

    df1['trip_distance'].describe()

#### <a name="visualization-box-plot-example"></a><span data-ttu-id="9ec63-270">Vizualizace: Příklad vykreslení pole</span><span class="sxs-lookup"><span data-stu-id="9ec63-270">Visualization: Box Plot Example</span></span>
<span data-ttu-id="9ec63-271">Potom se podíváme na hello Krabicový graf pro hello cestě vzdálenost toovisualize hello quantiles</span><span class="sxs-lookup"><span data-stu-id="9ec63-271">Next we look at hello box plot for hello trip distance toovisualize hello quantiles</span></span>

    df1.boxplot(column='trip_distance',return_type='dict')

![Vykreslení #1][1]

#### <a name="visualization-distribution-plot-example"></a><span data-ttu-id="9ec63-273">Vizualizaci: Příklad vykreslení distribuční</span><span class="sxs-lookup"><span data-stu-id="9ec63-273">Visualization: Distribution Plot Example</span></span>
    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Vykreslení #2][2]

#### <a name="visualization-bar-and-line-plots"></a><span data-ttu-id="9ec63-275">Vizualizaci: Panelu a řádku pozemků</span><span class="sxs-lookup"><span data-stu-id="9ec63-275">Visualization: Bar and Line Plots</span></span>
<span data-ttu-id="9ec63-276">V tomto příkladu jsme bin hello vzdálenost cesty do pěti přihrádek a vizualizovat hello přihrádkování výsledky.</span><span class="sxs-lookup"><span data-stu-id="9ec63-276">In this example, we bin hello trip distance into five bins and visualize hello binning results.</span></span>

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

<span data-ttu-id="9ec63-277">Nemůžeme vykreslení hello výše bin distribuce v pás nebo řádek výkresu, jak je uvedeno níže</span><span class="sxs-lookup"><span data-stu-id="9ec63-277">We can plot hello above bin distribution in a bar or line plot as below</span></span>

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Vykreslení #3][3]

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Vykreslení #4][4]

#### <a name="visualization-scatterplot-example"></a><span data-ttu-id="9ec63-280">Vizualizaci: Příklad Scatterplot</span><span class="sxs-lookup"><span data-stu-id="9ec63-280">Visualization: Scatterplot Example</span></span>
<span data-ttu-id="9ec63-281">Ukážeme bodové vykreslení mezi **cestě\_čas\_v\_sekundy** a **cestě\_vzdálenost** toosee, pokud neexistuje žádné korelace</span><span class="sxs-lookup"><span data-stu-id="9ec63-281">We show scatter plot between **trip\_time\_in\_secs** and **trip\_distance** toosee if there is any correlation</span></span>

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Vykreslení #6][6]

<span data-ttu-id="9ec63-283">Podobně jsme můžete zkontrolovat hello vztah mezi **míra\_kód** a **cestě\_vzdálenost**.</span><span class="sxs-lookup"><span data-stu-id="9ec63-283">Similarly we can check hello relationship between **rate\_code** and **trip\_distance**.</span></span>

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![Vykreslení #8][8]

### <a name="sub-sampling-hello-data-in-sql"></a><span data-ttu-id="9ec63-285">Hello dílčí vzorkování dat v systému SQL</span><span class="sxs-lookup"><span data-stu-id="9ec63-285">Sub-Sampling hello Data in SQL</span></span>
<span data-ttu-id="9ec63-286">Při přípravě dat pro sestavování ve model [Azure Machine Learning Studio](https://studio.azureml.net), můžete rozhodnout buď na hello **toouse dotazu SQL přímo v modulu importu dat hello** nebo zachovat hello analyzovány a vzorků data do nové tabulky, které můžete použít v hello [importovat Data] [ import-data] modul s jednoduchou **vyberte * FROM < vaše\_nové\_tabulky\_name >**.</span><span class="sxs-lookup"><span data-stu-id="9ec63-286">When preparing data for model building in [Azure Machine Learning Studio](https://studio.azureml.net), you may either decide on hello **SQL query toouse directly in hello Import Data module** or persist hello engineered and sampled data in a new table, which you could use in hello [Import Data][import-data] module with a simple **SELECT * FROM <your\_new\_table\_name>**.</span></span>

<span data-ttu-id="9ec63-287">V této části, které vytvoříme nové hello toohold tabulky vzorků a analyzována data.</span><span class="sxs-lookup"><span data-stu-id="9ec63-287">In this section we will create a new table toohold hello sampled and engineered data.</span></span> <span data-ttu-id="9ec63-288">Příkladem přímý dotaz SQL pro vytváření modelů je součástí hello [zkoumání dat a funkce Engineering v systému SQL Server](#dbexplore) části.</span><span class="sxs-lookup"><span data-stu-id="9ec63-288">An example of a direct SQL query for model building is provided in hello [Data Exploration and Feature Engineering in SQL Server](#dbexplore) section.</span></span>

#### <a name="create-a-sample-table-and-populate-with-1-of-hello-joined-tables-drop-table-first-if-it-exists"></a><span data-ttu-id="9ec63-289">Vytvoření ukázkové tabulky a vyplnit hello připojený tabulek: % 1.</span><span class="sxs-lookup"><span data-stu-id="9ec63-289">Create a Sample Table and Populate with 1% of hello Joined Tables.</span></span> <span data-ttu-id="9ec63-290">Pokud existuje vyřaďte první tabulky.</span><span class="sxs-lookup"><span data-stu-id="9ec63-290">Drop Table First if it Exists.</span></span>
<span data-ttu-id="9ec63-291">V této části jsme připojit hello tabulky **nyctaxi\_cestě** a **nyctaxi\_tarif**, extrahujte náhodného vzorku 1 %, a zachovat data hello vzorkovat nový název tabulky  **nyctaxi\_jeden\_procent**:</span><span class="sxs-lookup"><span data-stu-id="9ec63-291">In this section, we join hello tables **nyctaxi\_trip** and **nyctaxi\_fare**, extract a 1% random sample, and persist hello sampled data in a new table name **nyctaxi\_one\_percent**:</span></span>

    cursor = conn.cursor()

    drop_table_if_exists = '''
        IF OBJECT_ID('nyctaxi_one_percent', 'U') IS NOT NULL DROP TABLE nyctaxi_one_percent
    '''

    nyctaxi_one_percent_insert = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount
        INTO nyctaxi_one_percent
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (1 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
        AND   pickup_longitude <> '0' AND dropoff_longitude <> '0'
    '''

    cursor.execute(drop_table_if_exists)
    cursor.execute(nyctaxi_one_percent_insert)
    cursor.commit()

### <a name="data-exploration-using-sql-queries-in-ipython-notebook"></a><span data-ttu-id="9ec63-292">Zkoumání dat pomocí dotazů SQL v poznámkovém bloku IPython</span><span class="sxs-lookup"><span data-stu-id="9ec63-292">Data Exploration using SQL Queries in IPython Notebook</span></span>
<span data-ttu-id="9ec63-293">V této části nám prozkoumat distribuce dat pomocí hello 1 % vzorků dat, který je uchován v hello nové tabulky, kterou jsme vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="9ec63-293">In this section, we explore data distributions using hello 1% sampled data which is persisted in hello new table we created above.</span></span> <span data-ttu-id="9ec63-294">Všimněte si, že podobné explorations je možné provést pomocí původní tabulky hello, volitelně pomocí **TABLESAMPLE** toolimit hello zkoumání ukázku nebo omezením hello výsledků tooa zadané časové období pomocí hello **vyzvednutí \_data a času** oddíly, jak je ukázáno v hello [zkoumání dat a funkce Engineering v systému SQL Server](#dbexplore) části.</span><span class="sxs-lookup"><span data-stu-id="9ec63-294">Note that similar explorations can be performed using hello original tables, optionally using **TABLESAMPLE** toolimit hello exploration sample or by limiting hello results tooa given time period using hello **pickup\_datetime** partitions, as illustrated in hello [Data Exploration and Feature Engineering in SQL Server](#dbexplore) section.</span></span>

#### <a name="exploration-daily-distribution-of-trips"></a><span data-ttu-id="9ec63-295">Zkoumání: Denní distribuce služebních cest</span><span class="sxs-lookup"><span data-stu-id="9ec63-295">Exploration: Daily distribution of trips</span></span>
    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a><span data-ttu-id="9ec63-296">Zkoumání: Distribuční cesty za Medailon</span><span class="sxs-lookup"><span data-stu-id="9ec63-296">Exploration: Trip distribution per medallion</span></span>
    query = '''
        SELECT medallion,count(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

### <a name="feature-generation-using-sql-queries-in-ipython-notebook"></a><span data-ttu-id="9ec63-297">Funkce generování pomocí dotazů SQL v poznámkovém bloku IPython</span><span class="sxs-lookup"><span data-stu-id="9ec63-297">Feature Generation Using SQL Queries in IPython Notebook</span></span>
<span data-ttu-id="9ec63-298">V této části vygenerujeme nové štítky a funkce přímo pomocí dotazů SQL, operační v tabulce 1 % ukázka hello jsme vytvořili v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="9ec63-298">In this section we will generate new labels and features directly using SQL queries, operating on hello 1% sample table we created in hello previous section.</span></span>

#### <a name="label-generation-generate-class-labels"></a><span data-ttu-id="9ec63-299">Popisek generování: Generování popisků – třída</span><span class="sxs-lookup"><span data-stu-id="9ec63-299">Label Generation: Generate Class Labels</span></span>
<span data-ttu-id="9ec63-300">V následujícím příkladu hello vygeneruje dvě sady popisky toouse pro modelování:</span><span class="sxs-lookup"><span data-stu-id="9ec63-300">In hello following example, we generate two sets of labels toouse for modeling:</span></span>

1. <span data-ttu-id="9ec63-301">Binární třída popisky **vysypávány** (predikci, pokud bude mu udělená tip)</span><span class="sxs-lookup"><span data-stu-id="9ec63-301">Binary Class Labels **tipped** (predicting if a tip will be given)</span></span>
2. <span data-ttu-id="9ec63-302">Více třídami popisky **tip\_třída** (predikci hello popisek přihrádku nebo rozsah)</span><span class="sxs-lookup"><span data-stu-id="9ec63-302">Multiclass Labels **tip\_class** (predicting hello tip bin or range)</span></span>
   
        nyctaxi_one_percent_add_col = '''
            ALTER TABLE nyctaxi_one_percent ADD tipped bit, tip_class int
        '''
   
        cursor.execute(nyctaxi_one_percent_add_col)
        cursor.commit()
   
        nyctaxi_one_percent_update_col = '''
            UPDATE nyctaxi_one_percent
            SET
               tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
               tip_class = CASE WHEN (tip_amount = 0) THEN 0
                                WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                                WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                                WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                                ELSE 4
                            END
        '''
   
        cursor.execute(nyctaxi_one_percent_update_col)
        cursor.commit()

#### <a name="feature-engineering-count-features-for-categorical-columns"></a><span data-ttu-id="9ec63-303">Funkce inženýrství: Počet funkcí pro kategorií sloupce</span><span class="sxs-lookup"><span data-stu-id="9ec63-303">Feature Engineering: Count Features for Categorical Columns</span></span>
<span data-ttu-id="9ec63-304">Tento příklad transformuje pole kategorií na číselné pole nahrazením každou kategorii hello počet jeho výskytů v datech hello.</span><span class="sxs-lookup"><span data-stu-id="9ec63-304">This example transforms a categorical field into a numeric field by replacing each category with hello count of its occurrences in hello data.</span></span>

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD cmt_count int, vts_count int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B AS
        (
            SELECT medallion, hack_license,
                SUM(CASE WHEN vendor_id = 'cmt' THEN 1 ELSE 0 END) AS cmt_count,
                SUM(CASE WHEN vendor_id = 'vts' THEN 1 ELSE 0 END) AS vts_count
            FROM nyctaxi_one_percent
            GROUP BY medallion, hack_license
        )

        UPDATE nyctaxi_one_percent
        SET nyctaxi_one_percent.cmt_count = B.cmt_count,
            nyctaxi_one_percent.vts_count = B.vts_count
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion AND A.hack_license = B.hack_license
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-bin-features-for-numerical-columns"></a><span data-ttu-id="9ec63-305">Funkce inženýrství: Funkce Koš pro číselné sloupce</span><span class="sxs-lookup"><span data-stu-id="9ec63-305">Feature Engineering: Bin features for Numerical Columns</span></span>
<span data-ttu-id="9ec63-306">Tento příklad transformuje průběžné číselné pole na rozsahy přednastavené kategorie, tedy transformace číselné pole do pole kategorií.</span><span class="sxs-lookup"><span data-stu-id="9ec63-306">This example transforms a continuous numeric field into preset category ranges, i.e., transform numeric field into a categorical field.</span></span>

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD trip_time_bin int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B(medallion,hack_license,pickup_datetime,trip_time_in_secs, BinNumber ) AS
        (
            SELECT medallion,hack_license,pickup_datetime,trip_time_in_secs,
            NTILE(5) OVER (ORDER BY trip_time_in_secs) AS BinNumber from nyctaxi_one_percent
        )

        UPDATE nyctaxi_one_percent
        SET trip_time_bin = B.BinNumber
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion
        AND A.hack_license = B.hack_license
        AND A.pickup_datetime = B.pickup_datetime
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-extract-location-features-from-decimal-latitudelongitude"></a><span data-ttu-id="9ec63-307">Funkce inženýrství: Extrahování umístění funkce z Decimal zeměpisnou šířku a délku</span><span class="sxs-lookup"><span data-stu-id="9ec63-307">Feature Engineering: Extract Location Features from Decimal Latitude/Longitude</span></span>
<span data-ttu-id="9ec63-308">Tento příklad rozpis hello decimal reprezentace zeměpisnou šířku a zeměpisnou délku pole do více polí oblasti jiné členitosti, například, země, Město, města, blok atd. Všimněte si, že nová hello geo pole nejsou namapované tooactual umístění.</span><span class="sxs-lookup"><span data-stu-id="9ec63-308">This example breaks down hello decimal representation of a latitude and/or longitude field into multiple region fields of different granularity, such as, country, city, town, block, etc. Note that hello new geo-fields are not mapped tooactual locations.</span></span> <span data-ttu-id="9ec63-309">Informace o mapování geocode umístění najdete v tématu [služby Bing Maps REST](https://msdn.microsoft.com/library/ff701710.aspx).</span><span class="sxs-lookup"><span data-stu-id="9ec63-309">For information on mapping geocode locations, see [Bing Maps REST Services](https://msdn.microsoft.com/library/ff701710.aspx).</span></span>

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent
        ADD l1 varchar(6), l2 varchar(3), l3 varchar(3), l4 varchar(3),
            l5 varchar(3), l6 varchar(3), l7 varchar(3)
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        UPDATE nyctaxi_one_percent
        SET l1=round(pickup_longitude,0)
            , l2 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 1 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),1,1) ELSE '0' END     
            , l3 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 2 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),2,1) ELSE '0' END     
            , l4 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 3 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),3,1) ELSE '0' END     
            , l5 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 4 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),4,1) ELSE '0' END     
            , l6 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 5 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),5,1) ELSE '0' END     
            , l7 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 6 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),6,1) ELSE '0' END
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="verify-hello-final-form-of-hello-featurized-table"></a><span data-ttu-id="9ec63-310">Ověření formuláře konečné hello hello featurized tabulky</span><span class="sxs-lookup"><span data-stu-id="9ec63-310">Verify hello final form of hello featurized table</span></span>
    query = '''SELECT TOP 100 * FROM nyctaxi_one_percent'''
    pd.read_sql(query,conn)

<span data-ttu-id="9ec63-311">Snažíme se teď připravena tooproceed toomodel sestavení a nasazení modelu v [Azure Machine Learning](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="9ec63-311">We are now ready tooproceed toomodel building and model deployment in [Azure Machine Learning](https://studio.azureml.net).</span></span> <span data-ttu-id="9ec63-312">Hello data jsou připravena k některé z problémů předpovědi hello identifikuje dříve, konkrétně:</span><span class="sxs-lookup"><span data-stu-id="9ec63-312">hello data is ready for any of hello prediction problems identified earlier, namely:</span></span>

1. <span data-ttu-id="9ec63-313">Binární klasifikace: toopredict, jestli byl tip placené cesty.</span><span class="sxs-lookup"><span data-stu-id="9ec63-313">Binary classification: toopredict whether or not a tip was paid for a trip.</span></span>
2. <span data-ttu-id="9ec63-314">Více třídami klasifikace: rozsah hello toopredict tipu placené, podle toohello dříve definovaných tříd.</span><span class="sxs-lookup"><span data-stu-id="9ec63-314">Multiclass classification: toopredict hello range of tip paid, according toohello previously defined classes.</span></span>
3. <span data-ttu-id="9ec63-315">Úloha regrese: toopredict hello množství tip placené cesty.</span><span class="sxs-lookup"><span data-stu-id="9ec63-315">Regression task: toopredict hello amount of tip paid for a trip.</span></span>  

## <span data-ttu-id="9ec63-316"><a name="mlmodel"></a>Vytváření modelů v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="9ec63-316"><a name="mlmodel"></a>Building Models in Azure Machine Learning</span></span>
<span data-ttu-id="9ec63-317">toobegin hello modelování cvičení, protokolu v prostoru tooyour Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="9ec63-317">toobegin hello modeling exercise, log in tooyour Azure Machine Learning workspace.</span></span> <span data-ttu-id="9ec63-318">Pokud jste ještě nevytvořili machine learning pracovního prostoru, přečtěte si téma [vytvořit pracovní prostor služby Azure Machine Learning](machine-learning-create-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="9ec63-318">If you have not yet created a machine learning workspace, see [Create an Azure Machine Learning workspace](machine-learning-create-workspace.md).</span></span>

1. <span data-ttu-id="9ec63-319">tooget začít s Azure Machine Learning, najdete v části [co je Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)</span><span class="sxs-lookup"><span data-stu-id="9ec63-319">tooget started with Azure Machine Learning, see [What is Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)</span></span>
2. <span data-ttu-id="9ec63-320">Přihlaste se příliš[Azure Machine Learning Studio](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="9ec63-320">Log in too[Azure Machine Learning Studio](https://studio.azureml.net).</span></span>
3. <span data-ttu-id="9ec63-321">Hello Studio Domovská stránka obsahuje širokou řadu informací, videa, kurzy, odkazy toohello odkaz moduly a dalším prostředkům.</span><span class="sxs-lookup"><span data-stu-id="9ec63-321">hello Studio Home page provides a wealth of information, videos, tutorials, links toohello Modules Reference, and other resources.</span></span> <span data-ttu-id="9ec63-322">Další informace o Azure Machine Learning najdete hello [centru dokumentace Azure Machine Learning na](https://azure.microsoft.com/documentation/services/machine-learning/).</span><span class="sxs-lookup"><span data-stu-id="9ec63-322">Fore more information about Azure Machine Learning, consult hello [Azure Machine Learning Documentation Center](https://azure.microsoft.com/documentation/services/machine-learning/).</span></span>

<span data-ttu-id="9ec63-323">Typické výukový experiment sestává z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="9ec63-323">A typical training experiment consists of hello following:</span></span>

1. <span data-ttu-id="9ec63-324">Vytvoření **+ nový** experiment.</span><span class="sxs-lookup"><span data-stu-id="9ec63-324">Create a **+NEW** experiment.</span></span>
2. <span data-ttu-id="9ec63-325">Získání dat tooAzure hello Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="9ec63-325">Get hello data tooAzure Machine Learning.</span></span>
3. <span data-ttu-id="9ec63-326">Předběžně zpracovat, transformace a pracovat s daty hello podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="9ec63-326">Pre-process, transform and manipulate hello data as needed.</span></span>
4. <span data-ttu-id="9ec63-327">Funkce vygenerujte, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="9ec63-327">Generate features as needed.</span></span>
5. <span data-ttu-id="9ec63-328">Rozdělení dat hello na školení, ověření nebo testování datové sady (nebo samostatné datové sady pro každou).</span><span class="sxs-lookup"><span data-stu-id="9ec63-328">Split hello data into training/validation/testing datasets(or have separate datasets for each).</span></span>
6. <span data-ttu-id="9ec63-329">Vyberte jeden nebo více algoritmů strojového učení v závislosti na hello učení toosolve problém.</span><span class="sxs-lookup"><span data-stu-id="9ec63-329">Select one or more machine learning algorithms depending on hello learning problem toosolve.</span></span> <span data-ttu-id="9ec63-330">Například: binární klasifikace, klasifikace více třídami, regresi.</span><span class="sxs-lookup"><span data-stu-id="9ec63-330">E.g., binary classification, multiclass classification, regression.</span></span>
7. <span data-ttu-id="9ec63-331">Cvičení jednoho nebo více modelů použití hello školení datové sady.</span><span class="sxs-lookup"><span data-stu-id="9ec63-331">Train one or more models using hello training dataset.</span></span>
8. <span data-ttu-id="9ec63-332">Stanovení skóre hello ověření datové sady pomocí vyškolení modely hello.</span><span class="sxs-lookup"><span data-stu-id="9ec63-332">Score hello validation dataset using hello trained model(s).</span></span>
9. <span data-ttu-id="9ec63-333">Vyhodnoťte hello modely toocompute hello relevantní metriky pro hello učení problém.</span><span class="sxs-lookup"><span data-stu-id="9ec63-333">Evaluate hello model(s) toocompute hello relevant metrics for hello learning problem.</span></span>
10. <span data-ttu-id="9ec63-334">Vyladění hello modely a vyberte hello nejlepší toodeploy modelu.</span><span class="sxs-lookup"><span data-stu-id="9ec63-334">Fine tune hello model(s) and select hello best model toodeploy.</span></span>

<span data-ttu-id="9ec63-335">V tomto cvičení jsme již prozkoumali a analyzovány hello dat v systému SQL Server a rozhodli tooingest velikost vzorku hello v Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="9ec63-335">In this exercise, we have already explored and engineered hello data in SQL Server, and decided on hello sample size tooingest in Azure Machine Learning.</span></span> <span data-ttu-id="9ec63-336">toobuild jeden nebo více modelů předpovědi hello jsme se rozhodli:</span><span class="sxs-lookup"><span data-stu-id="9ec63-336">toobuild one or more of hello prediction models we decided:</span></span>

1. <span data-ttu-id="9ec63-337">Získání dat tooAzure hello Machine Learning pomocí hello [importovat Data] [ import-data] modulu, k dispozici v hello **vstupu a výstupu dat** části.</span><span class="sxs-lookup"><span data-stu-id="9ec63-337">Get hello data tooAzure Machine Learning using hello [Import Data][import-data] module, available in hello **Data Input and Output** section.</span></span> <span data-ttu-id="9ec63-338">Další informace najdete v tématu hello [importovat Data] [ import-data] stránka s referencemi modul.</span><span class="sxs-lookup"><span data-stu-id="9ec63-338">For more information, see hello [Import Data][import-data] module reference page.</span></span>
   
    ![Azure Machine Learning Import dat][17]
2. <span data-ttu-id="9ec63-340">Vyberte **Azure SQL Database** jako hello **zdroj dat** v hello **vlastnosti** panelu.</span><span class="sxs-lookup"><span data-stu-id="9ec63-340">Select **Azure SQL Database** as hello **Data source** in hello **Properties** panel.</span></span>
3. <span data-ttu-id="9ec63-341">Zadejte název DNS hello databáze v hello **název databázového serveru** pole.</span><span class="sxs-lookup"><span data-stu-id="9ec63-341">Enter hello database DNS name in hello **Database server name** field.</span></span> <span data-ttu-id="9ec63-342">Formát:`tcp:<your_virtual_machine_DNS_name>,1433`</span><span class="sxs-lookup"><span data-stu-id="9ec63-342">Format: `tcp:<your_virtual_machine_DNS_name>,1433`</span></span>
4. <span data-ttu-id="9ec63-343">Zadejte hello **název databáze** v odpovídající pole hello.</span><span class="sxs-lookup"><span data-stu-id="9ec63-343">Enter hello **Database name** in hello corresponding field.</span></span>
5. <span data-ttu-id="9ec63-344">Zadejte hello **uživatelské jméno SQL** v hello ** aqccount uživatelského jména a hesla hello v hello **heslo uživatelského účtu serveru**.</span><span class="sxs-lookup"><span data-stu-id="9ec63-344">Enter hello **SQL user name** in hello **Server user aqccount name, and hello password in hello **Server user account password**.</span></span>
6. <span data-ttu-id="9ec63-345">Zkontrolujte **přijmout některý z certifikátů serveru** možnost.</span><span class="sxs-lookup"><span data-stu-id="9ec63-345">Check **Accept any server certificate** option.</span></span>
7. <span data-ttu-id="9ec63-346">V hello **databázový dotaz** upravit textová oblast, vložte hello dotazu, který extrahuje hello potřeby databáze pole (včetně všech počítané pole, jako je popisky hello) a dolů – ukázky velikost vzorku toohello požadovaných dat hello.</span><span class="sxs-lookup"><span data-stu-id="9ec63-346">In hello **Database query** edit text area, paste hello query which extracts hello necessary database fields (including any computed fields such as hello labels) and down samples hello data toohello desired sample size.</span></span>

<span data-ttu-id="9ec63-347">Hello obrázek je příkladem binární klasifikace experimentů, čtení dat přímo z databáze systému SQL Server hello.</span><span class="sxs-lookup"><span data-stu-id="9ec63-347">An example of a binary classification experiment reading data directly from hello SQL Server database is in hello figure below.</span></span> <span data-ttu-id="9ec63-348">Podobně jako experimenty konstruovat pro více třídami klasifikaci a regresní problémy.</span><span class="sxs-lookup"><span data-stu-id="9ec63-348">Similar experiments can be constructed for multiclass classification and regression problems.</span></span>

![Azure Machine Learning Train][10]

> [!IMPORTANT]
> <span data-ttu-id="9ec63-350">V hello modelování extrakce dat a vzorkuje příklady dotazu uvedené v předchozí části **všech popisků hello tři modelování cvičení jsou zahrnuty v dotazu hello**.</span><span class="sxs-lookup"><span data-stu-id="9ec63-350">In hello modeling data extraction and sampling query examples provided in previous sections, **all labels for hello three modeling exercises are included in hello query**.</span></span> <span data-ttu-id="9ec63-351">Důležitým krokem (povinné) v každé z hello modelování cvičení je příliš**vyloučit** hello nepotřebné štítky pro hello další dva problémy a všemi ostatními **cíle nevracení**.</span><span class="sxs-lookup"><span data-stu-id="9ec63-351">An important (required) step in each of hello modeling exercises is too**exclude** hello unnecessary labels for hello other two problems, and any other **target leaks**.</span></span> <span data-ttu-id="9ec63-352">Pro například při použití binární klasifikace, používat hello popisek **vysypávány** a vyloučit hello pole **tip\_– třída**, **tip\_velikost**, a **celkový\_velikost**.</span><span class="sxs-lookup"><span data-stu-id="9ec63-352">For e.g., when using binary classification, use hello label **tipped** and exclude hello fields **tip\_class**, **tip\_amount**, and **total\_amount**.</span></span> <span data-ttu-id="9ec63-353">Hello druhé jsou placené cíl nevracení vzhledem k tomu, že implikují hello tip.</span><span class="sxs-lookup"><span data-stu-id="9ec63-353">hello latter are target leaks since they imply hello tip paid.</span></span>
> 
> <span data-ttu-id="9ec63-354">tooexclude nepotřebných sloupců a/nebo nevracení cíl, můžete použít hello [výběr sloupců v datové sadě] [ select-columns] modulu nebo hello [upravit Metadata] [ edit-metadata].</span><span class="sxs-lookup"><span data-stu-id="9ec63-354">tooexclude unnecessary columns and/or target leaks, you may use hello [Select Columns in Dataset][select-columns] module or hello [Edit Metadata][edit-metadata].</span></span> <span data-ttu-id="9ec63-355">Další informace najdete v tématu [výběr sloupců v datové sadě] [ select-columns] a [upravit Metadata] [ edit-metadata] referenční stránky.</span><span class="sxs-lookup"><span data-stu-id="9ec63-355">For more information, see [Select Columns in Dataset][select-columns] and [Edit Metadata][edit-metadata] reference pages.</span></span>
> 
> 

## <span data-ttu-id="9ec63-356"><a name="mldeploy"></a>Nasazování modelů v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="9ec63-356"><a name="mldeploy"></a>Deploying Models in Azure Machine Learning</span></span>
<span data-ttu-id="9ec63-357">Pokud váš model je připraven, můžete snadno nasadit ho jako webovou službu přímo z hello experimentu.</span><span class="sxs-lookup"><span data-stu-id="9ec63-357">When your model is ready, you can easily deploy it as a web service directly from hello experiment.</span></span> <span data-ttu-id="9ec63-358">Další informace o nasazení webové služby Azure Machine Learning najdete v tématu [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="9ec63-358">For more information about deploying Azure Machine Learning web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

<span data-ttu-id="9ec63-359">toodeploy novou webovou službu, musíte:</span><span class="sxs-lookup"><span data-stu-id="9ec63-359">toodeploy a new web service, you need to:</span></span>

1. <span data-ttu-id="9ec63-360">Vytvoření experimentu vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="9ec63-360">Create a scoring experiment.</span></span>
2. <span data-ttu-id="9ec63-361">Nasaďte hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="9ec63-361">Deploy hello web service.</span></span>

<span data-ttu-id="9ec63-362">toocreate vyhodnocování experimentovat z **dokončeno** cvičení experiment, klikněte na tlačítko **vytvořit vyhodnocování EXPERIMENTOVAT** akce panelu nižší hello.</span><span class="sxs-lookup"><span data-stu-id="9ec63-362">toocreate a scoring experiment from a **Finished** training experiment, click **CREATE SCORING EXPERIMENT** in hello lower action bar.</span></span>

![Výpočet skóre Azure][18]

<span data-ttu-id="9ec63-364">Azure Machine Learning se pokusí toocreate vyhodnocování experimentu podle hello součástí hello výukový experiment.</span><span class="sxs-lookup"><span data-stu-id="9ec63-364">Azure Machine Learning will attempt toocreate a scoring experiment based on hello components of hello training experiment.</span></span> <span data-ttu-id="9ec63-365">Konkrétně se:</span><span class="sxs-lookup"><span data-stu-id="9ec63-365">In particular, it will:</span></span>

1. <span data-ttu-id="9ec63-366">Uložit hello trained model a odeberte hello modelu školení moduly.</span><span class="sxs-lookup"><span data-stu-id="9ec63-366">Save hello trained model and remove hello model training modules.</span></span>
2. <span data-ttu-id="9ec63-367">Identifikovat logickou **vstupní port** toorepresent hello očekávána vstupní data schématu.</span><span class="sxs-lookup"><span data-stu-id="9ec63-367">Identify a logical **input port** toorepresent hello expected input data schema.</span></span>
3. <span data-ttu-id="9ec63-368">Identifikovat logickou **výstupní port** toorepresent hello očekávané webové služby výstupního schématu.</span><span class="sxs-lookup"><span data-stu-id="9ec63-368">Identify a logical **output port** toorepresent hello expected web service output schema.</span></span>

<span data-ttu-id="9ec63-369">Když hello vyhodnocování experimentu je vytvořen, zkontrolujte jej a podle potřeby upravte.</span><span class="sxs-lookup"><span data-stu-id="9ec63-369">When hello scoring experiment is created, review it and adjust as needed.</span></span> <span data-ttu-id="9ec63-370">Typické úpravu je tooreplace hello vstupní datové sady nebo dotaz s jedním, který vyloučí popisek pole, protože tyto nebudete mít k dispozici, když je volána hello služby.</span><span class="sxs-lookup"><span data-stu-id="9ec63-370">A typical adjustment is tooreplace hello input dataset and/or query with one which excludes label fields, as these will not be available when hello service is called.</span></span> <span data-ttu-id="9ec63-371">Je také že vhodné tooreduce hello velikost hello vstupní datové sady nebo tooa několik záznamů, akorát tooindicate hello vstupní schéma.</span><span class="sxs-lookup"><span data-stu-id="9ec63-371">It is also a good practice tooreduce hello size of hello input dataset and/or query tooa few records, just enough tooindicate hello input schema.</span></span> <span data-ttu-id="9ec63-372">Pro hello výstupní port, je běžné tooexclude všechny vstupní pole a obsahovat jenom hello **popisky vyhodnocení** a **skóre pro Magnitudu Pravděpodobnostech** v hello výstup pomocí hello [výběr sloupců v datové sadě ] [ select-columns] modulu.</span><span class="sxs-lookup"><span data-stu-id="9ec63-372">For hello output port, it is common tooexclude all input fields and only include hello **Scored Labels** and **Scored Probabilities** in hello output using hello [Select Columns in Dataset][select-columns] module.</span></span>

<span data-ttu-id="9ec63-373">Ukázka vyhodnocování experimentu je hello obrázek.</span><span class="sxs-lookup"><span data-stu-id="9ec63-373">A sample scoring experiment is in hello figure below.</span></span> <span data-ttu-id="9ec63-374">Jakmile budete připraveni toodeploy, klikněte na tlačítko hello **publikování webové služby** tlačítka na dolním panelu akcí hello.</span><span class="sxs-lookup"><span data-stu-id="9ec63-374">When ready toodeploy, click hello **PUBLISH WEB SERVICE** button in hello lower action bar.</span></span>

![Azure Machine Learning publikování][11]

<span data-ttu-id="9ec63-376">toorecap, v tomto kurzu návodu jste vytvořili prostředí vědecké účely dat Azure, práce s velké veřejné datové sady všechny způsob hello z dat pořízení toomodel školení a nasazení webové služby Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="9ec63-376">toorecap, in this walkthrough tutorial, you have created an Azure data science environment, worked with a large public dataset all hello way from data acquisition toomodel training and deploying of an Azure Machine Learning web service.</span></span>

### <a name="license-information"></a><span data-ttu-id="9ec63-377">Informace o licenci</span><span class="sxs-lookup"><span data-stu-id="9ec63-377">License Information</span></span>
<span data-ttu-id="9ec63-378">Tento návod ukázka a jeho doplňujícími skripty a IPython notebook(s) sdílí Microsoft v rámci licencí MIT hello.</span><span class="sxs-lookup"><span data-stu-id="9ec63-378">This sample walkthrough and its accompanying scripts and IPython notebook(s) are shared by Microsoft under hello MIT license.</span></span> <span data-ttu-id="9ec63-379">Zkontrolujte prosím soubor LICENSE.txt hello v adresáři hello hello ukázkový kód na Githubu další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="9ec63-379">Please check hello LICENSE.txt file in in hello directory of hello sample code on GitHub for more details.</span></span>

### <a name="references"></a><span data-ttu-id="9ec63-380">Odkazy</span><span class="sxs-lookup"><span data-stu-id="9ec63-380">References</span></span>
<span data-ttu-id="9ec63-381">• [Stránce pro stažení Andrés Monroy NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/)</span><span class="sxs-lookup"><span data-stu-id="9ec63-381">•    [Andrés Monroy NYC Taxi Trips Download Page](http://www.andresmh.com/nyctaxitrips/)</span></span>  
<span data-ttu-id="9ec63-382">• [FOILing NYC Taxi cestě dat podle Whong Jan](http://chriswhong.com/open-data/foil_nyc_taxi/) </span><span class="sxs-lookup"><span data-stu-id="9ec63-382">•    [FOILing NYC’s Taxi Trip Data by Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/) </span></span>  
<span data-ttu-id="9ec63-383">• [NYC taxíkem a Limousine Komise výzkum a statistiky](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span><span class="sxs-lookup"><span data-stu-id="9ec63-383">•    [NYC Taxi and Limousine Commission Research and Statistics](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span></span>

[1]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sql-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sql-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sql-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sql-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sql-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sql-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sql-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sql-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sql-walkthrough/amlscoring.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
