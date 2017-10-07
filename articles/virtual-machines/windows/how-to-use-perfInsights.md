---
title: aaaHow toouse PerfInsights v Microsoft Azure | Microsoft Docs
description: "Zjišťuje jak problémy s výkonem virtuálního počítače s Windows toouse PerfInsights tootroubleshoot."
services: virtual-machines-windows'
documentationcenter: 
author: genlin
manager: cshepard
editor: na
tags: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: genli
ms.openlocfilehash: f23ff7708c0c63bd02674b1bdc07753e8a89d9be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-perfinsights"></a>Jak toouse PerfInsights 

[PerfInsights](http://aka.ms/perfinsightsdownload) je automatizovaného skriptu, který shromažďuje diagnostické informace užitečné, běží zatížení vstupu a výstupu přízvuk a poskytuje toohelp sestavy analýzy vyřešit problémy s výkonem virtuálního počítače s Windows v Microsoft Azure. 

Doporučujeme vám, spusťte tento skript před otevřením lístku podpory se společností Microsoft pro problémy s výkonem virtuálních počítačů.

## <a name="supported-troubleshooting-scenarios"></a>Podporované scénáře řešení potíží

PerfInsights můžete shromažďovat a analyzovat několik druhů informace, které jsou seskupené do jedinečný scénáře.

### <a name="collect-disk-configuration"></a>Shromáždit konfiguraci disku 

Tento scénář shromažďuje hello konfiguraci disků a další důležité informace, včetně hello následující položky:

-   Protokoly událostí

-   Stav sítě pro všechny příchozí a odchozí připojení

-   Konfigurace nastavení sítě a brány firewall

-   Seznam úloh pro všechny aplikace, které jsou aktuálně spuštěny v systému hello

-   Soubor s informacemi o vytvořené msinfo32 hello virtuálního počítače (VM)

-   Nastavení konfigurace databáze serveru Microsoft SQL Server (Pokud hello virtuální počítač se identifikuje jako server, který se systémem SQL Server)

-   Čítače spolehlivost úložiště

-   Důležité opravy hotfix pro Windows

-   Ovladače nainstalované filtru

Toto je pasivní kolekce informace, které by neměly ovlivnit hello systému. 

>[!Note]
>Tento scénář je automaticky zahrnuty v každé z následujících scénářů hello.

### <a name="benchmarkstorage-performance-test"></a>Test výkonu srovnávacího testu/úložiště

Tento scénář spouští hello [diskspd](https://github.com/Microsoft/diskspd) srovnávací test testu (IOPS a MB/s) pro všechny disky, které jsou připojené toohello virtuálních počítačů. 

> [!Note]
> V tomto scénáři může ovlivnit hello system a nesmí spustit v systému za provozu produkční. V případě potřeby spusťte tento scénář v tooavoid okno údržby vyhrazené potíže. Zvýšit zatížení, která je způsobena trasování nebo srovnávacího testu testovací může nepříznivě ovlivnit výkon hello vašeho virtuálního počítače.
>

### <a name="general-vm-slow-analysis"></a>Analýza pomalá obecné virtuálních počítačů 

Tento scénář spustí [čítače výkonu](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx) trasování pomocí hello čítačů, které jsou určené v souboru Generalcounters.txt hello. Pokud hello virtuální počítač se identifikuje jako server, který je spuštěn SQL Server, spustí se pomocí hello čítačů, které se nacházejí v souboru Sqlcounters.txt hello trasování čítače výkonu. Zahrnuje také výkonu diagnostická data.

### <a name="vm-slow-analysis-and-benchmark"></a>Virtuální počítač pomalé analýzy a srovnávacího testu

Tento scénář spustí [čítače výkonu](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx) trasování, který je následován [diskspd](https://github.com/Microsoft/diskspd) srovnávacího testu. 

> [!Note]
> V tomto scénáři může ovlivnit hello system a nesmí spustit v systému za provozu produkční. V případě potřeby spusťte tento scénář v tooavoid okno údržby vyhrazené potíže. Zvýšit zatížení, která je způsobena trasování nebo srovnávacího testu testovací může nepříznivě ovlivnit výkon hello vašeho virtuálního počítače.
>

### <a name="azure-files-analysis"></a>Azure analysis soubory 

Tento scénář se spustí zachycení čítače výkonu speciální společně s provést síťové trasování. Zachytávání zahrnuje všechny čítače "Sdílených složek SMB klienta" hello. Hello Toto jsou některé klíčové SMB klienta sdílenou složku čítače výkonu, které jsou součástí zachycení hello:

| **Typ**     | **Čítač sdílené složky SMB klienta** |
|--------------|-------------------------------|
| IOPS         | Data požadavků za sekundu             |
|              | Přečtěte si požadavky/s             |
|              | Zápis požadavků za sekundu            |
| Latence      | Střední doba nebo dat požadavku         |
|              | Průměrná doba/čtení                 |
|              | Průměrná doba/zápis                |
| Velikost vstupně-výstupní operace      | Střední Požadavek na bajtů nebo dat       |
|              | Střední Bajty/čtení               |
|              | Střední Bajty a zápis              |
| Propustnost   | Data bajty/s                |
|              | Číst bajty/s                |
|              | Zápis bajtů za sekundu               |
| Délka fronty | Střední Délka fronty pro čtení        |
|              | Střední Délka fronty zápisu       |
|              | Střední Délka fronty dat        |

### <a name="custom-configuration"></a>Vlastní konfigurace 

Když spustíte vlastní konfiguraci, používáte všech trasování (diagnostiku výkonu, čítače výkonu, xperf, sítě, storport) paralelně, podle toho, kolik různých trasování jsou vybrané. Po dokončení trasování hello nástroj spustí srovnávacího testu diskspd hello, pokud je zaškrtnuto. 

> [!Note]
> V tomto scénáři může ovlivnit hello system a nesmí spustit v systému za provozu produkční. V případě potřeby spusťte tento scénář v tooavoid okno údržby vyhrazené potíže. Zvýšit zatížení, která je způsobena trasování nebo srovnávacího testu testovací může nepříznivě ovlivnit výkon hello vašeho virtuálního počítače.
>

## <a name="what-kind-of-information-is-collected-by-hello-script"></a>Jaký druh informací se shromažďují skriptem hello?

Informace o virtuální počítač s Windows, disky nebo úložiště fondy konfigurace, čítače výkonu, protokoly a různé trasování jsou shromažďovány v závislosti na scénáři výkonu hello používá:

|Data shromážděná                              |  |  | Scénáře výkonu |  |  | |
|----------------------------------|----------------------------|------------------------------------|--------------------------|--------------------------------|----------------------|----------------------|
|                              | Shromažďování konfigurace disku | Test výkonu srovnávacího testu/úložiště | Analýza pomalá obecné virtuálních počítačů | Virtuální počítač pomalé analýzy a srovnávacího testu | Azure analysis soubory | Vlastní konfigurace |
| Informace z protokolů událostí      | Ano                        | Ano                                | Ano                      | Ano                            | Ano                  | Ano                  |
| Informace o systému               | Ano                        | Ano                                | Ano                      | Ano                            | Ano                  | Ano                  |
| Svazek mapy                       | Ano                        | Ano                                | Ano                      | Ano                            | Ano                  | Ano                  |
| Mapa disku                         | Ano                        | Ano                                | Ano                      | Ano                            | Ano                  | Ano                  |
| Spuštěné úlohy                    | Ano                        | Ano                                | Ano                      | Ano                            | Ano                  | Ano                  |
| Čítače spolehlivost úložiště     | Ano                        | Ano                                | Ano                      | Ano                            | Ano                  | Ano                  |
| Informace o úložiště              | Ano                        | Ano                                | Ano                      | Ano                            | Ano                  | Ano                  |
| Výstup fsutil                    | Ano                        | Ano                                | Ano                      | Ano                            | Ano                  | Ano                  |
| Informace o ovladač filtru               | Ano                        | Ano                                | Ano                      | Ano                            | Ano                  | Ano                  |
| Výstup příkazu netstat                   | Ano                        | Ano                                | Ano                      | Ano                            | Ano                  | Ano                  |
| Konfigurace sítě            | Ano                        | Ano                                | Ano                      | Ano                            | Ano                  | Ano                  |
| Konfigurace brány firewall           | Ano                        | Ano                                | Ano                      | Ano                            | Ano                  | Ano                  |
| Konfigurace systému SQL Server         | Ano                        | Ano                                | Ano                      | Ano                            | Ano                  | Ano                  |
| Trasování diagnostiky výkonu * |                            |                                    | Ano                      |                                |                      | Ano                  |
| Čítač výkonu trasování **     |                            |                                    |                          |                                |                      | Ano                  |
| Čítač SMB trasování **             |                            |                                    |                          |                                | Ano                  |                      |
| Čítače systému SQL Server trasování **      |                            |                                    |                          |                                |                      | Ano                  |
| XPerf trasování                      |                            |                                    |                          |                                |                      | Ano                  |
| StorPort trasování                   |                            |                                    |                          |                                |                      | Ano                  |
| Trasování sítě                    |                            |                                    |                          |                                | Ano                  | Ano                  |
| Trasování srovnávacího testu Diskspd ***      |                            | Ano                                |                          | Ano                            |                      |                      |
|       |                            |                         |                                                   |                      |                      |

### <a name="performance-diagnostics-trace-"></a>Trasování výkonu diagnostiky (*)

Spustí modul založený na pravidlech v datech toocollect pozadí hello a diagnostikovat problémy s probíhající výkonem. aktuálně jsou podporovány Hello následující pravidla:

- Pravidlo HighCpuUsage: zjistí vysoké období využití procesoru a zobrazuje hello hlavních spotřebitelů využití procesoru během těchto období.
- Pravidlo HighDiskUsage: zjistí využití období vysoké disku na fyzických discích a zobrazuje nejvyšší disku hello příjemci využití během těchto období.
- Pravidlo HighResolutionDiskMetric: ukazuje IOP, propustnosti a latence metriky vstupně-výstupní operace za 50 milisekund pro každého fyzického disku. Pomáhá tooquickly identifikaci disku omezení tečky.
- Pravidlo HighMemoryUsage: zjistí období využití velkého množství paměti a zobrazuje hello hlavní paměti příjemci využití během těchto období.

> [!NOTE] 
> V současné době jsou podporovány verze Windows, které zahrnují hello rozhraní .NET Framework 3.5 nebo novější verze.

### <a name="performance-counter-trace-"></a>Trasování čítače výkonu (*)

Shromažďuje hello následující čítače výkonu:

- \Process, \Processor, \Memory, \Thread, \PhysicalDisk, \LogicalDisk
- \Cache\Dirty stránky, zápisů \Cache\Lazy/s, \Server\Pool nestránkovaného fondu, chyb, selhání \Server\Pool stránkovaného fondu
- Vybrané čítače pod \Network rozhraní \IPv4\Datagrams, \IPv6\Datagrams, \TCPv4\Segments, \TCPv6\Segments, \Network adaptéru, \WFPv4\Packets, \WFPv6\Packets, \UDPv4\Datagrams, \UDPv6\Datagrams, \TCPv4\Connection, \TCPv6\Connection, \Network QoS Policy\Packets, \Per procesor síťové rozhraní karty aktivity, \Microsoft Winsock BSP

#### <a name="for-sql-server-instances"></a>Pro instance systému SQL Server
- Správce serveru: vyrovnávací paměti \SQL, \SQLServer:Resource fondu statistiky, \SQLServer:SQL Statistics\
- \SQLServer:Locks, \SQLServer:General statistiky
- \SQLServer:Access metody

#### <a name="for-azure-files"></a>Pro soubory Azure
\SMB sdílených složek klienta

### <a name="diskspd-benchmark-trace-"></a>Trasování srovnávacího testu Diskspd (*)
Zatížení testy Diskspd vstupně-výstupní operace [Disk operačního systému (zápisu) a jednotky fondu (čtení a zápis)]

## <a name="run-hello-perfinsights-on-your-vm"></a>Spustit hello PerfInsights na vašem virtuálním počítači

### <a name="what-do-i-have-tooknow-before-i-run-hello-script"></a>Co jsou tooknow před I spuštěním skriptu hello? 

**Požadavky na skript**

1.  Tento skript musí být spuštěn na hello virtuální počítač, který má potíže s výkonem hello. 

2.  Hello jsou podporovány následující operační systémy: Windows Server 2008 R2, 2012, 2012 R2, 2016; Windows 8.1 a Windows 10.

**Možné problémy při spuštění skriptu hello na produkční virtuální počítače:**

1.  skript Hello může nepříznivě ovlivnit výkon hello hello virtuálních počítačů při použití spolu s hello "Srovnávacího testu" nebo "Vlastní" scénář, který je nakonfigurovaný pomocí XPerf nebo nástroje DiskSpd. Buďte opatrní při spuštění skriptu hello v produkčním prostředí.

2.  Použijete-li skript hello společně s hello "Srovnávacího testu" nebo "Vlastní" scénář, který je nakonfigurovaný pomocí nástroje DiskSpd, ujistěte se, že žádné další aktivita na pozadí naruší hello vstupně-výstupní úlohy na discích hello testována.

3.  Ve výchozím nastavení používá skript hello hello dočasné úložiště disku toocollect data. Pokud trasování zůstane povolena delší dobu, může být hello množství dat, které jsou shromážděny relevantní. To může snížit hello dostupnost místa na disku dočasné hello, proto by to ovlivnilo jakékoli aplikace, které jsou závislé na této jednotce.

### <a name="how-do-i-run-perfinsights"></a>Jak spustit PerfInsights? 

toorun hello skriptu, postupujte takto:

1. Stáhněte si [PerfInsights.zip](http://aka.ms/perfinsightsdownload).

2. Odblokování hello PerfInsights.zip souboru. toodo tohoto souboru PerfInsights.zip hello klikněte pravým tlačítkem, vyberte **vlastnosti**. V hello **Obecné** vyberte **Odblokovat** a pak vyberte **OK**. Tím se zajistí, že hello skript se spustí bez jakékoli další bezpečnostní zobrazí výzvu.  

    ![Odemknout soubor zip hello](media/how-to-use-perfInsights/unlock-file.png)

3.  Rozbalte soubor PerfInsights.zip hello komprimované do dočasné jednotky (ve výchozím nastavení je to obvykle jednotka hello D). Hello komprimovaný soubor by měl obsahovat hello následující soubory a složky:

    ![soubory ve složce zip hello](media/how-to-use-perfInsights/file-folder.png)

4.  Otevřete prostředí Windows PowerShell jako správce a spusťte skript PerfInsights.ps1 hello.

    ```
    cd <hello path of PerfInsights folder >
    Powershell.exe -ExecutionPolicy UnRestricted -NoProfile -File .\\PerfInsights.ps1
    ```

    Můžete mít tooenter "y" tooif jste dotaz tooconfirm, že chcete zásady spouštění toochange hello.

    V dialogovém okně právní omezení hello jsou uvedeny hello možnost tooshare diagnostické informace a Microsoft Support. Také musí vyjádřit souhlas toohello licenční smlouvy toocontinue. Proveďte požadovaná nastavení a potom klikněte na **spustit skript**.

    ![Pole právní omezení](media/how-to-use-perfInsights/disclaimer.png)

5.  Odeslání případu číslo hello, pokud je k dispozici, při spuštění skriptu hello (to je pro naše statistiky). Potom klikněte na **OK**.
    
    ![Zadejte ID podpory](media/how-to-use-perfInsights/enter-support-number.png)

6.  Vyberte jednotku dočasné úložiště. Hello skriptu můžete automaticky rozpoznat hello písmeno jednotky hello. Dojde-li všechny problémy v této fázi, může být výzva tooselect hello jednotky (hello výchozí jednotce je D). Vygenerovaný protokoly jsou zde uloženy v protokolu hello\_složky kolekce. Po zadání nebo přijmout hello písmeno jednotky, klikněte na tlačítko **OK**.

    ![Zadejte jednotku](media/how-to-use-perfInsights/enter-drive.png)

7.  Vyberte hello k dispozici seznamu odstraňování potíží.

       ![Vyberte scénáře podpory](media/how-to-use-perfInsights/select-scenarios.png)

8.  Můžete také spustit PerfInsights bez uživatelského rozhraní.

    Hello následující příkaz spustí hello "Obecné virtuálních počítačů pomalá analysis" řešení potíží s scénář bez výzvy k uživatelského rozhraní nebo zaznamenání dat po dobu 30 sekund. Vyzve vás tooconsent toohello stejné právní omezení a smlouva EULA, které jsou uvedené v kroku 4.

        powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -NoGui -Scenario vmslow -TracingDuration 30"

    Pokud chcete PerfInsights toorun v bezobslužném režimu, použijte **- AcceptDisclaimerAndShareDiagnostics** parametr. Například použijte hello následující příkaz:

        powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -NoGui -Scenario vmslow -TracingDuration 30 -AcceptDisclaimerAndShareDiagnostics"

### <a name="how-do-i-troubleshoot-issues-while-running-hello-script"></a>Jak řešit problémy při spouštění skriptu hello?

Pokud skript hello ukončí neobvyklým způsobem, můžete vyčistit nekonzistentním stavu spuštěním skriptu hello společně s hello – přepínač čištění, následujícím způsobem:

    powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -Cleanup"

Pokud všechny problémy, ke kterým došlo během hello automatické zjišťování hello jednotku, může být výzva tooselect hello jednotky (hello výchozí jednotce je D).

![Zadejte jednotku](media/how-to-use-perfInsights/enter-drive.png)

skript Hello odinstaluje hello nástroj nástroje a odebere dočasné složky.

### <a name="troubleshooting-other-script-issues"></a>Řešení potíží s další problémy skriptu 

Dojde-li potíže při spuštění skriptu hello, stiskněte kombinaci kláves Ctrl + C toointerrupt hello skriptu provádění. dočasné objekty tooremove, najdete v části "Vyčištění po abnormální ukončení" hello.

Pokud budete pokračovat selhání skriptu tooexperience ani po několika pokusech, doporučujeme spustit skript hello v "režimu ladění" pomocí hello "-Debug" parametr možnost při spuštění.

Selhání hello dojde, kopie hello plné výstupní hello konzoly prostředí PowerShell, a odešlete toohello Microsoft Support agenta, který pomáhá toohelp řešení problému hello.

### <a name="how-do-i-run-hello-script-in-custom-configuration-mode"></a>Jak spustit skript hello v režimu vlastní konfigurace?

Výběrem hello **vlastní** konfigurace, můžete povolit několik trasování paralelně (použijte toomulti vyberte Shift):

![Vyberte scénáře](media/how-to-use-perfInsights/select-scenario.png)

Když vyberete hello diagnostiku výkonu, trasování čítače výkonu, trasování XPerf, síťové trasování nebo scénářů, Storport trasování, postupujte podle pokynů hello v dialogových oknech hello a zkuste tooreproduce hello nízký výkon problém po spuštění hello trasování.

Hello následující dialogové okno umožňuje spustit trasování:

![Spuštění trasování](media/how-to-use-perfInsights/start-trace-message.png)

toostop hello trasování, máte tooconfirm hello příkaz v dialogovém okně druhý.

![Zastavit trasování](media/how-to-use-perfInsights/stop-trace-message.png)
![Zastavit trasování](media/how-to-use-perfInsights/ok-trace-message.png)

Když hello trasování nebo dokončení operace, se generuje nový soubor ve D:\\protokolu\_kolekce (nebo dočasné jednotce hello) s názvem **CollectedData\_rrrr MM-dd\_hh\_mm \_ss.zip.** Můžete odeslat tento soubor toohello podporu agent pro analýzu.

## <a name="review-hello-diagnostics-report-created-by-perfinsights"></a>Prohlédněte si sestavu diagnostiky hello vytvořené PerfInsights

V rámci hello **CollectedData\_rrrr MM-dd\_hh\_mm\_ss.zip soubor** je generován PerfInsights, můžete najít sestavu ve formátu HTML s podrobnými informacemi o hello nálezů PerfInsights. tooreview hello sestavy, rozbalte položku hello **CollectedData\_rrrr MM-dd\_hh\_mm\_ss.zip** souboru a pak otevřete hello **PerfInsights Report.html**souboru.

Vyberte hello **zjištění** kartě.

![Karta najít](media/how-to-use-perfInsights/findingtab.png)

**Poznámky k**

-   Zprávy červeně jsou známé problémy s konfigurací, které může způsobit problémy s výkonem.

-   Upozornění, které představují-optimální konfigurace, které nutně nezpůsobí problémy s výkonem se zprávy žlutě.

-   Zprávy modře jsou pouze informativní příkazy.

Zkontrolujte hello HTTP odkazy na všechny chybové zprávy v red tooget více podrobné informace o zjištění hello a jak může ovlivnit výkon hello nebo osvědčené postupy pro výkonu optimalizované konfigurace.

### <a name="disk-configuration-tab"></a>Na kartě Konfigurace disku

Hello **přehled** část zobrazuje různé náhledy hello konfigurace úložiště, včetně informací z nástroje Diskpart a prostory úložiště

Hello **DiskMap** a **VolumeMap** části popisují na hlediska duální jak logické svazky a fyzické disky jsou související tooeach jiné.

V hello perspektivy fyzický disk (DiskMap) hello tabulce jsou uvedeny všechny logické svazky, které jsou spuštěny na disku hello. V následujícím příkladu hello spustí PhysicalDrive2 2 logické svazků vytvořených na více oddílů (J a H):

![Karta data](media/how-to-use-perfInsights/disktab.png)

V hello perspektivy svazek (*VolumeMap*), hello tabulkách jsou všechny fyzické disky hello v rámci každé logické svazku. Všimněte si, že pro RAID nebo dynamické disky, můžete spustit logický svazek na několik fyzických disků. V následující ukázka hello *C:\\připojit* je přípojný bod nakonfigurovaný jako *SpannedDisk* na PhysicalDisks \#2 a \#3:

![Karta svazku](media/how-to-use-perfInsights/volumetab.png)

### <a name="sql-server-tab"></a>Karta systému SQL Server

Pokud hello cílový virtuální počítač hostuje všechny instance systému SQL Server, zobrazí se další karta hello sestavy, který je pojmenován **systému SQL Server**:

![Karta SQL](media/how-to-use-perfInsights/sqltab.png)

Tato část obsahuje přehled"" a další dílčí karty pro každou z instance systému SQL Server hello hostované na hello virtuálních počítačů.

Hello oddílu "Přehled" obsahuje užitečné tabulku, která shrnuje všechny hello fyzické disky (systémové a datové disky) se systémem, které obsahují směs datových souborů a souborů protokolů transakci.

V následujícím příkladu hello *PhysicalDrive0* (spuštění jednotky hello C) se zobrazí, protože obě hello *modeldev* a *modellog* soubory jsou umístěny na jednotce hello C a jsou různé typy (například datový soubor a transakčního protokolu v uvedeném pořadí):

![LogInfo](media/how-to-use-perfInsights/loginfo.png)

karty specifické pro instanci systému SQL Server Hello obsahují obecné oddíl, který zobrazuje základní informace o vybrané instance hello a dalších částech rozšířené informace, včetně nastavení, konfigurace a možnosti uživatele.

## <a name="references-toohello-external-tools-used"></a>Odkazuje na toohello používat externí nástroje

### <a name="diskspd"></a>Nástroje Diskspd

DISKSPD je úložiště zatížení generátor a výkon testu nástroj od hello Windows a Windows Server a Server infrastruktury cloudu engineering týmů. Další informace najdete v tématu [Diskspd](https://github.com/Microsoft/diskspd).

### <a name="xperf"></a>XPerf

XPerf je trasování toocapture nástroj pro příkazový řádek z hello sady nástrojů výkonu systému Windows.

Další informace najdete v tématu [nástrojů výkonu systému Windows – Xperf](https://blogs.msdn.microsoft.com/ntdebugging/2008/04/03/windows-performance-toolkit-xperf/).

## <a name="next-steps"></a>Další kroky

### <a name="upload-diagnostics-logs-and-reports-toomicrosoft-support-for-further-review"></a>Odeslat diagnostické protokoly a sestavy tooMicrosoft podpory pro další kontrolu

Při práci s hello pracovníci Microsoft Support, může být požadovaný tootransmit hello výstupu, který je generován hello tooassist PerfInsights procesu odstraňování potíží.

agent Hello podporu pro vytvoření pracovního prostoru DTM a obdržíte e-mailovou zprávu, která obsahuje odkaz toohello [DTM portálu (https://filetransfer.support.microsoft.com/EFTClient/Account/Login.htm) a jedinečné ID uživatele a hesla.

Tato zpráva bude odeslána z **CTS automatizovaná diagnostiky služba** (ctsadiag@microsoft.com).

![Ukázka uvítací zprávu](media/how-to-use-perfInsights/supportemail.png)

Pro dodatečné zabezpečení, bude toochange vyžaduje heslo na prvním použití.

Po přihlášení tooDTM najdete dialogové okno pole tooupload hello **CollectedData\_rrrr MM-dd\_hh\_mm\_ss.zip** soubor, který byl shromážděný nástrojem PerfInsights.
