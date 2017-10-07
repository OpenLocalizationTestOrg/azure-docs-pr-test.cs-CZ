---
title: "aaaAnalyze dat Twitteru pomocí Hadoop v HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak toouse Hive tooanalyze Twitter data v Hadoop v HDInsight toofind hello využití frekvenci určité slovo."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 78e4ea33-9714-424d-ac07-3d60ecaebf2e
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 40c0a1afbc1fff10c070d22a99cd9d32d42f230a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Analýza dat Twitteru pomocí Hive v HDInsight
Sociální weby jsou jedním z hlavních řízení vynutí pro přijetí velkých objemů dat hello. Veřejná rozhraní API poskytované lokality jako Twitter jsou užitečné zdroje dat pro analýzu a pochopení oblíbených trendy.
V tomto kurzu budete získat tweetů pomocí Twitteru, streamování rozhraní API a potom pomocí Apache Hive v Azure HDInsight tooget seznam Twitter uživatelů, kteří odeslané hello většina tweetů, které obsahovaly určité slovo.

> [!IMPORTANT]
> Hello kroky v tomto dokumentu vyžadují cluster HDInsight se systémem Windows. Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement). Kroky konkrétní tooa systémem Linux clusteru, najdete v části [Twitter analýza dat pomocí Hive v HDInsight (Linux)](hdinsight-analyze-twitter-data-linux.md).

## <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, musíte mít následující hello:

* **Pracovní stanice** pomocí prostředí Azure PowerShell nainstalovaný a nakonfigurovaný.

    tooexecute skriptů prostředí Windows PowerShell, je nutné spustit prostředí Azure PowerShell jako správce a nastavte zásady spouštění hello příliš*RemoteSigned*. V tématu [spustit prostředí Windows PowerShell skripty][powershell-script].

    Před spuštěním skriptů prostředí Windows PowerShell, zkontrolujte, zda že jsou připojené tooyour předplatného Azure pomocí hello následující rutiny:

    ```powershell
    Login-AzureRmAccount
    ```

    Pokud máte víc předplatných Azure, použijte následující rutinu tooset hello aktuální předplatné hello:

    ```powershell
    Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>
    ```

    > [!IMPORTANT]
    > Podpora prostředí Azure PowerShell pro správu prostředků služby HDInsight pomocí Azure Service Manageru je **zastaralá** a k 1. lednu 2017 jsme ji odebrali. kroky Hello, v tento dokument použít hello nové rutiny služby HDInsight, které fungují s Azure Resource Manager.
    >
    > Postupujte podle kroků hello v [nainstalovat a nakonfigurovat Azure PowerShell](/powershell/azureps-cmdlets-docs) tooinstall hello nejnovější verzi prostředí Azure PowerShell. Pokud máte skripty, že toobe potřeba upravit hello toouse nové se rutiny, které pracují s Azure Resource Managerem najdete v tématu [tooAzure migrace založené na správci prostředků vývoj nástroje pro clustery služby HDInsight](hdinsight-hadoop-development-using-azure-resource-manager.md) Další informace.

* **Cluster Azure HDInsight**. Postup při zřizování clusteru naleznete v tématu [začněte používat HDInsight] [ hdinsight-get-started] nebo [zřizování clusterů HDInsight][hdinsight-provision]. Název clusteru hello budete potřebovat později v kurzu hello.

Hello následující tabulka uvádí soubory hello použité v tomto kurzu:

| Soubory | Popis |
| --- | --- |
| /tutorials/Twitter/data/tweets.txt |Hello zdroje dat pro úlohy Hive hello. |
| /tutorials/Twitter/Output |Hello výstupní složky pro úlohy Hive hello. Hello výchozí Hive úlohy výstup název souboru je **000000_0**. |
| tutorials/Twitter/Twitter.hql |soubor skriptu HiveQL Hello. |
| /tutorials/Twitter/JobStatus |Hello stav úlohy Hadoop. |

## <a name="get-twitter-feed"></a>Get Twitteru
V tomto kurzu budete používat hello [Twitter streamování rozhraní API][twitter-streaming-api]. je technologie Hello konkrétní Twitter streamování použijete rozhraní API [stavy nebo filtr][twitter-statuses-filter].

