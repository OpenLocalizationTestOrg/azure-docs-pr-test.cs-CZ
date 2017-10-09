---
title: "zpracování v reálném čase aaaStream analýzy pro Azure Functions | Microsoft Docs"
description: "Zjistěte, jak toouse funkce Azure připojení frontou Service Bus, toopopulate Azure Redis Cache z výstupu hello úlohy Stream Analytics."
keywords: "datový proud, mezipaměti redis frontou service bus"
services: stream-analytics
author: ryancrawcour
manager: jhubbard
documentationcenter: 
ms.assetid: d428bb33-4244-4001-b93d-c77bed816527
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/28/2017
ms.author: ryancraw
ms.openlocfilehash: 5ef4fe76c2cadf896a80eeaf421f010c315918af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toostore-data-from-azure-stream-analytics-in-an-azure-redis-cache-using-azure-functions"></a>Jak toostore data ze služby Azure Stream Analytics v Azure Redis Cache pomocí Azure Functions
Azure Stream Analytics umožňuje rychle vyvíjet a nasadit řešení nízkonákladové toogain přehledy v reálném čase ze zařízení, senzorů, infrastruktury a aplikací nebo jakýkoli proud dat. Umožňuje různé případy použití, jako je například v reálném čase správy a monitorování, příkaz a řízení, odhalování podvodů, připojené aut a mnoho dalších. V mnoha takových scénářů můžete data toostore výstupem Azure Stream Analytics do úložiště distribuovaných datech například Azure Redis cache.

Předpokládejme, že jsou součástí telecommunications společnosti. Pokoušíte toodetect SIM podvod, kde více volání pocházejících z hello stejnou identitu, v hello stejný čas, ale v různých geograficky umístění. Můžete se musí s ukládáním do mezipaměti Azure Redis všechny hello potenciální podvodné telefonní hovory. V tomto blogu poskytujeme pokyny na tom, jak můžete snadno dokončení úkolu. 

## <a name="prerequisites"></a>Požadavky
Dokončení hello [odhalování podvodů v reálném čase] [ fraud-detection] návod pro ASA

## <a name="architecture-overview"></a>Přehled architektury
![Snímek obrazovky architektura](./media/stream-analytics-functions-redis/architecture-overview.png)

Jak je znázorněno v předchozích obrázek hello, Stream Analytics umožňuje streamování vstupní data toobe dotaz a odeslány tooan výstup. Výstup hello podle, můžete Azure Functions pak aktivovat nějaký typ události. 

V tomto blogu můžeme zaměřit na Azure Functions hello součástí tohoto kanálu nebo přesněji řečeno hello aktivaci událost, která ukládá do mezipaměti hello podvodné data.
Po dokončení hello [odhalování podvodů v reálném čase] [ fraud-detection] kurzu máte vstup (centra událostí), dotaz a výstup (úložiště objektů blob) už nakonfigurovaná a spuštěná. V tomto blogu místo toho Změníme toouse výstup hello frontou Service Bus. Potom se nám připojit frontu toothis funkce Azure. 

## <a name="create-and-connect-a-service-bus-queue-output"></a>Vytvořit a připojit výstupní fronty Service Bus
toocreate frontou Service Bus, postupujte podle kroků 1 a 2 v části .NET hello [začít pracovat s fronty služby Service Bus][servicebus-getstarted].
Teď umožňuje připojit hello úlohy služby Stream Analytics toohello v fronty, který byl vytvořen v hello starší návodu zjišťování podvodů.

1. V hello portálu Azure, přejděte toohello **výstupy** okno úloha a vyberte **přidat** hello horní části stránky hello.
   
    ![Přidání výstupy](./media/stream-analytics-functions-redis/adding-outputs.png)
2. Zvolte **frontou Service Bus** jako hello **jímky** a postupujte podle pokynů hello na úvodní obrazovka. Zda toochoose hello obor názvů hello frontou Service Bus můžete vytvořit v [začít pracovat s fronty služby Service Bus][servicebus-getstarted]. Až budete hotovi, klikněte na tlačítko "vpravo" hello.
3. Zadejte hello následující hodnoty:
   
   * **Formát serializátor událostí**: JSON
   * **Kódování**: UTF8
   * **Formát**: řádcích
4. Klikněte na tlačítko hello **vytvořit** tlačítko tooadd tento zdroj a tooverify Stream Analytics můžete úspěšně připojit toohello účet úložiště.
5. V hello **dotazu** kartě, nahraďte hello následující hello aktuální dotaz. Nahraďte * [svůj název služby SBĚRNICE] * názvem výstup hello jste vytvořili v kroku 3. 
   
    ```    
   
        SELECT 
            System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
   
        INTO [YOUR SERVICE BUS NAME]
   
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
            ON CS1.CallingIMSI = CS2.CallingIMSI AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
   
        WHERE CS1.SwitchNum != CS2.SwitchNum
   
    ```

