---
title: "Používání úložiště blob pro službu IIS a tabulka úložiště pro události v Azure Log Analytics | Microsoft Docs"
description: "Analýzy protokolů můžete přečíst v protokolech služby Azure, které zápis diagnostiky table Storage nebo protokoly služby IIS zapisovat do úložiště objektů blob."
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
ms.openlocfilehash: 459ef90ca1d76bada6565bfefd7b4bd1086197d5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-blob-storage-for-iis-and-azure-table-storage-for-events-with-log-analytics"></a><span data-ttu-id="c72f5-103">Používat úložiště objektů blob v Azure pro službu IIS a Azure úložiště table pro události se analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="c72f5-103">Use Azure blob storage for IIS and Azure table storage for events with Log Analytics</span></span>

<span data-ttu-id="c72f5-104">Analýzy protokolů můžete přečíst v protokolech u následujících služeb, které zapsat diagnostiky table Storage nebo protokoly služby IIS zapisovat do úložiště objektů blob:</span><span class="sxs-lookup"><span data-stu-id="c72f5-104">Log Analytics can read the logs for the following services that write diagnostics to table storage or IIS logs written to blob storage:</span></span>

* <span data-ttu-id="c72f5-105">Service Fabric clusterů (Preview)</span><span class="sxs-lookup"><span data-stu-id="c72f5-105">Service Fabric clusters (Preview)</span></span>
* <span data-ttu-id="c72f5-106">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="c72f5-106">Virtual Machines</span></span>
* <span data-ttu-id="c72f5-107">Web/role pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="c72f5-107">Web/Worker Roles</span></span>

<span data-ttu-id="c72f5-108">Před analýzy protokolů můžete shromažďovat data pro tyto prostředky, musí být povolena Azure diagnostics.</span><span class="sxs-lookup"><span data-stu-id="c72f5-108">Before Log Analytics can collect data for these resources, Azure diagnostics must be enabled.</span></span>

<span data-ttu-id="c72f5-109">Po diagnostiky jsou povolené, můžete použít portál Azure nebo PowerShell konfigurace analýzy protokolů shromažďovat protokoly.</span><span class="sxs-lookup"><span data-stu-id="c72f5-109">Once diagnostics are enabled, you can use the Azure portal or PowerShell configure Log Analytics to collect the logs.</span></span>

<span data-ttu-id="c72f5-110">Azure Diagnostics je rozšířením Azure, která umožňuje shromažďování diagnostických dat z role pracovního procesu, webovou roli nebo virtuální počítač spuštěný v Azure.</span><span class="sxs-lookup"><span data-stu-id="c72f5-110">Azure Diagnostics is an Azure extension that enables you to collect diagnostic data from a worker role, web role, or virtual machine running in Azure.</span></span> <span data-ttu-id="c72f5-111">Data je uložený v účtu úložiště Azure a můžete pak shromáždit pomocí analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="c72f5-111">The data is stored in an Azure storage account and can then be collected by Log Analytics.</span></span>

<span data-ttu-id="c72f5-112">Pro analýzy protokolů pro shromažďování těchto protokolů Azure Diagnostics musí být v protokolech v následujících umístěních:</span><span class="sxs-lookup"><span data-stu-id="c72f5-112">For Log Analytics to collect these Azure Diagnostics logs, the logs must be in the following locations:</span></span>