> [!NOTE]
> Soubor obsahující 10 000 tweetů a soubor skriptu Hive hello (popsaná v další části hello) byly odeslány ve veřejném kontejneru Blob. Pokud chcete soubory toouse hello nahrát, můžete tuto část přeskočit.

[Tweetů data](https://dev.twitter.com/docs/platform-objects/tweets) je uložený ve formátu JavaScript Object Notation (JSON) hello, která obsahuje komplexní vnořené struktury. Místo psaní velkého počtu řádků kódu s použitím konvenční programovací jazyk, můžete převést tuto strukturu vnořené do tabulky Hive, tak, aby může být dotazován podle jazyka SQL (Structured Query) – jako jazyka nazývaného HiveQL.

Twitter používá rozhraní API tooits OAuth tooprovide oprávnění přístupu. OAuth je ověřovací protokol, který umožňuje uživatelům tooapprove aplikace tooact jejich jménem bez sdílení své heslo. Další informace najdete na [oauth.net](http://oauth.net/) nebo v hello vynikající [tooOAuth příručka začátečníka](http://hueniverse.com/oauth/) z Hueniverse.

první krok toouse Hello OAuth je toocreate novou aplikaci na webu služby Twitter vývojáře hello.

**toocreate aplikace služby Twitter.**

1. Přihlaste se příliš[https://apps.twitter.com/](https://apps.twitter.com/). Klikněte na tlačítko hello **nyní** odkaz, pokud nemáte účet služby Twitter.
2. Klikněte na tlačítko **vytvořte novou aplikaci**.
3. Zadejte **název**, **popis**, **webu**. Můžete použít adresu URL pro hello **webu** pole. Hello následující tabulka uvádí některé ukázkové hodnoty toouse:

   | Pole | Hodnota |
   | --- | --- |
   |  Name (Název) |MyHDInsightApp |
   |  Popis |MyHDInsightApp |
   |  Web |http://www.myhdinsightapp.com |
4. Zkontrolujte **souhlasím**a potom klikněte na **vytvořit aplikaci služby Twitter**.
5. Klikněte na tlačítko hello **oprávnění** kartě hello výchozí oprávnění je **jen pro čtení**. Toto je dostatečný pro účely tohoto kurzu.
6. Klikněte na tlačítko hello **klíče a přístupové tokeny** kartě.
7. Klikněte na tlačítko **vytvořit můj přístupový token**.
8. Klikněte na tlačítko **testovací OAuth** v pravém horním rohu hello hello stránky.
9. Zapište **uživatelský klíč**, **uživatelský tajný klíč**, **přístupový token**, a **tajný klíč přístupového tokenu**. Hello hodnoty budete potřebovat později v kurzu hello.

V tomto kurzu použijete volání webové služby v prostředí Windows PowerShell toomake hello. C# .NET ukázku, najdete v části [analýzu v reálném čase sentimentu Twitter s HBase v HDInsight][hdinsight-hbase-twitter-sentiment]. je technologie Hello volání webové služby dalších oblíbených nástroj toomake [ *Curl*][curl]. Curl si můžete stáhnout z [sem][curl-download].

> [!NOTE]
> Pokud použijete příkaz curl hello v systému Windows, použijte dvojité uvozovky místo jednoduchých uvozovek a být pro hodnoty možnosti hello.

**tooget tweetů**

1. Otevřete Windows PowerShell Integrované skriptovací prostředí (ISE) hello. (Na obrazovce Windows 8 Start hello zadejte **PowerShell_ISE** a pak klikněte na **Windows PowerShell ISE**. V tématu [spuštění prostředí Windows PowerShell v systému Windows 8 a Windows][powershell-start].)
2. Zkopírujte následující skript do podokno skriptu hello hello:

    ```powershell
    #region - variables and constants
    $clusterName = "<HDInsightClusterName>" # Enter hello HDInsight cluster name

    # Enter hello OAuth information for your Twitter application
    $oauth_consumer_key = "<TwitterAppConsumerKey>";
    $oauth_consumer_secret = "<TwitterAppConsumerSecret>";
    $oauth_token = "<TwitterAppAccessToken>";
    $oauth_token_secret = "<TwitterAppAccessTokenSecret>";

    $destBlobName = "tutorials/twitter/data/tweets.txt" # This script saves hello tweets into this blob.

    $trackString = "Azure, Cloud, HDInsight" # This script gets hello tweets containing these keywords.
    $track = [System.Uri]::EscapeDataString($trackString);
    $lineMax = 10000  # hello script will get this number of tweets. It is about 3 minutes every 100 lines.
    #endregion

    #region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
    Login-AzureRmAccount
    #endregion

    #region - Create a block blob object for writing tweets into Blob storage
    Write-Host "Get hello default storage account name and Blob container name using hello cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -Name $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $storageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $containerName = $myCluster.DefaultStorageContainer
    Write-Host "`tThe storage account name is $storageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $containerName." -ForegroundColor Yellow

    Write-Host "Define hello Azure storage connection string ..." -ForegroundColor Green
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$storageAccountName;AccountKey=$storageAccountKey"
    Write-Host "`tThe connection string is $storageConnectionString." -ForegroundColor Yellow

    Write-Host "Create block blob object ..." -ForegroundColor Green
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($containerName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
    #end region

    # region - Format OAuth strings
    Write-Host "Format oauth strings ..." -ForegroundColor Green
    $oauth_nonce = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes([System.DateTime]::Now.Ticks.ToString()));
    $ts = [System.DateTime]::UtcNow - [System.DateTime]::ParseExact("01/01/1970", "dd/MM/yyyy", $null)
    $oauth_timestamp = [System.Convert]::ToInt64($ts.TotalSeconds).ToString();

    $signature = "POST&";
    $signature += [System.Uri]::EscapeDataString("https://stream.twitter.com/1.1/statuses/filter.json") + "&";
    $signature += [System.Uri]::EscapeDataString("oauth_consumer_key=" + $oauth_consumer_key + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_nonce=" + $oauth_nonce + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_signature_method=HMAC-SHA1&");
    $signature += [System.Uri]::EscapeDataString("oauth_timestamp=" + $oauth_timestamp + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_token=" + $oauth_token + "&");
    $signature += [System.Uri]::EscapeDataString("oauth_version=1.0&");
    $signature += [System.Uri]::EscapeDataString("track=" + $track);

    $signature_key = [System.Uri]::EscapeDataString($oauth_consumer_secret) + "&" + [System.Uri]::EscapeDataString($oauth_token_secret);

    $hmacsha1 = new-object System.Security.Cryptography.HMACSHA1;
    $hmacsha1.Key = [System.Text.Encoding]::ASCII.GetBytes($signature_key);
    $oauth_signature = [System.Convert]::ToBase64String($hmacsha1.ComputeHash([System.Text.Encoding]::ASCII.GetBytes($signature)));

    $oauth_authorization = 'OAuth ';
    $oauth_authorization += 'oauth_consumer_key="' + [System.Uri]::EscapeDataString($oauth_consumer_key) + '",';
    $oauth_authorization += 'oauth_nonce="' + [System.Uri]::EscapeDataString($oauth_nonce) + '",';
    $oauth_authorization += 'oauth_signature="' + [System.Uri]::EscapeDataString($oauth_signature) + '",';
    $oauth_authorization += 'oauth_signature_method="HMAC-SHA1",'
    $oauth_authorization += 'oauth_timestamp="' + [System.Uri]::EscapeDataString($oauth_timestamp) + '",'
    $oauth_authorization += 'oauth_token="' + [System.Uri]::EscapeDataString($oauth_token) + '",';
    $oauth_authorization += 'oauth_version="1.0"';

    $post_body = [System.Text.Encoding]::ASCII.GetBytes("track=" + $track);
    #endregion

    #region - Read tweets
    Write-Host "Create HTTP web request ..." -ForegroundColor Green
    [System.Net.HttpWebRequest] $request = [System.Net.WebRequest]::Create("https://stream.twitter.com/1.1/statuses/filter.json");
    $request.Method = "POST";
    $request.Headers.Add("Authorization", $oauth_authorization);
    $request.ContentType = "application/x-www-form-urlencoded";
    $body = $request.GetRequestStream();

    $body.write($post_body, 0, $post_body.length);
    $body.flush();
    $body.close();
    $response = $request.GetResponse() ;

    Write-Host "Start stream reading ..." -ForegroundColor Green

    Write-Host "Define a MemoryStream and a StreamWriter for writing ..." -ForegroundColor Green
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream

    $sReader = New-Object System.IO.StreamReader($response.GetResponseStream())

    $inrec = $sReader.ReadLine()
    $count = 0
    while (($inrec -ne $null) -and ($count -le $lineMax))
    {
        if ($inrec -ne "")
        {
            Write-Host "`n`t $count tweets received." -ForegroundColor Yellow

            $writeStream.WriteLine($inrec)
            $count ++
        }

        $inrec=$sReader.ReadLine()
    }
    #endregion

    #region - Write tweets tooBlob storage
    Write-Host "Write toohello destination blob ..." -ForegroundColor Green
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)

    $sReader.close()
    #endregion

    Write-Host "Completed!" -ForegroundColor Green
    ```

3. Nastavte hello prvních pět tooeight proměnné ve skriptu hello:

    Proměnná|Popis
    ---|---
    $clusterName|Toto je název clusteru HDInsight hello místo toorun hello aplikace hello.
    $oauth_consumer_key|Toto je aplikace hello Twitter **uživatelský klíč** jste si poznamenali dříve při vytváření aplikace hello Twitter.
    $oauth_consumer_secret|Toto je aplikace hello Twitter **uživatelský tajný klíč** jste si poznamenali dříve.
    $oauth_token|Toto je aplikace hello Twitter **přístupový token** jste si poznamenali dříve.
    $oauth_token_secret|Toto je aplikace hello Twitter **tajný klíč přístupového tokenu** jste si poznamenali dříve.
    $destBlobName|Toto je název objektu blob výstup hello. Hello výchozí hodnota je **tutorials/twitter/data/tweets.txt**. Pokud změníte výchozí hodnotu hello, budete potřebovat skriptů prostředí Windows PowerShell hello tooupdate odpovídajícím způsobem.
    $trackString|Webová služba Hello vrátí související toothese tweetů klíčová slova. Hello výchozí hodnota je **Azure Cloud, HDInsight**. Pokud změníte výchozí hodnotu hello, aktualizujte skriptů prostředí Windows PowerShell hello odpovídajícím způsobem.
    $lineMax|Hello hodnota určuje, kolik skriptu hello tweetů, bude číst. Trvá přibližně tři minuty tooread 100 tweetů. Můžete nastavit vyšší číslo, ale bude trvat další toodownload čas.

1. Stiskněte klávesu **F5** toorun hello skriptu. Pokud narazíte na problémy, jako řešení, vyberte všechny řádky hello a potom stiskněte klávesu **F8**.
2. Zobrazí se "Dokončit!" na konci hello hello výstupu. Chybové zprávy, se zobrazí červeně.

Jako postup ověření, můžete zkontrolovat soubor výstup hello, **/tutorials/twitter/data/tweets.txt**, ve službě Azure Blob storage pomocí Průzkumníka úložiště Azure nebo Azure PowerShell. Ukázkový skript prostředí Windows PowerShell pro výpis soubory, najdete v části [používání Blob storage s HDInsight][hdinsight-storage-powershell].

## <a name="create-hiveql-script"></a>Vytvořit skript HiveQL
Pomocí Azure PowerShell, můžete spustit více příkazy HiveQL jeden na čas, nebo balíček hello HiveQL příkaz do souboru skriptu. V tomto kurzu vytvoříte skript HiveQL. soubor skriptu Hello musí být nahrané tooAzure úložiště objektů Blob. V další části hello bude spuštěn soubor skriptu hello pomocí prostředí Azure PowerShell.

> [!NOTE]
> soubor skriptu Hive Hello a soubor obsahující 10 000 tweetů byly odeslány ve veřejném kontejneru Blob. Pokud chcete soubory toouse hello nahrát, můžete tuto část přeskočit.

Hello skript HiveQL provede hello následující:

1. **Odpojit tabulku tweets_raw hello** v případě, že hello tabulka již existuje.
2. **Vytvoří tabulku Hive tweets_raw hello**. Tato dočasná tabulka strukturovaných Hive obsahuje hello data pro další extrakce, transformace a načítání (ETL) zpracování. Informace o oddílech najdete v tématu [Hive kurzu][apache-hive-tutorial].
3. **Načtení dat** ze hello zdrojové složky /tutorials/twitter/data. datovou sadu tweetů velké Hello ve vnořených formátu JSON má nyní převedeny na dočasné struktura tabulky Hive.
4. **Vyřaďte hello tweetů tabulky** v případě, že hello tabulka již existuje.
5. **Vytvoření tabulky tweetů hello**. Než proti datovou sadu tweetů hello se můžete dotazovat pomocí Hive, musíte toorun jiný proces ETL. Tento proces ETL definuje podrobnější schématu tabulky pro hello data, která máte uložená v tabulce "twitter_raw" hello.
6. **Vložit přepsat tabulku**. Tento komplexní skript Hive se ji sadu dlouho úloh MapReduce clusterem Hadoop hello. V závislosti na vaší datovou sadu a hello velikost clusteru to může trvat asi 10 minut.
7. **Vložení přepsat directory**. Spusťte soubor tooa datovou sadu dotaz a výstup hello. Tento dotaz vrátí seznam Twitter uživatelů, kteří odeslané většina tweetů, které obsahovaly hello slovo "Azure".

**toocreate podregistru skript a nahrajte ho tooAzure**

1. Otevřete Windows PowerShell ISE.
2. Zkopírujte následující skript do podokno skriptu hello hello:

    ```powershell
    #region - variables and constants
    $clusterName = "<Existing HDInsight Cluster Name>" # Enter your HDInsight cluster name
    $subscriptionID = "<Azure Subscription ID>"

    $sourceDataPath = "/tutorials/twitter/data"
    $outputPath = "/tutorials/twitter/output"
    $hqlScriptFile = "tutorials/twitter/twitter.hql"

    $hqlStatements = @"
    set hive.exec.dynamic.partition = true;
    set hive.exec.dynamic.partition.mode = nonstrict;

    DROP TABLE tweets_raw;
    CREATE EXTERNAL TABLE tweets_raw (
        json_response STRING
    )
    STORED AS TEXTFILE LOCATION '$sourceDataPath';

    DROP TABLE tweets;
    CREATE TABLE tweets
    (
        id BIGINT,
        created_at STRING,
        created_at_date STRING,
        created_at_year STRING,
        created_at_month STRING,
        created_at_day STRING,
        created_at_time STRING,
        in_reply_to_user_id_str STRING,
        text STRING,
        contributors STRING,
        retweeted STRING,
        truncated STRING,
        coordinates STRING,
        source STRING,
        retweet_count INT,
        url STRING,
        hashtags array<STRING>,
        user_mentions array<STRING>,
        first_hashtag STRING,
        first_user_mention STRING,
        screen_name STRING,
        name STRING,
        followers_count INT,
        listed_count INT,
        friends_count INT,
        lang STRING,
        user_location STRING,
        time_zone STRING,
        profile_image_url STRING,
        json_response STRING
    );

    FROM tweets_raw
    INSERT OVERWRITE TABLE tweets
    SELECT
        cast(get_json_object(json_response, '$.id_str') as BIGINT),
        get_json_object(json_response, '$.created_at'),
        concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
        substr (get_json_object(json_response, '$.created_at'),27,4)),
        substr (get_json_object(json_response, '$.created_at'),27,4),
        case substr (get_json_object(json_response, '$.created_at'),5,3)
            when "Jan" then "01"
            when "Feb" then "02"
            when "Mar" then "03"
            when "Apr" then "04"
            when "May" then "05"
            when "Jun" then "06"
            when "Jul" then "07"
            when "Aug" then "08"
            when "Sep" then "09"
            when "Oct" then "10"
            when "Nov" then "11"
            when "Dec" then "12" end,
        substr (get_json_object(json_response, '$.created_at'),9,2),
        substr (get_json_object(json_response, '$.created_at'),12,8),
        get_json_object(json_response, '$.in_reply_to_user_id_str'),
        get_json_object(json_response, '$.text'),
        get_json_object(json_response, '$.contributors'),
        get_json_object(json_response, '$.retweeted'),
        get_json_object(json_response, '$.truncated'),
        get_json_object(json_response, '$.coordinates'),
        get_json_object(json_response, '$.source'),
        cast (get_json_object(json_response, '$.retweet_count') as INT),
        get_json_object(json_response, '$.entities.display_url'),
        array(
            trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[1].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[2].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[3].text'))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[4].text')))),
        array(
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[1].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[2].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[3].screen_name'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[4].screen_name')))),
        trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
        trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
        get_json_object(json_response, '$.user.screen_name'),
        get_json_object(json_response, '$.user.name'),
        cast (get_json_object(json_response, '$.user.followers_count') as INT),
        cast (get_json_object(json_response, '$.user.listed_count') as INT),
        cast (get_json_object(json_response, '$.user.friends_count') as INT),
        get_json_object(json_response, '$.user.lang'),
        get_json_object(json_response, '$.user.location'),
        get_json_object(json_response, '$.user.time_zone'),
        get_json_object(json_response, '$.user.profile_image_url'),
        json_response
    WHERE (length(json_response) > 500);

    INSERT OVERWRITE DIRECTORY '$outputPath'
    SELECT name, screen_name, count(1) as cc
        FROM tweets
        WHERE text like "%Azure%"
        GROUP BY name,screen_name
        ORDER BY cc DESC LIMIT 10;
    "@
    #endregion

    #region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green

    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }

    Select-AzureRmSubscription -SubscriptionId $subscriptionID

    #endregion

    #region - Create a block blob object for writing hello Hive script file
    Write-Host "Get hello default storage account name and container name based on hello cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow

    Write-Host "Define hello connection string ..." -ForegroundColor Green
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"

    Write-Host "Create block blob objects referencing hello hql script file" -ForegroundColor Green
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $hqlScriptBlob = $storageContainer.GetBlockBlobReference($hqlScriptFile)

    Write-Host "Define a MemoryStream and a StreamWriter for writing ... " -ForegroundColor Green
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream
    $writeStream.Writeline($hqlStatements)
    #endregion

    #region - Write hello Hive script file tooBlob storage
    Write-Host "Write toohello destination blob ... " -ForegroundColor Green
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $hqlScriptBlob.UploadFromStream($memStream)
    #endregion

    Write-Host "Completed!" -ForegroundColor Green
    ```

3. Nastavte hello první dvě proměnné ve skriptu hello:

   | Proměnná | Popis |
   | --- | --- |
   |  $clusterName |Zadejte název clusteru HDInsight hello místo toorun hello aplikace. |
   |  $subscriptionID |Zadejte ID vašeho předplatného Azure. |
   |  $sourceDataPath |Hello hello dotazů Hive, kde bude číst hello data z umístění úložiště objektů Blob v Azure. Tato proměnná nepotřebujete toochange. |
   |  $outputPath |Hello umístění úložiště objektů Blob v Azure, kde dotazů Hive hello výstup hello výsledky. Tato proměnná nepotřebujete toochange. |
   |  $hqlScriptFile |Hello umístění a název souboru hello soubor skriptu HiveQL hello. Tato proměnná nepotřebujete toochange. |
4. Stiskněte klávesu **F5** toorun hello skriptu. Pokud narazíte na problémy, jako řešení, vyberte všechny řádky hello a potom stiskněte klávesu **F8**.
5. Zobrazí se "Dokončit!" na konci hello hello výstupu. Chybové zprávy, se zobrazí červeně.

Jako postup ověření, můžete zkontrolovat soubor výstup hello, **/tutorials/twitter/twitter.hql**, ve službě Azure Blob storage pomocí Průzkumníka úložiště Azure nebo Azure PowerShell. Ukázkový skript prostředí Windows PowerShell pro výpis soubory, najdete v části [používání Blob storage s HDInsight][hdinsight-storage-powershell].

## <a name="process-twitter-data-by-using-hive"></a>Zpracování dat Twitteru pomocí Hive
Dokončení veškeré práce přípravy hello. Nyní můžete vyvolat hello skriptu Hive a hello výsledky kontroly.

### <a name="submit-a-hive-job"></a>Odeslání úlohy Hive
Použijte následující skript prostředí Windows PowerShell, toorun skriptu Hive hello hello. Budete potřebovat tooset hello první proměnné.

> [!NOTE]
> toouse hello tweetů a HiveQL skript, který jste nahráli v posledních dvou částech hello sadu $hqlScriptFile too"/tutorials/twitter/twitter.hql hello". hello toouse ty, které byly odeslány tooa veřejného objektu blob pro vás, nastavte $hqlScriptFile příliš"wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".

```powershell
#region variables and constants
$clusterName = "<Existing Azure HDInsight Cluster Name>"
$httpUserName = "admin"
$httpUserPassword = "<HDInsight Cluster HTTP User Password>"

