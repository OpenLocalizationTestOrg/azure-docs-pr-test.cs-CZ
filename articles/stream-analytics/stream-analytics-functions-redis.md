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
# <a name="how-toostore-data-from-azure-stream-analytics-in-an-azure-redis-cache-using-azure-functions"></a><span data-ttu-id="abf0d-104">Jak toostore data ze služby Azure Stream Analytics v Azure Redis Cache pomocí Azure Functions</span><span class="sxs-lookup"><span data-stu-id="abf0d-104">How toostore data from Azure Stream Analytics in an Azure Redis Cache using Azure Functions</span></span>
<span data-ttu-id="abf0d-105">Azure Stream Analytics umožňuje rychle vyvíjet a nasadit řešení nízkonákladové toogain přehledy v reálném čase ze zařízení, senzorů, infrastruktury a aplikací nebo jakýkoli proud dat.</span><span class="sxs-lookup"><span data-stu-id="abf0d-105">Azure Stream Analytics lets you rapidly develop and deploy low-cost solutions toogain real-time insights from devices, sensors, infrastructure, and applications, or any stream of data.</span></span> <span data-ttu-id="abf0d-106">Umožňuje různé případy použití, jako je například v reálném čase správy a monitorování, příkaz a řízení, odhalování podvodů, připojené aut a mnoho dalších.</span><span class="sxs-lookup"><span data-stu-id="abf0d-106">It enables various use cases such as real-time management and monitoring, command and control, fraud detection, connected cars and many more.</span></span> <span data-ttu-id="abf0d-107">V mnoha takových scénářů můžete data toostore výstupem Azure Stream Analytics do úložiště distribuovaných datech například Azure Redis cache.</span><span class="sxs-lookup"><span data-stu-id="abf0d-107">In many such scenarios, you may want toostore data outputted by Azure Stream Analytics into a distributed data store such as an Azure Redis cache.</span></span>

<span data-ttu-id="abf0d-108">Předpokládejme, že jsou součástí telecommunications společnosti.</span><span class="sxs-lookup"><span data-stu-id="abf0d-108">Suppose you are part of a telecommunications company.</span></span> <span data-ttu-id="abf0d-109">Pokoušíte toodetect SIM podvod, kde více volání pocházejících z hello stejnou identitu, v hello stejný čas, ale v různých geograficky umístění.</span><span class="sxs-lookup"><span data-stu-id="abf0d-109">You are trying toodetect SIM fraud where multiple calls coming from hello same identity, at hello same time, but in different geographically locations.</span></span> <span data-ttu-id="abf0d-110">Můžete se musí s ukládáním do mezipaměti Azure Redis všechny hello potenciální podvodné telefonní hovory.</span><span class="sxs-lookup"><span data-stu-id="abf0d-110">You are tasked with storing all hello potential fraudulent phone calls in an Azure Redis cache.</span></span> <span data-ttu-id="abf0d-111">V tomto blogu poskytujeme pokyny na tom, jak můžete snadno dokončení úkolu.</span><span class="sxs-lookup"><span data-stu-id="abf0d-111">In this blog, we provide guidance on how you can easily complete your task.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="abf0d-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="abf0d-112">Prerequisites</span></span>
<span data-ttu-id="abf0d-113">Dokončení hello [odhalování podvodů v reálném čase] [ fraud-detection] návod pro ASA</span><span class="sxs-lookup"><span data-stu-id="abf0d-113">Complete hello [Real-time Fraud Detection][fraud-detection] walk-through for ASA</span></span>

## <a name="architecture-overview"></a><span data-ttu-id="abf0d-114">Přehled architektury</span><span class="sxs-lookup"><span data-stu-id="abf0d-114">Architecture Overview</span></span>
![Snímek obrazovky architektura](./media/stream-analytics-functions-redis/architecture-overview.png)

<span data-ttu-id="abf0d-116">Jak je znázorněno v předchozích obrázek hello, Stream Analytics umožňuje streamování vstupní data toobe dotaz a odeslány tooan výstup.</span><span class="sxs-lookup"><span data-stu-id="abf0d-116">As shown in hello preceding figure, Stream Analytics allows streaming input data toobe queried and sent tooan output.</span></span> <span data-ttu-id="abf0d-117">Výstup hello podle, můžete Azure Functions pak aktivovat nějaký typ události.</span><span class="sxs-lookup"><span data-stu-id="abf0d-117">Based on hello output, Azure Functions can then trigger some type of event.</span></span> 

