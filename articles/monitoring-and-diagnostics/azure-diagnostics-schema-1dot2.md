---
title: "aaaAzure schéma konfigurace diagnostiky 1.2 | Microsoft Docs"
description: "Platí pouze pokud používáte Azure SDK 2.5 s Azure virtuálních počítačů sady škálování virtuálního počítače, Service Fabric nebo cloudové služby."
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
ms.openlocfilehash: 31559317b696556a64b51b58800b176ade9a4679
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-diagnostics-12-configuration-schema"></a>Schéma konfigurace Azure Diagnostics 1.2
> [!NOTE]
> Azure Diagnostics je čítače výkonu toocollect součástí používanou hello a další statistiky z virtuálních počítačů Azure, sady škálování virtuálního počítače, Service Fabric a cloudové služby.  Tato stránka je pouze relevantní, pokud používáte některou z těchto služeb.
>

Azure Diagnostics se používá s dalšími produkty Microsoftu diagnostiky jako monitorování Azure, Application Insights a analýzy protokolů.

Toto schéma definuje nastavení konfigurace diagnostiky tooinitialize můžete použít při spuštění programu hello diagnostiky sledování možné hodnoty hello.  


 Stáhněte si definice schématu souboru hello veřejné konfigurace tak, že spustíte následující příkaz prostředí PowerShell hello:  

