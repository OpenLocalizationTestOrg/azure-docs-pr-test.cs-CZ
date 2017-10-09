
1. Klikněte na tlačítko hello **App Services** tlačítko vyberte vaše mobilní aplikace back-endu, vyberte **rychlý Start**a pak vybrat klientskou platformu (iOS, Android, Xamarin, Cordova).

    ![Azure Portal se zvýrazněnou možností Rychlý start Mobile Apps][quickstart]

2. Pokud není nakonfigurováno připojení k databázi, vytvořte jednu pomocí tohoto postupu hello následující:

    ![Portál Azure s mobilní aplikací připojení toodatabase][connect]

    a. Vytvořte novou databázi SQL a server.

    ![Azure Portal s vytvořením nové databáze a serveru pro Mobile Apps][server]

    b. Počkejte, dokud hello datové připojení se úspěšně vytvořil.

    ![Oznámení na webu Azure Portal o úspěšném vytvoření datového připojení][notification]

    c. Vytvoření datového připojení musí proběhnout úspěšně.

    ![Oznámení na webu Azure Portal s textem „Už máte datové připojení“][already-connection]

3. V části **2. Vytvoření rozhraní API tabulky** vyberte jako **Jazyk back-endu** možnost Node.js. 
 
4. Přijměte potvrzení hello a pak vyberte **vytvořit tabulku TodoItem**.  
    Tato akce ve vaší databázi vytvoří novou tabulku úkolů. 

    >[!IMPORTANT]
    > Přepínání existující tooNode.js back-end přepíše veškerý obsah. Místo toho najdete v článku .NET back-end toocreate [pracovat s hello .NET back-end serveru SDK pro Mobile Apps][instructions].

<!-- Images. -->
[quickstart]: ./media/app-service-mobile-configure-new-backend/quickstart.png
[connect]: ./media/app-service-mobile-configure-new-backend/connect-to-bd.png
[notification]: ./media/app-service-mobile-configure-new-backend/notification-data-connection-create.png
[server]: ./media/app-service-mobile-configure-new-backend/create-new-server.png
[already-connection]: ./media/app-service-mobile-configure-new-backend/already-connection.png

<!-- URLs -->
[instructions]: ../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app
