---
title: aaaContinuous export telemetrie z Application Insights | Microsoft Docs
description: "Export dat toostorage diagnostiky a použití ve službě Microsoft Azure a stažení z ní."
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
ms.openlocfilehash: be9ed7e05922c1c8186df9ca4e642862adaa5fd0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="export-telemetry-from-application-insights"></a><span data-ttu-id="4824f-103">Export telemetrie z Application Insights</span><span class="sxs-lookup"><span data-stu-id="4824f-103">Export telemetry from Application Insights</span></span>
<span data-ttu-id="4824f-104">Chcete tookeep telemetrie po dobu delší než doba uchování standardní hello?</span><span class="sxs-lookup"><span data-stu-id="4824f-104">Want tookeep your telemetry for longer than hello standard retention period?</span></span> <span data-ttu-id="4824f-105">Nebo ji zpracovat nějakým způsobem specializované?</span><span class="sxs-lookup"><span data-stu-id="4824f-105">Or process it in some specialized way?</span></span> <span data-ttu-id="4824f-106">Průběžné Export je ideální pro tento.</span><span class="sxs-lookup"><span data-stu-id="4824f-106">Continuous Export is ideal for this.</span></span> <span data-ttu-id="4824f-107">Hello události, které vidíte na portálu služby Application Insights hello může být exportovaný toostorage v Microsoft Azure ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="4824f-107">hello events you see in hello Application Insights portal can be exported toostorage in Microsoft Azure in JSON format.</span></span> <span data-ttu-id="4824f-108">Odtud můžete stáhnout vaše data a zápis, ať kódu můžete potřebovat tooprocess ho.</span><span class="sxs-lookup"><span data-stu-id="4824f-108">From there you can download your data and write whatever code you need tooprocess it.</span></span>  

