---
title: "aaaEnabling úložiště metriky v hello portálu Azure | Microsoft Docs"
description: "Jak tooenable úložiště metriky pro hello službám Blob, fronty, tabulky a souborů"
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 0407adfc-2a41-4126-922d-b76e90b74563
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/14/2017
ms.author: robinsh
ms.openlocfilehash: f157dbdf9a256da7ab186f80db746b91d1a9beb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enabling-azure-storage-metrics-and-viewing-metrics-data"></a><span data-ttu-id="723d3-103">Zapnutí metrik Azure Storage a prohlížení dat metrik</span><span class="sxs-lookup"><span data-stu-id="723d3-103">Enabling Azure Storage metrics and viewing metrics data</span></span>
[!INCLUDE [storage-selector-portal-enable-and-view-metrics](../../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a><span data-ttu-id="723d3-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="723d3-104">Overview</span></span>
<span data-ttu-id="723d3-105">Úložiště Metrics je povolena ve výchozím nastavení, když vytvoříte nový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="723d3-105">Storage Metrics is enabled by default when you create a new storage account.</span></span> <span data-ttu-id="723d3-106">Můžete nakonfigurovat monitorování prostřednictvím hello [portál Azure](https://portal.azure.com) nebo prostředí Windows PowerShell, nebo prostřednictvím kódu programu prostřednictvím knihovny klienta úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="723d3-106">You can configure monitoring via hello [Azure portal](https://portal.azure.com) or Windows PowerShell, or programmatically via one of hello storage client libraries.</span></span>

<span data-ttu-id="723d3-107">Můžete nakonfigurovat dobu uchování dat metrik hello: Tato doba určuje, jak dlouho hello úložiště služby udržuje hello metriky a poplatky, můžete pro hello místo požadované toostore je.</span><span class="sxs-lookup"><span data-stu-id="723d3-107">You can configure a retention period for hello metrics data: this period determines for how long hello storage service keeps hello metrics and charges you for hello space required toostore them.</span></span> <span data-ttu-id="723d3-108">Obvykle měli byste použít kratší doby uchování pro minutu metriky než hodinová metriky kvůli hello významné přebytečné místo požadované pro minutu metriky.</span><span class="sxs-lookup"><span data-stu-id="723d3-108">Typically, you should use a shorter retention period for minute metrics than hourly metrics because of hello significant extra space required for minute metrics.</span></span> <span data-ttu-id="723d3-109">Měli byste vybrat dobu uchovávání tak, aby dostatečný čas tooanalyze hello dat a stáhnout všechny metriky, které chcete tookeep pro offline analýzu nebo pro účely generování sestav.</span><span class="sxs-lookup"><span data-stu-id="723d3-109">You should choose a retention period such that you have sufficient time tooanalyze hello data and download any metrics you wish tookeep for off-line analysis or reporting purposes.</span></span> <span data-ttu-id="723d3-110">Mějte na paměti, že vám budou dostávat také pro stahování dat metriky z vašeho účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="723d3-110">Remember that you will also be billed for downloading metrics data from your storage account.</span></span>

## <a name="how-tooenable-metrics-using-hello-azure-portal"></a><span data-ttu-id="723d3-111">Jak hello tooenable metriky pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="723d3-111">How tooenable metrics using hello Azure portal</span></span>
<span data-ttu-id="723d3-112">Postupujte podle těchto kroků tooenable metriky v hello [portál Azure](https://portal.azure.com):</span><span class="sxs-lookup"><span data-stu-id="723d3-112">Follow these steps tooenable metrics in hello [Azure portal](https://portal.azure.com):</span></span>

1. <span data-ttu-id="723d3-113">Přejděte tooyour účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="723d3-113">Navigate tooyour storage account.</span></span>
1. <span data-ttu-id="723d3-114">Vyberte **diagnostiky** na hello **nabídky** okna</span><span class="sxs-lookup"><span data-stu-id="723d3-114">Select **Diagnostics** on hello **Menu** blade</span></span>
1. <span data-ttu-id="723d3-115">Ujistěte se, že **stav** je nastaven příliš**na**.</span><span class="sxs-lookup"><span data-stu-id="723d3-115">Ensure that **Status** is set too**On**.</span></span>
1. <span data-ttu-id="723d3-116">Vyberte hello metriky pro služby hello chcete toomonitor.</span><span class="sxs-lookup"><span data-stu-id="723d3-116">Select hello metrics for hello services you wish toomonitor.</span></span>
1. <span data-ttu-id="723d3-117">Zadejte tooindicate zásady uchovávání informací jak dlouho data tooretain metriky a protokolu.</span><span class="sxs-lookup"><span data-stu-id="723d3-117">Specify a retention policy tooindicate how long tooretain metrics and log data.</span></span>
1. <span data-ttu-id="723d3-118">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="723d3-118">Select **Save**.</span></span>

<span data-ttu-id="723d3-119">Všimněte si, že hello [portál Azure](https://portal.azure.com) neumožňuje aktuálně tooconfigure minutu metriky ve vašem účtu úložiště; je nutné povolit minutu metriky pomocí prostředí PowerShell nebo prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="723d3-119">Note that hello [Azure portal](https://portal.azure.com) does not currently enable you tooconfigure minute metrics in your storage account; you must enable minute metrics using PowerShell or programmatically.</span></span>

## <a name="how-tooenable-metrics-using-powershell"></a><span data-ttu-id="723d3-120">Jak tooenable metriky pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="723d3-120">How tooenable metrics using PowerShell</span></span>
<span data-ttu-id="723d3-121">Pomocí prostředí PowerShell na váš místní počítač tooconfigure metriky úložiště ve vašem účtu úložiště pomocí hello prostředí Azure PowerShell rutinu Get-AzureStorageServiceMetricsProperty tooretrieve hello aktuální nastavení a hello rutiny Set-AzureStorageServiceMetricsProperty toochange hello aktuální nastavení.</span><span class="sxs-lookup"><span data-stu-id="723d3-121">You can use PowerShell on your local machine tooconfigure Storage Metrics in your storage account by using hello Azure PowerShell cmdlet Get-AzureStorageServiceMetricsProperty tooretrieve hello current settings, and hello cmdlet Set-AzureStorageServiceMetricsProperty toochange hello current settings.</span></span>

<span data-ttu-id="723d3-122">Hello rutiny, které řídí metriky úložiště používají hello následující parametry:</span><span class="sxs-lookup"><span data-stu-id="723d3-122">hello cmdlets that control Storage Metrics use hello following parameters:</span></span>

* <span data-ttu-id="723d3-123">MetricsType: možné hodnoty jsou hodin a minut.</span><span class="sxs-lookup"><span data-stu-id="723d3-123">MetricsType: possible values are Hour and Minute.</span></span>
* <span data-ttu-id="723d3-124">ServiceType: možné hodnoty jsou objekt Blob, fronty a tabulky.</span><span class="sxs-lookup"><span data-stu-id="723d3-124">ServiceType: possible values are Blob, Queue, and Table.</span></span>
* <span data-ttu-id="723d3-125">MetricsLevel: možné hodnoty jsou None, služby a ServiceAndApi.</span><span class="sxs-lookup"><span data-stu-id="723d3-125">MetricsLevel: possible values are None, Service, and ServiceAndApi.</span></span>

<span data-ttu-id="723d3-126">Například hello následující příkaz přepne na minutu metriky pro hello služby objektů Blob v účtu úložiště výchozí s dobou uchování hello nastavit toofive dny:</span><span class="sxs-lookup"><span data-stu-id="723d3-126">For example, hello following command switches on minute metrics for hello Blob service in your default storage account with hello retention period set toofive days:</span></span>

```powershell
Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5`
```

<span data-ttu-id="723d3-127">Hello následující příkaz načte hello aktuální hodinové metriky úrovně a jejich uchovávání dní hello služby objektů blob v účtu úložiště výchozí:</span><span class="sxs-lookup"><span data-stu-id="723d3-127">hello following command retrieves hello current hourly metrics level and retention days for hello blob service in your default storage account:</span></span>

```powershell
Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob
```

<span data-ttu-id="723d3-128">Informace o tom, jak tooconfigure hello prostředí Azure PowerShell rutiny toowork ve vašem předplatném Azure a jak tooselect hello výchozí úložiště účtu toouse najdete v tématu: [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="723d3-128">For information about how tooconfigure hello Azure PowerShell cmdlets toowork with your Azure subscription and how tooselect hello default storage account toouse, see: [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="how-tooenable-storage-metrics-programmatically"></a><span data-ttu-id="723d3-129">Jak tooenable metriky úložiště prostřednictvím kódu programu</span><span class="sxs-lookup"><span data-stu-id="723d3-129">How tooenable Storage metrics programmatically</span></span>
<span data-ttu-id="723d3-130">Následující fragment kódu jazyka C# Hello ukazuje, jak tooenable metrik a protokolování pro použití služby Blob hello hello Klientská knihovna pro úložiště pro .NET:</span><span class="sxs-lookup"><span data-stu-id="723d3-130">hello following C# snippet shows how tooenable metrics and logging for hello Blob service using hello storage client library for .NET:</span></span>

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

## <a name="viewing-storage-metrics"></a><span data-ttu-id="723d3-131">Zobrazení metriky úložiště</span><span class="sxs-lookup"><span data-stu-id="723d3-131">Viewing Storage metrics</span></span>
<span data-ttu-id="723d3-132">Po dokončení konfigurace účtu úložiště Storage Analytics metriky toomonitor, Storage Analytics zaznamenává hello metriky v sadě dobře známé tabulek ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="723d3-132">After you configure Storage Analytics metrics toomonitor your storage account, Storage Analytics records hello metrics in a set of well-known tables in your storage account.</span></span> <span data-ttu-id="723d3-133">Grafy tooview hodinové metriky můžete nakonfigurovat v hello [portál Azure](https://portal.azure.com):</span><span class="sxs-lookup"><span data-stu-id="723d3-133">You can configure charts tooview hourly metrics in hello [Azure portal](https://portal.azure.com):</span></span>

1. <span data-ttu-id="723d3-134">Přejděte tooyour účet úložiště v hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="723d3-134">Navigate tooyour storage account in hello [Azure portal](https://portal.azure.com).</span></span>
1. <span data-ttu-id="723d3-135">Vyberte **metriky** v hello **nabídky** okně hello služby jejichž metriky chcete tooview.</span><span class="sxs-lookup"><span data-stu-id="723d3-135">Select **Metrics** in hello **Menu** blade for hello service whose metrics you want tooview.</span></span>
1. <span data-ttu-id="723d3-136">Vyberte **upravit** v grafu hello chcete tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="723d3-136">Select **Edit** on hello chart you want tooconfigure.</span></span>
1. <span data-ttu-id="723d3-137">V hello **upravit graf** okně, vyberte hello **časový rozsah**, **typ grafu**a hello metriky, které chcete zobrazit v grafu hello.</span><span class="sxs-lookup"><span data-stu-id="723d3-137">In hello **Edit Chart** blade, select hello **Time Range**, **Chart type**, and hello metrics you want displayed in hello chart.</span></span>
1. <span data-ttu-id="723d3-138">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="723d3-138">Select **OK**</span></span>

<span data-ttu-id="723d3-139">Pokud chcete, aby toodownload hello metriky pro dlouhodobé uložení nebo tooanalyze je místně, budete muset:</span><span class="sxs-lookup"><span data-stu-id="723d3-139">If you want toodownload hello metrics for long-term storage or tooanalyze them locally, you will need to:</span></span>

* <span data-ttu-id="723d3-140">Použijte nástroj, který má informace o těchto tabulek a bude umožňují tooview a je stáhnout.</span><span class="sxs-lookup"><span data-stu-id="723d3-140">Use a tool that is aware of these tables and will allow you tooview and download them.</span></span>
* <span data-ttu-id="723d3-141">Napsat vlastní aplikace nebo skriptu tooread a ukládání tabulek hello.</span><span class="sxs-lookup"><span data-stu-id="723d3-141">Write a custom application or script tooread and store hello tables.</span></span>

<span data-ttu-id="723d3-142">Celou řadu nástrojů procházení úložiště jiných výrobců Uvědomte těchto tabulek a umožňují tooview je přímo.</span><span class="sxs-lookup"><span data-stu-id="723d3-142">Many third-party storage-browsing tools are aware of these tables and enable you tooview them directly.</span></span>
<span data-ttu-id="723d3-143">Najdete v tématu [Azure Storage Client Tools](storage-explorers.md) seznam dostupných nástrojů.</span><span class="sxs-lookup"><span data-stu-id="723d3-143">Please see [Azure Storage Client Tools](storage-explorers.md) for a list of available tools.</span></span>

> [!NOTE]
> <span data-ttu-id="723d3-144">Počínaje verzí 0.8.0 hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/), můžete zobrazit a stáhnout hello analytics metriky tabulky.</span><span class="sxs-lookup"><span data-stu-id="723d3-144">Starting with version 0.8.0 of hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/), you can view and download hello analytics metrics tables.</span></span>
> 
> 

<span data-ttu-id="723d3-145">V pořadí tooaccess hello analytics tabulky prostřednictvím kódu programu, Všimněte si, hello analytics tabulky se nezobrazí Pokud seznam všech tabulek hello ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="723d3-145">In order tooaccess hello analytics tables programmatically, do note that hello analytics tables do not appear if you list all hello tables in your storage account.</span></span> <span data-ttu-id="723d3-146">Můžete k nim přistupovat přímo podle názvu, nebo použijte hello [CloudAnalyticsClient API](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.analytics.cloudanalyticsclient.aspx) v hello .NET klienta knihovny tooquery hello názvy tabulek.</span><span class="sxs-lookup"><span data-stu-id="723d3-146">You can either access them directly by name, or use hello [CloudAnalyticsClient API](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.analytics.cloudanalyticsclient.aspx) in hello .NET client library tooquery hello table names.</span></span>

### <a name="hourly-metrics"></a><span data-ttu-id="723d3-147">Hodinové metriky</span><span class="sxs-lookup"><span data-stu-id="723d3-147">Hourly metrics</span></span>
* <span data-ttu-id="723d3-148">$MetricsHourPrimaryTransactionsBlob</span><span class="sxs-lookup"><span data-stu-id="723d3-148">$MetricsHourPrimaryTransactionsBlob</span></span>
* <span data-ttu-id="723d3-149">$MetricsHourPrimaryTransactionsTable</span><span class="sxs-lookup"><span data-stu-id="723d3-149">$MetricsHourPrimaryTransactionsTable</span></span>
* <span data-ttu-id="723d3-150">$MetricsHourPrimaryTransactionsQueue</span><span class="sxs-lookup"><span data-stu-id="723d3-150">$MetricsHourPrimaryTransactionsQueue</span></span>

### <a name="minute-metrics"></a><span data-ttu-id="723d3-151">Minutu metriky</span><span class="sxs-lookup"><span data-stu-id="723d3-151">Minute metrics</span></span>
* <span data-ttu-id="723d3-152">$MetricsMinutePrimaryTransactionsBlob</span><span class="sxs-lookup"><span data-stu-id="723d3-152">$MetricsMinutePrimaryTransactionsBlob</span></span>
* <span data-ttu-id="723d3-153">$MetricsMinutePrimaryTransactionsTable</span><span class="sxs-lookup"><span data-stu-id="723d3-153">$MetricsMinutePrimaryTransactionsTable</span></span>
* <span data-ttu-id="723d3-154">$MetricsMinutePrimaryTransactionsQueue</span><span class="sxs-lookup"><span data-stu-id="723d3-154">$MetricsMinutePrimaryTransactionsQueue</span></span>

### <a name="capacity"></a><span data-ttu-id="723d3-155">Kapacita</span><span class="sxs-lookup"><span data-stu-id="723d3-155">Capacity</span></span>
* <span data-ttu-id="723d3-156">$MetricsCapacityBlob</span><span class="sxs-lookup"><span data-stu-id="723d3-156">$MetricsCapacityBlob</span></span>

<span data-ttu-id="723d3-157">Můžete najít podrobnosti o hello schémata pro tyto tabulky v [schématu tabulky metriky Analytics úložiště](https://msdn.microsoft.com/library/azure/hh343264.aspx).</span><span class="sxs-lookup"><span data-stu-id="723d3-157">You can find full details of hello schemas for these tables at [Storage Analytics Metrics Table Schema](https://msdn.microsoft.com/library/azure/hh343264.aspx).</span></span> <span data-ttu-id="723d3-158">Hello ukázka řádků níže zobrazit pouze podmnožinu sloupců hello k dispozici, ale ilustrovat některé důležité funkce hello způsob, jakým Storage Metrics uloží tyto metriky:</span><span class="sxs-lookup"><span data-stu-id="723d3-158">hello sample rows below show only a subset of hello columns available, but illustrate some important features of hello way Storage Metrics saves these metrics:</span></span>

| <span data-ttu-id="723d3-159">Klíč oddílu</span><span class="sxs-lookup"><span data-stu-id="723d3-159">PartitionKey</span></span> | <span data-ttu-id="723d3-160">RowKey</span><span class="sxs-lookup"><span data-stu-id="723d3-160">RowKey</span></span> | <span data-ttu-id="723d3-161">časové razítko</span><span class="sxs-lookup"><span data-stu-id="723d3-161">Timestamp</span></span> | <span data-ttu-id="723d3-162">TotalRequests</span><span class="sxs-lookup"><span data-stu-id="723d3-162">TotalRequests</span></span> | <span data-ttu-id="723d3-163">TotalBillableRequests</span><span class="sxs-lookup"><span data-stu-id="723d3-163">TotalBillableRequests</span></span> | <span data-ttu-id="723d3-164">TotalIngress</span><span class="sxs-lookup"><span data-stu-id="723d3-164">TotalIngress</span></span> | <span data-ttu-id="723d3-165">TotalEgress</span><span class="sxs-lookup"><span data-stu-id="723d3-165">TotalEgress</span></span> | <span data-ttu-id="723d3-166">Dostupnost</span><span class="sxs-lookup"><span data-stu-id="723d3-166">Availability</span></span> | <span data-ttu-id="723d3-167">AverageE2ELatency</span><span class="sxs-lookup"><span data-stu-id="723d3-167">AverageE2ELatency</span></span> | <span data-ttu-id="723d3-168">AverageServerLatency</span><span class="sxs-lookup"><span data-stu-id="723d3-168">AverageServerLatency</span></span> | <span data-ttu-id="723d3-169">PercentSuccess</span><span class="sxs-lookup"><span data-stu-id="723d3-169">PercentSuccess</span></span> |
| --- |:---:| ---:| --- | --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="723d3-170">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="723d3-170">20140522T1100</span></span> |<span data-ttu-id="723d3-171">uživatel; Všechny</span><span class="sxs-lookup"><span data-stu-id="723d3-171">user;All</span></span> |<span data-ttu-id="723d3-172">2014-05-22T11:01:16.7650250Z</span><span class="sxs-lookup"><span data-stu-id="723d3-172">2014-05-22T11:01:16.7650250Z</span></span> |<span data-ttu-id="723d3-173">7</span><span class="sxs-lookup"><span data-stu-id="723d3-173">7</span></span> |<span data-ttu-id="723d3-174">7</span><span class="sxs-lookup"><span data-stu-id="723d3-174">7</span></span> |<span data-ttu-id="723d3-175">4003</span><span class="sxs-lookup"><span data-stu-id="723d3-175">4003</span></span> |<span data-ttu-id="723d3-176">46801</span><span class="sxs-lookup"><span data-stu-id="723d3-176">46801</span></span> |<span data-ttu-id="723d3-177">100</span><span class="sxs-lookup"><span data-stu-id="723d3-177">100</span></span> |<span data-ttu-id="723d3-178">104.4286</span><span class="sxs-lookup"><span data-stu-id="723d3-178">104.4286</span></span> |<span data-ttu-id="723d3-179">6.857143</span><span class="sxs-lookup"><span data-stu-id="723d3-179">6.857143</span></span> |<span data-ttu-id="723d3-180">100</span><span class="sxs-lookup"><span data-stu-id="723d3-180">100</span></span> |
| <span data-ttu-id="723d3-181">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="723d3-181">20140522T1100</span></span> |<span data-ttu-id="723d3-182">uživatel; QueryEntities</span><span class="sxs-lookup"><span data-stu-id="723d3-182">user;QueryEntities</span></span> |<span data-ttu-id="723d3-183">2014-05-22T11:01:16.7640250Z</span><span class="sxs-lookup"><span data-stu-id="723d3-183">2014-05-22T11:01:16.7640250Z</span></span> |<span data-ttu-id="723d3-184">5</span><span class="sxs-lookup"><span data-stu-id="723d3-184">5</span></span> |<span data-ttu-id="723d3-185">5</span><span class="sxs-lookup"><span data-stu-id="723d3-185">5</span></span> |<span data-ttu-id="723d3-186">2694</span><span class="sxs-lookup"><span data-stu-id="723d3-186">2694</span></span> |<span data-ttu-id="723d3-187">45951</span><span class="sxs-lookup"><span data-stu-id="723d3-187">45951</span></span> |<span data-ttu-id="723d3-188">100</span><span class="sxs-lookup"><span data-stu-id="723d3-188">100</span></span> |<span data-ttu-id="723d3-189">143.8</span><span class="sxs-lookup"><span data-stu-id="723d3-189">143.8</span></span> |<span data-ttu-id="723d3-190">7.8</span><span class="sxs-lookup"><span data-stu-id="723d3-190">7.8</span></span> |<span data-ttu-id="723d3-191">100</span><span class="sxs-lookup"><span data-stu-id="723d3-191">100</span></span> |
| <span data-ttu-id="723d3-192">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="723d3-192">20140522T1100</span></span> |<span data-ttu-id="723d3-193">uživatel; QueryEntity</span><span class="sxs-lookup"><span data-stu-id="723d3-193">user;QueryEntity</span></span> |<span data-ttu-id="723d3-194">2014-05-22T11:01:16.7650250Z</span><span class="sxs-lookup"><span data-stu-id="723d3-194">2014-05-22T11:01:16.7650250Z</span></span> |<span data-ttu-id="723d3-195">1</span><span class="sxs-lookup"><span data-stu-id="723d3-195">1</span></span> |<span data-ttu-id="723d3-196">1</span><span class="sxs-lookup"><span data-stu-id="723d3-196">1</span></span> |<span data-ttu-id="723d3-197">538</span><span class="sxs-lookup"><span data-stu-id="723d3-197">538</span></span> |<span data-ttu-id="723d3-198">633</span><span class="sxs-lookup"><span data-stu-id="723d3-198">633</span></span> |<span data-ttu-id="723d3-199">100</span><span class="sxs-lookup"><span data-stu-id="723d3-199">100</span></span> |<span data-ttu-id="723d3-200">3</span><span class="sxs-lookup"><span data-stu-id="723d3-200">3</span></span> |<span data-ttu-id="723d3-201">3</span><span class="sxs-lookup"><span data-stu-id="723d3-201">3</span></span> |<span data-ttu-id="723d3-202">100</span><span class="sxs-lookup"><span data-stu-id="723d3-202">100</span></span> |
| <span data-ttu-id="723d3-203">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="723d3-203">20140522T1100</span></span> |<span data-ttu-id="723d3-204">uživatel; UpdateEntity</span><span class="sxs-lookup"><span data-stu-id="723d3-204">user;UpdateEntity</span></span> |<span data-ttu-id="723d3-205">2014-05-22T11:01:16.7650250Z</span><span class="sxs-lookup"><span data-stu-id="723d3-205">2014-05-22T11:01:16.7650250Z</span></span> |<span data-ttu-id="723d3-206">1</span><span class="sxs-lookup"><span data-stu-id="723d3-206">1</span></span> |<span data-ttu-id="723d3-207">1</span><span class="sxs-lookup"><span data-stu-id="723d3-207">1</span></span> |<span data-ttu-id="723d3-208">771</span><span class="sxs-lookup"><span data-stu-id="723d3-208">771</span></span> |<span data-ttu-id="723d3-209">217</span><span class="sxs-lookup"><span data-stu-id="723d3-209">217</span></span> |<span data-ttu-id="723d3-210">100</span><span class="sxs-lookup"><span data-stu-id="723d3-210">100</span></span> |<span data-ttu-id="723d3-211">9</span><span class="sxs-lookup"><span data-stu-id="723d3-211">9</span></span> |<span data-ttu-id="723d3-212">6</span><span class="sxs-lookup"><span data-stu-id="723d3-212">6</span></span> |<span data-ttu-id="723d3-213">100</span><span class="sxs-lookup"><span data-stu-id="723d3-213">100</span></span> |

<span data-ttu-id="723d3-214">V těchto datech minutu metriky příklad používá klíč oddílu hello hello čas na minutu řešení.</span><span class="sxs-lookup"><span data-stu-id="723d3-214">In this example minute metrics data, hello partition key uses hello time at minute resolution.</span></span> <span data-ttu-id="723d3-215">klíč řádku Hello identifikuje hello typ informací uložených v řádku hello a tím se skládá ze dvou částí informace, typ přístupu hello a typ požadavku hello:</span><span class="sxs-lookup"><span data-stu-id="723d3-215">hello row key identifies hello type of information that is stored in hello row and this is composed of two pieces of information, hello access type, and hello request type:</span></span>

* <span data-ttu-id="723d3-216">Typ přístupu Hello je uživatele nebo systému, kde uživatel odkazuje tooall uživatelské požadavky toohello úložiště služby, a systém odkazuje toorequests provedené Storage Analytics.</span><span class="sxs-lookup"><span data-stu-id="723d3-216">hello access type is either user or system, where user refers tooall user requests toohello storage service, and system refers toorequests made by Storage Analytics.</span></span>
* <span data-ttu-id="723d3-217">Typ požadavku Hello je všechny v takovém případě je souhrn řádku, nebo ji identifikuje hello konkrétní API jako třeba QueryEntity nebo UpdateEntity.</span><span class="sxs-lookup"><span data-stu-id="723d3-217">hello request type is either all in which case it is a summary line, or it identifies hello specific API such as QueryEntity or UpdateEntity.</span></span>

<span data-ttu-id="723d3-218">Ukázková data Hello výš ukazuje, které všechny hello zaznamenává pro jednu minutu (počínaje 11:00 AM), takže hello počet požadavků QueryEntities plus hello počet požadavků QueryEntity plus počet hello UpdateEntity požadavků, které dohromady tooseven, což je hello celkový zobrazený na řádek uživatele: všechny Hello.</span><span class="sxs-lookup"><span data-stu-id="723d3-218">hello sample data above shows all hello records for a single minute (starting at 11:00AM), so hello number of QueryEntities requests plus hello number of QueryEntity requests plus hello number of UpdateEntity requests add up tooseven, which is hello total shown on hello user:All row.</span></span> <span data-ttu-id="723d3-219">Podobně můžete odvodit hello průměrnou dobu začátku do konce 104.4286 na řádku uživatele: všechny hello pomocí výpočtu ((143.8 * 5) + 3 + 9) / 7.</span><span class="sxs-lookup"><span data-stu-id="723d3-219">Similarly, you can derive hello average end-to-end latency 104.4286 on hello user:All row by calculating ((143.8 * 5) + 3 + 9)/7.</span></span>

## <a name="metrics-alerts"></a><span data-ttu-id="723d3-220">Metriky výstrahy</span><span class="sxs-lookup"><span data-stu-id="723d3-220">Metrics alerts</span></span>
<span data-ttu-id="723d3-221">Měli byste zvážit vytvoření výstrahy v hello [portál Azure](https://portal.azure.com) tak metriky úložiště automaticky vás může upozornit, důležité změny v chování hello vaší služby úložiště.</span><span class="sxs-lookup"><span data-stu-id="723d3-221">You should consider setting up alerts in hello [Azure portal](https://portal.azure.com) so Storage Metrics can automatically notify you of important changes in hello behavior of your storage services.</span></span> <span data-ttu-id="723d3-222">Pokud toodownload nástroj Průzkumníka úložiště používáte ve formátu odděleného tato data metriky, můžete data hello tooanalyze Microsoft Excel.</span><span class="sxs-lookup"><span data-stu-id="723d3-222">If you use a storage explorer tool toodownload this metrics data in a delimited format, you can use Microsoft Excel tooanalyze hello data.</span></span> <span data-ttu-id="723d3-223">V tématu [Azure Storage Client Tools](storage-explorers.md) seznam nástrojů Průzkumníka úložiště k dispozici.</span><span class="sxs-lookup"><span data-stu-id="723d3-223">See [Azure Storage Client Tools](storage-explorers.md) for a list of available storage explorer tools.</span></span> <span data-ttu-id="723d3-224">Výstrahy můžete nakonfigurovat v hello **výstrah pravidla** okně přístupná **monitorování** v okně nabídce účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="723d3-224">You can configure alerts in hello **Alert rules** blade, accessible under **Monitoring** in hello Storage account menu blade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="723d3-225">Může nastat zpoždění mezi událostí úložiště a když se zaznamenává hello odpovídající hodinových nebo minutu metriky data.</span><span class="sxs-lookup"><span data-stu-id="723d3-225">There may be a delay between a storage event and when hello corresponding hourly or minute metrics data is recorded.</span></span> <span data-ttu-id="723d3-226">V případě hello minutu metrik může několik minut dat zapsat najednou.</span><span class="sxs-lookup"><span data-stu-id="723d3-226">In hello case of minute metrics, several minutes of data may be written at once.</span></span> <span data-ttu-id="723d3-227">To může způsobit tootransactions z dřívějších minut agregovaný do hello transakci pro hello aktuální minuty.</span><span class="sxs-lookup"><span data-stu-id="723d3-227">This can lead tootransactions from earlier minutes being aggregated into hello transaction for hello current minute.</span></span> <span data-ttu-id="723d3-228">V takovém případě hello výstraha, že službu nemusí mít všechna data dostupné metriky pro hello nakonfigurovat výstrahy interval, což může vést tooalerts neočekávaně aktivuje.</span><span class="sxs-lookup"><span data-stu-id="723d3-228">When this happens, hello alert service may not have all available metrics data for hello configured alert interval, which may lead tooalerts firing unexpectedly.</span></span>
>

## <a name="accessing-metrics-data-programmatically"></a><span data-ttu-id="723d3-229">Přístup k datům metriky prostřednictvím kódu programu</span><span class="sxs-lookup"><span data-stu-id="723d3-229">Accessing metrics data programmatically</span></span>
<span data-ttu-id="723d3-230">Hello následující výpis ukazuje ukázka C# kód, který přistupuje k hello minutu metriky pro řadu minut a zobrazí výsledky hello v okně konzoly.</span><span class="sxs-lookup"><span data-stu-id="723d3-230">hello following listing shows sample C# code that accesses hello minute metrics for a range of minutes and displays hello results in a console Window.</span></span> <span data-ttu-id="723d3-231">Používá hello knihovny pro úložiště Azure verze 4, který zahrnuje hello CloudAnalyticsClient třídu, která zjednodušuje přístupem hello metriky tabulek v úložišti.</span><span class="sxs-lookup"><span data-stu-id="723d3-231">It uses hello Azure Storage Library version 4 that includes hello CloudAnalyticsClient class that simplifies accessing hello metrics tables in storage.</span></span>

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

## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a><span data-ttu-id="723d3-232">Jaké poplatky platit při povolení metrik úložiště?</span><span class="sxs-lookup"><span data-stu-id="723d3-232">What charges do you incur when you enable storage metrics?</span></span>
<span data-ttu-id="723d3-233">Požadavky toocreate tabulka entity metrik, které jsou výši hello standardních sazeb použít tooall Azure Storage operace zápisu</span><span class="sxs-lookup"><span data-stu-id="723d3-233">Write requests toocreate table entities for metrics are charged at hello standard rates applicable tooall Azure Storage operations.</span></span>

<span data-ttu-id="723d3-234">Čtení a odstraňování požadavků podle dat toometrics klienta jsou taky fakturovatelný na standardních sazeb.</span><span class="sxs-lookup"><span data-stu-id="723d3-234">Read and delete requests by a client toometrics data are also billable at standard rates.</span></span> <span data-ttu-id="723d3-235">Pokud jste nakonfigurovali zásadu uchovávání dat, vám není účtován když odstraní stará data metrik Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="723d3-235">If you have configured a data retention policy, you are not charged when Azure Storage deletes old metrics data.</span></span> <span data-ttu-id="723d3-236">Pokud odstraníte analytická data, váš účet účtovat pro operace odstranění hello.</span><span class="sxs-lookup"><span data-stu-id="723d3-236">However, if you delete analytics data, your account is charged for hello delete operations.</span></span>

<span data-ttu-id="723d3-237">kapacita Hello používá tabulky metriky hello je také fakturovatelný: můžete použít následující tooestimate hello množství kapacity, na které se používá k ukládání dat metriky hello:</span><span class="sxs-lookup"><span data-stu-id="723d3-237">hello capacity used by hello metrics tables is also billable: you can use hello following tooestimate hello amount of capacity used for storing metrics data:</span></span>

* <span data-ttu-id="723d3-238">Pokud každou hodinu služba využívá každé rozhraní API v každé službě, pak přibližně 148KB dat je uložený každou hodinu do tabulky transakcí metriky hello Pokud jste povolili službu a úroveň API souhrnu.</span><span class="sxs-lookup"><span data-stu-id="723d3-238">If each hour a service utilizes every API in every service, then approximately 148KB of data is stored every hour in hello metrics transaction tables if you have enabled both service and API level summary.</span></span>
* <span data-ttu-id="723d3-239">Pokud každou hodinu služba využívá každé rozhraní API v každé službě, pak přibližně 12KB dat je uložený každou hodinu do tabulky transakcí metriky hello Pokud jste povolili právě služby úrovni souhrnu.</span><span class="sxs-lookup"><span data-stu-id="723d3-239">If each hour a service utilizes every API in every service, then approximately 12KB of data is stored every hour in hello metrics transaction tables if you have enabled just service level summary.</span></span>
* <span data-ttu-id="723d3-240">Hello kapacity pro objekty BLOB má dva řádky přidat každý den (za předpokladu, že uživatel, má vyjádřit výslovný souhlas pro protokoly): to znamená, že každý den hello velikost této tabulky zvyšuje úroveň až tooapproximately 300 bajtů.</span><span class="sxs-lookup"><span data-stu-id="723d3-240">hello capacity table for blobs has two rows added each day (provided user has opted in for logs): this implies that every day hello size of this table increases by up tooapproximately 300 bytes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="723d3-241">Další kroky</span><span class="sxs-lookup"><span data-stu-id="723d3-241">Next steps</span></span>
[<span data-ttu-id="723d3-242">Povolení protokolování a přístup k datům protokolu úložiště</span><span class="sxs-lookup"><span data-stu-id="723d3-242">Enabling Storage Logging and Accessing Log Data</span></span>](/rest/api/storageservices/Enabling-Storage-Logging-and-Accessing-Log-Data)