| <span data-ttu-id="c72f5-113">Typ protokolu</span><span class="sxs-lookup"><span data-stu-id="c72f5-113">Log Type</span></span> | <span data-ttu-id="c72f5-114">Typ prostředku</span><span class="sxs-lookup"><span data-stu-id="c72f5-114">Resource Type</span></span> | <span data-ttu-id="c72f5-115">Umístění</span><span class="sxs-lookup"><span data-stu-id="c72f5-115">Location</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c72f5-116">Protokoly IIS</span><span class="sxs-lookup"><span data-stu-id="c72f5-116">IIS logs</span></span> |<span data-ttu-id="c72f5-117">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="c72f5-117">Virtual Machines</span></span> <br> <span data-ttu-id="c72f5-118">Webové role</span><span class="sxs-lookup"><span data-stu-id="c72f5-118">Web roles</span></span> <br> <span data-ttu-id="c72f5-119">Role pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="c72f5-119">Worker roles</span></span> |<span data-ttu-id="c72f5-120">wad logfiles služby iis (úložiště objektů Blob)</span><span class="sxs-lookup"><span data-stu-id="c72f5-120">wad-iis-logfiles (Blob Storage)</span></span> |
| <span data-ttu-id="c72f5-121">Syslog</span><span class="sxs-lookup"><span data-stu-id="c72f5-121">Syslog</span></span> |<span data-ttu-id="c72f5-122">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="c72f5-122">Virtual Machines</span></span> |<span data-ttu-id="c72f5-123">LinuxsyslogVer2v0 (tabulky úložiště)</span><span class="sxs-lookup"><span data-stu-id="c72f5-123">LinuxsyslogVer2v0 (Table Storage)</span></span> |
| <span data-ttu-id="c72f5-124">Provozní události služby prostředků infrastruktury</span><span class="sxs-lookup"><span data-stu-id="c72f5-124">Service Fabric Operational Events</span></span> |<span data-ttu-id="c72f5-125">Uzly Service Fabric</span><span class="sxs-lookup"><span data-stu-id="c72f5-125">Service Fabric nodes</span></span> |<span data-ttu-id="c72f5-126">WADServiceFabricSystemEventTable</span><span class="sxs-lookup"><span data-stu-id="c72f5-126">WADServiceFabricSystemEventTable</span></span> |
| <span data-ttu-id="c72f5-127">Události služby Fabric spolehlivé objektu Actor</span><span class="sxs-lookup"><span data-stu-id="c72f5-127">Service Fabric Reliable Actor Events</span></span> |<span data-ttu-id="c72f5-128">Uzly Service Fabric</span><span class="sxs-lookup"><span data-stu-id="c72f5-128">Service Fabric nodes</span></span> |<span data-ttu-id="c72f5-129">WADServiceFabricReliableActorEventTable</span><span class="sxs-lookup"><span data-stu-id="c72f5-129">WADServiceFabricReliableActorEventTable</span></span> |
| <span data-ttu-id="c72f5-130">Události spolehlivé služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="c72f5-130">Service Fabric Reliable Service Events</span></span> |<span data-ttu-id="c72f5-131">Uzly Service Fabric</span><span class="sxs-lookup"><span data-stu-id="c72f5-131">Service Fabric nodes</span></span> |<span data-ttu-id="c72f5-132">WADServiceFabricReliableServiceEventTable</span><span class="sxs-lookup"><span data-stu-id="c72f5-132">WADServiceFabricReliableServiceEventTable</span></span> |
| <span data-ttu-id="c72f5-133">Protokoly událostí systému Windows</span><span class="sxs-lookup"><span data-stu-id="c72f5-133">Windows Event logs</span></span> |<span data-ttu-id="c72f5-134">Uzly Service Fabric</span><span class="sxs-lookup"><span data-stu-id="c72f5-134">Service Fabric nodes</span></span> <br> <span data-ttu-id="c72f5-135">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="c72f5-135">Virtual Machines</span></span> <br> <span data-ttu-id="c72f5-136">Webové role</span><span class="sxs-lookup"><span data-stu-id="c72f5-136">Web roles</span></span> <br> <span data-ttu-id="c72f5-137">Role pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="c72f5-137">Worker roles</span></span> |<span data-ttu-id="c72f5-138">WADWindowsEventLogsTable (Table Storage)</span><span class="sxs-lookup"><span data-stu-id="c72f5-138">WADWindowsEventLogsTable (Table Storage)</span></span> |
| <span data-ttu-id="c72f5-139">Protokoly systému Windows trasování událostí pro Windows</span><span class="sxs-lookup"><span data-stu-id="c72f5-139">Windows ETW logs</span></span> |<span data-ttu-id="c72f5-140">Uzly Service Fabric</span><span class="sxs-lookup"><span data-stu-id="c72f5-140">Service Fabric nodes</span></span> <br> <span data-ttu-id="c72f5-141">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="c72f5-141">Virtual Machines</span></span> <br> <span data-ttu-id="c72f5-142">Webové role</span><span class="sxs-lookup"><span data-stu-id="c72f5-142">Web roles</span></span> <br> <span data-ttu-id="c72f5-143">Role pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="c72f5-143">Worker roles</span></span> |<span data-ttu-id="c72f5-144">WADETWEventTable (Table Storage)</span><span class="sxs-lookup"><span data-stu-id="c72f5-144">WADETWEventTable (Table Storage)</span></span> |

