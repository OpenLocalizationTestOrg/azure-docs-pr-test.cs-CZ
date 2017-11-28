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
# <a name="how-to-store-data-from-azure-stream-analytics-in-an-azure-redis-cache-using-azure-functions"></a><span data-ttu-id="3ff69-104">Tom, jak ukládat data z Azure Stream Analytics v Azure Redis Cache pomocí Azure Functions</span><span class="sxs-lookup"><span data-stu-id="3ff69-104">How to store data from Azure Stream Analytics in an Azure Redis Cache using Azure Functions</span></span>
<span data-ttu-id="3ff69-105">Azure Stream Analytics umožňuje rychle vyvíjet a nasadit řešení nízkými náklady a získáte přehled o ze zařízení, senzorů, infrastruktury a aplikací nebo jakýkoli proud dat v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="3ff69-105">Azure Stream Analytics lets you rapidly develop and deploy low-cost solutions to gain real-time insights from devices, sensors, infrastructure, and applications, or any stream of data.</span></span> <span data-ttu-id="3ff69-106">Umožňuje různé případy použití, jako je například v reálném čase správy a monitorování, příkaz a řízení, odhalování podvodů, připojené aut a mnoho dalších.</span><span class="sxs-lookup"><span data-stu-id="3ff69-106">It enables various use cases such as real-time management and monitoring, command and control, fraud detection, connected cars and many more.</span></span> <span data-ttu-id="3ff69-107">V mnoha takových scénářů můžete k ukládání dat do úložiště distribuovaných datech například Azure Redis cache výstupem Azure Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="3ff69-107">In many such scenarios, you may want to store data outputted by Azure Stream Analytics into a distributed data store such as an Azure Redis cache.</span></span>

<span data-ttu-id="3ff69-108">Předpokládejme, že jsou součástí telecommunications společnosti.</span><span class="sxs-lookup"><span data-stu-id="3ff69-108">Suppose you are part of a telecommunications company.</span></span> <span data-ttu-id="3ff69-109">Pokoušíte se odhalování podvodů SIM, kde více volání pocházejících z stejnou identitu, zároveň čas, ale v různých geograficky umístění.</span><span class="sxs-lookup"><span data-stu-id="3ff69-109">You are trying to detect SIM fraud where multiple calls coming from the same identity, at the same time, but in different geographically locations.</span></span> <span data-ttu-id="3ff69-110">Můžete se musí s ukládáním do mezipaměti Azure Redis všechny potenciální podvodné telefonní hovory.</span><span class="sxs-lookup"><span data-stu-id="3ff69-110">You are tasked with storing all the potential fraudulent phone calls in an Azure Redis cache.</span></span> <span data-ttu-id="3ff69-111">V tomto blogu poskytujeme pokyny na tom, jak můžete snadno dokončení úkolu.</span><span class="sxs-lookup"><span data-stu-id="3ff69-111">In this blog, we provide guidance on how you can easily complete your task.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="3ff69-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3ff69-112">Prerequisites</span></span>
<span data-ttu-id="3ff69-113">Dokončení [odhalování podvodů v reálném čase] [ fraud-detection] návod pro ASA</span><span class="sxs-lookup"><span data-stu-id="3ff69-113">Complete the [Real-time Fraud Detection][fraud-detection] walk-through for ASA</span></span>

## <a name="architecture-overview"></a><span data-ttu-id="3ff69-114">Přehled architektury</span><span class="sxs-lookup"><span data-stu-id="3ff69-114">Architecture Overview</span></span>
![Snímek obrazovky architektura](./media/stream-analytics-functions-redis/architecture-overview.png)

<span data-ttu-id="3ff69-116">Jak je vidět na předchozím obrázku, Stream Analytics umožňuje streamování vstupní data na dotaz a odesílají do výstupu.</span><span class="sxs-lookup"><span data-stu-id="3ff69-116">As shown in the preceding figure, Stream Analytics allows streaming input data to be queried and sent to an output.</span></span> <span data-ttu-id="3ff69-117">Výstup podle, můžete Azure Functions potom aktivovat nějaký typ události.</span><span class="sxs-lookup"><span data-stu-id="3ff69-117">Based on the output, Azure Functions can then trigger some type of event.</span></span> 

