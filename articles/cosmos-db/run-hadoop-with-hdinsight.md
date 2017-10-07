---
title: "aaaRun Hadoop úlohy pomocí Azure Cosmos DB a HDInsight | Microsoft Docs"
description: "Zjistěte, jak toorun jednoduché Hive, Pig a MapReduce úlohy s Azure Cosmos DB a Azure HDInsight."
services: cosmos-db
author: dennyglee
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 06f0ea9d-07cb-4593-a9c5-ab912b62ac42
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 06/08/2017
ms.author: denlee
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2e27499f2c4ba951af9a1ade1bcc9c1b6d298fcd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="Azure Cosmos DB-HDInsight"></a>Spustit úlohu Apache Hive, Pig nebo Hadoop pomocí Azure Cosmos DB a HDInsight
Tento kurz ukazuje, jak toorun [Apache Hive][apache-hive], [Apache Pig][apache-pig], a [Apache Hadoop] [ apache-hadoop] Úloh MapReduce v Azure HDInsight s konektorem Hadoop Cosmos DB. Cosmos DB na Hadoop konektor umožňuje tooact Cosmos DB jako zdroj a jímka pro úlohy Hive, Pig a MapReduce. V tomto kurzu použije Cosmos DB jako hello data zdrojového a cílového pro úlohy Hadoop.

Po dokončení tohoto kurzu, budete moct tooanswer hello následující otázky:

* Jak načíst data z databáze Cosmos pomocí úlohy Hive, Pig nebo MapReduce?
* Jak ukládat data do databáze Cosmos. pomocí úlohy Hive, Pig nebo MapReduce?

Doporučujeme začít sledování hello následující video, které jsme spuštění prostřednictvím úlohy Hive pomocí Cosmos databáze a HDInsight.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Use-Azure-DocumentDB-Hadoop-Connector-with-Azure-HDInsight/player]
>
>

Pak se vraťte toothis článek, kterou budete dostávat hello úplné podrobnosti o jak spouštět úlohy analytics na vaše data Cosmos DB.

> [!TIP]
> Tento kurz předpokládá, že máte zkušenosti s jazykem Apache Hadoop, Hive a Pig. Pokud jste nový tooApache Hadoop, Hive a Pig, doporučujeme navštívit hello [dokumentaci Apache Hadoop][apache-hadoop-doc]. V tomto kurzu také předpokládá, že máte zkušenosti s Cosmos DB a máte účet Cosmos DB. Pokud jste nový tooCosmos DB nebo nemáte účet Cosmos DB, podrobnosti naleznete v našem [Začínáme] [ getting-started] stránky.
>
>

Nemáte čas toocomplete hello kurzu a právě chcete tooget hello úplnou ukázku najdete na PowerShell skripty pro Hive, Pig a MapReduce? Nejedná se o problém, je získání [sem][hdinsight-samples]. stažení Hello také obsahuje hello hql, pig a java soubory pro tyto ukázky.

## <a name="NewestVersion"></a>Nejnovější verze
<table border='1'>
    <tr><th>Verze konektoru Hadoop</th>
        <td>1.2.0</td></tr>
    <tr><th>Identifikátor Uri skriptu</th>
        <td>https://portalcontent.BLOB.Core.Windows.NET/scriptaction/documentdb-hadoop-Installer-v04.ps1</td></tr>
    <tr><th>Datum změny</th>
        <td>04/26/2016</td></tr>
    <tr><th>Podporované HDInsight verze</th>
        <td>3.1, 3.2</td></tr>
    <tr><th>Protokol změn</th>
        <td>Aktualizované Cosmos Azure DB Java SDK too1.6.0</br>
            Přidaná podpora pro dělené kolekce jako zdroj a jímka</br>
        </td></tr>
</table>

## <a name="Prerequisites"></a>Požadavky
Než budete postupovat hello pokyny v tomto kurzu, zajistěte, abyste měli hello následující:

