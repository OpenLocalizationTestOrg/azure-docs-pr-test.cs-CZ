---
title: "Průběžné export telemetrie z Application Insights | Microsoft Docs"
description: "Exportovat data o využití a diagnostiku do úložiště v Microsoft Azure a stažení z ní."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 5b859200-b484-4c98-9d9f-929713f1030c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: bwren
ms.openlocfilehash: 6ac3bda5101593b5ca66b4c9035e2fdac9d1e833
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="export-telemetry-from-application-insights"></a><span data-ttu-id="03766-103">Export telemetrie z Application Insights</span><span class="sxs-lookup"><span data-stu-id="03766-103">Export telemetry from Application Insights</span></span>
<span data-ttu-id="03766-104">Chcete zachovat telemetrie po dobu delší než doba uchování standardní?</span><span class="sxs-lookup"><span data-stu-id="03766-104">Want to keep your telemetry for longer than the standard retention period?</span></span> <span data-ttu-id="03766-105">Nebo ji zpracovat nějakým způsobem specializované?</span><span class="sxs-lookup"><span data-stu-id="03766-105">Or process it in some specialized way?</span></span> <span data-ttu-id="03766-106">Průběžné Export je ideální pro tento.</span><span class="sxs-lookup"><span data-stu-id="03766-106">Continuous Export is ideal for this.</span></span> <span data-ttu-id="03766-107">Události, které vidíte na portálu služby Application Insights je možné exportovat do úložiště v Microsoft Azure ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="03766-107">The events you see in the Application Insights portal can be exported to storage in Microsoft Azure in JSON format.</span></span> <span data-ttu-id="03766-108">Odtud můžete stáhnout vaše data a zápis, ať kódu je třeba zpracovat.</span><span class="sxs-lookup"><span data-stu-id="03766-108">From there you can download your data and write whatever code you need to process it.</span></span>  