> [!NOTE]
> <span data-ttu-id="c72f5-145">Protokoly služby IIS z webů Azure nejsou aktuálně podporovány.</span><span class="sxs-lookup"><span data-stu-id="c72f5-145">IIS logs from Azure Websites are not currently supported.</span></span>
>
>

<span data-ttu-id="c72f5-146">Pro virtuální počítače, máte možnost nainstalovat [analýzy protokolů agenta](log-analytics-azure-vm-extension.md) do virtuálního počítače povolit další statistiky.</span><span class="sxs-lookup"><span data-stu-id="c72f5-146">For virtual machines, you have the option of installing the [Log Analytics agent](log-analytics-azure-vm-extension.md) into your virtual machine to enable additional insights.</span></span> <span data-ttu-id="c72f5-147">Kromě toho schopní analyzovat protokoly událostí a protokoly služby IIS, můžete provádět další analýzu, včetně sledování změn konfigurace, vyhodnocení SQL a vyhodnocení aktualizací.</span><span class="sxs-lookup"><span data-stu-id="c72f5-147">In addition to being able to analyze IIS logs and Event Logs, you can perform additional analysis including configuration change tracking, SQL assessment, and update assessment.</span></span>

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection"></a><span data-ttu-id="c72f5-148">Povolit Azure diagnostics ve virtuálním počítači pro protokol událostí a služby IIS protokolu kolekce</span><span class="sxs-lookup"><span data-stu-id="c72f5-148">Enable Azure diagnostics in a virtual machine for event log and IIS log collection</span></span>
<span data-ttu-id="c72f5-149">Pomocí následujícího postupu umožníte Azure diagnostics ve virtuálním počítači pro protokol událostí a služby IIS protokolu kolekci pomocí portálu Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="c72f5-149">Use the following procedure to enable Azure diagnostics in a virtual machine for Event Log and IIS log collection using the Microsoft Azure portal.</span></span>

### <a name="to-enable-azure-diagnostics-in-a-virtual-machine-with-the-azure-portal"></a><span data-ttu-id="c72f5-150">Chcete-li povolit Azure diagnostics ve virtuálním počítači pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="c72f5-150">To enable Azure diagnostics in a virtual machine with the Azure portal</span></span>
1. <span data-ttu-id="c72f5-151">Když vytvoříte virtuální počítač, nainstalujte agenta virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c72f5-151">Install the VM Agent when you create a virtual machine.</span></span> <span data-ttu-id="c72f5-152">Pokud virtuální počítač již existuje, ověřte, že je již nainstalován Agent virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c72f5-152">If the virtual machine already exists, verify that the VM Agent is already installed.</span></span>

   * <span data-ttu-id="c72f5-153">Na portálu Azure přejděte do virtuálního počítače, vyberte **volitelné konfiguraci**, pak **diagnostiky** a nastavte **stav** k **na**.</span><span class="sxs-lookup"><span data-stu-id="c72f5-153">In the Azure portal, navigate to the virtual machine, select **Optional Configuration**, then **Diagnostics** and set **Status** to **On**.</span></span>

     <span data-ttu-id="c72f5-154">Po dokončení zpracování se má virtuální počítač Azure Diagnostics rozšíření nainstalovaná a spuštěná.</span><span class="sxs-lookup"><span data-stu-id="c72f5-154">Upon completion, the VM has the Azure Diagnostics extension installed and running.</span></span> <span data-ttu-id="c72f5-155">Toto rozšíření je zodpovědná za shromažďování dat diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="c72f5-155">This extension is responsible for collecting your diagnostics data.</span></span>
