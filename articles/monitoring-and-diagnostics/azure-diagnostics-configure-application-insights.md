---
title: aaaConfigure Azure Diagnostics toosend data tooApplication Insights | Microsoft Docs
description: "Aktualizujte hello Azure Diagnostics veřejné konfigurace toosend data tooApplication statistiky."
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
ms.openlocfilehash: 7c36f29da8fdc12fa58c17458348a311b900b0f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="send-cloud-service-virtual-machine-or-service-fabric-diagnostic-data-tooapplication-insights"></a><span data-ttu-id="83a87-103">Odeslání diagnostických dat tooApplication Cloudová služba, virtuální počítač nebo Service Fabric Statistika</span><span class="sxs-lookup"><span data-stu-id="83a87-103">Send Cloud Service, Virtual Machine, or Service Fabric diagnostic data tooApplication Insights</span></span>
<span data-ttu-id="83a87-104">Cloudové služby, virtuální počítače sady škálování virtuálního počítače a Service Fabric všechny hello Azure Diagnostics rozšíření toocollect data použít.</span><span class="sxs-lookup"><span data-stu-id="83a87-104">Cloud services, Virtual Machines, Virtual Machine Scale Sets and Service Fabric all use hello Azure Diagnostics extension toocollect data.</span></span>  <span data-ttu-id="83a87-105">Azure diagnostics odešle data tooAzure úložiště tabulek.</span><span class="sxs-lookup"><span data-stu-id="83a87-105">Azure diagnostics sends data tooAzure Storage tables.</span></span>  <span data-ttu-id="83a87-106">Můžete však také kanálu všechny nebo jen některé hello dat tooother umístění pomocí rozšíření Azure Diagnostics 1.5 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="83a87-106">However, you can also pipe all or a subset of hello data tooother locations using Azure Diagnostics extension 1.5 or later.</span></span>

<span data-ttu-id="83a87-107">Tento článek popisuje, jak toosend data z hello tooApplication rozšíření Azure Diagnostics statistiky.</span><span class="sxs-lookup"><span data-stu-id="83a87-107">This article describes how toosend data from hello Azure Diagnostics extension tooApplication Insights.</span></span>

## <a name="diagnostics-configuration-explained"></a><span data-ttu-id="83a87-108">Vysvětlení konfigurace diagnostiky</span><span class="sxs-lookup"><span data-stu-id="83a87-108">Diagnostics configuration explained</span></span>
<span data-ttu-id="83a87-109">jímky rozšíření 1.5 zavedená Azure diagnostics Hello, které jsou dalších místech, kde můžete poslat diagnostická data.</span><span class="sxs-lookup"><span data-stu-id="83a87-109">hello Azure diagnostics extension 1.5 introduced sinks, which are additional locations where you can send diagnostic data.</span></span>

<span data-ttu-id="83a87-110">Příklad konfigurace jímka pro službu Application Insights:</span><span class="sxs-lookup"><span data-stu-id="83a87-110">Example configuration of a sink for Application Insights:</span></span>

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
- <span data-ttu-id="83a87-111">Hello **podřízený** *název* atribut je řetězcovou hodnotu, která jednoznačně identifikuje hello jímky.</span><span class="sxs-lookup"><span data-stu-id="83a87-111">hello **Sink** *name* attribute is a string value that uniquely identifies hello sink.</span></span>

