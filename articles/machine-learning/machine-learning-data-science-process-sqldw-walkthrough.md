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
# <a name="hello-team-data-science-process-in-action-using-sql-data-warehouse"></a>Hello proces vědecké účely Team dat v akci: pomocí SQL Data Warehouse
V tomto kurzu jsme vás provede procesem vytváření a nasazování modelu strojového učení pomocí SQL datového skladu (SQL DW) pro veřejně dostupné datové sady – hello [NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/) datovou sadu. Hello binární klasifikace model sestavený předpovídá, zda je tip placené cesty a které předpovědi hello distribuce pro objemy tip hello placené jsou popsány i modely pro více třídami klasifikace a regrese.

Postup Hello následuje hello [tým datové vědy procesu (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) pracovního postupu. Ukážeme, jak toosetup prostředí vědecké účely data jak tooload hello data do datového skladu SQL a o použití datového skladu SQL nebo IPython Poznámkový blok tooexplore hello dat a pracovníka funkce toomodel. Potom ukážeme, jak toobuild a model pomocí Azure Machine Learning nasadit.

## <a name="dataset"></a>datovou sadu cest taxíkem NYC Hello
Hello NYC taxíkem cestě dat se skládá z přibližně 20GB komprimovaných souborů CSV (nekomprimovaným ~ 48GB), zaznamenávání 173 milionů jednotlivých cest a hello tarify placené pro každou cestu. Každý záznam cestě zahrnuje hello vyzvednutí a odkládací umístění a časy, anonymní zabezpečení číslo licence (ovladač) a hello číslo Medailon (taxi na jedinečné id). Hello dat obsahuje všechny služebních cest v hello roku 2013 a je součástí hello následující dvě datové sady pro každý měsíc:

1. Hello **trip_data.csv** soubor obsahuje podrobnosti o cestě, například na počtu cestujících, vyzvednutí a dropoff body, doba trvání cesty a délka cesty. Tady je několik ukázkových záznamů:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. Hello **trip_fare.csv** soubor obsahuje podrobnosti o tarif hello placené pro každou cestu, například typ platby, velikost tarif, příplatek a daně, tipy a mýtné a celkovou velikost hello placené. Tady je několik ukázkových záznamů:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Hello **jedinečný klíč** používá toojoin cestě\_dat a cesty\_tarif se skládá z hello následující tři pole:

* medailonu,
* zabezpečení\_licencí a
* vyzvednutí\_data a času.

## <a name="mltasks"></a>Adresa tři typy úloh předpovědi
Jsme formulovali tři předpovědi problémy podle hello *tip\_velikost* tooillustrate tři druhy modelování úlohy:

1. **Binární klasifikace**: toopredict, jestli tip byl placené cesty, tj. *tip\_velikost* větší než $0 je pozitivní příklad, při *tip\_velikost* $ 0 je záporný příklad.
2. **Více třídami klasifikace**: rozsah hello toopredict tipu zaplacení hello cesty. Jsme rozdělit hello *tip\_velikost* do pěti přihrádek nebo třídy:
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. **Úloha regrese**: toopredict hello množství tip placené cesty.  

## <a name="setup"></a>Nastavení prostředí vědecké účely hello dat Azure pro pokročilou analýzu
tooset prostředí vědecké zpracování dat Azure, postupujte podle těchto kroků.

**Vytvořit svůj vlastní účet úložiště objektů blob v Azure**

* Při zřizování úložiště objektů blob v Azure, zvolte geografické umístění úložiště objektů blob v Azure v nebo co nejblíže příliš**jihu USA**, který je uloží hello NYC taxíkem data. Hello data se zkopírují pomocí AzCopy z hello veřejného objektu blob úložiště kontejneru tooa kontejneru v účtu úložiště. Hello blíže služby Azure blob storage je tooSouth střed USA, hello rychleji (krok 4) Tato úloha se dokončí.
* účet úložiště Azure toocreate, hello postupujte podle kroků uvedených v [účty Azure storage](../storage/common/storage-create-storage-account.md). Bude potřeba dále v tomto návodu být jisti toomake poznámky k hello hodnoty pro následující přihlašovací údaje účtu úložiště.
  
  * **Název účtu úložiště**
  * **Klíče účtu úložiště.**
  * **Název kontejneru** (aplikaci, kterou chcete toobe hello data uložená v hello úložiště objektů blob v Azure)

**Zřízení instance Azure SQL DW.**
Postupujte podle dokumentace hello v [vytvořit SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) tooprovision instanci SQL Data Warehouse. Ujistěte se, abyste vytvořili zápisy na hello následující přihlašovací údaje SQL Data Warehouse, které se použije v dalších krocích.

* **Název serveru**: <server Name>. database.windows.net
* **Název SQLDW (databáze)**
* **Uživatelské jméno**
* **Heslo**

**Instalace sady Visual Studio a SQL Server Data Tools.** Pokyny najdete v tématu [instalaci sady Visual Studio 2015 a SSDT (SQL Server Data Tools) pro SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md).

**Připojte tooyour Azure SQL DW pomocí sady Visual Studio.** Pokyny najdete v tématu kroky 1 a 2 v [připojit tooAzure SQL Data Warehouse pomocí sady Visual Studio](../sql-data-warehouse/sql-data-warehouse-connect-overview.md).

> [!NOTE]
> Spuštění hello následující dotaz SQL na databázi hello jste vytvořili v SQL Data Warehouse (místo hello dotazu zadaný v kroku 3 hello připojit tématu) příliš**vytvoření hlavního klíče**.
> 
> 

    BEGIN TRY
           --Try toocreate hello master key
        CREATE MASTER KEY
    END TRY
    BEGIN CATCH
           --If hello master key exists, do nothing
    END CATCH;

**Vytvořte pracovní prostor služby Azure Machine Learning v rámci vašeho předplatného Azure.** Pokyny najdete v tématu [vytvořit pracovní prostor služby Azure Machine Learning](machine-learning-create-workspace.md).

## <a name="getdata"></a>Načtení hello dat do SQL Data Warehouse
Otevřete konzolu příkazového prostředí Windows PowerShell. Spustit hello následující příkazy prostředí PowerShell toodownload hello příklad SQL skriptu soubory, které můžeme sdílet s vámi na Githubu tooa místní adresář s parametrem hello *- DestDir*. Můžete změnit hello hodnota parametru *- DestDir* tooany místního adresáře. Pokud *- DestDir* neexistuje, vytvoří se tím hello skript prostředí PowerShell.

> [!NOTE]
> Může být nutné příliš**spustit jako správce** při provádění hello následující skript prostředí PowerShell, pokud vaše *DestDir* directory potřebuje správce oprávnění toocreate nebo toowrite tooit.
> 
> 

    $source = "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/Download_Scripts_SQLDW_Walkthrough.ps1"
    $ps1_dest = "$pwd\Download_Scripts_SQLDW_Walkthrough.ps1"
    $wc = New-Object System.Net.WebClient
    $wc.DownloadFile($source, $ps1_dest)
    .\Download_Scripts_SQLDW_Walkthrough.ps1 –DestDir 'C:\tempSQLDW'

Po úspěšném provedení změny aktuální pracovní adresář příliš*- DestDir*. Nyní byste měli mít toosee obrazovku podobnou následující:

![][19]

Ve vašem *- DestDir*, spustit následující skript prostředí PowerShell v režimu správce hello:

    ./SQLDW_Data_Import.ps1

Spuštění hello skript prostředí PowerShell pro hello poprvé, zobrazí se výzva tooinput hello informace z vašeho datového skladu SQL Azure a účtu úložiště objektů blob v Azure. Po dokončení tohoto skriptu prostředí PowerShell systémem pro hello poprvé, přihlašovací údaje hello vstup vám bude mít byla zapsána tooa konfigurační soubor SQLDW.conf v hello přítomen pracovní adresář. Hello budoucí spustit tento soubor skriptu PowerShell má možnost tooread hello všechny potřebné parametry z tohoto konfiguračního souboru. Pokud potřebujete toochange některé parametry, můžete vybrat tooinput hello parametry úvodní obrazovka řádku odstranit tento konfigurační soubor a po zobrazení výzvy zadání hodnot parametrů hello nebo hodnoty parametru hello toochange úpravou souboru SQLDW.conf hello ve vašem *- DestDir* adresáře.

> [!NOTE]
> V pořadí tooavoid schématu název je v konfliktu s těmi, které již existují v Azure SQL DW, při čtení parametry přímo ze souboru SQLDW.conf hello náhodné číslo 3 číslice je přidat název schématu toohello ze souboru SQLDW.conf hello jako hello výchozí název schématu pro každé spuštění. Hello skript prostředí PowerShell můžete být vyzváni k zadání názvu schématu: uvážení uživatele může být zadán název hello.
> 
> 

To **skript prostředí PowerShell** souboru dokončení hello následující úlohy:

* **Stáhne a nainstaluje AzCopy**, pokud ještě není nainstalovaný nástroj AzCopy
  
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
* **Zkopíruje data tooyour privátní objekt blob úložiště účet** z veřejného objektu blob hello s AzCopy
  
        Write-Host "AzCopy is copying data from public blob tooyo storage account. It may take a while..." -ForegroundColor "Yellow"
        $start_time = Get-Date
        AzCopy.exe /Source:$Source /Dest:$DestURL /DestKey:$StorageAccountKey /S
        $end_time = Get-Date
        $time_span = $end_time - $start_time
        $total_seconds = [math]::Round($time_span.TotalSeconds,2)
        Write-Host "AzCopy finished copying data. Please check your storage account tooverify." -ForegroundColor "Yellow"
        Write-Host "This step (copying data from public blob tooyour storage account) takes $total_seconds seconds." -ForegroundColor "Green"
* **Načte data pomocí funkce Polybase (spuštěním LoadDataToSQLDW.sql) tooyour Azure SQL DW** z vašeho účtu úložiště objektů blob privátní s hello následující příkazy.
  
  * Vytvořte schéma
    
          EXEC (''CREATE SCHEMA {schemaname};'');
  * Vytvoření oboru databáze přihlašovacích údajů
    
          CREATE DATABASE SCOPED CREDENTIAL {KeyAlias}
          WITH IDENTITY = ''asbkey'' ,
          Secret = ''{StorageAccountKey}''
  * Vytvoření externího zdroje dat pro objekt blob úložiště Azure
    
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
  * Vytvořte formátu externí soubor pro soubor csv. Data nekomprimované a pole jsou odděleny znakem hello.
    
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
  * Vytvořte externí tarif a tabulky cesty pro datovou sadu taxíkem NYC v Azure blob storage.
    
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

    - Načtení dat z externí tabulky v Azure blob storage tooSQL datového skladu

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

    - Vytvoří tabulku ukázkových dat (NYCTaxi_Sample) a vložit data tooit vyberou dotazů SQL na služební cestě a tarif tabulky hello. (Některé kroky tohoto názorného postupu musí toouse tabulku.)

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

Hello zeměpisnou polohu účtů úložiště ovlivňuje časů načtení.

> [!NOTE]
> V závislosti na hello zeměpisné umístění účtu úložiště objektů blob privátní, hello proces kopírování dat z účtu privátní úložiště tooyour veřejného objektu blob může trvat přibližně 15 minut nebo i déle a hello procesu načítání dat z vašeho účtu úložiště tooyour Azure SQL DW může trvat 20 minut nebo déle.  
> 
> 

Budete mít toodecide co dělat, když máte duplicitním zdrojovým a cílovým souborů.

> [!NOTE]
> Pokud toobe soubory .csv hello zkopírovaných z účtu úložiště hello veřejného objektu blob úložiště tooyour privátní blob již existuje v účtu úložiště objektů blob privátní, AzCopy se zeptá, jestli chcete toooverwrite je. Pokud nechcete, aby toooverwrite je, vstupní  **n**  po zobrazení výzvy. Pokud chcete, aby toooverwrite **všechny** z nich, vstup **** po zobrazení výzvy. Můžete také zadat **y** toooverwrite .csv soubory jednotlivě.
> 
> 

![Vykreslení #21][21]

Můžete vytvořit svoje vlastní data. Pokud vaše data jsou v místním počítači ve vaší aplikaci reálného života, stále můžete služby AzCopy tooupload místní data tooyour privátní Azure blob storage. Potřebujete jenom toochange hello **zdroj** umístění, `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, v hello AzCopy příkaz hello prostředí PowerShell skriptu souboru toohello místní adresář, který obsahuje vaše data.

> [!TIP]
> Pokud vaše data je již ve službě privátní Azure blob storage ve vaší aplikaci reálného života, můžete přeskočit hello AzCopy krok v hello skript prostředí PowerShell a přímo nahrát hello data tooAzure SQL DW. To bude vyžadovat, že další upravuje z hello skriptu tootailor ho toohello formát data.
> 
> 

Tento skript prostředí Powershell také připojuje v hello Azure SQL DW informace do hello data zkoumání příklad souborů SQLDW_Explorations.sql, SQLDW_Explorations.ipynb a SQLDW_Explorations_Scripts.py tak, aby tyto tři soubory jsou připravené toobe se pokusila okamžitě Po dokončení hello skript prostředí PowerShell.

Po úspěšném spuštění, zobrazí se obrazovka jako níže:

![][20]

## <a name="dbexplore"></a>Zkoumání dat a funkce analýzy v Azure SQL Data Warehouse
V této části, můžeme provést zkoumání a funkce generování dat provádění dotazů SQL Azure SQL DW přímo pomocí **Data nástroje sady Visual Studio**. Všechny dotazy SQL použít v této části najdete v hello ukázkový skript s názvem *SQLDW_Explorations.sql*. Tento soubor je již stažené tooyour místního adresáře hello skript prostředí PowerShell. Můžete také načíst z [Githubu](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql). Ale hello souboru na Githubu, nemá informace o Azure SQL DW hello napájený ze sítě.

Připojení tooyour Azure SQL DW pomocí sady Visual Studio s hello SQL DW přihlašovací jméno a heslo a otevře hello **Průzkumník objektů systému SQL** tooconfirm hello databáze a tabulky byly importovány. Načtení hello *SQLDW_Explorations.sql* souboru.

> [!NOTE]
> tooopen editor dotazů Parallel Data Warehouse (PDW), použijte hello **nový dotaz** příkaz v hello se zapnutým vaše PDW **Průzkumník objektů systému SQL**. editor dotazů SQL standardní Hello nepodporuje PDW.
> 
> 

Tady jsou hello typ dat zkoumání a funkce generování úlohy provádějí v této části:

* Prozkoumejte data distribuce několik polí v různých časových oken.
* Prozkoumejte data quality hello zeměpisné šířky a délky polí.
* Generovat binární a více třídami klasifikační štítky podle hello **tip\_velikost**.
* Generovat funkce a výpočetní nebo porovnat cestě vzdálenosti.
* Připojení k hello dvou tabulek a extrahovat z náhodného vzorku, který bude použité toobuild modely.

### <a name="data-import-verification"></a>Ověření importu dat
Tyto dotazy poskytují rychlý ověření hello počtu řádků a sloupců v hello tabulky vyplněny dříve Polybase na paralelní hromadného importu,

    -- Report number of rows in table <nyctaxi_trip> without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')

    -- Report number of columns in table <nyctaxi_trip>
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = '<nyctaxi_trip>' AND table_schema = '<schemaname>'

**Výstup:** měli byste obdržet 173,179,759 řádků a sloupců 14.

### <a name="exploration-trip-distribution-by-medallion"></a>Zkoumání: Cestě distribuce podle Medailon
Tento příklad dotazu identifikuje medallions hello (taxi čísla), které byly dokončeny více než 100 služebních cest v rámci určeného časového období. Hello dotazu by využívat přístup k tabulce hello rozdělena na oddíly, vzhledem k tomu, že je podmíněno tím schéma oddílů hello **vyzvednutí\_data a času**. Dotazování hello úplnou datovou sadu se ujistěte se taky použít hello oddílů tabulky nebo indexu kontroly.

    SELECT medallion, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

**Výstup:** hello dotaz by měl vrátit tabulku s řádky zadání hello 13,369 medallions (taxislužby) a hello číslo cesty dokončit v 2013. poslední sloupec Hello obsahuje hello počet hello služebních cest byla dokončena.

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Zkoumání: Cestě distribuce podle Medailon a hack_license
Tento příklad identifikuje hello medallions (taxi čísla) a hack_license čísla (ovladače), dokončeno více než 100 služebních cest v rámci určeného časového období.

    SELECT medallion, hack_license, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

**Výstup:** hello dotazu by měla vrátit tabulku s 13,369 řádky zadání hello 13,369 car a ovladače ID, které mají více dokončit tento 100 služebních cest v 2013. poslední sloupec Hello obsahuje hello počet hello služebních cest byla dokončena.

### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Hodnocení kvality dat: ověřit záznamy s nesprávné délky a šířky
Tento příklad prověří, pokud jakýkoli hello zeměpisné délky a šířky polí buď obsahuje neplatnou hodnotu (radián stupňů musí být mezi -90 a 90), nebo (0, 0) souřadnice.

    SELECT COUNT(*) FROM <schemaname>.<nyctaxi_trip>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

**Výstup:** hello dotaz vrátí 837,467 služebních cest, které mají neplatná pole zeměpisné délky a šířky.

### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Zkoumání: Vysypávány oproti distribuční není šikmý služebních cest
Tento příklad vyhledá hello počet cest, které byly vysypávány oproti hello číslo, které nebyly vysypávány v zadaném časovém období (nebo v hello úplnou datovou sadu Pokud pokrývajících hello celý rok, jako je zde nastavený). Toto rozdělení odráží toobe distribuční binární popisek hello později použita k modelování binární klasifikace.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM <schemaname>.<nyctaxi_fare>
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

**Výstup:** hello dotaz by měl následující návratové hello tip pro hello roku 2013: 90,447,622 vysypávány a 82,264,709 vysypávány není frekvencí.

### <a name="exploration-tip-classrange-distribution"></a>Zkoumání: Distribuce třídy nebo rozsah Tip
Tento příklad vypočítá distribuci hello rozsahy tip v daném časovém období (nebo hello úplnou datovou sadu Pokud pokrývající celý rok hello). Toto je hello distribuční hello popisek tříd, které se použijí později pro modelování více třídami klasifikace.

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

**Výstup:**

| tip_class | tip_freq |
| --- | --- |
| 1 |82230915 |
| 2 |6198803 |
| 3 |1932223 |
| 0 |82264625 |
| 4 |85765 |

### <a name="exploration-compute-and-compare-trip-distance"></a>Zkoumání: Výpočetní a porovnání vzdálenost cesty
Tento příklad převede hello vyzvednutí a odkládací zeměpisné délky a šířky tooSQL geography body, vypočítá vzdálenost cestě hello pomocí SQL geography body rozdíl a vrátí z náhodného vzorku hello výsledky pro porovnání. Příklad Hello omezí výsledky hello toovalid koordinuje jenom pomocí hello kvality assessment dotaz na data popsané výše.

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

### <a name="feature-engineering-using-sql-functions"></a>Funkce inženýrství pomocí funkce SQL
Funkce SQL v některých případech může být efektivní možnost pro funkci inženýrství. V tomto návodu jsme definovali SQL funkce toocalculate hello přímé vzdálenosti mezi hello vyzvednutí a dropoff umístění. Můžete spustit následující skripty SQL v hello **Data nástroje sady Visual Studio**.

Zde je hello skript SQL, který definuje funkci vzdálenost hello.

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

Tady je toocall příklad této funkce toogenerate funkce v dotazu SQL:

    -- Sample query toocall hello function toocreate features
    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

**Výstup:** tento dotaz vygeneruje tabulku (s 2,803,538 řádky) s vyzvednutí a dropoff zeměpisné šířky a stupně zeměpisné délky a odpovídající hello přímé vzdálenosti v paliva. Zde jsou hello výsledky pro první 3 řádky:

|  | pickup_latitude | pickup_longitude | dropoff_latitude | dropoff_longitude | DirectDistance |
| --- | --- | --- | --- | --- | --- |
| 1 |40.731804 |-74.001083 |40.736622 |-73.988953 |.7169601222 |
| 2 |40.715794 |-74,010635 |40.725338 |-74.00399 |.7448343721 |
| 3 |40.761456 |-73.999886 |40.766544 |-73.988228 |0.7037227967 |

### <a name="prepare-data-for-model-building"></a>Příprava dat pro vytváření modelů
Hello následující dotaz spojení hello **nyctaxi\_cestě** a **nyctaxi\_tarif** tabulky, vygeneruje štítek binární klasifikace **vysypávány**, Popisek více třída klasifikace **tip\_třída**a extrahuje ukázku z hello úplné připojené k datové sadě. je potřeba Hello vzorkování načítání podmnožinu služebních cest hello na základě výstupní času.  Tento dotaz můžete zkopírovat a vložit přímo v hello [Azure Machine Learning Studio](https://studio.azureml.net) [importovat Data] [ import-data] modul pro přijímání přímé dat z instance databáze SQL hello v Azure. dotaz Hello vyloučí záznamy s nesprávnou (0, 0) souřadnice.

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

Pokud jste připravené tooproceed tooAzure Machine Learning, můžete se buď:  

1. Uložit hello konečné SQL dotaz tooextract a ukázkové hello dat a způsobené kopírováním a vkládáním hello dotaz přímo do [importovat Data] [ import-data] modulu v Azure Machine Learning, nebo
2. Zachovat hello vzorků a máte v plánu toouse pro model sestavení v nových SQL DW inženýrství dat tabulky a používání hello novou tabulku v hello [importovat Data] [ import-data] modulu v Azure Machine Learning. Můžete to bylo dokončeno Hello skript prostředí PowerShell v předchozím kroku. Si můžete přečíst přímo z této tabulky v modulu hello importovat Data.

## <a name="ipnb"></a>Zkoumání dat a funkce technikům v poznámkovém bloku IPython
V této části provedeme zkoumání dat a funkce generování pomocí obou Python a dotazy SQL pro hello SQL DW vytvořili dříve. Ukázka IPython Poznámkový blok s názvem **SQLDW_Explorations.ipynb** a soubor skriptu jazyka Python **SQLDW_Explorations_Scripts.py** byly stažené tooyour místního adresáře. Jsou k dispozici na [Githubu](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW). Tyto dva soubory jsou identické v skriptů Python. soubor skriptu jazyka Python Hello je k dispozici tooyou v případě, že jste k serveru IPython Poznámkový blok. Tyto dva ukázkové soubory jsou navrženy v části Python **Python 2.7**.

Hello potřebné informace Azure SQL DW v ukázce hello IPython Poznámkový blok a hello Python skriptu soubor stažený tooyour místního počítače je zapojen skript prostředí PowerShell hello dříve. Jsou spustitelné bez nutnosti jakékoli úpravy.

Pokud již jste vytvořili pracovní prostor služby AzureML, můžete přímo nahrát hello Ukázka služby poznámkového bloku IPython AzureML toohello IPython Poznámkový blok a spustit ho. Zde jsou hello kroky tooupload tooAzureML služby IPython Poznámkový blok:

1. Přihlášení tooyour AzureML prostoru, klikněte v horní části hello "Studio" a klikněte na "Notebooky" na levé straně hello hello webové stránky.
   
    ![Vykreslení #22][22]
2. Klikněte v levém dolním rohu hello hello webové stránky "NEW" a vyberte "Python 2". Potom zadejte název toohello Poznámkový blok a klikněte na hello zaškrtnutí toocreate hello novou prázdnou IPython Poznámkový blok.
   
    ![Vykreslení #23][23]
3. Kliknutím na symbol "Jupyter" hello na hello levého horního rohu hello nový poznámkový blok IPython.
   
    ![Vykreslení #24][24]
4. Přetáhnout myší hello ukázka poznámkového bloku IPython toohello **stromu** služby AzureML IPython Poznámkový blok a klikněte na tlačítko **nahrát**. Potom bude hello ukázkový poznámkový blok IPython nahrané toohello služby AzureML IPython poznámkového bloku.
   
    ![Vykreslení #25][25]

V pořadí toorun hello ukázkové IPython Poznámkový blok nebo hello soubor skriptu jazyka Python, hello následující balíčky Python, které jsou potřeba. Pokud používáte služby hello AzureML IPython Poznámkový blok, tyto balíčky byly předem nainstalovány.

    - pandas
    - numpy
    - matplotlib
    - pyodbc
    - PyTables

Hello doporučuje pořadí při sestavování Pokročilá analytická řešení na AzureML s velkých objemů dat je hello následující:

* Přečtěte si v malé ukázkové hello dat do data v paměti rámečku.
* Provést některé vizualizace a explorations pomocí hello jen Vzorkovaná data.
* Experimentů pomocí funkce inženýrství pomocí hello Vzorkovat data.
* Pro větší zkoumání dat, manipulaci s daty a funkce technikům použijte dotazy SQL tooissue Python přímo pro hello SQL DW.
* Rozhodněte, hello ukázka velikost toobe vhodný pro vytváření modelů Azure Machine Learning.

Hello tady jsou několik zkoumání dat, vizualizace dat a funkce engineering příklady. Další data explorations naleznete v hello ukázka IPython Poznámkový blok a souboru skriptu jazyka Python ukázka hello.

### <a name="initialize-database-credentials"></a>Inicializace přihlašovací údaje databáze
Inicializace nastavení připojení k databázi v hello následující proměnné:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database driver>

### <a name="create-database-connection"></a>Vytvoření připojení k databázi
Zde je hello připojovací řetězec, který vytvoří databázi toohello hello připojení.

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Sestava počtu řádků a sloupců v tabulce < nyctaxi_trip >
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

* Celkový počet řádků = 173179759  
* Celkový počet sloupců = 14

### <a name="report-number-of-rows-and-columns-in-table-nyctaxifare"></a>Sestava počtu řádků a sloupců v tabulce < nyctaxi_fare >
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

* Celkový počet řádků = 173179759  
* Celkový počet sloupců = 11

### <a name="read-in-a-small-data-sample-from-hello-sql-data-warehouse-database"></a>Pro čtení ve vzorku malá data z hello databáze serveru SQL datového skladu
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

Čas tooread hello ukázková tabulka je 14.096495 sekund.  
Počet řádků a sloupců načíst = (1000, 21).

### <a name="descriptive-statistics"></a>Popisný statistiky
Teď je připraven tooexplore hello Vzorkovat data. Začneme s prohlížení statistikami popisný pro hello **cestě\_vzdálenost** (nebo další pole vyberte toospecify).

    df1['trip_distance'].describe()

### <a name="visualization-box-plot-example"></a>Vizualizace: Příklad vykreslení pole
Další podíváme na hello Krabicový graf pro hello cestě vzdálenost toovisualize hello quantiles.

    df1.boxplot(column='trip_distance',return_type='dict')

![Vykreslení #1][1]

### <a name="visualization-distribution-plot-example"></a>Vizualizaci: Příklad vykreslení distribuční
Pozemků, které vizualizovat hello distribuce a histogram hello vzorkovat cestě vzdálenosti.

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Vykreslení #2][2]

### <a name="visualization-bar-and-line-plots"></a>Vizualizace: Panel a ukazuje zeměpisný řádku
V tomto příkladu jsme bin hello vzdálenost cesty do pěti přihrádek a vizualizovat hello přihrádkování výsledky.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Jsme můžete vykreslení hello výše bin distribuční panelu nebo výkresu s řádek:

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Vykreslení #3][3]

a

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Vykreslení #4][4]

### <a name="visualization-scatterplot-examples"></a>Vizualizaci: Příklady Scatterplot
Ukážeme bodové vykreslení mezi **cestě\_čas\_v\_sekundy** a **cestě\_vzdálenost** toosee, pokud neexistuje žádné korelace

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Vykreslení #6][6]

Podobně jsme můžete zkontrolovat hello vztah mezi **míra\_kód** a **cestě\_vzdálenost**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![Vykreslení #8][8]

### <a name="data-exploration-on-sampled-data-using-sql-queries-in-ipython-notebook"></a>Zkoumání dat na jen Vzorkovaná data pomocí dotazů SQL v poznámkovém bloku IPython
V této části nám prozkoumat distribuce dat pomocí hello vzorků dat, který je uchován v hello nové tabulky, kterou jsme vytvořili výše. Všimněte si, že podobné explorations je možné provést pomocí hello původní tabulky.

#### <a name="exploration-report-number-of-rows-and-columns-in-hello-sampled-table"></a>Zkoumání: Sestava počtu řádků a sloupců v hello vzorků tabulky
    nrows = pd.read_sql('''SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_sample>')''', conn)
    print 'Number of rows in sample = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''SELECT count(*) FROM information_schema.columns WHERE table_name = ('<nyctaxi_sample>') AND table_schema = '<schemaname>'''', conn)
    print 'Number of columns in sample = %d' % ncols.iloc[0,0]

#### <a name="exploration-tippednot-tripped-distribution"></a>Zkoumání: Vysypávány nebo nebyla přerušovačů distribuce
    query = '''
        SELECT tipped, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tipped
        '''

    pd.read_sql(query, conn)

#### <a name="exploration-tip-class-distribution"></a>Zkoumání: Distribuce třída Tip
    query = '''
        SELECT tip_class, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tip_class
    '''

    tip_class_dist = pd.read_sql(query, conn)

#### <a name="exploration-plot-hello-tip-distribution-by-class"></a>Zkoumání: Vykreslení hello tip distribuční třídou
    tip_class_dist['tip_freq'].plot(kind='bar')

![Vykreslení #26][26]

#### <a name="exploration-daily-distribution-of-trips"></a>Zkoumání: Denní distribuce služebních cest
    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Zkoumání: Distribuční cesty za Medailon
    query = '''
        SELECT medallion,count(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>Zkoumání: Cestě distribuce podle Medailon a hackerský licencí
    query = '''select medallion, hack_license,count(*) from <schemaname>.<nyctaxi_sample> group by medallion, hack_license'''
    pd.read_sql(query,conn)


#### <a name="exploration-trip-time-distribution"></a>Zkoumání: Distribuce doby cestě
    query = '''select trip_time_in_secs, count(*) from <schemaname>.<nyctaxi_sample> group by trip_time_in_secs order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-trip-distance-distribution"></a>Zkoumání: Distribuce vzdálenost cestě
    query = '''select floor(trip_distance/5)*5 as tripbin, count(*) from <schemaname>.<nyctaxi_sample> group by floor(trip_distance/5)*5 order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-payment-type-distribution"></a>Zkoumání: Distribuce typ platby
    query = '''select payment_type,count(*) from <schemaname>.<nyctaxi_sample> group by payment_type'''
    pd.read_sql(query,conn)

#### <a name="verify-hello-final-form-of-hello-featurized-table"></a>Ověření formuláře konečné hello hello featurized tabulky
    query = '''SELECT TOP 100 * FROM <schemaname>.<nyctaxi_sample>'''
    pd.read_sql(query,conn)

## <a name="mlmodel"></a>Vytvářet modely v Azure Machine Learning
Snažíme se teď připravena tooproceed toomodel sestavení a nasazení modelu v [Azure Machine Learning](https://studio.azureml.net). Hello data jsou připravena toobe použít v některé z problémů předpovědi hello identifikuje dříve, konkrétně:

1. **Binární klasifikace**: toopredict, jestli byl tip placené cesty.
2. **Více třídami klasifikace**: rozsah hello toopredict tipu placené, podle toohello dříve definovaných tříd.
3. **Úloha regrese**: toopredict hello množství tip placené cesty.  

toobegin hello modelování cvičení, přihlaste se tooyour **Azure Machine Learning** pracovního prostoru. Pokud jste ještě nevytvořili machine learning pracovního prostoru, přečtěte si téma [vytvořit pracovní prostor služby Azure ML](machine-learning-create-workspace.md).

1. tooget začít s Azure Machine Learning, najdete v části [co je Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)
2. Přihlaste se příliš[Azure Machine Learning Studio](https://studio.azureml.net).
3. Hello Studio Domovská stránka obsahuje širokou řadu informací, videa, kurzy, odkazy toohello odkaz moduly a dalším prostředkům. Další informace o Azure Machine Learning najdete hello [centru dokumentace Azure Machine Learning na](https://azure.microsoft.com/documentation/services/machine-learning/).

Typické výukový experiment sestává z hello následující kroky:

1. Vytvoření **+ nový** experiment.
2. Získáte hello data do Azure ML.
3. Předběžně zpracovat, transformace a pracovat s daty hello podle potřeby.
4. Funkce vygenerujte, podle potřeby.
5. Rozdělení dat hello na školení, ověření nebo testování datové sady (nebo samostatné datové sady pro každou).
6. Vyberte jeden nebo více algoritmů strojového učení v závislosti na hello učení toosolve problém. Například: binární klasifikace, klasifikace více třídami, regresi.
7. Cvičení jednoho nebo více modelů použití hello školení datové sady.
8. Stanovení skóre hello ověření datové sady pomocí vyškolení modely hello.
9. Vyhodnoťte hello modely toocompute hello relevantní metriky pro hello učení problém.
10. Vyladění hello modely a vyberte hello nejlepší toodeploy modelu.

V tomto cvičení jsme již prozkoumali a analýzou dat hello v SQL Data Warehouse a rozhodli tooingest velikost vzorku hello v Azure ML. Tady je postup toobuild hello jeden nebo více modelů předpovědi hello:

1. Získat hello data do Azure ML pomocí hello [importovat Data] [ import-data] modulu, k dispozici v hello **vstupu a výstupu dat** části. Další informace najdete v tématu hello [importovat Data] [ import-data] stránka s referencemi modul.
   
    ![Azure ML Import dat][17]
2. Vyberte **Azure SQL Database** jako hello **zdroj dat** v hello **vlastnosti** panelu.
3. Zadejte název DNS hello databáze v hello **název databázového serveru** pole. Formát:`tcp:<your_virtual_machine_DNS_name>,1433`
4. Zadejte hello **název databáze** v odpovídající pole hello.
5. Zadejte hello *uživatelské jméno SQL* v hello **název uživatelského účtu serveru**a hello *heslo* v hello **heslo uživatelského účtu serveru**.
6. Zkontrolujte hello **přijmout některý z certifikátů serveru** možnost.
7. V hello **databázový dotaz** upravit textová oblast, vložte hello dotazu, který extrahuje hello potřeby databáze pole (včetně všech počítané pole, jako je popisky hello) a dolů – ukázky velikost vzorku toohello požadovaných dat hello.

Je například binární klasifikace experimentů, čtení dat přímo z databáze SQL Data Warehouse hello hello obrázek (mějte na paměti, tooreplace hello názvy tabulek nyctaxi_trip a nyctaxi_fare ve schématu hello název a hello názvy tabulek, kterou jste použili v vaší návod). Podobně jako experimenty konstruovat pro více třídami klasifikaci a regresní problémy.

![Azure ML Train][10]

> [!IMPORTANT]
> V hello modelování extrakce dat a vzorkuje příklady dotazu uvedené v předchozí části **všech popisků hello tři modelování cvičení jsou zahrnuty v dotazu hello**. Důležitým krokem (povinné) v každé z hello modelování cvičení je příliš**vyloučit** hello nepotřebné štítky pro hello další dva problémy a všemi ostatními **cíle nevracení**. Například při použití binární klasifikace, použít hello popisek **vysypávány** a vyloučit hello pole **tip\_– třída**, **tip\_velikost**, a **celkový\_velikost**. Hello druhé jsou placené cíl nevracení vzhledem k tomu, že implikují hello tip.
> 
> tooexclude všechny nepotřebné sloupce nebo nevracení cíl, můžete použít hello [výběr sloupců v datové sadě] [ select-columns] modulu nebo hello [upravit Metadata] [ edit-metadata]. Další informace najdete v tématu [výběr sloupců v datové sadě] [ select-columns] a [upravit Metadata] [ edit-metadata] referenční stránky.
> 
> 

## <a name="mldeploy"></a>Nasazení modely v Azure Machine Learning
Pokud váš model je připraven, můžete snadno nasadit ho jako webovou službu přímo z hello experimentu. Další informace o nasazení webové služby Azure ML najdete v tématu [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).

toodeploy novou webovou službu, musíte:

1. Vytvoření experimentu vyhodnocování.
2. Nasaďte hello webové služby.

toocreate vyhodnocování experimentovat z **dokončeno** cvičení experiment, klikněte na tlačítko **vytvořit vyhodnocování EXPERIMENTOVAT** akce panelu nižší hello.

![Výpočet skóre Azure][18]

Azure Machine Learning se pokusí toocreate vyhodnocování experimentu podle hello součástí hello výukový experiment. Konkrétně se:

1. Uložit hello trained model a odeberte hello modelu školení moduly.
2. Identifikovat logickou **vstupní port** toorepresent hello očekávána vstupní data schématu.
3. Identifikovat logickou **výstupní port** toorepresent hello očekávané webové služby výstupního schématu.

Když je vytvořen hello vyhodnocování experiment, zkontrolujte ji a proveďte podle potřeby upravte. Typické úpravu je tooreplace hello vstupní datové sady nebo dotaz s jedním, který vyloučí popisek pole, protože tyto nebudete mít k dispozici, když je volána hello služby. Je také že vhodné tooreduce hello velikost hello vstupní datové sady nebo tooa několik záznamů, akorát tooindicate hello vstupní schéma. Pro hello výstupní port, je běžné tooexclude všechny vstupní pole a obsahovat jenom hello **popisky vyhodnocení** a **skóre pro Magnitudu Pravděpodobnostech** v hello výstup pomocí hello [výběr sloupců v datové sadě ] [ select-columns] modulu.

Ukázka vyhodnocování experimentu je součástí hello obrázek. Jakmile budete připraveni toodeploy, klikněte na tlačítko hello **publikování webové služby** tlačítka na dolním panelu akcí hello.

![Publikování Azure ML][11]

## <a name="summary"></a>Souhrn
toorecap jste vytvořili prostředí vědecké účely dat Azure, práce s velké veřejné datové sady, trvá prostřednictvím hello proces vědecké účely Team dat, všechny způsob hello z školení toomodel pořízení data, co jsme provedli v tomto kurzu návod a potom toohello nasazení webové služby Azure Machine Learning.

### <a name="license-information"></a>Informace o licenci
Tento návod ukázka a jeho doplňujícími skripty a IPython notebook(s) sdílí Microsoft v rámci licencí MIT hello. Zkontrolujte prosím soubor LICENSE.txt hello v adresáři hello hello ukázkový kód na Githubu další podrobnosti.

## <a name="references"></a>Odkazy
• [Stránce pro stažení Andrés Monroy NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing NYC Taxi cestě dat podle Whong Jan](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC taxíkem a Limousine Komise výzkum a statistiky](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)

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
