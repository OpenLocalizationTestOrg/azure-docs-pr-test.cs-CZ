---
title: "Zálohování Azure: Obnovovat soubory a složky ze zálohy virtuálního počítače Azure | Microsoft Docs"
description: "Obnovit soubory z bodu obnovení virtuálního počítače Azure"
services: backup
documentationcenter: dev-center-name
author: pvrk
manager: shivamg
keywords: "obnovení na úrovni položek; obnovení souborů ze zálohy virtuálního počítače Azure; Obnovit soubory z virtuálního počítače Azure"
ms.assetid: f1c067a2-4826-4da4-b97a-c5fd6c189a77
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/20/2017
ms.author: pullabhk;markgal
ms.openlocfilehash: 1a62a0ed83d61272c032ac0377a54099ed118db4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="recover-files-from-azure-virtual-machine-backup"></a>Obnovit soubory ze zálohy virtuálního počítače Azure

Azure backup poskytuje schopnost toorestore hello [disky a virtuální počítače Azure](./backup-azure-arm-restore-vms.md) ze záloh virtuálních počítačů Azure. Teď tento článek vysvětluje, jak můžete položek, jako jsou soubory a složky obnovit ze zálohy virtuálního počítače Azure.

> [!Note]
> Tato funkce je k dispozici pro nasazení pomocí modelu Resource Manager hello a chráněné tooa trezoru služeb zotavení virtuální počítače Azure.
> Obnovení souborů ze šifrovaných zálohy virtuálního počítače není podporováno.
>

## <a name="mount-hello-volume-and-copy-files"></a>Připojte svazek a zkopírujte soubory hello

