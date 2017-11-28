---
title: "Export tooSQL ze služby Azure Application Insights | Microsoft Docs"
description: "Nepřetržitě exportujte tooSQL Application Insights dat pomocí služby Stream Analytics."
services: application-insights
documentationcenter: 
author: noamben
manager: carmonm
ms.assetid: 48903032-2c99-4987-9948-d6e4559b4a63
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/06/2015
ms.author: bwren
ms.openlocfilehash: 58b579499113751a088dc7e66cbec71529773322
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-export-toosql-from-application-insights-using-stream-analytics"></a><span data-ttu-id="fd906-103">Návod: Export tooSQL ze služby Application Insights pomocí služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="fd906-103">Walkthrough: Export tooSQL from Application Insights using Stream Analytics</span></span>
<span data-ttu-id="fd906-104">Tento článek ukazuje, jak toomove vaše telemetrická data z [Azure Application Insights] [ start] do Azure SQL database pomocí [průběžné exportovat] [ export] a [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).</span><span class="sxs-lookup"><span data-stu-id="fd906-104">This article shows how toomove your telemetry data from [Azure Application Insights][start] into an Azure SQL database by using [Continuous Export][export] and [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).</span></span> 

<span data-ttu-id="fd906-105">Průběžné export přesune telemetrická data do úložiště Azure ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="fd906-105">Continuous export moves your telemetry data into Azure Storage in JSON format.</span></span> <span data-ttu-id="fd906-106">Jsme budete objekty JSON hello pomocí Azure Stream Analytics analyzovat a vytvořte řádky v tabulce databáze.</span><span class="sxs-lookup"><span data-stu-id="fd906-106">We'll parse hello JSON objects using Azure Stream Analytics and create rows in a database table.</span></span>

<span data-ttu-id="fd906-107">(Obecně platí, průběžné Export je způsob, jak toodo hello vlastní analýzu hello telemetrie aplikace odeslat tooApplication statistiky.</span><span class="sxs-lookup"><span data-stu-id="fd906-107">(More generally, Continuous Export is hello way toodo your own analysis of hello telemetry your apps send tooApplication Insights.</span></span> <span data-ttu-id="fd906-108">Vám může přizpůsobit tento toodo ukázkový kód jiných věcí pomocí telemetrie hello exportovali, jako je například agregace dat.)</span><span class="sxs-lookup"><span data-stu-id="fd906-108">You could adapt this code sample toodo other things with hello exported telemetry, such as aggregation of data.)</span></span>

<span data-ttu-id="fd906-109">Začneme s hello předpokládá, že už máte aplikaci hello se mají toomonitor.</span><span class="sxs-lookup"><span data-stu-id="fd906-109">We'll start with hello assumption that you already have hello app you want toomonitor.</span></span>

<span data-ttu-id="fd906-110">V tomto příkladu použijeme hello stránky zobrazení dat, ale hello stejného vzoru lze snadno rozšířit tooother datové typy, jako jsou vlastní události a výjimky.</span><span class="sxs-lookup"><span data-stu-id="fd906-110">In this example, we will be using hello page view data, but hello same pattern can easily be extended tooother data types such as custom events and exceptions.</span></span> 

## <a name="add-application-insights-tooyour-application"></a><span data-ttu-id="fd906-111">Přidání Application Insights tooyour aplikace</span><span class="sxs-lookup"><span data-stu-id="fd906-111">Add Application Insights tooyour application</span></span>
<span data-ttu-id="fd906-112">tooget spuštění:</span><span class="sxs-lookup"><span data-stu-id="fd906-112">tooget started:</span></span>

