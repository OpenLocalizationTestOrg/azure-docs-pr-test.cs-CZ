#### <a name="tooinstall-mpio-on-hello-host"></a>tooinstall funkci MPIO na hostiteli hello
1. Otevřete správce serveru na hostiteli s Windows Server. Ve výchozím nastavení správce serveru spustí, když se člen skupiny Administrators hello přihlásí tooa počítače se systémem Windows Server 2012 R2 nebo Windows Server 2012. Pokud hello správce serveru už není otevřený, klikněte na tlačítko **Start > Správce serveru**.
   
    ![Správce serveru](./media/storsimple-install-mpio-windows-server/IC740997.png)
2. Klikněte na tlačítko **správce serveru > řídicí panel > Přidat role a funkce**. Tím se spustí hello **přidat role a funkce** průvodce.
   
    ![Přidat role a funkce Průvodce 1](./media/storsimple-install-mpio-windows-server/IC740998.png)
3. V hello **přidat role a funkce** Průvodce hello následující:
   
   * Na hello **před zahájením** klikněte na tlačítko **Další**.
   * Na hello **vybrat typ instalace** přijměte výchozí nastavení hello **na základě rolí nebo na základě funkcí** instalace. Klikněte na **Další**.
     
       ![Přidat role a funkce Průvodce 2](./media/storsimple-install-mpio-windows-server/IC740999.png)
   * Na hello **vybrat cílový server** vyberte **vyberte server z fondu serverů hello**. Hostitelský server by měly být zjištěny automaticky. Klikněte na **Další**.
   * Na hello **vybrat role serveru** klikněte na tlačítko **Další**.
   * Na hello **vyberte funkce** vyberte **funkce Multipath I/O**a klikněte na tlačítko **Další**.
     
       ![Přidat role a funkce Průvodce 5](./media/storsimple-install-mpio-windows-server/IC741000.png)
   * Na hello **potvrdit vybrané možnosti instalace** potvrďte hello výběr a potom vyberte **restartování hello cílový server automaticky podle potřeby**, jak je uvedeno níže. Klikněte na **Nainstalovat**.
     
       ![Přidat role a funkce Průvodce 8](./media/storsimple-install-mpio-windows-server/IC741001.png)
   * Po dokončení instalace hello, budete upozorněni. Klikněte na tlačítko **Zavřít** tooclose hello průvodce.
     
       ![Přidat role a funkce Průvodce 9](./media/storsimple-install-mpio-windows-server/IC741002.png)

