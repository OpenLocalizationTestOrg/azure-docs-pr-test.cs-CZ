---
title: "aaaAzure schéma konfigurace diagnostiky 1.0 | Microsoft Docs"
description: "Platí pouze pokud používáte Azure SDK 2.4 a pod s Azure virtuálních počítačů sady škálování virtuálního počítače, Service Fabric nebo cloudové služby."
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
ms.openlocfilehash: bdd2b26217d6ea28f19e651ab429e7e7401ff57b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-diagnostics-10-configuration-schema"></a>Schéma konfigurace Azure Diagnostics 1.0
> [!NOTE]
> Azure Diagnostics je čítače výkonu toocollect součástí používanou hello a další statistiky z virtuálních počítačů Azure, sady škálování virtuálního počítače, Service Fabric a cloudové služby.  Tato stránka je pouze relevantní, pokud používáte některou z těchto služeb.
>

Azure Diagnostics se používá s dalšími produkty Microsoftu diagnostiky jako monitorování Azure, Application Insights a analýzy protokolů.

Hello Azure Diagnostics konfigurační soubor definuje hodnoty, které jsou používané tooinitialize hello monitorování diagnostiky. Tento soubor je použité tooinitialize konfigurace diagnostiky nastavení při monitorování diagnostiky hello spustí.  

 Ve výchozím nastavení, soubor schéma konfigurace Azure Diagnostics hello je nainstalovaná toohello `C:\Program Files\Microsoft SDKs\Azure\.NET SDK\<version>\schemas` adresáře. Nahraďte `<version>` hello nainstalované verzi hello [Azure SDK](http://www.windowsazure.com/develop/downloads/).  

> [!NOTE]
>  Hello diagnostiky konfigurační soubor se obvykle používá s spuštění úlohy, které vyžadují toobe diagnostických dat shromážděných dříve v procesu spouštění hello. Další informace o používání Azure Diagnostics najdete v tématu [shromažďovat protokolování dat pomocí Azure Diagnostics](assetId:///83a91c23-5ca2-4fc9-8df3-62036c37a3d7).  

## <a name="example-of-hello-diagnostics-configuration-file"></a>Příklad souboru konfigurace diagnostiky hello  
 Hello následující příklad ukazuje konfigurační soubor typické diagnostics:  

```xml  
<?xml version="1.0" encoding="utf-8"?>
<DiagnosticMonitorConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration"  
      configurationChangePollInterval="PT1M"  
      overallQuotaInMB="4096">  
   <DiagnosticInfrastructureLogs bufferQuotaInMB="1024"  
      scheduledTransferLogLevelFilter="Verbose"  
      scheduledTransferPeriod="PT1M" />  
   <Logs bufferQuotaInMB="1024"  
      scheduledTransferLogLevelFilter="Verbose"  
      scheduledTransferPeriod="PT1M" />  

   <Directories bufferQuotaInMB="1024"   
      scheduledTransferPeriod="PT1M">  

      <!-- These three elements specify hello special directories   
           that are set up for hello log types -->  
      <CrashDumps container="wad-crash-dumps" directoryQuotaInMB="256" />  
      <FailedRequestLogs container="wad-frq" directoryQuotaInMB="256" />  
      <IISLogs container="wad-iis" directoryQuotaInMB="256" />  

      <!-- For regular directories hello DataSources element is used -->  
      <DataSources>  
         <DirectoryConfiguration container="wad-panther" directoryQuotaInMB="128">  
            <!-- Absolute specifies an absolute path with optional environment expansion -->  
            <Absolute expandEnvironment="true" path="%SystemRoot%\system32\sysprep\Panther" />  
         </DirectoryConfiguration>  
         <DirectoryConfiguration container="wad-custom" directoryQuotaInMB="128">  
            <!-- LocalResource specifies a path relative tooa local   
                 resource defined in hello service definition -->  
            <LocalResource name="MyLoggingLocalResource" relativePath="logs" />  
         </DirectoryConfiguration>  
      </DataSources>  
   </Directories>  

   <PerformanceCounters bufferQuotaInMB="512" scheduledTransferPeriod="PT1M">  
      <!-- hello counter specifier is in hello same format as hello imperative   
           diagnostics configuration API -->  
      <PerformanceCounterConfiguration   
         counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT5S" />  
   </PerformanceCounters>  

   <WindowsEventLog bufferQuotaInMB="512"  
      scheduledTransferLogLevelFilter="Verbose"  
      scheduledTransferPeriod="PT1M">  
      <!-- hello event log name is in hello same format as hello imperative   
           diagnostics configuration API -->  
      <DataSource name="System!*" />  
   </WindowsEventLog>  
</DiagnosticMonitorConfiguration>  
```  

## <a name="diagnosticsconfiguration-namespace"></a>Namespace DiagnosticsConfiguration  
 obor názvů XML Hello hello diagnostiky konfiguračního souboru je:  

```  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  
```  

## <a name="schema-elements"></a>Prvky schématu  
 Hello diagnostiky konfigurační soubor obsahuje následující prvky hello.


## <a name="diagnosticmonitorconfiguration-element"></a>DiagnosticMonitorConfiguration Element  
element nejvyšší úrovně Hello hello diagnostiky konfiguračního souboru.  

Atributy:

|Atribut  |Typ   |Požaduje se| Výchozí | Popis|  
|-----------|-------|--------|---------|------------|  
|**configurationChangePollInterval**|Doba trvání|Nepovinné | PT1M| Určuje hello interval, ve které hlasování hello monitorování diagnostiky pro změny konfigurace diagnostiky.|  
|**overallQuotaInMB**|celé číslo bez znaménka|Nepovinné| 4000 MB. Pokud zadáte hodnotu, nesmí být delší než toto množství |Hello celkovou velikost souboru systému úložiště přidělené pro všechny protokolování vyrovnávací paměti.|  

## <a name="diagnosticinfrastructurelogs-element"></a>DiagnosticInfrastructureLogs Element  
Definuje konfiguraci hello vyrovnávací paměti pro hello protokoly, které jsou generovány hello základní infrastruktura diagnostiky.

Nadřazený Element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).  

Atributy:

|Atribut|Typ|Popis|  
|---------|----|-----------------|  
|**bufferQuotaInMB**|celé číslo bez znaménka|Volitelné. Určuje maximální množství hello úložiště systému souboru, který je k dispozici pro hello zadaná data.<br /><br /> Hello výchozí hodnota je 0.|  
|**scheduledTransferLogLevelFilter**|Řetězec|Volitelné. Určuje hello minimální úroveň závažnosti pro položky protokolu, které se přenáší. Hello výchozí hodnota je **Undefined**. Další možné hodnoty jsou **podrobné**, **informace**, **upozornění**, **chyba**, a **kritický**.|  
|**scheduledTransferPeriod**|Doba trvání|Volitelné. Určuje interval hello od naplánovanou přenosů dat, zaokrouhlit toohello nejbližší minutu.<br /><br /> Výchozí hodnota Hello je PT0S.|  

## <a name="logs-element"></a>Element protokoly  
 Definuje konfiguraci hello vyrovnávací paměti pro základní Azure protokoly.

 Nadřazený element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).  

