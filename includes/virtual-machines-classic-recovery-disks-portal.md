Pokud virtuální počítač (VM) v Azure dojde k chybě spouštěcí nebo disk, může být nutné tooperform řešení potíží s kroky hello virtuální pevný disk sám sebe. Běžným příkladem bude aplikaci, která selhala aktualizace, která zabrání spuštění úspěšně hello virtuálních počítačů. Tento článek popisuje, jak toouse Azure portálu tooconnect vaše virtuální pevný disk tooanother virtuálních počítačů toofix všechny chyby a potom je znovu vytvořit původní virtuální počítač.

## <a name="recovery-process-overview"></a>Přehled procesu obnovení
řešení potíží s procesem Hello vypadá takto:

1. Odstranit hello virtuální počítač, který je zjištění problémy, ale zachovat hello virtuální pevné disky.
2. Připojte a připojit virtuální pevný disk tooanother hello virtuálních počítačů pro řešení potíží.
3. Připojte toohello řešení potíží s virtuálních počítačů. Úpravy souborů nebo spuštěním nástroje toofix chyby v hello původní virtuální pevný disk.
4. Odpojte Image a odpojte hello virtuální pevný disk z hello řešení potíží s virtuálních počítačů.
5. Vytvoření virtuálního počítače pomocí hello původní virtuální pevný disk.

## <a name="delete-hello-original-vm"></a>Odstranit hello původní virtuální počítač
Virtuální pevné disky a virtuální počítače jsou v Azure dva různé prostředky. Virtuální pevný disk je, kde jsou uloženy hello operačního systému, aplikací a konfigurace. Hello virtuálního počítače je jenom metadata, která definuje hello velikosti nebo umístění a prostředkům, například virtuální pevný disk nebo virtuální síťová karta (NIC), který odkazuje. Každý virtuální pevný disk získá zapůjčení přiřazené připojené tooa virtuálních počítačů po tohoto disku. I když datových disků můžete připojit a odpojit i hello virtuálních počítačů se systémem, disk operačního systému hello nejde odpojit, pokud se odstraní hello prostředků virtuálního počítače. Hello zapůjčení pokračuje tooassociate hello operačního systému disku tooa virtuálních počítačů i v případě, že tento virtuální počítač je ve stavu Zastaveno a deallocated.

první krok toorecovering Hello virtuálního počítače je prostředků virtuálního počítače hello toodelete sám sebe. Odstraňování hello virtuálního počítače zůstane hello virtuální pevné disky ve vašem účtu úložiště. Po hello odstranění virtuálního počítače můžete připojit hello virtuálního pevného disku tooanother virtuálních počítačů tootroubleshoot a vyřešte chyby hello. 

1. Přihlaste se toohello [portál Azure](https://portal.azure.com). 
2. V nabídce hello na levé straně hello, klikněte na tlačítko **virtuálních počítačů (klasické)**.
3. Klikněte na virtuální počítač, který má problém hello vyberte hello **disky**a pak určit název hello hello virtuálního pevného disku. 
4. Vyberte virtuální pevný disk hello operačního systému a zkontrolujte hello **umístění** tooidentify hello úložiště účet, který obsahuje tento virtuální pevný disk. V následujícím příkladu hello, hello řetězec bezprostředně před ". blob.core.windows.net" je název účtu úložiště hello.

    ```
    https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd
    ```

    ![Obrázek Hello o umístění Virtuálního počítače](./media/virtual-machines-classic-recovery-disks-portal/vm-location.png)

5. Klikněte pravým tlačítkem na hello virtuálních počítačů a potom vyberte **odstranit**. Ujistěte se, že hello disky nejsou vybrané, při odstranění hello virtuálních počítačů.
6. Vytvořte nový virtuální počítač pro obnovení. Tento virtuální počítač musí být v hello stejné oblasti nebo skupině prostředků (Cloudová služba) jako hello problém virtuálních počítačů.
7. Vyberte virtuální počítač pro obnovení hello a pak vyberte **disky** > **připojit existující**.
8. tooselect existující virtuální pevný disk, klikněte na tlačítko **souboru virtuálního pevného disku**:

    ![Vyhledání existujícího VHD procházením](./media/virtual-machines-classic-recovery-disks-portal/select-vhd-location.png)

9. Vyberte účet úložiště hello > kontejner virtuálního pevného disku > Dobrý den virtuální pevný disk, klikněte na tlačítko hello **vyberte** tlačítko tooconfirm svého výběru.

    ![Výběr existujícího VHD](./media/virtual-machines-classic-recovery-disks-portal/select-vhd.png)

10. S svůj disk VHD nyní vybrána, vyberte **OK** tooattach hello existující virtuální pevný disk.
11. Za několik sekund, hello **disky** virtuálního počítače v podokně se zobrazí existující virtuální pevný disk připojený jako datový disk:

    ![Existující virtuální pevný disk připojený jako datový disk](./media/virtual-machines-classic-recovery-disks-portal/attached-disk.png)

## <a name="fix-issues-on-hello-original-virtual-hard-disk"></a>Vyřešte problémy na hello původní virtuální pevný disk
Pokud je připojené hello existující virtuální pevný disk, teď můžete dělat žádné údržby a řešení potíží s kroky, podle potřeby. Jakmile jste vyřešili problémy hello, pokračujte hello následující kroky.

## <a name="unmount-and-detach-hello-original-virtual-hard-disk"></a>Odpojte Image a odpojit hello původní virtuální pevný disk
Jakmile jsou všechny chyby vyřešeny, odpojte Image a odpojit hello existující virtuální pevný disk z virtuálního počítače řešení potíží. Virtuální pevný disk společně s jiných virtuálních počítačů nelze používat, dokud vydání hello zapůjčení, který připojí hello virtuálního pevného disku toohello řešení potíží s virtuálních počítačů.  

1. Přihlaste se toohello [portál Azure](https://portal.azure.com). 
2. V nabídce hello na levé straně hello vyberte **virtuálních počítačů (klasické)**.
3. Vyhledejte hello virtuální počítač pro obnovení. Vyberte disky, klikněte pravým tlačítkem na hello disku a pak vyberte **odpojení**.

## <a name="create-a-vm-from-hello-original-hard-disk"></a>Vytvoření virtuálního počítače z hello původní pevného disku

použijte virtuální počítač z původní virtuální pevný disk, toocreate [portál Azure classic](https://manage.windowsazure.com).

1. Přihlaste se k [portálu Azure Classic](https://manage.windowsazure.com).
2. V dolní části hello hello portálu, vyberte **nový** > **výpočetní** > **virtuálního počítače** > **z Galerie** .
3. V hello **zvolí obrázek** vyberte **Moje disky**, a pak vyberte hello původní virtuální pevný disk. Zkontrolujte informace o umístění hello. Toto je hello oblasti, kde musí být nasazený hello virtuálních počítačů. Kliknutím na tlačítko Další hello.
4. V hello **konfigurace virtuálního počítače** , zadejte název virtuálního počítače hello a vyberte velikost hello virtuálních počítačů.
