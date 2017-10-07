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
# <a name="azure-diagnostics-13-and-later-configuration-schema"></a><span data-ttu-id="a0ea9-103">Azure Diagnostics 1.3 a novější schéma konfigurace</span><span class="sxs-lookup"><span data-stu-id="a0ea9-103">Azure Diagnostics 1.3 and later configuration schema</span></span>
> [!NOTE]
> <span data-ttu-id="a0ea9-104">Hello rozšíření diagnostiky Azure je, že součást hello používá toocollect čítače výkonu a další statistiky z:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-104">hello Azure Diagnostics extension is hello component used toocollect performance counters and other statistics from:</span></span>
> - <span data-ttu-id="a0ea9-105">Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="a0ea9-105">Azure Virtual Machines</span></span> 
> - <span data-ttu-id="a0ea9-106">Škálovací sady virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="a0ea9-106">Virtual Machine Scale Sets</span></span>
> - <span data-ttu-id="a0ea9-107">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="a0ea9-107">Service Fabric</span></span> 
> - <span data-ttu-id="a0ea9-108">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="a0ea9-108">Cloud Services</span></span> 
> - <span data-ttu-id="a0ea9-109">Network Security Groups (Skupiny zabezpečení sítě)</span><span class="sxs-lookup"><span data-stu-id="a0ea9-109">Network Security Groups</span></span>
> 
> <span data-ttu-id="a0ea9-110">Tato stránka je pouze relevantní, pokud používáte některou z těchto služeb.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-110">This page is only relevant if you are using one of these services.</span></span>

