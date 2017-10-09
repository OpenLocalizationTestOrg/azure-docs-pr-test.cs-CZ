### <a name="open-tcp-ports-in-hello-windows-firewall-for-hello-default-instance-of-hello-database-engine"></a>Otevřete porty protokolu TCP v bráně firewall systému Windows hello pro hello výchozí instanci databázového stroje hello
1. Připojte toohello virtuálního počítače pomocí vzdálené plochy. Podrobné pokyny o připojení toohello virtuálních počítačů najdete v tématu [otevřete virtuálního počítače s SQL pomocí vzdálené plochy](../articles/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md#open-the-vm-with-remote-desktop).
2. Po přihlášení na úvodní obrazovce hello zadejte **WF.msc**a pak stiskněte ENTER.
   
    ![Spustit hello brány firewall](./media/virtual-machines-sql-server-connection-steps/12Open-WF.png)
3. V hello **brány Windows Firewall s pokročilým zabezpečením**, v levém podokně text hello, klikněte pravým tlačítkem na **příchozí pravidla**a potom klikněte na **nové pravidlo** v podokně Akce hello.
   
    ![Nové pravidlo](./media/virtual-machines-sql-server-connection-steps/13New-FW-Rule.png)
4. V hello **pravidla Průvodce vytvořením nového příchozího** dialogovém **typ pravidla**, vyberte **Port**a potom klikněte na **Další**.
5. V hello **protokol a porty** dialogové okno, použít výchozí hello **TCP**. V hello **určité místní porty** pole a pak typ hello portu počet hello instanci hello databázový stroj (**1433** pro výchozí instanci hello nebo zvoleného pro privátní port hello v kroku hello koncového bodu).
   
    ![Port 1433 protokolu TCP](./media/virtual-machines-sql-server-connection-steps/14Port-1433.png)
6. Klikněte na **Další**.
7. V hello **akce** dialogové okno, vyberte **povolit připojení hello**a potom klikněte na **Další**.
   
    **Poznámka k zabezpečení:** zvolíte **povolit připojení hello zabezpečená** můžete dále zvýšit zabezpečení. Tuto možnost vyberte, pokud chcete další bezpečnostní možnosti tooconfigure ve vašem prostředí.
   
    ![Povolení připojení](./media/virtual-machines-sql-server-connection-steps/15Allow-Connection.png)
8. V hello **profil** dialogové okno, vyberte **veřejné**, **privátní**, a **domény**. Pak klikněte na tlačítko **Další**.
   
    **Poznámka k zabezpečení:** zvolíte **veřejné** umožňuje přístup přes hello internet. Pokud je to možné, vybírejte vždy restriktivnější profil.
   
    ![Veřejný profil](./media/virtual-machines-sql-server-connection-steps/16Public-Private-Domain-Profile.png)
9. V hello **název** dialogové okno, zadejte název a popis pro toto pravidlo a pak klikněte na tlačítko **Dokončit**.
   
    ![Název pravidla](./media/virtual-machines-sql-server-connection-steps/17Rule-Name.png)

Podle potřeby otevřete další porty pro ostatní komponenty. Další informace najdete v tématu [konfigurace brány Windows Firewall tooAllow hello přístup k serveru SQL](http://msdn.microsoft.com/library/cc646023.aspx).

### <a name="configure-sql-server-toolisten-on-hello-tcp-protocol"></a>Konfigurace systému SQL Server toolisten na hello protokolu TCP

[!INCLUDE [Enable TCP](virtual-machines-sql-server-connection-tcp-protocol.md)]

### <a name="configure-sql-server-for-mixed-mode-authentication"></a>Konfigurace SQL Serveru pro kombinovaný režim ověřování
Hello databázového stroje SQL Server nelze použít ověřování systému Windows bez prostředí domény. tooconnect toohello databázový stroj z jiného počítače, nakonfigurujte pro smíšený režim ověřování systému SQL Server. Smíšený režim ověřování umožňuje ověřování systému SQL Server i ověřování systému Windows.

> [!NOTE]
> Konfigurace ověřování ve smíšeném režimu nemusí být nutná v případě, že jste nakonfigurovali Azure Virtual Network s nakonfigurovaným doménovým prostředím.
> 
> 

1. Při připojené toohello virtuální počítač, na hello úvodní stránky, zadejte **SQL Server Management Studio** a klikněte na ikonu vybrané hello.
   
    Při prvním spuštění Management Studio musí vytvořit prostředí Management Studio uživatelé hello Hello. Tato operace může chvíli trvat.
2. Management Studio uvede hello **připojit tooServer** dialogové okno. V hello **název serveru** pole, název typu hello hello virtuálního počítače tooconnect toohello databázový stroj s hello Průzkumník objektů (místo hello název virtuálního počítače můžete použít také **(místní)** nebo jedno období jako hello **název serveru**). Vyberte **ověřování systému Windows**a nechat  ***název_virtuálního_počítače*\your_local_administrator** v hello **uživatelské jméno** pole. Klikněte na **Připojit**.
   
    ![Připojit tooServer](./media/virtual-machines-sql-server-connection-steps/19Connect-to-Server.png)
3. V Průzkumník objektů systému SQL Server Management Studio, klikněte pravým tlačítkem na název hello hello instance systému SQL Server (hello název virtuálního počítače) a pak klikněte na tlačítko **vlastnosti**.
   
    ![Vlastnosti serveru](./media/virtual-machines-sql-server-connection-steps/20Server-Properties.png)
4. Na hello **zabezpečení** v části **ověřování serveru**, vyberte **systému SQL Server a ověřování systému Windows režimu**a potom klikněte na **OK** .
   
    ![Výběr režimu ověřování](./media/virtual-machines-sql-server-connection-steps/21Mixed-Mode.png)
5. V dialogovém okně SQL Server Management Studio hello, klikněte na tlačítko **OK** tooacknowledge hello požadavek toorestart systému SQL Server.
6. V Průzkumníku objektů klikněte pravým tlačítkem na server a potom klikněte na **Restartovat**. (Pokud je spuštěný agent systému SQL Server, musí se také restartovat.)
   
    ![Restartování](./media/virtual-machines-sql-server-connection-steps/22Restart2.png)
7. V dialogovém okně SQL Server Management Studio hello, klikněte na tlačítko **Ano** tooagree, které chcete toorestart systému SQL Server.

### <a name="create-sql-server-authentication-logins"></a>Vytvoření účtů ověřování serveru SQL
tooconnect toohello databázový stroj z jiného počítače, musíte vytvořit alespoň jeden účet ověřování systému SQL Server.

1. V Průzkumník objektů systému SQL Server Management Studio rozbalte složku hello instance hello serveru, který chcete toocreate hello nové přihlašovací údaje.
2. Klikněte pravým tlačítkem na hello **zabezpečení** složky, bod příliš**nový**a vyberte **přihlášení...** .
   
    ![Nové přihlášení](./media/virtual-machines-sql-server-connection-steps/23New-Login.png)
3. V hello **přihlášení – nové** dialogové okno, v hello **Obecné** stránky, zadejte název hello hello nového uživatele do hello **přihlašovací jméno** pole.
4. Vyberte **Ověřování SQL Serveru**.
5. V hello **heslo** pole, zadejte heslo pro nového uživatele hello. Znovu zadejte toto heslo do hello **Potvrdit heslo** pole.
6. Vyberte požadované možnosti pro vynucení hello hesla (**vynutit zásady hesel**, **vynutit vypršení platnosti hesla**, a **musí uživatel změnit heslo při příštím přihlášení**). Pokud používáte toto přihlášení pro sami, není nutné toorequire změnit heslo při příštím přihlášení hello.
7. Z hello **výchozí databázi** seznamu, vyberte výchozí databázi pro přihlášení hello. **hlavní** je hello výchozího nastavení pro tuto možnost. Pokud jste ještě nevytvořili uživatelské databáze, nechte toto nastavení příliš**hlavní**.
   
    ![Vlastnosti přihlášení](./media/virtual-machines-sql-server-connection-steps/24Test-Login.png)
8. Pokud je to první přihlášení hello, kterou vytváříte, můžete chtít toodesignate toto přihlášení jako správce systému SQL Server. Pokud ano, na hello **role serveru** stránka, zkontrolujte **sysadmin**.
   
   > [!NOTE]
   > Členové pevné role serveru sysadmin hello mít úplnou kontrolu nad hello databázového stroje. Členství v této roli byste měli pečlivě omezit.
   > 
   > 
   
   ![Správce systému](./media/virtual-machines-sql-server-connection-steps/25sysadmin.png)
9. Klikněte na tlačítko OK.

Další informace o přihlášeních SQL Serveru najdete v tématu věnovaném [vytvoření přihlášení](http://msdn.microsoft.com/library/aa337562.aspx).

