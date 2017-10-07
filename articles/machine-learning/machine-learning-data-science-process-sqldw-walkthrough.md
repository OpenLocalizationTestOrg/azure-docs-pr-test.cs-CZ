---
title: "Hello proces vědecké účely Team dat v akci: pomocí SQL Data Warehouse | Microsoft Docs"
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
ms.openlocfilehash: b1b6371583a023d32e33db59464cafd8c3b767d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="hello-team-data-science-process-in-action-using-sql-data-warehouse"></a><span data-ttu-id="5be11-103">Hello proces vědecké účely Team dat v akci: pomocí SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="5be11-103">hello Team Data Science Process in action: using SQL Data Warehouse</span></span>
<span data-ttu-id="5be11-104">V tomto kurzu jsme vás provede procesem vytváření a nasazování modelu strojového učení pomocí SQL datového skladu (SQL DW) pro veřejně dostupné datové sady – hello [NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/) datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="5be11-104">In this tutorial, we walk you through building and deploying a machine learning model using SQL Data Warehouse (SQL DW) for a publicly available dataset -- hello [NYC Taxi Trips](http://www.andresmh.com/nyctaxitrips/) dataset.</span></span> <span data-ttu-id="5be11-105">Hello binární klasifikace model sestavený předpovídá, zda je tip placené cesty a které předpovědi hello distribuce pro objemy tip hello placené jsou popsány i modely pro více třídami klasifikace a regrese.</span><span class="sxs-lookup"><span data-stu-id="5be11-105">hello binary classification model constructed predicts whether or not a tip is paid for a trip, and models for multiclass classification and regression are also discussed that predict hello distribution for hello tip amounts paid.</span></span>

<span data-ttu-id="5be11-106">Postup Hello následuje hello [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="5be11-106">hello procedure follows hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) workflow.</span></span> <span data-ttu-id="5be11-107">Ukážeme, jak toosetup prostředí vědecké účely data jak tooload hello data do datového skladu SQL a o použití datového skladu SQL nebo IPython Poznámkový blok tooexplore hello dat a pracovníka funkce toomodel.</span><span class="sxs-lookup"><span data-stu-id="5be11-107">We show how toosetup a data science environment, how tooload hello data into SQL DW, and how use either SQL DW or an IPython Notebook tooexplore hello data and engineer features toomodel.</span></span> <span data-ttu-id="5be11-108">Potom ukážeme, jak toobuild a model pomocí Azure Machine Learning nasadit.</span><span class="sxs-lookup"><span data-stu-id="5be11-108">We then show how toobuild and deploy a model with Azure Machine Learning.</span></span>

## <span data-ttu-id="5be11-109"><a name="dataset"></a>datovou sadu cest taxíkem NYC Hello</span><span class="sxs-lookup"><span data-stu-id="5be11-109"><a name="dataset"></a>hello NYC Taxi Trips dataset</span></span>
<span data-ttu-id="5be11-110">Hello NYC taxíkem cestě dat se skládá z přibližně 20GB komprimovaných souborů CSV (nekomprimovaným ~ 48GB), zaznamenávání 173 milionů jednotlivých cest a hello tarify placené pro každou cestu.</span><span class="sxs-lookup"><span data-stu-id="5be11-110">hello NYC Taxi Trip data consists of about 20GB of compressed CSV files (~48GB uncompressed), recording more than 173 million individual trips and hello fares paid for each trip.</span></span> <span data-ttu-id="5be11-111">Každý záznam cestě zahrnuje hello vyzvednutí a odkládací umístění a časy, anonymní zabezpečení číslo licence (ovladač) a hello číslo Medailon (taxi na jedinečné id).</span><span class="sxs-lookup"><span data-stu-id="5be11-111">Each trip record includes hello pickup and drop-off locations and times, anonymized hack (driver's) license number, and hello medallion (taxi’s unique id) number.</span></span> <span data-ttu-id="5be11-112">Hello dat obsahuje všechny služebních cest v hello roku 2013 a je součástí hello následující dvě datové sady pro každý měsíc:</span><span class="sxs-lookup"><span data-stu-id="5be11-112">hello data covers all trips in hello year 2013 and is provided in hello following two datasets for each month:</span></span>

1. <span data-ttu-id="5be11-113">Hello **trip_data.csv** soubor obsahuje podrobnosti o cestě, například na počtu cestujících, vyzvednutí a dropoff body, doba trvání cesty a délka cesty.</span><span class="sxs-lookup"><span data-stu-id="5be11-113">hello **trip_data.csv** file contains trip details, such as number of passengers, pickup and dropoff points, trip duration, and trip length.</span></span> <span data-ttu-id="5be11-114">Tady je několik ukázkových záznamů:</span><span class="sxs-lookup"><span data-stu-id="5be11-114">Here are a few sample records:</span></span>
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. <span data-ttu-id="5be11-115">Hello **trip_fare.csv** soubor obsahuje podrobnosti o tarif hello placené pro každou cestu, například typ platby, velikost tarif, příplatek a daně, tipy a mýtné a celkovou velikost hello placené.</span><span class="sxs-lookup"><span data-stu-id="5be11-115">hello **trip_fare.csv** file contains details of hello fare paid for each trip, such as payment type, fare amount, surcharge and taxes, tips and tolls, and hello total amount paid.</span></span> <span data-ttu-id="5be11-116">Tady je několik ukázkových záznamů:</span><span class="sxs-lookup"><span data-stu-id="5be11-116">Here are a few sample records:</span></span>
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

<span data-ttu-id="5be11-117">Hello **jedinečný klíč** používá toojoin cestě\_dat a cesty\_tarif se skládá z hello následující tři pole:</span><span class="sxs-lookup"><span data-stu-id="5be11-117">hello **unique key** used toojoin trip\_data and trip\_fare is composed of hello following three fields:</span></span>

* <span data-ttu-id="5be11-118">medailonu,</span><span class="sxs-lookup"><span data-stu-id="5be11-118">medallion,</span></span>
* <span data-ttu-id="5be11-119">zabezpečení\_licencí a</span><span class="sxs-lookup"><span data-stu-id="5be11-119">hack\_license and</span></span>
* <span data-ttu-id="5be11-120">vyzvednutí\_data a času.</span><span class="sxs-lookup"><span data-stu-id="5be11-120">pickup\_datetime.</span></span>

## <span data-ttu-id="5be11-121"><a name="mltasks"></a>Adresa tři typy úloh předpovědi</span><span class="sxs-lookup"><span data-stu-id="5be11-121"><a name="mltasks"></a>Address three types of prediction tasks</span></span>
<span data-ttu-id="5be11-122">Jsme formulovali tři předpovědi problémy podle hello *tip\_velikost* tooillustrate tři druhy modelování úlohy:</span><span class="sxs-lookup"><span data-stu-id="5be11-122">We formulate three prediction problems based on hello *tip\_amount* tooillustrate three kinds of modeling tasks:</span></span>

1. <span data-ttu-id="5be11-123">**Binární klasifikace**: toopredict, jestli tip byl placené cesty, tj. *tip\_velikost* větší než $0 je pozitivní příklad, při *tip\_velikost* $ 0 je záporný příklad.</span><span class="sxs-lookup"><span data-stu-id="5be11-123">**Binary classification**: toopredict whether or not a tip was paid for a trip, i.e. a *tip\_amount* that is greater than $0 is a positive example, while a *tip\_amount* of $0 is a negative example.</span></span>
2. <span data-ttu-id="5be11-124">**Více třídami klasifikace**: rozsah hello toopredict tipu zaplacení hello cesty.</span><span class="sxs-lookup"><span data-stu-id="5be11-124">**Multiclass classification**: toopredict hello range of tip paid for hello trip.</span></span> <span data-ttu-id="5be11-125">Jsme rozdělit hello *tip\_velikost* do pěti přihrádek nebo třídy:</span><span class="sxs-lookup"><span data-stu-id="5be11-125">We divide hello *tip\_amount* into five bins or classes:</span></span>
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. <span data-ttu-id="5be11-126">**Úloha regrese**: toopredict hello množství tip placené cesty.</span><span class="sxs-lookup"><span data-stu-id="5be11-126">**Regression task**: toopredict hello amount of tip paid for a trip.</span></span>  

## <span data-ttu-id="5be11-127"><a name="setup"></a>Nastavení prostředí vědecké účely hello dat Azure pro pokročilou analýzu</span><span class="sxs-lookup"><span data-stu-id="5be11-127"><a name="setup"></a>Set up hello Azure data science environment for advanced analytics</span></span>
<span data-ttu-id="5be11-128">tooset prostředí vědecké zpracování dat Azure, postupujte podle těchto kroků.</span><span class="sxs-lookup"><span data-stu-id="5be11-128">tooset up your Azure Data Science environment, follow these steps.</span></span>

<span data-ttu-id="5be11-129">**Vytvořit svůj vlastní účet úložiště objektů blob v Azure**</span><span class="sxs-lookup"><span data-stu-id="5be11-129">**Create your own Azure blob storage account**</span></span>

* <span data-ttu-id="5be11-130">Při zřizování úložiště objektů blob v Azure, zvolte geografické umístění úložiště objektů blob v Azure v nebo co nejblíže příliš**jihu USA**, který je uloží hello NYC taxíkem data.</span><span class="sxs-lookup"><span data-stu-id="5be11-130">When you provision your own Azure blob storage, choose a geo-location for your Azure blob storage in or as close as possible too**South Central US**, which is where hello NYC Taxi data is stored.</span></span> <span data-ttu-id="5be11-131">Hello data se zkopírují pomocí AzCopy z hello veřejného objektu blob úložiště kontejneru tooa kontejneru v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="5be11-131">hello data will be copied using AzCopy from hello public blob storage container tooa container in your own storage account.</span></span> <span data-ttu-id="5be11-132">Hello blíže služby Azure blob storage je tooSouth střed USA, hello rychleji (krok 4) Tato úloha se dokončí.</span><span class="sxs-lookup"><span data-stu-id="5be11-132">hello closer your Azure blob storage is tooSouth Central US, hello faster this task (Step 4) will be completed.</span></span>
* <span data-ttu-id="5be11-133">účet úložiště Azure toocreate, hello postupujte podle kroků uvedených v [účty Azure storage](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="5be11-133">toocreate your own Azure storage account, follow hello steps outlined at [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="5be11-134">Bude potřeba dále v tomto návodu být jisti toomake poznámky k hello hodnoty pro následující přihlašovací údaje účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="5be11-134">Be sure toomake notes on hello values for following storage account credentials as they will be needed later in this walkthrough.</span></span>
  
  * <span data-ttu-id="5be11-135">**Název účtu úložiště**</span><span class="sxs-lookup"><span data-stu-id="5be11-135">**Storage Account Name**</span></span>
  * <span data-ttu-id="5be11-136">**Klíče účtu úložiště.**</span><span class="sxs-lookup"><span data-stu-id="5be11-136">**Storage Account Key**</span></span>
  * <span data-ttu-id="5be11-137">**Název kontejneru** (aplikaci, kterou chcete toobe hello data uložená v hello úložiště objektů blob v Azure)</span><span class="sxs-lookup"><span data-stu-id="5be11-137">**Container Name** (which you want hello data toobe stored in hello Azure blob storage)</span></span>

<span data-ttu-id="5be11-138">**Zřízení instance Azure SQL DW.**</span><span class="sxs-lookup"><span data-stu-id="5be11-138">**Provision your Azure SQL DW instance.**</span></span>
<span data-ttu-id="5be11-139">Postupujte podle dokumentace hello v [vytvořit SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) tooprovision instanci SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5be11-139">Follow hello documentation at [Create a SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) tooprovision a SQL Data Warehouse instance.</span></span> <span data-ttu-id="5be11-140">Ujistěte se, abyste vytvořili zápisy na hello následující přihlašovací údaje SQL Data Warehouse, které se použije v dalších krocích.</span><span class="sxs-lookup"><span data-stu-id="5be11-140">Make sure that you make notations on hello following SQL Data Warehouse credentials which will be used in later steps.</span></span>

* <span data-ttu-id="5be11-141">**Název serveru**: <server Name>. database.windows.net</span><span class="sxs-lookup"><span data-stu-id="5be11-141">**Server Name**: <server Name>.database.windows.net</span></span>
* <span data-ttu-id="5be11-142">**Název SQLDW (databáze)**</span><span class="sxs-lookup"><span data-stu-id="5be11-142">**SQLDW (Database) Name**</span></span>
* <span data-ttu-id="5be11-143">**Uživatelské jméno**</span><span class="sxs-lookup"><span data-stu-id="5be11-143">**Username**</span></span>
* <span data-ttu-id="5be11-144">**Heslo**</span><span class="sxs-lookup"><span data-stu-id="5be11-144">**Password**</span></span>

<span data-ttu-id="5be11-145">**Instalace sady Visual Studio a SQL Server Data Tools.**</span><span class="sxs-lookup"><span data-stu-id="5be11-145">**Install Visual Studio and SQL Server Data Tools.**</span></span> <span data-ttu-id="5be11-146">Pokyny najdete v tématu [instalaci sady Visual Studio 2015 a SSDT (SQL Server Data Tools) pro SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="5be11-146">For instructions, see [Install Visual Studio 2015 and/or SSDT (SQL Server Data Tools) for SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md).</span></span>

<span data-ttu-id="5be11-147">**Připojte tooyour Azure SQL DW pomocí sady Visual Studio.**</span><span class="sxs-lookup"><span data-stu-id="5be11-147">**Connect tooyour Azure SQL DW with Visual Studio.**</span></span> <span data-ttu-id="5be11-148">Pokyny najdete v tématu kroky 1 a 2 v [připojit tooAzure SQL Data Warehouse pomocí sady Visual Studio](../sql-data-warehouse/sql-data-warehouse-connect-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5be11-148">For instructions, see steps 1 & 2 in [Connect tooAzure SQL Data Warehouse with Visual Studio](../sql-data-warehouse/sql-data-warehouse-connect-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="5be11-149">Spuštění hello následující dotaz SQL na databázi hello jste vytvořili v SQL Data Warehouse (místo hello dotazu zadaný v kroku 3 hello připojit tématu) příliš**vytvoření hlavního klíče**.</span><span class="sxs-lookup"><span data-stu-id="5be11-149">Run hello following SQL query on hello database you created in your SQL Data Warehouse (instead of hello query provided in step 3 of hello connect topic,) too**create a master key**.</span></span>
> 
> 

    BEGIN TRY
           --Try toocreate hello master key
        CREATE MASTER KEY
    END TRY
    BEGIN CATCH
           --If hello master key exists, do nothing
    END CATCH;

<span data-ttu-id="5be11-150">**Vytvořte pracovní prostor služby Azure Machine Learning v rámci vašeho předplatného Azure.**</span><span class="sxs-lookup"><span data-stu-id="5be11-150">**Create an Azure Machine Learning workspace under your Azure subscription.**</span></span> <span data-ttu-id="5be11-151">Pokyny najdete v tématu [vytvořit pracovní prostor služby Azure Machine Learning](machine-learning-create-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="5be11-151">For instructions, see [Create an Azure Machine Learning workspace](machine-learning-create-workspace.md).</span></span>

## <span data-ttu-id="5be11-152"><a name="getdata"></a>Načtení hello dat do SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="5be11-152"><a name="getdata"></a>Load hello data into SQL Data Warehouse</span></span>
<span data-ttu-id="5be11-153">Otevřete konzolu příkazového prostředí Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5be11-153">Open a Windows PowerShell command console.</span></span> <span data-ttu-id="5be11-154">Spustit hello následující příkazy prostředí PowerShell toodownload hello příklad SQL skriptu soubory, které můžeme sdílet s vámi na Githubu tooa místní adresář s parametrem hello *- DestDir*.</span><span class="sxs-lookup"><span data-stu-id="5be11-154">Run hello following PowerShell commands toodownload hello example SQL script files that we share with you on GitHub tooa local directory that you specify with hello parameter *-DestDir*.</span></span> <span data-ttu-id="5be11-155">Můžete změnit hello hodnota parametru *- DestDir* tooany místního adresáře.</span><span class="sxs-lookup"><span data-stu-id="5be11-155">You can change hello value of parameter *-DestDir* tooany local directory.</span></span> <span data-ttu-id="5be11-156">Pokud *- DestDir* neexistuje, vytvoří se tím hello skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5be11-156">If *-DestDir* does not exist, it will be created by hello PowerShell script.</span></span>

> [!NOTE]
> <span data-ttu-id="5be11-157">Může být nutné příliš**spustit jako správce** při provádění hello následující skript prostředí PowerShell, pokud vaše *DestDir* directory potřebuje správce oprávnění toocreate nebo toowrite tooit.</span><span class="sxs-lookup"><span data-stu-id="5be11-157">You might need too**Run as Administrator** when executing hello following PowerShell script if your *DestDir* directory needs Administrator privilege toocreate or toowrite tooit.</span></span>
> 
> 

    $source = "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/Download_Scripts_SQLDW_Walkthrough.ps1"
    $ps1_dest = "$pwd\Download_Scripts_SQLDW_Walkthrough.ps1"
    $wc = New-Object System.Net.WebClient
    $wc.DownloadFile($source, $ps1_dest)
    .\Download_Scripts_SQLDW_Walkthrough.ps1 –DestDir 'C:\tempSQLDW'

<span data-ttu-id="5be11-158">Po úspěšném provedení změny aktuální pracovní adresář příliš*- DestDir*.</span><span class="sxs-lookup"><span data-stu-id="5be11-158">After successful execution, your current working directory changes too*-DestDir*.</span></span> <span data-ttu-id="5be11-159">Nyní byste měli mít toosee obrazovku podobnou následující:</span><span class="sxs-lookup"><span data-stu-id="5be11-159">You should be able toosee screen like below:</span></span>

![][19]

<span data-ttu-id="5be11-160">Ve vašem *- DestDir*, spustit následující skript prostředí PowerShell v režimu správce hello:</span><span class="sxs-lookup"><span data-stu-id="5be11-160">In your *-DestDir*, execute hello following PowerShell script in administrator mode:</span></span>

    ./SQLDW_Data_Import.ps1

<span data-ttu-id="5be11-161">Spuštění hello skript prostředí PowerShell pro hello poprvé, zobrazí se výzva tooinput hello informace z vašeho datového skladu SQL Azure a účtu úložiště objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="5be11-161">When hello PowerShell script runs for hello first time, you will be asked tooinput hello information from your Azure SQL DW and your Azure blob storage account.</span></span> <span data-ttu-id="5be11-162">Po dokončení tohoto skriptu prostředí PowerShell systémem pro hello poprvé, přihlašovací údaje hello vstup vám bude mít byla zapsána tooa konfigurační soubor SQLDW.conf v hello přítomen pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="5be11-162">When this PowerShell script completes running for hello first time, hello credentials you input will have been written tooa configuration file SQLDW.conf in hello present working directory.</span></span> <span data-ttu-id="5be11-163">Hello budoucí spustit tento soubor skriptu PowerShell má možnost tooread hello všechny potřebné parametry z tohoto konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="5be11-163">hello future run of this PowerShell script file has hello option tooread all needed parameters from this configuration file.</span></span> <span data-ttu-id="5be11-164">Pokud potřebujete toochange některé parametry, můžete vybrat tooinput hello parametry úvodní obrazovka řádku odstranit tento konfigurační soubor a po zobrazení výzvy zadání hodnot parametrů hello nebo hodnoty parametru hello toochange úpravou souboru SQLDW.conf hello ve vašem *- DestDir* adresáře.</span><span class="sxs-lookup"><span data-stu-id="5be11-164">If you need toochange some parameters, you can choose tooinput hello parameters on hello screen upon prompt by deleting this configuration file and inputting hello parameters values as prompted or toochange hello parameter values by editing hello SQLDW.conf file in your *-DestDir* directory.</span></span>

> [!NOTE]
> <span data-ttu-id="5be11-165">V pořadí tooavoid schématu název je v konfliktu s těmi, které již existují v Azure SQL DW, při čtení parametry přímo ze souboru SQLDW.conf hello náhodné číslo 3 číslice je přidat název schématu toohello ze souboru SQLDW.conf hello jako hello výchozí název schématu pro každé spuštění.</span><span class="sxs-lookup"><span data-stu-id="5be11-165">In order tooavoid schema name conflicts with those that already exist in your Azure SQL DW, when reading parameters directly from hello SQLDW.conf file, a 3-digit random number is added toohello schema name from hello SQLDW.conf file as hello default schema name for each run.</span></span> <span data-ttu-id="5be11-166">Hello skript prostředí PowerShell můžete být vyzváni k zadání názvu schématu: uvážení uživatele může být zadán název hello.</span><span class="sxs-lookup"><span data-stu-id="5be11-166">hello PowerShell script may prompt you for a schema name: hello name may be specified at user discretion.</span></span>
> 
> 

<span data-ttu-id="5be11-167">To **skript prostředí PowerShell** souboru dokončení hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="5be11-167">This **PowerShell script** file completes hello following tasks:</span></span>

* <span data-ttu-id="5be11-168">**Stáhne a nainstaluje AzCopy**, pokud ještě není nainstalovaný nástroj AzCopy</span><span class="sxs-lookup"><span data-stu-id="5be11-168">**Downloads and installs AzCopy**, if AzCopy is not already installed</span></span>
  
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
* <span data-ttu-id="5be11-169">**Zkopíruje data tooyour privátní objekt blob úložiště účet** z veřejného objektu blob hello s AzCopy</span><span class="sxs-lookup"><span data-stu-id="5be11-169">**Copies data tooyour private blob storage account** from hello public blob with AzCopy</span></span>
  
        Write-Host "AzCopy is copying data from public blob tooyo storage account. It may take a while..." -ForegroundColor "Yellow"
        $start_time = Get-Date
        AzCopy.exe /Source:$Source /Dest:$DestURL /DestKey:$StorageAccountKey /S
        $end_time = Get-Date
        $time_span = $end_time - $start_time
        $total_seconds = [math]::Round($time_span.TotalSeconds,2)
        Write-Host "AzCopy finished copying data. Please check your storage account tooverify." -ForegroundColor "Yellow"
        Write-Host "This step (copying data from public blob tooyour storage account) takes $total_seconds seconds." -ForegroundColor "Green"
* <span data-ttu-id="5be11-170">**Načte data pomocí funkce Polybase (spuštěním LoadDataToSQLDW.sql) tooyour Azure SQL DW** z vašeho účtu úložiště objektů blob privátní s hello následující příkazy.</span><span class="sxs-lookup"><span data-stu-id="5be11-170">**Loads data using Polybase (by executing LoadDataToSQLDW.sql) tooyour Azure SQL DW** from your private blob storage account with hello following commands.</span></span>
  
  * <span data-ttu-id="5be11-171">Vytvořte schéma</span><span class="sxs-lookup"><span data-stu-id="5be11-171">Create a schema</span></span>
    
          EXEC (''CREATE SCHEMA {schemaname};'');
  * <span data-ttu-id="5be11-172">Vytvoření oboru databáze přihlašovacích údajů</span><span class="sxs-lookup"><span data-stu-id="5be11-172">Create a database scoped credential</span></span>
    
          CREATE DATABASE SCOPED CREDENTIAL {KeyAlias}
          WITH IDENTITY = ''asbkey'' ,
          Secret = ''{StorageAccountKey}''
  * <span data-ttu-id="5be11-173">Vytvoření externího zdroje dat pro objekt blob úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="5be11-173">Create an external data source for an Azure storage blob</span></span>
    
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
  * <span data-ttu-id="5be11-174">Vytvořte formátu externí soubor pro soubor csv.</span><span class="sxs-lookup"><span data-stu-id="5be11-174">Create an external file format for a csv file.</span></span> <span data-ttu-id="5be11-175">Data nekomprimované a pole jsou odděleny znakem hello.</span><span class="sxs-lookup"><span data-stu-id="5be11-175">Data is uncompressed and fields are separated with hello pipe character.</span></span>
    
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
  * <span data-ttu-id="5be11-176">Vytvořte externí tarif a tabulky cesty pro datovou sadu taxíkem NYC v Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="5be11-176">Create external fare and trip tables for NYC taxi dataset in Azure blob storage.</span></span>
    
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

    - <span data-ttu-id="5be11-177">Načtení dat z externí tabulky v Azure blob storage tooSQL datového skladu</span><span class="sxs-lookup"><span data-stu-id="5be11-177">Load data from external tables in Azure blob storage tooSQL Data Warehouse</span></span>

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

    - <span data-ttu-id="5be11-178">Vytvoří tabulku ukázkových dat (NYCTaxi_Sample) a vložit data tooit vyberou dotazů SQL na služební cestě a tarif tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="5be11-178">Create a sample data table (NYCTaxi_Sample) and insert data tooit from selecting SQL queries on hello trip and fare tables.</span></span> <span data-ttu-id="5be11-179">(Některé kroky tohoto názorného postupu musí toouse tabulku.)</span><span class="sxs-lookup"><span data-stu-id="5be11-179">(Some steps of this walkthrough needs toouse this sample table.)</span></span>

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

<span data-ttu-id="5be11-180">Hello zeměpisnou polohu účtů úložiště ovlivňuje časů načtení.</span><span class="sxs-lookup"><span data-stu-id="5be11-180">hello geographic location of your storage accounts affects load times.</span></span>

> [!NOTE]
> <span data-ttu-id="5be11-181">V závislosti na hello zeměpisné umístění účtu úložiště objektů blob privátní, hello proces kopírování dat z účtu privátní úložiště tooyour veřejného objektu blob může trvat přibližně 15 minut nebo i déle a hello procesu načítání dat z vašeho účtu úložiště tooyour Azure SQL DW může trvat 20 minut nebo déle.</span><span class="sxs-lookup"><span data-stu-id="5be11-181">Depending on hello geographical location of your private blob storage account, hello process of copying data from a public blob tooyour private storage account can take about 15 minutes, or even longer,and hello process of loading data from your storage account tooyour Azure SQL DW could take 20 minutes or longer.</span></span>  
> 
> 

<span data-ttu-id="5be11-182">Budete mít toodecide co dělat, když máte duplicitním zdrojovým a cílovým souborů.</span><span class="sxs-lookup"><span data-stu-id="5be11-182">You will have toodecide what do if you have duplicate source and destination files.</span></span>

> [!NOTE]
> <span data-ttu-id="5be11-183">Pokud toobe soubory .csv hello zkopírovaných z účtu úložiště hello veřejného objektu blob úložiště tooyour privátní blob již existuje v účtu úložiště objektů blob privátní, AzCopy se zeptá, jestli chcete toooverwrite je.</span><span class="sxs-lookup"><span data-stu-id="5be11-183">If hello .csv files toobe copied from hello public blob storage tooyour private blob storage account already exist in your private blob storage account, AzCopy will ask you whether you want toooverwrite them.</span></span> <span data-ttu-id="5be11-184">Pokud nechcete, aby toooverwrite je, vstupní  **n**  po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="5be11-184">If you do not want toooverwrite them, input **n** when prompted.</span></span> <span data-ttu-id="5be11-185">Pokud chcete, aby toooverwrite **všechny** z nich, vstup **** po zobrazení výzvy.</span><span class="sxs-lookup"><span data-stu-id="5be11-185">If you want toooverwrite **all** of them, input **a** when prompted.</span></span> <span data-ttu-id="5be11-186">Můžete také zadat **y** toooverwrite .csv soubory jednotlivě.</span><span class="sxs-lookup"><span data-stu-id="5be11-186">You can also input **y** toooverwrite .csv files individually.</span></span>
> 
> 

![Vykreslení #21][21]

<span data-ttu-id="5be11-188">Můžete vytvořit svoje vlastní data.</span><span class="sxs-lookup"><span data-stu-id="5be11-188">You can use your own data.</span></span> <span data-ttu-id="5be11-189">Pokud vaše data jsou v místním počítači ve vaší aplikaci reálného života, stále můžete služby AzCopy tooupload místní data tooyour privátní Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="5be11-189">If your data is in your on-premises machine in your real life application, you can still use AzCopy tooupload on-premises data tooyour private Azure blob storage.</span></span> <span data-ttu-id="5be11-190">Potřebujete jenom toochange hello **zdroj** umístění, `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, v hello AzCopy příkaz hello prostředí PowerShell skriptu souboru toohello místní adresář, který obsahuje vaše data.</span><span class="sxs-lookup"><span data-stu-id="5be11-190">You only need toochange hello **Source** location, `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, in hello AzCopy command of hello PowerShell script file toohello local directory that contains your data.</span></span>

> [!TIP]
> <span data-ttu-id="5be11-191">Pokud vaše data je již ve službě privátní Azure blob storage ve vaší aplikaci reálného života, můžete přeskočit hello AzCopy krok v hello skript prostředí PowerShell a přímo nahrát hello data tooAzure SQL DW.</span><span class="sxs-lookup"><span data-stu-id="5be11-191">If your data is already in your private Azure blob storage in your real life application, you can skip hello AzCopy step in hello PowerShell script and directly upload hello data tooAzure SQL DW.</span></span> <span data-ttu-id="5be11-192">To bude vyžadovat, že další upravuje z hello skriptu tootailor ho toohello formát data.</span><span class="sxs-lookup"><span data-stu-id="5be11-192">This will require additional edits of hello script tootailor it toohello format of your data.</span></span>
> 
> 

<span data-ttu-id="5be11-193">Tento skript prostředí Powershell také připojuje v hello Azure SQL DW informace do hello data zkoumání příklad souborů SQLDW_Explorations.sql, SQLDW_Explorations.ipynb a SQLDW_Explorations_Scripts.py tak, aby tyto tři soubory jsou připravené toobe se pokusila okamžitě Po dokončení hello skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5be11-193">This Powershell script also plugs in hello Azure SQL DW information into hello data exploration example files SQLDW_Explorations.sql, SQLDW_Explorations.ipynb, and SQLDW_Explorations_Scripts.py so that these three files are ready toobe tried out instantly after hello PowerShell script completes.</span></span>

<span data-ttu-id="5be11-194">Po úspěšném spuštění, zobrazí se obrazovka jako níže:</span><span class="sxs-lookup"><span data-stu-id="5be11-194">After a successful execution, you will see screen like below:</span></span>

![][20]

## <span data-ttu-id="5be11-195"><a name="dbexplore"></a>Zkoumání dat a funkce analýzy v Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="5be11-195"><a name="dbexplore"></a>Data exploration and feature engineering in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="5be11-196">V této části, můžeme provést zkoumání a funkce generování dat provádění dotazů SQL Azure SQL DW přímo pomocí **Data nástroje sady Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="5be11-196">In this section, we perform data exploration and feature generation by running SQL queries against Azure SQL DW directly using **Visual Studio Data Tools**.</span></span> <span data-ttu-id="5be11-197">Všechny dotazy SQL použít v této části najdete v hello ukázkový skript s názvem *SQLDW_Explorations.sql*.</span><span class="sxs-lookup"><span data-stu-id="5be11-197">All SQL queries used in this section can be found in hello sample script named *SQLDW_Explorations.sql*.</span></span> <span data-ttu-id="5be11-198">Tento soubor je již stažené tooyour místního adresáře hello skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5be11-198">This file has already been downloaded tooyour local directory by hello PowerShell script.</span></span> <span data-ttu-id="5be11-199">Můžete také načíst z [Githubu](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql).</span><span class="sxs-lookup"><span data-stu-id="5be11-199">You can also retrieve it from [GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql).</span></span> <span data-ttu-id="5be11-200">Ale hello souboru na Githubu, nemá informace o Azure SQL DW hello napájený ze sítě.</span><span class="sxs-lookup"><span data-stu-id="5be11-200">But hello file in GitHub does not have hello Azure SQL DW information plugged in.</span></span>

<span data-ttu-id="5be11-201">Připojení tooyour Azure SQL DW pomocí sady Visual Studio s hello SQL DW přihlašovací jméno a heslo a otevře hello **Průzkumník objektů systému SQL** tooconfirm hello databáze a tabulky byly importovány.</span><span class="sxs-lookup"><span data-stu-id="5be11-201">Connect tooyour Azure SQL DW using Visual Studio with hello SQL DW login name and password and open up hello **SQL Object Explorer** tooconfirm hello database and tables have been imported.</span></span> <span data-ttu-id="5be11-202">Načtení hello *SQLDW_Explorations.sql* souboru.</span><span class="sxs-lookup"><span data-stu-id="5be11-202">Retrieve hello *SQLDW_Explorations.sql* file.</span></span>

> [!NOTE]
> <span data-ttu-id="5be11-203">tooopen editor dotazů Parallel Data Warehouse (PDW), použijte hello **nový dotaz** příkaz v hello se zapnutým vaše PDW **Průzkumník objektů systému SQL**.</span><span class="sxs-lookup"><span data-stu-id="5be11-203">tooopen a Parallel Data Warehouse (PDW) query editor, use hello **New Query** command while your PDW is selected in hello **SQL Object Explorer**.</span></span> <span data-ttu-id="5be11-204">editor dotazů SQL standardní Hello nepodporuje PDW.</span><span class="sxs-lookup"><span data-stu-id="5be11-204">hello standard SQL query editor is not supported by PDW.</span></span>
> 
> 

<span data-ttu-id="5be11-205">Tady jsou hello typ dat zkoumání a funkce generování úlohy provádějí v této části:</span><span class="sxs-lookup"><span data-stu-id="5be11-205">Here are hello type of data exploration and feature generation tasks performed in this section:</span></span>

* <span data-ttu-id="5be11-206">Prozkoumejte data distribuce několik polí v různých časových oken.</span><span class="sxs-lookup"><span data-stu-id="5be11-206">Explore data distributions of a few fields in varying time windows.</span></span>
* <span data-ttu-id="5be11-207">Prozkoumejte data quality hello zeměpisné šířky a délky polí.</span><span class="sxs-lookup"><span data-stu-id="5be11-207">Investigate data quality of hello longitude and latitude fields.</span></span>
* <span data-ttu-id="5be11-208">Generovat binární a více třídami klasifikační štítky podle hello **tip\_velikost**.</span><span class="sxs-lookup"><span data-stu-id="5be11-208">Generate binary and multiclass classification labels based on hello **tip\_amount**.</span></span>
* <span data-ttu-id="5be11-209">Generovat funkce a výpočetní nebo porovnat cestě vzdálenosti.</span><span class="sxs-lookup"><span data-stu-id="5be11-209">Generate features and compute/compare trip distances.</span></span>
* <span data-ttu-id="5be11-210">Připojení k hello dvou tabulek a extrahovat z náhodného vzorku, který bude použité toobuild modely.</span><span class="sxs-lookup"><span data-stu-id="5be11-210">Join hello two tables and extract a random sample that will be used toobuild models.</span></span>

### <a name="data-import-verification"></a><span data-ttu-id="5be11-211">Ověření importu dat</span><span class="sxs-lookup"><span data-stu-id="5be11-211">Data import verification</span></span>
<span data-ttu-id="5be11-212">Tyto dotazy poskytují rychlý ověření hello počtu řádků a sloupců v hello tabulky vyplněny dříve Polybase na paralelní hromadného importu,</span><span class="sxs-lookup"><span data-stu-id="5be11-212">These queries provide a quick verification of hello number of rows and columns in hello tables populated earlier using Polybase's parallel bulk import,</span></span>

    -- Report number of rows in table <nyctaxi_trip> without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')

    -- Report number of columns in table <nyctaxi_trip>
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = '<nyctaxi_trip>' AND table_schema = '<schemaname>'

<span data-ttu-id="5be11-213">**Výstup:** měli byste obdržet 173,179,759 řádků a sloupců 14.</span><span class="sxs-lookup"><span data-stu-id="5be11-213">**Output:** You should get 173,179,759 rows and 14 columns.</span></span>

### <a name="exploration-trip-distribution-by-medallion"></a><span data-ttu-id="5be11-214">Zkoumání: Cestě distribuce podle Medailon</span><span class="sxs-lookup"><span data-stu-id="5be11-214">Exploration: Trip distribution by medallion</span></span>
<span data-ttu-id="5be11-215">Tento příklad dotazu identifikuje medallions hello (taxi čísla), které byly dokončeny více než 100 služebních cest v rámci určeného časového období.</span><span class="sxs-lookup"><span data-stu-id="5be11-215">This example query identifies hello medallions (taxi numbers) that completed more than 100 trips within a specified time period.</span></span> <span data-ttu-id="5be11-216">Hello dotazu by využívat přístup k tabulce hello rozdělena na oddíly, vzhledem k tomu, že je podmíněno tím schéma oddílů hello **vyzvednutí\_data a času**.</span><span class="sxs-lookup"><span data-stu-id="5be11-216">hello query would benefit from hello partitioned table access since it is conditioned by hello partition scheme of **pickup\_datetime**.</span></span> <span data-ttu-id="5be11-217">Dotazování hello úplnou datovou sadu se ujistěte se taky použít hello oddílů tabulky nebo indexu kontroly.</span><span class="sxs-lookup"><span data-stu-id="5be11-217">Querying hello full dataset will also make use of hello partitioned table and/or index scan.</span></span>

    SELECT medallion, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

<span data-ttu-id="5be11-218">**Výstup:** hello dotaz by měl vrátit tabulku s řádky zadání hello 13,369 medallions (taxislužby) a hello číslo cesty dokončit v 2013.</span><span class="sxs-lookup"><span data-stu-id="5be11-218">**Output:** hello query should return a table with rows specifying hello 13,369 medallions (taxis) and hello number of trip completed by them in 2013.</span></span> <span data-ttu-id="5be11-219">poslední sloupec Hello obsahuje hello počet hello služebních cest byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="5be11-219">hello last column contains hello count of hello number of trips completed.</span></span>

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a><span data-ttu-id="5be11-220">Zkoumání: Cestě distribuce podle Medailon a hack_license</span><span class="sxs-lookup"><span data-stu-id="5be11-220">Exploration: Trip distribution by medallion and hack_license</span></span>
<span data-ttu-id="5be11-221">Tento příklad identifikuje hello medallions (taxi čísla) a hack_license čísla (ovladače), dokončeno více než 100 služebních cest v rámci určeného časového období.</span><span class="sxs-lookup"><span data-stu-id="5be11-221">This example identifies hello medallions (taxi numbers) and hack_license numbers (drivers) that completed more than 100 trips within a specified time period.</span></span>

    SELECT medallion, hack_license, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

<span data-ttu-id="5be11-222">**Výstup:** hello dotazu by měla vrátit tabulku s 13,369 řádky zadání hello 13,369 car a ovladače ID, které mají více dokončit tento 100 služebních cest v 2013.</span><span class="sxs-lookup"><span data-stu-id="5be11-222">**Output:** hello query should return a table with 13,369 rows specifying hello 13,369 car/driver IDs that have completed more that 100 trips in 2013.</span></span> <span data-ttu-id="5be11-223">poslední sloupec Hello obsahuje hello počet hello služebních cest byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="5be11-223">hello last column contains hello count of hello number of trips completed.</span></span>

### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a><span data-ttu-id="5be11-224">Hodnocení kvality dat: ověřit záznamy s nesprávné délky a šířky</span><span class="sxs-lookup"><span data-stu-id="5be11-224">Data quality assessment: Verify records with incorrect longitude and/or latitude</span></span>
<span data-ttu-id="5be11-225">Tento příklad prověří, pokud jakýkoli hello zeměpisné délky a šířky polí buď obsahuje neplatnou hodnotu (radián stupňů musí být mezi -90 a 90), nebo (0, 0) souřadnice.</span><span class="sxs-lookup"><span data-stu-id="5be11-225">This example investigates if any of hello longitude and/or latitude fields either contain an invalid value (radian degrees should be between -90 and 90), or have (0, 0) coordinates.</span></span>

    SELECT COUNT(*) FROM <schemaname>.<nyctaxi_trip>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

<span data-ttu-id="5be11-226">**Výstup:** hello dotaz vrátí 837,467 služebních cest, které mají neplatná pole zeměpisné délky a šířky.</span><span class="sxs-lookup"><span data-stu-id="5be11-226">**Output:** hello query returns 837,467 trips that have invalid longitude and/or latitude fields.</span></span>

### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a><span data-ttu-id="5be11-227">Zkoumání: Vysypávány oproti distribuční není šikmý služebních cest</span><span class="sxs-lookup"><span data-stu-id="5be11-227">Exploration: Tipped vs. not tipped trips distribution</span></span>
<span data-ttu-id="5be11-228">Tento příklad vyhledá hello počet cest, které byly vysypávány oproti hello číslo, které nebyly vysypávány v zadaném časovém období (nebo v hello úplnou datovou sadu Pokud pokrývajících hello celý rok, jako je zde nastavený).</span><span class="sxs-lookup"><span data-stu-id="5be11-228">This example finds hello number of trips that were tipped vs. hello number that were not tipped in a specified time period (or in hello full dataset if covering hello full year as it is set up here).</span></span> <span data-ttu-id="5be11-229">Toto rozdělení odráží toobe distribuční binární popisek hello později použita k modelování binární klasifikace.</span><span class="sxs-lookup"><span data-stu-id="5be11-229">This distribution reflects hello binary label distribution toobe later used for binary classification modeling.</span></span>

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM <schemaname>.<nyctaxi_fare>
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

<span data-ttu-id="5be11-230">**Výstup:** hello dotaz by měl následující návratové hello tip pro hello roku 2013: 90,447,622 vysypávány a 82,264,709 vysypávány není frekvencí.</span><span class="sxs-lookup"><span data-stu-id="5be11-230">**Output:** hello query should return hello following tip frequencies for hello year 2013: 90,447,622 tipped and 82,264,709 not-tipped.</span></span>

### <a name="exploration-tip-classrange-distribution"></a><span data-ttu-id="5be11-231">Zkoumání: Distribuce třídy nebo rozsah Tip</span><span class="sxs-lookup"><span data-stu-id="5be11-231">Exploration: Tip class/range distribution</span></span>
<span data-ttu-id="5be11-232">Tento příklad vypočítá distribuci hello rozsahy tip v daném časovém období (nebo hello úplnou datovou sadu Pokud pokrývající celý rok hello).</span><span class="sxs-lookup"><span data-stu-id="5be11-232">This example computes hello distribution of tip ranges in a given time period (or in hello full dataset if covering hello full year).</span></span> <span data-ttu-id="5be11-233">Toto je hello distribuční hello popisek tříd, které se použijí později pro modelování více třídami klasifikace.</span><span class="sxs-lookup"><span data-stu-id="5be11-233">This is hello distribution of hello label classes that will be used later for multiclass classification modeling.</span></span>

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

<span data-ttu-id="5be11-234">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="5be11-234">**Output:**</span></span>

| <span data-ttu-id="5be11-235">tip_class</span><span class="sxs-lookup"><span data-stu-id="5be11-235">tip_class</span></span> | <span data-ttu-id="5be11-236">tip_freq</span><span class="sxs-lookup"><span data-stu-id="5be11-236">tip_freq</span></span> |
| --- | --- |
| <span data-ttu-id="5be11-237">1</span><span class="sxs-lookup"><span data-stu-id="5be11-237">1</span></span> |<span data-ttu-id="5be11-238">82230915</span><span class="sxs-lookup"><span data-stu-id="5be11-238">82230915</span></span> |
| <span data-ttu-id="5be11-239">2</span><span class="sxs-lookup"><span data-stu-id="5be11-239">2</span></span> |<span data-ttu-id="5be11-240">6198803</span><span class="sxs-lookup"><span data-stu-id="5be11-240">6198803</span></span> |
| <span data-ttu-id="5be11-241">3</span><span class="sxs-lookup"><span data-stu-id="5be11-241">3</span></span> |<span data-ttu-id="5be11-242">1932223</span><span class="sxs-lookup"><span data-stu-id="5be11-242">1932223</span></span> |
| <span data-ttu-id="5be11-243">0</span><span class="sxs-lookup"><span data-stu-id="5be11-243">0</span></span> |<span data-ttu-id="5be11-244">82264625</span><span class="sxs-lookup"><span data-stu-id="5be11-244">82264625</span></span> |
| <span data-ttu-id="5be11-245">4</span><span class="sxs-lookup"><span data-stu-id="5be11-245">4</span></span> |<span data-ttu-id="5be11-246">85765</span><span class="sxs-lookup"><span data-stu-id="5be11-246">85765</span></span> |

### <a name="exploration-compute-and-compare-trip-distance"></a><span data-ttu-id="5be11-247">Zkoumání: Výpočetní a porovnání vzdálenost cesty</span><span class="sxs-lookup"><span data-stu-id="5be11-247">Exploration: Compute and compare trip distance</span></span>
<span data-ttu-id="5be11-248">Tento příklad převede hello vyzvednutí a odkládací zeměpisné délky a šířky tooSQL geography body, vypočítá vzdálenost cestě hello pomocí SQL geography body rozdíl a vrátí z náhodného vzorku hello výsledky pro porovnání.</span><span class="sxs-lookup"><span data-stu-id="5be11-248">This example converts hello pickup and drop-off longitude and latitude tooSQL geography points, computes hello trip distance using SQL geography points difference, and returns a random sample of hello results for comparison.</span></span> <span data-ttu-id="5be11-249">Příklad Hello omezí výsledky hello toovalid koordinuje jenom pomocí hello kvality assessment dotaz na data popsané výše.</span><span class="sxs-lookup"><span data-stu-id="5be11-249">hello example limits hello results toovalid coordinates only using hello data quality assessment query covered earlier.</span></span>

    /****** Object:  UserDefinedFunction [dbo].[fnCalculateDistance] ******/
    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function toocalculate hello direct distance  in mile between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
          DECLARE @distance decimal(28, 10)
          -- Convert tooradians
          SET @Lat1 = @Lat1 / 57.2958
          SET @Long1 = @Long1 / 57.2958
          SET @Lat2 = @Lat2 / 57.2958
          SET @Long2 = @Long2 / 57.2958
          -- Calculate distance
          SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
          --Convert toomiles
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

### <a name="feature-engineering-using-sql-functions"></a><span data-ttu-id="5be11-250">Funkce inženýrství pomocí funkce SQL</span><span class="sxs-lookup"><span data-stu-id="5be11-250">Feature engineering using SQL functions</span></span>
<span data-ttu-id="5be11-251">Funkce SQL v některých případech může být efektivní možnost pro funkci inženýrství.</span><span class="sxs-lookup"><span data-stu-id="5be11-251">Sometimes SQL functions can be an efficient option for feature engineering.</span></span> <span data-ttu-id="5be11-252">V tomto návodu jsme definovali SQL funkce toocalculate hello přímé vzdálenosti mezi hello vyzvednutí a dropoff umístění.</span><span class="sxs-lookup"><span data-stu-id="5be11-252">In this walkthrough, we defined a SQL function toocalculate hello direct distance between hello pickup and dropoff locations.</span></span> <span data-ttu-id="5be11-253">Můžete spustit následující skripty SQL v hello **Data nástroje sady Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="5be11-253">You can run hello following SQL scripts in **Visual Studio Data Tools**.</span></span>

<span data-ttu-id="5be11-254">Zde je hello skript SQL, který definuje funkci vzdálenost hello.</span><span class="sxs-lookup"><span data-stu-id="5be11-254">Here is hello SQL script that defines hello distance function.</span></span>

    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function calculate hello direct distance between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
          DECLARE @distance decimal(28, 10)
          -- Convert tooradians
          SET @Lat1 = @Lat1 / 57.2958
          SET @Long1 = @Long1 / 57.2958
          SET @Lat2 = @Lat2 / 57.2958
          SET @Long2 = @Long2 / 57.2958
          -- Calculate distance
          SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
          --Convert toomiles
          IF @distance <> 0
          BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
          END
          RETURN @distance
    END
    GO

<span data-ttu-id="5be11-255">Tady je toocall příklad této funkce toogenerate funkce v dotazu SQL:</span><span class="sxs-lookup"><span data-stu-id="5be11-255">Here is an example toocall this function toogenerate features in your SQL query:</span></span>

    -- Sample query toocall hello function toocreate features
    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

<span data-ttu-id="5be11-256">**Výstup:** tento dotaz vygeneruje tabulku (s 2,803,538 řádky) s vyzvednutí a dropoff zeměpisné šířky a stupně zeměpisné délky a odpovídající hello přímé vzdálenosti v paliva.</span><span class="sxs-lookup"><span data-stu-id="5be11-256">**Output:** This query generates a table (with 2,803,538 rows) with pickup and dropoff latitudes and longitudes and hello corresponding direct distances in miles.</span></span> <span data-ttu-id="5be11-257">Zde jsou hello výsledky pro první 3 řádky:</span><span class="sxs-lookup"><span data-stu-id="5be11-257">Here are hello results for first 3 rows:</span></span>

|  | <span data-ttu-id="5be11-258">pickup_latitude</span><span class="sxs-lookup"><span data-stu-id="5be11-258">pickup_latitude</span></span> | <span data-ttu-id="5be11-259">pickup_longitude</span><span class="sxs-lookup"><span data-stu-id="5be11-259">pickup_longitude</span></span> | <span data-ttu-id="5be11-260">dropoff_latitude</span><span class="sxs-lookup"><span data-stu-id="5be11-260">dropoff_latitude</span></span> | <span data-ttu-id="5be11-261">dropoff_longitude</span><span class="sxs-lookup"><span data-stu-id="5be11-261">dropoff_longitude</span></span> | <span data-ttu-id="5be11-262">DirectDistance</span><span class="sxs-lookup"><span data-stu-id="5be11-262">DirectDistance</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="5be11-263">1</span><span class="sxs-lookup"><span data-stu-id="5be11-263">1</span></span> |<span data-ttu-id="5be11-264">40.731804</span><span class="sxs-lookup"><span data-stu-id="5be11-264">40.731804</span></span> |<span data-ttu-id="5be11-265">-74.001083</span><span class="sxs-lookup"><span data-stu-id="5be11-265">-74.001083</span></span> |<span data-ttu-id="5be11-266">40.736622</span><span class="sxs-lookup"><span data-stu-id="5be11-266">40.736622</span></span> |<span data-ttu-id="5be11-267">-73.988953</span><span class="sxs-lookup"><span data-stu-id="5be11-267">-73.988953</span></span> |<span data-ttu-id="5be11-268">.7169601222</span><span class="sxs-lookup"><span data-stu-id="5be11-268">.7169601222</span></span> |
| <span data-ttu-id="5be11-269">2</span><span class="sxs-lookup"><span data-stu-id="5be11-269">2</span></span> |<span data-ttu-id="5be11-270">40.715794</span><span class="sxs-lookup"><span data-stu-id="5be11-270">40.715794</span></span> |<span data-ttu-id="5be11-271">-74,010635</span><span class="sxs-lookup"><span data-stu-id="5be11-271">-74,010635</span></span> |<span data-ttu-id="5be11-272">40.725338</span><span class="sxs-lookup"><span data-stu-id="5be11-272">40.725338</span></span> |<span data-ttu-id="5be11-273">-74.00399</span><span class="sxs-lookup"><span data-stu-id="5be11-273">-74.00399</span></span> |<span data-ttu-id="5be11-274">.7448343721</span><span class="sxs-lookup"><span data-stu-id="5be11-274">.7448343721</span></span> |
| <span data-ttu-id="5be11-275">3</span><span class="sxs-lookup"><span data-stu-id="5be11-275">3</span></span> |<span data-ttu-id="5be11-276">40.761456</span><span class="sxs-lookup"><span data-stu-id="5be11-276">40.761456</span></span> |<span data-ttu-id="5be11-277">-73.999886</span><span class="sxs-lookup"><span data-stu-id="5be11-277">-73.999886</span></span> |<span data-ttu-id="5be11-278">40.766544</span><span class="sxs-lookup"><span data-stu-id="5be11-278">40.766544</span></span> |<span data-ttu-id="5be11-279">-73.988228</span><span class="sxs-lookup"><span data-stu-id="5be11-279">-73.988228</span></span> |<span data-ttu-id="5be11-280">0.7037227967</span><span class="sxs-lookup"><span data-stu-id="5be11-280">0.7037227967</span></span> |

### <a name="prepare-data-for-model-building"></a><span data-ttu-id="5be11-281">Příprava dat pro vytváření modelů</span><span class="sxs-lookup"><span data-stu-id="5be11-281">Prepare data for model building</span></span>
<span data-ttu-id="5be11-282">Hello následující dotaz spojení hello **nyctaxi\_cestě** a **nyctaxi\_tarif** tabulky, vygeneruje štítek binární klasifikace **vysypávány**, Popisek více třída klasifikace **tip\_třída**a extrahuje ukázku z hello úplné připojené k datové sadě.</span><span class="sxs-lookup"><span data-stu-id="5be11-282">hello following query joins hello **nyctaxi\_trip** and **nyctaxi\_fare** tables, generates a binary classification label **tipped**, a multi-class classification label **tip\_class**, and extracts a sample from hello full joined dataset.</span></span> <span data-ttu-id="5be11-283">je potřeba Hello vzorkování načítání podmnožinu služebních cest hello na základě výstupní času.</span><span class="sxs-lookup"><span data-stu-id="5be11-283">hello sampling is done by retrieving a subset of hello trips based on pickup time.</span></span>  <span data-ttu-id="5be11-284">Tento dotaz můžete zkopírovat a vložit přímo v hello [Azure Machine Learning Studio](https://studio.azureml.net) [importovat Data] [ import-data] modul pro přijímání přímé dat z instance databáze SQL hello v Azure.</span><span class="sxs-lookup"><span data-stu-id="5be11-284">This query can be copied then pasted directly in hello [Azure Machine Learning Studio](https://studio.azureml.net) [Import Data][import-data] module for direct data ingestion from hello SQL database instance in Azure.</span></span> <span data-ttu-id="5be11-285">dotaz Hello vyloučí záznamy s nesprávnou (0, 0) souřadnice.</span><span class="sxs-lookup"><span data-stu-id="5be11-285">hello query excludes records with incorrect (0, 0) coordinates.</span></span>

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

<span data-ttu-id="5be11-286">Pokud jste připravené tooproceed tooAzure Machine Learning, můžete se buď:</span><span class="sxs-lookup"><span data-stu-id="5be11-286">When you are ready tooproceed tooAzure Machine Learning, you may either:</span></span>  

1. <span data-ttu-id="5be11-287">Uložit hello konečné SQL dotaz tooextract a ukázkové hello dat a způsobené kopírováním a vkládáním hello dotaz přímo do [importovat Data] [ import-data] modulu v Azure Machine Learning, nebo</span><span class="sxs-lookup"><span data-stu-id="5be11-287">Save hello final SQL query tooextract and sample hello data and copy-paste hello query directly into a [Import Data][import-data] module in Azure Machine Learning, or</span></span>
2. <span data-ttu-id="5be11-288">Zachovat hello vzorků a máte v plánu toouse pro model sestavení v nových SQL DW inženýrství dat tabulky a používání hello novou tabulku v hello [importovat Data] [ import-data] modulu v Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="5be11-288">Persist hello sampled and engineered data you plan toouse for model building in a new SQL DW table and use hello new table in hello [Import Data][import-data] module in Azure Machine Learning.</span></span> <span data-ttu-id="5be11-289">Můžete to bylo dokončeno Hello skript prostředí PowerShell v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="5be11-289">hello PowerShell script in earlier step has done this for you.</span></span> <span data-ttu-id="5be11-290">Si můžete přečíst přímo z této tabulky v modulu hello importovat Data.</span><span class="sxs-lookup"><span data-stu-id="5be11-290">You can read directly from this table in hello Import Data module.</span></span>

## <span data-ttu-id="5be11-291"><a name="ipnb"></a>Zkoumání dat a funkce technikům v poznámkovém bloku IPython</span><span class="sxs-lookup"><span data-stu-id="5be11-291"><a name="ipnb"></a>Data exploration and feature engineering in IPython notebook</span></span>
<span data-ttu-id="5be11-292">V této části provedeme zkoumání dat a funkce generování pomocí obou Python a dotazy SQL pro hello SQL DW vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="5be11-292">In this section, we will perform data exploration and feature generation using both Python and SQL queries against hello SQL DW created earlier.</span></span> <span data-ttu-id="5be11-293">Ukázka IPython Poznámkový blok s názvem **SQLDW_Explorations.ipynb** a soubor skriptu jazyka Python **SQLDW_Explorations_Scripts.py** byly stažené tooyour místního adresáře.</span><span class="sxs-lookup"><span data-stu-id="5be11-293">A sample IPython notebook named **SQLDW_Explorations.ipynb** and a Python script file **SQLDW_Explorations_Scripts.py** have been downloaded tooyour local directory.</span></span> <span data-ttu-id="5be11-294">Jsou k dispozici na [Githubu](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW).</span><span class="sxs-lookup"><span data-stu-id="5be11-294">They are also available on [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW).</span></span> <span data-ttu-id="5be11-295">Tyto dva soubory jsou identické v skriptů Python.</span><span class="sxs-lookup"><span data-stu-id="5be11-295">These two files are identical in Python scripts.</span></span> <span data-ttu-id="5be11-296">soubor skriptu jazyka Python Hello je k dispozici tooyou v případě, že jste k serveru IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="5be11-296">hello Python script file is provided tooyou in case you do not have an IPython Notebook server.</span></span> <span data-ttu-id="5be11-297">Tyto dva ukázkové soubory jsou navrženy v části Python **Python 2.7**.</span><span class="sxs-lookup"><span data-stu-id="5be11-297">These two sample Python files are designed under **Python 2.7**.</span></span>

<span data-ttu-id="5be11-298">Hello potřebné informace Azure SQL DW v ukázce hello IPython Poznámkový blok a hello Python skriptu soubor stažený tooyour místního počítače je zapojen skript prostředí PowerShell hello dříve.</span><span class="sxs-lookup"><span data-stu-id="5be11-298">hello needed Azure SQL DW information in hello sample IPython Notebook and hello Python script file downloaded tooyour local machine has been plugged in by hello PowerShell script previously.</span></span> <span data-ttu-id="5be11-299">Jsou spustitelné bez nutnosti jakékoli úpravy.</span><span class="sxs-lookup"><span data-stu-id="5be11-299">They are executable without any modification.</span></span>

<span data-ttu-id="5be11-300">Pokud již jste vytvořili pracovní prostor služby AzureML, můžete přímo nahrát hello Ukázka služby poznámkového bloku IPython AzureML toohello IPython Poznámkový blok a spustit ho.</span><span class="sxs-lookup"><span data-stu-id="5be11-300">If you have already set up an AzureML workspace, you can directly upload hello sample IPython Notebook toohello AzureML IPython Notebook service and start running it.</span></span> <span data-ttu-id="5be11-301">Zde jsou hello kroky tooupload tooAzureML služby IPython Poznámkový blok:</span><span class="sxs-lookup"><span data-stu-id="5be11-301">Here are hello steps tooupload tooAzureML IPython Notebook service:</span></span>

1. <span data-ttu-id="5be11-302">Přihlášení tooyour AzureML prostoru, klikněte v horní části hello "Studio" a klikněte na "Notebooky" na levé straně hello hello webové stránky.</span><span class="sxs-lookup"><span data-stu-id="5be11-302">Log in tooyour AzureML workspace, click "Studio" at hello top, and click "NOTEBOOKS" on hello left side of hello web page.</span></span>
   
    ![Vykreslení #22][22]
2. <span data-ttu-id="5be11-304">Klikněte v levém dolním rohu hello hello webové stránky "NEW" a vyberte "Python 2".</span><span class="sxs-lookup"><span data-stu-id="5be11-304">Click "NEW" on hello left bottom corner of hello web page, and select "Python 2".</span></span> <span data-ttu-id="5be11-305">Potom zadejte název toohello Poznámkový blok a klikněte na hello zaškrtnutí toocreate hello novou prázdnou IPython Poznámkový blok.</span><span class="sxs-lookup"><span data-stu-id="5be11-305">Then, provide a name toohello notebook and click hello check mark toocreate hello new blank IPython Notebook.</span></span>
   
    ![Vykreslení #23][23]
3. <span data-ttu-id="5be11-307">Kliknutím na symbol "Jupyter" hello na hello levého horního rohu hello nový poznámkový blok IPython.</span><span class="sxs-lookup"><span data-stu-id="5be11-307">Click hello "Jupyter" symbol on hello left top corner of hello new IPython Notebook.</span></span>
   
    ![Vykreslení #24][24]
4. <span data-ttu-id="5be11-309">Přetáhnout myší hello ukázka poznámkového bloku IPython toohello **stromu** služby AzureML IPython Poznámkový blok a klikněte na tlačítko **nahrát**.</span><span class="sxs-lookup"><span data-stu-id="5be11-309">Drag and drop hello sample IPython Notebook toohello **tree** page of your AzureML IPython Notebook service, and click **Upload**.</span></span> <span data-ttu-id="5be11-310">Potom bude hello ukázkový poznámkový blok IPython nahrané toohello služby AzureML IPython poznámkového bloku.</span><span class="sxs-lookup"><span data-stu-id="5be11-310">Then, hello sample IPython Notebook will be uploaded toohello AzureML IPython Notebook service.</span></span>
   
    ![Vykreslení #25][25]

<span data-ttu-id="5be11-312">V pořadí toorun hello ukázkové IPython Poznámkový blok nebo hello soubor skriptu jazyka Python, hello následující balíčky Python, které jsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="5be11-312">In order toorun hello sample IPython Notebook or hello Python script file, hello following Python packages are needed.</span></span> <span data-ttu-id="5be11-313">Pokud používáte služby hello AzureML IPython Poznámkový blok, tyto balíčky byly předem nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="5be11-313">If you are using hello AzureML IPython Notebook service, these packages have been pre-installed.</span></span>

    - <span data-ttu-id="5be11-314">pandas</span><span class="sxs-lookup"><span data-stu-id="5be11-314">pandas</span></span>
    - <span data-ttu-id="5be11-315">numpy</span><span class="sxs-lookup"><span data-stu-id="5be11-315">numpy</span></span>
    - <span data-ttu-id="5be11-316">matplotlib</span><span class="sxs-lookup"><span data-stu-id="5be11-316">matplotlib</span></span>
    - <span data-ttu-id="5be11-317">pyodbc</span><span class="sxs-lookup"><span data-stu-id="5be11-317">pyodbc</span></span>
    - <span data-ttu-id="5be11-318">PyTables</span><span class="sxs-lookup"><span data-stu-id="5be11-318">PyTables</span></span>

<span data-ttu-id="5be11-319">Hello doporučuje pořadí při sestavování Pokročilá analytická řešení na AzureML s velkých objemů dat je hello následující:</span><span class="sxs-lookup"><span data-stu-id="5be11-319">hello recommended sequence when building advanced analytical solutions on AzureML with large data is hello following:</span></span>

* <span data-ttu-id="5be11-320">Přečtěte si v malé ukázkové hello dat do data v paměti rámečku.</span><span class="sxs-lookup"><span data-stu-id="5be11-320">Read in a small sample of hello data into an in-memory data frame.</span></span>
* <span data-ttu-id="5be11-321">Provést některé vizualizace a explorations pomocí hello jen Vzorkovaná data.</span><span class="sxs-lookup"><span data-stu-id="5be11-321">Perform some visualizations and explorations using hello sampled data.</span></span>
* <span data-ttu-id="5be11-322">Experimentů pomocí funkce inženýrství pomocí hello Vzorkovat data.</span><span class="sxs-lookup"><span data-stu-id="5be11-322">Experiment with feature engineering using hello sampled data.</span></span>
* <span data-ttu-id="5be11-323">Pro větší zkoumání dat, manipulaci s daty a funkce technikům použijte dotazy SQL tooissue Python přímo pro hello SQL DW.</span><span class="sxs-lookup"><span data-stu-id="5be11-323">For larger data exploration, data manipulation and feature engineering, use Python tooissue SQL Queries directly against hello SQL DW.</span></span>
* <span data-ttu-id="5be11-324">Rozhodněte, hello ukázka velikost toobe vhodný pro vytváření modelů Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="5be11-324">Decide hello sample size toobe suitable for Azure Machine Learning model building.</span></span>

<span data-ttu-id="5be11-325">Hello tady jsou několik zkoumání dat, vizualizace dat a funkce engineering příklady.</span><span class="sxs-lookup"><span data-stu-id="5be11-325">hello followings are a few data exploration, data visualization, and feature engineering examples.</span></span> <span data-ttu-id="5be11-326">Další data explorations naleznete v hello ukázka IPython Poznámkový blok a souboru skriptu jazyka Python ukázka hello.</span><span class="sxs-lookup"><span data-stu-id="5be11-326">More data explorations can be found in hello sample IPython Notebook and hello sample Python script file.</span></span>

### <a name="initialize-database-credentials"></a><span data-ttu-id="5be11-327">Inicializace přihlašovací údaje databáze</span><span class="sxs-lookup"><span data-stu-id="5be11-327">Initialize database credentials</span></span>
<span data-ttu-id="5be11-328">Inicializace nastavení připojení k databázi v hello následující proměnné:</span><span class="sxs-lookup"><span data-stu-id="5be11-328">Initialize your database connection settings in hello following variables:</span></span>

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database driver>

### <a name="create-database-connection"></a><span data-ttu-id="5be11-329">Vytvoření připojení k databázi</span><span class="sxs-lookup"><span data-stu-id="5be11-329">Create database connection</span></span>
<span data-ttu-id="5be11-330">Zde je hello připojovací řetězec, který vytvoří databázi toohello hello připojení.</span><span class="sxs-lookup"><span data-stu-id="5be11-330">Here is hello connection string that creates hello connection toohello database.</span></span>

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a><span data-ttu-id="5be11-331">Sestava počtu řádků a sloupců v tabulce < nyctaxi_trip ></span><span class="sxs-lookup"><span data-stu-id="5be11-331">Report number of rows and columns in table <nyctaxi_trip></span></span>
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

* <span data-ttu-id="5be11-332">Celkový počet řádků = 173179759</span><span class="sxs-lookup"><span data-stu-id="5be11-332">Total number of rows = 173179759</span></span>  
* <span data-ttu-id="5be11-333">Celkový počet sloupců = 14</span><span class="sxs-lookup"><span data-stu-id="5be11-333">Total number of columns = 14</span></span>

### <a name="report-number-of-rows-and-columns-in-table-nyctaxifare"></a><span data-ttu-id="5be11-334">Sestava počtu řádků a sloupců v tabulce < nyctaxi_fare ></span><span class="sxs-lookup"><span data-stu-id="5be11-334">Report number of rows and columns in table <nyctaxi_fare></span></span>
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

* <span data-ttu-id="5be11-335">Celkový počet řádků = 173179759</span><span class="sxs-lookup"><span data-stu-id="5be11-335">Total number of rows = 173179759</span></span>  
* <span data-ttu-id="5be11-336">Celkový počet sloupců = 11</span><span class="sxs-lookup"><span data-stu-id="5be11-336">Total number of columns = 11</span></span>

### <a name="read-in-a-small-data-sample-from-hello-sql-data-warehouse-database"></a><span data-ttu-id="5be11-337">Pro čtení ve vzorku malá data z hello databáze serveru SQL datového skladu</span><span class="sxs-lookup"><span data-stu-id="5be11-337">Read-in a small data sample from hello SQL Data Warehouse Database</span></span>
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
    print 'Time tooread hello sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

<span data-ttu-id="5be11-338">Čas tooread hello ukázková tabulka je 14.096495 sekund.</span><span class="sxs-lookup"><span data-stu-id="5be11-338">Time tooread hello sample table is 14.096495 seconds.</span></span>  
<span data-ttu-id="5be11-339">Počet řádků a sloupců načíst = (1000, 21).</span><span class="sxs-lookup"><span data-stu-id="5be11-339">Number of rows and columns retrieved = (1000, 21).</span></span>

### <a name="descriptive-statistics"></a><span data-ttu-id="5be11-340">Popisný statistiky</span><span class="sxs-lookup"><span data-stu-id="5be11-340">Descriptive statistics</span></span>
<span data-ttu-id="5be11-341">Teď je připraven tooexplore hello Vzorkovat data.</span><span class="sxs-lookup"><span data-stu-id="5be11-341">Now you are ready tooexplore hello sampled data.</span></span> <span data-ttu-id="5be11-342">Začneme s prohlížení statistikami popisný pro hello **cestě\_vzdálenost** (nebo další pole vyberte toospecify).</span><span class="sxs-lookup"><span data-stu-id="5be11-342">We start with looking at some descriptive statistics for hello **trip\_distance** (or any other fields you choose toospecify).</span></span>

    df1['trip_distance'].describe()

### <a name="visualization-box-plot-example"></a><span data-ttu-id="5be11-343">Vizualizace: Příklad vykreslení pole</span><span class="sxs-lookup"><span data-stu-id="5be11-343">Visualization: Box plot example</span></span>
<span data-ttu-id="5be11-344">Další podíváme na hello Krabicový graf pro hello cestě vzdálenost toovisualize hello quantiles.</span><span class="sxs-lookup"><span data-stu-id="5be11-344">Next we look at hello box plot for hello trip distance toovisualize hello quantiles.</span></span>

    df1.boxplot(column='trip_distance',return_type='dict')

![Vykreslení #1][1]

### <a name="visualization-distribution-plot-example"></a><span data-ttu-id="5be11-346">Vizualizaci: Příklad vykreslení distribuční</span><span class="sxs-lookup"><span data-stu-id="5be11-346">Visualization: Distribution plot example</span></span>
<span data-ttu-id="5be11-347">Pozemků, které vizualizovat hello distribuce a histogram hello vzorkovat cestě vzdálenosti.</span><span class="sxs-lookup"><span data-stu-id="5be11-347">Plots that visualize hello distribution and a histogram for hello sampled trip distances.</span></span>

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Vykreslení #2][2]

### <a name="visualization-bar-and-line-plots"></a><span data-ttu-id="5be11-349">Vizualizace: Panel a ukazuje zeměpisný řádku</span><span class="sxs-lookup"><span data-stu-id="5be11-349">Visualization: Bar and line plots</span></span>
<span data-ttu-id="5be11-350">V tomto příkladu jsme bin hello vzdálenost cesty do pěti přihrádek a vizualizovat hello přihrádkování výsledky.</span><span class="sxs-lookup"><span data-stu-id="5be11-350">In this example, we bin hello trip distance into five bins and visualize hello binning results.</span></span>

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

<span data-ttu-id="5be11-351">Jsme můžete vykreslení hello výše bin distribuční panelu nebo výkresu s řádek:</span><span class="sxs-lookup"><span data-stu-id="5be11-351">We can plot hello above bin distribution in a bar or line plot with:</span></span>

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Vykreslení #3][3]

<span data-ttu-id="5be11-353">a</span><span class="sxs-lookup"><span data-stu-id="5be11-353">and</span></span>

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Vykreslení #4][4]

### <a name="visualization-scatterplot-examples"></a><span data-ttu-id="5be11-355">Vizualizaci: Příklady Scatterplot</span><span class="sxs-lookup"><span data-stu-id="5be11-355">Visualization: Scatterplot examples</span></span>
<span data-ttu-id="5be11-356">Ukážeme bodové vykreslení mezi **cestě\_čas\_v\_sekundy** a **cestě\_vzdálenost** toosee, pokud neexistuje žádné korelace</span><span class="sxs-lookup"><span data-stu-id="5be11-356">We show scatter plot between **trip\_time\_in\_secs** and **trip\_distance** toosee if there is any correlation</span></span>

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Vykreslení #6][6]

<span data-ttu-id="5be11-358">Podobně jsme můžete zkontrolovat hello vztah mezi **míra\_kód** a **cestě\_vzdálenost**.</span><span class="sxs-lookup"><span data-stu-id="5be11-358">Similarly we can check hello relationship between **rate\_code** and **trip\_distance**.</span></span>

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![Vykreslení #8][8]

### <a name="data-exploration-on-sampled-data-using-sql-queries-in-ipython-notebook"></a><span data-ttu-id="5be11-360">Zkoumání dat na jen Vzorkovaná data pomocí dotazů SQL v poznámkovém bloku IPython</span><span class="sxs-lookup"><span data-stu-id="5be11-360">Data exploration on sampled data using SQL queries in IPython notebook</span></span>
<span data-ttu-id="5be11-361">V této části nám prozkoumat distribuce dat pomocí hello vzorků dat, který je uchován v hello nové tabulky, kterou jsme vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="5be11-361">In this section, we explore data distributions using hello sampled data which is persisted in hello new table we created above.</span></span> <span data-ttu-id="5be11-362">Všimněte si, že podobné explorations je možné provést pomocí hello původní tabulky.</span><span class="sxs-lookup"><span data-stu-id="5be11-362">Note that similar explorations can be performed using hello original tables.</span></span>

#### <a name="exploration-report-number-of-rows-and-columns-in-hello-sampled-table"></a><span data-ttu-id="5be11-363">Zkoumání: Sestava počtu řádků a sloupců v hello vzorků tabulky</span><span class="sxs-lookup"><span data-stu-id="5be11-363">Exploration: Report number of rows and columns in hello sampled table</span></span>
    nrows = pd.read_sql('''SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_sample>')''', conn)
    print 'Number of rows in sample = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''SELECT count(*) FROM information_schema.columns WHERE table_name = ('<nyctaxi_sample>') AND table_schema = '<schemaname>'''', conn)
    print 'Number of columns in sample = %d' % ncols.iloc[0,0]

#### <a name="exploration-tippednot-tripped-distribution"></a><span data-ttu-id="5be11-364">Zkoumání: Vysypávány nebo nebyla přerušovačů distribuce</span><span class="sxs-lookup"><span data-stu-id="5be11-364">Exploration: Tipped/not tripped Distribution</span></span>
    query = '''
        SELECT tipped, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tipped
        '''

    pd.read_sql(query, conn)

#### <a name="exploration-tip-class-distribution"></a><span data-ttu-id="5be11-365">Zkoumání: Distribuce třída Tip</span><span class="sxs-lookup"><span data-stu-id="5be11-365">Exploration: Tip class distribution</span></span>
    query = '''
        SELECT tip_class, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tip_class
    '''

    tip_class_dist = pd.read_sql(query, conn)

#### <a name="exploration-plot-hello-tip-distribution-by-class"></a><span data-ttu-id="5be11-366">Zkoumání: Vykreslení hello tip distribuční třídou</span><span class="sxs-lookup"><span data-stu-id="5be11-366">Exploration: Plot hello tip distribution by class</span></span>
    tip_class_dist['tip_freq'].plot(kind='bar')

![Vykreslení #26][26]

#### <a name="exploration-daily-distribution-of-trips"></a><span data-ttu-id="5be11-368">Zkoumání: Denní distribuce služebních cest</span><span class="sxs-lookup"><span data-stu-id="5be11-368">Exploration: Daily distribution of trips</span></span>
    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a><span data-ttu-id="5be11-369">Zkoumání: Distribuční cesty za Medailon</span><span class="sxs-lookup"><span data-stu-id="5be11-369">Exploration: Trip distribution per medallion</span></span>
    query = '''
        SELECT medallion,count(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a><span data-ttu-id="5be11-370">Zkoumání: Cestě distribuce podle Medailon a hackerský licencí</span><span class="sxs-lookup"><span data-stu-id="5be11-370">Exploration: Trip distribution by medallion and hack license</span></span>
    query = '''select medallion, hack_license,count(*) from <schemaname>.<nyctaxi_sample> group by medallion, hack_license'''
    pd.read_sql(query,conn)


#### <a name="exploration-trip-time-distribution"></a><span data-ttu-id="5be11-371">Zkoumání: Distribuce doby cestě</span><span class="sxs-lookup"><span data-stu-id="5be11-371">Exploration: Trip time distribution</span></span>
    query = '''select trip_time_in_secs, count(*) from <schemaname>.<nyctaxi_sample> group by trip_time_in_secs order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-trip-distance-distribution"></a><span data-ttu-id="5be11-372">Zkoumání: Distribuce vzdálenost cestě</span><span class="sxs-lookup"><span data-stu-id="5be11-372">Exploration: Trip distance distribution</span></span>
    query = '''select floor(trip_distance/5)*5 as tripbin, count(*) from <schemaname>.<nyctaxi_sample> group by floor(trip_distance/5)*5 order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-payment-type-distribution"></a><span data-ttu-id="5be11-373">Zkoumání: Distribuce typ platby</span><span class="sxs-lookup"><span data-stu-id="5be11-373">Exploration: Payment type distribution</span></span>
    query = '''select payment_type,count(*) from <schemaname>.<nyctaxi_sample> group by payment_type'''
    pd.read_sql(query,conn)

#### <a name="verify-hello-final-form-of-hello-featurized-table"></a><span data-ttu-id="5be11-374">Ověření formuláře konečné hello hello featurized tabulky</span><span class="sxs-lookup"><span data-stu-id="5be11-374">Verify hello final form of hello featurized table</span></span>
    query = '''SELECT TOP 100 * FROM <schemaname>.<nyctaxi_sample>'''
    pd.read_sql(query,conn)

## <span data-ttu-id="5be11-375"><a name="mlmodel"></a>Vytvářet modely v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="5be11-375"><a name="mlmodel"></a>Build models in Azure Machine Learning</span></span>
<span data-ttu-id="5be11-376">Snažíme se teď připravena tooproceed toomodel sestavení a nasazení modelu v [Azure Machine Learning](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="5be11-376">We are now ready tooproceed toomodel building and model deployment in [Azure Machine Learning](https://studio.azureml.net).</span></span> <span data-ttu-id="5be11-377">Hello data jsou připravena toobe použít v některé z problémů předpovědi hello identifikuje dříve, konkrétně:</span><span class="sxs-lookup"><span data-stu-id="5be11-377">hello data is ready toobe used in any of hello prediction problems identified earlier, namely:</span></span>

1. <span data-ttu-id="5be11-378">**Binární klasifikace**: toopredict, jestli byl tip placené cesty.</span><span class="sxs-lookup"><span data-stu-id="5be11-378">**Binary classification**: toopredict whether or not a tip was paid for a trip.</span></span>
2. <span data-ttu-id="5be11-379">**Více třídami klasifikace**: rozsah hello toopredict tipu placené, podle toohello dříve definovaných tříd.</span><span class="sxs-lookup"><span data-stu-id="5be11-379">**Multiclass classification**: toopredict hello range of tip paid, according toohello previously defined classes.</span></span>
3. <span data-ttu-id="5be11-380">**Úloha regrese**: toopredict hello množství tip placené cesty.</span><span class="sxs-lookup"><span data-stu-id="5be11-380">**Regression task**: toopredict hello amount of tip paid for a trip.</span></span>  

<span data-ttu-id="5be11-381">toobegin hello modelování cvičení, přihlaste se tooyour **Azure Machine Learning** pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="5be11-381">toobegin hello modeling exercise, log in tooyour **Azure Machine Learning** workspace.</span></span> <span data-ttu-id="5be11-382">Pokud jste ještě nevytvořili machine learning pracovního prostoru, přečtěte si téma [vytvořit pracovní prostor služby Azure ML](machine-learning-create-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="5be11-382">If you have not yet created a machine learning workspace, see [Create an Azure ML workspace](machine-learning-create-workspace.md).</span></span>

1. <span data-ttu-id="5be11-383">tooget začít s Azure Machine Learning, najdete v části [co je Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)</span><span class="sxs-lookup"><span data-stu-id="5be11-383">tooget started with Azure Machine Learning, see [What is Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)</span></span>
2. <span data-ttu-id="5be11-384">Přihlaste se příliš[Azure Machine Learning Studio](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="5be11-384">Log in too[Azure Machine Learning Studio](https://studio.azureml.net).</span></span>
3. <span data-ttu-id="5be11-385">Hello Studio Domovská stránka obsahuje širokou řadu informací, videa, kurzy, odkazy toohello odkaz moduly a dalším prostředkům.</span><span class="sxs-lookup"><span data-stu-id="5be11-385">hello Studio Home page provides a wealth of information, videos, tutorials, links toohello Modules Reference, and other resources.</span></span> <span data-ttu-id="5be11-386">Další informace o Azure Machine Learning najdete hello [centru dokumentace Azure Machine Learning na](https://azure.microsoft.com/documentation/services/machine-learning/).</span><span class="sxs-lookup"><span data-stu-id="5be11-386">For more information about Azure Machine Learning, consult hello [Azure Machine Learning Documentation Center](https://azure.microsoft.com/documentation/services/machine-learning/).</span></span>

<span data-ttu-id="5be11-387">Typické výukový experiment sestává z hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="5be11-387">A typical training experiment consists of hello following steps:</span></span>

1. <span data-ttu-id="5be11-388">Vytvoření **+ nový** experiment.</span><span class="sxs-lookup"><span data-stu-id="5be11-388">Create a **+NEW** experiment.</span></span>
2. <span data-ttu-id="5be11-389">Získáte hello data do Azure ML.</span><span class="sxs-lookup"><span data-stu-id="5be11-389">Get hello data into Azure ML.</span></span>
3. <span data-ttu-id="5be11-390">Předběžně zpracovat, transformace a pracovat s daty hello podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="5be11-390">Pre-process, transform and manipulate hello data as needed.</span></span>
4. <span data-ttu-id="5be11-391">Funkce vygenerujte, podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="5be11-391">Generate features as needed.</span></span>
5. <span data-ttu-id="5be11-392">Rozdělení dat hello na školení, ověření nebo testování datové sady (nebo samostatné datové sady pro každou).</span><span class="sxs-lookup"><span data-stu-id="5be11-392">Split hello data into training/validation/testing datasets(or have separate datasets for each).</span></span>
6. <span data-ttu-id="5be11-393">Vyberte jeden nebo více algoritmů strojového učení v závislosti na hello učení toosolve problém.</span><span class="sxs-lookup"><span data-stu-id="5be11-393">Select one or more machine learning algorithms depending on hello learning problem toosolve.</span></span> <span data-ttu-id="5be11-394">Například: binární klasifikace, klasifikace více třídami, regresi.</span><span class="sxs-lookup"><span data-stu-id="5be11-394">E.g., binary classification, multiclass classification, regression.</span></span>
7. <span data-ttu-id="5be11-395">Cvičení jednoho nebo více modelů použití hello školení datové sady.</span><span class="sxs-lookup"><span data-stu-id="5be11-395">Train one or more models using hello training dataset.</span></span>
8. <span data-ttu-id="5be11-396">Stanovení skóre hello ověření datové sady pomocí vyškolení modely hello.</span><span class="sxs-lookup"><span data-stu-id="5be11-396">Score hello validation dataset using hello trained model(s).</span></span>
9. <span data-ttu-id="5be11-397">Vyhodnoťte hello modely toocompute hello relevantní metriky pro hello učení problém.</span><span class="sxs-lookup"><span data-stu-id="5be11-397">Evaluate hello model(s) toocompute hello relevant metrics for hello learning problem.</span></span>
10. <span data-ttu-id="5be11-398">Vyladění hello modely a vyberte hello nejlepší toodeploy modelu.</span><span class="sxs-lookup"><span data-stu-id="5be11-398">Fine tune hello model(s) and select hello best model toodeploy.</span></span>

<span data-ttu-id="5be11-399">V tomto cvičení jsme již prozkoumali a analýzou dat hello v SQL Data Warehouse a rozhodli tooingest velikost vzorku hello v Azure ML.</span><span class="sxs-lookup"><span data-stu-id="5be11-399">In this exercise, we have already explored and engineered hello data in SQL Data Warehouse, and decided on hello sample size tooingest in Azure ML.</span></span> <span data-ttu-id="5be11-400">Tady je postup toobuild hello jeden nebo více modelů předpovědi hello:</span><span class="sxs-lookup"><span data-stu-id="5be11-400">Here is hello procedure toobuild one or more of hello prediction models:</span></span>

1. <span data-ttu-id="5be11-401">Získat hello data do Azure ML pomocí hello [importovat Data] [ import-data] modulu, k dispozici v hello **vstupu a výstupu dat** části.</span><span class="sxs-lookup"><span data-stu-id="5be11-401">Get hello data into Azure ML using hello [Import Data][import-data] module, available in hello **Data Input and Output** section.</span></span> <span data-ttu-id="5be11-402">Další informace najdete v tématu hello [importovat Data] [ import-data] stránka s referencemi modul.</span><span class="sxs-lookup"><span data-stu-id="5be11-402">For more information, see hello [Import Data][import-data] module reference page.</span></span>
   
    ![Azure ML Import dat][17]
2. <span data-ttu-id="5be11-404">Vyberte **Azure SQL Database** jako hello **zdroj dat** v hello **vlastnosti** panelu.</span><span class="sxs-lookup"><span data-stu-id="5be11-404">Select **Azure SQL Database** as hello **Data source** in hello **Properties** panel.</span></span>
3. <span data-ttu-id="5be11-405">Zadejte název DNS hello databáze v hello **název databázového serveru** pole.</span><span class="sxs-lookup"><span data-stu-id="5be11-405">Enter hello database DNS name in hello **Database server name** field.</span></span> <span data-ttu-id="5be11-406">Formát:`tcp:<your_virtual_machine_DNS_name>,1433`</span><span class="sxs-lookup"><span data-stu-id="5be11-406">Format: `tcp:<your_virtual_machine_DNS_name>,1433`</span></span>
4. <span data-ttu-id="5be11-407">Zadejte hello **název databáze** v odpovídající pole hello.</span><span class="sxs-lookup"><span data-stu-id="5be11-407">Enter hello **Database name** in hello corresponding field.</span></span>
5. <span data-ttu-id="5be11-408">Zadejte hello *uživatelské jméno SQL* v hello **název uživatelského účtu serveru**a hello *heslo* v hello **heslo uživatelského účtu serveru**.</span><span class="sxs-lookup"><span data-stu-id="5be11-408">Enter hello *SQL user name* in hello **Server user account name**, and hello *password* in hello **Server user account password**.</span></span>
6. <span data-ttu-id="5be11-409">Zkontrolujte hello **přijmout některý z certifikátů serveru** možnost.</span><span class="sxs-lookup"><span data-stu-id="5be11-409">Check hello **Accept any server certificate** option.</span></span>
7. <span data-ttu-id="5be11-410">V hello **databázový dotaz** upravit textová oblast, vložte hello dotazu, který extrahuje hello potřeby databáze pole (včetně všech počítané pole, jako je popisky hello) a dolů – ukázky velikost vzorku toohello požadovaných dat hello.</span><span class="sxs-lookup"><span data-stu-id="5be11-410">In hello **Database query** edit text area, paste hello query which extracts hello necessary database fields (including any computed fields such as hello labels) and down samples hello data toohello desired sample size.</span></span>

<span data-ttu-id="5be11-411">Je například binární klasifikace experimentů, čtení dat přímo z databáze SQL Data Warehouse hello hello obrázek (mějte na paměti, tooreplace hello názvy tabulek nyctaxi_trip a nyctaxi_fare ve schématu hello název a hello názvy tabulek, kterou jste použili v vaší návod).</span><span class="sxs-lookup"><span data-stu-id="5be11-411">An example of a binary classification experiment reading data directly from hello SQL Data Warehouse database is in hello figure below (remember tooreplace hello table names nyctaxi_trip and nyctaxi_fare by hello schema name and hello table names you used in your walkthrough).</span></span> <span data-ttu-id="5be11-412">Podobně jako experimenty konstruovat pro více třídami klasifikaci a regresní problémy.</span><span class="sxs-lookup"><span data-stu-id="5be11-412">Similar experiments can be constructed for multiclass classification and regression problems.</span></span>

![Azure ML Train][10]

> [!IMPORTANT]
> <span data-ttu-id="5be11-414">V hello modelování extrakce dat a vzorkuje příklady dotazu uvedené v předchozí části **všech popisků hello tři modelování cvičení jsou zahrnuty v dotazu hello**.</span><span class="sxs-lookup"><span data-stu-id="5be11-414">In hello modeling data extraction and sampling query examples provided in previous sections, **all labels for hello three modeling exercises are included in hello query**.</span></span> <span data-ttu-id="5be11-415">Důležitým krokem (povinné) v každé z hello modelování cvičení je příliš**vyloučit** hello nepotřebné štítky pro hello další dva problémy a všemi ostatními **cíle nevracení**.</span><span class="sxs-lookup"><span data-stu-id="5be11-415">An important (required) step in each of hello modeling exercises is too**exclude** hello unnecessary labels for hello other two problems, and any other **target leaks**.</span></span> <span data-ttu-id="5be11-416">Například při použití binární klasifikace, použít hello popisek **vysypávány** a vyloučit hello pole **tip\_– třída**, **tip\_velikost**, a **celkový\_velikost**.</span><span class="sxs-lookup"><span data-stu-id="5be11-416">For example, when using binary classification, use hello label **tipped** and exclude hello fields **tip\_class**, **tip\_amount**, and **total\_amount**.</span></span> <span data-ttu-id="5be11-417">Hello druhé jsou placené cíl nevracení vzhledem k tomu, že implikují hello tip.</span><span class="sxs-lookup"><span data-stu-id="5be11-417">hello latter are target leaks since they imply hello tip paid.</span></span>
> 
> <span data-ttu-id="5be11-418">tooexclude všechny nepotřebné sloupce nebo nevracení cíl, můžete použít hello [výběr sloupců v datové sadě] [ select-columns] modulu nebo hello [upravit Metadata] [ edit-metadata].</span><span class="sxs-lookup"><span data-stu-id="5be11-418">tooexclude any unnecessary columns or target leaks, you may use hello [Select Columns in Dataset][select-columns] module or hello [Edit Metadata][edit-metadata].</span></span> <span data-ttu-id="5be11-419">Další informace najdete v tématu [výběr sloupců v datové sadě] [ select-columns] a [upravit Metadata] [ edit-metadata] referenční stránky.</span><span class="sxs-lookup"><span data-stu-id="5be11-419">For more information, see [Select Columns in Dataset][select-columns] and [Edit Metadata][edit-metadata] reference pages.</span></span>
> 
> 

## <span data-ttu-id="5be11-420"><a name="mldeploy"></a>Nasazení modely v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="5be11-420"><a name="mldeploy"></a>Deploy models in Azure Machine Learning</span></span>
<span data-ttu-id="5be11-421">Pokud váš model je připraven, můžete snadno nasadit ho jako webovou službu přímo z hello experimentu.</span><span class="sxs-lookup"><span data-stu-id="5be11-421">When your model is ready, you can easily deploy it as a web service directly from hello experiment.</span></span> <span data-ttu-id="5be11-422">Další informace o nasazení webové služby Azure ML najdete v tématu [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="5be11-422">For more information about deploying Azure ML web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

<span data-ttu-id="5be11-423">toodeploy novou webovou službu, musíte:</span><span class="sxs-lookup"><span data-stu-id="5be11-423">toodeploy a new web service, you need to:</span></span>

1. <span data-ttu-id="5be11-424">Vytvoření experimentu vyhodnocování.</span><span class="sxs-lookup"><span data-stu-id="5be11-424">Create a scoring experiment.</span></span>
2. <span data-ttu-id="5be11-425">Nasaďte hello webové služby.</span><span class="sxs-lookup"><span data-stu-id="5be11-425">Deploy hello web service.</span></span>

<span data-ttu-id="5be11-426">toocreate vyhodnocování experimentovat z **dokončeno** cvičení experiment, klikněte na tlačítko **vytvořit vyhodnocování EXPERIMENTOVAT** akce panelu nižší hello.</span><span class="sxs-lookup"><span data-stu-id="5be11-426">toocreate a scoring experiment from a **Finished** training experiment, click **CREATE SCORING EXPERIMENT** in hello lower action bar.</span></span>

![Výpočet skóre Azure][18]

<span data-ttu-id="5be11-428">Azure Machine Learning se pokusí toocreate vyhodnocování experimentu podle hello součástí hello výukový experiment.</span><span class="sxs-lookup"><span data-stu-id="5be11-428">Azure Machine Learning will attempt toocreate a scoring experiment based on hello components of hello training experiment.</span></span> <span data-ttu-id="5be11-429">Konkrétně se:</span><span class="sxs-lookup"><span data-stu-id="5be11-429">In particular, it will:</span></span>

1. <span data-ttu-id="5be11-430">Uložit hello trained model a odeberte hello modelu školení moduly.</span><span class="sxs-lookup"><span data-stu-id="5be11-430">Save hello trained model and remove hello model training modules.</span></span>
2. <span data-ttu-id="5be11-431">Identifikovat logickou **vstupní port** toorepresent hello očekávána vstupní data schématu.</span><span class="sxs-lookup"><span data-stu-id="5be11-431">Identify a logical **input port** toorepresent hello expected input data schema.</span></span>
3. <span data-ttu-id="5be11-432">Identifikovat logickou **výstupní port** toorepresent hello očekávané webové služby výstupního schématu.</span><span class="sxs-lookup"><span data-stu-id="5be11-432">Identify a logical **output port** toorepresent hello expected web service output schema.</span></span>

<span data-ttu-id="5be11-433">Když je vytvořen hello vyhodnocování experiment, zkontrolujte ji a proveďte podle potřeby upravte.</span><span class="sxs-lookup"><span data-stu-id="5be11-433">When hello scoring experiment is created, review it and make adjust as needed.</span></span> <span data-ttu-id="5be11-434">Typické úpravu je tooreplace hello vstupní datové sady nebo dotaz s jedním, který vyloučí popisek pole, protože tyto nebudete mít k dispozici, když je volána hello služby.</span><span class="sxs-lookup"><span data-stu-id="5be11-434">A typical adjustment is tooreplace hello input dataset and/or query with one which excludes label fields, as these will not be available when hello service is called.</span></span> <span data-ttu-id="5be11-435">Je také že vhodné tooreduce hello velikost hello vstupní datové sady nebo tooa několik záznamů, akorát tooindicate hello vstupní schéma.</span><span class="sxs-lookup"><span data-stu-id="5be11-435">It is also a good practice tooreduce hello size of hello input dataset and/or query tooa few records, just enough tooindicate hello input schema.</span></span> <span data-ttu-id="5be11-436">Pro hello výstupní port, je běžné tooexclude všechny vstupní pole a obsahovat jenom hello **popisky vyhodnocení** a **skóre pro Magnitudu Pravděpodobnostech** v hello výstup pomocí hello [výběr sloupců v datové sadě ] [ select-columns] modulu.</span><span class="sxs-lookup"><span data-stu-id="5be11-436">For hello output port, it is common tooexclude all input fields and only include hello **Scored Labels** and **Scored Probabilities** in hello output using hello [Select Columns in Dataset][select-columns] module.</span></span>

<span data-ttu-id="5be11-437">Ukázka vyhodnocování experimentu je součástí hello obrázek.</span><span class="sxs-lookup"><span data-stu-id="5be11-437">A sample scoring experiment is provided in hello figure below.</span></span> <span data-ttu-id="5be11-438">Jakmile budete připraveni toodeploy, klikněte na tlačítko hello **publikování webové služby** tlačítka na dolním panelu akcí hello.</span><span class="sxs-lookup"><span data-stu-id="5be11-438">When ready toodeploy, click hello **PUBLISH WEB SERVICE** button in hello lower action bar.</span></span>

![Publikování Azure ML][11]

## <a name="summary"></a><span data-ttu-id="5be11-440">Souhrn</span><span class="sxs-lookup"><span data-stu-id="5be11-440">Summary</span></span>
<span data-ttu-id="5be11-441">toorecap jste vytvořili prostředí vědecké účely dat Azure, práce s velké veřejné datové sady, trvá prostřednictvím hello proces vědecké účely Team dat, všechny způsob hello z školení toomodel pořízení data, co jsme provedli v tomto kurzu návod a potom toohello nasazení webové služby Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="5be11-441">toorecap what we have done in this walkthrough tutorial, you have created an Azure data science environment, worked with a large public dataset, taking it through hello Team Data Science Process, all hello way from data acquisition toomodel training, and then toohello deployment of an Azure Machine Learning web service.</span></span>

### <a name="license-information"></a><span data-ttu-id="5be11-442">Informace o licenci</span><span class="sxs-lookup"><span data-stu-id="5be11-442">License information</span></span>
<span data-ttu-id="5be11-443">Tento návod ukázka a jeho doplňujícími skripty a IPython notebook(s) sdílí Microsoft v rámci licencí MIT hello.</span><span class="sxs-lookup"><span data-stu-id="5be11-443">This sample walkthrough and its accompanying scripts and IPython notebook(s) are shared by Microsoft under hello MIT license.</span></span> <span data-ttu-id="5be11-444">Zkontrolujte prosím soubor LICENSE.txt hello v adresáři hello hello ukázkový kód na Githubu další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="5be11-444">Please check hello LICENSE.txt file in in hello directory of hello sample code on GitHub for more details.</span></span>

## <a name="references"></a><span data-ttu-id="5be11-445">Odkazy</span><span class="sxs-lookup"><span data-stu-id="5be11-445">References</span></span>
<span data-ttu-id="5be11-446">• [Stránce pro stažení Andrés Monroy NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/)</span><span class="sxs-lookup"><span data-stu-id="5be11-446">•    [Andrés Monroy NYC Taxi Trips Download Page](http://www.andresmh.com/nyctaxitrips/)</span></span>  
<span data-ttu-id="5be11-447">• [FOILing NYC Taxi cestě dat podle Whong Jan](http://chriswhong.com/open-data/foil_nyc_taxi/) </span><span class="sxs-lookup"><span data-stu-id="5be11-447">•    [FOILing NYC’s Taxi Trip Data by Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/) </span></span>  
<span data-ttu-id="5be11-448">• [NYC taxíkem a Limousine Komise výzkum a statistiky](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span><span class="sxs-lookup"><span data-stu-id="5be11-448">•    [NYC Taxi and Limousine Commission Research and Statistics](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)</span></span>

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