Atributy:  

|Atribut|Typ|Popis|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|celé číslo bez znaménka|Volitelné. Určuje maximální množství hello úložiště systému souboru, který je k dispozici pro hello zadaná data.<br /><br /> Hello výchozí hodnota je 0.|  
|**scheduledTransferLogLevelFilter**|Řetězec|Volitelné. Určuje hello minimální úroveň závažnosti pro položky protokolu, které se přenáší. Hello výchozí hodnota je **Undefined**. Další možné hodnoty jsou **podrobné**, **informace**, **upozornění**, **chyba**, a **kritický**.|  
|**scheduledTransferPeriod**|Doba trvání|Volitelné. Určuje interval hello od naplánovanou přenosů dat, zaokrouhlit toohello nejbližší minutu.<br /><br /> Výchozí hodnota Hello je PT0S.|  

## <a name="directories-element"></a>Element adresáře  
Definuje konfiguraci hello vyrovnávací paměti na základě souborů protokolů, které můžete definovat.

Nadřazený element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).  


Atributy:  

|Atribut|Typ|Popis|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|celé číslo bez znaménka|Volitelné. Určuje maximální množství hello úložiště systému souboru, který je k dispozici pro hello zadaná data.<br /><br /> Hello výchozí hodnota je 0.|  
|**scheduledTransferPeriod**|Doba trvání|Volitelné. Určuje interval hello od naplánovanou přenosů dat, zaokrouhlit toohello nejbližší minutu.<br /><br /> Výchozí hodnota Hello je PT0S.|  

