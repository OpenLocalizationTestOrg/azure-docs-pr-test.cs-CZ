---
title: "schéma 1.3 a novější konfigurace rozšíření diagnostiky aaaAzure | Microsoft Docs"
description: "Verze schématu 1.3 a novější Azure diagnostics dodávána jako součást hello Microsoft Azure SDK 2.4 nebo novější."
services: monitoring-and-diagnostics
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/15/2017
ms.author: robb
ms.openlocfilehash: bd15d3a79ea818fcb3235854717e58d5da36518e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-diagnostics-13-and-later-configuration-schema"></a>Azure Diagnostics 1.3 a novější schéma konfigurace
> [!NOTE]
> Hello rozšíření diagnostiky Azure je, že součást hello používá toocollect čítače výkonu a další statistiky z:
> - Azure Virtual Machines 
> - Škálovací sady virtuálních počítačů
> - Service Fabric 
> - Cloud Services 
> - Network Security Groups (Skupiny zabezpečení sítě)
> 
> Tato stránka je pouze relevantní, pokud používáte některou z těchto služeb.

Tato stránka je platná pro verze 1.3 a novější (Azure SDK 2.4 nebo novější). Novější konfigurační oddíly jsou komentáři tooshow ve verzi, které byly přidány.  

soubor konfigurace Hello zde popsané je použité tooset konfigurace diagnostiky nastavení při monitorování diagnostiky hello spustí.  

rozšíření Hello se používá ve spojení s dalšími produkty Microsoftu diagnostiky jako monitorování Azure, Application Insights a analýzy protokolů.



Stáhněte si definice schématu souboru hello veřejné konfigurace tak, že spustíte následující příkaz prostředí PowerShell hello:  

```powershell  
(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File –Encoding utf8 -FilePath 'C:\temp\WadConfig.xsd'  
```  

Další informace o používání Azure Diagnostics najdete v tématu [rozšíření diagnostiky Azure](azure-diagnostics.md).  

## <a name="example-of-hello-diagnostics-configuration-file"></a>Příklad souboru konfigurace diagnostiky hello  
 Hello následující příklad ukazuje konfigurační soubor typické diagnostics:  