```PowerShell  
(Get-AzureServiceAvailableExtension -ExtensionName 'PaaSDiagnostics' -ProviderNamespace 'Microsoft.Azure.Diagnostics').PublicConfigurationSchema | Out-File –Encoding utf8 -FilePath 'C:\temp\WadConfig.xsd'  
```  

 Další informace o používání Azure Diagnostics najdete v tématu [povolení diagnostiky ve službě Azure Cloud Services](http://azure.microsoft.com/documentation/articles/cloud-services-dotnet-diagnostics/).  

## <a name="example-of-hello-diagnostics-configuration-file"></a>Příklad souboru konfigurace diagnostiky hello  
 Hello následující příklad ukazuje konfigurační soubor typické diagnostics:  

```xml
<?xml version="1.0" encoding="utf-8"?>  
<PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">  
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
        <EtwEventSourceProviderConfiguration provider="MyProviderClass" scheduledTransferPeriod="PT5M">  
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
      <CrashDumps containerName="wad-crashdumps" directoryQuotaPercentage="30" dumpType="Mini">  
        <CrashDumpConfiguration processName="mynewprocess.exe" />  
        <CrashDumpConfiguration processName="badapp.exe"/>  
      </CrashDumps>  
    </DiagnosticMonitorConfiguration>  
  </WadCfg>  
</PublicConfig>  

```  

## <a name="diagnostics-configuration-namespace"></a>Namespace konfigurace diagnostiky  
 obor názvů XML Hello hello diagnostiky konfiguračního souboru je:  

```  
http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration  
```  

## <a name="publicconfig-element"></a>PublicConfig Element  
 Element nejvyšší úrovně hello diagnostiky konfiguračního souboru. Hello následující tabulka popisuje prvky hello hello konfiguračního souboru.  

|Název elementu|Popis|  
|------------------|-----------------|  
|**WadCfg**|Povinná hodnota. Nastavení konfigurace pro hello telemetrická data toobe shromážděn.|  
|**StorageAccount**|Název Hello hello hello data toostore účtu Azure Storage v. To můžete také zadat jako parametr při spuštění rutiny Set-AzureServiceDiagnosticsExtension hello.|  
|**LocalResourceDirectory**|Hello adresáře na virtuální počítač toobe hello používá hello data události toostore Monitoring Agent. Pokud není sada hello výchozí adresář slouží:<br /><br /> Pro roli pracovního procesu nebo webové:`C:\Resources\<guid>\directory\<guid>.<RoleName.DiagnosticStore\`<br /><br /> Pro virtuální počítač:`C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<WADVersion>\WAD<WADVersion>`<br /><br /> Povinné atributy jsou:<br /><br /> -                      **cesta** – hello v toobe systému hello používá Azure Diagnostics.<br /><br /> -                      **expandEnvironment** -Určuje, zda jsou v názvu cesty hello rozbalit proměnné prostředí.|  

## <a name="wadcfg-element"></a>WadCFG Element  
Definuje nastavení konfigurace pro hello telemetrická data toobe shromažďují. Hello následující tabulka popisuje podřízených elementů:  

|Název elementu|Popis|  
|------------------|-----------------|  
|**DiagnosticMonitorConfiguration**|Povinná hodnota. Volitelné atributy jsou:<br /><br /> -                     **overallQuotaInMB** -hello maximální množství místa místního disku, které mohou být spotřebovávána hello různých typů diagnostická data shromažďovaná společností Azure Diagnostics. Hello výchozí nastavení je 5 120 MB.<br /><br /> -                     **useProxyServer** -konfigurovat Azure Diagnostics toouse hello nastavení proxy serveru jako sada v nastavení aplikace Internet Explorer.|  
|**CrashDumps**|Povolte shromažďování výpisy stavu systému. Volitelné atributy jsou:<br /><br /> -                     **containerName** -hello název hello kontejneru objektů blob ve vašem toobe účtu Azure Storage používá toostore výpisy stavu systému.<br /><br /> -                     **crashDumpType** -nakonfiguruje Azure Diagnostics toocollect havárií malé nebo úplné výpisy paměti.<br /><br /> -                     **directoryQuotaPercentage**-nakonfiguruje hello procento **overallQuotaInMB** toobe vyhrazena pro výpisů stavu systému na hello virtuálních počítačů.|  
|**DiagnosticInfrastructureLogs**|Povolte shromažďování protokolů generovaných Azure Diagnostics. Hello infrastruktury diagnostické protokoly jsou užitečné při řešení potíží samotného systému hello diagnostiky. Volitelné atributy jsou:<br /><br /> -                     **scheduledTransferLogLevelFilter** -nakonfiguruje úroveň závažnosti minimální hello shromážděné protokoly hello.<br /><br /> -                     **scheduledTransferPeriod** -hello interval mezi naplánované přenosy toostorage zaokrouhlit toohello nejbližší minutu. Hodnota Hello [XML "Doba trvání datového typu."](http://www.w3schools.com/schema/schema_dtypes_date.asp)|  
|**Adresáře**|Umožňuje hello kolekce hello obsah adresáře, služba IIS se nezdařilo, přístup k protokolům požadavku nebo protokoly služby IIS. Volitelný atribut:<br /><br /> **scheduledTransferPeriod** -hello interval mezi naplánované přenosy toostorage zaokrouhlit toohello nejbližší minutu. Hodnota Hello [XML "Doba trvání datového typu."](http://www.w3schools.com/schema/schema_dtypes_date.asp)|  
|**EtwProviders**|Nakonfiguruje shromažďování událostí trasování událostí pro Windows z EventSource nebo Manifest trasování událostí pro Windows na základě zprostředkovatele.|  
|**Metriky**|Tento element umožňuje toogenerate tabulku čítače výkonu, která je optimalizovaná pro rychlé dotazy. Jednotlivých čítačů výkonu, která je definována v hello **čítače výkonu** element je uložena v tabulce hello metriky v tabulce čítače výkonu toohello přidání. Požadovaný atribut:<br /><br /> **resourceId** -Toto je ID prostředku hello hello nasazujete Azure Diagnostics do virtuálního počítače. Získat hello **resourceID** z hello [portál Azure](https://portal.azure.com). Vyberte **Procházet** -> **skupiny prostředků** -> **< název\>**. Klikněte na tlačítko hello **vlastnosti** dlaždici a zkopírujte hodnotu hello z hello **ID** pole.|  
|**Čítače výkonu**|Umožňuje hello kolekci čítačů výkonu. Volitelný atribut:<br /><br /> **scheduledTransferPeriod** -hello interval mezi naplánované přenosy toostorage zaokrouhlit toohello nejbližší minutu. Hodnota je [XML "Doba trvání datový typ".](http://www.w3schools.com/schema/schema_dtypes_date.asp)|  
|**WindowsEventLog**|Umožňuje hello kolekce protokoly událostí systému Windows. Volitelný atribut:<br /><br /> **scheduledTransferPeriod** -hello interval mezi naplánované přenosy toostorage zaokrouhlit toohello nejbližší minutu. Hodnota je [XML "Doba trvání datový typ".](http://www.w3schools.com/schema/schema_dtypes_date.asp)|  

## <a name="crashdumps-element"></a>CrashDumps Element  
 Umožňuje kolekce výpisy stavu systému. Hello následující tabulka popisuje podřízených elementů:  

|Název elementu|Popis|  
|------------------|-----------------|  
|**CrashDumpConfiguration**|Povinná hodnota. Požadovaný atribut:<br /><br /> **název_procesu** – hello název procesu hello chcete Azure Diagnostics toocollect stav systému pro.|  
|**crashDumpType**|Nakonfiguruje výpisy stavu Azure Diagnostics toocollect mini nebo celého systému.|  
|**directoryQuotaPercentage**|Nakonfiguruje hello procento **overallQuotaInMB** toobe vyhrazena pro výpisů stavu systému na hello virtuálních počítačů.|  

## <a name="directories-element"></a>Element adresáře  
 Umožňuje hello kolekce hello obsah adresáře, služba IIS se nezdařilo, přístup k protokolům požadavku nebo protokoly služby IIS. Hello následující tabulka popisuje podřízených elementů:  

|Název elementu|Popis|  
|------------------|-----------------|  
|**Zdroje dat**|Seznam toomonitor adresáře.|  
|**FailedRequestLogs**|Tento element včetně v konfiguraci hello umožňuje shromažďování protokolů o neúspěšných požadavků tooan služby IIS webu nebo aplikaci. Je také nutné povolit trasování možnosti v části **systému. Webový server** v **Web.config**.|  
|**IISLogs**|Tento element včetně v konfiguraci hello umožňuje hello shromažďování protokolů služby IIS:<br /><br /> **containerName** -protokoly služby IIS hello toostore použit název hello hello kontejneru objektů blob ve vašem toobe účet úložiště Azure.|  

## <a name="datasources-element"></a>Element zdrojů dat  
 Seznam toomonitor adresáře. Hello následující tabulka popisuje podřízených elementů:  

|Název elementu|Popis|  
|------------------|-----------------|  
|**DirectoryConfiguration**|Povinná hodnota. Požadovaný atribut:<br /><br /> **containerName** -hello název hello kontejneru objektů blob v Azure Storage účtu toostore toobe používá soubory protokolů hello.|  

## <a name="directoryconfiguration-element"></a>DirectoryConfiguration Element  
 **DirectoryConfiguration** může obsahovat buď hello **absolutní** nebo **LocalResource** elementu, ale ne obojí. Hello následující tabulka popisuje podřízených elementů:  

|Název elementu|Popis|  
|------------------|-----------------|  
|**Absolutní**|Hello toomonitor directory toohello absolutní cesta. vyžadují se Hello následující atributy:<br /><br /> -                     **Cesta** -hello toomonitor directory toohello absolutní cesta.<br /><br /> -                      **expandEnvironment** – konfiguruje, zda jsou rozbalit proměnné prostředí v cestě.|  
|**LocalResource**|Hello cesta relativní tooa místní prostředek toomonitor. Povinné atributy jsou:<br /><br /> -                     **Název** -hello místní prostředek, který obsahuje hello directory toomonitor<br /><br /> -                     **relativePath** -hello relativní tooName cestu, která obsahuje adresář toomonitor hello|  

## <a name="etwproviders-element"></a>EtwProviders Element  
 Nakonfiguruje shromažďování událostí trasování událostí pro Windows z EventSource nebo Manifest trasování událostí pro Windows na základě zprostředkovatele. Hello následující tabulka popisuje podřízených elementů:  

|Název elementu|Popis|  
|------------------|-----------------|  
|**EtwEventSourceProviderConfiguration**|Nakonfiguruje kolekce událostí generovaných [EventSource – třída](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx). Požadovaný atribut:<br /><br /> **Zprostředkovatel** -hello název třídy událostí EventSource hello.<br /><br /> Volitelné atributy jsou:<br /><br /> -                     **scheduledTransferLogLevelFilter** -hello účet úložiště tooyour úrovně tootransfer minimální závažnosti.<br /><br /> -                     **scheduledTransferPeriod** -hello interval mezi naplánované přenosy toostorage zaokrouhlit toohello nejbližší minutu. Hodnota je [datový typ XML trvání](http://www.w3schools.com/schema/schema_dtypes_date.asp).|  
|**EtwManifestProviderConfiguration**|Požadovaný atribut:<br /><br /> **Zprostředkovatel** -hello GUID hello událostí zprostředkovatele<br /><br /> Volitelné atributy jsou:<br /><br /> - **scheduledTransferLogLevelFilter** -hello účet úložiště tooyour úrovně tootransfer minimální závažnosti.<br /><br /> -                     **scheduledTransferPeriod** -hello interval mezi naplánované přenosy toostorage zaokrouhlit toohello nejbližší minutu. Hodnota je [datový typ XML trvání](http://www.w3schools.com/schema/schema_dtypes_date.asp).|  

## <a name="etweventsourceproviderconfiguration-element"></a>EtwEventSourceProviderConfiguration Element  
 Nakonfiguruje kolekce událostí generovaných [EventSource – třída](http://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource\(v=vs.110\).aspx). Hello následující tabulka popisuje podřízených elementů:  

|Název elementu|Popis|  
|------------------|-----------------|  
|**DefaultEvents**|Volitelný atribut:<br /><br /> **eventDestination** – hello název hello tabulky toostore hello události v|  
|**Události**|Požadovaný atribut:<br /><br /> **ID** -hello id události hello.<br /><br /> Volitelný atribut:<br /><br /> **eventDestination** – hello název hello tabulky toostore hello události v|  

## <a name="etwmanifestproviderconfiguration-element"></a>EtwManifestProviderConfiguration Element  
 Hello následující tabulka popisuje podřízených elementů:  

|Název elementu|Popis|  
|------------------|-----------------|  
|**DefaultEvents**|Volitelný atribut:<br /><br /> **eventDestination** – hello název hello tabulky toostore hello události v|  
|**Události**|Požadovaný atribut:<br /><br /> **ID** -hello id události hello.<br /><br /> Volitelný atribut:<br /><br /> **eventDestination** – hello název hello tabulky toostore hello události v|  

## <a name="metrics-element"></a>Element metriky  
 Umožní vám toogenerate tabulku čítače výkonu, která je optimalizovaná pro rychlé dotazy. Hello následující tabulka popisuje podřízených elementů:  

|Název elementu|Popis|  
|------------------|-----------------|  
|**MetricAggregation**|Požadovaný atribut:<br /><br /> **scheduledTransferPeriod** -hello interval mezi naplánované přenosy toostorage zaokrouhlit toohello nejbližší minutu. Hodnota je [datový typ XML trvání](http://www.w3schools.com/schema/schema_dtypes_date.asp).|  

## <a name="performancecounters-element"></a>PerformanceCounters – Element  
 Umožňuje hello kolekci čítačů výkonu. Hello následující tabulka popisuje podřízených elementů:  

|Název elementu|Popis|  
|------------------|-----------------|  
|**PerformanceCounterConfiguration**|vyžadují se Hello následující atributy:<br /><br /> -                     **counterSpecifier** hello – název čítače výkonu hello. Například, `\Processor(_Total)\% Processor Time`. tooget seznam čítačů výkonu v hostiteli spusťte příkaz hello `typeperf`.<br /><br /> -                     **sampleRate** – jak často hello čítač se odeberou.<br /><br /> Volitelný atribut:<br /><br /> **jednotka** -hello Měrná jednotka služby hello čítače.|  

## <a name="performancecounterconfiguration-element"></a>PerformanceCounterConfiguration Element  
 Hello následující tabulka popisuje podřízených elementů:  

|Název elementu|Popis|  
|------------------|-----------------|  
|**Poznámky**|Požadovaný atribut:<br /><br /> **displayName** -hello zobrazovaný název čítače hello<br /><br /> Volitelný atribut:<br /><br /> **národní prostředí** -hello toouse národního prostředí, při zobrazení název čítače hello|  

## <a name="windowseventlog-element"></a>WindowsEventLog Element  
 Hello následující tabulka popisuje podřízených elementů:  

|Název elementu|Popis|  
|------------------|-----------------|  
|**Zdroj dat**|toocollect protokoly událostí systému Windows Hello. Požadovaný atribut:<br /><br /> **název** -shromážděných dotaz XPath hello popisující toobe události windows hello. Například:<br /><br /> `Application!*[System[(Level >= 3)]], System!*[System[(Level <=3)]], System!*[System[Provider[@Name='Microsoft Antimalware']]], Security!*[System[(Level >= 3]]`<br /><br /> Zadejte všechny události, toocollect "*".|
