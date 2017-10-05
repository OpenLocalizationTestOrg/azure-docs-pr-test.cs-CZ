---
title: "Stream Analytics v reálném čase, zpracování pro Azure Functions | Microsoft Docs"
description: "Zjistěte, jak používat funkci Azure připojené frontou Service Bus, k naplnění Azure Redis Cache z výstupu úlohy Stream Analytics."
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
ms.openlocfilehash: ad14cc858ff513573e2718a26a9ab5c524e1adc6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-store-data-from-azure-stream-analytics-in-an-azure-redis-cache-using-azure-functions"></a>Tom, jak ukládat data z Azure Stream Analytics v Azure Redis Cache pomocí Azure Functions
Azure Stream Analytics umožňuje rychle vyvíjet a nasadit řešení nízkými náklady a získáte přehled o ze zařízení, senzorů, infrastruktury a aplikací nebo jakýkoli proud dat v reálném čase. Umožňuje různé případy použití, jako je například v reálném čase správy a monitorování, příkaz a řízení, odhalování podvodů, připojené aut a mnoho dalších. V mnoha takových scénářů můžete k ukládání dat do úložiště distribuovaných datech například Azure Redis cache výstupem Azure Stream Analytics.

Předpokládejme, že jsou součástí telecommunications společnosti. Pokoušíte se odhalování podvodů SIM, kde více volání pocházejících z stejnou identitu, zároveň čas, ale v různých geograficky umístění. Můžete se musí s ukládáním do mezipaměti Azure Redis všechny potenciální podvodné telefonní hovory. V tomto blogu poskytujeme pokyny na tom, jak můžete snadno dokončení úkolu. 

## <a name="prerequisites"></a>Požadavky
Dokončení [odhalování podvodů v reálném čase] [ fraud-detection] návod pro ASA

## <a name="architecture-overview"></a>Přehled architektury
![Snímek obrazovky architektura](./media/stream-analytics-functions-redis/architecture-overview.png)

Jak je vidět na předchozím obrázku, Stream Analytics umožňuje streamování vstupní data na dotaz a odesílají do výstupu. Výstup podle, můžete Azure Functions potom aktivovat nějaký typ události. 

V tomto blogu zaměříme na Azure Functions součástí tohoto kanálu, nebo přesněji řečeno aktivuje událost, která ukládá podvodné data do mezipaměti.
Po dokončení [odhalování podvodů v reálném čase] [ fraud-detection] kurzu máte vstup (centra událostí), dotaz a výstup (úložiště objektů blob) už nakonfigurovaná a spuštěná. V tomto blogu Změníme výstup místo toho použít frontu sběrnice. Potom jsme funkce Azure připojit do této fronty. 

## <a name="create-and-connect-a-service-bus-queue-output"></a>Vytvořit a připojit výstupní fronty Service Bus
Chcete-li vytvořit frontu sběrnice, postupujte podle kroků 1 a 2 v části .NET v [začít pracovat s fronty služby Service Bus][servicebus-getstarted].
Teď umožňuje připojit fronty do úlohy Stream Analytics, která byla vytvořena ve starší návodu zjišťování podvodů.

1. V portálu Azure přejděte do **výstupy** okno úloha a vyberte **přidat** v horní části stránky.
   
    ![Přidání výstupy](./media/stream-analytics-functions-redis/adding-outputs.png)
2. Zvolte **frontou Service Bus** jako **jímky** a postupujte podle pokynů na obrazovce. Nezapomeňte vybrat obor názvů frontou Service Bus, kterou jste vytvořili v [začít pracovat s fronty služby Service Bus][servicebus-getstarted]. Až skončíte, klikněte na tlačítko "vpravo".
3. Zadejte následující hodnoty:
   
   * **Formát serializátor událostí**: JSON
   * **Kódování**: UTF8
   * **Formát**: řádcích
4. Klikněte **vytvořit** tlačítko Přidat tento zdroj a ověřte, zda Stream Analytics můžete úspěšně připojit k účtu úložiště.
5. V **dotazu** kartě, nahraďte následujícím kódem aktuálního dotazu. Nahraďte * [svůj název služby SBĚRNICE] * s název výstupu, který jste vytvořili v kroku 3. 
   
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
Vytvoření služby Azure Redis cache pomocí následujících části .NET v [postup použití Azure Redis Cache] [ use-rediscache] dokud není zavolána části ***konfigurace klientů mezipaměti***.
Po dokončení máte nové mezipaměti Redis. V části **všechna nastavení**, vyberte **přístupové klíče** a poznamenejte si ***primární připojovací řetězec***.