2. <span data-ttu-id="c72f5-156">Povolí monitorování a konfigurace protokolování událostí na existující virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="c72f5-156">Enable monitoring and configure event logging on an existing VM.</span></span> <span data-ttu-id="c72f5-157">Můžete povolit diagnostiku na úrovni virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c72f5-157">You can enable diagnostics at the VM level.</span></span> <span data-ttu-id="c72f5-158">Povolte diagnostiku a pak nastavte protokolování událostí, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c72f5-158">To enable diagnostics and then configure event logging, perform the following steps:</span></span>

   1. <span data-ttu-id="c72f5-159">Vyberte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="c72f5-159">Select the VM.</span></span>
   2. <span data-ttu-id="c72f5-160">Klikněte na tlačítko **monitorování**.</span><span class="sxs-lookup"><span data-stu-id="c72f5-160">Click **Monitoring**.</span></span>
   3. <span data-ttu-id="c72f5-161">Klikněte na tlačítko **diagnostiky**.</span><span class="sxs-lookup"><span data-stu-id="c72f5-161">Click **Diagnostics**.</span></span>
   4. <span data-ttu-id="c72f5-162">Nastavte **stav** k **ON**.</span><span class="sxs-lookup"><span data-stu-id="c72f5-162">Set the **Status** to **ON**.</span></span>
   5. <span data-ttu-id="c72f5-163">Vyberte každý diagnostiky protokolu, které chcete shromažďovat.</span><span class="sxs-lookup"><span data-stu-id="c72f5-163">Select each diagnostics log that you want to collect.</span></span>
   6. <span data-ttu-id="c72f5-164">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c72f5-164">Click **OK**.</span></span>

## <a name="enable-azure-diagnostics-in-a-web-role-for-iis-log-and-event-collection"></a><span data-ttu-id="c72f5-165">Povolit Azure diagnostics ve webové roli pro shromažďování protokolů a událostí služby IIS</span><span class="sxs-lookup"><span data-stu-id="c72f5-165">Enable Azure diagnostics in a Web role for IIS log and event collection</span></span>
<span data-ttu-id="c72f5-166">Odkazovat na [jak k povolení diagnostiky v cloudové službě](../cloud-services/cloud-services-dotnet-diagnostics.md) obecné pokyny k povolení Azure diagnostics.</span><span class="sxs-lookup"><span data-stu-id="c72f5-166">Refer to [How To Enable Diagnostics in a Cloud Service](../cloud-services/cloud-services-dotnet-diagnostics.md) for general steps on enabling Azure diagnostics.</span></span> <span data-ttu-id="c72f5-167">Níže uvedené pokyny použijte tyto informace a přizpůsobit pro použití s analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="c72f5-167">The instructions below use this information and customize it for use with Log Analytics.</span></span>

<span data-ttu-id="c72f5-168">S Azure diagnostics povoleno:</span><span class="sxs-lookup"><span data-stu-id="c72f5-168">With Azure diagnostics enabled:</span></span>

* <span data-ttu-id="c72f5-169">Protokoly služby IIS jsou uloženy ve výchozím nastavení, s přenést v intervalu scheduledTransferPeriod přenosu dat protokolu.</span><span class="sxs-lookup"><span data-stu-id="c72f5-169">IIS logs are stored by default, with log data transferred at the scheduledTransferPeriod transfer interval.</span></span>
* <span data-ttu-id="c72f5-170">Ve výchozím nastavení se nepřenesou protokoly událostí systému Windows.</span><span class="sxs-lookup"><span data-stu-id="c72f5-170">Windows Event Logs are not transferred by default.</span></span>