<span data-ttu-id="abf0d-118">V tomto blogu můžeme zaměřit na Azure Functions hello součástí tohoto kanálu nebo přesněji řečeno hello aktivaci událost, která ukládá do mezipaměti hello podvodné data.</span><span class="sxs-lookup"><span data-stu-id="abf0d-118">In this blog, we focus on hello Azure Functions part of this pipeline, or more specifically hello triggering of an event that stores fraudulent data into hello cache.</span></span>
<span data-ttu-id="abf0d-119">Po dokončení hello [odhalování podvodů v reálném čase] [ fraud-detection] kurzu máte vstup (centra událostí), dotaz a výstup (úložiště objektů blob) už nakonfigurovaná a spuštěná.</span><span class="sxs-lookup"><span data-stu-id="abf0d-119">After completing hello [Real-time Fraud Detection][fraud-detection] tutorial, you have an input (an event hub), a query, and an output (blob storage) already configured and running.</span></span> <span data-ttu-id="abf0d-120">V tomto blogu místo toho Změníme toouse výstup hello frontou Service Bus.</span><span class="sxs-lookup"><span data-stu-id="abf0d-120">In this blog, we change hello output toouse a Service Bus Queue instead.</span></span> <span data-ttu-id="abf0d-121">Potom se nám připojit frontu toothis funkce Azure.</span><span class="sxs-lookup"><span data-stu-id="abf0d-121">After that, we connect an Azure Function toothis queue.</span></span> 

## <a name="create-and-connect-a-service-bus-queue-output"></a><span data-ttu-id="abf0d-122">Vytvořit a připojit výstupní fronty Service Bus</span><span class="sxs-lookup"><span data-stu-id="abf0d-122">Create and connect a Service Bus Queue output</span></span>
<span data-ttu-id="abf0d-123">toocreate frontou Service Bus, postupujte podle kroků 1 a 2 v části .NET hello [začít pracovat s fronty služby Service Bus][servicebus-getstarted].</span><span class="sxs-lookup"><span data-stu-id="abf0d-123">toocreate a Service Bus Queue, follow steps 1 and 2 of hello .NET section in [Get Started with Service Bus Queues][servicebus-getstarted].</span></span>
<span data-ttu-id="abf0d-124">Teď umožňuje připojit hello úlohy služby Stream Analytics toohello v fronty, který byl vytvořen v hello starší návodu zjišťování podvodů.</span><span class="sxs-lookup"><span data-stu-id="abf0d-124">Now let's connect hello queue toohello Stream Analytics job that was created in hello earlier fraud detection walk-through.</span></span>

1. <span data-ttu-id="abf0d-125">V hello portálu Azure, přejděte toohello **výstupy** okno úloha a vyberte **přidat** hello horní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="abf0d-125">In hello Azure portal, go toohello **Outputs** blade of your job and select **Add** at hello top of hello page.</span></span>
   
    ![Přidání výstupy](./media/stream-analytics-functions-redis/adding-outputs.png)
2. <span data-ttu-id="abf0d-127">Zvolte **frontou Service Bus** jako hello **jímky** a postupujte podle pokynů hello na úvodní obrazovka.</span><span class="sxs-lookup"><span data-stu-id="abf0d-127">Choose **Service Bus Queue** as hello **Sink** and follow hello instructions on hello screen.</span></span> <span data-ttu-id="abf0d-128">Zda toochoose hello obor názvů hello frontou Service Bus můžete vytvořit v [začít pracovat s fronty služby Service Bus][servicebus-getstarted].</span><span class="sxs-lookup"><span data-stu-id="abf0d-128">Be sure toochoose hello namespace of hello Service Bus Queue you created in [Get Started with Service Bus Queues][servicebus-getstarted].</span></span> <span data-ttu-id="abf0d-129">Až budete hotovi, klikněte na tlačítko "vpravo" hello.</span><span class="sxs-lookup"><span data-stu-id="abf0d-129">Click hello "right" button when you are finished.</span></span>
3. <span data-ttu-id="abf0d-130">Zadejte hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="abf0d-130">Specify hello following values:</span></span>
   
   * <span data-ttu-id="abf0d-131">**Formát serializátor událostí**: JSON</span><span class="sxs-lookup"><span data-stu-id="abf0d-131">**Event Serializer Format**: JSON</span></span>
   * <span data-ttu-id="abf0d-132">**Kódování**: UTF8</span><span class="sxs-lookup"><span data-stu-id="abf0d-132">**Encoding**: UTF8</span></span>
   * <span data-ttu-id="abf0d-133">**Formát**: řádcích</span><span class="sxs-lookup"><span data-stu-id="abf0d-133">**FORMAT**: Line separated</span></span>
