---
title: "Konfigurace Azure Diagnostics k odesílání dat do služby Application Insights | Microsoft Docs"
description: "Aktualizace konfigurace veřejného Azure Diagnostics k odesílání dat do služby Application Insights."
services: monitoring-and-diagnostics
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: f9e12c3e-c307-435e-a149-ef0fef20513a
ms.service: monitoring-and-diagnostics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/19/2016
ms.author: robb
ms.openlocfilehash: 67dc2d5bbfa2012e4e098616edda593d023c4c1e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="send-cloud-service-virtual-machine-or-service-fabric-diagnostic-data-to-application-insights"></a><span data-ttu-id="db173-103">Posílat diagnostická data Cloudová služba, virtuální počítač nebo Service Fabric Application insights</span><span class="sxs-lookup"><span data-stu-id="db173-103">Send Cloud Service, Virtual Machine, or Service Fabric diagnostic data to Application Insights</span></span>
<span data-ttu-id="db173-104">Cloudové služby, virtuální počítače sady škálování virtuálního počítače a Service Fabric všechny použít ke shromažďování dat rozšíření Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="db173-104">Cloud services, Virtual Machines, Virtual Machine Scale Sets and Service Fabric all use the Azure Diagnostics extension to collect data.</span></span>  <span data-ttu-id="db173-105">Azure diagnostics odesílá data do tabulky služby Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="db173-105">Azure diagnostics sends data to Azure Storage tables.</span></span>  <span data-ttu-id="db173-106">Můžete však také všechny kanálu nebo podmnožinu dat do jiných umístění pomocí rozšíření Azure Diagnostics 1.5 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="db173-106">However, you can also pipe all or a subset of the data to other locations using Azure Diagnostics extension 1.5 or later.</span></span>

<span data-ttu-id="db173-107">Tento článek popisuje, jak k odesílání dat z rozšíření Azure Diagnostics do Application Insights.</span><span class="sxs-lookup"><span data-stu-id="db173-107">This article describes how to send data from the Azure Diagnostics extension to Application Insights.</span></span>

## <a name="diagnostics-configuration-explained"></a><span data-ttu-id="db173-108">Vysvětlení konfigurace diagnostiky</span><span class="sxs-lookup"><span data-stu-id="db173-108">Diagnostics configuration explained</span></span>
<span data-ttu-id="db173-109">Rozšíření Azure diagnostiky 1.5 zavedená jímky, které jsou dalších místech, kde můžete poslat diagnostická data.</span><span class="sxs-lookup"><span data-stu-id="db173-109">The Azure diagnostics extension 1.5 introduced sinks, which are additional locations where you can send diagnostic data.</span></span>

<span data-ttu-id="db173-110">Příklad konfigurace jímka pro službu Application Insights:</span><span class="sxs-lookup"><span data-stu-id="db173-110">Example configuration of a sink for Application Insights:</span></span>

```XML
<SinksConfig>
    <Sink name="ApplicationInsights">
      <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>
      <Channels>
        <Channel logLevel="Error" name="MyTopDiagData"  />
        <Channel logLevel="Verbose" name="MyLogData"  />
      </Channels>
    </Sink>
</SinksConfig>
```
```JSON
"SinksConfig": {
    "Sink": [
        {
            "name": "ApplicationInsights",
            "ApplicationInsights": "{Insert InstrumentationKey}",
            "Channels": {
                "Channel": [
                    {
                        "logLevel": "Error",
                        "name": "MyTopDiagData"
                    },
                    {
                        "logLevel": "Error",
                        "name": "MyLogData"
                    }
                ]
            }
        }
    ]
}
```
- <span data-ttu-id="db173-111">**Podřízený** *název* atribut je řetězcová hodnota, která jednoznačně identifikuje jímky.</span><span class="sxs-lookup"><span data-stu-id="db173-111">The **Sink** *name* attribute is a string value that uniquely identifies the sink.</span></span>

