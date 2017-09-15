<span data-ttu-id="cb20d-101">Pokud chcete ve své aplikaci povolit jemné resetování hesla, budete muset vytvořit zásadu resetování hesel.</span><span class="sxs-lookup"><span data-stu-id="cb20d-101">To enable fine-grained password reset on your application, you will need to create a password reset policy.</span></span> <span data-ttu-id="cb20d-102">Všimněte si, že možnost resetování hesel v rámci celého tenanta je specifikovaná [zde](../articles/active-directory-b2c/active-directory-b2c-reference-sspr.md).</span><span class="sxs-lookup"><span data-stu-id="cb20d-102">Note that the tenant-wide password reset option specified [here](../articles/active-directory-b2c/active-directory-b2c-reference-sspr.md).</span></span> <span data-ttu-id="cb20d-103">Tato zásada popisuje prostředí, kterými uživatelé budou procházet při resetování hesla, a obsah tokenů, které bude aplikace přijímat po úspěšném dokončení.</span><span class="sxs-lookup"><span data-stu-id="cb20d-103">This policy describes the experiences that the consumers will go through during password reset and the contents of tokens that the application will receive on successful completion.</span></span>

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

<span data-ttu-id="cb20d-104">V nastavení v části Zásady vyberte **Zásady resetování hesel** a klikněte na **+ Přidat**.</span><span class="sxs-lookup"><span data-stu-id="cb20d-104">In the policies section of settings, select **Password reset policies** and click **+ Add**.</span></span>

![Výběr zásad registrace nebo přihlášení a kliknutí na tlačítko Přidat](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-policy.png)

<span data-ttu-id="cb20d-106">Zadejte **Název** zásady, který v aplikaci použijete jako referenci.</span><span class="sxs-lookup"><span data-stu-id="cb20d-106">Enter a policy **Name** for your application to reference.</span></span> <span data-ttu-id="cb20d-107">Zadejte například `SSPR`.</span><span class="sxs-lookup"><span data-stu-id="cb20d-107">For example, enter `SSPR`.</span></span>

<span data-ttu-id="cb20d-108">Vyberte **Zprostředkovatelé identity** a zaškrtněte políčko **Resetovat heslo pomocí e-mailové adresy**.</span><span class="sxs-lookup"><span data-stu-id="cb20d-108">Select **Identity providers** and check **Reset password using email address**.</span></span> <span data-ttu-id="cb20d-109">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="cb20d-109">Click **OK**.</span></span>

![Výběr možnosti Resetovat heslo pomocí e-mailové adresy jako zprostředkovatele identity a kliknutí na tlačítko OK](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-identity-providers.png)

<span data-ttu-id="cb20d-111">Vyberte **Deklarace identit aplikace**.</span><span class="sxs-lookup"><span data-stu-id="cb20d-111">Select **Application claims**.</span></span> <span data-ttu-id="cb20d-112">Zvolte deklarace identit, které se mají vracet v autorizačních tokenech odesílaných zpět do aplikace po úspěšném resetování hesel.</span><span class="sxs-lookup"><span data-stu-id="cb20d-112">Choose claims you want returned in the authorization tokens sent back to your application after a successful password reset experience.</span></span> <span data-ttu-id="cb20d-113">Vyberte například **ID objektu uživatele**.</span><span class="sxs-lookup"><span data-stu-id="cb20d-113">For example, select **User's Object ID**.</span></span>

![Výběr deklarací identit aplikace a kliknutí na tlačítko OK](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-application-claims.png)

<span data-ttu-id="cb20d-115">Kliknutím na **Vytvořit** přidejte zásadu.</span><span class="sxs-lookup"><span data-stu-id="cb20d-115">Click **Create** to add the policy.</span></span> <span data-ttu-id="cb20d-116">Zásada se zobrazí jako **B2C_1_SSPR**.</span><span class="sxs-lookup"><span data-stu-id="cb20d-116">The policy is listed as **B2C_1_SSPR**.</span></span> <span data-ttu-id="cb20d-117">K názvu se připojí předpona **B2C_1_**.</span><span class="sxs-lookup"><span data-stu-id="cb20d-117">The **B2C_1_** prefix is appended to the name.</span></span>

<span data-ttu-id="cb20d-118">Otevřete zásadu výběrem **B2C_1_SSPR**.</span><span class="sxs-lookup"><span data-stu-id="cb20d-118">Open the policy by selecting **B2C_1_SSPR**.</span></span> <span data-ttu-id="cb20d-119">Ověřte nastavení uvedená v tabulce a potom klikněte na **Spustit**.</span><span class="sxs-lookup"><span data-stu-id="cb20d-119">Verify the settings specified in the table then click **Run now**.</span></span>

![Výběr a spuštění zásady](media/active-directory-b2c-create-password-reset-policy/run-b2c-password-reset-policy.png)

| <span data-ttu-id="cb20d-121">Nastavení</span><span class="sxs-lookup"><span data-stu-id="cb20d-121">Setting</span></span>      | <span data-ttu-id="cb20d-122">Hodnota</span><span class="sxs-lookup"><span data-stu-id="cb20d-122">Value</span></span>  |
| ------------ | ------ |
| <span data-ttu-id="cb20d-123">**Aplikace**</span><span class="sxs-lookup"><span data-stu-id="cb20d-123">**Applications**</span></span> | <span data-ttu-id="cb20d-124">Aplikace Contoso B2C</span><span class="sxs-lookup"><span data-stu-id="cb20d-124">Contoso B2C app</span></span> |
| <span data-ttu-id="cb20d-125">**Výběr adresy URL odpovědi**</span><span class="sxs-lookup"><span data-stu-id="cb20d-125">**Select reply url**</span></span> | `https://localhost:44316/` |

<span data-ttu-id="cb20d-126">Otevře se nová karta prohlížeče a můžete zkontrolovat uživatelské prostředí pro resetování hesla ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="cb20d-126">A new browser tab opens, and you can verify the password reset consumer experience in your application.</span></span>

> [!NOTE]
> <span data-ttu-id="cb20d-127">Trvá až minutu, než se vytvoření zásady a aktualizace projeví.</span><span class="sxs-lookup"><span data-stu-id="cb20d-127">It takes up to a minute for policy creation and updates to take effect.</span></span>
>
