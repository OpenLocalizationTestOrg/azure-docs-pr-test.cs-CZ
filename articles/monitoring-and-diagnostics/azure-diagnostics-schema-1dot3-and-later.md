---
title: "Rozšíření Azure Diagnostics 1.3 a novější schéma konfigurace | Microsoft Docs"
description: "Verze schématu 1.3 a novější Azure diagnostics dodávána jako součást Microsoft Azure SDK 2.4 a později."
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
ms.openlocfilehash: 0d814825fb08452238a254ccd30bde230380c74c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-diagnostics-13-and-later-configuration-schema"></a><span data-ttu-id="ed8b4-103">Azure Diagnostics 1.3 a novější schéma konfigurace</span><span class="sxs-lookup"><span data-stu-id="ed8b4-103">Azure Diagnostics 1.3 and later configuration schema</span></span>
> [!NOTE]
> <span data-ttu-id="ed8b4-104">Rozšíření diagnostiky Azure je komponenta, kterou používá ke shromažďování čítačů výkonu a další statistiky z:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-104">The Azure Diagnostics extension is the component used to collect performance counters and other statistics from:</span></span>
> - <span data-ttu-id="ed8b4-105">Azure Virtual Machines</span><span class="sxs-lookup"><span data-stu-id="ed8b4-105">Azure Virtual Machines</span></span> 
> - <span data-ttu-id="ed8b4-106">Škálovací sady virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="ed8b4-106">Virtual Machine Scale Sets</span></span>
> - <span data-ttu-id="ed8b4-107">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="ed8b4-107">Service Fabric</span></span> 
> - <span data-ttu-id="ed8b4-108">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="ed8b4-108">Cloud Services</span></span> 
> - <span data-ttu-id="ed8b4-109">Network Security Groups (Skupiny zabezpečení sítě)</span><span class="sxs-lookup"><span data-stu-id="ed8b4-109">Network Security Groups</span></span>
> 
> <span data-ttu-id="ed8b4-110">Tato stránka je pouze relevantní, pokud používáte některou z těchto služeb.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-110">This page is only relevant if you are using one of these services.</span></span>

<span data-ttu-id="ed8b4-111">Tato stránka je platná pro verze 1.3 a novější (Azure SDK 2.4 nebo novější).</span><span class="sxs-lookup"><span data-stu-id="ed8b4-111">This page is valid for versions 1.3 and newer (Azure SDK 2.4 and newer).</span></span> <span data-ttu-id="ed8b4-112">Novější konfigurační oddíly jsou vloženy do komentáře k zobrazení, v jaké verze byly přidány.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-112">Newer configuration sections are commented to show in what version they were added.</span></span>  

<span data-ttu-id="ed8b4-113">V souboru konfigurace zde popsané slouží k nastavení konfigurace diagnostiky při spuštění diagnostiky monitorování.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-113">The configuration file described here is used to set diagnostic configuration settings when the diagnostics monitor starts.</span></span>  

<span data-ttu-id="ed8b4-114">Rozšíření se používá ve spojení s dalšími produkty Microsoftu diagnostiky jako monitorování Azure, Application Insights a analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-114">The extension is used in conjunction with other Microsoft diagnostics products like Azure Monitor, Application Insights, and Log Analytics.</span></span>



<span data-ttu-id="ed8b4-115">Stáhněte si definici veřejné konfigurace souboru schématu spuštěním následujícího příkazu Powershellu:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-115">Download the public configuration file schema definition by executing the following PowerShell command:</span></span>  

```powershell  
(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File –Encoding utf8 -FilePath 'C:\temp\WadConfig.xsd'  
```  

