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
# <a name="azure-diagnostics-10-configuration-schema"></a><span data-ttu-id="49ab5-103">Schéma konfigurace Azure Diagnostics 1.0</span><span class="sxs-lookup"><span data-stu-id="49ab5-103">Azure Diagnostics 1.0 Configuration Schema</span></span>
> [!NOTE]
> <span data-ttu-id="49ab5-104">Azure Diagnostics je čítače výkonu toocollect součástí používanou hello a další statistiky z virtuálních počítačů Azure, sady škálování virtuálního počítače, Service Fabric a cloudové služby.</span><span class="sxs-lookup"><span data-stu-id="49ab5-104">Azure Diagnostics is hello component used toocollect performance counters and other statistics from Azure Virtual Machines, Virtual Machine Scale Sets, Service Fabric, and Cloud Services.</span></span>  <span data-ttu-id="49ab5-105">Tato stránka je pouze relevantní, pokud používáte některou z těchto služeb.</span><span class="sxs-lookup"><span data-stu-id="49ab5-105">This page is only relevant if you are using one of these services.</span></span>
>

<span data-ttu-id="49ab5-106">Azure Diagnostics se používá s dalšími produkty Microsoftu diagnostiky jako monitorování Azure, Application Insights a analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="49ab5-106">Azure Diagnostics is used with other Microsoft diagnostics products like Azure Monitor, Application Insights, and Log Analytics.</span></span>

<span data-ttu-id="49ab5-107">Hello Azure Diagnostics konfigurační soubor definuje hodnoty, které jsou používané tooinitialize hello monitorování diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="49ab5-107">hello Azure Diagnostics configuration file defines values that are used tooinitialize hello Diagnostics Monitor.</span></span> <span data-ttu-id="49ab5-108">Tento soubor je použité tooinitialize konfigurace diagnostiky nastavení při monitorování diagnostiky hello spustí.</span><span class="sxs-lookup"><span data-stu-id="49ab5-108">This file is used tooinitialize diagnostic configuration settings when hello diagnostics monitor starts.</span></span>  

 <span data-ttu-id="49ab5-109">Ve výchozím nastavení, soubor schéma konfigurace Azure Diagnostics hello je nainstalovaná toohello `C:\Program Files\Microsoft SDKs\Azure\.NET SDK\<version>\schemas` adresáře.</span><span class="sxs-lookup"><span data-stu-id="49ab5-109">By default, hello Azure Diagnostics configuration schema file is installed toohello `C:\Program Files\Microsoft SDKs\Azure\.NET SDK\<version>\schemas` directory.</span></span> <span data-ttu-id="49ab5-110">Nahraďte `<version>` hello nainstalované verzi hello [Azure SDK](http://www.windowsazure.com/develop/downloads/).</span><span class="sxs-lookup"><span data-stu-id="49ab5-110">Replace `<version>` with hello installed version of hello [Azure SDK](http://www.windowsazure.com/develop/downloads/).</span></span>  

> [!NOTE]
>  <span data-ttu-id="49ab5-111">Hello diagnostiky konfigurační soubor se obvykle používá s spuštění úlohy, které vyžadují toobe diagnostických dat shromážděných dříve v procesu spouštění hello.</span><span class="sxs-lookup"><span data-stu-id="49ab5-111">hello diagnostics configuration file is typically used with startup tasks that require diagnostic data toobe collected earlier in hello startup process.</span></span> <span data-ttu-id="49ab5-112">Další informace o používání Azure Diagnostics najdete v tématu [shromažďovat protokolování dat pomocí Azure Diagnostics](assetId:///83a91c23-5ca2-4fc9-8df3-62036c37a3d7).</span><span class="sxs-lookup"><span data-stu-id="49ab5-112">For more information about using Azure Diagnostics, see [Collect Logging Data by Using Azure Diagnostics](assetId:///83a91c23-5ca2-4fc9-8df3-62036c37a3d7).</span></span>  

## <a name="example-of-hello-diagnostics-configuration-file"></a><span data-ttu-id="49ab5-113">Příklad souboru konfigurace diagnostiky hello</span><span class="sxs-lookup"><span data-stu-id="49ab5-113">Example of hello diagnostics configuration file</span></span>  
 <span data-ttu-id="49ab5-114">Hello následující příklad ukazuje konfigurační soubor typické diagnostics:</span><span class="sxs-lookup"><span data-stu-id="49ab5-114">hello following example shows a typical diagnostics configuration file:</span></span>  

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

## <a name="diagnosticsconfiguration-namespace"></a><span data-ttu-id="49ab5-115">Namespace DiagnosticsConfiguration</span><span class="sxs-lookup"><span data-stu-id="49ab5-115">DiagnosticsConfiguration Namespace</span></span>  
 <span data-ttu-id="49ab5-116">obor názvů XML Hello hello diagnostiky konfiguračního souboru je:</span><span class="sxs-lookup"><span data-stu-id="49ab5-116">hello XML namespace for hello diagnostics configuration file is:</span></span>  

```  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  
```  

## <a name="schema-elements"></a><span data-ttu-id="49ab5-117">Prvky schématu</span><span class="sxs-lookup"><span data-stu-id="49ab5-117">Schema Elements</span></span>  
 <span data-ttu-id="49ab5-118">Hello diagnostiky konfigurační soubor obsahuje následující prvky hello.</span><span class="sxs-lookup"><span data-stu-id="49ab5-118">hello diagnostics configuration file includes hello following elements.</span></span>


## <a name="diagnosticmonitorconfiguration-element"></a><span data-ttu-id="49ab5-119">DiagnosticMonitorConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="49ab5-119">DiagnosticMonitorConfiguration Element</span></span>  
<span data-ttu-id="49ab5-120">element nejvyšší úrovně Hello hello diagnostiky konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="49ab5-120">hello top-level element of hello diagnostics configuration file.</span></span>  

<span data-ttu-id="49ab5-121">Atributy:</span><span class="sxs-lookup"><span data-stu-id="49ab5-121">Attributes:</span></span>

|<span data-ttu-id="49ab5-122">Atribut</span><span class="sxs-lookup"><span data-stu-id="49ab5-122">Attribute</span></span>  |<span data-ttu-id="49ab5-123">Typ</span><span class="sxs-lookup"><span data-stu-id="49ab5-123">Type</span></span>   |<span data-ttu-id="49ab5-124">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="49ab5-124">Required</span></span>| <span data-ttu-id="49ab5-125">Výchozí</span><span class="sxs-lookup"><span data-stu-id="49ab5-125">Default</span></span> | <span data-ttu-id="49ab5-126">Popis</span><span class="sxs-lookup"><span data-stu-id="49ab5-126">Description</span></span>|  
|-----------|-------|--------|---------|------------|  
|<span data-ttu-id="49ab5-127">**configurationChangePollInterval**</span><span class="sxs-lookup"><span data-stu-id="49ab5-127">**configurationChangePollInterval**</span></span>|<span data-ttu-id="49ab5-128">Doba trvání</span><span class="sxs-lookup"><span data-stu-id="49ab5-128">duration</span></span>|<span data-ttu-id="49ab5-129">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="49ab5-129">Optional</span></span> | <span data-ttu-id="49ab5-130">PT1M</span><span class="sxs-lookup"><span data-stu-id="49ab5-130">PT1M</span></span>| <span data-ttu-id="49ab5-131">Určuje hello interval, ve které hlasování hello monitorování diagnostiky pro změny konfigurace diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="49ab5-131">Specifies hello interval at which hello diagnostic monitor polls for diagnostic configuration changes.</span></span>|  
|<span data-ttu-id="49ab5-132">**overallQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="49ab5-132">**overallQuotaInMB**</span></span>|<span data-ttu-id="49ab5-133">celé číslo bez znaménka</span><span class="sxs-lookup"><span data-stu-id="49ab5-133">unsignedInt</span></span>|<span data-ttu-id="49ab5-134">Nepovinné</span><span class="sxs-lookup"><span data-stu-id="49ab5-134">Optional</span></span>| <span data-ttu-id="49ab5-135">4000 MB.</span><span class="sxs-lookup"><span data-stu-id="49ab5-135">4000 MB.</span></span> <span data-ttu-id="49ab5-136">Pokud zadáte hodnotu, nesmí být delší než toto množství</span><span class="sxs-lookup"><span data-stu-id="49ab5-136">If you provide a value, it must not exceed this amount</span></span> |<span data-ttu-id="49ab5-137">Hello celkovou velikost souboru systému úložiště přidělené pro všechny protokolování vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="49ab5-137">hello total amount of file system storage allocated for all logging buffers.</span></span>|  

## <a name="diagnosticinfrastructurelogs-element"></a><span data-ttu-id="49ab5-138">DiagnosticInfrastructureLogs Element</span><span class="sxs-lookup"><span data-stu-id="49ab5-138">DiagnosticInfrastructureLogs Element</span></span>  
<span data-ttu-id="49ab5-139">Definuje konfiguraci hello vyrovnávací paměti pro hello protokoly, které jsou generovány hello základní infrastruktura diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="49ab5-139">Defines hello buffer configuration for hello logs that are generated by hello underlying diagnostics infrastructure.</span></span>

<span data-ttu-id="49ab5-140">Nadřazený Element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).</span><span class="sxs-lookup"><span data-stu-id="49ab5-140">Parent Element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).</span></span>  