### <a name="to-enable-diagnostics"></a><span data-ttu-id="c72f5-171">Povolí se Diagnostika</span><span class="sxs-lookup"><span data-stu-id="c72f5-171">To enable diagnostics</span></span>
<span data-ttu-id="c72f5-172">Chcete-li povolit protokol událostí systému Windows, nebo změnit scheduledTransferPeriod, konfigurovat Azure Diagnostics pomocí konfiguračního souboru XML (diagnostics.wadcfg), jak je znázorněno v [krok 4: vytvoření konfiguračního souboru diagnostiky a nainstalovat rozšíření](../cloud-services/cloud-services-dotnet-diagnostics.md)</span><span class="sxs-lookup"><span data-stu-id="c72f5-172">To enable Windows Event Logs, or to change the scheduledTransferPeriod, configure Azure Diagnostics using the XML configuration file (diagnostics.wadcfg), as shown in [Step 4: Create your Diagnostics configuration file and install the extension](../cloud-services/cloud-services-dotnet-diagnostics.md)</span></span>

<span data-ttu-id="c72f5-173">Následující příklad konfiguračního souboru shromažďuje protokoly služby IIS a všechny události z protokolů systému a aplikace:</span><span class="sxs-lookup"><span data-stu-id="c72f5-173">The following example configuration file collects IIS Logs and all Events from the Application and System logs:</span></span>

```
    <?xml version="1.0" encoding="utf-8" ?>
    <DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"
          configurationChangePollInterval="PT1M"
          overallQuotaInMB="4096">

      <Directories bufferQuotaInMB="0"
         scheduledTransferPeriod="PT10M">  
        <!-- IISLogs are only relevant to Web roles -->
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

<span data-ttu-id="c72f5-174">Zajistěte, aby vaše ConfigurationSettings určovala účtu úložiště, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="c72f5-174">Ensure that your ConfigurationSettings specifies a storage account, as in the following example:</span></span>

```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"/>
    </ConfigurationSettings>
