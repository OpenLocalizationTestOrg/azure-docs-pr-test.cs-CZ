---
title: "úložiště objektů blob aaaUse pro službu IIS a tabulka úložiště pro události v Azure Log Analytics | Microsoft Docs"
description: "Analýzy protokolů může číst hello protokoly služby Azure, které zápis tootable úložiště diagnostiky nebo protokoly služby IIS zapisovat tooblob úložiště."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: bf444752-ecc1-4306-9489-c29cb37d6045
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ff3de04dc8cb6729c1443372ec31a0e8dc47f273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-blob-storage-for-iis-and-azure-table-storage-for-events-with-log-analytics"></a><span data-ttu-id="3cca0-103">Používat úložiště objektů blob v Azure pro službu IIS a Azure úložiště table pro události se analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="3cca0-103">Use Azure blob storage for IIS and Azure table storage for events with Log Analytics</span></span>

<span data-ttu-id="3cca0-104">Analýzy protokolů můžete číst hello protokoly pro následující služby, které zápisu diagnostiky tootable úložiště nebo IIS protokoly úložiště napsané tooblob hello:</span><span class="sxs-lookup"><span data-stu-id="3cca0-104">Log Analytics can read hello logs for hello following services that write diagnostics tootable storage or IIS logs written tooblob storage:</span></span>

* <span data-ttu-id="3cca0-105">Service Fabric clusterů (Preview)</span><span class="sxs-lookup"><span data-stu-id="3cca0-105">Service Fabric clusters (Preview)</span></span>
* <span data-ttu-id="3cca0-106">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="3cca0-106">Virtual Machines</span></span>
* <span data-ttu-id="3cca0-107">Web/role pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="3cca0-107">Web/Worker Roles</span></span>

<span data-ttu-id="3cca0-108">Před analýzy protokolů můžete shromažďovat data pro tyto prostředky, musí být povolena Azure diagnostics.</span><span class="sxs-lookup"><span data-stu-id="3cca0-108">Before Log Analytics can collect data for these resources, Azure diagnostics must be enabled.</span></span>

<span data-ttu-id="3cca0-109">Jakmile diagnostiky jsou povolené, můžete použít hello portál Azure nebo PowerShell konfigurace analýzy protokolů toocollect hello protokoly.</span><span class="sxs-lookup"><span data-stu-id="3cca0-109">Once diagnostics are enabled, you can use hello Azure portal or PowerShell configure Log Analytics toocollect hello logs.</span></span>

<span data-ttu-id="3cca0-110">Azure Diagnostics je rozšířením Azure, která vám umožní toocollect diagnostických dat z role pracovního procesu, webovou roli nebo virtuální počítač spuštěný v Azure.</span><span class="sxs-lookup"><span data-stu-id="3cca0-110">Azure Diagnostics is an Azure extension that enables you toocollect diagnostic data from a worker role, web role, or virtual machine running in Azure.</span></span> <span data-ttu-id="3cca0-111">Hello data je uložený v účtu úložiště Azure a můžete pak shromáždit pomocí analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="3cca0-111">hello data is stored in an Azure storage account and can then be collected by Log Analytics.</span></span>

<span data-ttu-id="3cca0-112">Pro analýzy protokolů toocollect musí být tyto protokoly Azure Diagnostics hello protokoly v hello následující umístění:</span><span class="sxs-lookup"><span data-stu-id="3cca0-112">For Log Analytics toocollect these Azure Diagnostics logs, hello logs must be in hello following locations:</span></span>

