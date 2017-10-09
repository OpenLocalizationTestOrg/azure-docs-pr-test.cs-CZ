
<!--
includes/sql-database-create-new-server-firewall-portal.md

Latest Freshness check:  2016-11-28 , rickbyh.

As of circa 2016-04-11, hello following topics might include this include:
articles/sql-database/sql-database-get-started.md
articles/sql-database/sql-database-configure-firewall-settings
articles/sql-data-warehouse-get-started-provision.md

-->
### <a name="create-a-server-level-firewall-rule-in-hello-azure-portal"></a><span data-ttu-id="eded5-101">Vytvoření pravidla brány firewall na úrovni serveru v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="eded5-101">Create a server-level firewall rule in hello Azure portal</span></span>

1. <span data-ttu-id="eded5-102">Na hello okna SQL serveru, v části nastavení, klikněte na tlačítko **brány Firewall** tooopen hello brány Firewall okno pro hello SQL server.</span><span class="sxs-lookup"><span data-stu-id="eded5-102">On hello SQL server blade, under Settings, click **Firewall** tooopen hello Firewall blade for hello SQL server.</span></span>

    <!-- ![sql server firewall](../articles/sql-database/media/sql-database-get-started/sql-server-firewall.png) -->

2. <span data-ttu-id="eded5-103">Zkontrolujte hello IP adresa klienta zobrazit a ověřit, že toto je vaše IP adresa na Internetu pomocí prohlížeče zvoleného hello (požádejte "Jaký je adresa IP).</span><span class="sxs-lookup"><span data-stu-id="eded5-103">Review hello client IP address displayed and validate that this is your IP address on hello Internet using a browser of your choice (ask "what is my IP address).</span></span> <span data-ttu-id="eded5-104">Čas od času si adresy z různých důvodů neodpovídají.</span><span class="sxs-lookup"><span data-stu-id="eded5-104">Occasionally they do not match for a various reasons.</span></span>

    <!-- ![your IP address](../articles/sql-database/media/sql-database-get-started/your-ip-address.png) -->

3. <span data-ttu-id="eded5-105">Za předpokladu, že hello IP adresy odpovídají, klikněte na tlačítko **přidat IP adresu klienta** na panelu nástrojů hello.</span><span class="sxs-lookup"><span data-stu-id="eded5-105">Assuming that hello IP addresses match, click **Add client IP** on hello toolbar.</span></span>

    ![Přidat IP adresu klienta](../articles/sql-data-warehouse/media/sql-data-warehouse-get-started-provision/add-client-ip.png)

    > [!NOTE]
    > <span data-ttu-id="eded5-107">Můžete otevřít hello firewall SQL Database na server hello tooa jednu IP adresu nebo celý rozsah adres.</span><span class="sxs-lookup"><span data-stu-id="eded5-107">You can open hello SQL Database firewall on hello server tooa single IP address or an entire range of addresses.</span></span> <span data-ttu-id="eded5-108">Otevírání hello brána firewall umožňuje správci SQL a uživatelé toologin tooany databáze na hello toowhich serveru mají platné přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="eded5-108">Opening hello firewall enables SQL administrators and users toologin tooany database on hello server toowhich they have valid credentials.</span></span>
    >

4. <span data-ttu-id="eded5-109">Klikněte na tlačítko **Uložit** na panelu nástrojů toosave hello toto pravidlo brány firewall na úrovni serveru a pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="eded5-109">Click **Save** on hello toolbar toosave this server-level firewall rule and then click **OK**.</span></span>

    ![Přidat IP adresu klienta](../articles/sql-database/media/sql-database-get-started-portal/server-firewall-rule.png)

> [!Tip]
> <span data-ttu-id="eded5-111">Kurz najdete v článku [Kurz k SQL Database: Vytvoření serveru, pravidla brány firewall na úrovni serveru, ukázkové databáze a pravidla brány firewall na úrovni databáze a připojení k SQL Serveru](../articles/sql-database/sql-database-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="eded5-111">For a tutorial, see [SQL Database tutorial: Create a server, a server-level firewall rule, a sample database, a database-level firewall rule and connect with SQL Server](../articles/sql-database/sql-database-get-started.md).</span></span>    
>