<span data-ttu-id="49ab5-141">Atributy:</span><span class="sxs-lookup"><span data-stu-id="49ab5-141">Attributes:</span></span>

|<span data-ttu-id="49ab5-142">Atribut</span><span class="sxs-lookup"><span data-stu-id="49ab5-142">Attribute</span></span>|<span data-ttu-id="49ab5-143">Typ</span><span class="sxs-lookup"><span data-stu-id="49ab5-143">Type</span></span>|<span data-ttu-id="49ab5-144">Popis</span><span class="sxs-lookup"><span data-stu-id="49ab5-144">Description</span></span>|  
|---------|----|-----------------|  
|<span data-ttu-id="49ab5-145">**bufferQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="49ab5-145">**bufferQuotaInMB**</span></span>|<span data-ttu-id="49ab5-146">celé číslo bez znaménka</span><span class="sxs-lookup"><span data-stu-id="49ab5-146">unsignedInt</span></span>|<span data-ttu-id="49ab5-147">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="49ab5-147">Optional.</span></span> <span data-ttu-id="49ab5-148">Určuje maximální množství hello úložiště systému souboru, který je k dispozici pro hello zadaná data.</span><span class="sxs-lookup"><span data-stu-id="49ab5-148">Specifies hello maximum amount of file system storage that is available for hello specified data.</span></span><br /><br /> <span data-ttu-id="49ab5-149">Hello výchozí hodnota je 0.</span><span class="sxs-lookup"><span data-stu-id="49ab5-149">hello default is 0.</span></span>|  
|<span data-ttu-id="49ab5-150">**scheduledTransferLogLevelFilter**</span><span class="sxs-lookup"><span data-stu-id="49ab5-150">**scheduledTransferLogLevelFilter**</span></span>|<span data-ttu-id="49ab5-151">Řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab5-151">string</span></span>|<span data-ttu-id="49ab5-152">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="49ab5-152">Optional.</span></span> <span data-ttu-id="49ab5-153">Určuje hello minimální úroveň závažnosti pro položky protokolu, které se přenáší.</span><span class="sxs-lookup"><span data-stu-id="49ab5-153">Specifies hello minimum severity level for log entries that are transferred.</span></span> <span data-ttu-id="49ab5-154">Hello výchozí hodnota je **Undefined**.</span><span class="sxs-lookup"><span data-stu-id="49ab5-154">hello default value is **Undefined**.</span></span> <span data-ttu-id="49ab5-155">Další možné hodnoty jsou **podrobné**, **informace**, **upozornění**, **chyba**, a **kritický**.</span><span class="sxs-lookup"><span data-stu-id="49ab5-155">Other possible values are **Verbose**, **Information**, **Warning**, **Error**, and **Critical**.</span></span>|  
|<span data-ttu-id="49ab5-156">**scheduledTransferPeriod**</span><span class="sxs-lookup"><span data-stu-id="49ab5-156">**scheduledTransferPeriod**</span></span>|<span data-ttu-id="49ab5-157">Doba trvání</span><span class="sxs-lookup"><span data-stu-id="49ab5-157">duration</span></span>|<span data-ttu-id="49ab5-158">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="49ab5-158">Optional.</span></span> <span data-ttu-id="49ab5-159">Určuje interval hello od naplánovanou přenosů dat, zaokrouhlit toohello nejbližší minutu.</span><span class="sxs-lookup"><span data-stu-id="49ab5-159">Specifies hello interval between scheduled transfers of data, rounded up toohello nearest minute.</span></span><br /><br /> <span data-ttu-id="49ab5-160">Výchozí hodnota Hello je PT0S.</span><span class="sxs-lookup"><span data-stu-id="49ab5-160">hello default is PT0S.</span></span>|  