* Účet Cosmos DB, databázi a kolekci s dokumenty uvnitř. Další informace najdete v tématu [Začínáme s Cosmos DB][getting-started]. Import ukázkových dat do účtu Cosmos DB s hello [nástroj pro import Cosmos DB][import-data].
* Propustnost. Čtení a zápisy z prostředí HDInsight započítají vůči vaší jednotek přiděleného žádosti pro kolekce.
* Kapacitu pro další uloženou proceduru v každém výstupní kolekce. Hello uložené procedury používají přenosu výsledné dokumentů.
* Kapacita hello výsledné dokumenty z úlohy Hive, Pig nebo MapReduce hello.
* [*Volitelné*] kapacity pro další kolekci.

> [!WARNING]
> V pořadí tooavoid hello vytvoření nové kolekce v žádné z hello úlohy můžete buď tisku hello výsledky toostdout, uložit kontejneru WASB tooyour výstup hello nebo zadat již existující kolekci. V případě hello zadávání existující kolekci se vytvoří nové dokumenty uvnitř hello kolekce a stávající dokumenty bude mít vliv pouze pokud dojde ke konfliktu v *ID*. **konektor Hello automaticky přepíše stávající dokumenty s docházet ke konfliktům id**. Tuto funkci můžete vypnout pomocí nastavení toofalse možnost upsert hello. Pokud je hodnota false upsert a dojde ke konfliktu, se nezdaří úlohy Hadoop hello; vytváření sestav chybu id konflikt.
>
>

## <a name="ProvisionHDInsight"></a>Krok 1: Vytvoření nového clusteru HDInsight
Tento kurz používá akce skriptu z portálu Azure toocustomize hello clusteru HDInsight. V tomto kurzu budeme používat portál Azure toocreate hello clusteru HDInsight. Pokyny, jak toouse rutiny prostředí PowerShell nebo hello SDK rozhraní .NET HDInsight, podívejte se [HDInsight přizpůsobit clustery pomocí akce skriptu] [ hdinsight-custom-provision] článku.

1. Přihlaste se toohello [portálu Azure][azure-portal].
2. Klikněte na tlačítko **+ nový** na hello horní části hello levé navigační, vyhledejte **HDInsight** v hello horním panelu vyhledávání v novém okně hello.
3. **HDInsight** publikováno **Microsoft** se zobrazí v horní části hello hello výsledků. Klikněte na něj a potom na **vytvořit**.
4. Na nový HDInsight Cluster hello vytvořit okno, zadejte vaše **název clusteru** a vyberte hello **předplatné** chcete tooprovision tento prostředek.

    <table border='1'>
        <tr><td>Název clusteru</td><td>Název clusteru hello.<br/>
Název serveru DNS musí spustit a končit znakem alpha číselné a může obsahovat pomlčky.<br/>
Hello pole musí být řetězec o délce 3 až 63 znaků.</td></tr>
        <tr><td>Název odběru</td>
            <td>Pokud máte více než jedno předplatné Azure, vyberte předplatné hello, který bude hostitelem clusteru HDInsight. </td></tr>
    </table>
5.Klikněte na tlačítko **vybrat typ clusteru** a následující vlastnosti toohello hello sadu zadané hodnoty.

    <table border='1'>
        <tr><td>Typ clusteru</td><td><strong>Hadoop</strong></td></tr>
        <tr><td>Vrstvy clusteru</td><td><strong>Standard</strong></td></tr>
        <tr><td>Operační systém</td><td><strong>Windows</strong></td></tr>
        <tr><td>Verze</td><td>nejnovější verzi</td></tr>
    </table>

    Nyní, klikněte na tlačítko **vyberte**.

    ![Zadejte podrobnosti o počáteční clusteru Hadoop HDInsight][image-customprovision-page1]
6. Klikněte na **pověření** tooset přihlašovací jméno a přihlašovací údaje vzdálený přístup. Zvolte vaše **uživatelské jméno přihlášení clusteru** a **clusteru heslo pro přihlášení**.

    Pokud chcete tooremote do clusteru, vyberte *Ano* v hello dolní části okna hello a zadejte uživatelské jméno a heslo.
