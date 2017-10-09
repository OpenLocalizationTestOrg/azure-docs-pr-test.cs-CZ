---
title: aaaTroubleshooting Azure Diagnostics | Microsoft Docs
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
ms.openlocfilehash: daaf9fa4c40982eb9ba04030d7e8ea1ad9fe085b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-diagnostics-troubleshooting"></a>Řešení potíží s Azure Diagnostics
Řešení potíží s toousing relevantní informace o Azure Diagnostics. Další informace o Azure diagnostics najdete v tématu [přehled Azure Diagnostics](azure-diagnostics.md).

## <a name="logical-components"></a>Logické součásti
**Modul plug-in Spouštěče diagnostiky (DiagnosticsPluginLauncher.exe)**: spustí rozšíření Azure Diagnostics hello. Slouží jako hello vstupního bodu procesu.

**Modul plug-in diagnostiky (DiagnosticsPlugin.exe)**: hlavní proces, který je spuštěn hello Spouštěče výše a konfiguruje hello Monitoring Agent, spustí ji a spravuje celé jeho životnosti. 

**Monitoring Agent (MonAgent\*procesy .exe)**: tyto procesy hello hromadné hello pracovní; například monitorování, kolekce a přenos hello diagnostická data.  

## <a name="logartifact-paths"></a>Protokol/artefaktů cesty
Tady jsou důležité protokoly toosome hello cesty a artefakty. Jsme zachovat odkazující toothese v rámci hello zbytek dokumentu hello:
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
Azure Diagnostics obsahuje spoustu metriky data, která lze zobrazit na portálu Azure. Pokud máte problémy s zobrazuje těchto dat na portálu, účet úložiště diagnostiky zkontrolujte hello -> WADMetrics\* tabulky toosee Pokud hello odpovídající metriky záznamy jsou k dispozici. Zde hello PartitionKey hello tabulky je ID prostředku hello virtuálního počítače nebo škálovací sadu virtuálních počítačů a hello RowKey je název metriky hello tj. název čítače výkonu.

Pokud hello ID prostředku není v pořádku, zkontrolujte konfiguraci diagnostiky -> metriky -> ResourceId toosee, pokud je ID prostředku hello správně nastavená.

Pokud nejsou žádná data pro konkrétní metrika hello, zkontrolujte konfiguraci diagnostiky -> PerformanceCounter toosee, pokud metrika hello (čítače výkonu) je součástí. Můžeme povolit hello následující čítače ve výchozím nastavení.
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

Pokud je správně nastavena konfigurace hello, ale stále nemůžete vidět data metriky hello, postupujte podle pokynů hello pod pro další šetření hello.


## <a name="azure-diagnostics-is-not-starting"></a>Azure Diagnostics není spouštění.
Podívejte se na **DiagnosticsPluginLauncher.log** a **DiagnosticsPlugin.log** soubory z umístění hello hello zadaný výše informace o důvod, proč diagnostiky se nezdařilo toostart soubory protokolu. 

Pokud tyto protokoly znamenat `Monitoring Agent not reporting success after launch`, znamená to, došlo k chybě spuštění MonAgentHost.exe. Podívejte se na hello protokoly pro tento v hello umístění pro `MonAgentHost log file` hello výše v části.

poslední řádek Hello hello souborů protokolu obsahuje hello ukončovací kód.  

