---
title: "řada aaaStorSimple 8000 jako cíl zálohování pomocí zálohování Exec | Microsoft Docs"
description: "Popisuje konfiguraci cíl zálohy zařízení StorSimple hello s Exec zálohování této společnosti."
services: storsimple
documentationcenter: 
author: harshakirank
manager: matd
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/05/2016
ms.author: hkanna
ms.openlocfilehash: 270ad95e1f6b367e80048cad42beb936f205f69c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-as-a-backup-target-with-backup-exec"></a>StorSimple jako cíl zálohování pomocí zálohování Exec

## <a name="overview"></a>Přehled

Azure StorSimple je řešením hybridní cloudové úložiště od společnosti Microsoft. StorSimple řeší hello složitosti nárůst exponenciální dat pomocí účtu úložiště Azure jako rozšíření hello v případě místních řešení a automaticky vrstvení dat mezi místní úložiště a úložiště v cloudu.

V tomto článku probereme StorSimple integrace s Exec zálohování této společnosti a osvědčené postupy pro integraci obě řešení. Můžeme také provést doporučení na způsobu, jakým tooset až toobest zálohování Exec integrovat StorSimple. Odložení jsme tooVeritas osvědčené postupy, zálohování architekty a správce pro hello nejlepší způsob, jak tooset zálohování Exec toomeet jednotlivé požadavky na zálohování a smlouvy o úrovni služeb (SLA).

I když jsme ilustrují kroky konfigurace a klíčové koncepty, tento článek je rozhodně není to podrobný Průvodce konfigurace nebo instalace. Předpokládáme, že hello základní součásti a infrastrukturu jsou v pořádku a připravena toosupport hello koncepty, které jsme popisují.

### <a name="who-should-read-this"></a>Komu je určen to?

Hello informace v tomto článku bude nejužitečnější toobackup správců, správců úložiště a úložiště architektům, kteří mají znalosti o úložiště, Windows Server 2012 R2, Ethernet, cloudové služby a Backup Exec.

## <a name="supported-versions"></a>Podporované verze