| <span data-ttu-id="3cca0-113">Typ protokolu</span><span class="sxs-lookup"><span data-stu-id="3cca0-113">Log Type</span></span> | <span data-ttu-id="3cca0-114">Typ prostředku</span><span class="sxs-lookup"><span data-stu-id="3cca0-114">Resource Type</span></span> | <span data-ttu-id="3cca0-115">Umístění</span><span class="sxs-lookup"><span data-stu-id="3cca0-115">Location</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3cca0-116">Protokoly IIS</span><span class="sxs-lookup"><span data-stu-id="3cca0-116">IIS logs</span></span> |<span data-ttu-id="3cca0-117">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="3cca0-117">Virtual Machines</span></span> <br> <span data-ttu-id="3cca0-118">Webové role</span><span class="sxs-lookup"><span data-stu-id="3cca0-118">Web roles</span></span> <br> <span data-ttu-id="3cca0-119">Role pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="3cca0-119">Worker roles</span></span> |<span data-ttu-id="3cca0-120">wad logfiles služby iis (úložiště objektů Blob)</span><span class="sxs-lookup"><span data-stu-id="3cca0-120">wad-iis-logfiles (Blob Storage)</span></span> |
| <span data-ttu-id="3cca0-121">Syslog</span><span class="sxs-lookup"><span data-stu-id="3cca0-121">Syslog</span></span> |<span data-ttu-id="3cca0-122">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="3cca0-122">Virtual Machines</span></span> |<span data-ttu-id="3cca0-123">LinuxsyslogVer2v0 (tabulky úložiště)</span><span class="sxs-lookup"><span data-stu-id="3cca0-123">LinuxsyslogVer2v0 (Table Storage)</span></span> |
| <span data-ttu-id="3cca0-124">Provozní události služby prostředků infrastruktury</span><span class="sxs-lookup"><span data-stu-id="3cca0-124">Service Fabric Operational Events</span></span> |<span data-ttu-id="3cca0-125">Uzly Service Fabric</span><span class="sxs-lookup"><span data-stu-id="3cca0-125">Service Fabric nodes</span></span> |<span data-ttu-id="3cca0-126">WADServiceFabricSystemEventTable</span><span class="sxs-lookup"><span data-stu-id="3cca0-126">WADServiceFabricSystemEventTable</span></span> |
| <span data-ttu-id="3cca0-127">Události služby Fabric spolehlivé objektu Actor</span><span class="sxs-lookup"><span data-stu-id="3cca0-127">Service Fabric Reliable Actor Events</span></span> |<span data-ttu-id="3cca0-128">Uzly Service Fabric</span><span class="sxs-lookup"><span data-stu-id="3cca0-128">Service Fabric nodes</span></span> |<span data-ttu-id="3cca0-129">WADServiceFabricReliableActorEventTable</span><span class="sxs-lookup"><span data-stu-id="3cca0-129">WADServiceFabricReliableActorEventTable</span></span> |
| <span data-ttu-id="3cca0-130">Události spolehlivé služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="3cca0-130">Service Fabric Reliable Service Events</span></span> |<span data-ttu-id="3cca0-131">Uzly Service Fabric</span><span class="sxs-lookup"><span data-stu-id="3cca0-131">Service Fabric nodes</span></span> |<span data-ttu-id="3cca0-132">WADServiceFabricReliableServiceEventTable</span><span class="sxs-lookup"><span data-stu-id="3cca0-132">WADServiceFabricReliableServiceEventTable</span></span> |
| <span data-ttu-id="3cca0-133">Protokoly událostí systému Windows</span><span class="sxs-lookup"><span data-stu-id="3cca0-133">Windows Event logs</span></span> |<span data-ttu-id="3cca0-134">Uzly Service Fabric</span><span class="sxs-lookup"><span data-stu-id="3cca0-134">Service Fabric nodes</span></span> <br> <span data-ttu-id="3cca0-135">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="3cca0-135">Virtual Machines</span></span> <br> <span data-ttu-id="3cca0-136">Webové role</span><span class="sxs-lookup"><span data-stu-id="3cca0-136">Web roles</span></span> <br> <span data-ttu-id="3cca0-137">Role pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="3cca0-137">Worker roles</span></span> |<span data-ttu-id="3cca0-138">WADWindowsEventLogsTable (Table Storage)</span><span class="sxs-lookup"><span data-stu-id="3cca0-138">WADWindowsEventLogsTable (Table Storage)</span></span> |
| <span data-ttu-id="3cca0-139">Protokoly systému Windows trasování událostí pro Windows</span><span class="sxs-lookup"><span data-stu-id="3cca0-139">Windows ETW logs</span></span> |<span data-ttu-id="3cca0-140">Uzly Service Fabric</span><span class="sxs-lookup"><span data-stu-id="3cca0-140">Service Fabric nodes</span></span> <br> <span data-ttu-id="3cca0-141">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="3cca0-141">Virtual Machines</span></span> <br> <span data-ttu-id="3cca0-142">Webové role</span><span class="sxs-lookup"><span data-stu-id="3cca0-142">Web roles</span></span> <br> <span data-ttu-id="3cca0-143">Role pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="3cca0-143">Worker roles</span></span> |<span data-ttu-id="3cca0-144">WADETWEventTable (Table Storage)</span><span class="sxs-lookup"><span data-stu-id="3cca0-144">WADETWEventTable (Table Storage)</span></span> |