<span data-ttu-id="03766-109">Pomocí průběžné exportovat mohou být účtovány dalších poplatků.</span><span class="sxs-lookup"><span data-stu-id="03766-109">Using Continuous Export may incur an additional charge.</span></span> <span data-ttu-id="03766-110">Zkontrolujte vaše [cenový model](http://azure.microsoft.com/pricing/details/application-insights/).</span><span class="sxs-lookup"><span data-stu-id="03766-110">Check your [pricing model](http://azure.microsoft.com/pricing/details/application-insights/).</span></span>

<span data-ttu-id="03766-111">Před nastavením průběžné export existují nějaké alternativy, které můžete vzít v úvahu:</span><span class="sxs-lookup"><span data-stu-id="03766-111">Before you set up continuous export, there are some alternatives you might want to consider:</span></span>

* <span data-ttu-id="03766-112">Tlačítko Export v horní části okna metriky nebo vyhledávání umožňuje přenos tabulky a grafy tabulky aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="03766-112">The Export button at the top of a metrics or search blade lets you transfer tables and charts to an Excel spreadsheet.</span></span>

* <span data-ttu-id="03766-113">[Analýza](app-insights-analytics.md) poskytuje účinný dotazovací jazyk pro telemetrie.</span><span class="sxs-lookup"><span data-stu-id="03766-113">[Analytics](app-insights-analytics.md) provides a powerful query language for telemetry.</span></span> <span data-ttu-id="03766-114">Je také možné exportovat výsledky.</span><span class="sxs-lookup"><span data-stu-id="03766-114">It can also export results.</span></span>
* <span data-ttu-id="03766-115">Pokud se pro díváte [zkoumat data v Power BI](app-insights-export-power-bi.md), můžete to udělat bez použití průběžné exportovat.</span><span class="sxs-lookup"><span data-stu-id="03766-115">If you're looking to [explore your data in Power BI](app-insights-export-power-bi.md), you can do that without using Continuous Export.</span></span>
* <span data-ttu-id="03766-116">[REST API přístup k datům](https://dev.applicationinsights.io/) vám umožní přístup k vaší telemetrie prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="03766-116">The [Data access REST API](https://dev.applicationinsights.io/) lets you access your telemetry programmatically.</span></span>

<span data-ttu-id="03766-117">Po průběžné exportovat zkopíruje data do úložiště (kde může zůstat pro, dokud se vám líbí), bude stále k dispozici ve službě Application Insights pro obvykle [dobu uchování](app-insights-data-retention-privacy.md).</span><span class="sxs-lookup"><span data-stu-id="03766-117">After Continuous Export copies your data to storage (where it can stay for as long as you like), it's still available in Application Insights for the usual [retention period](app-insights-data-retention-privacy.md).</span></span>

## <span data-ttu-id="03766-118"><a name="setup"></a>Vytvoření průběžné exportu</span><span class="sxs-lookup"><span data-stu-id="03766-118"><a name="setup"></a> Create a Continuous Export</span></span>
1. <span data-ttu-id="03766-119">V prostředku Application Insights pro aplikace, otevřete průběžné exportovat a zvolte **přidat**:</span><span class="sxs-lookup"><span data-stu-id="03766-119">In the Application Insights resource for your app, open Continuous Export and choose **Add**:</span></span>

    ![Posuňte se dolů a klikněte na tlačítko průběžné Export](./media/app-insights-export-telemetry/01-export.png)

2. <span data-ttu-id="03766-121">Zvolte telemetrii datové typy, které chcete provést export.</span><span class="sxs-lookup"><span data-stu-id="03766-121">Choose the telemetry data types you want to export.</span></span>

3. <span data-ttu-id="03766-122">Vytvořte nebo vyberte [účtu úložiště Azure](../storage/common/storage-introduction.md) místo, kam chcete data uložit.</span><span class="sxs-lookup"><span data-stu-id="03766-122">Create or select an [Azure storage account](../storage/common/storage-introduction.md) where you want to store the data.</span></span>

    > [!Warning]
    > <span data-ttu-id="03766-123">Ve výchozím umístění úložiště bude nastaveno na stejné zeměpisné oblasti jako prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="03766-123">By default, the storage location will be set to the same geographical region as your Application Insights resource.</span></span> <span data-ttu-id="03766-124">Pokud uchováváte v jiné oblasti, mohou být účtovány poplatky za přenos.</span><span class="sxs-lookup"><span data-stu-id="03766-124">If you store in a different region, you may incur transfer charges.</span></span>

    ![Klikněte na tlačítko Přidat, Export cílový účet úložiště a vytvořit nové úložiště nebo vyberte stávající úložiště](./media/app-insights-export-telemetry/02-add.png)

4. <span data-ttu-id="03766-126">Vytvořte nebo vyberte kontejner v úložišti:</span><span class="sxs-lookup"><span data-stu-id="03766-126">Create or select a container in the storage:</span></span>

    ![Klikněte na tlačítko Vybrat typy událostí](./media/app-insights-export-telemetry/create-container.png)

<span data-ttu-id="03766-128">Po vytvoření export, začne přejdete.</span><span class="sxs-lookup"><span data-stu-id="03766-128">Once you've created your export, it starts going.</span></span> <span data-ttu-id="03766-129">Zobrazí jenom data, která dorazí po vytvoření exportu.</span><span class="sxs-lookup"><span data-stu-id="03766-129">You only get data that arrives after you create the export.</span></span>

<span data-ttu-id="03766-130">Může být zpoždění o jednu hodinu, než se data zobrazí v úložišti.</span><span class="sxs-lookup"><span data-stu-id="03766-130">There can be a delay of about an hour before data appears in the storage.</span></span>

### <a name="to-edit-continuous-export"></a><span data-ttu-id="03766-131">Chcete-li upravit průběžné exportu</span><span class="sxs-lookup"><span data-stu-id="03766-131">To edit continuous export</span></span>

<span data-ttu-id="03766-132">Pokud chcete později změnit typy událostí, stačí upravte exportu:</span><span class="sxs-lookup"><span data-stu-id="03766-132">If you want to change the event types later, just edit the export:</span></span>

![Klikněte na tlačítko Vybrat typy událostí](./media/app-insights-export-telemetry/05-edit.png)

### <a name="to-stop-continuous-export"></a><span data-ttu-id="03766-134">Zastavit průběžné exportu</span><span class="sxs-lookup"><span data-stu-id="03766-134">To stop continuous export</span></span>

<span data-ttu-id="03766-135">Export, klikněte na tlačítko zakázat.</span><span class="sxs-lookup"><span data-stu-id="03766-135">To stop the export, click Disable.</span></span> <span data-ttu-id="03766-136">Když kliknete znovu povolit, export restartuje se nová data.</span><span class="sxs-lookup"><span data-stu-id="03766-136">When you click Enable again, the export will restart with new data.</span></span> <span data-ttu-id="03766-137">Nezískáte data, která dorazila na portálu exportu bylo zakázáno.</span><span class="sxs-lookup"><span data-stu-id="03766-137">You won't get the data that arrived in the portal while export was disabled.</span></span>

<span data-ttu-id="03766-138">K zastavení exportu trvale, odstraňte jej.</span><span class="sxs-lookup"><span data-stu-id="03766-138">To stop the export permanently, delete it.</span></span> <span data-ttu-id="03766-139">Díky tomu nedojde k odstranění dat z úložiště.</span><span class="sxs-lookup"><span data-stu-id="03766-139">Doing so doesn't delete your data from storage.</span></span>

### <a name="cant-add-or-change-an-export"></a><span data-ttu-id="03766-140">Nelze přidat nebo změnit exportu?</span><span class="sxs-lookup"><span data-stu-id="03766-140">Can't add or change an export?</span></span>
* <span data-ttu-id="03766-141">Pokud chcete přidat nebo změnit exportuje, je třeba vlastník, Přispěvatel nebo Application Insights Přispěvatel přístupová práva.</span><span class="sxs-lookup"><span data-stu-id="03766-141">To add or change exports, you need Owner, Contributor or Application Insights Contributor access rights.</span></span> <span data-ttu-id="03766-142">[Další informace o rolích][roles].</span><span class="sxs-lookup"><span data-stu-id="03766-142">[Learn about roles][roles].</span></span>

## <span data-ttu-id="03766-143"><a name="analyze"></a>Jaké události získáte?</span><span class="sxs-lookup"><span data-stu-id="03766-143"><a name="analyze"></a> What events do you get?</span></span>
<span data-ttu-id="03766-144">Exportovaná data je nezpracovaná telemetrická data, které obdržíme z vaší aplikace, s tím rozdílem, že přidáme data o umístění, které jsme vypočítat z IP adresy klienta.</span><span class="sxs-lookup"><span data-stu-id="03766-144">The exported data is the raw telemetry we receive from your application, except that we add location data which we calculate from the client IP address.</span></span>

<span data-ttu-id="03766-145">Data, která byla zahozena podle [vzorkování](app-insights-sampling.md) není součástí exportovaná data.</span><span class="sxs-lookup"><span data-stu-id="03766-145">Data that has been discarded by [sampling](app-insights-sampling.md) is not included in the exported data.</span></span>

<span data-ttu-id="03766-146">Další počítané metriky nejsou zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="03766-146">Other calculated metrics are not included.</span></span> <span data-ttu-id="03766-147">Například nemáte exportu průměrné využití procesoru, ale nezpracovaná telemetrická data, ze kterého průměr se vypočítává exportu.</span><span class="sxs-lookup"><span data-stu-id="03766-147">For example, we don't export average CPU utilisation, but we do export the raw telemetry from which the average is computed.</span></span>

<span data-ttu-id="03766-148">Data také zahrnuje všechny výsledky [testy dostupnosti webu](app-insights-monitor-web-app-availability.md) který jste nastavili.</span><span class="sxs-lookup"><span data-stu-id="03766-148">The data also includes the results of any [availability web tests](app-insights-monitor-web-app-availability.md) that you have set up.</span></span>

> [!NOTE]
> <span data-ttu-id="03766-149">**Vzorkování.**</span><span class="sxs-lookup"><span data-stu-id="03766-149">**Sampling.**</span></span> <span data-ttu-id="03766-150">Pokud vaše aplikace odešle velké množství dat, může funkci vzorkování pracovat a odesílat pouze část telemetrická generovaným.</span><span class="sxs-lookup"><span data-stu-id="03766-150">If your application sends a lot of data, the sampling feature may operate and send only a fraction of the generated telemetry.</span></span> [<span data-ttu-id="03766-151">Přečtěte si další informace o vzorkování.</span><span class="sxs-lookup"><span data-stu-id="03766-151">Learn more about sampling.</span></span>](app-insights-sampling.md)
>
>

## <span data-ttu-id="03766-152"><a name="get"></a>Kontrolovat data</span><span class="sxs-lookup"><span data-stu-id="03766-152"><a name="get"></a> Inspect the data</span></span>
<span data-ttu-id="03766-153">Si můžete prohlédnout úložiště přímo na portálu.</span><span class="sxs-lookup"><span data-stu-id="03766-153">You can inspect the storage directly in the portal.</span></span> <span data-ttu-id="03766-154">Klikněte na tlačítko **Procházet**, vyberte svůj účet úložiště a pak otevřete **kontejnery**.</span><span class="sxs-lookup"><span data-stu-id="03766-154">Click **Browse**, select your storage account, and then open **Containers**.</span></span>

<span data-ttu-id="03766-155">Chcete-li prověřit úložiště Azure v sadě Visual Studio, otevřete **zobrazení**, **Průzkumník cloudu**.</span><span class="sxs-lookup"><span data-stu-id="03766-155">To inspect Azure storage in Visual Studio, open **View**, **Cloud Explorer**.</span></span> <span data-ttu-id="03766-156">(Pokud nemáte tohoto příkazu v nabídce, budete muset nainstalovat sadu Azure SDK: Otevřete **nový projekt** dialogové okno, rozbalte položku Visual C# / cloudu a zvolte **získat Microsoft Azure SDK for .NET**.)</span><span class="sxs-lookup"><span data-stu-id="03766-156">(If you don't have that menu command, you need to install the Azure SDK: Open the **New Project** dialog, expand Visual C#/Cloud and choose **Get Microsoft Azure SDK for .NET**.)</span></span>

<span data-ttu-id="03766-157">Když otevřete váš úložišti objektů blob, uvidíte kontejner s určitou sadu souborů objektů blob.</span><span class="sxs-lookup"><span data-stu-id="03766-157">When you open your blob store, you'll see a container with a set of blob files.</span></span> <span data-ttu-id="03766-158">Identifikátor URI každého souboru odvozená od vašeho název prostředku Application Insights, jeho klíč instrumentace, telemetrie – typ nebo datum a čas.</span><span class="sxs-lookup"><span data-stu-id="03766-158">The URI of each file derived from your Application Insights resource name, its instrumentation key, telemetry-type/date/time.</span></span> <span data-ttu-id="03766-159">(Název prostředku je psaný malými písmeny a klíč instrumentace vynechá pomlčky.)</span><span class="sxs-lookup"><span data-stu-id="03766-159">(The resource name is all lowercase, and the instrumentation key omits dashes.)</span></span>

![Zkontrolujte úložišti objektů blob pomocí vhodného nástroje](./media/app-insights-export-telemetry/04-data.png)

<span data-ttu-id="03766-161">Datum a čas UTC se a jsou při telemetrii byl uložen v úložišti - není čas, který byl vygenerován.</span><span class="sxs-lookup"><span data-stu-id="03766-161">The date and time are UTC and are when the telemetry was deposited in the store - not the time it was generated.</span></span> <span data-ttu-id="03766-162">Proto pokud můžete napsat kód pro stažení dat, ho můžete přesunout lineárně přes data.</span><span class="sxs-lookup"><span data-stu-id="03766-162">So if you write code to download the data, it can move linearly through the data.</span></span>

<span data-ttu-id="03766-163">Tady je formu cesty:</span><span class="sxs-lookup"><span data-stu-id="03766-163">Here's the form of the path:</span></span>

    $"{applicationName}_{instrumentationKey}/{type}/{blobDeliveryTimeUtc:yyyy-MM-dd}/{ blobDeliveryTimeUtc:HH}/{blobId}_{blobCreationTimeUtc:yyyyMMdd_HHmmss}.blob"

<span data-ttu-id="03766-164">kde</span><span class="sxs-lookup"><span data-stu-id="03766-164">Where</span></span>

* <span data-ttu-id="03766-165">`blobCreationTimeUtc`je čas vytvoření objektů blob v interní pracovní úložiště</span><span class="sxs-lookup"><span data-stu-id="03766-165">`blobCreationTimeUtc` is time when blob was created in the internal staging storage</span></span>
* <span data-ttu-id="03766-166">`blobDeliveryTimeUtc`je čas při objekt blob je zkopírovat do cílového úložiště exportu</span><span class="sxs-lookup"><span data-stu-id="03766-166">`blobDeliveryTimeUtc` is the time when blob is copied to the export destination storage</span></span>

## <span data-ttu-id="03766-167"><a name="format"></a>Formát dat</span><span class="sxs-lookup"><span data-stu-id="03766-167"><a name="format"></a> Data format</span></span>
* <span data-ttu-id="03766-168">Každý objekt blob je textový soubor, který obsahuje více ' \n'-separated řádků.</span><span class="sxs-lookup"><span data-stu-id="03766-168">Each blob is a text file that contains multiple '\n'-separated rows.</span></span> <span data-ttu-id="03766-169">Obsahuje telemetrii zpracovaných za časové období zhruba poloviční minuta.</span><span class="sxs-lookup"><span data-stu-id="03766-169">It contains the telemetry processed over a time period of roughly half a minute.</span></span>
* <span data-ttu-id="03766-170">Každý řádek představuje bod telemetrická data například zobrazení požadavku nebo stránky.</span><span class="sxs-lookup"><span data-stu-id="03766-170">Each row represents a telemetry data point such as a request or page view.</span></span>
* <span data-ttu-id="03766-171">Každý řádek je neformátovaný dokument JSON.</span><span class="sxs-lookup"><span data-stu-id="03766-171">Each row is an unformatted JSON document.</span></span> <span data-ttu-id="03766-172">Pokud chcete nacházejí a stare na to, otevřete v sadě Visual Studio a zvolte upravte, Upřesnit, formát souboru:</span><span class="sxs-lookup"><span data-stu-id="03766-172">If you want to sit and stare at it, open it in Visual Studio and choose Edit, Advanced, Format File:</span></span>

![Zobrazení telemetrie pomocí vhodného nástroje](./media/app-insights-export-telemetry/06-json.png)

<span data-ttu-id="03766-174">Dobách trvání jsou v rysky, kde 10 000 značek = 1ms.</span><span class="sxs-lookup"><span data-stu-id="03766-174">Time durations are in ticks, where 10 000 ticks = 1ms.</span></span> <span data-ttu-id="03766-175">Tyto hodnoty například zobrazit čas 1ms odesílat požadavky z prohlížeče, 3ms ji přijmout a 1.8s pro zpracování stránky v prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="03766-175">For example, these values show a time of 1ms to send a request from the browser, 3ms to receive it, and 1.8s to process the page in the browser:</span></span>

    "sendRequest": {"value": 10000.0},
    "receiveRequest": {"value": 30000.0},
    "clientProcess": {"value": 17970000.0}

[<span data-ttu-id="03766-176">Podrobný datový model referenční informace pro vlastnost typů a hodnot.</span><span class="sxs-lookup"><span data-stu-id="03766-176">Detailed data model reference for the property types and values.</span></span>](app-insights-export-data-model.md)

## <a name="processing-the-data"></a><span data-ttu-id="03766-177">Zpracování dat</span><span class="sxs-lookup"><span data-stu-id="03766-177">Processing the data</span></span>
<span data-ttu-id="03766-178">V malém měřítku můžete napsat kód, který vyžádá od sebe vaše data, přečtěte si ho do tabulky a tak dále.</span><span class="sxs-lookup"><span data-stu-id="03766-178">On a small scale, you can write some code to pull apart your data, read it into a spreadsheet, and so on.</span></span> <span data-ttu-id="03766-179">Například:</span><span class="sxs-lookup"><span data-stu-id="03766-179">For example:</span></span>

    private IEnumerable<T> DeserializeMany<T>(string folderName)
    {
      var files = Directory.EnumerateFiles(folderName, "*.blob", SearchOption.AllDirectories);
      foreach (var file in files)
      {
         using (var fileReader = File.OpenText(file))
         {
            string fileContent = fileReader.ReadToEnd();
            IEnumerable<string> entities = fileContent.Split('\n').Where(s => !string.IsNullOrWhiteSpace(s));
            foreach (var entity in entities)
            {
                yield return JsonConvert.DeserializeObject<T>(entity);
            }
         }
      }
    }

<span data-ttu-id="03766-180">Větší ukázky kódu, najdete v části [pomocí rolí pracovního procesu][exportasa].</span><span class="sxs-lookup"><span data-stu-id="03766-180">For a larger code sample, see [using a worker role][exportasa].</span></span>

## <span data-ttu-id="03766-181"><a name="delete"></a>Odstranit stará data</span><span class="sxs-lookup"><span data-stu-id="03766-181"><a name="delete"></a>Delete your old data</span></span>
<span data-ttu-id="03766-182">Upozorňujeme, že jste zodpovědní za správu vaše kapacita úložiště a odstraňování stará data v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="03766-182">Please note that you are responsible for managing your storage capacity and deleting the old data if necessary.</span></span>

## <a name="if-you-regenerate-your-storage-key"></a><span data-ttu-id="03766-183">Pokud byste znovu generovali klíče účtu úložiště...</span><span class="sxs-lookup"><span data-stu-id="03766-183">If you regenerate your storage key...</span></span>
<span data-ttu-id="03766-184">Pokud změníte klíč do úložiště, průběžné export přestanou fungovat.</span><span class="sxs-lookup"><span data-stu-id="03766-184">If you change the key to your storage, continuous export will stop working.</span></span> <span data-ttu-id="03766-185">Uvidíte oznámení v účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="03766-185">You'll see a notification in your Azure account.</span></span>

<span data-ttu-id="03766-186">Otevřete okno průběžné exportovat a upravovat export.</span><span class="sxs-lookup"><span data-stu-id="03766-186">Open the Continuous Export blade and edit your export.</span></span> <span data-ttu-id="03766-187">Upravit cílový exportovat, ale nechte vybrané stejné úložiště.</span><span class="sxs-lookup"><span data-stu-id="03766-187">Edit the Export Destination, but just leave the same storage selected.</span></span> <span data-ttu-id="03766-188">Klikněte na tlačítko OK potvrďte volbu.</span><span class="sxs-lookup"><span data-stu-id="03766-188">Click OK to confirm.</span></span>

![Upravit průběžné exportu, otevření a zavření thí cíl pro export.](./media/app-insights-export-telemetry/07-resetstore.png)

<span data-ttu-id="03766-190">Průběžné exportu se restartuje.</span><span class="sxs-lookup"><span data-stu-id="03766-190">The continuous export will restart.</span></span>

## <a name="export-samples"></a><span data-ttu-id="03766-191">Export – ukázky</span><span class="sxs-lookup"><span data-stu-id="03766-191">Export samples</span></span>

* <span data-ttu-id="03766-192">[Exportovat do SQL pomocí služby Stream Analytics][exportasa]</span><span class="sxs-lookup"><span data-stu-id="03766-192">[Export to SQL using Stream Analytics][exportasa]</span></span>
* [<span data-ttu-id="03766-193">Stream Analytics ukázka 2</span><span class="sxs-lookup"><span data-stu-id="03766-193">Stream Analytics sample 2</span></span>](app-insights-export-stream-analytics.md)

<span data-ttu-id="03766-194">Na větších měřítek, vezměte v úvahu [HDInsight](https://azure.microsoft.com/services/hdinsight/) -clusterů systému Hadoop v cloudu.</span><span class="sxs-lookup"><span data-stu-id="03766-194">On larger scales, consider [HDInsight](https://azure.microsoft.com/services/hdinsight/) - Hadoop clusters in the cloud.</span></span> <span data-ttu-id="03766-195">HDInsight nabízí celou řadu technologií pro správu a analýze velkých objemů dat, a můžete ho použít ke zpracování dat, která byla exportována z Application Insights.</span><span class="sxs-lookup"><span data-stu-id="03766-195">HDInsight provides a variety of technologies for managing and analyzing big data, and you could use it to process data that has been exported from Application Insights.</span></span>

## <a name="q--a"></a><span data-ttu-id="03766-196">Dotazy a odpovědi</span><span class="sxs-lookup"><span data-stu-id="03766-196">Q & A</span></span>
* <span data-ttu-id="03766-197">*Ale všechny, které mají být je jednorázové stažení grafu.*</span><span class="sxs-lookup"><span data-stu-id="03766-197">*But all I want is a one-time download of a chart.*</span></span>  

    <span data-ttu-id="03766-198">Ano, můžete to udělat.</span><span class="sxs-lookup"><span data-stu-id="03766-198">Yes, you can do that.</span></span> <span data-ttu-id="03766-199">V horní části okna klikněte na tlačítko **Export dat**.</span><span class="sxs-lookup"><span data-stu-id="03766-199">At the top of the blade, click **Export Data**.</span></span>
* <span data-ttu-id="03766-200">*Mám nastavit exportu, ale nejsou žádná data v tomto úložišti.*</span><span class="sxs-lookup"><span data-stu-id="03766-200">*I set up an export, but there's no data in my store.*</span></span>

    <span data-ttu-id="03766-201">Application Insights zobrazila všechny telemetrie z vaší aplikace. vzhledem k tomu, že nastavíte exportu?</span><span class="sxs-lookup"><span data-stu-id="03766-201">Did Application Insights receive any telemetry from your app since you set up the export?</span></span> <span data-ttu-id="03766-202">Obdržíte jenom nová data.</span><span class="sxs-lookup"><span data-stu-id="03766-202">You'll only receive new data.</span></span>
* <span data-ttu-id="03766-203">*I při pokusu o nastavení exportu, ale byl odepřen přístup*</span><span class="sxs-lookup"><span data-stu-id="03766-203">*I tried to set up an export, but was denied access*</span></span>

    <span data-ttu-id="03766-204">Pokud vaše organizace vlastní účet, musíte být členem skupiny Vlastníci nebo přispěvatele.</span><span class="sxs-lookup"><span data-stu-id="03766-204">If the account is owned by your organization, you have to be a member of the owners or contributors groups.</span></span>
* <span data-ttu-id="03766-205">*Můžete exportovat přímo do vlastní místní úložiště?*</span><span class="sxs-lookup"><span data-stu-id="03766-205">*Can I export straight to my own on-premises store?*</span></span>

    <span data-ttu-id="03766-206">Ne, je nám líto.</span><span class="sxs-lookup"><span data-stu-id="03766-206">No, sorry.</span></span> <span data-ttu-id="03766-207">Naše export modul v současné době funkční s Azure storage v tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="03766-207">Our export engine currently only works with Azure storage at this time.</span></span>  
* <span data-ttu-id="03766-208">*Je k dispozici žádné omezení množství dat, která jste uložili v tomto úložišti?*</span><span class="sxs-lookup"><span data-stu-id="03766-208">*Is there any limit to the amount of data you put in my store?*</span></span>

    <span data-ttu-id="03766-209">Ne.</span><span class="sxs-lookup"><span data-stu-id="03766-209">No.</span></span> <span data-ttu-id="03766-210">Jsme budete zachovat předání dat dokud neodstraníte exportu.</span><span class="sxs-lookup"><span data-stu-id="03766-210">We'll keep pushing data in until you delete the export.</span></span> <span data-ttu-id="03766-211">Pokud jsme dosáhl vnější limity pro úložiště objektů blob, ale který je poměrně velký budete zastavení.</span><span class="sxs-lookup"><span data-stu-id="03766-211">We'll stop if we hit the outer limits for blob storage, but that's pretty huge.</span></span> <span data-ttu-id="03766-212">Je to na řízení jak velké úložiště používáte.</span><span class="sxs-lookup"><span data-stu-id="03766-212">It's up to you to control how much storage you use.</span></span>  
* <span data-ttu-id="03766-213">*Kolik objekty BLOB může zobrazit v úložišti?*</span><span class="sxs-lookup"><span data-stu-id="03766-213">*How many blobs should I see in the storage?*</span></span>

  * <span data-ttu-id="03766-214">Pro každý datový typ, který jste vybrali pro export do nového objektu blob se vytvoří každou minutu (pokud jsou k dispozici data).</span><span class="sxs-lookup"><span data-stu-id="03766-214">For every data type you selected to export, a new blob is created every minute (if data is available).</span></span>
  * <span data-ttu-id="03766-215">Pro aplikace s intenzivní provoz, navíc další oddíl jednotky jsou přiděleny.</span><span class="sxs-lookup"><span data-stu-id="03766-215">In addition, for applications with high traffic, additional partition units are allocated.</span></span> <span data-ttu-id="03766-216">V takovém případě jednotlivých jednotek vytvoří objekt blob každou minutu.</span><span class="sxs-lookup"><span data-stu-id="03766-216">In this case each unit creates a blob every minute.</span></span>
* <span data-ttu-id="03766-217">*I na můj úložiště znovu vygenerovat klíč nebo změnit název kontejneru a nyní exportu nefunguje.*</span><span class="sxs-lookup"><span data-stu-id="03766-217">*I regenerated the key to my storage or changed the name of the container, and now the export doesn't work.*</span></span>

    <span data-ttu-id="03766-218">Upravte exportu a otevřete okno cílové export.</span><span class="sxs-lookup"><span data-stu-id="03766-218">Edit the export and open the export destination blade.</span></span> <span data-ttu-id="03766-219">Nechte na stejné úložiště jako předtím vybrali a klikněte na tlačítko OK potvrďte volbu.</span><span class="sxs-lookup"><span data-stu-id="03766-219">Leave the same storage selected as before, and click OK to confirm.</span></span> <span data-ttu-id="03766-220">Export se restartuje.</span><span class="sxs-lookup"><span data-stu-id="03766-220">Export will restart.</span></span> <span data-ttu-id="03766-221">Pokud tato změna byla během posledních několik dní, abyste neztratili data.</span><span class="sxs-lookup"><span data-stu-id="03766-221">If the change was within the past few days, you won't lose data.</span></span>
* <span data-ttu-id="03766-222">*Můžete pozastavit exportu?*</span><span class="sxs-lookup"><span data-stu-id="03766-222">*Can I pause the export?*</span></span>

    <span data-ttu-id="03766-223">Ano.</span><span class="sxs-lookup"><span data-stu-id="03766-223">Yes.</span></span> <span data-ttu-id="03766-224">Klikněte na tlačítko zakázat.</span><span class="sxs-lookup"><span data-stu-id="03766-224">Click Disable.</span></span>

## <a name="code-samples"></a><span data-ttu-id="03766-225">Ukázky kódů</span><span class="sxs-lookup"><span data-stu-id="03766-225">Code samples</span></span>

* [<span data-ttu-id="03766-226">Ukázka Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="03766-226">Stream Analytics sample</span></span>](app-insights-export-stream-analytics.md)
* <span data-ttu-id="03766-227">[Exportovat do SQL pomocí služby Stream Analytics][exportasa]</span><span class="sxs-lookup"><span data-stu-id="03766-227">[Export to SQL using Stream Analytics][exportasa]</span></span>
* [<span data-ttu-id="03766-228">Podrobný datový model referenční informace pro vlastnost typů a hodnot.</span><span class="sxs-lookup"><span data-stu-id="03766-228">Detailed data model reference for the property types and values.</span></span>](app-insights-export-data-model.md)

<!--Link references-->

[exportasa]: app-insights-code-sample-export-sql-stream-analytics.md
[roles]: app-insights-resources-roles-access-control.md