- <span data-ttu-id="db173-112">**ApplicationInsights** element Určuje klíč instrumentace prostředku statistiky aplikace, kde Azure diagnostics data se odesílají.</span><span class="sxs-lookup"><span data-stu-id="db173-112">The **ApplicationInsights** element specifies instrumentation key of the Application insights resource where the Azure diagnostics data is sent.</span></span>
    - <span data-ttu-id="db173-113">Pokud nemáte existující prostředek Application Insights, přečtěte si téma [vytvořte nový prostředek Application Insights](../application-insights/app-insights-create-new-resource.md) Další informace o vytvoření prostředku a získání klíč instrumentace.</span><span class="sxs-lookup"><span data-stu-id="db173-113">If you don't have an existing Application Insights resource, see [Create a new Application Insights resource](../application-insights/app-insights-create-new-resource.md) for more information on creating a resource and getting the instrumentation key.</span></span>
    - <span data-ttu-id="db173-114">Pokud vyvíjíte cloudové služby s Azure SDK 2.8 nebo novější, tento klíč instrumentace se automaticky vyplní.</span><span class="sxs-lookup"><span data-stu-id="db173-114">If you are developing a Cloud Service with Azure SDK 2.8 and later, this instrumentation key is automatically populated.</span></span> <span data-ttu-id="db173-115">Hodnota je založena na **APPINSIGHTS_INSTRUMENTATIONKEY** nastavení konfigurace služby, když balení projekt cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="db173-115">The value is based on the **APPINSIGHTS_INSTRUMENTATIONKEY** service configuration setting when packaging the Cloud Service project.</span></span> <span data-ttu-id="db173-116">V tématu [pomocí Application Insights s potíží Cloudová služba Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics-applicationinsights.md).</span><span class="sxs-lookup"><span data-stu-id="db173-116">See [Use Application Insights with Azure Diagnostics to troubleshoot Cloud Service issues](../cloud-services/cloud-services-dotnet-diagnostics-applicationinsights.md).</span></span>

- <span data-ttu-id="db173-117">**Kanály** element obsahuje jeden nebo více **kanál** elementy.</span><span class="sxs-lookup"><span data-stu-id="db173-117">The **Channels** element contains one or more **Channel** elements.</span></span>
    - <span data-ttu-id="db173-118">*Název* atribut jednoznačně odkazuje na tomto kanálu.</span><span class="sxs-lookup"><span data-stu-id="db173-118">The *name* attribute uniquely refers to that channel.</span></span>
    - <span data-ttu-id="db173-119">*Loglevel* atribut umožňuje určit úroveň protokolu, který umožňuje kanál.</span><span class="sxs-lookup"><span data-stu-id="db173-119">The *loglevel* attribute lets you specify the log level that the channel allows.</span></span> <span data-ttu-id="db173-120">Jsou k dispozici protokolu úrovně v pořadí podle nejvíc minimálně informace:</span><span class="sxs-lookup"><span data-stu-id="db173-120">The available log levels in order of most to least information are:</span></span>
        - <span data-ttu-id="db173-121">Verbose</span><span class="sxs-lookup"><span data-stu-id="db173-121">Verbose</span></span>
        - <span data-ttu-id="db173-122">Informace</span><span class="sxs-lookup"><span data-stu-id="db173-122">Information</span></span>
        - <span data-ttu-id="db173-123">Upozornění</span><span class="sxs-lookup"><span data-stu-id="db173-123">Warning</span></span>
        - <span data-ttu-id="db173-124">Chyba</span><span class="sxs-lookup"><span data-stu-id="db173-124">Error</span></span>
        - <span data-ttu-id="db173-125">Kritické</span><span class="sxs-lookup"><span data-stu-id="db173-125">Critical</span></span>

<span data-ttu-id="db173-126">Kanál, který funguje jako filtr a umožňuje vybrat konkrétní protokolu úrovně k odeslání do cílové jímky.</span><span class="sxs-lookup"><span data-stu-id="db173-126">A channel acts like a filter and allows you to select specific log levels to send to the target sink.</span></span> <span data-ttu-id="db173-127">Například může shromáždit podrobné protokoly a jejich odeslání do úložiště ale odeslat pouze chyby jímky.</span><span class="sxs-lookup"><span data-stu-id="db173-127">For example, you could collect verbose logs and send them to storage, but send only Errors to the sink.</span></span>

<span data-ttu-id="db173-128">Tento vztah je znázorněný na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="db173-128">The following graphic shows this relationship.</span></span>

![Veřejná konfigurace diagnostiky](./media/azure-diagnostics-configure-applicationinsights/AzDiag_Channels_App_Insights.png)

<span data-ttu-id="db173-130">Na následujícím obrázku shrnuje hodnoty konfigurace a funkce.</span><span class="sxs-lookup"><span data-stu-id="db173-130">The following graphic summarizes the configuration values and how they work.</span></span> <span data-ttu-id="db173-131">V konfiguraci na různých úrovních v hierarchii můžete zahrnout více jímky.</span><span class="sxs-lookup"><span data-stu-id="db173-131">You can include multiple sinks in the configuration at different levels in the hierarchy.</span></span> <span data-ttu-id="db173-132">Podřízený na nejvyšší úrovni funguje jako globální nastavení a verze zadaná v jednotlivých element se chová jako přepsání pro tuto globální nastavení.</span><span class="sxs-lookup"><span data-stu-id="db173-132">The sink at the top level acts as a global setting and the one specified at the individual element acts like an override to that global setting.</span></span>