> [!NOTE]
> <span data-ttu-id="3cca0-145">Protokoly služby IIS z webů Azure nejsou aktuálně podporovány.</span><span class="sxs-lookup"><span data-stu-id="3cca0-145">IIS logs from Azure Websites are not currently supported.</span></span>
>
>

<span data-ttu-id="3cca0-146">Pro virtuální počítače, máte možnost hello instalace hello [analýzy protokolů agenta](log-analytics-azure-vm-extension.md) do další přehledy tooenable virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3cca0-146">For virtual machines, you have hello option of installing hello [Log Analytics agent](log-analytics-azure-vm-extension.md) into your virtual machine tooenable additional insights.</span></span> <span data-ttu-id="3cca0-147">Kromě toho protokoly služby IIS možné tooanalyze toobeing a protokoly událostí, můžete provádět další analýzu, včetně sledování změn konfigurace, vyhodnocení SQL a vyhodnocení aktualizací.</span><span class="sxs-lookup"><span data-stu-id="3cca0-147">In addition toobeing able tooanalyze IIS logs and Event Logs, you can perform additional analysis including configuration change tracking, SQL assessment, and update assessment.</span></span>

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a><span data-ttu-id="3cca0-148">Povolit Azure diagnostics ve virtuálním počítači pro protokol událostí a služby IIS protokolu kolekce</span><span class="sxs-lookup"><span data-stu-id="3cca0-148">Enable Azure diagnostics in a virtual machine for event log and IIS log collection</span></span>
<span data-ttu-id="3cca0-149">Hello použijte následující postup tooenable Azure diagnostics ve virtuálním počítači pro protokol událostí a služby IIS protokolu kolekce pomocí portálu Microsoft Azure hello.</span><span class="sxs-lookup"><span data-stu-id="3cca0-149">Use hello following procedure tooenable Azure diagnostics in a virtual machine for Event Log and IIS log collection using hello Microsoft Azure portal.</span></span>

### <a name="tooenable-azure-diagnostics-in-a-virtual-machine-with-hello-azure-portal"></a><span data-ttu-id="3cca0-150">tooenable Azure diagnostics ve virtuálním počítači s hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="3cca0-150">tooenable Azure diagnostics in a virtual machine with hello Azure portal</span></span>
1. <span data-ttu-id="3cca0-151">Když vytvoříte virtuální počítač, nainstalujte hello agenta virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3cca0-151">Install hello VM Agent when you create a virtual machine.</span></span> <span data-ttu-id="3cca0-152">Pokud hello virtuálního počítače již existuje, ověřte, že hello agenta virtuálního počítače je již nainstalována.</span><span class="sxs-lookup"><span data-stu-id="3cca0-152">If hello virtual machine already exists, verify that hello VM Agent is already installed.</span></span>

   * <span data-ttu-id="3cca0-153">V hello portálu Azure, přejděte toohello virtuálního počítače, vyberte **volitelné konfiguraci**, pak **diagnostiky** a nastavte **stav** příliš**na**.</span><span class="sxs-lookup"><span data-stu-id="3cca0-153">In hello Azure portal, navigate toohello virtual machine, select **Optional Configuration**, then **Diagnostics** and set **Status** too**On**.</span></span>

     <span data-ttu-id="3cca0-154">Po dokončení zpracování se hello virtuálních počítačů má rozšíření diagnostiky Azure hello nainstalovaná a spuštěná.</span><span class="sxs-lookup"><span data-stu-id="3cca0-154">Upon completion, hello VM has hello Azure Diagnostics extension installed and running.</span></span> <span data-ttu-id="3cca0-155">Toto rozšíření je zodpovědná za shromažďování dat diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="3cca0-155">This extension is responsible for collecting your diagnostics data.</span></span>
