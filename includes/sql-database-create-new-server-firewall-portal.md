
<!--
includes/sql-database-create-new-server-firewall-portal.md

Latest Freshness check:  2016-11-28 , rickbyh.

As of circa 2016-04-11, hello following topics might include this include:
articles/sql-database/sql-database-get-started.md
articles/sql-database/sql-database-configure-firewall-settings
articles/sql-data-warehouse-get-started-provision.md

-->
### <a name="create-a-server-level-firewall-rule-in-hello-azure-portal"></a>Vytvoření pravidla brány firewall na úrovni serveru v hello portálu Azure

1. Na hello okna SQL serveru, v části nastavení, klikněte na tlačítko **brány Firewall** tooopen hello brány Firewall okno pro hello SQL server.

    <!-- ![sql server firewall](../articles/sql-database/media/sql-database-get-started/sql-server-firewall.png) -->

2. Zkontrolujte hello IP adresa klienta zobrazit a ověřit, že toto je vaše IP adresa na Internetu pomocí prohlížeče zvoleného hello (požádejte "Jaký je adresa IP). Čas od času si adresy z různých důvodů neodpovídají.

    <!-- ![your IP address](../articles/sql-database/media/sql-database-get-started/your-ip-address.png) -->

3. Za předpokladu, že hello IP adresy odpovídají, klikněte na tlačítko **přidat IP adresu klienta** na panelu nástrojů hello.

    ![Přidat IP adresu klienta](../articles/sql-data-warehouse/media/sql-data-warehouse-get-started-provision/add-client-ip.png)

    > [!NOTE]
    > Můžete otevřít hello firewall SQL Database na server hello tooa jednu IP adresu nebo celý rozsah adres. Otevírání hello brána firewall umožňuje správci SQL a uživatelé toologin tooany databáze na hello toowhich serveru mají platné přihlašovací údaje.
    >

4. Klikněte na tlačítko **Uložit** na panelu nástrojů toosave hello toto pravidlo brány firewall na úrovni serveru a pak klikněte na **OK**.

    ![Přidat IP adresu klienta](../articles/sql-database/media/sql-database-get-started-portal/server-firewall-rule.png)

> [!Tip]
> Kurz najdete v článku [Kurz k SQL Database: Vytvoření serveru, pravidla brány firewall na úrovni serveru, ukázkové databáze a pravidla brány firewall na úrovni databáze a připojení k SQL Serveru](../articles/sql-database/sql-database-get-started.md).    
>