## <a name="logs-element"></a><span data-ttu-id="49ab5-161">Element protokoly</span><span class="sxs-lookup"><span data-stu-id="49ab5-161">Logs Element</span></span>  
 <span data-ttu-id="49ab5-162">Definuje konfiguraci hello vyrovnávací paměti pro základní Azure protokoly.</span><span class="sxs-lookup"><span data-stu-id="49ab5-162">Defines hello buffer configuration for basic Azure logs.</span></span>

 <span data-ttu-id="49ab5-163">Nadřazený element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).</span><span class="sxs-lookup"><span data-stu-id="49ab5-163">Parent element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).</span></span>  

<span data-ttu-id="49ab5-164">Atributy:</span><span class="sxs-lookup"><span data-stu-id="49ab5-164">Attributes:</span></span>  

|<span data-ttu-id="49ab5-165">Atribut</span><span class="sxs-lookup"><span data-stu-id="49ab5-165">Attribute</span></span>|<span data-ttu-id="49ab5-166">Typ</span><span class="sxs-lookup"><span data-stu-id="49ab5-166">Type</span></span>|<span data-ttu-id="49ab5-167">Popis</span><span class="sxs-lookup"><span data-stu-id="49ab5-167">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="49ab5-168">**bufferQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="49ab5-168">**bufferQuotaInMB**</span></span>|<span data-ttu-id="49ab5-169">celé číslo bez znaménka</span><span class="sxs-lookup"><span data-stu-id="49ab5-169">unsignedInt</span></span>|<span data-ttu-id="49ab5-170">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="49ab5-170">Optional.</span></span> <span data-ttu-id="49ab5-171">Určuje maximální množství hello úložiště systému souboru, který je k dispozici pro hello zadaná data.</span><span class="sxs-lookup"><span data-stu-id="49ab5-171">Specifies hello maximum amount of file system storage that is available for hello specified data.</span></span><br /><br /> <span data-ttu-id="49ab5-172">Hello výchozí hodnota je 0.</span><span class="sxs-lookup"><span data-stu-id="49ab5-172">hello default is 0.</span></span>|  
|<span data-ttu-id="49ab5-173">**scheduledTransferLogLevelFilter**</span><span class="sxs-lookup"><span data-stu-id="49ab5-173">**scheduledTransferLogLevelFilter**</span></span>|<span data-ttu-id="49ab5-174">Řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab5-174">string</span></span>|<span data-ttu-id="49ab5-175">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="49ab5-175">Optional.</span></span> <span data-ttu-id="49ab5-176">Určuje hello minimální úroveň závažnosti pro položky protokolu, které se přenáší.</span><span class="sxs-lookup"><span data-stu-id="49ab5-176">Specifies hello minimum severity level for log entries that are transferred.</span></span> <span data-ttu-id="49ab5-177">Hello výchozí hodnota je **Undefined**.</span><span class="sxs-lookup"><span data-stu-id="49ab5-177">hello default value is **Undefined**.</span></span> <span data-ttu-id="49ab5-178">Další možné hodnoty jsou **podrobné**, **informace**, **upozornění**, **chyba**, a **kritický**.</span><span class="sxs-lookup"><span data-stu-id="49ab5-178">Other possible values are **Verbose**, **Information**, **Warning**, **Error**, and **Critical**.</span></span>|  
|<span data-ttu-id="49ab5-179">**scheduledTransferPeriod**</span><span class="sxs-lookup"><span data-stu-id="49ab5-179">**scheduledTransferPeriod**</span></span>|<span data-ttu-id="49ab5-180">Doba trvání</span><span class="sxs-lookup"><span data-stu-id="49ab5-180">duration</span></span>|<span data-ttu-id="49ab5-181">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="49ab5-181">Optional.</span></span> <span data-ttu-id="49ab5-182">Určuje interval hello od naplánovanou přenosů dat, zaokrouhlit toohello nejbližší minutu.</span><span class="sxs-lookup"><span data-stu-id="49ab5-182">Specifies hello interval between scheduled transfers of data, rounded up toohello nearest minute.</span></span><br /><br /> <span data-ttu-id="49ab5-183">Výchozí hodnota Hello je PT0S.</span><span class="sxs-lookup"><span data-stu-id="49ab5-183">hello default is PT0S.</span></span>|  

## <a name="directories-element"></a><span data-ttu-id="49ab5-184">Element adresáře</span><span class="sxs-lookup"><span data-stu-id="49ab5-184">Directories Element</span></span>  
<span data-ttu-id="49ab5-185">Definuje konfiguraci hello vyrovnávací paměti na základě souborů protokolů, které můžete definovat.</span><span class="sxs-lookup"><span data-stu-id="49ab5-185">Defines hello buffer configuration for file-based logs that you can define.</span></span>

<span data-ttu-id="49ab5-186">Nadřazený element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).</span><span class="sxs-lookup"><span data-stu-id="49ab5-186">Parent element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).</span></span>  


<span data-ttu-id="49ab5-187">Atributy:</span><span class="sxs-lookup"><span data-stu-id="49ab5-187">Attributes:</span></span>  