- <span data-ttu-id="83a87-112">Hello **ApplicationInsights** element Určuje klíč instrumentace hello prostředek Application insights kam je odeslána hello Azure diagnostická data.</span><span class="sxs-lookup"><span data-stu-id="83a87-112">hello **ApplicationInsights** element specifies instrumentation key of hello Application insights resource where hello Azure diagnostics data is sent.</span></span>
    - <span data-ttu-id="83a87-113">Pokud nemáte existující prostředek Application Insights, přečtěte si téma [vytvořte nový prostředek Application Insights](../application-insights/app-insights-create-new-resource.md) Další informace o vytvoření prostředku a získání klíč instrumentace hello.</span><span class="sxs-lookup"><span data-stu-id="83a87-113">If you don't have an existing Application Insights resource, see [Create a new Application Insights resource](../application-insights/app-insights-create-new-resource.md) for more information on creating a resource and getting hello instrumentation key.</span></span>
    - <span data-ttu-id="83a87-114">Pokud vyvíjíte cloudové služby s Azure SDK 2.8 nebo novější, tento klíč instrumentace se automaticky vyplní.</span><span class="sxs-lookup"><span data-stu-id="83a87-114">If you are developing a Cloud Service with Azure SDK 2.8 and later, this instrumentation key is automatically populated.</span></span> <span data-ttu-id="83a87-115">Hodnota Hello je založená na hello **APPINSIGHTS_INSTRUMENTATIONKEY** nastavení konfigurace služby, když balení hello projekt cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="83a87-115">hello value is based on hello **APPINSIGHTS_INSTRUMENTATIONKEY** service configuration setting when packaging hello Cloud Service project.</span></span> <span data-ttu-id="83a87-116">V tématu [problémy cloudové služby Application Insights použití s Azure Diagnostics tootroubleshoot](../cloud-services/cloud-services-dotnet-diagnostics-applicationinsights.md).</span><span class="sxs-lookup"><span data-stu-id="83a87-116">See [Use Application Insights with Azure Diagnostics tootroubleshoot Cloud Service issues](../cloud-services/cloud-services-dotnet-diagnostics-applicationinsights.md).</span></span>

- <span data-ttu-id="83a87-117">Hello **kanály** element obsahuje jeden nebo více **kanál** elementy.</span><span class="sxs-lookup"><span data-stu-id="83a87-117">hello **Channels** element contains one or more **Channel** elements.</span></span>
    - <span data-ttu-id="83a87-118">Hello *název* atribut jednoznačně odkazuje toothat kanál.</span><span class="sxs-lookup"><span data-stu-id="83a87-118">hello *name* attribute uniquely refers toothat channel.</span></span>
    - <span data-ttu-id="83a87-119">Hello *loglevel* atributů umožňují určit umožňuje hello úroveň protokolu, který hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="83a87-119">hello *loglevel* attribute lets you specify hello log level that hello channel allows.</span></span> <span data-ttu-id="83a87-120">jsou k dispozici protokolu úrovně Hello v pořadí podle většina tooleast informace:</span><span class="sxs-lookup"><span data-stu-id="83a87-120">hello available log levels in order of most tooleast information are:</span></span>
        - <span data-ttu-id="83a87-121">Verbose</span><span class="sxs-lookup"><span data-stu-id="83a87-121">Verbose</span></span>
        - <span data-ttu-id="83a87-122">Informace</span><span class="sxs-lookup"><span data-stu-id="83a87-122">Information</span></span>
        - <span data-ttu-id="83a87-123">Upozornění</span><span class="sxs-lookup"><span data-stu-id="83a87-123">Warning</span></span>
        - <span data-ttu-id="83a87-124">Chyba</span><span class="sxs-lookup"><span data-stu-id="83a87-124">Error</span></span>
        - <span data-ttu-id="83a87-125">Kritické</span><span class="sxs-lookup"><span data-stu-id="83a87-125">Critical</span></span>

<span data-ttu-id="83a87-126">Kanál, který funguje jako filtr a umožní vám tooselect konkrétní protokolu úrovně toosend toohello cíl jímky.</span><span class="sxs-lookup"><span data-stu-id="83a87-126">A channel acts like a filter and allows you tooselect specific log levels toosend toohello target sink.</span></span> <span data-ttu-id="83a87-127">Například může shromáždit podrobné protokoly a jejich odeslání toostorage ale odeslat pouze podřízený toohello chyby.</span><span class="sxs-lookup"><span data-stu-id="83a87-127">For example, you could collect verbose logs and send them toostorage, but send only Errors toohello sink.</span></span>

<span data-ttu-id="83a87-128">Hello následující obrázek znázorňuje tuto relaci.</span><span class="sxs-lookup"><span data-stu-id="83a87-128">hello following graphic shows this relationship.</span></span>

![Veřejná konfigurace diagnostiky](./media/azure-diagnostics-configure-applicationinsights/AzDiag_Channels_App_Insights.png)

