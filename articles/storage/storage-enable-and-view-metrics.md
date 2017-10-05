---
title: "Povolení metrik úložiště na portálu Azure | Microsoft Docs"
description: "Postup povolení úložiště metriky pro služby objektů Blob, fronty, tabulky a souborů"
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
ms.openlocfilehash: 2906f808482d0b990e3ddd31ef5368e9fdd9f646
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="enabling-azure-storage-metrics-and-viewing-metrics-data"></a><span data-ttu-id="cfed7-103">Zapnutí metrik Azure Storage a prohlížení dat metrik</span><span class="sxs-lookup"><span data-stu-id="cfed7-103">Enabling Azure Storage metrics and viewing metrics data</span></span>
[!INCLUDE [storage-selector-portal-enable-and-view-metrics](../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a><span data-ttu-id="cfed7-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="cfed7-104">Overview</span></span>
<span data-ttu-id="cfed7-105">Úložiště Metrics je povolena ve výchozím nastavení, když vytvoříte nový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="cfed7-105">Storage Metrics is enabled by default when you create a new storage account.</span></span> <span data-ttu-id="cfed7-106">Můžete nakonfigurovat monitorování prostřednictvím [portál Azure](https://portal.azure.com) nebo prostředí Windows PowerShell, nebo prostřednictvím kódu programu prostřednictvím knihovny klienta úložiště.</span><span class="sxs-lookup"><span data-stu-id="cfed7-106">You can configure monitoring via the [Azure portal](https://portal.azure.com) or Windows PowerShell, or programmatically via one of the storage client libraries.</span></span>

<span data-ttu-id="cfed7-107">Můžete nakonfigurovat dobu uchování dat metrik: Toto období určuje, jak dlouho úložiště služby udržuje metriky a poplatky, že v prostoru třeba je uložit.</span><span class="sxs-lookup"><span data-stu-id="cfed7-107">You can configure a retention period for the metrics data: this period determines for how long the storage service keeps the metrics and charges you for the space required to store them.</span></span> <span data-ttu-id="cfed7-108">Obvykle měli byste použít kratší doby uchování pro minutu metriky než hodinová metriky z důvodu významné přebytečné místo požadované pro minutu metriky.</span><span class="sxs-lookup"><span data-stu-id="cfed7-108">Typically, you should use a shorter retention period for minute metrics than hourly metrics because of the significant extra space required for minute metrics.</span></span> <span data-ttu-id="cfed7-109">Měli byste vybrat dobu uchovávání tak, že máte dostatek času k analýze dat a stáhnout všechny metriky, které chcete zachovat pro offline analýzu nebo pro účely generování sestav.</span><span class="sxs-lookup"><span data-stu-id="cfed7-109">You should choose a retention period such that you have sufficient time to analyze the data and download any metrics you wish to keep for off-line analysis or reporting purposes.</span></span> <span data-ttu-id="cfed7-110">Mějte na paměti, že vám budou dostávat také pro stahování dat metriky z vašeho účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="cfed7-110">Remember that you will also be billed for downloading metrics data from your storage account.</span></span>

## <a name="how-to-enable-metrics-using-the-azure-portal"></a><span data-ttu-id="cfed7-111">Postup povolení metriky pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="cfed7-111">How to enable metrics using the Azure portal</span></span>
<span data-ttu-id="cfed7-112">Použijte následující postup metriky v povolit [portál Azure](https://portal.azure.com):</span><span class="sxs-lookup"><span data-stu-id="cfed7-112">Follow these steps to enable metrics in the [Azure portal](https://portal.azure.com):</span></span>

1. <span data-ttu-id="cfed7-113">Přejděte na svůj účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="cfed7-113">Navigate to your storage account.</span></span>
1. <span data-ttu-id="cfed7-114">Vyberte **diagnostiky** na **nabídky** okna</span><span class="sxs-lookup"><span data-stu-id="cfed7-114">Select **Diagnostics** on the **Menu** blade</span></span>
1. <span data-ttu-id="cfed7-115">Ujistěte se, že **stav** je nastaven na **na**.</span><span class="sxs-lookup"><span data-stu-id="cfed7-115">Ensure that **Status** is set to **On**.</span></span>
1. <span data-ttu-id="cfed7-116">Vyberte metriky pro služby, které chcete monitorovat.</span><span class="sxs-lookup"><span data-stu-id="cfed7-116">Select the metrics for the services you wish to monitor.</span></span>
1. <span data-ttu-id="cfed7-117">Zadejte zásady uchovávání informací a jak dlouho informuje zachovat metriky a protokolovat data.</span><span class="sxs-lookup"><span data-stu-id="cfed7-117">Specify a retention policy to indicate how long to retain metrics and log data.</span></span>
1. <span data-ttu-id="cfed7-118">Vyberte **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="cfed7-118">Select **Save**.</span></span>

<span data-ttu-id="cfed7-119">Všimněte si, že [portál Azure](https://portal.azure.com) neumožňuje aktuálně nakonfigurovat minutu metriky v účtu úložiště; je nutné povolit minutu metriky pomocí prostředí PowerShell nebo prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="cfed7-119">Note that the [Azure portal](https://portal.azure.com) does not currently enable you to configure minute metrics in your storage account; you must enable minute metrics using PowerShell or programmatically.</span></span>

## <a name="how-to-enable-metrics-using-powershell"></a><span data-ttu-id="cfed7-120">Postup povolení metriky pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="cfed7-120">How to enable metrics using PowerShell</span></span>
<span data-ttu-id="cfed7-121">Konfigurace úložiště metriky ve vašem účtu úložiště pomocí rutiny prostředí Azure PowerShell Get-AzureStorageServiceMetricsProperty načíst aktuální nastavení a rutinu Set-AzureStorageServiceMetricsProperty změnit aktuální nastavení můžete použít PowerShell na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="cfed7-121">You can use PowerShell on your local machine to configure Storage Metrics in your storage account by using the Azure PowerShell cmdlet Get-AzureStorageServiceMetricsProperty to retrieve the current settings, and the cmdlet Set-AzureStorageServiceMetricsProperty to change the current settings.</span></span>

<span data-ttu-id="cfed7-122">Rutiny, které řídí metriky úložiště použijte následující parametry:</span><span class="sxs-lookup"><span data-stu-id="cfed7-122">The cmdlets that control Storage Metrics use the following parameters:</span></span>

* <span data-ttu-id="cfed7-123">MetricsType: možné hodnoty jsou hodin a minut.</span><span class="sxs-lookup"><span data-stu-id="cfed7-123">MetricsType: possible values are Hour and Minute.</span></span>
* <span data-ttu-id="cfed7-124">ServiceType: možné hodnoty jsou objekt Blob, fronty a tabulky.</span><span class="sxs-lookup"><span data-stu-id="cfed7-124">ServiceType: possible values are Blob, Queue, and Table.</span></span>
* <span data-ttu-id="cfed7-125">MetricsLevel: možné hodnoty jsou None, služby a ServiceAndApi.</span><span class="sxs-lookup"><span data-stu-id="cfed7-125">MetricsLevel: possible values are None, Service, and ServiceAndApi.</span></span>

<span data-ttu-id="cfed7-126">Například následující příkaz přepne na minutu metriky pro služby objektů Blob v účtu úložiště výchozí s dobou uchování období nastavit na pět dní:</span><span class="sxs-lookup"><span data-stu-id="cfed7-126">For example, the following command switches on minute metrics for the Blob service in your default storage account with the retention period set to five days:</span></span>

```powershell
Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5`
```

<span data-ttu-id="cfed7-127">Tento příkaz načte aktuální hodinové metriky úrovně a jejich uchovávání dny služby objektů blob na výchozí účet úložiště:</span><span class="sxs-lookup"><span data-stu-id="cfed7-127">The following command retrieves the current hourly metrics level and retention days for the blob service in your default storage account:</span></span>

```powershell
Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob
```

<span data-ttu-id="cfed7-128">Informace o tom, jak nakonfigurovat rutin prostředí Azure PowerShell k práci s předplatným Azure a jak vybrat výchozí účet úložiště, který chcete použít, najdete v tématu: [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="cfed7-128">For information about how to configure the Azure PowerShell cmdlets to work with your Azure subscription and how to select the default storage account to use, see: [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="how-to-enable-storage-metrics-programmatically"></a><span data-ttu-id="cfed7-129">Postup povolení metrik úložiště prostřednictvím kódu programu</span><span class="sxs-lookup"><span data-stu-id="cfed7-129">How to enable Storage metrics programmatically</span></span>
<span data-ttu-id="cfed7-130">Následující fragment C# ukazuje, jak povolit protokolování pro službu Blob pomocí klientské knihovny pro úložiště pro .NET a metriky:</span><span class="sxs-lookup"><span data-stu-id="cfed7-130">The following C# snippet shows how to enable metrics and logging for the Blob service using the storage client library for .NET:</span></span>

```csharp
//Parse the connection string for the storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

// Create service client for credentialed access to the Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Enable Storage Analytics logging and set retention policy to 10 days.
ServiceProperties properties = new ServiceProperties();
properties.Logging.LoggingOperations = LoggingOperations.All;
properties.Logging.RetentionDays = 10;
properties.Logging.Version = "1.0";

// Configure service properties for metrics. Both metrics and logging must be set at the same time.
properties.HourMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
properties.HourMetrics.RetentionDays = 10;
properties.HourMetrics.Version = "1.0";

properties.MinuteMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
properties.MinuteMetrics.RetentionDays = 10;
properties.MinuteMetrics.Version = "1.0";

// Set the default service version to be used for anonymous requests.
properties.DefaultServiceVersion = "2015-04-05";

// Set the service properties.
blobClient.SetServiceProperties(properties);
```

## <a name="viewing-storage-metrics"></a><span data-ttu-id="cfed7-131">Zobrazení metriky úložiště</span><span class="sxs-lookup"><span data-stu-id="cfed7-131">Viewing Storage metrics</span></span>
<span data-ttu-id="cfed7-132">Po dokončení konfigurace metriky Storage Analytics k monitorování účtu úložiště, Storage Analytics zaznamenává metriky v sadě dobře známé tabulek ve vašem účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="cfed7-132">After you configure Storage Analytics metrics to monitor your storage account, Storage Analytics records the metrics in a set of well-known tables in your storage account.</span></span> <span data-ttu-id="cfed7-133">Můžete nakonfigurovat grafy, aby se po hodinách metriky v zobrazit [portál Azure](https://portal.azure.com):</span><span class="sxs-lookup"><span data-stu-id="cfed7-133">You can configure charts to view hourly metrics in the [Azure portal](https://portal.azure.com):</span></span>

1. <span data-ttu-id="cfed7-134">Přejděte na svůj účet úložiště [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="cfed7-134">Navigate to your storage account in the [Azure portal](https://portal.azure.com).</span></span>
1. <span data-ttu-id="cfed7-135">Vyberte **metriky** v **nabídky** okna pro službu jejichž metriky, které chcete zobrazit.</span><span class="sxs-lookup"><span data-stu-id="cfed7-135">Select **Metrics** in the **Menu** blade for the service whose metrics you want to view.</span></span>
1. <span data-ttu-id="cfed7-136">Vyberte **upravit** v grafu, kterou chcete konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="cfed7-136">Select **Edit** on the chart you want to configure.</span></span>
1. <span data-ttu-id="cfed7-137">V **upravit graf** okně, vyberte **časový rozsah**, **typ grafu**a metriky, které chcete zobrazit v grafu.</span><span class="sxs-lookup"><span data-stu-id="cfed7-137">In the **Edit Chart** blade, select the **Time Range**, **Chart type**, and the metrics you want displayed in the chart.</span></span>
1. <span data-ttu-id="cfed7-138">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="cfed7-138">Select **OK**</span></span>

<span data-ttu-id="cfed7-139">Pokud chcete stáhnout metriky pro dlouhodobé uložení nebo pro analýzu je místně, je potřeba:</span><span class="sxs-lookup"><span data-stu-id="cfed7-139">If you want to download the metrics for long-term storage or to analyze them locally, you will need to:</span></span>

* <span data-ttu-id="cfed7-140">Použijte nástroj, který má informace o těchto tabulek a vám umožní zobrazit a je stáhnout.</span><span class="sxs-lookup"><span data-stu-id="cfed7-140">Use a tool that is aware of these tables and will allow you to view and download them.</span></span>
* <span data-ttu-id="cfed7-141">Napište vlastní aplikaci nebo skript ke čtení a ukládání tabulek.</span><span class="sxs-lookup"><span data-stu-id="cfed7-141">Write a custom application or script to read and store the tables.</span></span>

<span data-ttu-id="cfed7-142">Celou řadu nástrojů procházení úložiště jiných výrobců jsou informace o těchto tabulek a vám umožní zobrazit přímo.</span><span class="sxs-lookup"><span data-stu-id="cfed7-142">Many third-party storage-browsing tools are aware of these tables and enable you to view them directly.</span></span>
<span data-ttu-id="cfed7-143">Najdete v tématu [Azure Storage Client Tools](storage-explorers.md) seznam dostupných nástrojů.</span><span class="sxs-lookup"><span data-stu-id="cfed7-143">Please see [Azure Storage Client Tools](storage-explorers.md) for a list of available tools.</span></span>

> [!NOTE]
> <span data-ttu-id="cfed7-144">Počínaje verzí 0.8.0 [Microsoft Azure Storage Explorer](http://storageexplorer.com/), můžete zobrazit a stáhnout tabulky metriky analytics.</span><span class="sxs-lookup"><span data-stu-id="cfed7-144">Starting with version 0.8.0 of the [Microsoft Azure Storage Explorer](http://storageexplorer.com/), you can view and download the analytics metrics tables.</span></span>
> 
> 

<span data-ttu-id="cfed7-145">K přístupu k tabulce analýzy prostřednictvím kódu programu, Všimněte si, analýzy tabulky se nezobrazí Pokud seznamu všechny tabulky v účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="cfed7-145">In order to access the analytics tables programmatically, do note that the analytics tables do not appear if you list all the tables in your storage account.</span></span> <span data-ttu-id="cfed7-146">Můžete k nim přistupovat přímo podle názvu, nebo použít [CloudAnalyticsClient API](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.analytics.cloudanalyticsclient.aspx) v klientské knihovny .NET k dotazování názvy tabulek.</span><span class="sxs-lookup"><span data-stu-id="cfed7-146">You can either access them directly by name, or use the [CloudAnalyticsClient API](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.analytics.cloudanalyticsclient.aspx) in the .NET client library to query the table names.</span></span>

### <a name="hourly-metrics"></a><span data-ttu-id="cfed7-147">Hodinové metriky</span><span class="sxs-lookup"><span data-stu-id="cfed7-147">Hourly metrics</span></span>
* <span data-ttu-id="cfed7-148">$MetricsHourPrimaryTransactionsBlob</span><span class="sxs-lookup"><span data-stu-id="cfed7-148">$MetricsHourPrimaryTransactionsBlob</span></span>
* <span data-ttu-id="cfed7-149">$MetricsHourPrimaryTransactionsTable</span><span class="sxs-lookup"><span data-stu-id="cfed7-149">$MetricsHourPrimaryTransactionsTable</span></span>
* <span data-ttu-id="cfed7-150">$MetricsHourPrimaryTransactionsQueue</span><span class="sxs-lookup"><span data-stu-id="cfed7-150">$MetricsHourPrimaryTransactionsQueue</span></span>

### <a name="minute-metrics"></a><span data-ttu-id="cfed7-151">Minutu metriky</span><span class="sxs-lookup"><span data-stu-id="cfed7-151">Minute metrics</span></span>
* <span data-ttu-id="cfed7-152">$MetricsMinutePrimaryTransactionsBlob</span><span class="sxs-lookup"><span data-stu-id="cfed7-152">$MetricsMinutePrimaryTransactionsBlob</span></span>
* <span data-ttu-id="cfed7-153">$MetricsMinutePrimaryTransactionsTable</span><span class="sxs-lookup"><span data-stu-id="cfed7-153">$MetricsMinutePrimaryTransactionsTable</span></span>
* <span data-ttu-id="cfed7-154">$MetricsMinutePrimaryTransactionsQueue</span><span class="sxs-lookup"><span data-stu-id="cfed7-154">$MetricsMinutePrimaryTransactionsQueue</span></span>

### <a name="capacity"></a><span data-ttu-id="cfed7-155">Kapacita</span><span class="sxs-lookup"><span data-stu-id="cfed7-155">Capacity</span></span>
* <span data-ttu-id="cfed7-156">$MetricsCapacityBlob</span><span class="sxs-lookup"><span data-stu-id="cfed7-156">$MetricsCapacityBlob</span></span>

<span data-ttu-id="cfed7-157">Můžete najít podrobnosti schémat pro tyto tabulky v [schématu tabulky metriky Analytics úložiště](https://msdn.microsoft.com/library/azure/hh343264.aspx).</span><span class="sxs-lookup"><span data-stu-id="cfed7-157">You can find full details of the schemas for these tables at [Storage Analytics Metrics Table Schema](https://msdn.microsoft.com/library/azure/hh343264.aspx).</span></span> <span data-ttu-id="cfed7-158">Ukázka řádků níže zobrazit pouze podmnožinu sloupců, které jsou k dispozici, ale ilustrovat některé důležité funkce způsob, jakým Storage Metrics uloží tyto metriky:</span><span class="sxs-lookup"><span data-stu-id="cfed7-158">The sample rows below show only a subset of the columns available, but illustrate some important features of the way Storage Metrics saves these metrics:</span></span>

| <span data-ttu-id="cfed7-159">Klíč oddílu</span><span class="sxs-lookup"><span data-stu-id="cfed7-159">PartitionKey</span></span> | <span data-ttu-id="cfed7-160">RowKey</span><span class="sxs-lookup"><span data-stu-id="cfed7-160">RowKey</span></span> | <span data-ttu-id="cfed7-161">časové razítko</span><span class="sxs-lookup"><span data-stu-id="cfed7-161">Timestamp</span></span> | <span data-ttu-id="cfed7-162">TotalRequests</span><span class="sxs-lookup"><span data-stu-id="cfed7-162">TotalRequests</span></span> | <span data-ttu-id="cfed7-163">TotalBillableRequests</span><span class="sxs-lookup"><span data-stu-id="cfed7-163">TotalBillableRequests</span></span> | <span data-ttu-id="cfed7-164">TotalIngress</span><span class="sxs-lookup"><span data-stu-id="cfed7-164">TotalIngress</span></span> | <span data-ttu-id="cfed7-165">TotalEgress</span><span class="sxs-lookup"><span data-stu-id="cfed7-165">TotalEgress</span></span> | <span data-ttu-id="cfed7-166">Dostupnost</span><span class="sxs-lookup"><span data-stu-id="cfed7-166">Availability</span></span> | <span data-ttu-id="cfed7-167">AverageE2ELatency</span><span class="sxs-lookup"><span data-stu-id="cfed7-167">AverageE2ELatency</span></span> | <span data-ttu-id="cfed7-168">AverageServerLatency</span><span class="sxs-lookup"><span data-stu-id="cfed7-168">AverageServerLatency</span></span> | <span data-ttu-id="cfed7-169">PercentSuccess</span><span class="sxs-lookup"><span data-stu-id="cfed7-169">PercentSuccess</span></span> |
| --- |:---:| ---:| --- | --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="cfed7-170">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="cfed7-170">20140522T1100</span></span> |<span data-ttu-id="cfed7-171">uživatel; Všechny</span><span class="sxs-lookup"><span data-stu-id="cfed7-171">user;All</span></span> |<span data-ttu-id="cfed7-172">2014-05-22T11:01:16.7650250Z</span><span class="sxs-lookup"><span data-stu-id="cfed7-172">2014-05-22T11:01:16.7650250Z</span></span> |<span data-ttu-id="cfed7-173">7</span><span class="sxs-lookup"><span data-stu-id="cfed7-173">7</span></span> |<span data-ttu-id="cfed7-174">7</span><span class="sxs-lookup"><span data-stu-id="cfed7-174">7</span></span> |<span data-ttu-id="cfed7-175">4003</span><span class="sxs-lookup"><span data-stu-id="cfed7-175">4003</span></span> |<span data-ttu-id="cfed7-176">46801</span><span class="sxs-lookup"><span data-stu-id="cfed7-176">46801</span></span> |<span data-ttu-id="cfed7-177">100</span><span class="sxs-lookup"><span data-stu-id="cfed7-177">100</span></span> |<span data-ttu-id="cfed7-178">104.4286</span><span class="sxs-lookup"><span data-stu-id="cfed7-178">104.4286</span></span> |<span data-ttu-id="cfed7-179">6.857143</span><span class="sxs-lookup"><span data-stu-id="cfed7-179">6.857143</span></span> |<span data-ttu-id="cfed7-180">100</span><span class="sxs-lookup"><span data-stu-id="cfed7-180">100</span></span> |
| <span data-ttu-id="cfed7-181">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="cfed7-181">20140522T1100</span></span> |<span data-ttu-id="cfed7-182">uživatel; QueryEntities</span><span class="sxs-lookup"><span data-stu-id="cfed7-182">user;QueryEntities</span></span> |<span data-ttu-id="cfed7-183">2014-05-22T11:01:16.7640250Z</span><span class="sxs-lookup"><span data-stu-id="cfed7-183">2014-05-22T11:01:16.7640250Z</span></span> |<span data-ttu-id="cfed7-184">5</span><span class="sxs-lookup"><span data-stu-id="cfed7-184">5</span></span> |<span data-ttu-id="cfed7-185">5</span><span class="sxs-lookup"><span data-stu-id="cfed7-185">5</span></span> |<span data-ttu-id="cfed7-186">2694</span><span class="sxs-lookup"><span data-stu-id="cfed7-186">2694</span></span> |<span data-ttu-id="cfed7-187">45951</span><span class="sxs-lookup"><span data-stu-id="cfed7-187">45951</span></span> |<span data-ttu-id="cfed7-188">100</span><span class="sxs-lookup"><span data-stu-id="cfed7-188">100</span></span> |<span data-ttu-id="cfed7-189">143.8</span><span class="sxs-lookup"><span data-stu-id="cfed7-189">143.8</span></span> |<span data-ttu-id="cfed7-190">7.8</span><span class="sxs-lookup"><span data-stu-id="cfed7-190">7.8</span></span> |<span data-ttu-id="cfed7-191">100</span><span class="sxs-lookup"><span data-stu-id="cfed7-191">100</span></span> |
| <span data-ttu-id="cfed7-192">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="cfed7-192">20140522T1100</span></span> |<span data-ttu-id="cfed7-193">uživatel; QueryEntity</span><span class="sxs-lookup"><span data-stu-id="cfed7-193">user;QueryEntity</span></span> |<span data-ttu-id="cfed7-194">2014-05-22T11:01:16.7650250Z</span><span class="sxs-lookup"><span data-stu-id="cfed7-194">2014-05-22T11:01:16.7650250Z</span></span> |<span data-ttu-id="cfed7-195">1</span><span class="sxs-lookup"><span data-stu-id="cfed7-195">1</span></span> |<span data-ttu-id="cfed7-196">1</span><span class="sxs-lookup"><span data-stu-id="cfed7-196">1</span></span> |<span data-ttu-id="cfed7-197">538</span><span class="sxs-lookup"><span data-stu-id="cfed7-197">538</span></span> |<span data-ttu-id="cfed7-198">633</span><span class="sxs-lookup"><span data-stu-id="cfed7-198">633</span></span> |<span data-ttu-id="cfed7-199">100</span><span class="sxs-lookup"><span data-stu-id="cfed7-199">100</span></span> |<span data-ttu-id="cfed7-200">3</span><span class="sxs-lookup"><span data-stu-id="cfed7-200">3</span></span> |<span data-ttu-id="cfed7-201">3</span><span class="sxs-lookup"><span data-stu-id="cfed7-201">3</span></span> |<span data-ttu-id="cfed7-202">100</span><span class="sxs-lookup"><span data-stu-id="cfed7-202">100</span></span> |
| <span data-ttu-id="cfed7-203">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="cfed7-203">20140522T1100</span></span> |<span data-ttu-id="cfed7-204">uživatel; UpdateEntity</span><span class="sxs-lookup"><span data-stu-id="cfed7-204">user;UpdateEntity</span></span> |<span data-ttu-id="cfed7-205">2014-05-22T11:01:16.7650250Z</span><span class="sxs-lookup"><span data-stu-id="cfed7-205">2014-05-22T11:01:16.7650250Z</span></span> |<span data-ttu-id="cfed7-206">1</span><span class="sxs-lookup"><span data-stu-id="cfed7-206">1</span></span> |<span data-ttu-id="cfed7-207">1</span><span class="sxs-lookup"><span data-stu-id="cfed7-207">1</span></span> |<span data-ttu-id="cfed7-208">771</span><span class="sxs-lookup"><span data-stu-id="cfed7-208">771</span></span> |<span data-ttu-id="cfed7-209">217</span><span class="sxs-lookup"><span data-stu-id="cfed7-209">217</span></span> |<span data-ttu-id="cfed7-210">100</span><span class="sxs-lookup"><span data-stu-id="cfed7-210">100</span></span> |<span data-ttu-id="cfed7-211">9</span><span class="sxs-lookup"><span data-stu-id="cfed7-211">9</span></span> |<span data-ttu-id="cfed7-212">6</span><span class="sxs-lookup"><span data-stu-id="cfed7-212">6</span></span> |<span data-ttu-id="cfed7-213">100</span><span class="sxs-lookup"><span data-stu-id="cfed7-213">100</span></span> |

<span data-ttu-id="cfed7-214">V těchto datech minutu metriky příklad používá klíč oddílu čas na minutu řešení.</span><span class="sxs-lookup"><span data-stu-id="cfed7-214">In this example minute metrics data, the partition key uses the time at minute resolution.</span></span> <span data-ttu-id="cfed7-215">Klíč řádku určuje typ informací, která je uložená v řádku, a to se skládá ze dvou částí informace, typ přístupu a typ požadavku:</span><span class="sxs-lookup"><span data-stu-id="cfed7-215">The row key identifies the type of information that is stored in the row and this is composed of two pieces of information, the access type, and the request type:</span></span>

* <span data-ttu-id="cfed7-216">Typ přístupu je uživatel nebo systém, uživatel odkazuje na všechny požadavky na uživatele ve službě úložiště, kde systém odkazuje na požadavky na analytika úložiště.</span><span class="sxs-lookup"><span data-stu-id="cfed7-216">The access type is either user or system, where user refers to all user requests to the storage service, and system refers to requests made by Storage Analytics.</span></span>
* <span data-ttu-id="cfed7-217">Typ požadavku je buď všechny v takovém případě ji souhrn řádku, nebo ji identifikuje konkrétní API jako třeba QueryEntity nebo UpdateEntity.</span><span class="sxs-lookup"><span data-stu-id="cfed7-217">The request type is either all in which case it is a summary line, or it identifies the specific API such as QueryEntity or UpdateEntity.</span></span>

<span data-ttu-id="cfed7-218">Výše uvedené ukázková data zobrazí všechny záznamy pro jednu minutu (počínaje 11:00 AM), tak počet požadavků QueryEntities plus počet QueryEntity požadavky plus počet UpdateEntity požadavků přidat až 7, což je celkový počet zobrazený na uživatele: všechny řádek.</span><span class="sxs-lookup"><span data-stu-id="cfed7-218">The sample data above shows all the records for a single minute (starting at 11:00AM), so the number of QueryEntities requests plus the number of QueryEntity requests plus the number of UpdateEntity requests add up to seven, which is the total shown on the user:All row.</span></span> <span data-ttu-id="cfed7-219">Podobně můžete odvodit Průměrná latence začátku do konce 104.4286 pro uživatele: všechny řádek pomocí výpočtu ((143.8 * 5) + 3 + 9) / 7.</span><span class="sxs-lookup"><span data-stu-id="cfed7-219">Similarly, you can derive the average end-to-end latency 104.4286 on the user:All row by calculating ((143.8 * 5) + 3 + 9)/7.</span></span>

## <a name="metrics-alerts"></a><span data-ttu-id="cfed7-220">Metriky výstrahy</span><span class="sxs-lookup"><span data-stu-id="cfed7-220">Metrics alerts</span></span>
<span data-ttu-id="cfed7-221">Měli byste zvážit vytvoření výstrahy v [portál Azure](https://portal.azure.com) tak metriky úložiště můžete automaticky upozorňují na důležité změny v chování nástroje vaší služby úložiště.</span><span class="sxs-lookup"><span data-stu-id="cfed7-221">You should consider setting up alerts in the [Azure portal](https://portal.azure.com) so Storage Metrics can automatically notify you of important changes in the behavior of your storage services.</span></span> <span data-ttu-id="cfed7-222">Pokud používáte nástroj Průzkumník úložišť pro stahování této metriky dat ve formátu odděleného, můžete k analýze dat aplikace Microsoft Excel.</span><span class="sxs-lookup"><span data-stu-id="cfed7-222">If you use a storage explorer tool to download this metrics data in a delimited format, you can use Microsoft Excel to analyze the data.</span></span> <span data-ttu-id="cfed7-223">V tématu [Azure Storage Client Tools](storage-explorers.md) seznam nástrojů Průzkumníka úložiště k dispozici.</span><span class="sxs-lookup"><span data-stu-id="cfed7-223">See [Azure Storage Client Tools](storage-explorers.md) for a list of available storage explorer tools.</span></span> <span data-ttu-id="cfed7-224">Výstrahy v můžete nakonfigurovat **výstrah pravidla** okně přístupná **monitorování** v okně nabídce účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="cfed7-224">You can configure alerts in the **Alert rules** blade, accessible under **Monitoring** in the Storage account menu blade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cfed7-225">Může nastat zpoždění mezi událostí úložiště a když se zaznamenává odpovídající data hodinových nebo minutu metriky.</span><span class="sxs-lookup"><span data-stu-id="cfed7-225">There may be a delay between a storage event and when the corresponding hourly or minute metrics data is recorded.</span></span> <span data-ttu-id="cfed7-226">V případě minutu metriky může několik minut data zapsat najednou.</span><span class="sxs-lookup"><span data-stu-id="cfed7-226">In the case of minute metrics, several minutes of data may be written at once.</span></span> <span data-ttu-id="cfed7-227">To může vést k transakce z dřívějších minut agregovaný do transakce pro do aktuální minuty.</span><span class="sxs-lookup"><span data-stu-id="cfed7-227">This can lead to transactions from earlier minutes being aggregated into the transaction for the current minute.</span></span> <span data-ttu-id="cfed7-228">V takovém případě výstrah služby nemusí mít všechna data dostupné metriky pro nakonfigurované výstrahy interval, což může vést k upozornění, která iniciovala neočekávaně.</span><span class="sxs-lookup"><span data-stu-id="cfed7-228">When this happens, the alert service may not have all available metrics data for the configured alert interval, which may lead to alerts firing unexpectedly.</span></span>
>

## <a name="accessing-metrics-data-programmatically"></a><span data-ttu-id="cfed7-229">Přístup k datům metriky prostřednictvím kódu programu</span><span class="sxs-lookup"><span data-stu-id="cfed7-229">Accessing metrics data programmatically</span></span>
<span data-ttu-id="cfed7-230">Následující seznam zobrazuje ukázka C# kód, který přistupuje k minutu metriky pro řadu minut a zobrazí výsledky v okně konzoly.</span><span class="sxs-lookup"><span data-stu-id="cfed7-230">The following listing shows sample C# code that accesses the minute metrics for a range of minutes and displays the results in a console Window.</span></span> <span data-ttu-id="cfed7-231">Používá knihovna pro úložiště Azure verze 4, který zahrnuje CloudAnalyticsClient třídu, která zjednodušuje přístup k tabulky metriky v úložišti.</span><span class="sxs-lookup"><span data-stu-id="cfed7-231">It uses the Azure Storage Library version 4 that includes the CloudAnalyticsClient class that simplifies accessing the metrics tables in storage.</span></span>

```csharp
private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)
{
    // Convert the dates to the format used in the PartitionKey
    var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");

    var services = Enum.GetValues(typeof(StorageService));
    foreach (StorageService service in services)
    {
        Console.WriteLine("Minute Metrics for Service {0} from {1} to {2} UTC", service, start, end);
        var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);
        var t = analyticsClient.GetMinuteMetricsTable(service);
        var opContext = new OperationContext();
        var query =
          from entity in metricsQuery
          // Note, you can't filter using the entity properties Time, AccessType, or TransactionType
          // because they are calculated fields in the MetricsEntity class.
          // The PartitionKey identifies the DataTime of the metrics.
          where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0
        select entity;

        // Filter on "user" transactions after fetching the metrics from Table Storage.
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

## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a><span data-ttu-id="cfed7-232">Jaké poplatky platit při povolení metrik úložiště?</span><span class="sxs-lookup"><span data-stu-id="cfed7-232">What charges do you incur when you enable storage metrics?</span></span>
<span data-ttu-id="cfed7-233">Zapsat požadavky na vytvoření entity tabulky metrik, které budou účtovat podle standardních sazeb platí pro všechny operace úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="cfed7-233">Write requests to create table entities for metrics are charged at the standard rates applicable to all Azure Storage operations.</span></span>

<span data-ttu-id="cfed7-234">Požadavků na čtení a odstranění klientem metriky dat jsou také fakturovatelný na standardních sazeb.</span><span class="sxs-lookup"><span data-stu-id="cfed7-234">Read and delete requests by a client to metrics data are also billable at standard rates.</span></span> <span data-ttu-id="cfed7-235">Pokud jste nakonfigurovali zásadu uchovávání dat, vám není účtován když odstraní stará data metrik Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="cfed7-235">If you have configured a data retention policy, you are not charged when Azure Storage deletes old metrics data.</span></span> <span data-ttu-id="cfed7-236">Pokud odstraníte analytická data, váš účet účtovat pro operace odstranění.</span><span class="sxs-lookup"><span data-stu-id="cfed7-236">However, if you delete analytics data, your account is charged for the delete operations.</span></span>

<span data-ttu-id="cfed7-237">Kapacita používané metriky tabulky je také fakturovatelný: k zjištění přibližné hodnoty množství kapacity, na které se používá k ukládání dat metriky můžete použít následující:</span><span class="sxs-lookup"><span data-stu-id="cfed7-237">The capacity used by the metrics tables is also billable: you can use the following to estimate the amount of capacity used for storing metrics data:</span></span>

* <span data-ttu-id="cfed7-238">Pokud každou hodinu služba využívá každé rozhraní API v každé službě, pak přibližně 148KB dat je uložený každou hodinu do tabulky transakcí metriky Pokud jste povolili službu a úroveň API souhrnu.</span><span class="sxs-lookup"><span data-stu-id="cfed7-238">If each hour a service utilizes every API in every service, then approximately 148KB of data is stored every hour in the metrics transaction tables if you have enabled both service and API level summary.</span></span>
* <span data-ttu-id="cfed7-239">Pokud každou hodinu služba využívá každé rozhraní API v každé službě, pak přibližně 12KB dat je uložený každou hodinu do tabulky transakcí metriky Pokud jste povolili právě služby úrovni souhrnu.</span><span class="sxs-lookup"><span data-stu-id="cfed7-239">If each hour a service utilizes every API in every service, then approximately 12KB of data is stored every hour in the metrics transaction tables if you have enabled just service level summary.</span></span>
* <span data-ttu-id="cfed7-240">Tabulka kapacity pro objekty BLOB obsahuje dva řádky přidat každý den (za předpokladu, že uživatel požádal v protokolů): to znamená, že každý den velikost této tabulky zvyšuje úroveň až přibližně 300 bajtů.</span><span class="sxs-lookup"><span data-stu-id="cfed7-240">The capacity table for blobs has two rows added each day (provided user has opted in for logs): this implies that every day the size of this table increases by up to approximately 300 bytes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cfed7-241">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cfed7-241">Next steps</span></span>
[<span data-ttu-id="cfed7-242">Povolení protokolování a přístup k datům protokolu úložiště</span><span class="sxs-lookup"><span data-stu-id="cfed7-242">Enabling Storage Logging and Accessing Log Data</span></span>](/rest/api/storageservices/Enabling-Storage-Logging-and-Accessing-Log-Data)