<span data-ttu-id="ed8b4-116">Další informace o používání Azure Diagnostics najdete v tématu [rozšíření diagnostiky Azure](azure-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="ed8b4-116">For more information about using Azure Diagnostics, see [Azure Diagnostics Extension](azure-diagnostics.md).</span></span>  

## <a name="example-of-the-diagnostics-configuration-file"></a><span data-ttu-id="ed8b4-117">Příklad konfiguračního souboru diagnostiky</span><span class="sxs-lookup"><span data-stu-id="ed8b4-117">Example of the diagnostics configuration file</span></span>  
 <span data-ttu-id="ed8b4-118">Následující příklad ukazuje konfigurační soubor typické diagnostics:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-118">The following example shows a typical diagnostics configuration file:</span></span>  

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

<span data-ttu-id="ed8b4-119">JSON ekvivalent předchozí konfiguračního souboru XML.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-119">JSON equivalent of the previous XML configuration file.</span></span> 

<span data-ttu-id="ed8b4-120">PublicConfig a PrivateConfig jsou oddělené, protože ve většině případů použití json jsou předávány jako jiné proměnné.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-120">The PublicConfig and PrivateConfig are separated because in most json usage cases, they are passed as different variables.</span></span> <span data-ttu-id="ed8b4-121">Tyto případy patří šablony Resource Manageru, sady škálování virtuálního počítače prostředí PowerShell a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-121">These cases include Resource Manager templates, Virtual Machine Scale set PowerShell, and Visual Studio.</span></span> 

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

## <a name="reading-this-page"></a><span data-ttu-id="ed8b4-122">Čtení této stránce</span><span class="sxs-lookup"><span data-stu-id="ed8b4-122">Reading this page</span></span>  
 <span data-ttu-id="ed8b4-123">Značky následující jsou přibližně v pořadí uvedeném v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-123">The tags following are roughly in order shown in the preceding example.</span></span>  <span data-ttu-id="ed8b4-124">Pokud nevidíte úplný popis kde očekáváte, vyhledejte stránce elementu nebo atributu.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-124">If you don't see a full description where you expect it, search the page for the element or attribute.</span></span>  

## <a name="common-attribute-types"></a><span data-ttu-id="ed8b4-125">Běžné typy atributů</span><span class="sxs-lookup"><span data-stu-id="ed8b4-125">Common Attribute Types</span></span>  
 <span data-ttu-id="ed8b4-126">**scheduledTransferPeriod** atributu se zobrazí v několika elementy.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-126">**scheduledTransferPeriod** attribute appears in several elements.</span></span> <span data-ttu-id="ed8b4-127">Je interval mezi naplánované přenosy do úložiště zaokrouhlený nahoru na nejbližší minutu.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-127">It is the interval between scheduled transfers to storage rounded up to the nearest minute.</span></span> <span data-ttu-id="ed8b4-128">Hodnota je [XML "Doba trvání datového typu."](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span><span class="sxs-lookup"><span data-stu-id="ed8b4-128">The value is an [XML “Duration Data Type.”](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span></span>


## <a name="diagnosticsconfiguration-element"></a><span data-ttu-id="ed8b4-129">DiagnosticsConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="ed8b4-129">DiagnosticsConfiguration Element</span></span>  
 <span data-ttu-id="ed8b4-130">*Stromu: Kořen - DiagnosticsConfiguration*</span><span class="sxs-lookup"><span data-stu-id="ed8b4-130">*Tree: Root - DiagnosticsConfiguration*</span></span>

<span data-ttu-id="ed8b4-131">Přidána do verze 1.3.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-131">Added in version 1.3.</span></span>  

<span data-ttu-id="ed8b4-132">Element nejvyšší úrovně v souboru konfigurace diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-132">The top-level element of the diagnostics configuration file.</span></span>  

<span data-ttu-id="ed8b4-133">**Atribut** xmlns – obor názvů XML pro konfigurační soubor diagnostiky je:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-133">**Attribute**  xmlns - The XML namespace for the diagnostics configuration file is:</span></span>  
<span data-ttu-id="ed8b4-134">http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration</span><span class="sxs-lookup"><span data-stu-id="ed8b4-134">http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration</span></span>  


|<span data-ttu-id="ed8b4-135">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="ed8b4-135">Child Elements</span></span>|<span data-ttu-id="ed8b4-136">Popis</span><span class="sxs-lookup"><span data-stu-id="ed8b4-136">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="ed8b4-137">**PublicConfig**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-137">**PublicConfig**</span></span>|<span data-ttu-id="ed8b4-138">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-138">Required.</span></span> <span data-ttu-id="ed8b4-139">Na této stránce najdete v popisu jinde.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-139">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="ed8b4-140">**PrivateConfig**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-140">**PrivateConfig**</span></span>|<span data-ttu-id="ed8b4-141">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-141">Optional.</span></span> <span data-ttu-id="ed8b4-142">Na této stránce najdete v popisu jinde.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-142">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="ed8b4-143">**Hodnotu IsEnabled**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-143">**IsEnabled**</span></span>|<span data-ttu-id="ed8b4-144">Logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-144">Boolean.</span></span> <span data-ttu-id="ed8b4-145">Na této stránce najdete v popisu jinde.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-145">See description elsewhere on this page.</span></span>|  

## <a name="publicconfig-element"></a><span data-ttu-id="ed8b4-146">PublicConfig Element</span><span class="sxs-lookup"><span data-stu-id="ed8b4-146">PublicConfig Element</span></span>  
 <span data-ttu-id="ed8b4-147">*Stromové struktury: PublicConfig kořenové - DiagnosticsConfiguration-*</span><span class="sxs-lookup"><span data-stu-id="ed8b4-147">*Tree: Root - DiagnosticsConfiguration - PublicConfig*</span></span>

 <span data-ttu-id="ed8b4-148">Popisuje konfiguraci veřejné diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-148">Describes the public diagnostics configuration.</span></span>  

|<span data-ttu-id="ed8b4-149">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="ed8b4-149">Child Elements</span></span>|<span data-ttu-id="ed8b4-150">Popis</span><span class="sxs-lookup"><span data-stu-id="ed8b4-150">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="ed8b4-151">**WadCfg**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-151">**WadCfg**</span></span>|<span data-ttu-id="ed8b4-152">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-152">Required.</span></span> <span data-ttu-id="ed8b4-153">Na této stránce najdete v popisu jinde.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-153">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="ed8b4-154">**StorageAccount**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-154">**StorageAccount**</span></span>|<span data-ttu-id="ed8b4-155">Název účtu úložiště Azure pro ukládání dat v.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-155">The name of the Azure Storage account to store the data in.</span></span> <span data-ttu-id="ed8b4-156">Můžete také zadat jako parametr při spuštění rutiny Set-AzureServiceDiagnosticsExtension.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-156">May also be specified as a parameter when executing the Set-AzureServiceDiagnosticsExtension cmdlet.</span></span>|  
|<span data-ttu-id="ed8b4-157">**StorageType**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-157">**StorageType**</span></span>|<span data-ttu-id="ed8b4-158">Může být *tabulky*, *Blob*, nebo *TableAndBlob*.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-158">Can be *Table*, *Blob*, or *TableAndBlob*.</span></span> <span data-ttu-id="ed8b4-159">Tabulka je výchozí.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-159">Table is default.</span></span> <span data-ttu-id="ed8b4-160">Při výběru TableAndBlob diagnostická data se zapisují dvakrát – jednou pro každý typ.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-160">When TableAndBlob is chosen, diagnostic data is written twice -- once to each type.</span></span>|  
|<span data-ttu-id="ed8b4-161">**LocalResourceDirectory**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-161">**LocalResourceDirectory**</span></span>|<span data-ttu-id="ed8b4-162">Adresář na virtuálním počítači, kde Monitoring Agent ukládá data události.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-162">The directory on the virtual machine where the Monitoring Agent stores event data.</span></span> <span data-ttu-id="ed8b4-163">Pokud ne, nastavit, používá výchozí adresář:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-163">If not, set, the default directory is used:</span></span><br /><br /> <span data-ttu-id="ed8b4-164">Pro roli pracovního procesu nebo webové:`C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`</span><span class="sxs-lookup"><span data-stu-id="ed8b4-164">For a Worker/web role: `C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`</span></span><br /><br /> <span data-ttu-id="ed8b4-165">Pro virtuální počítač:`C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`</span><span class="sxs-lookup"><span data-stu-id="ed8b4-165">For a Virtual Machine: `C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`</span></span><br /><br /> <span data-ttu-id="ed8b4-166">Povinné atributy jsou:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-166">Required attributes are:</span></span><br /><br /> <span data-ttu-id="ed8b4-167">- **cesta** -adresáři na serveru, který má být používána Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-167">- **path** - The directory on the system to be used by Azure Diagnostics.</span></span><br /><br /> <span data-ttu-id="ed8b4-168">- **expandEnvironment** -Určuje, zda jsou v názvu cesty rozbalit proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-168">- **expandEnvironment** - Controls whether environment variables are expanded in the path name.</span></span>|  

## <a name="wadcfg-element"></a><span data-ttu-id="ed8b4-169">WadCFG Element</span><span class="sxs-lookup"><span data-stu-id="ed8b4-169">WadCFG Element</span></span>  
 <span data-ttu-id="ed8b4-170">*Strom: Root - DiagnosticsConfiguration - PublicConfig - WadCFG*</span><span class="sxs-lookup"><span data-stu-id="ed8b4-170">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG*</span></span>
 
 <span data-ttu-id="ed8b4-171">Identifikuje a nakonfiguruje telemetrická data, které se mají shromažďovat.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-171">Identifies and configures the telemetry data to be collected.</span></span>  


## <a name="diagnosticmonitorconfiguration-element"></a><span data-ttu-id="ed8b4-172">DiagnosticMonitorConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="ed8b4-172">DiagnosticMonitorConfiguration Element</span></span> 
 <span data-ttu-id="ed8b4-173">*Stromové struktury: DiagnosticMonitorConfiguration PublicConfig - WadCFG - kořenové - DiagnosticsConfiguration-*</span><span class="sxs-lookup"><span data-stu-id="ed8b4-173">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration*</span></span>

 <span data-ttu-id="ed8b4-174">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="ed8b4-174">Required</span></span> 

|<span data-ttu-id="ed8b4-175">Atributy</span><span class="sxs-lookup"><span data-stu-id="ed8b4-175">Attributes</span></span>|<span data-ttu-id="ed8b4-176">Popis</span><span class="sxs-lookup"><span data-stu-id="ed8b4-176">Description</span></span>|  
|----------------|-----------------|  
| <span data-ttu-id="ed8b4-177">**overallQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-177">**overallQuotaInMB**</span></span> | <span data-ttu-id="ed8b4-178">Maximální množství místa na místní disk, který může být využívány službou různé typy diagnostická data shromažďovaná společností Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-178">The maximum amount of local disk space that may be consumed by the various types of diagnostic data collected by Azure Diagnostics.</span></span> <span data-ttu-id="ed8b4-179">Výchozí nastavení je 5 120 MB.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-179">The default setting is 5120 MB.</span></span><br />
|<span data-ttu-id="ed8b4-180">**useProxyServer**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-180">**useProxyServer**</span></span> | <span data-ttu-id="ed8b4-181">Konfigurace Azure Diagnostics chcete použít nastavení proxy serveru jako sada v nastavení aplikace Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-181">Configure Azure Diagnostics to use the proxy server settings as set in IE settings.</span></span>|  

<br /> <br />

|<span data-ttu-id="ed8b4-182">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="ed8b4-182">Child Elements</span></span>|<span data-ttu-id="ed8b4-183">Popis</span><span class="sxs-lookup"><span data-stu-id="ed8b4-183">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="ed8b4-184">**CrashDumps**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-184">**CrashDumps**</span></span>|<span data-ttu-id="ed8b4-185">Na této stránce najdete v popisu jinde.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-185">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="ed8b4-186">**DiagnosticInfrastructureLogs**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-186">**DiagnosticInfrastructureLogs**</span></span>|<span data-ttu-id="ed8b4-187">Povolte shromažďování protokolů generovaných Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-187">Enable collection of logs generated by Azure Diagnostics.</span></span> <span data-ttu-id="ed8b4-188">Infrastruktura diagnostické protokoly jsou užitečné pro řešení potíží s samotného systému diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-188">The diagnostic infrastructure logs are useful for troubleshooting the diagnostics system itself.</span></span> <span data-ttu-id="ed8b4-189">Volitelné atributy jsou:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-189">Optional attributes are:</span></span><br /><br /> <span data-ttu-id="ed8b4-190">- **scheduledTransferLogLevelFilter** -nakonfiguruje závažnost minimální úroveň shromážděné protokoly.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-190">- **scheduledTransferLogLevelFilter** - Configures the minimum severity level of the logs collected.</span></span><br /><br /> <span data-ttu-id="ed8b4-191">- **scheduledTransferPeriod** -interval mezi naplánované přenosy do úložiště zaokrouhlený nahoru na nejbližší minutu.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-191">- **scheduledTransferPeriod** - The interval between scheduled transfers to storage rounded up to the nearest minute.</span></span> <span data-ttu-id="ed8b4-192">Hodnota je [XML "Doba trvání datového typu."](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span><span class="sxs-lookup"><span data-stu-id="ed8b4-192">The value is an [XML “Duration Data Type.”](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span></span> |  
|<span data-ttu-id="ed8b4-193">**Adresáře**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-193">**Directories**</span></span>|<span data-ttu-id="ed8b4-194">Na této stránce najdete v popisu jinde.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-194">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="ed8b4-195">**EtwProviders**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-195">**EtwProviders**</span></span>|<span data-ttu-id="ed8b4-196">Na této stránce najdete v popisu jinde.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-196">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="ed8b4-197">**Metriky**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-197">**Metrics**</span></span>|<span data-ttu-id="ed8b4-198">Na této stránce najdete v popisu jinde.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-198">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="ed8b4-199">**Čítače výkonu**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-199">**PerformanceCounters**</span></span>|<span data-ttu-id="ed8b4-200">Na této stránce najdete v popisu jinde.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-200">See description elsewhere on this page.</span></span>|  
|<span data-ttu-id="ed8b4-201">**WindowsEventLog**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-201">**WindowsEventLog**</span></span>|<span data-ttu-id="ed8b4-202">Na této stránce najdete v popisu jinde.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-202">See description elsewhere on this page.</span></span>| 
|<span data-ttu-id="ed8b4-203">**DockerSources**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-203">**DockerSources**</span></span>|<span data-ttu-id="ed8b4-204">Na této stránce najdete v popisu jinde.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-204">See description elsewhere on this page.</span></span> | 



## <a name="crashdumps-element"></a><span data-ttu-id="ed8b4-205">CrashDumps Element</span><span class="sxs-lookup"><span data-stu-id="ed8b4-205">CrashDumps Element</span></span>  
 <span data-ttu-id="ed8b4-206">*Stromu: - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - CrashDumps kořen*</span><span class="sxs-lookup"><span data-stu-id="ed8b4-206">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - CrashDumps*</span></span>
 
 <span data-ttu-id="ed8b4-207">Povolení shromažďování těchto výpisy stavu systému.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-207">Enable the collection of crash dumps.</span></span>  

|<span data-ttu-id="ed8b4-208">Atributy</span><span class="sxs-lookup"><span data-stu-id="ed8b4-208">Attributes</span></span>|<span data-ttu-id="ed8b4-209">Popis</span><span class="sxs-lookup"><span data-stu-id="ed8b4-209">Description</span></span>|  
|----------------|-----------------|  
|<span data-ttu-id="ed8b4-210">**containerName**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-210">**containerName**</span></span>|<span data-ttu-id="ed8b4-211">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-211">Optional.</span></span> <span data-ttu-id="ed8b4-212">Název kontejneru objektů blob v účtu úložiště Azure, který se má použít k uložení výpisy stavu systému.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-212">The name of the blob container in your Azure Storage account to be used to store crash dumps.</span></span>|  
|<span data-ttu-id="ed8b4-213">**crashDumpType**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-213">**crashDumpType**</span></span>|<span data-ttu-id="ed8b4-214">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-214">Optional.</span></span>  <span data-ttu-id="ed8b4-215">Nakonfiguruje Azure Diagnostics ke shromažďování výpisů paměti mini nebo úplné selhat.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-215">Configures Azure Diagnostics to collect mini or full crash dumps.</span></span>|  
|<span data-ttu-id="ed8b4-216">**directoryQuotaPercentage**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-216">**directoryQuotaPercentage**</span></span>|<span data-ttu-id="ed8b4-217">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-217">Optional.</span></span>  <span data-ttu-id="ed8b4-218">Nakonfiguruje procento **overallQuotaInMB** jako vyhrazené pro výpisů stavu systému ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-218">Configures the percentage of **overallQuotaInMB** to be reserved for crash dumps on the VM.</span></span>|  

|<span data-ttu-id="ed8b4-219">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="ed8b4-219">Child Elements</span></span>|<span data-ttu-id="ed8b4-220">Popis</span><span class="sxs-lookup"><span data-stu-id="ed8b4-220">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="ed8b4-221">**CrashDumpConfiguration**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-221">**CrashDumpConfiguration**</span></span>|<span data-ttu-id="ed8b4-222">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-222">Required.</span></span> <span data-ttu-id="ed8b4-223">Definuje hodnoty konfigurace pro jednotlivé procesy.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-223">Defines configuration values for each process.</span></span><br /><br /> <span data-ttu-id="ed8b4-224">Je také nutný následující atribut:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-224">The following attribute is also required:</span></span><br /><br /> <span data-ttu-id="ed8b4-225">**název_procesu** -název procesu chcete shromažďovat výpis stavu pro Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-225">**processName** - The name of the process you want Azure Diagnostics to collect a crash dump for.</span></span>|  

## <a name="directories-element"></a><span data-ttu-id="ed8b4-226">Element adresáře</span><span class="sxs-lookup"><span data-stu-id="ed8b4-226">Directories Element</span></span> 
 <span data-ttu-id="ed8b4-227">*Strom: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - adresáře*</span><span class="sxs-lookup"><span data-stu-id="ed8b4-227">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration -  Directories*</span></span>

 <span data-ttu-id="ed8b4-228">Povoluje shromažďování obsah adresáře, protokoly žádost o přístup služby IIS se nezdařilo nebo protokoly služby IIS.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-228">Enables the collection of the contents of a directory, IIS failed access request logs and/or IIS logs.</span></span>  

 <span data-ttu-id="ed8b4-229">Volitelné **scheduledTransferPeriod** atribut.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-229">Optional **scheduledTransferPeriod** attribute.</span></span> <span data-ttu-id="ed8b4-230">Viz vysvětlení dříve.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-230">See explanation earlier.</span></span>  

|<span data-ttu-id="ed8b4-231">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="ed8b4-231">Child Elements</span></span>|<span data-ttu-id="ed8b4-232">Popis</span><span class="sxs-lookup"><span data-stu-id="ed8b4-232">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="ed8b4-233">**IISLogs**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-233">**IISLogs**</span></span>|<span data-ttu-id="ed8b4-234">Tento element včetně v konfiguraci povoluje shromažďování protokoly služby IIS:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-234">Including this element in the configuration enables the collection of IIS logs:</span></span><br /><br /> <span data-ttu-id="ed8b4-235">**containerName** -název kontejneru objektů blob v účtu úložiště Azure, který se má použít k ukládání protokolů služby IIS.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-235">**containerName** - The name of the blob container in your Azure Storage account to be used to store the IIS logs.</span></span>|   
|<span data-ttu-id="ed8b4-236">**FailedRequestLogs**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-236">**FailedRequestLogs**</span></span>|<span data-ttu-id="ed8b4-237">Umožňuje shromažďování protokolů o neúspěšných požadavků na web služby IIS nebo aplikaci, včetně tento prvek v konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-237">Including this element in the configuration enables collection of logs about failed requests to an IIS site or application.</span></span> <span data-ttu-id="ed8b4-238">Je také nutné povolit trasování možnosti v části **systému. Webový server** v **Web.config**.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-238">You must also enable tracing options under **system.WebServer** in **Web.config**.</span></span>|  
|<span data-ttu-id="ed8b4-239">**Zdroje dat**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-239">**DataSources**</span></span>|<span data-ttu-id="ed8b4-240">Seznam adresáře, které chcete monitorovat.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-240">A list of directories to monitor.</span></span>| 




## <a name="datasources-element"></a><span data-ttu-id="ed8b4-241">Element zdrojů dat</span><span class="sxs-lookup"><span data-stu-id="ed8b4-241">DataSources Element</span></span>  
 <span data-ttu-id="ed8b4-242">*Stromové struktury: Zdroje dat PublicConfig - WadCFG - DiagnosticMonitorConfiguration - adresáře - kořenové - DiagnosticsConfiguration-*</span><span class="sxs-lookup"><span data-stu-id="ed8b4-242">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Directories - DataSources*</span></span>

 <span data-ttu-id="ed8b4-243">Seznam adresáře, které chcete monitorovat.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-243">A list of directories to monitor.</span></span>  

|<span data-ttu-id="ed8b4-244">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="ed8b4-244">Child Elements</span></span>|<span data-ttu-id="ed8b4-245">Popis</span><span class="sxs-lookup"><span data-stu-id="ed8b4-245">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="ed8b4-246">**DirectoryConfiguration**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-246">**DirectoryConfiguration**</span></span>|<span data-ttu-id="ed8b4-247">Vyžaduje se.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-247">Required.</span></span> <span data-ttu-id="ed8b4-248">Požadovaný atribut:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-248">Required attribute:</span></span><br /><br /> <span data-ttu-id="ed8b4-249">**containerName** -název kontejneru objektů blob v Azure Storage účtu, který se má použít k ukládání souborů protokolu.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-249">**containerName** - The name of the blob container in your Azure Storage account that to be used to store the log files.</span></span>|  





## <a name="directoryconfiguration-element"></a><span data-ttu-id="ed8b4-250">DirectoryConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="ed8b4-250">DirectoryConfiguration Element</span></span>  
 <span data-ttu-id="ed8b4-251">*Stromu: Zdroje dat PublicConfig - WadCFG - DiagnosticMonitorConfiguration - adresáře - kořenové - DiagnosticsConfiguration - - DirectoryConfiguration*</span><span class="sxs-lookup"><span data-stu-id="ed8b4-251">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Directories - DataSources - DirectoryConfiguration*</span></span>

 <span data-ttu-id="ed8b4-252">Může obsahovat buď **absolutní** nebo **LocalResource** elementu, ale ne obojí.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-252">May include either the **Absolute** or **LocalResource** element but not both.</span></span>  

|<span data-ttu-id="ed8b4-253">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="ed8b4-253">Child Elements</span></span>|<span data-ttu-id="ed8b4-254">Popis</span><span class="sxs-lookup"><span data-stu-id="ed8b4-254">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="ed8b4-255">**Absolutní**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-255">**Absolute**</span></span>|<span data-ttu-id="ed8b4-256">Absolutní cesta k adresáři pro monitorování.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-256">The absolute path to the directory to monitor.</span></span> <span data-ttu-id="ed8b4-257">Vyžadují se následující atributy:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-257">The following attributes are required:</span></span><br /><br /> <span data-ttu-id="ed8b4-258">- **Cesta** -absolutní cestu k adresáři pro monitorování.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-258">- **Path** - The absolute path to the directory to monitor.</span></span><br /><br /> <span data-ttu-id="ed8b4-259">- **expandEnvironment** – konfiguruje, zda jsou rozbalit proměnné prostředí v cestě.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-259">- **expandEnvironment** - Configures whether environment variables in Path are expanded.</span></span>|  
|<span data-ttu-id="ed8b4-260">**LocalResource**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-260">**LocalResource**</span></span>|<span data-ttu-id="ed8b4-261">Cesta relativní k místní prostředek pro monitorování.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-261">The path relative to a local resource to monitor.</span></span> <span data-ttu-id="ed8b4-262">Povinné atributy jsou:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-262">Required attributes are:</span></span><br /><br /> <span data-ttu-id="ed8b4-263">- **Název** -místní prostředek, který obsahuje adresář, který chcete monitorovat</span><span class="sxs-lookup"><span data-stu-id="ed8b4-263">- **Name** - The local resource that contains the directory to monitor</span></span><br /><br /> <span data-ttu-id="ed8b4-264">- **relativePath** – relativní název, který obsahuje adresář, který chcete monitorovat cestu</span><span class="sxs-lookup"><span data-stu-id="ed8b4-264">- **relativePath** - The path relative to Name that contains the directory to monitor</span></span>|  



## <a name="etwproviders-element"></a><span data-ttu-id="ed8b4-265">EtwProviders Element</span><span class="sxs-lookup"><span data-stu-id="ed8b4-265">EtwProviders Element</span></span>  
 <span data-ttu-id="ed8b4-266">*Stromu: - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders kořen*</span><span class="sxs-lookup"><span data-stu-id="ed8b4-266">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders*</span></span>

 <span data-ttu-id="ed8b4-267">Nakonfiguruje shromažďování událostí trasování událostí pro Windows z EventSource nebo Manifest trasování událostí pro Windows na základě zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-267">Configures collection of ETW events from EventSource and/or ETW Manifest based providers.</span></span>  

|<span data-ttu-id="ed8b4-268">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="ed8b4-268">Child Elements</span></span>|<span data-ttu-id="ed8b4-269">Popis</span><span class="sxs-lookup"><span data-stu-id="ed8b4-269">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="ed8b4-270">**EtwEventSourceProviderConfiguration**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-270">**EtwEventSourceProviderConfiguration**</span></span>|<span data-ttu-id="ed8b4-271">Nakonfiguruje kolekce událostí generovaných [EventSource – třída](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="ed8b4-271">Configures collection of events generated from [EventSource Class](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).</span></span> <span data-ttu-id="ed8b4-272">Požadovaný atribut:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-272">Required attribute:</span></span><br /><br /> <span data-ttu-id="ed8b4-273">**Zprostředkovatel** -název třídy událostí EventSource.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-273">**provider** - The class name of the EventSource event.</span></span><br /><br /> <span data-ttu-id="ed8b4-274">Volitelné atributy jsou:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-274">Optional attributes are:</span></span><br /><br /> <span data-ttu-id="ed8b4-275">- **scheduledTransferLogLevelFilter** -úroveň závažnosti minimální přenést do účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-275">- **scheduledTransferLogLevelFilter** - The minimum severity level to transfer to your storage account.</span></span><br /><br /> <span data-ttu-id="ed8b4-276">- **scheduledTransferPeriod** -interval mezi naplánované přenosy do úložiště zaokrouhlený nahoru na nejbližší minutu.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-276">- **scheduledTransferPeriod** - The interval between scheduled transfers to storage rounded up to the nearest minute.</span></span> <span data-ttu-id="ed8b4-277">Hodnota je [XML "Doba trvání datového typu."](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span><span class="sxs-lookup"><span data-stu-id="ed8b4-277">The value is an [XML “Duration Data Type.”](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span></span> |  
|<span data-ttu-id="ed8b4-278">**EtwManifestProviderConfiguration**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-278">**EtwManifestProviderConfiguration**</span></span>|<span data-ttu-id="ed8b4-279">Požadovaný atribut:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-279">Required attribute:</span></span><br /><br /> <span data-ttu-id="ed8b4-280">**Zprostředkovatel** -GUID zprostředkovatele událostí</span><span class="sxs-lookup"><span data-stu-id="ed8b4-280">**provider** - The GUID of the event provider</span></span><br /><br /> <span data-ttu-id="ed8b4-281">Volitelné atributy jsou:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-281">Optional attributes are:</span></span><br /><br /> <span data-ttu-id="ed8b4-282">- **scheduledTransferLogLevelFilter** -úroveň závažnosti minimální přenést do účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-282">- **scheduledTransferLogLevelFilter** - The minimum severity level to transfer to your storage account.</span></span><br /><br /> <span data-ttu-id="ed8b4-283">- **scheduledTransferPeriod** -interval mezi naplánované přenosy do úložiště zaokrouhlený nahoru na nejbližší minutu.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-283">- **scheduledTransferPeriod** - The interval between scheduled transfers to storage rounded up to the nearest minute.</span></span> <span data-ttu-id="ed8b4-284">Hodnota je [XML "Doba trvání datového typu."](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span><span class="sxs-lookup"><span data-stu-id="ed8b4-284">The value is an [XML “Duration Data Type.”](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span></span> |  



## <a name="etweventsourceproviderconfiguration-element"></a><span data-ttu-id="ed8b4-285">EtwEventSourceProviderConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="ed8b4-285">EtwEventSourceProviderConfiguration Element</span></span>  
 <span data-ttu-id="ed8b4-286">*Stromu: Kořen - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - EtwEventSourceProviderConfiguration*</span><span class="sxs-lookup"><span data-stu-id="ed8b4-286">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders- EtwEventSourceProviderConfiguration*</span></span>

 <span data-ttu-id="ed8b4-287">Nakonfiguruje kolekce událostí generovaných [EventSource – třída](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="ed8b4-287">Configures collection of events generated from [EventSource Class](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx).</span></span>  

|<span data-ttu-id="ed8b4-288">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="ed8b4-288">Child Elements</span></span>|<span data-ttu-id="ed8b4-289">Popis</span><span class="sxs-lookup"><span data-stu-id="ed8b4-289">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="ed8b4-290">**DefaultEvents**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-290">**DefaultEvents**</span></span>|<span data-ttu-id="ed8b4-291">Volitelný atribut:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-291">Optional attribute:</span></span><br/><br/> <span data-ttu-id="ed8b4-292">**eventDestination** -název tabulku pro ukládání událostí v</span><span class="sxs-lookup"><span data-stu-id="ed8b4-292">**eventDestination** - The name of the table to store the events in</span></span>|  
|<span data-ttu-id="ed8b4-293">**Události**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-293">**Event**</span></span>|<span data-ttu-id="ed8b4-294">Požadovaný atribut:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-294">Required attribute:</span></span><br /><br /> <span data-ttu-id="ed8b4-295">**ID** -id události.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-295">**id** - The id of the event.</span></span><br /><br /> <span data-ttu-id="ed8b4-296">Volitelný atribut:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-296">Optional attribute:</span></span><br /><br /> <span data-ttu-id="ed8b4-297">**eventDestination** -název tabulku pro ukládání událostí v</span><span class="sxs-lookup"><span data-stu-id="ed8b4-297">**eventDestination** - The name of the table to store the events in</span></span>|  



## <a name="etwmanifestproviderconfiguration-element"></a><span data-ttu-id="ed8b4-298">EtwManifestProviderConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="ed8b4-298">EtwManifestProviderConfiguration Element</span></span>  
 <span data-ttu-id="ed8b4-299">*Stromové struktury: EtwManifestProviderConfiguration PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - kořenové - DiagnosticsConfiguration-*</span><span class="sxs-lookup"><span data-stu-id="ed8b4-299">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - EtwProviders - EtwManifestProviderConfiguration*</span></span>

|<span data-ttu-id="ed8b4-300">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="ed8b4-300">Child Elements</span></span>|<span data-ttu-id="ed8b4-301">Popis</span><span class="sxs-lookup"><span data-stu-id="ed8b4-301">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="ed8b4-302">**DefaultEvents**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-302">**DefaultEvents**</span></span>|<span data-ttu-id="ed8b4-303">Volitelný atribut:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-303">Optional attribute:</span></span><br /><br /> <span data-ttu-id="ed8b4-304">**eventDestination** -název tabulku pro ukládání událostí v</span><span class="sxs-lookup"><span data-stu-id="ed8b4-304">**eventDestination** - The name of the table to store the events in</span></span>|  
|<span data-ttu-id="ed8b4-305">**Události**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-305">**Event**</span></span>|<span data-ttu-id="ed8b4-306">Požadovaný atribut:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-306">Required attribute:</span></span><br /><br /> <span data-ttu-id="ed8b4-307">**ID** -id události.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-307">**id** - The id of the event.</span></span><br /><br /> <span data-ttu-id="ed8b4-308">Volitelný atribut:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-308">Optional attribute:</span></span><br /><br /> <span data-ttu-id="ed8b4-309">**eventDestination** -název tabulku pro ukládání událostí v</span><span class="sxs-lookup"><span data-stu-id="ed8b4-309">**eventDestination** - The name of the table to store the events in</span></span>|  



## <a name="metrics-element"></a><span data-ttu-id="ed8b4-310">Element metriky</span><span class="sxs-lookup"><span data-stu-id="ed8b4-310">Metrics Element</span></span>  
 <span data-ttu-id="ed8b4-311">*Strom: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - metriky*</span><span class="sxs-lookup"><span data-stu-id="ed8b4-311">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Metrics*</span></span>

 <span data-ttu-id="ed8b4-312">Umožňuje generovat tabulku čítače výkonu, která je optimalizovaná pro rychlé dotazy.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-312">Enables you to generate a performance counter table that is optimized for fast queries.</span></span> <span data-ttu-id="ed8b4-313">Jednotlivých čítačů výkonu, která je definována v **čítače výkonu** element je uložené v tabulce metriky kromě tabulky čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-313">Each performance counter that is defined in the **PerformanceCounters** element is stored in the Metrics table in addition to the Performance Counter table.</span></span>  

 <span data-ttu-id="ed8b4-314">**ResourceId** atribut je požadován.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-314">The **resourceId** attribute is required.</span></span>  <span data-ttu-id="ed8b4-315">ID prostředku nasazujete Azure Diagnostics do virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-315">The resource ID of the Virtual Machine you are deploying Azure Diagnostics to.</span></span> <span data-ttu-id="ed8b4-316">Získat **resourceID** z [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ed8b4-316">Get the **resourceID** from the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="ed8b4-317">Vyberte **Procházet** -> **skupiny prostředků** -> **< název\>**.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-317">Select **Browse** -> **Resource Groups** -> **<Name\>**.</span></span> <span data-ttu-id="ed8b4-318">Klikněte **vlastnosti** dlaždici a zkopírujte hodnotu z **ID** pole.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-318">Click the **Properties** tile and copy the value from the **ID** field.</span></span>  

|<span data-ttu-id="ed8b4-319">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="ed8b4-319">Child Elements</span></span>|<span data-ttu-id="ed8b4-320">Popis</span><span class="sxs-lookup"><span data-stu-id="ed8b4-320">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="ed8b4-321">**MetricAggregation**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-321">**MetricAggregation**</span></span>|<span data-ttu-id="ed8b4-322">Požadovaný atribut:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-322">Required attribute:</span></span><br /><br /> <span data-ttu-id="ed8b4-323">**scheduledTransferPeriod** -interval mezi naplánované přenosy do úložiště zaokrouhlený nahoru na nejbližší minutu.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-323">**scheduledTransferPeriod** - The interval between scheduled transfers to storage rounded up to the nearest minute.</span></span> <span data-ttu-id="ed8b4-324">Hodnota je [XML "Doba trvání datového typu."](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span><span class="sxs-lookup"><span data-stu-id="ed8b4-324">The value is an [XML “Duration Data Type.”](http://www.w3schools.com/schema/schema_dtypes_date.asp)</span></span> |  



## <a name="performancecounters-element"></a><span data-ttu-id="ed8b4-325">PerformanceCounters – Element</span><span class="sxs-lookup"><span data-stu-id="ed8b4-325">PerformanceCounters Element</span></span>  
 <span data-ttu-id="ed8b4-326">*Strom: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - čítače výkonu*</span><span class="sxs-lookup"><span data-stu-id="ed8b4-326">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - PerformanceCounters*</span></span>

 <span data-ttu-id="ed8b4-327">Povoluje shromažďování čítačů výkonu.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-327">Enables the collection of performance counters.</span></span>  

 <span data-ttu-id="ed8b4-328">Volitelný atribut:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-328">Optional attribute:</span></span>  

 <span data-ttu-id="ed8b4-329">Volitelné **scheduledTransferPeriod** atribut.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-329">Optional **scheduledTransferPeriod** attribute.</span></span> <span data-ttu-id="ed8b4-330">Viz vysvětlení dříve.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-330">See explanation earlier.</span></span>

|<span data-ttu-id="ed8b4-331">Podřízený Element</span><span class="sxs-lookup"><span data-stu-id="ed8b4-331">Child Element</span></span>|<span data-ttu-id="ed8b4-332">Popis</span><span class="sxs-lookup"><span data-stu-id="ed8b4-332">Description</span></span>|  
|-------------------|-----------------|  
|<span data-ttu-id="ed8b4-333">**PerformanceCounterConfiguration**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-333">**PerformanceCounterConfiguration**</span></span>|<span data-ttu-id="ed8b4-334">Vyžadují se následující atributy:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-334">The following attributes are required:</span></span><br /><br /> <span data-ttu-id="ed8b4-335">- **counterSpecifier** – název čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-335">- **counterSpecifier** - The name of the performance counter.</span></span> <span data-ttu-id="ed8b4-336">Například, `\Processor(_Total)\% Processor Time`.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-336">For example, `\Processor(_Total)\% Processor Time`.</span></span> <span data-ttu-id="ed8b4-337">Pokud chcete získat seznam čítačů výkonu na hostiteli, spusťte příkaz `typeperf`.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-337">To get a list of performance counters on your host, run the command `typeperf`.</span></span><br /><br /> <span data-ttu-id="ed8b4-338">- **sampleRate** – jak často se čítač odeberou.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-338">- **sampleRate** - How often the counter should be sampled.</span></span><br /><br /> <span data-ttu-id="ed8b4-339">Volitelný atribut:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-339">Optional attribute:</span></span><br /><br /> <span data-ttu-id="ed8b4-340">**jednotka** -měrnou čítače.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-340">**unit** - The unit of measure of the counter.</span></span>|  




## <a name="windowseventlog-element"></a><span data-ttu-id="ed8b4-341">WindowsEventLog Element</span><span class="sxs-lookup"><span data-stu-id="ed8b4-341">WindowsEventLog Element</span></span>
 <span data-ttu-id="ed8b4-342">*Stromu: - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - WindowsEventLog kořen*</span><span class="sxs-lookup"><span data-stu-id="ed8b4-342">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - WindowsEventLog*</span></span>
 
 <span data-ttu-id="ed8b4-343">Umožňuje kolekce protokoly událostí systému Windows.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-343">Enables the collection of Windows Event Logs.</span></span>  

 <span data-ttu-id="ed8b4-344">Volitelné **scheduledTransferPeriod** atribut.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-344">Optional **scheduledTransferPeriod** attribute.</span></span> <span data-ttu-id="ed8b4-345">Viz vysvětlení dříve.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-345">See explanation  earlier.</span></span>  

|<span data-ttu-id="ed8b4-346">Podřízený Element</span><span class="sxs-lookup"><span data-stu-id="ed8b4-346">Child Element</span></span>|<span data-ttu-id="ed8b4-347">Popis</span><span class="sxs-lookup"><span data-stu-id="ed8b4-347">Description</span></span>|  
|-------------------|-----------------|  
|<span data-ttu-id="ed8b4-348">**Zdroj dat**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-348">**DataSource**</span></span>|<span data-ttu-id="ed8b4-349">Protokol událostí systému Windows ke shromažďování.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-349">The Windows Event logs to collect.</span></span> <span data-ttu-id="ed8b4-350">Požadovaný atribut:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-350">Required attribute:</span></span><br /><br /> <span data-ttu-id="ed8b4-351">**název** – dotaz XPath popisující události systému windows, které se mají shromažďovat.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-351">**name** - The XPath query describing the windows events to be collected.</span></span> <span data-ttu-id="ed8b4-352">Například:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-352">For example:</span></span><br /><br /> `Application!*[System[(Level <=3)]], System!*[System[(Level <=3)]], System!*[System[Provider[@Name='Microsoft Antimalware']]], Security!*[System[(Level <= 3)]`<br /><br /> <span data-ttu-id="ed8b4-353">Ke shromažďování všech událostí, zadejte "*"</span><span class="sxs-lookup"><span data-stu-id="ed8b4-353">To collect all events, specify "*"</span></span>|  




## <a name="logs-element"></a><span data-ttu-id="ed8b4-354">Element protokoly</span><span class="sxs-lookup"><span data-stu-id="ed8b4-354">Logs Element</span></span>  
 <span data-ttu-id="ed8b4-355">*Strom: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - protokoly*</span><span class="sxs-lookup"><span data-stu-id="ed8b4-355">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - Logs*</span></span>

 <span data-ttu-id="ed8b4-356">Prezentovat v verze 1.0 a 1.1.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-356">Present in version 1.0 and 1.1.</span></span> <span data-ttu-id="ed8b4-357">Chybí v 1.2.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-357">Missing in 1.2.</span></span> <span data-ttu-id="ed8b4-358">Přidat zpět v 1.3.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-358">Added back in 1.3.</span></span>  

 <span data-ttu-id="ed8b4-359">Definuje konfiguraci vyrovnávací paměti pro základní Azure protokoly.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-359">Defines the buffer configuration for basic Azure logs.</span></span>  

|<span data-ttu-id="ed8b4-360">Atribut</span><span class="sxs-lookup"><span data-stu-id="ed8b4-360">Attribute</span></span>|<span data-ttu-id="ed8b4-361">Typ</span><span class="sxs-lookup"><span data-stu-id="ed8b4-361">Type</span></span>|<span data-ttu-id="ed8b4-362">Popis</span><span class="sxs-lookup"><span data-stu-id="ed8b4-362">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="ed8b4-363">**bufferQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-363">**bufferQuotaInMB**</span></span>|<span data-ttu-id="ed8b4-364">**celé číslo bez znaménka**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-364">**unsignedInt**</span></span>|<span data-ttu-id="ed8b4-365">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-365">Optional.</span></span> <span data-ttu-id="ed8b4-366">Určuje maximální množství úložiště systému souboru, který je k dispozici pro zadaná data.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-366">Specifies the maximum amount of file system storage that is available for the specified data.</span></span><br /><br /> <span data-ttu-id="ed8b4-367">Výchozí hodnota je 0.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-367">The default is 0.</span></span>|  
|<span data-ttu-id="ed8b4-368">**scheduledTransferLogLevelFilterr**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-368">**scheduledTransferLogLevelFilterr**</span></span>|<span data-ttu-id="ed8b4-369">**řetězec**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-369">**string**</span></span>|<span data-ttu-id="ed8b4-370">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-370">Optional.</span></span> <span data-ttu-id="ed8b4-371">Určuje úroveň závažnosti minimální pro položky protokolu, které se přenáší.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-371">Specifies the minimum severity level for log entries that are transferred.</span></span> <span data-ttu-id="ed8b4-372">Výchozí hodnota je **Undefined**, která přenáší všechny protokoly.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-372">The default value is **Undefined**, which transfers all logs.</span></span> <span data-ttu-id="ed8b4-373">Další možné hodnoty (v pořadí podle nejvíc minimálně informace) jsou **podrobné**, **informace**, **upozornění**, **chyba**, a **kritický**.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-373">Other possible values (in order of most to least information) are **Verbose**, **Information**, **Warning**, **Error**, and **Critical**.</span></span>|  
|<span data-ttu-id="ed8b4-374">**scheduledTransferPeriod**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-374">**scheduledTransferPeriod**</span></span>|<span data-ttu-id="ed8b4-375">**Doba trvání**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-375">**duration**</span></span>|<span data-ttu-id="ed8b4-376">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-376">Optional.</span></span> <span data-ttu-id="ed8b4-377">Určuje interval mezi naplánované přenosů dat, zaokrouhlený nahoru na nejbližší minutu.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-377">Specifies the interval between scheduled transfers of data, rounded up to the nearest minute.</span></span><br /><br /> <span data-ttu-id="ed8b4-378">Výchozí hodnota je PT0S.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-378">The default is PT0S.</span></span>|  
|<span data-ttu-id="ed8b4-379">**jímky** přidali v 1.5</span><span class="sxs-lookup"><span data-stu-id="ed8b4-379">**sinks** Added in 1.5</span></span>|<span data-ttu-id="ed8b4-380">**řetězec**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-380">**string**</span></span>|<span data-ttu-id="ed8b4-381">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-381">Optional.</span></span> <span data-ttu-id="ed8b4-382">Bodů na jímka umístění a také posílat diagnostická data.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-382">Points to a sink location to also send diagnostic data.</span></span> <span data-ttu-id="ed8b4-383">Například Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-383">For example, Application Insights.</span></span>|  

## <a name="dockersources"></a><span data-ttu-id="ed8b4-384">DockerSources</span><span class="sxs-lookup"><span data-stu-id="ed8b4-384">DockerSources</span></span>
 <span data-ttu-id="ed8b4-385">*Stromu: - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - DockerSources kořen*</span><span class="sxs-lookup"><span data-stu-id="ed8b4-385">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - DiagnosticMonitorConfiguration - DockerSources*</span></span>

 <span data-ttu-id="ed8b4-386">Přidaná do 1.9.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-386">Added in 1.9.</span></span>

|<span data-ttu-id="ed8b4-387">Název elementu</span><span class="sxs-lookup"><span data-stu-id="ed8b4-387">Element Name</span></span>|<span data-ttu-id="ed8b4-388">Popis</span><span class="sxs-lookup"><span data-stu-id="ed8b4-388">Description</span></span>|  
|------------------|-----------------|  
|<span data-ttu-id="ed8b4-389">**Statistiky**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-389">**Stats**</span></span>|<span data-ttu-id="ed8b4-390">Říká systému ke shromažďování statistik pro Docker kontejnery</span><span class="sxs-lookup"><span data-stu-id="ed8b4-390">Tells the system to collect stats for Docker containers</span></span>|  

## <a name="sinksconfig-element"></a><span data-ttu-id="ed8b4-391">SinksConfig Element</span><span class="sxs-lookup"><span data-stu-id="ed8b4-391">SinksConfig Element</span></span>  
 <span data-ttu-id="ed8b4-392">*Stromové struktury: SinksConfig PublicConfig - WadCFG - kořenové - DiagnosticsConfiguration-*</span><span class="sxs-lookup"><span data-stu-id="ed8b4-392">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig*</span></span>

 <span data-ttu-id="ed8b4-393">Seznam umístění odesílat data diagnostiky a konfigurace přidružené těchto umístěních.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-393">A list of locations to send diagnostics data to and the configuration associated with those locations.</span></span>  

|<span data-ttu-id="ed8b4-394">Název elementu</span><span class="sxs-lookup"><span data-stu-id="ed8b4-394">Element Name</span></span>|<span data-ttu-id="ed8b4-395">Popis</span><span class="sxs-lookup"><span data-stu-id="ed8b4-395">Description</span></span>|  
|------------------|-----------------|  
|<span data-ttu-id="ed8b4-396">**Podřízený**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-396">**Sink**</span></span>|<span data-ttu-id="ed8b4-397">Na této stránce najdete v popisu jinde.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-397">See description elsewhere on this page.</span></span>|  

## <a name="sink-element"></a><span data-ttu-id="ed8b4-398">Jímky – Element</span><span class="sxs-lookup"><span data-stu-id="ed8b4-398">Sink Element</span></span>
 <span data-ttu-id="ed8b4-399">*Strom: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - jímka*</span><span class="sxs-lookup"><span data-stu-id="ed8b4-399">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink*</span></span>

 <span data-ttu-id="ed8b4-400">Přidána do verze 1.5.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-400">Added in version 1.5.</span></span>  

 <span data-ttu-id="ed8b4-401">Definuje umístění poslat diagnostická data.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-401">Defines locations to send diagnostic data to.</span></span> <span data-ttu-id="ed8b4-402">Například službu Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-402">For example, the Application Insights service.</span></span>  

|<span data-ttu-id="ed8b4-403">Atribut</span><span class="sxs-lookup"><span data-stu-id="ed8b4-403">Attribute</span></span>|<span data-ttu-id="ed8b4-404">Typ</span><span class="sxs-lookup"><span data-stu-id="ed8b4-404">Type</span></span>|<span data-ttu-id="ed8b4-405">Popis</span><span class="sxs-lookup"><span data-stu-id="ed8b4-405">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="ed8b4-406">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-406">**name**</span></span>|<span data-ttu-id="ed8b4-407">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ed8b4-407">string</span></span>|<span data-ttu-id="ed8b4-408">Řetězec identifikující sinkname.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-408">A string identifying the sinkname.</span></span>|  

|<span data-ttu-id="ed8b4-409">Element</span><span class="sxs-lookup"><span data-stu-id="ed8b4-409">Element</span></span>|<span data-ttu-id="ed8b4-410">Typ</span><span class="sxs-lookup"><span data-stu-id="ed8b4-410">Type</span></span>|<span data-ttu-id="ed8b4-411">Popis</span><span class="sxs-lookup"><span data-stu-id="ed8b4-411">Description</span></span>|  
|-------------|----------|-----------------|  
|<span data-ttu-id="ed8b4-412">**Application Insights**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-412">**Application Insights**</span></span>|<span data-ttu-id="ed8b4-413">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ed8b4-413">string</span></span>|<span data-ttu-id="ed8b4-414">Použít pouze při odesílání dat do služby Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-414">Used only when sending data to Application Insights.</span></span> <span data-ttu-id="ed8b4-415">Obsahovat klíč instrumentace pro aktivní účet Application Insights, máte přístup.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-415">Contain the Instrumentation Key for an active Application Insights account that you have access to.</span></span>|  
|<span data-ttu-id="ed8b4-416">**Kanály**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-416">**Channels**</span></span>|<span data-ttu-id="ed8b4-417">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ed8b4-417">string</span></span>|<span data-ttu-id="ed8b4-418">Jeden pro každou další filtrování, který datového proudu, který jste</span><span class="sxs-lookup"><span data-stu-id="ed8b4-418">One for each additional filtering that stream that you</span></span>|  

## <a name="channels-element"></a><span data-ttu-id="ed8b4-419">Element kanály</span><span class="sxs-lookup"><span data-stu-id="ed8b4-419">Channels Element</span></span>  
 <span data-ttu-id="ed8b4-420">*Stromové struktury: Kanály SinksConfig - jímka - kořenové - DiagnosticsConfiguration - PublicConfig - WadCFG-*</span><span class="sxs-lookup"><span data-stu-id="ed8b4-420">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink - Channels*</span></span>

 <span data-ttu-id="ed8b4-421">Přidána do verze 1.5.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-421">Added in version 1.5.</span></span>  

 <span data-ttu-id="ed8b4-422">Definuje filtry pro datové proudy prošla jímku dat protokolu.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-422">Defines filters for streams of log data passing through a sink.</span></span>  

|<span data-ttu-id="ed8b4-423">Element</span><span class="sxs-lookup"><span data-stu-id="ed8b4-423">Element</span></span>|<span data-ttu-id="ed8b4-424">Typ</span><span class="sxs-lookup"><span data-stu-id="ed8b4-424">Type</span></span>|<span data-ttu-id="ed8b4-425">Popis</span><span class="sxs-lookup"><span data-stu-id="ed8b4-425">Description</span></span>|  
|-------------|----------|-----------------|  
|<span data-ttu-id="ed8b4-426">**Kanál**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-426">**Channel**</span></span>|<span data-ttu-id="ed8b4-427">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ed8b4-427">string</span></span>|<span data-ttu-id="ed8b4-428">Na této stránce najdete v popisu jinde.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-428">See description elsewhere on this page.</span></span>|  

## <a name="channel-element"></a><span data-ttu-id="ed8b4-429">Element kanálu</span><span class="sxs-lookup"><span data-stu-id="ed8b4-429">Channel Element</span></span>
 <span data-ttu-id="ed8b4-430">*Strom: Kořenové - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - jímka - kanály - kanál*</span><span class="sxs-lookup"><span data-stu-id="ed8b4-430">*Tree: Root - DiagnosticsConfiguration - PublicConfig - WadCFG - SinksConfig - Sink - Channels - Channel*</span></span>

 <span data-ttu-id="ed8b4-431">Přidána do verze 1.5.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-431">Added in version 1.5.</span></span>  

 <span data-ttu-id="ed8b4-432">Definuje umístění poslat diagnostická data.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-432">Defines locations to send diagnostic data to.</span></span> <span data-ttu-id="ed8b4-433">Například službu Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-433">For example, the Application Insights service.</span></span>  

|<span data-ttu-id="ed8b4-434">Atributy</span><span class="sxs-lookup"><span data-stu-id="ed8b4-434">Attributes</span></span>|<span data-ttu-id="ed8b4-435">Typ</span><span class="sxs-lookup"><span data-stu-id="ed8b4-435">Type</span></span>|<span data-ttu-id="ed8b4-436">Popis</span><span class="sxs-lookup"><span data-stu-id="ed8b4-436">Description</span></span>|  
|----------------|----------|-----------------|  
|<span data-ttu-id="ed8b4-437">**logLevel**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-437">**logLevel**</span></span>|<span data-ttu-id="ed8b4-438">**řetězec**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-438">**string**</span></span>|<span data-ttu-id="ed8b4-439">Určuje úroveň závažnosti minimální pro položky protokolu, které se přenáší.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-439">Specifies the minimum severity level for log entries that are transferred.</span></span> <span data-ttu-id="ed8b4-440">Výchozí hodnota je **Undefined**, která přenáší všechny protokoly.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-440">The default value is **Undefined**, which transfers all logs.</span></span> <span data-ttu-id="ed8b4-441">Další možné hodnoty (v pořadí podle nejvíc minimálně informace) jsou **podrobné**, **informace**, **upozornění**, **chyba**, a **kritický**.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-441">Other possible values (in order of most to least information) are **Verbose**, **Information**, **Warning**, **Error**, and **Critical**.</span></span>|  
|<span data-ttu-id="ed8b4-442">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-442">**name**</span></span>|<span data-ttu-id="ed8b4-443">**řetězec**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-443">**string**</span></span>|<span data-ttu-id="ed8b4-444">Jedinečný název kanálu k odkazování na</span><span class="sxs-lookup"><span data-stu-id="ed8b4-444">A unique name of the channel to refer to</span></span>|  


## <a name="privateconfig-element"></a><span data-ttu-id="ed8b4-445">PrivateConfig Element</span><span class="sxs-lookup"><span data-stu-id="ed8b4-445">PrivateConfig Element</span></span> 
 <span data-ttu-id="ed8b4-446">*Stromové struktury: PrivateConfig kořenové - DiagnosticsConfiguration-*</span><span class="sxs-lookup"><span data-stu-id="ed8b4-446">*Tree: Root - DiagnosticsConfiguration - PrivateConfig*</span></span>

 <span data-ttu-id="ed8b4-447">Přidána do verze 1.3.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-447">Added in version 1.3.</span></span>  

 <span data-ttu-id="ed8b4-448">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="ed8b4-448">Optional</span></span>  

 <span data-ttu-id="ed8b4-449">Ukládá soukromé podrobnosti o účtu úložiště (název, klíč a koncového bodu).</span><span class="sxs-lookup"><span data-stu-id="ed8b4-449">Stores the private details of the storage account (name, key, and endpoint).</span></span> <span data-ttu-id="ed8b4-450">Tyto informace je odeslána do virtuálního počítače, ale nelze načíst z něj.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-450">This information is sent to the virtual machine, but cannot be retrieved from it.</span></span>  

|<span data-ttu-id="ed8b4-451">Podřízené elementy</span><span class="sxs-lookup"><span data-stu-id="ed8b4-451">Child Elements</span></span>|<span data-ttu-id="ed8b4-452">Popis</span><span class="sxs-lookup"><span data-stu-id="ed8b4-452">Description</span></span>|  
|--------------------|-----------------|  
|<span data-ttu-id="ed8b4-453">**StorageAccount**</span><span class="sxs-lookup"><span data-stu-id="ed8b4-453">**StorageAccount**</span></span>|<span data-ttu-id="ed8b4-454">Účet úložiště používat.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-454">The storage account to use.</span></span> <span data-ttu-id="ed8b4-455">Následující atributy jsou požadovány</span><span class="sxs-lookup"><span data-stu-id="ed8b4-455">The following attributes are required</span></span><br /><br /> <span data-ttu-id="ed8b4-456">- **název** – název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-456">- **name** - The name of the storage account.</span></span><br /><br /> <span data-ttu-id="ed8b4-457">- **klíč** -klíč k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-457">- **key** - The key to the storage account.</span></span><br /><br /> <span data-ttu-id="ed8b4-458">- **koncový bod** -koncový bod pro přístup k účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-458">- **endpoint** - The endpoint to access the storage account.</span></span> <br /><br /> <span data-ttu-id="ed8b4-459">-**sasToken** (přidají 1.8.1)-tokenu SAS místo klíč účtu úložiště můžete zadat v privátní konfigurace.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-459">-**sasToken** (added 1.8.1)- You can specify an SAS token instead of a storage account key in the private config.</span></span> <span data-ttu-id="ed8b4-460">Pokud je zadán, klíče účtu úložiště se ignoruje.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-460">If provided, the storage account key is ignored.</span></span> <br /><span data-ttu-id="ed8b4-461">Požadavky pro SAS Token:</span><span class="sxs-lookup"><span data-stu-id="ed8b4-461">Requirements for the SAS Token:</span></span> <br /><span data-ttu-id="ed8b4-462">– Podporuje pouze tokenu SAS účtu</span><span class="sxs-lookup"><span data-stu-id="ed8b4-462">- Supports account SAS token only</span></span> <br /><span data-ttu-id="ed8b4-463">- *b*, *t* jsou požadovány typy služby.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-463">- *b*, *t* service types are required.</span></span> <br /> <span data-ttu-id="ed8b4-464">- **, *c*, *u*, *w* jsou požadována oprávnění.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-464">- *a*, *c*, *u*, *w* permissions are required.</span></span> <br /> <span data-ttu-id="ed8b4-465">- *c*, *o* jsou požadovány typy prostředků.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-465">- *c*, *o* resource types are required.</span></span> <br /> <span data-ttu-id="ed8b4-466">– Podporuje protokol HTTPS</span><span class="sxs-lookup"><span data-stu-id="ed8b4-466">- Supports the HTTPS protocol only</span></span> <br /> <span data-ttu-id="ed8b4-467">-Spuštění a čas vypršení platnosti musí být platné.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-467">- Start and expiry time must be valid.</span></span>|  


## <a name="isenabled-element"></a><span data-ttu-id="ed8b4-468">Element hodnotu IsEnabled</span><span class="sxs-lookup"><span data-stu-id="ed8b4-468">IsEnabled Element</span></span>  
 <span data-ttu-id="ed8b4-469">*Stromové struktury: Hodnotu IsEnabled kořenové - DiagnosticsConfiguration-*</span><span class="sxs-lookup"><span data-stu-id="ed8b4-469">*Tree: Root - DiagnosticsConfiguration - IsEnabled*</span></span>

 <span data-ttu-id="ed8b4-470">Logická hodnota.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-470">Boolean.</span></span> <span data-ttu-id="ed8b4-471">Použití `true` povolit diagnostiku nebo `false` zakázat diagnostiku.</span><span class="sxs-lookup"><span data-stu-id="ed8b4-471">Use `true` to enable the diagnostics or `false` to disable the diagnostics.</span></span>