<span data-ttu-id="a0ea9-111">Tato stránka je platná pro verze 1.3 a novější (Azure SDK 2.4 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="a0ea9-111">This page is valid for versions 1.3 and newer (Azure SDK 2.4 and newer).</span></span> <span data-ttu-id="a0ea9-112">Novější konfigurační oddíly jsou komentáři tooshow ve verzi, které byly přidány.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-112">Newer configuration sections are commented tooshow in what version they were added.</span></span>  

<span data-ttu-id="a0ea9-113">soubor konfigurace Hello zde popsané je použité tooset konfigurace diagnostiky nastavení při monitorování diagnostiky hello spustí.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-113">hello configuration file described here is used tooset diagnostic configuration settings when hello diagnostics monitor starts.</span></span>  

<span data-ttu-id="a0ea9-114">rozšíření Hello se používá ve spojení s dalšími produkty Microsoftu diagnostiky jako monitorování Azure, Application Insights a analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-114">hello extension is used in conjunction with other Microsoft diagnostics products like Azure Monitor, Application Insights, and Log Analytics.</span></span>



<span data-ttu-id="a0ea9-115">Stáhněte si definice schématu souboru hello veřejné konfigurace tak, že spustíte následující příkaz prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-115">Download hello public configuration file schema definition by executing hello following PowerShell command:</span></span>  

```powershell  
(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File –Encoding utf8 -FilePath 'C:\temp\WadConfig.xsd'  
```  

<span data-ttu-id="a0ea9-116">Další informace o používání Azure Diagnostics najdete v tématu [rozšíření diagnostiky Azure](azure-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="a0ea9-116">For more information about using Azure Diagnostics, see [Azure Diagnostics Extension](azure-diagnostics.md).</span></span>  

## <a name="example-of-hello-diagnostics-configuration-file"></a><span data-ttu-id="a0ea9-117">Příklad souboru konfigurace diagnostiky hello</span><span class="sxs-lookup"><span data-stu-id="a0ea9-117">Example of hello diagnostics configuration file</span></span>  
 <span data-ttu-id="a0ea9-118">Hello následující příklad ukazuje konfigurační soubor typické diagnostics:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-118">hello following example shows a typical diagnostics configuration file:</span></span>  

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

<span data-ttu-id="a0ea9-119">JSON ekvivalent hello předchozí konfiguračního souboru XML.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-119">JSON equivalent of hello previous XML configuration file.</span></span> 

<span data-ttu-id="a0ea9-120">Hello PublicConfig a PrivateConfig jsou oddělené, protože ve většině případů použití json jsou předávány jako jiné proměnné.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-120">hello PublicConfig and PrivateConfig are separated because in most json usage cases, they are passed as different variables.</span></span> <span data-ttu-id="a0ea9-121">Tyto případy patří šablony Resource Manageru, sady škálování virtuálního počítače prostředí PowerShell a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-121">These cases include Resource Manager templates, Virtual Machine Scale set PowerShell, and Visual Studio.</span></span> 

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

## <a name="reading-this-page"></a><span data-ttu-id="a0ea9-122">Čtení této stránce</span><span class="sxs-lookup"><span data-stu-id="a0ea9-122">Reading this page</span></span>  
 <span data-ttu-id="a0ea9-123">Hello značky následující jsou přibližně v pořadí uvedeném v předchozím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-123">hello tags following are roughly in order shown in hello preceding example.</span></span>  <span data-ttu-id="a0ea9-124">Pokud nevidíte úplný popis kde očekáváte, vyhledejte stránku hello hello elementu nebo atributu.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-124">If you don't see a full description where you expect it, search hello page for hello element or attribute.</span></span>  

## <a name="common-attribute-types"></a><span data-ttu-id="a0ea9-125">Běžné typy atributů</span><span class="sxs-lookup"><span data-stu-id="a0ea9-125">Common Attribute Types</span></span>  
 <span data-ttu-id="a0ea9-126">**scheduledTransferPeriod** atributu se zobrazí v několika elementy.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-126">**scheduledTransferPeriod** attribute appears in several elements.</span></span> <span data-ttu-id="a0ea9-127">Je hello interval mezi naplánované přenosy toostorage zaokrouhlit toohello nejbližší minutu.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-127">It is hello interval between scheduled transfers toostorage rounded up toohello nearest minute.</span></span> <span data-ttu-id="a0ea9-128">Hodnota Hello [XML "Doba trvání datového typu."](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span><span class="sxs-lookup"><span data-stu-id="a0ea9-128">hello value is an [XML “Duration Data Type.”](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span></span>


## <a name="diagnosticsconfiguration-element"></a><span data-ttu-id="a0ea9-129">DiagnosticsConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="a0ea9-129">DiagnosticsConfiguration Element</span></span>  
 <span data-ttu-id="a0ea9-130">*Stromu: Kořen - DiagnosticsConfiguration*</span><span class="sxs-lookup"><span data-stu-id="a0ea9-130">*Tree: Root - DiagnosticsConfiguration*</span></span>

<span data-ttu-id="a0ea9-131">Přidána do verze 1.3.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-131">Added in version 1.3.</span></span>  

<span data-ttu-id="a0ea9-132">element nejvyšší úrovně Hello hello diagnostiky konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-132">hello top-level element of hello diagnostics configuration file.</span></span>  

<span data-ttu-id="a0ea9-133">**Atribut** xmlns - hello obor názvů XML pro konfigurační soubor diagnostiky hello:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-133">**Attribute**  xmlns - hello XML namespace for hello diagnostics configuration file is:</span></span>  
<span data-ttu-id="a0ea9-134">http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration</span><span class="sxs-lookup"><span data-stu-id="a0ea9-134">http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration</span></span>  


|<span data-ttu-id="a0ea9-135">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="a0ea9-135">Child Elements</span></span>|<span data-ttu-id="a0ea9-136">Popis</span><span class="sxs-lookup"><span data-stu-id="a0ea9-136">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="a0ea9-137">**PublicConfig**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-137">**PublicConfig**</span></span>|<span data-ttu-id="a0ea9-138">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-138">Required.</span></span> <span data-ttu-id="a0ea9-139">Na této stránce najdete v popisu jinde.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-139">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="a0ea9-140">**PrivateConfig**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-140">**PrivateConfig**</span></span>|<span data-ttu-id="a0ea9-141">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-141">Optional.</span></span> <span data-ttu-id="a0ea9-142">Na této stránce najdete v popisu jinde.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-142">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="a0ea9-143">**Hodnotu IsEnabled**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-143">**IsEnabled**</span></span>|<span data-ttu-id="a0ea9-144">Logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-144">Boolean.</span></span> <span data-ttu-id="a0ea9-145">Na této stránce najdete v popisu jinde.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-145">See description elsewhere on this page.</span></span>|  

## <a name="publicconfig-element"></a><span data-ttu-id="a0ea9-146">PublicConfig Element</span><span class="sxs-lookup"><span data-stu-id="a0ea9-146">PublicConfig Element</span></span>  
 <span data-ttu-id="a0ea9-147">*Stromové struktury: PublicConfig kořenové - DiagnosticsConfiguration-*</span><span class="sxs-lookup"><span data-stu-id="a0ea9-147">*Tree: Root - DiagnosticsConfiguration - PublicConfig*</span></span>

 <span data-ttu-id="a0ea9-148">Popisuje konfiguraci veřejné diagnostiky hello.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-148">Describes hello public diagnostics configuration.</span></span>  

|<span data-ttu-id="a0ea9-149">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="a0ea9-149">Child Elements</span></span>|<span data-ttu-id="a0ea9-150">Popis</span><span class="sxs-lookup"><span data-stu-id="a0ea9-150">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="a0ea9-151">**WadCfg**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-151">**WadCfg**</span></span>|<span data-ttu-id="a0ea9-152">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-152">Required.</span></span> <span data-ttu-id="a0ea9-153">Na této stránce najdete v popisu jinde.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-153">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="a0ea9-154">**StorageAccount**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-154">**StorageAccount**</span></span>|<span data-ttu-id="a0ea9-155">Název Hello hello hello data toostore účtu Azure Storage v.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-155">hello name of hello Azure Storage account toostore hello data in.</span></span> <span data-ttu-id="a0ea9-156">Můžete také zadat jako parametr při spuštění rutiny Set-AzureServiceDiagnosticsExtension hello.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-156">May also be specified as a parameter when executing hello Set-AzureServiceDiagnosticsExtension cmdlet.</span></span>|  
|<span data-ttu-id="a0ea9-157">**StorageType**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-157">**StorageType**</span></span>|<span data-ttu-id="a0ea9-158">Může být *tabulky*, *Blob*, nebo *TableAndBlob*.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-158">Can be *Table*, *Blob*, or *TableAndBlob*.</span></span> <span data-ttu-id="a0ea9-159">Tabulka je výchozí.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-159">Table is default.</span></span> <span data-ttu-id="a0ea9-160">Při výběru TableAndBlob diagnostických dat se zapíše dvakrát – jednou tooeach typu.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-160">When TableAndBlob is chosen, diagnostic data is written twice -- once tooeach type.</span></span>|  
|<span data-ttu-id="a0ea9-161">**LocalResourceDirectory**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-161">**LocalResourceDirectory**</span></span>|<span data-ttu-id="a0ea9-162">adresář Hello hello virtuálního počítače, kde hello Monitoring Agent ukládá data události.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-162">hello directory on hello virtual machine where hello Monitoring Agent stores event data.</span></span> <span data-ttu-id="a0ea9-163">Pokud ne, nastavit, používá výchozí adresář hello:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-163">If not, set, hello default directory is used:</span></span><br /><br /> <span data-ttu-id="a0ea9-164">Pro roli pracovního procesu nebo webové:`C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`</span><span class="sxs-lookup"><span data-stu-id="a0ea9-164">For a Worker/web role: `C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`</span></span><br /><br /> <span data-ttu-id="a0ea9-165">Pro virtuální počítač:`C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`</span><span class="sxs-lookup"><span data-stu-id="a0ea9-165">For a Virtual Machine: `C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`</span></span><br /><br /> <span data-ttu-id="a0ea9-166">Povinné atributy jsou:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-166">Required attributes are:</span></span><br /><br /> <span data-ttu-id="a0ea9-167">- **cesta** – hello v toobe systému hello používá Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-167">- **path** - hello directory on hello system toobe used by Azure Diagnostics.</span></span><br /><br /> <span data-ttu-id="a0ea9-168">- **expandEnvironment** -Určuje, zda jsou v názvu cesty hello rozbalit proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-168">- **expandEnvironment** - Controls whether environment variables are expanded in hello path name.</span></span>|  

## <a name="wadcfg-element"></a><span data-ttu-id="a0ea9-169">WadCFG Element</span><span class="sxs-lookup"><span data-stu-id="a0ea9-169">WadCFG Element</span></span>  
 <span data-ttu-id="a0ea9-170">*Strom: Root - DiagnosticsConfiguration - PublicConfig - WadCFG*</span><span class="sxs-lookup"><span data-stu-id="a0ea9-170">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG*</span></span>
 
 <span data-ttu-id="a0ea9-171">Identifikuje a nakonfiguruje hello telemetrická data toobe shromažďují.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-171">Identifies and configures hello telemetry data toobe collected.</span></span>  


## <a name="diagnosticmonitorconfiguration-element"></a><span data-ttu-id="a0ea9-172">DiagnosticMonitorConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="a0ea9-172">DiagnosticMonitorConfiguration Element</span></span> 
 <span data-ttu-id="a0ea9-173">*Stromové struktury: DiagnosticMonitorConfiguration PublicConfig - WadCFG - kořenové - DiagnosticsConfiguration-*</span><span class="sxs-lookup"><span data-stu-id="a0ea9-173">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration*</span></span>

 <span data-ttu-id="a0ea9-174">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="a0ea9-174">Required</span></span> 

|<span data-ttu-id="a0ea9-175">Atributy</span><span class="sxs-lookup"><span data-stu-id="a0ea9-175">Attributes</span></span>|<span data-ttu-id="a0ea9-176">Popis</span><span class="sxs-lookup"><span data-stu-id="a0ea9-176">Description</span></span>|  
|----------------|-----------------|  
| <span data-ttu-id="a0ea9-177">**overallQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-177">**overallQuotaInMB**</span></span> | <span data-ttu-id="a0ea9-178">maximální množství místa místního disku, které mohou být spotřebovávána hello Hello různých typů dat diagnostiky shromažďuje Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-178">hello maximum amount of local disk space that may be consumed by hello various types of diagnostic data collected by Azure Diagnostics.</span></span> <span data-ttu-id="a0ea9-179">Hello výchozí nastavení je 5 120 MB.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-179">hello default setting is 5120 MB.</span></span><br />
|<span data-ttu-id="a0ea9-180">**useProxyServer**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-180">**useProxyServer**</span></span> | <span data-ttu-id="a0ea9-181">Nakonfigurujte nastavení serveru proxy hello toouse Azure Diagnostics jako sada v nastavení aplikace Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-181">Configure Azure Diagnostics toouse hello proxy server settings as set in IE settings.</span></span>|  

<br /> <br />

|<span data-ttu-id="a0ea9-182">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="a0ea9-182">Child Elements</span></span>|<span data-ttu-id="a0ea9-183">Popis</span><span class="sxs-lookup"><span data-stu-id="a0ea9-183">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="a0ea9-184">**CrashDumps**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-184">**CrashDumps**</span></span>|<span data-ttu-id="a0ea9-185">Na této stránce najdete v popisu jinde.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-185">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="a0ea9-186">**DiagnosticInfrastructureLogs**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-186">**DiagnosticInfrastructureLogs**</span></span>|<span data-ttu-id="a0ea9-187">Povolte shromažďování protokolů generovaných Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-187">Enable collection of logs generated by Azure Diagnostics.</span></span> <span data-ttu-id="a0ea9-188">Hello infrastruktury diagnostické protokoly jsou užitečné při řešení potíží samotného systému hello diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-188">hello diagnostic infrastructure logs are useful for troubleshooting hello diagnostics system itself.</span></span> <span data-ttu-id="a0ea9-189">Volitelné atributy jsou:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-189">Optional attributes are:</span></span><br /><br /> <span data-ttu-id="a0ea9-190">- **scheduledTransferLogLevelFilter** -nakonfiguruje úroveň závažnosti minimální hello shromážděné protokoly hello.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-190">- **scheduledTransferLogLevelFilter** - Configures hello minimum severity level of hello logs collected.</span></span><br /><br /> <span data-ttu-id="a0ea9-191">- **scheduledTransferPeriod** -hello interval mezi naplánované přenosy toostorage zaokrouhlit toohello nejbližší minutu.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-191">- **scheduledTransferPeriod** - hello interval between scheduled transfers toostorage rounded up toohello nearest minute.</span></span> <span data-ttu-id="a0ea9-192">Hodnota Hello [XML "Doba trvání datového typu."](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span><span class="sxs-lookup"><span data-stu-id="a0ea9-192">hello value is an [XML “Duration Data Type.”](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span></span> |  
|<span data-ttu-id="a0ea9-193">**Adresáře**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-193">**Directories**</span></span>|<span data-ttu-id="a0ea9-194">Na této stránce najdete v popisu jinde.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-194">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="a0ea9-195">**EtwProviders**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-195">**EtwProviders**</span></span>|<span data-ttu-id="a0ea9-196">Na této stránce najdete v popisu jinde.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-196">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="a0ea9-197">**Metriky**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-197">**Metrics**</span></span>|<span data-ttu-id="a0ea9-198">Na této stránce najdete v popisu jinde.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-198">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="a0ea9-199">**Čítače výkonu**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-199">**PerformanceCounters**</span></span>|<span data-ttu-id="a0ea9-200">Na této stránce najdete v popisu jinde.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-200">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="a0ea9-201">**WindowsEventLog**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-201">**WindowsEventLog**</span></span>|<span data-ttu-id="a0ea9-202">Na této stránce najdete v popisu jinde.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-202">See description elsewhere on this page.</span></span>| 
|<span data-ttu-id="a0ea9-203">**DockerSources**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-203">**DockerSources**</span></span>|<span data-ttu-id="a0ea9-204">Na této stránce najdete v popisu jinde.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-204">See description elsewhere on this page.</span></span> | 



## <a name="crashdumps-element"></a><span data-ttu-id="a0ea9-205">CrashDumps Element</span><span class="sxs-lookup"><span data-stu-id="a0ea9-205">CrashDumps Element</span></span>  
 <span data-ttu-id="a0ea9-206">*Stromu: - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - CrashDumps kořen*</span><span class="sxs-lookup"><span data-stu-id="a0ea9-206">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - CrashDumps*</span></span>
 
 <span data-ttu-id="a0ea9-207">Povolte shromažďování hello výpisy stavu systému.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-207">Enable hello collection of crash dumps.</span></span>  

|<span data-ttu-id="a0ea9-208">Atributy</span><span class="sxs-lookup"><span data-stu-id="a0ea9-208">Attributes</span></span>|<span data-ttu-id="a0ea9-209">Popis</span><span class="sxs-lookup"><span data-stu-id="a0ea9-209">Description</span></span>|  
|----------------|-----------------|  
|<span data-ttu-id="a0ea9-210">**containerName**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-210">**containerName**</span></span>|<span data-ttu-id="a0ea9-211">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-211">Optional.</span></span> <span data-ttu-id="a0ea9-212">Název Hello hello kontejneru objektů blob ve vašem toobe účtu Azure Storage používá toostore výpisy stavu systému.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-212">hello name of hello blob container in your Azure Storage account toobe used toostore crash dumps.</span></span>|  
|<span data-ttu-id="a0ea9-213">**crashDumpType**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-213">**crashDumpType**</span></span>|<span data-ttu-id="a0ea9-214">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-214">Optional.</span></span>  <span data-ttu-id="a0ea9-215">Nakonfiguruje výpisy stavu Azure Diagnostics toocollect mini nebo celého systému.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-215">Configures Azure Diagnostics toocollect mini or full crash dumps.</span></span>|  
|<span data-ttu-id="a0ea9-216">**directoryQuotaPercentage**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-216">**directoryQuotaPercentage**</span></span>|<span data-ttu-id="a0ea9-217">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-217">Optional.</span></span>  <span data-ttu-id="a0ea9-218">Nakonfiguruje hello procento **overallQuotaInMB** toobe vyhrazena pro výpisů stavu systému na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-218">Configures hello percentage of **overallQuotaInMB** toobe reserved for crash dumps on hello VM.</span></span>|  

|<span data-ttu-id="a0ea9-219">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="a0ea9-219">Child Elements</span></span>|<span data-ttu-id="a0ea9-220">Popis</span><span class="sxs-lookup"><span data-stu-id="a0ea9-220">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="a0ea9-221">**CrashDumpConfiguration**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-221">**CrashDumpConfiguration**</span></span>|<span data-ttu-id="a0ea9-222">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-222">Required.</span></span> <span data-ttu-id="a0ea9-223">Definuje hodnoty konfigurace pro jednotlivé procesy.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-223">Defines configuration values for each process.</span></span><br /><br /> <span data-ttu-id="a0ea9-224">také je vyžadována Hello následující atribut:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-224">hello following attribute is also required:</span></span><br /><br /> <span data-ttu-id="a0ea9-225">**název_procesu** – hello název procesu hello chcete Azure Diagnostics toocollect stav systému pro.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-225">**processName** - hello name of hello process you want Azure Diagnostics toocollect a crash dump for.</span></span>|  

## <a name="directories-element"></a><span data-ttu-id="a0ea9-226">Element adresáře</span><span class="sxs-lookup"><span data-stu-id="a0ea9-226">Directories Element</span></span> 
 <span data-ttu-id="a0ea9-227">*Strom: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - adresáře*</span><span class="sxs-lookup"><span data-stu-id="a0ea9-227">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration -  Directories*</span></span>

 <span data-ttu-id="a0ea9-228">Umožňuje hello kolekce hello obsah adresáře, služba IIS se nezdařilo, přístup k protokolům požadavku nebo protokoly služby IIS.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-228">Enables hello collection of hello contents of a directory, IIS failed access request logs and/or IIS logs.</span></span>  

 <span data-ttu-id="a0ea9-229">Volitelné **scheduledTransferPeriod** atribut.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-229">Optional **scheduledTransferPeriod** attribute.</span></span> <span data-ttu-id="a0ea9-230">Viz vysvětlení dříve.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-230">See explanation earlier.</span></span>  

|<span data-ttu-id="a0ea9-231">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="a0ea9-231">Child Elements</span></span>|<span data-ttu-id="a0ea9-232">Popis</span><span class="sxs-lookup"><span data-stu-id="a0ea9-232">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="a0ea9-233">**IISLogs**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-233">**IISLogs**</span></span>|<span data-ttu-id="a0ea9-234">Tento element včetně v konfiguraci hello umožňuje hello shromažďování protokolů služby IIS:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-234">Including this element in hello configuration enables hello collection of IIS logs:</span></span><br /><br /> <span data-ttu-id="a0ea9-235">**containerName** -protokoly služby IIS hello toostore použit název hello hello kontejneru objektů blob ve vašem toobe účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-235">**containerName** - hello name of hello blob container in your Azure Storage account toobe used toostore hello IIS logs.</span></span>|   
|<span data-ttu-id="a0ea9-236">**FailedRequestLogs**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-236">**FailedRequestLogs**</span></span>|<span data-ttu-id="a0ea9-237">Tento element včetně v konfiguraci hello umožňuje shromažďování protokolů o neúspěšných požadavků tooan služby IIS webu nebo aplikaci.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-237">Including this element in hello configuration enables collection of logs about failed requests tooan IIS site or application.</span></span> <span data-ttu-id="a0ea9-238">Je také nutné povolit trasování možnosti v části **systému. Webový server** v **Web.config**.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-238">You must also enable tracing options under **system.WebServer** in **Web.config**.</span></span>|  
|<span data-ttu-id="a0ea9-239">**Zdroje dat**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-239">**DataSources**</span></span>|<span data-ttu-id="a0ea9-240">Seznam toomonitor adresáře.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-240">A list of directories toomonitor.</span></span>| 




## <a name="datasources-element"></a><span data-ttu-id="a0ea9-241">Element zdrojů dat</span><span class="sxs-lookup"><span data-stu-id="a0ea9-241">DataSources Element</span></span>  
 <span data-ttu-id="a0ea9-242">*Stromové struktury: Zdroje dat PublicConfig - WadCFG - DiagnosticMonitorConfiguration - adresáře - kořenové - DiagnosticsConfiguration-*</span><span class="sxs-lookup"><span data-stu-id="a0ea9-242">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Directories - DataSources*</span></span>

 <span data-ttu-id="a0ea9-243">Seznam toomonitor adresáře.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-243">A list of directories toomonitor.</span></span>  

|<span data-ttu-id="a0ea9-244">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="a0ea9-244">Child Elements</span></span>|<span data-ttu-id="a0ea9-245">Popis</span><span class="sxs-lookup"><span data-stu-id="a0ea9-245">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="a0ea9-246">**DirectoryConfiguration**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-246">**DirectoryConfiguration**</span></span>|<span data-ttu-id="a0ea9-247">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-247">Required.</span></span> <span data-ttu-id="a0ea9-248">Požadovaný atribut:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-248">Required attribute:</span></span><br /><br /> <span data-ttu-id="a0ea9-249">**containerName** – hello název hello kontejneru objektů blob v účtu úložiště Azure, že toobe použité soubory protokolu toostore hello.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-249">**containerName** - hello name of hello blob container in your Azure Storage account that toobe used toostore hello log files.</span></span>|  





## <a name="directoryconfiguration-element"></a><span data-ttu-id="a0ea9-250">DirectoryConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="a0ea9-250">DirectoryConfiguration Element</span></span>  
 <span data-ttu-id="a0ea9-251">*Stromu: Zdroje dat PublicConfig - WadCFG - DiagnosticMonitorConfiguration - adresáře - kořenové - DiagnosticsConfiguration - - DirectoryConfiguration*</span><span class="sxs-lookup"><span data-stu-id="a0ea9-251">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Directories - DataSources - DirectoryConfiguration*</span></span>

 <span data-ttu-id="a0ea9-252">Může obsahovat buď hello **absolutní** nebo **LocalResource** elementu, ale ne obojí.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-252">May include either hello **Absolute** or **LocalResource** element but not both.</span></span>  

|<span data-ttu-id="a0ea9-253">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="a0ea9-253">Child Elements</span></span>|<span data-ttu-id="a0ea9-254">Popis</span><span class="sxs-lookup"><span data-stu-id="a0ea9-254">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="a0ea9-255">**Absolutní**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-255">**Absolute**</span></span>|<span data-ttu-id="a0ea9-256">Hello toomonitor directory toohello absolutní cesta.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-256">hello absolute path toohello directory toomonitor.</span></span> <span data-ttu-id="a0ea9-257">vyžadují se Hello následující atributy:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-257">hello following attributes are required:</span></span><br /><br /> <span data-ttu-id="a0ea9-258">- **Cesta** -hello toomonitor directory toohello absolutní cesta.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-258">- **Path** - hello absolute path toohello directory toomonitor.</span></span><br /><br /> <span data-ttu-id="a0ea9-259">- **expandEnvironment** – konfiguruje, zda jsou rozbalit proměnné prostředí v cestě.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-259">- **expandEnvironment** - Configures whether environment variables in Path are expanded.</span></span>|  
|<span data-ttu-id="a0ea9-260">**LocalResource**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-260">**LocalResource**</span></span>|<span data-ttu-id="a0ea9-261">Hello cesta relativní tooa místní prostředek toomonitor.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-261">hello path relative tooa local resource toomonitor.</span></span> <span data-ttu-id="a0ea9-262">Povinné atributy jsou:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-262">Required attributes are:</span></span><br /><br /> <span data-ttu-id="a0ea9-263">- **Název** -hello místní prostředek, který obsahuje hello directory toomonitor</span><span class="sxs-lookup"><span data-stu-id="a0ea9-263">- **Name** - hello local resource that contains hello directory toomonitor</span></span><br /><br /> <span data-ttu-id="a0ea9-264">- **relativePath** -hello relativní tooName cestu, která obsahuje adresář toomonitor hello</span><span class="sxs-lookup"><span data-stu-id="a0ea9-264">- **relativePath** - hello path relative tooName that contains hello directory toomonitor</span></span>|  



## <a name="etwproviders-element"></a><span data-ttu-id="a0ea9-265">EtwProviders Element</span><span class="sxs-lookup"><span data-stu-id="a0ea9-265">EtwProviders Element</span></span>  
 <span data-ttu-id="a0ea9-266">*Stromu: - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders kořen*</span><span class="sxs-lookup"><span data-stu-id="a0ea9-266">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders*</span></span>

 <span data-ttu-id="a0ea9-267">Nakonfiguruje shromažďování událostí trasování událostí pro Windows z EventSource nebo Manifest trasování událostí pro Windows na základě zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-267">Configures collection of ETW events from EventSource and/or ETW Manifest based providers.</span></span>  

|<span data-ttu-id="a0ea9-268">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="a0ea9-268">Child Elements</span></span>|<span data-ttu-id="a0ea9-269">Popis</span><span class="sxs-lookup"><span data-stu-id="a0ea9-269">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="a0ea9-270">**EtwEventSourceProviderConfiguration**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-270">**EtwEventSourceProviderConfiguration**</span></span>|<span data-ttu-id="a0ea9-271">Nakonfiguruje kolekce událostí generovaných [EventSource – třída](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="a0ea9-271">Configures collection of events generated from [EventSource Class](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).</span></span> <span data-ttu-id="a0ea9-272">Požadovaný atribut:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-272">Required attribute:</span></span><br /><br /> <span data-ttu-id="a0ea9-273">**Zprostředkovatel** -hello název třídy událostí EventSource hello.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-273">**provider** - hello class name of hello EventSource event.</span></span><br /><br /> <span data-ttu-id="a0ea9-274">Volitelné atributy jsou:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-274">Optional attributes are:</span></span><br /><br /> <span data-ttu-id="a0ea9-275">- **scheduledTransferLogLevelFilter** -hello účet úložiště tooyour úrovně tootransfer minimální závažnosti.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-275">- **scheduledTransferLogLevelFilter** - hello minimum severity level tootransfer tooyour storage account.</span></span><br /><br /> <span data-ttu-id="a0ea9-276">- **scheduledTransferPeriod** -hello interval mezi naplánované přenosy toostorage zaokrouhlit toohello nejbližší minutu.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-276">- **scheduledTransferPeriod** - hello interval between scheduled transfers toostorage rounded up toohello nearest minute.</span></span> <span data-ttu-id="a0ea9-277">Hodnota Hello [XML "Doba trvání datového typu."](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span><span class="sxs-lookup"><span data-stu-id="a0ea9-277">hello value is an [XML “Duration Data Type.”](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span></span> |  
|<span data-ttu-id="a0ea9-278">**EtwManifestProviderConfiguration**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-278">**EtwManifestProviderConfiguration**</span></span>|<span data-ttu-id="a0ea9-279">Požadovaný atribut:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-279">Required attribute:</span></span><br /><br /> <span data-ttu-id="a0ea9-280">**Zprostředkovatel** -hello GUID hello událostí zprostředkovatele</span><span class="sxs-lookup"><span data-stu-id="a0ea9-280">**provider** - hello GUID of hello event provider</span></span><br /><br /> <span data-ttu-id="a0ea9-281">Volitelné atributy jsou:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-281">Optional attributes are:</span></span><br /><br /> <span data-ttu-id="a0ea9-282">- **scheduledTransferLogLevelFilter** -hello účet úložiště tooyour úrovně tootransfer minimální závažnosti.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-282">- **scheduledTransferLogLevelFilter** - hello minimum severity level tootransfer tooyour storage account.</span></span><br /><br /> <span data-ttu-id="a0ea9-283">- **scheduledTransferPeriod** -hello interval mezi naplánované přenosy toostorage zaokrouhlit toohello nejbližší minutu.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-283">- **scheduledTransferPeriod** - hello interval between scheduled transfers toostorage rounded up toohello nearest minute.</span></span> <span data-ttu-id="a0ea9-284">Hodnota Hello [XML "Doba trvání datového typu."](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span><span class="sxs-lookup"><span data-stu-id="a0ea9-284">hello value is an [XML “Duration Data Type.”](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span></span> |  



## <a name="etweventsourceproviderconfiguration-element"></a><span data-ttu-id="a0ea9-285">EtwEventSourceProviderConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="a0ea9-285">EtwEventSourceProviderConfiguration Element</span></span>  
 <span data-ttu-id="a0ea9-286">*Stromu: Kořen - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - EtwEventSourceProviderConfiguration*</span><span class="sxs-lookup"><span data-stu-id="a0ea9-286">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders- EtwEventSourceProviderConfiguration*</span></span>

 <span data-ttu-id="a0ea9-287">Nakonfiguruje kolekce událostí generovaných [EventSource – třída](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="a0ea9-287">Configures collection of events generated from [EventSource Class](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).</span></span>  

|<span data-ttu-id="a0ea9-288">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="a0ea9-288">Child Elements</span></span>|<span data-ttu-id="a0ea9-289">Popis</span><span class="sxs-lookup"><span data-stu-id="a0ea9-289">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="a0ea9-290">**DefaultEvents**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-290">**DefaultEvents**</span></span>|<span data-ttu-id="a0ea9-291">Volitelný atribut:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-291">Optional attribute:</span></span><br/><br/> <span data-ttu-id="a0ea9-292">**eventDestination** – hello název hello tabulky toostore hello události v</span><span class="sxs-lookup"><span data-stu-id="a0ea9-292">**eventDestination** - hello name of hello table toostore hello events in</span></span>|  
|<span data-ttu-id="a0ea9-293">**Události**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-293">**Event**</span></span>|<span data-ttu-id="a0ea9-294">Požadovaný atribut:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-294">Required attribute:</span></span><br /><br /> <span data-ttu-id="a0ea9-295">**ID** -hello id události hello.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-295">**id** - hello id of hello event.</span></span><br /><br /> <span data-ttu-id="a0ea9-296">Volitelný atribut:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-296">Optional attribute:</span></span><br /><br /> <span data-ttu-id="a0ea9-297">**eventDestination** – hello název hello tabulky toostore hello události v</span><span class="sxs-lookup"><span data-stu-id="a0ea9-297">**eventDestination** - hello name of hello table toostore hello events in</span></span>|  



## <a name="etwmanifestproviderconfiguration-element"></a><span data-ttu-id="a0ea9-298">EtwManifestProviderConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="a0ea9-298">EtwManifestProviderConfiguration Element</span></span>  
 <span data-ttu-id="a0ea9-299">*Stromové struktury: EtwManifestProviderConfiguration PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - kořenové - DiagnosticsConfiguration-*</span><span class="sxs-lookup"><span data-stu-id="a0ea9-299">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - EtwManifestProviderConfiguration*</span></span>

|<span data-ttu-id="a0ea9-300">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="a0ea9-300">Child Elements</span></span>|<span data-ttu-id="a0ea9-301">Popis</span><span class="sxs-lookup"><span data-stu-id="a0ea9-301">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="a0ea9-302">**DefaultEvents**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-302">**DefaultEvents**</span></span>|<span data-ttu-id="a0ea9-303">Volitelný atribut:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-303">Optional attribute:</span></span><br /><br /> <span data-ttu-id="a0ea9-304">**eventDestination** – hello název hello tabulky toostore hello události v</span><span class="sxs-lookup"><span data-stu-id="a0ea9-304">**eventDestination** - hello name of hello table toostore hello events in</span></span>|  
|<span data-ttu-id="a0ea9-305">**Události**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-305">**Event**</span></span>|<span data-ttu-id="a0ea9-306">Požadovaný atribut:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-306">Required attribute:</span></span><br /><br /> <span data-ttu-id="a0ea9-307">**ID** -hello id události hello.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-307">**id** - hello id of hello event.</span></span><br /><br /> <span data-ttu-id="a0ea9-308">Volitelný atribut:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-308">Optional attribute:</span></span><br /><br /> <span data-ttu-id="a0ea9-309">**eventDestination** – hello název hello tabulky toostore hello události v</span><span class="sxs-lookup"><span data-stu-id="a0ea9-309">**eventDestination** - hello name of hello table toostore hello events in</span></span>|  



## <a name="metrics-element"></a><span data-ttu-id="a0ea9-310">Element metriky</span><span class="sxs-lookup"><span data-stu-id="a0ea9-310">Metrics Element</span></span>  
 <span data-ttu-id="a0ea9-311">*Strom: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - metriky*</span><span class="sxs-lookup"><span data-stu-id="a0ea9-311">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Metrics*</span></span>

 <span data-ttu-id="a0ea9-312">Umožní vám toogenerate tabulku čítače výkonu, která je optimalizovaná pro rychlé dotazy.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-312">Enables you toogenerate a performance counter table that is optimized for fast queries.</span></span> <span data-ttu-id="a0ea9-313">Jednotlivých čítačů výkonu, která je definována v hello **čítače výkonu** element je uložena v tabulce hello metriky v tabulce čítače výkonu toohello přidání.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-313">Each performance counter that is defined in hello **PerformanceCounters** element is stored in hello Metrics table in addition toohello Performance Counter table.</span></span>  

 <span data-ttu-id="a0ea9-314">Hello **resourceId** atribut je požadován.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-314">hello **resourceId** attribute is required.</span></span>  <span data-ttu-id="a0ea9-315">ID prostředku Hello hello nasazujete Azure Diagnostics do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-315">hello resource ID of hello Virtual Machine you are deploying Azure Diagnostics to.</span></span> <span data-ttu-id="a0ea9-316">Získat hello **resourceID** z hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a0ea9-316">Get hello **resourceID** from hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="a0ea9-317">Vyberte **Procházet** -> **skupiny prostředků** -> **< název\>**.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-317">Select **Browse** -> **Resource Groups** -> **<Name\>**.</span></span> <span data-ttu-id="a0ea9-318">Klikněte na tlačítko hello **vlastnosti** dlaždici a zkopírujte hodnotu hello z hello **ID** pole.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-318">Click hello **Properties** tile and copy hello value from hello **ID** field.</span></span>  

|<span data-ttu-id="a0ea9-319">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="a0ea9-319">Child Elements</span></span>|<span data-ttu-id="a0ea9-320">Popis</span><span class="sxs-lookup"><span data-stu-id="a0ea9-320">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="a0ea9-321">**MetricAggregation**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-321">**MetricAggregation**</span></span>|<span data-ttu-id="a0ea9-322">Požadovaný atribut:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-322">Required attribute:</span></span><br /><br /> <span data-ttu-id="a0ea9-323">**scheduledTransferPeriod** -hello interval mezi naplánované přenosy toostorage zaokrouhlit toohello nejbližší minutu.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-323">**scheduledTransferPeriod** - hello interval between scheduled transfers toostorage rounded up toohello nearest minute.</span></span> <span data-ttu-id="a0ea9-324">Hodnota Hello [XML "Doba trvání datového typu."](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span><span class="sxs-lookup"><span data-stu-id="a0ea9-324">hello value is an [XML “Duration Data Type.”](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span></span> |  



## <a name="performancecounters-element"></a><span data-ttu-id="a0ea9-325">PerformanceCounters – Element</span><span class="sxs-lookup"><span data-stu-id="a0ea9-325">PerformanceCounters Element</span></span>  
 <span data-ttu-id="a0ea9-326">*Strom: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - čítače výkonu*</span><span class="sxs-lookup"><span data-stu-id="a0ea9-326">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - PerformanceCounters*</span></span>

 <span data-ttu-id="a0ea9-327">Umožňuje hello kolekci čítačů výkonu.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-327">Enables hello collection of performance counters.</span></span>  

 <span data-ttu-id="a0ea9-328">Volitelný atribut:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-328">Optional attribute:</span></span>  

 <span data-ttu-id="a0ea9-329">Volitelné **scheduledTransferPeriod** atribut.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-329">Optional **scheduledTransferPeriod** attribute.</span></span> <span data-ttu-id="a0ea9-330">Viz vysvětlení dříve.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-330">See explanation earlier.</span></span>

|<span data-ttu-id="a0ea9-331">Podřízený Element</span><span class="sxs-lookup"><span data-stu-id="a0ea9-331">Child Element</span></span>|<span data-ttu-id="a0ea9-332">Popis</span><span class="sxs-lookup"><span data-stu-id="a0ea9-332">Description</span></span>|  
|-------------------|-----------------|  
|<span data-ttu-id="a0ea9-333">**PerformanceCounterConfiguration**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-333">**PerformanceCounterConfiguration**</span></span>|<span data-ttu-id="a0ea9-334">vyžadují se Hello následující atributy:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-334">hello following attributes are required:</span></span><br /><br /> <span data-ttu-id="a0ea9-335">- **counterSpecifier** hello – název čítače výkonu hello.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-335">- **counterSpecifier** - hello name of hello performance counter.</span></span> <span data-ttu-id="a0ea9-336">Například, `\Processor(_Total)\% Processor Time`.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-336">For example, `\Processor(_Total)\% Processor Time`.</span></span> <span data-ttu-id="a0ea9-337">tooget seznam výkonu čítače na hostiteli, spusťte příkaz hello `typeperf`.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-337">tooget a list of performance counters on your host, run hello command `typeperf`.</span></span><br /><br /> <span data-ttu-id="a0ea9-338">- **sampleRate** – jak často hello čítač se odeberou.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-338">- **sampleRate** - How often hello counter should be sampled.</span></span><br /><br /> <span data-ttu-id="a0ea9-339">Volitelný atribut:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-339">Optional attribute:</span></span><br /><br /> <span data-ttu-id="a0ea9-340">**jednotka** -hello Měrná jednotka služby hello čítače.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-340">**unit** - hello unit of measure of hello counter.</span></span>|  




## <a name="windowseventlog-element"></a><span data-ttu-id="a0ea9-341">WindowsEventLog Element</span><span class="sxs-lookup"><span data-stu-id="a0ea9-341">WindowsEventLog Element</span></span>
 <span data-ttu-id="a0ea9-342">*Stromu: - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - WindowsEventLog kořen*</span><span class="sxs-lookup"><span data-stu-id="a0ea9-342">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - WindowsEventLog*</span></span>
 
 <span data-ttu-id="a0ea9-343">Umožňuje hello kolekce protokoly událostí systému Windows.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-343">Enables hello collection of Windows Event Logs.</span></span>  

 <span data-ttu-id="a0ea9-344">Volitelné **scheduledTransferPeriod** atribut.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-344">Optional **scheduledTransferPeriod** attribute.</span></span> <span data-ttu-id="a0ea9-345">Viz vysvětlení dříve.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-345">See explanation  earlier.</span></span>  

|<span data-ttu-id="a0ea9-346">Podřízený Element</span><span class="sxs-lookup"><span data-stu-id="a0ea9-346">Child Element</span></span>|<span data-ttu-id="a0ea9-347">Popis</span><span class="sxs-lookup"><span data-stu-id="a0ea9-347">Description</span></span>|  
|-------------------|-----------------|  
|<span data-ttu-id="a0ea9-348">**Zdroj dat**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-348">**DataSource**</span></span>|<span data-ttu-id="a0ea9-349">toocollect protokoly událostí systému Windows Hello.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-349">hello Windows Event logs toocollect.</span></span> <span data-ttu-id="a0ea9-350">Požadovaný atribut:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-350">Required attribute:</span></span><br /><br /> <span data-ttu-id="a0ea9-351">**název** -shromážděných dotaz XPath hello popisující toobe události windows hello.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-351">**name** - hello XPath query describing hello windows events toobe collected.</span></span> <span data-ttu-id="a0ea9-352">Například:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-352">For example:</span></span><br /><br /> `Application!*[System[(Level <=3)]], System!*[System[(Level <=3)]], System!*[System[Provider[@Name='Microsoft Antimalware']]], Security!*[System[(Level <= 3)]`<br /><br /> <span data-ttu-id="a0ea9-353">Zadejte všechny události, toocollect "*"</span><span class="sxs-lookup"><span data-stu-id="a0ea9-353">toocollect all events, specify "*"</span></span>|  




## <a name="logs-element"></a><span data-ttu-id="a0ea9-354">Element protokoly</span><span class="sxs-lookup"><span data-stu-id="a0ea9-354">Logs Element</span></span>  
 <span data-ttu-id="a0ea9-355">*Strom: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - protokoly*</span><span class="sxs-lookup"><span data-stu-id="a0ea9-355">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Logs*</span></span>

 <span data-ttu-id="a0ea9-356">Prezentovat v verze 1.0 a 1.1.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-356">Present in version 1.0 and 1.1.</span></span> <span data-ttu-id="a0ea9-357">Chybí v 1.2.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-357">Missing in 1.2.</span></span> <span data-ttu-id="a0ea9-358">Přidat zpět v 1.3.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-358">Added back in 1.3.</span></span>  

 <span data-ttu-id="a0ea9-359">Definuje konfiguraci hello vyrovnávací paměti pro základní Azure protokoly.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-359">Defines hello buffer configuration for basic Azure logs.</span></span>  

|<span data-ttu-id="a0ea9-360">Atribut</span><span class="sxs-lookup"><span data-stu-id="a0ea9-360">Attribute</span></span>|<span data-ttu-id="a0ea9-361">Typ</span><span class="sxs-lookup"><span data-stu-id="a0ea9-361">Type</span></span>|<span data-ttu-id="a0ea9-362">Popis</span><span class="sxs-lookup"><span data-stu-id="a0ea9-362">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="a0ea9-363">**bufferQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-363">**bufferQuotaInMB**</span></span>|<span data-ttu-id="a0ea9-364">**celé číslo bez znaménka**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-364">**unsignedInt**</span></span>|<span data-ttu-id="a0ea9-365">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-365">Optional.</span></span> <span data-ttu-id="a0ea9-366">Určuje maximální množství hello úložiště systému souboru, který je k dispozici pro hello zadaná data.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-366">Specifies hello maximum amount of file system storage that is available for hello specified data.</span></span><br /><br /> <span data-ttu-id="a0ea9-367">Hello výchozí hodnota je 0.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-367">hello default is 0.</span></span>|  
|<span data-ttu-id="a0ea9-368">**scheduledTransferLogLevelFilterr**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-368">**scheduledTransferLogLevelFilterr**</span></span>|<span data-ttu-id="a0ea9-369">**řetězec**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-369">**string**</span></span>|<span data-ttu-id="a0ea9-370">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-370">Optional.</span></span> <span data-ttu-id="a0ea9-371">Určuje hello minimální úroveň závažnosti pro položky protokolu, které se přenáší.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-371">Specifies hello minimum severity level for log entries that are transferred.</span></span> <span data-ttu-id="a0ea9-372">Hello výchozí hodnota je **Undefined**, která přenáší všechny protokoly.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-372">hello default value is **Undefined**, which transfers all logs.</span></span> <span data-ttu-id="a0ea9-373">Další možné hodnoty (v pořadí, ve většině tooleast informací) jsou **podrobné**, **informace**, **upozornění**, **chyba**a **Kritické**.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-373">Other possible values (in order of most tooleast information) are **Verbose**, **Information**, **Warning**, **Error**, and **Critical**.</span></span>|  
|<span data-ttu-id="a0ea9-374">**scheduledTransferPeriod**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-374">**scheduledTransferPeriod**</span></span>|<span data-ttu-id="a0ea9-375">**Doba trvání**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-375">**duration**</span></span>|<span data-ttu-id="a0ea9-376">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-376">Optional.</span></span> <span data-ttu-id="a0ea9-377">Určuje interval hello od naplánovanou přenosů dat, zaokrouhlit toohello nejbližší minutu.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-377">Specifies hello interval between scheduled transfers of data, rounded up toohello nearest minute.</span></span><br /><br /> <span data-ttu-id="a0ea9-378">Výchozí hodnota Hello je PT0S.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-378">hello default is PT0S.</span></span>|  
|<span data-ttu-id="a0ea9-379">**jímky** přidali v 1.5</span><span class="sxs-lookup"><span data-stu-id="a0ea9-379">**sinks** Added in 1.5</span></span>|<span data-ttu-id="a0ea9-380">**řetězec**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-380">**string**</span></span>|<span data-ttu-id="a0ea9-381">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-381">Optional.</span></span> <span data-ttu-id="a0ea9-382">Body tooa podřízený umístění tooalso posílat diagnostická data.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-382">Points tooa sink location tooalso send diagnostic data.</span></span> <span data-ttu-id="a0ea9-383">Například Application Insights.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-383">For example, Application Insights.</span></span>|  

## <a name="dockersources"></a><span data-ttu-id="a0ea9-384">DockerSources</span><span class="sxs-lookup"><span data-stu-id="a0ea9-384">DockerSources</span></span>
 <span data-ttu-id="a0ea9-385">*Stromu: - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - DockerSources kořen*</span><span class="sxs-lookup"><span data-stu-id="a0ea9-385">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - DockerSources*</span></span>

 <span data-ttu-id="a0ea9-386">Přidaná do 1.9.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-386">Added in 1.9.</span></span>

|<span data-ttu-id="a0ea9-387">Název elementu</span><span class="sxs-lookup"><span data-stu-id="a0ea9-387">Element Name</span></span>|<span data-ttu-id="a0ea9-388">Popis</span><span class="sxs-lookup"><span data-stu-id="a0ea9-388">Description</span></span>|  
|------------------|-----------------|  
|<span data-ttu-id="a0ea9-389">**Statistiky**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-389">**Stats**</span></span>|<span data-ttu-id="a0ea9-390">Říká hello systému toocollect statistiky pro Docker kontejnery</span><span class="sxs-lookup"><span data-stu-id="a0ea9-390">Tells hello system toocollect stats for Docker containers</span></span>|  

## <a name="sinksconfig-element"></a><span data-ttu-id="a0ea9-391">SinksConfig Element</span><span class="sxs-lookup"><span data-stu-id="a0ea9-391">SinksConfig Element</span></span>  
 <span data-ttu-id="a0ea9-392">*Stromové struktury: SinksConfig PublicConfig - WadCFG - kořenové - DiagnosticsConfiguration-*</span><span class="sxs-lookup"><span data-stu-id="a0ea9-392">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig*</span></span>

 <span data-ttu-id="a0ea9-393">Seznam umístění toosend diagnostiky dat tooand hello konfigurace přidružené těchto umístěních.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-393">A list of locations toosend diagnostics data tooand hello configuration associated with those locations.</span></span>  

|<span data-ttu-id="a0ea9-394">Název elementu</span><span class="sxs-lookup"><span data-stu-id="a0ea9-394">Element Name</span></span>|<span data-ttu-id="a0ea9-395">Popis</span><span class="sxs-lookup"><span data-stu-id="a0ea9-395">Description</span></span>|  
|------------------|-----------------|  
|<span data-ttu-id="a0ea9-396">**Podřízený**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-396">**Sink**</span></span>|<span data-ttu-id="a0ea9-397">Na této stránce najdete v popisu jinde.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-397">See description elsewhere on this page.</span></span>|  

## <a name="sink-element"></a><span data-ttu-id="a0ea9-398">Jímky – Element</span><span class="sxs-lookup"><span data-stu-id="a0ea9-398">Sink Element</span></span>
 <span data-ttu-id="a0ea9-399">*Strom: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - jímka*</span><span class="sxs-lookup"><span data-stu-id="a0ea9-399">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink*</span></span>

 <span data-ttu-id="a0ea9-400">Přidána do verze 1.5.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-400">Added in version 1.5.</span></span>  

 <span data-ttu-id="a0ea9-401">Definuje diagnostická data toosend umístění.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-401">Defines locations toosend diagnostic data to.</span></span> <span data-ttu-id="a0ea9-402">Například hello služby Application Insights.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-402">For example, hello Application Insights service.</span></span>  

|<span data-ttu-id="a0ea9-403">Atribut</span><span class="sxs-lookup"><span data-stu-id="a0ea9-403">Attribute</span></span>|<span data-ttu-id="a0ea9-404">Typ</span><span class="sxs-lookup"><span data-stu-id="a0ea9-404">Type</span></span>|<span data-ttu-id="a0ea9-405">Popis</span><span class="sxs-lookup"><span data-stu-id="a0ea9-405">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="a0ea9-406">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-406">**name**</span></span>|<span data-ttu-id="a0ea9-407">Řetězec</span><span class="sxs-lookup"><span data-stu-id="a0ea9-407">string</span></span>|<span data-ttu-id="a0ea9-408">Řetězec identifikující hello sinkname.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-408">A string identifying hello sinkname.</span></span>|  

|<span data-ttu-id="a0ea9-409">Element</span><span class="sxs-lookup"><span data-stu-id="a0ea9-409">Element</span></span>|<span data-ttu-id="a0ea9-410">Typ</span><span class="sxs-lookup"><span data-stu-id="a0ea9-410">Type</span></span>|<span data-ttu-id="a0ea9-411">Popis</span><span class="sxs-lookup"><span data-stu-id="a0ea9-411">Description</span></span>|  
|-------------|----------|-----------------|  
|<span data-ttu-id="a0ea9-412">**Application Insights**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-412">**Application Insights**</span></span>|<span data-ttu-id="a0ea9-413">Řetězec</span><span class="sxs-lookup"><span data-stu-id="a0ea9-413">string</span></span>|<span data-ttu-id="a0ea9-414">Použít pouze při odesílání dat tooApplication statistiky.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-414">Used only when sending data tooApplication Insights.</span></span> <span data-ttu-id="a0ea9-415">Obsahovat hello klíč instrumentace pro aktivní účet Application Insights, máte přístup.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-415">Contain hello Instrumentation Key for an active Application Insights account that you have access to.</span></span>|  
|<span data-ttu-id="a0ea9-416">**Kanály**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-416">**Channels**</span></span>|<span data-ttu-id="a0ea9-417">Řetězec</span><span class="sxs-lookup"><span data-stu-id="a0ea9-417">string</span></span>|<span data-ttu-id="a0ea9-418">Jeden pro každou další filtrování, který datového proudu, který jste</span><span class="sxs-lookup"><span data-stu-id="a0ea9-418">One for each additional filtering that stream that you</span></span>|  

## <a name="channels-element"></a><span data-ttu-id="a0ea9-419">Element kanály</span><span class="sxs-lookup"><span data-stu-id="a0ea9-419">Channels Element</span></span>  
 <span data-ttu-id="a0ea9-420">*Stromové struktury: Kanály SinksConfig - jímka - kořenové - DiagnosticsConfiguration - PublicConfig - WadCFG-*</span><span class="sxs-lookup"><span data-stu-id="a0ea9-420">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink - Channels*</span></span>

 <span data-ttu-id="a0ea9-421">Přidána do verze 1.5.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-421">Added in version 1.5.</span></span>  

 <span data-ttu-id="a0ea9-422">Definuje filtry pro datové proudy prošla jímku dat protokolu.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-422">Defines filters for streams of log data passing through a sink.</span></span>  

|<span data-ttu-id="a0ea9-423">Element</span><span class="sxs-lookup"><span data-stu-id="a0ea9-423">Element</span></span>|<span data-ttu-id="a0ea9-424">Typ</span><span class="sxs-lookup"><span data-stu-id="a0ea9-424">Type</span></span>|<span data-ttu-id="a0ea9-425">Popis</span><span class="sxs-lookup"><span data-stu-id="a0ea9-425">Description</span></span>|  
|-------------|----------|-----------------|  
|<span data-ttu-id="a0ea9-426">**Kanál**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-426">**Channel**</span></span>|<span data-ttu-id="a0ea9-427">Řetězec</span><span class="sxs-lookup"><span data-stu-id="a0ea9-427">string</span></span>|<span data-ttu-id="a0ea9-428">Na této stránce najdete v popisu jinde.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-428">See description elsewhere on this page.</span></span>|  

## <a name="channel-element"></a><span data-ttu-id="a0ea9-429">Element kanálu</span><span class="sxs-lookup"><span data-stu-id="a0ea9-429">Channel Element</span></span>
 <span data-ttu-id="a0ea9-430">*Strom: Kořenové - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - jímka - kanály - kanál*</span><span class="sxs-lookup"><span data-stu-id="a0ea9-430">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink - Channels - Channel*</span></span>

 <span data-ttu-id="a0ea9-431">Přidána do verze 1.5.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-431">Added in version 1.5.</span></span>  

 <span data-ttu-id="a0ea9-432">Definuje diagnostická data toosend umístění.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-432">Defines locations toosend diagnostic data to.</span></span> <span data-ttu-id="a0ea9-433">Například hello služby Application Insights.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-433">For example, hello Application Insights service.</span></span>  

|<span data-ttu-id="a0ea9-434">Atributy</span><span class="sxs-lookup"><span data-stu-id="a0ea9-434">Attributes</span></span>|<span data-ttu-id="a0ea9-435">Typ</span><span class="sxs-lookup"><span data-stu-id="a0ea9-435">Type</span></span>|<span data-ttu-id="a0ea9-436">Popis</span><span class="sxs-lookup"><span data-stu-id="a0ea9-436">Description</span></span>|  
|----------------|----------|-----------------|  
|<span data-ttu-id="a0ea9-437">**logLevel**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-437">**logLevel**</span></span>|<span data-ttu-id="a0ea9-438">**řetězec**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-438">**string**</span></span>|<span data-ttu-id="a0ea9-439">Určuje hello minimální úroveň závažnosti pro položky protokolu, které se přenáší.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-439">Specifies hello minimum severity level for log entries that are transferred.</span></span> <span data-ttu-id="a0ea9-440">Hello výchozí hodnota je **Undefined**, která přenáší všechny protokoly.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-440">hello default value is **Undefined**, which transfers all logs.</span></span> <span data-ttu-id="a0ea9-441">Další možné hodnoty (v pořadí, ve většině tooleast informací) jsou **podrobné**, **informace**, **upozornění**, **chyba**a **Kritické**.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-441">Other possible values (in order of most tooleast information) are **Verbose**, **Information**, **Warning**, **Error**, and **Critical**.</span></span>|  
|<span data-ttu-id="a0ea9-442">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-442">**name**</span></span>|<span data-ttu-id="a0ea9-443">**řetězec**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-443">**string**</span></span>|<span data-ttu-id="a0ea9-444">Jedinečný název kanálu toorefer hello k</span><span class="sxs-lookup"><span data-stu-id="a0ea9-444">A unique name of hello channel toorefer to</span></span>|  


## <a name="privateconfig-element"></a><span data-ttu-id="a0ea9-445">PrivateConfig Element</span><span class="sxs-lookup"><span data-stu-id="a0ea9-445">PrivateConfig Element</span></span> 
 <span data-ttu-id="a0ea9-446">*Stromové struktury: PrivateConfig kořenové - DiagnosticsConfiguration-*</span><span class="sxs-lookup"><span data-stu-id="a0ea9-446">*Tree: Root - DiagnosticsConfiguration - PrivateConfig*</span></span>

 <span data-ttu-id="a0ea9-447">Přidána do verze 1.3.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-447">Added in version 1.3.</span></span>  

 <span data-ttu-id="a0ea9-448">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="a0ea9-448">Optional</span></span>  

 <span data-ttu-id="a0ea9-449">Ukládá podrobností soukromého hello účtu úložiště hello (název, klíč a koncového bodu).</span><span class="sxs-lookup"><span data-stu-id="a0ea9-449">Stores hello private details of hello storage account (name, key, and endpoint).</span></span> <span data-ttu-id="a0ea9-450">Tyto informace se odesílají toohello virtuálního počítače, ale nelze načíst z něj.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-450">This information is sent toohello virtual machine, but cannot be retrieved from it.</span></span>  

|<span data-ttu-id="a0ea9-451">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="a0ea9-451">Child Elements</span></span>|<span data-ttu-id="a0ea9-452">Popis</span><span class="sxs-lookup"><span data-stu-id="a0ea9-452">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="a0ea9-453">**StorageAccount**</span><span class="sxs-lookup"><span data-stu-id="a0ea9-453">**StorageAccount**</span></span>|<span data-ttu-id="a0ea9-454">Hello toouse účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-454">hello storage account toouse.</span></span> <span data-ttu-id="a0ea9-455">Hello následující atributy jsou požadovány</span><span class="sxs-lookup"><span data-stu-id="a0ea9-455">hello following attributes are required</span></span><br /><br /> <span data-ttu-id="a0ea9-456">- **název** hello – název účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-456">- **name** - hello name of hello storage account.</span></span><br /><br /> <span data-ttu-id="a0ea9-457">- **klíč** – hello klíče účtu úložiště toohello.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-457">- **key** - hello key toohello storage account.</span></span><br /><br /> <span data-ttu-id="a0ea9-458">- **koncový bod** -tooaccess koncový bod hello hello účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-458">- **endpoint** - hello endpoint tooaccess hello storage account.</span></span> <br /><br /> <span data-ttu-id="a0ea9-459">-**sasToken** (přidají 1.8.1)-tokenu SAS místo klíč účtu úložiště můžete zadat v privátní konfigurace hello. Pokud je zadán, klíč účtu úložiště hello se ignoruje.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-459">-**sasToken** (added 1.8.1)- You can specify an SAS token instead of a storage account key in hello private config. If provided, hello storage account key is ignored.</span></span> <br /><span data-ttu-id="a0ea9-460">Požadavky pro hello SAS Token:</span><span class="sxs-lookup"><span data-stu-id="a0ea9-460">Requirements for hello SAS Token:</span></span> <br /><span data-ttu-id="a0ea9-461">– Podporuje pouze tokenu SAS účtu</span><span class="sxs-lookup"><span data-stu-id="a0ea9-461">- Supports account SAS token only</span></span> <br /><span data-ttu-id="a0ea9-462">- *b*, *t* jsou požadovány typy služby.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-462">- *b*, *t* service types are required.</span></span> <br /> <span data-ttu-id="a0ea9-463">- **, *c*, *u*, *w* jsou požadována oprávnění.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-463">- *a*, *c*, *u*, *w* permissions are required.</span></span> <br /> <span data-ttu-id="a0ea9-464">- *c*, *o* jsou požadovány typy prostředků.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-464">- *c*, *o* resource types are required.</span></span> <br /> <span data-ttu-id="a0ea9-465">– Podporuje pouze protokol HTTPS hello</span><span class="sxs-lookup"><span data-stu-id="a0ea9-465">- Supports hello HTTPS protocol only</span></span> <br /> <span data-ttu-id="a0ea9-466">-Spuštění a čas vypršení platnosti musí být platné.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-466">- Start and expiry time must be valid.</span></span>|  


## <a name="isenabled-element"></a><span data-ttu-id="a0ea9-467">Element hodnotu IsEnabled</span><span class="sxs-lookup"><span data-stu-id="a0ea9-467">IsEnabled Element</span></span>  
 <span data-ttu-id="a0ea9-468">*Stromové struktury: Hodnotu IsEnabled kořenové - DiagnosticsConfiguration-*</span><span class="sxs-lookup"><span data-stu-id="a0ea9-468">*Tree: Root - DiagnosticsConfiguration - IsEnabled*</span></span>

 <span data-ttu-id="a0ea9-469">Logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-469">Boolean.</span></span> <span data-ttu-id="a0ea9-470">Použití `true` tooenable hello diagnostiky nebo `false` toodisable hello diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="a0ea9-470">Use `true` tooenable hello diagnostics or `false` toodisable hello diagnostics.</span></span>
