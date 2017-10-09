<span data-ttu-id="066cd-101">tooenable podrobných heslo resetovat na aplikace, budete potřebovat toocreate zásady resetování hesel.</span><span class="sxs-lookup"><span data-stu-id="066cd-101">tooenable fine-grained password reset on your application, you will need toocreate a password reset policy.</span></span> <span data-ttu-id="066cd-102">Poznámka: Toto heslo klienta celou hello resetovat zadána možnost [zde](../articles/active-directory-b2c/active-directory-b2c-reference-sspr.md).</span><span class="sxs-lookup"><span data-stu-id="066cd-102">Note that hello tenant-wide password reset option specified [here](../articles/active-directory-b2c/active-directory-b2c-reference-sspr.md).</span></span> <span data-ttu-id="066cd-103">Tato zásada popisuje hello prostředí, které budou spotřebitelé hello procházejí během resetování hesla a hello obsah tokeny, které hello aplikace se zobrazí při úspěšném dokončení.</span><span class="sxs-lookup"><span data-stu-id="066cd-103">This policy describes hello experiences that hello consumers will go through during password reset and hello contents of tokens that hello application will receive on successful completion.</span></span>

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

<span data-ttu-id="066cd-104">V části zásady hello nastavení, vyberte **heslo resetovat zásady** a klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="066cd-104">In hello policies section of settings, select **Password reset policies** and click **+ Add**.</span></span>

![Vyberte zásady registrace nebo přihlášení a klikněte na tlačítko Přidat hello](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-policy.png)

<span data-ttu-id="066cd-106">Zadejte zásadu **název** pro tooreference vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="066cd-106">Enter a policy **Name** for your application tooreference.</span></span> <span data-ttu-id="066cd-107">Zadejte například `SSPR`.</span><span class="sxs-lookup"><span data-stu-id="066cd-107">For example, enter `SSPR`.</span></span>

<span data-ttu-id="066cd-108">Vyberte **Zprostředkovatelé identity** a zaškrtněte políčko **Resetovat heslo pomocí e-mailové adresy**.</span><span class="sxs-lookup"><span data-stu-id="066cd-108">Select **Identity providers** and check **Reset password using email address**.</span></span> <span data-ttu-id="066cd-109">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="066cd-109">Click **OK**.</span></span>

![Vyberte resetovat heslo pomocí e-mailovou adresu jako zprostředkovatel identity a klikněte na tlačítko OK hello](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-identity-providers.png)

<span data-ttu-id="066cd-111">Vyberte **Deklarace identit aplikace**.</span><span class="sxs-lookup"><span data-stu-id="066cd-111">Select **Application claims**.</span></span> <span data-ttu-id="066cd-112">Vyberte, že deklarace identity, které má vrátit v tokenech autorizace hello odeslána zpět tooyour aplikace po úspěšné heslo resetovat prostředí.</span><span class="sxs-lookup"><span data-stu-id="066cd-112">Choose claims you want returned in hello authorization tokens sent back tooyour application after a successful password reset experience.</span></span> <span data-ttu-id="066cd-113">Vyberte například **ID objektu uživatele**.</span><span class="sxs-lookup"><span data-stu-id="066cd-113">For example, select **User's Object ID**.</span></span>

![Výběr deklarací identit aplikace a kliknutí na tlačítko OK](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-application-claims.png)

<span data-ttu-id="066cd-115">Klikněte na tlačítko **vytvořit** tooadd hello zásad.</span><span class="sxs-lookup"><span data-stu-id="066cd-115">Click **Create** tooadd hello policy.</span></span> <span data-ttu-id="066cd-116">zásady Hello je uveden jako **B2C_1_SSPR**.</span><span class="sxs-lookup"><span data-stu-id="066cd-116">hello policy is listed as **B2C_1_SSPR**.</span></span> <span data-ttu-id="066cd-117">Hello **B2C_1_** předpona je název připojení toohello.</span><span class="sxs-lookup"><span data-stu-id="066cd-117">hello **B2C_1_** prefix is appended toohello name.</span></span>

<span data-ttu-id="066cd-118">Spustit nástroj Zásady hello výběrem **B2C_1_SSPR**.</span><span class="sxs-lookup"><span data-stu-id="066cd-118">Open hello policy by selecting **B2C_1_SSPR**.</span></span> <span data-ttu-id="066cd-119">Ověřte nastavení hello zadané v tabulce hello pak klikněte na tlačítko **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="066cd-119">Verify hello settings specified in hello table then click **Run now**.</span></span>

![Výběr a spuštění zásady](media/active-directory-b2c-create-password-reset-policy/run-b2c-password-reset-policy.png)

| <span data-ttu-id="066cd-121">Nastavení</span><span class="sxs-lookup"><span data-stu-id="066cd-121">Setting</span></span>      | <span data-ttu-id="066cd-122">Hodnota</span><span class="sxs-lookup"><span data-stu-id="066cd-122">Value</span></span>  |
| ------------ | ------ |
| <span data-ttu-id="066cd-123">**Aplikace**</span><span class="sxs-lookup"><span data-stu-id="066cd-123">**Applications**</span></span> | <span data-ttu-id="066cd-124">Aplikace Contoso B2C</span><span class="sxs-lookup"><span data-stu-id="066cd-124">Contoso B2C app</span></span> |
| <span data-ttu-id="066cd-125">**Výběr adresy URL odpovědi**</span><span class="sxs-lookup"><span data-stu-id="066cd-125">**Select reply url**</span></span> | `https://localhost:44316/` |

<span data-ttu-id="066cd-126">Otevře novou kartu prohlížeče a můžete ověřit, že resetování hesla hello prostředí pro uživatele ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="066cd-126">A new browser tab opens, and you can verify hello password reset consumer experience in your application.</span></span>

> [!NOTE]
> <span data-ttu-id="066cd-127">Zabíral tooa minut pro vytvoření zásad a aktualizuje tootake vliv.</span><span class="sxs-lookup"><span data-stu-id="066cd-127">It takes up tooa minute for policy creation and updates tootake effect.</span></span>
>
