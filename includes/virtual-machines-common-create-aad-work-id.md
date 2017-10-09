
<br>

> [!NOTE]
> <span data-ttu-id="3b2e1-101">Pokud správce vám byl přiřazen uživatelské jméno a heslo, je velmi pravděpodobné, že už máte svůj pracovní nebo školní ID (někdy také říká *ID organizace*).</span><span class="sxs-lookup"><span data-stu-id="3b2e1-101">If you were given a user name and password by an administrator, there's a good chance that you already have a work or school ID (also sometimes called an *organizational ID*).</span></span> <span data-ttu-id="3b2e1-102">Pokud ano, můžete okamžitě začít toouse tooaccess prostředky Azure, které vyžadují jednu váš účet Azure.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-102">If so, you can immediately begin toouse your Azure account tooaccess Azure resources that require one.</span></span> <span data-ttu-id="3b2e1-103">Pokud zjistíte, že tyto prostředky nelze použít, musíte tooreturn toothis článek o pomoc.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-103">If you find that you cannot use those resources, you may need tooreturn toothis article for help.</span></span> <span data-ttu-id="3b2e1-104">Další informace najdete v tématu [účty, které můžete použít pro přihlášení](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SignInAccounts) a [jak Azure předplatného je související tooAzure AD](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SubRelationToDir).</span><span class="sxs-lookup"><span data-stu-id="3b2e1-104">For more information, see [Accounts that you can use for sign in](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SignInAccounts) and [How an Azure subscription is related tooAzure AD](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SubRelationToDir).</span></span>
> 
> 

<span data-ttu-id="3b2e1-105">Hello kroky jsou jednoduché.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-105">hello steps are simple.</span></span> <span data-ttu-id="3b2e1-106">Je třeba toolocate vaše podepsaný na identitu ve hello portál Azure classic, zjišťovat vaše výchozí doménu služby Azure Active Directory a přidat nové uživatele tooit jako Azure společné správce.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-106">You need toolocate your signed on identity in hello Azure classic portal, discover your default Azure Active Directory domain, and add a new user tooit as an Azure co-administrator.</span></span>

## <a name="locate-your-default-directory-in-hello-azure-classic-portal"></a><span data-ttu-id="3b2e1-107">Najít výchozí adresář v hello portál Azure classic</span><span class="sxs-lookup"><span data-stu-id="3b2e1-107">Locate your default directory in hello Azure classic portal</span></span>
<span data-ttu-id="3b2e1-108">Začněte tím, že přihlášení toohello [portál Azure classic](https://manage.windowsazure.com) s vaši osobní identitu účtu Microsoft.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-108">Start by logging in toohello [Azure classic portal](https://manage.windowsazure.com) with your personal Microsoft account identity.</span></span> <span data-ttu-id="3b2e1-109">Po přihlášení se posuňte se dolů hello blue panely hello levé straně a klikněte na tlačítko **služby ACTIVE DIRECTORY**.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-109">After you are logged in, scroll down hello blue panel on hello left side and click **ACTIVE DIRECTORY**.</span></span>

![Azure Active Directory](./media/virtual-machines-common-create-aad-work-id/azureactivedirectorywidget.png)

<span data-ttu-id="3b2e1-111">Začněme tím hledání některé informace o vaší identity v Azure.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-111">Let's start by finding some information about your identity in Azure.</span></span> <span data-ttu-id="3b2e1-112">Měli byste vidět něco podobného jako hello následující v hlavním podokně hello, zobrazující, že máte jeden výchozí adresář.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-112">You should see something like hello following in hello main pane, showing that you have one default directory.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/defaultaadlisting.png)

<span data-ttu-id="3b2e1-113">Umožňuje zjistit některé další informace o něm.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-113">Let's find out some more information about it.</span></span> <span data-ttu-id="3b2e1-114">Klikněte na tlačítko hello výchozí adresář řádek, což vám přináší do hello výchozí adresář vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-114">Click hello default directory row, which brings you into hello default directory properties.</span></span>  

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectorypage.png)

