
1. <span data-ttu-id="853c2-101">Klikněte na tlačítko hello **App Services** tlačítko vyberte vaše mobilní aplikace back-endu, vyberte **rychlý Start**a pak vybrat klientskou platformu (iOS, Android, Xamarin, Cordova).</span><span class="sxs-lookup"><span data-stu-id="853c2-101">Click hello **App Services** button, select your Mobile Apps back end, select **Quickstart**, and then select your client platform (iOS, Android, Xamarin, Cordova).</span></span>

    ![Azure Portal se zvýrazněnou možností Rychlý start Mobile Apps][quickstart]

2. <span data-ttu-id="853c2-103">Pokud není nakonfigurováno připojení k databázi, vytvořte jednu pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="853c2-103">If a database connection is not configured, create one by doing hello following:</span></span>

    ![Portál Azure s mobilní aplikací připojení toodatabase][connect]

    <span data-ttu-id="853c2-105">a.</span><span class="sxs-lookup"><span data-stu-id="853c2-105">a.</span></span> <span data-ttu-id="853c2-106">Vytvořte novou databázi SQL a server.</span><span class="sxs-lookup"><span data-stu-id="853c2-106">Create a new SQL database and server.</span></span>

    ![Azure Portal s vytvořením nové databáze a serveru pro Mobile Apps][server]

    <span data-ttu-id="853c2-108">b.</span><span class="sxs-lookup"><span data-stu-id="853c2-108">b.</span></span> <span data-ttu-id="853c2-109">Počkejte, dokud hello datové připojení se úspěšně vytvořil.</span><span class="sxs-lookup"><span data-stu-id="853c2-109">Wait until hello data connection is successfully created.</span></span>

    ![Oznámení na webu Azure Portal o úspěšném vytvoření datového připojení][notification]

    <span data-ttu-id="853c2-111">c.</span><span class="sxs-lookup"><span data-stu-id="853c2-111">c.</span></span> <span data-ttu-id="853c2-112">Vytvoření datového připojení musí proběhnout úspěšně.</span><span class="sxs-lookup"><span data-stu-id="853c2-112">Data connection must be successful.</span></span>

    ![Oznámení na webu Azure Portal s textem „Už máte datové připojení“][already-connection]

3. <span data-ttu-id="853c2-114">V části **2. Vytvoření rozhraní API tabulky** vyberte jako **Jazyk back-endu** možnost Node.js.</span><span class="sxs-lookup"><span data-stu-id="853c2-114">Under **2. Create a table API**, select Node.js for **Backend language**.</span></span> 
 
4. <span data-ttu-id="853c2-115">Přijměte potvrzení hello a pak vyberte **vytvořit tabulku TodoItem**.</span><span class="sxs-lookup"><span data-stu-id="853c2-115">Accept hello acknowledgment, and then select **Create TodoItem table**.</span></span>  
    <span data-ttu-id="853c2-116">Tato akce ve vaší databázi vytvoří novou tabulku úkolů.</span><span class="sxs-lookup"><span data-stu-id="853c2-116">This action creates a new to-do item table in your database.</span></span> 

    >[!IMPORTANT]
    > <span data-ttu-id="853c2-117">Přepínání existující tooNode.js back-end přepíše veškerý obsah.</span><span class="sxs-lookup"><span data-stu-id="853c2-117">Switching an existing back end tooNode.js overwrites all contents.</span></span> <span data-ttu-id="853c2-118">Místo toho najdete v článku .NET back-end toocreate [pracovat s hello .NET back-end serveru SDK pro Mobile Apps][instructions].</span><span class="sxs-lookup"><span data-stu-id="853c2-118">toocreate a .NET back end instead, see [Work with hello .NET back-end server SDK for Mobile Apps][instructions].</span></span>

<!-- Images. -->
[quickstart]: ./media/app-service-mobile-configure-new-backend/quickstart.png
[connect]: ./media/app-service-mobile-configure-new-backend/connect-to-bd.png
[notification]: ./media/app-service-mobile-configure-new-backend/notification-data-connection-create.png
[server]: ./media/app-service-mobile-configure-new-backend/create-new-server.png
[already-connection]: ./media/app-service-mobile-configure-new-backend/already-connection.png

<!-- URLs -->
[instructions]: ../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app