```xml  
<?xml version="1.0" encoding="utf-8"?>  
<DiagnosticsConfiguration  xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">   
  <PublicConfig>  
    <WadCfg>  
      <DiagnosticMonitorConfiguration overallQuotaInMB="10000">  

        <PerformanceCounters scheduledTransferPeriod="PT1M">  
          <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT1M" unit="percent" />  
        </PerformanceCounters>  

        <Directories scheduledTransferPeriod="PT5M">  
          <IISLogs containerName="iislogs" />  
          <FailedRequestLogs containerName="iisfailed" />  

          <DataSources>  
            <DirectoryConfiguration containerName="mynewprocess">  
              <Absolute path="C:\MyNewProcess" expandEnvironment="false" />  
            </DirectoryConfiguration>  
            <DirectoryConfiguration containerName="badapp">  
              <Absolute path="%SYSTEMDRIVE%\BadApp" expandEnvironment="true" />  
            </DirectoryConfiguration>  
            <DirectoryConfiguration containerName="goodapp">  
              <LocalResource name="Skippy" relativePath="..\PeanutButter"/>  
            </DirectoryConfiguration>  
          </DataSources>  

        </Directories>  

        <EtwProviders>  
          <EtwEventSourceProviderConfiguration   
                       provider="MyProviderClass"   
                       scheduledTransferPeriod="PT5M">  
            <Event id="0"/>  
            <Event id="1" eventDestination="errorTable"/>  
            <DefaultEvents />  
          </EtwEventSourceProviderConfiguration>  
          <EtwManifestProviderConfiguration provider="5974b00b-84c2-44bc-9e58-3a2451b4e3ad" scheduledTransferLogLevelFilter="Information" scheduledTransferPeriod="PT2M">  
            <Event id="0"/>  
            <DefaultEvents eventDestination="defaultTable"/>  
          </EtwManifestProviderConfiguration>  
        </EtwProviders>  

        <WindowsEventLog scheduledTransferPeriod="PT5M">  
          <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>  
          <DataSource name="System!*[System[Provider[@Name='NTFS'] and (EventID=55)]]" />  
          <DataSource name="System!*[System[Provider[@Name='disk'] and (EventID=7 or EventID=52 or EventID=55)]]" />  
        </WindowsEventLog>  

        <Logs  bufferQuotaInMB="1024"   
             scheduledTransferPeriod="PT1M"   
             scheduledTransferLogLevelFilter="Verbose"   
             sinks="ApplicationInsights.AppLogs"/>  <!-- sinks attribute added in 1.5 -->  

        <CrashDumps containerName="wad-crashdumps" directoryQuotaPercentage="30" dumpType="Mini">  
          <CrashDumpConfiguration processName="mynewprocess.exe" />  
          <CrashDumpConfiguration processName="badapp.exe"/>  
        </CrashDumps>  

        <DockerSources> <!-- Added in 1.9 --> 
          <Stats enabled="true" sampleRate="PT1M" scheduledTransferPeriod="PT1M" />
        </DockerSources>

      </DiagnosticMonitorConfiguration>  

      <SinksConfig>   <!-- Added in 1.5 -->  
        <Sink name="ApplicationInsights">   
          <ApplicationInsights>{Insert InstrumentationKey}</ApplicationInsights>   
          <Channels>   
            <Channel logLevel="Error" name="Errors"  />   
            <Channel logLevel="Verbose" name="AppLogs"  />   
          </Channels>   
        </Sink>   
        <Sink name="EventHub"> <!-- Added in 1.7 -->
          <EventHub Url="https://myeventhub-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" usePublisherId="false" />
        </Sink>
        <Sink name="secondaryEventHub"> <!-- Added in 1.7 -->
          <EventHub Url="https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub" SharedAccessKeyName="SendRule" usePublisherId="false" />
        </Sink>
        <Sink name="secondaryStorageAccount"> <!-- Added in 1.7 -->
          <StorageAccount name="secondarydiagstorageaccount" endpoint="https://core.windows.net" />
        </Sink>
   </SinksConfig>

  </WadCfg>  

  <StorageAccount>diagstorageaccount</StorageAccount>
  <StorageType>TableAndBlob</StorageType> <!-- Added in 1.8 -->  
  </PublicConfig>  

  <PrivateConfig>  <!-- Added in 1.3 -->  
    <StorageAccount name="" key="" endpoint="" sasToken="{sas token}"  />  <!-- sasToken in Private config added in 1.8.1 -->  
    <EventHub Url="https://myeventhub-ns.servicebus.windows.net/diageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="{base64 encoded key}" />
   
    <SecondaryStorageAccounts>
       <StorageAccount name="secondarydiagstorageaccount" key="{base64 encoded key}" endpoint="https://core.windows.net" sasToken="{sas token}" />
    </SecondaryStorageAccounts>
   
    <SecondaryEventHubs>
       <EventHub Url="https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub" SharedAccessKeyName="SendRule" SharedAccessKey="{base64 encoded key}" />
    </SecondaryEventHubs>

  </PrivateConfig>  
  <IsEnabled>true</IsEnabled>  
</DiagnosticsConfiguration>  

```  

JSON ekvivalent hello předchozí konfiguračního souboru XML. 

Hello PublicConfig a PrivateConfig jsou oddělené, protože ve většině případů použití json jsou předávány jako jiné proměnné. Tyto případy patří šablony Resource Manageru, sady škálování virtuálního počítače prostředí PowerShell a Visual Studio. 