2. <span data-ttu-id="3cca0-156">Povolí monitorování a konfigurace protokolování událostí na existující virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="3cca0-156">Enable monitoring and configure event logging on an existing VM.</span></span> <span data-ttu-id="3cca0-157">Můžete povolit diagnostiku v hello úroveň virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3cca0-157">You can enable diagnostics at hello VM level.</span></span> <span data-ttu-id="3cca0-158">tooenable diagnostiky a potom konfiguraci protokolování událostí, provádět hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="3cca0-158">tooenable diagnostics and then configure event logging, perform hello following steps:</span></span>

   1. <span data-ttu-id="3cca0-159">Vyberte hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="3cca0-159">Select hello VM.</span></span>
   2. <span data-ttu-id="3cca0-160">Klikněte na tlačítko **monitorování**.</span><span class="sxs-lookup"><span data-stu-id="3cca0-160">Click **Monitoring**.</span></span>
   3. <span data-ttu-id="3cca0-161">Klikněte na tlačítko **diagnostiky**.</span><span class="sxs-lookup"><span data-stu-id="3cca0-161">Click **Diagnostics**.</span></span>
   4. <span data-ttu-id="3cca0-162">Sada hello **stav** příliš**ON**.</span><span class="sxs-lookup"><span data-stu-id="3cca0-162">Set hello **Status** too**ON**.</span></span>
   5. <span data-ttu-id="3cca0-163">Vyberte každý protokol diagnostiky, které chcete toocollect.</span><span class="sxs-lookup"><span data-stu-id="3cca0-163">Select each diagnostics log that you want toocollect.</span></span>
   6. <span data-ttu-id="3cca0-164">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="3cca0-164">Click **OK**.</span></span>

## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a><span data-ttu-id="3cca0-165">Povolit Azure diagnostics ve webové roli pro shromažďování protokolů a událostí služby IIS</span><span class="sxs-lookup"><span data-stu-id="3cca0-165">Enable Azure diagnostics in a Web role for IIS log and event collection</span></span>
<span data-ttu-id="3cca0-166">Odkazovat příliš[jak tooEnable diagnostiky v cloudové službě](../cloud-services/cloud-services-dotnet-diagnostics.md) obecné pokyny k povolení Azure diagnostics.</span><span class="sxs-lookup"><span data-stu-id="3cca0-166">Refer too[How tooEnable Diagnostics in a Cloud Service](../cloud-services/cloud-services-dotnet-diagnostics.md) for general steps on enabling Azure diagnostics.</span></span> <span data-ttu-id="3cca0-167">níže uvedené pokyny Hello tyto informace používat a přizpůsobit pro použití s analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="3cca0-167">hello instructions below use this information and customize it for use with Log Analytics.</span></span>

<span data-ttu-id="3cca0-168">S Azure diagnostics povoleno:</span><span class="sxs-lookup"><span data-stu-id="3cca0-168">With Azure diagnostics enabled:</span></span>

* <span data-ttu-id="3cca0-169">Protokoly služby IIS jsou uloženy ve výchozím nastavení, s daty protokolu přenést v intervalu přenos scheduledTransferPeriod hello.</span><span class="sxs-lookup"><span data-stu-id="3cca0-169">IIS logs are stored by default, with log data transferred at hello scheduledTransferPeriod transfer interval.</span></span>
* <span data-ttu-id="3cca0-170">Ve výchozím nastavení se nepřenesou protokoly událostí systému Windows.</span><span class="sxs-lookup"><span data-stu-id="3cca0-170">Windows Event Logs are not transferred by default.</span></span>

