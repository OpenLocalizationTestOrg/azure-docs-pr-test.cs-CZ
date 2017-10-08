---
title: "aaaUse a Linux, řešení potíží s virtuálních počítačů v hello portálu Azure | Microsoft Docs"
description: "Zjistěte, jak hello tootroubleshoot Linux virtuálního počítače problémy pomocí připojování hello operačního systému disku tooa obnovení virtuálního počítače pomocí portálu Azure"
services: virtual-machines-linux
documentationCenter: 
authors: iainfoulds
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/14/2016
ms.author: iainfou
ms.openlocfilehash: 794daa06d7436215af84a61ab9088524254c47df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-linux-vm-by-attaching-hello-os-disk-tooa-recovery-vm-using-hello-azure-portal"></a>Řešení potíží s virtuálního počítače s Linuxem připojením obnovení tooa disku hello operačního systému virtuálního počítače pomocí hello portálu Azure
Pokud systém Linux virtuálního počítače (VM) dojde k chybě spouštěcí nebo disk, může být nutné tooperform řešení potíží s kroky hello virtuální pevný disk sám sebe. Běžným příkladem by neplatná položka v `/etc/fstab` , který brání hello virtuálních počítačů je možné tooboot úspěšně. Tento článek podrobnosti jak toouse hello Azure portálu tooconnect vaše virtuálního pevného disku virtuálního počítače s Linuxem toofix tooanother všechny chyby a potom ho znovu vytvořit původní virtuální počítač.

## <a name="recovery-process-overview"></a>Přehled procesu obnovení
řešení potíží s procesem Hello vypadá takto:

1. Odstraňte hello virtuálních počítačů zjištění problémy, udržování hello virtuální pevné disky.
2. Připojení a připojte tooanother hello virtuální pevný disk virtuálního počítače s Linuxem pro účely odstraňování potíží.
3. Připojte toohello řešení potíží s virtuálních počítačů. Úpravy souborů nebo spuštěním žádné nástroje toofix problémy v hello původní virtuální pevný disk.
4. Odpojte Image a odpojte hello virtuální pevný disk z hello řešení potíží s virtuálních počítačů.
5. Vytvoření virtuálního počítače pomocí hello původní virtuální pevný disk.


## <a name="determine-boot-issues"></a>Určení spouštěcí problémy
Zkontrolujte Diagnostika spouštění hello a toodetermine snímek virtuálního počítače, proč váš virtuální počítač není možné tooboot správně. Běžným příkladem by neplatná položka v `/etc/fstab`, nebo virtuální pevný disk, na kterém se odstranil nebo přesunul.

Vyberte virtuální počítač hello portálu a posuňte se dolů toohello **podporu + Poradce při potížích s** části. Klikněte na tlačítko **spouštění diagnostiky** tooview hello konzoly zprávy pomocí datového proudu vysílána z virtuálního počítače. Pokud můžete zjistit, proč hello virtuálního počítače došlo k problému, protokoly konzoly hello zkontrolujte toosee. Hello následující příklad ukazuje, že virtuální počítač zasekla v automatickém režimu údržby, který vyžaduje ruční zásah:

![Zobrazení virtuálních počítačů Diagnostika spouštění protokoly konzoly](./media/troubleshoot-recovery-disks-portal/boot-diagnostics-error.png)

Můžete také kliknout na **– snímek obrazovky** hello horním okraji hello spouštění diagnostiky protokolu toodownload zachycení hello snímek virtuálního počítače.


## <a name="view-existing-virtual-hard-disk-details"></a>Zobrazení podrobností existující virtuální pevný disk
Než můžete připojit vaše tooanother virtuální pevný disk virtuálního počítače, je třeba název hello tooidentify hello virtuálního pevného disku (VHD). 

Vyberte skupinu prostředků z portálu hello a potom vyberte účet úložiště. Klikněte na tlačítko **objekty BLOB**, jako v hello následující ukázka:

![Vyberte úložiště objektů BLOB](./media/troubleshoot-recovery-disks-portal/storage-account-overview.png)

Obvykle mají kontejner s názvem **virtuální pevné disky** , ukládá virtuální pevné disky. Vyberte kontejner tooview hello seznam virtuálních pevných disků. Poznámka: hello název vašeho virtuálního pevného disku (hello předpona je obvykle hello název vašeho virtuálního počítače):

![Identifikovat virtuální pevný disk v kontejneru úložiště](./media/troubleshoot-recovery-disks-portal/storage-container.png)

Vyberte ze seznamu hello existující virtuální pevný disk a zkopírujte hello URL pro použití v hello následující kroky:

![Zkopírujte adresu URL existující virtuální pevný disk](./media/troubleshoot-recovery-disks-portal/copy-vhd-url.png)