```json
"PublicConfig" {
    "WadCfg": {
        "DiagnosticMonitorConfiguration": {
            "overallQuotaInMB": 10000,
            "DiagnosticInfrastructureLogs": {
                "scheduledTransferLogLevelFilter": "Error"
            },
            "PerformanceCounters": {
                "scheduledTransferPeriod": "PT1M",
                "PerformanceCounterConfiguration": [
                    {
                        "counterSpecifier": "\\Processor(_Total)\\% Processor Time",
                        "sampleRate": "PT1M",
                        "unit": "percent"
                    }
                ]
            },
            "Directories": {
                "scheduledTransferPeriod": "PT5M",
                "IISLogs": {
                    "containerName": "iislogs"
                },
                "FailedRequestLogs": {
                    "containerName": "iisfailed"
                },
                "DataSources": [
                    {
                        "containerName": "mynewprocess",
                        "Absolute": {
                            "path": "C:\\MyNewProcess",
                            "expandEnvironment": false
                        }
                    },
                    {
                        "containerName": "badapp",
                        "Absolute": {
                            "path": "%SYSTEMDRIVE%\\BadApp",
                            "expandEnvironment": true
                        }
                    },
                    {
                        "containerName": "goodapp",
                        "LocalResource": {
                            "relativePath": "..\\PeanutButter",
                            "name": "Skippy"
                        }
                    }
                ]
            },
            "EtwProviders": {
                "sinks": "",
                "EtwEventSourceProviderConfiguration": [
                    {
                        "scheduledTransferPeriod": "PT5M",
                        "provider": "MyProviderClass",
                        "Event": [
                            {
                                "id": 0
                            },
                            {
                                "id": 1,
                                "eventDestination": "errorTable"
                            }
                        ],
                        "DefaultEvents": {
                        }
                    }
                ],
                "EtwManifestProviderConfiguration": [
                    {
                        "scheduledTransferPeriod": "PT2M",
                        "scheduledTransferLogLevelFilter": "Information",
                        "provider": "5974b00b-84c2-44bc-9e58-3a2451b4e3ad",
                        "Event": [
                            {
                                "id": 0
                            }
                        ],
                        "DefaultEvents": {
                        }
                    }
                ]
            },
            "WindowsEventLog": {
                "scheduledTransferPeriod": "PT5M",
                "DataSource": [
                    {
                        "name": "System!*[System[Provider[@Name='Microsoft Antimalware']]]"
                    },
                    {
                        "name": "System!*[System[Provider[@Name='NTFS'] and (EventID=55)]]"
                    },
                    {
                        "name": "System!*[System[Provider[@Name='disk'] and (EventID=7 or EventID=52 or EventID=55)]]"
                    }
                ]
            },
            "Logs": {
                "scheduledTransferPeriod": "PT1M",
                "scheduledTransferLogLevelFilter": "Verbose",
                "sinks": "ApplicationInsights.AppLogs"
            },
            "CrashDumps": {
                "directoryQuotaPercentage": 30,
                "dumpType": "Mini",
                "containerName": "wad-crashdumps",
                "CrashDumpConfiguration": [
                    {
                        "processName": "mynewprocess.exe"
                    },
                    {
                        "processName": "badapp.exe"
                    }
                ]
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
                                "name": "Errors"
                            },
                            {
                                "logLevel": "Verbose",
                                "name": "AppLogs"
                            }
                        ]
                    }
                },
                {
                    "name": "EventHub",
                    "EventHub": {
                        "Url": "https://myeventhub-ns.servicebus.windows.net/diageventhub",
                        "SharedAccessKeyName": "SendRule",
                        "usePublisherId": false
                    }
                },
                {
                    "name": "secondaryEventHub",
                    "EventHub": {
                        "Url": "https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub",
                        "SharedAccessKeyName": "SendRule",
                        "usePublisherId": false
                    }
                },
                {
                    "name": "secondaryStorageAccount",
                    "StorageAccount": {
                        "name": "secondarydiagstorageaccount",
                        "endpoint": "https://core.windows.net"
                    }
                }
            ]
        }
    },
    "StorageAccount": "diagstorageaccount",
    "StorageType": "TableAndBlob"
}
```

```json
"PrivateConfig" {
    "storageAccountName": "diagstorageaccount",
    "storageAccountKey": "{base64 encoded key}",
    "storageAccountEndPoint": "https://core.windows.net",
    "storageAccountSasToken": "{sas token}",
    "EventHub": {
        "Url": "https://myeventhub-ns.servicebus.windows.net/diageventhub",
        "SharedAccessKeyName": "SendRule",
        "SharedAccessKey": "{base64 encoded key}"
    },
    "SecondaryStorageAccounts": {
        "StorageAccount": [
            {
                "name": "secondarydiagstorageaccount",
                "key": "{base64 encoded key}",
                "endpoint": "https://core.windows.net",
                "sasToken": "{sas token}"
            }
        ]
    },
    "SecondaryEventHubs": {
        "EventHub": [
            {
                "Url": "https://myeventhub-ns.servicebus.windows.net/secondarydiageventhub",
                "SharedAccessKeyName": "SendRule",
                "SharedAccessKey": "{base64 encoded key}"
            }
        ]
    }
}

```

