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
ms.openlocfilehash: ae7c345c11a7db25413d60ad822f16f84ca37362
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="recover-files-from-azure-virtual-machine-backup"></a>Obnovit soubory ze zálohy virtuálního počítače Azure

Azure backup poskytuje možnosti obnovení [disky a virtuální počítače Azure](./backup-azure-arm-restore-vms.md) ze záloh virtuálních počítačů Azure. Teď tento článek vysvětluje, jak můžete položek, jako jsou soubory a složky obnovit ze zálohy virtuálního počítače Azure.

> [!Note]
> Tato funkce je k dispozici pro virtuální počítače Azure nasazení pomocí modelu Resource Manager a chránit do trezoru služeb zotavení.
> Obnovení souborů ze šifrovaných zálohy virtuálního počítače není podporováno.
>

## <a name="mount-the-volume-and-copy-files"></a>Připojte svazek a zkopírujte soubory

1. Přihlaste se k webu [Azure Portal](http://portal.Azure.com). Najděte příslušné trezoru služeb zotavení a bude požadovaná položka zálohování.

2. V okně Zálohování položky, klikněte na **obnovení souborů**

    ![Otevřete položky zálohování trezoru služeb zotavení](./media/backup-azure-restore-files-from-vm/open-vault-item.png)

    **Obnovení souboru** otevře se okno.

    ![Okno obnovení souboru](./media/backup-azure-restore-files-from-vm/file-recovery-blade.png)

3. Z **vyberte bod obnovení** rozevírací nabídky vyberte bod obnovení, který obsahuje soubory, které chcete. Ve výchozím nastavení je již vybrán poslední bod obnovení.

4. Klikněte na tlačítko **spustitelný soubor stáhnout** (pro virtuální počítač Windows Azure) nebo **stáhnout skript** (pro virtuální počítač Azure s Linuxem) ke stažení softwaru, které budete používat pro kopírování souborů z bodu obnovení.

  Spustitelný soubor nebo skript vytvoří připojení mezi místní počítač a pro zadaný bod obnovení.

5. Je třeba heslo ke spuštění staženého skriptu nebo spustitelného souboru. Heslo můžete zkopírovat z portálu pomocí tlačítko Kopírovat, pokud vedle vygenerované heslo

    ![Vygenerované heslo](./media/backup-azure-restore-files-from-vm/generated-pswd.png)

6. Na počítači, ve které chcete obnovit soubory spusťte skript nebo spustitelný soubor. Je nutné jej spustit pomocí přihlašovacích údajů správce. Pokud spustíte skript na počítači s omezeným přístupem, ujistěte se, je přístup k:

    - download.microsoft.com
    - Koncové body Azure použít pro zálohování virtuálních počítačů Azure
    - odchozí port 3260

   Pro systémy Linux vyžaduje skript 'open-iscsi' a 'lshw' součásti pro připojení k bodu obnovení. Pokud těch, které nejsou k dispozici na počítači, na kterém je spuštěná, požádá o oprávnění k instalaci příslušné komponenty a nainstaluje je po souhlasu.
   
   Zadejte heslo z portálu po zobrazení výzvy zkopírovali. Jakmile je zadáno platné heslo skripty se spojí s bodem obnovení.
      
    ![Okno obnovení souboru](./media/backup-azure-restore-files-from-vm/executable-output.png)
    
   
   Tento skript můžete spustit na jakýkoli počítač, který má operační systém stejného (nebo kompatibilní) jako zálohované virtuální počítač. Najdete v článku [kompatibilní operační systém tabulky](backup-azure-restore-files-from-vm.md#compatible-os) pro kompatibilní operační systémy. Pokud chráněný virtuální počítač Azure používá prostory úložiště ve Windows (pro virtuální počítače Windows Azure) nebo Arrays(for Linux VMs) LVM/RAID, nebudete moci na stejném virtuálním počítači spustit spustitelný soubor nebo skript. Místo toho jej spusťte v jiným počítačem s kompatibilní operační systém.

### <a name="compatible-os"></a>Kompatibilní operační systém

#### <a name="for-windows"></a>Pro Windows

V následující tabulce jsou uvedeny kompatibilitu mezi serverem a počítačem operační systémy. Při obnovování souborů, nelze obnovit soubory mezi operační systémy nekompatibilní.

|OS serveru | Kompatibilní klientského operačního systému  |
| --------------- | ---- |
| Windows Server 2012 R2 | Windows 8.1 |
| Windows Server 2012    | Windows 8  |
| Windows Server 2008 R2 | Windows 7   |

#### <a name="for-linux"></a>Pro Linux

V systému Linux základní požadavek je, že by měl operačního systému počítače, kde se skript spouští podporují systém souborů v virtuálního počítače s Linuxem zálohované soubory. Při výběru počítače ke spuštění skriptu, ujistěte se, že má kompatibilní operační systém a verze, jak je uvedeno v následující tabulce.

|Linux operačního systému | Verze  |
| --------------- | ---- |
| Ubuntu | 12.04 a vyšší |
| CentOS | verze 6.5 a vyšší  |
| RHEL | 6.7 a vyšší |
| Debian | 7 a vyšší |
| Oracle Linux | 6.4 a vyšší |

Skript také vyžaduje python a bash komponent ke spouštění a bezpečně připojit k bodu obnovení.

|Komponenta | Verze  |
| --------------- | ---- |
| Bash | 4 a novější |
| python | 2.6.6 a vyšší  |


### <a name="identifying-volumes"></a>Identifikace svazky

#### <a name="for-windows"></a>Pro Windows

Při spuštění exectuable připojí nové svazky operačního systému a přiřadí písmena jednotek. Můžete procházet tyto jednotky Průzkumníka Windows nebo Průzkumníka souborů. Písmena jednotek přiřazená k daným svazkům nesmí být stejná písmena jako původní virtuální počítač, ale je zachováno název svazku. Například, pokud byl svazek v původní virtuální počítač "datový Disk (E:\)", že svazek může být připojené jako "datový Disk ("Kterékoli písmeno jednotky k dispozici":\) v místním počítači. Procházejte všechny svazky uvedeno ve výstupu skriptu vyhledejte soubory nebo složku.  
       
   ![Okno obnovení souboru](./media/backup-azure-restore-files-from-vm/volumes-attached.png)
           
#### <a name="for-linux"></a>Pro Linux

V systému Linux svazky bodu obnovení jsou připojené do složky, kde je skript spuštěn. Připojené disky, svazky a odpovídající cesty připojení se zobrazí odpovídajícím způsobem. Tyto cesty připojení jsou viditelné pro uživatele s kořenové úrovně přístupu. Procházejte svazky uvedeno ve výstupu skriptu.

  ![Okno obnovení souboru Linux](./media/backup-azure-restore-files-from-vm/linux-mount-paths.png)
  

## <a name="closing-the-connection"></a>Probíhá ukončování připojení

Po identifikování souborů a jejich kopírování do umístění místního úložiště, odeberte (nebo odpojit) další jednotky. Odpojit jednotky, na **obnovení souboru** na portálu Azure klikněte na **odpojit disky**.

![Odpojení disky](./media/backup-azure-restore-files-from-vm/unmount-disks3.png)

Po disky se odpojily, zobrazí se, že byla úspěšná. To může trvat pár minut pro připojení k aktualizaci, které umožní odstranit disky.

V systému Linux po připojení k bodu obnovení je porušeno, operačního systému nemá odpovídající cesty připojení automaticky odebrat. Tyto existují jako "oddělena" svazky a že jsou viditelné, ale vyvolá chybu, když je přístup a zápis souborů. Je lze ručně odebrat. Skript, při spuštění, identifikuje všechny tyto svazky existující z jakékoli předchozí body obnovení a je vyčistí po souhlasu.

## <a name="special-configurations"></a>Zvláštní konfigurace

### <a name="dynamic-disks"></a>Dynamické disky

Pokud virtuální počítač Azure, která byla zálohována má svazky, které jsou rozmístěny v několika disků (předané a prokládané svazky) nebo odolné proti chybám svazky (RAID-5 a zrcadlené) na dynamické disky, nelze spustit spustitelný soubor skriptu na stejného virtuálního počítače. Místo toho spusťte spustitelný soubor skriptu z jakéhokoli počítače s kompatibilní operační systém.

### <a name="windows-storage-spaces"></a>Prostory úložiště ve Windows

Prostory úložiště ve Windows je technologie v úložišti systému Windows, která umožňuje Virtualizovat úložiště. Prostory úložiště ve Windows můžete seskupit standardní disky do fondů úložiště a pak vytvořit virtuálních disků, nazývaných prostory úložiště z dostupné místo v těchto fondů úložiště.

Pokud virtuální počítač Azure, která byla zálohována používá prostory úložiště ve Windows, nebudete moci na stejný virtuální počítač spustit spustitelný soubor skriptu. Místo toho spusťte spustitelný soubor skriptu z jakéhokoli počítače s kompatibilní operační systém.

### <a name="lvmraid-arrays"></a>Pole LVM/RAID

V systému Linux Správce logických svazků (LVM) nebo softwaru pole RAID se používají ke správě logické svazky přes několik disků. Pokud zálohovaných virtuálního počítače s Linuxem používá LVM nebo pole RAID, nelze spustit skript v stejného virtuálního počítače. Místo toho spuštění skriptu na jiné počítače s operačním systémem kompatibilní a který podporuje filesystem zálohovaných virtuálního počítače.

Výstup skriptu zobrazí disky LVM nebo pole RAID a svazky s typem oddílu, jak je uvedeno níže

   ![Okno výstup LVM Linux](./media/backup-azure-restore-files-from-vm/linux-LVMOutput.png)
   
Následující příkazy nutné spustit uživatel, aby tyto oddíly online. 

**Pro LVM oddíly**

```
$ pvs <volume name as shown above in the script output> 
```
Rutina Vypíše seznam názvů skupin svazku v části fyzický svazku.

```
$ lvdisplay <volume-group-name from the pvs command’s results> 
```
Rutina Vypíše seznam všech logických svazků, názvy a jejich cesty ve skupině svazku.

```
$ mount <LV path> </mountpath>
```
Chcete-li připojit logické svazky na cestu podle svého výběru.


**Pro pole RAID**

```
$ mdadm –detail –scan
```
Zobrazí podrobnosti o všech disků raid. Zobrazí se příslušné disku RAID jako`/dev/mdm/<RAID array name in the backed up VM>`

Pokud RAID disk fyzických svazků, použijte příkaz připojení.
```
$ mount [RAID Disk Path] [/mounthpath]
```

Pokud tento disk RAID má jiné LVM v nich konfigurovali potom postupujte stejným způsobem, jak je uvedeno výše pro LVM oddíly s názvem svazku probíhá na název disku diskového pole RAID

## <a name="troubleshooting"></a>Řešení potíží

Pokud máte potíže při obnovování soubory z virtuálních počítačů, zkontrolujte následující tabulce najdete další informace.

| Chybová zpráva nebo scénáře | Pravděpodobná příčina | Doporučená akce |
| ------------------------ | -------------- | ------------------ |
| Výstupní soubor EXE: *výjimka připojení k cíli* |Skript není možné získat přístup k bodu obnovení | Zkontrolujte, zda počítač splňuje všechny požadavky na přístup zmíněné|  
|   Výstupní soubor EXE: *cíl již byla zaznamenána prostřednictvím relace ISCSI.* | Skript byl již spuštěn na stejném počítači a byly připojeny jednotky | Již byly připojeny svazků bodu obnovení. Může připojených není s stejná písmena jednotek původní virtuálního počítače. Procházet všechny svazky, které jsou k dispozici v Průzkumníku souborů souboru |
| Výstupní soubor EXE: *tento skript je neplatný, protože disky mají byly odpojeny prostřednictvím 12-hr portál nebo překročil limit. Stáhněte si prosím nový skript z portálu.* |  Disky mají byla odpojena z portálu nebo byl překročen limit 12 hr |    Tato konkrétní exe je neplatný a nelze spustit. Pokud chcete přístup k souborům tohoto obnovení bodu v čase, přejděte na portálu pro nové exe|
| Na počítači, kde se spouští exe: nové svazky nejsou odpojeny po kliknutí na tlačítko odpojení |    Iniciátor ISCSI na počítači není reagovat nebo obnovení připojení k cíli a údržbu mezipaměti |    Počkejte několik minut, po stisknutí tlačítka odpojení. Pokud ještě nejsou odpojeny nové svazky, přejděte prosím prostřednictvím všechny svazky. Vynutí se tak iniciátoru aktualizovat připojení a svazek je odpojený s chybovou zprávou, disk není k dispozici|
| Výstupní soubor EXE: skript je spuštěn úspěšně, ale "Nové svazky připojené" se nezobrazí na výstup skriptu | Toto je přechodné chybě.   | Svazky by byly již připojeny. Otevřete Průzkumníka a přejděte. Pokud ke spouštění skriptů pokaždé, když používáte stejný počítač, zvažte, restartování počítače a v seznamu má být zobrazena v běží následné exe. |
| Konkrétní Linux: Nepodařilo se zobrazit požadované svazky | Operačního systému počítače, kde se skript spouští nemusí rozpoznat základní systém souborů zálohovaných virtuálního počítače | Zkontrolujte, zda bod obnovení je havárií konzistentní nebo konzistentními soubory. Pokud soubor konzistentní, spusťte skript na jiném počítači jejichž operační systém rozpozná zálohy zálohu systému souborů Virtuálního počítače |
| Specifické pro Windows: Nepodařilo se zobrazit požadované svazky | Jsou disky připojené, ale nebyly nakonfigurované svazky | Na obrazovce Správa disků zjistěte, které další disky týkající se bodu obnovení. Pokud jsou některé z těchto disků v režimu offline stavu zkuste přitom online kliknutím pravým tlačítkem na disk a klikněte na tlačítko 'Online.|