7. Klikněte na **zdroj dat** přístup k vaší primární umístění pro data tooset. Zvolte hello **metodu výběru** a zadejte již existující účet úložiště nebo vytvořte novou.
8. Na hello stejné okno, zadejte **výchozí kontejner** a **umístění**. A klikněte na tlačítko **vyberte**.

   > [!NOTE]
   > Vyberte zavřít tooyour umístění Cosmos DB oblast účtu pro lepší výkon
   >
   >
9. Klikněte na **cenová** tooselect hello počet a typ uzlů. Můžete později na zachovat hello výchozí konfigurace a škálování hello počet uzlů pracovního procesu.
10. Klikněte na tlačítko **volitelné konfiguraci**, pak **akcí skriptů** v hello volitelné konfigurace okna.

     Akce skriptu zadejte následující informace toocustomize hello clusteru HDInsight.

     <table border='1'>
         <tr><th>Vlastnost</th><th>Hodnota</th></tr>
         <tr><td>Name (Název)</td>
             <td>Zadejte název akce skriptu hello.</td></tr>
         <tr><td>Identifikátor URI skriptu</td>
             <td>Zadejte hello URI toohello skript, který je vyvolaná toocustomize hello clusteru.</br></br>
Zadejte: </br> <strong>https://portalcontent.BLOB.Core.Windows.NET/scriptaction/documentdb-hadoop-Installer-v04.ps1</strong>.</td></tr>
         <tr><td>Hlavní uzel</td>
             <td>Klikněte na tlačítko hello políčko toorun hello skript prostředí PowerShell na hello hlavní uzel.</br></br>
             <strong>Zaškrtněte toto políčko</strong>.</td></tr>
         <tr><td>Worker</td>
             <td>Klikněte na tlačítko skript prostředí PowerShell hello políčko toorun hello na hello pracovního uzlu.</br></br>
             <strong>Zaškrtněte toto políčko</strong>.</td></tr>
         <tr><td>Zookeeper</td>
             <td>Klikněte na tlačítko skript prostředí PowerShell hello políčko toorun hello na hello Zookeeper.</br></br>
             <strong>Není potřeba</strong>.
             </td></tr>
         <tr><td>Parametry</td>
             <td>Zadejte parametry hello, pokud to vyžaduje hello skriptu.</br></br>
             <strong>Žádné parametry potřeby</strong>.</td></tr>
     </table>
11.Buď vytvořit novou **skupiny prostředků** nebo použít existující skupinu prostředků v rámci svého předplatného Azure.
12. Teď, zkontrolujte **Pin toodashboard** tootrack jeho nasazení a klikněte na tlačítko **vytvořit**!

## <a name="InstallCmdlets"></a>Krok 2: Instalace a konfigurace prostředí Azure PowerShell
1. Nainstalujte Azure PowerShell. Pokyny naleznete [sem][powershell-install-configure].

   > [!NOTE]
   > Jenom pro dotazy Hive, případně můžete použít HDInsight je online Editor Hive. toodo tedy přihlásit toohello [portálu Azure][azure-portal], klikněte na tlačítko **HDInsight** tooview levém podokně na hello seznam clusterů HDInsight. Klikněte na cluster hello dotazů Hive toorun na a pak klikněte na tlačítko **dotazu konzoly**.
   >
   >
2. Otevřete hello Azure PowerShell Integrované skriptovací prostředí:

   * V počítači se systémem Windows 8 nebo Windows Server 2012 nebo vyšší, můžete použít předdefinované hello vyhledávání. Na úvodní obrazovce hello zadejte **prostředí powershell ise** a klikněte na tlačítko **Enter**.
   * V počítači se systémem starším než Windows 8 nebo Windows Server 2012 na verzi použijte nabídku Start hello. Z nabídky Start hello, zadejte **příkazového řádku** hello vyhledávacího pole a pak v seznamu hello výsledků, klikněte na tlačítko **příkazového řádku**. Hello příkazového řádku, zadejte **powershell_ise** a klikněte na tlačítko **Enter**.
