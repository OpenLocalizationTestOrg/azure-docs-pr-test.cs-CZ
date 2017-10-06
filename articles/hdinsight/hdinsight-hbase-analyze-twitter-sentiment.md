---
title: "aaaAnalyze v reálném čase sentimentu Twitter s HBase - Azure | Microsoft Docs"
description: "Zjistěte, jak toodo v reálném čase postojích analýzy velkých objemů dat z Twitteru pomocí HBase v clusteru služby HDInsight (Hadoop)."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 5c798ad3-a20d-4385-a463-f4f7705f9566
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: jgao
ms.openlocfilehash: 87e5c0c0a90d222a3f0bc3c3f3fce1e938320480
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-real-time-twitter-sentiment-with-hbase-in-hdinsight"></a>Analýza v reálném čase sentimentu Twitter s HBase v HDInsight
Zjistěte, jak v reálném čase toodo [postojích analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) velkých objemů dat z Twitteru pomocí HBase cluster v HDInsight.

Sociální weby jsou jedním z hlavních řízení vynutí hello pro velké objemy dat přijetí. Veřejná rozhraní API poskytované lokality jako Twitter jsou užitečné zdroje dat pro analýzu a pochopení oblíbených trendy. V tomto kurzu budete vyvíjet konzoly streamování aplikace služby a ASP.NET webové aplikace tooperform hello následující:

![Postojích HDInsight HBase analyzovat Twitter.][img-app-arch]

* Hello streamování aplikací

  * získat označené geograficky tweetů v reálném čase pomocí služby Twitter hello streamování rozhraní API
  * vyhodnocení hello postojích z těchto tweetů
  * uložení hello postojích informace v HBase pomocí hello Microsoft HBase SDK
* Hello aplikace na weby Azure

  * vykreslení v reálném čase statistické výsledky hello na Bing maps pomocí webové aplikace ASP.NET. Vizualizaci tweetů hello je podobné toohello následující snímek obrazovky:

    ![hdinsight.hbase.Twitter.sentiment.Bing.map][img-bing-map]

    Budete mít tooquery tweetů s určitým tooget klíčová slova představu o Pokud stanovisko hello vyjádřené v hello tweetů kladné, záporné nebo neutrální.

