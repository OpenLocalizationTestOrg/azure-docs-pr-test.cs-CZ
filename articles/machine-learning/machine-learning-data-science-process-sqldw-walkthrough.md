---
title: "Proces Team dat. vědecké účely v akci: pomocí SQL Data Warehouse | Microsoft Docs"
description: "Proces pokročilou analýzu a technologie v akci"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 88ba8e28-0bd7-49fe-8320-5dfa83b65724
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;hangzh;weig
ms.openlocfilehash: ce7de48af0f2f21576c66a962b88635a0f9f8333
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="the-team-data-science-process-in-action-using-sql-data-warehouse"></a><span data-ttu-id="60b27-103">Proces Team dat. vědecké účely v akci: pomocí SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="60b27-103">The Team Data Science Process in action: using SQL Data Warehouse</span></span>
<span data-ttu-id="60b27-104">V tomto kurzu jsme vás provede procesem vytváření a nasazování modelu strojového učení pomocí SQL datového skladu (SQL DW) pro veřejně dostupné datové sady – [NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/) datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="60b27-104">In this tutorial, we walk you through building and deploying a machine learning model using SQL Data Warehouse (SQL DW) for a publicly available dataset -- the [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset.</span></span> <span data-ttu-id="60b27-105">Binární klasifikace model sestavený předpovídá, zda je tip placené cesty a které předpovědi distribuce pro tip částky placené jsou popsány i modely pro více třídami klasifikace a regrese.</span><span class="sxs-lookup"><span data-stu-id="60b27-105">The binary classification model constructed predicts whether or not a tip is paid for a trip, and models for multiclass classification and regression are also discussed that predict the distribution for the tip amounts paid.</span></span>

<span data-ttu-id="60b27-106">Následuje postup [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="60b27-106">The procedure follows the [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) workflow.</span></span> <span data-ttu-id="60b27-107">Ukážeme, jak nastavit prostředí datového vědecké účely, jak načíst data do datového skladu SQL a o použití datového skladu SQL nebo IPython poznámkového bloku a prozkoumejte data a pracovníka funkce do modelu.</span><span class="sxs-lookup"><span data-stu-id="60b27-107">We show how to setup a data science environment, how to load the data into SQL DW, and how use either SQL DW or an IPython Notebook to explore the data and engineer features to model.</span></span> <span data-ttu-id="60b27-108">Potom ukážeme, jak vytvořit a nasadit model pomocí Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="60b27-108">We then show how to build and deploy a model with Azure Machine Learning.</span></span>

## <span data-ttu-id="60b27-109"><a name="dataset"></a>Datová sada NYC taxíkem cest</span><span class="sxs-lookup"><span data-stu-id="60b27-109"><a name="dataset"></a>The NYC Taxi Trips dataset</span></span>
<span data-ttu-id="60b27-110">Data NYC taxíkem cesty se skládá z přibližně 20GB komprimovaných souborů CSV (nekomprimovaným ~ 48GB), zaznamenávání 173 milionů jednotlivých cest a tarify placené pro každou cestu.</span><span class="sxs-lookup"><span data-stu-id="60b27-110">The NYC Taxi Trip data consists of about 20GB of compressed CSV files (~48GB uncompressed), recording more than 173 million individual trips and the fares paid for each trip.</span></span> <span data-ttu-id="60b27-111">Každý záznam cestě zahrnuje vyzvednutí a odkládací umístění a časy, anonymizovaná hackerský (ovladač) číslo licence a číslo Medailon (taxi na jedinečné id).</span><span class="sxs-lookup"><span data-stu-id="60b27-111">Each trip record includes the pickup and drop-off locations and times, anonymized hack (driver's) license number, and the medallion (taxi’s unique id) number.</span></span> <span data-ttu-id="60b27-112">Data obsahuje všechny služebních cest v roku 2013 a je dostupné pro každý měsíc následující dvě datové sady:</span><span class="sxs-lookup"><span data-stu-id="60b27-112">The data covers all trips in the year 2013 and is provided in the following two datasets for each month:</span></span>

1. <span data-ttu-id="60b27-113">**Trip_data.csv** soubor obsahuje podrobnosti o cestě, například na počtu cestujících, vyzvednutí a dropoff body, doba trvání cesty a délka cesty.</span><span class="sxs-lookup"><span data-stu-id="60b27-113">The **trip_data.csv** file contains trip details, such as number of passengers, pickup and dropoff points, trip duration, and trip length.</span></span> <span data-ttu-id="60b27-114">Tady je několik ukázkových záznamů:</span><span class="sxs-lookup"><span data-stu-id="60b27-114">Here are a few sample records:</span></span>
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. <span data-ttu-id="60b27-115">**Trip_fare.csv** soubor obsahuje podrobnosti o tarif placené pro každou cestu, například typ platby, velikost tarif, příplatek a daně, tipy a mýtné, a celkovou velikost placené.</span><span class="sxs-lookup"><span data-stu-id="60b27-115">The **trip_fare.csv** file contains details of the fare paid for each trip, such as payment type, fare amount, surcharge and taxes, tips and tolls, and the total amount paid.</span></span> <span data-ttu-id="60b27-116">Tady je několik ukázkových záznamů:</span><span class="sxs-lookup"><span data-stu-id="60b27-116">Here are a few sample records:</span></span>
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

<span data-ttu-id="60b27-117">**Jedinečný klíč** používá k připojení k cestě\_dat a cesty\_tarif se skládá z následující tři pole:</span><span class="sxs-lookup"><span data-stu-id="60b27-117">The **unique key** used to join trip\_data and trip\_fare is composed of the following three fields:</span></span>

* <span data-ttu-id="60b27-118">medailonu,</span><span class="sxs-lookup"><span data-stu-id="60b27-118">medallion,</span></span>
* <span data-ttu-id="60b27-119">zabezpečení\_licencí a</span><span class="sxs-lookup"><span data-stu-id="60b27-119">hack\_license and</span></span>
* <span data-ttu-id="60b27-120">vyzvednutí\_data a času.</span><span class="sxs-lookup"><span data-stu-id="60b27-120">pickup\_datetime.</span></span>

## <span data-ttu-id="60b27-121"><a name="mltasks"></a>Adresa tři typy úloh předpovědi</span><span class="sxs-lookup"><span data-stu-id="60b27-121"><a name="mltasks"></a>Address three types of prediction tasks</span></span>
<span data-ttu-id="60b27-122">Jsme formulovali tři předpovědi problémů na základě *tip\_velikost* pro ilustraci tři druhy modelování úlohy:</span><span class="sxs-lookup"><span data-stu-id="60b27-122">We formulate three prediction problems based on the *tip\_amount* to illustrate three kinds of modeling tasks:</span></span>

1. <span data-ttu-id="60b27-123">**Binární klasifikace**: K předvídání, jestli tip byl placené cesty, tj. *tip\_velikost* větší než $0 je pozitivní příklad, při *tip\_velikost* $ 0 je záporný příklad.</span><span class="sxs-lookup"><span data-stu-id="60b27-123">**Binary classification**: To predict whether or not a tip was paid for a trip, i.e. a *tip\_amount* that is greater than $0 is a positive example, while a *tip\_amount* of $0 is a negative example.</span></span>
2. <span data-ttu-id="60b27-124">**Více třídami klasifikace**: K předpovědi rozsahu tipu placené pro cestu.</span><span class="sxs-lookup"><span data-stu-id="60b27-124">**Multiclass classification**: To predict the range of tip paid for the trip.</span></span> <span data-ttu-id="60b27-125">Jsme rozdělit *tip\_velikost* do pěti přihrádek nebo třídy:</span><span class="sxs-lookup"><span data-stu-id="60b27-125">We divide the *tip\_amount* into five bins or classes:</span></span>
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. <span data-ttu-id="60b27-126">**Úloha regrese**: K předvídání množství tip placené cesty.</span><span class="sxs-lookup"><span data-stu-id="60b27-126">**Regression task**: To predict the amount of tip paid for a trip.</span></span>  

## <span data-ttu-id="60b27-127"><a name="setup"></a>Nastavení prostředí vědecké účely dat Azure pro pokročilou analýzu</span><span class="sxs-lookup"><span data-stu-id="60b27-127"><a name="setup"></a>Set up the Azure data science environment for advanced analytics</span></span>
<span data-ttu-id="60b27-128">K nastavení prostředí vědecké zpracování dat Azure, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="60b27-128">To set up your Azure Data Science environment, follow these steps.</span></span>

<span data-ttu-id="60b27-129">**Vytvořit svůj vlastní účet úložiště objektů blob v Azure**</span><span class="sxs-lookup"><span data-stu-id="60b27-129">**Create your own Azure blob storage account**</span></span>

* <span data-ttu-id="60b27-130">Při zřizování úložiště objektů blob v Azure, zvolte geografické umístění úložiště objektů blob v Azure v nebo co nejblíže k **jihu USA**, což je, které jsou uložená data NYC taxíkem.</span><span class="sxs-lookup"><span data-stu-id="60b27-130">When you provision your own Azure blob storage, choose a geo-location for your Azure blob storage in or as close as possible to **South Central US**, which is where the NYC Taxi data is stored.</span></span> <span data-ttu-id="60b27-131">Data se zkopírují pomocí AzCopy z úložiště kontejneru veřejného objektu blob do kontejneru v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="60b27-131">The data will be copied using AzCopy from the public blob storage container to a container in your own storage account.</span></span> <span data-ttu-id="60b27-132">Čím bližší služby Azure blob storage je jihu USA, tím rychleji se tento úkol (krok 4) dokončí.</span><span class="sxs-lookup"><span data-stu-id="60b27-132">The closer your Azure blob storage is to South Central US, the faster this task (Step 4) will be completed.</span></span>
* <span data-ttu-id="60b27-133">Při vytváření účtu úložiště Azure, postupujte podle kroků uvedených v [účty Azure storage](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="60b27-133">To create your own Azure storage account, follow the steps outlined at [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="60b27-134">Ujistěte se, chcete-li poznámky u hodnot pro tyto přihlašovací údaje účtu úložiště, jak bude potřeba dále v tomto návodu.</span><span class="sxs-lookup"><span data-stu-id="60b27-134">Be sure to make notes on the values for following storage account credentials as they will be needed later in this walkthrough.</span></span>
  
  * <span data-ttu-id="60b27-135">**Název účtu úložiště**</span><span class="sxs-lookup"><span data-stu-id="60b27-135">**Storage Account Name**</span></span>
  * <span data-ttu-id="60b27-136">**Klíče účtu úložiště.**</span><span class="sxs-lookup"><span data-stu-id="60b27-136">**Storage Account Key**</span></span>
  * <span data-ttu-id="60b27-137">**Název kontejneru** (aplikaci, kterou chcete data se neukládají v Azure blob storage)</span><span class="sxs-lookup"><span data-stu-id="60b27-137">**Container Name** (which you want the data to be stored in the Azure blob storage)</span></span>

<span data-ttu-id="60b27-138">**Zřízení instance Azure SQL DW.**</span><span class="sxs-lookup"><span data-stu-id="60b27-138">**Provision your Azure SQL DW instance.**</span></span>
<span data-ttu-id="60b27-139">Postupujte podle dokumentace v [vytvořit SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) ke zřízení instanci SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="60b27-139">Follow the documentation at [Create a SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) to provision a SQL Data Warehouse instance.</span></span> <span data-ttu-id="60b27-140">Ujistěte se, abyste vytvořili zápisy na následující přihlašovací údaje SQL Data Warehouse, které se použije v dalších krocích.</span><span class="sxs-lookup"><span data-stu-id="60b27-140">Make sure that you make notations on the following SQL Data Warehouse credentials which will be used in later steps.</span></span>

* <span data-ttu-id="60b27-141">**Název serveru**: <server Name>. database.windows.net</span><span class="sxs-lookup"><span data-stu-id="60b27-141">**Server Name**: <server Name>.database.windows.net</span></span>
* <span data-ttu-id="60b27-142">**Název SQLDW (databáze)**</span><span class="sxs-lookup"><span data-stu-id="60b27-142">**SQLDW (Database) Name**</span></span>
* <span data-ttu-id="60b27-143">**Uživatelské jméno**</span><span class="sxs-lookup"><span data-stu-id="60b27-143">**Username**</span></span>
* <span data-ttu-id="60b27-144">**Heslo**</span><span class="sxs-lookup"><span data-stu-id="60b27-144">**Password**</span></span>

<span data-ttu-id="60b27-145">**Instalace sady Visual Studio a SQL Server Data Tools.**</span><span class="sxs-lookup"><span data-stu-id="60b27-145">**Install Visual Studio and SQL Server Data Tools.**</span></span> <span data-ttu-id="60b27-146">Pokyny najdete v tématu [instalaci sady Visual Studio 2015 a SSDT (SQL Server Data Tools) pro SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="60b27-146">For instructions, see [Install Visual Studio 2015 and/or SSDT (SQL Server Data Tools) for SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md).</span></span>

<span data-ttu-id="60b27-147">**Připojte k vaší datového skladu Azure SQL pomocí sady Visual Studio.**</span><span class="sxs-lookup"><span data-stu-id="60b27-147">**Connect to your Azure SQL DW with Visual Studio.**</span></span> <span data-ttu-id="60b27-148">Pokyny najdete v tématu kroky 1 a 2 v [připojit k Azure SQL Data Warehouse pomocí sady Visual Studio](../sql-data-warehouse/sql-data-warehouse-connect-overview.md).</span><span class="sxs-lookup"><span data-stu-id="60b27-148">For instructions, see steps 1 & 2 in [Connect to Azure SQL Data Warehouse with Visual Studio](../sql-data-warehouse/sql-data-warehouse-connect-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="60b27-149">Spusťte následující dotaz SQL na databázi, kterou jste vytvořili v SQL Data Warehouse (namísto dotazu zadaný v kroku 3 tématu připojení) na **vytvoření hlavního klíče**.</span><span class="sxs-lookup"><span data-stu-id="60b27-149">Run the following SQL query on the database you created in your SQL Data Warehouse (instead of the query provided in step 3 of the connect topic,) to **create a master key**.</span></span>
> 
> 

    BEGIN TRY
           --Try to create the master key
        CREATE MASTER KEY
    END TRY
    BEGIN CATCH
           --If the master key exists, do nothing
    END CATCH;

<span data-ttu-id="60b27-150">**Vytvořte pracovní prostor služby Azure Machine Learning v rámci vašeho předplatného Azure.**</span><span class="sxs-lookup"><span data-stu-id="60b27-150">**Create an Azure Machine Learning workspace under your Azure subscription.**</span></span> <span data-ttu-id="60b27-151">Pokyny najdete v tématu [vytvořit pracovní prostor služby Azure Machine Learning](machine-learning-create-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="60b27-151">For instructions, see [Create an Azure Machine Learning workspace](machine-learning-create-workspace.md).</span></span>

## <span data-ttu-id="60b27-152"><a name="getdata"></a>Načítání dat do SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="60b27-152"><a name="getdata"></a>Load the data into SQL Data Warehouse</span></span>
<span data-ttu-id="60b27-153">Otevřete konzolu příkazového prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="60b27-153">Open a Windows PowerShell command console.</span></span> <span data-ttu-id="60b27-154">Spusťte PowerShell následující příkazy ke stažení v příkladu SQL skriptu soubory, které můžeme sdílet s vámi na Githubu do místního adresáře, který zadáte s parametrem *- DestDir*.</span><span class="sxs-lookup"><span data-stu-id="60b27-154">Run the following PowerShell commands to download the example SQL script files that we share with you on GitHub to a local directory that you specify with the parameter *-DestDir*.</span></span> <span data-ttu-id="60b27-155">Můžete změnit hodnotu parametru *- DestDir* do libovolného místního adresáře.</span><span class="sxs-lookup"><span data-stu-id="60b27-155">You can change the value of parameter *-DestDir* to any local directory.</span></span> <span data-ttu-id="60b27-156">Pokud *- DestDir* neexistuje, vytvoří se skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="60b27-156">If *-DestDir* does not exist, it will be created by the PowerShell script.</span></span>

> [!NOTE]
> <span data-ttu-id="60b27-157">Možná budete muset **spustit jako správce** při provádění následující skript prostředí PowerShell, pokud vaše *DestDir* directory potřebuje správce oprávnění k vytvoření nebo zápis.</span><span class="sxs-lookup"><span data-stu-id="60b27-157">You might need to **Run as Administrator** when executing the following PowerShell script if your *DestDir* directory needs Administrator privilege to create or to write to it.</span></span>
> 
> 

    $source = "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/Download_Scripts_SQLDW_Walkthrough.ps1"
    $ps1_dest = "$pwd\Download_Scripts_SQLDW_Walkthrough.ps1"
    $wc = New-Object System.Net.WebClient
    $wc.DownloadFile($source, $ps1_dest)
    .\Download_Scripts_SQLDW_Walkthrough.ps1 –DestDir 'C:\tempSQLDW'

<span data-ttu-id="60b27-158">Po úspěšném provedení změny aktuální pracovní adresář *- DestDir*.</span><span class="sxs-lookup"><span data-stu-id="60b27-158">After successful execution, your current working directory changes to *-DestDir*.</span></span> <span data-ttu-id="60b27-159">Nyní byste měli mít obrazovka jako níže:</span><span class="sxs-lookup"><span data-stu-id="60b27-159">You should be able to see screen like below:</span></span>

![][19]

<span data-ttu-id="60b27-160">Ve vašem *- DestDir*, spusťte následující skript prostředí PowerShell v režimu správce:</span><span class="sxs-lookup"><span data-stu-id="60b27-160">In your *-DestDir*, execute the following PowerShell script in administrator mode:</span></span>

    ./SQLDW_Data_Import.ps1

<span data-ttu-id="60b27-161">Při prvním spuštění skriptu prostředí PowerShell, zobrazí se výzva k vstupní informace z vašeho datového skladu SQL Azure a účtu úložiště objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="60b27-161">When the PowerShell script runs for the first time, you will be asked to input the information from your Azure SQL DW and your Azure blob storage account.</span></span> <span data-ttu-id="60b27-162">Po dokončení tohoto skriptu prostředí PowerShell s prvním, přihlašovací údaje vstup je bude mít byla zapsána do konfiguračního souboru SQLDW.conf v přítomen pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="60b27-162">When this PowerShell script completes running for the first time, the credentials you input will have been written to a configuration file SQLDW.conf in the present working directory.</span></span> <span data-ttu-id="60b27-163">Budoucí spuštění tento soubor skriptu PowerShell má možnost přečtěte si, že všechny parametry z tohoto konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="60b27-163">The future run of this PowerShell script file has the option to read all needed parameters from this configuration file.</span></span> <span data-ttu-id="60b27-164">Pokud potřebujete změnit některé parametry, můžete k vstupní parametry na obrazovce po řádku odstranit tento konfigurační soubor a po zobrazení výzvy zadání hodnot parametrů nebo ke změně hodnoty parametru úpravou souboru SQLDW.conf ve vašem *- DestDir* adresáře.</span><span class="sxs-lookup"><span data-stu-id="60b27-164">If you need to change some parameters, you can choose to input the parameters on the screen upon prompt by deleting this configuration file and inputting the parameters values as prompted or to change the parameter values by editing the SQLDW.conf file in your *-DestDir* directory.</span></span>

> [!NOTE]
> <span data-ttu-id="60b27-165">Aby se zabránilo schématu název je v konfliktu s těmi, které již existují v Azure SQL DW, při čtení parametry přímo ze souboru SQLDW.conf, náhodné číslo 3 číslice je přidat k názvu schématu ze souboru SQLDW.conf jako výchozí název schématu pro každé spuštění.</span><span class="sxs-lookup"><span data-stu-id="60b27-165">In order to avoid schema name conflicts with those that already exist in your Azure SQL DW, when reading parameters directly from the SQLDW.conf file, a 3-digit random number is added to the schema name from the SQLDW.conf file as the default schema name for each run.</span></span> <span data-ttu-id="60b27-166">Skript prostředí PowerShell můžete být vyzváni k zadání názvu schématu: uvážení uživatele může být zadán název.</span><span class="sxs-lookup"><span data-stu-id="60b27-166">The PowerShell script may prompt you for a schema name: the name may be specified at user discretion.</span></span>
> 
> 

<span data-ttu-id="60b27-167">To **skript prostředí PowerShell** souboru dokončí následující úkoly:</span><span class="sxs-lookup"><span data-stu-id="60b27-167">This **PowerShell script** file completes the following tasks:</span></span>

* <span data-ttu-id="60b27-168">**Stáhne a nainstaluje AzCopy**, pokud ještě není nainstalovaný nástroj AzCopy</span><span class="sxs-lookup"><span data-stu-id="60b27-168">**Downloads and installs AzCopy**, if AzCopy is not already installed</span></span>
  
        $AzCopy_path = SearchAzCopy
        if ($AzCopy_path -eq $null){
               Write-Host "AzCopy.exe is not found in C:\Program Files*. Now, start installing AzCopy..." -ForegroundColor "Yellow"
            InstallAzCopy
            $AzCopy_path = SearchAzCopy
        }
            $env_path = $env:Path
            for ($i=0; $i -lt $AzCopy_path.count; $i++){
                if ($AzCopy_path.count -eq 1){
                    $AzCopy_path_i = $AzCopy_path
                } else {
                    $AzCopy_path_i = $AzCopy_path[$i]
                }
                if ($env_path -notlike '*' +$AzCopy_path_i+'*'){
                    Write-Host $AzCopy_path_i 'not in system path, add it...'
                    [Environment]::SetEnvironmentVariable("Path", "$AzCopy_path_i;$env_path", "Machine")
                    $env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine")
                    $env_path = $env:Path
                }
* <span data-ttu-id="60b27-169">**Zkopíruje data na účtu úložiště objektů blob privátní** z veřejného objektu blob s AzCopy</span><span class="sxs-lookup"><span data-stu-id="60b27-169">**Copies data to your private blob storage account** from the public blob with AzCopy</span></span>
  
        Write-Host "AzCopy is copying data from public blob to yo storage account. It may take a while..." -ForegroundColor "Yellow"
        $start_time = Get-Date
        AzCopy.exe /Source:$Source /Dest:$DestURL /DestKey:$StorageAccountKey /S
        $end_time = Get-Date
        $time_span = $end_time - $start_time
        $total_seconds = [math]::Round($time_span.TotalSeconds,2)
        Write-Host "AzCopy finished copying data. Please check your storage account to verify." -ForegroundColor "Yellow"
        Write-Host "This step (copying data from public blob to your storage account) takes $total_seconds seconds." -ForegroundColor "Green"
* <span data-ttu-id="60b27-170">**Načte data pomocí Polybase (spuštěním LoadDataToSQLDW.sql) pro vaši Azure SQL DW** z vašeho účtu úložiště objektů blob privátní pomocí následujících příkazů.</span><span class="sxs-lookup"><span data-stu-id="60b27-170">**Loads data using Polybase (by executing LoadDataToSQLDW.sql) to your Azure SQL DW** from your private blob storage account with the following commands.</span></span>
  
  * <span data-ttu-id="60b27-171">Vytvořte schéma</span><span class="sxs-lookup"><span data-stu-id="60b27-171">Create a schema</span></span>
    
          EXEC (''CREATE SCHEMA {schemaname};'');
  * <span data-ttu-id="60b27-172">Vytvoření oboru databáze přihlašovacích údajů</span><span class="sxs-lookup"><span data-stu-id="60b27-172">Create a database scoped credential</span></span>
    
          CREATE DATABASE SCOPED CREDENTIAL {KeyAlias}
          WITH IDENTITY = ''asbkey'' ,
          Secret = ''{StorageAccountKey}''
  * <span data-ttu-id="60b27-173">Vytvoření externího zdroje dat pro objekt blob úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="60b27-173">Create an external data source for an Azure storage blob</span></span>
    
          CREATE EXTERNAL DATA SOURCE {nyctaxi_trip_storage}
          WITH
          (
              TYPE = HADOOP,
              LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
              CREDENTIAL = {KeyAlias}
          )
          ;
    
          CREATE EXTERNAL DATA SOURCE {nyctaxi_fare_storage}
          WITH
          (
              TYPE = HADOOP,
              LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
              CREDENTIAL = {KeyAlias}
          )
          ;
  * <span data-ttu-id="60b27-174">Vytvořte formátu externí soubor pro soubor csv.</span><span class="sxs-lookup"><span data-stu-id="60b27-174">Create an external file format for a csv file.</span></span> <span data-ttu-id="60b27-175">Data nekomprimované a pole jsou oddělená znakem kanálu.</span><span class="sxs-lookup"><span data-stu-id="60b27-175">Data is uncompressed and fields are separated with the pipe character.</span></span>
    
          CREATE EXTERNAL FILE FORMAT {csv_file_format}
          WITH
          (   
              FORMAT_TYPE = DELIMITEDTEXT,
              FORMAT_OPTIONS  
              (
                  FIELD_TERMINATOR ='','',
                  USE_TYPE_DEFAULT = TRUE
              )
          )
          ;
  * <span data-ttu-id="60b27-176">Vytvořte externí tarif a tabulky cesty pro datovou sadu taxíkem NYC v Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="60b27-176">Create external fare and trip tables for NYC taxi dataset in Azure blob storage.</span></span>
    
          CREATE EXTERNAL TABLE {external_nyctaxi_fare}
          (
              medallion varchar(50) not null,
              hack_license varchar(50) not null,
              vendor_id char(3),
              pickup_datetime datetime not null,
              payment_type char(3),
              fare_amount float,
              surcharge float,
              mta_tax float,
              tip_amount float,
              tolls_amount float,
              total_amount float
          )
          with (
              LOCATION    = ''/nyctaxifare/'',
              DATA_SOURCE = {nyctaxi_fare_storage},
              FILE_FORMAT = {csv_file_format},
              REJECT_TYPE = VALUE,
              REJECT_VALUE = 12     
          )  

            CREATE EXTERNAL TABLE {external_nyctaxi_trip}
            (
                   medallion varchar(50) not null,
                   hack_license varchar(50)  not null,
                   vendor_id char(3),
                   rate_code char(3),
                   store_and_fwd_flag char(3),
                   pickup_datetime datetime  not null,
                   dropoff_datetime datetime,
                   passenger_count int,
                   trip_time_in_secs bigint,
                   trip_distance float,
                   pickup_longitude varchar(30),
                   pickup_latitude varchar(30),
                   dropoff_longitude varchar(30),
                   dropoff_latitude varchar(30)
            )
            with (
                LOCATION    = ''/nyctaxitrip/'',
                DATA_SOURCE = {nyctaxi_trip_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12         
            )

    - <span data-ttu-id="60b27-177">Načtení dat z externí tabulky v Azure blob storage do SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="60b27-177">Load data from external tables in Azure blob storage to SQL Data Warehouse</span></span>

            CREATE TABLE {schemaname}.{nyctaxi_fare}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_fare}
            ;

            CREATE TABLE {schemaname}.{nyctaxi_trip}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_trip}
            ;

    - <span data-ttu-id="60b27-178">Vytvoří tabulku ukázkových dat (NYCTaxi_Sample) a vložit data, vyberou dotazů SQL na služební cestě a tarif tabulky.</span><span class="sxs-lookup"><span data-stu-id="60b27-178">Create a sample data table (NYCTaxi_Sample) and insert data to it from selecting SQL queries on the trip and fare tables.</span></span> <span data-ttu-id="60b27-179">(Některé kroky tohoto názorného postupu musí tato ukázková tabulka.)</span><span class="sxs-lookup"><span data-stu-id="60b27-179">(Some steps of this walkthrough needs to use this sample table.)</span></span>

            CREATE TABLE {schemaname}.{nyctaxi_sample}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            (
                SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount,
                tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
                tip_class = CASE
                        WHEN (tip_amount = 0) THEN 0
                        WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                        WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                        WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                        ELSE 4
                    END
                FROM {schemaname}.{nyctaxi_trip} t, {schemaname}.{nyctaxi_fare} f
                WHERE datepart("mi",t.pickup_datetime) = 1
                AND t.medallion = f.medallion
                AND   t.hack_license = f.hack_license
                AND   t.pickup_datetime = f.pickup_datetime
                AND   pickup_longitude <> ''0''
                AND   dropoff_longitude <> ''0''
            )
            ;

<span data-ttu-id="60b27-180">Geografické umístění účtů úložiště ovlivňuje časů načtení.</span><span class="sxs-lookup"><span data-stu-id="60b27-180">The geographic location of your storage accounts affects load times.</span></span>

> [!NOTE]
> <span data-ttu-id="60b27-181">V závislosti na jejich zeměpisné umístění svého účtu úložiště blob privátní proces kopírování dat z veřejného objektu blob na váš účet privátní úložiště může trvat přibližně 15 minut, nebo i déle a proces načítání dat z vašeho účtu úložiště do vaší Azure SQL DW může trvat 20 minut nebo déle.</span><span class="sxs-lookup"><span data-stu-id="60b27-181">Depending on the geographical location of your private blob storage account, the process of copying data from a public blob to your private storage account can take about 15 minutes, or even longer,and the process of loading data from your storage account to your Azure SQL DW could take 20 minutes or longer.</span></span>  
> 
> 

<span data-ttu-id="60b27-182">Je nutné se rozhodnout co proveďte, pokud máte duplicitní zdrojový a cílový soubor.</span><span class="sxs-lookup"><span data-stu-id="60b27-182">You will have to decide what do if you have duplicate source and destination files.</span></span>

> [!NOTE]
> <span data-ttu-id="60b27-183">Pokud soubory CSV zkopírovány z veřejného objektu blob úložiště k účtu úložiště objektů blob privátní již existuje v účtu úložiště objektů blob privátní, AzCopy se zeptá, jestli chcete je přepsat.</span><span class="sxs-lookup"><span data-stu-id="60b27-183">If the .csv files to be copied from the public blob storage to your private blob storage account already exist in your private blob storage account, AzCopy will ask you whether you want to overwrite them.</span></span> <span data-ttu-id="60b27-184">Pokud nechcete je přepsat, vstup  **n**  po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="60b27-184">If you do not want to overwrite them, input **n** when prompted.</span></span> <span data-ttu-id="60b27-185">Pokud chcete přepsat **všechny** z nich, vstup **** po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="60b27-185">If you want to overwrite **all** of them, input **a** when prompted.</span></span> <span data-ttu-id="60b27-186">Můžete také zadat **y** přepsat soubory .csv jednotlivě.</span><span class="sxs-lookup"><span data-stu-id="60b27-186">You can also input **y** to overwrite .csv files individually.</span></span>
> 
> 

![Vykreslení #21][21]

<span data-ttu-id="60b27-188">Můžete vytvořit svoje vlastní data.</span><span class="sxs-lookup"><span data-stu-id="60b27-188">You can use your own data.</span></span> <span data-ttu-id="60b27-189">Pokud vaše data jsou v místním počítači ve vaší aplikaci reálného života, můžete pořád použít AzCopy k nahrání místní data do vaší privátní Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="60b27-189">If your data is in your on-premises machine in your real life application, you can still use AzCopy to upload on-premises data to your private Azure blob storage.</span></span> <span data-ttu-id="60b27-190">Budete muset změnit **zdroj** umístění, `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, v příkazu AzCopy souboru skriptu prostředí PowerShell místní adresář, který obsahuje vaše data.</span><span class="sxs-lookup"><span data-stu-id="60b27-190">You only need to change the **Source** location, `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, in the AzCopy command of the PowerShell script file to the local directory that contains your data.</span></span>

> [!TIP]
> <span data-ttu-id="60b27-191">Pokud již vaše data v aplikaci reálného života ve vaší privátní Azure blob storage, můžete přeskočit na krok AzCopy v skriptu prostředí PowerShell a přímo nahrát data do Azure SQL DW.</span><span class="sxs-lookup"><span data-stu-id="60b27-191">If your data is already in your private Azure blob storage in your real life application, you can skip the AzCopy step in the PowerShell script and directly upload the data to Azure SQL DW.</span></span> <span data-ttu-id="60b27-192">To bude vyžadovat další úpravy skript, který chcete přizpůsobit na formát data.</span><span class="sxs-lookup"><span data-stu-id="60b27-192">This will require additional edits of the script to tailor it to the format of your data.</span></span>
> 
> 

<span data-ttu-id="60b27-193">Tento skript prostředí Powershell také připojuje v Azure SQL DW informace do datových souborů příklad zkoumání SQLDW_Explorations.sql, SQLDW_Explorations.ipynb a SQLDW_Explorations_Scripts.py tak, aby tyto tři soubory jsou připravení vyzkoušeny okamžitě po dokončení skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="60b27-193">This Powershell script also plugs in the Azure SQL DW information into the data exploration example files SQLDW_Explorations.sql, SQLDW_Explorations.ipynb, and SQLDW_Explorations_Scripts.py so that these three files are ready to be tried out instantly after the PowerShell script completes.</span></span>

<span data-ttu-id="60b27-194">Po úspěšném spuštění, zobrazí se obrazovka jako níže:</span><span class="sxs-lookup"><span data-stu-id="60b27-194">After a successful execution, you will see screen like below:</span></span>

![][20]

## <span data-ttu-id="60b27-195"><a name="dbexplore"></a>Zkoumání dat a funkce analýzy v Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="60b27-195"><a name="dbexplore"></a>Data exploration and feature engineering in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="60b27-196">V této části, můžeme provést zkoumání a funkce generování dat provádění dotazů SQL Azure SQL DW přímo pomocí **Data nástroje sady Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="60b27-196">In this section, we perform data exploration and feature generation by running SQL queries against Azure SQL DW directly using **Visual Studio Data Tools**.</span></span> <span data-ttu-id="60b27-197">Všechny dotazy SQL použít v této části najdete v ukázkový skript s názvem *SQLDW_Explorations.sql*.</span><span class="sxs-lookup"><span data-stu-id="60b27-197">All SQL queries used in this section can be found in the sample script named *SQLDW_Explorations.sql*.</span></span> <span data-ttu-id="60b27-198">Tento soubor má již byla stažena do vašeho místního adresáře skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="60b27-198">This file has already been downloaded to your local directory by the PowerShell script.</span></span> <span data-ttu-id="60b27-199">Můžete také načíst z [Githubu](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql).</span><span class="sxs-lookup"><span data-stu-id="60b27-199">You can also retrieve it from [GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql).</span></span> <span data-ttu-id="60b27-200">Ale soubor v Githubu nemá informace o Azure SQL DW napájený ze sítě.</span><span class="sxs-lookup"><span data-stu-id="60b27-200">But the file in GitHub does not have the Azure SQL DW information plugged in.</span></span>

<span data-ttu-id="60b27-201">Připojení k vaší Azure SQL DW pomocí sady Visual Studio s datovým Skladem SQL přihlašovací jméno a heslo a otevře **Průzkumník objektů systému SQL** potvrďte databáze a tabulky byly importovány.</span><span class="sxs-lookup"><span data-stu-id="60b27-201">Connect to your Azure SQL DW using Visual Studio with the SQL DW login name and password and open up the **SQL Object Explorer** to confirm the database and tables have been imported.</span></span> <span data-ttu-id="60b27-202">Načtení *SQLDW_Explorations.sql* souboru.</span><span class="sxs-lookup"><span data-stu-id="60b27-202">Retrieve the *SQLDW_Explorations.sql* file.</span></span>

> [!NOTE]
> <span data-ttu-id="60b27-203">Pokud chcete otevřít editor dotazů paralelní datového skladu (PDW), použijte **nový dotaz** příkaz vaší PDW vybráno v **Průzkumník objektů systému SQL**.</span><span class="sxs-lookup"><span data-stu-id="60b27-203">To open a Parallel Data Warehouse (PDW) query editor, use the **New Query** command while your PDW is selected in the **SQL Object Explorer**.</span></span> <span data-ttu-id="60b27-204">Standardní editoru dotazů SQL nepodporuje PDW.</span><span class="sxs-lookup"><span data-stu-id="60b27-204">The standard SQL query editor is not supported by PDW.</span></span>
> 
> 

<span data-ttu-id="60b27-205">Tady jsou typu dat zkoumání a funkce generování úlohy provádějí v této části:</span><span class="sxs-lookup"><span data-stu-id="60b27-205">Here are the type of data exploration and feature generation tasks performed in this section:</span></span>

* <span data-ttu-id="60b27-206">Prozkoumejte data distribuce několik polí v různých časových oken.</span><span class="sxs-lookup"><span data-stu-id="60b27-206">Explore data distributions of a few fields in varying time windows.</span></span>
* <span data-ttu-id="60b27-207">Prozkoumejte data quality polí zeměpisné šířky a délky.</span><span class="sxs-lookup"><span data-stu-id="60b27-207">Investigate data quality of the longitude and latitude fields.</span></span>
* <span data-ttu-id="60b27-208">Generovat binární a více třídami klasifikační štítky na základě **tip\_velikost**.</span><span class="sxs-lookup"><span data-stu-id="60b27-208">Generate binary and multiclass classification labels based on the **tip\_amount**.</span></span>
* <span data-ttu-id="60b27-209">Generovat funkce a výpočetní nebo porovnat cestě vzdálenosti.</span><span class="sxs-lookup"><span data-stu-id="60b27-209">Generate features and compute/compare trip distances.</span></span>
* <span data-ttu-id="60b27-210">Spojení dvou tabulek a extrahovat z náhodného vzorku, který bude použit k vytvoření modelů.</span><span class="sxs-lookup"><span data-stu-id="60b27-210">Join the two tables and extract a random sample that will be used to build models.</span></span>

### <a name="data-import-verification"></a><span data-ttu-id="60b27-211">Ověření importu dat</span><span class="sxs-lookup"><span data-stu-id="60b27-211">Data import verification</span></span>
<span data-ttu-id="60b27-212">Tyto dotazy poskytují rychlý ověření počtu řádků a sloupců v tabulkách vyplněny dříve Polybase na paralelní hromadného importu,</span><span class="sxs-lookup"><span data-stu-id="60b27-212">These queries provide a quick verification of the number of rows and columns in the tables populated earlier using Polybase's parallel bulk import,</span></span>

    -- Report number of rows in table <nyctaxi_trip> without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')

    -- Report number of columns in table <nyctaxi_trip>
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = '<nyctaxi_trip>' AND table_schema = '<schemaname>'

<span data-ttu-id="60b27-213">**Výstup:** měli byste obdržet 173,179,759 řádků a sloupců 14.</span><span class="sxs-lookup"><span data-stu-id="60b27-213">**Output:** You should get 173,179,759 rows and 14 columns.</span></span>

### <a name="exploration-trip-distribution-by-medallion"></a><span data-ttu-id="60b27-214">Zkoumání: Cestě distribuce podle Medailon</span><span class="sxs-lookup"><span data-stu-id="60b27-214">Exploration: Trip distribution by medallion</span></span>
<span data-ttu-id="60b27-215">Tento příklad dotazu identifikuje medallions (taxi čísla), které byly dokončeny více než 100 služebních cest v rámci určeného časového období.</span><span class="sxs-lookup"><span data-stu-id="60b27-215">This example query identifies the medallions (taxi numbers) that completed more than 100 trips within a specified time period.</span></span> <span data-ttu-id="60b27-216">Dotaz by využívat přístup dělenou tabulku vzhledem k tomu, že je podmíněno tím schéma oddílů **vyzvednutí\_data a času**.</span><span class="sxs-lookup"><span data-stu-id="60b27-216">The query would benefit from the partitioned table access since it is conditioned by the partition scheme of **pickup\_datetime**.</span></span> <span data-ttu-id="60b27-217">Dotazování úplné datové sadě také budou používat oddílů tabulky nebo indexu kontroly.</span><span class="sxs-lookup"><span data-stu-id="60b27-217">Querying the full dataset will also make use of the partitioned table and/or index scan.</span></span>

    SELECT medallion, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

<span data-ttu-id="60b27-218">**Výstup:** dotaz by měl vrátit tabulku s řádky zadání 13,369 medallions (taxislužby) a číslo cesty dokončit v 2013.</span><span class="sxs-lookup"><span data-stu-id="60b27-218">**Output:** The query should return a table with rows specifying the 13,369 medallions (taxis) and the number of trip completed by them in 2013.</span></span> <span data-ttu-id="60b27-219">Poslední sloupec obsahuje počet služebních cest byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="60b27-219">The last column contains the count of the number of trips completed.</span></span>

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a><span data-ttu-id="60b27-220">Zkoumání: Cestě distribuce podle Medailon a hack_license</span><span class="sxs-lookup"><span data-stu-id="60b27-220">Exploration: Trip distribution by medallion and hack_license</span></span>
<span data-ttu-id="60b27-221">Tento příklad identifikuje medallions (taxi čísla) a hack_license čísla (ovladače), dokončeno více než 100 služebních cest v rámci určeného časového období.</span><span class="sxs-lookup"><span data-stu-id="60b27-221">This example identifies the medallions (taxi numbers) and hack_license numbers (drivers) that completed more than 100 trips within a specified time period.</span></span>

    SELECT medallion, hack_license, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

<span data-ttu-id="60b27-222">**Výstup:** dotaz by měl vrátit tabulku s 13,369 řádky zadání 13,369 ID car a ovladače, které dokončily více, 100 cest v 2013.</span><span class="sxs-lookup"><span data-stu-id="60b27-222">**Output:** The query should return a table with 13,369 rows specifying the 13,369 car/driver IDs that have completed more that 100 trips in 2013.</span></span> <span data-ttu-id="60b27-223">Poslední sloupec obsahuje počet služebních cest byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="60b27-223">The last column contains the count of the number of trips completed.</span></span>

### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a><span data-ttu-id="60b27-224">Hodnocení kvality dat: ověřit záznamy s nesprávné délky a šířky</span><span class="sxs-lookup"><span data-stu-id="60b27-224">Data quality assessment: Verify records with incorrect longitude and/or latitude</span></span>
<span data-ttu-id="60b27-225">Tento příklad prověří, pokud jakýkoli z pole zeměpisné délky a šířky buď obsahuje neplatnou hodnotu (radián stupňů musí být mezi -90 a 90), nebo (0, 0) souřadnice.</span><span class="sxs-lookup"><span data-stu-id="60b27-225">This example investigates if any of the longitude and/or latitude fields either contain an invalid value (radian degrees should be between -90 and 90), or have (0, 0) coordinates.</span></span>

    SELECT COUNT(*) FROM <schemaname>.<nyctaxi_trip>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

<span data-ttu-id="60b27-226">**Výstup:** dotaz vrátí 837,467 služebních cest, které mají neplatná pole zeměpisné délky a šířky.</span><span class="sxs-lookup"><span data-stu-id="60b27-226">**Output:** The query returns 837,467 trips that have invalid longitude and/or latitude fields.</span></span>

### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a><span data-ttu-id="60b27-227">Zkoumání: Vysypávány oproti distribuční není šikmý služebních cest</span><span class="sxs-lookup"><span data-stu-id="60b27-227">Exploration: Tipped vs. not tipped trips distribution</span></span>
<span data-ttu-id="60b27-228">Tento příklad vyhledá počet cest, které byly vysypávány oproti číslo, které nebyly vysypávány v zadaném časovém období (nebo v úplné datové sady, pokud vztahující se na úplné rok jako je zde nastavený).</span><span class="sxs-lookup"><span data-stu-id="60b27-228">This example finds the number of trips that were tipped vs. the number that were not tipped in a specified time period (or in the full dataset if covering the full year as it is set up here).</span></span> <span data-ttu-id="60b27-229">Toto rozdělení odráží binární popisek distribuci do později použije pro modelování binární klasifikace.</span><span class="sxs-lookup"><span data-stu-id="60b27-229">This distribution reflects the binary label distribution to be later used for binary classification modeling.</span></span>

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM <schemaname>.<nyctaxi_fare>
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

<span data-ttu-id="60b27-230">**Výstup:** dotaz by měl vrátit následující četnosti tip pro roku 2013: 90,447,622 vysypávány a 82,264,709 vysypávány není.</span><span class="sxs-lookup"><span data-stu-id="60b27-230">**Output:** The query should return the following tip frequencies for the year 2013: 90,447,622 tipped and 82,264,709 not-tipped.</span></span>

### <a name="exploration-tip-classrange-distribution"></a><span data-ttu-id="60b27-231">Zkoumání: Distribuce třídy nebo rozsah Tip</span><span class="sxs-lookup"><span data-stu-id="60b27-231">Exploration: Tip class/range distribution</span></span>
<span data-ttu-id="60b27-232">Tento příklad vypočítá distribuci rozsahy tip v daném časovém období (nebo pokud vztahující se na úplné rok úplné datové sady).</span><span class="sxs-lookup"><span data-stu-id="60b27-232">This example computes the distribution of tip ranges in a given time period (or in the full dataset if covering the full year).</span></span> <span data-ttu-id="60b27-233">Toto je distribuce popisek třídy, které se později použije pro modelování více třídami klasifikace.</span><span class="sxs-lookup"><span data-stu-id="60b27-233">This is the distribution of the label classes that will be used later for multiclass classification modeling.</span></span>

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

<span data-ttu-id="60b27-234">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="60b27-234">**Output:**</span></span>

| <span data-ttu-id="60b27-235">tip_class</span><span class="sxs-lookup"><span data-stu-id="60b27-235">tip_class</span></span> | <span data-ttu-id="60b27-236">tip_freq</span><span class="sxs-lookup"><span data-stu-id="60b27-236">tip_freq</span></span> |
| --- | --- |
| <span data-ttu-id="60b27-237">1</span><span class="sxs-lookup"><span data-stu-id="60b27-237">1</span></span> |<span data-ttu-id="60b27-238">82230915</span><span class="sxs-lookup"><span data-stu-id="60b27-238">82230915</span></span> |
| <span data-ttu-id="60b27-239">2</span><span class="sxs-lookup"><span data-stu-id="60b27-239">2</span></span> |<span data-ttu-id="60b27-240">6198803</span><span class="sxs-lookup"><span data-stu-id="60b27-240">6198803</span></span> |
| <span data-ttu-id="60b27-241">3</span><span class="sxs-lookup"><span data-stu-id="60b27-241">3</span></span> |<span data-ttu-id="60b27-242">1932223</span><span class="sxs-lookup"><span data-stu-id="60b27-242">1932223</span></span> |
| <span data-ttu-id="60b27-243">0</span><span class="sxs-lookup"><span data-stu-id="60b27-243">0</span></span> |<span data-ttu-id="60b27-244">82264625</span><span class="sxs-lookup"><span data-stu-id="60b27-244">82264625</span></span> |
| <span data-ttu-id="60b27-245">4</span><span class="sxs-lookup"><span data-stu-id="60b27-245">4</span></span> |<span data-ttu-id="60b27-246">85765</span><span class="sxs-lookup"><span data-stu-id="60b27-246">85765</span></span> |

### <a name="exploration-compute-and-compare-trip-distance"></a><span data-ttu-id="60b27-247">Zkoumání: Výpočetní a porovnání vzdálenost cesty</span><span class="sxs-lookup"><span data-stu-id="60b27-247">Exploration: Compute and compare trip distance</span></span>
<span data-ttu-id="60b27-248">Tento příklad převede vyzvednutí a odkládací zeměpisné délky a šířky na SQL geography body, vypočítá pomocí SQL geography body rozdíl vzdálenost cestě a vrátí z náhodného vzorku výsledky pro porovnání.</span><span class="sxs-lookup"><span data-stu-id="60b27-248">This example converts the pickup and drop-off longitude and latitude to SQL geography points, computes the trip distance using SQL geography points difference, and returns a random sample of the results for comparison.</span></span> <span data-ttu-id="60b27-249">V příkladu omezí výsledky do platná souřadnice jenom pomocí dotazu hodnocení kvality dat popsané výše.</span><span class="sxs-lookup"><span data-stu-id="60b27-249">The example limits the results to valid coordinates only using the data quality assessment query covered earlier.</span></span>

    /****** Object:  UserDefinedFunction [dbo].[fnCalculateDistance] ******/
    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function to calculate the direct distance  in mile between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
          DECLARE @distance decimal(28, 10)
          -- Convert to radians
          SET @Lat1 = @Lat1 / 57.2958
          SET @Long1 = @Long1 / 57.2958
          SET @Lat2 = @Lat2 / 57.2958
          SET @Long2 = @Long2 / 57.2958
          -- Calculate distance
          SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
          --Convert to miles
          IF @distance <> 0
          BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
          END
          RETURN @distance
    END
    GO

    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

### <a name="feature-engineering-using-sql-functions"></a><span data-ttu-id="60b27-250">Funkce inženýrství pomocí funkce SQL</span><span class="sxs-lookup"><span data-stu-id="60b27-250">Feature engineering using SQL functions</span></span>
<span data-ttu-id="60b27-251">Funkce SQL v některých případech může být efektivní možnost pro funkci inženýrství.</span><span class="sxs-lookup"><span data-stu-id="60b27-251">Sometimes SQL functions can be an efficient option for feature engineering.</span></span> <span data-ttu-id="60b27-252">V tomto návodu jsme definovali funkce SQL k výpočtu přímé vzdálenost mezi vyzvednutí a dropoff umístění.</span><span class="sxs-lookup"><span data-stu-id="60b27-252">In this walkthrough, we defined a SQL function to calculate the direct distance between the pickup and dropoff locations.</span></span> <span data-ttu-id="60b27-253">Spuštěním následujících skriptů SQL v **Data nástroje sady Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="60b27-253">You can run the following SQL scripts in **Visual Studio Data Tools**.</span></span>

<span data-ttu-id="60b27-254">Zde je skript SQL, který definuje funkci vzdálenost.</span><span class="sxs-lookup"><span data-stu-id="60b27-254">Here is the SQL script that defines the distance function.</span></span>

    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function calculate the direct distance between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
          DECLARE @distance decimal(28, 10)
          -- Convert to radians
          SET @Lat1 = @Lat1 / 57.2958
          SET @Long1 = @Long1 / 57.2958
          SET @Lat2 = @Lat2 / 57.2958
          SET @Long2 = @Long2 / 57.2958
          -- Calculate distance
          SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
          --Convert to miles
          IF @distance <> 0
          BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
          END
          RETURN @distance
    END
    GO

<span data-ttu-id="60b27-255">Tady je příklad pro volání této funkce pro generování funkcí v dotazu SQL:</span><span class="sxs-lookup"><span data-stu-id="60b27-255">Here is an example to call this function to generate features in your SQL query:</span></span>

    -- Sample query to call the function to create features
    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

<span data-ttu-id="60b27-256">**Výstup:** tento dotaz vygeneruje tabulku (s 2,803,538 řádky) s vyzvednutí a dropoff zeměpisné šířky a stupně zeměpisné délky a odpovídající přímé vzdálenosti v paliva.</span><span class="sxs-lookup"><span data-stu-id="60b27-256">**Output:** This query generates a table (with 2,803,538 rows) with pickup and dropoff latitudes and longitudes and the corresponding direct distances in miles.</span></span> <span data-ttu-id="60b27-257">Zde jsou výsledky pro první 3 řádky:</span><span class="sxs-lookup"><span data-stu-id="60b27-257">Here are the results for first 3 rows:</span></span>

|  | <span data-ttu-id="60b27-258">pickup_latitude</span><span class="sxs-lookup"><span data-stu-id="60b27-258">pickup_latitude</span></span> | <span data-ttu-id="60b27-259">pickup_longitude</span><span class="sxs-lookup"><span data-stu-id="60b27-259">pickup_longitude</span></span> | <span data-ttu-id="60b27-260">dropoff_latitude</span><span class="sxs-lookup"><span data-stu-id="60b27-260">dropoff_latitude</span></span> | <span data-ttu-id="60b27-261">dropoff_longitude</span><span class="sxs-lookup"><span data-stu-id="60b27-261">dropoff_longitude</span></span> | <span data-ttu-id="60b27-262">DirectDistance</span><span class="sxs-lookup"><span data-stu-id="60b27-262">DirectDistance</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="60b27-263">1</span><span class="sxs-lookup"><span data-stu-id="60b27-263">1</span></span> |<span data-ttu-id="60b27-264">40.731804</span><span class="sxs-lookup"><span data-stu-id="60b27-264">40.731804</span></span> |<span data-ttu-id="60b27-265">-74.001083</span><span class="sxs-lookup"><span data-stu-id="60b27-265">-74.001083</span></span> |<span data-ttu-id="60b27-266">40.736622</span><span class="sxs-lookup"><span data-stu-id="60b27-266">40.736622</span></span> |<span data-ttu-id="60b27-267">-73.988953</span><span class="sxs-lookup"><span data-stu-id="60b27-267">-73.988953</span></span> |<span data-ttu-id="60b27-268">.7169601222</span><span class="sxs-lookup"><span data-stu-id="60b27-268">.7169601222</span></span> |
| <span data-ttu-id="60b27-269">2</span><span class="sxs-lookup"><span data-stu-id="60b27-269">2</span></span> |<span data-ttu-id="60b27-270">40.715794</span><span class="sxs-lookup"><span data-stu-id="60b27-270">40.715794</span></span> |<span data-ttu-id="60b27-271">-74,010635</span><span class="sxs-lookup"><span data-stu-id="60b27-271">-74,010635</span></span> |<span data-ttu-id="60b27-272">40.725338</span><span class="sxs-lookup"><span data-stu-id="60b27-272">40.725338</span></span> |<span data-ttu-id="60b27-273">-74.00399</span><span class="sxs-lookup"><span data-stu-id="60b27-273">-74.00399</span></span> |<span data-ttu-id="60b27-274">.7448343721</span><span class="sxs-lookup"><span data-stu-id="60b27-274">.7448343721</span></span> |
| <span data-ttu-id="60b27-275">3</span><span class="sxs-lookup"><span data-stu-id="60b27-275">3</span></span> |<span data-ttu-id="60b27-276">40.761456</span><span class="sxs-lookup"><span data-stu-id="60b27-276">40.761456</span></span> |<span data-ttu-id="60b27-277">-73.999886</span><span class="sxs-lookup"><span data-stu-id="60b27-277">-73.999886</span></span> |<span data-ttu-id="60b27-278">40.766544</span><span class="sxs-lookup"><span data-stu-id="60b27-278">40.766544</span></span> |<span data-ttu-id="60b27-279">-73.988228</span><span class="sxs-lookup"><span data-stu-id="60b27-279">-73.988228</span></span> |<span data-ttu-id="60b27-280">0.7037227967</span><span class="sxs-lookup"><span data-stu-id="60b27-280">0.7037227967</span></span> |

### <a name="prepare-data-for-model-building"></a><span data-ttu-id="60b27-281">Příprava dat pro vytváření modelů</span><span class="sxs-lookup"><span data-stu-id="60b27-281">Prepare data for model building</span></span>
<span data-ttu-id="60b27-282">Následující dotaz spojení **nyctaxi\_cestě** a **nyctaxi\_tarif** tabulky, vygeneruje štítek binární klasifikace **vysypávány**, Popisek více třída klasifikace **tip\_třída**a extrahuje ukázku z připojeného k úplné datové sadě.</span><span class="sxs-lookup"><span data-stu-id="60b27-282">The following query joins the **nyctaxi\_trip** and **nyctaxi\_fare** tables, generates a binary classification label **tipped**, a multi-class classification label **tip\_class**, and extracts a sample from the full joined dataset.</span></span> <span data-ttu-id="60b27-283">Je potřeba vzorkuje načítání podmnožinu služebních cest, na základě výstupní času.</span><span class="sxs-lookup"><span data-stu-id="60b27-283">The sampling is done by retrieving a subset of the trips based on pickup time.</span></span>  <span data-ttu-id="60b27-284">Tento dotaz můžete zkopírovat a vložit přímo v [Azure Machine Learning Studio](https://studio.azureml.net) [importovat Data] [ import-data] modul pro přijímání přímé dat z instance databáze SQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="60b27-284">This query can be copied then pasted directly in the [Azure Machine Learning Studio](https://studio.azureml.net) [Import Data][import-data] module for direct data ingestion from the SQL database instance in Azure.</span></span> <span data-ttu-id="60b27-285">Dotaz vyloučí záznamy s nesprávnou (0, 0) souřadnice.</span><span class="sxs-lookup"><span data-stu-id="60b27-285">The query excludes records with incorrect (0, 0) coordinates.</span></span>

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,     f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
    WHERE datepart("mi",t.pickup_datetime) = 1
    AND   t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

<span data-ttu-id="60b27-286">Jakmile budete připraveni pokračovat do Azure Machine Learning, můžete se buď:</span><span class="sxs-lookup"><span data-stu-id="60b27-286">When you are ready to proceed to Azure Machine Learning, you may either:</span></span>  

1. <span data-ttu-id="60b27-287">Poslední dotaz SQL k extrahování a ukázková data a způsobené kopírováním a vkládáním dotaz přímo do uložit [importovat Data] [ import-data] modulu v Azure Machine Learning, nebo</span><span class="sxs-lookup"><span data-stu-id="60b27-287">Save the final SQL query to extract and sample the data and copy-paste the query directly into a [Import Data][import-data] module in Azure Machine Learning, or</span></span>
2. <span data-ttu-id="60b27-288">Zachovat data jen Vzorkovaná a inženýrství máte v úmyslu použít pro model vytváření v nové tabulce SQL DW a použít nové tabulky v [importovat Data] [ import-data] modulu v Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="60b27-288">Persist the sampled and engineered data you plan to use for model building in a new SQL DW table and use the new table in the [Import Data][import-data] module in Azure Machine Learning.</span></span> <span data-ttu-id="60b27-289">Můžete to bylo dokončeno skript prostředí PowerShell v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="60b27-289">The PowerShell script in earlier step has done this for you.</span></span> <span data-ttu-id="60b27-290">Si můžete přečíst přímo z této tabulky v modulu, importovat Data.</span><span class="sxs-lookup"><span data-stu-id="60b27-290">You can read directly from this table in the Import Data module.</span></span>

## <span data-ttu-id="60b27-291"><a name="ipnb"></a>Zkoumání dat a funkce technikům v poznámkovém bloku IPython</span><span class="sxs-lookup"><span data-stu-id="60b27-291"><a name="ipnb"></a>Data exploration and feature engineering in IPython notebook</span></span>
<span data-ttu-id="60b27-292">V této části provedeme zkoumání dat a funkce generování pomocí obou Python a dotazy SQL pro SQL DW vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="60b27-292">In this section, we will perform data exploration and feature generation using both Python and SQL queries against the SQL DW created earlier.</span></span> <span data-ttu-id="60b27-293">Ukázka IPython Poznámkový blok s názvem **SQLDW_Explorations.ipynb** a soubor skriptu jazyka Python **SQLDW_Explorations_Scripts.py** byly staženy do vašeho místního adresáře.</span><span class="sxs-lookup"><span data-stu-id="60b27-293">A sample IPython notebook named **SQLDW_Explorations.ipynb** and a Python script file **SQLDW_Explorations_Scripts.py** have been downloaded to your local directory.</span></span> <span data-ttu-id="60b27-294">Jsou k dispozici na [Githubu](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW).</span><span class="sxs-lookup"><span data-stu-id="60b27-294">They are also available on [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW).</span></span> <span data-ttu-id="60b27-295">Tyto dva soubory jsou identické v skriptů Python.</span><span class="sxs-lookup"><span data-stu-id="60b27-295">These two files are identical in Python scripts.</span></span> <span data-ttu-id="60b27-296">Soubor skriptu jazyka Python je které jste získali v případě, že jste k serveru IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="60b27-296">The Python script file is provided to you in case you do not have an IPython Notebook server.</span></span> <span data-ttu-id="60b27-297">Tyto dva ukázkové soubory jsou navrženy v části Python **Python 2.7**.</span><span class="sxs-lookup"><span data-stu-id="60b27-297">These two sample Python files are designed under **Python 2.7**.</span></span>

<span data-ttu-id="60b27-298">Azure SQL DW potřebné informace v ukázce IPython Poznámkový blok a souboru skriptu jazyka Python stahovat do místního počítače je zapojen skript prostředí PowerShell dříve.</span><span class="sxs-lookup"><span data-stu-id="60b27-298">The needed Azure SQL DW information in the sample IPython Notebook and the Python script file downloaded to your local machine has been plugged in by the PowerShell script previously.</span></span> <span data-ttu-id="60b27-299">Jsou spustitelné bez nutnosti jakékoli úpravy.</span><span class="sxs-lookup"><span data-stu-id="60b27-299">They are executable without any modification.</span></span>

<span data-ttu-id="60b27-300">Pokud již jste vytvořili pracovní prostor služby AzureML, můžete přímo odeslání vzorku IPython Poznámkový blok službě AzureML IPython Poznámkový blok a spustit ho.</span><span class="sxs-lookup"><span data-stu-id="60b27-300">If you have already set up an AzureML workspace, you can directly upload the sample IPython Notebook to the AzureML IPython Notebook service and start running it.</span></span> <span data-ttu-id="60b27-301">Zde jsou kroky pro odeslání službě AzureML IPython Poznámkový blok:</span><span class="sxs-lookup"><span data-stu-id="60b27-301">Here are the steps to upload to AzureML IPython Notebook service:</span></span>

1. <span data-ttu-id="60b27-302">Přihlaste se do pracovního prostoru AzureML, klikněte na tlačítko "Studio" v horní části a klikněte na "Notebooky" na levé straně webové stránky.</span><span class="sxs-lookup"><span data-stu-id="60b27-302">Log in to your AzureML workspace, click "Studio" at the top, and click "NOTEBOOKS" on the left side of the web page.</span></span>
   
    ![Vykreslení #22][22]
2. <span data-ttu-id="60b27-304">Klikněte v levém dolním rohu webová stránka "NEW" a vyberte "Python 2".</span><span class="sxs-lookup"><span data-stu-id="60b27-304">Click "NEW" on the left bottom corner of the web page, and select "Python 2".</span></span> <span data-ttu-id="60b27-305">Potom zadejte název do poznámkového bloku a klikněte na tlačítko zaškrtnutí pro vytvoření nové prázdné IPython poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="60b27-305">Then, provide a name to the notebook and click the check mark to create the new blank IPython Notebook.</span></span>
   
    ![Vykreslení #23][23]
3. <span data-ttu-id="60b27-307">Kliknutím na symbol "Jupyter" v levém horním rohu nový poznámkový blok IPython.</span><span class="sxs-lookup"><span data-stu-id="60b27-307">Click the "Jupyter" symbol on the left top corner of the new IPython Notebook.</span></span>
   
    ![Vykreslení #24][24]
4. <span data-ttu-id="60b27-309">Přetažení ukázka IPython Poznámkový blok k **stromu** služby AzureML IPython Poznámkový blok a klikněte na tlačítko **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="60b27-309">Drag and drop the sample IPython Notebook to the **tree** page of your AzureML IPython Notebook service, and click **Upload**.</span></span> <span data-ttu-id="60b27-310">Potom ukázka IPython Poznámkový blok, nebude možné odesílat ke službě AzureML IPython poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="60b27-310">Then, the sample IPython Notebook will be uploaded to the AzureML IPython Notebook service.</span></span>
   
    ![Vykreslení #25][25]

<span data-ttu-id="60b27-312">Aby bylo možné spustit ukázku soubor, následující Python, balíčky jsou nutné skriptu IPython Poznámkový blok nebo Python.</span><span class="sxs-lookup"><span data-stu-id="60b27-312">In order to run the sample IPython Notebook or the Python script file, the following Python packages are needed.</span></span> <span data-ttu-id="60b27-313">Pokud používáte službu AzureML IPython Poznámkový blok, byly tyto balíčky předem nainstalovaná.</span><span class="sxs-lookup"><span data-stu-id="60b27-313">If you are using the AzureML IPython Notebook service, these packages have been pre-installed.</span></span>

    - <span data-ttu-id="60b27-314">pandas</span><span class="sxs-lookup"><span data-stu-id="60b27-314">pandas</span></span>
    - <span data-ttu-id="60b27-315">numpy</span><span class="sxs-lookup"><span data-stu-id="60b27-315">numpy</span></span>
    - <span data-ttu-id="60b27-316">matplotlib</span><span class="sxs-lookup"><span data-stu-id="60b27-316">matplotlib</span></span>
    - <span data-ttu-id="60b27-317">pyodbc</span><span class="sxs-lookup"><span data-stu-id="60b27-317">pyodbc</span></span>
    - <span data-ttu-id="60b27-318">PyTables</span><span class="sxs-lookup"><span data-stu-id="60b27-318">PyTables</span></span>

<span data-ttu-id="60b27-319">Doporučené pořadí při sestavování Pokročilá analytická řešení na AzureML s velkých objemů dat je následující:</span><span class="sxs-lookup"><span data-stu-id="60b27-319">The recommended sequence when building advanced analytical solutions on AzureML with large data is the following:</span></span>

* <span data-ttu-id="60b27-320">Přečtěte si v malé ukázkové dat do data v paměti rámečku.</span><span class="sxs-lookup"><span data-stu-id="60b27-320">Read in a small sample of the data into an in-memory data frame.</span></span>
* <span data-ttu-id="60b27-321">Proveďte některé vizualizace a explorations pomocí jen Vzorkovaná data.</span><span class="sxs-lookup"><span data-stu-id="60b27-321">Perform some visualizations and explorations using the sampled data.</span></span>
* <span data-ttu-id="60b27-322">Experimentujte s inženýrství funkce pomocí jen Vzorkovaná data.</span><span class="sxs-lookup"><span data-stu-id="60b27-322">Experiment with feature engineering using the sampled data.</span></span>
* <span data-ttu-id="60b27-323">Pro větší zkoumání dat, manipulaci s daty a funkce technikům používejte jazyk Python vydávat dotazy SQL na přímo pro SQL DW.</span><span class="sxs-lookup"><span data-stu-id="60b27-323">For larger data exploration, data manipulation and feature engineering, use Python to issue SQL Queries directly against the SQL DW.</span></span>
* <span data-ttu-id="60b27-324">Rozhodněte, velikost vzorku být vhodný pro vytváření modelů Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="60b27-324">Decide the sample size to be suitable for Azure Machine Learning model building.</span></span>

<span data-ttu-id="60b27-325">Tady je několik zkoumání dat, vizualizace dat a funkce technici příklady.</span><span class="sxs-lookup"><span data-stu-id="60b27-325">The followings are a few data exploration, data visualization, and feature engineering examples.</span></span> <span data-ttu-id="60b27-326">Další data explorations naleznete v ukázce IPython Poznámkový blok a ukázkový soubor skriptu jazyka Python.</span><span class="sxs-lookup"><span data-stu-id="60b27-326">More data explorations can be found in the sample IPython Notebook and the sample Python script file.</span></span>

### <a name="initialize-database-credentials"></a><span data-ttu-id="60b27-327">Inicializace přihlašovací údaje databáze</span><span class="sxs-lookup"><span data-stu-id="60b27-327">Initialize database credentials</span></span>
<span data-ttu-id="60b27-328">Inicializace nastavení připojení k databázi v následující proměnné:</span><span class="sxs-lookup"><span data-stu-id="60b27-328">Initialize your database connection settings in the following variables:</span></span>

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database driver>

### <a name="create-database-connection"></a><span data-ttu-id="60b27-329">Vytvoření připojení k databázi</span><span class="sxs-lookup"><span data-stu-id="60b27-329">Create database connection</span></span>
<span data-ttu-id="60b27-330">Tady je připojovací řetězec, který vytvoří připojení k databázi.</span><span class="sxs-lookup"><span data-stu-id="60b27-330">Here is the connection string that creates the connection to the database.</span></span>

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a><span data-ttu-id="60b27-331">Sestava počtu řádků a sloupců v tabulce < nyctaxi_trip ></span><span class="sxs-lookup"><span data-stu-id="60b27-331">Report number of rows and columns in table <nyctaxi_trip></span></span>
    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_trip>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

* <span data-ttu-id="60b27-332">Celkový počet řádků = 173179759</span><span class="sxs-lookup"><span data-stu-id="60b27-332">Total number of rows = 173179759</span></span>  
* <span data-ttu-id="60b27-333">Celkový počet sloupců = 14</span><span class="sxs-lookup"><span data-stu-id="60b27-333">Total number of columns = 14</span></span>

### <a name="report-number-of-rows-and-columns-in-table-nyctaxifare"></a><span data-ttu-id="60b27-334">Sestava počtu řádků a sloupců v tabulce < nyctaxi_fare ></span><span class="sxs-lookup"><span data-stu-id="60b27-334">Report number of rows and columns in table <nyctaxi_fare></span></span>
    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_fare>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_fare>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

* <span data-ttu-id="60b27-335">Celkový počet řádků = 173179759</span><span class="sxs-lookup"><span data-stu-id="60b27-335">Total number of rows = 173179759</span></span>  
* <span data-ttu-id="60b27-336">Celkový počet sloupců = 11</span><span class="sxs-lookup"><span data-stu-id="60b27-336">Total number of columns = 11</span></span>

### <a name="read-in-a-small-data-sample-from-the-sql-data-warehouse-database"></a><span data-ttu-id="60b27-337">Pro čtení ve vzorku malá data z databáze SQL datového skladu</span><span class="sxs-lookup"><span data-stu-id="60b27-337">Read-in a small data sample from the SQL Data Warehouse Database</span></span>
    t0 = time.time()

    query = '''
        SELECT TOP 10000 t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
        WHERE datepart("mi",t.pickup_datetime) = 1
        AND   t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

<span data-ttu-id="60b27-338">Čas číst že ukázkové tabulky je 14.096495 sekund.</span><span class="sxs-lookup"><span data-stu-id="60b27-338">Time to read the sample table is 14.096495 seconds.</span></span>  
<span data-ttu-id="60b27-339">Počet řádků a sloupců načíst = (1000, 21).</span><span class="sxs-lookup"><span data-stu-id="60b27-339">Number of rows and columns retrieved = (1000, 21).</span></span>

### <a name="descriptive-statistics"></a><span data-ttu-id="60b27-340">Popisný statistiky</span><span class="sxs-lookup"><span data-stu-id="60b27-340">Descriptive statistics</span></span>
<span data-ttu-id="60b27-341">Nyní jste připraveni na zkoumání jen Vzorkovaná data.</span><span class="sxs-lookup"><span data-stu-id="60b27-341">Now you are ready to explore the sampled data.</span></span> <span data-ttu-id="60b27-342">Začneme s prohlížení statistikami popisný pro **cestě\_vzdálenost** (nebo všechna pole, které určíte).</span><span class="sxs-lookup"><span data-stu-id="60b27-342">We start with looking at some descriptive statistics for the **trip\_distance** (or any other fields you choose to specify).</span></span>

    df1['trip_distance'].describe()

### <a name="visualization-box-plot-example"></a><span data-ttu-id="60b27-343">Vizualizace: Příklad vykreslení pole</span><span class="sxs-lookup"><span data-stu-id="60b27-343">Visualization: Box plot example</span></span>
<span data-ttu-id="60b27-344">Další podíváme na Krabicový pro vzdálenost cesty k vizualizaci quantiles.</span><span class="sxs-lookup"><span data-stu-id="60b27-344">Next we look at the box plot for the trip distance to visualize the quantiles.</span></span>

    df1.boxplot(column='trip_distance',return_type='dict')

![Vykreslení #1][1]

### <a name="visualization-distribution-plot-example"></a><span data-ttu-id="60b27-346">Vizualizaci: Příklad vykreslení distribuční</span><span class="sxs-lookup"><span data-stu-id="60b27-346">Visualization: Distribution plot example</span></span>
<span data-ttu-id="60b27-347">Pozemků, které vizualizovat distribuce a histogram vzdálenosti jen Vzorkovaná cesty.</span><span class="sxs-lookup"><span data-stu-id="60b27-347">Plots that visualize the distribution and a histogram for the sampled trip distances.</span></span>

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Vykreslení #2][2]

### <a name="visualization-bar-and-line-plots"></a><span data-ttu-id="60b27-349">Vizualizace: Panel a ukazuje zeměpisný řádku</span><span class="sxs-lookup"><span data-stu-id="60b27-349">Visualization: Bar and line plots</span></span>
<span data-ttu-id="60b27-350">V tomto příkladu jsme bin vzdálenost cesty do pěti přihrádek a vizualizace binning výsledků.</span><span class="sxs-lookup"><span data-stu-id="60b27-350">In this example, we bin the trip distance into five bins and visualize the binning results.</span></span>

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

<span data-ttu-id="60b27-351">Jsme můžete vykreslení výše uvedené distribuční bin panelu nebo výkresu s řádek:</span><span class="sxs-lookup"><span data-stu-id="60b27-351">We can plot the above bin distribution in a bar or line plot with:</span></span>

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Vykreslení #3][3]

<span data-ttu-id="60b27-353">a</span><span class="sxs-lookup"><span data-stu-id="60b27-353">and</span></span>

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Vykreslení #4][4]

### <a name="visualization-scatterplot-examples"></a><span data-ttu-id="60b27-355">Vizualizaci: Příklady Scatterplot</span><span class="sxs-lookup"><span data-stu-id="60b27-355">Visualization: Scatterplot examples</span></span>
<span data-ttu-id="60b27-356">Ukážeme bodové vykreslení mezi **cestě\_čas\_v\_sekundy** a **cestě\_vzdálenost** jestli jsou všechny korelace</span><span class="sxs-lookup"><span data-stu-id="60b27-356">We show scatter plot between **trip\_time\_in\_secs** and **trip\_distance** to see if there is any correlation</span></span>

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Vykreslení #6][6]

<span data-ttu-id="60b27-358">Podobně jsme můžete zkontrolovat vztah mezi **míra\_kód** a **cestě\_vzdálenost**.</span><span class="sxs-lookup"><span data-stu-id="60b27-358">Similarly we can check the relationship between **rate\_code** and **trip\_distance**.</span></span>

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![Vykreslení #8][8]

### <a name="data-exploration-on-sampled-data-using-sql-queries-in-ipython-notebook"></a><span data-ttu-id="60b27-360">Zkoumání dat na jen Vzorkovaná data pomocí dotazů SQL v poznámkovém bloku IPython</span><span class="sxs-lookup"><span data-stu-id="60b27-360">Data exploration on sampled data using SQL queries in IPython notebook</span></span>
<span data-ttu-id="60b27-361">V této části nám prozkoumat distribuce dat pomocí jen Vzorkovaná data, která je uchován v nové tabulky, kterou jsme vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="60b27-361">In this section, we explore data distributions using the sampled data which is persisted in the new table we created above.</span></span> <span data-ttu-id="60b27-362">Všimněte si, že podobné explorations je možné provést pomocí původní tabulky.</span><span class="sxs-lookup"><span data-stu-id="60b27-362">Note that similar explorations can be performed using the original tables.</span></span>

#### <a name="exploration-report-number-of-rows-and-columns-in-the-sampled-table"></a><span data-ttu-id="60b27-363">Zkoumání: Počet řádků a sloupců v tabulce jen Vzorkovaná sestavy</span><span class="sxs-lookup"><span data-stu-id="60b27-363">Exploration: Report number of rows and columns in the sampled table</span></span>
    nrows = pd.read_sql('''SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_sample>')''', conn)
    print 'Number of rows in sample = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''SELECT count(*) FROM information_schema.columns WHERE table_name = ('<nyctaxi_sample>') AND table_schema = '<schemaname>'''', conn)
    print 'Number of columns in sample = %d' % ncols.iloc[0,0]

#### <a name="exploration-tippednot-tripped-distribution"></a><span data-ttu-id="60b27-364">Zkoumání: Vysypávány nebo nebyla přerušovačů distribuce</span><span class="sxs-lookup"><span data-stu-id="60b27-364">Exploration: Tipped/not tripped Distribution</span></span>
    query = '''
        SELECT tipped, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tipped
        '''

    pd.read_sql(query, conn)

#### <a name="exploration-tip-class-distribution"></a><span data-ttu-id="60b27-365">Zkoumání: Distribuce třída Tip</span><span class="sxs-lookup"><span data-stu-id="60b27-365">Exploration: Tip class distribution</span></span>
    query = '''
        SELECT tip_class, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tip_class
    '''

    tip_class_dist = pd.read_sql(query, conn)

#### <a name="exploration-plot-the-tip-distribution-by-class"></a><span data-ttu-id="60b27-366">Zkoumání: Vykreslení distribuční tip třídou</span><span class="sxs-lookup"><span data-stu-id="60b27-366">Exploration: Plot the tip distribution by class</span></span>
    tip_class_dist['tip_freq'].plot(kind='bar')

![Vykreslení #26][26]

#### <a name="exploration-daily-distribution-of-trips"></a><span data-ttu-id="60b27-368">Zkoumání: Denní distribuce služebních cest</span><span class="sxs-lookup"><span data-stu-id="60b27-368">Exploration: Daily distribution of trips</span></span>
    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a><span data-ttu-id="60b27-369">Zkoumání: Distribuční cesty za Medailon</span><span class="sxs-lookup"><span data-stu-id="60b27-369">Exploration: Trip distribution per medallion</span></span>
    query = '''
        SELECT medallion,count(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a><span data-ttu-id="60b27-370">Zkoumání: Cestě distribuce podle Medailon a hackerský licencí</span><span class="sxs-lookup"><span data-stu-id="60b27-370">Exploration: Trip distribution by medallion and hack license</span></span>
    query = '''select medallion, hack_license,count(*) from <schemaname>.<nyctaxi_sample> group by medallion, hack_license'''
    pd.read_sql(query,conn)


#### <a name="exploration-trip-time-distribution"></a><span data-ttu-id="60b27-371">Zkoumání: Distribuce doby cestě</span><span class="sxs-lookup"><span data-stu-id="60b27-371">Exploration: Trip time distribution</span></span>
    query = '''select trip_time_in_secs, count(*) from <schemaname>.<nyctaxi_sample> group by trip_time_in_secs order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-trip-distance-distribution"></a><span data-ttu-id="60b27-372">Zkoumání: Distribuce vzdálenost cestě</span><span class="sxs-lookup"><span data-stu-id="60b27-372">Exploration: Trip distance distribution</span></span>
    query = '''select floor(trip_distance/5)*5 as tripbin, count(*) from <schemaname>.<nyctaxi_sample> group by floor(trip_distance/5)*5 order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-payment-type-distribution"></a><span data-ttu-id="60b27-373">Zkoumání: Distribuce typ platby</span><span class="sxs-lookup"><span data-stu-id="60b27-373">Exploration: Payment type distribution</span></span>
    query = '''select payment_type,count(*) from <schemaname>.<nyctaxi_sample> group by payment_type'''
    pd.read_sql(query,conn)

#### <a name="verify-the-final-form-of-the-featurized-table"></a><span data-ttu-id="60b27-374">Ověření konečné formuláře featurized tabulky</span><span class="sxs-lookup"><span data-stu-id="60b27-374">Verify the final form of the featurized table</span></span>
    query = '''SELECT TOP 100 * FROM <schemaname>.<nyctaxi_sample>'''
    pd.read_sql(query,conn)

## <span data-ttu-id="60b27-375"><a name="mlmodel"></a>Vytvářet modely v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="60b27-375"><a name="mlmodel"></a>Build models in Azure Machine Learning</span></span>
<span data-ttu-id="60b27-376">Snažíme se teď přejít k vytváření modelů a modelu nasazení v [Azure Machine Learning](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="60b27-376">We are now ready to proceed to model building and model deployment in [Azure Machine Learning](https://studio.azureml.net).</span></span> <span data-ttu-id="60b27-377">Data jsou připravené k použití v některém z identifikované dříve, a to předpovědi problémy:</span><span class="sxs-lookup"><span data-stu-id="60b27-377">The data is ready to be used in any of the prediction problems identified earlier, namely:</span></span>

1. <span data-ttu-id="60b27-378">**Binární klasifikace**: K předvídání, jestli tip byl placené cesty.</span><span class="sxs-lookup"><span data-stu-id="60b27-378">**Binary classification**: To predict whether or not a tip was paid for a trip.</span></span>
2. <span data-ttu-id="60b27-379">**Více třídami klasifikace**: K předpovědi rozsahu tipu placené podle dříve definovaných tříd.</span><span class="sxs-lookup"><span data-stu-id="60b27-379">**Multiclass classification**: To predict the range of tip paid, according to the previously defined classes.</span></span>
3. <span data-ttu-id="60b27-380">**Úloha regrese**: K předvídání množství tip placené cesty.</span><span class="sxs-lookup"><span data-stu-id="60b27-380">**Regression task**: To predict the amount of tip paid for a trip.</span></span>  

<span data-ttu-id="60b27-381">Pokud chcete začít cvičení modelování, přihlaste se k vaší **Azure Machine Learning** pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="60b27-381">To begin the modeling exercise, log in to your **Azure Machine Learning** workspace.</span></span> <span data-ttu-id="60b27-382">Pokud jste ještě nevytvořili machine learning pracovního prostoru, přečtěte si téma [vytvořit pracovní prostor služby Azure ML](machine-learning-create-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="60b27-382">If you have not yet created a machine learning workspace, see [Create an Azure ML workspace](machine-learning-create-workspace.md).</span></span>

1. <span data-ttu-id="60b27-383">Chcete-li začít s Azure Machine Learning, přečtěte si téma [co je Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)</span><span class="sxs-lookup"><span data-stu-id="60b27-383">To get started with Azure Machine Learning, see [What is Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)</span></span>
2. <span data-ttu-id="60b27-384">Přihlaste se k [Azure Machine Learning Studio](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="60b27-384">Log in to [Azure Machine Learning Studio](https://studio.azureml.net).</span></span>
3. <span data-ttu-id="60b27-385">Na domovskou stránku Studio poskytuje širokou řadu informací, videa, kurzy, odkazů na odkaz na moduly a dalším prostředkům.</span><span class="sxs-lookup"><span data-stu-id="60b27-385">The Studio Home page provides a wealth of information, videos, tutorials, links to the Modules Reference, and other resources.</span></span> <span data-ttu-id="60b27-386">Další informace o Azure Machine Learning, naleznete [centru dokumentace Azure Machine Learning na](https://azure.microsoft.com/documentation/services/machine-learning/).</span><span class="sxs-lookup"><span data-stu-id="60b27-386">For more information about Azure Machine Learning, consult the [Azure Machine Learning Documentation Center](https://azure.microsoft.com/documentation/services/machine-learning/).</span></span>

<span data-ttu-id="60b27-387">Typické výukový experiment sestává z následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="60b27-387">A typical training experiment consists of the following steps:</span></span>

1. <span data-ttu-id="60b27-388">Vytvoření **+ nový** experiment.</span><span class="sxs-lookup"><span data-stu-id="60b27-388">Create a **+NEW** experiment.</span></span>
2. <span data-ttu-id="60b27-389">Získáte data do Azure ML.</span><span class="sxs-lookup"><span data-stu-id="60b27-389">Get the data into Azure ML.</span></span>
3. <span data-ttu-id="60b27-390">Předběžně zpracovat, transformace a manipulovat s daty, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="60b27-390">Pre-process, transform and manipulate the data as needed.</span></span>
4. <span data-ttu-id="60b27-391">Funkce vygenerujte, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="60b27-391">Generate features as needed.</span></span>
5. <span data-ttu-id="60b27-392">Rozdělení dat do školení, ověření nebo testování datové sady (nebo samostatné datové sady pro každou).</span><span class="sxs-lookup"><span data-stu-id="60b27-392">Split the data into training/validation/testing datasets(or have separate datasets for each).</span></span>
6. <span data-ttu-id="60b27-393">Vyberte jeden nebo více algoritmy strojového učení v závislosti na learning problém vyřešit.</span><span class="sxs-lookup"><span data-stu-id="60b27-393">Select one or more machine learning algorithms depending on the learning problem to solve.</span></span> <span data-ttu-id="60b27-394">Například: binární klasifikace, klasifikace více třídami, regresi.</span><span class="sxs-lookup"><span data-stu-id="60b27-394">E.g., binary classification, multiclass classification, regression.</span></span>
7. <span data-ttu-id="60b27-395">Cvičení jednoho nebo více modelů použití školení datové sady.</span><span class="sxs-lookup"><span data-stu-id="60b27-395">Train one or more models using the training dataset.</span></span>
8. <span data-ttu-id="60b27-396">Stanovení skóre datovou sadu ověření pomocí vyškolení modely.</span><span class="sxs-lookup"><span data-stu-id="60b27-396">Score the validation dataset using the trained model(s).</span></span>
9. <span data-ttu-id="60b27-397">Vyhodnoťte modely k výpočtu relevantní metriky pro učení problém.</span><span class="sxs-lookup"><span data-stu-id="60b27-397">Evaluate the model(s) to compute the relevant metrics for the learning problem.</span></span>
10. <span data-ttu-id="60b27-398">Bez problémů ladit modely a vyberte doporučené modelu pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="60b27-398">Fine tune the model(s) and select the best model to deploy.</span></span>

<span data-ttu-id="60b27-399">V tomto cvičení jsme již prozkoumali a analýzou dat v SQL Data Warehouse a ingestování v Azure ML se rozhodli na velikost vzorku.</span><span class="sxs-lookup"><span data-stu-id="60b27-399">In this exercise, we have already explored and engineered the data in SQL Data Warehouse, and decided on the sample size to ingest in Azure ML.</span></span> <span data-ttu-id="60b27-400">Tady je postup vytvoření jednoho nebo více modelů předpovědi:</span><span class="sxs-lookup"><span data-stu-id="60b27-400">Here is the procedure to build one or more of the prediction models:</span></span>

1. <span data-ttu-id="60b27-401">Získat data do aplikace pomocí Azure ML [importovat Data] [ import-data] modulu, k dispozici v **vstupu a výstupu dat** části.</span><span class="sxs-lookup"><span data-stu-id="60b27-401">Get the data into Azure ML using the [Import Data][import-data] module, available in the **Data Input and Output** section.</span></span> <span data-ttu-id="60b27-402">Další informace najdete v tématu [importovat Data] [ import-data] stránka s referencemi modul.</span><span class="sxs-lookup"><span data-stu-id="60b27-402">For more information, see the [Import Data][import-data] module reference page.</span></span>
   
    ![Azure ML Import dat][17]
2. <span data-ttu-id="60b27-404">Vyberte **Azure SQL Database** jako **zdroj dat** v **vlastnosti** panelu.</span><span class="sxs-lookup"><span data-stu-id="60b27-404">Select **Azure SQL Database** as the **Data source** in the **Properties** panel.</span></span>
3. <span data-ttu-id="60b27-405">Zadejte název DNS databáze v **název databázového serveru** pole.</span><span class="sxs-lookup"><span data-stu-id="60b27-405">Enter the database DNS name in the **Database server name** field.</span></span> <span data-ttu-id="60b27-406">Formát:`tcp:<your_virtual_machine_DNS_name>,1433`</span><span class="sxs-lookup"><span data-stu-id="60b27-406">Format: `tcp:<your_virtual_machine_DNS_name>,1433`</span></span>
4. <span data-ttu-id="60b27-407">Zadejte **název databáze** v odpovídajícím poli.</span><span class="sxs-lookup"><span data-stu-id="60b27-407">Enter the **Database name** in the corresponding field.</span></span>
5. <span data-ttu-id="60b27-408">Zadejte *uživatelské jméno SQL* v **název uživatelského účtu serveru**a *heslo* v **heslo uživatelského účtu serveru**.</span><span class="sxs-lookup"><span data-stu-id="60b27-408">Enter the *SQL user name* in the **Server user account name**, and the *password* in the **Server user account password**.</span></span>
6. <span data-ttu-id="60b27-409">Zkontrolujte **přijmout některý z certifikátů serveru** možnost.</span><span class="sxs-lookup"><span data-stu-id="60b27-409">Check the **Accept any server certificate** option.</span></span>
7. <span data-ttu-id="60b27-410">V **databázový dotaz** upravit textová oblast, vložte dotaz, který extrahuje pole potřeby databáze (včetně všech počítané pole, jako je popisků) a nižší ukázky data na požadovanou velikost.</span><span class="sxs-lookup"><span data-stu-id="60b27-410">In the **Database query** edit text area, paste the query which extracts the necessary database fields (including any computed fields such as the labels) and down samples the data to the desired sample size.</span></span>

<span data-ttu-id="60b27-411">V níže uvedeném obrázku je například binární klasifikace experimentů, čtení dat přímo z databáze SQL Data Warehouse (Nezapomeňte nahradit tabulku názvy nyctaxi_trip a nyctaxi_fare tak, že název schématu a názvy tabulek, který jste použili v vaší návod).</span><span class="sxs-lookup"><span data-stu-id="60b27-411">An example of a binary classification experiment reading data directly from the SQL Data Warehouse database is in the figure below (remember to replace the table names nyctaxi_trip and nyctaxi_fare by the schema name and the table names you used in your walkthrough).</span></span> <span data-ttu-id="60b27-412">Podobně jako experimenty konstruovat pro více třídami klasifikaci a regresní problémy.</span><span class="sxs-lookup"><span data-stu-id="60b27-412">Similar experiments can be constructed for multiclass classification and regression problems.</span></span>

![Azure ML Train][10]

> [!IMPORTANT]
> <span data-ttu-id="60b27-414">V datech modelování extrakce a vzorkuje příklady dotazu zadaný v předchozích částech **všech popisků tři modelování cvičení jsou zahrnuty v dotazu**.</span><span class="sxs-lookup"><span data-stu-id="60b27-414">In the modeling data extraction and sampling query examples provided in previous sections, **all labels for the three modeling exercises are included in the query**.</span></span> <span data-ttu-id="60b27-415">Důležitým krokem (povinné) v každé cvičení modelování je **vyloučit** nepotřebné štítky pro dva problémy a všemi ostatními **cíle nevracení**.</span><span class="sxs-lookup"><span data-stu-id="60b27-415">An important (required) step in each of the modeling exercises is to **exclude** the unnecessary labels for the other two problems, and any other **target leaks**.</span></span> <span data-ttu-id="60b27-416">Například při použití binární klasifikace, použít popisek **vysypávány** a vyloučit pole **tip\_třída**, **tip\_velikost**a **celkový\_velikost**.</span><span class="sxs-lookup"><span data-stu-id="60b27-416">For example, when using binary classification, use the label **tipped** and exclude the fields **tip\_class**, **tip\_amount**, and **total\_amount**.</span></span> <span data-ttu-id="60b27-417">K tomu jsou cílové nevracení vzhledem k tomu, že implikují tip placené.</span><span class="sxs-lookup"><span data-stu-id="60b27-417">The latter are target leaks since they imply the tip paid.</span></span>
> 
> <span data-ttu-id="60b27-418">Vyloučit všechny nepotřebné sloupce nebo cíle nevracení, můžete použít [výběr sloupců v datové sadě] [ select-columns] modulu nebo [upravit Metadata][edit-metadata].</span><span class="sxs-lookup"><span data-stu-id="60b27-418">To exclude any unnecessary columns or target leaks, you may use the [Select Columns in Dataset][select-columns] module or the [Edit Metadata][edit-metadata].</span></span> <span data-ttu-id="60b27-419">Další informace najdete v tématu [výběr sloupců v datové sadě] [ select-columns] a [upravit Metadata] [ edit-metadata] referenční stránky.</span><span class="sxs-lookup"><span data-stu-id="60b27-419">For more information, see [Select Columns in Dataset][select-columns] and [Edit Metadata][edit-metadata] reference pages.</span></span>
> 
> 

## <span data-ttu-id="60b27-420"><a name="mldeploy"></a>Nasazení modely v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="60b27-420"><a name="mldeploy"></a>Deploy models in Azure Machine Learning</span></span>
<span data-ttu-id="60b27-421">Pokud váš model je připraven, můžete snadno nasadit ho jako webovou službu přímo z experimentu.</span><span class="sxs-lookup"><span data-stu-id="60b27-421">When your model is ready, you can easily deploy it as a web service directly from the experiment.</span></span> <span data-ttu-id="60b27-422">Další informace o nasazení webové služby Azure ML najdete v tématu [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="60b27-422">For more information about deploying Azure ML web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

<span data-ttu-id="60b27-423">Chcete-li nasadit novou webovou službu, je potřeba:</span><span class="sxs-lookup"><span data-stu-id="60b27-423">To deploy a new web service, you need to:</span></span>

1. <span data-ttu-id="60b27-424">Vytvoření experimentu vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="60b27-424">Create a scoring experiment.</span></span>
2. <span data-ttu-id="60b27-425">Nasazení webové služby.</span><span class="sxs-lookup"><span data-stu-id="60b27-425">Deploy the web service.</span></span>

<span data-ttu-id="60b27-426">K vytvoření vyhodnocování experimentu z **dokončeno** cvičení experiment, klikněte na tlačítko **vytvořit vyhodnocování EXPERIMENTOVAT** na dolním panelu akcí.</span><span class="sxs-lookup"><span data-stu-id="60b27-426">To create a scoring experiment from a **Finished** training experiment, click **CREATE SCORING EXPERIMENT** in the lower action bar.</span></span>

![Výpočet skóre Azure][18]

<span data-ttu-id="60b27-428">Azure Machine Learning se pokusí vytvořit vyhodnocování experimentu založené na součástech experimentu školení.</span><span class="sxs-lookup"><span data-stu-id="60b27-428">Azure Machine Learning will attempt to create a scoring experiment based on the components of the training experiment.</span></span> <span data-ttu-id="60b27-429">Konkrétně se:</span><span class="sxs-lookup"><span data-stu-id="60b27-429">In particular, it will:</span></span>

1. <span data-ttu-id="60b27-430">Uložení naučeného modelu a odeberte moduly školení modelu.</span><span class="sxs-lookup"><span data-stu-id="60b27-430">Save the trained model and remove the model training modules.</span></span>
2. <span data-ttu-id="60b27-431">Identifikovat logickou **vstupní port** představují schéma očekávané vstupní data.</span><span class="sxs-lookup"><span data-stu-id="60b27-431">Identify a logical **input port** to represent the expected input data schema.</span></span>
3. <span data-ttu-id="60b27-432">Identifikovat logickou **výstupní port** představují výstupního schématu očekávané webové služby.</span><span class="sxs-lookup"><span data-stu-id="60b27-432">Identify a logical **output port** to represent the expected web service output schema.</span></span>

<span data-ttu-id="60b27-433">Když je vytvořen vyhodnocování experiment, zkontrolujte ji a proveďte podle potřeby upravte.</span><span class="sxs-lookup"><span data-stu-id="60b27-433">When the scoring experiment is created, review it and make adjust as needed.</span></span> <span data-ttu-id="60b27-434">Typické úpravu je nahradit vstupní datové sady nebo dotaz s jedním, který vyloučí pole štítek, jak tyto nebudete mít k dispozici při volání služby.</span><span class="sxs-lookup"><span data-stu-id="60b27-434">A typical adjustment is to replace the input dataset and/or query with one which excludes label fields, as these will not be available when the service is called.</span></span> <span data-ttu-id="60b27-435">Je také vhodné zmenšit velikost vstupní datové sady a/nebo dotaz na několik záznamů, akorát, aby znamenat vstupní schéma.</span><span class="sxs-lookup"><span data-stu-id="60b27-435">It is also a good practice to reduce the size of the input dataset and/or query to a few records, just enough to indicate the input schema.</span></span> <span data-ttu-id="60b27-436">Na výstupní port je společné pro všechny vstupní pole vyloučit a obsahovat jenom **popisky vyhodnocení** a **skóre pro Magnitudu Pravděpodobnostech** ve výstupu pomocí [výběr sloupců v datové sadě] [ select-columns] modulu.</span><span class="sxs-lookup"><span data-stu-id="60b27-436">For the output port, it is common to exclude all input fields and only include the **Scored Labels** and **Scored Probabilities** in the output using the [Select Columns in Dataset][select-columns] module.</span></span>

<span data-ttu-id="60b27-437">Ukázka vyhodnocování experimentu najdete v níže uvedeném obrázku.</span><span class="sxs-lookup"><span data-stu-id="60b27-437">A sample scoring experiment is provided in the figure below.</span></span> <span data-ttu-id="60b27-438">Až bude připravený k nasazení, klikněte na tlačítko **publikování webové služby** tlačítko na dolním panelu akcí.</span><span class="sxs-lookup"><span data-stu-id="60b27-438">When ready to deploy, click the **PUBLISH WEB SERVICE** button in the lower action bar.</span></span>

![Publikování Azure ML][11]

## <a name="summary"></a><span data-ttu-id="60b27-440">Souhrn</span><span class="sxs-lookup"><span data-stu-id="60b27-440">Summary</span></span>
<span data-ttu-id="60b27-441">K recap, co jsme provedli v tomto kurzu návod, jste vytvořili prostředí vědecké účely dat Azure, práce s velké veřejné datové sady, trvá prostřednictvím Team Data vědecké účely procesu úplně z získávání dat na školení model a pak na nasazení webové služby Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="60b27-441">To recap what we have done in this walkthrough tutorial, you have created an Azure data science environment, worked with a large public dataset, taking it through the Team Data Science Process, all the way from data acquisition to model training, and then to the deployment of an Azure Machine Learning web service.</span></span>

### <a name="license-information"></a><span data-ttu-id="60b27-442">Informace o licenci</span><span class="sxs-lookup"><span data-stu-id="60b27-442">License information</span></span>
<span data-ttu-id="60b27-443">Tento návod ukázka a jeho doplňujícími skripty a IPython notebook(s) sdílí Microsoft v rámci licencí MIT.</span><span class="sxs-lookup"><span data-stu-id="60b27-443">This sample walkthrough and its accompanying scripts and IPython notebook(s) are shared by Microsoft under the MIT license.</span></span> <span data-ttu-id="60b27-444">Zkontrolujte prosím soubor LICENSE.txt v adresáři ukázkový kód na Githubu další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="60b27-444">Please check the LICENSE.txt file in in the directory of the sample code on GitHub for more details.</span></span>

## <a name="references"></a><span data-ttu-id="60b27-445">Odkazy</span><span class="sxs-lookup"><span data-stu-id="60b27-445">References</span></span>
<span data-ttu-id="60b27-446">• [Stránce pro stažení Andrés Monroy NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/)</span><span class="sxs-lookup"><span data-stu-id="60b27-446">•    [Andrés Monroy NYC Taxi Trips Download Page](http://www.andresmh.com/nyctaxitrips/)</span></span>  
<span data-ttu-id="60b27-447">• [FOILing NYC Taxi cestě dat podle Whong Jan](http://chriswhong.com/open-data/foil_nyc_taxi/) </span><span class="sxs-lookup"><span data-stu-id="60b27-447">•    [FOILing NYC’s Taxi Trip Data by Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/) </span></span>  
<span data-ttu-id="60b27-448">• [NYC taxíkem a Limousine Komise výzkum a statistiky](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span><span class="sxs-lookup"><span data-stu-id="60b27-448">•    [NYC Taxi and Limousine Commission Research and Statistics](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span></span>

[1]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremltrain.png
[11]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azuremlpublish.png
[12]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ssmsconnect.png
[13]: ./media/machine-learning-data-science-process-sqldw-walkthrough/executescript.png
[14]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqlserverproperties.png
[15]: ./media/machine-learning-data-science-process-sqldw-walkthrough/sqldefaultdirs.png
[16]: ./media/machine-learning-data-science-process-sqldw-walkthrough/bulkimport.png
[17]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlreader.png
[18]: ./media/machine-learning-data-science-process-sqldw-walkthrough/amlscoring.png
[19]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_download_scripts.png
[20]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ps_load_data.png
[21]: ./media/machine-learning-data-science-process-sqldw-walkthrough/azcopy-overwrite.png
[22]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-1.png
[23]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-2.png
[24]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-3.png
[25]: ./media/machine-learning-data-science-process-sqldw-walkthrough/ipnb-service-aml-4.png
[26]: ./media/machine-learning-data-science-process-sqldw-walkthrough/tip_class_hist_1.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
