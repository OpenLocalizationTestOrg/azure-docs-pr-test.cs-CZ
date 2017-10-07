---
title: "aaaReset místní heslo systému Windows bez agenta Azure | Microsoft Docs"
description: "Jak tooreset hello hesla místního uživatelského účtu systému Windows, když hello Azure hostovaný agent není nainstalován nebo je funkční na virtuálním počítači"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: cf353dd3-89c9-47f6-a449-f874f0957013
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/07/2017
ms.author: iainfou
ms.openlocfilehash: c559c31ea142f9cf50d2c5b6182c5355fec9bac5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-local-windows-password-for-azure-vm"></a>Jak tooreset místní heslo systému Windows pro virtuální počítač Azure
Můžete obnovit heslo místního Windows hello virtuálního počítače v Azure pomocí hello [portál Azure nebo Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) zadaný je nainstalován agent hosta Azure hello. Tato metoda je hello primární způsob tooreset heslo pro virtuální počítač Azure. Pokud narazíte na potíže s hello Azure hostovaný agent neodpovídá, nebo selhání tooinstall po nahrání vlastní image, můžete ručně obnovit heslo systému Windows. Tento článek podrobně popisuje, jak tooreset heslo místního účtu připojením hello tooanother zdrojový OS virtuálního disku virtuálního počítače. 

> [!WARNING]
> Tento proces lze používejte pouze jako poslední možnost. Vždy zkuste tooreset hesla pomocí hello [portál Azure nebo Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) první.
> 
> 

## <a name="overview-of-hello-process"></a>Přehled procesu hello
Hello základní kroky pro místní heslo resetovat pro virtuální počítač s Windows v Azure, když není žádný agent hosta Azure toohello přístup je následující:

* Odstraňte hello zdrojového virtuálního počítače. virtuální disky Hello zůstanou zachovány.
* Připojte tooanother disku operačního systému virtuálního počítače hello zdrojového Virtuálního počítače na hello stejné umístění v rámci vašeho předplatného Azure. Tento virtuální počítač je hello odkazované tooas řešení potíží s virtuálních počítačů.
* Pomocí hello řešení potíží s virtuálního počítače, vytvořte některé konfigurační soubory na disk s operačním systémem hello zdrojového Virtuálního počítače.
* Odpojte disk s operačním systémem hello Virtuálního počítače z hello řešení potíží s virtuálních počítačů.
* Použijte toocreate správce prostředků šablony virtuálního počítače, pomocí hello původní virtuální disk.
* Když hello nový virtuální počítač spustí, hello konfigurační soubory můžete vytvořit heslo hello aktualizace hello požadované uživatele.

## <a name="detailed-steps"></a>Podrobné kroky
Vždy zkuste tooreset hesla pomocí hello [portál Azure nebo Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) než to zkusíte hello následující kroky. Ujistěte se, že máte zálohu virtuálního počítače, před zahájením. 

1. Odstranit hello vliv virtuálního počítače na portálu Azure. Odstraňování hello virtuálních počítačů odstraní pouze metadata hello, hello odkaz hello virtuálních počítačů v rámci Azure. Při odstranění hello virtuálních počítačů, zůstanou zachovány Hello virtuální disky:
   
   * Vyberte hello virtuálních počítačů v hello portál Azure, klikněte na tlačítko *odstranit*:
     
     ![Odstraňte existující virtuální počítač](./media/reset-local-password-without-agent/delete_vm.png)
2. Připojte toohello disku operačního systému hello zdrojového Virtuálního počítače, řešení potíží s virtuálních počítačů. Hello řešení potíží s virtuálního počítače musí být v hello stejné oblasti jako disk s operačním systémem hello zdrojového Virtuálního počítače (například `West US`):
   
   * Vyberte hello řešení potíží s virtuálních počítačů v hello portálu Azure. Klikněte na tlačítko *disky* | *připojit existující*:
     
     ![Připojit stávající disk](./media/reset-local-password-without-agent/disks_attach_existing.png)
     
     Vyberte *souboru virtuálního pevného disku* a potom vyberte účet úložiště hello, která obsahuje vaše zdrojového virtuálního počítače:
     
     ![Výběr účtu úložiště](./media/reset-local-password-without-agent/disks_select_storageaccount.PNG)
     
     Vyberte kontejner zdroj hello. kontejner Hello zdroj je obvykle *virtuální pevné disky*:
     
     ![Vyberte kontejner úložiště](./media/reset-local-password-without-agent/disks_select_container.png)
     
     Vyberte hello tooattach virtuální pevný disk operačního systému. Klikněte na tlačítko *vyberte* toocomplete hello proces:
     
     ![Vybrat zdroj virtuálního disku](./media/reset-local-password-without-agent/disks_select_source_vhd.png)
3. Připojit toohello řešení potíží s virtuálního počítače pomocí vzdálené plochy a ujistěte se, že disk s operačním systémem hello zdrojového Virtuálního počítače je viditelná:
   
   * Vyberte hello řešení potíží s virtuálních počítačů v hello portál Azure a klikněte na *Connect*.
   * Otevřete soubor RDP hello, který stahuje. Zadejte hello uživatelské jméno a heslo hello řešení potíží s virtuálních počítačů.
   * V Průzkumníku souborů vyhledejte hello datový disk, který jste připojili. Když hello zdroj, který virtuální pevný disk Virtuálního počítače hello pouze datový disk připojený toohello řešení potíží s virtuálních počítačů, měla by být hello F: jednotky:
     
     ![Zobrazit připojené datový disk](./media/reset-local-password-without-agent/troubleshooting_vm_fileexplorer.png)
