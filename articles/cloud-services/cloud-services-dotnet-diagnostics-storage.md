---
title: "aaaStore a zobrazení diagnostických dat ve službě Azure Storage | Microsoft Docs"
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
ms.openlocfilehash: dd47a2ef6d6488c80c102c72b2ebf6ca6d2e473f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="store-and-view-diagnostic-data-in-azure-storage"></a><span data-ttu-id="3e8cd-103">Úložiště a zobrazení diagnostických dat ve službě Azure Storage</span><span class="sxs-lookup"><span data-stu-id="3e8cd-103">Store and view diagnostic data in Azure Storage</span></span>
<span data-ttu-id="3e8cd-104">Diagnostických dat není uložena trvale, pokud přenos toohello emulátor úložiště Microsoft Azure nebo tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-104">Diagnostic data is not permanently stored unless you transfer it toohello Microsoft Azure storage emulator or tooAzure storage.</span></span> <span data-ttu-id="3e8cd-105">Jednou v úložišti, prohlížení s jedním z několika dostupných nástrojů.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-105">Once in storage, it can be viewed with one of several available tools.</span></span>

## <a name="specify-a-storage-account"></a><span data-ttu-id="3e8cd-106">Zadejte účet úložiště</span><span class="sxs-lookup"><span data-stu-id="3e8cd-106">Specify a storage account</span></span>
<span data-ttu-id="3e8cd-107">Zadáte účet hello úložiště, které chcete toouse v souboru ServiceConfiguration.cscfg souboru hello.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-107">You specify hello storage account that you want toouse in hello ServiceConfiguration.cscfg file.</span></span> <span data-ttu-id="3e8cd-108">informace o účtu Hello je definován jako připojovací řetězec v nastavení konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-108">hello account information is defined as a connection string in a configuration setting.</span></span> <span data-ttu-id="3e8cd-109">Hello následující příklad ukazuje hello výchozí připojovací řetězec pro nový projekt cloudové služby v sadě Visual Studio vytvořit:</span><span class="sxs-lookup"><span data-stu-id="3e8cd-109">hello following example shows hello default connection string created for a new Cloud Service project in  Visual Studio:</span></span>

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
    </ConfigurationSettings>
