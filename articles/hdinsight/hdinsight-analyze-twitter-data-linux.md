---
title: "Analýza dat Twitteru pomocí Apache Hive - Azure HDInsight | Microsoft Docs"
description: "Další informace o použití použití Hive a Hadoop v HDInsight k transformaci dat ve formátu raw TWitter do prohledávatelné tabulku Hive."
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
ms.openlocfilehash: b8656123fa9c5158f366872ab050f370080ec18a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="analyze-twitter-data-using-hive-and-hadoop-on-hdinsight"></a><span data-ttu-id="07933-103">Analýza dat Twitteru pomocí Hive a Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="07933-103">Analyze Twitter data using Hive and Hadoop on HDInsight</span></span>

<span data-ttu-id="07933-104">Naučte se používat Apache Hive ke zpracování dat služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="07933-104">Learn how to use Apache Hive to process Twitter data.</span></span> <span data-ttu-id="07933-105">Výsledkem je seznam Twitter uživatelů, kteří odeslané většina tweetů, které obsahují určité slovo.</span><span class="sxs-lookup"><span data-stu-id="07933-105">The result is a list of Twitter users who sent the most tweets that contain a certain word.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="07933-106">Kroky v tomto dokumentu byly testovány na HDInsight 3.6.</span><span class="sxs-lookup"><span data-stu-id="07933-106">The steps in this document were tested on HDInsight 3.6.</span></span>
>
> <span data-ttu-id="07933-107">HDInsight od verze 3.4 výše používá výhradně operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="07933-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="07933-108">Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="07933-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="get-the-data"></a><span data-ttu-id="07933-109">Získání dat</span><span class="sxs-lookup"><span data-stu-id="07933-109">Get the data</span></span>

