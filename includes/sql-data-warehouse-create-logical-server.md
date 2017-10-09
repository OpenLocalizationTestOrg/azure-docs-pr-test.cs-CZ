### <a name="create-a-new-logical-sql-server-in-hello-azure-portal"></a>Vytvoření nového logického SQL serveru v hello portálu Azure

1. Klikněte na **Nový**, vyhledejte **logical server** a stiskněte **Enter**.

    ![vyhledání logického serveru](./media/sql-data-warehouse-create-logical-server/search-logical-server.png)
2. Vyberte **SQL Server (logický server)**. 

    ![výběr logického serveru](./media/sql-data-warehouse-create-logical-server/select-logical-server.png)
  
3. Klikněte na tlačítko **vytvořit** tooopen hello nové okno systému SQL Server (logický server).

   <kbd>![otevření okna logického serveru](./media/sql-data-warehouse-create-logical-server/open-logical-server-blade.png)</kbd><kbd>![okno logického serveru](./media/sql-data-warehouse-create-logical-server/logical-server-blade.png)</kbd>
  
3. V okně SQL Server (logický server) hello serveru název textového pole zadejte platný název pro nový logický server hello. Zelená značka zaškrtnutí označuje, že jste zadali platný název.
    
    ![Název nového serveru](./media/sql-data-warehouse-create-logical-server/new-name-logical-server.png)

    > [!IMPORTANT]
    > Hello plně kvalifikovaný název pro nový server bude < your_server_name >. database.windows.net.
    >
    
4. Hello serveru správce přihlášení textového pole zadejte uživatelské jméno pro přihlašovací jméno ověřování SQL hello pro tento server. Toto přihlášení je známý jako hlavní přihlášení na server hello. Zelená značka zaškrtnutí označuje, že jste zadali platný název.
    
    ![Přihlášení správce SQL](./media/sql-data-warehouse-create-logical-server/sql-admin-login.png)
5. V hello **heslo** a **potvrzení hesla** textová pole, zadejte heslo pro účet hlavní přihlášení na server hello. Zelená značka zaškrtnutí označuje, že jste zadali platné heslo.
    
    ![Heslo správce SQL](./media/sql-data-warehouse-create-logical-server/sql-admin-password.png)
6. Vyberte předplatné, ve které máte oprávnění toocreate objektů.

    ![předplatné](./media/sql-data-warehouse-create-logical-server/subscription.png)
7. Do textového pole hello prostředků skupiny, vyberte **vytvořit nový** a hello prostředků skupiny textového pole, zadejte platný název pro novou skupinu prostředků hello (můžete také použít existující skupinu prostředků. Pokud jste již vytvořili jednu pro sebe). Zelená značka zaškrtnutí označuje, že jste zadali platný název.

    ![Nová skupina prostředků](./media/sql-data-warehouse-create-logical-server/new-resource-group.png)

8. V hello **umístění** textového pole, vyberte data center odpovídající tooyour umístění – například "Austrálie – východ".
    
    ![Umístění serveru](./media/sql-data-warehouse-create-logical-server/server-location.png)
    
    > [!TIP]
    > Hello zaškrtávací políčko pro **povolit službám azure tooaccess server** na toto okno nelze změnit. Toto nastavení v okně brány firewall serveru hello můžete změnit. Další informace najdete v článku [Začínáme se zabezpečením](../articles/sql-database/sql-database-manage-servers-portal.md).
    >
    
9. Klikněte na možnost **Vytvořit**.

    ![Tlačítko Vytvořit](./media/sql-data-warehouse-create-logical-server/create.png)