<span data-ttu-id="3ff69-118">V tomto blogu zaměříme na Azure Functions součástí tohoto kanálu, nebo přesněji řečeno aktivuje událost, která ukládá podvodné data do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="3ff69-118">In this blog, we focus on the Azure Functions part of this pipeline, or more specifically the triggering of an event that stores fraudulent data into the cache.</span></span>
<span data-ttu-id="3ff69-119">Po dokončení [odhalování podvodů v reálném čase] [ fraud-detection] kurzu máte vstup (centra událostí), dotaz a výstup (úložiště objektů blob) už nakonfigurovaná a spuštěná.</span><span class="sxs-lookup"><span data-stu-id="3ff69-119">After completing the [Real-time Fraud Detection][fraud-detection] tutorial, you have an input (an event hub), a query, and an output (blob storage) already configured and running.</span></span> <span data-ttu-id="3ff69-120">V tomto blogu Změníme výstup místo toho použít frontu sběrnice.</span><span class="sxs-lookup"><span data-stu-id="3ff69-120">In this blog, we change the output to use a Service Bus Queue instead.</span></span> <span data-ttu-id="3ff69-121">Potom jsme funkce Azure připojit do této fronty.</span><span class="sxs-lookup"><span data-stu-id="3ff69-121">After that, we connect an Azure Function to this queue.</span></span> 

## <a name="create-and-connect-a-service-bus-queue-output"></a><span data-ttu-id="3ff69-122">Vytvořit a připojit výstupní fronty Service Bus</span><span class="sxs-lookup"><span data-stu-id="3ff69-122">Create and connect a Service Bus Queue output</span></span>
<span data-ttu-id="3ff69-123">Chcete-li vytvořit frontu sběrnice, postupujte podle kroků 1 a 2 v části .NET v [začít pracovat s fronty služby Service Bus][servicebus-getstarted].</span><span class="sxs-lookup"><span data-stu-id="3ff69-123">To create a Service Bus Queue, follow steps 1 and 2 of the .NET section in [Get Started with Service Bus Queues][servicebus-getstarted].</span></span>
<span data-ttu-id="3ff69-124">Teď umožňuje připojit fronty do úlohy Stream Analytics, která byla vytvořena ve starší návodu zjišťování podvodů.</span><span class="sxs-lookup"><span data-stu-id="3ff69-124">Now let's connect the queue to the Stream Analytics job that was created in the earlier fraud detection walk-through.</span></span>

1. <span data-ttu-id="3ff69-125">V portálu Azure přejděte do **výstupy** okno úloha a vyberte **přidat** v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="3ff69-125">In the Azure portal, go to the **Outputs** blade of your job and select **Add** at the top of the page.</span></span>
   
    ![Přidání výstupy](./media/stream-analytics-functions-redis/adding-outputs.png)
2. <span data-ttu-id="3ff69-127">Zvolte **frontou Service Bus** jako **jímky** a postupujte podle pokynů na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="3ff69-127">Choose **Service Bus Queue** as the **Sink** and follow the instructions on the screen.</span></span> <span data-ttu-id="3ff69-128">Nezapomeňte vybrat obor názvů frontou Service Bus, kterou jste vytvořili v [začít pracovat s fronty služby Service Bus][servicebus-getstarted].</span><span class="sxs-lookup"><span data-stu-id="3ff69-128">Be sure to choose the namespace of the Service Bus Queue you created in [Get Started with Service Bus Queues][servicebus-getstarted].</span></span> <span data-ttu-id="3ff69-129">Až skončíte, klikněte na tlačítko "vpravo".</span><span class="sxs-lookup"><span data-stu-id="3ff69-129">Click the "right" button when you are finished.</span></span>
3. <span data-ttu-id="3ff69-130">Zadejte následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="3ff69-130">Specify the following values:</span></span>
   
   * <span data-ttu-id="3ff69-131">**Formát serializátor událostí**: JSON</span><span class="sxs-lookup"><span data-stu-id="3ff69-131">**Event Serializer Format**: JSON</span></span>
   * <span data-ttu-id="3ff69-132">**Kódování**: UTF8</span><span class="sxs-lookup"><span data-stu-id="3ff69-132">**Encoding**: UTF8</span></span>
   * <span data-ttu-id="3ff69-133">**Formát**: řádcích</span><span class="sxs-lookup"><span data-stu-id="3ff69-133">**FORMAT**: Line separated</span></span>
