---
title: "aaaIoT v reálném čase datové proudy a Azure Stream Analytics | Microsoft Docs"
description: "Zařízení SensorTag pro IoT, proudy dat, analytické funkce pro analýzu proudů dat a zpracování dat v reálném čase"
keywords: "řešení iot, začínáme s iot"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 3e829055-75ed-469f-91f5-f0dc95046bdb
ms.service: stream-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 422e6b719d0289880aa7f17fdc585e2b768c63d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-stream-analytics-tooprocess-data-from-iot-devices"></a><span data-ttu-id="07ef1-104">Začínáme s Azure Stream Analytics tooprocess daty ze zařízení IoT</span><span class="sxs-lookup"><span data-stu-id="07ef1-104">Get started with Azure Stream Analytics tooprocess data from IoT devices</span></span>
<span data-ttu-id="07ef1-105">V tomto kurzu se dozvíte, jak toocreate datového proudu zpracování logiky toogather data ze zařízení, Internet věcí (IoT).</span><span class="sxs-lookup"><span data-stu-id="07ef1-105">In this tutorial, you will learn how toocreate stream-processing logic toogather data from Internet of Things (IoT) devices.</span></span> <span data-ttu-id="07ef1-106">Jak použijeme případu toodemonstrate reálného, Internet věcí (IoT) pomocí toobuild řešení rychlého a ekonomického.</span><span class="sxs-lookup"><span data-stu-id="07ef1-106">We will use a real-world, Internet of Things (IoT) use case toodemonstrate how toobuild your solution quickly and economically.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="07ef1-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="07ef1-107">Prerequisites</span></span>
* [<span data-ttu-id="07ef1-108">Předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="07ef1-108">Azure subscription</span></span>](https://azure.microsoft.com/pricing/free-trial/)
* <span data-ttu-id="07ef1-109">Ukázkový dotaz a datové soubory ke stažení z webu [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot)</span><span class="sxs-lookup"><span data-stu-id="07ef1-109">Sample query and data files downloadable from [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot)</span></span>

## <a name="scenario"></a><span data-ttu-id="07ef1-110">Scénář</span><span class="sxs-lookup"><span data-stu-id="07ef1-110">Scenario</span></span>
<span data-ttu-id="07ef1-111">Contoso, který je společnost v prostoru hello automatizace průmyslu, má zcela automatizovat svůj výrobní proces.</span><span class="sxs-lookup"><span data-stu-id="07ef1-111">Contoso, which is a company in hello industrial automation space, has completely automated its manufacturing process.</span></span> <span data-ttu-id="07ef1-112">Hello stroje v továrně společnosti má senzorů, které podporují vytváření datových proudů dat v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="07ef1-112">hello machinery in this plant has sensors that are capable of emitting streams of data in real time.</span></span> <span data-ttu-id="07ef1-113">Manažer provozovny v tomto scénáři chce přehledy v reálném čase toohave z hello senzor data toolook pro vzory a provést akce v nich.</span><span class="sxs-lookup"><span data-stu-id="07ef1-113">In this scenario, a production floor manager wants toohave real-time insights from hello sensor data toolook for patterns and take actions on them.</span></span> <span data-ttu-id="07ef1-114">Použijeme hello Stream Analytics dotazu jazyka SAQL () přes hello senzor data toofind zajímavých vzorců z hello, příchozí datový proud.</span><span class="sxs-lookup"><span data-stu-id="07ef1-114">We will use hello Stream Analytics Query Language (SAQL) over hello sensor data toofind interesting patterns from hello incoming stream of data.</span></span>

<span data-ttu-id="07ef1-115">Samotná data jsou generována zařízením Texas Instruments SensorTag.</span><span class="sxs-lookup"><span data-stu-id="07ef1-115">Here data is being generated from a Texas Instruments sensor tag device.</span></span>

![Texas Instruments SensorTag](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-01.jpg)

<span data-ttu-id="07ef1-117">hello datových částí Hello je ve formátu JSON a vypadá hello následující:</span><span class="sxs-lookup"><span data-stu-id="07ef1-117">hello payload of hello data is in JSON format and looks like hello following:</span></span>

    {
        "time": "2016-01-26T20:47:53.0000000",  
        "dspl": "sensorE",  
        "temp": 123,  
        "hmdt": 34  
    }  

<span data-ttu-id="07ef1-118">V podobném scénáři z reálného prostředí by se jednalo o stovky podobných snímačů generujících události jako datový proud.</span><span class="sxs-lookup"><span data-stu-id="07ef1-118">In a real-world scenario, you could have hundreds of these sensors generating events as a stream.</span></span> <span data-ttu-id="07ef1-119">V ideálním případě by zařízení brány by spustit kód toopush tyto události příliš[Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) nebo [Azure IoT Hubs](https://azure.microsoft.com/services/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="07ef1-119">Ideally, a gateway device would run code toopush these events too[Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) or [Azure IoT Hubs](https://azure.microsoft.com/services/iot-hub/).</span></span> <span data-ttu-id="07ef1-120">Úlohu Stream Analytics by ingestování tyto události ze služby Event Hubs a spouštění dotazů analýzu v reálném čase na datové proudy hello.</span><span class="sxs-lookup"><span data-stu-id="07ef1-120">Your Stream Analytics job would ingest these events from Event Hubs and run real-time analytics queries against hello streams.</span></span> <span data-ttu-id="07ef1-121">Potom může odeslat hello tooone výsledky z hello [podporované výstupy](stream-analytics-define-outputs.md).</span><span class="sxs-lookup"><span data-stu-id="07ef1-121">Then, you could send hello results tooone of hello [supported outputs](stream-analytics-define-outputs.md).</span></span>

<span data-ttu-id="07ef1-122">Pro snadnější použití tato příručka Začínáme poskytuje soubor ukázkových dat, která byla zaznamenána skutečnými zařízeními SensorTag.</span><span class="sxs-lookup"><span data-stu-id="07ef1-122">For ease of use, this getting started guide provides a sample data file, which was captured from real sensor tag devices.</span></span> <span data-ttu-id="07ef1-123">Můžete spouštět dotazy na hello ukázkových dat a zobrazte výsledky.</span><span class="sxs-lookup"><span data-stu-id="07ef1-123">You can run queries on hello sample data and see results.</span></span> <span data-ttu-id="07ef1-124">V následujících kurzech se dozvíte, jak tooconnect vaše úlohy tooinputs a výstupy a nasadit je toohello služby Azure.</span><span class="sxs-lookup"><span data-stu-id="07ef1-124">In subsequent tutorials, you will learn how tooconnect your job tooinputs and outputs and deploy them toohello Azure service.</span></span>

## <a name="create-a-stream-analytics-job"></a><span data-ttu-id="07ef1-125">Vytvoření úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="07ef1-125">Create a Stream Analytics job</span></span>
1. <span data-ttu-id="07ef1-126">V hello [portál Azure](http://portal.azure.com), klikněte na znaménko plus hello a pak zadejte **STREAM ANALYTICS** v hello text okno toohello doprava.</span><span class="sxs-lookup"><span data-stu-id="07ef1-126">In hello [Azure portal](http://portal.azure.com), click hello plus sign and then type **STREAM ANALYTICS** in hello text window toohello right.</span></span> <span data-ttu-id="07ef1-127">Potom vyberte **úlohy služby Stream Analytics** v seznamu výsledků hello.</span><span class="sxs-lookup"><span data-stu-id="07ef1-127">Then select **Stream Analytics job** in hello results list.</span></span>
   
    ![Vytvoření nové úlohy Stream Analytics](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)
2. <span data-ttu-id="07ef1-129">Zadejte název jedinečné úlohy a ověřte předplatné hello je hello správné jednu pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="07ef1-129">Enter a unique job name and verify hello subscription is hello correct one for your job.</span></span> <span data-ttu-id="07ef1-130">Potom vytvořte novou skupinu prostředků nebo vyberte existující skupinu v rámci svého předplatného.</span><span class="sxs-lookup"><span data-stu-id="07ef1-130">Then either create a new resource group or select an existing one on your subscription.</span></span>
3. <span data-ttu-id="07ef1-131">Pak vyberte umístění pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="07ef1-131">Then select a location for your job.</span></span> <span data-ttu-id="07ef1-132">Rychlost zpracování a snížení nákladů ve výběru hello přenos dat se doporučuje stejné umístění jako skupina prostředků hello a účet určený úložiště.</span><span class="sxs-lookup"><span data-stu-id="07ef1-132">For speed of processing and reduction of cost in data transfer selecting hello same location as hello resource group and intended storage account is recommended.</span></span>
   
    ![Podrobnosti o vytvoření nové úlohy Stream Analytics](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.png)
   
   > [!NOTE]
   > <span data-ttu-id="07ef1-134">Tento účet úložiště byste měli pro každý region vytvořit pouze jednou.</span><span class="sxs-lookup"><span data-stu-id="07ef1-134">You should create this storage account only once per region.</span></span> <span data-ttu-id="07ef1-135">Toto úložiště bude sdílené napříč všemi úlohami Stream Analytics vytvořenými v příslušné oblasti.</span><span class="sxs-lookup"><span data-stu-id="07ef1-135">This storage will be shared across all Stream Analytics jobs that are created in that region.</span></span>
   > 
   > 
4. <span data-ttu-id="07ef1-136">Pole tooplace hello úlohu na řídicím panelu a pak klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="07ef1-136">Check hello box tooplace your job on your dashboard and then click **CREATE**.</span></span>
   
    ![průběh vytváření úlohy](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03a.png)
5. <span data-ttu-id="07ef1-138">Měli byste vidět, nasazení zahájen' zobrazených v hello pravém horním rohu okna prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="07ef1-138">You should see a 'Deployment started...' displayed in hello top right of your browser window.</span></span> <span data-ttu-id="07ef1-139">Brzy změní okno tooa dokončit, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="07ef1-139">Soon it will change tooa completed window as shown below.</span></span>
   
    ![průběh vytváření úlohy](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03b.png)

### <a name="create-an-azure-stream-analytics-query"></a><span data-ttu-id="07ef1-141">Vytvoření dotazu služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="07ef1-141">Create an Azure Stream Analytics query</span></span>
<span data-ttu-id="07ef1-142">Úlohu po dobu tooopen jej vytvořil a sestavení dotazu.</span><span class="sxs-lookup"><span data-stu-id="07ef1-142">After your job is created it's time tooopen it and build a query.</span></span> <span data-ttu-id="07ef1-143">Kliknutím na dlaždici hello ho můžete snadný přístup k vaší práce.</span><span class="sxs-lookup"><span data-stu-id="07ef1-143">You can easily access your job by clicking hello tile for it.</span></span>

![Dlaždice úlohy](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-04.png)

<span data-ttu-id="07ef1-145">V hello **úlohy topologie** podokně klikněte na tlačítko hello **dotazu** pole toogo toohello editoru dotazů.</span><span class="sxs-lookup"><span data-stu-id="07ef1-145">In hello **Job Topology** pane click hello **QUERY** box toogo toohello Query Editor.</span></span> <span data-ttu-id="07ef1-146">Hello **dotazu** editoru vám umožní tooenter T-SQL dotaz, který provádí transformaci hello přes hello příchozích dat událostí.</span><span class="sxs-lookup"><span data-stu-id="07ef1-146">hello **QUERY** editor allows you tooenter a T-SQL query that performs hello transformation over hello incoming event data.</span></span>

![Pole Dotaz](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

### <a name="query-archive-your-raw-data"></a><span data-ttu-id="07ef1-148">Dotaz: Archivace nezpracovaných dat</span><span class="sxs-lookup"><span data-stu-id="07ef1-148">Query: Archive your raw data</span></span>
<span data-ttu-id="07ef1-149">Hello Nejjednodušším typem dotazu je průchozí dotaz, který archivy všechny vstupní data tooits určené výstup.</span><span class="sxs-lookup"><span data-stu-id="07ef1-149">hello simplest form of query is a pass-through query that archives all input data tooits designated output.</span></span> <span data-ttu-id="07ef1-150">Stáhnout hello ukázkový datový soubor z [Githubu](https://aka.ms/azure-stream-analytics-get-started-iot) tooa umístění v počítači.</span><span class="sxs-lookup"><span data-stu-id="07ef1-150">Download hello sample data file from [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot) tooa location on your computer.</span></span> 

1. <span data-ttu-id="07ef1-151">Vložte hello dotaz ze souboru PassThrough.txt hello.</span><span class="sxs-lookup"><span data-stu-id="07ef1-151">Paste hello query from hello PassThrough.txt file.</span></span> 
   
    ![Vstupní datový proud testu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06.png)
2. <span data-ttu-id="07ef1-153">Klikněte na vstup další tooyour hello třemi tečkami a vyberte **nahrát ukázková data ze souboru** pole.</span><span class="sxs-lookup"><span data-stu-id="07ef1-153">Click hello three dots next tooyour input and select **Upload sample data from file** box.</span></span>
   
    ![Vstupní datový proud testu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06a.png)
3. <span data-ttu-id="07ef1-155">Podokno otevře na hello práva jako výsledek, v ní vyberte hello HelloWorldASA-InputStream.json data z vaší stažené umístění a klikněte na tlačítko **OK** dolnímu hello hello podokna.</span><span class="sxs-lookup"><span data-stu-id="07ef1-155">A pane opens on hello right as a result, in it select hello HelloWorldASA-InputStream.json data file from your downloaded location and click **OK** at hello bottom of hello pane.</span></span>
   
    ![Vstupní datový proud testu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06b.png)
4. <span data-ttu-id="07ef1-157">Pak klikněte na tlačítko hello **testování** zařízení v hello nahoře vlevo oblast okna hello a zpracování dotazu testovací oproti hello ukázkovou datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="07ef1-157">Then click hello **Test** gear in hello top left area of hello window and process your test query against hello sample dataset.</span></span> <span data-ttu-id="07ef1-158">Po dokončení zpracování hello, otevře se okno výsledků níže dotazu.</span><span class="sxs-lookup"><span data-stu-id="07ef1-158">A results window will open below your query as hello processing is complete.</span></span>
   
    ![Výsledky testu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

### <a name="query-filter-hello-data-based-on-a-condition"></a><span data-ttu-id="07ef1-160">Dotaz: Filtrování dat hello na základě podmínky</span><span class="sxs-lookup"><span data-stu-id="07ef1-160">Query: Filter hello data based on a condition</span></span>
<span data-ttu-id="07ef1-161">Nyní si vyzkoušíte toofilter hello výsledků na základě podmínky.</span><span class="sxs-lookup"><span data-stu-id="07ef1-161">Let’s try toofilter hello results based on a condition.</span></span> <span data-ttu-id="07ef1-162">Rádi bychom znali tooshow výsledky pro jenom události, které pocházejí z "sensorA."</span><span class="sxs-lookup"><span data-stu-id="07ef1-162">We would like tooshow results for only those events that come from “sensorA.”</span></span> <span data-ttu-id="07ef1-163">Hello dotaz je v souboru Filtering.txt hello.</span><span class="sxs-lookup"><span data-stu-id="07ef1-163">hello query is in hello Filtering.txt file.</span></span>

![Filtrování datového proudu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

<span data-ttu-id="07ef1-165">Všimněte si, že hello malá a velká písmena dotazu porovnává hodnotu řetězce.</span><span class="sxs-lookup"><span data-stu-id="07ef1-165">Note that hello case-sensitive query compares a string value.</span></span> <span data-ttu-id="07ef1-166">Klikněte na tlačítko hello **Test** zařízení tooexecute hello dotaz znovu.</span><span class="sxs-lookup"><span data-stu-id="07ef1-166">Click hello **Test** gear again tooexecute hello query.</span></span> <span data-ttu-id="07ef1-167">Hello dotaz by měl vrátit řádky 389 z 1860 událostí.</span><span class="sxs-lookup"><span data-stu-id="07ef1-167">hello query should return 389 rows out of 1860 events.</span></span>

![Druhé výsledky výstupu z testu dotazu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-09.png)

### <a name="query-alert-tootrigger-a-business-workflow"></a><span data-ttu-id="07ef1-169">Dotaz: Výstrahy tootrigger pracovní postup společnosti</span><span class="sxs-lookup"><span data-stu-id="07ef1-169">Query: Alert tootrigger a business workflow</span></span>
<span data-ttu-id="07ef1-170">Vytvoříme podrobnější dotaz.</span><span class="sxs-lookup"><span data-stu-id="07ef1-170">Let's make our query more detailed.</span></span> <span data-ttu-id="07ef1-171">Pro každý typ senzor jsme chcete toomonitor průměrnou teplotu okno 30 sekund a zobrazit výsledky pouze v případě, že je hello teplota překročí 100 stupňů.</span><span class="sxs-lookup"><span data-stu-id="07ef1-171">For every type of sensor, we want toomonitor average temperature per 30-second window and display results only if hello average temperature is above 100 degrees.</span></span> <span data-ttu-id="07ef1-172">Jsme zapíše hello následující dotaz a pak klikněte na tlačítko **Test** toosee hello výsledky.</span><span class="sxs-lookup"><span data-stu-id="07ef1-172">We will write hello following query and then click **Test** toosee hello results.</span></span> <span data-ttu-id="07ef1-173">Hello dotaz je v souboru ThresholdAlerting.txt hello.</span><span class="sxs-lookup"><span data-stu-id="07ef1-173">hello query is in hello ThresholdAlerting.txt file.</span></span>

![30sekundový dotaz filtru](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

<span data-ttu-id="07ef1-175">Měli byste vidět výsledky, které obsahují pouze 245 řádků a názvy senzorů kde hello teplota je vyšší než 100.</span><span class="sxs-lookup"><span data-stu-id="07ef1-175">You should now see results that contain only 245 rows and names of sensors where hello average temperate is greater than 100.</span></span> <span data-ttu-id="07ef1-176">Tento dotaz skupiny hello datového proudu událostí podle **hodnoty dspl**, což je název senzoru hello, než **Přeskakujícího okna** 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="07ef1-176">This query groups hello stream of events by **dspl**, which is hello sensor name, over a **Tumbling Window** of 30 seconds.</span></span> <span data-ttu-id="07ef1-177">Dočasných dotazů musí stavu, jakým způsobem se bude tooprogress čas.</span><span class="sxs-lookup"><span data-stu-id="07ef1-177">Temporal queries must state how we want time tooprogress.</span></span> <span data-ttu-id="07ef1-178">Pomocí hello **TIMESTAMP BY** klauzuli jsme určili hello **OUTPUTTIME** časy tooassociate sloupec se všechny dočasné výpočty.</span><span class="sxs-lookup"><span data-stu-id="07ef1-178">By using hello **TIMESTAMP BY** clause, we have specified hello **OUTPUTTIME** column tooassociate times with all temporal calculations.</span></span> <span data-ttu-id="07ef1-179">Podrobné informace najdete v tématu hello MSDN články o [Správa času](https://msdn.microsoft.com/library/azure/mt582045.aspx) a [Oddílová funkce](https://msdn.microsoft.com/library/azure/dn835019.aspx).</span><span class="sxs-lookup"><span data-stu-id="07ef1-179">For detailed information, read hello MSDN articles about [Time Management](https://msdn.microsoft.com/library/azure/mt582045.aspx) and [Windowing functions](https://msdn.microsoft.com/library/azure/dn835019.aspx).</span></span>

### <a name="query-detect-absence-of-events"></a><span data-ttu-id="07ef1-180">Dotaz: Zjištění neexistence událostí</span><span class="sxs-lookup"><span data-stu-id="07ef1-180">Query: Detect absence of events</span></span>
<span data-ttu-id="07ef1-181">Jak můžete jsme napsat dotaz toofind chybějících vstupní události?</span><span class="sxs-lookup"><span data-stu-id="07ef1-181">How can we write a query toofind a lack of input events?</span></span> <span data-ttu-id="07ef1-182">Umožňuje najít hello poslední čas, kdy senzor odesílají data a pak události pro hello příští minuty neodeslal.</span><span class="sxs-lookup"><span data-stu-id="07ef1-182">Let’s find hello last time that a sensor sent data and then did not send events for hello next minute.</span></span> <span data-ttu-id="07ef1-183">Hello dotaz je v souboru AbsenseOfEvent.txt hello.</span><span class="sxs-lookup"><span data-stu-id="07ef1-183">hello query is in hello AbsenseOfEvent.txt file.</span></span>

![Zjištění neexistence událostí](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

<span data-ttu-id="07ef1-185">Tady používáme **LEVÉ vnější** připojení toohello stejný datový proud (spojení sama na sebe).</span><span class="sxs-lookup"><span data-stu-id="07ef1-185">Here we use a **LEFT OUTER** join toohello same data stream (self-join).</span></span> <span data-ttu-id="07ef1-186">Při použití příkazu **INNER JOIN** se vrátí výsledek, pouze když je nalezena shoda.</span><span class="sxs-lookup"><span data-stu-id="07ef1-186">For an **INNER** join, a result is returned only when a match is found.</span></span>  <span data-ttu-id="07ef1-187">Pro **LEVÉ vnější** připojení, pokud je nalezena shoda, událost z levé strany spojení hello hello řádek, který má hodnotu NULL pro všechny hello sloupce hello pravé straně, je vrácena.</span><span class="sxs-lookup"><span data-stu-id="07ef1-187">For a **LEFT OUTER** join, if an event from hello left side of hello join is unmatched, a row that has NULL for all hello columns of hello right side is returned.</span></span> <span data-ttu-id="07ef1-188">Tato technika je velmi užitečná toofind absence událostí.</span><span class="sxs-lookup"><span data-stu-id="07ef1-188">This technique is very useful toofind an absence of events.</span></span> <span data-ttu-id="07ef1-189">Další informace o příkazu [JOIN](https://msdn.microsoft.com/library/azure/dn835026.aspx) najdete v dokumentaci MSDN.</span><span class="sxs-lookup"><span data-stu-id="07ef1-189">See our MSDN documentation for more information about [JOIN](https://msdn.microsoft.com/library/azure/dn835026.aspx).</span></span>

## <a name="conclusion"></a><span data-ttu-id="07ef1-190">Závěr</span><span class="sxs-lookup"><span data-stu-id="07ef1-190">Conclusion</span></span>
<span data-ttu-id="07ef1-191">Hello účelu tohoto kurzu je toodemonstrate jak toowrite různé Stream Analytics Query Language dotazy a zjistit, výsledkem hello prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="07ef1-191">hello purpose of this tutorial is toodemonstrate how toowrite different Stream Analytics Query Language queries and see results in hello browser.</span></span> <span data-ttu-id="07ef1-192">Je to však jenom začátek.</span><span class="sxs-lookup"><span data-stu-id="07ef1-192">However, this is just getting started.</span></span> <span data-ttu-id="07ef1-193">Pomocí Stream Analytics toho můžete dělat mnohem víc.</span><span class="sxs-lookup"><span data-stu-id="07ef1-193">You can do so much more with Stream Analytics.</span></span> <span data-ttu-id="07ef1-194">Stream Analytics podporuje celou řadu vstupy a výstupy a můžete i pomocí funkcí v Azure Machine Learning toomake ho robustní nástroj pro analýzu datových proudů.</span><span class="sxs-lookup"><span data-stu-id="07ef1-194">Stream Analytics supports a variety of inputs and outputs and can even use functions in Azure Machine Learning toomake it a robust tool for analyzing data streams.</span></span> <span data-ttu-id="07ef1-195">Další informace o Stream Analytics tooexplore můžete spustit pomocí našich [mapa kurzů](https://azure.microsoft.com/documentation/learning-paths/stream-analytics/).</span><span class="sxs-lookup"><span data-stu-id="07ef1-195">You can start tooexplore more about Stream Analytics by using our [learning map](https://azure.microsoft.com/documentation/learning-paths/stream-analytics/).</span></span> <span data-ttu-id="07ef1-196">Další informace o tom, toowrite dotazy, přečtěte si článek hello o [běžné typy dotazů](stream-analytics-stream-analytics-query-patterns.md).</span><span class="sxs-lookup"><span data-stu-id="07ef1-196">For more information about how toowrite queries, read hello article about [common query patterns](stream-analytics-stream-analytics-query-patterns.md).</span></span>