<span data-ttu-id="07933-110">Twitter umožňuje načíst [data pro každou tweet](https://dev.twitter.com/docs/platform-objects/tweets) jako dokument JavaScript Object Notation (JSON) přes REST API.</span><span class="sxs-lookup"><span data-stu-id="07933-110">Twitter allows you to retrieve the [data for each tweet](https://dev.twitter.com/docs/platform-objects/tweets) as a JavaScript Object Notation (JSON) document through a REST API.</span></span> <span data-ttu-id="07933-111">[OAuth](http://oauth.net) je vyžadován pro ověření rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="07933-111">[OAuth](http://oauth.net) is required for authentication to the API.</span></span>

### <a name="create-a-twitter-application"></a><span data-ttu-id="07933-112">Vytvořit aplikaci služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="07933-112">Create a Twitter application</span></span>

1. <span data-ttu-id="07933-113">Z webového prohlížeče, přihlaste se k [https://apps.twitter.com/](https://apps.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="07933-113">From a web browser, sign in to [https://apps.twitter.com/](https://apps.twitter.com/).</span></span> <span data-ttu-id="07933-114">Klikněte na tlačítko **registrace nyní** odkaz, pokud nemáte účet služby Twitter.</span><span class="sxs-lookup"><span data-stu-id="07933-114">Click the **Sign-up now** link if you don't have a Twitter account.</span></span>

2. <span data-ttu-id="07933-115">Klikněte na tlačítko **vytvořte novou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="07933-115">Click **Create New App**.</span></span>

3. <span data-ttu-id="07933-116">Zadejte **název**, **popis**, **webu**.</span><span class="sxs-lookup"><span data-stu-id="07933-116">Enter **Name**, **Description**, **Website**.</span></span> <span data-ttu-id="07933-117">Můžete použít pro adresu URL **webu** pole.</span><span class="sxs-lookup"><span data-stu-id="07933-117">You can make up a URL for the **Website** field.</span></span> <span data-ttu-id="07933-118">Následující tabulka uvádí některé ukázkové hodnoty použít:</span><span class="sxs-lookup"><span data-stu-id="07933-118">The following table shows some sample values to use:</span></span>

   | <span data-ttu-id="07933-119">Pole</span><span class="sxs-lookup"><span data-stu-id="07933-119">Field</span></span> | <span data-ttu-id="07933-120">Hodnota</span><span class="sxs-lookup"><span data-stu-id="07933-120">Value</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="07933-121">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="07933-121">Name</span></span> |<span data-ttu-id="07933-122">MyHDInsightApp</span><span class="sxs-lookup"><span data-stu-id="07933-122">MyHDInsightApp</span></span> |
   | <span data-ttu-id="07933-123">Popis</span><span class="sxs-lookup"><span data-stu-id="07933-123">Description</span></span> |<span data-ttu-id="07933-124">MyHDInsightApp</span><span class="sxs-lookup"><span data-stu-id="07933-124">MyHDInsightApp</span></span> |
   | <span data-ttu-id="07933-125">Web</span><span class="sxs-lookup"><span data-stu-id="07933-125">Website</span></span> |<span data-ttu-id="07933-126">http://www.myhdinsightapp.com</span><span class="sxs-lookup"><span data-stu-id="07933-126">http://www.myhdinsightapp.com</span></span> |

4. <span data-ttu-id="07933-127">Zkontrolujte **souhlasím**a potom klikněte na **vytvořit aplikaci služby Twitter**.</span><span class="sxs-lookup"><span data-stu-id="07933-127">Check **Yes, I agree**, and then click **Create your Twitter application**.</span></span>

5. <span data-ttu-id="07933-128">Klikněte **oprávnění** kartě. Výchozí oprávnění je **jen pro čtení**.</span><span class="sxs-lookup"><span data-stu-id="07933-128">Click the **Permissions** tab. The default permission is **Read only**.</span></span>

6. <span data-ttu-id="07933-129">Klikněte **klíče a přístupové tokeny** kartě.</span><span class="sxs-lookup"><span data-stu-id="07933-129">Click the **Keys and Access Tokens** tab.</span></span>

7. <span data-ttu-id="07933-130">Klikněte na tlačítko **vytvořit můj přístupový token**.</span><span class="sxs-lookup"><span data-stu-id="07933-130">Click **Create my access token**.</span></span>

8. <span data-ttu-id="07933-131">Klikněte na tlačítko **testovací OAuth** v pravém horním rohu stránky.</span><span class="sxs-lookup"><span data-stu-id="07933-131">Click **Test OAuth** in the upper-right corner of the page.</span></span>

9. <span data-ttu-id="07933-132">Zapište **uživatelský klíč**, **uživatelský tajný klíč**, **přístupový token**, a **tajný klíč přístupového tokenu**.</span><span class="sxs-lookup"><span data-stu-id="07933-132">Write down **consumer key**, **Consumer secret**, **Access token**, and **Access token secret**.</span></span>

### <a name="download-tweets"></a><span data-ttu-id="07933-133">Stáhnout tweetů</span><span class="sxs-lookup"><span data-stu-id="07933-133">Download tweets</span></span>

<span data-ttu-id="07933-134">Následující kód Python stahování 10 000 tweetů z Twitteru a uložte je do souboru s názvem **tweets.txt**.</span><span class="sxs-lookup"><span data-stu-id="07933-134">The following Python code downloads 10,000 tweets from Twitter and save them to a file named **tweets.txt**.</span></span>

> [!NOTE]
> <span data-ttu-id="07933-135">Následující kroky jsou v clusteru HDInsight, provést, protože je již nainstalován jazyk Python.</span><span class="sxs-lookup"><span data-stu-id="07933-135">The following steps are performed on the HDInsight cluster, since Python is already installed.</span></span>

1. <span data-ttu-id="07933-136">Připojte se ke clusteru HDInsight pomocí protokolu SSH:</span><span class="sxs-lookup"><span data-stu-id="07933-136">Connect to the HDInsight cluster using SSH:</span></span>

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="07933-137">Další informace najdete v tématu [Použití SSH se službou HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="07933-137">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

3. <span data-ttu-id="07933-138">Použijte následující příkazy pro instalaci [Tweepy](http://www.tweepy.org/), [Progressbar](https://pypi.python.org/pypi/progressbar/2.2)a další požadované balíčky:</span><span class="sxs-lookup"><span data-stu-id="07933-138">Use the following commands to install [Tweepy](http://www.tweepy.org/), [Progressbar](https://pypi.python.org/pypi/progressbar/2.2), and other required packages:</span></span>

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

4. <span data-ttu-id="07933-139">Pomocí následujícího příkazu vytvořte soubor s názvem **gettweets.py**:</span><span class="sxs-lookup"><span data-stu-id="07933-139">Use the following command to create a file named **gettweets.py**:</span></span>

   ```bash
   nano gettweets.py
   ```

5. <span data-ttu-id="07933-140">Použít následující text jako obsah **gettweets.py** souboru:</span><span class="sxs-lookup"><span data-stu-id="07933-140">Use the following text as the contents of the **gettweets.py** file:</span></span>

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

   #The number of tweets we want to get
   max_tweets=10000

   #Create the listener class that receives and saves tweets
   class listener(StreamListener):
       #On init, set the counter to zero and create a progress bar
       def __init__(self, api=None):
           self.num_tweets = 0
           self.pbar = ProgressBar(widgets=[Percentage(), Bar()], maxval=max_tweets).start()

       #When data is received, do this
       def on_data(self, data):
           #Append the tweet to the 'tweets.txt' file
           with open('tweets.txt', 'a') as tweet_file:
               tweet_file.write(data)
               #Increment the number of tweets
               self.num_tweets += 1
               #Check to see if we have hit max_tweets and exit if so
               if self.num_tweets >= max_tweets:
                   self.pbar.finish()
                   sys.exit(0)
               else:
                   #increment the progress bar
                   self.pbar.update(self.num_tweets)
           return True

       #Handle any errors that may occur
       def on_error(self, status):
           print status

   #Get the OAuth token
   auth = OAuthHandler(consumer_key, consumer_secret)
   auth.set_access_token(access_token, access_token_secret)
   #Use the listener class for stream processing
   twitterStream = Stream(auth, listener())
   #Filter for these topics
   twitterStream.filter(track=["azure","cloud","hdinsight"])
   ```

    > [!IMPORTANT]
    > <span data-ttu-id="07933-141">Informace z vaší aplikace twitter nahraďte zástupný text pro následující položky:</span><span class="sxs-lookup"><span data-stu-id="07933-141">Replace the placeholder text for the following items with the information from your twitter application:</span></span>
    >
    > * `consumer_secret`
    > * `consumer_key`
    > * `access_token`
    > * `access_token_secret`

6. <span data-ttu-id="07933-142">Použití **kombinaci kláves Ctrl + X**, pak **Y** k uložení souboru.</span><span class="sxs-lookup"><span data-stu-id="07933-142">Use **Ctrl + X**, then **Y** to save the file.</span></span>

7. <span data-ttu-id="07933-143">Pomocí následujícího příkazu spusťte soubor a stáhnete tweetů:</span><span class="sxs-lookup"><span data-stu-id="07933-143">Use the following command to run the file and download tweets:</span></span>

    ```bash
    python gettweets.py
    ```

    <span data-ttu-id="07933-144">Se zobrazí indikátor průběhu.</span><span class="sxs-lookup"><span data-stu-id="07933-144">A progress indicator appears.</span></span> <span data-ttu-id="07933-145">Až o 100 % počítá jako tweetů staženy.</span><span class="sxs-lookup"><span data-stu-id="07933-145">It counts up to 100% as the tweets are downloaded.</span></span>

   > [!NOTE]
   > <span data-ttu-id="07933-146">Pokud to trvá dlouho indikátor průběhu posunut, měli byste změnit filtr pro sledování trendů témata.</span><span class="sxs-lookup"><span data-stu-id="07933-146">If it is taking a long time for the progress bar to advance, you should change the filter to track trending topics.</span></span> <span data-ttu-id="07933-147">Pokud nejsou k dispozici mnoho tweety o téma do filtru, můžete rychle získat 10000 tweetů potřeby.</span><span class="sxs-lookup"><span data-stu-id="07933-147">When there are many tweets about the topic in your filter, you can quickly get the 10000 tweets needed.</span></span>

### <a name="upload-the-data"></a><span data-ttu-id="07933-148">Odeslání dat</span><span class="sxs-lookup"><span data-stu-id="07933-148">Upload the data</span></span>

<span data-ttu-id="07933-149">Pokud chcete nahrát data do úložiště HDInsight, použijte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="07933-149">To upload the data to HDInsight storage, use the following commands:</span></span>

   ```bash
   hdfs dfs -mkdir -p /tutorials/twitter/data
   hdfs dfs -put tweets.txt /tutorials/twitter/data/tweets.txt
```

<span data-ttu-id="07933-150">Tyto příkazy ukládání dat v umístění, ke kterému mají přístup všechny uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="07933-150">These commands store the data in a location that all nodes in the cluster can access.</span></span>

## <a name="run-the-hiveql-job"></a><span data-ttu-id="07933-151">Spustit úlohu HiveQL</span><span class="sxs-lookup"><span data-stu-id="07933-151">Run the HiveQL job</span></span>

1. <span data-ttu-id="07933-152">K vytvoření souboru, který obsahuje příkazy HiveQL použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="07933-152">Use the following command to create a file containing HiveQL statements:</span></span>

   ```bash
   nano twitter.hql
   ```

    <span data-ttu-id="07933-153">Použijte následující text jako obsah souboru:</span><span class="sxs-lookup"><span data-stu-id="07933-153">Use the following text as the contents of the file:</span></span>

   ```hiveql
   set hive.exec.dynamic.partition = true;
   set hive.exec.dynamic.partition.mode = nonstrict;
   -- Drop table, if it exists
   DROP TABLE tweets_raw;
   -- Create it, pointing toward the tweets logged from Twitter
   CREATE EXTERNAL TABLE tweets_raw (
       json_response STRING
   )
   STORED AS TEXTFILE LOCATION '/tutorials/twitter/data';
   -- Drop and recreate the destination table
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
   -- Select tweets from the imported data, parse the JSON,
   -- and insert into the tweets table
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

2. <span data-ttu-id="07933-154">Stiskněte klávesu **kombinaci kláves Ctrl + X**, stiskněte **Y** k uložení souboru.</span><span class="sxs-lookup"><span data-stu-id="07933-154">Press **Ctrl + X**, then press **Y** to save the file.</span></span>
3. <span data-ttu-id="07933-155">Ke spuštění HiveQL obsažené v souboru použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="07933-155">Use the following command to run the HiveQL contained in the file:</span></span>

   ```bash
   beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -i twitter.hql
   ```

    <span data-ttu-id="07933-156">Tento příkaz spustí **twitter.hql** souboru.</span><span class="sxs-lookup"><span data-stu-id="07933-156">This command runs the the **twitter.hql** file.</span></span> <span data-ttu-id="07933-157">Po dokončení dotazu se zobrazí `jdbc:hive2//localhost:10001/>` řádku.</span><span class="sxs-lookup"><span data-stu-id="07933-157">Once the query completes, you see a `jdbc:hive2//localhost:10001/>` prompt.</span></span>

4. <span data-ttu-id="07933-158">Na příkazový řádek beeline ověřte, že importu dat pomocí následujícího dotazu:</span><span class="sxs-lookup"><span data-stu-id="07933-158">From the beeline prompt, use the following query to verify that data was imported:</span></span>

   ```hiveql
   SELECT name, screen_name, count(1) as cc
       FROM tweets
       WHERE text like "%Azure%"
       GROUP BY name,screen_name
       ORDER BY cc DESC LIMIT 10;
   ```

    <span data-ttu-id="07933-159">Tento dotaz vrací maximálně 10 tweetů, které obsahují slovo **Azure** v textu zprávy.</span><span class="sxs-lookup"><span data-stu-id="07933-159">This query returns a maximum of 10 tweets that contain the word **Azure** in the message text.</span></span>

## <a name="next-steps"></a><span data-ttu-id="07933-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="07933-160">Next steps</span></span>

<span data-ttu-id="07933-161">Jste se naučili postup transformace datové sadě služby nestrukturovaných JSON do strukturovaných tabulku Hive.</span><span class="sxs-lookup"><span data-stu-id="07933-161">You have learned how to transform an unstructured JSON dataset into a structured Hive table.</span></span> <span data-ttu-id="07933-162">Další informace o Hive v HDInsight, najdete v následujících dokumentech:</span><span class="sxs-lookup"><span data-stu-id="07933-162">To learn more about Hive on HDInsight, see the following documents:</span></span>

* [<span data-ttu-id="07933-163">Začínáme s HDInsight</span><span class="sxs-lookup"><span data-stu-id="07933-163">Get started with HDInsight</span></span>](hdinsight-hadoop-linux-tutorial-get-started.md)
* [<span data-ttu-id="07933-164">Analýza dat zpoždění letu pomocí HDInsight</span><span class="sxs-lookup"><span data-stu-id="07933-164">Analyze flight delay data using HDInsight</span></span>](hdinsight-analyze-flight-delay-data-linux.md)

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter
