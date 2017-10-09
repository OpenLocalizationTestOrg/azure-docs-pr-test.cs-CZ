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
# <a name="send-cloud-service-virtual-machine-or-service-fabric-diagnostic-data-tooapplication-insights"></a>Odeslání diagnostických dat tooApplication Cloudová služba, virtuální počítač nebo Service Fabric Statistika
Cloudové služby, virtuální počítače sady škálování virtuálního počítače a Service Fabric všechny hello Azure Diagnostics rozšíření toocollect data použít.  Azure diagnostics odešle data tooAzure úložiště tabulek.  Můžete však také kanálu všechny nebo jen některé hello dat tooother umístění pomocí rozšíření Azure Diagnostics 1.5 nebo novější.

Tento článek popisuje, jak toosend data z hello tooApplication rozšíření Azure Diagnostics statistiky.

## <a name="diagnostics-configuration-explained"></a>Vysvětlení konfigurace diagnostiky
jímky rozšíření 1.5 zavedená Azure diagnostics Hello, které jsou dalších místech, kde můžete poslat diagnostická data.

Příklad konfigurace jímka pro službu Application Insights:

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
- Hello **podřízený** *název* atribut je řetězcovou hodnotu, která jednoznačně identifikuje hello jímky.

- Hello **ApplicationInsights** element Určuje klíč instrumentace hello prostředek Application insights kam je odeslána hello Azure diagnostická data.
    - Pokud nemáte existující prostředek Application Insights, přečtěte si téma [vytvořte nový prostředek Application Insights](../application-insights/app-insights-create-new-resource.md) Další informace o vytvoření prostředku a získání klíč instrumentace hello.
    - Pokud vyvíjíte cloudové služby s Azure SDK 2.8 nebo novější, tento klíč instrumentace se automaticky vyplní. Hodnota Hello je založená na hello **APPINSIGHTS_INSTRUMENTATIONKEY** nastavení konfigurace služby, když balení hello projekt cloudové služby. V tématu [problémy cloudové služby Application Insights použití s Azure Diagnostics tootroubleshoot](../cloud-services/cloud-services-dotnet-diagnostics-applicationinsights.md).

- Hello **kanály** element obsahuje jeden nebo více **kanál** elementy.
    - Hello *název* atribut jednoznačně odkazuje toothat kanál.
    - Hello *loglevel* atributů umožňují určit umožňuje hello úroveň protokolu, který hello kanálu. jsou k dispozici protokolu úrovně Hello v pořadí podle většina tooleast informace:
        - Verbose
        - Informace
        - Upozornění
        - Chyba
        - Kritické

Kanál, který funguje jako filtr a umožní vám tooselect konkrétní protokolu úrovně toosend toohello cíl jímky. Například může shromáždit podrobné protokoly a jejich odeslání toostorage ale odeslat pouze podřízený toohello chyby.

Hello následující obrázek znázorňuje tuto relaci.

![Veřejná konfigurace diagnostiky](./media/azure-diagnostics-configure-applicationinsights/AzDiag_Channels_App_Insights.png)

Následující obrázek Hello shrnuje hello hodnoty konfigurace a funkce. Můžete zahrnout více jímky hello konfigurace na různých úrovních v hierarchii hello. podřízený Hello na nejvyšší úrovni hello funguje jako globální nastavení a hello jeden zadaný v jednotlivých hello, že element se chová jako přepsání toothat globální nastavení.

![Diagnostika jímky konfigurace pomocí nástroje Application Insights](./media/azure-diagnostics-configure-applicationinsights/Azure_Diagnostics_Sinks.png)

## <a name="complete-sink-configuration-example"></a>Příklad konfigurace dokončení jímka
Tady je kompletní příklad, jak veřejné konfigurace hello souboru, který
1. odešle všechny chyby tooApplication přehledy (zadaný v hello **DiagnosticMonitorConfiguration** uzlu)
2. rovněž odesílá podrobné úrovni protokoly pro hello protokoly aplikací (zadaný v hello **protokoly** uzlu).

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
V předchozí konfiguraci hello hello následující řádky mají následující významy hello:

### <a name="send-all-hello-data-that-is-being-collected-by-azure-diagnostics"></a>Odeslat veškerá data hello, které jsou shromažďována Azure diagnostics

```XML
<DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights">
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights",
}
```

### <a name="send-only-error-logs-toohello-application-insights-sink"></a>Odeslat pouze chyby protokoly toohello Application Insights jímka

```XML
<DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="ApplicationInsights.MyTopDiagdata">
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights.MyTopDiagData",
}
```

### <a name="send-verbose-application-logs-tooapplication-insights"></a>Odesílání aplikací podrobné protokoly tooApplication statistiky

```XML
<Logs scheduledTransferPeriod="PT1M" scheduledTransferLogLevelFilter="Verbose" sinks="ApplicationInsights.MyLogData"/>
```
```JSON
"DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "sinks": "ApplicationInsights.MyLogData",
}
```

## <a name="limitations"></a>Omezení

- **Kanály protokolu pouze typ a není výkonu čítače.** Pokud zadáte kanál s elementem čítače výkonu, je ignorován.
- **úroveň protokolu Hello pro kanál, který nesmí překročit hello úroveň protokolu pro co shromažďovaných Azure diagnostics.** Nelze například shromažďovat chyby v protokolu aplikace v elementu hello protokoly a akci toosend podrobné protokoly toohello přehled aplikace jímky. Hello *scheduledTransferLogLevelFilter* atributu musí vždy shromažďování rovna nebo další protokoly než hello protokoly, které se pokoušíte toosend tooa jímky.
- **Nelze odeslat blob data shromažďovaná společností tooApplication rozšíření Azure diagnostics statistiky.** Například nic zadané v položce hello *adresáře* uzlu. Pro výpisů stavu systému hello skutečný stav systému je odeslán tooblob úložiště a byl vygenerován pouze oznámení, že hello stav systému se odesílají tooApplication statistiky.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[zobrazit informace o Azure diagnostics](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-cloudservices#view-azure-diagnostic-events) ve službě Application Insights.
* Použití [prostředí PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md) tooenable hello rozšíření Azure diagnostiky pro vaši aplikaci.
* Použití [Visual Studio](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) tooenable hello rozšíření Azure diagnostiky pro vaši aplikaci
