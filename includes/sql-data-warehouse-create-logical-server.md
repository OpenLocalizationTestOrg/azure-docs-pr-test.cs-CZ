### <a name="create-a-new-logical-sql-server-in-hello-azure-portal"></a><span data-ttu-id="b0b38-101">Vytvoření nového logického SQL serveru v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b0b38-101">Create a new logical SQL server in hello Azure portal</span></span>

1. <span data-ttu-id="b0b38-102">Klikněte na **Nový**, vyhledejte **logical server** a stiskněte **Enter**.</span><span class="sxs-lookup"><span data-stu-id="b0b38-102">Click **New**, search **logical server**, and then hit **ENTER**.</span></span>

    ![vyhledání logického serveru](./media/sql-data-warehouse-create-logical-server/search-logical-server.png)
2. <span data-ttu-id="b0b38-104">Vyberte **SQL Server (logický server)**.</span><span class="sxs-lookup"><span data-stu-id="b0b38-104">Select **SQL server (logical server)**</span></span> 

    ![výběr logického serveru](./media/sql-data-warehouse-create-logical-server/select-logical-server.png)
  
3. <span data-ttu-id="b0b38-106">Klikněte na tlačítko **vytvořit** tooopen hello nové okno systému SQL Server (logický server).</span><span class="sxs-lookup"><span data-stu-id="b0b38-106">Click **Create** tooopen hello new SQL Server (logical server) blade.</span></span>

   <span data-ttu-id="b0b38-107"><kbd>![otevření okna logického serveru](./media/sql-data-warehouse-create-logical-server/open-logical-server-blade.png)</kbd><kbd>![okno logického serveru](./media/sql-data-warehouse-create-logical-server/logical-server-blade.png)</kbd></span><span class="sxs-lookup"><span data-stu-id="b0b38-107"><kbd> ![open logical server blade](./media/sql-data-warehouse-create-logical-server/open-logical-server-blade.png) </kbd> <kbd>![logical server blade](./media/sql-data-warehouse-create-logical-server/logical-server-blade.png) </kbd></span></span>
  
3. <span data-ttu-id="b0b38-108">V okně SQL Server (logický server) hello serveru název textového pole zadejte platný název pro nový logický server hello.</span><span class="sxs-lookup"><span data-stu-id="b0b38-108">In hello SQL Server (logical server) blade's server name text box, provide a valid name for hello new logical server.</span></span> <span data-ttu-id="b0b38-109">Zelená značka zaškrtnutí označuje, že jste zadali platný název.</span><span class="sxs-lookup"><span data-stu-id="b0b38-109">A green check mark indicates that you have provided a valid name.</span></span>
    
    ![Název nového serveru](./media/sql-data-warehouse-create-logical-server/new-name-logical-server.png)

    > [!IMPORTANT]
    > <span data-ttu-id="b0b38-111">Hello plně kvalifikovaný název pro nový server bude < your_server_name >. database.windows.net.</span><span class="sxs-lookup"><span data-stu-id="b0b38-111">hello fully qualified name for your new server will be <your_server_name>.database.windows.net.</span></span>
    >
    
4. <span data-ttu-id="b0b38-112">Hello serveru správce přihlášení textového pole zadejte uživatelské jméno pro přihlašovací jméno ověřování SQL hello pro tento server.</span><span class="sxs-lookup"><span data-stu-id="b0b38-112">In hello Server admin login text box, provide a user name for hello SQL authentication login for this server.</span></span> <span data-ttu-id="b0b38-113">Toto přihlášení je známý jako hlavní přihlášení na server hello.</span><span class="sxs-lookup"><span data-stu-id="b0b38-113">This login is known as hello server principal login.</span></span> <span data-ttu-id="b0b38-114">Zelená značka zaškrtnutí označuje, že jste zadali platný název.</span><span class="sxs-lookup"><span data-stu-id="b0b38-114">A green check mark indicates that you have provided a valid name.</span></span>
    
    ![Přihlášení správce SQL](./media/sql-data-warehouse-create-logical-server/sql-admin-login.png)
5. <span data-ttu-id="b0b38-116">V hello **heslo** a **potvrzení hesla** textová pole, zadejte heslo pro účet hlavní přihlášení na server hello.</span><span class="sxs-lookup"><span data-stu-id="b0b38-116">In hello **Password** and **Confirm password** text boxes, provide a password for hello server principal login account.</span></span> <span data-ttu-id="b0b38-117">Zelená značka zaškrtnutí označuje, že jste zadali platné heslo.</span><span class="sxs-lookup"><span data-stu-id="b0b38-117">A green check mark indicates that you have provided a valid password.</span></span>
    
    ![Heslo správce SQL](./media/sql-data-warehouse-create-logical-server/sql-admin-password.png)
6. <span data-ttu-id="b0b38-119">Vyberte předplatné, ve které máte oprávnění toocreate objektů.</span><span class="sxs-lookup"><span data-stu-id="b0b38-119">Select a subscription in which you have permission toocreate objects.</span></span>

    ![předplatné](./media/sql-data-warehouse-create-logical-server/subscription.png)
7. <span data-ttu-id="b0b38-121">Do textového pole hello prostředků skupiny, vyberte **vytvořit nový** a hello prostředků skupiny textového pole, zadejte platný název pro novou skupinu prostředků hello (můžete také použít existující skupinu prostředků. Pokud jste již vytvořili jednu pro sebe).</span><span class="sxs-lookup"><span data-stu-id="b0b38-121">In hello Resource group text box, select **Create new** and then, in hello resource group text box, provide a valid name for hello new resource group (you can also use an existing resource group if you have already created one for yourself).</span></span> <span data-ttu-id="b0b38-122">Zelená značka zaškrtnutí označuje, že jste zadali platný název.</span><span class="sxs-lookup"><span data-stu-id="b0b38-122">A green check mark indicates that you have provided a valid name.</span></span>

    ![Nová skupina prostředků](./media/sql-data-warehouse-create-logical-server/new-resource-group.png)

8. <span data-ttu-id="b0b38-124">V hello **umístění** textového pole, vyberte data center odpovídající tooyour umístění – například "Austrálie – východ".</span><span class="sxs-lookup"><span data-stu-id="b0b38-124">In hello **Location** text box, select a data center appropriate tooyour location - such as "Australia East".</span></span>
    
    ![Umístění serveru](./media/sql-data-warehouse-create-logical-server/server-location.png)
    
    > [!TIP]
    > <span data-ttu-id="b0b38-126">Hello zaškrtávací políčko pro **povolit službám azure tooaccess server** na toto okno nelze změnit.</span><span class="sxs-lookup"><span data-stu-id="b0b38-126">hello checkbox for **Allow azure services tooaccess server** cannot be changed on this blade.</span></span> <span data-ttu-id="b0b38-127">Toto nastavení v okně brány firewall serveru hello můžete změnit.</span><span class="sxs-lookup"><span data-stu-id="b0b38-127">You can change this setting on hello server firewall blade.</span></span> <span data-ttu-id="b0b38-128">Další informace najdete v článku [Začínáme se zabezpečením](../articles/sql-database/sql-database-manage-servers-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b0b38-128">For more information, see [Get started with security](../articles/sql-database/sql-database-manage-servers-portal.md).</span></span>
    >
    
9. <span data-ttu-id="b0b38-129">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="b0b38-129">Click **Create**.</span></span>

    ![Tlačítko Vytvořit](./media/sql-data-warehouse-create-logical-server/create.png)