#use one of hello following
$hqlScriptFile = "wasb://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql"
$hqlScriptFile = "/tutorials/twitter/twitter.hql"

$statusFolder = "/tutorials/twitter/jobstatus"
#endregion

$myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $myCluster.ResourceGroup
$defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
$defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value

$defaultBlobContainerName = $myCluster.DefaultStorageContainer

#region - Invoke Hive
Write-Host "Invoke Hive ... " -ForegroundColor Green

# Create hello HDInsight cluster
$pw = ConvertTo-SecureString -String $httpUserPassword -AsPlainText -Force
$httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

Use-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $httpCredential
$response = Invoke-AzureRmHDInsightHiveJob -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -file $hqlScriptFile -StatusFolder $statusFolder #-OutVariable $outVariable

Write-Host "Display hello standard error log ... " -ForegroundColor Green
$jobID = ($response | Select-String job_ | Select-Object -First 1) -replace ‘\s*$’ -replace ‘.*\s’
Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $jobID -DefaultContainer $defaultBlobContainerName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -HttpCredential $httpCredential
#endregion
```

### <a name="check-hello-results"></a>Výsledky kontroly hello
Použijte následující prostředí Windows PowerShell skriptu toocheck hello výstup úlohy Hive hello. Budete potřebovat tooset hello první dvě proměnné.

```powershell
#region variables and constants
$clusterName = "<Existing Azure HDInsight Cluster Name>"