<span data-ttu-id="83a87-130">Následující obrázek Hello shrnuje hello hodnoty konfigurace a funkce.</span><span class="sxs-lookup"><span data-stu-id="83a87-130">hello following graphic summarizes hello configuration values and how they work.</span></span> <span data-ttu-id="83a87-131">Můžete zahrnout více jímky hello konfigurace na různých úrovních v hierarchii hello.</span><span class="sxs-lookup"><span data-stu-id="83a87-131">You can include multiple sinks in hello configuration at different levels in hello hierarchy.</span></span> <span data-ttu-id="83a87-132">podřízený Hello na nejvyšší úrovni hello funguje jako globální nastavení a hello jeden zadaný v jednotlivých hello, že element se chová jako přepsání toothat globální nastavení.</span><span class="sxs-lookup"><span data-stu-id="83a87-132">hello sink at hello top level acts as a global setting and hello one specified at hello individual element acts like an override toothat global setting.</span></span>

![Diagnostika jímky konfigurace pomocí nástroje Application Insights](./media/azure-diagnostics-configure-applicationinsights/Azure_Diagnostics_Sinks.png)

## <a name="complete-sink-configuration-example"></a><span data-ttu-id="83a87-134">Příklad konfigurace dokončení jímka</span><span class="sxs-lookup"><span data-stu-id="83a87-134">Complete sink configuration example</span></span>
<span data-ttu-id="83a87-135">Tady je kompletní příklad, jak veřejné konfigurace hello souboru, který</span><span class="sxs-lookup"><span data-stu-id="83a87-135">Here is a complete example of hello public configuration file that</span></span>
1. <span data-ttu-id="83a87-136">odešle všechny chyby tooApplication přehledy (zadaný v hello **DiagnosticMonitorConfiguration** uzlu)</span><span class="sxs-lookup"><span data-stu-id="83a87-136">sends all errors tooApplication Insights (specified at hello **DiagnosticMonitorConfiguration** node)</span></span>
2. <span data-ttu-id="83a87-137">rovněž odesílá podrobné úrovni protokoly pro hello protokoly aplikací (zadaný v hello **protokoly** uzlu).</span><span class="sxs-lookup"><span data-stu-id="83a87-137">also sends Verbose level logs for hello Application Logs (specified at hello **Logs** node).</span></span>

