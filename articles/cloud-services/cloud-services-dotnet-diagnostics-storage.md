---
title: "Úložiště a zobrazení diagnostických dat v úložišti Azure | Microsoft Docs"
description: "Získat Azure diagnostická data do úložiště Azure a jeho zobrazení"
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: tysonn
ms.assetid: 18e0780d-43e7-41e4-b8e9-f1fb9a36eb03
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2016
ms.author: robb
ms.openlocfilehash: 374cc179e13c00e439415e3df16e0c6d5ccba5e3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="store-and-view-diagnostic-data-in-azure-storage"></a><span data-ttu-id="9205a-103">Úložiště a zobrazení diagnostických dat ve službě Azure Storage</span><span class="sxs-lookup"><span data-stu-id="9205a-103">Store and view diagnostic data in Azure Storage</span></span>
<span data-ttu-id="9205a-104">Diagnostických dat není uložena trvale, pokud ho přenést na emulátor úložiště Microsoft Azure nebo do úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="9205a-104">Diagnostic data is not permanently stored unless you transfer it to the Microsoft Azure storage emulator or to Azure storage.</span></span> <span data-ttu-id="9205a-105">Jednou v úložišti, prohlížení s jedním z několika dostupných nástrojů.</span><span class="sxs-lookup"><span data-stu-id="9205a-105">Once in storage, it can be viewed with one of several available tools.</span></span>

## <a name="specify-a-storage-account"></a><span data-ttu-id="9205a-106">Zadejte účet úložiště</span><span class="sxs-lookup"><span data-stu-id="9205a-106">Specify a storage account</span></span>
<span data-ttu-id="9205a-107">Zadáte účet úložiště, který chcete použít v souboru ServiceConfiguration.cscfg souboru.</span><span class="sxs-lookup"><span data-stu-id="9205a-107">You specify the storage account that you want to use in the ServiceConfiguration.cscfg file.</span></span> <span data-ttu-id="9205a-108">Informace o účtu je definován jako připojovací řetězec v nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9205a-108">The account information is defined as a connection string in a configuration setting.</span></span> <span data-ttu-id="9205a-109">Následující příklad ukazuje výchozí připojovací řetězec vytvořený pro nový projekt cloudové služby v sadě Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="9205a-109">The following example shows the default connection string created for a new Cloud Service project in  Visual Studio:</span></span>

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
    </ConfigurationSettings>