<span data-ttu-id="3b2e1-115">tooview výchozí název domény hello, klikněte na tlačítko **domény**.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-115">tooview hello default domain name, click **DOMAINS**.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/domainclicktoseeyourdefaultdomain.png)

<span data-ttu-id="3b2e1-116">V tomto poli musí být schopný toosee při vytvoření hello účet Azure, Azure Active Directory vytvořit osobní výchozí doméně, která je hodnota hash (číslo vygenerovat z textového řetězce) pro své osobní ID použít jako subdoména onmicrosoft.com. To je toowhich domény hello nyní přidáte nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-116">Here you should be able toosee that when hello Azure account was created, Azure Active Directory created a personal default domain that is a hash value (a number generated from a string of text) of your personal ID used as a subdomain of onmicrosoft.com. That's hello domain toowhich you will now add a new user.</span></span>

## <a name="creating-a-new-user-in-hello-default-domain"></a><span data-ttu-id="3b2e1-117">Vytvoření nového uživatele v doméně výchozí hello</span><span class="sxs-lookup"><span data-stu-id="3b2e1-117">Creating a new user in hello default domain</span></span>
<span data-ttu-id="3b2e1-118">Klikněte na tlačítko **uživatelé** a vyhledejte vaší jeden osobní účet.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-118">Click **USERS** and look for your single personal account.</span></span> <span data-ttu-id="3b2e1-119">Měli byste vidět v hello **ZDROJEM** sloupec, který je **účtu Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-119">You should see in hello **SOURCED FROM** column that it is a **Microsoft account**.</span></span> <span data-ttu-id="3b2e1-120">Chceme toocreate uživatele ve výchozím. onmicrosoft.com domény Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-120">We want toocreate a user in your default .onmicrosoft.com Azure Active Directory domain.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectoryuserslisting.png)