```
DiagnosticsPluginLauncher.exe Information: 0 : [4/16/2016 6:24:15 AM] DiagnosticPlugin exited with code 0
```
Pokud zjistíte, **záporné** ukončovací kód, získáte toohello [ukončovací kód tabulky](#azure-diagnostics-plugin-exit-codes) v hello [odkazy](#references).

## <a name="diagnostics-data-is-not-logged-tooazure-storage"></a>Diagnostická Data se protokolují není tooAzure úložiště
Určete, jestli je zobrazovat žádná data, nebo jenom některé z dat hello se nezobrazí.

### <a name="diagnostics-infrastructure-logs"></a>Diagnostické protokoly infrastruktury
Diagnostické protokoly infrastruktury je, kam azure diagnostics protokoluje všechny chyby, které běží na. Zajistěte, aby mít povolené ([postup?](#how-to-check-diagnostics-extension-configuration)) zaznamenání protokolů diagnostiky infrastruktury v konfiguraci a rychle vyhledat všechny relevantní chyby, které se zobrazí v hello `DiagnosticInfrastructureLogsTable` tabulky ve vašem účtu úložiště.

### <a name="no-data-is-showing-up"></a>Žádná data se zobrazuje, protože
Nejčastější příčinou Hello úplně chybí dat událostí je informace o účtu nesprávně definované úložiště.

Řešení: Opravte konfiguraci diagnostiky a znovu nainstalujte diagnostiky.

Pokud účet úložiště hello je správně nakonfigurovaná, vzdálené plochy do hello počítač a ujistěte se, DiagnosticsPlugin.exe a MonAgentCore.exe běží. Pokud neběží, postupujte podle [Azure Diagnostics není od](#azure-diagnostics-is-not-starting). Pokud jsou spuštěny hello procesy, přeskočit příliš[je data získávání zaznamenaná místně](#is-data-getting-captured-locally) a postupujte podle této příručky odtud na.

### <a name="part-of-hello-data-is-missing"></a>Chybí část dat, hello
Pokud jsou získáme nějaká data, ale ne jiné. To znamená hello shromažďování dat nebo přenos kanálu nastavena správně. Postupujte podle hello témata zde toonarrow dolů jaký problém hello je:
#### <a name="is-collection-configured"></a>Je nakonfigurované kolekce: 
Konfigurace diagnostiky obsahuje hello část, která dá pokyn pro konkrétní typ dat toobe shromažďují. [Zkontrolujte konfiguraci](#how-to-check-diagnostics-extension-configuration) toomake, zda nejsou hledáte data můžete nenakonfigurovali pro kolekci.
#### <a name="is-hello-host-generating-data"></a>Je hostitel hello generování dat:
- **Čítače výkonu**: Otevřete perfmon a zkontrolujte hello čítače.
- **Protokoly trasování**: použijte vzdálenou plochu do hello virtuálního počítače a přidejte aplikace TextWriterTraceListener toohello konfigurační soubor.  V tématu http://msdn.microsoft.com/library/sk36c28t.aspx tooset až textu hello naslouchací proces.  Ujistěte se, zda text hello `<trace>` má element `<trace autoflush="true">`.<br />
Pokud nevidíte generováno protokoly trasování, postupujte podle [více o trasování protokoly chybí](#more-about-trace-logs-missing).
- **Trasování událostí pro Windows trasování**: použijte vzdálenou plochu do hello virtuálních počítačů a nainstalujte nástroje PerfView.  Spuštění souboru -> Nástroje PerfView příkazu User -> etwprovder1 naslouchání, etwprovider2 atd.  Všimněte si, že příkaz naslouchání hello je malá a velká písmena a nemůže být mezery mezi hello čárkami oddělený seznam zprostředkovatelů trasování událostí pro Windows.  Pokud se příkaz hello nezdaří toorun, můžete kliknutím na tlačítko 'Protokolu' hello v pravé dolní hello části hello nástroje Perfview nástroj toosee, co se pokus o toorun a jaké výsledek hello byl.  Za předpokladu, že vstup hello je správný, že pak se nové okno pop nahoru a za několik sekund bude zahájena, zobrazuje trasování ETW.
- **Protokoly událostí**: použijte vzdálenou plochu do hello virtuálních počítačů. Otevřete `Event Viewer` a nezapomeňte hello události.
#### <a name="is-data-getting-captured-locally"></a>Je získávání dat zachytit místně:
Dále zkontrolujte, zda hello data získávání zachycenou místně.
Hello data je místně uložená v `*.tsf` soubory v [hello místní úložiště pro data diagnostiky](#log-artifacts-path). Různé druhy protokoly získat shromážděné v různých `.tsf` soubory. názvy Hello jsou podobné toohello názvy tabulek v úložišti azure. Například `Performance Counters` získat shromážděných v `PerformanceCountersTable.tsf`, získat shromáždí protokoly událostí v `WindowsEventLogsTable.tsf`. Postupujte pokynů hello v [místní protokolu extrakce](#local-log-extraction) části tooopen hello místního shromažďování souborů a zajistěte, aby je vidíte získávání shromážděných na disku.

Pokud nepoužíváte najdete v části získávání místně shromážděné protokoly a již ověříte, že hostitel hello je generování dat, pravděpodobně máte problém konfigurace. Zkontrolujte konfiguraci pečlivě pro odpovídající části hello. Také zkontrolujte konfiguraci hello vygenerované MonitoringAgent [MaConfig.xml](#log-artifacts-path) a ujistěte se, je některá část popisující hello příslušných protokolových zdroje a že nedojde ke ztrátě v překlad mezi azure diagnostics konfigurace a monitorování konfigurace agenta.
#### <a name="is-data-getting-transferred"></a>Získávání přenosu dat:
Pokud jste ověřili získávání dat hello zachycenou místně, ale přesto ho nevidíte ve vašem účtu úložiště: 
- Především Ujistěte se, zda jste zadali účet úložiště správný a že nebyly převracet hello etc.for klíče zadaný účet úložiště. Pro cloudové služby, někdy vidíme, že lidé Neaktualizovat jejich `useDevelopmentStorage=true`.
- Pokud předpokladu, že účet úložiště je správná. Ujistěte se, že nemáte určitá omezení sítě, které neumožňují hello součásti koncové body tooreach veřejného úložiště. Jedním ze způsobů toodo, který je tooremote plochu do hello počítač a zkuste to toowrite něco toohello stejné úložiště účet sami.
- Nakonec můžete akci a zjistit, jaké selhání se hlášených Monitoring Agent. Agent monitorování protokoly zapisují `maeventtable.tsf` umístěný v [hello místní úložiště pro data diagnostiky](#log-artifacts-path). Postupujte podle pokynů v [místní protokolu extrakce](#local-log-extraction) části tooopen tento soubor a zkuste to a zjistěte, zda existují `errors` označující selhání tooread místní soubory nebo zapisovat toostorage.

### <a name="capturing--archiving-logs"></a>Zaznamenání / archivaci protokoly
Došlo k prostřednictvím hello výše kroky, ale nelze zjistit, co byla chybná a jsou přemýšlení o obrátíte na podporu. Hello první, může vás vyzvou je toocollect protokolů z vašeho počítače. Díky této sami můžete ušetřit čas. Spustit hello `CollectGuestLogs.exe` nástroj na [cesta protokolu kolekce nástroje](#log-artifacts-path) a vygeneruje soubor zip s všechny relevantní azure protokoly v hello stejné složce.

## <a name="diagnostics-data-tables-not-found"></a>Diagnostická data tabulky nebyl nalezen
Hello tabulek v úložišti Azure, která uchovává události trasování událostí pro Windows jsou pojmenované pomocí hello následující kód:

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

### <a name="how-toocheck-diagnostics-extension-configuration"></a>Jak toocheck konfiguraci rozšíření diagnostiky
Nejjednodušší způsob, jak toocheck Hello konfiguraci rozšíření toonavigate toohttp://resources.azure.com, přejít toohello virtuálního počítače nebo cloudové služby na které hello rozšíření diagnostiky Azure (IaaSDiagnostics / PaaDiagnostics) je nejistá.

Alternativně vzdálenou plochu do počítače hello a podívejte se na hello Azure Diagnostics konfigurační soubor popsané v příslušné části hello [zde](#log-artifacts-path).

Buď případu vyhledávání pro **Microsoft.Azure.Diagnostics** pak pro hello **xmlCfg** nebo **WadCfg** pole. 

V případě virtuálních počítačů Pokud se nachází hello WadCfg pole, znamená to, že konfigurace hello je ve formátu JSON. Pokud hello xmlCfg pole je k dispozici, znamená to, konfigurace hello je v kódu XML a je kódování base64. Je třeba příliš[dekódovat](http://www.bing.com/search?q=base64+decoder) toosee hello XML, který byl načteny diagnostiky.

Pro roli cloudové služby, pokud vyberete hello konfigurace z disku, hello dat je zakódovaný pomocí base64, budete potřebovat příliš[dekódovat](http://www.bing.com/search?q=base64+decoder) toosee hello XML, který byl načteny diagnostiky.

### <a name="azure-diagnostics-plugin-exit-codes"></a>Kódy ukončení modul plug-in Azure Diagnostics
modul plug-in Hello vrátí hello následující ukončovací kód:

| Ukončovací kód | Popis |
| --- | --- |
| 0 |Úspěch |
| -1 |Obecná chyba. |
| -2 |Nelze tooload hello rcf soubor.<p>Tato interní chyba by měla pouze dojít, pokud hello hostovaného agenta modulu plug-in Spouštěče je vyvolána ručně, nesprávně, na hello virtuálních počítačů. |
| -3 |Nelze načíst konfigurační soubor hello diagnostiky.<p><p>Řešení: Způsobené konfigurační soubor není předávání ověřování schématu. řešení Hello je tooprovide konfiguračního souboru, který odpovídá schématu hello. |
| -4 |Jiná instance hello monitoring agent. Diagnostika už používá adresář hello místních prostředků.<p><p>Řešení: Zadat jinou hodnotu pro **LocalResourceDirectory**. |
| -6 |Spouštěč modulu plug-in agenta hosta Hello k pokusu o diagnostiku toolaunch s neplatnou příkazového řádku.<p><p>Tato interní chyba by měla pouze dojít, pokud hello hostovaného agenta modulu plug-in Spouštěče je vyvolána ručně, nesprávně, na hello virtuálních počítačů. |
| -10 |modul plug-in diagnostiky Hello byl ukončen se k neošetřené výjimce. |
| -11 |agent hosta Hello se proces hello nelze toocreate zodpovědný za spouštění a monitorování hello monitoring agent..<p><p>Řešení: Ověřte, že dostatek systémových prostředků jsou k dispozici toolaunch nové procesů.<p> |
| -101 |Neplatné argumenty při volání modulu plug-in hello diagnostiky.<p><p>Tato interní chyba by měla pouze dojít, pokud hello hostovaného agenta modulu plug-in Spouštěče je vyvolána ručně, nesprávně, na hello virtuálních počítačů. |
| -102 |proces modulu plug-in Hello je nelze tooinitialize sám sebe.<p><p>Řešení: Ověřte, že dostatek systémových prostředků jsou k dispozici toolaunch nové procesů. |
| -103 |proces modulu plug-in Hello je nelze tooinitialize sám sebe. Konkrétně je nelze toocreate hello protokolovacího nástroje objektu.<p><p>Řešení: Ověřte, že dostatek systémových prostředků jsou k dispozici toolaunch nové procesů. |
| -104 |Nelze tooload hello soubor rcf poskytované hello agenta hosta.<p><p>Tato interní chyba by měla pouze dojít, pokud hello hostovaného agenta modulu plug-in Spouštěče je vyvolána ručně, nesprávně, na hello virtuálních počítačů. |
| -105 |modul plug-in diagnostiky Hello nemůže otevřít soubor konfigurace diagnostiky hello.<p><p>Tato interní chyba by mohlo dojít pouze v Pokud modul plug-in diagnostiky hello je vyvolána ručně, nesprávně, na hello virtuálních počítačů. |
| -106 |Hello diagnostiky konfigurační soubor nelze přečíst.<p><p>Řešení: Způsobené konfigurační soubor není předávání ověřování schématu. Proto je řešení hello tooprovide konfiguračního souboru, který odpovídá schématu hello. V tématu [jak toocheck konfiguraci rozšíření diagnostiky](#how-to-check-diagnostics-extension-configuration). |
| -107 |Hello prostředků directory průchodu toohello agenta monitorování je neplatný.<p><p>Tato interní chyba by měla pouze dojít, pokud hello agenta monitorování je vyvolána ručně, nesprávně, na hello virtuálních počítačů.</p> |
| -108 |Nelze tooconvert hello diagnostiky konfigurační soubor do hello agenta konfiguračním souborem monitorování.<p><p>Tato interní chyba by mohlo dojít pouze v Pokud je modul plug-in diagnostiky hello je vyvolána ručně se souborem neplatná konfigurace. |
| -110 |Konfigurace diagnostiky obecné došlo k chybě.<p><p>Tato interní chyba by mohlo dojít pouze v Pokud je modul plug-in diagnostiky hello je vyvolána ručně se souborem neplatná konfigurace. |
| -111 |Agent monitorování nelze toostart hello.<p><p>Řešení: Ověřte, že jsou k dispozici dostatek systémových prostředků. |
| -112 |Obecná chyba |

### <a name="local-log-extraction"></a>Extrakce místního protokolu
Hello monitorování agent shromažďuje protokoly a artefaktů jako `.tsf` soubory. `.tsf`soubor není možné číst, ale můžete ji do převést `.csv` následujícím způsobem: 

```
<Azure diagnostics extension package>\Monitor\x64\table2csv.exe <relevantLogFile>.tsf
```
Nový soubor s názvem `<relevantLogFile>.csv` budou vytvořeny v hello stejnou cestu jako odpovídající hello `.tsf` souboru.

**Poznámka:**: stačí toorun tento nástroj proti hello hlavní tsf souboru (například PerformanceCountersTable.tsf). Hello doplňujícími soubory (například PerformanceCountersTables_\*\*001.tsf PerformanceCountersTables_\*\*002. tsf atd.) budou automaticky zpracovány.

### <a name="more-about-trace-logs-missing"></a>Další informace o chybí protokoly trasování

**Poznámka:** to většinou platí toocloud služby, pouze pokud jste nakonfigurovali hello DiagnosticsMonitorTraceListener na aplikace běžící na vašem virtuálním počítači IaaS. 

- Ujistěte se, zda text hello, DiagnosticMonitorTraceListener lze nastavit v hello soubor web.config nebo app.config.  To je ve výchozím nastavení nakonfigurována v projekty cloudových služeb, ale někteří zákazníci komentář ho out, což způsobí hello trasování příkazy toonot shromáždit pomocí diagnostiky. 
- Pokud protokoly získávání nezapisují z metody OnStart nebo spustit hello Ujistěte se, zda text hello, DiagnosticMonitorTraceListener je v souboru app.config hello.  Ve výchozím nastavení je v souboru web.config hello, ale které se týkají pouze toocode běžící v rámci w3wp.exe; Proto musíte ho v souboru app.config toocapture trasování spuštění v WaIISHost.exe.
- Ujistěte se, že používáte Diagnostics.Trace.TraceXXX místo Diagnostics.Debug.WriteXXX.  Hello příkazy ladění se odeberou ze sestavení pro vydání.
- Ujistěte se, zda hello zkompilovat kód ve skutečnosti má řádky Diagnostics.Trace hello (použijte Reflector, ildasm nebo ILSpy tooverify).  Příkazy Diagnostics.Trace jsou vyřazeny z hello kompilovány binární, pokud nechcete použít symbol Podmíněná kompilace hello trasování.  Pokud používáte projektu nástroje msbuild toobuild hello to je běžné toorun problém do.

## <a name="known-issues-and-mitigations"></a>Známé problémy a jejich zmírnění
Tady je seznam známých problémů s známé způsoby zmírnění rizik:

**1. rozhraní .NET 4.5 závislost:**

WAD má závislost na modul runtime na rozhraní .NET 4.5 nebo vyšší. V době psaní to hello všechny počítače, které jsou zřízené pro cloudové služby a také všechny oficiální azure základní bitové kopie virtuálních počítačů mít rozhraní .NET 4.5 nebo vyšší nainstalovat. Je ale stále možné tooland v situaci, kde se můžete pokusit toorun WAD na počítači, který nemá .NET 4.5 nebo vyšší. To se stane, když vytvoříte svůj počítač z původní bitové kopie nebo snímek; nebo přineste vlastní disk.

To obvykle manifesty jako ukončovací kód **255** při spuštění DiagnosticsPluginLauncher.exe. Selhání kvůli toohello neošetřená výjimka se stane toto: 
```
System.IO.FileLoadException: Could not load file or assembly 'System.Threading.Tasks, Version=1.5.11.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies
```

**Omezení rizik:** instalace rozhraní .NET 4.5 nebo vyšší na váš počítač.

**2. Data čítače výkonu, které jsou k dispozici v úložiště, ale nezobrazují v portálu**

Virtuální počítače portálu prostředí ukazuje některé čítače výkonu ve výchozím nastavení. Pokud je nevidíte a víte, že hello data je generováno, protože je k dispozici v úložišti. Kontrola:
- Pokud hello dat v úložišti má názvy čítačů v angličtině. Pokud nejsou názvy čítačů hello v angličtině, portálu metriky grafu nebude možné toorecognize ho.
- Pokud používáte zástupné znaky (\*) ve vaší názvy čítačů výkonu hello portálu nebude možné toocorrelate hello nakonfigurované a shromažďují čítač.

**Zmírnění dopadů**: změnit počítač hello tooEnglish jazyk pro systémové účty. Ovládací panely -> oblast -> správy -> Nastavení kopií -> zrušte zaškrtnutí políčka "Úvodní obrazovky a systém účty" tak, aby vlastní jazyk hello není použita toosystem účtu. Ujistěte se, že nepoužíváte zástupné znaky, pokud chcete portál toobe prostředí primární spotřeba také.
