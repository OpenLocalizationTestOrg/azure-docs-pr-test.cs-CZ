---
title: "aaaConfigure mapy služeb v Operations Management Suite | Microsoft Docs"
description: "Mapa služeb je do řešení služby Operations Management Suite, který automaticky zjišťuje součásti aplikace v systému Windows a systémy Linux a mapy hello komunikace mezi službami. Tento článek obsahuje podrobné informace pro nasazení mapy služeb ve vašem prostředí a jejich použití v různých scénářů."
services: operations-management-suite
documentationcenter: 
author: daveirwin1
manager: jwhit
editor: tysonn
ms.assetid: d3d66b45-9874-4aad-9c00-124734944b2e
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/18/2016
ms.author: daseidma;bwren;dairwin
ms.openlocfilehash: 3127f4440f2886370f8ff617c405c6d70a926eb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-service-map-in-operations-management-suite"></a>Konfigurace mapy služeb v Operations Management Suite
Mapa služeb automaticky vyhledá součásti aplikace v systémech Windows a Linux a mapy hello komunikace mezi službami. Můžete ho tooview vaše servery, jako jste je – představit jako vzájemně propojena systémy, které poskytování důležitých služeb. Mapy služeb zobrazí připojení mezi servery, procesy a porty mezi všechny architektura připojení TCP se žádná konfigurace vyžaduje, než instalace agenta.

Tento článek popisuje hello podrobnosti konfigurace agentů mapy služeb a registrace. Informace o používání mapy služeb najdete v tématu [použít hello mapy služeb řešení v Operations Management Suite](operations-management-suite-service-map.md).