-   [Zálohování Exec 16 a novější verze](http://backupexec.com/compatibility)
-   [StorSimple Update 3 a novějších verzích](storsimple-overview.md#storsimple-workload-summary)


## <a name="why-storsimple-as-a-backup-target"></a>Proč StorSimple jako cíl zálohování?

StorSimple je vhodná pro cíl zálohy, protože:

-   Poskytuje standardní, místní úložiště pro zálohování aplikace toouse jako cíl pro rychlé zálohování, beze změn. Můžete taky použít StorSimple pro rychlé obnovení poslední zálohy.
-   Jeho cloudu vrstvení je bezproblémově integrovaná s toouse účet úložiště služby Azure cloud nákladově efektivní úložiště Azure.
-   Automaticky poskytuje úložiště mimo pracoviště pro zotavení po havárii.

## <a name="key-concepts"></a>Klíčové koncepty

Míra změn a požadavků na kapacitu růstu stejně jako u jakékoli řešení úložiště, postupujte opatrně hodnocení výkonu úložiště hello řešení, SLA, je důležité toosuccess. main nápad Hello je, zavedením cloudovou vrstvu toohello cloudu čas přístupu a propustnost hrát zásadní roli v hello schopnost StorSimple toodo úlohy.

StorSimple je tooapplications navrženou tooprovide úložiště, která pracovat dobře definovaný pracovní sady dat (aktivní data). V tomto modelu hello pracovní sadu dat je uložena v místních vrstvách hello a hello zbývající nepracovní, uloží málo používaná data nebo archivovat sadu dat vrstvené toohello cloud. Tento model je reprezentována v hello následující obrázek. Hello téměř nestrukturované zelená řádek představuje hello data uložená na místních vrstvách hello hello zařízení StorSimple. Hello red řádek představuje hello celkové množství dat uložených na hello řešení StorSimple napříč všechny úrovně. Hello mezery mezi hello nestrukturované zelená řádku a hello exponenciální red křivky představuje hello celkové množství dat uložených v cloudu hello.

**Vrstvení StorSimple**
![vrstvení diagram StorSimple](./media/storsimple-configure-backup-target-using-backup-exec/image1.jpg)

Tato architektura pamatovat zjistíte, že zařízení StorSimple je ideální toooperate jako cíl zálohování. Můžete použít StorSimple na:
-   Vaše nejčastěji se vyskytující obnovení proveďte z hello místní pracovní sady dat.
-   Použijte hello cloudu pro obnovení po havárii mimo pracoviště a starší data, kde jsou méně časté obnovení.

## <a name="storsimple-benefits"></a>Výhody StorSimple

V případě místních řešení, která je bezproblémově integrovaná s Microsoft Azure poskytuje StorSimple, díky bezproblémový přístup k místní tooon a cloudového úložiště.

StorSimple používá automatické vrstvení mezi hello místní zařízení, která má SSD zařízení (SSD) a serial-attached SCSI (SAS), úložiště a Azure Storage. Automatické vrstvení udržuje často používaná data místní na vrstvy SSD a SAS hello. Přesune málokdy načítaná data tooAzure úložiště.

StorSimple nabízí tyto výhody:

-   Jedinečný odstraňování duplicitních dat a komprese algoritmy, které používají hello cloudu tooachieve nebývalá odstranění duplicit úrovně
-   Vysoká dostupnost
-   Geografická replikace pomocí Azure geografická replikace
-   Integrace se službou Azure
-   Šifrování dat v cloudu hello
-   Zotavení po havárii vylepšené a dodržování předpisů

I když StorSimple uvede dva scénáře hlavní nasazení (primární cíl zálohy a sekundární cíl zálohy), od základu, je prostý, zařízení s blokovým úložištěm. StorSimple všechny hello komprese a odstranění duplicit. Bezproblémově odešle a načte data mezi hello cloudu a hello aplikace a systému souborů.

Další informace o zařízení StorSimple, najdete v části [řady StorSimple 8000: hybridní cloudové úložiště řešení](storsimple-overview.md). Navíc můžete zkontrolovat hello [technických specifikací řady StorSimple 8000](storsimple-technical-specifications-and-compliance.md).

> [!IMPORTANT]
> Pomocí StorSimple zařízení jako cíl zálohování je podporováno pouze pro zařízení StorSimple 8000 Update 3 a novějších verzích.

## <a name="architecture-overview"></a>Přehled architektury

Hello následující tabulky popisují hello zařízení modelu architektura počáteční pokyny.

**StorSimple kapacity pro místní a cloudové úložiště**

| Kapacita úložiště       | 8100          | 8600            |
|------------------------|---------------|-----------------|
| Místní úložiště kapacity | &lt;10 TiB\*  | &lt;20 TiB\*  |
| Kapacita cloudového úložiště | &gt;200 TiB\* | &gt;500 TiB\* |
\*Velikost úložiště předpokládá bez odstranění duplicitních dat nebo kompresi.

**StorSimple kapacity pro primární a sekundární zálohování**

| Zálohování scénář  | Místní úložiště kapacity  | Kapacita cloudového úložiště  |
|---|---|---|
| Primární zálohu  | Nedávné zálohy uložený v místním úložišti pro rychlé obnovení toomeet cíl bodu obnovení (RPO) | Historie zálohování (RPO) se vejde kapacity cloudu |
| Sekundární zálohování | Sekundární kopii zálohovaných dat mohou být uloženy ve kapacity cloudu  | Není k dispozici  |

## <a name="storsimple-as-a-primary-backup-target"></a>StorSimple jako primární cíl zálohování

V tomto scénáři jsou uvedené svazky zařízení StorSimple toohello zálohovací aplikace jako hello jedinou úložiště pro zálohy. Hello následující obrázek znázorňuje architekturu řešení, ve kterém použijte všechny zálohy zařízení StorSimple zřízeny vrstvené svazky pro zálohování a obnovení.

![StorSimple jako primární cíl zálohy logického diagramu](./media/storsimple-configure-backup-target-using-backup-exec/primarybackuptargetlogicaldiagram.png)

### <a name="primary-target-backup-logical-steps"></a>Primární cíl zálohování logických kroků.

1.  Kontakty záložní server Hello hello cíl zálohování agenta a agenta zálohování hello odesílá data toohello zálohování serveru.
2.  Zálohování serveru Hello zapisuje data toohello StorSimple zřízeny vrstvené svazky.
3.  Zálohování serveru Hello aktualizuje hello katalogu databáze a pak dokončí úlohu zálohování hello.
4.  Skript snímku aktivuje hello cloud snapshot manager zařízení StorSimple (spuštění nebo odstranění).
5.  Zálohování serveru Hello odstraní vypršela platnost záloh na základě zásad uchovávání informací.


### <a name="primary-target-restore-logical-steps"></a>Primární cíl obnovení logických kroků

1.  spuštění zálohování serveru Hello obnovení hello příslušná data z úložiště hello úložiště.
2.  agent zálohování Hello přijímá hello data z hello zálohování serveru.
3.  Zálohování serveru Hello dokončení úlohy obnovení hello.

## <a name="storsimple-as-a-secondary-backup-target"></a>StorSimple jako sekundární cíl zálohování

V tomto scénáři se svazky zařízení StorSimple primárně používají pro dlouhodobé uchovávání nebo archivovat.

Hello následující obrázek ukazuje architekturu, ve které prvotní zálohy a obnoví vysoce výkonné cílový svazek. Tyto zálohy jsou zkopírovány a archivovaný tooa StorSimple vrstvené svazku podle nastaveného plánu.

Je důležité toosize je váš svazek vysoce výkonné tak, aby ji může zpracovat požadavky vaší uchování zásad kapacitu a výkon.

![StorSimple jako sekundární cíl zálohy logického diagramu](./media/storsimple-configure-backup-target-using-backup-exec/secondarybackuptargetlogicaldiagram.png)

### <a name="secondary-target-backup-logical-steps"></a>Sekundární cíl zálohování logických kroků.

1.  Kontakty záložní server Hello hello cíl zálohování agenta a agenta zálohování hello odesílá data toohello zálohování serveru.
2.  Zálohování serveru Hello zapíše data toohigh výkon úložiště.
3.  Zálohování serveru Hello aktualizuje hello katalogu databáze a pak dokončí úlohu zálohování hello.
4.  Zálohování serveru Hello zkopíruje tooStorSimple zálohy na základě zásad uchovávání informací.
5.  Skript snímku aktivuje hello cloud snapshot manager zařízení StorSimple (spuštění nebo odstranění).
6.  Zálohování serveru Hello odstraní vypršela platnost záloh na základě zásad uchovávání informací.

### <a name="secondary-target-restore-logical-steps"></a>Sekundární cíl obnovení logických kroků

1.  spuštění zálohování serveru Hello obnovení hello příslušná data z úložiště hello úložiště.
2.  agent zálohování Hello přijímá hello data z hello zálohování serveru.
3.  Zálohování serveru Hello dokončení úlohy obnovení hello.

## <a name="deploy-hello-solution"></a>Nasazení řešení hello

Nasazení řešení hello vyžaduje tři kroky:
1. Připravte hello síťové infrastruktury.
2. Nasazení zařízení StorSimple jako cíl zálohování.
3. Nasazení zálohování Exec.

Každý krok je podrobněji v následující části hello.

### <a name="set-up-hello-network"></a>Nastavení sítě hello

Protože StorSimple je řešení, která je integrovaná s hello cloudu Azure, vyžaduje StorSimple aktivní a funkční připojení k toohello cloudu Azure. Toto připojení se používá pro operace, jako je cloudových snímků, správu a metadata přenos a tootier starší, méně používaná data tooAzure cloudového úložiště.

Pro hello řešení tooperform optimálně, doporučujeme dodržovat tyto doporučené postupy pro sítě:

-   Hello odkaz, který se připojí vaše vrstvení tooAzure StorSimple musí splňovat požadavky na šířku pásma. tooachieve, použít hello nezbytné Quality of Service (QoS) úrovni tooyour infrastruktury přepínače toomatch plánovaný bod obnovení a obnovení čas SLA cíl (RTO).
-   Maximální latence přístupu k úložišti objektů Blob v Azure by měl být přibližně 80 ms.

### <a name="deploy-storsimple"></a>Nasazení zařízení StorSimple

Podrobné pokyny StorSimple nasazení najdete v tématu [nasazení místního zařízení StorSimple](storsimple-deployment-walkthrough-u2.md).

### <a name="deploy-backup-exec"></a>Nasazení zálohování Exec

Osvědčené postupy instalace Exec zálohování, naleznete v části [osvědčené postupy pro instalaci zálohování Exec](https://www.veritas.com/support/en_US/article.000068207).

## <a name="set-up-hello-solution"></a>Nastavit řešení hello

V této části ukážeme některé příklady konfigurace. Hello následující příklady a doporučení znázorňují hello nejvíce základní a základní implementaci. Tato implementace nemusí vztahovat přímo tooyour konkrétní požadavky na zálohování.

### <a name="set-up-storsimple"></a>Nastavení pro zařízení StorSimple

| Úlohy nasazení zařízení StorSimple  | Další komentáře |
|---|---|
| Nasazení místního zařízení StorSimple. | Podporované verze: aktualizace 3 a vyšší verze. |
| Zapněte hello cíl zálohy. | Použijte tyto příkazy tooturn na nebo vypněte režim cíl zálohy a tooget stavu. Další informace najdete v tématu [vzdáleně připojit zařízení StorSimple tooa](storsimple-remote-connect.md).</br> tooturn na režim zálohování: `Set-HCSBackupApplianceMode -enable`. </br> tooturn vypnout režim zálohování: `Set-HCSBackupApplianceMode -disable`. </br> tooget hello aktuální stav nastavení režim zálohování: `Get-HCSBackupApplianceMode`. |
| Vytvořte kontejner svazků společné pro váš svazek, který zálohuje data hello. Všechna data v kontejneru svazku se odstraňují. | Kontejnery svazků zařízení StorSimple definování domén odstranění duplicit.  |
| Vytvořte svazky zařízení StorSimple. | Vytvořte svazky s velikostí jako zavřít toohello očekává využití nejdříve, protože velikost svazku ovlivňují cloud snímku doba trvání. Informace o toosize svazek, přečtěte si informace o [zásady uchovávání informací](#retention-policies).</br> </br> Použití StorSimple vrstvené svazky a vyberte hello **použít tento svazek pro archivní data s méně často používaná** zaškrtávací políčko. </br> Použití pouze místně vázaný svazky není podporováno. |
| Vytvoření jedinečné zásady StorSimple zálohování pro všechny svazky hello cíl zálohy. | Zásady zálohování StorSimple definuje skupinu konzistence hello svazku. |
| Plán hello zakážete, protože platnost vyprší hello snímky. | Snímky jsou aktivovány jako následného zpracování operace. |

### <a name="set-up-hello-host-backup-server-storage"></a>Nastavení úložiště zálohování serveru hostitele hello

Nastavení úložiště zálohování serveru hostitele hello podle toothese pokyny:  

- Nepoužívejte rozložené svazky (vytvořený Správa disků systému Windows). Rozložené disky nejsou podporovány.
- Formátovat pomocí systému souborů NTFS o velikosti 64 KB alokační svazků.
- Mapování svazky zařízení StorSimple hello přímo toohello Exec zálohování serveru.
    - Použijte iSCSI pro fyzických serverů.
    - Použijte průchozí disky pro virtuální servery.

## <a name="best-practices-for-storsimple-and-backup-exec"></a>Osvědčené postupy pro StorSimple a Exec zálohování

Nastavte řešení podle toohello pokyny v následující části hello.

### <a name="operating-system-best-practices"></a>Osvědčené postupy operačního systému

-   Zakažte šifrování na Windows serveru a odstranění duplicit pro systém souborů NTFS hello.
-   Zakážete defragmentace systému Windows Server na svazky zařízení StorSimple hello.
-   Zakažte indexování na hello svazky zařízení StorSimple systému Windows Server.
-   Spusťte antivirovým na zdrojovém hostiteli hello (ne proti svazky zařízení StorSimple hello).
-   Vypnout hello výchozí [údržby systému Windows Server](https://msdn.microsoft.com/library/windows/desktop/hh848037.aspx) ve Správci úloh. Proveďte to v jednom z následujících způsobů hello:
   - Vypněte hello údržby configurator v Plánovači úloh systému Windows.
   - Stáhněte si [PsExec](https://technet.microsoft.com/sysinternals/bb897553.aspx) z webu Windows Sysinternals. Po stažení nástroje PsExec, spusťte prostředí Azure PowerShell jako správce a zadejte:
      ```powershell
      psexec \\%computername% -s schtasks /change /tn “MicrosoftWindowsTaskSchedulerMaintenance Configurator" /disable
      ```

### <a name="storsimple-best-practices"></a>Osvědčené postupy StorSimple

  -   Ujistěte se, toto zařízení StorSimple hello se aktualizuje příliš[Update 3 nebo novější](storsimple-install-update-3.md).
  -   Izolovat přenosy iSCSI a cloudu. Použijte vyhrazené iSCSI připojení pro přenos dat mezi StorSimple a hello zálohování serveru.
  -   Ujistěte se, že zařízení StorSimple je vyhrazené cíl zálohy. Smíšené úlohy nejsou podporované, protože mají vliv na RTO a plánovaný bod obnovení.

### <a name="backup-exec-best-practices"></a>Zálohování Exec osvědčené postupy

-   Exec – zálohování musí být nainstalována na místním disku serveru hello a ne na svazek StorSimple.
-   Nastavení úložiště zálohování Exec hello **souběžných operací zápisu** toohello maximální povolené.
    -   Nastavení úložiště zálohování Exec hello **velikost bloku a vyrovnávací paměť** too512 KB.
    -   Zapnout úložiště zálohování Exec **uložená do vyrovnávací paměti pro čtení a zápisu**.
-   StorSimple podporuje Exec zálohování úplné a přírůstkové zálohování. Doporučujeme, abyste nepoužívali syntetické a rozdílové zálohy.
-   Zálohování datových souborů by měl obsahovat data jen pro určité úlohy. Například žádné médium připojí přes různé úlohy jsou povoleny.
-   Vypněte úlohu ověřování. V případě potřeby nutné naplánovat ověření po hello nejnovější úlohu zálohování. Je důležité toounderstand, že tato úloha ovlivňuje okno zálohování.
-   Vyberte **úložiště** > **disk** > **podrobnosti** > **vlastnosti**. Vypnout **předem přidělit místo na disku**.

Nejnovější nastavení zálohování Exec hello a osvědčené postupy při implementaci těchto požadavků naleznete v tématu [hello této společnosti webu](https://www.veritas.com).

## <a name="retention-policies"></a>Zásady uchovávání informací

Jeden z typů zásad uchovávání záloh nejběžnější hello je zásada Dědečka otec a SYN (GFS). V zásadách GFS přírůstkové zálohy se provádí denně a úplné zálohy se provádějí týdenní a měsíční. Výsledkem zásad šesti StorSimple zřízeny vrstvené svazky. Jeden svazek obsahuje hello týdenní, měsíční nebo roční úplné zálohy. Hello jiných pět svazků ukládání každodenní přírůstkové zálohování.

V následující ukázka hello použijeme rotaci GFS. Příklad Hello předpokládá hello následující:

-   Bez zajištěná nebo komprimovaných dat se používá.
-   Úplné zálohy představují 1 TiB.
-   Každodenní přírůstkové zálohování jsou 500 GiB.
-   Čtyři týdenní zálohy jsou v měsíci.
-   Dvanáct měsíční zálohy jsou v roce.
-   Jeden rok zálohování je udržováno 10 let.

Podle hello předcházející předpoklady, vytvořit 26-TiB StorSimple vrstvené svazek pro hello měsíční a roční úplné zálohy. Vytvoření 5-TiB StorSimple vrstvené svazku pro každý hello přírůstkové denní zálohy.

| Uchování typ zálohy | Velikost (TiB) | Násobitel GFS\* | Celková kapacita (TiB)  |
|---|---|---|---|
| Týdenní úplné | 1 | 4  | 4 |
| Každodenní přírůstkové | 0.5 | 20 (cykly stejný počet týdnů v měsíci) | 12 (2 pro další kvóty) |
| Měsíční úplné | 1 | 12 | 12 |
| Roční úplné | 1  | 10 | 10 |
| Požadavek GFS |   | 38 |   |
| Další kvóty  | 4  |   | 42 celkový požadavek GFS  |
\*Hello GFS násobitel je hello počet kopií potřebujete tooprotect a zachovat toomeet požadavků zásady zálohování.

## <a name="set-up-backup-exec-storage"></a>Nastavení úložiště zálohování Exec

### <a name="tooset-up-backup-exec-storage"></a>tooset Exec zálohování úložiště

1.  V konzole pro správu zálohování Exec hello vyberte **úložiště** > **konfigurace úložiště** > **diskové úložiště**  >   **Další**.

    ![Zálohování Exec konzoly pro správu, konfigurace úložiště stránky](./media/storsimple-configure-backup-target-using-backup-exec/image4.png)

2.  Vyberte **diskového úložiště**a potom vyberte **Další**.

    ![Zálohování Exec konzoly pro správu, vyberte úložiště stránky](./media/storsimple-configure-backup-target-using-backup-exec/image5.png)

3.  Například zadejte název reprezentativní **sobota úplné**a popis. Vyberte **Další**.

    ![Zálohování Exec správy konzoly, název a popis stránky](./media/storsimple-configure-backup-target-using-backup-exec/image7.png)

4.  Vyberte hello disku, kam chcete toocreate hello disku úložné zařízení a potom vyberte **Další**.

    ![Zálohování Exec konzoly pro správu, stránka pro výběr disku úložiště](./media/storsimple-configure-backup-target-using-backup-exec/image9.png)

5.  Zvýší počet operací zápisu na hello příliš**16**a potom vyberte **Další**.

    ![Zálohování Exec konzoly pro správu souběžné zapsat operace nastavení stránky](./media/storsimple-configure-backup-target-using-backup-exec/image10.png)

6.  Zkontrolujte nastavení hello a pak vyberte **Dokončit**.

    ![Zálohování Exec konzoly pro správu úložiště konfigurace souhrnná stránka](./media/storsimple-configure-backup-target-using-backup-exec/image11.png)

7.  Na konci každé přiřazení svazku na hello, změnit hello úložiště zařízení nastavení toomatch ty doporučené na [osvědčené postupy pro StorSimple a Backup Exec](#best-practices-for-storsimple-and-backup-exec).

    ![Zálohování Exec konzoly pro správu, stránky nastavení zařízení úložiště](./media/storsimple-configure-backup-target-using-backup-exec/image12.png)

8.  Opakujte kroky 1-7, dokud přiřazení vaší tooBackup svazků zařízení StorSimple Exec.

## <a name="set-up-storsimple-as-a-primary-backup-target"></a>Nastavit StorSimple jako primární cíl zálohy

> [!NOTE]
> Obnovení dat ze zálohy, která byla vrstvené toohello cloudu nastane v cloudu rychlosti.

Hello následující obrázek znázorňuje hello mapování úlohu zálohování tooa typické svazku. V takovém případě všechny týdenní zálohy hello mapování toohello sobota celého disku a přírůstkové zálohování hello mapování tooMonday pátek přírůstkové disky. Všechny hello zálohování a obnovení jsou z StorSimple vrstvené svazku.

![Logický diagram konfigurace primární cíl zálohy](./media/storsimple-configure-backup-target-using-backup-exec/primarybackuptargetdiagram.png)

### <a name="storsimple-as-a-primary-backup-target-gfs-schedule-example"></a>StorSimple jako primární cíl zálohování GFS naplánovat příklad

Tady je příklad plánu otočení GFS čtyři týdny, měsíční nebo roční:

| Typ frekvence nebo zálohování | Úplná | Přírůstkové (1-5 dnů)  |   
|---|---|---|
| Každý týden (1 – 4 týdny) | Sobota | Pondělí – pátek |
| Měsíční  | Sobota  |   |
| Ročně | Sobota  |   |   |


### <a name="assign-storsimple-volumes-tooa-backup-exec-backup-job"></a>Přiřadit úlohu zálohování tooa zálohování Exec svazky pro StorSimple

Hello následující pořadí předpokládá, že tento cílový hostitel Exec zálohování a hello jsou nakonfigurované v souladu s pokyny agenta zálohování Exec hello.

#### <a name="tooassign-storsimple-volumes-tooa-backup-exec-backup-job"></a>tooassign StorSimple svazky tooa zálohování Exec zálohování úlohy

1.  V konzole pro správu zálohování Exec hello vyberte **hostitele** > **zálohování** > **zálohování tooDisk**.

    ![Zálohování Exec konzoly pro správu, vyberte hostitele, zálohování a zálohování toodisk](./media/storsimple-configure-backup-target-using-backup-exec/image14.png)

2.  V hello **vlastnosti definice zálohování** dialogovém **zálohování**, vyberte **upravit**.

    ![Zálohování Exec konzoly pro správu, dialogové okno Vlastnosti definice zálohování](./media/storsimple-configure-backup-target-using-backup-exec/image15.png)

3.  Nastavte úplné a přírůstkové zálohování, která splňují vaše požadavky na plánovaný bod obnovení a RTO a požadavkům tooVeritas osvědčené postupy.

4.  V hello **možnosti zálohování** dialogové okno, vyberte **úložiště**.

    ![Zálohování Exec konzoly pro správu, dialogové okno Možnosti úložiště záloh](./media/storsimple-configure-backup-target-using-backup-exec/image16.png)

5.  Přiřaďte odpovídající StorSimple svazky tooyour plán zálohování.

    > [!NOTE]
    > **Komprese** a **typ šifrování** nastaveny příliš**žádné**.

6.  V části **ověřte**, vyberte hello **Neověřovat data pro tuto úlohu** zaškrtávací políčko. Pomocí této možnosti může ovlivnit vrstvení StorSimple.

    > [!NOTE]
    > Defragmentace, indexování a ověření pozadí negativně ovlivnit vrstvení StorSimple hello.

    ![Ověřte nastavení zálohování Exec konzoly pro správu, možnosti zálohování](./media/storsimple-configure-backup-target-using-backup-exec/image17.png)

7.  Pokud jste nastavili hello zbytek vaše možnosti zálohování toomeet vaše požadavky, vyberte **OK** toofinish.

## <a name="set-up-storsimple-as-a-secondary-backup-target"></a>Nastavit StorSimple jako sekundární cíl zálohy

> [!NOTE]
>Obnovení dat ze zálohy, která byla vrstvené toohello cloudu dojít rychlostmi cloudu.

V tomto modelu musí mít tooserve médium (jiné než StorSimple) úložiště jako dočasnou vyrovnávací paměť. Například můžete použít redundantní pole nezávislých disků (RAID) tooaccommodate místa na disku, vstupní a výstupní (I/O) a šířky pásma. Doporučujeme používat RAID 5, 50 a 10.

Hello následující obrázek znázorňuje Typická krátkodobý uchování místní (toohello server) svazky a dlouhodobé uchovávání archivy svazky. V tomto scénáři spustit všechny zálohy na místní hello (toohello server) svazek RAID. Tyto zálohy jsou pravidelně duplicitní a archivovaný tooan archivy svazku. Ho je důležité toosize (toohello server) na místní svazek RAID, takže ji může zpracovat krátkodobé uchování požadavky kapacitu a výkon.

### <a name="storsimple-as-a-secondary-backup-target-gfs-example"></a>StorSimple jako v příkladu GFS sekundární cíl zálohy

![StorSimple jako sekundární cíl zálohy logického diagramu](./media/storsimple-configure-backup-target-using-backup-exec/secondarybackuptargetdiagram.png)

Hello následující tabulka ukazuje, jak tooset až toorun zálohy na místní hello a StorSimple disky. Její součástí jsou požadavky na jednotlivé a celkové kapacity.

### <a name="backup-configuration-and-capacity-requirements"></a>Konfigurace zálohování a požadavky na kapacitu

| Typ zálohování a uchovávání | Úložiště | Velikost (TiB) | Násobitel GFS | Celková kapacita\* (TiB) |
|---|---|---|---|---|
| Týden 1 (úplné a přírůstkové) |Místní disk (krátkodobé)| 1 | 1 | 1 |
| Týdny StorSimple 2 – 4 |StorSimple disku (dlouhodobé) | 1 | 4 | 4 |
| Měsíční úplné |StorSimple disku (dlouhodobé) | 1 | 12 | 12 |
| Roční úplné |StorSimple disku (dlouhodobé) | 1 | 1 | 1 |
|Požadavek na velikost svazků GFS |  |  |  | 18*|
\*Celkové kapacity zahrnuje 17 TiB StorSimple disky a 1 TiB místní svazek RAID.


### <a name="gfs-example-schedule-gfs-rotation-weekly-monthly-and-yearly-schedule"></a>Plán GFS příklad: GFS otočení týdenní, měsíční a roční plán

| Týden | Úplná | Přírůstkové den 1 | Přírůstkové den 2 | Přírůstkové den 3 | Přírůstkové den 4 | Přírůstkové den 5 |
|---|---|---|---|---|---|---|
| 1 týden | Místní svazek RAID  | Místní svazek RAID | Místní svazek RAID | Místní svazek RAID | Místní svazek RAID | Místní svazek RAID |
| Týden 2 | Týdny StorSimple 2 – 4 |   |   |   |   |   |
| Týden 3 | Týdny StorSimple 2 – 4 |   |   |   |   |   |
| Týden 4 | Týdny StorSimple 2 – 4 |   |   |   |   |   |
| Měsíční | Měsíčně StorSimple |   |   |   |   |   |
| Ročně | Ročně StorSimple  |   |   |   |   |   |   |


### <a name="assign-storsimple-volumes-tooa-backup-exec-archive-and-deduplication-job"></a>Přiřadit zařízení StorSimple svazky tooa zálohování Exec archivu a odstranění duplicit úlohy

#### <a name="tooassign-storsimple-volumes-tooa-backup-exec-archive-and-duplication-job"></a>tooassign StorSimple svazky tooa zálohování Exec archivu a duplikování úlohy

1.  V konzole pro správu zálohování Exec hello, klikněte pravým tlačítkem na úlohu hello má svazek StorSimple tooa tooarchive a pak vyberte **vlastnosti definice zálohování** > **upravit**.

    ![Zálohování Exec konzoly pro správu, karta Vlastnosti definice zálohování](./media/storsimple-configure-backup-target-using-backup-exec/image19.png)

2.  Vyberte **přidat fázi** > **duplicitní tooDisk** > **upravit**.

    ![Zálohování konzoly pro správu Exec, přidejte fáze](./media/storsimple-configure-backup-target-using-backup-exec/image20.png)

3.  V hello **duplicitní možnosti** dialogové okno, vyberte hello hodnoty, které chcete toouse pro **zdroj** a **plán**.

    ![Zálohování Exec konzoly pro správu, zálohování definice vlastnosti a možnosti duplicitní](./media/storsimple-configure-backup-target-using-backup-exec/image21.png)

4.  V hello **úložiště** rozevíracího seznamu, vyberte hello StorSimple svazku, kde chcete hello archivu úlohy toostore hello data.

    ![Zálohování Exec konzoly pro správu, zálohování definice vlastnosti a možnosti duplicitní](./media/storsimple-configure-backup-target-using-backup-exec/image22.png)

5.  Vyberte **ověřte**a potom vyberte hello **Neověřovat data pro tuto úlohu** zaškrtávací políčko.

    ![Zálohování Exec konzoly pro správu, zálohování definice vlastnosti a možnosti duplicitní](./media/storsimple-configure-backup-target-using-backup-exec/image23.png)

6.  Vyberte **OK**.

    ![Zálohování Exec konzoly pro správu, zálohování definice vlastnosti a možnosti duplicitní](./media/storsimple-configure-backup-target-using-backup-exec/image24.png)

7.  V hello **zálohování** sloupce, přidat nové fázi. Pro zdroj hello použít **přírůstkové**. Pro cíl hello zvolte hello svazek StorSimple, kde bude archivován hello úloha přírůstkového zálohování. Opakujte kroky 1 až 6.

## <a name="storsimple-cloud-snapshots"></a>Cloudové snímky StorSimple

Cloudové snímky StorSimple chránit hello data, která se nachází v zařízení StorSimple. Vytvoření snímku cloudu je ekvivalentní tooshipping místní záložní pásky tooan mimo pracoviště zařízení. Pokud používáte Azure geograficky redundantní úložiště, vytvoření cloudový snímek je ekvivalentní tooshipping záložní pásky toomultiple lokalit. Pokud potřebujete toorestore zařízení po havárii, můžete převést online jiná zařízení StorSimple a selhání. Po převzetí služeb při selhání hello bude možné tooaccess hello dat (cloudu rychlostmi) z hello nejnovější cloudový snímek.

Hello následující část popisuje, jak toocreate toostart krátké skriptu a odstranění zařízení StorSimple cloudové snímky během zálohování po zpracování.

> [!NOTE]
> Snímky vytvořené ručně nebo z programu nepostupujte podle zásad vypršení platnosti snímku StorSimple hello. Tyto snímky musíte ručně nebo z programu smazat.

### <a name="start-and-delete-cloud-snapshots-by-using-a-script"></a>Spuštění a odstraňte snímky cloudu pomocí skriptu

> [!NOTE]
> Pečlivě vyhodnoťte hello dodržování předpisů a dopady uchování dat před odstraněním snímku StorSimple. Další informace o tom, toorun pozálohovací skript, najdete v části hello [zálohování Exec dokumentaci](https://www.veritas.com/support/en_US/15047.html).

### <a name="backup-lifecycle"></a>Zálohování životního cyklu

![Zálohování diagram životního cyklu](./media/storsimple-configure-backup-target-using-backup-exec/backuplifecycle.png)

### <a name="requirements"></a>Požadavky

-   server Hello, která spouští skript hello musí mít přístup k prostředkům cloudu tooAzure.
-   Hello uživatelský účet musí mít potřebná oprávnění hello.
-   Zásady zálohování StorSimple s hello související StorSimple svazky musí být nastaven, ale není zapnutá.
-   Budete potřebovat hello název prostředku StorSimple, registrační klíč, název zařízení a ID zásady zálohování.

### <a name="toostart-or-delete-a-cloud-snapshot"></a>toostart nebo odstranění snímku cloudu

1.  [Nainstalujte prostředí Azure PowerShell](/powershell/azure/overview).
2.  [Stahování a import nastavení a informace o předplatném publikovat](https://msdn.microsoft.com/library/dn385850.aspx).
3.  V hello portál Azure classic, získat název prostředku hello a [registračního klíče služby StorSimple Manager](storsimple-deployment-walkthrough-u2.md#step-2-get-the-service-registration-key).
4.  Na server hello, který spouští hello skriptu spusťte prostředí PowerShell jako správce. Zadejte tento příkaz:

    `Get-AzureStorSimpleDeviceBackupPolicy –DeviceName <device name>`

    ID Poznámka hello zásady zálohování.
5.  V poznámkovém bloku vytvořte nový skript prostředí PowerShell pomocí hello následující kód.

    Zkopírujte a vložte tento fragment kódu:
    ```powershell
    Import-AzurePublishSettingsFile "c:\\CloudSnapshot Snapshot\\myAzureSettings.publishsettings"
    Disable-AzureDataCollection
    $ApplianceName = <myStorSimpleApplianceName>
    $RetentionInDays = 20
    $RetentionInDays = -$RetentionInDays
    $Today = Get-Date
    $ExpirationDate = $Today.AddDays($RetentionInDays)
    Select-AzureStorSimpleResource -ResourceName "myResource" –RegistrationKey
    Start-AzureStorSimpleDeviceBackupJob –DeviceName $ApplianceName -BackupType CloudSnapshot -BackupPolicyId <BackupId> -Verbose
    $CompletedSnapshots =@()
    $CompletedSnapshots = Get-AzureStorSimpleDeviceBackup -DeviceName $ApplianceName
    Write-Host "hello Expiration date is " $ExpirationDate
    Write-Host

    ForEach ($SnapShot in $CompletedSnapshots)
    {
        $SnapshotStartTimeStamp = $Snapshot.CreatedOn
        if ($SnapshotStartTimeStamp -lt $ExpirationDate)

        {
            $SnapShotInstanceID = $SnapShot.InstanceId
            Write-Host "This snpashotdate was created on " $SnapshotStartTimeStamp.Date.ToShortDateString()
            Write-Host "Instance ID " $SnapShotInstanceID
            Write-Host "This snpashotdate is older and needs toobe deleted"
            Write-host "\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#"
            Remove-AzureStorSimpleDeviceBackup -DeviceName $ApplianceName -BackupId $SnapShotInstanceID -Force -Verbose
        }
    }
    ```
      Uložit toohello skript prostředí PowerShell hello se nastavení publikování stejné umístění, kam jste uložili vašeho Azure. Například uložte jako C:\CloudSnapshot\StorSimpleCloudSnapshot.ps1.
6.  Přidáte úloha zálohování tooyour hello skript v zálohování Exec úpravy možnosti zálohování Exec – úloha zpracování před a po zpracování příkazů.

    ![Zálohování Exec konzoly, možnosti zálohování, karta před a po zpracování příkazů](./media/storsimple-configure-backup-target-using-backup-exec/image25.png)

> [!NOTE]
> Doporučujeme vám spustit zásady zálohování StorSimple cloudových snímků jako skript následného zpracování na konci hello každodenní úlohy zálohování. Další informace o tom, jak tooback zálohu a obnovení toohelp prostředí vaší aplikace pro zálohování splňujete plánovaný bod obnovení a RTO, obraťte se na vaše zálohování architekti.

## <a name="storsimple-as-a-restore-source"></a>StorSimple jako zdroj obnovení

Obnoví z pracovní zařízení StorSimple jako obnovení ze všech zařízení s blokovým úložištěm. Obnovení dat, která je vrstvené toohello cloudu nastane v cloudu rychlosti. Pro místní data dojde k obnovení rychlostí místního disku hello hello zařízení. Informace o tooperform obnovení, naleznete v dokumentaci k zálohování Exec hello. Doporučujeme, abyste shodu tooBackup Exec obnovení osvědčené postupy.

## <a name="storsimple-failover-and-disaster-recovery"></a>StorSimple převzetí služeb při selhání a zotavení po havárii

> [!NOTE]
> Pro scénáře cíl zálohy zařízení StorSimple cloudu nepodporuje jako cíl obnovení.

Havárie může být způsobeno různých faktorech. Hello následující tabulka obsahuje seznam běžných scénářů zotavení po havárii.

| Scénář | Dopad | Jak toorecover | Poznámky |
|---|---|---|---|
| Selhání zařízení StorSimple | Operace zálohování a obnovení jsou přerušena. | Nahraďte hello selhání zařízení a provádět [StorSimple převzetí služeb při selhání a zotavení po havárii](storsimple-device-failover-disaster-recovery.md). | Pokud potřebujete tooperform obnovení po obnovení zařízení, úplné datové sady pracovní se načítají z hello cloud toohello nové zařízení. Všechny operace jsou rychlostmi cloudu. Hello indexování a katalogizaci opětovná kontrola procesu může způsobit všechny zálohovací sklady toobe kontroloval a načtený z hello cloudovou vrstvu toohello místní zařízení, na vrstvu, což může být zdlouhavý proces. |
| Selhání zálohování serveru Exec | Operace zálohování a obnovení jsou přerušena. | Znovu sestavit server hello zálohování a obnovení databáze podle popisu v [jak toodo databázi ruční zálohování a obnovení ze zálohy Exec (BEDB)](http://www.veritas.com/docs/000041083). | Musíte znovu sestavit nebo obnovit hello Exec zálohování serveru v lokalitě pro obnovení po havárii hello. Hello databáze toohello nejnovější bod obnovení. Pokud hello obnovit zálohování Exec databáze není synchronizované s vaší nejnovější úlohy zálohování, indexování a do katalogu je povinný. Tento index a katalog opětovná kontrola procesu může dojít k všechny zálohovací sklady toobe kontroloval a vyžádat z hello cloudu vrstvy toohello místní zařízení vrstvy. Díky tomu další časově náročný. |
| Selhání lokality, který vede ke ztrátě hello hello zálohování serveru a StorSimple | Operace zálohování a obnovení jsou přerušena. | Nejprve obnovit StorSimple a potom obnovte Exec zálohování. | Nejprve obnovit StorSimple a potom obnovte Exec zálohování. Pokud potřebujete tooperform obnovení po obnovení zařízení, hello úplné datové pracovní sady se načítají z hello cloud toohello nové zařízení. Všechny operace jsou rychlostmi cloudu. |

## <a name="references"></a>Odkazy

Hello následující dokumenty odkazují pro v tomto článku:

- [Funkce multipath vstupně-výstupní operace instalace zařízení StorSimple](storsimple-configure-mpio-windows-server.md)
- [Scénáře úložiště: dynamické zajišťování](http://msdn.microsoft.com/library/windows/hardware/dn265487.aspx)
- [Pomocí GPT jednotky](http://msdn.microsoft.com/windows/hardware/gg463524.aspx#EHD)
- [Nastavení stínových kopií pro sdílené složky](http://technet.microsoft.com/library/cc771893.aspx)

## <a name="next-steps"></a>Další kroky

- Další informace o příliš[obnovení ze zálohovacího skladu](storsimple-restore-from-backup-set-u2.md).
- Další informace o tooperform [zařízení převzetí služeb při selhání a zotavení po havárii](storsimple-device-failover-disaster-recovery.md).
