---
title: "aaaAnalyze dat Twitteru pomocí Apache Hive - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toouse pomocí Hive a Hadoop na HDInsight tootransform nezpracovaná TWitter data do prohledávatelné tabulku Hive."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: e1e249ed-5f57-40d6-b3bc-a1b4d9a871d3
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 02c4d027c7bbf390ac1c3724c14f8d549ea5195e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-twitter-data-using-hive-and-hadoop-on-hdinsight"></a><span data-ttu-id="89332-103">Analýza dat Twitteru pomocí Hive a Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="89332-103">Analyze Twitter data using Hive and Hadoop on HDInsight</span></span>

<span data-ttu-id="89332-104">Zjistěte, jak toouse Apache Hive tooprocess dat Twitteru.</span><span class="sxs-lookup"><span data-stu-id="89332-104">Learn how toouse Apache Hive tooprocess Twitter data.</span></span> <span data-ttu-id="89332-105">Výsledkem Hello je seznam Twitter uživatelů, kteří odeslané hello většina tweetů, které obsahují určité slovo.</span><span class="sxs-lookup"><span data-stu-id="89332-105">hello result is a list of Twitter users who sent hello most tweets that contain a certain word.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="89332-106">na HDInsight 3.6 byly testovány Hello kroky v tomto dokumentu.</span><span class="sxs-lookup"><span data-stu-id="89332-106">hello steps in this document were tested on HDInsight 3.6.</span></span>
>
> <span data-ttu-id="89332-107">Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="89332-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="89332-108">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="89332-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="get-hello-data"></a><span data-ttu-id="89332-109">Získat hello data</span><span class="sxs-lookup"><span data-stu-id="89332-109">Get hello data</span></span>