3. Přidání účtu Azure.

   1. V podokně konzoly hello, zadejte **Add-AzureAccount** a klikněte na tlačítko **Enter**.
   2. Zadejte e-mailovou adresu hello spojené s předplatným Azure a klikněte na tlačítko **pokračovat**.
   3. Zadejte heslo hello předplatného Azure.
   4. Klikněte na tlačítko **přihlášení**.
4. Následující diagram Hello identifikuje hello důležitou součástí prostředí Azure PowerShell skriptování.

    ![Diagram pro prostředí Azure PowerShell][azure-powershell-diagram]

## <a name="RunHive"></a>Krok 3: Spuštění úlohy Hive pomocí Cosmos databáze a HDInsight
> [!IMPORTANT]
> Všechny proměnné indikován < > musí být zadána pomocí nastavení konfigurace.
>
>

1. Nastavit hello následující proměnné v váš skript prostředí PowerShell.

        # Provide Azure subscription name, hello Azure Storage account and container that is used for hello default HDInsight file system.
        $subscriptionName = "<SubscriptionName>"
        $storageAccountName = "<AzureStorageAccountName>"
        $containerName = "<AzureStorageContainerName>"

        # Provide hello HDInsight cluster name where you want toorun hello Hive job.
        $clusterName = "<HDInsightClusterName>"
2. <p>Začněme, vytváření řetězec vašeho dotazu. Jsme budete napsat dotaz Hive, které trvá vygenerované systémem časová razítka (_ts) a jedinečné ID (_rid) z Azure Cosmos DB kolekce všechny dokumenty, zaznamená všechny dokumenty o hello minutu a pak uloží výsledky hello zpět do nové kolekce Azure Cosmos DB.</p>

    <p>Nejdříve vytvoříme tabulku Hive z našich kolekce Azure Cosmos DB. Přidejte následující kód fragment kódu toohello podokno skriptu prostředí PowerShell hello <strong>po</strong> fragment kódu hello od #1. Ujistěte se, že naše _ts toojust dokumenty a _rid obsahuje hello volitelné DocumentDB.query parametr t uvolnění dočasné paměti.</p>

   > [!NOTE]
   > **Pojmenování DocumentDB.inputCollections se jedná o chybu.** Ano, jsme povolit přidávání více kolekcí jako vstup: </br>
   >
   >

        '*DocumentDB.inputCollections*' = '*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*' A1A</br> hello collection names are separated without spaces, using only a single comma.

        # Create a Hive table using data from DocumentDB. Pass DocumentDB hello query toofilter transferred data too_rid and _ts.
        $queryStringPart1 = "drop table DocumentDB_timestamps; "  +
                            "create external table DocumentDB_timestamps(id string, ts BIGINT) "  +
                            "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' "  +
                            "tblproperties ( " +
                                "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                "'DocumentDB.key' = '<DocumentDB Primary Key>', " +
                                "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                "'DocumentDB.inputCollections' = '<DocumentDB Input Collection Name>', " +
                                "'DocumentDB.query' = 'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

1. V dalším kroku vytvoříme tabulku Hive pro kolekci výstup hello. Vlastnosti dokumentu výstup Hello bude hello měsíc, den, hodinu, minutu a celkový počet výskytů hello.

   > [!NOTE]
   > **Ještě znovu pojmenování DocumentDB.outputCollections se jedná o chybu.** Ano, jsme povolit přidávání více kolekcí jako výstup: </br>
   > '*DocumentDB.outputCollections*'='*\<název kolekce DocumentDB výstupu 1\>*,*\<název kolekce DocumentDB výstupu 2\>*. </br> názvy Hello kolekce jsou oddělené bez mezer pouze jeden čárkou. </br></br>
   > Dokumenty bude distribuované kruhového dotazování napříč více kolekcí. Batch dokumentů se budou ukládat do jedné kolekce a potom druhý dávky dokumenty se uloží v hello další kolekce a tak dále.
   >
   >

       # Create a Hive table for hello output data tooDocumentDB.
       $queryStringPart2 = "drop table DocumentDB_analytics; " +
                             "create external table DocumentDB_analytics(Month INT, Day INT, Hour INT, Minute INT, Total INT) " +
                             "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' " +
                             "tblproperties ( " +
                                 "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                 "'DocumentDB.key' = '<DocumentDB Primary Key>', " +  
                                 "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                 "'DocumentDB.outputCollections' = '<DocumentDB Output Collection Name>' ); "
