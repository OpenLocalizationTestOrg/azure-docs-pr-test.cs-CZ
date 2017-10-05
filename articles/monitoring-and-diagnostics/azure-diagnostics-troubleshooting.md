---
title: "Řešení potíží s Azure Diagnostics | Microsoft Docs"
description: "Vyřešení problémů při používání Azure diagnostics v Azure Virtual Machines, Service Fabric nebo cloudové služby."
services: monitoring-and-diagnostics
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: 66469bce-d457-4d1e-b550-a08d2be4d28c
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/12/2017
ms.author: robb
ms.openlocfilehash: a0cb529836b14df71e83616f4f625a002c535b7b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-diagnostics-troubleshooting"></a>Řešení potíží s Azure Diagnostics
Řešení potíží s informací, které jsou relevantní pro používání Azure Diagnostics. Další informace o Azure diagnostics najdete v tématu [přehled Azure Diagnostics](azure-diagnostics.md).

## <a name="logical-components"></a>Logické součásti
**Modul plug-in Spouštěče diagnostiky (DiagnosticsPluginLauncher.exe)**: spustí rozšíření Azure Diagnostics. Slouží jako položka bodu procesu.

**Modul plug-in diagnostiky (DiagnosticsPlugin.exe)**: hlavní proces, který je spuštěn Spouštěče výše a konfiguruje Monitoring Agent, spustí ji a spravuje celé jeho životnosti. 

**Monitoring Agent (MonAgent\*procesy .exe)**: tyto procesy udělat velkou část pracovní; například monitorování, kolekce a přenos dat diagnostiky.  

## <a name="logartifact-paths"></a>Protokol/artefaktů cesty
Tady jsou cesty k některé důležité protokoly a artefakty. Jsme zachovat odkazující na tyto ve zbývající části dokumentu:
### <a name="cloud-services"></a>Cloud Services
| Artefaktů | Cesta |
| --- | --- |
| **Azure Diagnostics konfiguračního souboru** | %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<verze > \Config.txt |
| **Soubory protokolu** | C:\Logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<verze > \ |
| **Místní úložiště pro data diagnostiky** | C:\Resources\Directory\<CloudServiceDeploymentID >.\< RoleName >. DiagnosticStore\WAD0107\Tables |
| **Agent konfiguračním souborem monitorování** | C:\Resources\Directory\<CloudServiceDeploymentID >.\< RoleName >. DiagnosticStore\WAD0107\Configuration\MaConfig.xml |
| **Balíček rozšíření Azure diagnostics** | %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<verze > |
| **Cesta kolekce nástroje protokolu** | %SystemDrive%\Packages\GuestAgent\ |
| **Soubor protokolu MonAgentHost** | C:\Resources\Directory\<CloudServiceDeploymentID >.\< RoleName >. DiagnosticStore\WAD0107\Configuration\MonAgentHost. < seq_num > .log |

### <a name="virtual-machines"></a>Virtuální počítače
| Artefaktů | Cesta |
| --- | --- |
| **Azure Diagnostics konfiguračního souboru** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<verze > \RuntimeSettings |
| **Soubory protokolu** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<verze > \Logs\ |
| **Místní úložiště pro data diagnostiky** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion > \WAD0107\Tables |
| **Agent konfiguračním souborem monitorování** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion > \WAD0107\Configuration\MaConfig.xml |
| **Stav souboru** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<verze > \Status |
| **Balíček rozšíření Azure diagnostics** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion >|
| **Cesta kolekce nástroje protokolu** | C:\WindowsAzure\Packages |
| **Soubor protokolu MonAgentHost** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion > \WAD0107\Configuration\MonAgentHost. < seq_num > .log |

## <a name="metric-data-doesnt-show-in-azure-portal"></a>Data metriky nezobrazí na portálu Azure
Azure Diagnostics obsahuje spoustu metriky data, která lze zobrazit na portálu Azure. Pokud máte problémy s zobrazuje těchto dat na portálu, zkontrolujte účet úložiště diagnostiky -> WADMetrics\* tabulce najdete, pokud existují odpovídající záznamy metriky. Zde PartitionKey tabulky je ID prostředku virtuálního počítače nebo škálovací sadu virtuálních počítačů, a RowKey je název metriky tj. název čítače výkonu.

