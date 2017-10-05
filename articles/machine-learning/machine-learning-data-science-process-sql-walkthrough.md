---
title: "Sestavení a nasazení modelu strojového učení pomocí systému SQL Server na virtuálním počítači Azure | Microsoft Docs"
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
ms.openlocfilehash: 6c5361c7e47209c8eb4d5630b44b3dcfeedeaf01
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="the-team-data-science-process-in-action-using-sql-server"></a><span data-ttu-id="2bd41-103">Proces Team dat. vědecké účely v akci: pomocí SQL serveru</span><span class="sxs-lookup"><span data-stu-id="2bd41-103">The Team Data Science Process in action: using SQL Server</span></span>
<span data-ttu-id="2bd41-104">V tomto kurzu vás provede procesem vytváření a nasazování modelu strojového učení pomocí systému SQL Server a veřejně dostupné datové sady – [NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/) datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="2bd41-104">In this tutorial, you walk through the process of building and deploying a machine learning model using SQL Server and a publicly available dataset -- the [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset.</span></span> <span data-ttu-id="2bd41-105">Postup dodržovat standardní data vědecké účely pracovního postupu: ingestování a zkoumat data, pracovníka funkce pro usnadnění učení se pak sestavení a nasazení modelu.</span><span class="sxs-lookup"><span data-stu-id="2bd41-105">The procedure follows a standard data science workflow: ingest and explore the data, engineer features to facilitate learning, then build and deploy a model.</span></span>

## <span data-ttu-id="2bd41-106"><a name="dataset"></a>NYC taxíkem cest v popis datové sady</span><span class="sxs-lookup"><span data-stu-id="2bd41-106"><a name="dataset"></a>NYC Taxi Trips Dataset Description</span></span>
<span data-ttu-id="2bd41-107">Data NYC taxíkem cesty je přibližně 20GB komprimovaných souborů CSV (nekomprimovaným ~ 48GB), skládající se z více než 173 milionů jednotlivých cest a tarify placené pro každou cestu.</span><span class="sxs-lookup"><span data-stu-id="2bd41-107">The NYC Taxi Trip data is about 20GB of compressed CSV files (~48GB uncompressed), comprising more than 173 million individual trips and the fares paid for each trip.</span></span> <span data-ttu-id="2bd41-108">Každý záznam cestě zahrnuje číslo licence vyzvednutí a odkládací umístění a čas, anonymizovaná hackerský (ovladač) a číslo Medailon (taxi na jedinečné id).</span><span class="sxs-lookup"><span data-stu-id="2bd41-108">Each trip record includes the pickup and drop-off location and time, anonymized hack (driver's) license number and medallion (taxi’s unique id) number.</span></span> <span data-ttu-id="2bd41-109">Data obsahuje všechny služebních cest v roku 2013 a je dostupné pro každý měsíc následující dvě datové sady:</span><span class="sxs-lookup"><span data-stu-id="2bd41-109">The data covers all trips in the year 2013 and is provided in the following two datasets for each month:</span></span>

1. <span data-ttu-id="2bd41-110">'trip_data' CSV obsahuje podrobnosti o cestě, například na počtu cestujících, vyzvednutí a dropoff body, doba trvání cesty a délka cesty.</span><span class="sxs-lookup"><span data-stu-id="2bd41-110">The 'trip_data' CSV contains trip details, such as number of passengers, pickup and dropoff points, trip duration, and trip length.</span></span> <span data-ttu-id="2bd41-111">Tady je několik ukázkových záznamů:</span><span class="sxs-lookup"><span data-stu-id="2bd41-111">Here are a few sample records:</span></span>
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. <span data-ttu-id="2bd41-112">Trip_fare CSV obsahující podrobnosti o tarif placené pro každou cestu, například typ platby, velikost tarif, příplatek a daně, tipy a mýtné, a celkovou velikost placené.</span><span class="sxs-lookup"><span data-stu-id="2bd41-112">The 'trip_fare' CSV contains details of the fare paid for each trip, such as payment type, fare amount, surcharge and taxes, tips and tolls, and the total amount paid.</span></span> <span data-ttu-id="2bd41-113">Tady je několik ukázkových záznamů:</span><span class="sxs-lookup"><span data-stu-id="2bd41-113">Here are a few sample records:</span></span>
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

<span data-ttu-id="2bd41-114">Jedinečný klíč pro připojení k cestě\_dat a cesty\_tarif se skládá z pole: medailonu, hackerský\_licence a vyzvednutí\_data a času.</span><span class="sxs-lookup"><span data-stu-id="2bd41-114">The unique key to join trip\_data and trip\_fare is composed of the fields: medallion, hack\_licence and pickup\_datetime.</span></span>

## <span data-ttu-id="2bd41-115"><a name="mltasks"></a>Příklady předpovědi úlohy</span><span class="sxs-lookup"><span data-stu-id="2bd41-115"><a name="mltasks"></a>Examples of Prediction Tasks</span></span>
<span data-ttu-id="2bd41-116">Jsme se formulovali tři předpovědi problémů na základě *tip\_velikost*, konkrétně:</span><span class="sxs-lookup"><span data-stu-id="2bd41-116">We will formulate three prediction problems based on the *tip\_amount*, namely:</span></span>

1. <span data-ttu-id="2bd41-117">Binární klasifikace: předpovědět, zda byl tip placené cesty, tj. *tip\_velikost* větší než $0 je pozitivní příklad, při *tip\_velikost* $ 0 je Příklad záporné.</span><span class="sxs-lookup"><span data-stu-id="2bd41-117">Binary classification: Predict whether or not a tip was paid for a trip, i.e. a *tip\_amount* that is greater than $0 is a positive example, while a *tip\_amount* of $0 is a negative example.</span></span>
2. <span data-ttu-id="2bd41-118">Více třídami klasifikace: K předpovědi rozsahu tipu placené pro cestu.</span><span class="sxs-lookup"><span data-stu-id="2bd41-118">Multiclass classification: To predict the range of tip paid for the trip.</span></span> <span data-ttu-id="2bd41-119">Jsme rozdělit *tip\_velikost* do pěti přihrádek nebo třídy:</span><span class="sxs-lookup"><span data-stu-id="2bd41-119">We divide the *tip\_amount* into five bins or classes:</span></span>
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. <span data-ttu-id="2bd41-120">Úloha regrese: K předvídání množství tip placené cesty.</span><span class="sxs-lookup"><span data-stu-id="2bd41-120">Regression task: To predict the amount of tip paid for a trip.</span></span>  

## <span data-ttu-id="2bd41-121"><a name="setup"></a>Nastavení si Azure datové vědy prostředí pro pokročilou analýzu</span><span class="sxs-lookup"><span data-stu-id="2bd41-121"><a name="setup"></a>Setting Up the Azure data science environment for advanced analytics</span></span>
<span data-ttu-id="2bd41-122">Jak vidíte z [plánování vašeho prostředí](machine-learning-data-science-plan-your-environment.md) průvodce, máte několik možností pro práci s datovou sadu NYC taxíkem cest v Azure:</span><span class="sxs-lookup"><span data-stu-id="2bd41-122">As you can see from the [Plan Your Environment](machine-learning-data-science-plan-your-environment.md) guide, there are several options to work with the NYC Taxi Trips dataset in Azure:</span></span>

* <span data-ttu-id="2bd41-123">Práce s data do objektů BLOB služby Azure a modelu v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="2bd41-123">Work with the data in Azure blobs then model in Azure Machine Learning</span></span>
* <span data-ttu-id="2bd41-124">Načíst data do databáze systému SQL Server a modelu v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="2bd41-124">Load the data into a SQL Server database then model in Azure Machine Learning</span></span>

<span data-ttu-id="2bd41-125">V tomto kurzu si předvedeme paralelní hromadného importu dat do SQL Server, který zkoumání dat, funkce technici a dolů vzorkování pomocí SQL Server Management Studio a také pomocí IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="2bd41-125">In this tutorial we will demonstrate parallel bulk import of the data to a SQL Server, data exploration, feature engineering and down sampling using SQL Server Management Studio as well as using IPython Notebook.</span></span> <span data-ttu-id="2bd41-126">[Ukázkové skripty](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) a [poznámkových bloků IPython](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) jsou sdíleny v Githubu.</span><span class="sxs-lookup"><span data-stu-id="2bd41-126">[Sample scripts](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) and [IPython notebooks](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) are shared in GitHub.</span></span> <span data-ttu-id="2bd41-127">Ukázkový IPython Poznámkový blok pro práci s daty v Azure blob je k dispozici také ve stejném umístění.</span><span class="sxs-lookup"><span data-stu-id="2bd41-127">A sample IPython notebook to work with the data in Azure blobs is also available in the same location.</span></span>

<span data-ttu-id="2bd41-128">Nastavení prostředí vědecké zpracování dat Azure:</span><span class="sxs-lookup"><span data-stu-id="2bd41-128">To set up your Azure Data Science environment:</span></span>

1. [<span data-ttu-id="2bd41-129">Vytvoření účtu úložiště</span><span class="sxs-lookup"><span data-stu-id="2bd41-129">Create a storage account</span></span>](../storage/common/storage-create-storage-account.md)
2. [<span data-ttu-id="2bd41-130">Vytvořit pracovní prostor služby Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="2bd41-130">Create an Azure Machine Learning workspace</span></span>](machine-learning-create-workspace.md)
3. <span data-ttu-id="2bd41-131">[Zřízení virtuálního počítače s datové vědy](machine-learning-data-science-setup-sql-server-virtual-machine.md), který poskytuje systému SQL Server a server služby IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="2bd41-131">[Provision a Data Science Virtual Machine](machine-learning-data-science-setup-sql-server-virtual-machine.md), which provides a SQL Server and an IPython Notebook server.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="2bd41-132">Ukázkové skripty a poznámkových bloků IPython bude stažen do virtuálního počítače vědecké zpracování dat během procesu instalace.</span><span class="sxs-lookup"><span data-stu-id="2bd41-132">The sample scripts and IPython notebooks will be downloaded to your Data Science virtual machine during the setup process.</span></span> <span data-ttu-id="2bd41-133">Po dokončení skriptu po instalaci virtuálního počítače, bude mít ukázky knihovna dokumentů Virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="2bd41-133">When the VM post-installation script completes, the samples will be in your VM's Documents library:</span></span>  
   > 
   > * <span data-ttu-id="2bd41-134">Ukázku skripty:`C:\Users\<user_name>\Documents\Data Science Scripts`</span><span class="sxs-lookup"><span data-stu-id="2bd41-134">Sample Scripts: `C:\Users\<user_name>\Documents\Data Science Scripts`</span></span>  
   > * <span data-ttu-id="2bd41-135">Ukázka IPython poznámkových bloků:`C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`</span><span class="sxs-lookup"><span data-stu-id="2bd41-135">Sample IPython Notebooks: `C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`</span></span>  
   >   <span data-ttu-id="2bd41-136">kde `<user_name>` je váš virtuální počítač Windows přihlašovací jméno.</span><span class="sxs-lookup"><span data-stu-id="2bd41-136">where `<user_name>` is your VM's Windows login name.</span></span> <span data-ttu-id="2bd41-137">Bude označujeme ukázkové složky jako **ukázkové skripty** a **poznámkových bloků IPython ukázka**.</span><span class="sxs-lookup"><span data-stu-id="2bd41-137">We will refer to the sample folders as **Sample Scripts** and **Sample IPython Notebooks**.</span></span>
   > 
   > 

<span data-ttu-id="2bd41-138">Na základě velikost datové sady, umístění zdroje dat a vybrané Azure cílovém prostředí, tento scénář je podobná [scénář \#5: velké datové sady v místních souborů cíle systému SQL Server ve virtuálním počítači Azure](machine-learning-data-science-plan-sample-scenarios.md#largelocaltodb).</span><span class="sxs-lookup"><span data-stu-id="2bd41-138">Based on the dataset size, data source location, and the selected Azure target environment, this scenario is similar to [Scenario \#5: Large dataset in a local files, target SQL Server in Azure VM](machine-learning-data-science-plan-sample-scenarios.md#largelocaltodb).</span></span>

## <span data-ttu-id="2bd41-139"><a name="getdata"></a>Získat Data z veřejné zdroje</span><span class="sxs-lookup"><span data-stu-id="2bd41-139"><a name="getdata"></a>Get the Data from Public Source</span></span>
<span data-ttu-id="2bd41-140">Chcete-li získat [NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/) datové sady z veřejného umístění, můžete použít některou z metod popsaných v [přesun dat do a z Azure Blob Storage](machine-learning-data-science-move-azure-blob.md) zkopírovat data do nového virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="2bd41-140">To get the [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset from its public location, you may use any of the methods described in [Move Data to and from Azure Blob Storage](machine-learning-data-science-move-azure-blob.md) to copy the data to your new virtual machine.</span></span>

<span data-ttu-id="2bd41-141">Ke zkopírování dat pomocí nástroje AzCopy:</span><span class="sxs-lookup"><span data-stu-id="2bd41-141">To copy the data using AzCopy:</span></span>

1. <span data-ttu-id="2bd41-142">Přihlaste se k virtuálnímu počítači (VM)</span><span class="sxs-lookup"><span data-stu-id="2bd41-142">Log in to your virtual machine (VM)</span></span>
2. <span data-ttu-id="2bd41-143">Vytvořte nový adresář v datový disk Virtuálního počítače (Poznámka: Nepoužívejte dočasné Disk, který se dodává s virtuální počítač jako datový Disk).</span><span class="sxs-lookup"><span data-stu-id="2bd41-143">Create a new directory in the VM's data disk (Note: Do not use the Temporary Disk which comes with the VM as a Data Disk).</span></span>
3. <span data-ttu-id="2bd41-144">V okně příkazového řádku spusťte následující příkazového řádku Azcopy nahrazení < path_to_data_folder > vaše data složky vytvořené v (2):</span><span class="sxs-lookup"><span data-stu-id="2bd41-144">In a Command Prompt window, run the following Azcopy command line, replacing <path_to_data_folder> with your data folder created in (2):</span></span>
   
        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S
   
    <span data-ttu-id="2bd41-145">Po dokončení AzCopy CSV soubory ZIP celkem 24 (12 pro cestu\_dat a 12 pro cestu\_tarif) by měla být ve složce data.</span><span class="sxs-lookup"><span data-stu-id="2bd41-145">When the AzCopy completes, a total of 24 zipped CSV files (12 for trip\_data and 12 for trip\_fare) should be in the data folder.</span></span>
4. <span data-ttu-id="2bd41-146">Rozbalte stažené soubory.</span><span class="sxs-lookup"><span data-stu-id="2bd41-146">Unzip the downloaded files.</span></span> <span data-ttu-id="2bd41-147">Všimněte si, složky, kde jsou umístěny nekomprimovaných souborů.</span><span class="sxs-lookup"><span data-stu-id="2bd41-147">Note the folder where the uncompressed files reside.</span></span> <span data-ttu-id="2bd41-148">Tato složka se označuje jako < cesta\_k\_data\_soubory\>.</span><span class="sxs-lookup"><span data-stu-id="2bd41-148">This folder will be referred to as the <path\_to\_data\_files\>.</span></span>

## <span data-ttu-id="2bd41-149"><a name="dbload"></a>Hromadný Import dat do databáze serveru SQL</span><span class="sxs-lookup"><span data-stu-id="2bd41-149"><a name="dbload"></a>Bulk Import Data into SQL Server Database</span></span>
<span data-ttu-id="2bd41-150">Je možné zlepšit výkon načítání nebo přenos velkých objemů dat do databáze SQL a následné dotazy pomocí *rozdělena na oddíly tabulky a zobrazení*.</span><span class="sxs-lookup"><span data-stu-id="2bd41-150">The performance of loading/transferring large amounts of data to an SQL database and subsequent queries can be improved by using *Partitioned Tables and Views*.</span></span> <span data-ttu-id="2bd41-151">V této části jsme postupujte podle pokynů v tématu [paralelní hromadný Import pomocí SQL oddílu tabulky dat](machine-learning-data-science-parallel-load-sql-partitioned-tables.md) vytvořit novou databázi a načítání dat do dělené tabulky paralelně.</span><span class="sxs-lookup"><span data-stu-id="2bd41-151">In this section, we will follow the instructions described in [Parallel Bulk Data Import Using SQL Partition Tables](machine-learning-data-science-parallel-load-sql-partitioned-tables.md) to create a new database and load the data into partitioned tables in parallel.</span></span>

1. <span data-ttu-id="2bd41-152">Při přihlášení k virtuálnímu počítači, spusťte **SQL Server Management Studio**.</span><span class="sxs-lookup"><span data-stu-id="2bd41-152">While logged in to your VM, start **SQL Server Management Studio**.</span></span>
2. <span data-ttu-id="2bd41-153">Připojte pomocí ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="2bd41-153">Connect using Windows Authentication.</span></span>
   
    ![Připojení přes SSMS][12]
3. <span data-ttu-id="2bd41-155">Pokud máte ještě změnit režim ověřování systému SQL Server a vytvořit nového uživatele SQL přihlášení, otevřete soubor skriptu s názvem **změnit\_auth.sql** v **ukázkové skripty** složky.</span><span class="sxs-lookup"><span data-stu-id="2bd41-155">If you have not yet changed the SQL Server authentication mode and created a new SQL login user, open the script file named **change\_auth.sql** in the **Sample Scripts** folder.</span></span> <span data-ttu-id="2bd41-156">Změňte výchozí uživatelské jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="2bd41-156">Change the  default user name and password.</span></span> <span data-ttu-id="2bd41-157">Klikněte na tlačítko **! Spuštění** na panelu nástrojů pro spuštění skriptu.</span><span class="sxs-lookup"><span data-stu-id="2bd41-157">Click **!Execute** in the toolbar to run the script.</span></span>
   
    ![Spuštění skriptu][13]
4. <span data-ttu-id="2bd41-159">Ověřte nebo změnit systému SQL Server výchozí složky databáze a protokolu zajistit, které nově vytvořený databáze bude uložen v datový Disk.</span><span class="sxs-lookup"><span data-stu-id="2bd41-159">Verify and/or change the SQL Server default database and log folders to ensure that newly created databases will be stored in a Data Disk.</span></span> <span data-ttu-id="2bd41-160">Virtuální počítač s SQL serverem bitovou kopii, která je optimalizovaná pro datawarehousing zatížení je předem nakonfigurovaný s disky protokolu a data.</span><span class="sxs-lookup"><span data-stu-id="2bd41-160">The SQL Server VM image that is optimized for datawarehousing loads is pre-configured with data and log disks.</span></span> <span data-ttu-id="2bd41-161">Pokud virtuální počítač neobsahuje datový Disk a přidat nové virtuální pevné disky během procesu instalace virtuálních počítačů, změňte výchozí složky takto:</span><span class="sxs-lookup"><span data-stu-id="2bd41-161">If your VM did not include a Data Disk and you added new virtual hard disks during the VM setup process, change the default folders as follows:</span></span>
   
   * <span data-ttu-id="2bd41-162">Klikněte pravým tlačítkem na název serveru SQL Server na levém panelu a klikněte na tlačítko **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="2bd41-162">Right-click the SQL Server name in the left panel and click **Properties**.</span></span>
     
       ![Vlastnosti serveru SQL][14]
   * <span data-ttu-id="2bd41-164">Vyberte **nastavení databáze** z **vybrat stránku** seznamu na levé straně.</span><span class="sxs-lookup"><span data-stu-id="2bd41-164">Select **Database Settings** from the **Select a page** list to the left.</span></span>
   * <span data-ttu-id="2bd41-165">Ověřte nebo změňte **databáze výchozí umístění** k **datový Disk** umístění podle vaší volby.</span><span class="sxs-lookup"><span data-stu-id="2bd41-165">Verify and/or change the **Database default locations** to the **Data Disk** locations of your choice.</span></span> <span data-ttu-id="2bd41-166">Toto je, kde jsou umístěny nové databáze, pokud vytvořili pomocí výchozích nastavení umístění.</span><span class="sxs-lookup"><span data-stu-id="2bd41-166">This is where new databases reside if created with the default location settings.</span></span>
     
       ![Výchozí nastavení databáze SQL][15]  
5. <span data-ttu-id="2bd41-168">Chcete-li vytvořit novou databázi a sadu skupin souborů pro uložení dělené tabulky, otevřete ukázkový skript **vytvořit\_db\_default.sql**.</span><span class="sxs-lookup"><span data-stu-id="2bd41-168">To create a new database and a set of filegroups to hold the partitioned tables, open the sample script **create\_db\_default.sql**.</span></span> <span data-ttu-id="2bd41-169">Skript se vytvoří novou databázi s názvem **TaxiNYC** a 12 skupiny souborů ve výchozím umístění data.</span><span class="sxs-lookup"><span data-stu-id="2bd41-169">The script will create a new database named **TaxiNYC** and 12 filegroups in the default data location.</span></span> <span data-ttu-id="2bd41-170">Každou skupinu souborů bude obsahovat jeden měsíc cesty\_dat a cesty\_jízdenky data.</span><span class="sxs-lookup"><span data-stu-id="2bd41-170">Each filegroup will hold one month of trip\_data and trip\_fare data.</span></span> <span data-ttu-id="2bd41-171">Změňte název databáze, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="2bd41-171">Modify the database name, if desired.</span></span> <span data-ttu-id="2bd41-172">Klikněte na tlačítko **! Spuštění** pro spuštění skriptu.</span><span class="sxs-lookup"><span data-stu-id="2bd41-172">Click **!Execute** to run the script.</span></span>
6. <span data-ttu-id="2bd41-173">Dále vytvořte dvě tabulky oddílů, jeden pro cestu\_dat a druhý pro cestu\_tarif.</span><span class="sxs-lookup"><span data-stu-id="2bd41-173">Next, create two partition tables, one for the trip\_data and another for the trip\_fare.</span></span> <span data-ttu-id="2bd41-174">Otevřete ukázkový skript **vytvořit\_oddílů\_table.sql**, který bude:</span><span class="sxs-lookup"><span data-stu-id="2bd41-174">Open the sample script **create\_partitioned\_table.sql**, which will:</span></span>
   
   * <span data-ttu-id="2bd41-175">Vytvořte funkci oddílu k rozdělení dat po měsících.</span><span class="sxs-lookup"><span data-stu-id="2bd41-175">Create a partition function to split the data by month.</span></span>
   * <span data-ttu-id="2bd41-176">Vytvořte schéma oddílu mapování dat každý měsíc do jiné skupiny souborů.</span><span class="sxs-lookup"><span data-stu-id="2bd41-176">Create a partition scheme to map each month's data to a different filegroup.</span></span>
   * <span data-ttu-id="2bd41-177">Vytvořte dva dělené tabulky, které jsou mapovány na schéma oddílu: **nyctaxi\_cestě** bude obsahovat cestu\_dat a **nyctaxi\_tarif** bude obsahovat cestě\_jízdenky data.</span><span class="sxs-lookup"><span data-stu-id="2bd41-177">Create two partitioned tables mapped to the partition scheme: **nyctaxi\_trip** will hold the trip\_data and **nyctaxi\_fare** will hold the trip\_fare data.</span></span>
     
     <span data-ttu-id="2bd41-178">Klikněte na tlačítko **! Spuštění** spusťte skript a vytvořit dělené tabulky.</span><span class="sxs-lookup"><span data-stu-id="2bd41-178">Click **!Execute** to run the script and create the partitioned tables.</span></span>
7. <span data-ttu-id="2bd41-179">V **ukázkové skripty** složky, existují dva ukázkové skripty prostředí PowerShell poskytuje k předvedení paralelní hromadné importuje dat do tabulek systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2bd41-179">In the **Sample Scripts** folder, there are two sample PowerShell scripts provided to demonstrate parallel bulk imports of data to SQL Server tables.</span></span>
   
   * <span data-ttu-id="2bd41-180">**BCP\_paralelní\_generic.ps1** je obecný skript k paralelní hromadně importovat data do tabulky.</span><span class="sxs-lookup"><span data-stu-id="2bd41-180">**bcp\_parallel\_generic.ps1** is a generic script to parallel bulk import data into a table.</span></span> <span data-ttu-id="2bd41-181">Upravte tento skript nastavit proměnné vstup a cíle, jak je uvedeno v řádcích komentáře ve skriptu.</span><span class="sxs-lookup"><span data-stu-id="2bd41-181">Modify this script to set the input and target variables as indicated in the comment lines in the script.</span></span>
   * <span data-ttu-id="2bd41-182">**BCP\_paralelní\_nyctaxi.ps1** je předem nakonfigurovaný verze obecný skript a může sloužit k načtení obě tabulky pro data NYC taxíkem cest.</span><span class="sxs-lookup"><span data-stu-id="2bd41-182">**bcp\_parallel\_nyctaxi.ps1** is a pre-configured version of the generic script and can be used to to load both tables for the NYC Taxi Trips data.</span></span>  
8. <span data-ttu-id="2bd41-183">Klikněte pravým tlačítkem myši **bcp\_paralelní\_nyctaxi.ps1** název skriptu a klikněte na tlačítko **upravit** a otevře se v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2bd41-183">Right-click the **bcp\_parallel\_nyctaxi.ps1** script name and click **Edit** to open it in PowerShell.</span></span> <span data-ttu-id="2bd41-184">Zkontrolujte přednastavené proměnné a upravte podle název vybrané databáze, složka vstupních dat, cílové složce protokolu a cesty k ukázkové soubory formátu **nyctaxi_trip.xml** a **nyctaxi\_fare.xml** (součástí **ukázkové skripty** složku).</span><span class="sxs-lookup"><span data-stu-id="2bd41-184">Review the preset variables and modify according to your selected database name, input data folder, target log folder, and paths to the  sample format files **nyctaxi_trip.xml** and **nyctaxi\_fare.xml** (provided in the **Sample Scripts** folder).</span></span>
   
    ![Hromadný Import dat][16]
   
    <span data-ttu-id="2bd41-186">Můžete také vybrat režim ověřování, výchozí hodnota je ověřování systému Windows.</span><span class="sxs-lookup"><span data-stu-id="2bd41-186">You may also select the authentication mode, default is Windows Authentication.</span></span> <span data-ttu-id="2bd41-187">Klikněte na zelenou šipku na panelu nástrojů ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="2bd41-187">Click the green arrow in the toolbar to run.</span></span> <span data-ttu-id="2bd41-188">Skript se spustí 24 operace hromadného importu v paralelní, 12 pro každou tabulku oddílů.</span><span class="sxs-lookup"><span data-stu-id="2bd41-188">The script will launch 24 bulk import operations in parallel, 12 for each partitioned table.</span></span> <span data-ttu-id="2bd41-189">Probíhá import dat může sledovat otevřením složky dat SQL Server výchozí výše.</span><span class="sxs-lookup"><span data-stu-id="2bd41-189">You may monitor the data import progress by opening the SQL Server default data folder as set above.</span></span>
9. <span data-ttu-id="2bd41-190">Skript prostředí PowerShell hlásí počáteční a koncový čas.</span><span class="sxs-lookup"><span data-stu-id="2bd41-190">The PowerShell script reports the starting and ending times.</span></span> <span data-ttu-id="2bd41-191">Pokud všechny hromadně importy dokončení, uvede se čas dokončení.</span><span class="sxs-lookup"><span data-stu-id="2bd41-191">When all bulk imports complete, the ending time is reported.</span></span> <span data-ttu-id="2bd41-192">Zkontrolujte cílové složce protokol ověření, že hromadné importy byla úspěšná, tedy žádné chyby hlášené v cílové složce protokolu.</span><span class="sxs-lookup"><span data-stu-id="2bd41-192">Check the target log folder to verify that the bulk imports were successful, i.e., no errors reported in the target log folder.</span></span>
10. <span data-ttu-id="2bd41-193">Vaše databáze je nyní připraven pro zkoumání, technici funkce a další operace podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="2bd41-193">Your database is now ready for exploration, feature engineering, and other operations as desired.</span></span> <span data-ttu-id="2bd41-194">Vzhledem k tomu, že jsou rozděleny do oddílů tabulky podle **vyzvednutí\_data a času** pole, dotazy, které zahrnují **vyzvednutí\_data a času** podmínky v **kde** klauzule výhod schéma oddílu.</span><span class="sxs-lookup"><span data-stu-id="2bd41-194">Since the tables are partitioned according to the **pickup\_datetime** field, queries which include **pickup\_datetime** conditions in the **WHERE** clause will benefit from the partition scheme.</span></span>
11. <span data-ttu-id="2bd41-195">V **SQL Server Management Studio**, prozkoumejte zadaný ukázkový skript **ukázka\_queries.sql**.</span><span class="sxs-lookup"><span data-stu-id="2bd41-195">In **SQL Server Management Studio**, explore the provided sample script **sample\_queries.sql**.</span></span> <span data-ttu-id="2bd41-196">Pokud chcete spustit všechny ukázkové dotazy, označte řádky dotazu a poté klikněte na tlačítko **! Spuštění** na panelu nástrojů.</span><span class="sxs-lookup"><span data-stu-id="2bd41-196">To run any of the sample queries, highlight the query lines then click **!Execute** in the toolbar.</span></span>
12. <span data-ttu-id="2bd41-197">Data odezvy taxíkem NYC je načten do dvou samostatných tabulkách.</span><span class="sxs-lookup"><span data-stu-id="2bd41-197">The NYC Taxi Trips data is loaded in two separate tables.</span></span> <span data-ttu-id="2bd41-198">Pokud chcete zlepšit operace spojení, důrazně doporučujeme k indexu tabulky.</span><span class="sxs-lookup"><span data-stu-id="2bd41-198">To improve join operations, it is highly recommended to index the tables.</span></span> <span data-ttu-id="2bd41-199">Ukázkový skript **vytvořit\_oddílů\_index.sql** vytváří indexy oddílů spojení složený klíč **medailonu, hackerský\_licence a vyzvednutí\_ data a času**.</span><span class="sxs-lookup"><span data-stu-id="2bd41-199">The sample script **create\_partitioned\_index.sql** creates partitioned indexes on the composite join key **medallion, hack\_license, and pickup\_datetime**.</span></span>

## <span data-ttu-id="2bd41-200"><a name="dbexplore"></a>Zkoumání dat a konstruování funkce v systému SQL Server</span><span class="sxs-lookup"><span data-stu-id="2bd41-200"><a name="dbexplore"></a>Data Exploration and Feature Engineering in SQL Server</span></span>
<span data-ttu-id="2bd41-201">V této části, provedeme zkoumání a funkce generování dat přímo v spuštěním dotazů SQL **SQL Server Management Studio** pomocí databázi systému SQL Server vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="2bd41-201">In this section, we will perform data exploration and feature generation by running SQL queries directly in the **SQL Server Management Studio** using the SQL Server database created earlier.</span></span> <span data-ttu-id="2bd41-202">Ukázkový skript s názvem **ukázka\_queries.sql** je součástí **ukázkové skripty** složky.</span><span class="sxs-lookup"><span data-stu-id="2bd41-202">A sample script named **sample\_queries.sql** is provided in the **Sample Scripts** folder.</span></span> <span data-ttu-id="2bd41-203">Upravte skript, který chcete změnit název databáze, pokud se liší od výchozí hodnota: **TaxiNYC**.</span><span class="sxs-lookup"><span data-stu-id="2bd41-203">Modify the script to change the database name, if it is different from the default: **TaxiNYC**.</span></span>

<span data-ttu-id="2bd41-204">V tomto cvičení provedeme následující:</span><span class="sxs-lookup"><span data-stu-id="2bd41-204">In this exercise, we will:</span></span>

* <span data-ttu-id="2bd41-205">Připojení k **SQL Server Management Studio** pomocí buď ověřování systému Windows nebo pomocí ověřování SQL a SQL přihlašovací jméno a heslo.</span><span class="sxs-lookup"><span data-stu-id="2bd41-205">Connect to **SQL Server Management Studio** using either Windows Authentication or using SQL Authentication and the SQL login name and password.</span></span>
* <span data-ttu-id="2bd41-206">Prozkoumejte data distribuce několik polí v různých časových oken.</span><span class="sxs-lookup"><span data-stu-id="2bd41-206">Explore data distributions of a few fields in varying time windows.</span></span>
* <span data-ttu-id="2bd41-207">Prozkoumejte data quality polí zeměpisné šířky a délky.</span><span class="sxs-lookup"><span data-stu-id="2bd41-207">Investigate data quality of the longitude and latitude fields.</span></span>
* <span data-ttu-id="2bd41-208">Generovat binární a více třídami klasifikační štítky na základě **tip\_velikost**.</span><span class="sxs-lookup"><span data-stu-id="2bd41-208">Generate binary and multiclass classification labels based on the **tip\_amount**.</span></span>
* <span data-ttu-id="2bd41-209">Generovat funkce a výpočetní nebo porovnat cestě vzdálenosti.</span><span class="sxs-lookup"><span data-stu-id="2bd41-209">Generate features and compute/compare trip distances.</span></span>
* <span data-ttu-id="2bd41-210">Spojení dvou tabulek a extrahovat z náhodného vzorku, který bude použit k vytvoření modelů.</span><span class="sxs-lookup"><span data-stu-id="2bd41-210">Join the two tables and extract a random sample that will be used to build models.</span></span>

<span data-ttu-id="2bd41-211">Jakmile budete připraveni pokračovat do Azure Machine Learning, můžete se buď:</span><span class="sxs-lookup"><span data-stu-id="2bd41-211">When you are ready to proceed to Azure Machine Learning, you may either:</span></span>  

1. <span data-ttu-id="2bd41-212">Poslední dotaz SQL k extrahování a ukázková data a způsobené kopírováním a vkládáním dotaz přímo do uložit [importovat Data] [ import-data] modulu v Azure Machine Learning, nebo</span><span class="sxs-lookup"><span data-stu-id="2bd41-212">Save the final SQL query to extract and sample the data and copy-paste the query directly into a [Import Data][import-data] module in Azure Machine Learning, or</span></span>
2. <span data-ttu-id="2bd41-213">Zachovat jen Vzorkovaná a inženýrství dat, které máte v úmyslu použít pro model sestavování v nové databáze tabulky a používat nové tabulky v [importovat Data] [ import-data] modulu v Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="2bd41-213">Persist the sampled and engineered data you plan to use for model building in a new database table and use the new table in the [Import Data][import-data] module in Azure Machine Learning.</span></span>

<span data-ttu-id="2bd41-214">V této části jsme se uložit poslední dotaz k extrahování a ukázková data.</span><span class="sxs-lookup"><span data-stu-id="2bd41-214">In this section we will save the final query to extract and sample the data.</span></span> <span data-ttu-id="2bd41-215">Druhá metoda ukazují [zkoumání dat a funkce Engineering v poznámkovém bloku IPython](#ipnb) části.</span><span class="sxs-lookup"><span data-stu-id="2bd41-215">The second method is demonstrated in the [Data Exploration and Feature Engineering in IPython Notebook](#ipnb) section.</span></span>

<span data-ttu-id="2bd41-216">Pro rychlé ověření počtu řádků a sloupců v tabulkách vyplněny dříve paralelní hromadného importu</span><span class="sxs-lookup"><span data-stu-id="2bd41-216">For a quick verification of the number of rows and columns in the tables populated earlier using parallel bulk import,</span></span>

    -- Report number of rows in table nyctaxi_trip without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('nyctaxi_trip')

    -- Report number of columns in table nyctaxi_trip
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = 'nyctaxi_trip'

#### <a name="exploration-trip-distribution-by-medallion"></a><span data-ttu-id="2bd41-217">Zkoumání: Cestě distribuce podle Medailon</span><span class="sxs-lookup"><span data-stu-id="2bd41-217">Exploration: Trip distribution by medallion</span></span>
<span data-ttu-id="2bd41-218">Tento příklad identifikuje Medailon (taxi čísla) s více než 100 služebních cest v daném časovém období.</span><span class="sxs-lookup"><span data-stu-id="2bd41-218">This example identifies the medallion (taxi numbers) with more than 100 trips within a given time period.</span></span> <span data-ttu-id="2bd41-219">Dotaz by využívat přístup dělenou tabulku vzhledem k tomu, že je podmíněno tím schéma oddílů **vyzvednutí\_data a času**.</span><span class="sxs-lookup"><span data-stu-id="2bd41-219">The query would benefit from the partitioned table access since it is conditioned by the partition scheme of **pickup\_datetime**.</span></span> <span data-ttu-id="2bd41-220">Dotazování úplné datové sadě také budou používat oddílů tabulky nebo indexu kontroly.</span><span class="sxs-lookup"><span data-stu-id="2bd41-220">Querying the full dataset will also make use of the partitioned table and/or index scan.</span></span>

    SELECT medallion, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

#### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a><span data-ttu-id="2bd41-221">Zkoumání: Cestě distribuce podle Medailon a hack_license</span><span class="sxs-lookup"><span data-stu-id="2bd41-221">Exploration: Trip distribution by medallion and hack_license</span></span>
    SELECT medallion, hack_license, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

#### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a><span data-ttu-id="2bd41-222">Hodnocení kvality dat: Ověřte záznamy s nesprávné délky a šířky</span><span class="sxs-lookup"><span data-stu-id="2bd41-222">Data Quality Assessment: Verify records with incorrect longitude and/or latitude</span></span>
<span data-ttu-id="2bd41-223">Tento příklad prověří, pokud jakýkoli z pole zeměpisné délky a šířky buď obsahuje neplatnou hodnotu (radián stupňů musí být mezi -90 a 90), nebo (0, 0) souřadnice.</span><span class="sxs-lookup"><span data-stu-id="2bd41-223">This example investigates if any of the longitude and/or latitude fields either contain an invalid value (radian degrees should be between -90 and 90), or have (0, 0) coordinates.</span></span>

    SELECT COUNT(*) FROM nyctaxi_trip
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

#### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a><span data-ttu-id="2bd41-224">Zkoumání: Šikmý vs. Není vysypávány služebních cest distribučního</span><span class="sxs-lookup"><span data-stu-id="2bd41-224">Exploration: Tipped vs. Not Tipped Trips distribution</span></span>
<span data-ttu-id="2bd41-225">Tento příklad vyhledá počet cest, které byly vysypávány oproti není vysypávány v daném časovém období (nebo pokud vztahující se na úplné rok úplné datové sady).</span><span class="sxs-lookup"><span data-stu-id="2bd41-225">This example finds the number of trips that were tipped vs. not tipped in a given time period (or in the full dataset if covering the full year).</span></span> <span data-ttu-id="2bd41-226">Toto rozdělení odráží binární popisek distribuci do později použije pro modelování binární klasifikace.</span><span class="sxs-lookup"><span data-stu-id="2bd41-226">This distribution reflects the binary label distribution to be later used for binary classification modeling.</span></span>

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM nyctaxi_fare
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

#### <a name="exploration-tip-classrange-distribution"></a><span data-ttu-id="2bd41-227">Zkoumání: Třída nebo rozsah distribuční Tip</span><span class="sxs-lookup"><span data-stu-id="2bd41-227">Exploration: Tip Class/Range Distribution</span></span>
<span data-ttu-id="2bd41-228">Tento příklad vypočítá distribuci rozsahy tip v daném časovém období (nebo pokud vztahující se na úplné rok úplné datové sady).</span><span class="sxs-lookup"><span data-stu-id="2bd41-228">This example computes the distribution of tip ranges in a given time period (or in the full dataset if covering the full year).</span></span> <span data-ttu-id="2bd41-229">Toto je distribuce popisek třídy, které se později použije pro modelování více třídami klasifikace.</span><span class="sxs-lookup"><span data-stu-id="2bd41-229">This is the distribution of the label classes that will be used later for multiclass classification modeling.</span></span>

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

#### <a name="exploration-compute-and-compare-trip-distance"></a><span data-ttu-id="2bd41-230">Zkoumání: Výpočetní a porovnání vzdálenost cesty</span><span class="sxs-lookup"><span data-stu-id="2bd41-230">Exploration: Compute and Compare Trip Distance</span></span>
<span data-ttu-id="2bd41-231">Tento příklad převede vyzvednutí a odkládací zeměpisné délky a šířky na SQL geography body, vypočítá pomocí SQL geography body rozdíl vzdálenost cestě a vrátí z náhodného vzorku výsledky pro porovnání.</span><span class="sxs-lookup"><span data-stu-id="2bd41-231">This example converts the pickup and drop-off longitude and latitude to SQL geography points, computes the trip distance using SQL geography points difference, and returns a random sample of the results for comparison.</span></span> <span data-ttu-id="2bd41-232">V příkladu omezí výsledky do platná souřadnice jenom pomocí dotazu hodnocení kvality dat popsané výše.</span><span class="sxs-lookup"><span data-stu-id="2bd41-232">The example limits the results to valid coordinates only using the data quality assessment query covered earlier.</span></span>

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

#### <a name="feature-engineering-in-sql-queries"></a><span data-ttu-id="2bd41-233">Funkce analýzy v dotazy SQL</span><span class="sxs-lookup"><span data-stu-id="2bd41-233">Feature Engineering in SQL Queries</span></span>
<span data-ttu-id="2bd41-234">Popisek generování a zeměpisnou převod zkoumání dotazy mohou sloužit také ke generování popisků nebo funkcích odstraněním počítání části.</span><span class="sxs-lookup"><span data-stu-id="2bd41-234">The label generation and geography conversion exploration queries can also be used to generate labels/features by removing the counting part.</span></span> <span data-ttu-id="2bd41-235">Další funkce engineering SQL příklady jsou součástí [zkoumání dat a funkce Engineering v poznámkovém bloku IPython](#ipnb) části.</span><span class="sxs-lookup"><span data-stu-id="2bd41-235">Additional feature engineering SQL examples are provided in the [Data Exploration and Feature Engineering in IPython Notebook](#ipnb) section.</span></span> <span data-ttu-id="2bd41-236">Je efektivnější spustit funkci generování dotazy na úplné datové sady nebo velké podmnožinu pomocí dotazů SQL, které se spustit přímo na instanci databáze systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2bd41-236">It is more efficient to run the feature generation queries on the full dataset or a large subset of it using SQL queries which run directly on the SQL Server database instance.</span></span> <span data-ttu-id="2bd41-237">Dotazy může být spuštěna v **SQL Server Management Studio**, IPython Poznámkový blok nebo všechny vývoj nástroj/prostředí, které máte přístup k databázi, místně nebo vzdáleně.</span><span class="sxs-lookup"><span data-stu-id="2bd41-237">The queries may be executed in **SQL Server Management Studio**, IPython Notebook or any development tool/environment which can access the database locally or remotely.</span></span>

#### <a name="preparing-data-for-model-building"></a><span data-ttu-id="2bd41-238">Připravují se Data k vytváření modelů</span><span class="sxs-lookup"><span data-stu-id="2bd41-238">Preparing Data for Model Building</span></span>
<span data-ttu-id="2bd41-239">Následující dotaz spojení **nyctaxi\_cestě** a **nyctaxi\_tarif** tabulky, vygeneruje štítek binární klasifikace **vysypávány**, Popisek více třída klasifikace **tip\_třída**a extrahuje z náhodného vzorku 1 % z připojeného k úplné datové sadě.</span><span class="sxs-lookup"><span data-stu-id="2bd41-239">The following query joins the **nyctaxi\_trip** and **nyctaxi\_fare** tables, generates a binary classification label **tipped**, a multi-class classification label **tip\_class**, and extracts a 1% random sample from the full joined dataset.</span></span> <span data-ttu-id="2bd41-240">Tento dotaz můžete zkopírovat a vložit přímo v [Azure Machine Learning Studio](https://studio.azureml.net) [importovat Data] [ import-data] modul pro přijímání přímé dat z databáze serveru SQL instance v Azure.</span><span class="sxs-lookup"><span data-stu-id="2bd41-240">This query can be copied then pasted directly in the [Azure Machine Learning Studio](https://studio.azureml.net) [Import Data][import-data] module for direct data ingestion from the SQL Server database instance in Azure.</span></span> <span data-ttu-id="2bd41-241">Dotaz vyloučí záznamy s nesprávnou (0, 0) souřadnice.</span><span class="sxs-lookup"><span data-stu-id="2bd41-241">The query excludes records with incorrect (0, 0) coordinates.</span></span>

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


## <span data-ttu-id="2bd41-242"><a name="ipnb"></a>Zkoumání dat a funkce technikům v poznámkovém bloku IPython</span><span class="sxs-lookup"><span data-stu-id="2bd41-242"><a name="ipnb"></a>Data Exploration and Feature Engineering in IPython Notebook</span></span>
<span data-ttu-id="2bd41-243">V této části provedeme zkoumání dat a funkce generování pomocí jazyka Python a SQL dotazů v databázi systému SQL Server vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="2bd41-243">In this section, we will perform data exploration and feature generation using both Python and SQL queries against the SQL Server database created earlier.</span></span> <span data-ttu-id="2bd41-244">Ukázka IPython Poznámkový blok s názvem **machine-Learning-data-science-process-sql-story.ipynb** je součástí **poznámkových bloků IPython ukázka** složky.</span><span class="sxs-lookup"><span data-stu-id="2bd41-244">A sample IPython notebook named **machine-Learning-data-science-process-sql-story.ipynb** is provided in the **Sample IPython Notebooks** folder.</span></span> <span data-ttu-id="2bd41-245">Tento poznámkový blok je také k dispozici na [Githubu](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks).</span><span class="sxs-lookup"><span data-stu-id="2bd41-245">This notebook is also available on [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks).</span></span>

<span data-ttu-id="2bd41-246">Doporučené pořadí při práci s velkým objemem dat je následující:</span><span class="sxs-lookup"><span data-stu-id="2bd41-246">The recommended sequence when working with big data is the following:</span></span>

* <span data-ttu-id="2bd41-247">Přečtěte si v malé ukázkové dat do data v paměti rámečku.</span><span class="sxs-lookup"><span data-stu-id="2bd41-247">Read in a small sample of the data into an in-memory data frame.</span></span>
* <span data-ttu-id="2bd41-248">Proveďte některé vizualizace a explorations pomocí jen Vzorkovaná data.</span><span class="sxs-lookup"><span data-stu-id="2bd41-248">Perform some visualizations and explorations using the sampled data.</span></span>
* <span data-ttu-id="2bd41-249">Experimentujte s inženýrství funkce pomocí jen Vzorkovaná data.</span><span class="sxs-lookup"><span data-stu-id="2bd41-249">Experiment with feature engineering using the sampled data.</span></span>
* <span data-ttu-id="2bd41-250">Pro větší zkoumání dat, manipulaci s daty a funkce technikům používejte jazyk Python k vydávání dotazů SQL přímo s databází systému SQL Server ve virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="2bd41-250">For larger data exploration, data manipulation and feature engineering, use Python to issue SQL Queries directly against the SQL Server database in the Azure VM.</span></span>
* <span data-ttu-id="2bd41-251">Rozhodněte, velikost vzorku, který chcete použít pro vytváření modelů Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="2bd41-251">Decide the sample size to use for Azure Machine Learning model building.</span></span>

<span data-ttu-id="2bd41-252">Až budete připraveni pokračovat Azure Machine Learning, můžete se buď:</span><span class="sxs-lookup"><span data-stu-id="2bd41-252">When ready to proceed to Azure Machine Learning, you may either:</span></span>  

1. <span data-ttu-id="2bd41-253">Poslední dotaz SQL k extrahování a ukázková data a způsobené kopírováním a vkládáním dotaz přímo do uložit [importovat Data] [ import-data] modulu v Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="2bd41-253">Save the final SQL query to extract and sample the data and copy-paste the query directly into a [Import Data][import-data] module in Azure Machine Learning.</span></span> <span data-ttu-id="2bd41-254">Tato metoda je znázorněn v [vytváření modelů v Azure Machine Learning](#mlmodel) části.</span><span class="sxs-lookup"><span data-stu-id="2bd41-254">This method is demonstrated in the [Building Models in Azure Machine Learning](#mlmodel) section.</span></span>    
2. <span data-ttu-id="2bd41-255">Zachovat data jen Vzorkovaná a inženýrství máte v úmyslu použít pro model sestavování v nové databázové tabulky, potom použijte novou tabulku v [importovat Data] [ import-data] modulu.</span><span class="sxs-lookup"><span data-stu-id="2bd41-255">Persist the sampled and engineered data you plan to use for model building in a new database table, then use the new table in the [Import Data][import-data] module.</span></span>

<span data-ttu-id="2bd41-256">Následuje několik zkoumání dat, vizualizace dat a funkce technici příklady.</span><span class="sxs-lookup"><span data-stu-id="2bd41-256">The following are a few data exploration, data visualization, and feature engineering examples.</span></span> <span data-ttu-id="2bd41-257">Další příklady najdete v tématu ukázkový poznámkový blok SQL IPython v **poznámkových bloků IPython ukázka** složky.</span><span class="sxs-lookup"><span data-stu-id="2bd41-257">For more examples, see the sample SQL IPython notebook in the **Sample IPython Notebooks** folder.</span></span>

#### <a name="initialize-database-credentials"></a><span data-ttu-id="2bd41-258">Inicializace přihlašovací údaje databáze</span><span class="sxs-lookup"><span data-stu-id="2bd41-258">Initialize Database Credentials</span></span>
<span data-ttu-id="2bd41-259">Inicializace nastavení připojení k databázi v následující proměnné:</span><span class="sxs-lookup"><span data-stu-id="2bd41-259">Initialize your database connection settings in the following variables:</span></span>

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database server>

#### <a name="create-database-connection"></a><span data-ttu-id="2bd41-260">Vytvoření připojení k databázi</span><span class="sxs-lookup"><span data-stu-id="2bd41-260">Create Database Connection</span></span>
    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

#### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a><span data-ttu-id="2bd41-261">Sestava počtu řádků a sloupců v tabulce nyctaxi_trip</span><span class="sxs-lookup"><span data-stu-id="2bd41-261">Report number of rows and columns in table nyctaxi_trip</span></span>
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

* <span data-ttu-id="2bd41-262">Celkový počet řádků = 173179759</span><span class="sxs-lookup"><span data-stu-id="2bd41-262">Total number of rows = 173179759</span></span>  
* <span data-ttu-id="2bd41-263">Celkový počet sloupců = 14</span><span class="sxs-lookup"><span data-stu-id="2bd41-263">Total number of columns = 14</span></span>

#### <a name="read-in-a-small-data-sample-from-the-sql-server-database"></a><span data-ttu-id="2bd41-264">Pro čtení ve vzorku malá data z databáze serveru SQL</span><span class="sxs-lookup"><span data-stu-id="2bd41-264">Read-in a small data sample from the SQL Server Database</span></span>
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
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

<span data-ttu-id="2bd41-265">Doba čtení ukázkové tabulky je 6.492000 sekund</span><span class="sxs-lookup"><span data-stu-id="2bd41-265">Time to read the sample table is 6.492000 seconds</span></span>  
<span data-ttu-id="2bd41-266">Počet řádků a sloupců načíst = (84952, 21)</span><span class="sxs-lookup"><span data-stu-id="2bd41-266">Number of rows and columns retrieved = (84952, 21)</span></span>

#### <a name="descriptive-statistics"></a><span data-ttu-id="2bd41-267">Popisný statistiky</span><span class="sxs-lookup"><span data-stu-id="2bd41-267">Descriptive Statistics</span></span>
<span data-ttu-id="2bd41-268">Teď je připravena k prozkoumání jen Vzorkovaná data.</span><span class="sxs-lookup"><span data-stu-id="2bd41-268">Now are ready to explore the sampled data.</span></span> <span data-ttu-id="2bd41-269">Začneme s prohlížení popisný statistiku pro **cestě\_vzdálenost** (nebo libovolný jiný) polí:</span><span class="sxs-lookup"><span data-stu-id="2bd41-269">We start with looking at descriptive statistics for the **trip\_distance** (or any other) field(s):</span></span>

    df1['trip_distance'].describe()

#### <a name="visualization-box-plot-example"></a><span data-ttu-id="2bd41-270">Vizualizace: Příklad vykreslení pole</span><span class="sxs-lookup"><span data-stu-id="2bd41-270">Visualization: Box Plot Example</span></span>
<span data-ttu-id="2bd41-271">Potom se podíváme na Krabicový pro vzdálenost cesty k vizualizaci quantiles</span><span class="sxs-lookup"><span data-stu-id="2bd41-271">Next we look at the box plot for the trip distance to visualize the quantiles</span></span>

    df1.boxplot(column='trip_distance',return_type='dict')

![Vykreslení #1][1]

#### <a name="visualization-distribution-plot-example"></a><span data-ttu-id="2bd41-273">Vizualizaci: Příklad vykreslení distribuční</span><span class="sxs-lookup"><span data-stu-id="2bd41-273">Visualization: Distribution Plot Example</span></span>
    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Vykreslení #2][2]

#### <a name="visualization-bar-and-line-plots"></a><span data-ttu-id="2bd41-275">Vizualizaci: Panelu a řádku pozemků</span><span class="sxs-lookup"><span data-stu-id="2bd41-275">Visualization: Bar and Line Plots</span></span>
<span data-ttu-id="2bd41-276">V tomto příkladu jsme bin vzdálenost cesty do pěti přihrádek a vizualizace binning výsledků.</span><span class="sxs-lookup"><span data-stu-id="2bd41-276">In this example, we bin the trip distance into five bins and visualize the binning results.</span></span>

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

<span data-ttu-id="2bd41-277">Nemůžeme vykreslení výše uvedené distribuční bin panelu nebo řádek výkresu, jak je uvedeno níže</span><span class="sxs-lookup"><span data-stu-id="2bd41-277">We can plot the above bin distribution in a bar or line plot as below</span></span>

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Vykreslení #3][3]

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Vykreslení #4][4]

#### <a name="visualization-scatterplot-example"></a><span data-ttu-id="2bd41-280">Vizualizaci: Příklad Scatterplot</span><span class="sxs-lookup"><span data-stu-id="2bd41-280">Visualization: Scatterplot Example</span></span>
<span data-ttu-id="2bd41-281">Ukážeme bodové vykreslení mezi **cestě\_čas\_v\_sekundy** a **cestě\_vzdálenost** jestli jsou všechny korelace</span><span class="sxs-lookup"><span data-stu-id="2bd41-281">We show scatter plot between **trip\_time\_in\_secs** and **trip\_distance** to see if there is any correlation</span></span>

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Vykreslení #6][6]

<span data-ttu-id="2bd41-283">Podobně jsme můžete zkontrolovat vztah mezi **míra\_kód** a **cestě\_vzdálenost**.</span><span class="sxs-lookup"><span data-stu-id="2bd41-283">Similarly we can check the relationship between **rate\_code** and **trip\_distance**.</span></span>

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![Vykreslení #8][8]

### <a name="sub-sampling-the-data-in-sql"></a><span data-ttu-id="2bd41-285">Vzorkování dat v systému SQL</span><span class="sxs-lookup"><span data-stu-id="2bd41-285">Sub-Sampling the Data in SQL</span></span>
<span data-ttu-id="2bd41-286">Při přípravě dat pro model vytváření [Azure Machine Learning Studio](https://studio.azureml.net), můžete rozhodnout buď na **dotazu SQL pro použití přímo v modulu importovat Data** nebo zachovat v nové inženýrství a jen Vzorkovaná data tabulky a který můžete použít v [importovat Data] [ import-data] modul s jednoduchou **vyberte * FROM < vaší\_nové\_tabulky\_name >** .</span><span class="sxs-lookup"><span data-stu-id="2bd41-286">When preparing data for model building in [Azure Machine Learning Studio](https://studio.azureml.net), you may either decide on the **SQL query to use directly in the Import Data module** or persist the engineered and sampled data in a new table, which you could use in the [Import Data][import-data] module with a simple **SELECT * FROM <your\_new\_table\_name>**.</span></span>

<span data-ttu-id="2bd41-287">V této části vytvoříme novou tabulku pro uložení dat jen Vzorkovaná a inženýrství.</span><span class="sxs-lookup"><span data-stu-id="2bd41-287">In this section we will create a new table to hold the sampled and engineered data.</span></span> <span data-ttu-id="2bd41-288">Je součástí příkladem přímý dotaz SQL pro vytváření modelů [zkoumání dat a funkce Engineering v systému SQL Server](#dbexplore) části.</span><span class="sxs-lookup"><span data-stu-id="2bd41-288">An example of a direct SQL query for model building is provided in the [Data Exploration and Feature Engineering in SQL Server](#dbexplore) section.</span></span>

#### <a name="create-a-sample-table-and-populate-with-1-of-the-joined-tables-drop-table-first-if-it-exists"></a><span data-ttu-id="2bd41-289">Vytvořit vzorek, tabulky a naplnění 1 % spojené tabulky.</span><span class="sxs-lookup"><span data-stu-id="2bd41-289">Create a Sample Table and Populate with 1% of the Joined Tables.</span></span> <span data-ttu-id="2bd41-290">Pokud existuje vyřaďte první tabulky.</span><span class="sxs-lookup"><span data-stu-id="2bd41-290">Drop Table First if it Exists.</span></span>
<span data-ttu-id="2bd41-291">V této části jsme připojení tabulky **nyctaxi\_cestě** a **nyctaxi\_tarif**, extrahujte náhodného vzorku 1 %, a zachovat jen Vzorkovaná data v názvu nové tabulky  **nyctaxi\_jeden\_procent**:</span><span class="sxs-lookup"><span data-stu-id="2bd41-291">In this section, we join the tables **nyctaxi\_trip** and **nyctaxi\_fare**, extract a 1% random sample, and persist the sampled data in a new table name **nyctaxi\_one\_percent**:</span></span>

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

### <a name="data-exploration-using-sql-queries-in-ipython-notebook"></a><span data-ttu-id="2bd41-292">Zkoumání dat pomocí dotazů SQL v poznámkovém bloku IPython</span><span class="sxs-lookup"><span data-stu-id="2bd41-292">Data Exploration using SQL Queries in IPython Notebook</span></span>
<span data-ttu-id="2bd41-293">V této části nám prozkoumat distribuce dat pomocí 1 % vzorků dat, který je uchován v nové tabulky, kterou jsme vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="2bd41-293">In this section, we explore data distributions using the 1% sampled data which is persisted in the new table we created above.</span></span> <span data-ttu-id="2bd41-294">Všimněte si, že podobné explorations je možné provést pomocí původní tabulky, volitelně pomocí **TABLESAMPLE** omezit zkoumání ukázku nebo omezením výsledky do dané časové období pomocí **vyzvednutí\_data a času** oddíly, jak je znázorněno [zkoumání dat a funkce Engineering v systému SQL Server](#dbexplore) části.</span><span class="sxs-lookup"><span data-stu-id="2bd41-294">Note that similar explorations can be performed using the original tables, optionally using **TABLESAMPLE** to limit the exploration sample or by limiting the results to a given time period using the **pickup\_datetime** partitions, as illustrated in the [Data Exploration and Feature Engineering in SQL Server](#dbexplore) section.</span></span>

#### <a name="exploration-daily-distribution-of-trips"></a><span data-ttu-id="2bd41-295">Zkoumání: Denní distribuce služebních cest</span><span class="sxs-lookup"><span data-stu-id="2bd41-295">Exploration: Daily distribution of trips</span></span>
    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a><span data-ttu-id="2bd41-296">Zkoumání: Distribuční cesty za Medailon</span><span class="sxs-lookup"><span data-stu-id="2bd41-296">Exploration: Trip distribution per medallion</span></span>
    query = '''
        SELECT medallion,count(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

### <a name="feature-generation-using-sql-queries-in-ipython-notebook"></a><span data-ttu-id="2bd41-297">Funkce generování pomocí dotazů SQL v poznámkovém bloku IPython</span><span class="sxs-lookup"><span data-stu-id="2bd41-297">Feature Generation Using SQL Queries in IPython Notebook</span></span>
<span data-ttu-id="2bd41-298">V této části vygenerujeme nové štítky a funkce přímo pomocí dotazů SQL, fungujícími v tabulce 1 % jsme vytvořili v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="2bd41-298">In this section we will generate new labels and features directly using SQL queries, operating on the 1% sample table we created in the previous section.</span></span>

#### <a name="label-generation-generate-class-labels"></a><span data-ttu-id="2bd41-299">Popisek generování: Generování popisků – třída</span><span class="sxs-lookup"><span data-stu-id="2bd41-299">Label Generation: Generate Class Labels</span></span>
<span data-ttu-id="2bd41-300">V následujícím příkladu se vygeneruje dvě sady popisky, které chcete použít pro modelování:</span><span class="sxs-lookup"><span data-stu-id="2bd41-300">In the following example, we generate two sets of labels to use for modeling:</span></span>

1. <span data-ttu-id="2bd41-301">Binární třída popisky **vysypávány** (predikci, pokud bude mu udělená tip)</span><span class="sxs-lookup"><span data-stu-id="2bd41-301">Binary Class Labels **tipped** (predicting if a tip will be given)</span></span>
2. <span data-ttu-id="2bd41-302">Více třídami popisky **tip\_třída** (predikci popisek přihrádku nebo rozsah)</span><span class="sxs-lookup"><span data-stu-id="2bd41-302">Multiclass Labels **tip\_class** (predicting the tip bin or range)</span></span>
   
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

#### <a name="feature-engineering-count-features-for-categorical-columns"></a><span data-ttu-id="2bd41-303">Funkce inženýrství: Počet funkcí pro kategorií sloupce</span><span class="sxs-lookup"><span data-stu-id="2bd41-303">Feature Engineering: Count Features for Categorical Columns</span></span>
<span data-ttu-id="2bd41-304">Tento příklad transformuje pole kategorií na číselné pole nahrazením každou kategorii počet jeho výskytů v datech.</span><span class="sxs-lookup"><span data-stu-id="2bd41-304">This example transforms a categorical field into a numeric field by replacing each category with the count of its occurrences in the data.</span></span>

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

#### <a name="feature-engineering-bin-features-for-numerical-columns"></a><span data-ttu-id="2bd41-305">Funkce inženýrství: Funkce Koš pro číselné sloupce</span><span class="sxs-lookup"><span data-stu-id="2bd41-305">Feature Engineering: Bin features for Numerical Columns</span></span>
<span data-ttu-id="2bd41-306">Tento příklad transformuje průběžné číselné pole na rozsahy přednastavené kategorie, tedy transformace číselné pole do pole kategorií.</span><span class="sxs-lookup"><span data-stu-id="2bd41-306">This example transforms a continuous numeric field into preset category ranges, i.e., transform numeric field into a categorical field.</span></span>

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

#### <a name="feature-engineering-extract-location-features-from-decimal-latitudelongitude"></a><span data-ttu-id="2bd41-307">Funkce inženýrství: Extrahování umístění funkce z Decimal zeměpisnou šířku a délku</span><span class="sxs-lookup"><span data-stu-id="2bd41-307">Feature Engineering: Extract Location Features from Decimal Latitude/Longitude</span></span>
<span data-ttu-id="2bd41-308">Tento příklad rozpis decimal reprezentace zeměpisnou šířku a zeměpisnou délku pole do více polí oblasti jiné členitosti, jako země, Město, města, blok atd. Všimněte si, že nová geo pole nejsou namapované na skutečné umístění.</span><span class="sxs-lookup"><span data-stu-id="2bd41-308">This example breaks down the decimal representation of a latitude and/or longitude field into multiple region fields of different granularity, such as, country, city, town, block, etc. Note that the new geo-fields are not mapped to actual locations.</span></span> <span data-ttu-id="2bd41-309">Informace o mapování geocode umístění najdete v tématu [služby Bing Maps REST](https://msdn.microsoft.com/library/ff701710.aspx).</span><span class="sxs-lookup"><span data-stu-id="2bd41-309">For information on mapping geocode locations, see [Bing Maps REST Services](https://msdn.microsoft.com/library/ff701710.aspx).</span></span>

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

#### <a name="verify-the-final-form-of-the-featurized-table"></a><span data-ttu-id="2bd41-310">Ověření konečné formuláře featurized tabulky</span><span class="sxs-lookup"><span data-stu-id="2bd41-310">Verify the final form of the featurized table</span></span>
    query = '''SELECT TOP 100 * FROM nyctaxi_one_percent'''
    pd.read_sql(query,conn)

<span data-ttu-id="2bd41-311">Snažíme se teď přejít k vytváření modelů a modelu nasazení v [Azure Machine Learning](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="2bd41-311">We are now ready to proceed to model building and model deployment in [Azure Machine Learning](https://studio.azureml.net).</span></span> <span data-ttu-id="2bd41-312">Data je připraven pro některý z identifikované dříve, a to předpovědi problémy:</span><span class="sxs-lookup"><span data-stu-id="2bd41-312">The data is ready for any of the prediction problems identified earlier, namely:</span></span>

1. <span data-ttu-id="2bd41-313">Binární klasifikace: K předvídání, jestli tip byl placené cesty.</span><span class="sxs-lookup"><span data-stu-id="2bd41-313">Binary classification: To predict whether or not a tip was paid for a trip.</span></span>
2. <span data-ttu-id="2bd41-314">Více třídami klasifikace: K předpovědi rozsahu tipu placené podle dříve definovaných tříd.</span><span class="sxs-lookup"><span data-stu-id="2bd41-314">Multiclass classification: To predict the range of tip paid, according to the previously defined classes.</span></span>
3. <span data-ttu-id="2bd41-315">Úloha regrese: K předvídání množství tip placené cesty.</span><span class="sxs-lookup"><span data-stu-id="2bd41-315">Regression task: To predict the amount of tip paid for a trip.</span></span>  

## <span data-ttu-id="2bd41-316"><a name="mlmodel"></a>Vytváření modelů v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="2bd41-316"><a name="mlmodel"></a>Building Models in Azure Machine Learning</span></span>
<span data-ttu-id="2bd41-317">Pokud chcete začít cvičení modelování, přihlaste se do pracovního prostoru Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="2bd41-317">To begin the modeling exercise, log in to your Azure Machine Learning workspace.</span></span> <span data-ttu-id="2bd41-318">Pokud jste ještě nevytvořili machine learning pracovního prostoru, přečtěte si téma [vytvořit pracovní prostor služby Azure Machine Learning](machine-learning-create-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="2bd41-318">If you have not yet created a machine learning workspace, see [Create an Azure Machine Learning workspace](machine-learning-create-workspace.md).</span></span>

1. <span data-ttu-id="2bd41-319">Chcete-li začít s Azure Machine Learning, přečtěte si téma [co je Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)</span><span class="sxs-lookup"><span data-stu-id="2bd41-319">To get started with Azure Machine Learning, see [What is Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)</span></span>
2. <span data-ttu-id="2bd41-320">Přihlaste se k [Azure Machine Learning Studio](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="2bd41-320">Log in to [Azure Machine Learning Studio](https://studio.azureml.net).</span></span>
3. <span data-ttu-id="2bd41-321">Na domovskou stránku Studio poskytuje širokou řadu informací, videa, kurzy, odkazů na odkaz na moduly a dalším prostředkům.</span><span class="sxs-lookup"><span data-stu-id="2bd41-321">The Studio Home page provides a wealth of information, videos, tutorials, links to the Modules Reference, and other resources.</span></span> <span data-ttu-id="2bd41-322">Další informace o Azure Machine Learning, najdete [centru dokumentace Azure Machine Learning na](https://azure.microsoft.com/documentation/services/machine-learning/).</span><span class="sxs-lookup"><span data-stu-id="2bd41-322">Fore more information about Azure Machine Learning, consult the [Azure Machine Learning Documentation Center](https://azure.microsoft.com/documentation/services/machine-learning/).</span></span>

<span data-ttu-id="2bd41-323">Typické výukový experiment sestává z následujících akcí:</span><span class="sxs-lookup"><span data-stu-id="2bd41-323">A typical training experiment consists of the following:</span></span>

1. <span data-ttu-id="2bd41-324">Vytvoření **+ nový** experiment.</span><span class="sxs-lookup"><span data-stu-id="2bd41-324">Create a **+NEW** experiment.</span></span>
2. <span data-ttu-id="2bd41-325">Načíst data do Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="2bd41-325">Get the data to Azure Machine Learning.</span></span>
3. <span data-ttu-id="2bd41-326">Předběžně zpracovat, transformace a manipulovat s daty, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="2bd41-326">Pre-process, transform and manipulate the data as needed.</span></span>
4. <span data-ttu-id="2bd41-327">Funkce vygenerujte, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="2bd41-327">Generate features as needed.</span></span>
5. <span data-ttu-id="2bd41-328">Rozdělení dat do školení, ověření nebo testování datové sady (nebo samostatné datové sady pro každou).</span><span class="sxs-lookup"><span data-stu-id="2bd41-328">Split the data into training/validation/testing datasets(or have separate datasets for each).</span></span>
6. <span data-ttu-id="2bd41-329">Vyberte jeden nebo více algoritmy strojového učení v závislosti na learning problém vyřešit.</span><span class="sxs-lookup"><span data-stu-id="2bd41-329">Select one or more machine learning algorithms depending on the learning problem to solve.</span></span> <span data-ttu-id="2bd41-330">Například: binární klasifikace, klasifikace více třídami, regresi.</span><span class="sxs-lookup"><span data-stu-id="2bd41-330">E.g., binary classification, multiclass classification, regression.</span></span>
7. <span data-ttu-id="2bd41-331">Cvičení jednoho nebo více modelů použití školení datové sady.</span><span class="sxs-lookup"><span data-stu-id="2bd41-331">Train one or more models using the training dataset.</span></span>
8. <span data-ttu-id="2bd41-332">Stanovení skóre datovou sadu ověření pomocí vyškolení modely.</span><span class="sxs-lookup"><span data-stu-id="2bd41-332">Score the validation dataset using the trained model(s).</span></span>
9. <span data-ttu-id="2bd41-333">Vyhodnoťte modely k výpočtu relevantní metriky pro učení problém.</span><span class="sxs-lookup"><span data-stu-id="2bd41-333">Evaluate the model(s) to compute the relevant metrics for the learning problem.</span></span>
10. <span data-ttu-id="2bd41-334">Bez problémů ladit modely a vyberte doporučené modelu pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="2bd41-334">Fine tune the model(s) and select the best model to deploy.</span></span>

<span data-ttu-id="2bd41-335">V tomto cvičení jsme již prozkoumali a analýzou dat v systému SQL Server a rozhodli na velikost vzorku ingestování v Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="2bd41-335">In this exercise, we have already explored and engineered the data in SQL Server, and decided on the sample size to ingest in Azure Machine Learning.</span></span> <span data-ttu-id="2bd41-336">K vytvoření jednoho nebo více modelů předpovědi jsme se rozhodli:</span><span class="sxs-lookup"><span data-stu-id="2bd41-336">To build one or more of the prediction models we decided:</span></span>

1. <span data-ttu-id="2bd41-337">Získat data pomocí Azure Machine Learning [importovat Data] [ import-data] modulu, k dispozici v **vstupu a výstupu dat** části.</span><span class="sxs-lookup"><span data-stu-id="2bd41-337">Get the data to Azure Machine Learning using the [Import Data][import-data] module, available in the **Data Input and Output** section.</span></span> <span data-ttu-id="2bd41-338">Další informace najdete v tématu [importovat Data] [ import-data] stránka s referencemi modul.</span><span class="sxs-lookup"><span data-stu-id="2bd41-338">For more information, see the [Import Data][import-data] module reference page.</span></span>
   
    ![Azure Machine Learning Import dat][17]
2. <span data-ttu-id="2bd41-340">Vyberte **Azure SQL Database** jako **zdroj dat** v **vlastnosti** panelu.</span><span class="sxs-lookup"><span data-stu-id="2bd41-340">Select **Azure SQL Database** as the **Data source** in the **Properties** panel.</span></span>
3. <span data-ttu-id="2bd41-341">Zadejte název DNS databáze v **název databázového serveru** pole.</span><span class="sxs-lookup"><span data-stu-id="2bd41-341">Enter the database DNS name in the **Database server name** field.</span></span> <span data-ttu-id="2bd41-342">Formát:`tcp:<your_virtual_machine_DNS_name>,1433`</span><span class="sxs-lookup"><span data-stu-id="2bd41-342">Format: `tcp:<your_virtual_machine_DNS_name>,1433`</span></span>
4. <span data-ttu-id="2bd41-343">Zadejte **název databáze** v odpovídajícím poli.</span><span class="sxs-lookup"><span data-stu-id="2bd41-343">Enter the **Database name** in the corresponding field.</span></span>
5. <span data-ttu-id="2bd41-344">Zadejte **uživatelské jméno SQL** v ** aqccount uživatelského jména a hesla **heslo uživatelského účtu serveru**.</span><span class="sxs-lookup"><span data-stu-id="2bd41-344">Enter the **SQL user name** in the **Server user aqccount name, and the password in the **Server user account password**.</span></span>
6. <span data-ttu-id="2bd41-345">Zkontrolujte **přijmout některý z certifikátů serveru** možnost.</span><span class="sxs-lookup"><span data-stu-id="2bd41-345">Check **Accept any server certificate** option.</span></span>
7. <span data-ttu-id="2bd41-346">V **databázový dotaz** upravit textová oblast, vložte dotaz, který extrahuje pole potřeby databáze (včetně všech počítané pole, jako je popisků) a nižší ukázky data na požadovanou velikost.</span><span class="sxs-lookup"><span data-stu-id="2bd41-346">In the **Database query** edit text area, paste the query which extracts the necessary database fields (including any computed fields such as the labels) and down samples the data to the desired sample size.</span></span>

<span data-ttu-id="2bd41-347">Příkladem binární klasifikace experimentů, čtení dat přímo z databáze serveru SQL je na obrázku níže.</span><span class="sxs-lookup"><span data-stu-id="2bd41-347">An example of a binary classification experiment reading data directly from the SQL Server database is in the figure below.</span></span> <span data-ttu-id="2bd41-348">Podobně jako experimenty konstruovat pro více třídami klasifikaci a regresní problémy.</span><span class="sxs-lookup"><span data-stu-id="2bd41-348">Similar experiments can be constructed for multiclass classification and regression problems.</span></span>

![Azure Machine Learning Train][10]

> [!IMPORTANT]
> <span data-ttu-id="2bd41-350">V datech modelování extrakce a vzorkuje příklady dotazu zadaný v předchozích částech **všech popisků tři modelování cvičení jsou zahrnuty v dotazu**.</span><span class="sxs-lookup"><span data-stu-id="2bd41-350">In the modeling data extraction and sampling query examples provided in previous sections, **all labels for the three modeling exercises are included in the query**.</span></span> <span data-ttu-id="2bd41-351">Důležitým krokem (povinné) v každé cvičení modelování je **vyloučit** nepotřebné štítky pro dva problémy a všemi ostatními **cíle nevracení**.</span><span class="sxs-lookup"><span data-stu-id="2bd41-351">An important (required) step in each of the modeling exercises is to **exclude** the unnecessary labels for the other two problems, and any other **target leaks**.</span></span> <span data-ttu-id="2bd41-352">Pro například binární klasifikace, používejte návěští **vysypávány** a vyloučit pole **tip\_třída**, **tip\_velikost**a **celkový\_velikost**.</span><span class="sxs-lookup"><span data-stu-id="2bd41-352">For e.g., when using binary classification, use the label **tipped** and exclude the fields **tip\_class**, **tip\_amount**, and **total\_amount**.</span></span> <span data-ttu-id="2bd41-353">K tomu jsou cílové nevracení vzhledem k tomu, že implikují tip placené.</span><span class="sxs-lookup"><span data-stu-id="2bd41-353">The latter are target leaks since they imply the tip paid.</span></span>
> 
> <span data-ttu-id="2bd41-354">Vyloučení nepotřebných sloupců nebo cíle nevracení, můžete použít [výběr sloupců v datové sadě] [ select-columns] modulu nebo [upravit Metadata][edit-metadata].</span><span class="sxs-lookup"><span data-stu-id="2bd41-354">To exclude unnecessary columns and/or target leaks, you may use the [Select Columns in Dataset][select-columns] module or the [Edit Metadata][edit-metadata].</span></span> <span data-ttu-id="2bd41-355">Další informace najdete v tématu [výběr sloupců v datové sadě] [ select-columns] a [upravit Metadata] [ edit-metadata] referenční stránky.</span><span class="sxs-lookup"><span data-stu-id="2bd41-355">For more information, see [Select Columns in Dataset][select-columns] and [Edit Metadata][edit-metadata] reference pages.</span></span>
> 
> 

## <span data-ttu-id="2bd41-356"><a name="mldeploy"></a>Nasazování modelů v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="2bd41-356"><a name="mldeploy"></a>Deploying Models in Azure Machine Learning</span></span>
<span data-ttu-id="2bd41-357">Pokud váš model je připraven, můžete snadno nasadit ho jako webovou službu přímo z experimentu.</span><span class="sxs-lookup"><span data-stu-id="2bd41-357">When your model is ready, you can easily deploy it as a web service directly from the experiment.</span></span> <span data-ttu-id="2bd41-358">Další informace o nasazení webové služby Azure Machine Learning najdete v tématu [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="2bd41-358">For more information about deploying Azure Machine Learning web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

<span data-ttu-id="2bd41-359">Chcete-li nasadit novou webovou službu, je potřeba:</span><span class="sxs-lookup"><span data-stu-id="2bd41-359">To deploy a new web service, you need to:</span></span>

1. <span data-ttu-id="2bd41-360">Vytvoření experimentu vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="2bd41-360">Create a scoring experiment.</span></span>
2. <span data-ttu-id="2bd41-361">Nasazení webové služby.</span><span class="sxs-lookup"><span data-stu-id="2bd41-361">Deploy the web service.</span></span>

<span data-ttu-id="2bd41-362">K vytvoření vyhodnocování experimentu z **dokončeno** cvičení experiment, klikněte na tlačítko **vytvořit vyhodnocování EXPERIMENTOVAT** na dolním panelu akcí.</span><span class="sxs-lookup"><span data-stu-id="2bd41-362">To create a scoring experiment from a **Finished** training experiment, click **CREATE SCORING EXPERIMENT** in the lower action bar.</span></span>

![Výpočet skóre Azure][18]

<span data-ttu-id="2bd41-364">Azure Machine Learning se pokusí vytvořit vyhodnocování experimentu založené na součástech experimentu školení.</span><span class="sxs-lookup"><span data-stu-id="2bd41-364">Azure Machine Learning will attempt to create a scoring experiment based on the components of the training experiment.</span></span> <span data-ttu-id="2bd41-365">Konkrétně se:</span><span class="sxs-lookup"><span data-stu-id="2bd41-365">In particular, it will:</span></span>

1. <span data-ttu-id="2bd41-366">Uložení naučeného modelu a odeberte moduly školení modelu.</span><span class="sxs-lookup"><span data-stu-id="2bd41-366">Save the trained model and remove the model training modules.</span></span>
2. <span data-ttu-id="2bd41-367">Identifikovat logickou **vstupní port** představují schéma očekávané vstupní data.</span><span class="sxs-lookup"><span data-stu-id="2bd41-367">Identify a logical **input port** to represent the expected input data schema.</span></span>
3. <span data-ttu-id="2bd41-368">Identifikovat logickou **výstupní port** představují výstupního schématu očekávané webové služby.</span><span class="sxs-lookup"><span data-stu-id="2bd41-368">Identify a logical **output port** to represent the expected web service output schema.</span></span>

<span data-ttu-id="2bd41-369">Při vyhodnocování experimentu je vytvořen, zkontrolujte jej a podle potřeby upravte.</span><span class="sxs-lookup"><span data-stu-id="2bd41-369">When the scoring experiment is created, review it and adjust as needed.</span></span> <span data-ttu-id="2bd41-370">Typické úpravu je nahradit vstupní datové sady nebo dotaz s jedním, který vyloučí pole štítek, jak tyto nebudete mít k dispozici při volání služby.</span><span class="sxs-lookup"><span data-stu-id="2bd41-370">A typical adjustment is to replace the input dataset and/or query with one which excludes label fields, as these will not be available when the service is called.</span></span> <span data-ttu-id="2bd41-371">Je také vhodné zmenšit velikost vstupní datové sady a/nebo dotaz na několik záznamů, akorát, aby znamenat vstupní schéma.</span><span class="sxs-lookup"><span data-stu-id="2bd41-371">It is also a good practice to reduce the size of the input dataset and/or query to a few records, just enough to indicate the input schema.</span></span> <span data-ttu-id="2bd41-372">Na výstupní port je společné pro všechny vstupní pole vyloučit a obsahovat jenom **popisky vyhodnocení** a **skóre pro Magnitudu Pravděpodobnostech** ve výstupu pomocí [výběr sloupců v datové sadě] [ select-columns] modulu.</span><span class="sxs-lookup"><span data-stu-id="2bd41-372">For the output port, it is common to exclude all input fields and only include the **Scored Labels** and **Scored Probabilities** in the output using the [Select Columns in Dataset][select-columns] module.</span></span>

<span data-ttu-id="2bd41-373">Ukázka vyhodnocování experimentu je na obrázku níže.</span><span class="sxs-lookup"><span data-stu-id="2bd41-373">A sample scoring experiment is in the figure below.</span></span> <span data-ttu-id="2bd41-374">Až bude připravený k nasazení, klikněte na tlačítko **publikování webové služby** tlačítko na dolním panelu akcí.</span><span class="sxs-lookup"><span data-stu-id="2bd41-374">When ready to deploy, click the **PUBLISH WEB SERVICE** button in the lower action bar.</span></span>

![Azure Machine Learning publikování][11]

<span data-ttu-id="2bd41-376">K recap v tomto kurzu návodu jste vytvořili prostředí vědecké účely dat Azure, práce s velké veřejné datové sady úplně z získávání dat pro modelování školení a nasazení webové služby Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="2bd41-376">To recap, in this walkthrough tutorial, you have created an Azure data science environment, worked with a large public dataset all the way from data acquisition to model training and deploying of an Azure Machine Learning web service.</span></span>

### <a name="license-information"></a><span data-ttu-id="2bd41-377">Informace o licenci</span><span class="sxs-lookup"><span data-stu-id="2bd41-377">License Information</span></span>
<span data-ttu-id="2bd41-378">Tento návod ukázka a jeho doplňujícími skripty a IPython notebook(s) sdílí Microsoft v rámci licencí MIT.</span><span class="sxs-lookup"><span data-stu-id="2bd41-378">This sample walkthrough and its accompanying scripts and IPython notebook(s) are shared by Microsoft under the MIT license.</span></span> <span data-ttu-id="2bd41-379">Zkontrolujte prosím soubor LICENSE.txt v adresáři ukázkový kód na Githubu další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="2bd41-379">Please check the LICENSE.txt file in in the directory of the sample code on GitHub for more details.</span></span>

### <a name="references"></a><span data-ttu-id="2bd41-380">Odkazy</span><span class="sxs-lookup"><span data-stu-id="2bd41-380">References</span></span>
<span data-ttu-id="2bd41-381">• [Stránce pro stažení Andrés Monroy NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/)</span><span class="sxs-lookup"><span data-stu-id="2bd41-381">•    [Andrés Monroy NYC Taxi Trips Download Page](http://www.andresmh.com/nyctaxitrips/)</span></span>  
<span data-ttu-id="2bd41-382">• [FOILing NYC Taxi cestě dat podle Whong Jan](http://chriswhong.com/open-data/foil_nyc_taxi/) </span><span class="sxs-lookup"><span data-stu-id="2bd41-382">•    [FOILing NYC’s Taxi Trip Data by Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/) </span></span>  
<span data-ttu-id="2bd41-383">• [NYC taxíkem a Limousine Komise výzkum a statistiky](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span><span class="sxs-lookup"><span data-stu-id="2bd41-383">•    [NYC Taxi and Limousine Commission Research and Statistics](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span></span>

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