4. <span data-ttu-id="abf0d-134">Klikněte na tlačítko hello **vytvořit** tlačítko tooadd tento zdroj a tooverify Stream Analytics můžete úspěšně připojit toohello účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="abf0d-134">Click hello **Create** button tooadd this source and tooverify that Stream Analytics can successfully connect toohello storage account.</span></span>
5. <span data-ttu-id="abf0d-135">V hello **dotazu** kartě, nahraďte hello následující hello aktuální dotaz.</span><span class="sxs-lookup"><span data-stu-id="abf0d-135">In hello **Query** tab, replace hello current query with hello following.</span></span> <span data-ttu-id="abf0d-136">Nahraďte * [svůj název služby SBĚRNICE] * názvem výstup hello jste vytvořili v kroku 3.</span><span class="sxs-lookup"><span data-stu-id="abf0d-136">Replace *[YOUR SERVICE BUS NAME] * with hello output name you created in step 3.</span></span> 
   
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

## <a name="create-an-azure-redis-cache"></a><span data-ttu-id="abf0d-137">Vytvoření Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="abf0d-137">Create an Azure Redis Cache</span></span>
<span data-ttu-id="abf0d-138">Vytvoření mezipamětí Azure Redis podle části .NET hello v [jak tooUse Azure Redis Cache] [ use-rediscache] dokud není zavolána hello části ***konfigurace klientů mezipaměti hello***.</span><span class="sxs-lookup"><span data-stu-id="abf0d-138">Create an Azure Redis cache by following hello .NET section in [How tooUse Azure Redis Cache][use-rediscache] until hello section called ***Configure hello cache clients***.</span></span>
<span data-ttu-id="abf0d-139">Po dokončení máte nové mezipaměti Redis.</span><span class="sxs-lookup"><span data-stu-id="abf0d-139">Once complete, you have a new Redis Cache.</span></span> <span data-ttu-id="abf0d-140">V části **všechna nastavení**, vyberte **přístupové klíče** a zapište hello ***primární připojovací řetězec***.</span><span class="sxs-lookup"><span data-stu-id="abf0d-140">Under **All settings**, select **Access keys** and note down hello ***Primary connection string***.</span></span>

![Snímek obrazovky architektura](./media/stream-analytics-functions-redis/redis-cache-keys.png)