Pokud je ID prostředku není v pořádku, zkontrolujte konfiguraci diagnostiky -> metriky -> ResourceId, pokud je ID prostředku nastavena správně.

Pokud nejsou žádná data pro konkrétní metriku, zkontrolujte konfiguraci diagnostiky -> PerformanceCounter, jestli jsou zahrnuté metrika (čítače výkonu). Ve výchozím nastavení povolíme následující čítače.
- \Processor(_Total)\% Processor Time
- \Memory\Available Bytes
- \ASP.NET aplikace (__celkový__) \Requests/Sec
- \ASP.NET aplikace (__celkový__) \Errors celkem/s
- \ASP.NET\Requests zařazených do fronty
- \ASP.NET\Requests odmítl
- \Processor(W3wp)\% času procesoru
- Bajty \Private \Process (w3wp)
- \Process(WaIISHost)\% času procesoru
- Bajty \Private \Process (WaIISHost)
- \Process(WaWorkerHost)\% času procesoru
- Bajty \Private \Process (WaWorkerHost)
- \Memory\Page chyby/s
- \.NET CLR paměti (_globální_)\% čas
- Zápis \Disk \LogicalDisk (C:) bajty/s
- \Disk \LogicalDisk (C:) přečtených bajtů/s
- Zápis \Disk \LogicalDisk (D:) bajty/s
- \Disk \LogicalDisk (D:) přečtených bajtů/s

Pokud je správně nastavena konfigurace, ale stále nemůžete vidět data metriky, postupujte podle pokynů níže pro další šetření.


## <a name="azure-diagnostics-is-not-starting"></a>Azure Diagnostics není spouštění.
Podívejte se na **DiagnosticsPluginLauncher.log** a **DiagnosticsPlugin.log** soubory z umístění souborů protokolu výše uvedeného informace o Proč se nepodařilo spustit diagnostiku. 

Pokud tyto protokoly znamenat `Monitoring Agent not reporting success after launch`, znamená to, došlo k chybě spuštění MonAgentHost.exe. Vyhledejte v protokolech, v umístění určeném pro `MonAgentHost log file` výše v části.

Poslední řádek souboru protokolu obsahuje ukončovací kód.  