|<span data-ttu-id="49ab5-188">Atribut</span><span class="sxs-lookup"><span data-stu-id="49ab5-188">Attribute</span></span>|<span data-ttu-id="49ab5-189">Typ</span><span class="sxs-lookup"><span data-stu-id="49ab5-189">Type</span></span>|<span data-ttu-id="49ab5-190">Popis</span><span class="sxs-lookup"><span data-stu-id="49ab5-190">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="49ab5-191">**bufferQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="49ab5-191">**bufferQuotaInMB**</span></span>|<span data-ttu-id="49ab5-192">celé číslo bez znaménka</span><span class="sxs-lookup"><span data-stu-id="49ab5-192">unsignedInt</span></span>|<span data-ttu-id="49ab5-193">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="49ab5-193">Optional.</span></span> <span data-ttu-id="49ab5-194">Určuje maximální množství hello úložiště systému souboru, který je k dispozici pro hello zadaná data.</span><span class="sxs-lookup"><span data-stu-id="49ab5-194">Specifies hello maximum amount of file system storage that is available for hello specified data.</span></span><br /><br /> <span data-ttu-id="49ab5-195">Hello výchozí hodnota je 0.</span><span class="sxs-lookup"><span data-stu-id="49ab5-195">hello default is 0.</span></span>|  
|<span data-ttu-id="49ab5-196">**scheduledTransferPeriod**</span><span class="sxs-lookup"><span data-stu-id="49ab5-196">**scheduledTransferPeriod**</span></span>|<span data-ttu-id="49ab5-197">Doba trvání</span><span class="sxs-lookup"><span data-stu-id="49ab5-197">duration</span></span>|<span data-ttu-id="49ab5-198">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="49ab5-198">Optional.</span></span> <span data-ttu-id="49ab5-199">Určuje interval hello od naplánovanou přenosů dat, zaokrouhlit toohello nejbližší minutu.</span><span class="sxs-lookup"><span data-stu-id="49ab5-199">Specifies hello interval between scheduled transfers of data, rounded up toohello nearest minute.</span></span><br /><br /> <span data-ttu-id="49ab5-200">Výchozí hodnota Hello je PT0S.</span><span class="sxs-lookup"><span data-stu-id="49ab5-200">hello default is PT0S.</span></span>|  