```

<span data-ttu-id="c72f5-175">**AccountName** a **AccountKey** hodnoty se nacházejí v portálu Azure v řídicím panelu účet úložiště, v části Spravovat přístupové klíče.</span><span class="sxs-lookup"><span data-stu-id="c72f5-175">The **AccountName** and **AccountKey** values are found in the Azure portal in the storage account dashboard, under Manage Access Keys.</span></span> <span data-ttu-id="c72f5-176">Protokol pro připojovací řetězec musí být **https**.</span><span class="sxs-lookup"><span data-stu-id="c72f5-176">The protocol for the connection string must be **https**.</span></span>

<span data-ttu-id="c72f5-177">Jakmile aktualizované konfigurace diagnostiky se použije ke cloudové službě a diagnostiky ho je zápis do úložiště Azure, pak jste připraveni ke konfiguraci analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="c72f5-177">Once the updated diagnostic configuration is applied to your cloud service and it is writing diagnostics to Azure Storage, then you are ready to configure Log Analytics.</span></span>

## <a name="use-the-azure-portal-to-collect-logs-from-azure-storage"></a><span data-ttu-id="c72f5-178">Pomocí portálu Azure ke shromažďování protokolů z Azure Storage</span><span class="sxs-lookup"><span data-stu-id="c72f5-178">Use the Azure portal to collect logs from Azure Storage</span></span>
<span data-ttu-id="c72f5-179">Na portálu Azure můžete použít ke konfiguraci analýzy protokolů pro shromažďování protokolů u následujících služeb Azure:</span><span class="sxs-lookup"><span data-stu-id="c72f5-179">You can use the Azure portal to configure Log Analytics to collect the logs for the following Azure services:</span></span>

* <span data-ttu-id="c72f5-180">Clusterů Service Fabric</span><span class="sxs-lookup"><span data-stu-id="c72f5-180">Service Fabric clusters</span></span>
* <span data-ttu-id="c72f5-181">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="c72f5-181">Virtual Machines</span></span>
* <span data-ttu-id="c72f5-182">Web/role pracovního procesu</span><span class="sxs-lookup"><span data-stu-id="c72f5-182">Web/Worker Roles</span></span>

<span data-ttu-id="c72f5-183">Na portálu Azure přejděte do pracovního prostoru analýzy protokolů a provádět následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="c72f5-183">In the Azure portal, navigate to your Log Analytics workspace and perform the following tasks:</span></span>

1. <span data-ttu-id="c72f5-184">Klikněte na tlačítko *protokoly účtů úložiště*</span><span class="sxs-lookup"><span data-stu-id="c72f5-184">Click *Storage accounts logs*</span></span>
2. <span data-ttu-id="c72f5-185">Klikněte *přidat* úloh</span><span class="sxs-lookup"><span data-stu-id="c72f5-185">Click the *Add* task</span></span>
3. <span data-ttu-id="c72f5-186">Vyberte účet úložiště, který obsahuje protokolů diagnostiky</span><span class="sxs-lookup"><span data-stu-id="c72f5-186">Select the Storage account that contains the diagnostics logs</span></span>
   * <span data-ttu-id="c72f5-187">Tento účet může být účet úložiště classic nebo účet úložiště Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c72f5-187">This account can be either a classic storage account or an Azure Resource Manager storage account</span></span>
4. <span data-ttu-id="c72f5-188">Vyberte datový typ, který chcete shromáždit protokoly pro</span><span class="sxs-lookup"><span data-stu-id="c72f5-188">Select the Data Type you want to collect logs for</span></span>
   * <span data-ttu-id="c72f5-189">Možnosti jsou protokoly služby IIS. Události; Syslog (Linux); Protokoly trasování událostí pro Windows. Události služby prostředků infrastruktury</span><span class="sxs-lookup"><span data-stu-id="c72f5-189">The choices are IIS Logs; Events; Syslog (Linux); ETW Logs; Service Fabric Events</span></span>
5. <span data-ttu-id="c72f5-190">Hodnota pro zdroj je automaticky vyplněno na základě typu dat a nedá se změnit</span><span class="sxs-lookup"><span data-stu-id="c72f5-190">The value for Source is automatically populated based on the data type and cannot be changed</span></span>
6. <span data-ttu-id="c72f5-191">Kliknutím na tlačítko OK uložte konfiguraci</span><span class="sxs-lookup"><span data-stu-id="c72f5-191">Click OK to save the configuration</span></span>

<span data-ttu-id="c72f5-192">Opakujte kroky 2 až 6 pro další účty úložiště a datové typy, které chcete pro shromažďování protokolů analýzy.</span><span class="sxs-lookup"><span data-stu-id="c72f5-192">Repeat steps 2-6 for additional storage accounts and data types that you want Log Analytics to collect.</span></span>

<span data-ttu-id="c72f5-193">V přibližně 30 minut budete moci zobrazit data z účtu úložiště v analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="c72f5-193">In approximately 30 minutes, you are able to see data from the storage account in Log Analytics.</span></span> <span data-ttu-id="c72f5-194">Zobrazí se pouze data, která je zapsán do úložiště po použití konfigurace.</span><span class="sxs-lookup"><span data-stu-id="c72f5-194">You will only see data that is written to storage after the configuration is applied.</span></span> <span data-ttu-id="c72f5-195">Analýzy protokolů číst existující data z účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="c72f5-195">Log Analytics does not read the pre-existing data from the storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="c72f5-196">Na portálu neověřuje, zda zdroj existuje v účtu úložiště nebo pokud probíhá zápis nová data.</span><span class="sxs-lookup"><span data-stu-id="c72f5-196">The portal does not validate that the Source exists in the storage account or if new data is being written.</span></span>
>
>

## <a name="enable-azure-diagnostics-in-a-virtual-machine-for-event-log-and-iis-log-collection-using-powershell"></a><span data-ttu-id="c72f5-197">Povolit Azure diagnostics ve virtuálním počítači pro protokol událostí a služby IIS protokolu kolekce pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c72f5-197">Enable Azure diagnostics in a virtual machine for event log and IIS log collection using PowerShell</span></span>
<span data-ttu-id="c72f5-198">Postupujte podle kroků v [konfigurace analýzy protokolů Azure diagnostics indexování](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) číst z Azure diagnostiky, které jsou napsané table Storage pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c72f5-198">Use the steps in [Configuring Log Analytics to index Azure diagnostics](log-analytics-powershell-workspace-configuration.md#configuring-log-analytics-to-index-azure-diagnostics) to use PowerShell to read from Azure diagnostics that are written to table storage.</span></span>

<span data-ttu-id="c72f5-199">Pomocí Azure PowerShell přesněji můžete události, které se zapisují do služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="c72f5-199">Using Azure PowerShell you can more precisely specify the events that are written to Azure Storage.</span></span>
<span data-ttu-id="c72f5-200">Další informace najdete v tématu [povolení diagnostiky v Azure Virtual Machines](../virtual-machines-dotnet-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="c72f5-200">For more information, see [Enabling Diagnostics in Azure Virtual Machines](../virtual-machines-dotnet-diagnostics.md).</span></span>

<span data-ttu-id="c72f5-201">Můžete povolit a aktualizovat Azure diagnostics pomocí následujícího skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c72f5-201">You can enable and update Azure diagnostics using the following PowerShell script.</span></span>
<span data-ttu-id="c72f5-202">Tento skript můžete také použít s vlastní konfiguraci protokolování.</span><span class="sxs-lookup"><span data-stu-id="c72f5-202">You can also use this script with a custom logging configuration.</span></span>
<span data-ttu-id="c72f5-203">Upravte skript tak, aby nastavení účtu úložiště, název služby a název virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c72f5-203">Modify the script to set the storage account, service name, and virtual machine name.</span></span>
<span data-ttu-id="c72f5-204">Tento skript využívá rutiny pro klasické virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="c72f5-204">The script uses cmdlets for classic virtual machines.</span></span>

<span data-ttu-id="c72f5-205">Zkontrolujte následující ukázka skriptu, zkopírujte jej, upravte ho požadovaným způsobem, uložit ukázku jako soubor skriptu prostředí PowerShell a spusťte skript.</span><span class="sxs-lookup"><span data-stu-id="c72f5-205">Review the following script sample, copy it, modify it as needed, save the sample as a PowerShell script file, and then run the script.</span></span>

```
    #Connect to Azure
    Add-AzureAccount

    # settings to change:
    $wad_storage_account_name = "myStorageAccount"
    $service_name = "myService"
    $vm_name = "myVM"

    #Construct Azure Diagnostics public config and convert to config format

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
    $wad_version = (Get-AzureVMAvailableExtension -Publisher $wad_publisher -ExtensionName $wad_extension_name).Version # Gets latest version of the extension

    (Get-AzureVM -ServiceName $service_name -Name $vm_name) | Set-AzureVMExtension -ExtensionName $wad_extension_name -Publisher $wad_publisher -PublicConfiguration $wad_public_config -PrivateConfiguration $wad_private_config -Version $wad_version | Update-AzureVM
```


## <a name="next-steps"></a><span data-ttu-id="c72f5-206">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c72f5-206">Next steps</span></span>
* <span data-ttu-id="c72f5-207">[Shromažďovat protokoly a metriky pro služby Azure](log-analytics-azure-storage.md) podporovaných službami Azure.</span><span class="sxs-lookup"><span data-stu-id="c72f5-207">[Collect logs and metrics for Azure services](log-analytics-azure-storage.md) for supported Azure services.</span></span>
* <span data-ttu-id="c72f5-208">[Povolit řešení](log-analytics-add-solutions.md) zajistit přehled o data.</span><span class="sxs-lookup"><span data-stu-id="c72f5-208">[Enable Solutions](log-analytics-add-solutions.md) to provide insight into the data.</span></span>
* <span data-ttu-id="c72f5-209">[Použijte vyhledávací dotazy](log-analytics-log-searches.md) analyzovat data.</span><span class="sxs-lookup"><span data-stu-id="c72f5-209">[Use search queries](log-analytics-log-searches.md) to analyze the data.</span></span>