4. Vytvoření `gpt.ini` v `\Windows\System32\GroupPolicy` na jednotce hello zdrojového Virtuálního počítače (pokud existuje gpt.ini, přejmenujte toogpt.ini.bak):
   
   > [!WARNING]
   > Zajistěte, aby náhodně vytvořit hello následující soubory v C:\Windows, hello jednotky operačního systému pro hello řešení potíží s virtuálních počítačů. Vytvořte hello následující soubory v jednotce hello operačního systému pro vaše zdrojového virtuálního počítače, který je připojen jako datový disk.
   > 
   > 
   
   * Přidejte následující řádky do hello hello `gpt.ini` souborů, které jste vytvořili:
     
     ```
     [General]
     gPCFunctionalityVersion=2
     gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
     Version=1
     ```
     
     ![Vytvoření gpt.ini](./media/reset-local-password-without-agent/create_gpt_ini.png)
5. Vytvoření `scripts.ini` v `\Windows\System32\GroupPolicy\Machine\Scripts`. Ujistěte se, že se zobrazují skrytých složek. V případě potřeby vytvořte hello `Machine` nebo `Scripts` složek.
   
   * Přidejte následující řádky hello hello `scripts.ini` souborů, které jste vytvořili:
     
     ```
     [Startup]
     0CmdLine=C:\Windows\System32\FixAzureVM.cmd
     0Parameters=
     ```
     
     ![Vytvoření scripts.ini](./media/reset-local-password-without-agent/create_scripts_ini.png)
6. Vytvoření `FixAzureVM.cmd` v `\Windows\System32` s hello následující obsah, nahraďte `<username>` a `<newpassword>` vlastními hodnotami:
   
    ```
    net user <username> <newpassword> /add
    net localgroup administrators <username> /add
    net localgroup "remote desktop users" <username> /add

    ```

    ![Vytvoření FixAzureVM.cmd](./media/reset-local-password-without-agent/create_fixazure_cmd.png)
   
    Při definování hello nové heslo, musí splňovat požadavky na složitost hesla hello nakonfigurované pro virtuální počítač.
7. Na portálu Azure odpojte disk hello od hello řešení potíží s virtuálních počítačů:
   
   * Vyberte hello řešení potíží s virtuálních počítačů v hello portálu Azure, klikněte na *disky*.
   * Vyberte hello datový disk připojený v kroku 2, klikněte na tlačítko *odpojení*:
     
     ![Odpojte disk](./media/reset-local-password-without-agent/detach_disk.png)
8. Než vytvoříte virtuální počítač, získejte hello URI tooyour zdrojový OS disk:
   
   * Klikněte na účet úložiště vyberte hello hello portál Azure, *objekty BLOB*.
   * Vyberte kontejner hello. kontejner Hello zdroj je obvykle *virtuální pevné disky*:
     
     ![Vyberte účet úložiště objektů blob](./media/reset-local-password-without-agent/select_storage_details.png)
     
     Vyberte zdroj virtuální pevný disk operačního systému virtuálního počítače a klikněte na tlačítko hello *kopie* tlačítko Další toohello *URL* název:
     
     ![Zkopírujte disk identifikátor URI](./media/reset-local-password-without-agent/copy_source_vhd_uri.png)
9. Vytvoření virtuálního počítače z disku operačního systému hello zdrojového Virtuálního počítače:
   
   * Použití [této šablony Azure Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) toocreate virtuálního počítače z specializované virtuálního pevného disku. Klikněte na tlačítko hello `Deploy tooAzure` tlačítko tooopen hello portál Azure s použitím šablon podrobnostmi hello vyplní za vás.
   * Pokud chcete tooretain všech hello předchozí nastavení pro hello virtuálního počítače, vyberte *úpravy šablony* tooprovide existující virtuální síť, podsíť, síťový adaptér nebo veřejné IP adresy.
   * V hello `OSDISKVHDURI` parametr textového pole, vložte hello získat identifikátor URI zdrojového virtuálního pevného disku v předchozím kroku hello:
     
     ![Vytvoření virtuálního počítače ze šablony](./media/reset-local-password-without-agent/create_new_vm_from_template.png)
10. Po hello nový virtuální počítač spuštěný, připojit toohello virtuálního počítače pomocí vzdálené plochy s hello nové heslo jste zadali v hello `FixAzureVM.cmd` skriptu.
11. Z vaší relace vzdálené toohello nového virtuálního počítače, odeberte hello následující soubory tooclean hello prostředí:
    
    * Z %windir%\System32
      * odebrat FixAzureVM.cmd
    * Z %windir%\System32\GroupPolicy\Machine\
      * odebrat scripts.ini
    * Z %windir%\System32\GroupPolicy
      * Odeberte gpt.ini (pokud existoval gpt.ini a přejmenovat toogpt.ini.bak, přejmenování hello .bak souboru back toogpt.ini)

## <a name="next-steps"></a>Další kroky
Pokud se pořád nemůžete připojit pomocí vzdálené plochy, přečtěte si téma hello [Průvodci odstraňováním potíží RDP](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Hello [podrobný Průvodce odstraňováním potíží s RDP](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) vypadá na řešení potíží s metody, nikoli konkrétní kroky. Můžete také [otevřete žádost o podporu Azure](https://azure.microsoft.com/support/options/) praktických pomoc.

