### <a name="configure-a-dns-label-for-hello-public-ip-address"></a>Konfigurace názvu DNS pro hello veřejnou IP adresu

tooconnect toohello databázového stroje SQL Server z hello Internetu, zvažte vytvoření názvu DNS veřejné IP adresy. Můžete připojit pomocí IP adresy, ale hello popisek DNS vytvoří záznam A, který je snazší tooidentify a přehledů hello základní veřejnou IP adresu.

> [!NOTE]
> Názvy DNS nejsou vyžadovány, pokud plán tooonly připojení toohello systému SQL Server instance v rámci hello stejné virtuální sítě nebo jenom místně.

toocreate název DNS, vyberte nejdřív **virtuální počítače** hello portálu. Vyberte váš virtuální počítač s SQL serverem toobring si jeho vlastnosti.

1. V části Přehled hello virtuálního počítače, vyberte vaše **veřejnou IP adresu**.

    ![Veřejná IP adresa](./media/virtual-machines-sql-server-connection-steps/rm-public-ip-address.png)

1. Ve vlastnostech hello pro veřejnou IP adresu, rozbalte položku **konfigurace**.

1. Zadejte název DNS. Tento název je záznam A, který může být přímo použít tooconnect tooyour virtuální počítač SQL Server pomocí názvu, namísto pomocí IP adresy.

1. Klikněte na tlačítko hello **Uložit** tlačítko.

    ![Název DNS](./media/virtual-machines-sql-server-connection-steps/rm-dns-label.png)

### <a name="connect-toohello-database-engine-from-another-computer"></a>Připojit toohello databázový stroj z jiného počítače

1. Na počítači připojené toohello Internetu, otevřete SQL Server Management Studio (SSMS). Pokud aplikaci SQL Server Management Studio nemáte, [tady](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) si ji můžete stáhnout.

1. V hello **připojit tooServer** nebo **připojit tooDatabase modul** dialogové okno, upravit hello **název serveru** hodnotu. Zadejte hello IP adresu nebo úplný název DNS hello virtuálního počítače (určený v předchozí úloze hello). Můžete také přidat čárku a zadat port TCP SQL Serveru. Například, `mysqlvmlabel.eastus.cloudapp.azure.com,1433`.

1. V hello **ověřování** vyberte **ověřování systému SQL Server**.

1. V hello **přihlášení** pole, název typu hello platného přihlášení SQL.

1. V hello **heslo** pole, zadejte heslo hello hello přihlášení.

1. Klikněte na **Připojit**.

    ![Připojení přes SSMS](./media/virtual-machines-sql-server-connection-steps/rm-ssms-connect.png)