4. <span data-ttu-id="3ff69-134">Klikněte **vytvořit** tlačítko Přidat tento zdroj a ověřte, zda Stream Analytics můžete úspěšně připojit k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="3ff69-134">Click the **Create** button to add this source and to verify that Stream Analytics can successfully connect to the storage account.</span></span>
5. <span data-ttu-id="3ff69-135">V **dotazu** kartě, nahraďte následujícím kódem aktuálního dotazu.</span><span class="sxs-lookup"><span data-stu-id="3ff69-135">In the **Query** tab, replace the current query with the following.</span></span> <span data-ttu-id="3ff69-136">Nahraďte * [svůj název služby SBĚRNICE] * s název výstupu, který jste vytvořili v kroku 3.</span><span class="sxs-lookup"><span data-stu-id="3ff69-136">Replace *[YOUR SERVICE BUS NAME] * with the output name you created in step 3.</span></span> 
   
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

## <a name="create-an-azure-redis-cache"></a><span data-ttu-id="3ff69-137">Vytvoření Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="3ff69-137">Create an Azure Redis Cache</span></span>
<span data-ttu-id="3ff69-138">Vytvoření služby Azure Redis cache pomocí následujících části .NET v [postup použití Azure Redis Cache] [ use-rediscache] dokud není zavolána části ***konfigurace klientů mezipaměti***.</span><span class="sxs-lookup"><span data-stu-id="3ff69-138">Create an Azure Redis cache by following the .NET section in [How to Use Azure Redis Cache][use-rediscache] until the section called ***Configure the cache clients***.</span></span>
<span data-ttu-id="3ff69-139">Po dokončení máte nové mezipaměti Redis.</span><span class="sxs-lookup"><span data-stu-id="3ff69-139">Once complete, you have a new Redis Cache.</span></span> <span data-ttu-id="3ff69-140">V části **všechna nastavení**, vyberte **přístupové klíče** a poznamenejte si ***primární připojovací řetězec***.</span><span class="sxs-lookup"><span data-stu-id="3ff69-140">Under **All settings**, select **Access keys** and note down the ***Primary connection string***.</span></span>

![Snímek obrazovky architektura](./media/stream-analytics-functions-redis/redis-cache-keys.png)