### <a name="tooenable-diagnostics"></a><span data-ttu-id="3cca0-171">tooenable diagnostiky</span><span class="sxs-lookup"><span data-stu-id="3cca0-171">tooenable diagnostics</span></span>
<span data-ttu-id="3cca0-172">tooenable protokoly událostí systému Windows nebo toochange hello scheduledTransferPeriod, nakonfigurujte Azure Diagnostics pomocí konfigurační soubor XML hello (diagnostics.wadcfg), jak je znázorněno v [krok 4: vytvoření konfiguračního souboru diagnostiky a instalace hello rozšíření](../cloud-services/cloud-services-dotnet-diagnostics.md)</span><span class="sxs-lookup"><span data-stu-id="3cca0-172">tooenable Windows Event Logs, or toochange hello scheduledTransferPeriod, configure Azure Diagnostics using hello XML configuration file (diagnostics.wadcfg), as shown in [Step 4: Create your Diagnostics configuration file and install hello extension](../cloud-services/cloud-services-dotnet-diagnostics.md)</span></span>

<span data-ttu-id="3cca0-173">Hello následující příklad konfigurační soubor shromažďuje protokoly služby IIS a všechny události z hello aplikace a systémové protokoly:</span><span class="sxs-lookup"><span data-stu-id="3cca0-173">hello following example configuration file collects IIS Logs and all Events from hello Application and System logs:</span></span>

```
    <?xml version="1.0" encoding="utf-8" ?>
    <DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"
          configurationChangePollInterval="PT1M"
          overallQuotaInMB="4096">

      <Directories bufferQuotaInMB="0"
         scheduledTransferPeriod="PT10M">  
        <!-- IISLogs are only relevant tooWeb roles -->
        <IISLogs container="wad-iis" directoryQuotaInMB="0" />
      </Directories>

      <WindowsEventLog bufferQuotaInMB="0"
         scheduledTransferLogLevelFilter="Verbose"
         scheduledTransferPeriod="PT10M">
        <DataSource name="Application!*" />
        <DataSource name="System!*" />
      </WindowsEventLog>

    </DiagnosticMonitorConfiguration>
```

<span data-ttu-id="3cca0-174">Zajistěte, aby vaše ConfigurationSettings určovala účtu úložiště, jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="3cca0-174">Ensure that your ConfigurationSettings specifies a storage account, as in hello following example:</span></span>

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

<span data-ttu-id="3cca0-175">Hello **AccountName** a **AccountKey** hodnoty se nacházejí v hello portál Azure hello panelu účet úložiště, v části Spravovat přístupové klíče.</span><span class="sxs-lookup"><span data-stu-id="3cca0-175">hello **AccountName** and **AccountKey** values are found in hello Azure portal in hello storage account dashboard, under Manage Access Keys.</span></span> <span data-ttu-id="3cca0-176">Hello protokol pro hello připojovací řetězec musí být **https**.</span><span class="sxs-lookup"><span data-stu-id="3cca0-176">hello protocol for hello connection string must be **https**.</span></span>

<span data-ttu-id="3cca0-177">Po aktualizované konfigurace diagnostiky hello tooyour cloudové služby a zapisuje diagnostiky tooAzure úložiště, pak jsou připravené tooconfigure analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="3cca0-177">Once hello updated diagnostic configuration is applied tooyour cloud service and it is writing diagnostics tooAzure Storage, then you are ready tooconfigure Log Analytics.</span></span>

## <a name="use-hello-azure-portal-toocollect-logs-from-azure-storage"></a><span data-ttu-id="3cca0-178">Použití protokolů Azure portálu toocollect hello ze služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="3cca0-178">Use hello Azure portal toocollect logs from Azure Storage</span></span>
<span data-ttu-id="3cca0-179">Hello Azure tooconfigure portálu analýzy protokolů toocollect hello protokoly můžete použít k hello následující služby Azure:</span><span class="sxs-lookup"><span data-stu-id="3cca0-179">You can use hello Azure portal tooconfigure Log Analytics toocollect hello logs for hello following Azure services:</span></span>

* <span data-ttu-id="3cca0-180">Clusterů Service Fabric</span><span class="sxs-lookup"><span data-stu-id="3cca0-180">Service Fabric clusters</span></span>
* <span data-ttu-id="3cca0-181">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="3cca0-181">Virtual Machines</span></span>
* <span data-ttu-id="3cca0-182">Web/role pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="3cca0-182">Web/Worker Roles</span></span>

<span data-ttu-id="3cca0-183">V hello portálu Azure přejděte tooyour pracovní prostor analýzy protokolů a provádět hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="3cca0-183">In hello Azure portal, navigate tooyour Log Analytics workspace and perform hello following tasks:</span></span>

