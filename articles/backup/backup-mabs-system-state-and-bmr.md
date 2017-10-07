---
title: "aaaAzure zálohovat Server chrání stav systému a obnoví toobare operačního systému | Microsoft Docs"
description: "Pomocí serveru Azure Backup tooback zálohu stavu systému a zajištění ochrany úplné obnovení systému (BMR)."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
keywords: 
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.targetplatform: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: markgal,masaran
ms.openlocfilehash: d34c8bbdc7cc24c905f81ceaf199698c1ee923db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-system-state-and-restore-toobare-metal-with-azure-backup-server"></a>Zálohování stavu systému a obnovení toobare operačního systému pomocí serveru Azure Backup

Azure Backup Server zálohuje stav systému a poskytuje ochranu úplné obnovení systému (BMR).

*   **Zálohování stavu systému**: zálohuje soubory operačního systému, takže můžete obnovit, pokud se počítač spustí, ale systémové soubory a hello registry budou ztraceny. Zahrnuje zálohu stavu systému:
    * Člen domény: spouštěcí soubory, registrační databáze třídy modelu COM +, registru
    * Řadič domény: Windows Server Active Directory (NTDS), spouštěcí soubory, registrační databáze třídy modelu COM +, registru, systémový svazek (SYSVOL)
    * Počítač, který spouští služby clusteru: metadata clusterového serveru
    * Počítač, který spouští služby certificate services: certifikát dat
* **Úplné zálohování**: zálohuje soubory operačního systému a všechna data na nepostradatelné svazky (s výjimkou uživatelských dat). Podle definice zálohování BMR zahrnuje zálohu stavu systému. Když počítač se nespustí a mít toorecover všechno, co poskytuje ochranu.

Hello následující tabulka shrnuje, co můžete zálohovat a obnovit. Podrobné informace o verzích aplikace, které se dají chránit pomocí stavu systému a úplné obnovení systému najdete v tématu [jaké serveru Azure Backup zálohuje?](backup-mabs-protection-matrix.md).

|Zálohování|Problém|Obnovení ze zálohy Azure Backup Server|Obnovení ze zálohy stavu systému|ÚPLNÉ OBNOVENÍ SYSTÉMU|
|----------|---------|---------------------------|------------------------------------|-------|
|**Data souborů**<br /><br />Zálohování regulární dat<br /><br />Zálohu BMR nebo stavu|Soubor ke ztrátě dat|Ano|N|N|
|**Data souborů**<br /><br />Azure Backup Server zálohování dat souborů<br /><br />Zálohu BMR nebo stavu|Ztraceného nebo poškozeného operačního systému|N|Ano|Ano|
|**Data souborů**<br /><br />Azure Backup Server zálohování dat souborů<br /><br />Zálohu BMR nebo stavu|Ztráta serveru (datové svazky v pořádku)|N|N|Ano|
|**Data souborů**<br /><br />Azure Backup Server zálohování dat souborů<br /><br />Zálohu BMR nebo stavu|Ztráta serveru (datové svazky ztraceny)|Ano|Ne|Ano (BMR následované pravidelným obnovením zálohovaných souborová data)|
|**Data služby SharePoint**:<br /><br />Zálohování Azure záložní Server farmy dat<br /><br />Zálohu BMR nebo stavu|Ke ztrátě lokality, seznamy, položky seznamu, dokumenty|Ano|N|N|
|**Data služby SharePoint**:<br /><br />Zálohování Azure záložní Server farmy dat<br /><br />Zálohu BMR nebo stavu|Ztraceného nebo poškozeného operačního systému|N|Ano|Ano|
|**Data služby SharePoint**:<br /><br />Zálohování Azure záložní Server farmy dat<br /><br />Zálohu BMR nebo stavu|Zotavení po havárii|N|N|N|
|Windows Server 2012 R2 Hyper-V<br /><br />Zálohování Azure zálohování serveru hostitele Hyper-V nebo hosta<br /><br />Zálohování BMR nebo systému stav hostitele|Ke ztrátě virtuálních počítačů|Ano|N|N|
|Hyper-V<br /><br />Zálohování Azure zálohování serveru hostitele Hyper-V nebo hosta<br /><br />Zálohování BMR nebo systému stav hostitele|Ztraceného nebo poškozeného operačního systému|N|Ano|Ano|
|Hyper-V<br /><br />Zálohování Azure zálohování serveru hostitele Hyper-V nebo hosta<br /><br />Zálohování BMR nebo systému stav hostitele|Ke ztrátě hostitele Hyper-V (virtuálních počítačů beze změn)|N|N|Ano|
|Hyper-V<br /><br />Zálohování Azure zálohování serveru hostitele Hyper-V nebo hosta<br /><br />Zálohování BMR nebo systému stav hostitele|Ke ztrátě hostitele Hyper-V (virtuální počítače ke ztrátě)|N|N|Ano<br /><br />BMR, následované pravidelným obnovením serveru Azure Backup|
|SQL Server nebo Exchange<br /><br />Azure zálohování aplikace Zálohování serveru<br /><br />Zálohu BMR nebo stavu|Data ke ztrátě aplikací|Ano|N|N|
|SQL Server nebo Exchange<br /><br />Azure zálohování aplikace Zálohování serveru<br /><br />Zálohu BMR nebo stavu|Ztraceného nebo poškozeného operačního systému|N|Y|Ano|
|SQL Server nebo Exchange<br /><br />Azure zálohování aplikace Zálohování serveru<br /><br />Zálohu BMR nebo stavu|Ztráta serveru (protokoly databáze nebo transakcí beze změn)|N|N|Ano|
|SQL Server nebo Exchange<br /><br />Azure zálohování aplikace Zálohování serveru<br /><br />Zálohu BMR nebo stavu|Ztráta serveru (protokoly transakcí databáze nebo ke ztrátě)|N|N|Ano<br /><br />Obnovení BMR následované pravidelným obnovením serveru Azure Backup|