2. Nakonec umožňuje tally hello dokumentů měsíc, den, hodinu a minutu a vložení hello výsledky zpět do hello výstupní tabulku Hive.

        # GROUP BY minute, COUNT entries for each, INSERT INTO output Hive table.
        $queryStringPart3 = "INSERT INTO table DocumentDB_analytics " +
                              "SELECT month(from_unixtime(ts)) as Month, day(from_unixtime(ts)) as Day, " +
                              "hour(from_unixtime(ts)) as Hour, minute(from_unixtime(ts)) as Minute, " +
                              "COUNT(*) AS Total " +
                              "FROM DocumentDB_timestamps " +
                              "GROUP BY month(from_unixtime(ts)), day(from_unixtime(ts)), " +
                              "hour(from_unixtime(ts)) , minute(from_unixtime(ts)); "
3. Přidejte následující fragment kódu toocreate skriptu definice úlohy Hive z předchozího dotazu hello hello.

        # Create a Hive job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $hiveJobDefinition = New-AzureHDInsightHiveJobDefinition -Query $queryString

    Můžete také použít hello – soubor přepínač toospecify soubor skriptu HiveQL na HDFS.
4. Přidejte následující fragment kódu toosave hello počáteční čas hello a odeslat úlohu Hive hello.

        # Save hello start time and submit hello job toohello cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $hiveJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $hiveJobDefinition
5. Přidejte následující toowait pro toocomplete úlohy Hive hello hello.

        # Wait for hello Hive job toocomplete.
        Wait-AzureHDInsightJob -Job $hiveJob -WaitTimeoutInSeconds 3600
6. Přidejte následující počáteční hello tooprint standardní výstupní zařízení a hello hello a ukončení.

        # Print hello standard error, hello standard output of hello Hive job, and hello start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $hiveJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
7. **Spustit** nový skript! **Klikněte na tlačítko** hello zelená provést tlačítko.
8. Zkontrolujte výsledky hello. Přihlaste se k hello [portálu Azure][azure-portal].

   1. Klikněte na tlačítko <strong>Procházet</strong> na levé straně panelu hello. </br>
   2. Klikněte na tlačítko <strong>všechno, co</strong> v pravém horním hello hello procházet panelu. </br>
   3. Najít a klikněte na tlačítko <strong>Azure Cosmos DB účty</strong>. </br>
   4. Poté vyhledejte vaše <strong>Azure Cosmos DB účet</strong>, pak <strong>Azure Cosmos DB databáze</strong> a <strong>Azure Cosmos DB kolekce</strong> přidružené zadaná v kolekce výstup hello dotaz Hive.</br>
   5. Nakonec klikněte na <strong>Průzkumníka dokumentů</strong> pod <strong>Developer Tools</strong>.</br></p>

   Zobrazí se hello výsledky dotazu Hive.

   ![Výsledky dotazu Hive][image-hive-query-results]

## <a name="RunPig"></a>Krok 4: Spuštění úlohy Pig pomocí Cosmos databáze a HDInsight
> [!IMPORTANT]
> Všechny proměnné indikován < > musí být zadána pomocí nastavení konfigurace.
>
>

1. Nastavit hello následující proměnné v váš skript prostředí PowerShell.

        # Provide Azure subscription name.
        $subscriptionName = "Azure Subscription Name"

        # Provide HDInsight cluster name where you want toorun hello Pig job.
        $clusterName = "Azure HDInsight Cluster Name"