```XML
<WadCfg>
  <DiagnosticMonitorConfiguration overallQuotaInMB="4096"
       sinks="ApplicationInsights.MyTopDiagData"> <!-- All info below sent toothis channel -->
    <DiagnosticInfrastructureLogs />
    <PerformanceCounters>
      <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
      <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
    </PerformanceCounters>
    <WindowsEventLog scheduledTransferPeriod="PT1M">
      <DataSource name="Application!*" />
    </WindowsEventLog>
    <Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Verbose"
            sinks="ApplicationInsights.MyLogData"/> <!-- This specific info sent toothis channel -->
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
        "sinks": "ApplicationInsights.MyTopDiagData", "_comment": "All info below sent toothis channel",
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
            "sinks": "ApplicationInsights.MyLogData", "_comment": "This specific info sent toothis channel"
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
<span data-ttu-id="83a87-138">V předchozí konfiguraci hello hello následující řádky mají následující významy hello:</span><span class="sxs-lookup"><span data-stu-id="83a87-138">In hello previous configuration, hello following lines have hello following meanings:</span></span>

### <a name="send-all-hello-data-that-is-being-collected-by-azure-diagnostics"></a><span data-ttu-id="83a87-139">Odeslat veškerá data hello, které jsou shromažďována Azure diagnostics</span><span class="sxs-lookup"><span data-stu-id="83a87-139">Send all hello data that is being collected by Azure diagnostics</span></span>

```XML
<DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights">
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights",
}
```

### <a name="send-only-error-logs-toohello-application-insights-sink"></a><span data-ttu-id="83a87-140">Odeslat pouze chyby protokoly toohello Application Insights jímka</span><span class="sxs-lookup"><span data-stu-id="83a87-140">Send only error logs toohello Application Insights sink</span></span>

```XML
<DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights.MyTopDiagdata">
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights.MyTopDiagData",
}
```

### <a name="send-verbose-application-logs-tooapplication-insights"></a><span data-ttu-id="83a87-141">Odesílání aplikací podrobné protokoly tooApplication statistiky</span><span class="sxs-lookup"><span data-stu-id="83a87-141">Send Verbose application logs tooApplication Insights</span></span>

```XML
<Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Verbose" sinks="ApplicationInsights.MyLogData"/>
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights.MyLogData",
}
```

## <a name="limitations"></a><span data-ttu-id="83a87-142">Omezení</span><span class="sxs-lookup"><span data-stu-id="83a87-142">Limitations</span></span>

- <span data-ttu-id="83a87-143">**Kanály protokolu pouze typ a není výkonu čítače.**</span><span class="sxs-lookup"><span data-stu-id="83a87-143">**Channels only log type and not performance counters.**</span></span> <span data-ttu-id="83a87-144">Pokud zadáte kanál s elementem čítače výkonu, je ignorován.</span><span class="sxs-lookup"><span data-stu-id="83a87-144">If you specify a channel with a performance counter element, it is ignored.</span></span>
- <span data-ttu-id="83a87-145">**úroveň protokolu Hello pro kanál, který nesmí překročit hello úroveň protokolu pro co shromažďovaných Azure diagnostics.**</span><span class="sxs-lookup"><span data-stu-id="83a87-145">**hello log level for a channel cannot exceed hello log level for what is being collected by Azure diagnostics.**</span></span> <span data-ttu-id="83a87-146">Nelze například shromažďovat chyby v protokolu aplikace v elementu hello protokoly a akci toosend podrobné protokoly toohello přehled aplikace jímky.</span><span class="sxs-lookup"><span data-stu-id="83a87-146">For example, you cannot collect Application Log errors in hello Logs element and try toosend Verbose logs toohello Application Insight sink.</span></span> <span data-ttu-id="83a87-147">Hello *scheduledTransferLogLevelFilter* atributu musí vždy shromažďování rovna nebo další protokoly než hello protokoly, které se pokoušíte toosend tooa jímky.</span><span class="sxs-lookup"><span data-stu-id="83a87-147">hello *scheduledTransferLogLevelFilter* attribute must always collect equal or more logs than hello logs you are trying toosend tooa sink.</span></span>
- <span data-ttu-id="83a87-148">**Nelze odeslat blob data shromažďovaná společností tooApplication rozšíření Azure diagnostics statistiky.**</span><span class="sxs-lookup"><span data-stu-id="83a87-148">**You cannot send blob data collected by Azure diagnostics extension tooApplication Insights.**</span></span> <span data-ttu-id="83a87-149">Například nic zadané v položce hello *adresáře* uzlu.</span><span class="sxs-lookup"><span data-stu-id="83a87-149">For example, anything specified under hello *Directories* node.</span></span> <span data-ttu-id="83a87-150">Pro výpisů stavu systému hello skutečný stav systému je odeslán tooblob úložiště a byl vygenerován pouze oznámení, že hello stav systému se odesílají tooApplication statistiky.</span><span class="sxs-lookup"><span data-stu-id="83a87-150">For Crash Dumps hello actual crash dump is sent tooblob storage and only a notification that hello crash dump was generated is sent tooApplication Insights.</span></span>

## <a name="next-steps"></a><span data-ttu-id="83a87-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="83a87-151">Next Steps</span></span>
* <span data-ttu-id="83a87-152">Zjistěte, jak příliš[zobrazit informace o Azure diagnostics](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-cloudservices#view-azure-diagnostic-events) ve službě Application Insights.</span><span class="sxs-lookup"><span data-stu-id="83a87-152">Learn how too[view your Azure diagnostics information](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-cloudservices#view-azure-diagnostic-events) in Application Insights.</span></span>
* <span data-ttu-id="83a87-153">Použití [prostředí PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md) tooenable hello rozšíření Azure diagnostiky pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="83a87-153">Use [PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md) tooenable hello Azure diagnostics extension for your application.</span></span>
* <span data-ttu-id="83a87-154">Použití [Visual Studio](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) tooenable hello rozšíření Azure diagnostiky pro vaši aplikaci</span><span class="sxs-lookup"><span data-stu-id="83a87-154">Use [Visual Studio](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) tooenable hello Azure diagnostics extension for your application</span></span>
