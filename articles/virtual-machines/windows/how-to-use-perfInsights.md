---
title: "Jak používat PerfInsights v Microsoft Azure | Microsoft Docs"
description: "Zjišťuje použití PerfInsights k řešení potíží s výkonem virtuálního počítače s Windows."
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
ms.openlocfilehash: f22bd42302b96118dba0d4e5e387c6798a0b8777
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-perfinsights"></a>Jak používat PerfInsights 

[PerfInsights](http://aka.ms/perfinsightsdownload) je automatizovaného skriptu, který shromažďuje diagnostické informace užitečné, běží zatížení vstupu a výstupu přízvuk a poskytuje sestavy analýzy k řešení potíží s výkonem virtuálního počítače s Windows v Microsoft Azure. 

Doporučujeme vám, spusťte tento skript před otevřením lístku podpory se společností Microsoft pro problémy s výkonem virtuálních počítačů.

## <a name="supported-troubleshooting-scenarios"></a>Podporované scénáře řešení potíží

PerfInsights můžete shromažďovat a analyzovat několik druhů informace, které jsou seskupené do jedinečný scénáře.

### <a name="collect-disk-configuration"></a>Shromáždit konfiguraci disku 

Tento scénář shromažďuje konfigurace disku a další důležité informace, včetně následující položky:

-   Protokoly událostí

-   Stav sítě pro všechny příchozí a odchozí připojení

-   Konfigurace nastavení sítě a brány firewall

-   Seznam úloh pro všechny aplikace, které jsou aktuálně spuštěny v systému

-   Soubor s informacemi o vytvořené msinfo32 pro virtuální počítač (VM)

-   Nastavení konfigurace databáze serveru Microsoft SQL Server (Pokud je virtuální počítač se identifikuje jako server, který se systémem SQL Server)

-   Čítače spolehlivost úložiště

-   Důležité opravy hotfix pro Windows

-   Ovladače nainstalované filtru

Toto je pasivní kolekce informace, které by neměly vliv na systém. 

>[!Note]
>Tento scénář je automaticky zahrnuty v každé z následujících scénářů.

### <a name="benchmarkstorage-performance-test"></a>Test výkonu srovnávacího testu/úložiště

Tento scénář běží [diskspd](https://github.com/Microsoft/diskspd) srovnávacího testu (IOPS a MB/s) pro všechny disky, které jsou připojené k virtuálnímu počítači. 

> [!Note]
> V tomto scénáři může mít vliv na systém a není doporučeno spouštět v systému za provozu produkční. V případě potřeby spusťte tento scénář v vyhrazené údržby vyhnuli potížím. Zvýšit zatížení, která je způsobena trasování nebo srovnávacího testu testovací může nepříznivě ovlivnit výkon virtuálního počítače.
>

### <a name="general-vm-slow-analysis"></a>Analýza pomalá obecné virtuálních počítačů 

Tento scénář spustí [čítače výkonu](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx) trasování pomocí čítačů, které jsou určené v souboru Generalcounters.txt. Pokud virtuální počítač se identifikuje jako server, který je spuštěn SQL Server, spustí se trasování čítače výkonu pomocí čítačů, které se nacházejí v souboru Sqlcounters.txt. Zahrnuje také výkonu diagnostická data.

### <a name="vm-slow-analysis-and-benchmark"></a>Virtuální počítač pomalé analýzy a srovnávacího testu

Tento scénář spustí [čítače výkonu](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx) trasování, který je následován [diskspd](https://github.com/Microsoft/diskspd) srovnávacího testu. 

> [!Note]
> V tomto scénáři může mít vliv na systém a není doporučeno spouštět v systému za provozu produkční. V případě potřeby spusťte tento scénář v vyhrazené údržby vyhnuli potížím. Zvýšit zatížení, která je způsobena trasování nebo srovnávacího testu testovací může nepříznivě ovlivnit výkon virtuálního počítače.
>

### <a name="azure-files-analysis"></a>Azure analysis soubory 

Tento scénář se spustí zachycení čítače výkonu speciální společně s provést síťové trasování. Zachytávání zahrnuje všechny čítače "Sdílených složek SMB klienta". Tady jsou některé klíčové SMB klienta sdílenou složku čítače výkonu, které jsou součástí zachytávání:

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

Když spustíte vlastní konfiguraci, používáte všech trasování (diagnostiku výkonu, čítače výkonu, xperf, sítě, storport) paralelně, podle toho, kolik různých trasování jsou vybrané. Po dokončení trasování je nástroj spuštěn srovnávacího testu diskspd, pokud je zaškrtnuto. 

> [!Note]
> V tomto scénáři může mít vliv na systém a není doporučeno spouštět v systému za provozu produkční. V případě potřeby spusťte tento scénář v vyhrazené údržby vyhnuli potížím. Zvýšit zatížení, která je způsobena trasování nebo srovnávacího testu testovací může nepříznivě ovlivnit výkon virtuálního počítače.
>

## <a name="what-kind-of-information-is-collected-by-the-script"></a>Jaký druh informací se shromažďují skript?

Informace o virtuální počítač s Windows, disky nebo úložiště fondy konfigurace, čítače výkonu, protokoly a různé trasování jsou shromažďovány v závislosti na scénáři výkonu použít:

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

Běží na pozadí pro shromažďování dat a diagnostikovat problémy s výkonem probíhající modul založený na pravidlech. Aktuálně jsou podporovány následující pravidla:

- Pravidlo HighCpuUsage: zjistí vysoké období využití procesoru a zobrazuje hlavních spotřebitelů využití procesoru během těchto období.
- Pravidlo HighDiskUsage: zjistí využití období vysoké disku na fyzických discích a zobrazuje nejvyšší disku příjemci využití během těchto období.
- Pravidlo HighResolutionDiskMetric: ukazuje IOP, propustnosti a latence metriky vstupně-výstupní operace za 50 milisekund pro každého fyzického disku. Pomáhá rychle zjistit disku omezení tečky.
- Pravidlo HighMemoryUsage: zjistí období využití velkého množství paměti a zobrazuje hlavní paměti příjemci využití během těchto období.

> [!NOTE] 
> V současné době jsou podporovány verze Windows, které zahrnují rozhraní .NET Framework 3.5 nebo novější verze.

### <a name="performance-counter-trace-"></a>Trasování čítače výkonu (*)

Shromažďuje následující čítače výkonu:

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

## <a name="run-the-perfinsights-on-your-vm"></a>Spustit PerfInsights na vašem virtuálním počítači

### <a name="what-do-i-have-to-know-before-i-run-the-script"></a>Co je nutné vědět před I spusťte skript? 

**Požadavky na skript**

1.  Tento skript musí spustit na virtuálním počítači, který má problémy s výkonem. 

2.  Jsou podporovány následující operační systémy: Windows Server 2008 R2, 2012, 2012 R2, 2016; Windows 8.1 a Windows 10.

**Možné problémy při spuštění skriptu na produkční virtuální počítače:**

1.  Skript může nepříznivě ovlivnit výkon virtuálního počítače, pokud se používá spolu s "Srovnávacího testu" nebo "Vlastní" scénář, který je nakonfigurovaný pomocí XPerf nebo nástroje DiskSpd. Buďte opatrní při spuštění skriptu v produkčním prostředí.

2.  Pokud použijete skript společně s "Srovnávacího testu" nebo "Vlastní" scénář, který je nakonfigurovaný pomocí nástroje DiskSpd, ujistěte se, že žádné další aktivita na pozadí naruší vstupně-výstupní úlohy na otestované disky.

3.  Ve výchozím nastavení používá skript ke shromažďování dat na jednotku dočasné úložiště. Pokud trasování zůstane povolena delší dobu, může být množství dat, které jsou shromážděny relevantní. To může snížit dostupnost místa na disku dočasné, proto by to ovlivnilo jakékoli aplikace, které jsou závislé na této jednotce.

### <a name="how-do-i-run-perfinsights"></a>Jak spustit PerfInsights? 

Pokud chcete spustit skript, postupujte takto:

1. Stáhněte si [PerfInsights.zip](http://aka.ms/perfinsightsdownload).

2. Odblokování PerfInsights.zip souboru. Chcete-li to provést, klikněte pravým tlačítkem na soubor PerfInsights.zip, vyberte možnost **vlastnosti**. V **Obecné** vyberte **Odblokovat** a pak vyberte **OK**. Tím se zajistí, že skript se spustí bez jakékoli další bezpečnostní zobrazí výzvu.  

    ![Odemkněte tento soubor zip](media/how-to-use-perfInsights/unlock-file.png)

3.  Rozbalte komprimovaný soubor PerfInsights.zip do dočasné jednotky (ve výchozím nastavení je to obvykle diskovou jednotku d). Komprimovaný soubor musí obsahovat následující soubory a složky:

    ![soubory ve složce zip](media/how-to-use-perfInsights/file-folder.png)

4.  Otevřete prostředí Windows PowerShell jako správce a spusťte skript PerfInsights.ps1.

    ```
    cd <the path of PerfInsights folder >
    Powershell.exe -ExecutionPolicy UnRestricted -NoProfile -File .\\PerfInsights.ps1
    ```

    Můžete chtít zadejte "y" Pokud se zobrazí výzva k potvrzení, že chcete změnit zásady spouštění.

    V dialogovém okně právní omezení budete mít možnost sdílet diagnostické informace s Microsoft Support. Budete také musí souhlasit s licenční smlouvy pokračujte. Proveďte požadovaná nastavení a potom klikněte na **spustit skript**.

    ![Pole právní omezení](media/how-to-use-perfInsights/disclaimer.png)

5.  Číslo případu odeslání, pokud je k dispozici, při spuštění skriptu (to je pro naše statistiky). Potom klikněte na **OK**.
    
    ![Zadejte ID podpory](media/how-to-use-perfInsights/enter-support-number.png)

6.  Vyberte jednotku dočasné úložiště. Skript může automaticky rozpoznat písmeno jednotky. Dojde-li všechny problémy v této fázi, může být vyzvání k vyberte jednotky (výchozí jednotce je D). Vygenerovaný protokoly jsou zde uloženy v protokolu\_složky kolekce. Po zadání nebo přijměte písmeno jednotky, klikněte na tlačítko **OK**.

    ![Zadejte jednotku](media/how-to-use-perfInsights/enter-drive.png)

7.  Vyberte ze seznamu zadaný odstraňování potíží.

       ![Vyberte scénáře podpory](media/how-to-use-perfInsights/select-scenarios.png)

8.  Můžete také spustit PerfInsights bez uživatelského rozhraní.

    Tento příkaz spustí "Obecné virtuálních počítačů pomalá analysis" řešení potíží s scénář bez výzvy k uživatelského rozhraní nebo zaznamenání dat po dobu 30 sekund. Budete vyzváni k souhlas s právní omezení a smlouva EULA, které jsou uvedené v kroku 4.

        powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -NoGui -Scenario vmslow -TracingDuration 30"

    Pokud chcete PerfInsights spouštět v bezobslužném režimu, použijte **- AcceptDisclaimerAndShareDiagnostics** parametr. Například použijte následující příkaz:

        powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -NoGui -Scenario vmslow -TracingDuration 30 -AcceptDisclaimerAndShareDiagnostics"

### <a name="how-do-i-troubleshoot-issues-while-running-the-script"></a>Jak řešit problémy při spouštění skriptu?

Pokud skript ukončí neobvyklým způsobem, můžete vyčistit nekonzistentním stavu spuštěním skriptu společně s přepínačem-čištění následujícím způsobem:

    powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -Cleanup"

Pokud všechny problémy, ke kterým došlo během automatické zjišťování dočasné jednotce, může být vyzvání k vyberte jednotky (výchozí jednotce je D).

![Zadejte jednotku](media/how-to-use-perfInsights/enter-drive.png)

Skript odinstaluje nástroj nástroje a odebere dočasné složky.

### <a name="troubleshooting-other-script-issues"></a>Řešení potíží s další problémy skriptu 

Dojde-li potíže při spuštění skriptu, stiskněte kombinaci kláves Ctrl + C přerušení provádění skriptu. Chcete-li odebrat dočasné objekty, najdete v části "Vyčištění po abnormální ukončení".

Pokud nadále dochází k selhání skriptu ani po několika pokusech, doporučujeme, abyste spusťte skript v "režimu ladění" pomocí "-Debug" parametr možnost při spuštění.

Po dojde k selhání, zkopírujte úplný výstup konzoly prostředí PowerShell a odeslat do agenta nástroje Microsoft Support, který pomáhá vám pomoci při řešení problému.

### <a name="how-do-i-run-the-script-in-custom-configuration-mode"></a>Jak spustit skript v režimu vlastní konfigurace?

Výběrem **vlastní** konfigurace, můžete povolit několik trasování paralelně (použijte posunu vícenásobným výběrem):

![Vyberte scénáře](media/how-to-use-perfInsights/select-scenario.png)

Když vyberete diagnostiku výkonu, trasování čítače výkonu, trasování XPerf, síťové trasování nebo scénářů, Storport trasování, postupujte podle pokynů v dialogových oknech a pokuste se znovu provést problémy s pomalým výkonem po spuštění trasování.

Následující dialogové okno umožňuje spustit trasování:

![Spuštění trasování](media/how-to-use-perfInsights/start-trace-message.png)

Chcete-li zastavit trasování, budete muset potvrzení příkazu v dialogovém okně druhý.

![Zastavit trasování](media/how-to-use-perfInsights/stop-trace-message.png)
![Zastavit trasování](media/how-to-use-perfInsights/ok-trace-message.png)

Po dokončení trasování nebo operací se generuje nový soubor ve D:\\protokolu\_kolekce (nebo dočasné jednotce) s názvem **CollectedData\_rrrr MM-dd\_hh\_mm\_ss.zip.** Tento soubor můžete odeslat do agenta podporu pro analýzu.

## <a name="review-the-diagnostics-report-created-by-perfinsights"></a>Zkontrolujte sestavu diagnostiky vytvořené PerfInsights

V rámci **CollectedData\_rrrr MM-dd\_hh\_mm\_ss.zip soubor** je generován PerfInsights, můžete najít s podrobnými informacemi o zjištění PerfInsights sestavu ve formátu HTML. Zkontrolujte sestavu, rozbalte **CollectedData\_rrrr MM-dd\_hh\_mm\_ss.zip** souboru a pak otevřete **PerfInsights Report.html** souboru.

Vyberte **zjištění** kartě.

![Karta najít](media/how-to-use-perfInsights/findingtab.png)

**Poznámky k**

-   Zprávy červeně jsou známé problémy s konfigurací, které může způsobit problémy s výkonem.

-   Upozornění, které představují-optimální konfigurace, které nutně nezpůsobí problémy s výkonem se zprávy žlutě.

-   Zprávy modře jsou pouze informativní příkazy.

Zkontrolujte odkazy HTTP pro všechny chybové zprávy červeně získat podrobnější informace o zjištění a jak může ovlivnit výkon nebo osvědčené postupy pro výkonu optimalizované konfigurace.

### <a name="disk-configuration-tab"></a>Na kartě Konfigurace disku

**Přehled** část zobrazuje různé náhledy konfiguraci úložiště, včetně informací z nástroje Diskpart a prostory úložiště

**DiskMap** a **VolumeMap** části popisují na hlediska duální jak logické svazky a fyzické disky jsou vzájemně souvisí.

V pohledu fyzický disk (DiskMap) v tabulce jsou uvedeny všechny logické svazky, které běží na disku. V následujícím příkladu spustí PhysicalDrive2 2 logické svazků vytvořených na více oddílů (J a H):

![Karta data](media/how-to-use-perfInsights/disktab.png)

Ve svazku perspektivy (*VolumeMap*), tabulky zobrazují všechny fyzické disky v rámci každé logické svazku. Všimněte si, že pro RAID nebo dynamické disky, můžete spustit logický svazek na několik fyzických disků. V následující ukázce *C:\\připojit* je přípojný bod nakonfigurovaný jako *SpannedDisk* na PhysicalDisks \#2 a \#3:

![Karta svazku](media/how-to-use-perfInsights/volumetab.png)

### <a name="sql-server-tab"></a>Karta systému SQL Server

Pokud cílový počítač hostuje všechny instance systému SQL Server, zobrazí se další karta v sestavě, který je pojmenován **systému SQL Server**:

![Karta SQL](media/how-to-use-perfInsights/sqltab.png)

Tato část obsahuje přehled"" a další dílčí karty pro každou z instance systému SQL Server, který je hostitelem virtuálního počítače.

V části "Přehled" obsahuje užitečné tabulku, která shrnuje všechny fyzické disky (systémové a datové disky) se systémem, které obsahují směs datových souborů a souborů protokolů transakci.

V následujícím příkladu *PhysicalDrive0* (spouštění jednotka C) se zobrazí, protože jak *modeldev* a *modellog* soubory jsou umístěny na jednotce C a jsou různých typů (jako je datový soubor a transakčního protokolu v uvedeném pořadí):

![LogInfo](media/how-to-use-perfInsights/loginfo.png)

Karty specifické pro instanci systému SQL Server obsahovat obecné oddíl, který zobrazuje základní informace o vybrané instanci a dalších částech rozšířené informace, včetně nastavení, konfigurace a možnosti uživatele.

## <a name="references-to-the-external-tools-used"></a>Odkazy na externí nástroje použít

### <a name="diskspd"></a>Nástroje Diskspd

DISKSPD je úložiště zatížení generátor a výkon testu nástroj od týmů engineering Windows a Windows Server a Server infrastruktury cloudu. Další informace najdete v tématu [Diskspd](https://github.com/Microsoft/diskspd).

### <a name="xperf"></a>XPerf

XPerf je nástroj příkazového řádku pro zachycení trasování ze sady nástrojů výkonu systému Windows.

Další informace najdete v tématu [nástrojů výkonu systému Windows – Xperf](https://blogs.msdn.microsoft.com/ntdebugging/2008/04/03/windows-performance-toolkit-xperf/).

## <a name="next-steps"></a>Další kroky

### <a name="upload-diagnostics-logs-and-reports-to-microsoft-support-for-further-review"></a>Odeslat diagnostické protokoly a sestavy do služby Microsoft Support ke další kontrolu

Při práci s zaměstnancům společnosti Microsoft Support, můžete být vyzváni k přenosu výstupu, který je generován PerfInsights pomůže proces řešení potíží.

Podpora agenta pro vytvoření pracovního prostoru DTM a obdržíte e-mailovou zprávu, která obsahuje odkaz [DTM portálu (https://filetransfer.support.microsoft.com/EFTClient/Account/Login.htm) a jedinečné ID uživatele a heslo.

Tato zpráva bude odeslána z **CTS automatizovaná diagnostiky služba** (ctsadiag@microsoft.com).

![Ukázka zprávy](media/how-to-use-perfInsights/supportemail.png)

Za účelem zvýšení zabezpečení bude nutné změnit heslo při prvním použití.

Po přihlášení do DTM zjistíte, dialogové okno pro odeslání **CollectedData\_rrrr MM-dd\_hh\_mm\_ss.zip** soubor, který byl shromážděný nástrojem PerfInsights.