## <a name="delete-existing-vm"></a>Odstraňte existující virtuální počítač
Virtuální pevné disky a virtuální počítače jsou v Azure dva různé prostředky. Virtuální pevný disk je, kde jsou uloženy hello operačního systému, samotné, aplikace a konfigurace. Hello virtuální počítač je jenom metadata, která definuje hello velikosti nebo umístění a odkazuje na prostředky, jako je virtuální pevný disk nebo virtuální síťová karta (NIC). Každý virtuální pevný disk má zapůjčení přiřazen při připojené tooa virtuálních počítačů. I když datových disků můžete připojit a odpojit i hello virtuálních počítačů se systémem, disk operačního systému hello nejde odpojit, pokud se odstraní hello prostředků virtuálního počítače. Hello zapůjčení pokračuje tooassociate hello operačního systému disku v případě virtuálních počítačů i v případě, že tento virtuální počítač je ve stavu Zastaveno a deallocated.

první krok toorecover Hello virtuálního počítače je prostředků virtuálního počítače hello toodelete sám sebe. Odstraňování hello virtuálního počítače zůstane hello virtuální pevné disky ve vašem účtu úložiště. Po hello je odstranit virtuální počítač připojte hello virtuálního pevného disku tooanother virtuálních počítačů tootroubleshoot a vyřešte chyby hello.

Vyberte virtuální počítač hello portálu a pak klikněte na tlačítko **odstranit**:

![Virtuální počítač spouštěcí diagnostiky snímek obrazovky zobrazující chyby spouštěcí](./media/troubleshoot-recovery-disks-portal/stop-delete-vm.png)

Počkejte, dokud hello virtuálních počítačů dokončí odstraňování před připojením tooanother hello virtuální pevný disk virtuálního počítače. Hello zapůjčení na hello virtuální pevný disk, který přidruží k němu hello virtuálních počítačů musí toobe vydán dříve, než je možné připojit virtuální pevný disk tooanother hello virtuálních počítačů.


## <a name="attach-existing-virtual-hard-disk-tooanother-vm"></a>Připojit existující virtuální pevný disk tooanother virtuálních počítačů
Pro hello vedle několik kroků, použijte jiný počítač pro účely odstraňování potíží. Připojte hello existující virtuální pevný disk toothis řešení potíží s možné toobrowse toobe virtuálních počítačů a upravit obsah hello disku. Tento proces vám umožní toocorrect všechny chyby konfigurace nebo zkontrolujte další aplikace nebo systému souborů, například protokolu. Vyberte nebo vytvořte jinou toouse virtuálních počítačů pro účely odstraňování potíží.

1. Vyberte skupinu prostředků z portálu hello a potom vyberte řešení potíží virtuálního počítače. Vyberte **disky** a pak klikněte na **připojit existující**:

    ![Připojit stávající disk hello portálu](./media/troubleshoot-recovery-disks-portal/attach-existing-disk.png)

2. tooselect existující virtuální pevný disk, klikněte na tlačítko **souboru virtuálního pevného disku**:

    ![Vyhledání existujícího VHD procházením](./media/troubleshoot-recovery-disks-portal/select-vhd-location.png)

3. Vyberte účet úložiště a kontejneru a pak klikněte na existující virtuální pevný disk. Klikněte na tlačítko hello **vyberte** tlačítko tooconfirm zvoleného:

    ![Výběr existujícího VHD](./media/troubleshoot-recovery-disks-portal/select-vhd.png)

4. S svůj disk VHD nyní vybraný, klikněte na tlačítko **OK** tooattach hello existující virtuální pevný disk:

    ![Zkontrolujte připojení existujícího virtuálního pevného disku](./media/troubleshoot-recovery-disks-portal/attach-disk-confirm.png)

5. Za několik sekund, hello **disky** podokno pro virtuální počítač obsahuje existující virtuální pevný disk připojený jako datový disk:

    ![Existující virtuální pevný disk připojený jako datový disk](./media/troubleshoot-recovery-disks-portal/attached-disk.png)


## <a name="mount-hello-attached-data-disk"></a>Připojte disk připojená data hello

> [!NOTE]
> Hello následující příklady podrobnosti hello kroky na virtuálního počítače s Ubuntu. Pokud používáte jiný distro Linux, například Red Hat Enterprise Linux nebo SUSE, hello umístění souborů protokolu a `mount` příkazy se můžou mírně lišit. Naleznete v dokumentaci toohello pro vaše konkrétní distro hello příslušné změny v příkazy.

1. Řešení potíží s virtuálního počítače pomocí příslušných přihlašovacích údajů hello tooyour SSH. Pokud tento disk je hello první datový disk připojený tooyour řešení potíží s virtuálních počítačů, je pravděpodobně připojený příliš`/dev/sdc`. Použití `dmseg` toolist připojenými disky:

    ```bash
    dmesg | grep SCSI
    ```
    Hello výstup je podobné toohello následující ukázka:

    ```bash
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
    ```

    V předchozím příkladu hello, je disk hello operačního systému na `/dev/sda` a hello dočasným diskovým zadaná pro každý virtuální počítač je v `/dev/sdb`. Pokud jste měli více datových disků, musí být v `/dev/sdd`, `/dev/sde`a tak dále.

