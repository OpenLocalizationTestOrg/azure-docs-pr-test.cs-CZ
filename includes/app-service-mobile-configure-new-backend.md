
1. <span data-ttu-id="c0014-101">Klikněte **App Services** tlačítko vyberte vaše mobilní aplikace back-endu, vyberte **rychlý Start**a pak vybrat klientskou platformu (iOS, Android, Xamarin, Cordova).</span><span class="sxs-lookup"><span data-stu-id="c0014-101">Click the **App Services** button, select your Mobile Apps back end, select **Quickstart**, and then select your client platform (iOS, Android, Xamarin, Cordova).</span></span>

    ![Portál Azure s mobilní aplikace rychlý start zvýrazněná][quickstart]

2. <span data-ttu-id="c0014-103">Pokud není nakonfigurováno připojení k databázi, vytvořte ji následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="c0014-103">If a database connection is not configured, create one by doing the following:</span></span>

    ![Portál Azure s mobilní aplikací připojení k databázi][connect]

    <span data-ttu-id="c0014-105">a.</span><span class="sxs-lookup"><span data-stu-id="c0014-105">a.</span></span> <span data-ttu-id="c0014-106">Vytvořte novou databázi SQL a serveru.</span><span class="sxs-lookup"><span data-stu-id="c0014-106">Create a new SQL database and server.</span></span>

    ![Portál Azure s Mobile Apps vytvářet nové databáze a serveru][server]

    <span data-ttu-id="c0014-108">b.</span><span class="sxs-lookup"><span data-stu-id="c0014-108">b.</span></span> <span data-ttu-id="c0014-109">Počkejte na úspěšné vytvoření datového připojení.</span><span class="sxs-lookup"><span data-stu-id="c0014-109">Wait until the data connection is successfully created.</span></span>

    ![Azure portálu oznámení o úspěšném vytvoření datové připojení][notification]

    <span data-ttu-id="c0014-111">c.</span><span class="sxs-lookup"><span data-stu-id="c0014-111">c.</span></span> <span data-ttu-id="c0014-112">Vytvoření datového připojení musí proběhnout úspěšně.</span><span class="sxs-lookup"><span data-stu-id="c0014-112">Data connection must be successful.</span></span>

    ![Azure portálu oznámení "Již máte data připojení"][already-connection]

3. <span data-ttu-id="c0014-114">V části **2. Vytvoření rozhraní API tabulky** vyberte jako **Jazyk back-endu** možnost Node.js.</span><span class="sxs-lookup"><span data-stu-id="c0014-114">Under **2. Create a table API**, select Node.js for **Backend language**.</span></span> 
 
4. <span data-ttu-id="c0014-115">Přijměte potvrzení a pak vyberte **vytvořit tabulku TodoItem**.</span><span class="sxs-lookup"><span data-stu-id="c0014-115">Accept the acknowledgment, and then select **Create TodoItem table**.</span></span>  
    <span data-ttu-id="c0014-116">Tato akce vytvoří novou tabulku položky úkolů ve vaší databázi.</span><span class="sxs-lookup"><span data-stu-id="c0014-116">This action creates a new to-do item table in your database.</span></span> 

    >[!IMPORTANT]
    > <span data-ttu-id="c0014-117">Probíhá přepínání existující back-end na Node.js přepíše veškerý obsah.</span><span class="sxs-lookup"><span data-stu-id="c0014-117">Switching an existing back end to Node.js overwrites all contents.</span></span> <span data-ttu-id="c0014-118">Místo toho vytvoření rozhraní .NET back-end, naleznete v tématu [pracovat s .NET back-end server SDK pro Mobile Apps][instructions].</span><span class="sxs-lookup"><span data-stu-id="c0014-118">To create a .NET back end instead, see [Work with the .NET back-end server SDK for Mobile Apps][instructions].</span></span>

<!-- Images. -->
[quickstart]: ./media/app-service-mobile-configure-new-backend/quickstart.png
[connect]: ./media/app-service-mobile-configure-new-backend/connect-to-bd.png
[notification]: ./media/app-service-mobile-configure-new-backend/notification-data-connection-create.png
[server]: ./media/app-service-mobile-configure-new-backend/create-new-server.png
[already-connection]: ./media/app-service-mobile-configure-new-backend/already-connection.png

<!-- URLs -->
[instructions]: ../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app
