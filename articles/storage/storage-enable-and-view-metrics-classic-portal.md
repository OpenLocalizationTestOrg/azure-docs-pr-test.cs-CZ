---
title: "aaaEnabling úložiště metriky v hello portálu Azure | Microsoft Docs"
description: "Jak tooenable úložiště metriky pro hello službám Blob, fronty, tabulky a souborů"
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 2fb5b229-f099-4334-92be-4e0e7dd257d7
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/03/2017
ms.author: robinsh
ms.openlocfilehash: 4c990371e08a6586d935b0535149eabd4960cfaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enabling-storage-metrics-and-viewing-metrics-data"></a><span data-ttu-id="e2fd8-103">Zapnutí metrik úložiště a prohlížení dat metrik</span><span class="sxs-lookup"><span data-stu-id="e2fd8-103">Enabling Storage metrics and viewing metrics data</span></span>
[!INCLUDE [storage-selector-portal-enable-and-view-metrics](../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a><span data-ttu-id="e2fd8-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="e2fd8-104">Overview</span></span>
<span data-ttu-id="e2fd8-105">Úložiště Metrics je povolena ve výchozím nastavení, když vytvoříte nový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-105">Storage Metrics is enabled by default when you create a new storage account.</span></span> <span data-ttu-id="e2fd8-106">Můžete nakonfigurovat monitorování pomocí buď hello [portálu Azure Classic](https://manage.windowsazure.com), prostředí Windows PowerShell nebo programově pomocí rozhraní API úložiště.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-106">You can configure monitoring using either hello [Azure Classic Portal](https://manage.windowsazure.com), Windows PowerShell, or programmatically through a storage API.</span></span>

<span data-ttu-id="e2fd8-107">Pokud povolíte metriky úložiště, musíte vybrat dobu uchování dat hello: Tato doba určuje, jak dlouho hello úložiště služby udržuje hello metriky a poplatky, můžete pro hello místo požadované toostore je.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-107">When you enable Storage Metrics, you must choose a retention period for hello data: this period determines for how long hello storage service keeps hello metrics and charges you for hello space required toostore them.</span></span> <span data-ttu-id="e2fd8-108">Obvykle měli byste použít kratší doby uchování pro minutu metriky než hodinová metriky kvůli hello významné přebytečné místo požadované pro minutu metriky.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-108">Typically, you should use a shorter retention period for minute metrics than hourly metrics because of hello significant extra space required for minute metrics.</span></span> <span data-ttu-id="e2fd8-109">Měli byste vybrat dobu uchovávání tak, aby dostatečný čas tooanalyze hello dat a stáhnout všechny metriky, které chcete tookeep pro offline analýzu nebo pro účely generování sestav.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-109">You should choose a retention period such that you have sufficient time tooanalyze hello data and download any metrics you wish tookeep for off-line analysis or reporting purposes.</span></span> <span data-ttu-id="e2fd8-110">Mějte na paměti, že vám budou dostávat také pro stahování dat metriky z vašeho účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-110">Remember that you will also be billed for downloading metrics data from your storage account.</span></span>

## <a name="how-tooenable-storage-metrics-using-hello-azure-classic-portal"></a><span data-ttu-id="e2fd8-111">Jak hello metriky tooenable úložiště pomocí portálu Azure Classic</span><span class="sxs-lookup"><span data-stu-id="e2fd8-111">How tooenable Storage metrics using hello Azure Classic Portal</span></span>
<span data-ttu-id="e2fd8-112">V hello [portálu Azure Classic](https://manage.windowsazure.com), použijte stránku hello konfigurovat pro toocontrol účet úložiště Storage Metrics.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-112">In hello [Azure Classic Portal](https://manage.windowsazure.com), you use hello Configure page for a storage account toocontrol Storage Metrics.</span></span> <span data-ttu-id="e2fd8-113">Pro monitorování, můžete nastavit úroveň a doby uchování ve dnech pro všechny objekty BLOB, tabulek a front.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-113">For monitoring, you can set a level and a retention period in days for each of blobs, tables, and queues.</span></span> <span data-ttu-id="e2fd8-114">V každém případě hello úroveň je jedna z následujících hello:</span><span class="sxs-lookup"><span data-stu-id="e2fd8-114">In each case, hello level is one of hello following:</span></span>

* <span data-ttu-id="e2fd8-115">Vypnutí – Nebyly shromážděny žádné metriky.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-115">Off — No metrics are collected.</span></span>
* <span data-ttu-id="e2fd8-116">Minimální – Storage Metrics shromažďuje základní sadu metriky jako vstupní/výstupní, dostupnosti, latence a procenta úspěšnosti, které se shromažďují pro služby objektů Blob, Table a Queue hello.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-116">Minimal — Storage Metrics collects a basic set of metrics such as ingress/egress, availability, latency, and success percentages, which are aggregated for hello Blob, Table, and Queue services.</span></span>
* <span data-ttu-id="e2fd8-117">Verbose – Shromažďuje metriky úložiště úplnou sadu metriky, které zahrnuje hello stejné metriky pro každý úložiště operace rozhraní API, kromě toohello úrovně služeb metriky.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-117">Verbose — Storage Metrics collects a full set of metrics that includes hello same metrics for each storage API operation, in addition toohello service-level metrics.</span></span> <span data-ttu-id="e2fd8-118">Podrobné metriky povolit blíže analyzovat problémy, ke kterým došlo během operací aplikace.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-118">Verbose metrics enable closer analysis of issues that occur during application operations.</span></span>

<span data-ttu-id="e2fd8-119">Všimněte si, že hello portálu Azure Classic neumožňuje aktuálně tooconfigure minutu metriky ve vašem účtu úložiště; je nutné povolit minutu metriky pomocí prostředí PowerShell nebo prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-119">Note that hello Azure Classic Portal does not currently enable you tooconfigure minute metrics in your storage account; you must enable minute metrics using PowerShell or programmatically.</span></span>

## <a name="how-tooenable-storage-metrics-using-powershell"></a><span data-ttu-id="e2fd8-120">Jak tooenable metriky úložiště pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="e2fd8-120">How tooenable Storage metrics using PowerShell</span></span>
<span data-ttu-id="e2fd8-121">Pomocí prostředí PowerShell na váš místní počítač tooconfigure metriky úložiště ve vašem účtu úložiště pomocí hello prostředí Azure PowerShell rutinu Get-AzureStorageServiceMetricsProperty tooretrieve hello aktuální nastavení a hello rutiny Set-AzureStorageServiceMetricsProperty toochange hello aktuální nastavení.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-121">You can use PowerShell on your local machine tooconfigure Storage Metrics in your storage account by using hello Azure PowerShell cmdlet Get-AzureStorageServiceMetricsProperty tooretrieve hello current settings, and hello cmdlet Set-AzureStorageServiceMetricsProperty toochange hello current settings.</span></span>

<span data-ttu-id="e2fd8-122">Hello rutiny, které řídí metriky úložiště používají hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="e2fd8-122">hello cmdlets that control Storage Metrics use hello following parameters:</span></span>

* <span data-ttu-id="e2fd8-123">MetricsType možné hodnoty jsou hodin a minut.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-123">MetricsType possible values are Hour and Minute.</span></span>
* <span data-ttu-id="e2fd8-124">ServiceType možné hodnoty jsou objekt Blob, fronty a tabulky.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-124">ServiceType possible values are Blob, Queue, and Table.</span></span>
* <span data-ttu-id="e2fd8-125">MetricsLevel možné hodnoty jsou None (ekvivalentní tooOff v hello portálu Azure Classic), služba (ekvivalentní tooMinimal v hello portálu Azure Classic) a ServiceAndApi (ekvivalentní tooVerbose v hello portálu Azure Classic).</span><span class="sxs-lookup"><span data-stu-id="e2fd8-125">MetricsLevel possible values are None (equivalent tooOff in hello Azure Classic Portal), Service (equivalent tooMinimal in hello Azure Classic Portal), and ServiceAndApi (equivalent tooVerbose in hello Azure Classic Portal).</span></span>

<span data-ttu-id="e2fd8-126">Například hello následující příkaz přepne na minutu metriky pro hello služby objektů blob v účtu úložiště výchozí s dobou uchování hello nastavit toofive dny:</span><span class="sxs-lookup"><span data-stu-id="e2fd8-126">For example, hello following command switches on minute metrics for hello blob service in your default storage account with hello retention period set toofive days:</span></span>

```powershell
Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5
```
<span data-ttu-id="e2fd8-127">Hello následující příkaz načte hello aktuální hodinové metriky úrovně a jejich uchovávání dní hello služby objektů blob v účtu úložiště výchozí:</span><span class="sxs-lookup"><span data-stu-id="e2fd8-127">hello following command retrieves hello current hourly metrics level and retention days for hello blob service in your default storage account:</span></span>

```powershell
Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob
```
<span data-ttu-id="e2fd8-128">Informace o tom, jak tooconfigure hello prostředí Azure PowerShell rutiny toowork ve vašem předplatném Azure a jak tooselect hello výchozí úložiště účtu toouse najdete v tématu: [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e2fd8-128">For information about how tooconfigure hello Azure PowerShell cmdlets toowork with your Azure subscription and how tooselect hello default storage account toouse, see: [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="how-tooenable-storage-metrics-programmatically"></a><span data-ttu-id="e2fd8-129">Jak tooenable metriky úložiště prostřednictvím kódu programu</span><span class="sxs-lookup"><span data-stu-id="e2fd8-129">How tooenable Storage metrics programmatically</span></span>
<span data-ttu-id="e2fd8-130">Následující fragment kódu jazyka C# Hello ukazuje, jak tooenable metrik a protokolování pro použití služby Blob hello hello Klientská knihovna pro úložiště pro .NET:</span><span class="sxs-lookup"><span data-stu-id="e2fd8-130">hello following C# snippet shows how tooenable metrics and logging for hello Blob service using hello storage client library for .NET:</span></span>

```csharp
//Parse hello connection string for hello storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

// Create service client for credentialed access toohello Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Enable Storage Analytics logging and set retention policy too10 days. 
ServiceProperties properties = new ServiceProperties();
properties.Logging.LoggingOperations = LoggingOperations.All;
properties.Logging.RetentionDays = 10;
properties.Logging.Version = "1.0";

// Configure service properties for metrics. Both metrics and logging must be set at hello same time.
properties.HourMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
properties.HourMetrics.RetentionDays = 10;
properties.HourMetrics.Version = "1.0";

properties.MinuteMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
properties.MinuteMetrics.RetentionDays = 10;
properties.MinuteMetrics.Version = "1.0";

// Set hello default service version toobe used for anonymous requests.
properties.DefaultServiceVersion = "2015-04-05";

// Set hello service properties.
blobClient.SetServiceProperties(properties);
```

## <a name="viewing-storage-metrics"></a><span data-ttu-id="e2fd8-131">Zobrazení metriky úložiště</span><span class="sxs-lookup"><span data-stu-id="e2fd8-131">Viewing Storage metrics</span></span>
<span data-ttu-id="e2fd8-132">Pokud jste nakonfigurovali úložiště metriky toomonitor účtu úložiště, zaznamenává hello metriky v sadě dobře známé tabulek ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-132">When you have configured Storage Metrics toomonitor your storage account, it records hello metrics in a set of well-known tables in your storage account.</span></span> <span data-ttu-id="e2fd8-133">Můžete stránku hello monitorování pro váš účet úložiště na portálu Azure Classic tooview hello hodinové metriky hello, jakmile budou k dispozici v grafu.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-133">You can use hello Monitor page for your storage account in hello Azure Classic Portal tooview hello hourly metrics as they become available on a chart.</span></span> <span data-ttu-id="e2fd8-134">Na tuto stránku hello portálu Azure Classic můžete:</span><span class="sxs-lookup"><span data-stu-id="e2fd8-134">On this page in hello Azure Classic Portal, you can:</span></span>

* <span data-ttu-id="e2fd8-135">Vyberte které tooplot metriky v grafu hello (hello volba dostupné metriky závisí na tom, jestli jste zvolili, podrobný nebo minimální monitorování služby hello na stránce konfigurace hello).</span><span class="sxs-lookup"><span data-stu-id="e2fd8-135">Select which metrics tooplot on hello chart (hello choice of available metrics will depend on whether you chose verbose or minimal monitoring for hello service on hello Configure page).</span></span>
* <span data-ttu-id="e2fd8-136">Vyberte časové rozmezí hello hello metriky zobrazené v grafu hello.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-136">Select hello time range for hello metrics displayed on hello chart.</span></span>
* <span data-ttu-id="e2fd8-137">Zvolte toouse absolutní nebo relativní škálování tooplot hello metriky.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-137">Choose toouse an absolute or relative scale tooplot hello metrics.</span></span>
* <span data-ttu-id="e2fd8-138">Konfigurace e-mailové výstrahy toonotify případě určité metriky dosáhne určitou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-138">Configure email alerts toonotify you when a specific metric reaches a certain value.</span></span>

<span data-ttu-id="e2fd8-139">Pokud chcete toodownload hello metriky pro dlouhodobé uložení nebo tooanalyze je místně, budete potřebovat toouse nástroj nebo napsat kód tooread hello tabulky.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-139">If you want toodownload hello metrics for long-term storage or tooanalyze them locally, you will need toouse a tool or write some code tooread hello tables.</span></span> <span data-ttu-id="e2fd8-140">Je nutné stáhnout hello minutu metriky pro analýzu.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-140">You must download hello minute metrics for analysis.</span></span> <span data-ttu-id="e2fd8-141">Pokud seznam všech tabulek hello ve vašem účtu úložiště, ale můžete je přejít přímo pomocí názvu tabulky Hello nejsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-141">hello tables do not appear if you list all hello tables in your storage account, but you can access them directly by name.</span></span> <span data-ttu-id="e2fd8-142">Celou řadu nástrojů procházení úložiště jiných výrobců Uvědomte těchto tabulek a umožňují tooview je přímo (najdete v příspěvku blogu hello [Průzkumníci úložiště Microsoft Azure](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) seznam dostupných nástrojů).</span><span class="sxs-lookup"><span data-stu-id="e2fd8-142">Many third-party storage-browsing tools are aware of these tables and enable you tooview them directly (see hello blog post [Microsoft Azure Storage Explorers](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) for a list of available tools).</span></span>

### <a name="hourly-metrics"></a><span data-ttu-id="e2fd8-143">Hodinové metriky</span><span class="sxs-lookup"><span data-stu-id="e2fd8-143">Hourly metrics</span></span>
* <span data-ttu-id="e2fd8-144">$MetricsHourPrimaryTransactionsBlob</span><span class="sxs-lookup"><span data-stu-id="e2fd8-144">$MetricsHourPrimaryTransactionsBlob</span></span>
* <span data-ttu-id="e2fd8-145">$MetricsHourPrimaryTransactionsTable</span><span class="sxs-lookup"><span data-stu-id="e2fd8-145">$MetricsHourPrimaryTransactionsTable</span></span>
* <span data-ttu-id="e2fd8-146">$MetricsHourPrimaryTransactionsQueue</span><span class="sxs-lookup"><span data-stu-id="e2fd8-146">$MetricsHourPrimaryTransactionsQueue</span></span>

### <a name="minute-metrics"></a><span data-ttu-id="e2fd8-147">Minutu metriky</span><span class="sxs-lookup"><span data-stu-id="e2fd8-147">Minute metrics</span></span>
* <span data-ttu-id="e2fd8-148">$MetricsMinutePrimaryTransactionsBlob</span><span class="sxs-lookup"><span data-stu-id="e2fd8-148">$MetricsMinutePrimaryTransactionsBlob</span></span>
* <span data-ttu-id="e2fd8-149">$MetricsMinutePrimaryTransactionsTable</span><span class="sxs-lookup"><span data-stu-id="e2fd8-149">$MetricsMinutePrimaryTransactionsTable</span></span>
* <span data-ttu-id="e2fd8-150">$MetricsMinutePrimaryTransactionsQueue</span><span class="sxs-lookup"><span data-stu-id="e2fd8-150">$MetricsMinutePrimaryTransactionsQueue</span></span>

### <a name="capacity"></a><span data-ttu-id="e2fd8-151">Kapacita</span><span class="sxs-lookup"><span data-stu-id="e2fd8-151">Capacity</span></span>
* <span data-ttu-id="e2fd8-152">$MetricsCapacityBlob</span><span class="sxs-lookup"><span data-stu-id="e2fd8-152">$MetricsCapacityBlob</span></span>

<span data-ttu-id="e2fd8-153">Můžete najít podrobnosti o hello schémata pro tyto tabulky v [schématu tabulky metriky Analytics úložiště](https://msdn.microsoft.com/library/azure/hh343264.aspx).</span><span class="sxs-lookup"><span data-stu-id="e2fd8-153">You can find full details of hello schemas for these tables at [Storage Analytics Metrics Table Schema](https://msdn.microsoft.com/library/azure/hh343264.aspx).</span></span> <span data-ttu-id="e2fd8-154">Hello ukázka řádků níže zobrazit pouze podmnožinu sloupců hello k dispozici, ale ilustrovat některé důležité funkce hello způsob, jakým Storage Metrics uloží tyto metriky:</span><span class="sxs-lookup"><span data-stu-id="e2fd8-154">hello sample rows below show only a subset of hello columns available, but illustrate some important features of hello way Storage Metrics saves these metrics:</span></span>

| <span data-ttu-id="e2fd8-155">Klíč oddílu</span><span class="sxs-lookup"><span data-stu-id="e2fd8-155">PartitionKey</span></span> | <span data-ttu-id="e2fd8-156">RowKey</span><span class="sxs-lookup"><span data-stu-id="e2fd8-156">RowKey</span></span> | <span data-ttu-id="e2fd8-157">časové razítko</span><span class="sxs-lookup"><span data-stu-id="e2fd8-157">Timestamp</span></span> | <span data-ttu-id="e2fd8-158">TotalRequests</span><span class="sxs-lookup"><span data-stu-id="e2fd8-158">TotalRequests</span></span> | <span data-ttu-id="e2fd8-159">TotalBillableRequests</span><span class="sxs-lookup"><span data-stu-id="e2fd8-159">TotalBillableRequests</span></span> | <span data-ttu-id="e2fd8-160">TotalIngress</span><span class="sxs-lookup"><span data-stu-id="e2fd8-160">TotalIngress</span></span> | <span data-ttu-id="e2fd8-161">TotalEgress</span><span class="sxs-lookup"><span data-stu-id="e2fd8-161">TotalEgress</span></span> | <span data-ttu-id="e2fd8-162">Dostupnost</span><span class="sxs-lookup"><span data-stu-id="e2fd8-162">Availability</span></span> | <span data-ttu-id="e2fd8-163">AverageE2ELatency</span><span class="sxs-lookup"><span data-stu-id="e2fd8-163">AverageE2ELatency</span></span> | <span data-ttu-id="e2fd8-164">AverageServerLatency</span><span class="sxs-lookup"><span data-stu-id="e2fd8-164">AverageServerLatency</span></span> | <span data-ttu-id="e2fd8-165">PercentSuccess</span><span class="sxs-lookup"><span data-stu-id="e2fd8-165">PercentSuccess</span></span> |
| --- |:---:| ---:| --- | --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="e2fd8-166">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="e2fd8-166">20140522T1100</span></span> |<span data-ttu-id="e2fd8-167">uživatel; Všechny</span><span class="sxs-lookup"><span data-stu-id="e2fd8-167">user;All</span></span> |<span data-ttu-id="e2fd8-168">2014-05-22T11:01:16.7650250Z</span><span class="sxs-lookup"><span data-stu-id="e2fd8-168">2014-05-22T11:01:16.7650250Z</span></span> |<span data-ttu-id="e2fd8-169">7</span><span class="sxs-lookup"><span data-stu-id="e2fd8-169">7</span></span> |<span data-ttu-id="e2fd8-170">7</span><span class="sxs-lookup"><span data-stu-id="e2fd8-170">7</span></span> |<span data-ttu-id="e2fd8-171">4003</span><span class="sxs-lookup"><span data-stu-id="e2fd8-171">4003</span></span> |<span data-ttu-id="e2fd8-172">46801</span><span class="sxs-lookup"><span data-stu-id="e2fd8-172">46801</span></span> |<span data-ttu-id="e2fd8-173">100</span><span class="sxs-lookup"><span data-stu-id="e2fd8-173">100</span></span> |<span data-ttu-id="e2fd8-174">104.4286</span><span class="sxs-lookup"><span data-stu-id="e2fd8-174">104.4286</span></span> |<span data-ttu-id="e2fd8-175">6.857143</span><span class="sxs-lookup"><span data-stu-id="e2fd8-175">6.857143</span></span> |<span data-ttu-id="e2fd8-176">100</span><span class="sxs-lookup"><span data-stu-id="e2fd8-176">100</span></span> |
| <span data-ttu-id="e2fd8-177">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="e2fd8-177">20140522T1100</span></span> |<span data-ttu-id="e2fd8-178">uživatel; QueryEntities</span><span class="sxs-lookup"><span data-stu-id="e2fd8-178">user;QueryEntities</span></span> |<span data-ttu-id="e2fd8-179">2014-05-22T11:01:16.7640250Z</span><span class="sxs-lookup"><span data-stu-id="e2fd8-179">2014-05-22T11:01:16.7640250Z</span></span> |<span data-ttu-id="e2fd8-180">5</span><span class="sxs-lookup"><span data-stu-id="e2fd8-180">5</span></span> |<span data-ttu-id="e2fd8-181">5</span><span class="sxs-lookup"><span data-stu-id="e2fd8-181">5</span></span> |<span data-ttu-id="e2fd8-182">2694</span><span class="sxs-lookup"><span data-stu-id="e2fd8-182">2694</span></span> |<span data-ttu-id="e2fd8-183">45951</span><span class="sxs-lookup"><span data-stu-id="e2fd8-183">45951</span></span> |<span data-ttu-id="e2fd8-184">100</span><span class="sxs-lookup"><span data-stu-id="e2fd8-184">100</span></span> |<span data-ttu-id="e2fd8-185">143.8</span><span class="sxs-lookup"><span data-stu-id="e2fd8-185">143.8</span></span> |<span data-ttu-id="e2fd8-186">7.8</span><span class="sxs-lookup"><span data-stu-id="e2fd8-186">7.8</span></span> |<span data-ttu-id="e2fd8-187">100</span><span class="sxs-lookup"><span data-stu-id="e2fd8-187">100</span></span> |
| <span data-ttu-id="e2fd8-188">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="e2fd8-188">20140522T1100</span></span> |<span data-ttu-id="e2fd8-189">uživatel; QueryEntity</span><span class="sxs-lookup"><span data-stu-id="e2fd8-189">user;QueryEntity</span></span> |<span data-ttu-id="e2fd8-190">2014-05-22T11:01:16.7650250Z</span><span class="sxs-lookup"><span data-stu-id="e2fd8-190">2014-05-22T11:01:16.7650250Z</span></span> |<span data-ttu-id="e2fd8-191">1</span><span class="sxs-lookup"><span data-stu-id="e2fd8-191">1</span></span> |<span data-ttu-id="e2fd8-192">1</span><span class="sxs-lookup"><span data-stu-id="e2fd8-192">1</span></span> |<span data-ttu-id="e2fd8-193">538</span><span class="sxs-lookup"><span data-stu-id="e2fd8-193">538</span></span> |<span data-ttu-id="e2fd8-194">633</span><span class="sxs-lookup"><span data-stu-id="e2fd8-194">633</span></span> |<span data-ttu-id="e2fd8-195">100</span><span class="sxs-lookup"><span data-stu-id="e2fd8-195">100</span></span> |<span data-ttu-id="e2fd8-196">3</span><span class="sxs-lookup"><span data-stu-id="e2fd8-196">3</span></span> |<span data-ttu-id="e2fd8-197">3</span><span class="sxs-lookup"><span data-stu-id="e2fd8-197">3</span></span> |<span data-ttu-id="e2fd8-198">100</span><span class="sxs-lookup"><span data-stu-id="e2fd8-198">100</span></span> |
| <span data-ttu-id="e2fd8-199">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="e2fd8-199">20140522T1100</span></span> |<span data-ttu-id="e2fd8-200">uživatel; UpdateEntity</span><span class="sxs-lookup"><span data-stu-id="e2fd8-200">user;UpdateEntity</span></span> |<span data-ttu-id="e2fd8-201">2014-05-22T11:01:16.7650250Z</span><span class="sxs-lookup"><span data-stu-id="e2fd8-201">2014-05-22T11:01:16.7650250Z</span></span> |<span data-ttu-id="e2fd8-202">1</span><span class="sxs-lookup"><span data-stu-id="e2fd8-202">1</span></span> |<span data-ttu-id="e2fd8-203">1</span><span class="sxs-lookup"><span data-stu-id="e2fd8-203">1</span></span> |<span data-ttu-id="e2fd8-204">771</span><span class="sxs-lookup"><span data-stu-id="e2fd8-204">771</span></span> |<span data-ttu-id="e2fd8-205">217</span><span class="sxs-lookup"><span data-stu-id="e2fd8-205">217</span></span> |<span data-ttu-id="e2fd8-206">100</span><span class="sxs-lookup"><span data-stu-id="e2fd8-206">100</span></span> |<span data-ttu-id="e2fd8-207">9</span><span class="sxs-lookup"><span data-stu-id="e2fd8-207">9</span></span> |<span data-ttu-id="e2fd8-208">6</span><span class="sxs-lookup"><span data-stu-id="e2fd8-208">6</span></span> |<span data-ttu-id="e2fd8-209">100</span><span class="sxs-lookup"><span data-stu-id="e2fd8-209">100</span></span> |

<span data-ttu-id="e2fd8-210">V těchto datech minutu metriky příklad používá klíč oddílu hello hello čas na minutu řešení.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-210">In this example minute metrics data, hello partition key uses hello time at minute resolution.</span></span> <span data-ttu-id="e2fd8-211">klíč řádku Hello identifikuje hello typ informací uložených v řádku hello a tím se skládá ze dvou částí informace, typ přístupu hello a typ požadavku hello:</span><span class="sxs-lookup"><span data-stu-id="e2fd8-211">hello row key identifies hello type of information that is stored in hello row and this is composed of two pieces of information, hello access type, and hello request type:</span></span>

* <span data-ttu-id="e2fd8-212">Typ přístupu Hello je uživatele nebo systému, kde uživatel odkazuje tooall uživatelské požadavky toohello úložiště služby, a systém odkazuje toorequests provedené Storage Analytics.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-212">hello access type is either user or system, where user refers tooall user requests toohello storage service, and system refers toorequests made by Storage Analytics.</span></span>
* <span data-ttu-id="e2fd8-213">Typ požadavku Hello je všechny v takovém případě je souhrn řádku, nebo ji identifikuje hello konkrétní API jako třeba QueryEntity nebo UpdateEntity.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-213">hello request type is either all in which case it is a summary line, or it identifies hello specific API such as QueryEntity or UpdateEntity.</span></span>

<span data-ttu-id="e2fd8-214">Ukázková data Hello výš ukazuje, které všechny hello zaznamenává pro jednu minutu (počínaje 11:00 AM), takže hello počet požadavků QueryEntities plus hello počet požadavků QueryEntity plus počet hello UpdateEntity požadavků, které dohromady tooseven, což je hello celkový zobrazený na řádek uživatele: všechny Hello.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-214">hello sample data above shows all hello records for a single minute (starting at 11:00AM), so hello number of QueryEntities requests plus hello number of QueryEntity requests plus hello number of UpdateEntity requests add up tooseven, which is hello total shown on hello user:All row.</span></span> <span data-ttu-id="e2fd8-215">Podobně můžete odvodit hello průměrnou dobu začátku do konce 104.4286 na řádku uživatele: všechny hello pomocí výpočtu ((143.8 * 5) + 3 + 9) / 7.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-215">Similarly, you can derive hello average end-to-end latency 104.4286 on hello user:All row by calculating ((143.8 * 5) + 3 + 9)/7.</span></span>

<span data-ttu-id="e2fd8-216">Nastavení výstrah v hello portálu Azure Classic na stránce monitorování hello byste měli zvážit, aby metriky úložiště může automaticky upozornit na důležité změny v chování hello vaší služby úložiště. Pokud toodownload nástroj Průzkumníka úložiště používáte ve formátu odděleného tato data metriky, můžete data hello tooanalyze Microsoft Excel.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-216">You should consider setting up alerts in hello Azure Classic Portal on hello Monitor page so that Storage Metrics can automatically notify you of any important changes in hello behavior of your storage services.If you use a storage explorer tool toodownload this metrics data in a delimited format, you can use Microsoft Excel tooanalyze hello data.</span></span> <span data-ttu-id="e2fd8-217">Najdete v příspěvku blogu hello [Průzkumníci úložiště Microsoft Azure](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) seznam nástrojů Průzkumníka úložiště k dispozici.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-217">See hello blog post [Microsoft Azure Storage Explorers](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) for a list of available storage explorer tools.</span></span>

## <a name="accessing-metrics-data-programmatically"></a><span data-ttu-id="e2fd8-218">Přístup k datům metriky prostřednictvím kódu programu</span><span class="sxs-lookup"><span data-stu-id="e2fd8-218">Accessing metrics data programmatically</span></span>
<span data-ttu-id="e2fd8-219">Hello následující výpis ukazuje ukázka C# kód, který přistupuje k hello minutu metriky pro řadu minut a zobrazí výsledky hello v okně konzoly.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-219">hello following listing shows sample C# code that accesses hello minute metrics for a range of minutes and displays hello results in a console Window.</span></span> <span data-ttu-id="e2fd8-220">Používá hello knihovny pro úložiště Azure verze 4, který zahrnuje hello CloudAnalyticsClient třídu, která zjednodušuje přístupem hello metriky tabulek v úložišti.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-220">It uses hello Azure Storage Library version 4 that includes hello CloudAnalyticsClient class that simplifies accessing hello metrics tables in storage.</span></span>

```csharp
private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)
{
    // Convert hello dates toohello format used in hello PartitionKey
    var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");

    var services = Enum.GetValues(typeof(StorageService));
    foreach (StorageService service in services)
    {
        Console.WriteLine("Minute Metrics for Service {0} from {1} too{2} UTC", service, start, end);
        var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);
        var t = analyticsClient.GetMinuteMetricsTable(service);
        var opContext = new OperationContext();
        var query =
          from entity in metricsQuery
          // Note, you can't filter using hello entity properties Time, AccessType, or TransactionType
          // because they are calculated fields in hello MetricsEntity class.
          // hello PartitionKey identifies hello DataTime of hello metrics.
          where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0 
        select entity;

        // Filter on "user" transactions after fetching hello metrics from Table Storage.
        // (StartsWith is not supported using LINQ with Azure table storage)
        var results = query.ToList().Where(m => m.RowKey.StartsWith("user"));
        var resultString = results.Aggregate(new StringBuilder(), (builder, metrics) => builder.AppendLine(MetricsString(metrics, opContext))).ToString();
        Console.WriteLine(resultString);
    }
}

private static string MetricsString(MetricsEntity entity, OperationContext opContext)
{
    var entityProperties = entity.WriteEntity(opContext);
    var entityString =
    string.Format("Time: {0}, ", entity.Time) +
    string.Format("AccessType: {0}, ", entity.AccessType) +
    string.Format("TransactionType: {0}, ", entity.TransactionType) +
    string.Join(",", entityProperties.Select(e => new KeyValuePair<string, string>(e.Key.ToString(), e.Value.PropertyAsObject.ToString())));
    return entityString;
}
```

## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a><span data-ttu-id="e2fd8-221">Jaké poplatky platit při povolení metrik úložiště?</span><span class="sxs-lookup"><span data-stu-id="e2fd8-221">What charges do you incur when you enable storage metrics?</span></span>
<span data-ttu-id="e2fd8-222">Požadavky toocreate tabulka entity metrik, které jsou výši hello standardních sazeb použít tooall Azure Storage operace zápisu</span><span class="sxs-lookup"><span data-stu-id="e2fd8-222">Write requests toocreate table entities for metrics are charged at hello standard rates applicable tooall Azure Storage operations.</span></span>

<span data-ttu-id="e2fd8-223">Čtení a odstraňování požadavků podle dat toometrics klienta jsou taky fakturovatelný na standardních sazeb.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-223">Read and delete requests by a client toometrics data are also billable at standard rates.</span></span> <span data-ttu-id="e2fd8-224">Pokud jste nakonfigurovali zásadu uchovávání dat, vám není účtován když odstraní stará data metrik Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-224">If you have configured a data retention policy, you are not charged when Azure Storage deletes old metrics data.</span></span> <span data-ttu-id="e2fd8-225">Pokud odstraníte analytická data, váš účet účtovat pro operace odstranění hello.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-225">However, if you delete analytics data, your account is charged for hello delete operations.</span></span>

<span data-ttu-id="e2fd8-226">kapacita Hello používá tabulky metriky hello je také fakturovatelný: můžete použít následující tooestimate hello množství kapacity, na které se používá k ukládání dat metriky hello:</span><span class="sxs-lookup"><span data-stu-id="e2fd8-226">hello capacity used by hello metrics tables is also billable: you can use hello following tooestimate hello amount of capacity used for storing metrics data:</span></span>

* <span data-ttu-id="e2fd8-227">Pokud každou hodinu služba využívá každé rozhraní API v každé službě, pak přibližně 148KB dat je uložený každou hodinu do tabulky transakcí metriky hello Pokud jste povolili službu a úroveň API souhrnu.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-227">If each hour a service utilizes every API in every service, then approximately 148KB of data is stored every hour in hello metrics transaction tables if you have enabled both service and API level summary.</span></span>
* <span data-ttu-id="e2fd8-228">Pokud každou hodinu služba využívá každé rozhraní API v každé službě, pak přibližně 12KB dat je uložený každou hodinu do tabulky transakcí metriky hello Pokud jste povolili právě služby úrovni souhrnu.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-228">If each hour a service utilizes every API in every service, then approximately 12KB of data is stored every hour in hello metrics transaction tables if you have enabled just service level summary.</span></span>
* <span data-ttu-id="e2fd8-229">Hello kapacity pro objekty BLOB má dva řádky přidat každý den (za předpokladu, že uživatel, má vyjádřit výslovný souhlas pro protokoly): to znamená, že každý den hello velikost této tabulky zvyšuje úroveň až tooapproximately 300 bajtů.</span><span class="sxs-lookup"><span data-stu-id="e2fd8-229">hello capacity table for blobs has two rows added each day (provided user has opted in for logs): this implies that every day hello size of this table increases by up tooapproximately 300 bytes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e2fd8-230">Další kroky:</span><span class="sxs-lookup"><span data-stu-id="e2fd8-230">Next steps:</span></span>
[<span data-ttu-id="e2fd8-231">Povolení analytika úložiště protokolování a přístup k datům protokolu</span><span class="sxs-lookup"><span data-stu-id="e2fd8-231">Enabling Storage Analytics Logging and Accessing Log Data</span></span>](https://msdn.microsoft.com/library/dn782840.aspx)
