1. Přihlaste se na hello [portálu Azure].
2. Klikněte na **+NOVÝ** > **Web + mobilní zařízení** > **Mobilní aplikace** a potom zadejte název back-endu mobilní aplikace .
3. Pro hello **skupiny prostředků**, vyberte existující skupinu prostředků nebo vytvořte novou (pomocí hello stejný název jako vaše aplikace.) 
   
    Můžete buď vybrat jiný plán služby App Service nebo vytvořit nový. Další informace o plánech aplikační služby a jak toocreate nového plánu v různých cenových vrstvy a v požadovaném umístění najdete v části [podrobný přehled plánů služby Azure App Service](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
4. Pro hello **plán služby App Service**, hello výchozí plán (v hello [úrovně Standard](https://azure.microsoft.com/pricing/details/app-service/)) je vybrána. Můžete také vybrat jiný plán nebo [vytvořte novou](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan). Hello plán služby App Service určuje nastavení hello [umístění, funkce a nákladové a výpočetní prostředky](https://azure.microsoft.com/pricing/details/app-service/) přidružené k vaší aplikaci. 
   
    Jakmile se rozhodnete hello plán, klikněte na tlačítko **vytvořit**. Tím vytvoříte back-end mobilní aplikace hello. 
5. V hello **nastavení** hello nový mobilní back-endu aplikace, klikněte na **úvodní** > aplikační platforma vašeho klienta > **připojit databázi**. 
   
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-data-connection.png)
6. V hello **přidat datové připojení** okně klikněte na tlačítko **SQL Database** > **vytvořit novou databázi**, databáze typu hello **název**, Zvolte cenovou úroveň a potom klikněte na **Server**.  Novou databázi můžete použít opakovaně. Pokud už máte databázi v hello stejné umístění, můžete místo toho zvolíte **použít existující databázi**. použití Hello databáze v jiném umístění se nedoporučuje kvůli toobandwidth náklady a zhorší latenci.
   
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png)
7. V hello **nový server** okno, zadejte název jedinečné serveru hello **název serveru** pole, zadejte přihlašovací jméno a heslo, zkontrolujte **povolit službám azure tooaccess server**a klikněte na tlačítko **OK**. Tím se vytvoří nová databáze hello.
8. Zpět v hello **přidat datové připojení** okně klikněte na tlačítko **připojovací řetězec**, zadejte hodnoty hello přihlašovací jméno a heslo pro vaši databázi a klikněte na tlačítko **OK**. Počkejte několik minut toobe hello databáze úspěšně nasadili předtím, než budete pokračovat.

<!-- URLs. -->
[portálu Azure]: https://portal.azure.com/
