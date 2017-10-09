---
title: "aaaUse a Windows řešení potíží s virtuálních počítačů v hello portálu Azure | Microsoft Docs"
description: "Zjistěte, jak hello tootroubleshoot problémy Windows virtuálního počítače v Azure pomocí připojování hello operačního systému disku tooa obnovení virtuálního počítače pomocí portálu Azure"
services: virtual-machines-windows
documentationCenter: 
authors: genlin
manager: timlt
editor: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 396f70338fa39f80bb9adcb9244d3c83f2a233eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-windows-vm-by-attaching-hello-os-disk-tooa-recovery-vm-using-hello-azure-portal"></a>Řešení potíží s virtuální počítač s Windows hello operačního systému disku tooa obnovení virtuálního počítače připojením pomocí hello portálu Azure
Pokud Windows virtuálního počítače (VM) v prostředí Azure dojde k chybě spouštěcí nebo disk, může být nutné tooperform řešení potíží s kroky hello virtuální pevný disk sám sebe. Běžným příkladem bude aplikaci, která selhala aktualizace, která brání hello virtuální počítač schopný tooboot úspěšně. Tento článek podrobnosti jak toouse hello Azure portálu tooconnect vaše virtuálního pevného disku virtuálního počítače s Windows toofix tooanother všechny chyby a potom ho znovu vytvořit původní virtuální počítač.

## <a name="recovery-process-overview"></a>Přehled procesu obnovení
řešení potíží s procesem Hello vypadá takto:

1. Odstraňte hello virtuálních počítačů zjištění problémy, udržování hello virtuální pevné disky.
2. Připojení a připojte tooanother hello virtuální pevný disk virtuálního počítače s Windows pro účely odstraňování potíží.
3. Připojte toohello řešení potíží s virtuálních počítačů. Úpravy souborů nebo spuštěním žádné nástroje toofix problémy v hello původní virtuální pevný disk.
4. Odpojte Image a odpojte hello virtuální pevný disk z hello řešení potíží s virtuálních počítačů.
5. Vytvoření virtuálního počítače pomocí hello původní virtuální pevný disk.


## <a name="determine-boot-issues"></a>Určení spouštěcí problémy
toodetermine proč virtuálního počítače není možné tooboot správně, zkontrolujte Diagnostika spouštění hello snímek virtuálního počítače. Běžným příkladem by být aktualizaci selhání aplikace nebo virtuální pevný disk, na kterém se odstranil nebo přesunul.

Vyberte virtuální počítač hello portálu a posuňte se dolů toohello **podporu + Poradce při potížích s** části. Klikněte na tlačítko **spouštění diagnostiky** tooview hello snímek. Poznámka: všechny specifické chybové zprávy nebo kódy chyb toohelp zjistit, proč je hello virtuálních počítačů zjištění problému. Hello následující příklad ukazuje čekání na zastavení služeb virtuálního počítače:

![Zobrazení virtuálních počítačů Diagnostika spouštění protokoly konzoly](./media/troubleshoot-recovery-disks-portal/screenshot-error.png)

Můžete také kliknout na **– snímek obrazovky** toodownload zachycení hello snímek virtuálního počítače.


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

1. Otevřete tooyour připojení ke vzdálené ploše virtuálního počítače. Vyberte virtuální počítač hello portálu a klikněte na **Connect**. Stáhněte a otevřete soubor připojení RDP hello. Zadejte vaše přihlašovací údaje toolog v tooyour virtuální počítač následujícím způsobem:

    ![Přihlaste se tooyour virtuálního počítače pomocí vzdálené plochy](./media/troubleshoot-recovery-disks-portal/open-remote-desktop.png)

2. Otevřete **správce serveru**, pak vyberte **Souborová služba a služba úložiště**. 

    ![Vyberte Souborová služba a služba úložiště ve Správci serveru](./media/troubleshoot-recovery-disks-portal/server-manager-select-storage.png)

3. Hello datový disk je automaticky zjistil a připojené. toosee seznam hello připojené disky, vyberte **disky**. Můžete vybrat data tooview svazku informace o disku, včetně hello písmeno jednotky. Následující příklad ukazuje hello připojit datový disk a pomocí Hello **F:**:

    ![Disk připojený a informace o svazku ve Správci serveru](./media/troubleshoot-recovery-disks-portal/server-manager-disk-attached.png)


## <a name="fix-issues-on-original-virtual-hard-disk"></a>Vyřešte problémy na původní virtuální pevný disk
S hello existující virtuální pevný disk připojit teď můžete dělat žádné údržby a řešení potíží s kroky, podle potřeby. Jakmile jste vyřešili problémy hello, pokračujte hello následující kroky.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Odpojte Image a odpojit původní virtuální pevný disk
Jakmile jsou vaše chyby vyřešeny, odpojte hello existující virtuální pevný disk z virtuálního počítače řešení potíží. Virtuální pevný disk s jiných virtuálních počítačů nelze používat, dokud vydání hello zapůjčení připojení hello virtuálního pevného disku toohello řešení potíží s virtuálních počítačů.

1. Hello RDP relace tooyour virtuálních počítačů, otevřete **správce serveru**, pak vyberte **Souborová služba a služba úložiště**:

    ![Vyberte Souborová služba a služba úložiště ve Správci serveru](./media/troubleshoot-recovery-disks-portal/server-manager-select-storage.png)

2. Vyberte **disky** a pak vyberte datový disk. Klikněte pravým tlačítkem na datový disk a vyberte **převést do offline režimu**:

    ![Nastavte hello datový disk jako offline ve Správci serveru](./media/troubleshoot-recovery-disks-portal/server-manager-set-disk-offline.png)

3. Nyní odpojte hello virtuálního pevného disku z hello virtuálních počítačů. Vyberte virtuální počítač v hello portál Azure a klikněte na tlačítko **disky**. Vyberte existující virtuální pevný disk a potom klikněte na **odpojení**:

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
Pokud máte problémy s připojením tooyour virtuálních počítačů, přečtěte si téma [tooan řešení potíží s RDP připojení virtuálního počítače Azure](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Problémy s přístupem k aplikacím spuštěným na vašem virtuálním počítači najdete v tématu [problémů s připojením aplikace na virtuálním počítači Windows](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Další informace o používání správce prostředků najdete v tématu [přehled Azure Resource Manageru](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).