## <a name="how-system-state-backup-works"></a>Jak funguje zálohování stavu systému

Při spuštění zálohy stavu systému, Backup Server komunikuje se zálohování serveru toorequest zálohu stavu systému hello server. Ve výchozím nastavení používají hello jednotku, která má nejvíce dostupného volného místa hello zálohování serveru a zálohování serveru. Informace o této jednotky se ukládají do hello soubor PSDataSourceConfig.xml. Toto je hello jednotku, která používá zálohování serveru k zálohování.

Můžete přizpůsobit hello jednotku, která používá záložní Server pro zálohování stavu systému hello. Na serveru hello chráněné přejděte tooC:\Program Files\Microsoft Manager\MABS\Datasources ochrany dat. Otevřete soubor PSDataSourceConfig.xml hello pro úpravy. Změna hello \<FilesToProtect\> hodnotu hello písmeno jednotky. Uložte a zavřete soubor hello. Pokud je skupina ochrany nastavit stav systému hello tooprotect hello počítače, spusťte kontrolu konzistence. Pokud je vygenerována výstraha, vyberte **změnit chráněnou skupinu** v hello výstrahy a pak dokončení hello průvodce. Pak spusťte další kontrolu konzistence.

Všimněte si, že pokud hello ochrany serveru je v clusteru, je možné, že disk klastru bude zvolen jako hello jednotku s hello nejvíce volného místa. Pokud změně vlastnictví této jednotky byl vypnuté tooanother uzlu a spustí zálohování stavu systému, jednotka hello není k dispozici a hello zálohování se nezdaří. V tomto scénáři upravte soubor PSDataSourceConfig.xml toopoint tooa místní disk.

V dalším kroku zálohování serveru vytvoří složku s názvem WindowsImageBackup v kořenovém hello hello obnovení složky. Jako Zálohování serveru vytvoří hello zálohování, všechna data hello je umístěn v této složce. Po dokončení zálohování hello hello soubor je počítač přenášená toohello zálohování serveru. Všimněte si hello následující informace:

* Tato složka a její obsah nejsou vyčistit až po dokončení zálohování hello nebo přenos. Hello nejlepší způsob, jak toothink tohoto objektu je, že hello dochází k rezervaci prostoru pro hello dalším spuštění zálohy je dokončena.
* Hello složka se vytvoří pokaždé, když se provádí zálohu. razítko času a data Hello projeví hello čas poslední zálohy stavu systému.

## <a name="bmr-backup"></a>Zálohování BMR

Pro BMR (včetně zálohu stavu systému) úloha zálohování hello uložen přímo tooa sdílené složky v počítači zálohovat Server hello. Složka tooa není uloženo na serveru hello chráněný.

Zálohování serveru volá zálohování serveru a sdílených složek na hello svazek repliky pro tuto zálohu BMR. V takovém případě neříká zálohování serveru toouse hello jednotku s hello nejvíce volného místa. Místo toho používá hello sdílenou složku, která byla vytvořena pro úlohu hello.