```
DiagnosticsPluginLauncher.exe Information: 0 : [4/16/2016 6:24:15 AM] DiagnosticPlugin exited with code 0
```
Pokud zjistíte, **záporné** ukončovací kód, najdete [ukončovací kód tabulky](#azure-diagnostics-plugin-exit-codes) v [odkazy](#references).

## <a name="diagnostics-data-is-not-logged-to-azure-storage"></a>Diagnostická Data není přihlášení do služby Azure Storage
Určete, jestli je zobrazovat žádná data, nebo pouze některá data se nezobrazí.

### <a name="diagnostics-infrastructure-logs"></a>Diagnostické protokoly infrastruktury
Diagnostické protokoly infrastruktury je, kam azure diagnostics protokoluje všechny chyby, které běží na. Zajistěte, aby mít povolené ([postup?](#how-to-check-diagnostics-extension-configuration)) zaznamenání protokolů diagnostiky infrastruktury v konfiguraci a rychle vyhledat všechny relevantní chyby, které se zobrazí v `DiagnosticInfrastructureLogsTable` tabulky ve vašem účtu úložiště.

### <a name="no-data-is-showing-up"></a>Žádná data se zobrazuje, protože
Nejčastější příčinou události, který data úplně chybí je nesprávně definované informace o účtu úložiště.

Řešení: Opravte konfiguraci diagnostiky a znovu nainstalujte diagnostiky.

Pokud účet úložiště není správně nakonfigurovaná, vzdálené plochy do počítače a ujistěte se, že DiagnosticsPlugin.exe a MonAgentCore.exe běží. Pokud neběží, postupujte podle [Azure Diagnostics není od](#azure-diagnostics-is-not-starting). Pokud procesů, které běží, přejít na [je data získávání zaznamenaná místně](#is-data-getting-captured-locally) a postupujte podle této příručky odtud na.

### <a name="part-of-the-data-is-missing"></a>Chybí část dat
Pokud jsou získáme nějaká data, ale ne jiné. To znamená shromažďování dat nebo přenos kanálu nastavena správně. Můžete zúžit co je problém, postupujte podle zde témata:
#### <a name="is-collection-configured"></a>Je nakonfigurované kolekce: 
Konfigurace diagnostiky obsahuje část, která se dá pokyn pro určitý typ dat, které se mají shromažďovat. [Zkontrolujte konfiguraci](#how-to-check-diagnostics-extension-configuration) a ujistěte se, nejsou vyhledávání dat, které jste nenakonfigurovali pro kolekci.
#### <a name="is-the-host-generating-data"></a>Je hostitel generování dat:
- **Čítače výkonu**: Otevřete perfmon a zkontrolujte čítač.
- **Protokoly trasování**: použijte vzdálenou plochu do virtuálního počítače a přidejte TextWriterTraceListener konfiguračního souboru aplikace.  V tématu http://msdn.microsoft.com/library/sk36c28t.aspx nastavit naslouchacího procesu text.  Zajistěte, aby `<trace>` má element `<trace autoflush="true">`.<br />
Pokud nevidíte generováno protokoly trasování, postupujte podle [více o trasování protokoly chybí](#more-about-trace-logs-missing).
- **Trasování událostí pro Windows trasování**: Vzdálená plocha do virtuálních počítačů a nainstalujte nástroje PerfView.  Spuštění souboru -> Nástroje PerfView příkazu User -> etwprovder1 naslouchání, etwprovider2 atd.  Všimněte si, že je příkaz naslouchání velká a malá písmena a nemůže být mezery mezi čárkami oddělený seznam zprostředkovatelů trasování událostí pro Windows.  Pokud příkaz se nezdaří, můžete klikněte na tlačítko 'Protokolu' v pravém dolním rohu nástroj nástroje Perfview co došlo k pokusu o spuštění a jaký byl výsledek.  Za předpokladu, že vstup je správná, že pak se nové okno pop nahoru a za několik sekund, bude zahájena zobrazuje trasování ETW.
- **Protokoly událostí**: použijte vzdálenou plochu do virtuálního počítače. Otevřete `Event Viewer` a nezapomeňte události.
#### <a name="is-data-getting-captured-locally"></a>Je získávání dat zachytit místně:
Dále zkontrolujte, zda že získávání zachycení místně.
Jsou data uložená místně v `*.tsf` soubory v [místní úložiště pro data diagnostiky](#log-artifacts-path). Různé druhy protokoly získat shromážděné v různých `.tsf` soubory. Názvy jsou podobné názvy tabulek v úložišti azure. Například `Performance Counters` získat shromážděných v `PerformanceCountersTable.tsf`, získat shromáždí protokoly událostí v `WindowsEventLogsTable.tsf`. Postupujte podle pokynů v [místní protokolu extrakce](#local-log-extraction) oddílu otevřete místního shromažďování souborů a zajistěte, aby je vidíte získávání shromážděných na disku.

Pokud nevidíte získávání místně shromážděné protokoly a již ověříte, že je hostitel generování dat, máte pravděpodobně problém konfigurace. Zkontrolujte konfiguraci pečlivě pro příslušné části. Také zkontrolujte konfiguraci vygenerované MonitoringAgent [MaConfig.xml](#log-artifacts-path) a zkontrolujte zkontrolujte, zda je některá část popisující zdroj příslušných protokolových a že nedojde ke ztrátě v překlad mezi konfigurace diagnostiky azure a monitorování konfigurace agenta.
#### <a name="is-data-getting-transferred"></a>Získávání přenosu dat:
Pokud jste ověřili získávání zachycení místně, ale přesto ho nevidíte ve vašem účtu úložiště: 
- Především Ujistěte se, zda jste zadali účet úložiště správný a že nebyly vráceny přes etc.for klíče účtu daného úložiště. Pro cloudové služby, někdy vidíme, že lidé Neaktualizovat jejich `useDevelopmentStorage=true`.
- Pokud předpokladu, že účet úložiště je správná. Ujistěte se, že nemáte určitá omezení sítě, které neumožňují komponenty k dosažení koncové body veřejné úložiště. Jeden způsob, jak to udělat, je Vzdálená plocha do počítače a akci k zápisu něco stejný účet úložiště sami.
- Nakonec můžete akci a zjistit, jaké selhání se hlášených Monitoring Agent. Agent monitorování protokoly zapisují `maeventtable.tsf` umístěný v [místní úložiště pro data diagnostiky](#log-artifacts-path). Postupujte podle pokynů v [místní protokolu extrakce](#local-log-extraction) části otevřít tento soubor a zkuste to a zjistěte, zda jsou `errors` označující selhání místní soubory číst nebo zapisovat do úložiště.

### <a name="capturing--archiving-logs"></a>Zaznamenání / archivaci protokoly
Došlo k prostřednictvím výše uvedené kroky, ale nelze zjistit, co byla chybná a jsou přemýšlení o obrátíte na podporu. Je první věcí, kterou může vás vyzvou k shromažďování protokolů z vašeho počítače. Díky této sami můžete ušetřit čas. Spustit `CollectGuestLogs.exe` nástroj na [cesta protokolu kolekce nástroje](#log-artifacts-path) a vygeneruje soubor zip s všechny relevantní azure protokoly ve stejné složce.

## <a name="diagnostics-data-tables-not-found"></a>Diagnostická data tabulky nebyl nalezen
Tabulky v úložišti Azure, která uchovává události trasování událostí pro Windows jsou název, pomocí následujícího kódu:

```C#
        if (String.IsNullOrEmpty(eventDestination)) {
            if (e == "DefaultEvents")
                tableName = "WADDefault" + MD5(provider);
            else
                tableName = "WADEvent" + MD5(provider) + eventId;
        }
        else
            tableName = "WAD" + eventDestination;
```

Zde naleznete příklad:

```XML
        <EtwEventSourceProviderConfiguration provider="prov1">
          <Event id="1" />
          <Event id="2" eventDestination="dest1" />
          <DefaultEvents />
        </EtwEventSourceProviderConfiguration>
        <EtwEventSourceProviderConfiguration provider="prov2">
          <DefaultEvents eventDestination="dest2" />
        </EtwEventSourceProviderConfiguration>
```
```JSON
"EtwEventSourceProviderConfiguration": [
    {
        "provider": "prov1",
        "Event": [
            {
                "id": 1
            },
            {
                "id": 2,
                "eventDestination": "dest1"
            }
        ],
        "DefaultEvents": {
            "eventDestination": "DefaultEventDestination",
            "sinks": ""
        }
    },
    {
        "provider": "prov2",
        "DefaultEvents": {
            "eventDestination": "dest2"
        }
    }
]
```
4 tabulky, která generuje:

| Událost | Název tabulky |
| --- | --- |
| Zprostředkovatel = "prov1" &lt;událost s id = "1" nebo&gt; |WADEvent + MD5("prov1") + "1" |
| Zprostředkovatel = "prov1" &lt;událost s id = "2" eventDestination = "dest1" /&gt; |WADdest1 |
| Zprostředkovatel = "prov1" &lt;DefaultEvents /&gt; |WADDefault+MD5("prov1") |
| Zprostředkovatel = "prov2" &lt;DefaultEvents eventDestination = "dest2" /&gt; |WADdest2 |

## <a name="references"></a>Odkazy

### <a name="how-to-check-diagnostics-extension-configuration"></a>Jak zkontrolovat konfiguraci rozšíření diagnostiky
Nejjednodušší způsob, jak zkontrolovat konfiguraci rozšíření je přejděte na http://resources.azure.com, přejděte do virtuálního počítače nebo Cloudová služba, na kterém rozšíření diagnostiky Azure (IaaSDiagnostics / PaaDiagnostics) je nejistá.

Alternativně použijte vzdálenou plochu do počítače a podívejte se na Azure Diagnostics konfiguračního souboru popsané v příslušné části [zde](#log-artifacts-path).

Buď případu vyhledávání pro **Microsoft.Azure.Diagnostics** pak pro **xmlCfg** nebo **WadCfg** pole. 

V případě virtuálních počítačů Pokud se nachází pole WadCfg, znamená to, že konfigurace je ve formátu JSON. Pokud pole xmlCfg k dispozici, znamená to, konfigurace je v kódu XML a je kódování base64. Budete muset [dekódovat](http://www.bing.com/search?q=base64+decoder) zobrazíte XML, který byl načteny diagnostiky.

Pro roli cloudové služby, pokud vyberete konfiguraci z disku, data je zakódovaný pomocí base64, budete muset [dekódovat](http://www.bing.com/search?q=base64+decoder) zobrazíte XML, který byl načteny diagnostiky.

### <a name="azure-diagnostics-plugin-exit-codes"></a>Kódy ukončení modul plug-in Azure Diagnostics
Tento modul plug-in vrátí následující ukončovací kód:

| Ukončovací kód | Popis |
| --- | --- |
| 0 |Úspěch |
| -1 |Obecná chyba. |
| -2 |Nelze načíst soubor rcf.<p>Tato interní chyba by měla pouze dojít, pokud spouštěč modulu plug-in agenta hosta je vyvolána ručně, nesprávně, na virtuálním počítači. |
| -3 |Nelze načíst konfigurační soubor diagnostiky.<p><p>Řešení: Způsobené konfigurační soubor není předávání ověřování schématu. Řešení je poskytnout konfiguračního souboru, který je v souladu se schématem. |
| -4 |Jiná instance agenta monitorování diagnostiky už používá adresář místních prostředků.<p><p>Řešení: Zadat jinou hodnotu pro **LocalResourceDirectory**. |
| -6 |Spouštěč hostovaného agenta modulu plug-in došlo k pokusu o spouštění diagnostiky neplatný příkazového řádku.<p><p>Tato interní chyba by měla pouze dojít, pokud spouštěč modulu plug-in agenta hosta je vyvolána ručně, nesprávně, na virtuálním počítači. |
| -10 |Modul plug-in diagnostiky byl ukončen se k neošetřené výjimce. |
| -11 |Agentovi hosta se nepodařilo vytvořit proces zodpovědný za spouštění a monitorování agent sledování.<p><p>Řešení: Ověřte, že jsou k dispozici nové procesy dostatek systémových prostředků.<p> |
| -101 |Neplatné argumenty při volání modulu plug-in diagnostiky.<p><p>Tato interní chyba by měla pouze dojít, pokud spouštěč modulu plug-in agenta hosta je vyvolána ručně, nesprávně, na virtuálním počítači. |
| -102 |Proces modulu plug-in se nepodařilo inicializovat.<p><p>Řešení: Ověřte, že jsou k dispozici nové procesy dostatek systémových prostředků. |
| -103 |Proces modulu plug-in se nepodařilo inicializovat. Konkrétně se nepodařilo vytvořit objekt protokolovacího nástroje.<p><p>Řešení: Ověřte, že jsou k dispozici nové procesy dostatek systémových prostředků. |
| -104 |Nelze načíst soubor rcf poskytované agenta hosta.<p><p>Tato interní chyba by měla pouze dojít, pokud spouštěč modulu plug-in agenta hosta je vyvolána ručně, nesprávně, na virtuálním počítači. |
| -105 |Modul plug-in Diagnostics nelze otevřít soubor konfigurace diagnostiky.<p><p>Tato interní chyba by mohlo dojít pouze v Pokud modul plug-in diagnostiky je vyvolána ručně, nesprávně, na virtuálním počítači. |
| -106 |Diagnostika konfigurační soubor nelze přečíst.<p><p>Řešení: Způsobené konfigurační soubor není předávání ověřování schématu. Proto řešením je zajistit konfiguračního souboru, který je v souladu se schématem. V tématu [jak zkontrolovat konfiguraci rozšíření diagnostiky](#how-to-check-diagnostics-extension-configuration). |
| -107 |Pass directory prostředků pro agenta monitorování je neplatný.<p><p>Tato interní chyba by mohlo dojít pouze v Pokud agenta monitorování je vyvolána ručně, nesprávně, na virtuálním počítači.</p> |
| -108 |Nebylo možné převést konfiguračního souboru diagnostiku do konfiguračního souboru monitorování agenta.<p><p>Tato interní chyba by mohlo dojít pouze v Pokud je modul plug-in diagnostiky je vyvolána ručně se souborem neplatná konfigurace. |
| -110 |Konfigurace diagnostiky obecné došlo k chybě.<p><p>Tato interní chyba by mohlo dojít pouze v Pokud je modul plug-in diagnostiky je vyvolána ručně se souborem neplatná konfigurace. |
| -111 |Nepodařilo se spustit agent sledování.<p><p>Řešení: Ověřte, že jsou k dispozici dostatek systémových prostředků. |
| -112 |Obecná chyba |

### <a name="local-log-extraction"></a>Extrakce místního protokolu
Agent monitorování shromažďuje protokoly a artefaktů jako `.tsf` soubory. `.tsf`soubor není možné číst, ale můžete ji do převést `.csv` následujícím způsobem: 

```
<Azure diagnostics extension package>\Monitor\x64\table2csv.exe <relevantLogFile>.tsf
```
Nový soubor s názvem `<relevantLogFile>.csv` budou vytvořeny v stejnou cestu jako odpovídající `.tsf` souboru.

**Poznámka:**: stačí ke spuštění tohoto nástroje u souboru hlavní tsf (například PerformanceCountersTable.tsf). Doprovodné soubory (například PerformanceCountersTables_\*\*001.tsf PerformanceCountersTables_\*\*002. tsf atd.) budou automaticky zpracovány.

### <a name="more-about-trace-logs-missing"></a>Další informace o chybí protokoly trasování

**Poznámka:** to většinou platí cloudových služeb, pouze pokud jste nakonfigurovali DiagnosticsMonitorTraceListener na aplikace běžící na vašem virtuálním počítači IaaS. 

- Ujistěte se, že že diagnosticmonitortracelistener je nakonfigurován v souboru web.config nebo app.config.  To je ve výchozím nastavení nakonfigurována v projekty cloudových služeb, ale někteří zákazníci komentář, který ho způsobí, že příkazy trasování, které nelze shromažďovat diagnostické. 
- Pokud protokoly získávání nezapisují z metody OnStart nebo spustit Ujistěte se, že se DiagnosticMonitorTraceListener souboru app.config.  Ve výchozím nastavení je v souboru web.config, ale které se týkají pouze kód spuštěný v rámci w3wp.exe; Proto musíte ho v souboru app.config zaznamenat trasování spuštění v WaIISHost.exe.
- Ujistěte se, že používáte Diagnostics.Trace.TraceXXX místo Diagnostics.Debug.WriteXXX.  Příkazy ladění se odeberou ze sestavení pro vydání.
- Ujistěte se, zda že zkompilovaný kód ve skutečnosti má řádky Diagnostics.Trace (použijte Reflector ildasm nebo ILSpy ověření).  Příkazy Diagnostics.Trace jsou vyřazeny z kompilované binární, pokud nechcete použít symbol Podmíněná kompilace trasování.  Pokud se používá pro sestavení projektu msbuild jde častých problémů narazíte na.

## <a name="known-issues-and-mitigations"></a>Známé problémy a jejich zmírnění
Tady je seznam známých problémů s známé způsoby zmírnění rizik:

**1. rozhraní .NET 4.5 závislost:**

WAD má závislost na modul runtime na rozhraní .NET 4.5 nebo vyšší. Všechny počítače, které jsou zřízené pro cloudové služby a také všechny oficiální azure základní bitové kopie virtuálních počítačů v době psaní to mít rozhraní .NET 4.5 nebo výše nainstalovat. Stále je ale možné zobrazovat v situaci, kdy zkusíte spustit WAD na počítači, který nemá .NET 4.5 nebo vyšší. To se stane, když vytvoříte svůj počítač z původní bitové kopie nebo snímek; nebo přineste vlastní disk.

To obvykle manifesty jako ukončovací kód **255** při spuštění DiagnosticsPluginLauncher.exe. Selhání dochází z důvodu neošetřené výjimky: 
```
System.IO.FileLoadException: Could not load file or assembly 'System.Threading.Tasks, Version=1.5.11.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies
```

**Omezení rizik:** instalace rozhraní .NET 4.5 nebo vyšší na váš počítač.

**2. Data čítače výkonu, které jsou k dispozici v úložiště, ale nezobrazují v portálu**

Virtuální počítače portálu prostředí ukazuje některé čítače výkonu ve výchozím nastavení. Pokud je nevidíte a víte, že data je generováno, protože je k dispozici v úložišti. Kontrola:
- Pokud data v úložišti mají názvy čítačů v angličtině. Pokud nejsou názvy čítačů v angličtině, nebude možné ho rozpoznat portálu metriky grafu.
- Pokud používáte zástupné znaky (\*) v názvech čítače výkonu, nebudou mít ke korelaci čítač nakonfigurované a shromáždění portálu.

**Zmírnění dopadů**: Změna počítače jazyka na angličtinu pro účty systému. Ovládací panely -> oblast -> správy -> Nastavení kopií -> tak, aby vlastní jazyk neplatí pro účet system, zrušte zaškrtnutí políčka "Úvodní obrazovky a systém účty". Ujistěte se, že nepoužíváte zástupné znaky, pokud chcete portál tak, aby se prostředí primární spotřeba také.
