<span data-ttu-id="ea36f-101">profil tooenable úpravy ve vaší aplikaci, budete potřebovat toocreate profil úpravy zásad.</span><span class="sxs-lookup"><span data-stu-id="ea36f-101">tooenable profile editing on your application, you will need toocreate a profile editing policy.</span></span> <span data-ttu-id="ea36f-102">Tato zásada popisuje hello prostředí, které budou uživatelé procházejí během profil úpravy a hello obsah tokeny, které aplikace hello obdrží při úspěšném dokončení.</span><span class="sxs-lookup"><span data-stu-id="ea36f-102">This policy describes hello experiences that consumers will go through during profile editing and hello contents of tokens that hello application will receive on successful completion.</span></span>

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

<span data-ttu-id="ea36f-103">V části zásady hello nastavení, vyberte **zásady pro úpravy profilu** a klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="ea36f-103">In hello policies section of settings, select **Profile editing policies** and click **+ Add**.</span></span>

![Vyberte zásady pro úpravy profilu a klikněte na tlačítko Přidat hello](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-policy.png)

<span data-ttu-id="ea36f-105">Zadejte zásadu **název** pro tooreference vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ea36f-105">Enter a policy **Name** for your application tooreference.</span></span> <span data-ttu-id="ea36f-106">Zadejte například `SiPe`.</span><span class="sxs-lookup"><span data-stu-id="ea36f-106">For example, enter `SiPe`.</span></span>

<span data-ttu-id="ea36f-107">Vyberte **Zprostředkovatelé identity** a zaškrtněte políčko **Přihlášení místním účtem**.</span><span class="sxs-lookup"><span data-stu-id="ea36f-107">Select **Identity providers** and check **Local Account Signin**.</span></span> <span data-ttu-id="ea36f-108">Volitelně můžete vybrat také zprostředkovatele sociální identity, pokud už jsou nakonfigurováni.</span><span class="sxs-lookup"><span data-stu-id="ea36f-108">Optionally, you can also select social identity providers, if already configured.</span></span> <span data-ttu-id="ea36f-109">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="ea36f-109">Click **OK**.</span></span>

![Vyberte místní přihlášení účtu jako zprostředkovatel identity a klikněte na tlačítko OK hello](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-identity-providers.png)

<span data-ttu-id="ea36f-111">Vyberte **Atributy profilu**.</span><span class="sxs-lookup"><span data-stu-id="ea36f-111">Select **Profile attributes**.</span></span> <span data-ttu-id="ea36f-112">Vyberte atributy hello příjemce můžete zobrazit a upravit v svůj profil.</span><span class="sxs-lookup"><span data-stu-id="ea36f-112">Choose attributes hello consumer can view and edit in their profile.</span></span> <span data-ttu-id="ea36f-113">Zaškrtněte například políčka **Země/oblast**, **Zobrazované jméno** a **PSČ**.</span><span class="sxs-lookup"><span data-stu-id="ea36f-113">For example, check **Country/Region**, **Display Name**, and **Postal Code**.</span></span> <span data-ttu-id="ea36f-114">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="ea36f-114">Click **OK**.</span></span>

![Vyberte některých atributů a klikněte na tlačítko OK hello](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-attributes.png)

<span data-ttu-id="ea36f-116">Vyberte **Deklarace identit aplikace**.</span><span class="sxs-lookup"><span data-stu-id="ea36f-116">Select **Application claims**.</span></span> <span data-ttu-id="ea36f-117">Vyberte, že deklarace identity, které má vrátit v tokenech autorizace hello odeslána zpět tooyour aplikace po úspěšné profil prostředí pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="ea36f-117">Choose claims you want returned in hello authorization tokens sent back tooyour application after a successful profile editing experience.</span></span> <span data-ttu-id="ea36f-118">Vyberte například **Zobrazované jméno** a **PSČ**.</span><span class="sxs-lookup"><span data-stu-id="ea36f-118">For example, select **Display Name**, **Postal Code**.</span></span>

![Výběr deklarací identit aplikace a kliknutí na tlačítko OK](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-application-claims.png)

<span data-ttu-id="ea36f-120">Klikněte na tlačítko **vytvořit** tooadd hello zásad.</span><span class="sxs-lookup"><span data-stu-id="ea36f-120">Click **Create** tooadd hello policy.</span></span> <span data-ttu-id="ea36f-121">zásady Hello je uveden jako **B2C_1_SiPe**.</span><span class="sxs-lookup"><span data-stu-id="ea36f-121">hello policy is listed as **B2C_1_SiPe**.</span></span> <span data-ttu-id="ea36f-122">Hello **B2C_1_** předpona je název připojení toohello.</span><span class="sxs-lookup"><span data-stu-id="ea36f-122">hello **B2C_1_** prefix is appended toohello name.</span></span>

<span data-ttu-id="ea36f-123">Spustit nástroj Zásady hello výběrem **B2C_1_SiPe**.</span><span class="sxs-lookup"><span data-stu-id="ea36f-123">Open hello policy by selecting **B2C_1_SiPe**.</span></span> <span data-ttu-id="ea36f-124">Ověřte nastavení hello zadané v tabulce hello pak klikněte na tlačítko **spustit nyní**.</span><span class="sxs-lookup"><span data-stu-id="ea36f-124">Verify hello settings specified in hello table then click **Run now**.</span></span>

![Výběr a spuštění zásady](media/active-directory-b2c-create-profile-editing-policy/run-b2c-editing-policy.png)

| <span data-ttu-id="ea36f-126">Nastavení</span><span class="sxs-lookup"><span data-stu-id="ea36f-126">Setting</span></span>      | <span data-ttu-id="ea36f-127">Hodnota</span><span class="sxs-lookup"><span data-stu-id="ea36f-127">Value</span></span>  |
| ------------ | ------ |
| <span data-ttu-id="ea36f-128">**Aplikace**</span><span class="sxs-lookup"><span data-stu-id="ea36f-128">**Applications**</span></span> | <span data-ttu-id="ea36f-129">Aplikace Contoso B2C</span><span class="sxs-lookup"><span data-stu-id="ea36f-129">Contoso B2C app</span></span> |
| <span data-ttu-id="ea36f-130">**Výběr adresy URL odpovědi**</span><span class="sxs-lookup"><span data-stu-id="ea36f-130">**Select reply url**</span></span> | `https://localhost:44316/` |

<span data-ttu-id="ea36f-131">Otevře novou kartu prohlížeče a můžete ověřit podle konfigurace pro úpravy prostředí pro uživatele hello profilu.</span><span class="sxs-lookup"><span data-stu-id="ea36f-131">A new browser tab opens, and you can verify hello profile editing consumer experience as configured.</span></span>

> [!NOTE]
> <span data-ttu-id="ea36f-132">Zabíral tooa minut pro vytvoření zásad a aktualizuje tootake vliv.</span><span class="sxs-lookup"><span data-stu-id="ea36f-132">It takes up tooa minute for policy creation and updates tootake effect.</span></span>
>