2. <p>Začněme, vytváření řetězec vašeho dotazu. Jsme budete napsat dotaz Pig, které trvá vygenerované systémem časová razítka (_ts) a jedinečné ID (_rid) z Azure Cosmos DB kolekce všechny dokumenty, zaznamená všechny dokumenty o hello minutu a pak uloží výsledky hello zpět do nové kolekce Azure Cosmos DB.</p>
    <p>Nejdřív načíst dokumenty z databáze Cosmos do HDInsight. Přidejte následující kód fragment kódu toohello podokno skriptu prostředí PowerShell hello <strong>po</strong> fragment kódu hello od #1. Zajistěte, aby naše _ts toojust dokumenty a _rid dotazů tooadd DocumentDB tootrim parametr dotazu volitelné DocumentDB toohello.</p>

   > [!NOTE]
   > Ano, jsme povolit přidávání více kolekcí jako vstup: </br>
   > '*\<Název kolekce DocumentDB vstup 1\>*,*\<název kolekce DocumentDB vstup 2\>*.</br> názvy Hello kolekce jsou oddělené bez mezer pouze jeden čárkou. </b>
   >
   >

    Dokumenty bude distribuované kruhového dotazování napříč více kolekcí. Batch dokumentů se budou ukládat do jedné kolekce a potom druhý dávky dokumenty se uloží v hello další kolekce a tak dále.

        # Load data from Cosmos DB. Pass DocumentDB query toofilter transferred data too_rid and _ts.
        $queryStringPart1 = "DocumentDB_timestamps = LOAD '<DocumentDB Endpoint>' USING com.microsoft.azure.documentdb.pig.DocumentDBLoader( " +
                                                        "'<DocumentDB Primary Key>', " +
                                                        "'<DocumentDB Database Name>', " +
                                                        "'<DocumentDB Input Collection Name>', " +
                                                        "'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "
3. Dále umožňuje tally hello dokumentů podle hello měsíc, den, hodinu, minutu a celkový počet výskytů hello.

       # GROUP BY minute and COUNT entries for each.
       $queryStringPart2 = "timestamp_record = FOREACH DocumentDB_timestamps GENERATE `$0#'id' as id:int, ToDate((long)(`$0#'ts') * 1000) as timestamp:datetime; " +
                           "by_minute = GROUP timestamp_record BY (GetYear(timestamp), GetMonth(timestamp), GetDay(timestamp), GetHour(timestamp), GetMinute(timestamp)); " +
                           "by_minute_count = FOREACH by_minute GENERATE FLATTEN(group) as (Year:int, Month:int, Day:int, Hour:int, Minute:int), COUNT(timestamp_record) as Total:int; "
4. Umožňuje uložit nakonec hello výsledky do naší nové kolekce výstup.

   > [!NOTE]
   > Ano, jsme povolit přidávání více kolekcí jako výstup: </br>
   > '\<Název kolekce DocumentDB výstupu 1\>,\<název kolekce DocumentDB výstupu 2\>.</br> názvy Hello kolekce jsou oddělené bez mezer pouze jeden čárkou.</br>
   > Dokumenty budou být distribuované kruhového dotazování v rámci hello více kolekcí. Batch dokumentů se budou ukládat do jedné kolekce a potom druhý dávky dokumenty se uloží v hello další kolekce a tak dále.
   >
   >

        # Store output data tooCosmos DB.
        $queryStringPart3 = "STORE by_minute_count INTO '<DocumentDB Endpoint>' " +
                            "USING com.microsoft.azure.documentdb.pig.DocumentDBStorage( " +
                                "'<DocumentDB Primary Key>', " +
                                "'<DocumentDB Database Name>', " +
                                "'<DocumentDB Output Collection Name>'); "
