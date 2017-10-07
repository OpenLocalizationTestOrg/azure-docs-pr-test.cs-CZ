---
title: "aaaTroubleshoot Azure zálohování chyby portálu classic | Microsoft Docs"
description: "Řešení potíží s Azure zálohování a obnovení virtuálních počítačích Azure na portálu classic hello."
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 117201fb-c0cd-4be4-b47f-abd88fe914cf
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/23/2017
ms.author: trinadhk;markgal;
ms.openlocfilehash: b9907f6fa57c631f5446c4d00f946a57c4efd72b
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

## <a name="discovery"></a>Zjišťování
| Operace zálohování | Podrobnosti o chybě | Alternativní řešení |
| --- | --- | --- |
| Zjišťování |Nepodařilo toodiscover nové body – Microsoft Azure Backup došlo k vnitřní chybě. Počkejte několik minut a hello operaci opakujte. |Proces zjišťování hello opakujte po 15 minutách. |
| Zjišťování |Nepodařilo toodiscover nové položky – další zjišťování již probíhá operace. Počkejte prosím na dokončení aktuální operace zjišťování hello. |Žádný |

## <a name="register"></a>Registrace
| Operace zálohování | Podrobnosti o chybě | Alternativní řešení |
| --- | --- | --- |
| Registrace |Počet datových disků připojených toohello virtuálního počítače překročil hello podporované limit – prosím odpojit některé datových disků v této operaci hello virtuální počítač a zkuste to znovu. Azure backup podporuje až too16 datových disků připojených tooan virtuálního počítače, které jsou pro zálohování Azure |Žádný |
| Registrace |Microsoft Azure Backup se vyskytla vnitřní chyba - Počkejte několik minut a hello operaci opakujte. Pokud hello potíže potrvají, obraťte se na Microsoft Support. |Tato chyba z důvodu tooone hello následující nepodporované konfigurace virtuálního počítače můžete získat na Premium LRS. <br> Virtuální počítače služby storage úrovně Premium můžete zálohovat pomocí trezoru služeb zotavení. [Další informace](backup-introduction-to-azure-backup.md#using-premium-storage-vms-with-azure-backup) |
| Registrace |Registrace s vypršením časového limitu operace instalace agenta se nezdařila. |Zkontrolujte, pokud se podporuje verze operačního systému hello hello virtuálního počítače. |
| Registrace |Příkaz se nezdařilo - jiná operace probíhá spuštění u této položky. Počkejte prosím na dokončení předchozí operace hello |Žádný |
| Registrace |Virtuální počítače s virtuální pevné disky uložené na storage úrovně Premium nejsou podporované pro zálohování |Žádný |
| Registrace |Agent virtuálního počítače není k dispozici na virtuálním počítači hello – nainstalujte hello požadované nezbytné, agent virtuálního počítače a restartujte operaci hello. |[Další informace](#vm-agent) o instalace agenta virtuálního počítače a jak toovalidate hello instalace agenta virtuálního počítače. |

## <a name="backup"></a>Zálohování
| Operace zálohování | Podrobnosti o chybě | Alternativní řešení |
| --- | --- | --- |
| Zálohování |Nešlo komunikovat s hello agenta virtuálního počítače pro snímek stavu. Vypršel časový limit úloze dílčí snímků virtuálních počítačů. -Najdete v tématu Průvodce na postupy řešení potíží s hello tooresolve to. |Tato chyba se vyvolá, pokud došlo k potížím s hello agenta virtuálního počítače, nebo blokovaný přístup toohello síťové infrastruktury Azure nějakým způsobem. Další informace o [ladění si virtuální počítač snímek problémy](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md). <br> Pokud hello agenta virtuálního počítače není způsobuje problémy, restartujte hello virtuálních počítačů. Nesprávný stav virtuálního počítače v některých případech může způsobit problémy a restartování hello virtuálního počítače obnoví tento "chybný stav" |
| Zálohování |Zálohování došlo k vnitřní chybě – zkuste to prosím znovu hello operaci za několik minut. Pokud hello potíže potrvají, obraťte se na Microsoft Support |Zkontrolujte prosím, pokud je dočasný problém s přístupem ke úložiště virtuálního počítače. Zkontrolujte, zda [stavu Azure](https://azure.microsoft.com/en-us/status/) toosee, pokud neexistuje žádné průběžné vydání související toocompute/úložiště nebo sítě v oblasti hello. Prosím opakovat hello zálohování post problém zmírnit. |
| Zálohování |Nelze provést operaci hello, jako virtuální počítač už existuje. |Zálohování nelze provést, protože hello konfigurovat pro zálohování virtuálních počítačů byla odstraněna. Zastavit další zálohy budete tooProtected položky zobrazení, vyberte chráněné položky a klikněte na Zastavit ochranu. Vyberte možnost zachovat zálohu dat můžete zachovat data. Později můžete obnovit ochrany pro tento virtuální počítač kliknutím na Konfigurace ochrany z registrované položky zobrazení |
| Zálohování |Neúspěšné tooinstall hello rozšíření služeb zotavení Azure na hello vybrané položky - Agent virtuálního počítače je nezbytný předpoklad pro Azure Recovery Services rozšíření. Nainstalujte agenta virtuálního počítače Azure hello a restartujte operaci registrace hello |<ol> <li>Zkontrolujte, zda správně nainstalován hello agenta virtuálního počítače. <li>Ujistěte se, že hello příznak na hello konfigurace nastavena správně.</ol> [Další informace](#validating-vm-agent-installation) o instalace agenta virtuálního počítače a jak toovalidate hello instalace agenta virtuálního počítače. |
| Zálohování |Příkaz spuštění se nezdařilo - jiná operace právě probíhá u této položky. Počkejte prosím na dokončení předchozí operace hello a poté opakujte |Existující úlohy zálohování nebo obnovení pro hello virtuálních počítačů se systémem a novou úlohu nelze spustit, pokud je spuštěná úloha existující hello. |
| Zálohování |Instalace rozšíření se nezdařilo s chybou hello "modelu COM + je nelze tootalk toohello Microsoft Distributed Transaction Coordinator |To obvykle znamená, že hello modelu COM + není spuštěná. Nápovědu k opravě tohoto problému, kontaktujte podporu společnosti Microsoft. |
| Zálohování |Snímek operace se nezdařila s hello chyba operaci služby Stínová kopie svazku "Tato jednotka je uzamčen nástrojem BitLocker Drive Encryption. Je třeba odemknout tuto jednotku v Ovládacích panelech. |Vypnout nástroj BitLocker pro všechny disky na hello virtuálních počítačů a zjistíte, zda text hello VSS problém vyřešen |
| Zálohování |Virtuální počítače s virtuální pevné disky uložené na storage úrovně Premium nejsou podporované pro zálohování |Žádný |
| Zálohování |Virtuální počítač Azure nebyla nalezena. |To se stane, když hello primárního virtuálního počítače je odstraněn, ale zásady zálohování hello stále toolook pro zálohu tooperform virtuálních počítačů. toofix tuto chybu: <ol><li>Znovu vytvořte virtuální počítač hello s hello stejný název a stejný název skupiny prostředků [název cloudové služby], <br>(NEBO) <li> Zakažte ochranu pro tento virtuální počítač tak, aby nebude získat aktivuje následné zálohy. </ol> |
| Zálohování |Agent virtuálního počítače není k dispozici na virtuálním počítači hello – nainstalujte hello požadované nezbytné, agent virtuálního počítače a restartujte operaci hello. |[Další informace](#vm-agent) o instalace agenta virtuálního počítače a jak toovalidate hello instalace agenta virtuálního počítače. |

## <a name="jobs"></a>Úlohy
| Operace | Podrobnosti o chybě | Alternativní řešení |
| --- | --- | --- |
| Zrušení úlohy |Zrušení není podporován pro tento typ úlohy – počkejte na dokončení úlohy hello. |Žádný |
| Zrušení úlohy |Hello úloha není ve stavu, možné zrušit – počkejte na dokončení úlohy hello. <br>NEBO<br> Hello vybraná úloha není ve stavu, možné zrušit – hello úlohy toocomplete Počkejte prosím. |Ve všech pravděpodobnost je téměř dokončena úloha hello; Počkejte prosím na dokončení úlohy hello |
| Zrušení úlohy |Nelze zrušit úlohu hello, protože není v průběhu – zrušení je podporována pouze pro úlohy, které jsou v průběhu. Prosím pokus o zrušení na v průběhu úlohy. |K tomu dochází z důvodu tooa přechodné stavu. Počkejte několik minut a opakujte operaci zrušit hello |
| Zrušení úlohy |Neúspěšné toocancel hello úlohy - Počkejte prosím, dokud nebude dokončeno úlohy. |Žádný |

## <a name="restore"></a>Obnovení
| Operace | Podrobnosti o chybě | Alternativní řešení |
| --- | --- | --- |
| Obnovení |Obnovení se nezdařilo s cloudu vnitřní chyba |<ol><li>Cloudové služby toowhich se pokoušíte toorestore je nakonfigurováno nastavení DNS. Můžete zkontrolovat <br>$deployment = get-AzureDeployment - ServiceName "ServiceName"-slotu "Výroba" Get-AzureDns - DnsSettings $deployment. DnsSettings<br>Pokud je nakonfigurovanou adresu, znamená to, zda jsou nakonfigurovány nastavení DNS.<br> <li>Cloudové služby toowhich tooyou zkoušíte toorestore je nakonfigurován pomocí vyhrazené IP adresy a stávající virtuální počítače v rámci cloudové služby jsou v zastaveném stavu.<br>Můžete zkontrolovat, že cloudové služby má vyhrazená IP adresa pomocí následující rutiny prostředí powershell:<br>$deployment = get-AzureDeployment - ServiceName "servicename"-slotu $ "Výroba" dep. ReservedIPName <br><li>Pokoušíte toorestore virtuální počítač s následující zvláštní síťové konfigurace v toosame cloudové služby. <br>-Virtuální počítače v části Konfigurace služby Vyrovnávání zatížení (interní a externí)<br>-Virtuální počítače s víc vyhrazených IP adres<br>-Virtuální počítače s více síťovými kartami<br>Vyberte novou cloudovou službu v hello uživatelského rozhraní nebo naleznete příliš[obnovit aspekty](backup-azure-restore-vms.md#restoring-vms-with-special-network-configurations) u virtuálních počítačů s zvláštní síťové konfigurace</ol> |
| Obnovení |název DNS Hello vybrané už je zabraný – zadejte jiný název DNS a zkuste to znovu. |Hello sem název DNS odkazuje toohello název cloudové služby (obvykle konče. cloudapp.net). Tento krok je nutné toobe jedinečný. Pokud dojde k této chybě, musíte během obnovení toochoose jiný název virtuálního počítače. <br><br> Tato chyba se zobrazí pouze toousers hello portálu Azure. Hello operace obnovení pomocí prostředí PowerShell úspěšné protože pouze obnoví hello disky a nevytváří hello virtuálních počítačů. Chyba Hello se potýkají, když hello virtuálního počítače je explicitně vytvořený můžete po operaci obnovení disku hello. |
| Obnovení |Hello zadané virtuální síti konfigurace není správná – zadejte konfiguraci s jinou virtuální síť a zkuste to znovu. |Žádný |
| Obnovení |Hello zadaná, používá vyhrazenou IP adresu, která nebude shodovat s konfigurací hello hello virtuálního počítače, který se má obnovit – zadejte jinou cloudovou službu, která není pomocí vyhrazené IP adresy nebo zvolte jinou toorestore bod obnovení z cloudové služby. |Žádný |
| Obnovení |Cloudové služby bylo dosaženo limitu počtu vstupních koncových bodů - hello operace opakování zadáním jiné cloudové služby nebo pomocí existující koncový bod. |Žádný |
| Obnovení |Zálohování úložiště a cílový účet úložiště jsou ve dvou různých oblastech – zkontrolujte, zda text hello úložiště účet zadaný v operaci obnovení v hello stejné oblasti Azure jako trezor záloh hello. |Žádný |
| Obnovení |Účet úložiště, zadaný pro operaci obnovení hello není podporována – účty úložiště pouze Basic nebo Standard s místně redundantní nebo geograficky redundantní replikaci nastavení nejsou podporovány. Vyberte prosím účet podporované úložiště |Žádný |
| Obnovení |Typ zadaný pro operaci obnovení účtu úložiště není online – Ujistěte se, že je účet úložiště hello zadaný v operaci obnovení online |K tomu může dojít z důvodu přechodné chyby ve službě Azure Storage nebo z důvodu tooan výpadku. Zvolte prosím jiný účet úložiště. |
| Obnovení |Bylo dosaženo kvóty skupina prostředků – prosím odstraňte některé skupiny prostředků z portálu Azure nebo požádejte podporu Azure tooincrease hello omezení. |Žádný |
| Obnovení |Vybraná podsíť neexistuje – vyberte podsíť, která již existuje |Žádný |

## <a name="policy"></a>Zásada
| Operace | Podrobnosti o chybě | Alternativní řešení |
| --- | --- | --- |
| Vytvoření zásad |Se nezdařilo toocreate hello zásady – snižte toocontinue volby uchování hello s konfigurací zásad. |Žádný |

## <a name="vm-agent"></a>Agent virtuálního počítače
### <a name="setting-up-hello-vm-agent"></a>Nastavení hello agenta virtuálního počítače
Obvykle hello agenta virtuálního počítače již existuje ve virtuálních počítačích, které jsou vytvořené pomocí hello Galerie Azure. Virtuální počítače, které jsou migrované z místních datových center však nebude mít hello Agent virtuálního počítače nainstalovaný. Pro tyto virtuální počítače musí hello agenta virtuálního počítače toobe explicitně nainstalována. Další informace o [instalaci agenta virtuálního počítače hello na existující virtuální počítač](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx).

Pro virtuální počítače Windows:

* Stáhněte a nainstalujte hello [MSI agenta](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Budete potřebovat instalaci hello toocomplete oprávnění správce.
* [Aktualizovat vlastnosti virtuálního počítače hello](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) tooindicate, který hello agenta je nainstalovaná.

Pro virtuální počítače Linux:

* Nainstalujte nejnovější [agenta systému Linux](https://github.com/Azure/WALinuxAgent) z githubu.
* [Aktualizovat vlastnosti virtuálního počítače hello](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) tooindicate, který hello agenta je nainstalovaná.

### <a name="updating-hello-vm-agent"></a>Aktualizace hello agenta virtuálního počítače
Pro virtuální počítače Windows:

* Hello aktualizace agenta virtuálního počítače je stejně jednoduché jako přeinstalování hello [binárních souborů agenta virtuálního počítače](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Je však třeba tooensure, která při hello Probíhá aktualizace agenta virtuálního počítače neběží žádná operace zálohování.

Pro virtuální počítače Linux:

* Postupujte podle pokynů hello na [aktualizace agenta virtuálního počítače Linux](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="validating-vm-agent-installation"></a>Ověření instalace agenta virtuálního počítače
Jak toocheck pro hello verze agenta virtuálního počítače na virtuálních počítačích Windows:

1. Přihlaste toohello virtuální počítač Azure a přejděte složky toohello *C:\WindowsAzure\Packages*. Byste měli najít přítomný soubor WaAppAgent.exe hello.
2. Klikněte pravým tlačítkem na soubor hello, přejděte příliš**vlastnosti**a potom vyberte hello **podrobnosti** kartě hello verze produktu pole by mělo být 2.6.1198.718 nebo vyšší