1. Přihlaste se k hello [portál Azure](http://portal.Azure.com). Vyhledá hello relevantní trezoru služeb zotavení a zálohování položek hello vyžaduje.

2. V okně hello zálohované položky, klikněte na tlačítko **obnovení souborů**

    ![Otevřete položky zálohování trezoru služeb zotavení](./media/backup-azure-restore-files-from-vm/open-vault-item.png)

    Hello **obnovení souboru** otevře se okno.

    ![Okno obnovení souboru](./media/backup-azure-restore-files-from-vm/file-recovery-blade.png)

3. Z hello **vyberte bod obnovení** rozevírací nabídky, vyberte hello bod obnovení, který obsahuje soubory hello chcete. Ve výchozím nastavení je už vybraná hello nejnovější bod obnovení.

4. Klikněte na tlačítko **spustitelný soubor stáhnout** (pro virtuální počítač Windows Azure) nebo **stáhnout skript** (pro virtuální počítač Azure s Linuxem) toodownload hello softwaru použijete toocopy soubory z bodu obnovení hello.

  Hello spustitelný soubor nebo skript vytvoří připojení mezi hello místního počítače a hello Zadaný bod obnovení.

5. Je nutné heslo toorun hello stáhnout skript nebo spustitelný soubor. Hello heslo můžete zkopírovat z portálu hello pomocí tlačítka kopírování hello vedle hello vygenerované heslo

    ![Vygenerované heslo](./media/backup-azure-restore-files-from-vm/generated-pswd.png)

6. Na počítači hello místo toorecover hello soubory spusťte hello spustitelný soubor nebo skript. Je nutné jej spustit pomocí přihlašovacích údajů správce. Pokud spustíte skript hello na počítači s omezeným přístupem, ujistěte se, je přístup k:

    - download.microsoft.com
    - Koncové body Azure použít pro zálohování virtuálních počítačů Azure
    - odchozí port 3260

   Pro systémy Linux vyžaduje skript hello 'open-iscsi' a 'lshw' součásti bodu obnovení toohello tooconnect. Pokud neexistují těch, na kterém je spuštěná počítači hello, požádá o oprávnění tooinstall hello příslušné komponenty a nainstaluje je po souhlasu.
   
   Zadejte heslo hello zkopírovat z portálu hello po zobrazení výzvy. Jakmile je zadáno platné heslo hello hello skripty připojí toohello bod obnovení.
      
    ![Okno obnovení souboru](./media/backup-azure-restore-files-from-vm/executable-output.png)
    
   
   Hello skript můžete spustit na jakýkoli počítač, který má hello stejného (nebo kompatibilní) operačního systému jako hello zálohy virtuálních počítačů. V tématu hello [kompatibilní operační systém tabulky](backup-azure-restore-files-from-vm.md#compatible-os) pro kompatibilní operační systémy. Pokud hello chráněný Azure virtuální počítač používá prostory úložiště ve Windows (pro virtuální počítače Windows Azure) nebo Arrays(for Linux VMs) LVM/RAID a pak nelze spustit hello spustitelný soubor nebo skript na hello stejného virtuálního počítače. Místo toho jej spusťte v jiným počítačem s kompatibilní operační systém.

### <a name="compatible-os"></a>Kompatibilní operační systém

#### <a name="for-windows"></a>Pro Windows

Následující tabulka ukazuje hello kompatibilitu mezi serverem a operační systémy počítačů Hello. Při obnovování souborů, nelze obnovit soubory mezi operační systémy nekompatibilní.

|OS serveru | Kompatibilní klientského operačního systému  |
| --------------- | ---- |
| Windows Server 2012 R2 | Windows 8.1 |
| Windows Server 2012    | Windows 8  |
| Windows Server 2008 R2 | Windows 7   |

#### <a name="for-linux"></a>Pro Linux

V systému Linux, základní požadavek hello je, že hello OS hello počítače, kde je spuštěna hello skriptu by měly podporovat hello filesystem Dobrý den soubory, které jsou součástí hello zálohy virtuálního počítače s Linuxem. Při výběru skript hello toorun počítač, zkontrolujte, zda že má hello kompatibilní operační systém a hello verze, jak je uvedeno v tabulce hello.

|Linux operačního systému | Verze  |
| --------------- | ---- |
| Ubuntu | 12.04 a vyšší |
| CentOS | verze 6.5 a vyšší  |
| RHEL | 6.7 a vyšší |
| Debian | 7 a vyšší |
| Oracle Linux | 6.4 a vyšší |

Hello skript také vyžaduje python a bash tooexecute součásti a zabezpečené připojení toohello bod obnovení.

|Komponenta | Verze  |
| --------------- | ---- |
| Bash | 4 a novější |
| python | 2.6.6 a vyšší  |


### <a name="identifying-volumes"></a>Identifikace svazky

#### <a name="for-windows"></a>Pro Windows

Při spuštění hello exectuable hello operační systém připojí hello nové svazky a přiřadí písmena jednotek. Toobrowse Průzkumníka Windows nebo Průzkumníka souborů můžete používat tyto jednotky. Hello písmena jednotek, které jsou přiřazené toohello svazky nemusí být hello, které se zachovala stejná písmena jako hello původní virtuální počítač, ale hello název svazku. Například pokud hello svazek na hello původní virtuální počítač byl "datový Disk (E:\)", že svazek může být připojené jako "datový Disk ('kterékoli písmeno jednotky k dispozici.:\) hello místního počítače. Procházejte všechny svazky uvedeno ve výstupu skriptu hello vyhledejte soubory nebo složku.  
       
   ![Okno obnovení souboru](./media/backup-azure-restore-files-from-vm/volumes-attached.png)
           
#### <a name="for-linux"></a>Pro Linux

V systému Linux jsou hello svazků bodu obnovení hello připojené toohello složku, kde spuštění skriptu hello. Hello připojené disky, svazky a hello odpovídající připojení, které cesty se zobrazují odpovídajícím způsobem. Tyto cesty připojení jsou viditelné toousers kořenové úrovně přístupu. Procházejte svazky hello uvedeno ve výstupu skriptu hello.

  ![Okno obnovení souboru Linux](./media/backup-azure-restore-files-from-vm/linux-mount-paths.png)
  

## <a name="closing-hello-connection"></a>Zavřením hello připojení

Až určíte hello souborů a jejich kopírování tooa místní úložiště umístění, odeberte (nebo odpojit) hello další jednotky. toounmount hello jednotky, na hello **obnovení souboru** v hello portál Azure, klikněte na **odpojit disky**.

![Odpojení disky](./media/backup-azure-restore-files-from-vm/unmount-disks3.png)

Po hello disky se odpojily, zobrazí se, že byla úspěšná. Ho může trvat několik minut, než toorefresh hello připojení, které umožní odstranit hello disky.

V systému Linux poté, co je bod obnovení toohello připojení hello porušeno, hello OS neodstraní hello odpovídající připojení cesty automaticky. Tyto existují jako "oddělena" svazky a že jsou viditelné, ale vyvolá chybu, když je přístup a zápis souborů hello. Je lze ručně odebrat. skript Hello, pokud spuštěn, identifikuje všechny tyto svazky existující z jakékoli předchozí body obnovení a je vyčistí po souhlasu.

## <a name="special-configurations"></a>Zvláštní konfigurace

### <a name="dynamic-disks"></a>Dynamické disky

Pokud hello virtuálního počítače Azure, která byla zálohována svazky, které zahrnují více disků (rozložené a prokládané svazky) nebo odolné proti chybám svazky (RAID-5 a zrcadlené) na dynamické disky, pak hello spustitelný soubor skriptu nelze spustit hello stejného virtuálního počítače. Místo toho spusťte spustitelný soubor skriptu hello z jakéhokoli počítače s kompatibilní operační systém.

### <a name="windows-storage-spaces"></a>Prostory úložiště ve Windows

Prostory úložiště ve Windows je technologie ve Windows storage, který vám umožní toovirtualize úložiště. Prostory úložiště ve Windows můžete seskupit standardní disky do fondů úložiště a pak vytvořit virtuálních disků, nazývaných prostory úložiště z hello dostupné místo v těchto fondů úložiště.

Pokud hello virtuálního počítače Azure, která byla zálohována používá prostory úložiště Windows hello spustitelný soubor skriptu nelze spustit hello stejného virtuálního počítače. Místo toho spusťte spustitelný soubor skriptu hello z jakéhokoli počítače s kompatibilní operační systém.

### <a name="lvmraid-arrays"></a>Pole LVM/RAID

V systému Linux, Správce logických svazků (LVM) nebo softwaru pole RAID jsou logické svazky používané toomanage přes několik disků. Pokud hello zálohovat virtuální počítač s Linuxem používá LVM nebo pole RAID, nelze spustit skript hello na hello stejného virtuálního počítače. Místo toho spusťte skript hello z jakéhokoli počítače s operačním systémem kompatibilní a který podporuje filesystem hello zálohovat virtuální počítač.

výstup skriptu Hello zobrazí hello LVM nebo disky pole RAID a svazky hello s typem oddílu hello, jak je uvedeno níže

   ![Okno výstup LVM Linux](./media/backup-azure-restore-files-from-vm/linux-LVMOutput.png)
   
Hello následující příkazy, třeba toobe spustit hello uživatele toobring tyto oddíly v režimu online. 

**Pro LVM oddíly**

```
$ pvs <volume name as shown above in hello script output> 
```
Rutina Vypíše seznam názvy skupin hello svazku v části fyzický svazku.

```
$ lvdisplay <volume-group-name from hello pvs command’s results> 
```
Rutina Vypíše seznam všech logických svazků, názvy a jejich cesty ve skupině svazku.

```
$ mount <LV path> </mountpath>
```
toomount hello logické svazky toohello cesta podle svého výběru.


**Pro pole RAID**

```
$ mdadm –detail –scan
```
Zobrazí podrobnosti o všech disků raid. Hello relevantní RAID disku se zobrazí jako`/dev/mdm/<RAID array name in hello backed up VM>`

Pokud hello RAID disk fyzických svazků, použijte příkaz připojení hello.
```
$ mount [RAID Disk Path] [/mounthpath]
```

Pokud tento disk RAID má jiné LVM v nich konfigurovali potom postupovat podle hello stejným způsobem, jak je uvedeno výše pro LVM oddíly s názvem svazku hello se název disku RAID hello

## <a name="troubleshooting"></a>Řešení potíží

Pokud máte potíže při obnovování souborů z hello virtuálních počítačů, zkontrolujte hello následující tabulka pro další informace.

| Chybová zpráva nebo scénáře | Pravděpodobná příčina | Doporučená akce |
| ------------------------ | -------------- | ------------------ |
| Výstupní soubor EXE: *výjimky připojení toohello cíl* |Skript není možné tooaccess bod obnovení hello | Zkontrolujte, zda hello počítač splňuje všechny výše uvedené požadavky na přístup hello|  
|   Výstupní soubor EXE: *hello cíl již byla zaznamenána prostřednictvím relace ISCSI.* |   skript Hello již byl proveden v hello, které byly připojeny stejný počítač a hello jednotky |   již byly připojeny Hello svazků bodu obnovení hello. Může není připojen s hello stejná písmena z jednotek hello původní virtuální počítač. Procházet všech dostupných svazků hello v Průzkumníku souborů hello souboru |
| Výstupní soubor EXE: *tento skript je neplatný, protože hello disky mají byly odpojeny prostřednictvím portálu nebo překročil hello 12 hr limit. Stáhněte si prosím nový skript z portálu hello.* |    Hello disky mají byla odpojena z portálu hello nebo byl překročen limit 12 hr hello |  Tato konkrétní exe je neplatný a nelze spustit. Pokud budete chtít tooaccess hello soubory tohoto obnovení bodu v čase, navštivte hello portál pro nové exe|
| Na počítači hello, kde je spuštěna hello exe: hello nové svazky nejsou odpojeny po kliknutí na tlačítko odpojení hello |    Hello iniciátor ISCSI na počítači hello není cíli toohello připojení neodpovídá nebo aktualizace a údržba hello mezipaměti |    Počkejte několik minut, po stisknutí tlačítka odpojení hello. Pokud ještě nejsou odpojeny hello nové svazky, přejděte prosím prostřednictvím všechny svazky hello. Tato vynutí, které hello iniciátor toorefresh hello připojení a hello svazek je odpojený s chybovou zprávou, které hello disku není k dispozici|
| Výstupní soubor EXE: skript je spuštěn úspěšně, ale "Nové svazky připojené" se nezobrazí na výstup skriptu hello |   Toto je přechodné chybě.   | Hello svazky by byly již připojeny. Otevřete Průzkumníka toobrowse. Pokud ke spouštění skriptů pokaždé, když používáte hello stejný počítač, zvažte, restartování počítače hello a seznamu hello má být zobrazena v běží následné exe hello. |
| Konkrétní Linux: nebylo možné tooview hello potřeby svazky | Hello OS hello počítači, kde se spouští skript hello nemusí rozpoznat základní filesystem hello Dobrý den, zálohovat virtuální počítač | Kontrola, zda bod obnovení hello je havárií konzistentní nebo konzistentními soubory. Pokud soubor hello konzistentní, spusťte skript na jiném počítači, jejichž operační systém rozpozná hello zálohovat systém souborů Virtuálního počítače |
| Specifické pro Windows: nebylo možné tooview hello potřeby svazky | Hello disky připojené, ale nebyly nakonfigurované hello svazky | Z úvodní obrazovka správu disku určete bod obnovení související toohello hello dalších disků. Pokud jsou některé z těchto disků v režimu offline stavu zkuste přitom online kliknutím pravým tlačítkem na hello disk a klikněte na tlačítko 'Online.|