5. Přidejte následující fragment kódu toocreate skriptu definice úlohy Pig z předchozího dotazu hello hello.

        # Create a Pig job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $pigJobDefinition = New-AzureHDInsightPigJobDefinition -Query $queryString -StatusFolder $statusFolder

    Můžete také použít hello – soubor přepínač toospecify soubor skriptu Pig na HDFS.
6. Přidejte následující fragment kódu toosave hello počáteční čas hello a odeslat úlohu Pig hello.

        # Save hello start time and submit hello job toohello cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $pigJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $pigJobDefinition
7. Přidejte následující toowait pro toocomplete úlohy Pig hello hello.

        # Wait for hello Pig job toocomplete.
        Wait-AzureHDInsightJob -Job $pigJob -WaitTimeoutInSeconds 3600
8. Přidejte následující počáteční hello tooprint standardní výstupní zařízení a hello hello a ukončení.

        # Print hello standard error, hello standard output of hello Hive job, and hello start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $pigJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
9. **Spustit** nový skript! **Klikněte na tlačítko** hello zelená provést tlačítko.
10. Zkontrolujte výsledky hello. Přihlaste se k hello [portálu Azure][azure-portal].

    1. Klikněte na tlačítko <strong>Procházet</strong> na levé straně panelu hello. </br>
    2. Klikněte na tlačítko <strong>všechno, co</strong> v pravém horním hello hello procházet panelu. </br>
    3. Najít a klikněte na tlačítko <strong>Azure Cosmos DB účty</strong>. </br>
    4. Poté vyhledejte vaše <strong>Azure Cosmos DB účet</strong>, pak <strong>Azure Cosmos DB databáze</strong> a <strong>Azure Cosmos DB kolekce</strong> přidružené zadaná v kolekce výstup hello Pig dotazu.</br>
    5. Nakonec klikněte na <strong>Průzkumníka dokumentů</strong> pod <strong>Developer Tools</strong>.</br></p>

    Zobrazí se výsledky dotazu Pig hello.

    ![Výsledky dotazu pig][image-pig-query-results]

## <a name="RunMapReduce"></a>Krok 5: Spustit úlohu MapReduce pomocí Azure Cosmos DB a HDInsight
1. Nastavit hello následující proměnné v váš skript prostředí PowerShell.

        $subscriptionName = "<SubscriptionName>"   # Azure subscription name
        $clusterName = "<ClusterName>"             # HDInsight cluster name
2. Jsme budete spustit úlohu MapReduce, která sečte hello počet výskytů pro každou vlastnost dokumentu z kolekce Azure Cosmos DB. Přidejte tento fragment skriptu **po** výše fragmentu hello.

        # Define hello MapReduce job.
        $TallyPropertiesJobDefinition = New-AzureHDInsightMapReduceJobDefinition -JarFile "wasb:///example/jars/TallyProperties-v01.jar" -ClassName "TallyProperties" -Arguments "<DocumentDB Endpoint>","<DocumentDB Primary Key>", "<DocumentDB Database Name>","<DocumentDB Input Collection Name>","<DocumentDB Output Collection Name>","<[Optional] DocumentDB Query>"

   > [!NOTE]
   > TallyProperties v01.jar se dodává s hello vlastní instalace hello Cosmos DB Hadoop konektor.
   >
   >
3. Přidejte následující úlohu MapReduce hello toosubmit příkaz hello.

        # Save hello start time and submit hello job.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $TallyPropertiesJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $TallyPropertiesJobDefinition | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600  

    Kromě toho toohello definice úlohy MapReduce, je také zadat název clusteru HDInsight hello, kam chcete úlohu MapReduce hello toorun a přihlašovací údaje hello. Hello počáteční AzureHDInsightJob je synchronního volání. dokončení hello toocheck hello úlohy, použijte hello *čekání AzureHDInsightJob* rutiny.
