### <a name="create-a-new-logical-sql-server-in-the-azure-portal"></a><span data-ttu-id="6f500-101">Vytvoření nového logického SQL serveru na webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6f500-101">Create a new logical SQL server in the Azure portal</span></span>

1. <span data-ttu-id="6f500-102">Klikněte na **Nový**, vyhledejte **logical server** a stiskněte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="6f500-102">Click **New**, search **logical server**, and then hit **ENTER**.</span></span>

    ![vyhledání logického serveru](./media/sql-data-warehouse-create-logical-server/search-logical-server.png)
2. <span data-ttu-id="6f500-104">Vyberte **SQL Server (logický server)**.</span><span class="sxs-lookup"><span data-stu-id="6f500-104">Select **SQL server (logical server)**</span></span> 

    ![výběr logického serveru](./media/sql-data-warehouse-create-logical-server/select-logical-server.png)
  
3. <span data-ttu-id="6f500-106">Kliknutím na **Vytvořit** otevřete nové okno SQL Server (logický server).</span><span class="sxs-lookup"><span data-stu-id="6f500-106">Click **Create** to open the new SQL Server (logical server) blade.</span></span>

   <span data-ttu-id="6f500-107"><kbd>![otevřete okno logického serveru](./media/sql-data-warehouse-create-logical-server/open-logical-server-blade.png) </kbd> <kbd> ![okno logického serveru](./media/sql-data-warehouse-create-logical-server/logical-server-blade.png)</kbd></span><span class="sxs-lookup"><span data-stu-id="6f500-107"><kbd> ![open logical server blade](./media/sql-data-warehouse-create-logical-server/open-logical-server-blade.png) </kbd> <kbd>![logical server blade](./media/sql-data-warehouse-create-logical-server/logical-server-blade.png) </kbd></span></span>
  
3. <span data-ttu-id="6f500-108">Do pole pro název serveru v okně SQL Server (logický server) zadejte platný název pro nový logický server.</span><span class="sxs-lookup"><span data-stu-id="6f500-108">In the SQL Server (logical server) blade's server name text box, provide a valid name for the new logical server.</span></span> <span data-ttu-id="6f500-109">Zelená značka zaškrtnutí označuje, že jste zadali platný název.</span><span class="sxs-lookup"><span data-stu-id="6f500-109">A green check mark indicates that you have provided a valid name.</span></span>
    
    ![Název nového serveru](./media/sql-data-warehouse-create-logical-server/new-name-logical-server.png)

    > [!IMPORTANT]
    > <span data-ttu-id="6f500-111">Plně kvalifikovaný název nového serveru bude <název_serveru>.database.windows.net.</span><span class="sxs-lookup"><span data-stu-id="6f500-111">The fully qualified name for your new server will be <your_server_name>.database.windows.net.</span></span>
    >
    
4. <span data-ttu-id="6f500-112">Do textového pole Přihlášení správce serveru zadejte uživatelské jméno pro 	účet ověřování SQL pro tento server.</span><span class="sxs-lookup"><span data-stu-id="6f500-112">In the Server admin login text box, provide a user name for the SQL authentication login for this server.</span></span> <span data-ttu-id="6f500-113">Toto přihlášení se označuje jako přihlášení objektu zabezpečení serveru.</span><span class="sxs-lookup"><span data-stu-id="6f500-113">This login is known as the server principal login.</span></span> <span data-ttu-id="6f500-114">Zelená značka zaškrtnutí označuje, že jste zadali platný název.</span><span class="sxs-lookup"><span data-stu-id="6f500-114">A green check mark indicates that you have provided a valid name.</span></span>
    
    ![Přihlášení správce SQL](./media/sql-data-warehouse-create-logical-server/sql-admin-login.png)
5. <span data-ttu-id="6f500-116">Do textových polí **Heslo** a **Potvrzení hesla** zadejte heslo pro přihlašovací účet objektu zabezpečení serveru.</span><span class="sxs-lookup"><span data-stu-id="6f500-116">In the **Password** and **Confirm password** text boxes, provide a password for the server principal login account.</span></span> <span data-ttu-id="6f500-117">Zelená značka zaškrtnutí označuje, že jste zadali platné heslo.</span><span class="sxs-lookup"><span data-stu-id="6f500-117">A green check mark indicates that you have provided a valid password.</span></span>
    
    ![Heslo správce SQL](./media/sql-data-warehouse-create-logical-server/sql-admin-password.png)
6. <span data-ttu-id="6f500-119">Vyberte předplatné, ve kterém máte oprávnění k vytváření objektů.</span><span class="sxs-lookup"><span data-stu-id="6f500-119">Select a subscription in which you have permission to create objects.</span></span>

    ![předplatné](./media/sql-data-warehouse-create-logical-server/subscription.png)
7. <span data-ttu-id="6f500-121">V textovém poli Skupina prostředků vyberte **Vytvořit nový** a potom do textového pole skupiny prostředků zadejte platný název pro novou skupinu prostředků (můžete také použít existující skupinu prostředků, pokud jste si už nějakou vytvořili).</span><span class="sxs-lookup"><span data-stu-id="6f500-121">In the Resource group text box, select **Create new** and then, in the resource group text box, provide a valid name for the new resource group (you can also use an existing resource group if you have already created one for yourself).</span></span> <span data-ttu-id="6f500-122">Zelená značka zaškrtnutí označuje, že jste zadali platný název.</span><span class="sxs-lookup"><span data-stu-id="6f500-122">A green check mark indicates that you have provided a valid name.</span></span>

    ![Nová skupina prostředků](./media/sql-data-warehouse-create-logical-server/new-resource-group.png)

8. <span data-ttu-id="6f500-124">V textovém poli **Umístění** vyberte datacentrum odpovídající vašemu umístění, třeba Austrálie – východ.</span><span class="sxs-lookup"><span data-stu-id="6f500-124">In the **Location** text box, select a data center appropriate to your location - such as "Australia East".</span></span>
    
    ![Umístění serveru](./media/sql-data-warehouse-create-logical-server/server-location.png)
    
    > [!TIP]
    > <span data-ttu-id="6f500-126">V tomto okně nejde měnit zaškrtnutí políčka **Povolit službám Azure přístup k serveru**.</span><span class="sxs-lookup"><span data-stu-id="6f500-126">The checkbox for **Allow azure services to access server** cannot be changed on this blade.</span></span> <span data-ttu-id="6f500-127">Toto nastavení můžete změnit v okně brány firewall serveru.</span><span class="sxs-lookup"><span data-stu-id="6f500-127">You can change this setting on the server firewall blade.</span></span> <span data-ttu-id="6f500-128">Další informace najdete v článku [Začínáme se zabezpečením](../articles/sql-database/sql-database-manage-servers-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6f500-128">For more information, see [Get started with security](../articles/sql-database/sql-database-manage-servers-portal.md).</span></span>
    >
    
9. <span data-ttu-id="6f500-129">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="6f500-129">Click **Create**.</span></span>

    ![Tlačítko Vytvořit](./media/sql-data-warehouse-create-logical-server/create.png)