2. Vytvořte adresář toomount existující virtuální pevný disk. Hello následující příklad vytvoří adresář s názvem `troubleshootingdisk`:

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. Pokud máte více oddílů na existující virtuální pevný disk, připojte hello požadované oddílu. Hello následující příklad připojí hello na první primární oddíl `/dev/sdc1`:

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > Osvědčeným postupem je, že toomount datové disky na virtuálních počítačích v Azure pomocí hello identifikátor UUID (UUID) hello virtuálního pevného disku. Pro tento krátký odstraňování potíží není nutné připojování hello virtuální pevný disk pomocí hello UUID. Ale při normálním používání úpravy `/etc/fstab` toomount virtuálních pevných disků pomocí název zařízení, nikoli UUID může způsobit hello tooboot toofail virtuálních počítačů.


## <a name="fix-issues-on-original-virtual-hard-disk"></a>Vyřešte problémy na původní virtuální pevný disk
S hello existující virtuální pevný disk připojit teď můžete dělat žádné údržby a řešení potíží s kroky, podle potřeby. Jakmile jste vyřešili problémy hello, pokračujte hello následující kroky.

## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Odpojte Image a odpojit původní virtuální pevný disk
Jakmile jsou vaše chyby vyřešeny, odpojte hello existující virtuální pevný disk z virtuálního počítače řešení potíží. Virtuální pevný disk s jiných virtuálních počítačů nelze používat, dokud vydání hello zapůjčení připojení hello virtuálního pevného disku toohello řešení potíží s virtuálních počítačů.

1. Z relace tooyour SSH hello řešení potíží virtuální počítač odpojte hello existující virtuální pevný disk. Nejprve změňte mimo hello nadřazený adresář pro přípojného bodu:

    ```bash
    cd /
    ```

    Nyní odpojte hello existující virtuální pevný disk. Hello následující příklad odpojí hello zařízení na `/dev/sdc1`:

    ```bash
    sudo umount /dev/sdc1
    ```

2. Nyní odpojte hello virtuálního pevného disku z hello virtuálních počítačů. Vyberte virtuální počítač hello portálu a klikněte na **disky**. Vyberte existující virtuální pevný disk a potom klikněte na **odpojení**:

    ![Odpojte existující virtuální pevný disk](./media/troubleshoot-recovery-disks-portal/detach-disk.png)

    Počkejte, dokud hello virtuálního počítače úspěšně odpojil hello datový disk než budete pokračovat.

## <a name="create-vm-from-original-hard-disk"></a>Vytvoření virtuálního počítače z původního pevného disku
použijte virtuální počítač z původní virtuální pevný disk, toocreate [této šablony Azure Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet). Šablona Hello nasadí virtuální počítač do existující virtuální síť pomocí hello adresu URL VHD z hello dříve příkaz. Klikněte na tlačítko hello **nasazení tooAzure** tlačítko následujícím způsobem:

![Nasazení virtuálních počítačů z šablony z Githubu](./media/troubleshoot-recovery-disks-portal/deploy-template-from-github.png)

Šablona Hello je načten do hello portál Azure pro nasazení. Zadejte názvy hello pro nový virtuální počítač a prostředky existující Azure a vložte hello URL tooyour existující virtuální pevný disk. toobegin hello nasazení, klikněte na tlačítko **nákupu**:

![Nasazení virtuálního počítače ze šablony](./media/troubleshoot-recovery-disks-portal/deploy-from-image.png)


## <a name="re-enable-boot-diagnostics"></a>Opětovné povolení Diagnostika spouštění
Při vytváření virtuálního počítače z hello existující virtuální pevný disk, Diagnostika spouštění není automaticky povolené. toocheck hello stav Diagnostika spouštění a zapnout, vyberte virtuální počítač hello portálu v případě potřeby. V části **monitorování**, klikněte na tlačítko **nastavení diagnostiky**. Zkontrolujte stav hello **na**, a hello zaškrtnutí vedle příliš**spouštění diagnostiky** je vybrána. Pokud provedete změny, klikněte na tlačítko **Uložit**:

![Aktualizovat nastavení diagnostiky spouštění](./media/troubleshoot-recovery-disks-portal/reenable-boot-diagnostics.png)

## <a name="next-steps"></a>Další kroky
Pokud máte problémy s připojením tooyour virtuálních počítačů, přečtěte si téma [řešení SSH připojení tooan virtuálního počítače Azure](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Problémy s přístupem k aplikacím spuštěným na vašem virtuálním počítači najdete v tématu [problémů s připojením aplikace na virtuální počítač s Linuxem](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Další informace o používání správce prostředků najdete v tématu [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
