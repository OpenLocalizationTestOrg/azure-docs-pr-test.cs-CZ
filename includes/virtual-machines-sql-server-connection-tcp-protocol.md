1. Při připojené toohello virtuálního počítače pomocí vzdálené plochy, vyhledejte **nástroje Configuration Manager**:

    ![Otevření SSCM](./media/virtual-machines-sql-server-connection-tcp-protocol/sql-server-configuration-manager.png)

1. Ve Správci konfigurace serveru SQL v podokně konzoly hello rozbalte **konfigurace sítě serveru SQL Server**.

1. V podokně hello konzoly, klikněte na **protokoly pro MSSQLSERVER** (hello výchozí název instance.) V podokně podrobností hello klikněte pravým tlačítkem na **TCP** a klikněte na tlačítko **povolit** není-li již povoleno.

    ![Povolení protokolu TCP](./media/virtual-machines-sql-server-connection-tcp-protocol/enable-tcp.png)

1. V podokně hello konzoly, klikněte na **služby SQL Server**. V podokně podrobností hello klikněte pravým tlačítkem na  **systému SQL Server (*název instance*) ** (hello výchozí instance je **serveru SQL (MSSQLSERVER)**) a pak klikněte na tlačítko **restartování** , toostop a restartujte hello instance systému SQL Server.

    ![Restartování databázového stroje](./media/virtual-machines-sql-server-connection-tcp-protocol/restart-sql-server.png)

1. Zavřete SQL Server Configuration Manager.

Další informace o povolení protokoly pro hello databázového stroje SQL Server najdete v tématu [povolit nebo zakázat síťový protokol serveru](http://msdn.microsoft.com/library/ms191294.aspx).