![Diagnostika jímky konfigurace pomocí nástroje Application Insights](./media/azure-diagnostics-configure-applicationinsights/Azure_Diagnostics_Sinks.png)

## <a name="complete-sink-configuration-example"></a><span data-ttu-id="db173-134">Příklad konfigurace dokončení jímka</span><span class="sxs-lookup"><span data-stu-id="db173-134">Complete sink configuration example</span></span>
<span data-ttu-id="db173-135">Tady je kompletní příklad veřejné konfigurace souboru, který</span><span class="sxs-lookup"><span data-stu-id="db173-135">Here is a complete example of the public configuration file that</span></span>
1. <span data-ttu-id="db173-136">odešle všechny chyby Application insights (zadané v **DiagnosticMonitorConfiguration** uzlu)</span><span class="sxs-lookup"><span data-stu-id="db173-136">sends all errors to Application Insights (specified at the **DiagnosticMonitorConfiguration** node)</span></span>
2. <span data-ttu-id="db173-137">rovněž odesílá podrobné úrovni protokoly pro protokoly aplikací (zadané v **protokoly** uzlu).</span><span class="sxs-lookup"><span data-stu-id="db173-137">also sends Verbose level logs for the Application Logs (specified at the **Logs** node).</span></span>

```XML
<WadCfg>
  <DiagnosticMonitorConfiguration overallQuotaInMB="4096"
       sinks="ApplicationInsights.MyTopDiagData"> <!-- All info below sent to this channel -->
    <DiagnosticInfrastructureLogs />
    <PerformanceCounters>
      <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
      <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
    </PerformanceCounters>
    <WindowsEventLog scheduledTransferPeriod="PT1M">
      <DataSource name="Application!*" />
    </WindowsEventLog>
    <Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Verbose"
            sinks="ApplicationInsights.MyLogData"/> <!-- This specific info sent to this channel -->
  </DiagnosticMonitorConfiguration>

<SinksConfig>
    <Sink name="ApplicationInsights">
      <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>
      <Channels>
        <Channel logLevel="Error" name="MyTopDiagData"  />
        <Channel logLevel="Verbose" name="MyLogData"  />
      </Channels>
    </Sink>
  </SinksConfig>
</WadCfg>
```
```JSON
"WadCfg": {
    "DiagnosticMonitorConfiguration": {
        "overallQuotaInMB": 4096,
        "sinks": "ApplicationInsights.MyTopDiagData", "_comment": "All info below sent to this channel",
        "DiagnosticInfrastructureLogs": {
        },
        "PerformanceCounters": {
            "PerformanceCounterConfiguration": [
                {
                    "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                    "sampleRate": "PT3M"
                },
                {
                    "counterSpecifier": "\\Memory\\Available MBytes",
                    "sampleRate": "PT3M"
                }
            ]
        },
        "WindowsEventLog": {
            "scheduledTransferPeriod": "PT1M",
            "DataSource": [
                {
                    "name": "Application!*"
                }
            ]
        },
        "Logs": {
            "scheduledTransferPeriod": "PT1M",
            "scheduledTransferLogLevelFilter": "Verbose",
            "sinks": "ApplicationInsights.MyLogData", "_comment": "This specific info sent to this channel"
        }
    },
    "SinksConfig": {
        "Sink": [
            {
                "name": "ApplicationInsights",
                "ApplicationInsights": "{Insert InstrumentationKey}",
                "Channels": {
                    "Channel": [
                        {
                            "logLevel": "Error",
                            "name": "MyTopDiagData"
                        },
                        {
                            "logLevel": "Verbose",
                            "name": "MyLogData"
                        }
                    ]
                }
            }
        ]
    }
}
```
<span data-ttu-id="db173-138">Následující řádky v předchozí konfiguraci, mají následující významy:</span><span class="sxs-lookup"><span data-stu-id="db173-138">In the previous configuration, the following lines have the following meanings:</span></span>

### <a name="send-all-the-data-that-is-being-collected-by-azure-diagnostics"></a><span data-ttu-id="db173-139">Odeslat veškerá data, které jsou shromažďována Azure diagnostics</span><span class="sxs-lookup"><span data-stu-id="db173-139">Send all the data that is being collected by Azure diagnostics</span></span>

