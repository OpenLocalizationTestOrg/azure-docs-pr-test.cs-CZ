<span data-ttu-id="0fd1a-101">Pokud chcete ve své aplikaci povolit přihlašování, budete muset vytvořit zásadu přihlašování.</span><span class="sxs-lookup"><span data-stu-id="0fd1a-101">To enable sign-in on your application, you will need to create a sign-in policy.</span></span> <span data-ttu-id="0fd1a-102">Tato zásada popisuje prostředí, kterými uživatelé budou procházet při přihlašování, a obsah tokenů, které bude aplikace přijímat po úspěšných přihlášeních.</span><span class="sxs-lookup"><span data-stu-id="0fd1a-102">This policy describes the experiences that consumers will go through during sign-in and the contents of tokens that the application will receive on successful sign-ins.</span></span>

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

<span data-ttu-id="0fd1a-103">V nastavení v části Zásady vyberte **Zásady registrace nebo přihlašování** a klikněte na **+ Přidat**.</span><span class="sxs-lookup"><span data-stu-id="0fd1a-103">In the policies section of settings, select **Sign-up or sign-in policies** and click **+ Add**.</span></span>

![Výběr zásad registrace nebo přihlašování a kliknutí na tlačítko Přidat](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-policy.png)

<span data-ttu-id="0fd1a-105">Zadejte **Název** zásady, který v aplikaci použijete jako referenci.</span><span class="sxs-lookup"><span data-stu-id="0fd1a-105">Enter a policy **Name** for your application to reference.</span></span> <span data-ttu-id="0fd1a-106">Zadejte například `SiUpIn`.</span><span class="sxs-lookup"><span data-stu-id="0fd1a-106">For example, enter `SiUpIn`.</span></span>

<span data-ttu-id="0fd1a-107">Vyberte **Zprostředkovatelé identity** a zaškrtněte políčko **Registrace pomocí e-mailu**.</span><span class="sxs-lookup"><span data-stu-id="0fd1a-107">Select **Identity providers** and check **Email signup**.</span></span> <span data-ttu-id="0fd1a-108">Volitelně můžete vybrat také zprostředkovatele sociální identity, pokud už jsou nakonfigurováni.</span><span class="sxs-lookup"><span data-stu-id="0fd1a-108">Optionally, you can also select social identity providers, if already configured.</span></span> <span data-ttu-id="0fd1a-109">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="0fd1a-109">Click **OK**.</span></span>

![Výběr možnosti Přihlášení místním účtem jako zprostředkovatele identity a kliknutí na tlačítko OK](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-identity-providers.png)

<span data-ttu-id="0fd1a-111">Vyberte **Atributy registrace**.</span><span class="sxs-lookup"><span data-stu-id="0fd1a-111">Select **Sign-up attributes**.</span></span> <span data-ttu-id="0fd1a-112">Zvolte atributy, které od uživatele chcete během registrace shromažďovat.</span><span class="sxs-lookup"><span data-stu-id="0fd1a-112">Choose attributes you want to collect from the consumer during sign-up.</span></span> <span data-ttu-id="0fd1a-113">Zaškrtněte například políčka **Země/oblast**, **Zobrazované jméno** a **PSČ**.</span><span class="sxs-lookup"><span data-stu-id="0fd1a-113">For example, check **Country/Region**, **Display Name**, and **Postal Code**.</span></span> <span data-ttu-id="0fd1a-114">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="0fd1a-114">Click **OK**.</span></span>

![Výběr atributů a kliknutí na tlačítko OK](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-sign-up-attributes.png)

<span data-ttu-id="0fd1a-116">Vyberte **Deklarace identit aplikace**.</span><span class="sxs-lookup"><span data-stu-id="0fd1a-116">Select **Application claims**.</span></span> <span data-ttu-id="0fd1a-117">Zvolte deklarace identit, které se mají vracet v autorizačních tokenech odesílaných zpět do aplikace po úspěšné registraci nebo přihlášení.</span><span class="sxs-lookup"><span data-stu-id="0fd1a-117">Choose claims you want returned in the authorization tokens sent back to your application after a successful sign-up or sign-in experience.</span></span> <span data-ttu-id="0fd1a-118">Vyberte například **Zobrazované jméno**, **Zprostředkovatel identity**, **PSČ**, **Uživatel je nový** a **ID objektu uživatele**.</span><span class="sxs-lookup"><span data-stu-id="0fd1a-118">For example, select **Display Name**, **Identity Provider**, **Postal Code**, **User is new** and **User's Object ID**.</span></span>

![Výběr deklarací identit aplikace a kliknutí na tlačítko OK](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-application-claims.png)

<span data-ttu-id="0fd1a-120">Kliknutím na **Vytvořit** přidejte zásadu.</span><span class="sxs-lookup"><span data-stu-id="0fd1a-120">Click **Create** to add the policy.</span></span> <span data-ttu-id="0fd1a-121">Zásada se zobrazí jako **B2C_1_SiUpIn**.</span><span class="sxs-lookup"><span data-stu-id="0fd1a-121">The policy is listed as **B2C_1_SiUpIn**.</span></span> <span data-ttu-id="0fd1a-122">K názvu se připojí předpona **B2C_1_**.</span><span class="sxs-lookup"><span data-stu-id="0fd1a-122">The **B2C_1_** prefix is appended to the name.</span></span>

<span data-ttu-id="0fd1a-123">Otevřete zásadu výběrem **B2C_1_SiUpIn**.</span><span class="sxs-lookup"><span data-stu-id="0fd1a-123">Open the policy by selecting **B2C_1_SiUpIn**.</span></span> <span data-ttu-id="0fd1a-124">Ověřte nastavení uvedená v tabulce a potom klikněte na **Spustit**.</span><span class="sxs-lookup"><span data-stu-id="0fd1a-124">Verify the settings specified in the table then click **Run now**.</span></span>

![Výběr a spuštění zásady](media/active-directory-b2c-create-sign-in-sign-up-policy/run-b2c-signup-signin-policy.png)

| <span data-ttu-id="0fd1a-126">Nastavení</span><span class="sxs-lookup"><span data-stu-id="0fd1a-126">Setting</span></span>      | <span data-ttu-id="0fd1a-127">Hodnota</span><span class="sxs-lookup"><span data-stu-id="0fd1a-127">Value</span></span>  |
| ------------ | ------ |
| <span data-ttu-id="0fd1a-128">**Aplikace**</span><span class="sxs-lookup"><span data-stu-id="0fd1a-128">**Applications**</span></span> | <span data-ttu-id="0fd1a-129">Aplikace Contoso B2C</span><span class="sxs-lookup"><span data-stu-id="0fd1a-129">Contoso B2C app</span></span> |
| <span data-ttu-id="0fd1a-130">**Výběr adresy URL odpovědi**</span><span class="sxs-lookup"><span data-stu-id="0fd1a-130">**Select reply url**</span></span> | `https://localhost:44316/` |

<span data-ttu-id="0fd1a-131">Otevře se nová karta prohlížeče a můžete zkontrolovat nakonfigurované uživatelské prostředí pro registraci a přihlášení.</span><span class="sxs-lookup"><span data-stu-id="0fd1a-131">A new browser tab opens, and you can verify the sign-up or sign-in consumer experience as configured.</span></span>

> [!NOTE]
> <span data-ttu-id="0fd1a-132">Trvá až minutu, než se vytvoření zásady a aktualizace projeví.</span><span class="sxs-lookup"><span data-stu-id="0fd1a-132">It takes up to a minute for policy creation and updates to take effect.</span></span>
>