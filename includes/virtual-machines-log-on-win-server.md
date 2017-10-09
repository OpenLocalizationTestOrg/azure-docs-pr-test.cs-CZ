1. Kliknutím na **Připojit** vytvoříte a stáhnete soubor protokolu Remote Desktop Protocol (.rdp). Klikněte na tlačítko **otevřete** toouse tento soubor.
2. Zobrazí se upozornění této hello RDP je od neznámého vydavatele. To je normální. V okně hello vzdálené plochy, klikněte na tlačítko **připojit** toocontinue.
   
    ![Snímek obrazovky upozornění na neznámého vydavatele](./media/virtual-machines-log-on-win-server/rdp-warn.png)
3. V hello **zabezpečení systému Windows** okno, zadejte hello pověření pro účet, na hello virtuálního počítače a potom klikněte na **OK**.
   
     **Místní účet** – to je obvykle hello místní účet, uživatelské jméno a heslo, které jste zadali při vytvoření hello virtuálního počítače. V takovém případě hello doména je název hello hello virtuálního počítače a je zadána jako *vmname*&#92; *uživatelské jméno*.  
   
    **Virtuální počítač připojený k doméně** – Pokud hello virtuálního počítače patří tooa domény, zadejte hello uživatelské jméno ve formátu hello *domény*&#92; *Uživatelské jméno*. Hello účet taky tooeither musí být v hello správci skupiny nebo byl udělen vzdálený přístup oprávnění toohello virtuálních počítačů.
   
    **Řadič domény** – Pokud hello virtuální počítač je řadič domény, typ hello uživatelské jméno a heslo účtu správce domény pro tuto doménu.
4. Klikněte na tlačítko **Ano** tooverify hello identity hello virtuální počítač a dokončete přihlášení.
   
   ![Snímek obrazovky zobrazující zprávu o ověření identity hello hello virtuálních počítačů.](./media/virtual-machines-log-on-win-server/cert-warning.png)