<span data-ttu-id="4824f-109">Pomocí průběžné exportovat mohou být účtovány dalších poplatků.</span><span class="sxs-lookup"><span data-stu-id="4824f-109">Using Continuous Export may incur an additional charge.</span></span> <span data-ttu-id="4824f-110">Zkontrolujte vaše [cenový model](http://azure.microsoft.com/pricing/details/application-insights/).</span><span class="sxs-lookup"><span data-stu-id="4824f-110">Check your [pricing model](http://azure.microsoft.com/pricing/details/application-insights/).</span></span>

<span data-ttu-id="4824f-111">Před nastavením průběžné export neexistují nějaké alternativy, můžete chtít tooconsider:</span><span class="sxs-lookup"><span data-stu-id="4824f-111">Before you set up continuous export, there are some alternatives you might want tooconsider:</span></span>

* <span data-ttu-id="4824f-112">tlačítko Exportovat Hello hello horní části okna metriky nebo vyhledávání umožňuje přenos tabulku aplikace Excel tooan tabulky a grafy.</span><span class="sxs-lookup"><span data-stu-id="4824f-112">hello Export button at hello top of a metrics or search blade lets you transfer tables and charts tooan Excel spreadsheet.</span></span>

* <span data-ttu-id="4824f-113">[Analýza](app-insights-analytics.md) poskytuje účinný dotazovací jazyk pro telemetrie.</span><span class="sxs-lookup"><span data-stu-id="4824f-113">[Analytics](app-insights-analytics.md) provides a powerful query language for telemetry.</span></span> <span data-ttu-id="4824f-114">Je také možné exportovat výsledky.</span><span class="sxs-lookup"><span data-stu-id="4824f-114">It can also export results.</span></span>
* <span data-ttu-id="4824f-115">Pokud se díváte příliš[zkoumat data v Power BI](app-insights-export-power-bi.md), můžete to udělat bez použití průběžné exportovat.</span><span class="sxs-lookup"><span data-stu-id="4824f-115">If you're looking too[explore your data in Power BI](app-insights-export-power-bi.md), you can do that without using Continuous Export.</span></span>
* <span data-ttu-id="4824f-116">Hello [REST API přístup k datům](https://dev.applicationinsights.io/) vám umožní přístup k vaší telemetrie prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="4824f-116">hello [Data access REST API](https://dev.applicationinsights.io/) lets you access your telemetry programmatically.</span></span>

<span data-ttu-id="4824f-117">Po průběžné exportovat kopie vaše data toostorage (kde může zůstat pro, dokud se vám líbí), bude stále k dispozici ve službě Application Insights hello obvyklé [dobu uchování](app-insights-data-retention-privacy.md).</span><span class="sxs-lookup"><span data-stu-id="4824f-117">After Continuous Export copies your data toostorage (where it can stay for as long as you like), it's still available in Application Insights for hello usual [retention period](app-insights-data-retention-privacy.md).</span></span>

## <span data-ttu-id="4824f-118"><a name="setup"></a>Vytvoření průběžné exportu</span><span class="sxs-lookup"><span data-stu-id="4824f-118"><a name="setup"></a> Create a Continuous Export</span></span>
1. <span data-ttu-id="4824f-119">V hello prostředek Application Insights pro aplikace, otevřete průběžné exportovat a zvolte **přidat**:</span><span class="sxs-lookup"><span data-stu-id="4824f-119">In hello Application Insights resource for your app, open Continuous Export and choose **Add**:</span></span>

    ![Posuňte se dolů a klikněte na tlačítko průběžné Export](./media/app-insights-export-telemetry/01-export.png)

2. <span data-ttu-id="4824f-121">Zvolte hello telemetrie datové typy chcete tooexport.</span><span class="sxs-lookup"><span data-stu-id="4824f-121">Choose hello telemetry data types you want tooexport.</span></span>

3. <span data-ttu-id="4824f-122">Vytvořte nebo vyberte [účtu úložiště Azure](../storage/common/storage-introduction.md) místo toostore hello data.</span><span class="sxs-lookup"><span data-stu-id="4824f-122">Create or select an [Azure storage account](../storage/common/storage-introduction.md) where you want toostore hello data.</span></span>

    > [!Warning]
    > <span data-ttu-id="4824f-123">Ve výchozím nastavení, bude umístění úložiště hello nastavena toohello stejné zeměpisné oblasti jako prostředek Application Insights.</span><span class="sxs-lookup"><span data-stu-id="4824f-123">By default, hello storage location will be set toohello same geographical region as your Application Insights resource.</span></span> <span data-ttu-id="4824f-124">Pokud uchováváte v jiné oblasti, mohou být účtovány poplatky za přenos.</span><span class="sxs-lookup"><span data-stu-id="4824f-124">If you store in a different region, you may incur transfer charges.</span></span>

    ![Klikněte na tlačítko Přidat, Export cílový účet úložiště a vytvořit nové úložiště nebo vyberte stávající úložiště](./media/app-insights-export-telemetry/02-add.png)

4. <span data-ttu-id="4824f-126">Vytvořte nebo vyberte kontejner v úložišti hello:</span><span class="sxs-lookup"><span data-stu-id="4824f-126">Create or select a container in hello storage:</span></span>

    ![Klikněte na tlačítko Vybrat typy událostí](./media/app-insights-export-telemetry/create-container.png)

<span data-ttu-id="4824f-128">Po vytvoření export, začne přejdete.</span><span class="sxs-lookup"><span data-stu-id="4824f-128">Once you've created your export, it starts going.</span></span> <span data-ttu-id="4824f-129">Zobrazí jenom data, která dorazí po vytvoření hello export.</span><span class="sxs-lookup"><span data-stu-id="4824f-129">You only get data that arrives after you create hello export.</span></span>

<span data-ttu-id="4824f-130">Může být zpoždění o jednu hodinu, než se data zobrazí v úložišti hello.</span><span class="sxs-lookup"><span data-stu-id="4824f-130">There can be a delay of about an hour before data appears in hello storage.</span></span>

### <a name="tooedit-continuous-export"></a><span data-ttu-id="4824f-131">průběžné export tooedit</span><span class="sxs-lookup"><span data-stu-id="4824f-131">tooedit continuous export</span></span>

<span data-ttu-id="4824f-132">Pokud chcete typů událostí toochange hello později, upravte hello export:</span><span class="sxs-lookup"><span data-stu-id="4824f-132">If you want toochange hello event types later, just edit hello export:</span></span>

![Klikněte na tlačítko Vybrat typy událostí](./media/app-insights-export-telemetry/05-edit.png)

### <a name="toostop-continuous-export"></a><span data-ttu-id="4824f-134">průběžné export toostop</span><span class="sxs-lookup"><span data-stu-id="4824f-134">toostop continuous export</span></span>

<span data-ttu-id="4824f-135">toostop hello export, klikněte na tlačítko zakázat.</span><span class="sxs-lookup"><span data-stu-id="4824f-135">toostop hello export, click Disable.</span></span> <span data-ttu-id="4824f-136">Když kliknete znovu povolit, hello export restartuje se nová data.</span><span class="sxs-lookup"><span data-stu-id="4824f-136">When you click Enable again, hello export will restart with new data.</span></span> <span data-ttu-id="4824f-137">Nebude získat hello data, které byly přijaty portálu hello exportu bylo zakázáno.</span><span class="sxs-lookup"><span data-stu-id="4824f-137">You won't get hello data that arrived in hello portal while export was disabled.</span></span>

<span data-ttu-id="4824f-138">toostop hello export trvale, odstraňte jej.</span><span class="sxs-lookup"><span data-stu-id="4824f-138">toostop hello export permanently, delete it.</span></span> <span data-ttu-id="4824f-139">Díky tomu nedojde k odstranění dat z úložiště.</span><span class="sxs-lookup"><span data-stu-id="4824f-139">Doing so doesn't delete your data from storage.</span></span>

### <a name="cant-add-or-change-an-export"></a><span data-ttu-id="4824f-140">Nelze přidat nebo změnit exportu?</span><span class="sxs-lookup"><span data-stu-id="4824f-140">Can't add or change an export?</span></span>
* <span data-ttu-id="4824f-141">Exportuje tooadd nebo změnit, je třeba vlastník, Přispěvatel nebo Application Insights Přispěvatel přístupová práva.</span><span class="sxs-lookup"><span data-stu-id="4824f-141">tooadd or change exports, you need Owner, Contributor or Application Insights Contributor access rights.</span></span> <span data-ttu-id="4824f-142">[Další informace o rolích][roles].</span><span class="sxs-lookup"><span data-stu-id="4824f-142">[Learn about roles][roles].</span></span>

## <span data-ttu-id="4824f-143"><a name="analyze"></a>Jaké události získáte?</span><span class="sxs-lookup"><span data-stu-id="4824f-143"><a name="analyze"></a> What events do you get?</span></span>
<span data-ttu-id="4824f-144">Hello exportovaná data je nezpracovaná telemetrická data hello, které obdržíme z vaší aplikace, s tím rozdílem, že přidáme data o umístění, které jsme vypočítat z IP adresy klienta hello.</span><span class="sxs-lookup"><span data-stu-id="4824f-144">hello exported data is hello raw telemetry we receive from your application, except that we add location data which we calculate from hello client IP address.</span></span>

<span data-ttu-id="4824f-145">Data, která byla zahozena podle [vzorkování](app-insights-sampling.md) není součástí hello exportovat data.</span><span class="sxs-lookup"><span data-stu-id="4824f-145">Data that has been discarded by [sampling](app-insights-sampling.md) is not included in hello exported data.</span></span>

<span data-ttu-id="4824f-146">Další počítané metriky nejsou zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="4824f-146">Other calculated metrics are not included.</span></span> <span data-ttu-id="4824f-147">Například nemáte exportu průměrné využití procesoru, ale hello nezpracovaná telemetrická data, ze kterého hello průměr se vypočítává exportu.</span><span class="sxs-lookup"><span data-stu-id="4824f-147">For example, we don't export average CPU utilisation, but we do export hello raw telemetry from which hello average is computed.</span></span>

<span data-ttu-id="4824f-148">Hello data také obsahují hello výsledky všech [testy dostupnosti webu](app-insights-monitor-web-app-availability.md) který jste nastavili.</span><span class="sxs-lookup"><span data-stu-id="4824f-148">hello data also includes hello results of any [availability web tests](app-insights-monitor-web-app-availability.md) that you have set up.</span></span>

> [!NOTE]
> <span data-ttu-id="4824f-149">**Vzorkování.**</span><span class="sxs-lookup"><span data-stu-id="4824f-149">**Sampling.**</span></span> <span data-ttu-id="4824f-150">Pokud vaše aplikace odešle velké množství dat, hello vzorkování funkce může pracovat a odesílat pouze část hello generované telemetrie.</span><span class="sxs-lookup"><span data-stu-id="4824f-150">If your application sends a lot of data, hello sampling feature may operate and send only a fraction of hello generated telemetry.</span></span> [<span data-ttu-id="4824f-151">Přečtěte si další informace o vzorkování.</span><span class="sxs-lookup"><span data-stu-id="4824f-151">Learn more about sampling.</span></span>](app-insights-sampling.md)
>
>

## <span data-ttu-id="4824f-152"><a name="get"></a>Zkontrolujte hello data</span><span class="sxs-lookup"><span data-stu-id="4824f-152"><a name="get"></a> Inspect hello data</span></span>
<span data-ttu-id="4824f-153">Si můžete prohlédnout hello úložiště přímo na portálu hello.</span><span class="sxs-lookup"><span data-stu-id="4824f-153">You can inspect hello storage directly in hello portal.</span></span> <span data-ttu-id="4824f-154">Klikněte na tlačítko **Procházet**, vyberte svůj účet úložiště a pak otevřete **kontejnery**.</span><span class="sxs-lookup"><span data-stu-id="4824f-154">Click **Browse**, select your storage account, and then open **Containers**.</span></span>

<span data-ttu-id="4824f-155">tooinspect úložiště Azure v sadě Visual Studio otevřete **zobrazení**, **Průzkumník cloudu**.</span><span class="sxs-lookup"><span data-stu-id="4824f-155">tooinspect Azure storage in Visual Studio, open **View**, **Cloud Explorer**.</span></span> <span data-ttu-id="4824f-156">(Pokud nemáte tohoto příkazu v nabídce, je třeba tooinstall hello Azure SDK: Otevřete hello **nový projekt** dialogové okno, rozbalte položku Visual C# / cloudu a zvolte **získat Microsoft Azure SDK for .NET**.)</span><span class="sxs-lookup"><span data-stu-id="4824f-156">(If you don't have that menu command, you need tooinstall hello Azure SDK: Open hello **New Project** dialog, expand Visual C#/Cloud and choose **Get Microsoft Azure SDK for .NET**.)</span></span>

<span data-ttu-id="4824f-157">Když otevřete váš úložišti objektů blob, uvidíte kontejner s určitou sadu souborů objektů blob.</span><span class="sxs-lookup"><span data-stu-id="4824f-157">When you open your blob store, you'll see a container with a set of blob files.</span></span> <span data-ttu-id="4824f-158">Hello URI každého souboru odvozená od vašeho název prostředku Application Insights, jeho klíč instrumentace, telemetrie – typ nebo datum a čas.</span><span class="sxs-lookup"><span data-stu-id="4824f-158">hello URI of each file derived from your Application Insights resource name, its instrumentation key, telemetry-type/date/time.</span></span> <span data-ttu-id="4824f-159">(název prostředku hello je psaný malými písmeny a klíč instrumentace hello vynechá pomlčky.)</span><span class="sxs-lookup"><span data-stu-id="4824f-159">(hello resource name is all lowercase, and hello instrumentation key omits dashes.)</span></span>

![Zkontrolujte hello úložišti objektů blob pomocí vhodného nástroje](./media/app-insights-export-telemetry/04-data.png)

<span data-ttu-id="4824f-161">Hello datum a čas UTC se a jsou při hello telemetrie byl uložen v úložišti hello – není hello čas byl vygenerován.</span><span class="sxs-lookup"><span data-stu-id="4824f-161">hello date and time are UTC and are when hello telemetry was deposited in hello store - not hello time it was generated.</span></span> <span data-ttu-id="4824f-162">Takže pokud napíšete kód toodownload hello data, můžete lineárně přesunout prostřednictvím hello data.</span><span class="sxs-lookup"><span data-stu-id="4824f-162">So if you write code toodownload hello data, it can move linearly through hello data.</span></span>

<span data-ttu-id="4824f-163">Tady je hello formu cesty hello:</span><span class="sxs-lookup"><span data-stu-id="4824f-163">Here's hello form of hello path:</span></span>

    $"{applicationName}_{instrumentationKey}/{type}/{blobDeliveryTimeUtc:yyyy-MM-dd}/{ blobDeliveryTimeUtc:HH}/{blobId}_{blobCreationTimeUtc:yyyyMMdd_HHmmss}.blob"

<span data-ttu-id="4824f-164">kde</span><span class="sxs-lookup"><span data-stu-id="4824f-164">Where</span></span>

* <span data-ttu-id="4824f-165">`blobCreationTimeUtc`je čas, kdy objektu blob byla vytvořena v hello interní pracovní úložiště</span><span class="sxs-lookup"><span data-stu-id="4824f-165">`blobCreationTimeUtc` is time when blob was created in hello internal staging storage</span></span>
* <span data-ttu-id="4824f-166">`blobDeliveryTimeUtc`je čas hello po zkopírovaný toohello export cílového úložiště objektů blob</span><span class="sxs-lookup"><span data-stu-id="4824f-166">`blobDeliveryTimeUtc` is hello time when blob is copied toohello export destination storage</span></span>

## <span data-ttu-id="4824f-167"><a name="format"></a>Formát dat</span><span class="sxs-lookup"><span data-stu-id="4824f-167"><a name="format"></a> Data format</span></span>
* <span data-ttu-id="4824f-168">Každý objekt blob je textový soubor, který obsahuje více ' \n'-separated řádků.</span><span class="sxs-lookup"><span data-stu-id="4824f-168">Each blob is a text file that contains multiple '\n'-separated rows.</span></span> <span data-ttu-id="4824f-169">Obsahuje telemetrii hello zpracovaných za časové období zhruba poloviční minuta.</span><span class="sxs-lookup"><span data-stu-id="4824f-169">It contains hello telemetry processed over a time period of roughly half a minute.</span></span>
* <span data-ttu-id="4824f-170">Každý řádek představuje bod telemetrická data například zobrazení požadavku nebo stránky.</span><span class="sxs-lookup"><span data-stu-id="4824f-170">Each row represents a telemetry data point such as a request or page view.</span></span>
* <span data-ttu-id="4824f-171">Každý řádek je neformátovaný dokument JSON.</span><span class="sxs-lookup"><span data-stu-id="4824f-171">Each row is an unformatted JSON document.</span></span> <span data-ttu-id="4824f-172">Pokud chcete toosit a stare na to, otevřete v sadě Visual Studio a zvolte upravte, Upřesnit, formát souboru:</span><span class="sxs-lookup"><span data-stu-id="4824f-172">If you want toosit and stare at it, open it in Visual Studio and choose Edit, Advanced, Format File:</span></span>

![Zobrazení telemetrie hello pomocí vhodného nástroje](./media/app-insights-export-telemetry/06-json.png)

<span data-ttu-id="4824f-174">Dobách trvání jsou v rysky, kde 10 000 značek = 1ms.</span><span class="sxs-lookup"><span data-stu-id="4824f-174">Time durations are in ticks, where 10 000 ticks = 1ms.</span></span> <span data-ttu-id="4824f-175">Například tyto hodnoty zobrazit čas 1ms toosend žádost od hello prohlížeče, 3ms tooreceive a 1.8s tooprocess hello stránku v prohlížeči hello:</span><span class="sxs-lookup"><span data-stu-id="4824f-175">For example, these values show a time of 1ms toosend a request from hello browser, 3ms tooreceive it, and 1.8s tooprocess hello page in hello browser:</span></span>

    "sendRequest": {"value": 10000.0},
    "receiveRequest": {"value": 30000.0},
    "clientProcess": {"value": 17970000.0}

[<span data-ttu-id="4824f-176">Podrobný datový model referenční informace pro hello typy a hodnoty vlastností.</span><span class="sxs-lookup"><span data-stu-id="4824f-176">Detailed data model reference for hello property types and values.</span></span>](app-insights-export-data-model.md)

## <a name="processing-hello-data"></a><span data-ttu-id="4824f-177">Zpracování dat hello</span><span class="sxs-lookup"><span data-stu-id="4824f-177">Processing hello data</span></span>
<span data-ttu-id="4824f-178">V malém měřítku můžete napsat některé toopull kódu od sebe vaše data, přečtěte si ho do tabulky a tak dále.</span><span class="sxs-lookup"><span data-stu-id="4824f-178">On a small scale, you can write some code toopull apart your data, read it into a spreadsheet, and so on.</span></span> <span data-ttu-id="4824f-179">Například:</span><span class="sxs-lookup"><span data-stu-id="4824f-179">For example:</span></span>

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

<span data-ttu-id="4824f-180">Větší ukázky kódu, najdete v části [pomocí rolí pracovního procesu][exportasa].</span><span class="sxs-lookup"><span data-stu-id="4824f-180">For a larger code sample, see [using a worker role][exportasa].</span></span>

## <span data-ttu-id="4824f-181"><a name="delete"></a>Odstranit stará data</span><span class="sxs-lookup"><span data-stu-id="4824f-181"><a name="delete"></a>Delete your old data</span></span>
<span data-ttu-id="4824f-182">Upozorňujeme, že jste zodpovědní za správu vaše kapacita úložiště a odstraňování starých dat hello v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="4824f-182">Please note that you are responsible for managing your storage capacity and deleting hello old data if necessary.</span></span>

## <a name="if-you-regenerate-your-storage-key"></a><span data-ttu-id="4824f-183">Pokud byste znovu generovali klíče účtu úložiště...</span><span class="sxs-lookup"><span data-stu-id="4824f-183">If you regenerate your storage key...</span></span>
<span data-ttu-id="4824f-184">Pokud změníte hello klíče tooyour úložiště, průběžné export přestanou fungovat.</span><span class="sxs-lookup"><span data-stu-id="4824f-184">If you change hello key tooyour storage, continuous export will stop working.</span></span> <span data-ttu-id="4824f-185">Uvidíte oznámení v účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="4824f-185">You'll see a notification in your Azure account.</span></span>

<span data-ttu-id="4824f-186">Otevřete okno hello průběžné exportovat a upravovat export.</span><span class="sxs-lookup"><span data-stu-id="4824f-186">Open hello Continuous Export blade and edit your export.</span></span> <span data-ttu-id="4824f-187">Upravit hello exportovat cílové, ale nechte hello vybrané stejné úložiště.</span><span class="sxs-lookup"><span data-stu-id="4824f-187">Edit hello Export Destination, but just leave hello same storage selected.</span></span> <span data-ttu-id="4824f-188">Klikněte na tlačítko OK tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="4824f-188">Click OK tooconfirm.</span></span>

![Upravit hello průběžné exportovat, otevírají a zavírají thí cíl exportu.](./media/app-insights-export-telemetry/07-resetstore.png)

<span data-ttu-id="4824f-190">průběžné export Hello se restartuje.</span><span class="sxs-lookup"><span data-stu-id="4824f-190">hello continuous export will restart.</span></span>

## <a name="export-samples"></a><span data-ttu-id="4824f-191">Export – ukázky</span><span class="sxs-lookup"><span data-stu-id="4824f-191">Export samples</span></span>

* <span data-ttu-id="4824f-192">[Export tooSQL pomocí služby Stream Analytics][exportasa]</span><span class="sxs-lookup"><span data-stu-id="4824f-192">[Export tooSQL using Stream Analytics][exportasa]</span></span>
* [<span data-ttu-id="4824f-193">Stream Analytics ukázka 2</span><span class="sxs-lookup"><span data-stu-id="4824f-193">Stream Analytics sample 2</span></span>](app-insights-export-stream-analytics.md)

<span data-ttu-id="4824f-194">Na větších měřítek, vezměte v úvahu [HDInsight](https://azure.microsoft.com/services/hdinsight/) -clusterů systému Hadoop v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="4824f-194">On larger scales, consider [HDInsight](https://azure.microsoft.com/services/hdinsight/) - Hadoop clusters in hello cloud.</span></span> <span data-ttu-id="4824f-195">HDInsight nabízí celou řadu technologií pro správu a analýze velkých objemů dat, a můžete ji použít tooprocess data, která byla exportována z Application Insights.</span><span class="sxs-lookup"><span data-stu-id="4824f-195">HDInsight provides a variety of technologies for managing and analyzing big data, and you could use it tooprocess data that has been exported from Application Insights.</span></span>

## <a name="q--a"></a><span data-ttu-id="4824f-196">Dotazy a odpovědi</span><span class="sxs-lookup"><span data-stu-id="4824f-196">Q & A</span></span>
* <span data-ttu-id="4824f-197">*Ale všechny, které mají být je jednorázové stažení grafu.*</span><span class="sxs-lookup"><span data-stu-id="4824f-197">*But all I want is a one-time download of a chart.*</span></span>  

    <span data-ttu-id="4824f-198">Ano, můžete to udělat.</span><span class="sxs-lookup"><span data-stu-id="4824f-198">Yes, you can do that.</span></span> <span data-ttu-id="4824f-199">V horní části hello hello okna, klikněte na tlačítko **Export dat**.</span><span class="sxs-lookup"><span data-stu-id="4824f-199">At hello top of hello blade, click **Export Data**.</span></span>
* <span data-ttu-id="4824f-200">*Mám nastavit exportu, ale nejsou žádná data v tomto úložišti.*</span><span class="sxs-lookup"><span data-stu-id="4824f-200">*I set up an export, but there's no data in my store.*</span></span>

    <span data-ttu-id="4824f-201">Application Insights zobrazila všechny telemetrie z vaší aplikace. vzhledem k tomu, že nastavíte hello exportu?</span><span class="sxs-lookup"><span data-stu-id="4824f-201">Did Application Insights receive any telemetry from your app since you set up hello export?</span></span> <span data-ttu-id="4824f-202">Obdržíte jenom nová data.</span><span class="sxs-lookup"><span data-stu-id="4824f-202">You'll only receive new data.</span></span>
* <span data-ttu-id="4824f-203">*I pokusili tooset nahoru exportu, ale byl odepřen přístup*</span><span class="sxs-lookup"><span data-stu-id="4824f-203">*I tried tooset up an export, but was denied access*</span></span>

    <span data-ttu-id="4824f-204">Pokud vaše organizace vlastní účet hello, máte toobe členem skupiny hello vlastníky nebo přispěvatele.</span><span class="sxs-lookup"><span data-stu-id="4824f-204">If hello account is owned by your organization, you have toobe a member of hello owners or contributors groups.</span></span>
* <span data-ttu-id="4824f-205">*Můžete exportovat přímých toomy vlastní místní úložiště?*</span><span class="sxs-lookup"><span data-stu-id="4824f-205">*Can I export straight toomy own on-premises store?*</span></span>

    <span data-ttu-id="4824f-206">Ne, je nám líto.</span><span class="sxs-lookup"><span data-stu-id="4824f-206">No, sorry.</span></span> <span data-ttu-id="4824f-207">Naše export modul v současné době funkční s Azure storage v tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="4824f-207">Our export engine currently only works with Azure storage at this time.</span></span>  
* <span data-ttu-id="4824f-208">*Je k dispozici žádné omezení toohello množství dat, která jste uložili v tomto úložišti?*</span><span class="sxs-lookup"><span data-stu-id="4824f-208">*Is there any limit toohello amount of data you put in my store?*</span></span>

    <span data-ttu-id="4824f-209">Ne.</span><span class="sxs-lookup"><span data-stu-id="4824f-209">No.</span></span> <span data-ttu-id="4824f-210">Jsme budete zachovat předání dat dokud neodstraníte hello export.</span><span class="sxs-lookup"><span data-stu-id="4824f-210">We'll keep pushing data in until you delete hello export.</span></span> <span data-ttu-id="4824f-211">Pokud jsme dosáhl hello vnější limity pro úložiště objektů blob, ale který je poměrně velký budete zastavení.</span><span class="sxs-lookup"><span data-stu-id="4824f-211">We'll stop if we hit hello outer limits for blob storage, but that's pretty huge.</span></span> <span data-ttu-id="4824f-212">Je to tooyou toocontrol jak velké úložiště používáte.</span><span class="sxs-lookup"><span data-stu-id="4824f-212">It's up tooyou toocontrol how much storage you use.</span></span>  
* <span data-ttu-id="4824f-213">*Kolik objekty BLOB může zobrazit v úložišti hello?*</span><span class="sxs-lookup"><span data-stu-id="4824f-213">*How many blobs should I see in hello storage?*</span></span>

  * <span data-ttu-id="4824f-214">Pro každý datový typ vybraného tooexport, nový objekt blob se vytvoří každou minutu (pokud jsou k dispozici data).</span><span class="sxs-lookup"><span data-stu-id="4824f-214">For every data type you selected tooexport, a new blob is created every minute (if data is available).</span></span>
  * <span data-ttu-id="4824f-215">Pro aplikace s intenzivní provoz, navíc další oddíl jednotky jsou přiděleny.</span><span class="sxs-lookup"><span data-stu-id="4824f-215">In addition, for applications with high traffic, additional partition units are allocated.</span></span> <span data-ttu-id="4824f-216">V takovém případě jednotlivých jednotek vytvoří objekt blob každou minutu.</span><span class="sxs-lookup"><span data-stu-id="4824f-216">In this case each unit creates a blob every minute.</span></span>
* <span data-ttu-id="4824f-217">*I znovu vygenerovat hello klíče toomy úložiště, nebo změnit název hello hello kontejneru a nyní hello export nefunguje.*</span><span class="sxs-lookup"><span data-stu-id="4824f-217">*I regenerated hello key toomy storage or changed hello name of hello container, and now hello export doesn't work.*</span></span>

    <span data-ttu-id="4824f-218">Upravit hello exportu a otevřete okno cílové export hello.</span><span class="sxs-lookup"><span data-stu-id="4824f-218">Edit hello export and open hello export destination blade.</span></span> <span data-ttu-id="4824f-219">Nechte hello stejné úložiště jako předtím vybrali a klikněte na OK tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="4824f-219">Leave hello same storage selected as before, and click OK tooconfirm.</span></span> <span data-ttu-id="4824f-220">Export se restartuje.</span><span class="sxs-lookup"><span data-stu-id="4824f-220">Export will restart.</span></span> <span data-ttu-id="4824f-221">Pokud hello změnu byla v rámci hello po několik dní, abyste neztratili data.</span><span class="sxs-lookup"><span data-stu-id="4824f-221">If hello change was within hello past few days, you won't lose data.</span></span>
* <span data-ttu-id="4824f-222">*Můžete pozastavit hello exportu?*</span><span class="sxs-lookup"><span data-stu-id="4824f-222">*Can I pause hello export?*</span></span>

    <span data-ttu-id="4824f-223">Ano.</span><span class="sxs-lookup"><span data-stu-id="4824f-223">Yes.</span></span> <span data-ttu-id="4824f-224">Klikněte na tlačítko zakázat.</span><span class="sxs-lookup"><span data-stu-id="4824f-224">Click Disable.</span></span>

## <a name="code-samples"></a><span data-ttu-id="4824f-225">Ukázky kódů</span><span class="sxs-lookup"><span data-stu-id="4824f-225">Code samples</span></span>

* [<span data-ttu-id="4824f-226">Ukázka Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="4824f-226">Stream Analytics sample</span></span>](app-insights-export-stream-analytics.md)
* <span data-ttu-id="4824f-227">[Export tooSQL pomocí služby Stream Analytics][exportasa]</span><span class="sxs-lookup"><span data-stu-id="4824f-227">[Export tooSQL using Stream Analytics][exportasa]</span></span>
* [<span data-ttu-id="4824f-228">Podrobný datový model referenční informace pro hello typy a hodnoty vlastností.</span><span class="sxs-lookup"><span data-stu-id="4824f-228">Detailed data model reference for hello property types and values.</span></span>](app-insights-export-data-model.md)

<!--Link references-->

[exportasa]: app-insights-code-sample-export-sql-stream-analytics.md
[roles]: app-insights-resources-roles-access-control.md
