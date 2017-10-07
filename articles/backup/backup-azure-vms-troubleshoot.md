---
title: "aaaTroubleshoot chybám zálohování virtuálního počítače Azure | Microsoft Docs"
description: "Řešení zálohování a obnovení virtuálních počítačů Azure"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 73214212-57a4-4b57-a2e2-eaf9d7fde67f
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: trinadhk;markgal;jpallavi;
ms.openlocfilehash: 9224c47e02b52688adcba5876c674c88502557c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-virtual-machine-backup"></a>Odstraňování potíží se zálohováním virtuálních počítačů Azure
> [!div class="op_single_selector"]
> * [Trezor služeb zotavení](backup-azure-vms-troubleshoot.md)
> * [Úložiště záloh](backup-azure-vms-troubleshoot-classic.md)
>
>

Řešení potíží s chyb zjištěných při pomocí Azure Backup informace uvedené v tabulce hello.

## <a name="backup"></a>Zálohování
| Podrobnosti o chybě | Alternativní řešení |
| --- | --- |
| Nelze provést operaci hello, jako virtuální počítač už existuje. -Zastavení ochrany virtuálního počítače bez odstraňují se záložní data. Další podrobnosti o http://go.microsoft.com/fwlink/?LinkId=808124 |To se stane, když hello primárního virtuálního počítače je odstraněn, ale zásady zálohování hello stále vyhledávání pro virtuální počítač tooback. toofix tuto chybu: <ol><li> Znovu vytvořte virtuální počítač hello s hello stejný název a stejný název skupiny prostředků [název cloudové služby],<br>(NEBO)</li><li> Zastavte ochranu virtuálního počítače s nebo bez odstranění hello zálohovaná data. [Další informace](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol> |
| Operace snímku se nezdařilo z důvodu toono připojení k síti na virtuálním počítači hello - zajistěte, aby virtuální počítač přístup k síti. U snímku toosucceed buď rozsahy IP seznamu povolených IP adres Azure datacenter nebo nastavení proxy serveru pro přístup k síti. Další podrobnosti najdete v části příliš http://go.microsoft.com/fwlink/?LinkId=800034. Pokud už používáte proxy server, ujistěte se, že jsou správně nakonfigurované nastavení proxy serveru | Tato chyba se vyvolá, když odepřete hello odchozí připojení k Internetu hello virtuálního počítače. Připojení k Internetu je vyžadována pro virtuální počítač snímek rozšíření tootake snímek discích hello virtuálního počítače. [Další informace](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine) na tom, jak toofix snímku selhání kvůli tooblocked přístup k síti. |
| Agent virtuálního počítače je nelze toocommunicate s hello služby zálohování Azure. -Zkontrolujte hello virtuální počítač má síťové připojení a agent virtuálního počítače hello nejnovější a spuštěná. Další informace naleznete v příliš http://go.microsoft.com/fwlink/?LinkId=800034 |Tato chyba se vyvolá, pokud došlo k potížím s hello agenta virtuálního počítače, nebo blokovaný přístup toohello síťové infrastruktury Azure nějakým způsobem. [Další informace](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#vm-agent-unable-to-communicate-with-azure-backup) o ladění do virtuálních počítačů snímku problémy.<br> Pokud hello agenta virtuálního počítače není způsobuje problémy, restartujte hello virtuálních počítačů. Nesprávný stav virtuálního počítače v některých případech může způsobit problémy a restartování hello virtuálního počítače obnoví tento "chybný stav". |
| Virtuální počítač je v chybovém stavu zřizování – restartujte hello virtuální počítač a ujistěte se, že hello virtuální počítač je ve stavu spuštění nebo vypnutí pro zálohování | K tomu dochází, pokud jeden z hello rozšíření selhání vede toobe stav virtuálního počítače v chybovém stavu zřizování. Přejděte tooextensions seznamu a jestli je neúspěšné rozšíření, odeberte ji a zkuste restartovat virtuální počítač hello. Pokud všechna rozšíření je v běžícím stavu, zkontrolujte, zda je spuštěna Služba agenta virtuálního počítače. Pokud ne, restartujte službu agent hello virtuálních počítačů. | 
| VMSnapshot rozšíření operace se nezdařila pro spravované disky - prosím operaci zálohování opakujte hello. Pokud hello problém opakuje, postupujte podle pokynů hello 'http://go.microsoft.com/fwlink/?LinkId=800034'. Pokud selže dál, kontaktujte prosím podporu společnosti Microsoft | Tato chyba, pokud služba Azure Backup nezdaří tootrigger snímku. [Další informace](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#vmsnapshot-extension-operation-failed) o ladění virtuální počítač snímek problémy. |
| Může není snímek hello kopie hello virtuálního počítače, z důvodu tooinsufficient volného místa v účtu úložiště hello - zkontrolujte, zda má účet úložiště volné místo ekvivalentní toohello data obsažená na discích úložiště premium hello připojené toohello virtuálního počítače | V případě virtuálních počítačů úrovně premium zkopírujte jsme účet toostorage hello snímku. Toto je toomake jistotu, že provoz zálohování správy, který pracuje na snímek, není omezit hello počet IOPS aplikace k dispozici toohello pomocí prémiové disky. Společnost Microsoft doporučuje, že přidělíte jenom 50 % hello celkového účet prostoru úložiště, takže hello služby Azure Backup můžete zkopírovat hello snímku toostorage účet a přenos dat z tohoto umístění zkopírovaný v trezoru toohello účet úložiště. | 
| Operaci nelze tooperform hello jako hello agenta virtuálního počítače neodpovídá |Tato chyba se vyvolá, pokud došlo k potížím s hello agenta virtuálního počítače, nebo blokovaný přístup toohello síťové infrastruktury Azure nějakým způsobem. Pro virtuální počítače Windows zkontrolujte stav služby agent virtuálního počítače hello služeb a určuje, zda text hello agenta zobrazí v programy v Ovládacích panelech. Odebírání programu hello zkuste z ovládacích panelů přeinstalaci hello agenta a jak je uvedeno [pod](#vm-agent). Po opětovné instalaci agenta hello, spustíte zálohování tooverify ad hoc. |
| Obnovení služby rozšíření operace se nezdařila. -Prosím ujistěte, že je k dispozici na virtuálním počítači hello nejnovější agenta virtuálního počítače a je spuštěna Služba agenta. Opakujte operaci zálohování a pokud se nezdaří, obraťte se na podporu společnosti Microsoft. |Tato chyba se vyvolá, když agenta virtuálního počítače je zastaralý. Najdete v části "Hello aktualizace agenta virtuálního počítače" pod agenta virtuálního počítače tooupdate hello. |
| Virtuální počítač neexistuje. -Prosím zkontrolujte, zda že tento virtuální počítač existuje, nebo vyberte jiný virtuální počítač. |To se stane, když hello primárního virtuálního počítače je odstraněn, ale zásady zálohování hello stále toolook pro zálohu tooperform virtuálních počítačů. toofix tuto chybu: <ol><li> Znovu vytvořte virtuální počítač hello s hello stejný název a stejný název skupiny prostředků [název cloudové služby],<br>(NEBO)<br></li><li>Zastavení ochrany hello virtuálního počítače bez odstranění hello zálohovaná data. [Další informace](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol> |
| Provedení příkazu se nezdařilo. -Na této položce aktuálně probíhá jiná operace. Počkejte prosím na dokončení předchozí operace hello a poté opakujte |Existující zálohy na hello virtuálních počítačů se systémem a novou úlohu nelze spustit, pokud je spuštěná úloha existující hello. |
| Kopírování virtuálních pevných disků z trezoru záloh hello vypršel časový limit – zkuste to prosím znovu hello operaci za několik minut. Pokud hello potíže potrvají, obraťte se na Microsoft Support. | K tomu dojde, pokud dojde k přechodné chybě na straně úložiště nebo pokud se služba backup není dostatečná IOPS z účtu úložiště hostování hello virtuálních počítačů v pořadí tootransfer dat v rámci doby toovault časový limit. Ujistěte se, že jste udělali [osvědčené postupy](backup-azure-vms-introduction.md#best-practices) při nastavení zálohování. Zkuste přesunout virtuální počítač tooa jiného úložiště účet, který není načtený a opakujte zálohování.|
| Zálohování došlo k vnitřní chybě – zkuste to prosím znovu hello operaci za několik minut. Pokud hello potíže potrvají, obraťte se na Microsoft Support |Tuto chybu můžete získat z důvodů 2: <ol><li> V přístupu k hello úložiště virtuálního počítače je dočasný problém. Zkontrolujte, zda [stavu Azure](https://azure.microsoft.com/en-us/status/) toosee, pokud je jakýkoli průběžné problém související s toocompute, úložiště a sítě v oblasti hello. Opakujte úlohu zálohování hello po hello problém se vyřeší. <li>Hello původní virtuální počítač byl odstraněn, a proto nelze provést hello bod obnovení. tookeep hello zálohovaná data pro virtuální počítač odstraněný, ale odeberte chyby zálohování hello: zrušení hello virtuálních počítačů a zvolte hello možnost tookeep hello data. Tato akce ukončí hello zálohovací úlohy a hello opakovaného chybové zprávy. |
| Rozšíření služeb zotavení Azure hello tooinstall hello vybrané položky se nezdařilo - hello agenta virtuálního počítače je předpokladem pro hello Azure Recovery Services rozšíření. Nainstalujte agenta virtuálního počítače Azure hello a restartujte operaci registrace hello |<ol> <li>Zkontrolujte, zda správně nainstalován hello agenta virtuálního počítače. <li>Zkontrolujte, zda je správně nastaven příznak hello na hello konfigurace virtuálního počítače.</ol> [Další informace](#validating-vm-agent-installation) o instalaci agenta virtuálního počítače hello a jak toovalidate hello instalace agenta virtuálního počítače. |
| Instalace rozšíření se nezdařilo s chybou hello "modelu COM + je nelze tootalk toohello Microsoft Distributed Transaction Coordinator |To obvykle znamená, že hello modelu COM + není spuštěná. Nápovědu k opravě tohoto problému, kontaktujte podporu společnosti Microsoft. |
| Snímek operace se nezdařila s hello chyba operaci služby Stínová kopie svazku "Tato jednotka je uzamčen nástrojem BitLocker Drive Encryption. Je třeba odemknout tuto jednotku z hello ovládací panely. |Vypnout nástroj BitLocker pro všechny disky na hello virtuálních počítačů a zjistíte, zda text hello VSS problém vyřešen |
| Virtuální počítač není ve stavu, který umožňuje zálohování. |<ul><li>Zkontrolujte, jestli virtuální počítač je ve stavu přechodný mezi spuštění a vypnutí dolů. Pokud se jedná, počkejte hello toobe stav virtuálního počítače, jeden z nich a aktivuje zálohovat znovu. <li> Pokud hello virtuálního počítače je virtuální počítač s Linuxem a používá [Linux rozšířené zabezpečení] modulu jádra, je třeba tooexclude hello cesta agenta pro Linux (_/var/lib/waagent_) z zabezpečení získá zásady toomake opravdu zálohování rozšíření nainstalované.  |
| Virtuální počítač Azure nebyla nalezena. |To se stane, když hello primárního virtuálního počítače je odstraněn, ale zásady zálohování hello stále toolook pro virtuální počítač tooperform zálohování. toofix tuto chybu: <ol><li>Znovu vytvořte virtuální počítač hello s hello stejný název a stejný název skupiny prostředků [název cloudové služby], <br>(NEBO) <li> Vypněte ochranu pro tento virtuální počítač tak, aby úlohy zálohování hello se nevytvoří. </ol> |
| Agent virtuálního počítače není k dispozici na virtuálním počítači hello – nainstalujte všechny požadované a hello agenta virtuálního počítače a pak restartujte operaci hello. |[Další informace](#vm-agent) o instalace agenta virtuálního počítače a jak toovalidate hello instalace agenta virtuálního počítače. |
| Operace snímku se nezdařilo z důvodu tooVSS zapisovače ve špatném stavu |Je nutné toorestart zapisovače VSS (službu Stínová kopie svazku), které jsou ve špatném stavu. tooachieve, z příkazového řádku se zvýšenými oprávněními spustíte _vssadmin seznam zapisovačů_. Výstup obsahuje všechny zapisovače VSS a jejich stavu. Pro každý zapisovač VSS, jejichž stav není "[1] stabilní" restartujte zapisovače VSS spuštěním následujících příkazů z příkazového řádku se zvýšenými oprávněními:<br> _serviceName net stop_ <br> _net start serviceName_|
| Operace snímku se nezdařilo z důvodu tooa analýzy selhání hello konfigurace |To se stane, že kvůli toochanged oprávnění v adresáři MachineKeys hello: _%systemdrive%\programdata\microsoft\crypto\rsa\machinekeys_ <br>Spusťte následující příkaz a ověřte, zda jsou oprávnění v adresáři MachineKeys výchozí-ty, které jsou:<br>_icacls %systemdrive%\programdata\microsoft\crypto\rsa\machinekeys_ <br><br> Výchozí oprávnění jsou:<br>Everyone:(R,W) <br>BUILTIN\Administrators:(F)<br><br>Pokud se v adresáři MachineKeys jiný než výchozí oprávnění, postupujte podle níže kroky toocorrect oprávnění, odstraňte certifikát hello a spustit zálohování hello.<ol><li>Opravte oprávnění v adresáři MachineKeys.<br>Pomocí Průzkumníka vlastnosti zabezpečení a Upřesnit nastavení zabezpečení v adresáři hello, resetování oprávnění zpět toohello výchozí hodnoty, odeberte navíc (než výchozí nastavení) uživatelských objektů z adresáře hello a ujistěte se, že hello 'všichni měl zvláštní oprávnění přístup pro:<br>-Zobrazovat obsah složky / Číst data <br>– Přečtěte si atributy <br>– Přečtěte si rozšířené atributy <br>-Vytvářet soubory / Zapisovat data <br>-Vytvářet složky / Připojovat data<br>-Zápis atributů<br>-Zapisovat rozšířené atributy<br>-Oprávnění ke čtení<br><br><li>Odstranit všechny certifikáty vydané při s = "Windows Azure Service Management pro rozšíření" nebo "Windows Azure CRP certifikát generátor".<ul><li>[Otevřete konzolu Certifikáty (místní počítač)](https://msdn.microsoft.com/library/ms788967(v=vs.110).aspx)<li>Odstranit všechny certifikáty (v části osobní -> certifikáty) s pole "Vydáno pro" = "Windows Azure Service Management pro rozšíření" nebo "Windows Azure CRP certifikát generátor".</ul><li>Spouští se zálohování virtuálních počítačů. </ol>|
| Ověření se nezdařilo, protože virtuální počítač je šifrován BEK samostatně. Zálohování se dá nastavit jenom pro virtuální počítače šifrován BEK a KEK. |Virtuální počítač by se šifrovat pomocí šifrovacího klíče nástroje BitLocker a šifrovací klíče. Potom by měla být povolená zálohování. |
| Služba Azure Backup nemá dostatečná oprávnění tooKey trezor pro zálohování šifrované virtuálních počítačů. |Služba zálohování je nutné zadat položek tato oprávnění v prostředí PowerShell pomocí kroků uvedených v **povolit zálohování** části [prostředí PowerShell dokumentaci](backup-azure-vms-automation.md). |
|Snímek instalaci rozšíření se nezdařilo s chybou – modelu COM + byla nelze tootalk toohello Microsoft Distributed Transaction Coordinator | Prosím zkuste toostart windows služby "aplikace modelu COM + systému" (z příkazového řádku se zvýšenými - _net start COMSysApp_). <br>Pokud jsou selhání při spouštění, postupujte podle následujících kroků:<ol><li> Ověřte, že přihlašovací účet služby "Koordinátor distribuovaných transakcí" hello "Síťová služba". Pokud není, změňte ho příliš "Síťová služba", restartujte tuto službu a poté toostart služby "aplikace modelu COM + systému". "<li>Pokud stále nedaří toostart, odinstalovat a instalaci služby "Koordinátor distribuovaných transakcí" podle kroků v následujících kroků:<br> -Zastavit služba koordinátoru MS DTC hello<br> -Otevřete příkazový řádek (cmd) <br> – Spusťte příkaz "msdtc-odinstalovat" <br> – Spusťte příkaz "msdtc-instalaci" <br> -Start služba koordinátoru MS DTC hello<li>Spustit službu systému windows "aplikace modelu COM + systému" a po jeho spuštění aktivuje zálohování z portálu.</ol> |
|  Operace snímku se nezdařilo z důvodu chyby tooCOM + | Hello Doporučená akce je služba systému windows toorestart "aplikace modelu COM + systému" (z příkazového řádku se zvýšenými - _net start COMSysApp_). Pokud hello problém přetrvává, restartujte hello virtuálních počítačů. Pokud restartování virtuálního počítače není pomoct hello, zkuste [odebrání hello VMSnapshot rozšíření](https://docs.microsoft.com/en-us/azure/backup/backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout#cause-3-the-backup-extension-fails-to-update-or-load) a spustit zálohování hello ručně. |
| Se nezdařilo toofreeze jeden nebo více přípojných bodech tohoto hello virtuálních počítačů tootake konzistentního snímku systému souborů | Použijte hello následující kroky: <ol><li>Zkontrolujte stav systému souborů hello všech připojených zařízení pomocí _'tune2fs'_ příkaz.<br> Např: tune2fs -l/dev/sdb1 \| GREP "Filesystem stavu" <li>Odpojení hello zařízení, pro které filesystem stav není čistá pomocí _'umount'_ příkaz <li> Spustit kontrolu FileSystemConsistency na tato zařízení pomocí _'fsck'_ příkaz <li> Znovu připojte hello zařízení a vyzkoušejte zálohování.</ol> |
| Pořízení snímku se nezdařilo z důvodu operace toofailure vytvořit zabezpečený komunikační kanál | <ol><Li> Spuštěním regedit.exe v režimu zvýšené otevřete Editor registru. <li> Identifikujte všechny verze. NetFramework v systému existuje. Jsou v hierarchii hello klíče registru "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft" <li> Pro každou. NetFramework nachází v klíči registru a přidejte následující klíče: <br> "SchUseStrongCrypto" = dword: 00000001 </ol>|
| Pořízení snímku se nezdařilo z důvodu operace toofailure v instalaci Visual C++ Redistributable for Visual Studio 2012 | Přejděte tooC:\Packages\Plugins\Microsoft.Azure.RecoveryServices.VMSnapshot\agentVersion a nainstalujte vcredist2012_x64. Ujistěte se, zda hodnota klíče registru pro povolení této instalace služby je nastavena hodnota toocorrect tj hodnota klíče registru _HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Msiserver_ nastavena too3 a není 4. Pokud se stále setkávají problémy s instalací, restartujte službu instalace spuštěním _MSIEXEC/UNREGISTER_ následuje _MSIEXEC /REGISTER_ z příkazového řádku se zvýšenými oprávněními.  |


## <a name="jobs"></a>Úlohy
| Podrobnosti o chybě | Alternativní řešení |
| --- | --- |
| Zrušení není podporován pro tento typ úlohy – počkejte na dokončení úlohy hello. |Žádný |
| Hello úloha není ve stavu, možné zrušit – počkejte na dokončení úlohy hello. <br>NEBO<br> Hello vybraná úloha není ve stavu, možné zrušit – hello úlohy toocomplete Počkejte prosím. |Ve všech pravděpodobnost je téměř dokončena úloha hello. Počkejte prosím na dokončení úlohy hello.|
| Nelze zrušit úlohu hello, protože není v průběhu – zrušení je podporována pouze pro úlohy, které jsou v průběhu. Prosím pokus o zrušení na v průběhu úlohy. |K tomu dochází z důvodu tooa přechodné stavu. Počkejte několik minut a opakujte operaci zrušit hello. |
| Neúspěšné toocancel hello úlohy - Počkejte, dokud je úloha dokončena. |Žádný |

## <a name="restore"></a>Obnovení
| Podrobnosti o chybě | Alternativní řešení |
| --- | --- |
| Obnovení se nezdařilo s cloudu vnitřní chyba |<ol><li>Cloudové služby toowhich se pokoušíte toorestore je nakonfigurováno nastavení DNS. Můžete zkontrolovat <br>$deployment = get-AzureDeployment - ServiceName "ServiceName"-slotu "Výroba" Get-AzureDns - DnsSettings $deployment. DnsSettings<br>Pokud je nakonfigurovanou adresu, znamená to, zda jsou nakonfigurovány nastavení DNS.<br> <li>Cloudové služby toowhich tooyou zkoušíte toorestore je nakonfigurován pomocí vyhrazené IP adresy a stávající virtuální počítače v rámci cloudové služby jsou v zastaveném stavu.<br>Můžete zkontrolovat, že cloudové služby má vyhrazená IP adresa pomocí následující rutiny prostředí powershell:<br>$deployment = get-AzureDeployment - ServiceName "servicename"-slotu $ "Výroba" dep. ReservedIPName <br><li>Pokoušíte toorestore virtuální počítač s následující zvláštní síťové konfigurace v toosame cloudové služby. <br>-Virtuální počítače v části Konfigurace služby Vyrovnávání zatížení (interní a externí)<br>-Virtuální počítače s víc vyhrazených IP adres<br>-Virtuální počítače s více síťovými kartami<br>Vyberte novou cloudovou službu v hello uživatelského rozhraní nebo naleznete příliš[obnovit aspekty](backup-azure-arm-restore-vms.md#restoring-vms-with-special-network-configurations) u virtuálních počítačů s zvláštní síťové konfigurace.</ol> |
| název DNS Hello vybrané už je zabraný – zadejte jiný název DNS a zkuste to znovu. |Hello sem název DNS odkazuje toohello název cloudové služby (obvykle konče. cloudapp.net). Tento krok je nutné toobe jedinečný. Pokud dojde k této chybě, musíte během obnovení toochoose jiný název virtuálního počítače. <br><br> Tato chyba se zobrazí pouze toousers hello portálu Azure. Hello operace obnovení pomocí prostředí PowerShell bude úspěšné, protože pouze obnoví hello disky a nevytváří hello virtuálních počítačů. Chyba Hello se potýkají, když hello virtuálního počítače je explicitně vytvořený můžete po operaci obnovení disku hello. |
| Hello zadané virtuální síti konfigurace není správná – zadejte konfiguraci s jinou virtuální síť a zkuste to znovu. |Žádný |
| Hello zadaná, používá vyhrazenou IP adresu, která nebude shodovat s konfigurací hello hello virtuálního počítače, který se má obnovit – Zadejte prosím jinou cloudovou službu, která není pomocí vyhrazené IP adresy nebo zvolte jinou toorestore bod obnovení z cloudové služby. |Žádný |
| Cloudové služby bylo dosaženo limitu počtu vstupních koncových bodů - hello operace opakování zadáním jiné cloudové služby nebo pomocí existující koncový bod. |Žádný |
| Zálohování úložiště a cílový účet úložiště jsou ve dvou různých oblastech – zkontrolujte, zda text hello úložiště účet zadaný v operaci obnovení v hello stejné oblasti Azure jako trezor záloh hello. |Žádný |
| Účet úložiště, zadaný pro operaci obnovení hello není podporována – účty úložiště pouze Basic nebo Standard s místně redundantní nebo geograficky redundantní replikaci nastavení nejsou podporovány. Vyberte prosím účet podporované úložiště |Žádný |
| Typ zadaný pro operaci obnovení účtu úložiště není online – Ujistěte se, že je účet úložiště hello zadaný v operaci obnovení online |K tomu může dojít z důvodu přechodné chyby ve službě Azure Storage nebo z důvodu tooan výpadku. Zvolte prosím jiný účet úložiště. |
| Bylo dosaženo kvóty skupina prostředků – prosím odstraňte některé skupiny prostředků z portálu Azure nebo požádejte podporu Azure tooincrease hello omezení. |Žádný |
| Vybraná podsíť neexistuje – vyberte podsíť, která již existuje |Žádný |
| Služba zálohování nemá autorizace tooaccess prostředky ve vašem předplatném. |tooresolve se první obnovení disků pomocí kroků uvedených v části **obnovení zálohovaných disky** v [konfigurace obnovení výběru virtuálního počítače](backup-azure-arm-restore-vms.md#choosing-a-vm-restore-configuration). Potom pomocí prostředí PowerShell kroků uvedených v [vytvoření virtuálního počítače z obnovené disků](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) toocreate úplné z obnovené disků virtuálních počítačů. |

## <a name="backup-or-restore-taking-time"></a>Zálohování nebo obnovení doba
Pokud se zobrazí vaše backup(>12 hours) nebo obnovení trvá time(>6 hours):
* Pochopení [faktory, které přispívají toobackup čas](backup-azure-vms-introduction.md#total-vm-backup-time) a [faktory, které přispívají toorestore čas](backup-azure-vms-introduction.md#total-restore-time).
* Ujistěte se, že [zálohování osvědčené postupy](backup-azure-vms-introduction.md#best-practices). 

## <a name="vm-agent"></a>Agent virtuálního počítače
### <a name="setting-up-hello-vm-agent"></a>Nastavení hello agenta virtuálního počítače
Obvykle hello agenta virtuálního počítače již existuje ve virtuálních počítačích, které jsou vytvořené pomocí hello Galerie Azure. Virtuální počítače, které jsou migrované z místních datových center však nebude mít hello Agent virtuálního počítače nainstalovaný. Pro tyto virtuální počítače musí hello agenta virtuálního počítače toobe explicitně nainstalována.

Pro virtuální počítače Windows:

* Stáhněte a nainstalujte hello [MSI agenta](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Potřebujete instalaci hello toocomplete oprávnění správce.
* Klasické virtuální počítače [aktualizujte vlastnost virtuálního počítače hello](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) tooindicate, který hello agenta je nainstalovaná. Tento krok není požadovaná pro virtuální počítače správce prostředků.

Pro virtuální počítače Linux:

* Nainstalujte nejnovější z distribuční úložiště. Jsme **důrazně doporučujeme** instalaci agenta pouze prostřednictvím distribuční úložiště. Podrobnosti o název balíčku naleznete příliš[Linux agent úložiště](https://github.com/Azure/WALinuxAgent) 
* Pro klasické virtuální počítače [aktualizujte vlastnost virtuálního počítače hello](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) tooindicate, který hello agenta je nainstalovaná. Tento krok není požadovaná pro virtuální počítače správce prostředků.

### <a name="updating-hello-vm-agent"></a>Aktualizace hello agenta virtuálního počítače
Pro virtuální počítače Windows:

* Hello aktualizace agenta virtuálního počítače je stejně jednoduché jako přeinstalování hello [binárních souborů agenta virtuálního počítače](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Je však třeba tooensure, která při hello Probíhá aktualizace agenta virtuálního počítače neběží žádná operace zálohování.

Pro virtuální počítače Linux:

* Postupujte podle pokynů hello na [aktualizace agenta virtuálního počítače Linux](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
Jsme **důrazně doporučujeme** aktualizací agenta pouze prostřednictvím distribuční úložiště. Nedoporučujeme stahování hello agenta kód z githubu přímo a jeho aktualizaci. Pokud není k dispozici pro distribuční nejnovější verze agenta, kontaktujte podporu toodistribution pokyny, jak tooinstall nejnovější verze agenta. Můžete zkontrolovat nejnovější [Windows Azure Linux agent](https://github.com/Azure/WALinuxAgent/releases) informace v úložišti github.

### <a name="validating-vm-agent-installation"></a>Ověření instalace agenta virtuálního počítače
Jak toocheck pro hello verze agenta virtuálního počítače na virtuálních počítačích Windows:

1. Přihlaste toohello virtuální počítač Azure a přejděte složky toohello *C:\WindowsAzure\Packages*. Byste měli najít přítomný soubor WaAppAgent.exe hello.
2. Klikněte pravým tlačítkem na soubor hello, přejděte příliš**vlastnosti**a potom vyberte hello **podrobnosti** kartě hello verze produktu pole by mělo být 2.6.1198.718 nebo vyšší

## <a name="troubleshoot-vm-snapshot-issues"></a>Řešení problémů snímek virtuálního počítače
Zálohování virtuálních počítačů spoléhá na vydání příkazů snímku toounderlying úložiště. Nemá přístup toostorage nebo zpoždění při provádění úloh snímku může způsobit toofail hello úlohu zálohování. Následující Hello může způsobit selhání úkolů snímku.

1. TooStorage přístup k síti je blokovaný pomocí NSG<br>
    Další informace o tom, příliš[povolit přístup k síti](backup-azure-vms-prepare.md#network-connectivity) tooStorage pomocí buď povolených IP adres nebo prostřednictvím proxy serveru.
2. Virtuální počítače pomocí zálohování serveru Sql Server nakonfigurován může způsobit zpoždění úloha snímku <br>
   Ve výchozím nastavení virtuálního počítače zálohování problémů služby Stínová kopie svazku úplné zálohování na virtuální počítače Windows. Na virtuálních počítačích se systémem Sql Server a jestli je nakonfigurovaná zálohování serveru Sql Server to může způsobit zpoždění při provádění snímku. Nastavte následující klíč registru, pokud dochází k chybám zálohování z důvodu problémů s snímku.

   ```
   [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
   "USEVSSCOPYBACKUP"="TRUE"
   ```
3. Stav virtuálního počítače se nesprávně uvést, protože virtuální počítač je vypnutý v protokolu RDP.  <br>
   Pokud vypnete hello virtuálního počítače v protokolu RDP, Zkontrolujte prosím zpět v hello portál, aby se správně odráží stav virtuálního počítače. V opačném případě ukončete hello virtuálních počítačů v portálu pomocí možnosti "Vypnout" na řídicím panelu virtuálních počítačů.
4. Pokud sdílená složka více než čtyři virtuální počítače hello stejné cloudové služby, konfigurace více hello toostage zásady zálohování čas zálohování proto žádné, více než čtyři zálohování virtuálních počítačů jsou zahájená hello stejnou dobu. Zkuste toospread hello zálohování doby spuštění hodinu od sebe mezi zásadami.
5. Virtuální počítač spuštěný na vysoké využití procesoru nebo paměti.<br>
   Pokud hello virtuální počítač běží na vysoké využití procesoru usage(>90%) nebo paměť, je snímek úloh zařazených do fronty, zpoždění a bude nakonec získá vypršení časového limitu. Opakujte zálohování na vyžádání v takových situacích.

<br>

## <a name="networking"></a>Sítě
Všechna rozšíření, jako je třeba rozšíření zálohování přístup k toohello veřejný internet toowork. Nemá přístup k toohello veřejného Internetu může projevit různými způsoby:

* instalace rozšíření Hello může selhat.
* operace zálohování Hello (např. disku snímek) může selhat.
* Zobrazení stavu hello hello operace zálohování může selhat.

potřebu Hello řešení veřejné internetové adresy byla kloubové. [zde](http://blogs.msdn.com/b/mast/archive/2014/06/18/azure-vm-provisioning-stuck-on-quot-installing-extensions-on-virtual-machine-quot.aspx). Konfigurace DNS hello toocheck nutnost hello virtuální sítě a ujistěte se, že hello identifikátory URI Azure lze přeložit.

Po dokončení hello překlad správně přístup toohello IP adres Azure také je nutné zadat toobe. toounblock přístup toohello infrastrukturu Azure, použijte jednu z těchto kroků:

1. Povolit hello datové centrum Azure rozsahy IP adres.
   * Získejte seznam hello [IP adres Azure datacenter](https://www.microsoft.com/download/details.aspx?id=41653) toobe seznam povolených adres.
   * Odblokování hello IP adres pomocí hello [New-NetRoute](https://technet.microsoft.com/library/hh826148.aspx) rutiny. Tato rutina v rámci hello virtuálního počítače Azure, spusťte v okně prostředí PowerShell zvýšenými oprávněními (Spustit jako správce).
   * (Pokud nemáte na místě) přidejte toohello pravidla NSG tooallow přístup toohello IP adresy.
2. Vytvoření cesty pro tooflow provoz protokolu HTTP
   * Pokud máte nějaké omezení sítě na místě (skupina zabezpečení sítě, například) nasadit provozu hello tooroute HTTP proxy serveru. Server Proxy protokolu HTTP lze nalézt toodeploy kroky [zde](backup-azure-vms-prepare.md#network-connectivity).
   * (Pokud nemáte na místě) přidejte toohello pravidla NSG tooallow toohello přístup k Internetu z hello server Proxy protokolu HTTP.

> [!NOTE]
> DHCP musí být povolena uvnitř hello hosta pro zálohování virtuálních počítačů IaaS toowork.  Pokud potřebujete statickou privátní IP adresu, byste ho měli nakonfigurovat prostřednictvím platformy hello. Hello možnost DHCP uvnitř hello virtuální počítač by měl být ponecháno povolena.
> Zobrazit další informace o [nastavení statickou privátní IP interní](../virtual-network/virtual-networks-reserved-private-ip.md).
>
>
