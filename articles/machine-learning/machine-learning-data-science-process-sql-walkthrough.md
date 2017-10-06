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
# <a name="hello-team-data-science-process-in-action-using-sql-server"></a>Hello proces vědecké účely Team dat v akci: pomocí SQL serveru
V tomto kurzu jste provede hello proces vytváření a nasazování model strojového učení pomocí systému SQL Server a veřejně dostupné datové sady – hello [NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/) datovou sadu. Postup Hello dodržovat standardní data vědecké účely pracovního postupu: ingestování a zkoumat hello data, analyzovat funkce toofacilitate učení se pak sestavení a nasazení modelu.

## <a name="dataset"></a>NYC taxíkem cest v popis datové sady
Hello NYC taxíkem cestě dat je přibližně 20GB komprimovaných souborů CSV (nekomprimovaným ~ 48GB), která obsahuje více než 173 milionů jednotlivých cest a hello tarify placené pro každou cestu. Každý záznam cestě zahrnuje hello vyzvednutí a odkládací umístění a čas, číslo licence anonymizovaná hackerský (ovladač) a číslo Medailon (taxi na jedinečné id). Hello dat obsahuje všechny služebních cest v hello roku 2013 a je součástí hello následující dvě datové sady pro každý měsíc:

1. Hello 'trip_data' CSV obsahuje podrobnosti o cestě, například na počtu cestujících, vyzvednutí a dropoff body, doba trvání cesty a délka cesty. Tady je několik ukázkových záznamů:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. Hello CSV obsahuje podrobnosti o tarif hello placené pro každou cestu, například typ platby, velikost tarif, příplatek a daně, tipy a mýtné a celkovou velikost hello placené trip_fare. Tady je několik ukázkových záznamů:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Hello jedinečné klíče toojoin cestě\_dat a cesty\_tarif se skládá z pole hello: medailonu, hackerský\_licence a vyzvednutí\_data a času.

## <a name="mltasks"></a>Příklady předpovědi úlohy
Jsme se formulovali tři předpovědi problémy podle hello *tip\_velikost*, konkrétně:

1. Binární klasifikace: předpovědět, zda byl tip placené cesty, tj. *tip\_velikost* větší než $0 je pozitivní příklad, při *tip\_velikost* $ 0 je Příklad záporné.
2. Více třídami klasifikace: rozsah hello toopredict tipu zaplacení hello cesty. Jsme rozdělit hello *tip\_velikost* do pěti přihrádek nebo třídy:
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. Úloha regrese: toopredict hello množství tip placené cesty.  

## <a name="setup"></a>Nastavení se hello dat Azure vědecké účely prostředí pro pokročilou analýzu
Jak vidíte z hello [plánování vašeho prostředí](machine-learning-data-science-plan-your-environment.md) průvodce, existuje několik možností toowork s datovou sadu cest taxíkem NYC hello v Azure:

* Práce s hello data do objektů BLOB služby Azure a modelu v Azure Machine Learning
* Načtení dat hello do databáze systému SQL Server a modelu v Azure Machine Learning

V tomto kurzu si předvedeme paralelní hromadným importem tooa hello dat systému SQL Server, zkoumání dat, funkce technici a dolů vzorkování pomocí SQL Server Management Studio a také pomocí IPython Poznámkový blok. [Ukázkové skripty](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) a [poznámkových bloků IPython](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) jsou sdíleny v Githubu. Ukázkové IPython poznámkového bloku toowork s daty hello v Azure blob je také dostupná v hello stejné umístění.

tooset prostředí vědecké zpracování dat Azure:

1. [Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md)
2. [Vytvořit pracovní prostor služby Azure Machine Learning](machine-learning-create-workspace.md)
3. [Zřízení virtuálního počítače s datové vědy](machine-learning-data-science-setup-sql-server-virtual-machine.md), který poskytuje systému SQL Server a server služby IPython Poznámkový blok.
   
   > [!NOTE]
   > Hello ukázkové skripty a IPython poznámkových bloků se stažené tooyour vědecké zpracování dat virtuálního počítače během procesu instalace hello. Po dokončení hello virtuálního počítače po instalaci skriptu hello ukázky bude v knihovně dokumentů Virtuálního počítače:  
   > 
   > * Ukázku skripty:`C:\Users\<user_name>\Documents\Data Science Scripts`  
   > * Ukázka IPython poznámkových bloků:`C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`  
   >   kde `<user_name>` je váš virtuální počítač Windows přihlašovací jméno. Označujeme toohello ukázkové složky jako **ukázkové skripty** a **poznámkových bloků IPython ukázka**.
   > 
   > 