## <a name="dependency-agent-downloads"></a>Agent služby Dependency soubory ke stažení
| File | Operační systém | Verze | ALGORITMUS SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.0.5 | 73B3F6A2A76A08D58F72A550947FF839B588591C48E6EDDD6DDF73AA3FD82B43 |
| [InstallDependencyAgent Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.0.5 | A1BAD0B36EBF79F2B69113A07FCF48C68D90BD169C722689F9C83C69FC032371 |


## <a name="connected-sources"></a>Připojené zdroje
Mapa služeb získává data z hello Agent služby Dependency společnosti Microsoft. Hello Agent služby Dependency závisí na hello agenta OMS pro jeho připojení tooOperations Management Suite. To znamená, že server musí mít hello nainstalován a nakonfigurován první a potom hello, může být nainstalován Agent služby Dependency OMS Agent. Hello následující tabulka popisuje hello připojené zdroje, které podporuje hello mapy služeb řešení.

| Připojené zdroje | Podporuje se | Popis |
|:--|:--|:--|
| Agenti systému Windows | Ano | Mapa služeb analyzuje a shromažďuje data z počítače se systémem Windows agenta. <br><br>V přidání toohello [agenta OMS](../log-analytics/log-analytics-windows-agents.md), vyžadují hello Agent služby Microsoft Dependency agentů v systému Windows. V tématu hello [podporované operační systémy](#supported-operating-systems) úplný seznam verzí operačního systému. |
| Agenti systému Linux | Ano | Mapa služeb analyzuje a shromažďuje data z počítače se systémem Linux agent. <br><br>V přidání toohello [agenta OMS](../log-analytics/log-analytics-linux-agents.md), agenty Linux vyžadují hello Agent služby Dependency společnosti Microsoft. V tématu hello [podporované operační systémy](#supported-operating-systems) úplný seznam verzí operačního systému. |
| Skupina pro správu nástroje System Center Operations Manager | Ano | Mapa služeb analyzuje a shromažďuje data z agentů systému Windows a Linux v připojeného [skupiny pro správu System Center Operations Manager](../log-analytics/log-analytics-om-agents.md). <br><br>Přímé připojení z tooOperations počítače agenta System Center Operations Manager hello je Management Suite. Data se předají z hello správy skupiny toohello Operations Management Suite úložiště.|
| Účet služby Azure Storage | Ne | Mapy služeb shromažďuje data z počítačů agentů, takže není žádná data z něj toocollect ze služby Azure Storage. |

Mapa služeb podporuje pouze 64bitové platformy.

V systému Windows hello Microsoft Monitoring Agent (MMA) používá toogather System Center Operations Manager a Operations Management Suite a odeslání dat monitorování. (Tomuto agentovi, se nazývá hello agenta System Center Operations Manager, OMS Agent, Agent analýzy protokolů, MMA nebo přímé agenta, v závislosti na kontextu hello). System Center Operations Manager a Operations Management Suite poskytují různé pole na více systémů hello verzích hello MMA. Tyto verze lze každou zprávu tooSystem Center Operations Manager, tooOperations Management Suite nebo tooboth.  

V systému Linux hello OMS agenta pro Linux shromáždí a odešle monitorování dat tooOperations Management Suite. Mapa služeb můžete použít na serverech s agenty přímé OMS nebo na servery, které jsou připojené tooOperations Management Suite prostřednictvím skupin pro správu System Center Operations Manager.  

V tomto článku budeme označovat tooall agentů – jestli Linux nebo Windows, zda připojené skupiny pro správu System Center Operations Manager tooa nebo přímo tooOperations Management Suite – jako hello "OMS Agent." Název konkrétní nasazení hello hello agenta použijeme jenom v případě, že je potřeba pro kontext.

Mapy služeb agenta Hello nepřenáší samotná data a nevyžaduje žádné změny toofirewalls nebo porty. Hello data v mapy služeb je vždy přenášených v rámci hello agenta OMS tooOperations Management Suite, buď přímo nebo prostřednictvím hello OMS brány.

![Agenti mapy služeb](media/oms-service-map/agents.png)

Pokud jste zákazník s System Center Operations Manager s tooOperations připojené skupiny správy Management Suite:

- Pokud agenty nástroje System Center Operations Manager můžete získat přístup k hello Internet tooconnect tooOperations Management Suite, žádná další konfigurace se nevyžaduje.  
- Pokud agenty nástroje System Center Operations Manager nemůže získat přístup k Operations Management Suite přes hello Internet, je třeba tooconfigure hello OMS brány toowork nástrojem System Center Operations Manager.
  
Pokud používáte hello přímé agenta OMS, musíte tooconfigure hello samotného agenta OMS tooconnect tooOperations Management Suite nebo tooyour OMS brány. Hello OMS brány lze stáhnout z hello [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=52666).

### <a name="management-packs"></a>Sady Management Pack
Po aktivaci mapy služeb v pracovním prostoru služby Operations Management Suite 300 KB management pack se odesílají servery Windows hello tooall v něm. Pokud používáte System Center Operations Manager agentů v [připojené skupiny pro správu](../log-analytics/log-analytics-om-agents.md), hello mapy služeb sady management pack je nasazen z nástroje System Center Operations Manager. Pokud jsou připojeny přímo hello agentů, přináší Operations Management Suite hello sady management pack.

Sada management pack Hello jmenuje Microsoft.IntelligencePacks.ApplicationDependencyMonitor. Jeho napsané too%Programfiles%\Microsoft Packs\ State\Management služby Agent\Agent\Health monitorování. Hello zdroj dat, který používá sada management pack hello je % Program files%\Microsoft monitorování Agent\Agent\Health služby State\Resources\<AutoGeneratedID > \ Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.

## <a name="installation"></a>Instalace
### <a name="install-hello-dependency-agent-on-microsoft-windows"></a>Nainstalujte hello Agent služby Dependency na Microsoft Windows
Oprávnění správce se vyžaduje tooinstall nebo odinstalujte agenta hello.

Hello Agent služby Dependency nainstalovaný na počítačích s Windows pomocí InstallDependencyAgent Windows.exe. Pokud spustíte tento spustitelný soubor, bez jakékoli možnosti, spustí průvodce, můžete postupovat podle tooinstall interaktivně.  

Použijte následující postup tooinstall hello Agent služby Dependency na každém počítači s Windows hello:

1.  Instalace agenta OMS hello pomocí pokynů hello [toohello počítače připojit Windows analýzy protokolů služby ve službě Azure](../log-analytics/log-analytics-windows-agents.md).
2.  Stáhnout agenta pro Windows hello a spusťte ji pomocí hello následující příkaz: <br>`InstallDependencyAgent-Windows.exe`
3.  Postupujte podle hello Průvodce tooinstall hello agenta.
4.  V případě selhání toostart hello Agent služby Dependency protokolech hello podrobné informace o chybě. Na agentech Windows hello adresář protokolu je %Programfiles%\Microsoft Agent\logs závislostí. 

#### <a name="windows-command-line"></a>Příkazový řádek systému Windows
Použijte možnosti hello následující tabulky tooinstall z příkazového řádku. toosee seznam hello instalace příznaky, spusťte instalační program hello pomocí hello /? Příznak následujícím způsobem.

    InstallDependencyAgent-Windows.exe /?

| Příznak | Popis |
|:--|:--|
| /? | Získejte seznam možností příkazového řádku hello. |
| /S | Proveďte bezobslužnou instalaci s žádné uživatelské výzvy. |

Soubory pro hello Agent služby Dependency Windows jsou umístěny v Agent služby Dependency C:\Program Files\Microsoft ve výchozím nastavení.

### <a name="install-hello-dependency-agent-on-linux"></a>Instalace hello Agent služby Dependency na platformě Linux
Kořenový přístup je požadovaná tooinstall nebo konfiguraci agenta hello.

Hello Agent služby Dependency nainstalovaný na počítače se systémem Linux prostřednictvím InstallDependencyAgent-Linux64.bin, skript prostředí s samorozbalující binární. Můžete spustit soubor hello pomocí dílet nebo přidat provést samotný soubor toohello oprávnění.
 
Použijte následující postup tooinstall hello Agent služby Dependency na každý počítač se systémem Linux hello:

1.  Instalace agenta OMS hello pomocí pokynů hello [shromažďování a správě dat z počítače se systémem Linux](https://technet.microsoft.com/library/mt622052.aspx).
2.  Nainstalujte agenta hello závislostí Linux jako kořenová pomocí hello následující příkaz:<br>`sh InstallDependencyAgent-Linux64.bin`
3.  V případě selhání toostart hello Agent služby Dependency protokolech hello podrobné informace o chybě. Na agentech Linux adresář protokolu hello je /var/opt/microsoft/dependency-agent/log.

toosee seznam hello instalace příznaky, spustit instalační program hello s hello - help příznak následujícím způsobem.

    InstallDependencyAgent-Linux64.bin -help

| Příznak | Popis |
|:--|:--|
| – Nápověda | Získejte seznam možností příkazového řádku hello. |
| -s | Proveďte bezobslužnou instalaci s žádné uživatelské výzvy. |
| – Zkontrolujte | Zkontrolujte oprávnění a hello operačního systému, ale není nainstalovaný hello agent. |

Soubory pro hello Agent služby Dependency jsou umístěny v hello následující adresáře:

| Soubory | Umístění |
|:--|:--|
| Soubory jádra | /OPT/Microsoft/Dependency-Agent |
| Soubory protokolu | /var/OPT/Microsoft/Dependency-Agent/log |
| Konfigurační soubory | /ETC/OPT/Microsoft/Dependency-Agent/config |
| Služby spustitelné soubory | /OPT/Microsoft/Dependency-Agent/Bin/Microsoft-Dependency-Agent<br>/OPT/Microsoft/Dependency-Agent/Bin/Microsoft-Dependency-Agent-Manager |
| Úložiště binární soubory | /var/OPT/Microsoft/Dependency-Agent/Storage |

## <a name="installation-script-examples"></a>Příklady skriptů instalace
tooeasily nasadit hello Agent služby Dependency na mnoha serverech najednou, pomáhá toouse skriptu. Můžete použít následující skript příklady toodownload hello a nainstalovat hello Agent služby Dependency na systému Windows nebo Linux.

### <a name="powershell-script-for-windows"></a>Skript prostředí PowerShell pro systém Windows
```PowerShell
Invoke-WebRequest "https://aka.ms/dependencyagentwindows" -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S
```

### <a name="shell-script-for-linux"></a>Skript prostředí pro Linux
```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
sh InstallDependencyAgent-Linux64.bin -s
```

## <a name="desired-state-configuration"></a>Konfigurace požadovaného stavu
toodeploy hello Agent služby Dependency prostřednictvím konfigurace požadovaného stavu, můžete použít modul xPSDesiredStateConfiguration hello a bit kódu jako hello následující:
```
configuration ServiceMap {

Import-DscResource -ModuleName xPSDesiredStateConfiguration

$DAPackageLocalPath = "C:\InstallDependencyAgent-Windows.exe"

Node localhost
{ 
    # Download and install hello Dependency Agent
    xRemoteFile DAPackage 
    {
        Uri = "https://aka.ms/dependencyagentwindows"
        DestinationPath = $DAPackageLocalPath
    }

    xPackage DA
    {
        Ensure="Present"
        Name = "Dependency Agent"
        Path = $DAPackageLocalPath
        Arguments = '/S'
        ProductId = ""
        InstalledCheckRegKey = "HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\DependencyAgent"
        InstalledCheckRegValueName = "DisplayName"
        InstalledCheckRegValueData = "Dependency Agent"
        DependsOn = "[xRemoteFile]DAPackage"
    }
  }
}
```

## <a name="uninstallation"></a>Odinstalace
### <a name="uninstall-hello-dependency-agent-on-windows"></a>Odinstalujte hello Agent služby Dependency v systému Windows
Správce můžete odinstalovat hello závislostí agenta pro Windows pomocí ovládacích panelů.

Správce můžete také spouštět %Programfiles%\Microsoft závislostí Agent\Uninstall.exe toouninstall hello Agent služby Dependency.

### <a name="uninstall-hello-dependency-agent-on-linux"></a>Odinstalujte hello Agent služby Dependency na systému Linux
Odinstalace toocompletely hello Agent služby Dependency ze systému Linux, je nutné odstranit vlastní agent hello a hello konektor, který se instaluje automaticky s agentem hello. Můžete odinstalovat i pomocí hello následující jeden příkaz:

    rpm -e dependency-agent dependency-agent-connector

## <a name="troubleshooting"></a>Řešení potíží
Pokud máte potíže s instalaci nebo spuštění mapy služeb, v této části vám může pomoct. Pokud stále nemůžete vyřešit problém, kontaktujte prosím Microsoft Support.

### <a name="dependency-agent-installation-problems"></a>Problémy instalace agenta závislostí
#### <a name="installer-asks-for-a-reboot"></a>Instalační program požádá o restartování
Hello Agent služby Dependency *obecně* nevyžaduje restartování instalace nebo odinstalace. V některých výjimečných případech, vyžaduje Windows Server toocontinue restartování s instalací. To se stane, když závislost, obvykle hello Microsoft Visual C++ Redistributable, vyžaduje restartování počítače z důvodu uzamčení souborů.

#### <a name="message-unable-tooinstall-dependency-agent-visual-studio-runtime-libraries-failed-tooinstall-code--codenumber-appears"></a>Zpráva "nelze tooinstall Agent služby Dependency: knihovny modulu Runtime Visual Studio se nezdařilo tooinstall (kód = [číslo_účtu])" se zobrazí

Agent služby Microsoft Dependency Hello je založen na knihovny Microsoft Visual Studio runtime hello. Pokud dojde k problému při instalaci hello knihoven, zobrazí se zpráva s. 

Instalační programy knihovna modulu runtime Hello vytvořit protokoly ve složce %LOCALAPPDATA%\temp hello. soubor Hello je dd_vcredist_arch_yyyymmddhhmmss.log, kde *architektura* "x86" nebo "amd64" a *rrrrmmddhhmmss* je hello datum a čas (24hodinovém formátu), kdy byla vytvořena hello protokolu. Hello protokolu poskytuje podrobnosti o problému hello, která blokuje instalaci.

Může to být užitečné tooinstall hello [nejnovější modulu runtime knihoven](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) sami první.

Hello následující tabulka uvádí čísla kódu a návrhy řešení.

| Kód | Popis | Řešení |
|:--|:--|:--|
| 0x17 | Instalační program Hello knihovně vyžaduje služby Windows update, který nebyl nainstalován. | Vyhledejte v protokolu hello nejnovější knihovny Instalační služby.<br><br>Pokud odkaz příliš "Windows8.1-KB2999226-x64.msu" následuje řádek "Chyba 0x80240017: selhání tooexecute MSU balíčku," nemáte hello požadavky tooinstall KB2999226. Postupujte podle pokynů hello v části předpoklady hello v [Universal C Runtime v systému Windows](https://support.microsoft.com/kb/2999226). Můžete třeba toorun služby Windows Update a restartovat víckrát v pořadí tooinstall hello požadavky.<br><br>Agent služby Microsoft Dependency hello instalační program spusťte znovu. |

### <a name="post-installation-issues"></a>Po instalaci problémy
#### <a name="server-doesnt-appear-in-service-map"></a>Server se nezobrazí v mapy služeb
Pokud vaše Agent služby Dependency instalace proběhla úspěšně, ale nevidíte serveru v hello řešení mapy služeb:
* Hello Agent služby Dependency úspěšné instalaci? Můžete to ověřit pomocí kontrola toosee, pokud je služba hello nainstalovaná a spuštěná.<br><br>
**Windows**: Vyhledejte hello služby s názvem "Agent služby Microsoft Dependency."<br>
**Linux**: Vyhledejte hello spouštění procesu "microsoft závislost agent."

* Jste si na hello [volné cenová úroveň Operations Management Suite/Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers)? plánu úrovně Free hello umožňuje až toofive jedinečných serverů mapy služeb. Všechny následující servery nezobrazí v mapě služby i v případě, že hello předchozí pět už odesílají data.

* Je odesílání protokolů serveru a výkonu dat tooOperations Management Suite? Přejděte tooLog vyhledávání a spusťte následující dotaz pro tento počítač hello: 

        * Computer="<your computer name here>" | measure count() by Type
        
  Obdrželi jste celou řadu událostí ve výsledcích hello? Je poslední hello dat? Pokud ano, je agenta OMS fungování a komunikaci s hello služby Operations Management Suite. Pokud ne, vyhledejte hello agenta OMS na serveru: [řešení potíží s agentem OMS pro systém Windows](https://support.microsoft.com/help/3126513/how-to-troubleshoot-operations-management-suite-onboarding-issues) nebo [agenta OMS pro řešení potíží s Linux](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md).

#### <a name="server-appears-in-service-map-but-has-no-processes"></a>Server se zobrazí v mapy služeb, ale nemá žádné procesy
Pokud se zobrazí váš server v mapy služeb, ale nemá žádné zpracovat ani připojení data, která určuje, že hello Agent služby Dependency je nainstalovaná a spuštěná, ale nebyla načíst ovladač jádra hello. 

Zkontrolujte soubor C:\Program Files\Microsoft závislostí Agent\logs\wrapper.log hello (Windows) nebo soubor /var/opt/microsoft/dependency-agent/log/service.log (Linux). poslední řádky Hello hello souboru by měl být uveden, proč nebylo načíst hello jádra. Například hello jádra nemusí být podporováno v systému Linux, je-li aktualizovat vaše jádra.

## <a name="data-collection"></a>Shromažďování dat
Každý agent tootransmit můžete očekávat přibližně 25 MB za den, v závislosti na tom, jak komplexní jsou závislosti vašeho systému. Každý agent odesílá data závislostí mapy služeb každých 15 sekund.  

Hello Agent služby Dependency obvykle spotřebuje 0,1 systémové paměti a procentem 0,1 systému procesoru.

## <a name="supported-azure-regions"></a>Podporované oblasti Azure
Mapa služeb je nyní k dispozici v následujících oblastech Azure hello:
- Východ USA
- Západní Evropa
- Západní střed USA


## <a name="supported-operating-systems"></a>Podporované operační systémy
Hello následující části uvádějí hello podporované operační systémy pro hello Agent služby Dependency. Mapa služeb nepodporuje 32bitové architektury pro všechny operační systémy.

### <a name="windows-server"></a>Windows Server
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1

### <a name="windows-desktop"></a>Plocha Windows
- Windows 10
- Windows 8.1
- Windows 8
- Windows 7

### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a>Red Hat Enterprise Linux, CentOS Linux a Oracle Linux (s RHEL jádra)
- Podporovány jsou pouze výchozí a verze SMP Linux jádra.
- Nestandardní jádra uvolní, například PAE a Xen, nejsou podporovány pro všechny distribuci systému Linux. Například systém s hello verze řetězec "2.6.16.21-0.8-xen" nepodporuje.
- Vlastní jádra, včetně opakovaných kompilací standardní jádra, nejsou podporovány.
- CentOSPlus jádra není podporována.
- Oracle nedělitelné Enterprise jádra (UEK) je popsaná v další části tohoto článku.


#### <a name="red-hat-linux-7"></a>Red Hat Linux 7
| Verze operačního systému | Verze jádra |
|:--|:--|
| 7.0 | 3.10.0-123 |
| 7.1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |
| 7.3 | 3.10.0-514 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6
| Verze operačního systému | Verze jádra |
|:--|:--|
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
| Verze operačního systému | Verze jádra |
|:--|:--|
| 5.8 | 2.6.18-308 |
| 5.9 | 2.6.18-348 |
| 5.10 | 2.6.18-371 |
| 5.11 | 2.6.18-398<br>2.6.18-400<br>2.6.18-402<br>2.6.18-404<br>2.6.18-406<br>2.6.18-407<br>2.6.18-408<br>2.6.18-409<br>2.6.18-410<br>2.6.18-411<br>2.6.18-412<br>2.6.18-416<br>2.6.18-417<br>2.6.18-419 |

#### <a name="oracle-enterprise-linux-with-unbreakable-enterprise-kernel"></a>Oracle Linux Enterprise s nedělitelné Enterprise jádra

#### <a name="oracle-linux-6"></a>Oracle Linux 6
| Verze operačního systému | Verze jádra
|:--|:--|
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4 | Oracle 2.6.39-400 (UEK R2) |
| 6.5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-linux-5"></a>Oracle Linux 5

| Verze operačního systému | Verze jádra
|:--|:--|
| 5.8 | Oracle 2.6.32-300 (UEK R1) |
| 5.9 | Oracle 2.6.39-300 (UEK R2) |
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server

#### <a name="suse-linux-11"></a>SUSE Linux 11
| Verze operačního systému | Verze jádra
|:--|:--|
| 11 | 2.6.27 |
| 11 SP1 | 2.6.32 |
| 11 SP2 | 3.0.13 |
| 11 SP3 | 3.0.76 |
| 11 SP4 | 3.0.101 |

#### <a name="suse-linux-10"></a>SUSE Linux 10
| Verze operačního systému | Verze jádra
|:--|:--|
| 10 SP4 | 2.6.16.60 |

## <a name="diagnostic-and-usage-data"></a>data o využití a Diagnostika
Microsoft automaticky shromažďuje data o využití a výkonu prostřednictvím vašeho používání hello služby mapy služeb. Společnost Microsoft používá tato data tooprovide a zlepšit hello kvality, zabezpečení a integrity hello služby mapy služeb. Data zahrnují informace o konfiguraci vašeho softwaru, jako je verze operačního systému a hello. Zahrnuje taky IP adresu, název DNS a název pracovní stanice v pořadí tooprovide přesná a efektivní možnosti pro odstraňování potíží. Neshromažďujeme jména, adresy ani jiné kontaktní informace.

Další informace o shromažďování a používání dat najdete v tématu hello [prohlášení o ochraně osobních údajů služeb Microsoft Online](https://go.microsoft.com/fwlink/?LinkId=512132).



## <a name="next-steps"></a>Další kroky
- Zjistěte, jak příliš[pomocí mapy služeb](operations-management-suite-service-map.md) po byla nasazena a nakonfigurována.