<span data-ttu-id="89332-110">Twitter vám umožní tooretrieve hello [data pro každou tweet](https://dev.twitter.com/docs/platform-objects/tweets) jako dokument JavaScript Object Notation (JSON) přes REST API.</span><span class="sxs-lookup"><span data-stu-id="89332-110">Twitter allows you tooretrieve hello [data for each tweet](https://dev.twitter.com/docs/platform-objects/tweets) as a JavaScript Object Notation (JSON) document through a REST API.</span></span> <span data-ttu-id="89332-111">[OAuth](http://oauth.net) je vyžadován pro ověření toohello rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="89332-111">[OAuth](http://oauth.net) is required for authentication toohello API.</span></span>

### <a name="create-a-twitter-application"></a><span data-ttu-id="89332-112">Vytvořit aplikaci služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="89332-112">Create a Twitter application</span></span>

1. <span data-ttu-id="89332-113">Z webového prohlížeče, přihlaste se příliš[https://apps.twitter.com/](https://apps.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="89332-113">From a web browser, sign in too[https://apps.twitter.com/](https://apps.twitter.com/).</span></span> <span data-ttu-id="89332-114">Klikněte na tlačítko hello **registrace nyní** odkaz, pokud nemáte účet služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="89332-114">Click hello **Sign-up now** link if you don't have a Twitter account.</span></span>

2. <span data-ttu-id="89332-115">Klikněte na tlačítko **vytvořte novou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="89332-115">Click **Create New App**.</span></span>

3. <span data-ttu-id="89332-116">Zadejte **název**, **popis**, **webu**.</span><span class="sxs-lookup"><span data-stu-id="89332-116">Enter **Name**, **Description**, **Website**.</span></span> <span data-ttu-id="89332-117">Můžete použít adresu URL pro hello **webu** pole.</span><span class="sxs-lookup"><span data-stu-id="89332-117">You can make up a URL for hello **Website** field.</span></span> <span data-ttu-id="89332-118">Hello následující tabulka uvádí některé ukázkové hodnoty toouse:</span><span class="sxs-lookup"><span data-stu-id="89332-118">hello following table shows some sample values toouse:</span></span>

   | <span data-ttu-id="89332-119">Pole</span><span class="sxs-lookup"><span data-stu-id="89332-119">Field</span></span> | <span data-ttu-id="89332-120">Hodnota</span><span class="sxs-lookup"><span data-stu-id="89332-120">Value</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="89332-121">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="89332-121">Name</span></span> |<span data-ttu-id="89332-122">MyHDInsightApp</span><span class="sxs-lookup"><span data-stu-id="89332-122">MyHDInsightApp</span></span> |
   | <span data-ttu-id="89332-123">Popis</span><span class="sxs-lookup"><span data-stu-id="89332-123">Description</span></span> |<span data-ttu-id="89332-124">MyHDInsightApp</span><span class="sxs-lookup"><span data-stu-id="89332-124">MyHDInsightApp</span></span> |
   | <span data-ttu-id="89332-125">Web</span><span class="sxs-lookup"><span data-stu-id="89332-125">Website</span></span> |<span data-ttu-id="89332-126">http://www.myhdinsightapp.com</span><span class="sxs-lookup"><span data-stu-id="89332-126">http://www.myhdinsightapp.com</span></span> |

4. <span data-ttu-id="89332-127">Zkontrolujte **souhlasím**a potom klikněte na **vytvořit aplikaci služby Twitter**.</span><span class="sxs-lookup"><span data-stu-id="89332-127">Check **Yes, I agree**, and then click **Create your Twitter application**.</span></span>

5. <span data-ttu-id="89332-128">Klikněte na tlačítko hello **oprávnění** kartě hello výchozí oprávnění je **jen pro čtení**.</span><span class="sxs-lookup"><span data-stu-id="89332-128">Click hello **Permissions** tab. hello default permission is **Read only**.</span></span>

6. <span data-ttu-id="89332-129">Klikněte na tlačítko hello **klíče a přístupové tokeny** kartě.</span><span class="sxs-lookup"><span data-stu-id="89332-129">Click hello **Keys and Access Tokens** tab.</span></span>

7. <span data-ttu-id="89332-130">Klikněte na tlačítko **vytvořit můj přístupový token**.</span><span class="sxs-lookup"><span data-stu-id="89332-130">Click **Create my access token**.</span></span>

8. <span data-ttu-id="89332-131">Klikněte na tlačítko **testovací OAuth** v pravém horním rohu hello hello stránky.</span><span class="sxs-lookup"><span data-stu-id="89332-131">Click **Test OAuth** in hello upper-right corner of hello page.</span></span>

9. <span data-ttu-id="89332-132">Zapište **uživatelský klíč**, **uživatelský tajný klíč**, **přístupový token**, a **tajný klíč přístupového tokenu**.</span><span class="sxs-lookup"><span data-stu-id="89332-132">Write down **consumer key**, **Consumer secret**, **Access token**, and **Access token secret**.</span></span>

### <a name="download-tweets"></a><span data-ttu-id="89332-133">Stáhnout tweetů</span><span class="sxs-lookup"><span data-stu-id="89332-133">Download tweets</span></span>

<span data-ttu-id="89332-134">Následující kód Python Hello stáhne 10 000 tweetů z Twitteru a uložte je tooa soubor s názvem **tweets.txt**.</span><span class="sxs-lookup"><span data-stu-id="89332-134">hello following Python code downloads 10,000 tweets from Twitter and save them tooa file named **tweets.txt**.</span></span>

> [!NOTE]
> <span data-ttu-id="89332-135">Hello následující kroky jsou v clusteru HDInsight hello provést, protože je již nainstalován jazyk Python.</span><span class="sxs-lookup"><span data-stu-id="89332-135">hello following steps are performed on hello HDInsight cluster, since Python is already installed.</span></span>

1. <span data-ttu-id="89332-136">Připojte toohello clusteru HDInsight pomocí protokolu SSH:</span><span class="sxs-lookup"><span data-stu-id="89332-136">Connect toohello HDInsight cluster using SSH:</span></span>

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="89332-137">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="89332-137">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

3. <span data-ttu-id="89332-138">Použití hello následující příkazy tooinstall [Tweepy](http://www.tweepy.org/), [Progressbar](https://pypi.python.org/pypi/progressbar/2.2)a další požadované balíčky:</span><span class="sxs-lookup"><span data-stu-id="89332-138">Use hello following commands tooinstall [Tweepy](http://www.tweepy.org/), [Progressbar](https://pypi.python.org/pypi/progressbar/2.2), and other required packages:</span></span>

   ```bash
   sudo apt install python-dev libffi-dev libssl-dev
   sudo apt remove python-openssl
   pip install virtualenv
   mkdir gettweets
   cd gettweets
   virtualenv gettweets
   source gettweets/bin/activate
   pip install tweepy progressbar pyOpenSSL requests[security]
   ```

4. <span data-ttu-id="89332-139">Použití hello následující příkaz toocreate soubor s názvem **gettweets.py**:</span><span class="sxs-lookup"><span data-stu-id="89332-139">Use hello following command toocreate a file named **gettweets.py**:</span></span>

   ```bash
   nano gettweets.py
   ```

5. <span data-ttu-id="89332-140">Použití hello následující text jako hello obsah hello **gettweets.py** souboru:</span><span class="sxs-lookup"><span data-stu-id="89332-140">Use hello following text as hello contents of hello **gettweets.py** file:</span></span>

   ```python
   #!/usr/bin/python

   from tweepy import Stream, OAuthHandler
   from tweepy.streaming import StreamListener
   from progressbar import ProgressBar, Percentage, Bar
   import json
   import sys

   #Twitter app information
   consumer_secret='Your consumer secret'
   consumer_key='Your consumer key'
   access_token='Your access token'
   access_token_secret='Your access token secret'

   #hello number of tweets we want tooget
   max_tweets=10000

   #Create hello listener class that receives and saves tweets
   class listener(StreamListener):
       #On init, set hello counter toozero and create a progress bar
       def __init__(self, api=None):
           self.num_tweets = 0
           self.pbar = ProgressBar(widgets=[Percentage(), Bar()], maxval=max_tweets).start()

       #When data is received, do this
       def on_data(self, data):
           #Append hello tweet toohello 'tweets.txt' file
           with open('tweets.txt', 'a') as tweet_file:
               tweet_file.write(data)
               #Increment hello number of tweets
               self.num_tweets += 1
               #Check toosee if we have hit max_tweets and exit if so
               if self.num_tweets >= max_tweets:
                   self.pbar.finish()
                   sys.exit(0)
               else:
                   #increment hello progress bar
                   self.pbar.update(self.num_tweets)
           return True

       #Handle any errors that may occur
       def on_error(self, status):
           print status

   #Get hello OAuth token
   auth = OAuthHandler(consumer_key, consumer_secret)
   auth.set_access_token(access_token, access_token_secret)
   #Use hello listener class for stream processing
   twitterStream = Stream(auth, listener())
   #Filter for these topics
   twitterStream.filter(track=["azure","cloud","hdinsight"])
   ```

    > [!IMPORTANT]
    > <span data-ttu-id="89332-141">Nahraďte zástupný text hello pro následující položky, které hello informace z vaší aplikace twitter hello:</span><span class="sxs-lookup"><span data-stu-id="89332-141">Replace hello placeholder text for hello following items with hello information from your twitter application:</span></span>
    >
    > * `consumer_secret`
    > * `consumer_key`
    > * `access_token`
    > * `access_token_secret`

6. <span data-ttu-id="89332-142">Použití **kombinaci kláves Ctrl + X**, pak **Y** toosave hello souboru.</span><span class="sxs-lookup"><span data-stu-id="89332-142">Use **Ctrl + X**, then **Y** toosave hello file.</span></span>

7. <span data-ttu-id="89332-143">Použijte následující příkaz toorun hello soubor hello a stáhnout tweetů:</span><span class="sxs-lookup"><span data-stu-id="89332-143">Use hello following command toorun hello file and download tweets:</span></span>

    ```bash
    python gettweets.py
    ```

    <span data-ttu-id="89332-144">Se zobrazí indikátor průběhu.</span><span class="sxs-lookup"><span data-stu-id="89332-144">A progress indicator appears.</span></span> <span data-ttu-id="89332-145">% Too100 počítá jako hello tweetů staženy.</span><span class="sxs-lookup"><span data-stu-id="89332-145">It counts up too100% as hello tweets are downloaded.</span></span>

   > [!NOTE]
   > <span data-ttu-id="89332-146">Pokud to trvá dlouho hello průběh panelu tooadvance, měli byste změnit hello filtru tootrack trendů témata.</span><span class="sxs-lookup"><span data-stu-id="89332-146">If it is taking a long time for hello progress bar tooadvance, you should change hello filter tootrack trending topics.</span></span> <span data-ttu-id="89332-147">Pokud nejsou k dispozici mnoho tweety o hello téma do filtru, můžete rychle získat hello 10000 tweetů potřeby.</span><span class="sxs-lookup"><span data-stu-id="89332-147">When there are many tweets about hello topic in your filter, you can quickly get hello 10000 tweets needed.</span></span>

### <a name="upload-hello-data"></a><span data-ttu-id="89332-148">Nahrání dat hello</span><span class="sxs-lookup"><span data-stu-id="89332-148">Upload hello data</span></span>

<span data-ttu-id="89332-149">tooupload hello tooHDInsight úložiště dat, hello použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="89332-149">tooupload hello data tooHDInsight storage, use hello following commands:</span></span>

   ```bash
   hdfs dfs -mkdir -p /tutorials/twitter/data
   hdfs dfs -put tweets.txt /tutorials/twitter/data/tweets.txt
```

<span data-ttu-id="89332-150">Tyto příkazy hello data uložit na místo, kterému mají přístup všechny uzly v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="89332-150">These commands store hello data in a location that all nodes in hello cluster can access.</span></span>

## <a name="run-hello-hiveql-job"></a><span data-ttu-id="89332-151">Spustit úlohu HiveQL hello</span><span class="sxs-lookup"><span data-stu-id="89332-151">Run hello HiveQL job</span></span>

1. <span data-ttu-id="89332-152">Použijte následující příkaz toocreate souboru, který obsahuje příkazy HiveQL hello:</span><span class="sxs-lookup"><span data-stu-id="89332-152">Use hello following command toocreate a file containing HiveQL statements:</span></span>

   ```bash
   nano twitter.hql
   ```

    <span data-ttu-id="89332-153">Použijte následující text jako hello obsah souboru hello hello:</span><span class="sxs-lookup"><span data-stu-id="89332-153">Use hello following text as hello contents of hello file:</span></span>

   ```hiveql
   set hive.exec.dynamic.partition = true;
   set hive.exec.dynamic.partition.mode = nonstrict;
   -- Drop table, if it exists
   DROP TABLE tweets_raw;
   -- Create it, pointing toward hello tweets logged from Twitter
   CREATE EXTERNAL TABLE tweets_raw (
       json_response STRING
   )
   STORED AS TEXTFILE LOCATION '/tutorials/twitter/data';
   -- Drop and recreate hello destination table
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
   -- Select tweets from hello imported data, parse hello JSON,
   -- and insert into hello tweets table
   FROM tweets_raw
   INSERT OVERWRITE TABLE tweets
   SELECT
       cast(get_json_object(json_response, '$.id_str') as BIGINT),
       get_json_object(json_response, '$.created_at'),
       concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
       substr (get_json_object(json_response, '$.created_at'),27,4)),
       substr (get_json_object(json_response, '$.created_at'),27,4),
       case substr (get_json_object(json_response,    '$.created_at'),5,3)
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
   ```

2. <span data-ttu-id="89332-154">Stiskněte klávesu **kombinaci kláves Ctrl + X**, stiskněte **Y** toosave hello souboru.</span><span class="sxs-lookup"><span data-stu-id="89332-154">Press **Ctrl + X**, then press **Y** toosave hello file.</span></span>
3. <span data-ttu-id="89332-155">Použijte následující příkaz toorun hello HiveQL obsažené v souboru hello hello:</span><span class="sxs-lookup"><span data-stu-id="89332-155">Use hello following command toorun hello HiveQL contained in hello file:</span></span>

   ```bash
   beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -i twitter.hql
   ```

    <span data-ttu-id="89332-156">Tento příkaz spustí hello hello **twitter.hql** souboru.</span><span class="sxs-lookup"><span data-stu-id="89332-156">This command runs hello hello **twitter.hql** file.</span></span> <span data-ttu-id="89332-157">Po dokončení dotazu hello zobrazí `jdbc:hive2//localhost:10001/>` řádku.</span><span class="sxs-lookup"><span data-stu-id="89332-157">Once hello query completes, you see a `jdbc:hive2//localhost:10001/>` prompt.</span></span>

4. <span data-ttu-id="89332-158">Hello beeline řádku můžete hello následující dotaz tooverify importu dat:</span><span class="sxs-lookup"><span data-stu-id="89332-158">From hello beeline prompt, use hello following query tooverify that data was imported:</span></span>

   ```hiveql
   SELECT name, screen_name, count(1) as cc
       FROM tweets
       WHERE text like "%Azure%"
       GROUP BY name,screen_name
       ORDER BY cc DESC LIMIT 10;
   ```

    <span data-ttu-id="89332-159">Tento dotaz vrací maximálně 10 tweetů, které obsahují slovo hello **Azure** v hello text zprávy.</span><span class="sxs-lookup"><span data-stu-id="89332-159">This query returns a maximum of 10 tweets that contain hello word **Azure** in hello message text.</span></span>

## <a name="next-steps"></a><span data-ttu-id="89332-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="89332-160">Next steps</span></span>

<span data-ttu-id="89332-161">Jste se naučili, jak tootransform nestrukturovaných JSON datové sady do strukturovaných tabulku Hive.</span><span class="sxs-lookup"><span data-stu-id="89332-161">You have learned how tootransform an unstructured JSON dataset into a structured Hive table.</span></span> <span data-ttu-id="89332-162">toolearn Další informace o Hive v HDInsight, najdete v části hello následující dokumenty:</span><span class="sxs-lookup"><span data-stu-id="89332-162">toolearn more about Hive on HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="89332-163">Začínáme s HDInsight</span><span class="sxs-lookup"><span data-stu-id="89332-163">Get started with HDInsight</span></span>](hdinsight-hadoop-linux-tutorial-get-started.md)
* [<span data-ttu-id="89332-164">Analýza dat zpoždění letu pomocí HDInsight</span><span class="sxs-lookup"><span data-stu-id="89332-164">Analyze flight delay data using HDInsight</span></span>](hdinsight-analyze-flight-delay-data-linux.md)

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter
