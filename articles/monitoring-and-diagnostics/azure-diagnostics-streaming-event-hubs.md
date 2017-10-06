---
title: "aaaStreaming dat diagnostiky Azure v hello aktivní trase pomocí služby Event Hubs | Microsoft Docs"
description: "Konfigurace Azure Diagnostics službou Event Hubs ukončení tooend, včetně pokyny pro běžné scénáře."
services: event-hubs
documentationcenter: na
author: rboucher
manager: carmonm
editor: 
ms.assetid: edeebaac-1c47-4b43-9687-f28e7e1e446a
ms.service: monitoring-and-diagnostics
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/13/2017
ms.author: robb
ms.openlocfilehash: a2528ddd0688d1c23a8631e769ca016dd79e4159
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="streaming-azure-diagnostics-data-in-hello-hot-path-by-using-event-hubs"></a>Azure Diagnostics dat v hello aktivní trase pomocí služby Event Hubs
Azure Diagnostics poskytuje flexibilní způsoby toocollect metriky a protokoly z cloudové služby virtuálních počítačů (VM) a přenos výsledky tooAzure úložiště. Počínaje hello března 2016 (SDK 2.9) časový rámec, můžete odesílat diagnostiky toocustom zdroje dat a přenos dat aktivní trase v sekundách pomocí [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/).

Podporované datové typy patří:

* Události Trasování událostí pro Windows
* Čítače výkonu
* Protokoly událostí systému Windows
* Protokoly aplikací
* Protokoly infrastruktury Azure Diagnostics

Tento článek ukazuje, jak Azure Diagnostics tooconfigure službou Event Hubs z ukončení tooend. Příručka je taky pokyny pro následující běžné scénáře hello:

* Jak toocustomize hello protokoly a metriky, získat odeslaný tooEvent rozbočovače
* Jak toochange konfigurace v každé prostředí
* Jak tooview Event Hubs Streamovat data
* Jak tootroubleshoot hello připojení  

## <a name="prerequisites"></a>Požadavky
Event Hubs receieving dat z Azure Diagnostics je podporována v cloudové služby, virtuální počítače, sady škálování virtuálního počítače a počínaje hello sadu Azure SDK 2.9 a hello odpovídající nástroje Azure pro sadu Visual Studio Service Fabric.

* Rozšíření diagnostiky Azure 1.6 ([Azure SDK pro .NET 2.9 nebo novější](https://azure.microsoft.com/downloads/) cílem to ve výchozím nastavení)
* [Visual Studio 2013 nebo novější](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* Stávající konfigurace Azure Diagnostics v aplikaci pomocí *.wadcfgx* souboru a jeden z následujících metod hello:
  * Visual Studio: [konfigurace diagnostiky pro cloudové služby Azure a virtuální počítače](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)
  * Prostředí Windows PowerShell: [zapněte diagnostiku ve službě Azure Cloud Services pomocí prostředí PowerShell](../cloud-services/cloud-services-diagnostics-powershell.md)
* Obor názvů centra událostí zřídit na článek hello [Začínáme se službou Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

## <a name="connect-azure-diagnostics-tooevent-hubs-sink"></a>Připojení Azure Diagnostics tooEvent centra jímka
Ve výchozím nastavení odešle Azure Diagnostics vždy protokoly a metriky tooan účet úložiště Azure. Aplikace může také odeslat data tooEvent centra přidáním nové **jímky** oddílu v části hello **PublicConfig** / **WadCfg** element hello *.wadcfgx* souboru. V sadě Visual Studio, hello *.wadcfgx* soubor je uložen v hello následující cestu: **projekt cloudové služby** > **role** > **() RoleName)** > **diagnostics.wadcfgx** souboru.

```xml
<SinksConfig>
  <Sink name="HotPath">
    <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" />
  </Sink>
</SinksConfig>
```
```JSON
"SinksConfig": {
    "Sink": [
        {
            "name": "HotPath",
            "EventHub": {
                "Url": "https://diags-mycompany-ns.servicebus.windows.net/diageventhub",
                "SharedAccessKeyName": "SendRule"
            }
        }
    ]
}
```

V tomto příkladu centra událostí hello nastavení adresy URL toohello plně kvalifikovaných názvů centra událostí hello: obor názvů služby Event Hubs + "/" + název centra událostí.  

Adresa URL se zobrazí v hello centra událostí Hello [portál Azure](http://go.microsoft.com/fwlink/?LinkID=213885) na řídicím panelu služby Event Hubs hello.  

Hello **jímky** název lze nastavit tooany platný řetězec, dokud hello stejnou hodnotu se používá konzistentně napříč hello konfiguračního souboru.

> [!NOTE]
> Mohou existovat další jímky, jako například *applicationInsights* nakonfigurované v této části. Azure Diagnostics umožňuje jeden nebo více jímky toobe definována, pokud se v hello je také deklarováno každý podřízený **PrivateConfig** části.  
>
>

Hello Event Hubs podřízený musí také být deklarován a definované v hello **PrivateConfig** části hello *.wadcfgx* konfiguračního souboru.

```XML
<PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <StorageAccount name="{account name}" key="{account key}" endpoint="{optional storage endpoint}" />
  <EventHub Url="https://diags-mycompany-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="{base64 encoded key}" />
</PrivateConfig>
```
```JSON
{
    "storageAccountName": "{account name}",
    "storageAccountKey": "{account key}",
    "storageAccountEndPoint": "{optional storage endpoint}",
    "EventHub": {
        "Url": "https://diags-mycompany-ns.servicebus.windows.net/diageventhub",
        "SharedAccessKeyName": "SendRule",
        "SharedAccessKey": "{base64 encoded key}"
    }
}
```

Hello `SharedAccessKeyName` hodnota musí odpovídat klíč sdíleného přístupového podpisu (SAS) a zásad, která byla definována v hello **Event Hubs** oboru názvů. Procházet toohello Event Hubs řídicího panelu hello [portál Azure](https://manage.windowsazure.com), klikněte na tlačítko hello **konfigurace** kartě a nastavte s názvem zásady (například "SendRule"), který má *odeslat* oprávnění. Hello **StorageAccount** také deklarovaného v **PrivateConfig**. Není nutné toochange hodnoty zde Pokud pracují. V tomto příkladu jsme ponechte hello hodnoty prázdná, což je znaménkem podřízené asset nastaví hello hodnoty. Například hello *ServiceConfiguration.Cloud.cscfg* prostředí konfigurační soubor nastaví hello příslušné prostředí názvů a klíče.  

> [!WARNING]
> klíč SAS centra událostí Hello je uložen v prostém textu v hello *.wadcfgx* souboru. Často tento klíč se změnami kódu toosource nebo není k dispozici jako prostředek na vašem serveru sestavení, takže byste měli chránit podle potřeby. Doporučujeme používat klíč SAS se zde *odesílat pouze* oprávnění tak, aby uživatel se zlými úmysly nelze zapsat toohello centra událostí, ale naslouchání tooit nebo k její správě.
>
>

## <a name="configure-azure-diagnostics-toosend-logs-and-metrics-tooevent-hubs"></a>Konfigurace Azure Diagnostics toosend protokoly a metriky tooEvent rozbočovače
Jak je popsáno dříve, všechny výchozí a vlastní diagnostická data, to znamená, metriky a protokoly, je automaticky odeslán tooAzure úložiště v intervalech hello nakonfigurované. Event Hubs a všechny další podřízený můžete zadat libovolný uzel kořenovou nebo listu v toobe hierarchie hello odeslané toohello centra událostí. To zahrnuje události trasování událostí, čítače výkonu, protokoly událostí systému Windows a protokolů aplikace.   

Je důležité tooconsider, kolik datových bodů ve skutečnosti by měla být přenesena tooEvent rozbočovače. Vývojáři obvykle přenášet data za provozu cesty s nízkou latencí, která musí využívat a rychle interpretovat. Systémy, které monitorují výstrahy nebo pravidel automatického škálování jsou příklady. Vývojář může také nakonfigurovat úložišti alternativní analýzy nebo hledání úložiště – například Azure Stream Analytics, Elasticsearch, vlastní monitorování systému nebo oblíbených monitorování systému od jiných uživatelů.

Hello Následují některé příklad konfigurace.

```xml
<PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
</PerformanceCounters>
```
```JSON
"PerformanceCounters": {
    "scheduledTransferPeriod": "PT1M",
    "sinks": "HotPath",
    "PerformanceCounterConfiguration": [
        {
            "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Memory\\Available MBytes",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
            "sampleRate": "PT3M"
        }
    ]
}
```

V hello výše příklad, je podřízený hello nadřazené použité toohello **čítače výkonu** uzlu v hierarchii hello, takže všechny podřízené **čítače výkonu** odešle tooEvent rozbočovače.  

```xml
<PerformanceCounters scheduledTransferPeriod="PT1M">
  <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" sinks="HotPath" />
  <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" sinks="HotPath"/>
  <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" sinks="HotPath"/>
</PerformanceCounters>
```
```JSON
"PerformanceCounters": {
    "scheduledTransferPeriod": "PT1M",
    "PerformanceCounterConfiguration": [
        {
            "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
            "sampleRate": "PT3M",
            "sinks": "HotPath"
        },
        {
            "counterSpecifier": "\\Memory\\Available MBytes",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
            "sampleRate": "PT3M"
        },
        {
            "counterSpecifier": "\\ASP.NET\\Requests Rejected",
            "sampleRate": "PT3M",
            "sinks": "HotPath"
        },
        {
            "counterSpecifier": "\\ASP.NET\\Requests Queued",
            "sampleRate": "PT3M",
            "sinks": "HotPath"
        }
    ]
}
```

V předchozím příkladu hello podřízený hello je použité tooonly tři čítače: **požadavky ve frontě**, **požadavky zamítnuty**, a **% času procesoru**.  

Hello následující příklad ukazuje, jak může vývojář omezit hello množství odeslaná data toobe hello kritické metriky, které se používají pro tuto službu stavu.  

```XML
<Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
```
```JSON
"Logs": {
    "scheduledTransferPeriod": "PT1M",
    "scheduledTransferLogLevelFilter": "Error",
    "sinks": "HotPath"
}
```

V tomto příkladu podřízený hello je použité toologs a je filtrovaná pouze tooerror úrovně trasování.

## <a name="deploy-and-update-a-cloud-services-application-and-diagnostics-config"></a>Nasazení a aktualizace konfigurace aplikace a Diagnostika cloudové služby
Visual Studio poskytuje hello nejjednodušší cesta toodeploy hello aplikace a služby Event Hubs podřízený konfigurace. tooview a úpravy hello souboru, otevřete hello *.wadcfgx* souborů v sadě Visual Studio, upravovat a uložte ho. Cesta Hello je **projekt cloudové služby** > **role** > **(RoleName)** > **diagnostics.wadcfgx**.  

V tomto okamžiku všechny nasazení a nasazení aktualizací akcí v sadě Visual Studio, Visual Studio Team System a všechny příkazy nebo skripty, které jsou založeny na MSBuild a používat hello **/t: publikování** cíl zahrnují hello *.wadcfgx*  v procesu balení hello. Kromě toho nasazení a aktualizace nasadit hello souboru tooAzure pomocí hello odpovídající Azure Diagnostics agenta rozšíření na virtuální počítače.

Po nasazení aplikace hello a konfigurace Azure Diagnostics, zobrazí se okamžitě aktivity na řídicím panelu hello hello centra událostí. To znamená, že jste připravené toomove na tooviewing hello horkou cesta dat v nástroji klienta nebo analysis naslouchací proces hello podle svého výběru.  

V hello následující obrázek hello Event Hubs řídicího panelu ukazuje pořádku odesílání diagnostiky dat toohello události rozbočovače spuštění nějakou dobu zopakovat po 23: 00. Kdy je nasazená aplikace hello s aktualizované *.wadcfgx* souborů a hello podřízený byla nakonfigurována správně.

![][0]  

> [!NOTE]
> Pokud provedete aktualizace toohello Azure Diagnostics konfigurační soubor (.wadcfgx), se doporučuje push hello aktualizace toohello celá aplikace, jakož i konfigurace hello pomocí sady Visual Studio publikování nebo skript prostředí Windows PowerShell.  
>
>

## <a name="view-hot-path-data"></a>Data za provozu cesty zobrazení
Jak je popsáno dříve, existuje mnoho případů použití pro naslouchání tooand zpracování dat služby Event Hubs.

Jeden ze způsobů jednoduché je toocreate testu malých konzole aplikace toolisten toohello centra událostí a tisku hello výstupního datového proudu. Můžete umístit hello následující kód, který je vysvětlené podrobněji v [Začínáme se službou Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)), v konzolové aplikaci.  

Všimněte si, že hello konzolové aplikace musí obsahovat hello [balíček NuGet hostitele procesor událostí](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost/).  

Mějte na paměti, tooreplace hello hodnoty v lomených závorkách v hello **hlavní** funkce s hodnotami pro vaše prostředky.   

```csharp
//Console application code for EventHub test client
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace EventHubListener
{
    class SimpleEventProcessor : IEventProcessor
    {
        Stopwatch checkpointStopWatch;

        async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
            if (reason == CloseReason.Shutdown)
            {
                await context.CheckpointAsync();
            }
        }

        Task IEventProcessor.OpenAsync(PartitionContext context)
        {
            Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
            this.checkpointStopWatch = new Stopwatch();
            this.checkpointStopWatch.Start();
            return Task.FromResult<object>(null);
        }

        async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (EventData eventData in messages)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                    Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                        context.Lease.PartitionId, data));

                foreach (var x in eventData.Properties)
                {
                    Console.WriteLine(string.Format("    {0} = {1}", x.Key, x.Value));
                }
            }

            //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
            if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
            {
                await context.CheckpointAsync();
                this.checkpointStopWatch.Restart();
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string eventHubConnectionString = "Endpoint= <your connection string>”
            string eventHubName = "<Event hub name>";
            string storageAccountName = "<Storage account name>";
            string storageAccountKey = "<Storage account key>”;
            string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);

            string eventProcessorHostName = Guid.NewGuid().ToString();
            EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
            Console.WriteLine("Registering EventProcessor...");
            var options = new EventProcessorOptions();
            options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
            eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();

            Console.WriteLine("Receiving. Press enter key toostop worker.");
            Console.ReadLine();
            eventProcessorHost.UnregisterEventProcessorAsync().Wait();
        }
    }
}
```

## <a name="troubleshoot-event-hubs-sinks"></a>Řešení potíží s jímky služby Event Hubs
* centra událostí Hello nezobrazuje aktivity příchozích nebo odchozích události podle očekávání.

    Zkontrolujte, že je úspěšně zřízený Centrum událostí. Všechny informace o připojení v hello **PrivateConfig** části *.wadcfgx* musí odpovídat hello hodnoty prostředku, jak je vidět na portálu hello. Ujistěte se, že máte SAS zásady definované ("SendRule" v příkladu hello) v hello portál a které *odeslat* je povoleno.  
* Po aktualizaci centra událostí hello přestane zobrazovat aktivity příchozích nebo odchozích události.

    Zkontrolujte, že hello centra událostí a informace o konfiguraci jsou správné, jak je popsáno dříve. Někdy hello **PrivateConfig** v aktualizaci nasazení se resetuje. Hello doporučený, opravte je toomake všechny změny příliš*.wadcfgx* v hello projektu a pak poslat aktualizaci dokončení aplikace. Pokud to není možné, ujistěte se, že aktualizace diagnostiky hello nabízených oznámení úplná **PrivateConfig** který obsahuje klíč SAS hello.  
* Byl proveden o hello návrhy a hello centra událostí, ale stále nefunguje.

    Podívejte se do v hello Azure Storage tabulku, která obsahuje chyby a protokolování Azure Diagnostics samotné: **WADDiagnosticInfrastructureLogsTable**. Jednou z možností nástroje, jako je toouse [Azure Storage Explorer](http://www.storageexplorer.com) účet úložiště toothis tooconnect, zobrazit tuto tabulku a přidat dotaz pro časové razítko v hello posledních 24 hodin. Můžete použít nástroj tooexport hello soubor .csv a otevřete jej v aplikaci, jako je například aplikace Microsoft Excel. Aplikace Excel umožňuje snadno toosearch pro volací karty řetězce, jako například **EventHubs**, toosee, jaké se chybová zpráva.  

## <a name="next-steps"></a>Další kroky
• [Další informace o službě Event Hubs](https://azure.microsoft.com/services/event-hubs/)

## <a name="appendix-complete-azure-diagnostics-configuration-file-wadcfgx-example"></a>Dodatek: Dokončení příkladu Azure Diagnostics konfigurační soubor (.wadcfgx)
```xml
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticsConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
  <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <WadCfg>
      <DiagnosticMonitorConfiguration overallQuotaInMB="4096" sinks="applicationInsights.errors">
        <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter="Error" />
        <Directories scheduledTransferPeriod="PT1M">
          <IISLogs containerName="wad-iis-logfiles" />
          <FailedRequestLogs containerName="wad-failedrequestlogs" />
        </Directories>
        <PerformanceCounters scheduledTransferPeriod="PT1M" sinks="HotPath">
          <PerformanceCounterConfiguration counterSpecifier="\Memory\Available MBytes" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\ISAPI Extension Requests/sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Web Service(_Total)\Bytes Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Requests/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET Applications(__Total__)\Errors Total/Sec" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Queued" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\ASP.NET\Requests Rejected" sampleRate="PT3M" />
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT3M" />
        </PerformanceCounters>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
          <DataSource name="Application!*" />
        </WindowsEventLog>
        <CrashDumps>
          <CrashDumpConfiguration processName="WaIISHost.exe" />
          <CrashDumpConfiguration processName="WaWorkerHost.exe" />
          <CrashDumpConfiguration processName="w3wp.exe" />
        </CrashDumps>
        <Logs scheduledTransferPeriod="PT1M" sinks="HotPath" scheduledTransferLogLevelFilter="Error" />
      </DiagnosticMonitorConfiguration>
      <SinksConfig>
        <Sink name="HotPath">
          <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" />
        </Sink>
        <Sink name="applicationInsights">
          <ApplicationInsights />
          <Channels>
            <Channel logLevel="Error" name="errors" />
          </Channels>
        </Sink>
      </SinksConfig>
    </WadCfg>
    <StorageAccount>ACCOUNT_NAME</StorageAccount>
  </PublicConfig>
  <PrivateConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <StorageAccount name="{account name}" key="{account key}" endpoint="{storage endpoint}" />
    <EventHub Url="https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py" SharedAccessKeyName="SendRule" SharedAccessKey="YOUR_KEY_HERE" />
  </PrivateConfig>
  <IsEnabled>true</IsEnabled>
</DiagnosticsConfiguration>
```

Hello doplňkové *ServiceConfiguration.Cloud.cscfg* pro tento příklad vypadá jako následující hello.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="MyFixItCloudService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2015-04.2.6">
  <Role name="MyFixIt.WorkerRole">
    <Instances count="1" />
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="YOUR_CONNECTION_STRING" />
    </ConfigurationSettings>
  </Role>
</ServiceConfiguration>
```

Ekvivalentní Json na základě nastavení pro virtuální počítače je následující:
```JSON
"settings": {
    "WadCfg": {
        "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": 4096,
            "sinks": "applicationInsights.errors",
            "DiagnosticInfrastructureLogs": {
                "scheduledTransferLogLevelFilter": "Error"
            },
            "Directories": {
                "scheduledTransferPeriod": "PT1M",
                "IISLogs": {
                    "containerName": "wad-iis-logfiles"
                },
                "FailedRequestLogs": {
                    "containerName": "wad-failedrequestlogs"
                }
            },
            "PerformanceCounters": {
                "scheduledTransferPeriod": "PT1M",
                "sinks": "HotPath",
                "PerformanceCounterConfiguration": [
                    {
                        "counterSpecifier": "\\Memory\\Available MBytes",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\Web Service(_Total)\\ISAPI Extension Requests/sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\Web Service(_Total)\\Bytes Total/Sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Requests/Sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET Applications(__Total__)\\Errors Total/Sec",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET\\Requests Queued",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\ASP.NET\\Requests Rejected",
                        "sampleRate": "PT3M"
                    },
                    {
                        "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
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
                "scheduledTransferLogLevelFilter": "Error",
                "sinks": "HotPath"
            }
        },
        "SinksConfig": {
            "Sink": [
                {
                    "name": "HotPath",
                    "EventHub": {
                        "Url": "https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py",
                        "SharedAccessKeyName": "SendRule"
                    }
                },
                {
                    "name": "applicationInsights",
                    "ApplicationInsights": "",
                    "Channels": {
                        "Channel": [
                            {
                                "logLevel": "Error",
                                "name": "errors"
                            }
                        ]
                    }
                }
            ]
        }
    },
    "StorageAccount": "{account name}"
}


"protectedSettings": {
    "storageAccountName": "{account name}",
    "storageAccountKey": "{account key}",
    "storageAccountEndPoint": "{storage endpoint}",
    "EventHub": {
        "Url": "https://diageventhub-py-ns.servicebus.windows.net/diageventhub-py",
        "SharedAccessKeyName": "SendRule",
        "SharedAccessKey": "YOUR_KEY_HERE"
    }
}
```

## <a name="next-steps"></a>Další kroky
Další informace o službě Event Hubs návštěvou hello následující odkazy:

* [Přehled služby Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)
* [Vytvoření centra událostí](../event-hubs/event-hubs-create.md)
* [Nejčastější dotazy k Event Hubs](../event-hubs/event-hubs-faq.md)

<!-- Images. -->
[0]: ../event-hubs/media/event-hubs-streaming-azure-diags-data/dashboard.png