## <a name="crashdumps-element"></a>CrashDumps Element  
 Definuje adresář výpisy hello havárií.

 Nadřazený Element: [adresáře Element](#Directories).  

Atributy:  

|Atribut|Typ|Popis|  
|---------------|----------|-----------------|  
|**kontejner**|Řetězec|Název Hello hello kontejneru, kde je obsah hello hello adresáře toobe přenést.|  
|**directoryQuotaInMB**|celé číslo bez znaménka|Volitelné. Určuje maximální velikost hello hello adresáře v megabajtech.<br /><br /> Hello výchozí hodnota je 0.|  

## <a name="failedrequestlogs-element"></a>FailedRequestLogs Element  
 Definuje adresář protokolu hello chybných požadavků.

 Nadřazený Element [adresáře Element](#Directories).  

Atributy:  

|Atribut|Typ|Popis|  
|---------------|----------|-----------------|  
|**kontejner**|Řetězec|Název Hello hello kontejneru, kde je obsah hello hello adresáře toobe přenést.|  
|**directoryQuotaInMB**|celé číslo bez znaménka|Volitelné. Určuje maximální velikost hello hello adresáře v megabajtech.<br /><br /> Hello výchozí hodnota je 0.|  

##  <a name="iislogs-element"></a>IISLogs Element  
 Definuje adresář protokolu služby IIS hello.

 Nadřazený Element [adresáře Element](#Directories).  

Atributy:  

|Atribut|Typ|Popis|  
|---------------|----------|-----------------|  
|**kontejner**|Řetězec|Název Hello hello kontejneru, kde je obsah hello hello adresáře toobe přenést.|  
|**directoryQuotaInMB**|celé číslo bez znaménka|Volitelné. Určuje maximální velikost hello hello adresáře v megabajtech.<br /><br /> Hello výchozí hodnota je 0.|  

## <a name="datasources-element"></a>Element zdrojů dat  
 Definuje nula nebo více dodatečných protokolů adresáře.

 Nadřazený Element: [adresáře Element](#Directories).

## <a name="directoryconfiguration-element"></a>DirectoryConfiguration Element  
 Definuje adresář hello toomonitor soubory protokolu.

 Nadřazený Element: [zdrojů dat Element](#DataSources).

Atributy:

|Atribut|Typ|Popis|  
|---------------|----------|-----------------|  
|**kontejner**|Řetězec|Název Hello hello kontejneru, kde je obsah hello hello adresáře toobe přenést.|  
|**directoryQuotaInMB**|celé číslo bez znaménka|Volitelné. Určuje maximální velikost hello hello adresáře v megabajtech.<br /><br /> Hello výchozí hodnota je 0.|  

## <a name="absolute-element"></a>Absolutní – Element  
 Definuje absolutní cesta toomonitor directory hello s prostředí volitelné rozšíření.

 Nadřazený Element: [DirectoryConfiguration Element](#DirectoryConfiguration).  

Atributy:  

|Atribut|Typ|Popis|  
|---------------|----------|-----------------|  
|**Cesta**|Řetězec|Povinná hodnota. Hello toomonitor directory toohello absolutní cesta.|  
|**expandEnvironment**|Logická hodnota|Povinná hodnota. Pokud nastavení příliš**true**, jsou-li rozbalit proměnné prostředí v cestě hello.|  

## <a name="localresource-element"></a>LocalResource Element  
 Definuje cestu relativní tooa místního prostředku v definici služby hello.

 Nadřazený Element: [DirectoryConfiguration Element](#DirectoryConfiguration).  

Atributy:  

|Atribut|Typ|Popis|  
|---------------|----------|-----------------|  
|**Jméno**|Řetězec|Povinná hodnota. Název Hello hello místní prostředek, který obsahuje hello directory toomonitor.|  
|**relativePath**|Řetězec|Povinná hodnota. Hello cesta relativní toohello místní prostředek toomonitor.|  

## <a name="performancecounters-element"></a>PerformanceCounters – Element  
 Definuje hello cesta toohello výkonu čítač toocollect.

 Nadřazený Element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).


 Atributy:  

|Atribut|Typ|Popis|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|celé číslo bez znaménka|Volitelné. Určuje maximální množství hello úložiště systému souboru, který je k dispozici pro hello zadaná data.<br /><br /> Hello výchozí hodnota je 0.|  
|**scheduledTransferPeriod**|Doba trvání|Volitelné. Určuje interval hello od naplánovanou přenosů dat, zaokrouhlit toohello nejbližší minutu.<br /><br /> Výchozí hodnota Hello je PT0S.|  

## <a name="performancecounterconfiguration-element"></a>PerformanceCounterConfiguration Element  
 Definuje toocollect čítače výkonu hello.

 Nadřazený Element: [PerformanceCounters – Element](#PerformanceCounters).  

 Atributy:  

|Atribut|Typ|Popis|  
|---------------|----------|-----------------|  
|**counterSpecifier**|Řetězec|Povinná hodnota. Hello toocollect čítače výkonu toohello cesta.|  
|**sampleRate**|Doba trvání|Povinná hodnota. rychlost Hello, na které hello by měly být shromažďovány čítače výkonu.|  

## <a name="windowseventlog-element"></a>WindowsEventLog Element  
 Definuje toomonitor hello protokoly událostí.

 Nadřazený Element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).

  Atributy:

|Atribut|Typ|Popis|  
|---------------|----------|-----------------|  
|**bufferQuotaInMB**|celé číslo bez znaménka|Volitelné. Určuje maximální množství hello úložiště systému souboru, který je k dispozici pro hello zadaná data.<br /><br /> Hello výchozí hodnota je 0.|  
|**scheduledTransferLogLevelFilter**|Řetězec|Volitelné. Určuje hello minimální úroveň závažnosti pro položky protokolu, které se přenáší. Hello výchozí hodnota je **Undefined**. Další možné hodnoty jsou **podrobné**, **informace**, **upozornění**, **chyba**, a **kritický**.|  
|**scheduledTransferPeriod**|Doba trvání|Volitelné. Určuje interval hello od naplánovanou přenosů dat, zaokrouhlit toohello nejbližší minutu.<br /><br /> Výchozí hodnota Hello je PT0S.|  

## <a name="datasource-element"></a>Element zdroje dat  
 Definuje toomonitor hello protokolu událostí.

 Nadřazený Element: [WindowsEventLog Element](#windowsEventLog).  

 Atributy:

|Atribut|Typ|Popis|  
|---------------|----------|-----------------|  
|**Jméno**|Řetězec|Povinná hodnota. Výraz XPath určující toocollect protokolu hello.|  