## <a name="create-an-azure-function"></a><span data-ttu-id="abf0d-142">Vytvoření Azure funkce</span><span class="sxs-lookup"><span data-stu-id="abf0d-142">Create an Azure Function</span></span>
<span data-ttu-id="abf0d-143">Postupujte podle [vytvoření první funkce Azure] [ functions-getstarted] kurz tooget začít s Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="abf0d-143">Follow [Create your first Azure Function][functions-getstarted] tutorial tooget started with Azure Functions.</span></span> <span data-ttu-id="abf0d-144">Pokud už máte Azure funkce by jako toouse pak přeskočit příliš[zápis tooRedis mezipaměti](#Writing-to-Redis-Cache)</span><span class="sxs-lookup"><span data-stu-id="abf0d-144">If you already have an Azure function you would like toouse, then skip ahead too[Writing tooRedis Cache](#Writing-to-Redis-Cache)</span></span>

1. <span data-ttu-id="abf0d-145">Hello portálu vyberte aplikační služby z levé navigační hello a pak klikněte na web Azure funkce aplikace název tooget toohello funkce pro aplikace.</span><span class="sxs-lookup"><span data-stu-id="abf0d-145">In hello portal, select App Services from hello left-hand navigation, then click your Azure function app name tooget toohello Function's app website.</span></span>
    <span data-ttu-id="abf0d-146">![Snímek obrazovky seznamu funkce služby aplikací](./media/stream-analytics-functions-redis/app-services-function-list.png)</span><span class="sxs-lookup"><span data-stu-id="abf0d-146">![Screenshot of app services function list](./media/stream-analytics-functions-redis/app-services-function-list.png)</span></span>
2. <span data-ttu-id="abf0d-147">Klikněte na tlačítko **novou funkci > ServiceBusQueueTrigger – C#**.</span><span class="sxs-lookup"><span data-stu-id="abf0d-147">Click **New Function > ServiceBusQueueTrigger – C#**.</span></span> <span data-ttu-id="abf0d-148">Pro hello následující pole, postupujte podle těchto pokynů:</span><span class="sxs-lookup"><span data-stu-id="abf0d-148">For hello following fields, follow these instructions:</span></span>
   
   * <span data-ttu-id="abf0d-149">**Název fronty**: hello stejný název jako název hello jste zadali při vytvoření fronty hello v [začít pracovat s fronty služby Service Bus] [ servicebus-getstarted] (ne hello názvem hello service Bus).</span><span class="sxs-lookup"><span data-stu-id="abf0d-149">**Queue name**: hello same name as hello name you entered when you created hello queue in [Get Started with Service Bus Queues][servicebus-getstarted] (not hello name of hello service bus).</span></span> <span data-ttu-id="abf0d-150">Ujistěte se, že použijete hello fronty, která je připojená toohello, které výstup Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="abf0d-150">Make sure you use hello queue that is connected toohello Stream Analytics output.</span></span>
   * <span data-ttu-id="abf0d-151">**Připojení k Service Bus**: vyberte **přidat připojovací řetězec**.</span><span class="sxs-lookup"><span data-stu-id="abf0d-151">**Service Bus connection**: Select **Add a connection string**.</span></span> <span data-ttu-id="abf0d-152">Vyberte toofind hello připojovací řetězec, portál classic přejděte toohello **Service Bus**, hello služby service bus, které jste vytvořili, a **informace o připojení** v hello dolní části obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="abf0d-152">toofind hello connection string, go toohello classic portal, select **Service Bus**, hello service bus you created, and **CONNECTION INFORMATION** at hello bottom of hello screen.</span></span> <span data-ttu-id="abf0d-153">Ujistěte se, že jste na úvodní obrazovka hlavní na této stránce.</span><span class="sxs-lookup"><span data-stu-id="abf0d-153">Make sure you are on hello main screen on this page.</span></span> <span data-ttu-id="abf0d-154">Zkopírujte a vložte připojovací řetězec hello.</span><span class="sxs-lookup"><span data-stu-id="abf0d-154">Copy and paste hello connection string.</span></span> <span data-ttu-id="abf0d-155">Zaregistrované volné tooenter libovolný název připojení.</span><span class="sxs-lookup"><span data-stu-id="abf0d-155">Feel free tooenter any connection name.</span></span>
     
       ![Snímek obrazovky připojení sběrnice služby](./media/stream-analytics-functions-redis/servicebus-connection.png)
   * <span data-ttu-id="abf0d-157">**AccessRights**: Zvolte **spravovat**</span><span class="sxs-lookup"><span data-stu-id="abf0d-157">**AccessRights**: Choose **Manage**</span></span>
3. <span data-ttu-id="abf0d-158">Klikněte na **Vytvořit**</span><span class="sxs-lookup"><span data-stu-id="abf0d-158">Click **Create**</span></span>

## <a name="writing-tooredis-cache"></a><span data-ttu-id="abf0d-159">Zápis tooRedis mezipaměti</span><span class="sxs-lookup"><span data-stu-id="abf0d-159">Writing tooRedis Cache</span></span>
<span data-ttu-id="abf0d-160">Nyní jsme vytvořili funkce Azure, který čte z fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="abf0d-160">We have now created an Azure Function that reads from a Service Bus Queue.</span></span> <span data-ttu-id="abf0d-161">Všechno, co je ponechán toodo je, použijte naše funkce toowrite tato data toohello Redis Cache.</span><span class="sxs-lookup"><span data-stu-id="abf0d-161">All that is left toodo is use our Function toowrite this data toohello Redis Cache.</span></span> 

1. <span data-ttu-id="abf0d-162">Vyberte nově vytvořenou **ServiceBusQueueTrigger**a klikněte na tlačítko **funkce nastavení aplikace** na hello pravém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="abf0d-162">Select your newly created **ServiceBusQueueTrigger**, and click **Function app settings** on hello top right corner.</span></span> <span data-ttu-id="abf0d-163">Vyberte **přejít nastavení služby tooApp > Nastavení > Nastavení aplikace**</span><span class="sxs-lookup"><span data-stu-id="abf0d-163">Select **Go tooApp Service Settings > Settings > Application settings**</span></span>
2. <span data-ttu-id="abf0d-164">V části řetězce připojení hello, vytvořte název v hello **název** části.</span><span class="sxs-lookup"><span data-stu-id="abf0d-164">In hello Connection strings section, create a name in hello **Name** section.</span></span> <span data-ttu-id="abf0d-165">Vložte hello primární připojovací řetězec jste našli v hello **vytvoření mezipaměti Redis** krokování s vnořením hello **hodnotu** části.</span><span class="sxs-lookup"><span data-stu-id="abf0d-165">Paste hello primary connection string you found in hello **Create a Redis Cache** step into hello **Value** section.</span></span> <span data-ttu-id="abf0d-166">Vyberte **vlastní** kde uvádí **SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="abf0d-166">Select **Custom** where it says **SQL Database**.</span></span>
3. <span data-ttu-id="abf0d-167">Klikněte na tlačítko **Uložit** v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="abf0d-167">Click **Save** at hello top.</span></span>
   
    ![Snímek obrazovky připojení sběrnice služby](./media/stream-analytics-functions-redis/function-connection-string.png)
4. <span data-ttu-id="abf0d-169">Nyní přejděte zpět nastavení služby toohello aplikace a vyberte **nástroje > aplikace Service Editor (Preview) > na > přejděte**.</span><span class="sxs-lookup"><span data-stu-id="abf0d-169">Now go back toohello App Service Settings and select **Tools > App Service Editor (Preview) > On > Go**.</span></span>
   
    ![Snímek obrazovky připojení sběrnice služby](./media/stream-analytics-functions-redis/app-service-editor.png)
5. <span data-ttu-id="abf0d-171">V editoru podle své volby, vytvořte soubor JSON s názvem **project.json** s hello následující a uložit ho tooyour místní disk.</span><span class="sxs-lookup"><span data-stu-id="abf0d-171">In an editor of your choice, create a JSON file named **project.json** with hello following and save it tooyour local disk.</span></span>
   
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
6. <span data-ttu-id="abf0d-172">Tento soubor nahrajte do kořenového adresáře hello funkce (ne WWWROOT).</span><span class="sxs-lookup"><span data-stu-id="abf0d-172">Upload this file into hello root directory of your function (not WWWROOT).</span></span> <span data-ttu-id="abf0d-173">Měli byste vidět soubor s názvem **project.lock.json** nezobrazí automaticky, potvrzení, že hello Nuget byly naimportovány balíčků "StackExchange.Redis" a "Newtonsoft.Json".</span><span class="sxs-lookup"><span data-stu-id="abf0d-173">You should see a file named **project.lock.json** automatically appear, confirming that hello Nuget packages “StackExchange.Redis” and “Newtonsoft.Json” have been imported.</span></span>
7. <span data-ttu-id="abf0d-174">V hello **run.csx** souboru, nahraďte hello následující kód hello předem generovaného kódu.</span><span class="sxs-lookup"><span data-stu-id="abf0d-174">In hello **run.csx** file, replace hello pre-generated code with hello following code.</span></span> <span data-ttu-id="abf0d-175">Ve funkci lazyConnection hello, nahraďte názvem hello jste vytvořili v kroku 2 ""připojení OBJEVÍ název **ukládání dat do mezipaměti Redis hello**.</span><span class="sxs-lookup"><span data-stu-id="abf0d-175">In hello lazyConnection function, replace “CONN NAME” with hello name you created in step 2 of **Store data into hello Redis cache**.</span></span>

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

## <a name="start-hello-stream-analytics-job"></a><span data-ttu-id="abf0d-176">Spuštění úlohy Stream Analytics hello</span><span class="sxs-lookup"><span data-stu-id="abf0d-176">Start hello Stream Analytics job</span></span>
1. <span data-ttu-id="abf0d-177">Spusťte aplikaci telcodatagen.exe hello.</span><span class="sxs-lookup"><span data-stu-id="abf0d-177">Start hello telcodatagen.exe application.</span></span> <span data-ttu-id="abf0d-178">využití Hello vypadá takto:````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````</span><span class="sxs-lookup"><span data-stu-id="abf0d-178">hello usage is as follows: ````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````</span></span>
2. <span data-ttu-id="abf0d-179">V okně úlohy Stream Analytics hello hello portálu, klikněte na tlačítko **spustit** hello horní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="abf0d-179">From hello Stream Analytics Job blade in hello portal, click **Start** at hello top of hello page.</span></span>
   
    ![Snímek obrazovky spuštění úlohy](./media/stream-analytics-functions-redis/starting-job.png)
3. <span data-ttu-id="abf0d-181">V hello **spuštění úlohy** okno, které se zobrazí, vyberte **nyní** a pak klikněte na tlačítko hello **spustit** tlačítko dole hello úvodní obrazovka.</span><span class="sxs-lookup"><span data-stu-id="abf0d-181">In hello **Start job** blade that appears, select **Now** and then click hello **Start** button at hello bottom of hello screen.</span></span> <span data-ttu-id="abf0d-182">Stav úlohy Hello změny tooStarting a po některé čas tooRunning změny.</span><span class="sxs-lookup"><span data-stu-id="abf0d-182">hello job status changes tooStarting and after some time changes tooRunning.</span></span>
   
    ![Snímek obrazovky výběru čas spuštění úlohy](./media/stream-analytics-functions-redis/start-job-time.png)

## <a name="run-solution-and-check-results"></a><span data-ttu-id="abf0d-184">Spuštění řešení a výsledky kontroly</span><span class="sxs-lookup"><span data-stu-id="abf0d-184">Run solution and check results</span></span>
<span data-ttu-id="abf0d-185">Návratem tooyour **ServiceBusQueueTrigger** stránky, měli byste vidět příkazy protokolu.</span><span class="sxs-lookup"><span data-stu-id="abf0d-185">Going back tooyour **ServiceBusQueueTrigger** page, you should now see log statements.</span></span> <span data-ttu-id="abf0d-186">Tyto protokoly zobrazit něco získali z hello frontou Service Bus, umístí jej do databáze hello a načtených pomocí hello čas jako klíč hello!</span><span class="sxs-lookup"><span data-stu-id="abf0d-186">These logs show that you got something from hello Service Bus Queue, put it into hello database, and fetched it out using hello time as hello key!</span></span>

<span data-ttu-id="abf0d-187">tooverify, že vaše data jsou v vaše mezipaměť Redis přejděte tooyour Redis cache stránku hello nového portálu (jak je uvedeno v předchozí hello [vytvořit Azure Redis Cache](#Create-an-Azure-Redis-Cache) krok) a vyberte konzoly.</span><span class="sxs-lookup"><span data-stu-id="abf0d-187">tooverify that your data is in your Redis cache, go tooyour Redis cache page in hello new portal (as shown in hello preceding [Create an Azure Redis Cache](#Create-an-Azure-Redis-Cache) step) and select Console.</span></span>

<span data-ttu-id="abf0d-188">Teď můžete napsat Redis příkazy tooconfirm ve skutečnosti se data v mezipaměti hello.</span><span class="sxs-lookup"><span data-stu-id="abf0d-188">Now you can write Redis commands tooconfirm that data is in fact in hello cache.</span></span>

![Snímek obrazovky konzola Redis](./media/stream-analytics-functions-redis/redis-console.png)

## <a name="next-steps"></a><span data-ttu-id="abf0d-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="abf0d-190">Next steps</span></span>
<span data-ttu-id="abf0d-191">Jsme nadšení o nové věci hello Azure Functions a Stream analytics můžete provést společně a Věříme, že to odemkne nové možnosti pro vás.</span><span class="sxs-lookup"><span data-stu-id="abf0d-191">We’re excited about hello new things Azure Functions and Stream analytics can do together, and we hope this unlocks new possibilities for you.</span></span> <span data-ttu-id="abf0d-192">Pokud máte jakékoli zpětnou vazbu na co chcete vedle, klidně hello volné toouse [Azure UserVoice lokality](https://feedback.azure.com/forums/270577-stream-analytics).</span><span class="sxs-lookup"><span data-stu-id="abf0d-192">If you have any feedback on what you want next, feel free toouse hello [Azure UserVoice site](https://feedback.azure.com/forums/270577-stream-analytics).</span></span>

<span data-ttu-id="abf0d-193">Pokud jste nový Microsoft Azure, doporučujeme vám tootry ho odhlašování pomocí registrujete [Bezplatný zkušební účet Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="abf0d-193">If you are new Microsoft Azure, we invite you tootry it out by signing up for a [free Azure trial account](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="abf0d-194">Pokud se nový tooStream analýzy, pak doporučujeme příliš[vytvoření vaší první úlohy Stream Analytics](stream-analytics-create-a-job.md).</span><span class="sxs-lookup"><span data-stu-id="abf0d-194">If you are new tooStream Analytics, then we invite you too[create your first Stream Analytics job](stream-analytics-create-a-job.md).</span></span>

<span data-ttu-id="abf0d-195">Pokud potřebujete některé pomoc nebo máte otázky, můžete zveřejnit na [MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) nebo [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) fóra.</span><span class="sxs-lookup"><span data-stu-id="abf0d-195">If you need any help or have questions, post on [MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) or [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) forums.</span></span> 

<span data-ttu-id="abf0d-196">Můžete také zjistit hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="abf0d-196">You can also see hello following resources:</span></span>

* [<span data-ttu-id="abf0d-197">Referenční informace pro vývojáře Azure Functions</span><span class="sxs-lookup"><span data-stu-id="abf0d-197">Azure Functions developer reference</span></span>](../azure-functions/functions-reference.md)
* [<span data-ttu-id="abf0d-198">Azure funkcí jazyka C# referenční informace pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="abf0d-198">Azure Functions C# developer reference</span></span>](../azure-functions/functions-reference-csharp.md)
* [<span data-ttu-id="abf0d-199">Azure funkce F # referenční informace pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="abf0d-199">Azure Functions F# developer reference</span></span>](../azure-functions/functions-reference-fsharp.md)
* [<span data-ttu-id="abf0d-200">Referenční informace pro vývojáře Azure funkce NodeJS</span><span class="sxs-lookup"><span data-stu-id="abf0d-200">Azure Functions NodeJS developer reference</span></span>](../azure-functions/functions-reference.md)
* [<span data-ttu-id="abf0d-201">Azure funkce triggerů a vazeb</span><span class="sxs-lookup"><span data-stu-id="abf0d-201">Azure Functions triggers and bindings</span></span>](../azure-functions/functions-triggers-bindings.md)
* [<span data-ttu-id="abf0d-202">Jak toomonitor Azure mezipaměti Redis</span><span class="sxs-lookup"><span data-stu-id="abf0d-202">How toomonitor Azure Redis Cache</span></span>](../redis-cache/cache-how-to-monitor.md)

<span data-ttu-id="abf0d-203">postupujte podle toostay aktuální na všech hello nejnovější informace a funkce, [ @AzureStreaming ](https://twitter.com/AzureStreaming) na Twitteru.</span><span class="sxs-lookup"><span data-stu-id="abf0d-203">toostay up-to-date on all hello latest news and features, follow [@AzureStreaming](https://twitter.com/AzureStreaming) on Twitter.</span></span>

[fraud-detection]: stream-analytics-real-time-fraud-detection.md
[servicebus-getstarted]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[use-rediscache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md
[functions-getstarted]: ../azure-functions/functions-create-first-azure-function.md
