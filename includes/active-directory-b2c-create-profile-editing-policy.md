<span data-ttu-id="92c7c-101">Pokud chcete ve své aplikaci povolit upravování profilu, budete muset vytvořit zásadu upravování profilu.</span><span class="sxs-lookup"><span data-stu-id="92c7c-101">To enable profile editing on your application, you will need to create a profile editing policy.</span></span> <span data-ttu-id="92c7c-102">Tato zásada popisuje prostředí, kterými uživatelé budou procházet při upravování profilu, a obsah tokenů, které bude aplikace přijímat po úspěšném dokončení.</span><span class="sxs-lookup"><span data-stu-id="92c7c-102">This policy describes the experiences that consumers will go through during profile editing and the contents of tokens that the application will receive on successful completion.</span></span>

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

<span data-ttu-id="92c7c-103">V nastavení v části Zásady vyberte **Zásady upravování profilu** a klikněte na **+ Přidat**.</span><span class="sxs-lookup"><span data-stu-id="92c7c-103">In the policies section of settings, select **Profile editing policies** and click **+ Add**.</span></span>

![Výběr zásad upravování profilu a kliknutí na tlačítko Přidat](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-policy.png)

<span data-ttu-id="92c7c-105">Zadejte **Název** zásady, který v aplikaci použijete jako referenci.</span><span class="sxs-lookup"><span data-stu-id="92c7c-105">Enter a policy **Name** for your application to reference.</span></span> <span data-ttu-id="92c7c-106">Zadejte například `SiPe`.</span><span class="sxs-lookup"><span data-stu-id="92c7c-106">For example, enter `SiPe`.</span></span>

<span data-ttu-id="92c7c-107">Vyberte **Zprostředkovatelé identity** a zaškrtněte políčko **Přihlášení místním účtem**.</span><span class="sxs-lookup"><span data-stu-id="92c7c-107">Select **Identity providers** and check **Local Account Signin**.</span></span> <span data-ttu-id="92c7c-108">Volitelně můžete vybrat také zprostředkovatele sociální identity, pokud už jsou nakonfigurováni.</span><span class="sxs-lookup"><span data-stu-id="92c7c-108">Optionally, you can also select social identity providers, if already configured.</span></span> <span data-ttu-id="92c7c-109">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="92c7c-109">Click **OK**.</span></span>

![Výběr možnosti Přihlášení místním účtem jako zprostředkovatele identity a kliknutí na tlačítko OK](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-identity-providers.png)

<span data-ttu-id="92c7c-111">Vyberte **Atributy profilu**.</span><span class="sxs-lookup"><span data-stu-id="92c7c-111">Select **Profile attributes**.</span></span> <span data-ttu-id="92c7c-112">Zvolte, které atributy může uživatel ve svém profilu zobrazit a upravit.</span><span class="sxs-lookup"><span data-stu-id="92c7c-112">Choose attributes the consumer can view and edit in their profile.</span></span> <span data-ttu-id="92c7c-113">Zaškrtněte například políčka **Země/oblast**, **Zobrazované jméno** a **PSČ**.</span><span class="sxs-lookup"><span data-stu-id="92c7c-113">For example, check **Country/Region**, **Display Name**, and **Postal Code**.</span></span> <span data-ttu-id="92c7c-114">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="92c7c-114">Click **OK**.</span></span>

![Výběr atributů a kliknutí na tlačítko OK](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-attributes.png)

<span data-ttu-id="92c7c-116">Vyberte **Deklarace identit aplikace**.</span><span class="sxs-lookup"><span data-stu-id="92c7c-116">Select **Application claims**.</span></span> <span data-ttu-id="92c7c-117">Zvolte deklarace identit, které se mají vracet v autorizačních tokenech odesílaných zpět do aplikace po úspěšném upravování profilu.</span><span class="sxs-lookup"><span data-stu-id="92c7c-117">Choose claims you want returned in the authorization tokens sent back to your application after a successful profile editing experience.</span></span> <span data-ttu-id="92c7c-118">Vyberte například **Zobrazované jméno** a **PSČ**.</span><span class="sxs-lookup"><span data-stu-id="92c7c-118">For example, select **Display Name**, **Postal Code**.</span></span>

![Výběr deklarací identit aplikace a kliknutí na tlačítko OK](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-application-claims.png)

<span data-ttu-id="92c7c-120">Kliknutím na **Vytvořit** přidejte zásadu.</span><span class="sxs-lookup"><span data-stu-id="92c7c-120">Click **Create** to add the policy.</span></span> <span data-ttu-id="92c7c-121">Zásada se zobrazí jako **B2C_1_SiPe**.</span><span class="sxs-lookup"><span data-stu-id="92c7c-121">The policy is listed as **B2C_1_SiPe**.</span></span> <span data-ttu-id="92c7c-122">K názvu se připojí předpona **B2C_1_**.</span><span class="sxs-lookup"><span data-stu-id="92c7c-122">The **B2C_1_** prefix is appended to the name.</span></span>

<span data-ttu-id="92c7c-123">Otevřete zásadu výběrem **B2C_1_SiPe**.</span><span class="sxs-lookup"><span data-stu-id="92c7c-123">Open the policy by selecting **B2C_1_SiPe**.</span></span> <span data-ttu-id="92c7c-124">Ověřte nastavení uvedená v tabulce a potom klikněte na **Spustit**.</span><span class="sxs-lookup"><span data-stu-id="92c7c-124">Verify the settings specified in the table then click **Run now**.</span></span>

![Výběr a spuštění zásady](media/active-directory-b2c-create-profile-editing-policy/run-b2c-editing-policy.png)

| <span data-ttu-id="92c7c-126">Nastavení</span><span class="sxs-lookup"><span data-stu-id="92c7c-126">Setting</span></span>      | <span data-ttu-id="92c7c-127">Hodnota</span><span class="sxs-lookup"><span data-stu-id="92c7c-127">Value</span></span>  |
| ------------ | ------ |
| <span data-ttu-id="92c7c-128">**Aplikace**</span><span class="sxs-lookup"><span data-stu-id="92c7c-128">**Applications**</span></span> | <span data-ttu-id="92c7c-129">Aplikace Contoso B2C</span><span class="sxs-lookup"><span data-stu-id="92c7c-129">Contoso B2C app</span></span> |
| <span data-ttu-id="92c7c-130">**Výběr adresy URL odpovědi**</span><span class="sxs-lookup"><span data-stu-id="92c7c-130">**Select reply url**</span></span> | `https://localhost:44316/` |

<span data-ttu-id="92c7c-131">Otevře se nová karta prohlížeče a můžete zkontrolovat nakonfigurované uživatelské prostředí pro upravování profilu.</span><span class="sxs-lookup"><span data-stu-id="92c7c-131">A new browser tab opens, and you can verify the profile editing consumer experience as configured.</span></span>

> [!NOTE]
> <span data-ttu-id="92c7c-132">Trvá až minutu, než se vytvoření zásady a aktualizace projeví.</span><span class="sxs-lookup"><span data-stu-id="92c7c-132">It takes up to a minute for policy creation and updates to take effect.</span></span>
>