$blob = "tutorials/twitter/output/000000_0" # hello name of hello blob toobe downloaded.
#endregion

#region - Create an Azure storage context object
Write-Host "Get hello default storage account name and container name based on hello cluster name ..." -ForegroundColor Green
$myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $myCluster.ResourceGroup
$defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
$defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
$defaultBlobContainerName = $myCluster.DefaultStorageContainer

Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow

Write-Host "Create a context object ... " -ForegroundColor Green
$storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
#endregion

#region - Download blob and display blob
Write-Host "Download hello blob ..." -ForegroundColor Green
cd $HOME
Get-AzureStorageBlobContent -Container $defaultBlobContainerName -Blob $blob -Context $storageContext -Force

Write-Host "Display hello output ..." -ForegroundColor Green
Write-Host "==================================" -ForegroundColor Green
cat "./$blob"
Write-Host "==================================" -ForegroundColor Green
#end region
```

> [!NOTE]
> Tabulka Hive Hello používá \001 jako hello oddělovačem polí. Oddělovač Hello není zobrazená v výstup hello.

Po výsledky analýzy hello byly umístěny do úložiště objektů Blob v Azure, můžete hello data tooan Azure SQL database nebo SQL server pro export, export tooExcel hello dat pomocí Power Query nebo připojení vašich dat toohello aplikace pomocí hello ovladače ODBC Hive. Další informace najdete v tématu [Sqoop použití s HDInsight][hdinsight-use-sqoop], [analýza letu zpoždění dat pomocí HDInsight][hdinsight-analyze-flight-delay-data], [ Připojení aplikace Excel tooHDInsight doplněk Power Query][hdinsight-power-query], a [tooHDInsight připojení aplikace Excel s hello ovladače ODBC Microsoft Hive][hdinsight-hive-odbc].

## <a name="next-steps"></a>Další kroky
V tomto kurzu jsme viděli, jak tootransform nestrukturovaných JSON datové sady do strukturovaných tooquery tabulku Hive zkoumat a analyzovat data z Twitteru pomocí HDInsight v Azure. toolearn více, najdete v části:

* [Začínáme s HDInsight][hdinsight-get-started]
* [Analýza v reálném čase sentimentu Twitter s HBase v HDInsight][hdinsight-hbase-twitter-sentiment]
* [Analýza dat zpoždění letu pomocí HDInsight][hdinsight-analyze-flight-delay-data]
* [Připojení aplikace Excel tooHDInsight doplněk Power Query][hdinsight-power-query]
* [Připojení aplikace Excel tooHDInsight s hello ovladače ODBC Microsoft Hive][hdinsight-hive-odbc]
* [Použití nástroje Sqoop s HDInsight][hdinsight-use-sqoop]

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176961.aspx

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]:hdinsight-hadoop-use-blob-storage.md#access-blobs-using-azure-powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
