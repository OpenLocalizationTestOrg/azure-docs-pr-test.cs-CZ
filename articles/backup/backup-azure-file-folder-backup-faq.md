---
title: "agent zálohování aaaAzure – nejčastější dotazy | Microsoft Docs"
description: "Odpovědi toocommon otázky týkající se: jak hello omezení funguje, zálohování a uchovávání Azure backup agent."
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
keywords: "zálohování a zotavení po havárii; služba zálohování"
ms.assetid: 778c6ccf-3e57-4103-a022-367cc60c411a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/18/2017
ms.author: trinadhk;pullabhk;
ms.openlocfilehash: bdefb4efb39301f38cdf692bdc93c841a2bbb441
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="questions-about-hello-azure-backup-agent"></a>Dotazy týkající se agenta Azure Backup hello
Tento článek obsahuje odpovědi toocommon otázky toohelp rychle pochopit součásti hello Azure Backup agent. V některých hello odpovědi jsou články toohello odkazy, které mají komplexní informace. Také můžete pokládat dotazy týkající se služby Azure Backup hello v hello [diskusní fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="configure-backup"></a>Konfigurace zálohování
### <a name="where-can-i-download-hello-latest-azure-backup-agent-br"></a>Kde lze stáhnout hello nejnovější verze agenta Azure Backup? <br/>
Si můžete stáhnout nejnovější verzi agenta hello k zálohování systému Windows Server, System Center DPM nebo klienta Windows z [zde](http://aka.ms/azurebackup_agent). Pokud chcete tooback virtuální počítač, použijte hello agenta virtuálního počítače (který automaticky nainstaluje správné rozšíření hello). Hello agenta virtuálního počítače je již nainstalován na virtuálních počítačích vytvořených z Galerie Azure hello.

### <a name="when-configuring-hello-azure-backup-agent-i-am-prompted-tooenter-hello-vault-credentials-do-vault-credentials-expire"></a>Při konfiguraci agenta Azure Backup hello, jsem přihlašovací údaje trezoru výzvami tooenter hello. Mohou přihlašovací údaje trezoru vypršet?
Ano, hello přihlašovací údaje trezoru vyprší po 48 hodinách. Pokud hello soubor vyprší, soubory protokolu jsou v toohello Azure portál a stahování hello trezoru přihlašovací údaje z trezoru.

### <a name="what-types-of-drives-can-i-back-up-files-and-folders-from-br"></a>Z jakých typů jednotek můžu zálohovat soubory a složky? <br/>
Hello následující jednotky/svazky nelze zálohovat:

* Vyměnitelné médium: Všechny zdroje položek k zálohování se musí hlásit jako pevné.
* Svazky jen pro čtení: svazek hello musí být zapisovatelný hello svazku stínové kopie (svazku VSS) služby toofunction.
* Offline svazky: svazek hello musí být online pro toofunction služby Stínová kopie svazku.
* Síťové sdílené složky: svazek hello musí být místní toohello server toobe zálohovat pomocí online zálohování.
* Svazky chráněné nástrojem BitLocker: musí být hello svazek odemčený, než může dojít k zálohování hello.
* Identifikace systému souborů: Systém souborů NTFS je hello jediným podporovaným systémem souborů.

### <a name="what-file-and-folder-types-can-i-back-up-from-my-serverbr"></a>Jaké typy souborů a složek mohu zálohovat ze svého serveru?<br/>
jsou podporovány následující typy Hello:

* Šifrované
* Komprimované
* Řídké
* Komprimované a řídké
* Pevné odkazy: Není podporováno, vynecháno
* Bod rozboru: Není podporováno, vynecháno
* Šifrované a řídké: Není podporováno, vynecháno
* Komprimovaný datový proud: Není podporováno, vynecháno
* Řídký datový proud: Není podporováno, vynecháno

### <a name="can-i-install-hello-azure-backup-agent-on-an-azure-vm-already-backed-by-hello-azure-backup-service-using-hello-vm-extension-br"></a>Můžete nainstalovat agenta Azure Backup hello na virtuální počítač Azure už je zálohovaný službou Azure Backup hello pomocí hello rozšíření virtuálního počítače? <br/>
Jistě. Azure Backup poskytuje zálohování na úrovni virtuálních počítačů pro virtuální počítače Azure pomocí rozšíření virtuálního počítače hello. tooprotect soubory a složky na hello hostovaného operačního systému Windows, nainstalujte agenta Azure Backup hello na hello hostovaného operačního systému Windows.

### <a name="can-i-install-hello-azure-backup-agent-on-an-azure-vm-tooback-up-files-and-folders-present-on-temporary-storage-provided-by-hello-azure-vm-br"></a>Můžete nainstalovat agenta Azure Backup hello na tooback virtuálního počítače Azure se souborů a složek umístěných na dočasném úložišti poskytnutém podle hello virtuálního počítače Azure? <br/>
Ano. Nainstalujte agenta Azure Backup hello na hello hostovaný operační systém Windows a zálohovat soubory a složky tootemporary úložiště. Jakmile dojde k vymazání dat na dočasném úložišti, úlohy zálohování selžou. Navíc pokud dat na dočasném úložišti hello byl odstraněn, lze obnovit pouze toonon volatile úložiště.

### <a name="whats-hello-minimum-size-requirement-for-hello-cache-folder-br"></a>Co je požadavek na minimální velikost hello hello složky mezipaměti? <br/>
Hello velikost složky mezipaměti hello určuje hello množství dat, která zálohujete. Složka mezipaměti musí být 5 % hello místo požadované pro datové úložiště.

### <a name="how-do-i-register-my-server-tooanother-datacenterbr"></a>Jak se zaregistruji Moje datacenter tooanother serveru?<br/>
Zálohovaná data se odesílají toohello datového centra toowhich hello trezoru, je registrován. Hello nejjednodušší způsob, jak toochange hello datacenter toouninstall hello Agent a znovu nainstalujte agenta hello a zaregistrujte tooa nové úložiště, který patří toodesired datacenter.

### <a name="does-hello-azure-backup-agent-work-on-a-server-that-uses-windows-server-2012-deduplication-br"></a>Podporuje hello Azure Backup agent fungovat na serveru, který používá odstranění duplicitních dat Windows serveru 2012? <br/>
Ano. Služba agenta Hello převede hello odstranění duplicit dat toonormal data během přípravy operace zálohování hello. Potom optimalizuje hello data pro zálohování, zašifruje hello dat a pak odešle hello zašifrovaná data toohello online služby zálohování.

## <a name="backup"></a>Zálohování
### <a name="how-do-i-change-hello-cache-location-specified-for-hello-azure-backup-agentbr"></a>Změna umístění mezipaměti hello zadané pro agenta Azure Backup hello<br/>
Použití hello následující seznam umístění mezipaměti toochange hello.

1. Zastavte modul zálohování hello tak, že spustíte následující příkaz v příkazovém řádku se zvýšenými hello:

    ```PS C:\> Net stop obengine``` 
  
2. Hello soubory nepřesouvejte. Místo toho zkopírujte hello mezipaměti místo složky tooa jinou jednotku s dostatkem místa. Po potvrzení hello zálohy fungují s hello novým místem v mezipaměti se dá odebrat Hello původní místo v mezipaměti.
3. Aktualizujte hello následující položky registru s hello cesta toohello nové místo složky mezipaměti.<br/>

    | Cesta k registru | Klíč registru | Hodnota |
    | --- | --- | --- |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` |ScratchLocation |*Nové umístění složky mezipaměti* |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` |ScratchLocation |*Nové umístění složky mezipaměti* |

4. Restartujte hello modul Backup spuštěním hello následující příkaz v příkazovém řádku se zvýšenými oprávněními:

    ```PS C:\> Net start obengine```

Po úspěšném dokončení vytvoření zálohy hello v hello nové umístění mezipaměti můžete původní složku mezipaměti hello odebrat.


### <a name="where-can-i-put-hello-cache-folder-for-hello-azure-backup-agent-toowork-as-expectedbr"></a>Kam mohu dát složku mezipaměti hello hello Azure Backup Agent toowork podle očekávání?<br/>
Hello následující umístění složky mezipaměti hello nejsou doporučené:

* Sdílenou síťovou složku nebo vyměnitelné médium: složka mezipaměti hello musí být místní toohello serveru, který potřebuje zálohování pomocí online zálohování. Síťová umístění a vyměnitelná média jako jednotky USB nejsou podporovaná.
* Offline svazky: složka mezipaměti hello musí být online pro očekávané zálohování pomocí agenta Azure Backup.

### <a name="are-there-any-attributes-of-hello-cache-folder-that-are-not-supportedbr"></a>Jsou nějaké atributy hello-složky mezipaměti, které nejsou podporovány?<br/>
Hello následující atributy nebo jejich kombinace nejsou podporovány pro složku mezipaměti hello:

* Šifrované
* S odstraněním duplicit
* Komprimované
* Řídké
* Bod rozboru

složka mezipaměti Hello a hello metadata virtuálního pevného disku není nutné hello nezbytné atributy pro agenta Azure Backup hello.

### <a name="is-there-a-way-tooadjust-hello-amount-of-bandwidth-used-by-hello-backup-servicebr"></a>Je k dispozici způsob tooadjust hello šířku pásma, které používá služba zálohování hello?<br/>
  Ano, použít hello **změnit vlastnosti** možnost hello Backup Agent tooadjust šířky pásma. Můžete upravit hello množství šířky pásma a hello časy, kdy tuto šířku pásma používáte. Podrobné pokyny najdete v tématu **[Povolení omezení využití sítě](backup-configure-vault.md#enable-network-throttling)**.

## <a name="manage-backups"></a>Správa záloh
### <a name="what-happens-if-i-rename-a-windows-server-that-is-backing-up-data-tooazurebr"></a>Co se stane, když přejmenuji server Windows, který zálohuje data tooAzure?<br/>
Když server přejmenujete, všechna stávající nastavená zálohování se zastaví.
Zaregistrujte nový název hello hello serveru s úložištěm záloh hello. Když registrujete nový název hello hello trezoru, hello první zálohování je *úplné* zálohování. Pokud potřebujete zálohovat toohello trezoru se starým názvem hello data toorecover, použijte hello [ **jiný server** ](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine) možnost v hello **obnovit Data** průvodce.

### <a name="what-is-hello-maximum-file-path-length-that-can-be-specified-in-backup-policy-using-azure-backup-agent-br"></a>Co je hello maximální délka cesty k souboru, může být uveden v zásadách zálohování pomocí agenta Azure Backup? <br/>
Agent Azure Backup se spoléhá na systém souborů NTFS. Hello [specifikace délky cesta k souboru je omezena hello rozhraní API systému Windows](https://msdn.microsoft.com/library/aa365247.aspx#fully_qualified_vs._relative_paths). Pokud hello soubory, které chcete tooprotect mít délku cestu souboru déle, než je povolené podle hello rozhraní API systému Windows, zálohujte hello nadřazenou složku nebo diskovou jednotku hello.  

### <a name="what-characters-are-allowed-in-file-path-of-azure-backup-policy-using-azure-backup-agent-br"></a>Jaké znaky jsou povolené v cestě k souboru zásady Azure Backup pomocí agenta Azure Backup? <br>
 Agent Azure Backup se spoléhá na systém souborů NTFS. Pro specifikaci souboru povoluje [znaky podporované systémem souborů NTFS](https://msdn.microsoft.com/library/aa365247.aspx#naming_conventions). 
 
### <a name="i-receive-hello-warning-azure-backups-have-not-been-configured-for-this-server-even-though-i-configured-a-backup-policy-br"></a>Přestože lze nakonfigurovat zásadu zálohování se zobrazí upozornění, hello "Zálohování Azure není nakonfigurovaná pro tento server" <br/>
Toto upozornění se zobrazí, pokud hello nastavení plánu zálohování uložené na místním serveru hello nejsou stejná jako hello nastavení uložená v úložišti záloh hello hello. Pokud hello server nebo nastavení hello byly obnovené tooa známé dobrý stav, plánů zálohování hello může dojít ke ztrátě synchronizace. Pokud se zobrazí toto upozornění [překonfigurujte zásadu zálohování hello](backup-azure-manage-windows-server.md) a potom **spusťte zálohovat nyní** tooresynchronize hello místního serveru s Azure.