<span data-ttu-id="3b2e1-121">Vytvoříme toofollow [tyto pokyny](https://technet.microsoft.com/library/hh967632.aspx#BKMK_1) v hello několika dalších krocích, ale použít konkrétní příklad.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-121">We're going toofollow [these instructions](https://technet.microsoft.com/library/hh967632.aspx#BKMK_1) in hello next few steps, but use a specific example.</span></span>

<span data-ttu-id="3b2e1-122">V dolní části hello hello stránky, klikněte na tlačítko **+ přidat uživatele**.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-122">At hello bottom of hello page, click **+ADD USER**.</span></span> <span data-ttu-id="3b2e1-123">Hello stránce, který se zobrazí, zadejte hello nové uživatelské jméno a ujistěte se, hello **typ uživatele** **nového uživatele ve vaší organizaci**.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-123">In hello page that appears, type hello new user name, and make hello **Type of User** a **New user in your organization**.</span></span> <span data-ttu-id="3b2e1-124">V tomto příkladu hello nové uživatelské jméno je `ahmet`.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-124">In this example, hello new user name is `ahmet`.</span></span> <span data-ttu-id="3b2e1-125">Vyberte hello výchozí doménu, který je zjištěn dříve jako hello doménu pro ahmet na e-mailovou adresu.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-125">Select hello default domain that you discovered previously as hello domain for ahmet's email address.</span></span> <span data-ttu-id="3b2e1-126">Klikněte na šipku další hello po dokončení.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-126">Click hello next arrow when finished.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/addingauserwithdirectorydropdown.png)

<span data-ttu-id="3b2e1-127">Přidat další podrobnosti pro Ahmet, ale ujistěte se, zda text hello tooselect odpovídající **ROLE** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-127">Add more details for Ahmet, but make sure tooselect hello appropriate **ROLE** value.</span></span> <span data-ttu-id="3b2e1-128">Je snadno toouse **globálního správce** pracují toomake zda věcí, ale pokud používáte roli nižší úrovně, které je vhodné.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-128">It's easy toouse **Global Admin** toomake sure things are working, but if you can use a lesser role, that's a good idea.</span></span> <span data-ttu-id="3b2e1-129">Tento příklad používá hello **uživatele** role.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-129">This example uses hello **User** role.</span></span> <span data-ttu-id="3b2e1-130">(Další informace v [oprávnění správce role](https://msdn.microsoft.com/library/azure/dn468213.aspx#BKMK_1).) Nepovolujte službu Multi-Factor authentication, pokud chcete, aby toouse vícefaktorového ověřování pro každý protokol v operaci.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-130">(Find out more at [Administrator permissions by role](https://msdn.microsoft.com/library/azure/dn468213.aspx#BKMK_1).) Do not enable multi-factor authentication unless you want toouse multifactor authentication for each log in operation.</span></span> <span data-ttu-id="3b2e1-131">Až budete hotovi, klikněte na šipku další hello.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-131">Click hello next arrow when you're finished.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/userprofileuseradmin.png)

<span data-ttu-id="3b2e1-132">Klikněte na tlačítko hello **vytvořit** tlačítko toogenerate a zobrazení Ahmet dočasné heslo.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-132">Click hello **create** button toogenerate and display a temporary password for Ahmet.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/gettemporarypasswordforuser.png)

<span data-ttu-id="3b2e1-133">Zkopírujte hello uživatelské jméno e-mailovou adresu, nebo použijte **odeslání HESLA v e-MAILU**.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-133">Copy hello user name email address, or use **SEND PASSWORD IN EMAIL**.</span></span> <span data-ttu-id="3b2e1-134">Hello informace toolog na budete potřebovat za chvíli.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-134">You'll need hello information toolog on shortly.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/receivedtemporarypassworddialog.png)

<span data-ttu-id="3b2e1-135">Teď byste měli vidět hello nového uživatele, **Ahmet hello vývojáře**, z domácích zdrojů z Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-135">Now you should see hello new user, **Ahmet hello Developer**, sourced from Azure Active Directory.</span></span> <span data-ttu-id="3b2e1-136">Hello nový pracovní nebo školní identity jste vytvořili v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-136">You've created hello new work or school identity with Azure Active Directory.</span></span> <span data-ttu-id="3b2e1-137">Ale tuto identitu ještě nemá oprávnění toouse Azure prostředky.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-137">However, this identity does not yet have permissions toouse Azure resources.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectoryusersaftercreate.png)

<span data-ttu-id="3b2e1-138">Pokud používáte **odeslání HESLA v e-MAILU**, je odeslána hello následující druh e-mailu.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-138">If you use **SEND PASSWORD IN EMAIL**, hello following kind of email is sent.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/emailreceivedfromnewusercreation.png)

## <a name="adding-azure-co-administrator-rights-for-subscriptions"></a><span data-ttu-id="3b2e1-139">Přidání práv Azure spolusprávce pro předplatné</span><span class="sxs-lookup"><span data-stu-id="3b2e1-139">Adding Azure co-administrator rights for subscriptions</span></span>
<span data-ttu-id="3b2e1-140">Teď musíte tooadd hello nového uživatele jako spolusprávce předplatného, hello nového uživatele můžete přihlásit toohello portálu pro správu.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-140">Now you need tooadd hello new user as a co-administrator of your subscription so hello new user can sign in toohello Management Portal.</span></span> <span data-ttu-id="3b2e1-141">toodo, hello levém panelu klikněte na tlačítko **nastavení**.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-141">toodo this, in hello lower-left panel click **Settings**.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/thesettingswidget.png)

<span data-ttu-id="3b2e1-142">V oblasti hello hlavní nastavení, klikněte na tlačítko **správci** v hello top a jste měli vidět jenom vaše osobní Microsoft identitu účtu.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-142">In hello main settings area, click **ADMINISTRATORS** at hello top and you should see only your personal Microsoft account identity.</span></span> <span data-ttu-id="3b2e1-143">V dolní části hello hello stránky, klikněte na tlačítko **+ přidat** toospecify společné správce.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-143">At hello bottom of hello page, click **+ADD** toospecify a co-administrator.</span></span> <span data-ttu-id="3b2e1-144">Zde zadejte e-mailovou adresu hello hello nového uživatele, které jste vytvořili, včetně výchozí doménu.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-144">Here, enter hello email address of hello new user you had created, including your default domain.</span></span> <span data-ttu-id="3b2e1-145">Jak ukazuje následující snímek obrazovky hello, se zobrazí zelená značka zaškrtnutí další uživatele toohello hello výchozí adresář.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-145">As shown in hello next screenshot, a green check mark appears next toohello user for hello default directory.</span></span> <span data-ttu-id="3b2e1-146">Mějte na paměti, tooselect všechny hello odběry, které chcete mít tooadminister tento uživatel toobe.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-146">Remember tooselect all of hello subscriptions that you would like this user toobe able tooadminister.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/addingnewuserascoadmin.png)

<span data-ttu-id="3b2e1-147">Až skončíte, teď byste měli vidět dvě uživatelů, včetně nové identity společné správce.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-147">When you are done, you should now see two users, including your new co-administrator identity.</span></span> <span data-ttu-id="3b2e1-148">Odhlaste se z portálu hello.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-148">Log out of hello portal.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/newuseraddedascoadministrator.png)

## <a name="logging-in-and-changing-hello-new-users-password"></a><span data-ttu-id="3b2e1-149">Protokolování a změna hello nové heslo uživatele</span><span class="sxs-lookup"><span data-stu-id="3b2e1-149">Logging in and changing hello new user's password</span></span>
<span data-ttu-id="3b2e1-150">Přihlaste se jako hello nového uživatele, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-150">Log in as hello new user you created.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/signinginwithnewuser.png)

<span data-ttu-id="3b2e1-151">Budete mít okamžitě výzvami toocreate nové heslo.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-151">You will immediately be prompted toocreate a new password.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/mustupdateyourpassword.png)

<span data-ttu-id="3b2e1-152">Jste měli údaje úspěch, který může vypadat následující hello.</span><span class="sxs-lookup"><span data-stu-id="3b2e1-152">You should be rewarded with success that looks like hello following.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/successtourdialog.png)