Po dokončení zálohování hello hello soubor je počítač přenášená toohello zálohování serveru. Protokoly se ukládají do C:\Windows\Logs\WindowsServerBackup.

## <a name="prerequisites-and-limitations"></a>Požadavky a omezení

-   Úplné obnovení systému není podporována pro počítače se systémem Windows Server 2003 nebo pro počítače s operačním systémem klienta.

-   Nelze chránit BMR a stavu systému pro hello stejné počítače ve různých ochranných skupinách.

-   Zálohování serveru počítač nemůže chránit sám sebe pro BMR.

-   Krátkodobé ochrany tootape (disk na pásku nebo D2T) není u BMR podporována. Dlouhodobé úložiště tootape (disku na disk na pásku nebo D2D2T) je podporováno.

-   Pro ochranu BMR zálohování serveru musí být nainstalován v počítači hello chráněný.

-   Pro ochranu BMR na rozdíl od ochrany stavu systému, zálohovat Server nemá žádné požadavky na místo na hello chráněné počítače. Zálohování serveru přímo převádí počítači zálohy toohello zálohování serveru. Úloha zálohování přenosu Hello se nezobrazí v hello zálohování serveru **úlohy** zobrazení.

-   Zálohování serveru rezervuje pro BMR 30 GB místa na svazku repliky hello. Toto můžete změnit na hello **přidělení disku** stránky na hello Průvodce změnou skupiny ochrany nebo pomocí rutiny Get-DatasourceDiskAllocation a Set-DatasourceDiskAllocation prostředí PowerShell hello. Na svazku bodu obnovení hello vyžaduje ochrana BMR přibližně 6 GB pro uchování 5 dní.
    * Všimněte si, že nemůžete snížit hello repliky svazek velikost tooless než 15 GB.
    * Zálohování serveru nepočítá velikost zdroje dat BMR hello hello. Předpokládá 30 GB pro všechny servery. Změňte hodnotu hello podle hello velikosti záloh BMR, které byste očekávali ve vašem prostředí. Hello velikost zálohy BMR může být přibližně počítá jako hello součet využitého místa na všech klíčových svazcích. Nepostradatelné svazky = spouštěcí svazek + systémový svazek + svazek, který je hostitelem dat stavu systému, jako je Active Directory.

-   Pokud změníte z ochrany tooBMR ochrany stavu systému, vyžaduje ochrana BMR méně místa na hello *svazek bodu obnovení*. Ale hello přebytečné místo na svazku hello neuvolní. Můžete ručně zmenšit velikost svazku hello na hello **upravit přidělení disku** hello Průvodce změnou skupiny ochrany nebo pomocí rutiny Get-DatasourceDiskAllocation a Set-DatasourceDiskAllocation prostředí PowerShell hello.

    Pokud změníte z ochrany tooBMR ochrany stavu systému, ochrana BMR vyžadovat další místo na hello *svazek repliky*. svazek Hello je automaticky rozšířena. Pokud chcete toochange hello výchozí přidělení prostoru, použijte rutinu prostředí PowerShell Modify-DiskAllocation hello.

-   Pokud změníte z ochrany stavu toosystem ochranu BMR, je třeba více místa na svazku bodu obnovení hello. Zálohování serveru mohou zkuste tooautomatically zvýšení hello svazku. Pokud není dostatek místa ve fondu úložiště hello, dojde k chybě.

    Pokud změníte z ochrany stavu toosystem ochranu BMR, musíte místo na hello chráněné počítače. Je to proto ochrany stavu systému nejprve zapíše hello repliky toohello místního počítače a pak ji přesune počítači toohello zálohování serveru.

## <a name="before-you-begin"></a>Než začnete