## <a name="create-an-azure-function"></a><span data-ttu-id="3ff69-142">Vytvoření Azure funkce</span><span class="sxs-lookup"><span data-stu-id="3ff69-142">Create an Azure Function</span></span>
<span data-ttu-id="3ff69-143">Postupujte podle [vytvoření první funkce Azure] [ functions-getstarted] kurzu Začínáme s Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="3ff69-143">Follow [Create your first Azure Function][functions-getstarted] tutorial to get started with Azure Functions.</span></span> <span data-ttu-id="3ff69-144">Pokud již máte Azure funkce, které chcete použít, pak pokračujte [zápis k Redis Cache](#Writing-to-Redis-Cache)</span><span class="sxs-lookup"><span data-stu-id="3ff69-144">If you already have an Azure function you would like to use, then skip ahead to [Writing to Redis Cache](#Writing-to-Redis-Cache)</span></span>

1. <span data-ttu-id="3ff69-145">Na portálu vyberte aplikační služby z levé navigaci a potom klikněte na název aplikace Azure funkce získat na funkce aplikace Web.</span><span class="sxs-lookup"><span data-stu-id="3ff69-145">In the portal, select App Services from the left-hand navigation, then click your Azure function app name to get to the Function's app website.</span></span>
    <span data-ttu-id="3ff69-146">![Snímek obrazovky seznamu funkce služby aplikací](./media/stream-analytics-functions-redis/app-services-function-list.png)</span><span class="sxs-lookup"><span data-stu-id="3ff69-146">![Screenshot of app services function list](./media/stream-analytics-functions-redis/app-services-function-list.png)</span></span>
2. <span data-ttu-id="3ff69-147">Klikněte na tlačítko **novou funkci > ServiceBusQueueTrigger – C#**.</span><span class="sxs-lookup"><span data-stu-id="3ff69-147">Click **New Function > ServiceBusQueueTrigger – C#**.</span></span> <span data-ttu-id="3ff69-148">Pro následující pole postupujte podle těchto pokynů:</span><span class="sxs-lookup"><span data-stu-id="3ff69-148">For the following fields, follow these instructions:</span></span>
   
   * <span data-ttu-id="3ff69-149">**Název fronty**: stejný název jako název, který jste zadali při vytvoření fronty v [začít pracovat s fronty služby Service Bus] [ servicebus-getstarted] (nikoli název služby service bus).</span><span class="sxs-lookup"><span data-stu-id="3ff69-149">**Queue name**: The same name as the name you entered when you created the queue in [Get Started with Service Bus Queues][servicebus-getstarted] (not the name of the service bus).</span></span> <span data-ttu-id="3ff69-150">Ujistěte se, že používáte fronty, která je připojena k výstupu Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="3ff69-150">Make sure you use the queue that is connected to the Stream Analytics output.</span></span>
   * <span data-ttu-id="3ff69-151">**Připojení k Service Bus**: vyberte **přidat připojovací řetězec**.</span><span class="sxs-lookup"><span data-stu-id="3ff69-151">**Service Bus connection**: Select **Add a connection string**.</span></span> <span data-ttu-id="3ff69-152">Chcete-li najít připojovací řetězec, přejděte na portálu classic, vyberte **Service Bus**, sběrnici služby, který jste vytvořili, a **informace o připojení** v dolní části obrazovky.</span><span class="sxs-lookup"><span data-stu-id="3ff69-152">To find the connection string, go to the classic portal, select **Service Bus**, the service bus you created, and **CONNECTION INFORMATION** at the bottom of the screen.</span></span> <span data-ttu-id="3ff69-153">Ujistěte se, že jste na obrazovce hlavní na této stránce.</span><span class="sxs-lookup"><span data-stu-id="3ff69-153">Make sure you are on the main screen on this page.</span></span> <span data-ttu-id="3ff69-154">Zkopírujte a vložte připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="3ff69-154">Copy and paste the connection string.</span></span> <span data-ttu-id="3ff69-155">Nebojte se, že zadat libovolný název připojení.</span><span class="sxs-lookup"><span data-stu-id="3ff69-155">Feel free to enter any connection name.</span></span>
     
       ![Snímek obrazovky připojení sběrnice služby](./media/stream-analytics-functions-redis/servicebus-connection.png)
   * <span data-ttu-id="3ff69-157">**AccessRights**: Zvolte **spravovat**</span><span class="sxs-lookup"><span data-stu-id="3ff69-157">**AccessRights**: Choose **Manage**</span></span>
3. <span data-ttu-id="3ff69-158">Klikněte na **Vytvořit**</span><span class="sxs-lookup"><span data-stu-id="3ff69-158">Click **Create**</span></span>

## <a name="writing-to-redis-cache"></a><span data-ttu-id="3ff69-159">Zápis do mezipaměti Redis</span><span class="sxs-lookup"><span data-stu-id="3ff69-159">Writing to Redis Cache</span></span>
<span data-ttu-id="3ff69-160">Nyní jsme vytvořili funkce Azure, který čte z fronty Service Bus.</span><span class="sxs-lookup"><span data-stu-id="3ff69-160">We have now created an Azure Function that reads from a Service Bus Queue.</span></span> <span data-ttu-id="3ff69-161">Je již zbývá udělat, použijte naše funkce zapsat tato data do mezipaměti Redis.</span><span class="sxs-lookup"><span data-stu-id="3ff69-161">All that is left to do is use our Function to write this data to the Redis Cache.</span></span> 

1. <span data-ttu-id="3ff69-162">Vyberte nově vytvořenou **ServiceBusQueueTrigger**a klikněte na tlačítko **funkce nastavení aplikace** v pravém horním rohu.</span><span class="sxs-lookup"><span data-stu-id="3ff69-162">Select your newly created **ServiceBusQueueTrigger**, and click **Function app settings** on the top right corner.</span></span> <span data-ttu-id="3ff69-163">Vyberte **přejděte do nastavení aplikace služby > Nastavení > Nastavení aplikace**</span><span class="sxs-lookup"><span data-stu-id="3ff69-163">Select **Go to App Service Settings > Settings > Application settings**</span></span>
2. <span data-ttu-id="3ff69-164">V části řetězce připojení vytvořit název v **název** části.</span><span class="sxs-lookup"><span data-stu-id="3ff69-164">In the Connection strings section, create a name in the **Name** section.</span></span> <span data-ttu-id="3ff69-165">Vložte primární připojovací řetězec, který jste získali v **vytvoření mezipaměti Redis** zanořte se do **hodnotu** části.</span><span class="sxs-lookup"><span data-stu-id="3ff69-165">Paste the primary connection string you found in the **Create a Redis Cache** step into the **Value** section.</span></span> <span data-ttu-id="3ff69-166">Vyberte **vlastní** kde uvádí **SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="3ff69-166">Select **Custom** where it says **SQL Database**.</span></span>
3. <span data-ttu-id="3ff69-167">Klikněte na tlačítko **Uložit** v horní části.</span><span class="sxs-lookup"><span data-stu-id="3ff69-167">Click **Save** at the top.</span></span>
   
    ![Snímek obrazovky připojení sběrnice služby](./media/stream-analytics-functions-redis/function-connection-string.png)
4. <span data-ttu-id="3ff69-169">Nyní přejděte zpět na službu nastavení aplikace a vyberte **nástroje > aplikace Service Editor (Preview) > na > přejděte**.</span><span class="sxs-lookup"><span data-stu-id="3ff69-169">Now go back to the App Service Settings and select **Tools > App Service Editor (Preview) > On > Go**.</span></span>
   
    ![Snímek obrazovky připojení sběrnice služby](./media/stream-analytics-functions-redis/app-service-editor.png)
5. <span data-ttu-id="3ff69-171">V editoru podle své volby, vytvořte soubor JSON s názvem **project.json** následujícím textem a uložte ho na místní disk.</span><span class="sxs-lookup"><span data-stu-id="3ff69-171">In an editor of your choice, create a JSON file named **project.json** with the following and save it to your local disk.</span></span>
   
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
6. <span data-ttu-id="3ff69-172">Tento soubor nahrajte do kořenového adresáře funkce (ne WWWROOT).</span><span class="sxs-lookup"><span data-stu-id="3ff69-172">Upload this file into the root directory of your function (not WWWROOT).</span></span> <span data-ttu-id="3ff69-173">Měli byste vidět soubor s názvem **project.lock.json** automaticky zobrazí potvrzení, zda byly naimportovány balíčky Nuget "StackExchange.Redis" a "Newtonsoft.Json".</span><span class="sxs-lookup"><span data-stu-id="3ff69-173">You should see a file named **project.lock.json** automatically appear, confirming that the Nuget packages “StackExchange.Redis” and “Newtonsoft.Json” have been imported.</span></span>
7. <span data-ttu-id="3ff69-174">V **run.csx** souboru, předem generovaného kódu nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="3ff69-174">In the **run.csx** file, replace the pre-generated code with the following code.</span></span> <span data-ttu-id="3ff69-175">Ve funkci lazyConnection nahraďte názvem, který jste vytvořili v kroku 2 ""připojení OBJEVÍ název **ukládání dat do mezipaměti Redis**.</span><span class="sxs-lookup"><span data-stu-id="3ff69-175">In the lazyConnection function, replace “CONN NAME” with the name you created in step 2 of **Store data into the Redis cache**.</span></span>

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

## <a name="start-the-stream-analytics-job"></a><span data-ttu-id="3ff69-176">Spustit úlohu služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="3ff69-176">Start the Stream Analytics job</span></span>
1. <span data-ttu-id="3ff69-177">Spusťte aplikaci telcodatagen.exe.</span><span class="sxs-lookup"><span data-stu-id="3ff69-177">Start the telcodatagen.exe application.</span></span> <span data-ttu-id="3ff69-178">Využití je následující:````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````</span><span class="sxs-lookup"><span data-stu-id="3ff69-178">The usage is as follows: ````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````</span></span>
2. <span data-ttu-id="3ff69-179">V okně úlohy Stream Analytics na portálu klikněte na tlačítko **spustit** v horní části stránky.</span><span class="sxs-lookup"><span data-stu-id="3ff69-179">From the Stream Analytics Job blade in the portal, click **Start** at the top of the page.</span></span>
   
    ![Snímek obrazovky spuštění úlohy](./media/stream-analytics-functions-redis/starting-job.png)
3. <span data-ttu-id="3ff69-181">V **spuštění úlohy** okno, které se zobrazí, vyberte **nyní** a pak klikněte na tlačítko **spustit** tlačítko v dolní části obrazovky.</span><span class="sxs-lookup"><span data-stu-id="3ff69-181">In the **Start job** blade that appears, select **Now** and then click the **Start** button at the bottom of the screen.</span></span> <span data-ttu-id="3ff69-182">Změny stavu úlohy počáteční a po některé změny času ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="3ff69-182">The job status changes to Starting and after some time changes to Running.</span></span>
   
    ![Snímek obrazovky výběru čas spuštění úlohy](./media/stream-analytics-functions-redis/start-job-time.png)

## <a name="run-solution-and-check-results"></a><span data-ttu-id="3ff69-184">Spuštění řešení a výsledky kontroly</span><span class="sxs-lookup"><span data-stu-id="3ff69-184">Run solution and check results</span></span>
<span data-ttu-id="3ff69-185">Po návratu do vaší **ServiceBusQueueTrigger** stránky, měli byste vidět příkazy protokolu.</span><span class="sxs-lookup"><span data-stu-id="3ff69-185">Going back to your **ServiceBusQueueTrigger** page, you should now see log statements.</span></span> <span data-ttu-id="3ff69-186">Tyto protokoly zobrazit něco získali z frontou Service Bus, umístí jej do databáze a načtených pomocí čas jako klíč!</span><span class="sxs-lookup"><span data-stu-id="3ff69-186">These logs show that you got something from the Service Bus Queue, put it into the database, and fetched it out using the time as the key!</span></span>

<span data-ttu-id="3ff69-187">Pokud chcete ověřit, že vaše data jsou v mezipaměti Redis, přejděte na stránku mezipaměti Redis v nového portálu (jak je znázorněno v předchozím [vytvořit Azure Redis Cache](#Create-an-Azure-Redis-Cache) krok) a vyberte konzoly.</span><span class="sxs-lookup"><span data-stu-id="3ff69-187">To verify that your data is in your Redis cache, go to your Redis cache page in the new portal (as shown in the preceding [Create an Azure Redis Cache](#Create-an-Azure-Redis-Cache) step) and select Console.</span></span>

<span data-ttu-id="3ff69-188">Teď můžete napsat příkazy Redis k potvrzení, že data jsou ve skutečnosti v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="3ff69-188">Now you can write Redis commands to confirm that data is in fact in the cache.</span></span>

![Snímek obrazovky konzola Redis](./media/stream-analytics-functions-redis/redis-console.png)

## <a name="next-steps"></a><span data-ttu-id="3ff69-190">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3ff69-190">Next steps</span></span>
<span data-ttu-id="3ff69-191">Jsme nadšení o nové věci Azure Functions a Stream analytics můžete provést společně a Věříme, že to odemkne nové možnosti pro vás.</span><span class="sxs-lookup"><span data-stu-id="3ff69-191">We’re excited about the new things Azure Functions and Stream analytics can do together, and we hope this unlocks new possibilities for you.</span></span> <span data-ttu-id="3ff69-192">Pokud máte jakékoli zpětnou vazbu na co chcete vedle, klidně používat [Azure UserVoice lokality](https://feedback.azure.com/forums/270577-stream-analytics).</span><span class="sxs-lookup"><span data-stu-id="3ff69-192">If you have any feedback on what you want next, feel free to use the [Azure UserVoice site](https://feedback.azure.com/forums/270577-stream-analytics).</span></span>

<span data-ttu-id="3ff69-193">Pokud jste nový Microsoft Azure, doporučujeme vám vyzkoušet odhlašování pomocí registrujete [Bezplatný zkušební účet Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3ff69-193">If you are new Microsoft Azure, we invite you to try it out by signing up for a [free Azure trial account](https://azure.microsoft.com/pricing/free-trial/).</span></span> <span data-ttu-id="3ff69-194">Pokud jsou pro vás nové do služby Stream Analytics, pak doporučujeme [vytvoření vaší první úlohy Stream Analytics](stream-analytics-create-a-job.md).</span><span class="sxs-lookup"><span data-stu-id="3ff69-194">If you are new to Stream Analytics, then we invite you to [create your first Stream Analytics job](stream-analytics-create-a-job.md).</span></span>

<span data-ttu-id="3ff69-195">Pokud potřebujete některé pomoc nebo máte otázky, můžete zveřejnit na [MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) nebo [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) fóra.</span><span class="sxs-lookup"><span data-stu-id="3ff69-195">If you need any help or have questions, post on [MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) or [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) forums.</span></span> 

<span data-ttu-id="3ff69-196">Můžete také najdete v následujících zdrojích informací:</span><span class="sxs-lookup"><span data-stu-id="3ff69-196">You can also see the following resources:</span></span>

* [<span data-ttu-id="3ff69-197">Referenční informace pro vývojáře Azure Functions</span><span class="sxs-lookup"><span data-stu-id="3ff69-197">Azure Functions developer reference</span></span>](../azure-functions/functions-reference.md)
* [<span data-ttu-id="3ff69-198">Azure funkcí jazyka C# referenční informace pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="3ff69-198">Azure Functions C# developer reference</span></span>](../azure-functions/functions-reference-csharp.md)
* [<span data-ttu-id="3ff69-199">Azure funkce F # referenční informace pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="3ff69-199">Azure Functions F# developer reference</span></span>](../azure-functions/functions-reference-fsharp.md)
* [<span data-ttu-id="3ff69-200">Referenční informace pro vývojáře Azure funkce NodeJS</span><span class="sxs-lookup"><span data-stu-id="3ff69-200">Azure Functions NodeJS developer reference</span></span>](../azure-functions/functions-reference.md)
* [<span data-ttu-id="3ff69-201">Azure funkce triggerů a vazeb</span><span class="sxs-lookup"><span data-stu-id="3ff69-201">Azure Functions triggers and bindings</span></span>](../azure-functions/functions-triggers-bindings.md)
* [<span data-ttu-id="3ff69-202">Tom, jak monitorovat Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="3ff69-202">How to monitor Azure Redis Cache</span></span>](../redis-cache/cache-how-to-monitor.md)

<span data-ttu-id="3ff69-203">Udržovat přehled na nejnovější novinky a funkce, postupujte podle [ @AzureStreaming ](https://twitter.com/AzureStreaming) na Twitteru.</span><span class="sxs-lookup"><span data-stu-id="3ff69-203">To stay up-to-date on all the latest news and features, follow [@AzureStreaming](https://twitter.com/AzureStreaming) on Twitter.</span></span>

[fraud-detection]: stream-analytics-real-time-fraud-detection.md
[servicebus-getstarted]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[use-rediscache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md
[functions-getstarted]: ../azure-functions/functions-create-first-azure-function.md