```

<span data-ttu-id="3e8cd-110">Tento připojovací řetězec tooprovide informace o účtu pro účet úložiště Azure můžete změnit.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-110">You can change this connection string tooprovide account information for an Azure storage account.</span></span>

<span data-ttu-id="3e8cd-111">V závislosti na typu hello diagnostických dat, jež jsou shromažďována používá Azure Diagnostics hello služby objektů Blob nebo služby Table hello.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-111">Depending on hello type of diagnostic data that is being collected, Azure Diagnostics uses either hello Blob service or hello Table service.</span></span> <span data-ttu-id="3e8cd-112">Hello následující tabulka uvádí hello datových zdrojů, které jsou nastavené jako trvalé a jejich formátu.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-112">hello following table shows hello data sources that are persisted and their format.</span></span>

| <span data-ttu-id="3e8cd-113">Zdroj dat</span><span class="sxs-lookup"><span data-stu-id="3e8cd-113">Data source</span></span> | <span data-ttu-id="3e8cd-114">Formát úložiště</span><span class="sxs-lookup"><span data-stu-id="3e8cd-114">Storage format</span></span> |
| --- | --- |
| <span data-ttu-id="3e8cd-115">Azure protokoly</span><span class="sxs-lookup"><span data-stu-id="3e8cd-115">Azure logs</span></span> |<span data-ttu-id="3e8cd-116">Table</span><span class="sxs-lookup"><span data-stu-id="3e8cd-116">Table</span></span> |
| <span data-ttu-id="3e8cd-117">Protokoly služby IIS 7.0</span><span class="sxs-lookup"><span data-stu-id="3e8cd-117">IIS 7.0 logs</span></span> |<span data-ttu-id="3e8cd-118">Objekt blob</span><span class="sxs-lookup"><span data-stu-id="3e8cd-118">Blob</span></span> |
| <span data-ttu-id="3e8cd-119">Protokoly infrastruktury Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="3e8cd-119">Azure Diagnostics infrastructure logs</span></span> |<span data-ttu-id="3e8cd-120">Table</span><span class="sxs-lookup"><span data-stu-id="3e8cd-120">Table</span></span> |
| <span data-ttu-id="3e8cd-121">Protokoly trasování požadavku se nezdařilo</span><span class="sxs-lookup"><span data-stu-id="3e8cd-121">Failed Request Trace logs</span></span> |<span data-ttu-id="3e8cd-122">Objekt blob</span><span class="sxs-lookup"><span data-stu-id="3e8cd-122">Blob</span></span> |
| <span data-ttu-id="3e8cd-123">Protokoly událostí systému Windows</span><span class="sxs-lookup"><span data-stu-id="3e8cd-123">Windows Event logs</span></span> |<span data-ttu-id="3e8cd-124">Table</span><span class="sxs-lookup"><span data-stu-id="3e8cd-124">Table</span></span> |
| <span data-ttu-id="3e8cd-125">Čítače výkonu</span><span class="sxs-lookup"><span data-stu-id="3e8cd-125">Performance counters</span></span> |<span data-ttu-id="3e8cd-126">Table</span><span class="sxs-lookup"><span data-stu-id="3e8cd-126">Table</span></span> |
| <span data-ttu-id="3e8cd-127">Výpisy stavu systému</span><span class="sxs-lookup"><span data-stu-id="3e8cd-127">Crash dumps</span></span> |<span data-ttu-id="3e8cd-128">Objekt blob</span><span class="sxs-lookup"><span data-stu-id="3e8cd-128">Blob</span></span> |
| <span data-ttu-id="3e8cd-129">Vlastní chybové protokoly</span><span class="sxs-lookup"><span data-stu-id="3e8cd-129">Custom error logs</span></span> |<span data-ttu-id="3e8cd-130">Objekt blob</span><span class="sxs-lookup"><span data-stu-id="3e8cd-130">Blob</span></span> |

## <a name="transfer-diagnostic-data"></a><span data-ttu-id="3e8cd-131">Přenos dat diagnostiky</span><span class="sxs-lookup"><span data-stu-id="3e8cd-131">Transfer diagnostic data</span></span>
<span data-ttu-id="3e8cd-132">Pro sadu SDK, 2.5 nebo novější může dojít, hello požadavek tootransfer diagnostických dat prostřednictvím hello konfigurační soubor.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-132">For SDK 2.5 and later, hello request tootransfer diagnostic data can occur through hello configuration file.</span></span> <span data-ttu-id="3e8cd-133">Diagnostická data můžou přenášet v naplánovaných intervalech jako zadaný v konfiguraci hello.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-133">You can transfer diagnostic data at scheduled intervals as specified in hello configuration.</span></span>

<span data-ttu-id="3e8cd-134">SDK 2.4 a předchozí můžete požádat tootransfer hello diagnostických dat prostřednictvím hello konfigurační soubor také jako prostřednictvím kódu programu.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-134">For SDK 2.4 and previous you can request tootransfer hello diagnostic data through hello configuration file as well as programmatically.</span></span> <span data-ttu-id="3e8cd-135">Hello programový přístup také umožňuje přenosy toodo na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-135">hello programmatic approach also allows you toodo on-demand transfers.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3e8cd-136">Při přenosu dat diagnostiky tooan účtu úložiště Azure se vynakládá pro hello prostředky úložiště, které používá diagnostická data.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-136">When you transfer diagnostic data tooan Azure storage account, you incur costs for hello storage resources that your diagnostic data uses.</span></span>
> 
> 

## <a name="store-diagnostic-data"></a><span data-ttu-id="3e8cd-137">Ukládání diagnostických dat</span><span class="sxs-lookup"><span data-stu-id="3e8cd-137">Store diagnostic data</span></span>
<span data-ttu-id="3e8cd-138">Data protokolu se ukládají v úložišti objektů Blob nebo Table s hello následující názvy:</span><span class="sxs-lookup"><span data-stu-id="3e8cd-138">Log data is stored in either Blob or Table storage with hello following names:</span></span>

<span data-ttu-id="3e8cd-139">**Tabulky**</span><span class="sxs-lookup"><span data-stu-id="3e8cd-139">**Tables**</span></span>

* <span data-ttu-id="3e8cd-140">**WadLogsTable** – protokoly, které jsou napsané v kódu pomocí hello naslouchací proces trasování.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-140">**WadLogsTable** - Logs written in code using hello trace listener.</span></span>
* <span data-ttu-id="3e8cd-141">**WADDiagnosticInfrastructureLogsTable** -diagnostiky změny monitorování a konfigurace.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-141">**WADDiagnosticInfrastructureLogsTable** - Diagnostic monitor and configuration changes.</span></span>
* <span data-ttu-id="3e8cd-142">**WADDirectoriesTable** – adresářů tohoto monitorování diagnostiky hello je monitorování.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-142">**WADDirectoriesTable** – Directories that hello diagnostic monitor is monitoring.</span></span>  <span data-ttu-id="3e8cd-143">To zahrnuje protokoly služby IIS, služba IIS nezdařilo požadavek protokoly a vlastní adresářů.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-143">This includes IIS logs, IIS failed request logs, and custom directories.</span></span>  <span data-ttu-id="3e8cd-144">Hello umístění souboru protokolu hello objektu blob je zadána v poli hello kontejneru a hello název objektu hello blob je v poli RelativePath hello.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-144">hello location of hello blob log file is specified in hello Container field and hello name of hello blob is in hello RelativePath field.</span></span>  <span data-ttu-id="3e8cd-145">Hello Absolutní_cesta pole určuje hello umístění a název souboru hello tak, jak byly na hello virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-145">hello AbsolutePath field indicates hello location and name of hello file as it existed on hello Azure virtual machine.</span></span>
* <span data-ttu-id="3e8cd-146">**WADPerformanceCountersTable** – čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-146">**WADPerformanceCountersTable** – Performance counters.</span></span>
* <span data-ttu-id="3e8cd-147">**WADWindowsEventLogsTable** – protokoly událostí systému Windows.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-147">**WADWindowsEventLogsTable** – Windows Event logs.</span></span>

<span data-ttu-id="3e8cd-148">**Objekty blob**</span><span class="sxs-lookup"><span data-stu-id="3e8cd-148">**Blobs**</span></span>

* <span data-ttu-id="3e8cd-149">**kontejneru ovládacího prvku wad** – (jenom pro SDK 2.4 a předchozí) obsahuje hello XML konfigurační soubory, které se řídí hello Azure diagnostics.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-149">**wad-control-container** – (Only for SDK 2.4 and previous) Contains hello XML configuration files that controls hello Azure diagnostics .</span></span>
* <span data-ttu-id="3e8cd-150">**wad. Služba iis failedreqlogfiles** – obsahuje informace z protokolů žádostí služby IIS se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-150">**wad-iis-failedreqlogfiles** – Contains information from IIS Failed Request logs.</span></span>
* <span data-ttu-id="3e8cd-151">**wad. Služba iis logfiles** – obsahuje informace o protokoly služby IIS.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-151">**wad-iis-logfiles** – Contains information about IIS logs.</span></span>
* <span data-ttu-id="3e8cd-152">**"vlastní"** – vlastní kontejner podle konfigurace adresáře, které jsou monitorovány pomocí monitorování diagnostiky hello.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-152">**"custom"** – A custom container based on configuring directories that are monitored by hello diagnostic monitor.</span></span>  <span data-ttu-id="3e8cd-153">Hello název kontejneru objektu blob budou zadány v WADDirectoriesTable.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-153">hello name of this blob container will be specified in WADDirectoriesTable.</span></span>

## <a name="tools-tooview-diagnostic-data"></a><span data-ttu-id="3e8cd-154">Nástroje pro tooview diagnostických dat</span><span class="sxs-lookup"><span data-stu-id="3e8cd-154">Tools tooview diagnostic data</span></span>
<span data-ttu-id="3e8cd-155">Několik nástrojů jsou k dispozici tooview hello data po přenášená toostorage.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-155">Several tools are available tooview hello data after it is transferred toostorage.</span></span> <span data-ttu-id="3e8cd-156">Například:</span><span class="sxs-lookup"><span data-stu-id="3e8cd-156">For example:</span></span>

* <span data-ttu-id="3e8cd-157">Průzkumník serveru v sadě Visual Studio – Pokud jste nainstalovali nástroje hello Azure pro sadu Microsoft Visual Studio, můžete hello uzlu úložiště Azure v Průzkumníku serveru tooview jen pro čtení objektů blob a tabulku dat z účtům Azure storage.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-157">Server Explorer in Visual Studio - If you have installed hello Azure Tools for Microsoft Visual Studio, you can use hello Azure Storage node in Server Explorer tooview read-only blob and table data from your Azure storage accounts.</span></span> <span data-ttu-id="3e8cd-158">Můžete zobrazit data z účtu emulátor místního úložiště a taky z účtů úložiště jste vytvořili pro Azure.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-158">You can display data from your local storage emulator account and also from storage accounts you have created for Azure.</span></span> <span data-ttu-id="3e8cd-159">Další informace najdete v tématu [procházení a Správa prostředků úložiště pomocí Průzkumníka serveru](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md).</span><span class="sxs-lookup"><span data-stu-id="3e8cd-159">For more information, see [Browsing and Managing Storage Resources with Server Explorer](../vs-azure-tools-storage-resources-server-explorer-browse-manage.md).</span></span>
* <span data-ttu-id="3e8cd-160">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) je samostatná aplikace, která vám umožní tooeasily práci s daty Azure Storage v systému Windows, na OSX a Linux.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-160">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a standalone app that enables you tooeasily work with Azure Storage data on Windows, OSX, and Linux.</span></span>
* <span data-ttu-id="3e8cd-161">[Azure Management Studio](http://www.cerebrata.com/products/azure-management-studio/introduction) zahrnuje Azure Diagnostics Manager, která vám umožní tooview, stahování a správě hello diagnostická data shromažďovaná společností hello aplikace běžící v Azure.</span><span class="sxs-lookup"><span data-stu-id="3e8cd-161">[Azure Management Studio](http://www.cerebrata.com/products/azure-management-studio/introduction) includes Azure Diagnostics Manager which allows you tooview, download and manage hello diagnostics data collected by hello applications running on Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3e8cd-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3e8cd-162">Next Steps</span></span>
[<span data-ttu-id="3e8cd-163">Tok hello trasování v aplikaci s Azure Diagnostics cloudové služby</span><span class="sxs-lookup"><span data-stu-id="3e8cd-163">Trace hello flow in a Cloud Services application with Azure Diagnostics</span></span>](cloud-services-dotnet-diagnostics-trace-flow.md)