1.  **Nasadit Azure Backup Server**. Ověřte, zda je správně nasazeny zálohování serveru. Další informace naleznete v tématu:
    * [Požadavky na systém pro Azure Backup Server](http://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites)
    * [Matice ochrany zálohování serveru](backup-mabs-protection-matrix.md)

2.  **Nastavení úložiště**. Zálohovaná data můžete ukládat na disk na pásku a v cloudu hello s Azure. Další informace najdete v tématu [Příprava úložiště dat](https://docs.microsoft.com/system-center/dpm/plan-long-and-short-term-data-storage).

3.  **Nastavte agenta ochrany hello**. Nainstalujte agenta ochrany hello hello počítače, který chcete tooback nahoru. Další informace najdete v tématu [agenta ochrany aplikace DPM nasadit hello](http://docs.microsoft.com/system-center/dpm/deploy-dpm-protection-agent).

## <a name="back-up-system-state-and-bare-metal"></a>Zálohování stavu systému a úplné obnovení
Nastavit skupinu ochrany, jak je popsáno v [nasazení skupin ochrany](http://docs.microsoft.com/system-center/dpm/create-dpm-protection-groups). Všimněte si, že nemůže chránit BMR a stavu systému pro hello stejný počítač v různých skupinách. Navíc když vyberete BMR, stav systému je automaticky povolené.


1.  Průvodce vytvořením nové skupiny ochrany hello tooopen v konzole pro správu zálohování serveru, vyberte hello **ochrany** > **akce** > **vytvoření ochrany Skupiny**.

2.  Na hello **vybrat typ skupiny ochrany** vyberte **servery**a potom vyberte **Další**.

3.  Na hello **vybrat členy skupiny** stránky, rozbalte položku hello počítače a pak vyberte buď **BMR** nebo **stav systému**.

    Mějte na paměti, že nemůžete chránit jak BMR a stav systému pro hello stejný počítač v různých skupinách. Navíc když vyberete BMR, stav systému je automaticky povolené. Další informace najdete v tématu [nasazení skupin ochrany](http://docs.microsoft.com/system-center/dpm/create-dpm-protection-groups).

4.  Na hello **vyberte způsob ochrany dat** vyberte, jakým způsobem chcete toohandle krátkodobé a dlouhodobé zálohování. Krátkodobé zálohování je vždy toodisk nejprve, s možností hello zálohování z disku toohello hello Azure v cloudu pomocí Azure Backup (krátkodobá nebo dlouhodobá). Zálohování toohello cloud alternativní toolong termín je tooset až dlouhodobé zálohování tooa samostatnou pásku pásek nebo zařízení knihovny, která připojil tooBackup serveru.

5.  Na hello **vybrat krátkodobé cíle** vyberte, jakým způsobem chcete tooback tooshort termín úložiště na disku:
    1. Pro **rozsah uchování**, vyberte, jak dlouho chcete tookeep hello data na disku. 
    2. Pro **četnost synchronizace**, vyberte, jak často chcete toorun toodisk přírůstkové zálohování. Pokud nechcete, aby tooset intervalu zálohování, můžete zkontrolovat hello **těsně před bodem obnovení** možnost. Zálohování serveru se spustí expresní úplné zálohování těsně před každým bodem obnovení je naplánováno.

6.  Pokud chcete toostore data na pásku pro dlouhodobé ukládání na hello **zadat dlouhodobé cíle** vyberte, jak dlouho chcete data na pásce tookeep (1 – 99 let). 
    1. Pro **četnost záloh**, vyberte, jak často se má spustit zálohování tootape. frekvence Hello je založena na hello rozsahu uchování, kterou jste vybrali:
        * Když je rozsah uchování hello 1 – 99 let, můžete vybrat zálohování toooccur denní, týdenní, jednou za dva týdny, měsíčně, čtvrtletně, půl roku nebo ročně.
        * Když je rozsah uchování hello 1 – 11 měsíců, můžete vybrat toooccur zálohování denně, týdně, jednou za dva týdny nebo měsíčně.
        * Když je rozsah uchování hello 1 – 4 týdny, můžete vybrat zálohování toooccur den nebo každý týden.

    2. Na hello **vyberte pásku a podrobnosti ke knihovně** stránky, vyberte hello toouse pásky a knihovny, a zda má komprimovaná a šifrovaná data.

7.  Na hello **zkontrolovat přidělení disku** si prohlédněte hello disku místa ve fondu úložiště přidělené pro skupinu ochrany hello.

    1. **Celková velikost dat** je hello velikost hello data, která chcete tooback nahoru.
    2. **Toobe místa na disku zřízena na serveru Azure Backup** hello místa, která doporučuje zálohovat Server pro skupinu ochrany hello. Zálohování serveru zvolí hello ideální zálohování svazku na základě nastavení hello. Však můžete upravit možnosti zálohování svazku hello v **podrobnosti přidělení disku**. 
    3. Pro úlohy vyberte v rozevírací nabídce hello hello preferované úložiště. Vaše úpravy změnit hodnoty hello **celkové úložiště** a **volného** v hello **úložiště k dispozici disku** podokně. Underprovisioned prostor je hello velikost úložiště, zálohovat Server naznačuje, že přidáte toohello svazku, tooensure smooth zálohy.

8.  Na hello **vyberte způsob vytvoření repliky** vyberte, jakým způsobem chcete toohandle hello počáteční úplná data replikace. Pokud si zvolíte tooreplicate přes síť hello, doporučujeme, abyste zvolili dobu mimo špičku. Pro velké objemy dat nebo síťové podmínky, které jsou menší než optimální zvažte replikaci hello dat offline pomocí vyměnitelného média.

9. Na hello **zvolte možnosti kontroly konzistence** vyberte, jakým způsobem chcete tooautomate kontroly konzistence. Můžete zvolit toorun kontrolu pouze v případě, že data repliky se stane nekonzistentní, nebo podle plánu. Pokud nechcete, aby tooconfigure automatické kontroly konzistence, můžete kdykoli spustit ruční kontrolu. toorun ruční kontrolu, v hello **ochrany** oblasti hello konzoly Správce serveru zálohování, klikněte pravým tlačítkem na hello ochranu skupiny a pak vyberte **provést kontrolu konzistence**.

10. Pokud jste vybrali tooback až toohello cloudu pomocí Azure Backup na hello **zadat Data Online ochrany** stránky, ujistěte se, že vyberete zatížení hello chcete tooback až tooAzure.

11. Na hello **zadejte plán Online zálohování** vyberte frekvenci přírůstkových záloh tooAzure dojde. Můžete naplánovat zálohování toorun každý den, týden, měsíc a rok a vyberte hello čas a datum, na kterých by měly být spuštěny. Zálohování může dojít, až tootwice denně. Pokaždé, když spuštění zálohování dat vytvoří bod obnovení je v Azure z kopie hello hello zálohovaná data uložená na disku hello zálohování serveru.

12. Na hello **zadejte zásady uchovávání Online** vyberte, jak v Azure jsou uchovány hello body obnovení, které jsou vytvořené pomocí hello denní, týdenní, měsíční a roční zálohy.

13. Na hello **výběr Online replikace** vyberte, jak proběhne počáteční úplná replikace dat hello. Můžete replikovat přes síť hello nebo proveďte offline zálohování (offline synchronizace replik indexů). Zálohování offline používá hello funkce Import úložiště Azure. Další informace najdete v tématu [pracovní postup Offline zálohování v Azure Backup](backup-azure-backup-import-export.md).

14. Na hello **Souhrn** stránka, zkontrolujte nastavení. Po výběru **vytvořit skupinu**, dojde k počáteční replikaci dat hello. Při replikaci dat skončí u hello **stav** stránky, je stav skupiny ochrany hello **OK**. Potom se provede na hello ochrany nastavení skupiny.

## <a name="recover-system-state-or-bmr"></a>Obnovení stavu systému nebo úplné obnovení systému
Můžete obnovit BMR nebo systému stavu tooa umístění v síti. Pokud jste zálohovali BMR, použijte prostředí Windows Recovery Environment (WinRE) toostart systému a připojte ho toohello sítě. Pak pomocí zálohování serveru toorecover z umístění v síti hello. Pokud jste zálohovali stav systému, použijte z umístění v síti hello toorecover zálohování serveru.

### <a name="restore-bmr"></a>Obnovit BMR
Spusťte obnovení v počítači zálohovat Server hello:

1.  V hello **obnovení** podokně, najít hello počítače chcete toorecover a potom vyberte **úplné obnovení systému**.

2.  Dostupné body obnovení jsou vypsány v kalendáři hello tučně. Vyberte hello datum a čas pro hello bod obnovení, které chcete toouse.

3.  Na hello **vybrat typ obnovení** vyberte **kopie tooa síťové složky.**

4.  Na hello **zadejte cíl** vyberte, kam chcete toocopy hello data. Pamatujte na to, že vybrané cílové hello musí toohave dostatek místa. Doporučujeme vám, že vytvoříte novou složku.

5.  Na hello **zadat možnosti obnovení** stránky, tooapply nastavení zabezpečení vyberte hello. Potom vyberte, zda má (SAN) toouse storage area network – na základě snímků hardwaru pro rychlejší obnovení. (To je možnost jenom v případě, že máte síť SAN pomocí této funkce, které jsou k dispozici a hello možnost toocreate a rozdělení toomake klonování ho s možností zápisu. In addition, hello chráněného počítače a počítač zálohování serveru musí být připojené toohello stejné síti.)

6.  Nastavte možnosti oznámení. Na hello **potvrzení** vyberte **obnovit**.

Nastavte umístění sdílené složky hello:

1.  V umístění pro obnovení hello přejděte toohello složka, která má hello zálohování.

2.  Sdílenou složku hello, která je jednu úroveň nad souborem WindowsImageBackup tak, aby hello kořenovém hello sdílené složky je složka WindowsImageBackup hello. Pokud to neuděláte, obnovení nebude možné najít hello zálohování. tooconnect pomocí prostředí Windows Recovery Environment (WinRE), je nutné sdílenou složku, která se zobrazí v prostředí WinRE se hello správnou IP adresu a přihlašovací údaje.

Obnovení systému hello:

1.  Spustit hello počítač, na kterém chcete toorestore hello bitovou kopii pomocí disku DVD systému Windows hello pro systém hello obnovujete.

2.  Na první stránku hello ověřte nastavení jazyka a národního prostředí. Na hello **nainstalovat** vyberte **opravit tento počítač**.

3.  Na hello **možnosti obnovení systému** vyberte **obnovení počítače pomocí bitové kopie systému, který jste vytvořili dříve**.

4.  Na hello **výběr zálohy bitové kopie systému** vyberte **Vybrat bitovou kopii systému** > **Upřesnit** > **vyhledávání pro systém bitové kopie v síti hello**. Pokud se zobrazí upozornění, vyberte **Ano**. Přejděte toohello cesta ke sdílené složce, zadejte přihlašovací údaje hello a pak vyberte bod obnovení hello. Tato funkce proskenuje pro konkrétní zálohování, které jsou k dispozici v daném bodě obnovení. Vyberte bod obnovení hello, které chcete toouse.

5.  Na hello **zvolte, jak hello zálohování toorestore** vyberte **formátovat a znovu rozdělit disky**. Na další stránku hello ověřte nastavení. 

6.  toobegin hello obnovení, vyberte **Dokončit**. Je vyžadováno restartování.

### <a name="restore-system-state"></a>Obnovení stavu systému

Spuštění obnovení zálohování serveru:

1.  V hello **obnovení** podokně najít hello počítače chcete toorecover a pak vyberte **úplné obnovení systému**.

2.  Dostupné body obnovení jsou vypsány v kalendáři hello tučně. Vyberte hello datum a čas pro hello bod obnovení, které chcete toouse.

3.  Na hello **vybrat typ obnovení** vyberte **kopie tooa síťové složky**.

4.  Na hello **zadejte cíl** vyberte, kam chcete toocopy hello data. Mějte na paměti, že dané hello vybrané cílové umístění je dostatek místa. Doporučujeme vám, že vytvoříte novou složku.

5.  Na hello **zadat možnosti obnovení** stránky, tooapply nastavení zabezpečení vyberte hello. Potom vyberte, zda chcete použít snímky hardwaru založené na síti SAN toouse pro rychlejší obnovení. (To je možnost jenom v případě, že máte síť SAN pomocí této funkce a hello možnost toocreate a rozdělení toomake klonování ho s možností zápisu. In addition, hello chráněný počítač a zálohování serveru musí být připojené toohello stejné síti.)

6.  Nastavte možnosti oznámení. Na hello **potvrzení** vyberte **obnovit**.

Spusťte zálohování serveru:

1.  Vyberte **akce** > **obnovit** > **tento Server** > **Další**.

2.  Vyberte **jiný Server**, vyberte hello **zadat typ umístění** a pak vyberte **vzdálené sdílené složce**. Zadejte hello cesta toohello složky, která obsahuje bod obnovení hello.

3.  Na hello **vybrat typ obnovení** vyberte **stav systému**. 

4. Na hello **vyberte umístění pro obnovení stavu systému** vyberte **původního umístění**.

5.  Na hello **potvrzení** vyberte **obnovit**. Po obnovení hello restartujte hello server.

6.  Obnovení stavu systému hello také můžete spustit na příkazovém řádku. toodo se spustit zálohování Windows serveru na počítači hello chcete toorecover. identifikátor verze hello tooget, na příkazovém řádku, zadejte:```wbadmin get versions -backuptarget \<servername\sharename\>```

    Použijte obnovení stavu systému toostart hello verze identifikátor hello. Hello příkazového řádku zadejte:```wbadmin start systemstaterecovery -version:<versionidentified> -backuptarget:<servername\sharename>```

    Potvrďte, že chcete toostart hello obnovení. Můžete zobrazit hello procesu v okně příkazového řádku hello. Vytvoří se protokol obnovení. Po obnovení hello restartujte hello server.

