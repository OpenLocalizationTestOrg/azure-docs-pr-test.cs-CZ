---
title: "aaaWire řešení dat v Log Analytics | Microsoft Docs"
description: "Data kabelové sítě je konsolidované sítě a výkon data z počítačů s agenty OMS, včetně nástroje Operations Manager a agenti připojená k systému Windows. Data sítě je v kombinaci s vaší toohelp data protokolu korelovat data."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: fc3d7127-0baa-4772-858a-5ba995d1519b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: banders
ms.openlocfilehash: adafdf98dfbda9d87759643a1a606a84eafd1348
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="wire-data-20-preview-solution-in-log-analytics"></a>Řešení přenosu dat 2.0 (Preview) v analýzy protokolů

![Přenosová datový symbol](./media/log-analytics-wire-data/wire-data2-symbol.png)

Data kabelové sítě je konsolidované sítě a výkon data z počítačů s agenty OMS, včetně nástroje Operations Manager, připojení systému Windows a Linux agenty. Data sítě je v kombinaci s vaší jiných toohelp data protokolu korelovat data.

Kromě toho tooOMS agenti hello řešení přenosu dat používá Microsoft Dependency agentů, které můžete nainstalovat na počítače ve vaší infrastruktuře IT. Závislost agenty monitorování sítě data odeslaná tooand z počítačů pro síť úrovně 2 – 3 v hello [OSI model](https://en.wikipedia.org/wiki/OSI_model), včetně hello různé protokoly a porty používané. Data se pak odešlou tooLog analýzy využití agentů.

> [!NOTE]
> Nelze přidat hello předchozí verze hello Data kabelové sítě řešení toonew pracovních prostorů. Pokud máte hello původní Data kabelové sítě povolené žádné řešení, můžete dál toouse ho. Ale toouse přenosu dat 2.0, je nutné nejprve odebrat hello původní verze.

Ve výchozím nastavení analýzy protokolů shromažďuje data protokolu pro procesoru, paměti, disku a data výkonu sítě z čítačů, které jsou součástí systému Windows. Sítě a jiných shromažďování dat se provádí v v reálném čase pro každého agenta, včetně podsítě a úrovni aplikace protokoly hello počítače. Na stránce nastavení hello na kartě hello protokoly můžete přidat další čítače výkonu.

Pokud jste použili [sFlow](http://www.sflow.org/) nebo jiný software s [společnosti Cisco NetFlow protokol](http://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html), pak hello statistiky a data se zobrazí z data kabelové sítě budou známé tooyou.

Mezi typy hello předdefinované dotazy vyhledávání protokolu patří:

- Agenti poskytující data kabelové sítě
- IP adresy agentů poskytujících data kabelové sítě
- Odchozí komunikace podle IP adresy
- Počet bajtů odeslaných protokoly aplikací
- Počet bajtů odeslaných služba aplikace
- Počet přijatých bajtů podle různé protokoly
- Celkový počet bajtů odeslaných a přijatých podle verze protokolu IP
- Průměrná latence pro připojení, které byly spolehlivě
- Počítač, aby iniciovaly nebo přijaly síťový provoz zpracovává
- Objem síťového přenosu pro zpracování

Při hledání pomocí data kabelové sítě, můžete filtrovat a skupiny dat tooview informace o hello nejvyšší agenty a horní protokoly. Nebo můžete zobrazit, kdy některé počítače (IP adresy MAC adresy) přenášená mezi sebou, jak dlouho a kolik data byla odeslána – v zásadě platí, zobrazit metadata o síťovém provozu, který je na základě hledání.

Ale vzhledem k tomu, že zobrazíte metadata, není nutně užitečné pro odstraňování potíží. Data kabelové sítě v Log Analytics není úplná sběru dat v síti. Ano rozhraní není určeno pro řešení potíží s hloubky úrovni paketů. Dobrý den výhodou použití hello agenta, porovnání metod kolekcí tooother, je, že nebudete mít tooinstall zařízení, konfigurovat síťové přepínače nebo preform složitá konfigurace. Data kabelové sítě je jednoduše založené na agentovi – nainstalujte agenta hello v počítači a jeho bude monitorování vlastní síťového provozu. Další výhodou je, když chcete, aby toomonitor úlohy běžící v cloudu poskytovatelů nebo poskytovatel hostitelských služeb nebo Microsoft Azure, kde uživatel hello není vlastníkem vrstvy hello prostředků infrastruktury.

## <a name="connected-sources"></a>Připojené zdroje

Data kabelové sítě získá data z hello Agent služby Dependency společnosti Microsoft. Hello Agent služby Dependency závisí na hello agenta OMS pro jeho připojení tooLog Analytics. To znamená, že server musí mít hello agenta OMS nainstalovaný a nakonfigurovaný první, a pak nainstalujte hello Agent služby Dependency. Hello následující tabulka popisuje hello připojené zdroje, které podporuje hello Data kabelové sítě řešení.

| **Připojené zdroje** | **Podporuje se** | **Popis** |
| --- | --- | --- |
| Agenti systému Windows | Ano | Data kabelové sítě analyzuje a shromažďuje data z počítače se systémem Windows agenta. <br><br> V přidání toohello [agenta OMS](log-analytics-windows-agents.md), vyžadují hello Agent služby Microsoft Dependency agentů v systému Windows. V tématu hello [podporované operační systémy](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) úplný seznam verzí operačního systému. |
| Agenti systému Linux | Ano | Data kabelové sítě analyzuje a shromažďuje data z počítače se systémem Linux agent.<br><br> V přidání toohello [agenta OMS](log-analytics-linux-agents.md), agenty Linux vyžadují hello Agent služby Dependency společnosti Microsoft. V tématu hello [podporované operační systémy](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems) úplný seznam verzí operačního systému. |
| Skupina pro správu nástroje System Center Operations Manager | Ano | Analyzuje Data kabelové sítě a shromažďuje data z agentů systému Windows a Linux v připojeného [skupiny pro správu System Center Operations Manager](log-analytics-om-agents.md). <br><br> Přímé připojení z tooLog počítače agenta System Center Operations Manager hello Analytics je požadovaná. Z hello správy skupiny tooLog Analytics se předají data. |
| Účet služby Azure Storage | Ne | Data kabelové sítě shromažďuje data z počítačů agentů, takže není žádná data z něj toocollect ze služby Azure Storage. |

V systému Windows hello Microsoft Monitoring Agent (MMA) je používána toogather System Center Operations Manager a analýzy protokolů a odesílat data. V závislosti na kontextu hello se nazývá hello agenta hello agenta System Center Operations Manager, OMS Agent, Agent analýzy protokolů, MMA nebo přímé agenta. System Center Operations Manager a analýzy protokolů poskytují mírně různých verzích hello MMA. Tyto verze lze každou zprávu tooSystem Center Operations Manager, tooLog analýzy nebo tooboth.

V systému Linux hello OMS agenta pro Linux shromažďuje a odesílá data tooLog Analytics. Data kabelové sítě můžete použít na serverech s agenty přímé OMS nebo na servery, které jsou připojené tooLog analýzy prostřednictvím skupin pro správu System Center Operations Manager.

V tomto článku odkazuje tooall agentů, jestli Linux nebo Windows, zda tooa připojené skupiny pro správu System Center Operations Manager, nebo přímo tooLog Analytics se říká hello _agenta OMS_. Název konkrétní nasazení hello hello agenta použijeme jenom v případě, že je potřeba pro kontext.

Hello Agent služby Dependency nepřenáší samotná data a nevyžaduje žádné změny toofirewalls nebo porty. Hello dat v Data kabelové sítě je vždy přenášených v rámci hello OMS agenta tooLog analýzy, buď přímo nebo pomocí hello OMS brány.

![diagram agenta](./media/log-analytics-wire-data/agents.png)

Pokud jste uživatele System Center Operations Manager s tooLog připojené skupiny správy Analytics:

- V případě agenty nástroje System Center Operations Manager můžete přístup hello Internet tooconnect tooLog analýzy, není nutná žádná další konfigurace.
- Musíte tooconfigure hello OMS brány toowork nástrojem System Center Operations Manager, když agenty nástroje System Center Operations Manager nelze přistoupit k analýze protokolů přes hello Internet.

Pokud používáte hello přímé agenta, je třeba tooconfigure hello OMS vlastní agent tooconnect tooLog analýzy nebo tooyour OMS brány. Hello OMS brány si můžete stáhnout z hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=52666).

## <a name="prerequisites"></a>Požadavky

- Vyžaduje hello [přehledy a analýzy](https://www.microsoft.com/cloud-platform/operations-management-suite-pricing) nabídka řešení.
- Pokud používáte předchozí verzi hello hello Data kabelové sítě řešení, je nutné ji odebrat. Všechna data zaznamenaná prostřednictvím hello původní Data kabelové sítě řešení je ale stále k dispozici v přenosu dat 2.0 a hledání protokolů.
- Oprávnění správce se vyžaduje tooinstall nebo odinstalovat hello Agent služby Dependency.
- Hello Agent služby Dependency musí být nainstalován na počítači s 64bitový operační systém.

### <a name="operating-systems"></a>Operační systémy

Hello následující části uvádějí hello podporované operační systémy pro hello Agent služby Dependency. Data kabelové sítě nepodporuje 32bitové architektury pro všechny operační systémy.

#### <a name="windows-server"></a>Windows Server

- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1

#### <a name="windows-desktop"></a>Plocha Windows

- Windows 10
- Windows 8.1
- Windows 8
- Windows 7

#### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a>Red Hat Enterprise Linux, CentOS Linux a Oracle Linux (s RHEL jádra)

- Podporovány jsou pouze výchozí a verze SMP Linux jádra.
- Nestandardní jádra uvolní, například PAE a Xen, nejsou podporovány pro všechny distribuci systému Linux. Například systém s řetězec verze hello _2.6.16.21-0.8-xen_ není podporován.
- Vlastní jádra, včetně opakovaných kompilací standardní jádra, nejsou podporovány.
- CentOSPlus jádra není podporována.
- Oracle nedělitelné Enterprise jádra (UEK) je popsaná v další části tohoto článku.

#### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| **Verze operačního systému** | **Verze jádra** |
| --- | --- |
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |
| 7.3 | 3.10.0-514 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| **Verze operačního systému** | **Verze jádra** |
| --- | --- |
| 6.0 | 2.6.32-71 |
| 6.1 | 2.6.32-131 |
| 6.2 | 2.6.32-220 |
| 6.3 | 2.6.32-279 |
| 6.4 | 2.6.32-358 |
| 6.5 | 2.6.32-431 |
| 6.6 | 2.6.32-504 |
| 6.7 | 2.6.32-573 |
| 6.8 | 2.6.32-642 |

#### <a name="red-hat-linux-5"></a>Red Hat Linux 5

| **Verze operačního systému** | **Verze jádra** |
| --- | --- |
| 5.8 | 2.6.18-308 |
| 5.9 | 2.6.18-348 |
| 5.10 | 2.6.18-371 |
| 5.11 | 2.6.18-398 <br> 2.6.18-400 <br>2.6.18-402 <br>2.6.18-404 <br>2.6.18-406 <br> 2.6.18-407 <br> 2.6.18-408 <br> 2.6.18-409 <br> 2.6.18-410 <br> 2.6.18-411 <br> 2.6.18-412 <br> 2.6.18-416 <br> 2.6.18-417 <br> 2.6.18-419 |

#### <a name="oracle-enterprise-linux-with-unbreakable-enterprise-kernel"></a>Oracle Linux Enterprise s nedělitelné Enterprise jádra

#### <a name="oracle-linux-6"></a>Oracle Linux 6

| **Verze operačního systému** | **Verze jádra** |
| --- | --- |
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4 | Oracle 2.6.39-400 (UEK R2) |
| 6.5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-linux-5"></a>Oracle Linux 5

| **Verze operačního systému** | **Verze jádra** |
| --- | --- |
| 5.8 | Oracle 2.6.32-300 (UEK R1) |
| 5.9 | Oracle 2.6.39-300 (UEK R2) |
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server

#### <a name="suse-linux-11"></a>SUSE Linux 11

| **Verze operačního systému** | **Verze jádra** |
| --- | --- |
| 11 | 2.6.27 |
| 11 SP1 | 2.6.32 |
| 11 SP2 | 3.0.13 |
| 11 SP3 | 3.0.76 |
| 11 SP4 | 3.0.101 |

#### <a name="suse-linux-10"></a>SUSE Linux 10

| **Verze operačního systému** | **Verze jádra** |
| --- | --- |
| 10 SP4 | 2.6.16.60 |

#### <a name="dependency-agent-downloads"></a>Agent služby Dependency soubory ke stažení

| **File** | **OPERAČNÍ SYSTÉM** | **Verze** | **ALGORITMUS SHA-256** |
| --- | --- | --- | --- |
| [InstallDependencyAgent Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.0.5 | 73B3F6A2A76A08D58F72A550947FF839B588591C48E6EDDD6DDF73AA3FD82B43 |
| [InstallDependencyAgent Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.0.5 | A1BAD0B36EBF79F2B69113A07FCF48C68D90BD169C722689F9C83C69FC032371 |



## <a name="configuration"></a>Konfigurace

Proveďte následující kroky tooconfigure hello Data kabelové sítě řešení pro vaše pracovní prostory hello.

1. Povolit hello analýzy protokolů aktivity řešení z hello [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WireData2OMS?tab=Overview) nebo pomocí hello procesu popsaného v tématu [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md).
2. Nainstalujte hello Agent služby Dependency na každém počítači, kde chcete tooget data. Hello Agent služby Dependency můžete sledovat okolí tooimmediate připojení, takže nemusí potřebovat agenta, v každém počítači.

### <a name="install-hello-dependency-agent-on-windows"></a>Nainstalujte hello Agent služby Dependency na systému Windows

Oprávnění správce se vyžaduje tooinstall nebo odinstalujte agenta hello.

Hello Agent služby Dependency nainstalovaný na počítačích se systémem Windows prostřednictvím InstallDependencyAgent Windows.exe. Pokud spustíte tento spustitelný soubor, bez jakékoli možnosti, spustí průvodce, můžete postupovat podle tooinstall interaktivně.

Použijte následující postup tooinstall hello Agent služby Dependency na každém počítači se systémem Windows hello:

1. Instalace agenta OMS hello pomocí pokynů hello [toohello počítače připojit Windows analýzy protokolů služby ve službě Azure](log-analytics-windows-agents.md).
2. Stáhnout agenta pro Windows hello pomocí hello odkaz v předchozí části hello a spusťte jej pomocí hello následující příkaz: InstallDependencyAgent Windows.exe
3. Postupujte podle hello Průvodce tooinstall hello agenta.
4. V případě selhání toostart hello Agent služby Dependency protokolech hello podrobné informace o chybě. Na agentech Windows hello adresář protokolu je %Programfiles%\Microsoft Agent\logs závislostí.

#### <a name="windows-command-line"></a>Příkazový řádek systému Windows

Použijte možnosti hello následující tabulky tooinstall z příkazového řádku. toosee seznam hello instalace příznaky, spusťte instalační program hello pomocí hello /? Příznak následujícím způsobem.

InstallDependencyAgent Windows.exe /?

| **Příznak** | **Popis** |
| --- | --- |
| <code>/?</code> | Získejte seznam možností příkazového řádku hello. |
| <code>/S</code> | Proveďte bezobslužnou instalaci s žádné uživatelské výzvy. |

Soubory pro hello Agent služby Dependency Windows jsou umístěny v Agent služby Dependency C:\Program Files\Microsoft ve výchozím nastavení.

### <a name="install-hello-dependency-agent-on-linux"></a>Instalace hello Agent služby Dependency na platformě Linux

Kořenový přístup je požadovaná tooinstall nebo konfiguraci agenta hello.

Hello Agent služby Dependency nainstalovaný na počítače se systémem Linux prostřednictvím InstallDependencyAgent-Linux64.bin, skript prostředí s samorozbalující binární. Soubor hello můžete spustit pomocí _dílet_ nebo přidejte provést samotný soubor toohello oprávnění.

Použijte následující postup tooinstall hello Agent služby Dependency na každý počítač se systémem Linux hello:

1. Instalace agenta OMS hello pomocí pokynů hello [shromažďování a správě dat z počítače se systémem Linux](log-analytics-agent-linux.md).
2. Agent služby Linux Dependency hello pomocí hello odkaz v předchozí části hello stáhněte a nainstalujte ji jako kořenová pomocí hello následující příkaz: dílet InstallDependencyAgent Linux64.bin
3. V případě selhání toostart hello Agent služby Dependency protokolech hello podrobné informace o chybě. Na agentech Linux adresář protokolu hello je: /var/opt/microsoft/dependency-agent/log.

toosee seznam hello instalace příznaky, spustit instalační program hello s hello `-help` příznak následujícím způsobem.

```
InstallDependencyAgent-Linux64.bin -help
```

| **Příznak** | **Popis** |
| --- | --- |
| <code>-help</code> | Získejte seznam možností příkazového řádku hello. |
| <code>-s</code> | Proveďte bezobslužnou instalaci s žádné uživatelské výzvy. |
| <code>--check</code> | Zkontrolujte oprávnění a hello operačního systému, ale není nainstalovaný hello agent. |

Soubory pro hello Agent služby Dependency jsou umístěny v hello následující adresáře:

| **Soubory** | **Umístění** |
| --- | --- |
| Soubory jádra | /OPT/Microsoft/Dependency-Agent |
| Soubory protokolu | /var/OPT/Microsoft/Dependency-Agent/log |
| Konfigurační soubory | /ETC/OPT/Microsoft/Dependency-Agent/config |
| Služby spustitelné soubory | /OPT/Microsoft/Dependency-Agent/Bin/Microsoft-Dependency-Agent<br><br>/OPT/Microsoft/Dependency-Agent/Bin/Microsoft-Dependency-Agent-Manager |
| Úložiště binární soubory | /var/OPT/Microsoft/Dependency-Agent/Storage |

### <a name="installation-script-examples"></a>Příklady skriptů instalace

tooeasily nasadit hello Agent služby Dependency na mnoha serverech najednou, pomáhá toouse skriptu. Můžete použít následující skript příklady toodownload hello a nainstalovat hello Agent služby Dependency na systému Windows nebo Linux.

#### <a name="powershell-script-for-windows"></a>Skript prostředí PowerShell pro systém Windows

```PowerShell

Invoke-WebRequest &quot;https://aka.ms/dependencyagentwindows&quot; -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S

```

#### <a name="shell-script-for-linux"></a>Skript prostředí pro Linux

```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
```

```
sh InstallDependencyAgent-Linux64.bin -s
```

### <a name="desired-state-configuration"></a>Konfigurace požadovaného stavu

toodeploy hello Agent služby Dependency prostřednictvím konfigurace požadovaného stavu, můžete použít modul xPSDesiredStateConfiguration hello a bit kódu jako hello následující:

```
Import-DscResource -ModuleName xPSDesiredStateConfiguration

$DAPackageLocalPath = &quot;C:\InstallDependencyAgent-Windows.exe&quot;



Node $NodeName

{

    # Download and install hello Dependency Agent

    xRemoteFile DAPackage

    {

        Uri = &quot;https://aka.ms/dependencyagentwindows&quot;

        DestinationPath = $DAPackageLocalPath

        DependsOn = &quot;[Package]OI&quot;

    }

    xPackage DA

    {

        Ensure=&quot;Present&quot;

        Name = &quot;Dependency Agent&quot;

        Path = $DAPackageLocalPath

        Arguments = '/S'

        ProductId = &quot;&quot;

        InstalledCheckRegKey = &quot;HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\DependencyAgent&quot;

        InstalledCheckRegValueName = &quot;DisplayName&quot;

        InstalledCheckRegValueData = &quot;Dependency Agent&quot;

    }

}

```
### <a name="uninstall-hello-dependency-agent"></a>Odinstalujte hello Agent služby Dependency

Použijte následující oddíly toohelp odebrat hello Agent služby Dependency hello.

#### <a name="uninstall-hello-dependency-agent-on-windows"></a>Odinstalujte hello Agent služby Dependency v systému Windows

Správce můžete odinstalovat hello závislostí agenta pro Windows pomocí ovládacích panelů.

Správce můžete také spouštět %Programfiles%\Microsoft závislostí Agent\Uninstall.exe toouninstall hello Agent služby Dependency.

#### <a name="uninstall-hello-dependency-agent-on-linux"></a>Odinstalujte hello Agent služby Dependency na systému Linux

Odinstalace toocompletely hello Agent služby Dependency ze systému Linux, je nutné odstranit vlastní agent hello a hello konektor, který se instaluje automaticky s agentem hello. Můžete odinstalovat i pomocí hello následující jeden příkaz:

```
rpm -e dependency-agent dependency-agent-connector
```

## <a name="management-packs"></a>Sady Management Pack

Po aktivaci Data kabelové sítě v pracovním prostoru analýzy protokolů 300 KB management pack se odesílají servery Windows hello tooall v něm. Pokud používáte System Center Operations Manager agentů v [připojené skupiny pro správu](log-analytics-om-agents.md), z System Center Operations Manager je nasazena hello sady management pack monitorování závislostí. Pokud jsou připojeny přímo hello agentů, analýzy protokolů přináší hello sady management pack.

Sada management pack Hello jmenuje Microsoft.IntelligencePacks.ApplicationDependencyMonitor. Je zapsán do: %Programfiles%\Microsoft monitorování Agent\Agent\Health služby State\Management balíčky. Hello zdroj dat, který používá sada management pack hello je: % Program files%\Microsoft monitorování Agent\Agent\Health služby State\Resources&lt;AutoGeneratedID&gt;\ Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.

## <a name="using-hello-solution"></a>Pomocí řešení hello

**Instalace a konfigurace řešení hello**

Použijte následující informace tooinstall hello a nakonfigurujte hello řešení.

- Hello Data kabelové sítě řešení operace čtení dat z počítačů se systémem Windows Server 2012 R2, Windows 8.1 a novější operační systémy.
- Na počítačích, kam chcete data kabelové sítě tooacquire z se vyžaduje rozhraní Microsoft .NET Framework 4.0 nebo novější.
- Přidat hello Data kabelové sítě řešení tooyour pracovní prostor analýzy protokolů pomocí hello procesu popsaného v tématu [řešení přidat analýzy protokolů z hello řešení Galerie](log-analytics-add-solutions.md). Není nutná žádná další konfigurace.
- Pokud chcete data kabelové sítě tooview pro konkrétní řešení, je nutné řešení hello toohave již přidán tooyour prostoru.

Po mají nainstalovat agenti a instalaci hello řešení, dlaždice hello 2.0 přenosu dat se zobrazí v pracovním prostoru.

> [!NOTE]
> V současné době je nutné použít data kabelové sítě portálu tooview hello OMS. Nelze použít data kabelové sítě Azure portálu tooview hello.

![Dlaždice Data kabelové sítě](./media/log-analytics-wire-data/wire-data-tile.png)

## <a name="using-hello-wire-data-20-solution"></a>Pomocí řešení hello 2.0 přenosu dat

Na portálu OMS hello, klikněte na tlačítko hello **přenosu dat 2.0** řídicí panel dlaždice tooopen hello Data kabelové sítě. řídicí panel Hello zahrnuje hello oken v hello následující tabulka. Každý okno uvádí až too10 položky odpovídající, aby na okno kritéria pro hello zadán oboru a časový rozsah. Můžete spustit vyhledávání protokolu, který vrátí všechny záznamy kliknutím **zobrazit všechny** dole hello v okně hello nebo kliknutím na záhlaví okna hello.

| **Okno** | **Popis** |
| --- | --- |
| Agenti zachytávající síťový přenos | Zobrazuje hello počet agentů, kteří jsou zachytávání síťového provozu a uvádí hello top 10 počítačů, které jsou zachycení provozu. Klikněte na číslo toorun hello protokolu vyhledejte <code>Type:WireData &#124; measure Sum(TotalBytes) by Computer &#124; top 500000</code>. Klikněte na počítač, v seznamu toorun hello protokolu vyhledávání vrací hello celkový počet bajtů zaznamenat. |
| Místní podsítě | Zobrazuje počet hello místní podsítě, které byly zjištěny agenty.  Klikněte na číslo toorun hello protokolu vyhledejte <code>Type:WireData &#124; Measure Sum(TotalBytes) by LocalSubnet</code> , obsahuje seznam všech podsítí s hello počet bajtů odeslaných přes každé z nich. Klikněte na podsíť v seznamu toorun hello protokolu vyhledávání vrací hello celkový počet bajtů odeslaných přes hello podsítě. |
| Protokoly na úrovni aplikace | Zobrazuje hello počet protokoly na úrovni aplikace používána, při zjištění agenty. Klikněte na číslo toorun hello protokolu vyhledejte <code>Type:WireData &#124; Measure Sum(TotalBytes) by ApplicationProtocol</code>. Klikněte na protokol toorun protokolu vyhledávání vrací hello celkový počet bajtů odeslaných pomocí protokolu hello. |

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Řídicí panel přenosu dat](./media/log-analytics-wire-data/wire-data-dash.png)

Můžete použít hello **agenti zachytávající síťový přenos** okno toodetermine, jaký poměr šířky pásma sítě je spotřebovávanou počítače. Toto okno můžete snadno najít hello _chattiest_ počítač ve vašem prostředí. Tyto počítače může být přetížený, funguje neobvyklým způsobem, nebo pomocí více síťových prostředků, než normální.

![Příklad protokolu vyhledávání](./media/log-analytics-wire-data/log-search-example01.png)

Podobně můžete použít hello **místní podsítě** okno toodetermine kolik síťový provoz je procházení podsítě. Uživatelé jsou často definovat podsítě kolem důležité oblasti pro své aplikace. Toto okno nabízí zobrazení do těchto oblastí.

![Příklad protokolu vyhledávání](./media/log-analytics-wire-data/log-search-example02.png)

Hello **protokoly na úrovni aplikace** okno je užitečné, protože je užitečné vědět, co protokoly jsou používány. Například by se dalo očekávat SSH toonot má v použít v prostředí vaší sítě. Zobrazení informací, které jsou k dispozici v okně hello můžete rychle potvrďte nebo disprove vaše očekávání.

![Příklad protokolu vyhledávání](./media/log-analytics-wire-data/log-search-example03.png)

V tomto příkladu může přejít k podrobnostem počítače, které používají SSH a mnoho dalších podrobností komunikace programem SSH podrobnosti toosee.

![Zo výsledky hledání](./media/log-analytics-wire-data/ssh-details.png)

Je také užitečné tooknow Pokud provozu protokolu se zvýšením nebo snížením v čase. Například pokud je zvýšení množství hello data přenesená aplikací, který může být něco, co byste měli vědět, nebo že můžete zjistit pozoruhodné.

## <a name="input-data"></a>Vstupní data

Data kabelové sítě shromažďuje metadata o síťovém provozu pomocí hello agentů, které jste povolili. Každý agent odesílá data o každých 15 sekund.

## <a name="output-data"></a>výstupní data

Záznam s typem _WireData_ se vytvoří pro každý typ vstupní data. WireData záznamy mají vlastnosti zobrazené v následující tabulce hello:

| Vlastnost | Popis |
|---|---|
| Počítač | Název počítače, kde nebyla shromážděna data |
| TimeGenerated | Čas záznamu hello |
| LocalIP | IP adresa místního počítače hello |
| SessionState | Připojení nebo odpojení |
| ReceivedBytes | Počet přijatých bajtů |
| ProtocolName | Název používá protokol sítě hello |
| Parametr IPVersion | Verze protokolu IP |
| Směr | Příchozí nebo odchozí |
| MaliciousIP | IP adresa známé škodlivé zdroje |
| Závažnost | Závažnost možného malwaru |
| RemoteIPCountry | Země hello vzdálené IP adresy |
| ManagementGroupName | Název skupiny pro správu nástroje Operations Manager hello |
| SourceSystem | Zdroj, kde nebyla shromážděna data |
| SessionStartTime | Čas spuštění relace |
| SessionEndTime | Čas ukončení relace |
| LocalSubnet | Podsíť, kde nebyla shromážděna data |
| LocalPortNumber | Číslem místního portu |
| Vzdálená adresa IP | Vzdálené IP adresy používané hello vzdáleného počítače |
| RemotePortNumber | Číslo portu použité podle vzdálené IP adresy hello |
| ID relace | Jednoznačná hodnota, která identifikuje relace komunikace mezi dvě IP adresy |
| SentBytes | Počet bajtů odeslaných |
| TotalBytes | Celkový počet bajtů odeslaných během relace |
| ApplicationProtocol | Typ protokolu sítě používá   |
| ID procesu | ID procesu systému Windows |
| Název_procesu | Cesta a název souboru procesu hello |
| RemoteIPLongitude | Zeměpisná délka IP |
| RemoteIPLatitude | Zeměpisná šířka IP |


## <a name="next-steps"></a>Další kroky

- [V protokolech Hledat](log-analytics-log-searches.md) tooview podrobné záznamy přenosu dat hledání.