![Snímek obrazovky architektura](./media/stream-analytics-functions-redis/redis-cache-keys.png)

## <a name="create-an-azure-function"></a>Vytvoření Azure funkce
Postupujte podle [vytvoření první funkce Azure] [ functions-getstarted] kurzu Začínáme s Azure Functions. Pokud již máte Azure funkce, které chcete použít, pak pokračujte [zápis k Redis Cache](#Writing-to-Redis-Cache)

1. Na portálu vyberte aplikační služby z levé navigaci a potom klikněte na název aplikace Azure funkce získat na funkce aplikace Web.
    ![Snímek obrazovky seznamu funkce služby aplikací](./media/stream-analytics-functions-redis/app-services-function-list.png)
2. Klikněte na tlačítko **novou funkci > ServiceBusQueueTrigger – C#**. Pro následující pole postupujte podle těchto pokynů:
   
   * **Název fronty**: stejný název jako název, který jste zadali při vytvoření fronty v [začít pracovat s fronty služby Service Bus] [ servicebus-getstarted] (nikoli název služby service bus). Ujistěte se, že používáte fronty, která je připojena k výstupu Stream Analytics.
   * **Připojení k Service Bus**: vyberte **přidat připojovací řetězec**. Chcete-li najít připojovací řetězec, přejděte na portálu classic, vyberte **Service Bus**, sběrnici služby, který jste vytvořili, a **informace o připojení** v dolní části obrazovky. Ujistěte se, že jste na obrazovce hlavní na této stránce. Zkopírujte a vložte připojovací řetězec. Nebojte se, že zadat libovolný název připojení.
     
       ![Snímek obrazovky připojení sběrnice služby](./media/stream-analytics-functions-redis/servicebus-connection.png)
   * **AccessRights**: Zvolte **spravovat**
3. Klikněte na **Vytvořit**

## <a name="writing-to-redis-cache"></a>Zápis do mezipaměti Redis
Nyní jsme vytvořili funkce Azure, který čte z fronty Service Bus. Je již zbývá udělat, použijte naše funkce zapsat tato data do mezipaměti Redis. 

1. Vyberte nově vytvořenou **ServiceBusQueueTrigger**a klikněte na tlačítko **funkce nastavení aplikace** v pravém horním rohu. Vyberte **přejděte do nastavení aplikace služby > Nastavení > Nastavení aplikace**
2. V části řetězce připojení vytvořit název v **název** části. Vložte primární připojovací řetězec, který jste získali v **vytvoření mezipaměti Redis** zanořte se do **hodnotu** části. Vyberte **vlastní** kde uvádí **SQL Database**.
3. Klikněte na tlačítko **Uložit** v horní části.
   
    ![Snímek obrazovky připojení sběrnice služby](./media/stream-analytics-functions-redis/function-connection-string.png)
4. Nyní přejděte zpět na službu nastavení aplikace a vyberte **nástroje > aplikace Service Editor (Preview) > na > přejděte**.
   
    ![Snímek obrazovky připojení sběrnice služby](./media/stream-analytics-functions-redis/app-service-editor.png)
5. V editoru podle své volby, vytvořte soubor JSON s názvem **project.json** následujícím textem a uložte ho na místní disk.
   
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
6. Tento soubor nahrajte do kořenového adresáře funkce (ne WWWROOT). Měli byste vidět soubor s názvem **project.lock.json** automaticky zobrazí potvrzení, zda byly naimportovány balíčky Nuget "StackExchange.Redis" a "Newtonsoft.Json".
7. V **run.csx** souboru, předem generovaného kódu nahraďte následujícím kódem. Ve funkci lazyConnection nahraďte názvem, který jste vytvořili v kroku 2 ""připojení OBJEVÍ název **ukládání dat do mezipaměti Redis**.

````

    using System;
    using System.Threading.Tasks;
    using StackExchange.Redis;
    using Newtonsoft.Json;
    using System.Configuration;

    public static void Run(string myQueueItem, TraceWriter log)
    {
        log.Info($"Function processed message: {myQueueItem}");

        // Connection refers to a property that returns a ConnectionMultiplexer
        IDatabase db = Connection.GetDatabase();
        log.Info($"Created database {db}");

        // Parse JSON and extract the time
        var message = JsonConvert.DeserializeObject<dynamic>(myQueueItem);
        string time = message.time;
        string callingnum1 = message.callingnum1;

        // Perform cache operations using the cache object...
        // Simple put of integral data types into the cache
        string key = time + " - " + callingnum1;
        db.StringSet(key, myQueueItem);
        log.Info($"Object put in database. Key is {key} and value is {myQueueItem}");

        // Simple get of data types from the cache
        string value = db.StringGet(key);
        log.Info($"Database got: {value}"); 
    }

    // Connect to the Service Bus
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

## <a name="start-the-stream-analytics-job"></a>Spustit úlohu služby Stream Analytics
1. Spusťte aplikaci telcodatagen.exe. Využití je následující:````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````
2. V okně úlohy Stream Analytics na portálu klikněte na tlačítko **spustit** v horní části stránky.
   
    ![Snímek obrazovky spuštění úlohy](./media/stream-analytics-functions-redis/starting-job.png)
3. V **spuštění úlohy** okno, které se zobrazí, vyberte **nyní** a pak klikněte na tlačítko **spustit** tlačítko v dolní části obrazovky. Změny stavu úlohy počáteční a po některé změny času ke spuštění.
   
    ![Snímek obrazovky výběru čas spuštění úlohy](./media/stream-analytics-functions-redis/start-job-time.png)

## <a name="run-solution-and-check-results"></a>Spuštění řešení a výsledky kontroly
Po návratu do vaší **ServiceBusQueueTrigger** stránky, měli byste vidět příkazy protokolu. Tyto protokoly zobrazit něco získali z frontou Service Bus, umístí jej do databáze a načtených pomocí čas jako klíč!

Pokud chcete ověřit, že vaše data jsou v mezipaměti Redis, přejděte na stránku mezipaměti Redis v nového portálu (jak je znázorněno v předchozím [vytvořit Azure Redis Cache](#Create-an-Azure-Redis-Cache) krok) a vyberte konzoly.

Teď můžete napsat příkazy Redis k potvrzení, že data jsou ve skutečnosti v mezipaměti.

![Snímek obrazovky konzola Redis](./media/stream-analytics-functions-redis/redis-console.png)

## <a name="next-steps"></a>Další kroky
Jsme nadšení o nové věci Azure Functions a Stream analytics můžete provést společně a Věříme, že to odemkne nové možnosti pro vás. Pokud máte jakékoli zpětnou vazbu na co chcete vedle, klidně používat [Azure UserVoice lokality](https://feedback.azure.com/forums/270577-stream-analytics).

Pokud jste nový Microsoft Azure, doporučujeme vám vyzkoušet odhlašování pomocí registrujete [Bezplatný zkušební účet Azure](https://azure.microsoft.com/pricing/free-trial/). Pokud jsou pro vás nové do služby Stream Analytics, pak doporučujeme [vytvoření vaší první úlohy Stream Analytics](stream-analytics-create-a-job.md).

Pokud potřebujete některé pomoc nebo máte otázky, můžete zveřejnit na [MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) nebo [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) fóra. 

Můžete také najdete v následujících zdrojích informací:

* [Referenční informace pro vývojáře Azure Functions](../azure-functions/functions-reference.md)
* [Azure funkcí jazyka C# referenční informace pro vývojáře](../azure-functions/functions-reference-csharp.md)
* [Azure funkce F # referenční informace pro vývojáře](../azure-functions/functions-reference-fsharp.md)
* [Referenční informace pro vývojáře Azure funkce NodeJS](../azure-functions/functions-reference.md)
* [Azure funkce triggerů a vazeb](../azure-functions/functions-triggers-bindings.md)
* [Tom, jak monitorovat Azure Redis Cache](../redis-cache/cache-how-to-monitor.md)

Udržovat přehled na nejnovější novinky a funkce, postupujte podle [ @AzureStreaming ](https://twitter.com/AzureStreaming) na Twitteru.

[fraud-detection]: stream-analytics-real-time-fraud-detection.md
[servicebus-getstarted]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[use-rediscache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md
[functions-getstarted]: ../azure-functions/functions-create-first-azure-function.md
