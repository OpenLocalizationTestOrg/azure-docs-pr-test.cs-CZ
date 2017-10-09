## <a name="prepare-tooauthenticate-azure-resource-manager-requests"></a><span data-ttu-id="ae295-101">Příprava tooauthenticate, které požadavky Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="ae295-101">Prepare tooauthenticate Azure Resource Manager requests</span></span>
<span data-ttu-id="ae295-102">Je třeba ověřit všechny hello operace, které můžete provádět na prostředky pomocí hello [Azure Resource Manager] [ lnk-authenticate-arm] s Azure Active Directory (AD).</span><span class="sxs-lookup"><span data-stu-id="ae295-102">You must authenticate all hello operations that you perform on resources using hello [Azure Resource Manager][lnk-authenticate-arm] with Azure Active Directory (AD).</span></span> <span data-ttu-id="ae295-103">Hello tooconfigure nejjednodušší způsob, jak je to toouse prostředí PowerShell nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="ae295-103">hello easiest way tooconfigure this is toouse PowerShell or Azure CLI.</span></span>

<span data-ttu-id="ae295-104">Nainstalujte hello [rutin prostředí Azure PowerShell] [ lnk-powershell-install] než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="ae295-104">Install hello [Azure PowerShell cmdlets][lnk-powershell-install] before you continue.</span></span>

<span data-ttu-id="ae295-105">Dobrý den, jak následující kroky zobrazit tooset až ověřování hesla pro aplikaci AD pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ae295-105">hello following steps show how tooset up password authentication for an AD application using PowerShell.</span></span> <span data-ttu-id="ae295-106">Tyto příkazy můžete spustit ve standardní relaci prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ae295-106">You can run these commands in a standard PowerShell session.</span></span>

1. <span data-ttu-id="ae295-107">Přihlaste se tooyour předplatného Azure, pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ae295-107">Log in tooyour Azure subscription using hello following command:</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

