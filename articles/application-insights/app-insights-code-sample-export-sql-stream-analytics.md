---
title: Exportovat do SQL z Azure Application Insights | Microsoft Docs
description: "Nepřetržitě exportujte data Application Insights do SQL pomocí služby Stream Analytics."
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
ms.openlocfilehash: d51e80509ffb63cef0d01133a2295d58757d5b1a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="walkthrough-export-to-sql-from-application-insights-using-stream-analytics"></a><span data-ttu-id="34645-103">Návod: Export do SQL z Application Insights pomocí služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="34645-103">Walkthrough: Export to SQL from Application Insights using Stream Analytics</span></span>
<span data-ttu-id="34645-104">Tento článek ukazuje, jak přesunout data telemetrie z [Azure Application Insights] [ start] do Azure SQL database pomocí [průběžné exportovat] [ export] a [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).</span><span class="sxs-lookup"><span data-stu-id="34645-104">This article shows how to move your telemetry data from [Azure Application Insights][start] into an Azure SQL database by using [Continuous Export][export] and [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/).</span></span> 

<span data-ttu-id="34645-105">Průběžné export přesune telemetrická data do úložiště Azure ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="34645-105">Continuous export moves your telemetry data into Azure Storage in JSON format.</span></span> <span data-ttu-id="34645-106">Jsme budete objekty JSON pomocí Azure Stream Analytics analyzovat a vytvořte řádky v tabulce databáze.</span><span class="sxs-lookup"><span data-stu-id="34645-106">We'll parse the JSON objects using Azure Stream Analytics and create rows in a database table.</span></span>

<span data-ttu-id="34645-107">(Obecně platí, průběžné Export je způsob, jak provést vlastní analýzu telemetrie aplikace odesílání Application Insights.</span><span class="sxs-lookup"><span data-stu-id="34645-107">(More generally, Continuous Export is the way to do your own analysis of the telemetry your apps send to Application Insights.</span></span> <span data-ttu-id="34645-108">Vám může přizpůsobit této ukázce kódu provádět další akce s exportovaný telemetrických dat, jako je například agregace dat.)</span><span class="sxs-lookup"><span data-stu-id="34645-108">You could adapt this code sample to do other things with the exported telemetry, such as aggregation of data.)</span></span>

<span data-ttu-id="34645-109">Začneme s se předpokládá, že už máte aplikaci, kterou chcete monitorovat.</span><span class="sxs-lookup"><span data-stu-id="34645-109">We'll start with the assumption that you already have the app you want to monitor.</span></span>

<span data-ttu-id="34645-110">V tomto příkladu použijeme data zobrazit na stránce, ale stejného vzoru lze snadno rozšířit na jiné datové typy, jako jsou vlastní události a výjimky.</span><span class="sxs-lookup"><span data-stu-id="34645-110">In this example, we will be using the page view data, but the same pattern can easily be extended to other data types such as custom events and exceptions.</span></span> 

## <a name="add-application-insights-to-your-application"></a><span data-ttu-id="34645-111">Přidejte Application Insights do vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="34645-111">Add Application Insights to your application</span></span>
<span data-ttu-id="34645-112">Abyste mohli začít:</span><span class="sxs-lookup"><span data-stu-id="34645-112">To get started:</span></span>