```

<span data-ttu-id="9205a-110">Tento připojovací řetězec k poskytování informací o účtu pro účet úložiště Azure, můžete změnit.</span><span class="sxs-lookup"><span data-stu-id="9205a-110">You can change this connection string to provide account information for an Azure storage account.</span></span>

<span data-ttu-id="9205a-111">V závislosti na typu diagnostických dat, jež jsou shromažďována používá Azure Diagnostics služby objektů Blob nebo služby Table.</span><span class="sxs-lookup"><span data-stu-id="9205a-111">Depending on the type of diagnostic data that is being collected, Azure Diagnostics uses either the Blob service or the Table service.</span></span> <span data-ttu-id="9205a-112">V následující tabulce jsou zdroje dat, které jsou nastavené jako trvalé a jejich formátu.</span><span class="sxs-lookup"><span data-stu-id="9205a-112">The following table shows the data sources that are persisted and their format.</span></span>

| <span data-ttu-id="9205a-113">Zdroj dat</span><span class="sxs-lookup"><span data-stu-id="9205a-113">Data source</span></span> | <span data-ttu-id="9205a-114">Formát úložiště</span><span class="sxs-lookup"><span data-stu-id="9205a-114">Storage format</span></span> |
| --- | --- |
| <span data-ttu-id="9205a-115">Azure protokoly</span><span class="sxs-lookup"><span data-stu-id="9205a-115">Azure logs</span></span> |<span data-ttu-id="9205a-116">Table</span><span class="sxs-lookup"><span data-stu-id="9205a-116">Table</span></span> |
| <span data-ttu-id="9205a-117">Protokoly služby IIS 7.0</span><span class="sxs-lookup"><span data-stu-id="9205a-117">IIS 7.0 logs</span></span> |<span data-ttu-id="9205a-118">Objekt blob</span><span class="sxs-lookup"><span data-stu-id="9205a-118">Blob</span></span> |
| <span data-ttu-id="9205a-119">Protokoly infrastruktury Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="9205a-119">Azure Diagnostics infrastructure logs</span></span> |<span data-ttu-id="9205a-120">Table</span><span class="sxs-lookup"><span data-stu-id="9205a-120">Table</span></span> |
| <span data-ttu-id="9205a-121">Protokoly trasování požadavku se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="9205a-121">Failed Request Trace logs</span></span> |<span data-ttu-id="9205a-122">Objekt blob</span><span class="sxs-lookup"><span data-stu-id="9205a-122">Blob</span></span> |
| <span data-ttu-id="9205a-123">Protokoly událostí systému Windows</span><span class="sxs-lookup"><span data-stu-id="9205a-123">Windows Event logs</span></span> |<span data-ttu-id="9205a-124">Table</span><span class="sxs-lookup"><span data-stu-id="9205a-124">Table</span></span> |
| <span data-ttu-id="9205a-125">Čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="9205a-125">Performance counters</span></span> |<span data-ttu-id="9205a-126">Table</span><span class="sxs-lookup"><span data-stu-id="9205a-126">Table</span></span> |
| <span data-ttu-id="9205a-127">Výpisy stavu systému</span><span class="sxs-lookup"><span data-stu-id="9205a-127">Crash dumps</span></span> |<span data-ttu-id="9205a-128">Objekt blob</span><span class="sxs-lookup"><span data-stu-id="9205a-128">Blob</span></span> |
| <span data-ttu-id="9205a-129">Vlastní chybové protokoly</span><span class="sxs-lookup"><span data-stu-id="9205a-129">Custom error logs</span></span> |<span data-ttu-id="9205a-130">Objekt blob</span><span class="sxs-lookup"><span data-stu-id="9205a-130">Blob</span></span> |

## <a name="transfer-diagnostic-data"></a><span data-ttu-id="9205a-131">Přenos dat diagnostiky</span><span class="sxs-lookup"><span data-stu-id="9205a-131">Transfer diagnostic data</span></span>
<span data-ttu-id="9205a-132">Pro sadu SDK, 2.5 nebo novější může dojít, žádosti o převedení diagnostických dat pomocí konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="9205a-132">For SDK 2.5 and later, the request to transfer diagnostic data can occur through the configuration file.</span></span> <span data-ttu-id="9205a-133">Diagnostická data můžou přenášet v naplánovaných intervalech jako zadaný v konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="9205a-133">You can transfer diagnostic data at scheduled intervals as specified in the configuration.</span></span>

<span data-ttu-id="9205a-134">SDK 2.4 a předchozí může požádat o přenos diagnostických dat prostřednictvím konfiguračním souboru jako prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="9205a-134">For SDK 2.4 and previous you can request to transfer the diagnostic data through the configuration file as well as programmatically.</span></span> <span data-ttu-id="9205a-135">Programový přístup také umožňuje provádět přenosy na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="9205a-135">The programmatic approach also allows you to do on-demand transfers.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9205a-136">Při přenosu dat diagnostiky do účtu úložiště Azure, se vynakládá pro prostředky úložiště, které používá diagnostická data.</span><span class="sxs-lookup"><span data-stu-id="9205a-136">When you transfer diagnostic data to an Azure storage account, you incur costs for the storage resources that your diagnostic data uses.</span></span>
> 
> 

## <a name="store-diagnostic-data"></a><span data-ttu-id="9205a-137">Ukládání diagnostických dat</span><span class="sxs-lookup"><span data-stu-id="9205a-137">Store diagnostic data</span></span>
<span data-ttu-id="9205a-138">Data protokolu se ukládají v úložišti objektů Blob nebo tabulky s těmito názvy:</span><span class="sxs-lookup"><span data-stu-id="9205a-138">Log data is stored in either Blob or Table storage with the following names:</span></span>

<span data-ttu-id="9205a-139">**Tabulky**</span><span class="sxs-lookup"><span data-stu-id="9205a-139">**Tables**</span></span>

* <span data-ttu-id="9205a-140">**WadLogsTable** – protokoly, které jsou napsané v kódu pomocí naslouchací proces trasování.</span><span class="sxs-lookup"><span data-stu-id="9205a-140">**WadLogsTable** - Logs written in code using the trace listener.</span></span>
* <span data-ttu-id="9205a-141">**WADDiagnosticInfrastructureLogsTable** -diagnostiky změny monitorování a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="9205a-141">**WADDiagnosticInfrastructureLogsTable** - Diagnostic monitor and configuration changes.</span></span>
* <span data-ttu-id="9205a-142">**WADDirectoriesTable** – adresáře, které monitoruje monitorování diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="9205a-142">**WADDirectoriesTable** – Directories that the diagnostic monitor is monitoring.</span></span>  <span data-ttu-id="9205a-143">To zahrnuje protokoly služby IIS, služba IIS nezdařilo požadavek protokoly a vlastní adresářů.</span><span class="sxs-lookup"><span data-stu-id="9205a-143">This includes IIS logs, IIS failed request logs, and custom directories.</span></span>  <span data-ttu-id="9205a-144">Umístění souboru protokolu objektu blob je zadána v poli kontejneru a název objektu blob je v poli RelativePath.</span><span class="sxs-lookup"><span data-stu-id="9205a-144">The location of the blob log file is specified in the Container field and the name of the blob is in the RelativePath field.</span></span>  <span data-ttu-id="9205a-145">Pole Absolutní_cesta označuje umístění a název souboru, který existoval na virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="9205a-145">The AbsolutePath field indicates the location and name of the file as it existed on the Azure virtual machine.</span></span>
* <span data-ttu-id="9205a-146">**WADPerformanceCountersTable** – čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="9205a-146">**WADPerformanceCountersTable** – Performance counters.</span></span>
* <span data-ttu-id="9205a-147">**WADWindowsEventLogsTable** – protokoly událostí systému Windows.</span><span class="sxs-lookup"><span data-stu-id="9205a-147">**WADWindowsEventLogsTable** – Windows Event logs.</span></span>

<span data-ttu-id="9205a-148">**Objekty blob**</span><span class="sxs-lookup"><span data-stu-id="9205a-148">**Blobs**</span></span>

* <span data-ttu-id="9205a-149">**kontejneru ovládacího prvku wad** – (jenom pro SDK 2.4 a předchozí) obsahuje konfigurační soubory XML, které se řídí Azure diagnostics.</span><span class="sxs-lookup"><span data-stu-id="9205a-149">**wad-control-container** – (Only for SDK 2.4 and previous) Contains the XML configuration files that controls the Azure diagnostics .</span></span>
* <span data-ttu-id="9205a-150">**wad. Služba iis failedreqlogfiles** – obsahuje informace z protokolů žádostí služby IIS se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="9205a-150">**wad-iis-failedreqlogfiles** – Contains information from IIS Failed Request logs.</span></span>
* <span data-ttu-id="9205a-151">**wad. Služba iis logfiles** – obsahuje informace o protokoly služby IIS.</span><span class="sxs-lookup"><span data-stu-id="9205a-151">**wad-iis-logfiles** – Contains information about IIS logs.</span></span>
* <span data-ttu-id="9205a-152">**"vlastní"** – vlastní kontejner podle konfigurace adresáře, které jsou monitorovány pomocí monitorování diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="9205a-152">**"custom"** – A custom container based on configuring directories that are monitored by the diagnostic monitor.</span></span>  <span data-ttu-id="9205a-153">Název kontejneru objektu blob budou zadány v WADDirectoriesTable.</span><span class="sxs-lookup"><span data-stu-id="9205a-153">The name of this blob container will be specified in WADDirectoriesTable.</span></span>

## <a name="tools-to-view-diagnostic-data"></a><span data-ttu-id="9205a-154">Nástroje pro zobrazení diagnostických dat</span><span class="sxs-lookup"><span data-stu-id="9205a-154">Tools to view diagnostic data</span></span>
<span data-ttu-id="9205a-155">Několik nástrojů je možné zobrazit data, jakmile se přenese do úložiště.</span><span class="sxs-lookup"><span data-stu-id="9205a-155">Several tools are available to view the data after it is transferred to storage.</span></span> <span data-ttu-id="9205a-156">Například:</span><span class="sxs-lookup"><span data-stu-id="9205a-156">For example:</span></span>

* <span data-ttu-id="9205a-157">Průzkumník serveru v sadě Visual Studio – Pokud jste nainstalovali nástroje Azure pro sadu Microsoft Visual Studio, můžete v uzlu úložiště Azure v Průzkumníku serveru k zobrazení jen pro čtení objektů blob a data tabulky z účtům Azure storage.</span><span class="sxs-lookup"><span data-stu-id="9205a-157">Server Explorer in Visual Studio - If you have installed the Azure Tools for Microsoft Visual Studio, you can use the Azure Storage node in Server Explorer to view read-only blob and table data from your Azure storage accounts.</span></span> <span data-ttu-id="9205a-158">Můžete zobrazit data z účtu emulátor místního úložiště a taky z účtů úložiště jste vytvořili pro Azure.</span><span class="sxs-lookup"><span data-stu-id="9205a-158">You can display data from your local storage emulator account and also from storage accounts you have created for Azure.</span></span> <span data-ttu-id="9205a-159">Další informace najdete v tématu [procházení a Správa prostředků úložiště pomocí Průzkumníka serveru](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md).</span><span class="sxs-lookup"><span data-stu-id="9205a-159">For more information, see [Browsing and Managing Storage Resources with Server Explorer](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md).</span></span>
* <span data-ttu-id="9205a-160">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatná aplikace, která umožňuje snadno pracovat s daty Azure Storage v systému Windows, na OSX a Linux.</span><span class="sxs-lookup"><span data-stu-id="9205a-160">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a standalone app that enables you to easily work with Azure Storage data on Windows, OSX, and Linux.</span></span>
* <span data-ttu-id="9205a-161">[Azure Management Studio](http://www.cerebrata.com/products/azure-management-studio/introduction) zahrnuje Azure Diagnostics správce, který umožňuje zobrazit, stahování a správě diagnostiky data shromažďovaná společností aplikace běžící v Azure.</span><span class="sxs-lookup"><span data-stu-id="9205a-161">[Azure Management Studio](http://www.cerebrata.com/products/azure-management-studio/introduction) includes Azure Diagnostics Manager which allows you to view, download and manage the diagnostics data collected by the applications running on Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9205a-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9205a-162">Next Steps</span></span>
[<span data-ttu-id="9205a-163">Trasování toku v aplikaci s Azure Diagnostics cloudové služby</span><span class="sxs-lookup"><span data-stu-id="9205a-163">Trace the flow in a Cloud Services application with Azure Diagnostics</span></span>](cloud-services-dotnet-diagnostics-trace-flow.md)