1. <span data-ttu-id="3cca0-184">Klikněte na tlačítko *protokoly účtů úložiště*</span><span class="sxs-lookup"><span data-stu-id="3cca0-184">Click *Storage accounts logs*</span></span>
2. <span data-ttu-id="3cca0-185">Klikněte na tlačítko hello *přidat* úloh</span><span class="sxs-lookup"><span data-stu-id="3cca0-185">Click hello *Add* task</span></span>
3. <span data-ttu-id="3cca0-186">Vyberte účet úložiště hello, který obsahuje hello protokolů diagnostiky</span><span class="sxs-lookup"><span data-stu-id="3cca0-186">Select hello Storage account that contains hello diagnostics logs</span></span>
   * <span data-ttu-id="3cca0-187">Tento účet může být účet úložiště classic nebo účet úložiště Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="3cca0-187">This account can be either a classic storage account or an Azure Resource Manager storage account</span></span>
4. <span data-ttu-id="3cca0-188">Vyberte hello chcete toocollect protokoly pro datový typ</span><span class="sxs-lookup"><span data-stu-id="3cca0-188">Select hello Data Type you want toocollect logs for</span></span>
   * <span data-ttu-id="3cca0-189">Volby Hello jsou protokoly služby IIS. Události; Syslog (Linux); Protokoly trasování událostí pro Windows. Události služby prostředků infrastruktury</span><span class="sxs-lookup"><span data-stu-id="3cca0-189">hello choices are IIS Logs; Events; Syslog (Linux); ETW Logs; Service Fabric Events</span></span>
5. <span data-ttu-id="3cca0-190">Hodnota Hello zdroje se automaticky vyplní podle hello datový typ a nelze změnit</span><span class="sxs-lookup"><span data-stu-id="3cca0-190">hello value for Source is automatically populated based on hello data type and cannot be changed</span></span>
6. <span data-ttu-id="3cca0-191">Klikněte na tlačítko OK toosave hello konfigurace</span><span class="sxs-lookup"><span data-stu-id="3cca0-191">Click OK toosave hello configuration</span></span>

<span data-ttu-id="3cca0-192">Opakujte kroky 2 až 6 pro další úložiště účtů a datové typy, které chcete toocollect analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="3cca0-192">Repeat steps 2-6 for additional storage accounts and data types that you want Log Analytics toocollect.</span></span>

<span data-ttu-id="3cca0-193">V přibližně 30 minut jsou možné toosee data z účtu úložiště hello v analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="3cca0-193">In approximately 30 minutes, you are able toosee data from hello storage account in Log Analytics.</span></span> <span data-ttu-id="3cca0-194">Zobrazí se pouze data, která je zapsán toostorage po použití konfigurace hello.</span><span class="sxs-lookup"><span data-stu-id="3cca0-194">You will only see data that is written toostorage after hello configuration is applied.</span></span> <span data-ttu-id="3cca0-195">Analýzy protokolů číst hello už existující data z účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="3cca0-195">Log Analytics does not read hello pre-existing data from hello storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="3cca0-196">portál Hello neověřuje, že hello zdroj existuje v účtu úložiště hello nebo pokud probíhá zápis nová data.</span><span class="sxs-lookup"><span data-stu-id="3cca0-196">hello portal does not validate that hello Source exists in hello storage account or if new data is being written.</span></span>
>
>

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a><span data-ttu-id="3cca0-197">Povolit Azure diagnostics ve virtuálním počítači pro protokol událostí a služby IIS protokolu kolekce pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="3cca0-197">Enable Azure diagnostics in a virtual machine for event log and IIS log collection using PowerShell</span></span>
<span data-ttu-id="3cca0-198">Hello použijte kroky [tooindex konfigurace analýzy protokolů Azure diagnostics](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) toouse prostředí PowerShell tooread z Azure diagnostiky, které jsou napsané tootable úložiště.</span><span class="sxs-lookup"><span data-stu-id="3cca0-198">Use hello steps in [Configuring Log Analytics tooindex Azure diagnostics](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) toouse PowerShell tooread from Azure diagnostics that are written tootable storage.</span></span>