1. <span data-ttu-id="ae295-108">Pokud máte víc předplatných Azure, přihlášení tooAzure uděluje přístup tooall hello předplatná Azure přidružená přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="ae295-108">If you have multiple Azure subscriptions, signing in tooAzure grants you access tooall hello Azure subscriptions associated with your credentials.</span></span> <span data-ttu-id="ae295-109">Použijte následující příkaz toolist hello předplatná Azure k dispozici pro vás toouse hello:</span><span class="sxs-lookup"><span data-stu-id="ae295-109">Use hello following command toolist hello Azure subscriptions available for you toouse:</span></span>

    ```powershell
    Get-AzureRMSubscription
    ```

    <span data-ttu-id="ae295-110">Použijte následující příkaz tooselect předplatné, že budete chtít toouse toorun hello příkazy toomanage služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="ae295-110">Use hello following command tooselect subscription that you want toouse toorun hello commands toomanage your IoT hub.</span></span> <span data-ttu-id="ae295-111">Název odběru hello nebo ID můžete použít z hello výstup hello předchozí příkaz:</span><span class="sxs-lookup"><span data-stu-id="ae295-111">You can use either hello subscription name or ID from hello output of hello previous command:</span></span>

    ```powershell
    Select-AzureRMSubscription `
        -SubscriptionName "{your subscription name}"
    ```

2. <span data-ttu-id="ae295-112">Poznamenejte si vaše **TenantId** a **SubscriptionId**.</span><span class="sxs-lookup"><span data-stu-id="ae295-112">Make a note of your **TenantId** and **SubscriptionId**.</span></span> <span data-ttu-id="ae295-113">Budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="ae295-113">You need them later.</span></span>
3. <span data-ttu-id="ae295-114">Vytvořte novou aplikaci Azure Active Directory pomocí hello následující příkaz, nahraďte zástupného hello:</span><span class="sxs-lookup"><span data-stu-id="ae295-114">Create a new Azure Active Directory application using hello following command, replacing hello place holders:</span></span>
   
   * <span data-ttu-id="ae295-115">**{Zobrazovaný název}:** zobrazovaný název pro vaši aplikaci, jako **MySampleApp**</span><span class="sxs-lookup"><span data-stu-id="ae295-115">**{Display name}:** a display name for your application such as **MySampleApp**</span></span>
   * <span data-ttu-id="ae295-116">**{Adresu URL domovské stránky}:** hello URL hello domovské stránky aplikace jako **http://mysampleapp/home**.</span><span class="sxs-lookup"><span data-stu-id="ae295-116">**{Home page URL}:** hello URL of hello home page of your app such as **http://mysampleapp/home**.</span></span> <span data-ttu-id="ae295-117">Tato adresa URL nemusí toopoint tooa reálné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ae295-117">This URL does not need toopoint tooa real application.</span></span>
   * <span data-ttu-id="ae295-118">**{Identifikátor aplikace}:** jedinečný identifikátor, jako **http://mysampleapp**.</span><span class="sxs-lookup"><span data-stu-id="ae295-118">**{Application identifier}:** A unique identifier such as **http://mysampleapp**.</span></span> <span data-ttu-id="ae295-119">Tato adresa URL nemusí toopoint tooa reálné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ae295-119">This URL does not need toopoint tooa real application.</span></span>
   * <span data-ttu-id="ae295-120">**{Heslo}:** heslo používat tooauthenticate s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="ae295-120">**{Password}:** A password that you use tooauthenticate with your app.</span></span>
     
     ```powershell
     New-AzureRmADApplication -DisplayName {Display name} -HomePage {Home page URL} -IdentifierUris {Application identifier} -Password {Password}
     ```
4. <span data-ttu-id="ae295-121">Poznamenejte si hello **ApplicationId** hello aplikace, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="ae295-121">Make a note of hello **ApplicationId** of hello application you created.</span></span> <span data-ttu-id="ae295-122">Budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="ae295-122">You need this later.</span></span>
5. <span data-ttu-id="ae295-123">Vytvořit nový objekt služby pomocí hello následující příkaz, nahraďte **{MyApplicationId}** s hello **ApplicationId** hello v předchozím kroku:</span><span class="sxs-lookup"><span data-stu-id="ae295-123">Create a new service principal using hello following command, replacing **{MyApplicationId}** with hello **ApplicationId** from hello previous step:</span></span>
   
    ```powershell
    New-AzureRmADServicePrincipal -ApplicationId {MyApplicationId}
    ```
6. <span data-ttu-id="ae295-124">Nastavení přiřazení role pomocí hello následující příkaz, nahraďte **{MyApplicationId}** s vaší **ApplicationId**.</span><span class="sxs-lookup"><span data-stu-id="ae295-124">Set up a role assignment using hello following command, replacing **{MyApplicationId}** with your **ApplicationId**.</span></span>
   
    ```powershell
    New-AzureRmRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName {MyApplicationId}
    ```

<span data-ttu-id="ae295-125">Nyní dokončení vytváření hello aplikaci Azure AD, která vám umožní tooauthenticate z vaší vlastní aplikaci C#.</span><span class="sxs-lookup"><span data-stu-id="ae295-125">You have now finished creating hello Azure AD application that enables you tooauthenticate from your custom C# application.</span></span> <span data-ttu-id="ae295-126">Je třeba hello později v tomto kurzu následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="ae295-126">You need hello following values later in this tutorial:</span></span>

* <span data-ttu-id="ae295-127">TenantId</span><span class="sxs-lookup"><span data-stu-id="ae295-127">TenantId</span></span>
* <span data-ttu-id="ae295-128">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="ae295-128">SubscriptionId</span></span>
* <span data-ttu-id="ae295-129">ApplicationId</span><span class="sxs-lookup"><span data-stu-id="ae295-129">ApplicationId</span></span>
* <span data-ttu-id="ae295-130">Heslo</span><span class="sxs-lookup"><span data-stu-id="ae295-130">Password</span></span>

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