## <a name="create-an-azure-redis-cache"></a>Vytvoření Azure Redis Cache
Vytvoření mezipamětí Azure Redis podle části .NET hello v [jak tooUse Azure Redis Cache] [ use-rediscache] dokud není zavolána hello části ***konfigurace klientů mezipaměti hello***.
Po dokončení máte nové mezipaměti Redis. V části **všechna nastavení**, vyberte **přístupové klíče** a zapište hello ***primární připojovací řetězec***.

![Snímek obrazovky architektura](./media/stream-analytics-functions-redis/redis-cache-keys.png)

## <a name="create-an-azure-function"></a>Vytvoření Azure funkce
Postupujte podle [vytvoření první funkce Azure] [ functions-getstarted] kurz tooget začít s Azure Functions. Pokud už máte Azure funkce by jako toouse pak přeskočit příliš[zápis tooRedis mezipaměti](#Writing-to-Redis-Cache)

1. Hello portálu vyberte aplikační služby z levé navigační hello a pak klikněte na web Azure funkce aplikace název tooget toohello funkce pro aplikace.
    ![Snímek obrazovky seznamu funkce služby aplikací](./media/stream-analytics-functions-redis/app-services-function-list.png)
2. Klikněte na tlačítko **novou funkci > ServiceBusQueueTrigger – C#**. Pro hello následující pole, postupujte podle těchto pokynů:
   
   * **Název fronty**: hello stejný název jako název hello jste zadali při vytvoření fronty hello v [začít pracovat s fronty služby Service Bus] [ servicebus-getstarted] (ne hello názvem hello service Bus). Ujistěte se, že použijete hello fronty, která je připojená toohello, které výstup Stream Analytics.
   * **Připojení k Service Bus**: vyberte **přidat připojovací řetězec**. Vyberte toofind hello připojovací řetězec, portál classic přejděte toohello **Service Bus**, hello služby service bus, které jste vytvořili, a **informace o připojení** v hello dolní části obrazovky hello. Ujistěte se, že jste na úvodní obrazovka hlavní na této stránce. Zkopírujte a vložte připojovací řetězec hello. Zaregistrované volné tooenter libovolný název připojení.
     
       ![Snímek obrazovky připojení sběrnice služby](./media/stream-analytics-functions-redis/servicebus-connection.png)
   * **AccessRights**: Zvolte **spravovat**
3. Klikněte na **Vytvořit**

## <a name="writing-tooredis-cache"></a>Zápis tooRedis mezipaměti
Nyní jsme vytvořili funkce Azure, který čte z fronty Service Bus. Všechno, co je ponechán toodo je, použijte naše funkce toowrite tato data toohello Redis Cache. 

1. Vyberte nově vytvořenou **ServiceBusQueueTrigger**a klikněte na tlačítko **funkce nastavení aplikace** na hello pravém horním rohu. Vyberte **přejít nastavení služby tooApp > Nastavení > Nastavení aplikace**
2. V části řetězce připojení hello, vytvořte název v hello **název** části. Vložte hello primární připojovací řetězec jste našli v hello **vytvoření mezipaměti Redis** krokování s vnořením hello **hodnotu** části. Vyberte **vlastní** kde uvádí **SQL Database**.
3. Klikněte na tlačítko **Uložit** v horní části hello.
   
    ![Snímek obrazovky připojení sběrnice služby](./media/stream-analytics-functions-redis/function-connection-string.png)
4. Nyní přejděte zpět nastavení služby toohello aplikace a vyberte **nástroje > aplikace Service Editor (Preview) > na > přejděte**.
   
    ![Snímek obrazovky připojení sběrnice služby](./media/stream-analytics-functions-redis/app-service-editor.png)
5. V editoru podle své volby, vytvořte soubor JSON s názvem **project.json** s hello následující a uložit ho tooyour místní disk.
   
        {
            "frameworks": {
                "net46": {
                    "dependencies": {
                        "StackExchange.Redis":"1.1.603",
                        "Newtonsoft.Json": "9.0.1"
                    }
                }
            }
        }
6. Tento soubor nahrajte do kořenového adresáře hello funkce (ne WWWROOT). Měli byste vidět soubor s názvem **project.lock.json** nezobrazí automaticky, potvrzení, že hello Nuget byly naimportovány balíčků "StackExchange.Redis" a "Newtonsoft.Json".
7. V hello **run.csx** souboru, nahraďte hello následující kód hello předem generovaného kódu. Ve funkci lazyConnection hello, nahraďte názvem hello jste vytvořili v kroku 2 ""připojení OBJEVÍ název **ukládání dat do mezipaměti Redis hello**.

````

    using System;
    using System.Threading.Tasks;
    using StackExchange.Redis;
    using Newtonsoft.Json;
    using System.Configuration;

    public static void Run(string myQueueItem, TraceWriter log)
    {
        log.Info($"Function processed message: {myQueueItem}");

        // Connection refers tooa property that returns a ConnectionMultiplexer
        IDatabase db = Connection.GetDatabase();
        log.Info($"Created database {db}");

        // Parse JSON and extract hello time
        var message = JsonConvert.DeserializeObject<dynamic>(myQueueItem);
        string time = message.time;
        string callingnum1 = message.callingnum1;

        // Perform cache operations using hello cache object...
        // Simple put of integral data types into hello cache
        string key = time + " - " + callingnum1;
        db.StringSet(key, myQueueItem);
        log.Info($"Object put in database. Key is {key} and value is {myQueueItem}");

        // Simple get of data types from hello cache
        string value = db.StringGet(key);
        log.Info($"Database got: {value}"); 
    }

    // Connect toohello Service Bus
    private static Lazy<ConnectionMultiplexer> lazyConnection = 
        new Lazy<ConnectionMultiplexer>(() =>
            {
                var cnn = ConfigurationManager.ConnectionStrings["CONN NAME"].ConnectionString
                return ConnectionMultiplexer.Connect();
            });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

````

## <a name="start-hello-stream-analytics-job"></a>Spuštění úlohy Stream Analytics hello
1. Spusťte aplikaci telcodatagen.exe hello. využití Hello vypadá takto:````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````
2. V okně úlohy Stream Analytics hello hello portálu, klikněte na tlačítko **spustit** hello horní části stránky hello.
   
    ![Snímek obrazovky spuštění úlohy](./media/stream-analytics-functions-redis/starting-job.png)
3. V hello **spuštění úlohy** okno, které se zobrazí, vyberte **nyní** a pak klikněte na tlačítko hello **spustit** tlačítko dole hello úvodní obrazovka. Stav úlohy Hello změny tooStarting a po některé čas tooRunning změny.
   
    ![Snímek obrazovky výběru čas spuštění úlohy](./media/stream-analytics-functions-redis/start-job-time.png)

## <a name="run-solution-and-check-results"></a>Spuštění řešení a výsledky kontroly
Návratem tooyour **ServiceBusQueueTrigger** stránky, měli byste vidět příkazy protokolu. Tyto protokoly zobrazit něco získali z hello frontou Service Bus, umístí jej do databáze hello a načtených pomocí hello čas jako klíč hello!

tooverify, že vaše data jsou v vaše mezipaměť Redis přejděte tooyour Redis cache stránku hello nového portálu (jak je uvedeno v předchozí hello [vytvořit Azure Redis Cache](#Create-an-Azure-Redis-Cache) krok) a vyberte konzoly.

Teď můžete napsat Redis příkazy tooconfirm ve skutečnosti se data v mezipaměti hello.

![Snímek obrazovky konzola Redis](./media/stream-analytics-functions-redis/redis-console.png)

## <a name="next-steps"></a>Další kroky
Jsme nadšení o nové věci hello Azure Functions a Stream analytics můžete provést společně a Věříme, že to odemkne nové možnosti pro vás. Pokud máte jakékoli zpětnou vazbu na co chcete vedle, klidně hello volné toouse [Azure UserVoice lokality](https://feedback.azure.com/forums/270577-stream-analytics).

Pokud jste nový Microsoft Azure, doporučujeme vám tootry ho odhlašování pomocí registrujete [Bezplatný zkušební účet Azure](https://azure.microsoft.com/pricing/free-trial/). Pokud se nový tooStream analýzy, pak doporučujeme příliš[vytvoření vaší první úlohy Stream Analytics](stream-analytics-create-a-job.md).

Pokud potřebujete některé pomoc nebo máte otázky, můžete zveřejnit na [MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) nebo [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) fóra. 

Můžete také zjistit hello následující prostředky:

* [Referenční informace pro vývojáře Azure Functions](../azure-functions/functions-reference.md)
* [Azure funkcí jazyka C# referenční informace pro vývojáře](../azure-functions/functions-reference-csharp.md)
* [Azure funkce F # referenční informace pro vývojáře](../azure-functions/functions-reference-fsharp.md)
* [Referenční informace pro vývojáře Azure funkce NodeJS](../azure-functions/functions-reference.md)
* [Azure funkce triggerů a vazeb](../azure-functions/functions-triggers-bindings.md)
* [Jak toomonitor Azure mezipaměti Redis](../redis-cache/cache-how-to-monitor.md)

postupujte podle toostay aktuální na všech hello nejnovější informace a funkce, [ @AzureStreaming ](https://twitter.com/AzureStreaming) na Twitteru.

[fraud-detection]: stream-analytics-real-time-fraud-detection.md
[servicebus-getstarted]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[use-rediscache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md
[functions-getstarted]: ../azure-functions/functions-create-first-azure-function.md