<span data-ttu-id="3cca0-199">Pomocí Azure PowerShell můžete přesněji určit hello události, které jsou zapsány tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="3cca0-199">Using Azure PowerShell you can more precisely specify hello events that are written tooAzure Storage.</span></span>
<span data-ttu-id="3cca0-200">Další informace najdete v tématu [povolení diagnostiky v Azure Virtual Machines](../virtual-machines-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="3cca0-200">For more information, see [Enabling Diagnostics in Azure Virtual Machines](../virtual-machines-dotnet-diagnostics.md).</span></span>

<span data-ttu-id="3cca0-201">Můžete povolit a aktualizovat Azure diagnostics pomocí hello následující skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3cca0-201">You can enable and update Azure diagnostics using hello following PowerShell script.</span></span>
<span data-ttu-id="3cca0-202">Tento skript můžete také použít s vlastní konfiguraci protokolování.</span><span class="sxs-lookup"><span data-stu-id="3cca0-202">You can also use this script with a custom logging configuration.</span></span>
<span data-ttu-id="3cca0-203">Upravte účet úložiště hello skriptu tooset hello, název služby a název virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="3cca0-203">Modify hello script tooset hello storage account, service name, and virtual machine name.</span></span>
<span data-ttu-id="3cca0-204">skript Hello používá rutiny pro klasické virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="3cca0-204">hello script uses cmdlets for classic virtual machines.</span></span>

<span data-ttu-id="3cca0-205">Zkontrolujte hello následující ukázka skriptu, zkopírujte jej, upravte ho požadovaným způsobem, uložit hello ukázka jako soubor skriptu prostředí PowerShell a potom spusťte hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="3cca0-205">Review hello following script sample, copy it, modify it as needed, save hello sample as a PowerShell script file, and then run hello script.</span></span>

```
    #Connect tooAzure
    Add-AzureAccount

    # settings toochange:
    $wad_storage_account_name = "myStorageAccount"
    $service_name = "myService"
    $vm_name = "myVM"

    #Construct Azure Diagnostics public config and convert tooconfig format

    # Collect just system error events:
    $wad_xml_config = "<WadCfg><DiagnosticMonitorConfiguration><WindowsEventLog scheduledTransferPeriod=""PT1M""><DataSource name=""System!* "" /></WindowsEventLog></DiagnosticMonitorConfiguration></WadCfg>"

    $wad_b64_config = [System.Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes($wad_xml_config))
    $wad_public_config = [string]::Format("{{""xmlCfg"":""{0}""}}",$wad_b64_config)

    #Construct Azure diagnostics private config

    $wad_storage_account_key = (Get-AzureStorageKey $wad_storage_account_name).Primary
    $wad_private_config = [string]::Format("{{""storageAccountName"":""{0}"",""storageAccountKey"":""{1}""}}",$wad_storage_account_name,$wad_storage_account_key)

    #Enable Diagnostics Extension for Virtual Machine

    $wad_extension_name = "IaaSDiagnostics"
    $wad_publisher = "Microsoft.Azure.Diagnostics"
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of hello extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM
```


## <a name="next-steps"></a><span data-ttu-id="3cca0-206">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3cca0-206">Next steps</span></span>
* <span data-ttu-id="3cca0-207">[Shromažďovat protokoly a metriky pro služby Azure](log-analytics-azure-storage.md) podporovaných službami Azure.</span><span class="sxs-lookup"><span data-stu-id="3cca0-207">[Collect logs and metrics for Azure services](log-analytics-azure-storage.md) for supported Azure services.</span></span>
* <span data-ttu-id="3cca0-208">[Povolit řešení](log-analytics-add-solutions.md) tooprovide vhled do dat hello.</span><span class="sxs-lookup"><span data-stu-id="3cca0-208">[Enable Solutions](log-analytics-add-solutions.md) tooprovide insight into hello data.</span></span>
* <span data-ttu-id="3cca0-209">[Použijte vyhledávací dotazy](log-analytics-log-searches.md) tooanalyze hello data.</span><span class="sxs-lookup"><span data-stu-id="3cca0-209">[Use search queries](log-analytics-log-searches.md) tooanalyze hello data.</span></span>