## <a name="next-steps"></a><span data-ttu-id="3b2e1-153">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3b2e1-153">Next steps</span></span>
<span data-ttu-id="3b2e1-154">Teď můžete použít vaše nové identity toouse Azure Active Directory [šablony skupiny prostředků Azure](../articles/xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="3b2e1-154">You can now use your new Azure Active Directory identity toouse [Azure resource group templates](../articles/xplat-cli-azure-resource-manager.md).</span></span>

    azure login
    info:    Executing command login
    warn:    Please note that currently you can login only via Microsoft organizational account or service principal. For instructions on how tooset them up, please read http://aka.ms/Dhf67j.
    Username: ahmet@aztrainpassxxxxxoutlook.onmicrosoft.com
    Password: *********
    /info:    Added subscription Azure Pass
    info:    Setting subscription Azure Pass as default
    +
    info:    login command OK
    ralph@local:~$ azure config mode arm
    info:    New mode is arm
    ralph@local:~$ azure group list
    info:    Executing command group list
    + Listing resource groups
    info:    No matched resource groups were found
    info:    group list command OK
    ralph@local:~$ azure group create newgroup westus
    info:    Executing command group create
    + Getting resource group newgroup
    + Creating resource group newgroup
    info:    Created resource group newgroup
    data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/newgroup
    data:    Name:                newgroup
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags:
    data:
    info:    group create command OK