4. Přidejte následující příkaz toocheck hello žádné chyby u spuštěná úloha MapReduce hello.

        # Get hello job output and print hello start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $TallyPropertiesJob.JobId -StandardError
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
5. **Spustit** nový skript! **Klikněte na tlačítko** hello zelená provést tlačítko.
6. Zkontrolujte výsledky hello. Přihlaste se k hello [portálu Azure][azure-portal].

   1. Klikněte na tlačítko <strong>Procházet</strong> na levé straně panelu hello.
   2. Klikněte na tlačítko <strong>všechno, co</strong> v pravém horním hello hello procházet panelu.
   3. Najít a klikněte na tlačítko <strong>Azure Cosmos DB účty</strong>.
   4. Poté vyhledejte vaše <strong>Azure Cosmos DB účet</strong>, pak <strong>Azure Cosmos DB databáze</strong> a <strong>Azure Cosmos DB kolekce</strong> přidružené zadaná v kolekce výstup hello úlohu MapReduce.
   5. Nakonec klikněte na <strong>Průzkumníka dokumentů</strong> pod <strong>Developer Tools</strong>.

      Zobrazí se hello výsledků úlohy MapReduce.

      ![MapReduce výsledky dotazu][image-mapreduce-query-results]

## <a name="NextSteps"></a>Další kroky
Blahopřejeme! Právě jste spustili vaše první úlohy Hive, Pig a MapReduce pomocí Azure Cosmos DB a HDInsight.

Máme open source naše konektor Hadoop. Pokud byste chtěli, můžete přispívat na [Githubu][github].

toolearn více, najdete v části hello následující články:

* [Vývoj aplikace Java pomocí Documentdb][documentdb-java-application]
* [Vývoj aplikací Java MapReduce pro Hadoop v HDInsight][hdinsight-develop-deploy-java-mapreduce]
* [Začínáme používat Hadoop s Hive v HDInsight tooanalyze mobilního telefonu použití][hdinsight-get-started]
* [Používání nástroje MapReduce s HDInsight][hdinsight-use-mapreduce]
* [Použití Hivu se službou HDInsight][hdinsight-use-hive]
* [Použití Pigu se službou HDInsight][hdinsight-use-pig]
* [Přizpůsobení clusterů HDInsight pomocí akce skriptu][hdinsight-hadoop-customize-cluster]

[apache-hadoop]: http://hadoop.apache.org/
[apache-hadoop-doc]: http://hadoop.apache.org/docs/current/
[apache-hive]: http://hive.apache.org/
[apache-pig]: http://pig.apache.org/
[getting-started]: documentdb-get-started.md

[azure-portal]: https://portal.azure.com/
[azure-powershell-diagram]: ./media/run-hadoop-with-hdinsight/azurepowershell-diagram-med.png

[hdinsight-samples]: http://portalcontent.blob.core.windows.net/samples/documentdb-hdinsight-samples.zip
[github]: https://github.com/Azure/azure-documentdb-hadoop
[documentdb-java-application]: documentdb-java-application.md
[import-data]: import-data.md

[hdinsight-custom-provision]: ../hdinsight/hdinsight-provision-clusters.md
[hdinsight-develop-deploy-java-mapreduce]: ../hdinsight/hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-hadoop-customize-cluster]: ../hdinsight/hdinsight-hadoop-customize-cluster.md
[hdinsight-get-started]: ../hdinsight/hdinsight-hadoop-tutorial-get-started-windows.md
[hdinsight-storage]: ../hdinsight/hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: ../hdinsight/hdinsight-use-hive.md
[hdinsight-use-mapreduce]: ../hdinsight/hdinsight-use-mapreduce.md
[hdinsight-use-pig]: ../hdinsight/hdinsight-use-pig.md

[image-customprovision-page1]: ./media/run-hadoop-with-hdinsight/customprovision-page1.png
[image-hive-query-results]: ./media/run-hadoop-with-hdinsight/hivequeryresults.PNG
[image-mapreduce-query-results]: ./media/run-hadoop-with-hdinsight/mapreducequeryresults.PNG
[image-pig-query-results]: ./media/run-hadoop-with-hdinsight/pigqueryresults.PNG

[powershell-install-configure]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0