```XML
<DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights">
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights",
}
```

### <a name="send-only-error-logs-to-the-application-insights-sink"></a><span data-ttu-id="db173-140">Pouze protokoly chyb poslat jímky Application Insights</span><span class="sxs-lookup"><span data-stu-id="db173-140">Send only error logs to the Application Insights sink</span></span>

```XML
<DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights.MyTopDiagdata">
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights.MyTopDiagData",
}
```

### <a name="send-verbose-application-logs-to-application-insights"></a><span data-ttu-id="db173-141">Odeslání protokolů s podrobné aplikace Application Insights</span><span class="sxs-lookup"><span data-stu-id="db173-141">Send Verbose application logs to Application Insights</span></span>

```XML
<Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Verbose" sinks="ApplicationInsights.MyLogData"/>
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights.MyLogData",
}
```

## <a name="limitations"></a><span data-ttu-id="db173-142">Omezení</span><span class="sxs-lookup"><span data-stu-id="db173-142">Limitations</span></span>

- <span data-ttu-id="db173-143">**Kanály protokolu pouze typ a není výkonu čítače.**</span><span class="sxs-lookup"><span data-stu-id="db173-143">**Channels only log type and not performance counters.**</span></span> <span data-ttu-id="db173-144">Pokud zadáte kanál s elementem čítače výkonu, je ignorován.</span><span class="sxs-lookup"><span data-stu-id="db173-144">If you specify a channel with a performance counter element, it is ignored.</span></span>
- <span data-ttu-id="db173-145">**Úroveň protokolu pro kanál, který nesmí překročit úroveň protokolu pro co shromažďovaných Azure diagnostics.**</span><span class="sxs-lookup"><span data-stu-id="db173-145">**The log level for a channel cannot exceed the log level for what is being collected by Azure diagnostics.**</span></span> <span data-ttu-id="db173-146">Například nelze shromažďovat chyby v protokolu aplikace v elementu protokoly a odešlete podrobné protokoly aplikace přehledy jímky.</span><span class="sxs-lookup"><span data-stu-id="db173-146">For example, you cannot collect Application Log errors in the Logs element and try to send Verbose logs to the Application Insight sink.</span></span> <span data-ttu-id="db173-147">*ScheduledTransferLogLevelFilter* atributu musí vždy shromažďování rovna nebo víc protokolů, než protokoly chcete poslat jímky.</span><span class="sxs-lookup"><span data-stu-id="db173-147">The *scheduledTransferLogLevelFilter* attribute must always collect equal or more logs than the logs you are trying to send to a sink.</span></span>
- <span data-ttu-id="db173-148">**Nelze odeslat data objektu blob shromážděné rozšíření Azure diagnostics Application Insights.**</span><span class="sxs-lookup"><span data-stu-id="db173-148">**You cannot send blob data collected by Azure diagnostics extension to Application Insights.**</span></span> <span data-ttu-id="db173-149">Například nic zadané v položce *adresáře* uzlu.</span><span class="sxs-lookup"><span data-stu-id="db173-149">For example, anything specified under the *Directories* node.</span></span> <span data-ttu-id="db173-150">Pro výpisů stavu systému je odeslána úložiště objektů blob skutečný stav systému a pouze oznámení, že byl vygenerován stav systému jsou odeslána do Application Insights.</span><span class="sxs-lookup"><span data-stu-id="db173-150">For Crash Dumps the actual crash dump is sent to blob storage and only a notification that the crash dump was generated is sent to Application Insights.</span></span>

## <a name="next-steps"></a><span data-ttu-id="db173-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="db173-151">Next Steps</span></span>
* <span data-ttu-id="db173-152">Zjistěte, jak [zobrazit informace o Azure diagnostics](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-cloudservices#view-azure-diagnostic-events) ve službě Application Insights.</span><span class="sxs-lookup"><span data-stu-id="db173-152">Learn how to [view your Azure diagnostics information](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-cloudservices#view-azure-diagnostic-events) in Application Insights.</span></span>
* <span data-ttu-id="db173-153">Použití [prostředí PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md) povolit rozšíření Azure diagnostics pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="db173-153">Use [PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md) to enable the Azure diagnostics extension for your application.</span></span>
* <span data-ttu-id="db173-154">Použití [Visual Studio](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) povolit rozšíření Azure diagnostics pro vaši aplikaci</span><span class="sxs-lookup"><span data-stu-id="db173-154">Use [Visual Studio](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) to enable the Azure diagnostics extension for your application</span></span>