Ucelenou ukázku řešení sady Visual Studio naleznete na webu GitHub: [aplikace analýzy v reálném čase sociálních postojích](https://github.com/maxluk/tweet-sentiment).

### <a name="prerequisites"></a>Požadavky
Než začnete tento kurz, musíte mít následující hello:

* **Cluster HBase v HDInsight**. Pokyny k vytváření clusterů, v tématu [Začínáme používat HBase s Hadoop v HDInsight][hbase-get-started]. 

* **Pracovní stanice** s Visual Studio 2013 nebo 2015 nebo 2017 nainstalována. Pokyny najdete v tématu [instalaci sady Visual Studio](http://msdn.microsoft.com/library/e2h7fzkw.aspx).

## <a name="create-a-twitter-application-id-and-secrets"></a>Vytvoření ID aplikace služby Twitter a tajné klíče
Hello Twitter, pomocí rozhraní API pro streamování [OAuth](http://oauth.net/) tooauthorize požadavky. první krok toouse Hello OAuth je toocreate novou aplikaci na webu pro vývojáře hello Twitter.

**ID aplikace Twitter toocreate a tajné klíče**

1. Přihlaste se příliš[Twitter aplikace](https://apps.twitter.com/). Klikněte na tlačítko hello **nyní** odkaz, pokud nemáte účet služby Twitter.
2. Klikněte na tlačítko **vytvořte novou aplikaci**.
3. Zadejte **název**, **popis**, a **webu**. název aplikace služby Twitter Hello musí být jedinečný název. pole webu Hello nepoužívá skutečně. Toobe nemá platnou adresu URL.
4. Zkontrolujte **souhlasím**a potom klikněte na **vytvořit aplikaci služby Twitter**.
5. Klikněte na tlačítko hello **oprávnění** a pak klikněte **jen pro čtení**. Hello oprávnění jen pro čtení je dostačující pro účely tohoto kurzu.
6. Klikněte na tlačítko hello **klíče a přístupové tokeny** kartě.
7. Klikněte na tlačítko **vytvořit můj přístupový token** na hello dolní části stránky hello.
9. Kopírování hello **uživatelský klíč (klíč rozhraní API)**, **uživatelský tajný klíč (tajný klíč rozhraní API)**, **přístupový token**, a **tajný klíč přístupového tokenu** hodnoty. Je nutné tyto hodnoty později v kurzu hello.

    > ! Tlačítko Test OAuth hello [poznámka] už nefunguje.

## <a name="create-twitter-streaming-service"></a>Vytvoření datových proudů služby Twitter
Je třeba toocreate tweetů tooget aplikaci, výpočtu skóre postojích tweet a odeslat tooHBase slova tweet hello zpracovat.

**hello toocreate streamování aplikací**

1. Otevřete **Visual Studio**a vytvořte konzolovou aplikaci Visual C# názvem **TweetSentimentStreaming**.
2. Z **Konzola správce balíčků**spusťte hello následující příkazy:

        Install-Package Microsoft.HBase.Client -version 0.4.2.0
        Install-Package TweetinviAPI -version 1.0.0.0

    Tyto příkazy instalace hello [HBase .NET SDK](https://www.nuget.org/packages/Microsoft.HBase.Client/) balíček, který je hello klienta knihovny tooaccess hello cluster HBase a hello [Tweetinvi API](https://www.nuget.org/packages/TweetinviAPI/) balíček, který je použité tooaccess hello rozhraní API služby Twitter.

   > [!NOTE]
   > Ukázka Hello používané v tomto článku byl testován pomocí verze hello uveden výše.  Můžete odebrat hello - verze přepínač tooinstall hello nejnovější verzi.
   >
   >
3. Z **Průzkumníku řešení**, přidejte **System.Configuration** toohello odkaz.
4. Přidat nový třída souboru toohello projekt s názvem **HBaseWriter.cs**a pak nahraďte kód hello hello následující:

        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Text;
        using System.Threading;
        using Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling;
        using org.apache.hadoop.hbase.rest.protobuf.generated;
        using Microsoft.HBase.Client;
        using Tweetinvi.Models;

        namespace TweetSentimentStreaming
        {
            class HBaseWriter
            {
                // HDinsight HBase cluster and HBase table information
                const string CLUSTERNAME = "https://<Enter Your Cluster Name>.azurehdinsight.net/";
                const string HADOOPUSERNAME = "admin"; //hello default name is "admin"
                const string HADOOPUSERPASSWORD = "<Enter hello Hadoop User Password>";

                const string HBASETABLENAME = "tweets_by_words";
                const string COUNT_ROW_KEY = "~ROWCOUNT";
                const string COUNT_COLUMN_NAME = "d:COUNT";

                long rowCount = 0;

                // Sentiment dictionary file and hello punctuation characters
                const string DICTIONARYFILENAME = @"..\..\dictionary.tsv";
                private static char[] _punctuationChars = new[] {
            ' ', '!', '\"', '#', '$', '%', '&', '\'', '(', ')', '*', '+', ',', '-', '.', '/',   //ascii 23--47
            ':', ';', '<', '=', '>', '?', '@', '[', ']', '^', '_', '`', '{', '|', '}', '~' };   //ascii 58--64 + misc.

                // For writting tooHBase
                HBaseClient client;

                // a sentiment dictionary for estimate sentiment. It is loaded from a physical file.
                Dictionary<string, DictionaryItem> dictionary;

                // use multithread write
                Thread writerThread;
                Queue<ITweet> queue = new Queue<ITweet>();
                bool threadRunning = true;

                // This function connects tooHBase, loads hello sentiment dictionary, and starts hello thread for writting.
                public HBaseWriter()
                {
                    ClusterCredentials credentials = new ClusterCredentials(new Uri(CLUSTERNAME), HADOOPUSERNAME, HADOOPUSERPASSWORD);
                    client = new HBaseClient(credentials);

                    // create hello HBase table if it doesn't exist
                    if (!client.ListTablesAsync().Result.name.Contains(HBASETABLENAME))
                    {
                        TableSchema tableSchema = new TableSchema();
                        tableSchema.name = HBASETABLENAME;
                        tableSchema.columns.Add(new ColumnSchema { name = "d" });
                        client.CreateTableAsync(tableSchema).Wait();
                        Console.WriteLine("Table \"{0}\" is created.", HBASETABLENAME);
                    }

                    // Read current row count cell
                    rowCount = GetRowCount();

                    // Load sentiment dictionary from a file
                    LoadDictionary();

                    // Start a thread for writting tooHBase
                    writerThread = new Thread(new ThreadStart(WriterThreadFunction));
                    writerThread.Start();
                }

                ~HBaseWriter()
                {
                    threadRunning = false;
                }

                private long GetRowCount()
                {
                    try
                    {
                        RequestOptions options = RequestOptions.GetDefaultOptions();
                        options.RetryPolicy = RetryPolicy.NoRetry;
                        var cellSet = client.GetCellsAsync(HBASETABLENAME, COUNT_ROW_KEY, null, null, options).Result;
                        if (cellSet.rows.Count != 0)
                        {
                            var countCol = cellSet.rows[0].values.Find(cell => Encoding.UTF8.GetString(cell.column) == COUNT_COLUMN_NAME);
                            if (countCol != null)
                            {
                                return Convert.ToInt64(Encoding.UTF8.GetString(countCol.data));
                            }
                        }
                    }
                    catch(Exception ex)
                    {
                        if (ex.InnerException.Message.Equals("hello remote server returned an error: (404) Not Found.", StringComparison.OrdinalIgnoreCase))
                        {
                            return 0;
                        }
                        else
                        {
                            throw ex;
                        }

                    }

                    return 0;
                }

                // Enqueue hello Tweets received
                public void WriteTweet(ITweet tweet)
                {
                    lock (queue)
                    {
                        queue.Enqueue(tweet);
                    }
                }

                // Load sentiment dictionary from a file
                private void LoadDictionary()
                {
                    List<string> lines = File.ReadAllLines(DICTIONARYFILENAME).ToList();
                    var items = lines.Select(line =>
                    {
                        var fields = line.Split('\t');
                        var pos = 0;
                        return new DictionaryItem
                        {
                            Type = fields[pos++],
                            Length = Convert.ToInt32(fields[pos++]),
                            Word = fields[pos++],
                            Pos = fields[pos++],
                            Stemmed = fields[pos++],
                            Polarity = fields[pos++]
                        };
                    });

                    dictionary = new Dictionary<string, DictionaryItem>();
                    foreach (var item in items)
                    {
                        if (!dictionary.Keys.Contains(item.Word))
                        {
                            dictionary.Add(item.Word, item);
                        }
                    }
                }

                // Calculate sentiment score
                private int CalcSentimentScore(string[] words)
                {
                    Int32 total = 0;
                    foreach (string word in words)
                    {
                        if (dictionary.Keys.Contains(word))
                        {
                            switch (dictionary[word].Polarity)
                            {
                                case "negative": total -= 1; break;
                                case "positive": total += 1; break;
                            }
                        }
                    }
                    if (total > 0)
                    {
                        return 1;
                    }
                    else if (total < 0)
                    {
                        return -1;
                    }
                    else
                    {
                        return 0;
                    }
                }

                // Popular a CellSet object toobe written into HBase
                private void CreateTweetByWordsCells(CellSet set, ITweet tweet)
                {
                    // Split hello Tweet into words
                    string[] words = tweet.Text.ToLower().Split(_punctuationChars);

                    // Calculate sentiment score base on hello words
                    int sentimentScore = CalcSentimentScore(words);
                    var word_pairs = words.Take(words.Length - 1)
                                        .Select((word, idx) => string.Format("{0} {1}", word, words[idx + 1]));
                    var all_words = words.Concat(word_pairs).ToList();

                    // For each word in hello Tweet add a row toohello HBase table
                    foreach (string word in all_words)
                    {
                        string time_index = (ulong.MaxValue - (ulong)tweet.CreatedAt.ToBinary()).ToString().PadLeft(20) + tweet.IdStr;
                        string key = word + "_" + time_index;

                        // Create a row
                        var row = new CellSet.Row { key = Encoding.UTF8.GetBytes(key) };

                        // Add columns toohello row, including Tweet identifier, language, coordinator(if available), and sentiment
                        var value = new Cell { column = Encoding.UTF8.GetBytes("d:id_str"), data = Encoding.UTF8.GetBytes(tweet.IdStr) };
                        row.values.Add(value);

                        value = new Cell { column = Encoding.UTF8.GetBytes("d:lang"), data = Encoding.UTF8.GetBytes(tweet.Language.ToString()) };
                        row.values.Add(value);

                        if (tweet.Coordinates != null)
                        {
                            var str = tweet.Coordinates.Longitude.ToString() + "," + tweet.Coordinates.Latitude.ToString();
                            value = new Cell { column = Encoding.UTF8.GetBytes("d:coor"), data = Encoding.UTF8.GetBytes(str) };
                            row.values.Add(value);
                        }

                        value = new Cell { column = Encoding.UTF8.GetBytes("d:sentiment"), data = Encoding.UTF8.GetBytes(sentimentScore.ToString()) };
                        row.values.Add(value);

                        set.rows.Add(row);
                    }
                }

                // Write a Tweet (CellSet) tooHBase
                public void WriterThreadFunction()
                {
                    try
                    {
                        while (threadRunning)
                        {
                            if (queue.Count > 0)
                            {
                                CellSet set = new CellSet();
                                lock (queue)
                                {
                                    do
                                    {
                                        ITweet tweet = queue.Dequeue();
                                        CreateTweetByWordsCells(set, tweet);
                                    } while (queue.Count > 0);
                                }

                                // Write hello Tweet by words cell set toohello HBase table
                                client.StoreCellsAsync(HBASETABLENAME, set).Wait();
                                Console.WriteLine("\tRows written: {0}", set.rows.Count);
                            }
                            Thread.Sleep(100);
                        }
                    }
                    catch (Exception ex)
                    {
                        Console.WriteLine("Exception: " + ex.Message);
                    }
                }
            }
            public class DictionaryItem
            {
                public string Type { get; set; }
                public int Length { get; set; }
                public string Word { get; set; }
                public string Pos { get; set; }
                public string Stemmed { get; set; }
                public string Polarity { get; set; }
            }
        }
5. Nastavit hello konstanty v hello předchozí kód, včetně **CLUSTERNAME**, **HADOOPUSERNAME**, **HADOOPUSERPASSWORD**a DICTIONARYFILENAME. Hello DICTIONARYFILENAME je hello název souboru a umístění hello hello direction.tsv.  soubor Hello si můžete stáhnout z **https://hditutorialdata.blob.core.windows.net/twittersentiment/dictionary.tsv**. Pokud chcete název tabulky HBase hello toochange, musíte změnit název tabulky hello ve webové aplikaci hello odpovídajícím způsobem.
6. Otevřete **Program.cs**a nahraďte kód hello hello následující:

        using System;
        using System.Diagnostics;
        using Tweetinvi;
        using Tweetinvi.Models;

        namespace TweetSentimentStreaming
        {
            class Program
            {
                const string TWITTERAPPACCESSTOKEN = "<Enter Twitter App Access Token>";
                const string TWITTERAPPACCESSTOKENSECRET = "<Enter Twitter Access Token Secret>";
                const string TWITTERAPPAPIKEY = "<Enter Twitter App API Key>";
                const string TWITTERAPPAPISECRET = "<Enter Twitter App API Secret>";

                static void Main(string[] args)
                {
                    Auth.SetUserCredentials(TWITTERAPPAPIKEY, TWITTERAPPAPISECRET, TWITTERAPPACCESSTOKEN, TWITTERAPPACCESSTOKENSECRET);

                    Stream_FilteredStreamExample();
                }

                private static void Stream_FilteredStreamExample()
                {
                    for (;;)
                    {
                        try
                        {
                            HBaseWriter hbase = new HBaseWriter();
                            var stream = Stream.CreateFilteredStream();
                            stream.AddLocation(new Coordinates(90, -180), new Coordinates(-90,180));

                            var tweetCount = 0;
                            var timer = Stopwatch.StartNew();

                            stream.MatchingTweetReceived += (sender, args) =>
                            {
                                tweetCount++;
                                var tweet = args.Tweet;

                                // Write Tweets tooHBase
                                hbase.WriteTweet(tweet);

                                if (timer.ElapsedMilliseconds > 1000)
                                {
                                    if (tweet.Coordinates != null)
                                    {
                                        Console.ForegroundColor = ConsoleColor.Green;
                                        Console.WriteLine("\n{0}: {1} {2}", tweet.Id, tweet.Language.ToString(), tweet.Text);
                                        Console.ForegroundColor = ConsoleColor.White;
                                        Console.WriteLine("\tLocation: {0}, {1}", tweet.Coordinates.Longitude, tweet.Coordinates.Latitude);
                                    }

                                    timer.Restart();
                                    Console.WriteLine("\tTweets/sec: {0}", tweetCount);
                                    tweetCount = 0;
                                }
                            };

                            stream.StartStreamMatchingAllConditions();
                        }
                        catch (Exception ex)
                        {
                            Console.WriteLine("Exception: {0}", ex.Message);
                        }
                    }
                }

            }
        }
7. Nastavit hello konstanty včetně **TWITTERAPPACCESSTOKEN**, **TWITTERAPPACCESSTOKENSECRET**, **TWITTERAPPAPIKEY** a **TWITTERAPPAPISECRET**.

hello toorun streamování služby, stiskněte klávesu **F5**. Následující Hello je snímek obrazovky aplikace hello konzoly:

![hdinsight.hbase.Twitter.sentiment.Streaming.Service][img-streaming-service]

Zachovat hello streamování konzolové aplikace běžící při vývoji hello webové aplikace, abyste získali další toouse data. data hello tooexamine vloženy do tabulky hello, můžete použít prostředí HBase. V tématu [začít pracovat s HBase v HDInsight](hdinsight-hbase-tutorial-get-started-linux.md#create-tables-and-insert-data).

## <a name="visualize-real-time-sentiment"></a>Vizualizace postojích v reálném čase
V této části vytvoříte v reálném čase postojích dat nástroje webové aplikace tooread hello rozhraní ASP.NET MVC z dat hello HBase a výkresu na mapy Bing.

**toocreate aplikace technologie ASP.NET MVC**

1. Otevřete sadu Visual Studio.
2. Klikněte na tlačítko **soubor**, klikněte na tlačítko **nový**a potom klikněte na **projektu**.
3. Zadejte hello následující informace:

   * Šablona kategorie: **Visual C# / Web**
   * Šablona: **webové aplikace ASP.NET**
   * Název: **TweetSentimentWeb**
   * Umístění: **C:\Tutorials**
4. Klikněte na **OK**.
5. V **vyberte šablonu**, klikněte na tlačítko **MVC**.
6. V **Microsoft Azure**, klikněte na tlačítko **Spravovat odběry**.
7. Z **spravovat předplatná Microsoft Azure**, klikněte na tlačítko **přihlášení**.
8. Zadejte přihlašovací údaje Azure. Informace o vašem předplatném Azure je zobrazený na hello **účty** kartě.
9. Klikněte na tlačítko **Zavřít** tooclose hello **spravovat předplatná Microsoft Azure** okno.
10. Z **nový projekt ASP.NET - TweetSentimentWeb**, klikněte na tlačítko **OK**.
11. Z **nakonfigurovat nastavení webu Microsoft Azure**, vyberte hello **oblast** který je nejblíže tooyou. Nepotřebujete toospecify databázový server.
12. Klikněte na **OK**.

**balíčky NuGet tooinstall**

1. Z hello **nástroje** nabídky, klikněte na tlačítko **Správce balíčků Nuget**a potom klikněte na **Konzola správce balíčků**. panelu konzoly Hello je otevřen v hello dolní části stránky hello.
2. Použití hello následující příkaz tooinstall hello [HBase .NET SDK](https://www.nuget.org/packages/Microsoft.HBase.Client/) balíček, který je hello klienta knihovny tooaccess HBase cluster:

        Install-Package Microsoft.HBase.Client

**tooadd HBaseReader – třída**

1. Z **Průzkumníku řešení**, rozbalte položku **TweetSentiment**.
2. Klikněte pravým tlačítkem na **modely**, klikněte na tlačítko **přidat**a potom klikněte na **třída**.
3. V hello **název** zadejte **HBaseReader.cs**a potom klikněte na **přidat**.
4. Nahraďte kód hello hello následující:

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Web;

        using System.Configuration;
        using System.Threading.Tasks;
        using System.Text;
        using Microsoft.HBase.Client;
        using org.apache.hadoop.hbase.rest.protobuf.generated;

        namespace TweetSentimentWeb.Models
        {
            public class HBaseReader
            {
                // For reading Tweet sentiment data from HDInsight HBase
                HBaseClient client;

                // HDinsight HBase cluster and HBase table information
                const string CLUSTERNAME = "<HBaseClusterName>";
                const string HADOOPUSERNAME = "<HBaseClusterHadoopUserName>"
                const string HADOOPUSERPASSWORD = "<HBaseCluserUserPassword>";
                const string HBASETABLENAME = "tweets_by_words";

                // hello constructor
                public HBaseReader()
                {
                    ClusterCredentials creds = new ClusterCredentials(
                                    new Uri(CLUSTERNAME),
                                    HADOOPUSERNAME,
                                    HADOOPUSERPASSWORD);
                    client = new HBaseClient(creds);
                }

                // Query Tweets sentiment data from hello HBase table asynchronously
                public async Task<IEnumerable<Tweet>> QueryTweetsByKeywordAsync(string keyword)
                {
                    List<Tweet> list = new List<Tweet>();

                    // Demonstrate Filtering hello data from hello past 6 hours hello row key
                    string timeIndex = (ulong.MaxValue -
                        (ulong)DateTime.UtcNow.Subtract(new TimeSpan(6, 0, 0)).ToBinary()).ToString().PadLeft(20);
                    string startRow = keyword + "_" + timeIndex;
                    string endRow = keyword + "|";
                    Scanner scanSettings = new Scanner
                    {
                        batch = 100000,
                        startRow = Encoding.UTF8.GetBytes(startRow),
                        endRow = Encoding.UTF8.GetBytes(endRow)
                    };

                    // Make async scan call
                    ScannerInformation scannerInfo =
                        await client.CreateScannerAsync(HBASETABLENAME, scanSettings);

                    CellSet next;

                    while ((next = await client.ScannerGetNextAsync(scannerInfo)) != null)
                    {
                        foreach (CellSet.Row row in next.rows)
                        {
                            // find hello cell with string pattern "d:coor"
                            var coordinates =
                                row.values.Find(c => Encoding.UTF8.GetString(c.column) == "d:coor");

                            if (coordinates != null)
                            {
                                string[] lonlat = Encoding.UTF8.GetString(coordinates.data).Split(',');

                                var sentimentField =
                                    row.values.Find(c => Encoding.UTF8.GetString(c.column) == "d:sentiment");
                                Int32 sentiment = 0;
                                if (sentimentField != null)
                                {
                                    sentiment = Convert.ToInt32(Encoding.UTF8.GetString(sentimentField.data));
                                }

                                list.Add(new Tweet
                                {
                                    Longtitude = Convert.ToDouble(lonlat[0]),
                                    Latitude = Convert.ToDouble(lonlat[1]),
                                    Sentiment = sentiment
                                });
                            }

                            if (coordinates != null)
                            {
                                string[] lonlat = Encoding.UTF8.GetString(coordinates.data).Split(',');
                            }
                        }
                    }

                    return list;
                }
            }

            public class Tweet
            {
                public string IdStr { get; set; }
                public string Text { get; set; }
                public string Lang { get; set; }
                public double Longtitude { get; set; }
                public double Latitude { get; set; }
                public int Sentiment { get; set; }
            }
        }
5. Uvnitř hello **HBaseReader** třídy, změňte hodnoty konstant hello následujícím způsobem:

   * **Název clusteru**: hello HBase název clusteru, například *https://<HBaseClusterName>.azurehdinsight.net/*.
   * **HADOOPUSERNAME**: hello HBase clusteru Hadoop uživatel uživatelské jméno. Hello výchozí název je *správce*.
   * **HADOOPUSERPASSWORD**: heslo uživatele hello HBase clusteru Hadoop.
   * **HBASETABLENAME** = "tweets_by_words";

     Název tabulky HBase Hello je **"tweets_by_words";**. Hello hodnoty musí odpovídat hello hodnoty, které jste odeslali v hello streamování služby, aby webová aplikace hello čte hello data z hello stejné tabulky HBase.

**tooadd TweetsController řadiče**

1. Z **Průzkumníku řešení**, rozbalte položku **TweetSentimentWeb**.
2. Klikněte pravým tlačítkem na **řadiče**, klikněte na tlačítko **přidat**a potom klikněte na **řadič**.
3. Klikněte na tlačítko **webové rozhraní API 2 řadiče - prázdný**a potom klikněte na **přidat**.
4. V hello **názvu Kontroleru** zadejte **TweetsController**a potom klikněte na **přidat**.
5. Z **Průzkumníku**, poklikejte na soubor hello tooopen TweetsController.cs.
6. Upravte soubor hello, takže to vypadá hello následující:

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Net;
        using System.Net.Http;
        using System.Web.Http;

        using System.Threading.Tasks;
        using TweetSentimentWeb.Models;

        namespace TweetSentimentWeb.Controllers
        {
            public class TweetsController : ApiController
            {
                HBaseReader hbase = new HBaseReader();

                public async Task<IEnumerable<Tweet>> GetTweetsByQuery(string query)
                {
                    return await hbase.QueryTweetsByKeywordAsync(query);
                }
            }
        }

**tooadd heatmap.js**

1. Z **Průzkumníku řešení**, rozbalte položku **TweetSentimentWeb**.
2. Klikněte pravým tlačítkem na **skripty**, klikněte na tlačítko **přidat**, klikněte na tlačítko **soubor JavaScript**.
3. V hello **název položky** zadejte **heatmap.js**.
4. Vložte následující kód do souboru hello hello. Kód Hello napsal Alastair Aitchison. Další informace najdete v tématu [Bing Maps AJAX v7 HeatMap knihovny](http://alastaira.wordpress.com/2011/04/15/bing-maps-ajax-v7-heatmap-library/).

        /*******************************************************************************
        * Author: Alastair Aitchison
        * Website: http://alastaira.wordpress.com
        * Date: 15th April 2011
        *
        * Description:
        * This JavaScript file provides an algorithm that can be used tooadd a heatmap
        * overlay on a Bing Maps v7 control. hello intensity and temperature palette
        * of hello heatmap are designed toobe easily customisable.
        *
        * Requirements:
        * hello heatmap layer itself is created dynamically on hello client-side using
        * hello HTML5 &lt;canvas> element, and therefore requires a browser that supports
        * this element. It has been tested on IE9, Firefox 3.6/4 and
        * Chrome 10 browsers. If you can confirm whether it works on other browsers or
        * not, I'd love toohear from you!
        *
        * Usage:
        * hello HeatMapLayer constructor requires:
        * - A reference tooa map object
        * - An array or Microsoft.Maps.Location items
        * - Optional parameters toocustomise hello appearance of hello layer
        *  (Radius,, Unit, Intensity, and ColourGradient), and a callback function
        */

        var HeatMapLayer = function (map, locations, options) {

            /* Private Properties */
            var _map = map,
                _canvas,
                _temperaturemap,
                _locations = [],
                _viewchangestarthandler,
                _viewchangeendhandler;

            // Set default options
            var _options = {
                // Opacity at hello centre of each heat point
                intensity: 0.5,

                // Affected radius of each heat point
                radius: 1000,

                // Whether hello radius is an absolute pixel value or meters
                unit: 'meters',

                // Colour temperature gradient of hello map
                colourgradient: {
                    "0.00": 'rgba(255,0,255,20)',  // Magenta
                    "0.25": 'rgba(0,0,255,40)',    // Blue
                    "0.50": 'rgba(0,255,0,80)',    // Green
                    "0.75": 'rgba(255,255,0,120)', // Yellow
                    "1.00": 'rgba(255,0,0,150)'    // Red
                },

                // Callback function toobe fired after heatmap layer has been redrawn
                callback: null
            };

            /* Private Methods */
            function _init() {
                var _mapDiv = _map.getRootElement();

                if (_mapDiv.childNodes.length >= 3 && _mapDiv.childNodes[2].childNodes.length >= 2) {
                    // Create hello canvas element
                    _canvas = document.createElement('canvas');
                    _canvas.style.position = 'relative';

                    var container = document.createElement('div');
                    container.style.position = 'absolute';
                    container.style.left = '0px';
                    container.style.top = '0px';
                    container.appendChild(_canvas);

                    _mapDiv.childNodes[2].childNodes[1].appendChild(container);

                    // Override defaults with any options passed in hello constructor
                    _setOptions(options);

                    // Load array of location data
                    _setPoints(locations);

                    // Create a colour gradient from hello suppied colourstops
                    _temperaturemap = _createColourGradient(_options.colourgradient);

                    // Wire up hello event handler tooredraw heatmap canvas
                    _viewchangestarthandler = Microsoft.Maps.Events.addHandler(_map, 'viewchangestart', _clearHeatMap);
                    _viewchangeendhandler = Microsoft.Maps.Events.addHandler(_map, 'viewchangeend', _createHeatMap);

                    _createHeatMap();

                    delete _init;
                } else {
                    setTimeout(_init, 100);
                }
            }

            // Resets hello heat map
            function _clearHeatMap() {
                var ctx = _canvas.getContext("2d");
                ctx.clearRect(0, 0, _canvas.width, _canvas.height);
            }

            // Creates a colour gradient from supplied colour stops on initialisation
            function _createColourGradient(colourstops) {
                var ctx = document.createElement('canvas').getContext('2d');
                var grd = ctx.createLinearGradient(0, 0, 256, 0);
                for (var c in colourstops) {
                    grd.addColorStop(c, colourstops[c]);
                }
                ctx.fillStyle = grd;
                ctx.fillRect(0, 0, 256, 1);
                return ctx.getImageData(0, 0, 256, 1).data;
            }

            // Applies a colour gradient toohello intensity map
            function _colouriseHeatMap() {
                var ctx = _canvas.getContext("2d");
                var dat = ctx.getImageData(0, 0, _canvas.width, _canvas.height);
                var pix = dat.data; // pix is a CanvasPixelArray containing height x width x 4 bytes of data (RGBA)
                for (var p = 0, len = pix.length; p < len;) {
                    var a = pix[p + 3] * 4; // get hello alpha of this pixel
                    if (a != 0) { // If there is any data tooplot
                        pix[p] = _temperaturemap[a]; // set hello red value of hello gradient that corresponds toothis alpha
                        pix[p + 1] = _temperaturemap[a + 1]; //set hello green value based on alpha
                        pix[p + 2] = _temperaturemap[a + 2]; //set hello blue value based on alpha
                    }
                    p += 4; // Move on toohello next pixel
                }
                ctx.putImageData(dat, 0, 0);
            }

            // Sets any options passed in
            function _setOptions(options) {
                for (attrname in options) {
                    _options[attrname] = options[attrname];
                }
            }

            // Sets hello heatmap points from an array of Microsoft.Maps.Locations  
            function _setPoints(locations) {
                _locations = locations;
            }

            // Main method toodraw hello heatmap
            function _createHeatMap() {
                // Ensure hello canvas matches hello current dimensions of hello map
                // This also has hello effect of resetting hello canvas
                _canvas.height = _map.getHeight();
                _canvas.width = _map.getWidth();

                _canvas.style.top = -_canvas.height / 2 + 'px';
                _canvas.style.left = -_canvas.width / 2 + 'px';

                // Calculate hello pixel radius of each heatpoint at hello current map zoom
                if (_options.unit == "pixels") {
                    radiusInPixel = _options.radius;
                } else {
                    radiusInPixel = _options.radius / _map.getMetersPerPixel();
                }

                var ctx = _canvas.getContext("2d");

                // Convert lat/long toopixel location
                var pixlocs = _map.tryLocationToPixel(_locations, Microsoft.Maps.PixelReference.control);
                var shadow = 'rgba(0, 0, 0, ' + _options.intensity + ')';
                var mapWidth = 256 * Math.pow(2, _map.getZoom());

                // Create hello Intensity Map by looping through each location
                for (var i = 0, len = pixlocs.length; i < len; i++) {
                    var x = pixlocs[i].x;
                    var y = pixlocs[i].y;

                    if (x < 0) {
                        x += mapWidth * Math.ceil(Math.abs(x / mapWidth));
                    }

                    // Create radial gradient centred on this point
                    var grd = ctx.createRadialGradient(x, y, 0, x, y, radiusInPixel);
                    grd.addColorStop(0.0, shadow);
                    grd.addColorStop(1.0, 'transparent');

                    // Draw hello heatpoint onto hello canvas
                    ctx.fillStyle = grd;
                    ctx.fillRect(x - radiusInPixel, y - radiusInPixel, 2 * radiusInPixel, 2 * radiusInPixel);
                }

                // Apply hello specified colour gradient toohello intensity map
                _colouriseHeatMap();

                // Call hello callback function, if specified
                if (_options.callback) {
                    _options.callback();
                }
            }

            /* Public Methods */

            this.Show = function () {
                if (_canvas) {
                    _canvas.style.display = '';
                }
            };

            this.Hide = function () {
                if (_canvas) {
                    _canvas.style.display = 'none';
                }
            };

            // Sets options for intensity, radius, colourgradient etc.
            this.SetOptions = function (options) {
                _setOptions(options);
            }

            // Sets an array of Microsoft.Maps.Locations from which hello heatmap is created
            this.SetPoints = function (locations) {
                // Reset hello existing heatmap layer
                _clearHeatMap();
                // Pass in hello new set of locations
                _setPoints(locations);
                // Recreate hello layer
                _createHeatMap();
            }

            // Removes hello heatmap layer from hello DOM
            this.Remove = function () {
                _canvas.parentNode.parentNode.removeChild(_canvas.parentNode);

                if (_viewchangestarthandler) { Microsoft.Maps.Events.removeHandler(_viewchangestarthandler); }
                if (_viewchangeendhandler) { Microsoft.Maps.Events.removeHandler(_viewchangeendhandler); }

                _locations = null;
                _temperaturemap = null;
                _canvas = null;
                _options = null;
                _viewchangestarthandler = null;
                _viewchangeendhandler = null;
            }

            // Call hello initialisation routine
            _init();
        };

        // Call hello Module Loaded method
        Microsoft.Maps.moduleLoaded('HeatMapModule');

**tooadd twitterStream.js**

1. Z **Průzkumníku řešení**, rozbalte položku **TweetSentimentWeb**.
2. Klikněte pravým tlačítkem na **skripty**, klikněte na tlačítko **přidat**, klikněte na tlačítko **soubor JavaScript**.
3. V hello **název položky** zadejte**twitterStream.js**.
4. Zkopírujte a vložte následující kód do souboru hello hello:

        var liveTweetsPos = [];
        var liveTweets = [];
        var liveTweetsNeg = [];
        var map;
        var heatmap;
        var heatmapNeg;
        var heatmapPos;

        function initialize() {
            // Initialize hello map
            var options = {
                credentials: "AvFJTZPZv8l3gF8VC3Y7BPBd0r7LKo8dqKG02EAlqg9WAi0M7la6zSIT-HwkMQbx",
                center: new Microsoft.Maps.Location(23.0, 8.0),
                mapTypeId: Microsoft.Maps.MapTypeId.ordnanceSurvey,
                labelOverlay: Microsoft.Maps.LabelOverlay.hidden,
                zoom: 2.5
            };
            var map = new Microsoft.Maps.Map(document.getElementById('map_canvas'), options);

            // Heatmap options for positive, neutral and negative layers

            var heatmapOptions = {
                // Opacity at hello centre of each heat point
                intensity: 0.5,

                // Affected radius of each heat point
                radius: 15,

                // Whether hello radius is an absolute pixel value or meters
                unit: 'pixels'
            };

            var heatmapPosOptions = {
                // Opacity at hello centre of each heat point
                intensity: 0.5,

                // Affected radius of each heat point
                radius: 15,

                // Whether hello radius is an absolute pixel value or meters
                unit: 'pixels',

                colourgradient: {
                    0.0: 'rgba(0, 255, 255, 0)',
                    0.1: 'rgba(0, 255, 255, 1)',
                    0.2: 'rgba(0, 255, 191, 1)',
                    0.3: 'rgba(0, 255, 127, 1)',
                    0.4: 'rgba(0, 255, 63, 1)',
                    0.5: 'rgba(0, 127, 0, 1)',
                    0.7: 'rgba(0, 159, 0, 1)',
                    0.8: 'rgba(0, 191, 0, 1)',
                    0.9: 'rgba(0, 223, 0, 1)',
                    1.0: 'rgba(0, 255, 0, 1)'
                }
            };

            var heatmapNegOptions = {
                // Opacity at hello centre of each heat point
                intensity: 0.5,

                // Affected radius of each heat point
                radius: 15,

                // Whether hello radius is an absolute pixel value or meters
                unit: 'pixels',

                colourgradient: {
                    0.0: 'rgba(0, 255, 255, 0)',
                    0.1: 'rgba(0, 255, 255, 1)',
                    0.2: 'rgba(0, 191, 255, 1)',
                    0.3: 'rgba(0, 127, 255, 1)',
                    0.4: 'rgba(0, 63, 255, 1)',
                    0.5: 'rgba(0, 0, 127, 1)',
                    0.7: 'rgba(0, 0, 159, 1)',
                    0.8: 'rgba(0, 0, 191, 1)',
                    0.9: 'rgba(0, 0, 223, 1)',
                    1.0: 'rgba(0, 0, 255, 1)'
                }
            };

            // Register and load hello Client Side HeatMap Module
            Microsoft.Maps.registerModule("HeatMapModule", "scripts/heatmap.js");
            Microsoft.Maps.loadModule("HeatMapModule", {
                callback: function () {
                    // Create heatmap layers for positive, neutral and negative tweets
                    heatmapPos = new HeatMapLayer(map, liveTweetsPos, heatmapPosOptions);
                    heatmap = new HeatMapLayer(map, liveTweets, heatmapOptions);
                    heatmapNeg = new HeatMapLayer(map, liveTweetsNeg, heatmapNegOptions);
                }
            });

            $("#searchbox").val("xbox");
            $("#searchBtn").click(onsearch);
            $("#positiveBtn").click(onPositiveBtn);
            $("#negativeBtn").click(onNegativeBtn);
            $("#neutralBtn").click(onNeutralBtn);
            $("#neutralBtn").button("toggle");
        }

        function onsearch() {
            var uri = 'api/tweets?query=';
            var query = $('#searchbox').val();
            $.getJSON(uri + query)
                .done(function (data) {
                    liveTweetsPos = [];
                    liveTweets = [];
                    liveTweetsNeg = [];

                    // On success, 'data' contains a list of tweets.
                    $.each(data, function (key, item) {
                        addTweet(item);
                    });

                    if (!$("#neutralBtn").hasClass('active')) {
                        $("#neutralBtn").button("toggle");
                    }
                    onNeutralBtn();
                })
                .fail(function (jqXHR, textStatus, err) {
                    $('#statustext').text('Error: ' + err);
                });
        }

        function addTweet(item) {
            //Add tweet toohello heat map arrays.
            var tweetLocation = new Microsoft.Maps.Location(item.Latitude, item.Longtitude);
            if (item.Sentiment > 0) {
                liveTweetsPos.push(tweetLocation);
            } else if (item.Sentiment < 0) {
                liveTweetsNeg.push(tweetLocation);
            } else {
                liveTweets.push(tweetLocation);
            }
        }

        function onPositiveBtn() {
            if ($("#neutralBtn").hasClass('active')) {
                $("#neutralBtn").button("toggle");
            }
            if ($("#negativeBtn").hasClass('active')) {
                $("#negativeBtn").button("toggle");
            }

            heatmapPos.SetPoints(liveTweetsPos);
            heatmapPos.Show();
            heatmapNeg.Hide();
            heatmap.Hide();

            $('#statustext').text('Tweets: ' + liveTweetsPos.length + "   " + getPosNegRatio());
        }

        function onNeutralBtn() {
            if ($("#positiveBtn").hasClass('active')) {
                $("#positiveBtn").button("toggle");
            }
            if ($("#negativeBtn").hasClass('active')) {
                $("#negativeBtn").button("toggle");
            }

            heatmap.SetPoints(liveTweets);
            heatmap.Show();
            heatmapNeg.Hide();
            heatmapPos.Hide();

            $('#statustext').text('Tweets: ' + liveTweets.length + "   " + getPosNegRatio());
        }

        function onNegativeBtn() {
            if ($("#positiveBtn").hasClass('active')) {
                $("#positiveBtn").button("toggle");
            }
            if ($("#neutralBtn").hasClass('active')) {
                $("#neutralBtn").button("toggle");
            }

            heatmapNeg.SetPoints(liveTweetsNeg);
            heatmapNeg.Show();
            heatmap.Hide();;
            heatmapPos.Hide();;

            $('#statustext').text('Tweets: ' + liveTweetsNeg.length + "\t" + getPosNegRatio());
        }

        function getPosNegRatio() {
            if (liveTweetsNeg.length == 0) {
                return "";
            }
            else {
                var ratio = liveTweetsPos.length / liveTweetsNeg.length;
                var str = parseFloat(Math.round(ratio * 10) / 10).toFixed(1);
                return "Positive/Negative Ratio: " + str;
            }
        }

**toomodify hello layout.cshtml**

1. Z **Průzkumníku řešení**, rozbalte položku **TweetSentimentWeb**, rozbalte položku **zobrazení**, rozbalte položku **sdílené**a potom dvakrát klikněte na _**Layout.cshtml**.
2. Nahraďte obsah hello hello následující:

        <!DOCTYPE html>
        <html>
        <head>
            <meta charset="utf-8" />
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>@ViewBag.Title</title>
            @Styles.Render("~/Content/css")
            @Scripts.Render("~/bundles/modernizr")
            <!-- Bing Maps -->
            <script type="text/javascript" src="http://ecn.dev.virtualearth.net/mapcontrol/mapcontrol.ashx?v=7.0&mkt=en-gb"></script>
            <!-- Spatial Dashboard JavaScript -->
            <script src="~/Scripts/twitterStream.js" type="text/javascript"></script>
        </head>
        <body onload="initialize()">
            <div class="navbar navbar-inverse navbar-fixed-top">
                <div class="container">
                    <div class="navbar-header">
                        <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
                            <span class="icon-bar"></span>
                            <span class="icon-bar"></span>
                            <span class="icon-bar"></span>
                        </button>
                    </div>
                    <div class="navbar-collapse collapse">
                        <div class="row">
                            <ul class="nav navbar-nav col-lg-5">
                                <li class="col-lg-12">
                                    <div class="navbar-form">
                                        <input id="searchbox" type="search" class="form-control">
                                        <button type="button" id="searchBtn" class="btn btn-primary">Go</button>
                                    </div>
                                </li>
                            </ul>
                            <ul class="nav navbar-nav col-lg-7">
                                <li>
                                    <div class="navbar-form">
                                        <div class="btn-group" data-toggle="buttons-radio">
                                            <button type="button" id="positiveBtn" class="btn btn-primary">Positive</button>
                                            <button type="button" id="neutralBtn" class="btn btn-primary">Neutral</button>
                                            <button type="button" id="negativeBtn" class="btn btn-primary">Negative</button>
                                        </div>
                                    </div>
                                </li>
                                <li><span id="statustext" class="navbar-text"></span></li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
            <div class="map_container">
                @RenderBody()
            </div>
            @Scripts.Render("~/bundles/jquery")
            @Scripts.Render("~/bundles/bootstrap")
            @RenderSection("scripts", required: false)
        </body>
        </html>

**toomodify hello Index.cshtml**

1. Z **Průzkumníku řešení**, rozbalte položku **TweetSentimentWeb**, rozbalte položku **zobrazení**, rozbalte položku **Domů**a potom dvakrát klikněte na  **Index.cshtml**.
2. Nahraďte obsah hello hello následující:

        @{
            ViewBag.Title = "Tweet Sentiment";
        }

        <div class="map_container">
            <div id="map_canvas"/>
        </div>

**souboru site.css toomodify hello**

1. Z **Průzkumníku řešení**, rozbalte položku **TweetSentimentWeb**, rozbalte položku **obsahu**a potom dvakrát klikněte na **Site.css**.
2. Připojte hello následující soubor toohello kódu:

        /* make container, and thus map, 100% width */
        .map_container {
            width: 100%;
            height: 100%;
        }

        #map_canvas{
          height:100%;
        }

        #tweets{
          position: absolute;
          top: 60px;
          left: 75px;
          z-index:1000;
          font-size: 30px;
        }

**soubor global.asax toomodify hello**

1. Z **Průzkumníku řešení**, rozbalte položku **TweetSentimentWeb**a potom dvakrát klikněte na **Global.asax**.
2. Přidejte následující hello **pomocí** příkaz:

        using System.Web.Http;
3. Přidejte následující řádky uvnitř hello hello **Application_Start()** funkce:

        // Register API routes
        GlobalConfiguration.Configure(WebApiConfig.Register);

    Upravte registraci hello hello API trasy toomake hello webového rozhraní API řadiče práce v aplikaci MVC hello.

**toorun hello webové aplikace**

1. Ověřte, že tento hello streamování aplikace konzoly služby je stále spuštěná, abyste viděli změny v reálném čase hello.
2. Stiskněte klávesu **F5** toorun hello webové aplikace:

    ![hdinsight.hbase.Twitter.sentiment.Bing.map][img-bing-map]
3. Hello textového pole zadejte klíčové slovo a pak klikněte na tlačítko **přejděte**.  V závislosti na hello data shromážděná v tabulce HBase hello pravděpodobně nebyly nalezeny některé klíčová slova. Zkuste některé běžné klíčová slova, například "rádi," "xbox" a "playstation."
4. Přepínání mezi **kladné**, **neutrální**, a **záporné** toocompare postojích na hello subjektu.
5. Umožňují hello streamování službu spustit jinou hodinu a pak hledání hello stejné klíčová slova a porovnejte výsledky hello.

Volitelně můžete nasadit aplikace tooAzure hello weby. Pokyny najdete v tématu [začít pracovat s weby Azure a ASP.NET][website-get-started].

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se dozvěděli, jak tooget tweetů, analyzovat hello postojích z tweetů, uložte hello postojích data tooHBase a existuje hello v reálném čase Twitter postojích data tooBing mapy. toolearn více, najdete v části:

* [Začínáme s HDInsight][hdinsight-get-started]
* [Konfigurace replikace HBase v HDInsight](hdinsight-hbase-replication.md)
* [Analýza dat Twitteru pomocí Hadoop ve službě HDInsight][hdinsight-analyze-twitter-data]
* [Analýza dat zpoždění letu pomocí HDInsight][hdinsight-analyze-flight-delay-data]
* [Vývoj aplikací Java MapReduce pro HDInsight][hdinsight-develop-mapreduce]

[hbase-get-started]: hdinsight-hbase-tutorial-get-started-linux.md
[website-get-started]: ../app-service-web/app-service-web-get-started-dotnet.md



[img-app-arch]: ./media/hdinsight-hbase-analyze-twitter-sentiment/AppArchitecture.png
[img-twitter-app]: ./media/hdinsight-hbase-analyze-twitter-sentiment/TwitterApp.png
[img-streaming-service]: ./media/hdinsight-hbase-analyze-twitter-sentiment/StreamingService.png
[img-bing-map]: ./media/hdinsight-hbase-analyze-twitter-sentiment/TwitterSentimentBingMap.png



[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-analyze-twitter-data]: hdinsight-analyze-twitter-data.md




[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-ODBC-driver.md