## <a name="crashdumps-element"></a><span data-ttu-id="49ab5-201">CrashDumps Element</span><span class="sxs-lookup"><span data-stu-id="49ab5-201">CrashDumps Element</span></span>  
 <span data-ttu-id="49ab5-202">Definuje adresář výpisy hello havárií.</span><span class="sxs-lookup"><span data-stu-id="49ab5-202">Defines hello crash dumps directory.</span></span>

 <span data-ttu-id="49ab5-203">Nadřazený Element: [adresáře Element](#Directories).</span><span class="sxs-lookup"><span data-stu-id="49ab5-203">Parent Element: [Directories Element](#Directories).</span></span>  

<span data-ttu-id="49ab5-204">Atributy:</span><span class="sxs-lookup"><span data-stu-id="49ab5-204">Attributes:</span></span>  

|<span data-ttu-id="49ab5-205">Atribut</span><span class="sxs-lookup"><span data-stu-id="49ab5-205">Attribute</span></span>|<span data-ttu-id="49ab5-206">Typ</span><span class="sxs-lookup"><span data-stu-id="49ab5-206">Type</span></span>|<span data-ttu-id="49ab5-207">Popis</span><span class="sxs-lookup"><span data-stu-id="49ab5-207">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="49ab5-208">**kontejner**</span><span class="sxs-lookup"><span data-stu-id="49ab5-208">**container**</span></span>|<span data-ttu-id="49ab5-209">Řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab5-209">string</span></span>|<span data-ttu-id="49ab5-210">Název Hello hello kontejneru, kde je obsah hello hello adresáře toobe přenést.</span><span class="sxs-lookup"><span data-stu-id="49ab5-210">hello name of hello container where hello contents of hello directory is toobe transferred.</span></span>|  
|<span data-ttu-id="49ab5-211">**directoryQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="49ab5-211">**directoryQuotaInMB**</span></span>|<span data-ttu-id="49ab5-212">celé číslo bez znaménka</span><span class="sxs-lookup"><span data-stu-id="49ab5-212">unsignedInt</span></span>|<span data-ttu-id="49ab5-213">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="49ab5-213">Optional.</span></span> <span data-ttu-id="49ab5-214">Určuje maximální velikost hello hello adresáře v megabajtech.</span><span class="sxs-lookup"><span data-stu-id="49ab5-214">Specifies hello maximum size of hello directory in megabytes.</span></span><br /><br /> <span data-ttu-id="49ab5-215">Hello výchozí hodnota je 0.</span><span class="sxs-lookup"><span data-stu-id="49ab5-215">hello default is 0.</span></span>|  

## <a name="failedrequestlogs-element"></a><span data-ttu-id="49ab5-216">FailedRequestLogs Element</span><span class="sxs-lookup"><span data-stu-id="49ab5-216">FailedRequestLogs Element</span></span>  
 <span data-ttu-id="49ab5-217">Definuje adresář protokolu hello chybných požadavků.</span><span class="sxs-lookup"><span data-stu-id="49ab5-217">Defines hello failed request log directory.</span></span>

 <span data-ttu-id="49ab5-218">Nadřazený Element [adresáře Element](#Directories).</span><span class="sxs-lookup"><span data-stu-id="49ab5-218">Parent Element [Directories Element](#Directories).</span></span>  

<span data-ttu-id="49ab5-219">Atributy:</span><span class="sxs-lookup"><span data-stu-id="49ab5-219">Attributes:</span></span>  

|<span data-ttu-id="49ab5-220">Atribut</span><span class="sxs-lookup"><span data-stu-id="49ab5-220">Attribute</span></span>|<span data-ttu-id="49ab5-221">Typ</span><span class="sxs-lookup"><span data-stu-id="49ab5-221">Type</span></span>|<span data-ttu-id="49ab5-222">Popis</span><span class="sxs-lookup"><span data-stu-id="49ab5-222">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="49ab5-223">**kontejner**</span><span class="sxs-lookup"><span data-stu-id="49ab5-223">**container**</span></span>|<span data-ttu-id="49ab5-224">Řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab5-224">string</span></span>|<span data-ttu-id="49ab5-225">Název Hello hello kontejneru, kde je obsah hello hello adresáře toobe přenést.</span><span class="sxs-lookup"><span data-stu-id="49ab5-225">hello name of hello container where hello contents of hello directory is toobe transferred.</span></span>|  
|<span data-ttu-id="49ab5-226">**directoryQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="49ab5-226">**directoryQuotaInMB**</span></span>|<span data-ttu-id="49ab5-227">celé číslo bez znaménka</span><span class="sxs-lookup"><span data-stu-id="49ab5-227">unsignedInt</span></span>|<span data-ttu-id="49ab5-228">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="49ab5-228">Optional.</span></span> <span data-ttu-id="49ab5-229">Určuje maximální velikost hello hello adresáře v megabajtech.</span><span class="sxs-lookup"><span data-stu-id="49ab5-229">Specifies hello maximum size of hello directory in megabytes.</span></span><br /><br /> <span data-ttu-id="49ab5-230">Hello výchozí hodnota je 0.</span><span class="sxs-lookup"><span data-stu-id="49ab5-230">hello default is 0.</span></span>|  

##  <a name="iislogs-element"></a><span data-ttu-id="49ab5-231">IISLogs Element</span><span class="sxs-lookup"><span data-stu-id="49ab5-231">IISLogs Element</span></span>  
 <span data-ttu-id="49ab5-232">Definuje adresář protokolu služby IIS hello.</span><span class="sxs-lookup"><span data-stu-id="49ab5-232">Defines hello IIS log directory.</span></span>

 <span data-ttu-id="49ab5-233">Nadřazený Element [adresáře Element](#Directories).</span><span class="sxs-lookup"><span data-stu-id="49ab5-233">Parent Element [Directories Element](#Directories).</span></span>  

<span data-ttu-id="49ab5-234">Atributy:</span><span class="sxs-lookup"><span data-stu-id="49ab5-234">Attributes:</span></span>  

|<span data-ttu-id="49ab5-235">Atribut</span><span class="sxs-lookup"><span data-stu-id="49ab5-235">Attribute</span></span>|<span data-ttu-id="49ab5-236">Typ</span><span class="sxs-lookup"><span data-stu-id="49ab5-236">Type</span></span>|<span data-ttu-id="49ab5-237">Popis</span><span class="sxs-lookup"><span data-stu-id="49ab5-237">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="49ab5-238">**kontejner**</span><span class="sxs-lookup"><span data-stu-id="49ab5-238">**container**</span></span>|<span data-ttu-id="49ab5-239">Řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab5-239">string</span></span>|<span data-ttu-id="49ab5-240">Název Hello hello kontejneru, kde je obsah hello hello adresáře toobe přenést.</span><span class="sxs-lookup"><span data-stu-id="49ab5-240">hello name of hello container where hello contents of hello directory is toobe transferred.</span></span>|  
|<span data-ttu-id="49ab5-241">**directoryQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="49ab5-241">**directoryQuotaInMB**</span></span>|<span data-ttu-id="49ab5-242">celé číslo bez znaménka</span><span class="sxs-lookup"><span data-stu-id="49ab5-242">unsignedInt</span></span>|<span data-ttu-id="49ab5-243">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="49ab5-243">Optional.</span></span> <span data-ttu-id="49ab5-244">Určuje maximální velikost hello hello adresáře v megabajtech.</span><span class="sxs-lookup"><span data-stu-id="49ab5-244">Specifies hello maximum size of hello directory in megabytes.</span></span><br /><br /> <span data-ttu-id="49ab5-245">Hello výchozí hodnota je 0.</span><span class="sxs-lookup"><span data-stu-id="49ab5-245">hello default is 0.</span></span>|  

## <a name="datasources-element"></a><span data-ttu-id="49ab5-246">Element zdrojů dat</span><span class="sxs-lookup"><span data-stu-id="49ab5-246">DataSources Element</span></span>  
 <span data-ttu-id="49ab5-247">Definuje nula nebo více dodatečných protokolů adresáře.</span><span class="sxs-lookup"><span data-stu-id="49ab5-247">Defines zero or more additional log directories.</span></span>

 <span data-ttu-id="49ab5-248">Nadřazený Element: [adresáře Element](#Directories).</span><span class="sxs-lookup"><span data-stu-id="49ab5-248">Parent Element: [Directories Element](#Directories).</span></span>

## <a name="directoryconfiguration-element"></a><span data-ttu-id="49ab5-249">DirectoryConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="49ab5-249">DirectoryConfiguration Element</span></span>  
 <span data-ttu-id="49ab5-250">Definuje adresář hello toomonitor soubory protokolu.</span><span class="sxs-lookup"><span data-stu-id="49ab5-250">Defines hello directory of log files toomonitor.</span></span>

 <span data-ttu-id="49ab5-251">Nadřazený Element: [zdrojů dat Element](#DataSources).</span><span class="sxs-lookup"><span data-stu-id="49ab5-251">Parent Element: [DataSources Element](#DataSources).</span></span>

<span data-ttu-id="49ab5-252">Atributy:</span><span class="sxs-lookup"><span data-stu-id="49ab5-252">Attributes:</span></span>

|<span data-ttu-id="49ab5-253">Atribut</span><span class="sxs-lookup"><span data-stu-id="49ab5-253">Attribute</span></span>|<span data-ttu-id="49ab5-254">Typ</span><span class="sxs-lookup"><span data-stu-id="49ab5-254">Type</span></span>|<span data-ttu-id="49ab5-255">Popis</span><span class="sxs-lookup"><span data-stu-id="49ab5-255">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="49ab5-256">**kontejner**</span><span class="sxs-lookup"><span data-stu-id="49ab5-256">**container**</span></span>|<span data-ttu-id="49ab5-257">Řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab5-257">string</span></span>|<span data-ttu-id="49ab5-258">Název Hello hello kontejneru, kde je obsah hello hello adresáře toobe přenést.</span><span class="sxs-lookup"><span data-stu-id="49ab5-258">hello name of hello container where hello contents of hello directory is toobe transferred.</span></span>|  
|<span data-ttu-id="49ab5-259">**directoryQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="49ab5-259">**directoryQuotaInMB**</span></span>|<span data-ttu-id="49ab5-260">celé číslo bez znaménka</span><span class="sxs-lookup"><span data-stu-id="49ab5-260">unsignedInt</span></span>|<span data-ttu-id="49ab5-261">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="49ab5-261">Optional.</span></span> <span data-ttu-id="49ab5-262">Určuje maximální velikost hello hello adresáře v megabajtech.</span><span class="sxs-lookup"><span data-stu-id="49ab5-262">Specifies hello maximum size of hello directory in megabytes.</span></span><br /><br /> <span data-ttu-id="49ab5-263">Hello výchozí hodnota je 0.</span><span class="sxs-lookup"><span data-stu-id="49ab5-263">hello default is 0.</span></span>|  

## <a name="absolute-element"></a><span data-ttu-id="49ab5-264">Absolutní – Element</span><span class="sxs-lookup"><span data-stu-id="49ab5-264">Absolute Element</span></span>  
 <span data-ttu-id="49ab5-265">Definuje absolutní cesta toomonitor directory hello s prostředí volitelné rozšíření.</span><span class="sxs-lookup"><span data-stu-id="49ab5-265">Defines an absolute path of hello directory toomonitor with optional environment expansion.</span></span>

 <span data-ttu-id="49ab5-266">Nadřazený Element: [DirectoryConfiguration Element](#DirectoryConfiguration).</span><span class="sxs-lookup"><span data-stu-id="49ab5-266">Parent Element: [DirectoryConfiguration Element](#DirectoryConfiguration).</span></span>  

<span data-ttu-id="49ab5-267">Atributy:</span><span class="sxs-lookup"><span data-stu-id="49ab5-267">Attributes:</span></span>  

|<span data-ttu-id="49ab5-268">Atribut</span><span class="sxs-lookup"><span data-stu-id="49ab5-268">Attribute</span></span>|<span data-ttu-id="49ab5-269">Typ</span><span class="sxs-lookup"><span data-stu-id="49ab5-269">Type</span></span>|<span data-ttu-id="49ab5-270">Popis</span><span class="sxs-lookup"><span data-stu-id="49ab5-270">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="49ab5-271">**Cesta**</span><span class="sxs-lookup"><span data-stu-id="49ab5-271">**path**</span></span>|<span data-ttu-id="49ab5-272">Řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab5-272">string</span></span>|<span data-ttu-id="49ab5-273">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="49ab5-273">Required.</span></span> <span data-ttu-id="49ab5-274">Hello toomonitor directory toohello absolutní cesta.</span><span class="sxs-lookup"><span data-stu-id="49ab5-274">hello absolute path toohello directory toomonitor.</span></span>|  
|<span data-ttu-id="49ab5-275">**expandEnvironment**</span><span class="sxs-lookup"><span data-stu-id="49ab5-275">**expandEnvironment**</span></span>|<span data-ttu-id="49ab5-276">Logická hodnota</span><span class="sxs-lookup"><span data-stu-id="49ab5-276">boolean</span></span>|<span data-ttu-id="49ab5-277">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="49ab5-277">Required.</span></span> <span data-ttu-id="49ab5-278">Pokud nastavení příliš**true**, jsou-li rozbalit proměnné prostředí v cestě hello.</span><span class="sxs-lookup"><span data-stu-id="49ab5-278">If set too**true**, environment variables in hello path are expanded.</span></span>|  

## <a name="localresource-element"></a><span data-ttu-id="49ab5-279">LocalResource Element</span><span class="sxs-lookup"><span data-stu-id="49ab5-279">LocalResource Element</span></span>  
 <span data-ttu-id="49ab5-280">Definuje cestu relativní tooa místního prostředku v definici služby hello.</span><span class="sxs-lookup"><span data-stu-id="49ab5-280">Defines a path relative tooa local resource defined in hello service definition.</span></span>

 <span data-ttu-id="49ab5-281">Nadřazený Element: [DirectoryConfiguration Element](#DirectoryConfiguration).</span><span class="sxs-lookup"><span data-stu-id="49ab5-281">Parent Element: [DirectoryConfiguration Element](#DirectoryConfiguration).</span></span>  

<span data-ttu-id="49ab5-282">Atributy:</span><span class="sxs-lookup"><span data-stu-id="49ab5-282">Attributes:</span></span>  

|<span data-ttu-id="49ab5-283">Atribut</span><span class="sxs-lookup"><span data-stu-id="49ab5-283">Attribute</span></span>|<span data-ttu-id="49ab5-284">Typ</span><span class="sxs-lookup"><span data-stu-id="49ab5-284">Type</span></span>|<span data-ttu-id="49ab5-285">Popis</span><span class="sxs-lookup"><span data-stu-id="49ab5-285">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="49ab5-286">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="49ab5-286">**name**</span></span>|<span data-ttu-id="49ab5-287">Řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab5-287">string</span></span>|<span data-ttu-id="49ab5-288">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="49ab5-288">Required.</span></span> <span data-ttu-id="49ab5-289">Název Hello hello místní prostředek, který obsahuje hello directory toomonitor.</span><span class="sxs-lookup"><span data-stu-id="49ab5-289">hello name of hello local resource that contains hello directory toomonitor.</span></span>|  
|<span data-ttu-id="49ab5-290">**relativePath**</span><span class="sxs-lookup"><span data-stu-id="49ab5-290">**relativePath**</span></span>|<span data-ttu-id="49ab5-291">Řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab5-291">string</span></span>|<span data-ttu-id="49ab5-292">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="49ab5-292">Required.</span></span> <span data-ttu-id="49ab5-293">Hello cesta relativní toohello místní prostředek toomonitor.</span><span class="sxs-lookup"><span data-stu-id="49ab5-293">hello path relative toohello local resource toomonitor.</span></span>|  

## <a name="performancecounters-element"></a><span data-ttu-id="49ab5-294">PerformanceCounters – Element</span><span class="sxs-lookup"><span data-stu-id="49ab5-294">PerformanceCounters Element</span></span>  
 <span data-ttu-id="49ab5-295">Definuje hello cesta toohello výkonu čítač toocollect.</span><span class="sxs-lookup"><span data-stu-id="49ab5-295">Defines hello path toohello performance counter toocollect.</span></span>

 <span data-ttu-id="49ab5-296">Nadřazený Element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).</span><span class="sxs-lookup"><span data-stu-id="49ab5-296">Parent Element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).</span></span>


 <span data-ttu-id="49ab5-297">Atributy:</span><span class="sxs-lookup"><span data-stu-id="49ab5-297">Attributes:</span></span>  

|<span data-ttu-id="49ab5-298">Atribut</span><span class="sxs-lookup"><span data-stu-id="49ab5-298">Attribute</span></span>|<span data-ttu-id="49ab5-299">Typ</span><span class="sxs-lookup"><span data-stu-id="49ab5-299">Type</span></span>|<span data-ttu-id="49ab5-300">Popis</span><span class="sxs-lookup"><span data-stu-id="49ab5-300">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="49ab5-301">**bufferQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="49ab5-301">**bufferQuotaInMB**</span></span>|<span data-ttu-id="49ab5-302">celé číslo bez znaménka</span><span class="sxs-lookup"><span data-stu-id="49ab5-302">unsignedInt</span></span>|<span data-ttu-id="49ab5-303">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="49ab5-303">Optional.</span></span> <span data-ttu-id="49ab5-304">Určuje maximální množství hello úložiště systému souboru, který je k dispozici pro hello zadaná data.</span><span class="sxs-lookup"><span data-stu-id="49ab5-304">Specifies hello maximum amount of file system storage that is available for hello specified data.</span></span><br /><br /> <span data-ttu-id="49ab5-305">Hello výchozí hodnota je 0.</span><span class="sxs-lookup"><span data-stu-id="49ab5-305">hello default is 0.</span></span>|  
|<span data-ttu-id="49ab5-306">**scheduledTransferPeriod**</span><span class="sxs-lookup"><span data-stu-id="49ab5-306">**scheduledTransferPeriod**</span></span>|<span data-ttu-id="49ab5-307">Doba trvání</span><span class="sxs-lookup"><span data-stu-id="49ab5-307">duration</span></span>|<span data-ttu-id="49ab5-308">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="49ab5-308">Optional.</span></span> <span data-ttu-id="49ab5-309">Určuje interval hello od naplánovanou přenosů dat, zaokrouhlit toohello nejbližší minutu.</span><span class="sxs-lookup"><span data-stu-id="49ab5-309">Specifies hello interval between scheduled transfers of data, rounded up toohello nearest minute.</span></span><br /><br /> <span data-ttu-id="49ab5-310">Výchozí hodnota Hello je PT0S.</span><span class="sxs-lookup"><span data-stu-id="49ab5-310">hello default is PT0S.</span></span>|  

## <a name="performancecounterconfiguration-element"></a><span data-ttu-id="49ab5-311">PerformanceCounterConfiguration Element</span><span class="sxs-lookup"><span data-stu-id="49ab5-311">PerformanceCounterConfiguration Element</span></span>  
 <span data-ttu-id="49ab5-312">Definuje toocollect čítače výkonu hello.</span><span class="sxs-lookup"><span data-stu-id="49ab5-312">Defines hello performance counter toocollect.</span></span>

 <span data-ttu-id="49ab5-313">Nadřazený Element: [PerformanceCounters – Element](#PerformanceCounters).</span><span class="sxs-lookup"><span data-stu-id="49ab5-313">Parent Element: [PerformanceCounters Element](#PerformanceCounters).</span></span>  

 <span data-ttu-id="49ab5-314">Atributy:</span><span class="sxs-lookup"><span data-stu-id="49ab5-314">Attributes:</span></span>  

|<span data-ttu-id="49ab5-315">Atribut</span><span class="sxs-lookup"><span data-stu-id="49ab5-315">Attribute</span></span>|<span data-ttu-id="49ab5-316">Typ</span><span class="sxs-lookup"><span data-stu-id="49ab5-316">Type</span></span>|<span data-ttu-id="49ab5-317">Popis</span><span class="sxs-lookup"><span data-stu-id="49ab5-317">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="49ab5-318">**counterSpecifier**</span><span class="sxs-lookup"><span data-stu-id="49ab5-318">**counterSpecifier**</span></span>|<span data-ttu-id="49ab5-319">Řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab5-319">string</span></span>|<span data-ttu-id="49ab5-320">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="49ab5-320">Required.</span></span> <span data-ttu-id="49ab5-321">Hello toocollect čítače výkonu toohello cesta.</span><span class="sxs-lookup"><span data-stu-id="49ab5-321">hello path toohello performance counter toocollect.</span></span>|  
|<span data-ttu-id="49ab5-322">**sampleRate**</span><span class="sxs-lookup"><span data-stu-id="49ab5-322">**sampleRate**</span></span>|<span data-ttu-id="49ab5-323">Doba trvání</span><span class="sxs-lookup"><span data-stu-id="49ab5-323">duration</span></span>|<span data-ttu-id="49ab5-324">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="49ab5-324">Required.</span></span> <span data-ttu-id="49ab5-325">rychlost Hello, na které hello by měly být shromažďovány čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="49ab5-325">hello rate at which hello performance counter should be collected.</span></span>|  

## <a name="windowseventlog-element"></a><span data-ttu-id="49ab5-326">WindowsEventLog Element</span><span class="sxs-lookup"><span data-stu-id="49ab5-326">WindowsEventLog Element</span></span>  
 <span data-ttu-id="49ab5-327">Definuje toomonitor hello protokoly událostí.</span><span class="sxs-lookup"><span data-stu-id="49ab5-327">Defines hello event logs toomonitor.</span></span>

 <span data-ttu-id="49ab5-328">Nadřazený Element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).</span><span class="sxs-lookup"><span data-stu-id="49ab5-328">Parent Element: [DiagnosticMonitorConfiguration Element](#DiagnosticMonitorConfiguration).</span></span>

  <span data-ttu-id="49ab5-329">Atributy:</span><span class="sxs-lookup"><span data-stu-id="49ab5-329">Attributes:</span></span>

|<span data-ttu-id="49ab5-330">Atribut</span><span class="sxs-lookup"><span data-stu-id="49ab5-330">Attribute</span></span>|<span data-ttu-id="49ab5-331">Typ</span><span class="sxs-lookup"><span data-stu-id="49ab5-331">Type</span></span>|<span data-ttu-id="49ab5-332">Popis</span><span class="sxs-lookup"><span data-stu-id="49ab5-332">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="49ab5-333">**bufferQuotaInMB**</span><span class="sxs-lookup"><span data-stu-id="49ab5-333">**bufferQuotaInMB**</span></span>|<span data-ttu-id="49ab5-334">celé číslo bez znaménka</span><span class="sxs-lookup"><span data-stu-id="49ab5-334">unsignedInt</span></span>|<span data-ttu-id="49ab5-335">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="49ab5-335">Optional.</span></span> <span data-ttu-id="49ab5-336">Určuje maximální množství hello úložiště systému souboru, který je k dispozici pro hello zadaná data.</span><span class="sxs-lookup"><span data-stu-id="49ab5-336">Specifies hello maximum amount of file system storage that is available for hello specified data.</span></span><br /><br /> <span data-ttu-id="49ab5-337">Hello výchozí hodnota je 0.</span><span class="sxs-lookup"><span data-stu-id="49ab5-337">hello default is 0.</span></span>|  
|<span data-ttu-id="49ab5-338">**scheduledTransferLogLevelFilter**</span><span class="sxs-lookup"><span data-stu-id="49ab5-338">**scheduledTransferLogLevelFilter**</span></span>|<span data-ttu-id="49ab5-339">Řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab5-339">string</span></span>|<span data-ttu-id="49ab5-340">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="49ab5-340">Optional.</span></span> <span data-ttu-id="49ab5-341">Určuje hello minimální úroveň závažnosti pro položky protokolu, které se přenáší.</span><span class="sxs-lookup"><span data-stu-id="49ab5-341">Specifies hello minimum severity level for log entries that are transferred.</span></span> <span data-ttu-id="49ab5-342">Hello výchozí hodnota je **Undefined**.</span><span class="sxs-lookup"><span data-stu-id="49ab5-342">hello default value is **Undefined**.</span></span> <span data-ttu-id="49ab5-343">Další možné hodnoty jsou **podrobné**, **informace**, **upozornění**, **chyba**, a **kritický**.</span><span class="sxs-lookup"><span data-stu-id="49ab5-343">Other possible values are **Verbose**, **Information**, **Warning**, **Error**, and **Critical**.</span></span>|  
|<span data-ttu-id="49ab5-344">**scheduledTransferPeriod**</span><span class="sxs-lookup"><span data-stu-id="49ab5-344">**scheduledTransferPeriod**</span></span>|<span data-ttu-id="49ab5-345">Doba trvání</span><span class="sxs-lookup"><span data-stu-id="49ab5-345">duration</span></span>|<span data-ttu-id="49ab5-346">Volitelné.</span><span class="sxs-lookup"><span data-stu-id="49ab5-346">Optional.</span></span> <span data-ttu-id="49ab5-347">Určuje interval hello od naplánovanou přenosů dat, zaokrouhlit toohello nejbližší minutu.</span><span class="sxs-lookup"><span data-stu-id="49ab5-347">Specifies hello interval between scheduled transfers of data, rounded up toohello nearest minute.</span></span><br /><br /> <span data-ttu-id="49ab5-348">Výchozí hodnota Hello je PT0S.</span><span class="sxs-lookup"><span data-stu-id="49ab5-348">hello default is PT0S.</span></span>|  

## <a name="datasource-element"></a><span data-ttu-id="49ab5-349">Element zdroje dat</span><span class="sxs-lookup"><span data-stu-id="49ab5-349">DataSource Element</span></span>  
 <span data-ttu-id="49ab5-350">Definuje toomonitor hello protokolu událostí.</span><span class="sxs-lookup"><span data-stu-id="49ab5-350">Defines hello event log toomonitor.</span></span>

 <span data-ttu-id="49ab5-351">Nadřazený Element: [WindowsEventLog Element](#windowsEventLog).</span><span class="sxs-lookup"><span data-stu-id="49ab5-351">Parent Element: [WindowsEventLog Element](#windowsEventLog).</span></span>  

 <span data-ttu-id="49ab5-352">Atributy:</span><span class="sxs-lookup"><span data-stu-id="49ab5-352">Attributes:</span></span>

|<span data-ttu-id="49ab5-353">Atribut</span><span class="sxs-lookup"><span data-stu-id="49ab5-353">Attribute</span></span>|<span data-ttu-id="49ab5-354">Typ</span><span class="sxs-lookup"><span data-stu-id="49ab5-354">Type</span></span>|<span data-ttu-id="49ab5-355">Popis</span><span class="sxs-lookup"><span data-stu-id="49ab5-355">Description</span></span>|  
|---------------|----------|-----------------|  
|<span data-ttu-id="49ab5-356">**Jméno**</span><span class="sxs-lookup"><span data-stu-id="49ab5-356">**name**</span></span>|<span data-ttu-id="49ab5-357">Řetězec</span><span class="sxs-lookup"><span data-stu-id="49ab5-357">string</span></span>|<span data-ttu-id="49ab5-358">Povinná hodnota.</span><span class="sxs-lookup"><span data-stu-id="49ab5-358">Required.</span></span> <span data-ttu-id="49ab5-359">Výraz XPath určující toocollect protokolu hello.</span><span class="sxs-lookup"><span data-stu-id="49ab5-359">An XPath expression specifying hello log toocollect.</span></span>|  