Na základě hello velikost datové sady, umístění zdroje dat a hello vybrané Azure cílové prostředí, tento scénář je podobné příliš[scénář \#5: velké datové sady v místních souborů cíle systému SQL Server ve virtuálním počítači Azure](machine-learning-data-science-plan-sample-scenarios.md#largelocaltodb).

## <a name="getdata"></a>Získat hello Data z veřejné zdroje
tooget hello [NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/) datové sady z veřejného umístění, můžete použít některou z metod hello popsané v [tooand přesun dat z Azure Blob Storage](machine-learning-data-science-move-azure-blob.md) toocopy hello data tooyour nového virtuálního počítače.

pomocí AzCopy data hello toocopy:

1. Přihlaste se tooyour virtuální počítač (VM)
2. Vytvořte nový adresář v datový disk hello Virtuálního počítače (Poznámka: Nepoužívejte hello dočasné Disk, který se dodává s hello virtuální počítač jako datový Disk).
3. V okně příkazového řádku spusťte hello následující příkazového řádku Azcopy, nahraďte < path_to_data_folder > vaše data složky vytvořené v (2):
   
        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S
   
    Po dokončení hello AzCopy CSV soubory ZIP celkem 24 (12 pro cestu\_dat a 12 pro cestu\_tarif) by měl být ve složce data hello.
4. Rozbalte hello stažené soubory. Všimněte si hello složku, kde jsou uloženy soubory hello nekomprimovaným. Tato složka bude odkazované tooas hello < cesta\_k\_data\_soubory\>.

## <a name="dbload"></a>Hromadný Import dat do databáze serveru SQL
Hello výkon načítání nebo přenos velkých objemů dat tooan SQL database a následné dotazy lze zvýšit pomocí *rozdělena na oddíly tabulky a zobrazení*. V této části jsme postupujte podle pokynů hello v tématu [paralelní hromadný Import pomocí SQL oddílu tabulky dat](machine-learning-data-science-parallel-load-sql-partitioned-tables.md) toocreate nová data hello databáze a zatížení do dělené tabulky paralelně.

1. Během přihlášení tooyour virtuálního počítače, spusťte **SQL Server Management Studio**.
2. Připojte pomocí ověřování systému Windows.
   
    ![Připojení přes SSMS][12]
3. Pokud máte ještě změnit režim ověřování systému SQL Server hello a vytvořit nového uživatele SQL přihlášení, otevřete soubor skriptu hello s názvem **změnit\_auth.sql** v hello **ukázkové skripty** složky. Změňte hello výchozí uživatelské jméno a heslo. Klikněte na tlačítko **! Spuštění** ve hello nástrojů toorun hello skriptu.
   
    ![Spuštění skriptu][13]
4. Ověřte nebo změňte hello výchozí databáze a protokolu složky tooensure nově vytvořené databáze bude uložen v datový Disk systému SQL Server. bitovou kopii Hello virtuální počítač SQL Server, která je optimalizovaná pro datawarehousing zatížení je předem nakonfigurovaný s disky protokolu a data. Pokud virtuální počítač neobsahuje datový Disk a přidat nové virtuální pevné disky během hello procesu instalace virtuálních počítačů, změňte výchozí složky hello takto:
   
   * Klikněte pravým tlačítkem na název serveru SQL Server hello v hello zbývajících panelu a klikněte na tlačítko **vlastnosti**.
     
       ![Vlastnosti serveru SQL][14]
   * Vyberte **nastavení databáze** z hello **vybrat stránku** seznamu toohello doleva.
   * Ověřte nebo změňte hello **databáze výchozí umístění** toohello **datový Disk** umístění podle vaší volby. Toto je, kde jsou umístěny nové databáze, pokud vytvořen s hello výchozí umístění nastavení.
     
       ![Výchozí nastavení databáze SQL][15]  
5. toocreate novou databázi a sadu skupin souborů toohold hello dělené tabulky, otevřete hello ukázkový skript **vytvořit\_db\_default.sql**. skript Hello vytvoří novou databázi s názvem **TaxiNYC** a 12 skupiny souborů v umístění dat výchozí hello. Každou skupinu souborů bude obsahovat jeden měsíc cesty\_dat a cesty\_jízdenky data. Upravte hello název databáze, v případě potřeby. Klikněte na tlačítko **! Spuštění** toorun hello skriptu.
6. Dále vytvořte dvě tabulky oddílů, jeden pro cestu hello\_dat a druhý pro cestu hello\_tarif. Otevřete hello ukázkový skript **vytvořit\_oddílů\_table.sql**, který bude:
   
   * Vytvoření oddílu funkce toosplit hello dat po měsících.
   * Vytvořte toomap schéma oddílu každý měsíc dat tooa různých souborů.
   * Vytvořte dva schéma oddílu namapované toohello dělené tabulky: **nyctaxi\_cestě** bude obsahovat hello cestě\_dat a **nyctaxi\_tarif** bude obsahovat hello cesty \_jízdenky data.
     
     Klikněte na tlačítko **! Spuštění** toorun hello skriptu a vytvářet tabulky hello rozdělena na oddíly.
7. V hello **ukázkové skripty** složky, existují dva ukázkové skripty prostředí PowerShell zadat toodemonstrate paralelní hromadné importy dat tooSQL serveru tabulek.
   
   * **BCP\_paralelní\_generic.ps1** je obecný skript tooparallel hromadného importu dat do tabulky. Umožňuje změnit tento skript tooset hello vstup a cíle proměnné, které je uvedené v řádcích hello komentáře ve skriptu hello.
   * **BCP\_paralelní\_nyctaxi.ps1** je předem nakonfigurovaný verze hello obecný skript a může být použité tootooload obě tabulky pro data NYC taxíkem cest hello.  
8. Klikněte pravým tlačítkem na hello **bcp\_paralelní\_nyctaxi.ps1** název skriptu a klikněte na tlačítko **upravit** tooopen v prostředí PowerShell. Zkontrolujte hello předvolby proměnné a úpravám podle názvu tooyour vybrané databáze, složka vstupních dat, cílové složce protokolu a cesty toohello ukázkových formát souborů **nyctaxi_trip.xml** a **nyctaxi\_ fare.xml** (součástí hello **ukázkové skripty** složku).
   
    ![Hromadný Import dat][16]
   
    Můžete také vybrat režim ověřování hello, výchozí hodnota je ověřování systému Windows. Klikněte na tlačítko hello zelenou šipku v panelu nástrojů toorun hello. Hello skript se spustí 24 operace hromadného importu v paralelní, 12 pro každou tabulku oddílů. Probíhá import dat hello může sledovat otevřením složky dat výchozí systému SQL Server hello výše.
9. sestavy skript prostředí PowerShell Hello Dobrý den, počáteční a koncový čas. Pokud všechny hromadně importy dokončení, uvede se hello čas dokončení. Žádné chyby hlášené hello do cílové složky protokolů zkontrolujte hello cíl protokolu složky tooverify, který hello hromadný import byl úspěšný, tj.
10. Vaše databáze je nyní připraven pro zkoumání, technici funkce a další operace podle potřeby. Vzhledem k tomu, že hello tabulky jsou rozděleny do oddílů podle toohello **vyzvednutí\_data a času** pole, dotazy, které zahrnují **vyzvednutí\_data a času** podmínky v hello  **KDE** klauzule výhod schéma oddílu hello.
11. V **SQL Server Management Studio**, prozkoumejte hello poskytuje ukázkový skript **ukázka\_queries.sql**. toorun žádné hello ukázkové dotazy, zvýraznění hello dotaz na řádky a pak klikněte na **! Spuštění** v panelu nástrojů hello.
12. Hello NYC taxíkem cest dat je načten do dvou samostatných tabulkách. operace spojení tooimprove, důrazně doporučujeme tooindex hello tabulky. Hello ukázkový skript **vytvořit\_oddílů\_index.sql** vytváří indexy oddílů hello spojení složený klíč **medailonu, hackerský\_licence a vyzvednutí\_data a času**.

## <a name="dbexplore"></a>Zkoumání dat a konstruování funkce v systému SQL Server
V této části, provedeme zkoumání a funkce generování dat spuštěním dotazů SQL přímo v hello **SQL Server Management Studio** použití databáze systému SQL Server hello vytvořili dříve. Ukázkový skript s názvem **ukázka\_queries.sql** je součástí hello **ukázkové skripty** složky. Upravit název hello skriptu toochange hello databáze, pokud se liší od výchozí hello: **TaxiNYC**.

V tomto cvičení provedeme následující:

* Připojit příliš**SQL Server Management Studio** pomocí buď ověřování systému Windows nebo pomocí ověřování SQL a hello SQL přihlašovací jméno a heslo.
* Prozkoumejte data distribuce několik polí v různých časových oken.
* Prozkoumejte data quality hello zeměpisné šířky a délky polí.
* Generovat binární a více třídami klasifikační štítky podle hello **tip\_velikost**.
* Generovat funkce a výpočetní nebo porovnat cestě vzdálenosti.
* Připojení k hello dvou tabulek a extrahovat z náhodného vzorku, který bude použité toobuild modely.

Pokud jste připravené tooproceed tooAzure Machine Learning, můžete se buď:  

1. Uložit hello konečné SQL dotaz tooextract a ukázkové hello dat a způsobené kopírováním a vkládáním hello dotaz přímo do [importovat Data] [ import-data] modulu v Azure Machine Learning, nebo
2. Zachovat hello vzorků a inženýrství dat, které máte v plánu toouse pro model sestavování v nové databáze tabulky a používání hello novou tabulku v hello [importovat Data] [ import-data] modulu v Azure Machine Learning.

V této části jsme se hello poslední dotaz tooextract a ukázkové hello data uložit. druhý metoda Hello je znázorněn v hello [zkoumání dat a funkce Engineering v poznámkovém bloku IPython](#ipnb) části.

Pro rychlou kontrolu hello počtu řádků a sloupců v hello tabulky vyplněny dříve paralelní hromadného importu

    -- Report number of rows in table nyctaxi_trip without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('nyctaxi_trip')

    -- Report number of columns in table nyctaxi_trip
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = 'nyctaxi_trip'

#### <a name="exploration-trip-distribution-by-medallion"></a>Zkoumání: Cestě distribuce podle Medailon
Tento příklad identifikuje Medailon hello (taxi čísla) s více než 100 služebních cest v daném časovém období. Hello dotazu by využívat přístup k tabulce hello rozdělena na oddíly, vzhledem k tomu, že je podmíněno tím schéma oddílů hello **vyzvednutí\_data a času**. Dotazování hello úplnou datovou sadu se ujistěte se taky použít hello oddílů tabulky nebo indexu kontroly.

    SELECT medallion, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

#### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Zkoumání: Cestě distribuce podle Medailon a hack_license
    SELECT medallion, hack_license, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

#### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Hodnocení kvality dat: Ověřte záznamy s nesprávné délky a šířky
Tento příklad prověří, pokud jakýkoli hello zeměpisné délky a šířky polí buď obsahuje neplatnou hodnotu (radián stupňů musí být mezi -90 a 90), nebo (0, 0) souřadnice.

    SELECT COUNT(*) FROM nyctaxi_trip
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

#### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Zkoumání: Šikmý vs. Není vysypávány služebních cest distribučního
Tento příklad vyhledá hello počet cest, které byly vysypávány oproti není vysypávány v daném časovém období (nebo hello úplnou datovou sadu Pokud pokrývající celý rok hello). Toto rozdělení odráží toobe distribuční binární popisek hello později použita k modelování binární klasifikace.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM nyctaxi_fare
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

#### <a name="exploration-tip-classrange-distribution"></a>Zkoumání: Třída nebo rozsah distribuční Tip
Tento příklad vypočítá distribuci hello rozsahy tip v daném časovém období (nebo hello úplnou datovou sadu Pokud pokrývající celý rok hello). Toto je hello distribuční hello popisek tříd, které se použijí později pro modelování více třídami klasifikace.

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

#### <a name="exploration-compute-and-compare-trip-distance"></a>Zkoumání: Výpočetní a porovnání vzdálenost cesty
Tento příklad převede hello vyzvednutí a odkládací zeměpisné délky a šířky tooSQL geography body, vypočítá vzdálenost cestě hello pomocí SQL geography body rozdíl a vrátí z náhodného vzorku hello výsledky pro porovnání. Příklad Hello omezí výsledky hello toovalid koordinuje jenom pomocí hello kvality assessment dotaz na data popsané výše.

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

#### <a name="feature-engineering-in-sql-queries"></a>Funkce analýzy v dotazy SQL
Hello popisek generování geography převod zkoumání dotazy a může být také použít toogenerate popisky nebo funkcích odebráním hello počítání část. Další funkce engineering SQL příklady jsou uvedeny v hello [zkoumání dat a funkce Engineering v poznámkovém bloku IPython](#ipnb) části. Je efektivnější toorun hello funkce generování dotazy na úplnou datovou sadu hello nebo velké podmnožinu pomocí dotazů SQL, které se spustit přímo na instanci databáze systému SQL Server hello. Hello dotazy může být spuštěna v **SQL Server Management Studio**, IPython Poznámkový blok nebo všechny vývojové nástroje/prostředí který má přístup k databázi hello místně nebo vzdáleně.

#### <a name="preparing-data-for-model-building"></a>Připravují se Data k vytváření modelů
Hello následující dotaz spojení hello **nyctaxi\_cestě** a **nyctaxi\_tarif** tabulky, vygeneruje štítek binární klasifikace **vysypávány**, Popisek více třída klasifikace **tip\_třída**a extrahuje z náhodného vzorku 1 % z hello úplné připojené k datové sadě. Tento dotaz můžete zkopírovat a vložit přímo v hello [Azure Machine Learning Studio](https://studio.azureml.net) [importovat Data] [ import-data] modul pro přijímání přímé dat z databáze serveru SQL Server hello instance v Azure. dotaz Hello vyloučí záznamy s nesprávnou (0, 0) souřadnice.

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


## <a name="ipnb"></a>Zkoumání dat a funkce technikům v poznámkovém bloku IPython
V této části provedeme zkoumání dat a funkce generování pomocí jazyka Python a SQL dotazů na databázi systému SQL Server hello vytvořili dříve. Ukázka IPython Poznámkový blok s názvem **machine-Learning-data-science-process-sql-story.ipynb** je součástí hello **poznámkových bloků IPython ukázka** složky. Tento poznámkový blok je také k dispozici na [Githubu](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks).

Hello Doporučené pořadí při práci s velkým objemem dat je hello následující:

* Přečtěte si v malé ukázkové hello dat do data v paměti rámečku.
* Provést některé vizualizace a explorations pomocí hello jen Vzorkovaná data.
* Experimentů pomocí funkce inženýrství pomocí hello Vzorkovat data.
* Pro větší zkoumání dat manipulaci s daty a funkce technologie, použijte dotazy SQL tooissue Python přímo pro databázi systému SQL Server hello v hello virtuálního počítače Azure.
* Rozhodněte, hello ukázka velikost toouse pro vytváření modelů Azure Machine Learning.

Jakmile budete připraveni tooproceed tooAzure Machine Learning, můžete buď může:  

1. Uložit hello konečné SQL dotaz tooextract a ukázkové hello dat a způsobené kopírováním a vkládáním hello dotaz přímo do [importovat Data] [ import-data] modulu v Azure Machine Learning. Tato metoda je znázorněn v hello [vytváření modelů v Azure Machine Learning](#mlmodel) části.    
2. Zachování hello vzorků a inženýrství dat, které máte v plánu toouse pro model sestavování v nové databázové tabulky, potom použijte novou tabulku hello v hello [importovat Data] [ import-data] modulu.

Hello následují několik zkoumání dat, vizualizace dat a funkce technici příklady. Další příklady najdete v tématu hello ukázkový SQL IPython Poznámkový blok v hello **poznámkových bloků IPython ukázka** složky.

#### <a name="initialize-database-credentials"></a>Inicializace přihlašovací údaje databáze
Inicializace nastavení připojení k databázi v hello následující proměnné:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database server>

#### <a name="create-database-connection"></a>Vytvoření připojení k databázi
    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

#### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Sestava počtu řádků a sloupců v tabulce nyctaxi_trip
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

* Celkový počet řádků = 173179759  
* Celkový počet sloupců = 14

#### <a name="read-in-a-small-data-sample-from-hello-sql-server-database"></a>Pro čtení ve vzorku malá data z databáze systému SQL Server hello
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

Čas tooread hello ukázková tabulka je 6.492000 sekund  
Počet řádků a sloupců načíst = (84952, 21)

#### <a name="descriptive-statistics"></a>Popisný statistiky
Nyní jsou data připravena tooexplore hello vzorků. Začneme s prohlížení popisný statistiku pro hello **cestě\_vzdálenost** (nebo libovolný jiný) polí:

    df1['trip_distance'].describe()

#### <a name="visualization-box-plot-example"></a>Vizualizace: Příklad vykreslení pole
Potom se podíváme na hello Krabicový graf pro hello cestě vzdálenost toovisualize hello quantiles

    df1.boxplot(column='trip_distance',return_type='dict')

![Vykreslení #1][1]

#### <a name="visualization-distribution-plot-example"></a>Vizualizaci: Příklad vykreslení distribuční
    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Vykreslení #2][2]

#### <a name="visualization-bar-and-line-plots"></a>Vizualizaci: Panelu a řádku pozemků
V tomto příkladu jsme bin hello vzdálenost cesty do pěti přihrádek a vizualizovat hello přihrádkování výsledky.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

Nemůžeme vykreslení hello výše bin distribuce v pás nebo řádek výkresu, jak je uvedeno níže

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Vykreslení #3][3]

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Vykreslení #4][4]

#### <a name="visualization-scatterplot-example"></a>Vizualizaci: Příklad Scatterplot
Ukážeme bodové vykreslení mezi **cestě\_čas\_v\_sekundy** a **cestě\_vzdálenost** toosee, pokud neexistuje žádné korelace

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![Vykreslení #6][6]

Podobně jsme můžete zkontrolovat hello vztah mezi **míra\_kód** a **cestě\_vzdálenost**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![Vykreslení #8][8]

### <a name="sub-sampling-hello-data-in-sql"></a>Hello dílčí vzorkování dat v systému SQL
Při přípravě dat pro sestavování ve model [Azure Machine Learning Studio](https://studio.azureml.net), můžete rozhodnout buď na hello **toouse dotazu SQL přímo v modulu importu dat hello** nebo zachovat hello analyzovány a vzorků data do nové tabulky, které můžete použít v hello [importovat Data] [ import-data] modul s jednoduchou **vyberte * FROM < vaše\_nové\_tabulky\_name >**.

V této části, které vytvoříme nové hello toohold tabulky vzorků a analyzována data. Příkladem přímý dotaz SQL pro vytváření modelů je součástí hello [zkoumání dat a funkce Engineering v systému SQL Server](#dbexplore) části.

#### <a name="create-a-sample-table-and-populate-with-1-of-hello-joined-tables-drop-table-first-if-it-exists"></a>Vytvoření ukázkové tabulky a vyplnit hello připojený tabulek: % 1. Pokud existuje vyřaďte první tabulky.
V této části jsme připojit hello tabulky **nyctaxi\_cestě** a **nyctaxi\_tarif**, extrahujte náhodného vzorku 1 %, a zachovat data hello vzorkovat nový název tabulky  **nyctaxi\_jeden\_procent**:

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

### <a name="data-exploration-using-sql-queries-in-ipython-notebook"></a>Zkoumání dat pomocí dotazů SQL v poznámkovém bloku IPython
V této části nám prozkoumat distribuce dat pomocí hello 1 % vzorků dat, který je uchován v hello nové tabulky, kterou jsme vytvořili výše. Všimněte si, že podobné explorations je možné provést pomocí původní tabulky hello, volitelně pomocí **TABLESAMPLE** toolimit hello zkoumání ukázku nebo omezením hello výsledků tooa zadané časové období pomocí hello **vyzvednutí \_data a času** oddíly, jak je ukázáno v hello [zkoumání dat a funkce Engineering v systému SQL Server](#dbexplore) části.

#### <a name="exploration-daily-distribution-of-trips"></a>Zkoumání: Denní distribuce služebních cest
    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Zkoumání: Distribuční cesty za Medailon
    query = '''
        SELECT medallion,count(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

### <a name="feature-generation-using-sql-queries-in-ipython-notebook"></a>Funkce generování pomocí dotazů SQL v poznámkovém bloku IPython
V této části vygenerujeme nové štítky a funkce přímo pomocí dotazů SQL, operační v tabulce 1 % ukázka hello jsme vytvořili v předchozí části hello.

#### <a name="label-generation-generate-class-labels"></a>Popisek generování: Generování popisků – třída
V následujícím příkladu hello vygeneruje dvě sady popisky toouse pro modelování:

1. Binární třída popisky **vysypávány** (predikci, pokud bude mu udělená tip)
2. Více třídami popisky **tip\_třída** (predikci hello popisek přihrádku nebo rozsah)
   
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

#### <a name="feature-engineering-count-features-for-categorical-columns"></a>Funkce inženýrství: Počet funkcí pro kategorií sloupce
Tento příklad transformuje pole kategorií na číselné pole nahrazením každou kategorii hello počet jeho výskytů v datech hello.

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

#### <a name="feature-engineering-bin-features-for-numerical-columns"></a>Funkce inženýrství: Funkce Koš pro číselné sloupce
Tento příklad transformuje průběžné číselné pole na rozsahy přednastavené kategorie, tedy transformace číselné pole do pole kategorií.

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

#### <a name="feature-engineering-extract-location-features-from-decimal-latitudelongitude"></a>Funkce inženýrství: Extrahování umístění funkce z Decimal zeměpisnou šířku a délku
Tento příklad rozpis hello decimal reprezentace zeměpisnou šířku a zeměpisnou délku pole do více polí oblasti jiné členitosti, například, země, Město, města, blok atd. Všimněte si, že nová hello geo pole nejsou namapované tooactual umístění. Informace o mapování geocode umístění najdete v tématu [služby Bing Maps REST](https://msdn.microsoft.com/library/ff701710.aspx).

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

#### <a name="verify-hello-final-form-of-hello-featurized-table"></a>Ověření formuláře konečné hello hello featurized tabulky
    query = '''SELECT TOP 100 * FROM nyctaxi_one_percent'''
    pd.read_sql(query,conn)

Snažíme se teď připravena tooproceed toomodel sestavení a nasazení modelu v [Azure Machine Learning](https://studio.azureml.net). Hello data jsou připravena k některé z problémů předpovědi hello identifikuje dříve, konkrétně:

1. Binární klasifikace: toopredict, jestli byl tip placené cesty.
2. Více třídami klasifikace: rozsah hello toopredict tipu placené, podle toohello dříve definovaných tříd.
3. Úloha regrese: toopredict hello množství tip placené cesty.  

## <a name="mlmodel"></a>Vytváření modelů v Azure Machine Learning
toobegin hello modelování cvičení, protokolu v prostoru tooyour Azure Machine Learning. Pokud jste ještě nevytvořili machine learning pracovního prostoru, přečtěte si téma [vytvořit pracovní prostor služby Azure Machine Learning](machine-learning-create-workspace.md).

1. tooget začít s Azure Machine Learning, najdete v části [co je Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md)
2. Přihlaste se příliš[Azure Machine Learning Studio](https://studio.azureml.net).
3. Hello Studio Domovská stránka obsahuje širokou řadu informací, videa, kurzy, odkazy toohello odkaz moduly a dalším prostředkům. Další informace o Azure Machine Learning najdete hello [centru dokumentace Azure Machine Learning na](https://azure.microsoft.com/documentation/services/machine-learning/).

Typické výukový experiment sestává z následujících hello:

1. Vytvoření **+ nový** experiment.
2. Získání dat tooAzure hello Machine Learning.
3. Předběžně zpracovat, transformace a pracovat s daty hello podle potřeby.
4. Funkce vygenerujte, podle potřeby.
5. Rozdělení dat hello na školení, ověření nebo testování datové sady (nebo samostatné datové sady pro každou).
6. Vyberte jeden nebo více algoritmů strojového učení v závislosti na hello učení toosolve problém. Například: binární klasifikace, klasifikace více třídami, regresi.
7. Cvičení jednoho nebo více modelů použití hello školení datové sady.
8. Stanovení skóre hello ověření datové sady pomocí vyškolení modely hello.
9. Vyhodnoťte hello modely toocompute hello relevantní metriky pro hello učení problém.
10. Vyladění hello modely a vyberte hello nejlepší toodeploy modelu.

V tomto cvičení jsme již prozkoumali a analyzovány hello dat v systému SQL Server a rozhodli tooingest velikost vzorku hello v Azure Machine Learning. toobuild jeden nebo více modelů předpovědi hello jsme se rozhodli:

1. Získání dat tooAzure hello Machine Learning pomocí hello [importovat Data] [ import-data] modulu, k dispozici v hello **vstupu a výstupu dat** části. Další informace najdete v tématu hello [importovat Data] [ import-data] stránka s referencemi modul.
   
    ![Azure Machine Learning Import dat][17]
2. Vyberte **Azure SQL Database** jako hello **zdroj dat** v hello **vlastnosti** panelu.
3. Zadejte název DNS hello databáze v hello **název databázového serveru** pole. Formát:`tcp:<your_virtual_machine_DNS_name>,1433`
4. Zadejte hello **název databáze** v odpovídající pole hello.
5. Zadejte hello **uživatelské jméno SQL** v hello ** aqccount uživatelského jména a hesla hello v hello **heslo uživatelského účtu serveru**.
6. Zkontrolujte **přijmout některý z certifikátů serveru** možnost.
7. V hello **databázový dotaz** upravit textová oblast, vložte hello dotazu, který extrahuje hello potřeby databáze pole (včetně všech počítané pole, jako je popisky hello) a dolů – ukázky velikost vzorku toohello požadovaných dat hello.

Hello obrázek je příkladem binární klasifikace experimentů, čtení dat přímo z databáze systému SQL Server hello. Podobně jako experimenty konstruovat pro více třídami klasifikaci a regresní problémy.

![Azure Machine Learning Train][10]

> [!IMPORTANT]
> V hello modelování extrakce dat a vzorkuje příklady dotazu uvedené v předchozí části **všech popisků hello tři modelování cvičení jsou zahrnuty v dotazu hello**. Důležitým krokem (povinné) v každé z hello modelování cvičení je příliš**vyloučit** hello nepotřebné štítky pro hello další dva problémy a všemi ostatními **cíle nevracení**. Pro například při použití binární klasifikace, používat hello popisek **vysypávány** a vyloučit hello pole **tip\_– třída**, **tip\_velikost**, a **celkový\_velikost**. Hello druhé jsou placené cíl nevracení vzhledem k tomu, že implikují hello tip.
> 
> tooexclude nepotřebných sloupců a/nebo nevracení cíl, můžete použít hello [výběr sloupců v datové sadě] [ select-columns] modulu nebo hello [upravit Metadata] [ edit-metadata]. Další informace najdete v tématu [výběr sloupců v datové sadě] [ select-columns] a [upravit Metadata] [ edit-metadata] referenční stránky.
> 
> 

## <a name="mldeploy"></a>Nasazování modelů v Azure Machine Learning
Pokud váš model je připraven, můžete snadno nasadit ho jako webovou službu přímo z hello experimentu. Další informace o nasazení webové služby Azure Machine Learning najdete v tématu [nasazení webové služby Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).

toodeploy novou webovou službu, musíte:

1. Vytvoření experimentu vyhodnocování.
2. Nasaďte hello webové služby.

toocreate vyhodnocování experimentovat z **dokončeno** cvičení experiment, klikněte na tlačítko **vytvořit vyhodnocování EXPERIMENTOVAT** akce panelu nižší hello.

![Výpočet skóre Azure][18]

Azure Machine Learning se pokusí toocreate vyhodnocování experimentu podle hello součástí hello výukový experiment. Konkrétně se:

1. Uložit hello trained model a odeberte hello modelu školení moduly.
2. Identifikovat logickou **vstupní port** toorepresent hello očekávána vstupní data schématu.
3. Identifikovat logickou **výstupní port** toorepresent hello očekávané webové služby výstupního schématu.

Když hello vyhodnocování experimentu je vytvořen, zkontrolujte jej a podle potřeby upravte. Typické úpravu je tooreplace hello vstupní datové sady nebo dotaz s jedním, který vyloučí popisek pole, protože tyto nebudete mít k dispozici, když je volána hello služby. Je také že vhodné tooreduce hello velikost hello vstupní datové sady nebo tooa několik záznamů, akorát tooindicate hello vstupní schéma. Pro hello výstupní port, je běžné tooexclude všechny vstupní pole a obsahovat jenom hello **popisky vyhodnocení** a **skóre pro Magnitudu Pravděpodobnostech** v hello výstup pomocí hello [výběr sloupců v datové sadě ] [ select-columns] modulu.

Ukázka vyhodnocování experimentu je hello obrázek. Jakmile budete připraveni toodeploy, klikněte na tlačítko hello **publikování webové služby** tlačítka na dolním panelu akcí hello.

![Azure Machine Learning publikování][11]

toorecap, v tomto kurzu návodu jste vytvořili prostředí vědecké účely dat Azure, práce s velké veřejné datové sady všechny způsob hello z dat pořízení toomodel školení a nasazení webové služby Azure Machine Learning.

### <a name="license-information"></a>Informace o licenci
Tento návod ukázka a jeho doplňujícími skripty a IPython notebook(s) sdílí Microsoft v rámci licencí MIT hello. Zkontrolujte prosím soubor LICENSE.txt hello v adresáři hello hello ukázkový kód na Githubu další podrobnosti.

### <a name="references"></a>Odkazy
• [Stránce pro stažení Andrés Monroy NYC taxíkem cest](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing NYC Taxi cestě dat podle Whong Jan](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC taxíkem a Limousine Komise výzkum a statistiky](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)

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
