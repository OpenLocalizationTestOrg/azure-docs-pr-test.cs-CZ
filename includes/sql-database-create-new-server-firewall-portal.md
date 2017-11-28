
<!--
includes/sql-database-create-new-server-firewall-portal.md

Latest Freshness check:  2016-11-28 , rickbyh.

As of circa 2016-04-11, the following topics might include this include:
articles/sql-database/sql-database-get-started.md
articles/sql-database/sql-database-configure-firewall-settings
articles/sql-data-warehouse-get-started-provision.md

-->
### <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a><span data-ttu-id="c70d1-101">Vytvoření pravidla brány firewall na úrovni serveru na webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c70d1-101">Create a server-level firewall rule in the Azure portal</span></span>

1. <span data-ttu-id="c70d1-102">V okně SQL Server v části Nastavení klikněte na **Brána firewall**. Otevře se okno Brána firewall pro SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c70d1-102">On the SQL server blade, under Settings, click **Firewall** to open the Firewall blade for the SQL server.</span></span>

    <!-- ![sql server firewall](../articles/sql-database/media/sql-database-get-started/sql-server-firewall.png) -->

2. <span data-ttu-id="c70d1-103">Zkontrolujte zobrazenou IP adresu klienta a pomocí prohlížeče podle vašeho výběru ověřte, že je to vaše IP adresa na internetu (zadejte dotaz Jaká je moje IP adresa).</span><span class="sxs-lookup"><span data-stu-id="c70d1-103">Review the client IP address displayed and validate that this is your IP address on the Internet using a browser of your choice (ask "what is my IP address).</span></span> <span data-ttu-id="c70d1-104">Čas od času si adresy z různých důvodů neodpovídají.</span><span class="sxs-lookup"><span data-stu-id="c70d1-104">Occasionally they do not match for a various reasons.</span></span>

    <!-- ![your IP address](../articles/sql-database/media/sql-database-get-started/your-ip-address.png) -->

3. <span data-ttu-id="c70d1-105">Za předpokladu, že se IP adresy shodují, klikněte na panelu nástrojů na **Přidat IP adresu klienta**.</span><span class="sxs-lookup"><span data-stu-id="c70d1-105">Assuming that the IP addresses match, click **Add client IP** on the toolbar.</span></span>

    ![Přidat IP adresu klienta](../articles/sql-data-warehouse/media/sql-data-warehouse-get-started-provision/add-client-ip.png)

    > [!NOTE]
    > <span data-ttu-id="c70d1-107">Bránu firewall služby SQL Database na serveru můžete otevřít pro jednu IP adresu nebo pro celý rozsah adres.</span><span class="sxs-lookup"><span data-stu-id="c70d1-107">You can open the SQL Database firewall on the server to a single IP address or an entire range of addresses.</span></span> <span data-ttu-id="c70d1-108">Otevření brány firewall umožňuje uživatelům a správcům SQL přihlásit se k jakékoli databázi na serveru, ke kterému mají platné přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="c70d1-108">Opening the firewall enables SQL administrators and users to login to any database on the server to which they have valid credentials.</span></span>
    >

4. <span data-ttu-id="c70d1-109">Kliknutím na **Uložit** na panelu nástrojů uložte toto pravidlo brány firewall na úrovni serveru a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c70d1-109">Click **Save** on the toolbar to save this server-level firewall rule and then click **OK**.</span></span>

    ![Přidat IP adresu klienta](../articles/sql-database/media/sql-database-get-started-portal/server-firewall-rule.png)

> [!Tip]
> <span data-ttu-id="c70d1-111">Kurz najdete v článku [Kurz k SQL Database: Vytvoření serveru, pravidla brány firewall na úrovni serveru, ukázkové databáze a pravidla brány firewall na úrovni databáze a připojení k SQL Serveru](../articles/sql-database/sql-database-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c70d1-111">For a tutorial, see [SQL Database tutorial: Create a server, a server-level firewall rule, a sample database, a database-level firewall rule and connect with SQL Server](../articles/sql-database/sql-database-get-started.md).</span></span>    
>