## <a name="reading-this-page"></a>Čtení této stránce  
 Hello značky následující jsou přibližně v pořadí uvedeném v předchozím příkladu hello.  Pokud nevidíte úplný popis kde očekáváte, vyhledejte stránku hello hello elementu nebo atributu.  

## <a name="common-attribute-types"></a>Běžné typy atributů  
 **scheduledTransferPeriod** atributu se zobrazí v několika elementy. Je hello interval mezi naplánované přenosy toostorage zaokrouhlit toohello nejbližší minutu. Hodnota Hello [XML "Doba trvání datového typu."](http://www.w3schools.com/schema/schema_dtypes_date.asp)


## <a name="diagnosticsconfiguration-element"></a>DiagnosticsConfiguration Element  
 *Stromu: Kořen - DiagnosticsConfiguration*

Přidána do verze 1.3.  

element nejvyšší úrovně Hello hello diagnostiky konfiguračního souboru.  

**Atribut** xmlns - hello obor názvů XML pro konfigurační soubor diagnostiky hello:  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  


|Podřízené elementy|Popis|  
|--------------------|-----------------|  
|**PublicConfig**|Povinná hodnota. Na této stránce najdete v popisu jinde.|  
|**PrivateConfig**|Volitelné. Na této stránce najdete v popisu jinde.|  
|**Hodnotu IsEnabled**|Logická hodnota. Na této stránce najdete v popisu jinde.|  

## <a name="publicconfig-element"></a>PublicConfig Element  
 *Stromové struktury: PublicConfig kořenové - DiagnosticsConfiguration-*

 Popisuje konfiguraci veřejné diagnostiky hello.  

|Podřízené elementy|Popis|  
|--------------------|-----------------|  
|**WadCfg**|Povinná hodnota. Na této stránce najdete v popisu jinde.|  
|**StorageAccount**|Název Hello hello hello data toostore účtu Azure Storage v. Můžete také zadat jako parametr při spuštění rutiny Set-AzureServiceDiagnosticsExtension hello.|  
|**StorageType**|Může být *tabulky*, *Blob*, nebo *TableAndBlob*. Tabulka je výchozí. Při výběru TableAndBlob diagnostických dat se zapíše dvakrát – jednou tooeach typu.|  
|**LocalResourceDirectory**|adresář Hello hello virtuálního počítače, kde hello Monitoring Agent ukládá data události. Pokud ne, nastavit, používá výchozí adresář hello:<br /><br /> Pro roli pracovního procesu nebo webové:`C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`<br /><br /> Pro virtuální počítač:`C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`<br /><br /> Povinné atributy jsou:<br /><br /> - **cesta** – hello v toobe systému hello používá Azure Diagnostics.<br /><br /> - **expandEnvironment** -Určuje, zda jsou v názvu cesty hello rozbalit proměnné prostředí.|  

## <a name="wadcfg-element"></a>WadCFG Element  
 *Strom: Root - DiagnosticsConfiguration - PublicConfig - WadCFG*
 
 Identifikuje a nakonfiguruje hello telemetrická data toobe shromažďují.  


## <a name="diagnosticmonitorconfiguration-element"></a>DiagnosticMonitorConfiguration Element 
 *Stromové struktury: DiagnosticMonitorConfiguration PublicConfig - WadCFG - kořenové - DiagnosticsConfiguration-*

 Požaduje se 

|Atributy|Popis|  
|----------------|-----------------|  
| **overallQuotaInMB** | maximální množství místa místního disku, které mohou být spotřebovávána hello Hello různých typů dat diagnostiky shromažďuje Azure Diagnostics. Hello výchozí nastavení je 5 120 MB.<br />
|**useProxyServer** | Nakonfigurujte nastavení serveru proxy hello toouse Azure Diagnostics jako sada v nastavení aplikace Internet Explorer.|  

<br /> <br />

|Podřízené elementy|Popis|  
|--------------------|-----------------|  
|**CrashDumps**|Na této stránce najdete v popisu jinde.|  
|**DiagnosticInfrastructureLogs**|Povolte shromažďování protokolů generovaných Azure Diagnostics. Hello infrastruktury diagnostické protokoly jsou užitečné při řešení potíží samotného systému hello diagnostiky. Volitelné atributy jsou:<br /><br /> - **scheduledTransferLogLevelFilter** -nakonfiguruje úroveň závažnosti minimální hello shromážděné protokoly hello.<br /><br /> - **scheduledTransferPeriod** -hello interval mezi naplánované přenosy toostorage zaokrouhlit toohello nejbližší minutu. Hodnota Hello [XML "Doba trvání datového typu."](http://www.w3schools.com/schema/schema_dtypes_date.asp) |  
|**Adresáře**|Na této stránce najdete v popisu jinde.|  
|**EtwProviders**|Na této stránce najdete v popisu jinde.|  
|**Metriky**|Na této stránce najdete v popisu jinde.|  
|**Čítače výkonu**|Na této stránce najdete v popisu jinde.|  
|**WindowsEventLog**|Na této stránce najdete v popisu jinde.| 
|**DockerSources**|Na této stránce najdete v popisu jinde. | 



## <a name="crashdumps-element"></a>CrashDumps Element  
 *Stromu: - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - CrashDumps kořen*
 
 Povolte shromažďování hello výpisy stavu systému.  

|Atributy|Popis|  
|----------------|-----------------|  
|**containerName**|Volitelné. Název Hello hello kontejneru objektů blob ve vašem toobe účtu Azure Storage používá toostore výpisy stavu systému.|  
|**crashDumpType**|Volitelné.  Nakonfiguruje výpisy stavu Azure Diagnostics toocollect mini nebo celého systému.|  
|**directoryQuotaPercentage**|Volitelné.  Nakonfiguruje hello procento **overallQuotaInMB** toobe vyhrazena pro výpisů stavu systému na hello virtuálních počítačů.|  

|Podřízené elementy|Popis|  
|--------------------|-----------------|  
|**CrashDumpConfiguration**|Povinná hodnota. Definuje hodnoty konfigurace pro jednotlivé procesy.<br /><br /> také je vyžadována Hello následující atribut:<br /><br /> **název_procesu** – hello název procesu hello chcete Azure Diagnostics toocollect stav systému pro.|  

## <a name="directories-element"></a>Element adresáře 
 *Strom: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - adresáře*

 Umožňuje hello kolekce hello obsah adresáře, služba IIS se nezdařilo, přístup k protokolům požadavku nebo protokoly služby IIS.  

 Volitelné **scheduledTransferPeriod** atribut. Viz vysvětlení dříve.  

|Podřízené elementy|Popis|  
|--------------------|-----------------|  
|**IISLogs**|Tento element včetně v konfiguraci hello umožňuje hello shromažďování protokolů služby IIS:<br /><br /> **containerName** -protokoly služby IIS hello toostore použit název hello hello kontejneru objektů blob ve vašem toobe účet úložiště Azure.|   
|**FailedRequestLogs**|Tento element včetně v konfiguraci hello umožňuje shromažďování protokolů o neúspěšných požadavků tooan služby IIS webu nebo aplikaci. Je také nutné povolit trasování možnosti v části **systému. Webový server** v **Web.config**.|  
|**Zdroje dat**|Seznam toomonitor adresáře.| 




## <a name="datasources-element"></a>Element zdrojů dat  
 *Stromové struktury: Zdroje dat PublicConfig - WadCFG - DiagnosticMonitorConfiguration - adresáře - kořenové - DiagnosticsConfiguration-*

 Seznam toomonitor adresáře.  

|Podřízené elementy|Popis|  
|--------------------|-----------------|  
|**DirectoryConfiguration**|Povinná hodnota. Požadovaný atribut:<br /><br /> **containerName** – hello název hello kontejneru objektů blob v účtu úložiště Azure, že toobe použité soubory protokolu toostore hello.|  





## <a name="directoryconfiguration-element"></a>DirectoryConfiguration Element  
 *Stromu: Zdroje dat PublicConfig - WadCFG - DiagnosticMonitorConfiguration - adresáře - kořenové - DiagnosticsConfiguration - - DirectoryConfiguration*

 Může obsahovat buď hello **absolutní** nebo **LocalResource** elementu, ale ne obojí.  

|Podřízené elementy|Popis|  
|--------------------|-----------------|  
|**Absolutní**|Hello toomonitor directory toohello absolutní cesta. vyžadují se Hello následující atributy:<br /><br /> - **Cesta** -hello toomonitor directory toohello absolutní cesta.<br /><br /> - **expandEnvironment** – konfiguruje, zda jsou rozbalit proměnné prostředí v cestě.|  
|**LocalResource**|Hello cesta relativní tooa místní prostředek toomonitor. Povinné atributy jsou:<br /><br /> - **Název** -hello místní prostředek, který obsahuje hello directory toomonitor<br /><br /> - **relativePath** -hello relativní tooName cestu, která obsahuje adresář toomonitor hello|  



## <a name="etwproviders-element"></a>EtwProviders Element  
 *Stromu: - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders kořen*

 Nakonfiguruje shromažďování událostí trasování událostí pro Windows z EventSource nebo Manifest trasování událostí pro Windows na základě zprostředkovatele.  

|Podřízené elementy|Popis|  
|--------------------|-----------------|  
|**EtwEventSourceProviderConfiguration**|Nakonfiguruje kolekce událostí generovaných [EventSource – třída](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx). Požadovaný atribut:<br /><br /> **Zprostředkovatel** -hello název třídy událostí EventSource hello.<br /><br /> Volitelné atributy jsou:<br /><br /> - **scheduledTransferLogLevelFilter** -hello účet úložiště tooyour úrovně tootransfer minimální závažnosti.<br /><br /> - **scheduledTransferPeriod** -hello interval mezi naplánované přenosy toostorage zaokrouhlit toohello nejbližší minutu. Hodnota Hello [XML "Doba trvání datového typu."](http://www.w3schools.com/schema/schema_dtypes_date.asp) |  
|**EtwManifestProviderConfiguration**|Požadovaný atribut:<br /><br /> **Zprostředkovatel** -hello GUID hello událostí zprostředkovatele<br /><br /> Volitelné atributy jsou:<br /><br /> - **scheduledTransferLogLevelFilter** -hello účet úložiště tooyour úrovně tootransfer minimální závažnosti.<br /><br /> - **scheduledTransferPeriod** -hello interval mezi naplánované přenosy toostorage zaokrouhlit toohello nejbližší minutu. Hodnota Hello [XML "Doba trvání datového typu."](http://www.w3schools.com/schema/schema_dtypes_date.asp) |  



## <a name="etweventsourceproviderconfiguration-element"></a>EtwEventSourceProviderConfiguration Element  
 *Stromu: Kořen - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - EtwEventSourceProviderConfiguration*

 Nakonfiguruje kolekce událostí generovaných [EventSource – třída](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).  

|Podřízené elementy|Popis|  
|--------------------|-----------------|  
|**DefaultEvents**|Volitelný atribut:<br/><br/> **eventDestination** – hello název hello tabulky toostore hello události v|  
|**Události**|Požadovaný atribut:<br /><br /> **ID** -hello id události hello.<br /><br /> Volitelný atribut:<br /><br /> **eventDestination** – hello název hello tabulky toostore hello události v|  



## <a name="etwmanifestproviderconfiguration-element"></a>EtwManifestProviderConfiguration Element  
 *Stromové struktury: EtwManifestProviderConfiguration PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - kořenové - DiagnosticsConfiguration-*

|Podřízené elementy|Popis|  
|--------------------|-----------------|  
|**DefaultEvents**|Volitelný atribut:<br /><br /> **eventDestination** – hello název hello tabulky toostore hello události v|  
|**Události**|Požadovaný atribut:<br /><br /> **ID** -hello id události hello.<br /><br /> Volitelný atribut:<br /><br /> **eventDestination** – hello název hello tabulky toostore hello události v|  



## <a name="metrics-element"></a>Element metriky  
 *Strom: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - metriky*

 Umožní vám toogenerate tabulku čítače výkonu, která je optimalizovaná pro rychlé dotazy. Jednotlivých čítačů výkonu, která je definována v hello **čítače výkonu** element je uložena v tabulce hello metriky v tabulce čítače výkonu toohello přidání.  

 Hello **resourceId** atribut je požadován.  ID prostředku Hello hello nasazujete Azure Diagnostics do virtuálního počítače. Získat hello **resourceID** z hello [portál Azure](https://portal.azure.com). Vyberte **Procházet** -> **skupiny prostředků** -> **< název\>**. Klikněte na tlačítko hello **vlastnosti** dlaždici a zkopírujte hodnotu hello z hello **ID** pole.  

|Podřízené elementy|Popis|  
|--------------------|-----------------|  
|**MetricAggregation**|Požadovaný atribut:<br /><br /> **scheduledTransferPeriod** -hello interval mezi naplánované přenosy toostorage zaokrouhlit toohello nejbližší minutu. Hodnota Hello [XML "Doba trvání datového typu."](http://www.w3schools.com/schema/schema_dtypes_date.asp) |  



## <a name="performancecounters-element"></a>PerformanceCounters – Element  
 *Strom: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - čítače výkonu*

 Umožňuje hello kolekci čítačů výkonu.  

 Volitelný atribut:  

 Volitelné **scheduledTransferPeriod** atribut. Viz vysvětlení dříve.

|Podřízený Element|Popis|  
|-------------------|-----------------|  
|**PerformanceCounterConfiguration**|vyžadují se Hello následující atributy:<br /><br /> - **counterSpecifier** hello – název čítače výkonu hello. Například, `\Processor(_Total)\% Processor Time`. tooget seznam výkonu čítače na hostiteli, spusťte příkaz hello `typeperf`.<br /><br /> - **sampleRate** – jak často hello čítač se odeberou.<br /><br /> Volitelný atribut:<br /><br /> **jednotka** -hello Měrná jednotka služby hello čítače.|  




## <a name="windowseventlog-element"></a>WindowsEventLog Element
 *Stromu: - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - WindowsEventLog kořen*
 
 Umožňuje hello kolekce protokoly událostí systému Windows.  

 Volitelné **scheduledTransferPeriod** atribut. Viz vysvětlení dříve.  

|Podřízený Element|Popis|  
|-------------------|-----------------|  
|**Zdroj dat**|toocollect protokoly událostí systému Windows Hello. Požadovaný atribut:<br /><br /> **název** -shromážděných dotaz XPath hello popisující toobe události windows hello. Například:<br /><br /> `Application!*[System[(Level <=3)]], System!*[System[(Level <=3)]], System!*[System[Provider[@Name='Microsoft Antimalware']]], Security!*[System[(Level <= 3)]`<br /><br /> Zadejte všechny události, toocollect "*"|  




## <a name="logs-element"></a>Element protokoly  
 *Strom: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - protokoly*

 Prezentovat v verze 1.0 a 1.1. Chybí v 1.2. Přidat zpět v 1.3.  

 Definuje konfiguraci hello vyrovnávací paměti pro základní Azure protokoly.  

|Atribut|Typ|Popis|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|**celé číslo bez znaménka**|Volitelné. Určuje maximální množství hello úložiště systému souboru, který je k dispozici pro hello zadaná data.<br /><br /> Hello výchozí hodnota je 0.|  
|**scheduledTransferLogLevelFilterr**|**řetězec**|Volitelné. Určuje hello minimální úroveň závažnosti pro položky protokolu, které se přenáší. Hello výchozí hodnota je **Undefined**, která přenáší všechny protokoly. Další možné hodnoty (v pořadí, ve většině tooleast informací) jsou **podrobné**, **informace**, **upozornění**, **chyba**a **Kritické**.|  
|**scheduledTransferPeriod**|**Doba trvání**|Volitelné. Určuje interval hello od naplánovanou přenosů dat, zaokrouhlit toohello nejbližší minutu.<br /><br /> Výchozí hodnota Hello je PT0S.|  
|**jímky** přidali v 1.5|**řetězec**|Volitelné. Body tooa podřízený umístění tooalso posílat diagnostická data. Například Application Insights.|  

## <a name="dockersources"></a>DockerSources
 *Stromu: - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - DockerSources kořen*

 Přidaná do 1.9.

|Název elementu|Popis|  
|------------------|-----------------|  
|**Statistiky**|Říká hello systému toocollect statistiky pro Docker kontejnery|  

## <a name="sinksconfig-element"></a>SinksConfig Element  
 *Stromové struktury: SinksConfig PublicConfig - WadCFG - kořenové - DiagnosticsConfiguration-*

 Seznam umístění toosend diagnostiky dat tooand hello konfigurace přidružené těchto umístěních.  

|Název elementu|Popis|  
|------------------|-----------------|  
|**Podřízený**|Na této stránce najdete v popisu jinde.|  

## <a name="sink-element"></a>Jímky – Element
 *Strom: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - jímka*

 Přidána do verze 1.5.  

 Definuje diagnostická data toosend umístění. Například hello služby Application Insights.  

|Atribut|Typ|Popis|  
|---------------|----------|-----------------|  
|**Jméno**|Řetězec|Řetězec identifikující hello sinkname.|  

|Element|Typ|Popis|  
|-------------|----------|-----------------|  
|**Application Insights**|Řetězec|Použít pouze při odesílání dat tooApplication statistiky. Obsahovat hello klíč instrumentace pro aktivní účet Application Insights, máte přístup.|  
|**Kanály**|Řetězec|Jeden pro každou další filtrování, který datového proudu, který jste|  

## <a name="channels-element"></a>Element kanály  
 *Stromové struktury: Kanály SinksConfig - jímka - kořenové - DiagnosticsConfiguration - PublicConfig - WadCFG-*

 Přidána do verze 1.5.  

 Definuje filtry pro datové proudy prošla jímku dat protokolu.  

|Element|Typ|Popis|  
|-------------|----------|-----------------|  
|**Kanál**|Řetězec|Na této stránce najdete v popisu jinde.|  

## <a name="channel-element"></a>Element kanálu
 *Strom: Kořenové - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - jímka - kanály - kanál*

 Přidána do verze 1.5.  

 Definuje diagnostická data toosend umístění. Například hello služby Application Insights.  

|Atributy|Typ|Popis|  
|----------------|----------|-----------------|  
|**logLevel**|**řetězec**|Určuje hello minimální úroveň závažnosti pro položky protokolu, které se přenáší. Hello výchozí hodnota je **Undefined**, která přenáší všechny protokoly. Další možné hodnoty (v pořadí, ve většině tooleast informací) jsou **podrobné**, **informace**, **upozornění**, **chyba**a **Kritické**.|  
|**Jméno**|**řetězec**|Jedinečný název kanálu toorefer hello k|  


## <a name="privateconfig-element"></a>PrivateConfig Element 
 *Stromové struktury: PrivateConfig kořenové - DiagnosticsConfiguration-*

 Přidána do verze 1.3.  

 Nepovinné  

 Ukládá podrobností soukromého hello účtu úložiště hello (název, klíč a koncového bodu). Tyto informace se odesílají toohello virtuálního počítače, ale nelze načíst z něj.  

|Podřízené elementy|Popis|  
|--------------------|-----------------|  
|**StorageAccount**|Hello toouse účet úložiště. Hello následující atributy jsou požadovány<br /><br /> - **název** hello – název účtu úložiště hello.<br /><br /> - **klíč** – hello klíče účtu úložiště toohello.<br /><br /> - **koncový bod** -tooaccess koncový bod hello hello účet úložiště. <br /><br /> -**sasToken** (přidají 1.8.1)-tokenu SAS místo klíč účtu úložiště můžete zadat v privátní konfigurace hello. Pokud je zadán, klíč účtu úložiště hello se ignoruje. <br />Požadavky pro hello SAS Token: <br />– Podporuje pouze tokenu SAS účtu <br />- *b*, *t* jsou požadovány typy služby. <br /> - **, *c*, *u*, *w* jsou požadována oprávnění. <br /> - *c*, *o* jsou požadovány typy prostředků. <br /> – Podporuje pouze protokol HTTPS hello <br /> -Spuštění a čas vypršení platnosti musí být platné.|  


## <a name="isenabled-element"></a>Element hodnotu IsEnabled  
 *Stromové struktury: Hodnotu IsEnabled kořenové - DiagnosticsConfiguration-*

 Logická hodnota. Použití `true` tooenable hello diagnostiky nebo `false` toodisable hello diagnostiky.