1. <span data-ttu-id="34645-113">[Nastavte Application Insights pro webové stránky](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="34645-113">[Set up Application Insights for your web pages](app-insights-javascript.md).</span></span> 
   
    <span data-ttu-id="34645-114">(V tomto příkladu budete zaměříme na zpracování stránky zobrazení dat v prohlížečích klienta, ale můžete také nastavit Application Insights pro na straně serveru vaší [Java](app-insights-java-get-started.md) nebo [ASP.NET](app-insights-asp-net.md) aplikace a proces žádosti závislosti a další telemetrií serveru.)</span><span class="sxs-lookup"><span data-stu-id="34645-114">(In this example, we'll focus on processing page view data from the client browsers, but you could also set up Application Insights for the server side of your [Java](app-insights-java-get-started.md) or [ASP.NET](app-insights-asp-net.md) app and process request, dependency and other server telemetry.)</span></span>
2. <span data-ttu-id="34645-115">Publikování aplikace a sledujte telemetrii dat zobrazovaných v prostředku Application Insights.</span><span class="sxs-lookup"><span data-stu-id="34645-115">Publish your app, and watch telemetry data appearing in your Application Insights resource.</span></span>

## <a name="create-storage-in-azure"></a><span data-ttu-id="34645-116">Vytvořte úložiště v Azure</span><span class="sxs-lookup"><span data-stu-id="34645-116">Create storage in Azure</span></span>
<span data-ttu-id="34645-117">Průběžné export vždy poskytuje data pro účet úložiště Azure, musíte nejprve vytvořit úložiště.</span><span class="sxs-lookup"><span data-stu-id="34645-117">Continuous export always outputs data to an Azure Storage account, so you need to create the storage first.</span></span>

1. <span data-ttu-id="34645-118">Vytvořit účet úložiště v rámci vašeho předplatného v [portál Azure][portal].</span><span class="sxs-lookup"><span data-stu-id="34645-118">Create a storage account in your subscription in the [Azure portal][portal].</span></span>
   
    ![Na portálu Azure zvolte nový, Data, úložiště.](./media/app-insights-code-sample-export-sql-stream-analytics/040-store.png)
2. <span data-ttu-id="34645-122">Vytvoření kontejneru</span><span class="sxs-lookup"><span data-stu-id="34645-122">Create a container</span></span>
   
    ![V novém úložišti vyberte kontejnery, klikněte na dlaždici kontejnery a potom přidat](./media/app-insights-code-sample-export-sql-stream-analytics/050-container.png)
3. <span data-ttu-id="34645-124">Kopii přístupového klíče úložiště.</span><span class="sxs-lookup"><span data-stu-id="34645-124">Copy the storage access key</span></span>
   
    <span data-ttu-id="34645-125">Musíte ho brzy k nastavení vstup do služby stream analytics.</span><span class="sxs-lookup"><span data-stu-id="34645-125">You'll need it soon to set up the input to the stream analytics service.</span></span>
   
    ![V úložišti otevře se nastavení klíče a proveďte kopii primární přístupový klíč](./media/app-insights-code-sample-export-sql-stream-analytics/21-storage-key.png)

## <a name="start-continuous-export-to-azure-storage"></a><span data-ttu-id="34645-127">Spustit průběžné export do úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="34645-127">Start continuous export to Azure storage</span></span>
1. <span data-ttu-id="34645-128">Na portálu Azure přejděte do prostředku Application Insights, kterou jste vytvořili pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="34645-128">In the Azure portal, browse to the Application Insights resource you created for your application.</span></span>
   
    ![Vyberte možnost Procházet, Application Insights, vaše aplikace](./media/app-insights-code-sample-export-sql-stream-analytics/060-browse.png)
2. <span data-ttu-id="34645-130">Vytvořte průběžné export.</span><span class="sxs-lookup"><span data-stu-id="34645-130">Create a continuous export.</span></span>
   
    ![Vyberte nastavení, průběžné exportu, přidejte](./media/app-insights-code-sample-export-sql-stream-analytics/070-export.png)

    <span data-ttu-id="34645-132">Vyberte účet úložiště, které jste vytvořili dříve:</span><span class="sxs-lookup"><span data-stu-id="34645-132">Select the storage account you created earlier:</span></span>

    ![Nastavit cíl exportu](./media/app-insights-code-sample-export-sql-stream-analytics/080-add.png)

    <span data-ttu-id="34645-134">Nastavte typy událostí, které chcete zobrazit:</span><span class="sxs-lookup"><span data-stu-id="34645-134">Set the event types you want to see:</span></span>

    ![Vyberte typy událostí](./media/app-insights-code-sample-export-sql-stream-analytics/085-types.png)


1. <span data-ttu-id="34645-136">Některá data hromadí let.</span><span class="sxs-lookup"><span data-stu-id="34645-136">Let some data accumulate.</span></span> <span data-ttu-id="34645-137">Sledujte a umožnit lidem nějakou dobu používat vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="34645-137">Sit back and let people use your application for a while.</span></span> <span data-ttu-id="34645-138">Telemetrická data se odešlou a zobrazí statistické grafy v [metriky explorer](app-insights-metrics-explorer.md) a jednotlivé události v [diagnostické vyhledávání](app-insights-diagnostic-search.md).</span><span class="sxs-lookup"><span data-stu-id="34645-138">Telemetry will come in and you'll see statistical charts in [metric explorer](app-insights-metrics-explorer.md) and individual events in [diagnostic search](app-insights-diagnostic-search.md).</span></span> 
   
    <span data-ttu-id="34645-139">A navíc bude exportovat data do úložiště.</span><span class="sxs-lookup"><span data-stu-id="34645-139">And also, the data will export to your storage.</span></span> 
2. <span data-ttu-id="34645-140">Zkontrolujte exportovaná data, buď v portálu – volba **Procházet**, vyberte svůj účet úložiště a potom **kontejnery** - nebo v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="34645-140">Inspect the exported data, either in the portal - choose **Browse**, select your storage account, and then **Containers** - or in Visual Studio.</span></span> <span data-ttu-id="34645-141">V sadě Visual Studio, vyberte **zobrazení / cloudu Explorer**a otevřete Azure nebo úložiště.</span><span class="sxs-lookup"><span data-stu-id="34645-141">In Visual Studio, choose **View / Cloud Explorer**, and open Azure / Storage.</span></span> <span data-ttu-id="34645-142">(Pokud nemáte tento příkaz nabídky, budete muset nainstalovat sadu Azure SDK: Otevřete dialogové okno Nový projekt a otevřete Visual C# / Cloud / získat Microsoft Azure SDK pro .NET.)</span><span class="sxs-lookup"><span data-stu-id="34645-142">(If you don't have this menu option, you need to install the Azure SDK: Open the New Project dialog and open Visual C# / Cloud / Get Microsoft Azure SDK for .NET.)</span></span>
   
    ![V sadě Visual Studio otevřete prohlížeč Server, Azure, úložiště](./media/app-insights-code-sample-export-sql-stream-analytics/087-explorer.png)
   
    <span data-ttu-id="34645-144">Poznamenejte si část běžný název cesty, které je odvozeno z klíče název a instrumentace aplikací.</span><span class="sxs-lookup"><span data-stu-id="34645-144">Make a note of the common part of the path name, which is derived from the application name and instrumentation key.</span></span> 

<span data-ttu-id="34645-145">Události se zapisují do objektu blob soubory ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="34645-145">The events are written to blob files in JSON format.</span></span> <span data-ttu-id="34645-146">Každý soubor může obsahovat jeden nebo více událostí.</span><span class="sxs-lookup"><span data-stu-id="34645-146">Each file may contain one or more events.</span></span> <span data-ttu-id="34645-147">Proto jsme chtěli číst data události a filtrovat pole, která má být.</span><span class="sxs-lookup"><span data-stu-id="34645-147">So we'd like to read the event data and filter out the fields we want.</span></span> <span data-ttu-id="34645-148">Jsou k dispozici všechny typy věcí, které můžeme udělat s daty, ale pokud chcete přesunout data do databáze SQL pomocí služby Stream Analytics dnes je naše plán.</span><span class="sxs-lookup"><span data-stu-id="34645-148">There are all kinds of things we could do with the data, but our plan today is to use Stream Analytics to move the data to a SQL database.</span></span> <span data-ttu-id="34645-149">To bude usnadní spouštět velké množství zajímavé dotazy.</span><span class="sxs-lookup"><span data-stu-id="34645-149">That will make it easy to run lots of interesting queries.</span></span>

## <a name="create-an-azure-sql-database"></a><span data-ttu-id="34645-150">Vytvoření databáze Azure SQL</span><span class="sxs-lookup"><span data-stu-id="34645-150">Create an Azure SQL Database</span></span>
<span data-ttu-id="34645-151">Znovu se spouští ze svého předplatného v [portál Azure][portal], vytvořit databázi (a nový server, pokud již máte jeden) kterého budete zapsat data.</span><span class="sxs-lookup"><span data-stu-id="34645-151">Once again starting from your subscription in [Azure portal][portal], create the database (and a new server, unless you've already got one) to which you'll write the data.</span></span>

![Nový dat, SQL](./media/app-insights-code-sample-export-sql-stream-analytics/090-sql.png)

<span data-ttu-id="34645-153">Ujistěte se, že databázový server umožňuje přístup ke službám Azure:</span><span class="sxs-lookup"><span data-stu-id="34645-153">Make sure that the database server allows access to Azure services:</span></span>

![Procházet, servery, server, nastavení brány Firewall, povolit přístup k Azure](./media/app-insights-code-sample-export-sql-stream-analytics/100-sqlaccess.png)

## <a name="create-a-table-in-azure-sql-db"></a><span data-ttu-id="34645-155">Vytvoření tabulky v databázi SQL Azure</span><span class="sxs-lookup"><span data-stu-id="34645-155">Create a table in Azure SQL DB</span></span>
<span data-ttu-id="34645-156">Připojte k databázi vytvořené v předchozí části s vaší nástroj pro správu upřednostňované.</span><span class="sxs-lookup"><span data-stu-id="34645-156">Connect to the database created in the previous section with your preferred management tool.</span></span> <span data-ttu-id="34645-157">V tomto návodu použijeme [nástroje správy systému SQL Server](https://msdn.microsoft.com/ms174173.aspx) (SSMS).</span><span class="sxs-lookup"><span data-stu-id="34645-157">In this walkthrough, we will be using [SQL Server Management Tools](https://msdn.microsoft.com/ms174173.aspx) (SSMS).</span></span>

![](./media/app-insights-code-sample-export-sql-stream-analytics/31-sql-table.png)

<span data-ttu-id="34645-158">Vytvořit nový dotaz a spusťte následující T-SQL:</span><span class="sxs-lookup"><span data-stu-id="34645-158">Create a new query, and execute the following T-SQL:</span></span>

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

<span data-ttu-id="34645-159">V této ukázce používáme data ze zobrazení stránky.</span><span class="sxs-lookup"><span data-stu-id="34645-159">In this sample, we are using data from page views.</span></span> <span data-ttu-id="34645-160">Pokud chcete zobrazit dostupné data, zkontrolujte výstupu JSON a najdete v článku [Exportovat datový model](app-insights-export-data-model.md).</span><span class="sxs-lookup"><span data-stu-id="34645-160">To see the other data available, inspect your JSON output, and see the [export data model](app-insights-export-data-model.md).</span></span>

## <a name="create-an-azure-stream-analytics-instance"></a><span data-ttu-id="34645-161">Vytvoření instance služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="34645-161">Create an Azure Stream Analytics instance</span></span>
<span data-ttu-id="34645-162">Z [portál Azure Classic](https://manage.windowsazure.com/), vyberte službu Azure Stream Analytics a vytvořit novou úlohu služby Stream Analytics:</span><span class="sxs-lookup"><span data-stu-id="34645-162">From the [Classic Azure Portal](https://manage.windowsazure.com/), select the Azure Stream Analytics service, and create a new Stream Analytics job:</span></span>

![](./media/app-insights-code-sample-export-sql-stream-analytics/37-create-stream-analytics.png)

![](./media/app-insights-code-sample-export-sql-stream-analytics/38-create-stream-analytics-form.png)

<span data-ttu-id="34645-163">Když je vytvořena nová úloha, rozbalte položku Podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="34645-163">When the new job is created, expand its details:</span></span>

![](./media/app-insights-code-sample-export-sql-stream-analytics/41-sa-job.png)

#### <a name="set-blob-location"></a><span data-ttu-id="34645-164">Nastavení umístění objektu blob</span><span class="sxs-lookup"><span data-stu-id="34645-164">Set blob location</span></span>
<span data-ttu-id="34645-165">Nastavte ji tak, aby vstupní z objektu blob služby průběžné Export:</span><span class="sxs-lookup"><span data-stu-id="34645-165">Set it to take input from your Continuous Export blob:</span></span>

![](./media/app-insights-code-sample-export-sql-stream-analytics/42-sa-wizard1.png)

<span data-ttu-id="34645-166">Nyní budete potřebovat primární přístupový klíč z vašeho účtu úložiště, který jste si předtím poznamenali.</span><span class="sxs-lookup"><span data-stu-id="34645-166">Now you'll need the Primary Access Key from your Storage Account, which you noted earlier.</span></span> <span data-ttu-id="34645-167">Nastavením této hodnoty jako klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="34645-167">Set this as the Storage Account Key.</span></span>

![](./media/app-insights-code-sample-export-sql-stream-analytics/46-sa-wizard2.png)

#### <a name="set-path-prefix-pattern"></a><span data-ttu-id="34645-168">Sada cesta předpona vzoru</span><span class="sxs-lookup"><span data-stu-id="34645-168">Set path prefix pattern</span></span>
![](./media/app-insights-code-sample-export-sql-stream-analytics/47-sa-wizard3.png)

<span data-ttu-id="34645-169">Nastavte formát data **rrrr-MM-DD** (s **pomlčky**).</span><span class="sxs-lookup"><span data-stu-id="34645-169">Be sure to set the Date Format to **YYYY-MM-DD** (with **dashes**).</span></span>

<span data-ttu-id="34645-170">Cesta předpony vzor Určuje, jak Stream Analytics vyhledá vstupní soubory v úložišti.</span><span class="sxs-lookup"><span data-stu-id="34645-170">The Path Prefix Pattern specifies how Stream Analytics finds the input files in the storage.</span></span> <span data-ttu-id="34645-171">Budete muset nastavit tak, aby odpovídaly jak průběžné exportovat data uloží.</span><span class="sxs-lookup"><span data-stu-id="34645-171">You need to set it to correspond to how Continuous Export stores the data.</span></span> <span data-ttu-id="34645-172">Nastavte takto:</span><span class="sxs-lookup"><span data-stu-id="34645-172">Set it like this:</span></span>

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

<span data-ttu-id="34645-173">V tomto příkladu:</span><span class="sxs-lookup"><span data-stu-id="34645-173">In this example:</span></span>

* <span data-ttu-id="34645-174">`webapplication27`je název prostředku Application Insights **vše na malá písmena**.</span><span class="sxs-lookup"><span data-stu-id="34645-174">`webapplication27` is the name of the Application Insights resource, **all in lower case**.</span></span> 
* <span data-ttu-id="34645-175">`1234...`je klíč instrumentace prostředku Application Insights **s pomlčkami odebrat**.</span><span class="sxs-lookup"><span data-stu-id="34645-175">`1234...` is the instrumentation key of the Application Insights resource **with dashes removed**.</span></span> 
* <span data-ttu-id="34645-176">`PageViews`je typ dat, který chcete analyzovat.</span><span class="sxs-lookup"><span data-stu-id="34645-176">`PageViews` is the type of data we want to analyze.</span></span> <span data-ttu-id="34645-177">Dostupné typy závisí na filtr, který nastavíte v průběžné exportovat.</span><span class="sxs-lookup"><span data-stu-id="34645-177">The available types depend on the filter you set in Continuous Export.</span></span> <span data-ttu-id="34645-178">Zkontrolujte exportovaná data zobrazit dostupné typy a zobrazit [Exportovat datový model](app-insights-export-data-model.md).</span><span class="sxs-lookup"><span data-stu-id="34645-178">Examine the exported data to see the other available types, and see the [export data model](app-insights-export-data-model.md).</span></span>
* <span data-ttu-id="34645-179">`/{date}/{time}`vzor zapsána oznámena.</span><span class="sxs-lookup"><span data-stu-id="34645-179">`/{date}/{time}` is a pattern written literally.</span></span>

<span data-ttu-id="34645-180">Chcete-li získat název a iKey prostředku Application Insights, otevřete Essentials na stránku s jeho přehled nebo otevřete nastavení.</span><span class="sxs-lookup"><span data-stu-id="34645-180">To get the name and iKey of your Application Insights resource, open Essentials on its overview page, or open Settings.</span></span>

#### <a name="finish-initial-setup"></a><span data-ttu-id="34645-181">Dokončit počáteční nastavení</span><span class="sxs-lookup"><span data-stu-id="34645-181">Finish initial setup</span></span>
<span data-ttu-id="34645-182">Zkontrolujte formát serializace:</span><span class="sxs-lookup"><span data-stu-id="34645-182">Confirm the serialization format:</span></span>

![Potvrďte a zavřete průvodce](./media/app-insights-code-sample-export-sql-stream-analytics/48-sa-wizard4.png)

<span data-ttu-id="34645-184">Zavřete průvodce a počkejte na dokončení instalace.</span><span class="sxs-lookup"><span data-stu-id="34645-184">Close the wizard and wait for the setup to complete.</span></span>

> [!TIP]
> <span data-ttu-id="34645-185">Zkontrolujte, že jste správně nastavili vstupní cesta použijte funkci Sample.</span><span class="sxs-lookup"><span data-stu-id="34645-185">Use the Sample function to check that you have set the input path correctly.</span></span> <span data-ttu-id="34645-186">Pokud se nezdaří: Zkontrolujte, zda je data v úložišti pro ukázkové časové rozmezí jste zvolili.</span><span class="sxs-lookup"><span data-stu-id="34645-186">If it fails: Check that there is data in the storage for the sample time range you chose.</span></span> <span data-ttu-id="34645-187">Upravte definici vstupní a zkontrolujte nastavení účtu úložiště, cesta předponu a datum formátu správně.</span><span class="sxs-lookup"><span data-stu-id="34645-187">Edit the input definition and check you set the storage account, path prefix and date format correctly.</span></span>
> 
> 

## <a name="set-query"></a><span data-ttu-id="34645-188">Sada dotazu</span><span class="sxs-lookup"><span data-stu-id="34645-188">Set query</span></span>
<span data-ttu-id="34645-189">Otevřete část dotazu:</span><span class="sxs-lookup"><span data-stu-id="34645-189">Open the query section:</span></span>

![V služby stream analytics vyberte dotazu](./media/app-insights-code-sample-export-sql-stream-analytics/51-query.png)

<span data-ttu-id="34645-191">Nahraďte výchozí dotaz s:</span><span class="sxs-lookup"><span data-stu-id="34645-191">Replace the default query with:</span></span>

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

<span data-ttu-id="34645-192">Všimněte si, že první několik vlastností jsou specifické pro data zobrazení stránky.</span><span class="sxs-lookup"><span data-stu-id="34645-192">Notice that the first few properties are specific to page view data.</span></span> <span data-ttu-id="34645-193">Export jiné typy telemetrických dat bude mít jiné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="34645-193">Exports of other telemetry types will have different properties.</span></span> <span data-ttu-id="34645-194">Najdete v článku [podrobné referenční model dat pro typy vlastností a hodnoty.](app-insights-export-data-model.md)</span><span class="sxs-lookup"><span data-stu-id="34645-194">See the [detailed data model reference for the property types and values.](app-insights-export-data-model.md)</span></span>

## <a name="set-up-output-to-database"></a><span data-ttu-id="34645-195">Nastavit výstup do databáze</span><span class="sxs-lookup"><span data-stu-id="34645-195">Set up output to database</span></span>
<span data-ttu-id="34645-196">Vyberte SQL jako výstup.</span><span class="sxs-lookup"><span data-stu-id="34645-196">Select SQL as the output.</span></span>

![V služby stream analytics vyberte výstupy](./media/app-insights-code-sample-export-sql-stream-analytics/53-store.png)

<span data-ttu-id="34645-198">Zadejte databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="34645-198">Specify the SQL database.</span></span>

![Zadejte podrobnosti databáze](./media/app-insights-code-sample-export-sql-stream-analytics/55-output.png)

<span data-ttu-id="34645-200">Zavřete průvodce a čekat na oznámení, že byla nastavena výstup.</span><span class="sxs-lookup"><span data-stu-id="34645-200">Close the wizard and wait for a notification that the output has been set up.</span></span>

## <a name="start-processing"></a><span data-ttu-id="34645-201">Spuštění zpracování</span><span class="sxs-lookup"><span data-stu-id="34645-201">Start processing</span></span>
<span data-ttu-id="34645-202">Spustíte úlohu z panelu akcí:</span><span class="sxs-lookup"><span data-stu-id="34645-202">Start the job from the action bar:</span></span>

![V služby stream analytics klikněte na tlačítko Start](./media/app-insights-code-sample-export-sql-stream-analytics/61-start.png)

<span data-ttu-id="34645-204">Můžete zvolit, jestli se má spustit zpracování dat od teď nebo začínat starší data.</span><span class="sxs-lookup"><span data-stu-id="34645-204">You can choose whether to start processing the data starting from now, or to start with earlier data.</span></span> <span data-ttu-id="34645-205">Je užitečné, pokud jste předtím průběžné exportovat už běží nějakou dobu.</span><span class="sxs-lookup"><span data-stu-id="34645-205">The latter is useful if you have had Continuous Export already running for a while.</span></span>

![V služby stream analytics klikněte na tlačítko Start](./media/app-insights-code-sample-export-sql-stream-analytics/63-start.png)

<span data-ttu-id="34645-207">Po několika minutách vraťte se zpátky a nástroje správy systému SQL Server a sledovat data předávaná v.</span><span class="sxs-lookup"><span data-stu-id="34645-207">After a few minutes, go back to SQL Server Management Tools and watch the data flowing in.</span></span> <span data-ttu-id="34645-208">Například použijte dotaz takto:</span><span class="sxs-lookup"><span data-stu-id="34645-208">For example, use a query like this:</span></span>

    SELECT TOP 100 *
    FROM [dbo].[PageViewsTable]


## <a name="related-articles"></a><span data-ttu-id="34645-209">Související články</span><span class="sxs-lookup"><span data-stu-id="34645-209">Related articles</span></span>
* [<span data-ttu-id="34645-210">Exportovat do PowerBI pomocí služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="34645-210">Export to PowerBI using Stream Analytics</span></span>](app-insights-export-power-bi.md)
* [<span data-ttu-id="34645-211">Podrobný datový model referenční informace pro vlastnost typů a hodnot.</span><span class="sxs-lookup"><span data-stu-id="34645-211">Detailed data model reference for the property types and values.</span></span>](app-insights-export-data-model.md)
* [<span data-ttu-id="34645-212">Průběžné Export ve službě Application Insights</span><span class="sxs-lookup"><span data-stu-id="34645-212">Continuous Export in Application Insights</span></span>](app-insights-export-telemetry.md)
* [<span data-ttu-id="34645-213">Application Insights</span><span class="sxs-lookup"><span data-stu-id="34645-213">Application Insights</span></span>](https://azure.microsoft.com/services/application-insights/)

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[export]: app-insights-export-telemetry.md
[metrics]: app-insights-metrics-explorer.md
[portal]: http://portal.azure.com/
[start]: app-insights-overview.md