1. <span data-ttu-id="fd906-113">[Nastavte Application Insights pro webové stránky](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="fd906-113">[Set up Application Insights for your web pages](app-insights-javascript.md).</span></span> 
   
    <span data-ttu-id="fd906-114">(V tomto příkladu budete zaměříme na zpracování stránky zobrazení dat z hello klientský prohlížeč, ale můžete také nastavit Application Insights pro hello na straně serveru vaší [Java](app-insights-java-get-started.md) nebo [ASP.NET](app-insights-asp-net.md) aplikace a proces žádosti závislosti a další telemetrií serveru.)</span><span class="sxs-lookup"><span data-stu-id="fd906-114">(In this example, we'll focus on processing page view data from hello client browsers, but you could also set up Application Insights for hello server side of your [Java](app-insights-java-get-started.md) or [ASP.NET](app-insights-asp-net.md) app and process request, dependency and other server telemetry.)</span></span>
2. <span data-ttu-id="fd906-115">Publikování aplikace a sledujte telemetrii dat zobrazovaných v prostředku Application Insights.</span><span class="sxs-lookup"><span data-stu-id="fd906-115">Publish your app, and watch telemetry data appearing in your Application Insights resource.</span></span>

## <a name="create-storage-in-azure"></a><span data-ttu-id="fd906-116">Vytvořte úložiště v Azure</span><span class="sxs-lookup"><span data-stu-id="fd906-116">Create storage in Azure</span></span>
<span data-ttu-id="fd906-117">Průběžné export vždy výstupy účet úložiště Azure tooan dat, proto musíte nejprve toocreate hello úložiště.</span><span class="sxs-lookup"><span data-stu-id="fd906-117">Continuous export always outputs data tooan Azure Storage account, so you need toocreate hello storage first.</span></span>

1. <span data-ttu-id="fd906-118">Vytvořit účet úložiště v rámci vašeho předplatného v hello [portál Azure][portal].</span><span class="sxs-lookup"><span data-stu-id="fd906-118">Create a storage account in your subscription in hello [Azure portal][portal].</span></span>
   
    ![Na portálu Azure zvolte nový, Data, úložiště.](./media/app-insights-code-sample-export-sql-stream-analytics/040-store.png)
2. <span data-ttu-id="fd906-122">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="fd906-122">Create a container</span></span>
   
    ![V novém úložišti hello vyberte kontejnery, klikněte na dlaždici hello kontejnery a potom přidat](./media/app-insights-code-sample-export-sql-stream-analytics/050-container.png)
3. <span data-ttu-id="fd906-124">Zkopírovat přístupový klíč k úložišti hello</span><span class="sxs-lookup"><span data-stu-id="fd906-124">Copy hello storage access key</span></span>
   
    <span data-ttu-id="fd906-125">Budete je potřebovat brzy tooset až hello vstupní toohello stream analytics služby.</span><span class="sxs-lookup"><span data-stu-id="fd906-125">You'll need it soon tooset up hello input toohello stream analytics service.</span></span>
   
    ![V úložišti hello otevře se nastavení klíče a proveďte kopii hello primární přístupový klíč](./media/app-insights-code-sample-export-sql-stream-analytics/21-storage-key.png)

## <a name="start-continuous-export-tooazure-storage"></a><span data-ttu-id="fd906-127">Spustit průběžné export tooAzure úložiště</span><span class="sxs-lookup"><span data-stu-id="fd906-127">Start continuous export tooAzure storage</span></span>
1. <span data-ttu-id="fd906-128">V hello portálu Azure vyhledejte prostředek Application Insights toohello, který jste vytvořili pro vaše aplikace.</span><span class="sxs-lookup"><span data-stu-id="fd906-128">In hello Azure portal, browse toohello Application Insights resource you created for your application.</span></span>
   
    ![Vyberte možnost Procházet, Application Insights, vaše aplikace](./media/app-insights-code-sample-export-sql-stream-analytics/060-browse.png)
2. <span data-ttu-id="fd906-130">Vytvořte průběžné export.</span><span class="sxs-lookup"><span data-stu-id="fd906-130">Create a continuous export.</span></span>
   
    ![Vyberte nastavení, průběžné exportu, přidejte](./media/app-insights-code-sample-export-sql-stream-analytics/070-export.png)

    <span data-ttu-id="fd906-132">Vyberte účet úložiště hello, které jste vytvořili dříve:</span><span class="sxs-lookup"><span data-stu-id="fd906-132">Select hello storage account you created earlier:</span></span>

    ![Nastavit cíl exportu hello](./media/app-insights-code-sample-export-sql-stream-analytics/080-add.png)

    <span data-ttu-id="fd906-134">Nastavte hello typy událostí, které se mají toosee:</span><span class="sxs-lookup"><span data-stu-id="fd906-134">Set hello event types you want toosee:</span></span>

    ![Vyberte typy událostí](./media/app-insights-code-sample-export-sql-stream-analytics/085-types.png)


1. <span data-ttu-id="fd906-136">Některá data hromadí let.</span><span class="sxs-lookup"><span data-stu-id="fd906-136">Let some data accumulate.</span></span> <span data-ttu-id="fd906-137">Sledujte a umožnit lidem nějakou dobu používat vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fd906-137">Sit back and let people use your application for a while.</span></span> <span data-ttu-id="fd906-138">Telemetrická data se odešlou a zobrazí statistické grafy v [metriky explorer](app-insights-metrics-explorer.md) a jednotlivé události v [diagnostické vyhledávání](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="fd906-138">Telemetry will come in and you'll see statistical charts in [metric explorer](app-insights-metrics-explorer.md) and individual events in [diagnostic search](app-insights-diagnostic-search.md).</span></span> 
   
    <span data-ttu-id="fd906-139">A navíc hello dat bude exportovat tooyour úložiště.</span><span class="sxs-lookup"><span data-stu-id="fd906-139">And also, hello data will export tooyour storage.</span></span> 
2. <span data-ttu-id="fd906-140">Zkontrolujte hello export dat, buď v portálu hello – volba **Procházet**, vyberte svůj účet úložiště a potom **kontejnery** - nebo v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="fd906-140">Inspect hello exported data, either in hello portal - choose **Browse**, select your storage account, and then **Containers** - or in Visual Studio.</span></span> <span data-ttu-id="fd906-141">V sadě Visual Studio, vyberte **zobrazení / cloudu Explorer**a otevřete Azure nebo úložiště.</span><span class="sxs-lookup"><span data-stu-id="fd906-141">In Visual Studio, choose **View / Cloud Explorer**, and open Azure / Storage.</span></span> <span data-ttu-id="fd906-142">(Pokud nemáte tento příkaz nabídky, je třeba tooinstall hello Azure SDK: Otevřete dialogové okno Nový projekt hello a otevřete Visual C# / Cloud / získat Microsoft Azure SDK pro .NET.)</span><span class="sxs-lookup"><span data-stu-id="fd906-142">(If you don't have this menu option, you need tooinstall hello Azure SDK: Open hello New Project dialog and open Visual C# / Cloud / Get Microsoft Azure SDK for .NET.)</span></span>
   
    ![V sadě Visual Studio otevřete prohlížeč Server, Azure, úložiště](./media/app-insights-code-sample-export-sql-stream-analytics/087-explorer.png)
   
    <span data-ttu-id="fd906-144">Poznamenejte si hello běžné součástí hello název cesty, které je odvozeno z názvu a instrumentace klíč aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="fd906-144">Make a note of hello common part of hello path name, which is derived from hello application name and instrumentation key.</span></span> 

<span data-ttu-id="fd906-145">Hello události se zapisují tooblob soubory ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="fd906-145">hello events are written tooblob files in JSON format.</span></span> <span data-ttu-id="fd906-146">Každý soubor může obsahovat jeden nebo více událostí.</span><span class="sxs-lookup"><span data-stu-id="fd906-146">Each file may contain one or more events.</span></span> <span data-ttu-id="fd906-147">Proto rádi bychom znali data události hello tooread a filtrování hello pole, která má být.</span><span class="sxs-lookup"><span data-stu-id="fd906-147">So we'd like tooread hello event data and filter out hello fields we want.</span></span> <span data-ttu-id="fd906-148">Jsou k dispozici všechny typy věcí, které můžeme udělat s daty hello, ale naše plán dnes je toouse Stream Analytics toomove hello data tooa databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="fd906-148">There are all kinds of things we could do with hello data, but our plan today is toouse Stream Analytics toomove hello data tooa SQL database.</span></span> <span data-ttu-id="fd906-149">Která znamená, že snadno toorun spoustu zajímavé dotazy.</span><span class="sxs-lookup"><span data-stu-id="fd906-149">That will make it easy toorun lots of interesting queries.</span></span>

## <a name="create-an-azure-sql-database"></a><span data-ttu-id="fd906-150">Vytvoření databáze Azure SQL</span><span class="sxs-lookup"><span data-stu-id="fd906-150">Create an Azure SQL Database</span></span>
<span data-ttu-id="fd906-151">Znovu se spouští ze svého předplatného v [portál Azure][portal], vytvořit databázi hello (a nový server, pokud již máte jeden) toowhich napíšete hello data.</span><span class="sxs-lookup"><span data-stu-id="fd906-151">Once again starting from your subscription in [Azure portal][portal], create hello database (and a new server, unless you've already got one) toowhich you'll write hello data.</span></span>

![Nový dat, SQL](./media/app-insights-code-sample-export-sql-stream-analytics/090-sql.png)

<span data-ttu-id="fd906-153">Zkontrolujte, zda že server databáze hello umožňuje přístup tooAzure služby:</span><span class="sxs-lookup"><span data-stu-id="fd906-153">Make sure that hello database server allows access tooAzure services:</span></span>

![Procházet, servery, server, nastavení, brána Firewall, povolit přístup tooAzure](./media/app-insights-code-sample-export-sql-stream-analytics/100-sqlaccess.png)

## <a name="create-a-table-in-azure-sql-db"></a><span data-ttu-id="fd906-155">Vytvoření tabulky v databázi SQL Azure</span><span class="sxs-lookup"><span data-stu-id="fd906-155">Create a table in Azure SQL DB</span></span>
<span data-ttu-id="fd906-156">Připojte databázi toohello vytvořené v předchozí části hello s vaší nástroj pro správu upřednostňované.</span><span class="sxs-lookup"><span data-stu-id="fd906-156">Connect toohello database created in hello previous section with your preferred management tool.</span></span> <span data-ttu-id="fd906-157">V tomto návodu použijeme [nástroje správy systému SQL Server](https://msdn.microsoft.com/ms174173.aspx) (SSMS).</span><span class="sxs-lookup"><span data-stu-id="fd906-157">In this walkthrough, we will be using [SQL Server Management Tools](https://msdn.microsoft.com/ms174173.aspx) (SSMS).</span></span>

![](./media/app-insights-code-sample-export-sql-stream-analytics/31-sql-table.png)

<span data-ttu-id="fd906-158">Vytvořit nový dotaz a spusťte hello následující T-SQL:</span><span class="sxs-lookup"><span data-stu-id="fd906-158">Create a new query, and execute hello following T-SQL:</span></span>

```SQL

CREATE TABLE [dbo].[PageViewsTable](
    [pageName] [nvarchar](max) NOT NULL,
    [viewCount] [int] NOT NULL,
    [url] [nvarchar](max) NULL,
    [urlDataPort] [int] NULL,
    [urlDataprotocol] [nvarchar](50) NULL,
    [urlDataHost] [nvarchar](50) NULL,
    [urlDataBase] [nvarchar](50) NULL,
    [urlDataHashTag] [nvarchar](max) NULL,
    [eventTime] [datetime] NOT NULL,
    [isSynthetic] [nvarchar](50) NULL,
    [deviceId] [nvarchar](50) NULL,
    [deviceType] [nvarchar](50) NULL,
    [os] [nvarchar](50) NULL,
    [osVersion] [nvarchar](50) NULL,
    [locale] [nvarchar](50) NULL,
    [userAgent] [nvarchar](max) NULL,
    [browser] [nvarchar](50) NULL,
    [browserVersion] [nvarchar](50) NULL,
    [screenResolution] [nvarchar](50) NULL,
    [sessionId] [nvarchar](max) NULL,
    [sessionIsFirst] [nvarchar](50) NULL,
    [clientIp] [nvarchar](50) NULL,
    [continent] [nvarchar](50) NULL,
    [country] [nvarchar](50) NULL,
    [province] [nvarchar](50) NULL,
    [city] [nvarchar](50) NULL
)

CREATE CLUSTERED INDEX [pvTblIdx] ON [dbo].[PageViewsTable]
(
    [eventTime] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON)

```

![](./media/app-insights-code-sample-export-sql-stream-analytics/34-create-table.png)

<span data-ttu-id="fd906-159">V této ukázce používáme data ze zobrazení stránky.</span><span class="sxs-lookup"><span data-stu-id="fd906-159">In this sample, we are using data from page views.</span></span> <span data-ttu-id="fd906-160">toosee hello jiná data dostupná, zkontrolujte výstupu JSON a najdete v části hello [Exportovat datový model](app-insights-export-data-model.md).</span><span class="sxs-lookup"><span data-stu-id="fd906-160">toosee hello other data available, inspect your JSON output, and see hello [export data model](app-insights-export-data-model.md).</span></span>

## <a name="create-an-azure-stream-analytics-instance"></a><span data-ttu-id="fd906-161">Vytvoření instance služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="fd906-161">Create an Azure Stream Analytics instance</span></span>
<span data-ttu-id="fd906-162">Z hello [portál Azure Classic](https://manage.windowsazure.com/), vyberte službu Azure Stream Analytics hello a vytvořit novou úlohu služby Stream Analytics:</span><span class="sxs-lookup"><span data-stu-id="fd906-162">From hello [Classic Azure Portal](https://manage.windowsazure.com/), select hello Azure Stream Analytics service, and create a new Stream Analytics job:</span></span>

![](./media/app-insights-code-sample-export-sql-stream-analytics/37-create-stream-analytics.png)

![](./media/app-insights-code-sample-export-sql-stream-analytics/38-create-stream-analytics-form.png)

<span data-ttu-id="fd906-163">Když je vytvořena nová úloha hello, rozbalte položku Podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="fd906-163">When hello new job is created, expand its details:</span></span>

![](./media/app-insights-code-sample-export-sql-stream-analytics/41-sa-job.png)

#### <a name="set-blob-location"></a><span data-ttu-id="fd906-164">Nastavení umístění objektu blob</span><span class="sxs-lookup"><span data-stu-id="fd906-164">Set blob location</span></span>
<span data-ttu-id="fd906-165">Nastavte tootake vstup z objektu blob služby průběžné Export:</span><span class="sxs-lookup"><span data-stu-id="fd906-165">Set it tootake input from your Continuous Export blob:</span></span>

![](./media/app-insights-code-sample-export-sql-stream-analytics/42-sa-wizard1.png)

<span data-ttu-id="fd906-166">Nyní budete potřebovat hello primární přístupový klíč z vašeho účtu úložiště, který jste si předtím poznamenali.</span><span class="sxs-lookup"><span data-stu-id="fd906-166">Now you'll need hello Primary Access Key from your Storage Account, which you noted earlier.</span></span> <span data-ttu-id="fd906-167">Nastavením této hodnoty jako hello klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="fd906-167">Set this as hello Storage Account Key.</span></span>

![](./media/app-insights-code-sample-export-sql-stream-analytics/46-sa-wizard2.png)

#### <a name="set-path-prefix-pattern"></a><span data-ttu-id="fd906-168">Sada cesta předpona vzoru</span><span class="sxs-lookup"><span data-stu-id="fd906-168">Set path prefix pattern</span></span>
![](./media/app-insights-code-sample-export-sql-stream-analytics/47-sa-wizard3.png)

<span data-ttu-id="fd906-169">Zda tooset hello formát data byla příliš**rrrr-MM-DD** (s **pomlčky**).</span><span class="sxs-lookup"><span data-stu-id="fd906-169">Be sure tooset hello Date Format too**YYYY-MM-DD** (with **dashes**).</span></span>

<span data-ttu-id="fd906-170">Hello cesta předpony vzor Určuje, jak Stream Analytics vyhledá hello vstupní soubory v úložišti hello.</span><span class="sxs-lookup"><span data-stu-id="fd906-170">hello Path Prefix Pattern specifies how Stream Analytics finds hello input files in hello storage.</span></span> <span data-ttu-id="fd906-171">Je třeba tooset ho toocorrespond toohow průběžné exportovat ukládá hello data.</span><span class="sxs-lookup"><span data-stu-id="fd906-171">You need tooset it toocorrespond toohow Continuous Export stores hello data.</span></span> <span data-ttu-id="fd906-172">Nastavte takto:</span><span class="sxs-lookup"><span data-stu-id="fd906-172">Set it like this:</span></span>

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

<span data-ttu-id="fd906-173">V tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="fd906-173">In this example:</span></span>

* <span data-ttu-id="fd906-174">`webapplication27`je název hello hello prostředku Application Insights **vše na malá písmena**.</span><span class="sxs-lookup"><span data-stu-id="fd906-174">`webapplication27` is hello name of hello Application Insights resource, **all in lower case**.</span></span> 
* <span data-ttu-id="fd906-175">`1234...`je klíč instrumentace hello hello prostředek Application Insights **s pomlčkami odebrat**.</span><span class="sxs-lookup"><span data-stu-id="fd906-175">`1234...` is hello instrumentation key of hello Application Insights resource **with dashes removed**.</span></span> 
* <span data-ttu-id="fd906-176">`PageViews`hello typu dat chceme tooanalyze.</span><span class="sxs-lookup"><span data-stu-id="fd906-176">`PageViews` is hello type of data we want tooanalyze.</span></span> <span data-ttu-id="fd906-177">dostupné typy Hello závisí na hello filtr, který nastavíte v průběžné exportovat.</span><span class="sxs-lookup"><span data-stu-id="fd906-177">hello available types depend on hello filter you set in Continuous Export.</span></span> <span data-ttu-id="fd906-178">Zkontrolujte hello exportovaná data toosee hello jiné typy k dispozici a zobrazí hello [Exportovat datový model](app-insights-export-data-model.md).</span><span class="sxs-lookup"><span data-stu-id="fd906-178">Examine hello exported data toosee hello other available types, and see hello [export data model](app-insights-export-data-model.md).</span></span>
* <span data-ttu-id="fd906-179">`/{date}/{time}`vzor zapsána oznámena.</span><span class="sxs-lookup"><span data-stu-id="fd906-179">`/{date}/{time}` is a pattern written literally.</span></span>

<span data-ttu-id="fd906-180">Název hello tooget a iKey prostředku Application Insights, otevřete Essentials na stránku s jeho přehled nebo otevřete nastavení.</span><span class="sxs-lookup"><span data-stu-id="fd906-180">tooget hello name and iKey of your Application Insights resource, open Essentials on its overview page, or open Settings.</span></span>

#### <a name="finish-initial-setup"></a><span data-ttu-id="fd906-181">Dokončit počáteční nastavení</span><span class="sxs-lookup"><span data-stu-id="fd906-181">Finish initial setup</span></span>
<span data-ttu-id="fd906-182">Zkontrolujte formát serializace hello:</span><span class="sxs-lookup"><span data-stu-id="fd906-182">Confirm hello serialization format:</span></span>

![Potvrďte a zavřete průvodce](./media/app-insights-code-sample-export-sql-stream-analytics/48-sa-wizard4.png)

<span data-ttu-id="fd906-184">Zavřete průvodce hello a počkejte toocomplete hello instalační program.</span><span class="sxs-lookup"><span data-stu-id="fd906-184">Close hello wizard and wait for hello setup toocomplete.</span></span>

> [!TIP]
> <span data-ttu-id="fd906-185">Použijte hello ukázka funkce toocheck, že jste správně nastavili hello vstupní cesta.</span><span class="sxs-lookup"><span data-stu-id="fd906-185">Use hello Sample function toocheck that you have set hello input path correctly.</span></span> <span data-ttu-id="fd906-186">Pokud se nezdaří: Zkontrolujte, zda je data v úložišti hello hello ukázka časovém rozmezí jste zvolili.</span><span class="sxs-lookup"><span data-stu-id="fd906-186">If it fails: Check that there is data in hello storage for hello sample time range you chose.</span></span> <span data-ttu-id="fd906-187">Upravte definici vstupní hello a zkontrolujte nastavení hello účet úložiště, cesta předponu a datum formátu správně.</span><span class="sxs-lookup"><span data-stu-id="fd906-187">Edit hello input definition and check you set hello storage account, path prefix and date format correctly.</span></span>
> 
> 

## <a name="set-query"></a><span data-ttu-id="fd906-188">Sada dotazu</span><span class="sxs-lookup"><span data-stu-id="fd906-188">Set query</span></span>
<span data-ttu-id="fd906-189">Otevřete část dotazu hello:</span><span class="sxs-lookup"><span data-stu-id="fd906-189">Open hello query section:</span></span>

![V služby stream analytics vyberte dotazu](./media/app-insights-code-sample-export-sql-stream-analytics/51-query.png)

<span data-ttu-id="fd906-191">Nahraďte hello výchozí dotaz s:</span><span class="sxs-lookup"><span data-stu-id="fd906-191">Replace hello default query with:</span></span>

```SQL

    SELECT flat.ArrayValue.name as pageName
    , flat.ArrayValue.count as viewCount
    , flat.ArrayValue.url as url
    , flat.ArrayValue.urlData.port as urlDataPort
    , flat.ArrayValue.urlData.protocol as urlDataprotocol
    , flat.ArrayValue.urlData.host as urlDataHost
    , flat.ArrayValue.urlData.base as urlDataBase
    , flat.ArrayValue.urlData.hashTag as urlDataHashTag
      ,A.context.data.eventTime as eventTime
      ,A.context.data.isSynthetic as isSynthetic
      ,A.context.device.id as deviceId
      ,A.context.device.type as deviceType
      ,A.context.device.os as os
      ,A.context.device.osVersion as osVersion
      ,A.context.device.locale as locale
      ,A.context.device.userAgent as userAgent
      ,A.context.device.browser as browser
      ,A.context.device.browserVersion as browserVersion
      ,A.context.device.screenResolution.value as screenResolution
      ,A.context.session.id as sessionId
      ,A.context.session.isFirst as sessionIsFirst
      ,A.context.location.clientip as clientIp
      ,A.context.location.continent as continent
      ,A.context.location.country as country
      ,A.context.location.province as province
      ,A.context.location.city as city
    INTO
      AIOutput
    FROM AIinput A
    CROSS APPLY GetElements(A.[view]) as flat


```

<span data-ttu-id="fd906-192">Všimněte si, že nejprve hello několik vlastností jsou konkrétní toopage dat zobrazení.</span><span class="sxs-lookup"><span data-stu-id="fd906-192">Notice that hello first few properties are specific toopage view data.</span></span> <span data-ttu-id="fd906-193">Export jiné typy telemetrických dat bude mít jiné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="fd906-193">Exports of other telemetry types will have different properties.</span></span> <span data-ttu-id="fd906-194">V tématu hello [podrobné referenční model dat pro hello typy a hodnoty vlastností.](app-insights-export-data-model.md)</span><span class="sxs-lookup"><span data-stu-id="fd906-194">See hello [detailed data model reference for hello property types and values.](app-insights-export-data-model.md)</span></span>

## <a name="set-up-output-toodatabase"></a><span data-ttu-id="fd906-195">Nastavit toodatabase výstup</span><span class="sxs-lookup"><span data-stu-id="fd906-195">Set up output toodatabase</span></span>
<span data-ttu-id="fd906-196">Vyberte SQL jako výstup hello.</span><span class="sxs-lookup"><span data-stu-id="fd906-196">Select SQL as hello output.</span></span>

![V služby stream analytics vyberte výstupy](./media/app-insights-code-sample-export-sql-stream-analytics/53-store.png)

<span data-ttu-id="fd906-198">Zadejte databáze SQL hello.</span><span class="sxs-lookup"><span data-stu-id="fd906-198">Specify hello SQL database.</span></span>

![Zadejte podrobnosti hello databáze](./media/app-insights-code-sample-export-sql-stream-analytics/55-output.png)

<span data-ttu-id="fd906-200">Zavřete průvodce hello a čekat na oznámení, že byla nastavena výstup hello.</span><span class="sxs-lookup"><span data-stu-id="fd906-200">Close hello wizard and wait for a notification that hello output has been set up.</span></span>

## <a name="start-processing"></a><span data-ttu-id="fd906-201">Spuštění zpracování</span><span class="sxs-lookup"><span data-stu-id="fd906-201">Start processing</span></span>
<span data-ttu-id="fd906-202">Spuštění úlohy hello z panelu hello akce:</span><span class="sxs-lookup"><span data-stu-id="fd906-202">Start hello job from hello action bar:</span></span>

![V služby stream analytics klikněte na tlačítko Start](./media/app-insights-code-sample-export-sql-stream-analytics/61-start.png)

<span data-ttu-id="fd906-204">Můžete vybrat, zda zpracování toostart hello data od teď nebo toostart starší daty.</span><span class="sxs-lookup"><span data-stu-id="fd906-204">You can choose whether toostart processing hello data starting from now, or toostart with earlier data.</span></span> <span data-ttu-id="fd906-205">Hello druhé je užitečné, pokud jste předtím průběžné exportovat už běží nějakou dobu.</span><span class="sxs-lookup"><span data-stu-id="fd906-205">hello latter is useful if you have had Continuous Export already running for a while.</span></span>

![V služby stream analytics klikněte na tlačítko Start](./media/app-insights-code-sample-export-sql-stream-analytics/63-start.png)

<span data-ttu-id="fd906-207">Po několika minutách přejděte zpět tooSQL nástroje pro správu serveru a podívejte se na hello dat odesílaných v.</span><span class="sxs-lookup"><span data-stu-id="fd906-207">After a few minutes, go back tooSQL Server Management Tools and watch hello data flowing in.</span></span> <span data-ttu-id="fd906-208">Například použijte dotaz takto:</span><span class="sxs-lookup"><span data-stu-id="fd906-208">For example, use a query like this:</span></span>

    SELECT TOP 100 *
    FROM [dbo].[PageViewsTable]


## <a name="related-articles"></a><span data-ttu-id="fd906-209">Související články</span><span class="sxs-lookup"><span data-stu-id="fd906-209">Related articles</span></span>
* [<span data-ttu-id="fd906-210">Export tooPowerBI pomocí služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="fd906-210">Export tooPowerBI using Stream Analytics</span></span>](app-insights-export-power-bi.md)
* [<span data-ttu-id="fd906-211">Podrobný datový model referenční informace pro hello typy a hodnoty vlastností.</span><span class="sxs-lookup"><span data-stu-id="fd906-211">Detailed data model reference for hello property types and values.</span></span>](app-insights-export-data-model.md)
* [<span data-ttu-id="fd906-212">Průběžné Export ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="fd906-212">Continuous Export in Application Insights</span></span>](app-insights-export-telemetry.md)
* [<span data-ttu-id="fd906-213">Application Insights</span><span class="sxs-lookup"><span data-stu-id="fd906-213">Application Insights</span></span>](https://azure.microsoft.com/services/application-insights/)

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[export]: app-insights-export-telemetry.md
[metrics]: app-insights-metrics-explorer.md
[portal]: http://portal.azure.com/
[start]: app-insights-overview.md

