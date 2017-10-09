## <a name="set-up-hello-development-environment"></a><span data-ttu-id="9b779-101">Nastavit hello vývojového prostředí</span><span class="sxs-lookup"><span data-stu-id="9b779-101">Set up hello development environment</span></span>

<span data-ttu-id="9b779-102">V této části najdete nastavení vašeho vývojového prostředí, včetně vytváření aplikace ASP.NET MVC, přidává se připojení připojené služby, přidávání řadiče, a vyžaduje zadání hello direktivy oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="9b779-102">This section walks you setting up your development environment, including creating an ASP.NET MVC app, adding a Connected Services connection, adding a controller, and specifying hello required namespace directives.</span></span>

### <a name="create-an-aspnet-mvc-app-project"></a><span data-ttu-id="9b779-103">Vytvoření projektu aplikace ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="9b779-103">Create an ASP.NET MVC app project</span></span>

1. <span data-ttu-id="9b779-104">Otevřete sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9b779-104">Open Visual Studio.</span></span>

1. <span data-ttu-id="9b779-105">Vyberte **souboru -> Nový -> projekt** z hlavní nabídky hello</span><span class="sxs-lookup"><span data-stu-id="9b779-105">Select **File->New->Project** from hello main menu</span></span>

1. <span data-ttu-id="9b779-106">Na hello **nový projekt** dialogové okno, zadejte možnosti hello jako zvýrazněných v hello následující obrázek:</span><span class="sxs-lookup"><span data-stu-id="9b779-106">On hello **New Project** dialog, specify hello options as highlighted in hello following figure:</span></span>

    ![Vytvoření projektu ASP.NET](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-1.png)

1. <span data-ttu-id="9b779-108">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="9b779-108">Select **OK**.</span></span>

1. <span data-ttu-id="9b779-109">Na hello **nový projekt ASP.NET** dialogové okno, zadejte možnosti hello jako zvýrazněných v hello následující obrázek:</span><span class="sxs-lookup"><span data-stu-id="9b779-109">On hello **New ASP.NET Project** dialog, specify hello options as highlighted in hello following figure:</span></span>

    ![Zadejte MVC](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-2.png)

1. <span data-ttu-id="9b779-111">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="9b779-111">Select **OK**.</span></span>

### <a name="use-connected-services-tooconnect-tooan-azure-storage-account"></a><span data-ttu-id="9b779-112">Použít účet úložiště Azure tooan tooconnect připojení služby</span><span class="sxs-lookup"><span data-stu-id="9b779-112">Use Connected Services tooconnect tooan Azure storage account</span></span>

1. <span data-ttu-id="9b779-113">V hello **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt hello a hello místní nabídce vyberte **Přidat -> připojené služby**.</span><span class="sxs-lookup"><span data-stu-id="9b779-113">In hello **Solution Explorer**, right-click hello project, and from hello context menu, select **Add->Connected Service**.</span></span>

1. <span data-ttu-id="9b779-114">Na hello **přidat připojení službě** dialogovém okně, vyberte **Azure Storage**a potom vyberte **konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="9b779-114">On hello **Add Connected Service** dialog, select **Azure Storage**, and then select **Configure**.</span></span>

    ![Dialogové okno připojené služby](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-3.png)

1. <span data-ttu-id="9b779-116">Na hello **Azure Storage** dialogovém okně, vyberte hello účtu požadované úložiště Azure, se kterým chcete toowork a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="9b779-116">On hello **Azure Storage** dialog, select hello desired Azure storage account with which you want toowork, and select **Add**.</span></span>
