## <a name="prepare-to-authenticate-azure-resource-manager-requests"></a><span data-ttu-id="b0241-101">Příprava k ověřování žádostí o Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b0241-101">Prepare to authenticate Azure Resource Manager requests</span></span>
<span data-ttu-id="b0241-102">Je třeba ověřit všechny operace, které můžete provádět na prostředky pomocí [Azure Resource Manager] [ lnk-authenticate-arm] s Azure Active Directory (AD).</span><span class="sxs-lookup"><span data-stu-id="b0241-102">You must authenticate all the operations that you perform on resources using the [Azure Resource Manager][lnk-authenticate-arm] with Azure Active Directory (AD).</span></span> <span data-ttu-id="b0241-103">Nejjednodušší způsob, jak nakonfigurovat tuto funkci je pomocí Powershellu nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="b0241-103">The easiest way to configure this is to use PowerShell or Azure CLI.</span></span>

<span data-ttu-id="b0241-104">Nainstalujte [rutin prostředí Azure PowerShell] [ lnk-powershell-install] než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="b0241-104">Install the [Azure PowerShell cmdlets][lnk-powershell-install] before you continue.</span></span>

<span data-ttu-id="b0241-105">Následující kroky ukazují, jak nastavit ověřování hesla pro aplikaci AD pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b0241-105">The following steps show how to set up password authentication for an AD application using PowerShell.</span></span> <span data-ttu-id="b0241-106">Tyto příkazy můžete spustit ve standardní relaci prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b0241-106">You can run these commands in a standard PowerShell session.</span></span>

1. <span data-ttu-id="b0241-107">Přihlaste se k předplatnému Azure, pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="b0241-107">Log in to your Azure subscription using the following command:</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

1. <span data-ttu-id="b0241-108">Pokud máte víc předplatných Azure, přihlášení do Azure uděluje přístup do všech předplatná Azure přidružená přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="b0241-108">If you have multiple Azure subscriptions, signing in to Azure grants you access to all the Azure subscriptions associated with your credentials.</span></span> <span data-ttu-id="b0241-109">Pomocí následujícího příkazu zobrazíte seznam předplatných Azure, které je k dispozici pro použití:</span><span class="sxs-lookup"><span data-stu-id="b0241-109">Use the following command to list the Azure subscriptions available for you to use:</span></span>

    ```powershell
    Get-AzureRMSubscription
    ```

    <span data-ttu-id="b0241-110">Pomocí následujícího příkazu vyberte předplatné, které chcete použít ke spuštění příkazů ke správě služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="b0241-110">Use the following command to select subscription that you want to use to run the commands to manage your IoT hub.</span></span> <span data-ttu-id="b0241-111">Z výstupu předchozí příkaz můžete použít buď název odběru nebo ID:</span><span class="sxs-lookup"><span data-stu-id="b0241-111">You can use either the subscription name or ID from the output of the previous command:</span></span>

    ```powershell
    Select-AzureRMSubscription `
        -SubscriptionName "{your subscription name}"
    ```

2. <span data-ttu-id="b0241-112">Poznamenejte si vaše **TenantId** a **SubscriptionId**.</span><span class="sxs-lookup"><span data-stu-id="b0241-112">Make a note of your **TenantId** and **SubscriptionId**.</span></span> <span data-ttu-id="b0241-113">Budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="b0241-113">You need them later.</span></span>
3. <span data-ttu-id="b0241-114">Vytvořte novou aplikaci Azure Active Directory pomocí následujícího příkazu, nahraďte svými držiteli místní:</span><span class="sxs-lookup"><span data-stu-id="b0241-114">Create a new Azure Active Directory application using the following command, replacing the place holders:</span></span>
   
   * <span data-ttu-id="b0241-115">**{Zobrazovaný název}:** zobrazovaný název pro vaši aplikaci, jako **MySampleApp**</span><span class="sxs-lookup"><span data-stu-id="b0241-115">**{Display name}:** a display name for your application such as **MySampleApp**</span></span>
   * <span data-ttu-id="b0241-116">**{Adresu URL domovské stránky}:** adresu URL domovské stránky aplikace jako **http://mysampleapp/home**.</span><span class="sxs-lookup"><span data-stu-id="b0241-116">**{Home page URL}:** the URL of the home page of your app such as **http://mysampleapp/home**.</span></span> <span data-ttu-id="b0241-117">Tato adresa URL nemusí tak, aby odkazoval na reálné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b0241-117">This URL does not need to point to a real application.</span></span>
   * <span data-ttu-id="b0241-118">**{Identifikátor aplikace}:** jedinečný identifikátor, jako **http://mysampleapp**.</span><span class="sxs-lookup"><span data-stu-id="b0241-118">**{Application identifier}:** A unique identifier such as **http://mysampleapp**.</span></span> <span data-ttu-id="b0241-119">Tato adresa URL nemusí tak, aby odkazoval na reálné aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b0241-119">This URL does not need to point to a real application.</span></span>
   * <span data-ttu-id="b0241-120">**{Heslo}:** heslo, které použijete k ověření s vaší aplikací.</span><span class="sxs-lookup"><span data-stu-id="b0241-120">**{Password}:** A password that you use to authenticate with your app.</span></span>
     
     ```powershell
     New-AzureRmADApplication -DisplayName {Display name} -HomePage {Home page URL} -IdentifierUris {Application identifier} -Password {Password}
     ```
4. <span data-ttu-id="b0241-121">Poznamenejte si **ApplicationId** aplikace, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="b0241-121">Make a note of the **ApplicationId** of the application you created.</span></span> <span data-ttu-id="b0241-122">Budete potřebovat později.</span><span class="sxs-lookup"><span data-stu-id="b0241-122">You need this later.</span></span>
5. <span data-ttu-id="b0241-123">Vytvořit nový objekt služby pomocí následujícího příkazu, nahraďte **{MyApplicationId}** s **ApplicationId** z předchozího kroku:</span><span class="sxs-lookup"><span data-stu-id="b0241-123">Create a new service principal using the following command, replacing **{MyApplicationId}** with the **ApplicationId** from the previous step:</span></span>
   
    ```powershell
    New-AzureRmADServicePrincipal -ApplicationId {MyApplicationId}
    ```
6. <span data-ttu-id="b0241-124">Nastavení přiřazení role pomocí následujícího příkazu, nahraďte **{MyApplicationId}** s vaší **ApplicationId**.</span><span class="sxs-lookup"><span data-stu-id="b0241-124">Set up a role assignment using the following command, replacing **{MyApplicationId}** with your **ApplicationId**.</span></span>
   
    ```powershell
    New-AzureRmRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName {MyApplicationId}
    ```

<span data-ttu-id="b0241-125">Nyní dokončení vytvoření aplikace Azure AD, která umožňuje ověření z vaší vlastní aplikaci C#.</span><span class="sxs-lookup"><span data-stu-id="b0241-125">You have now finished creating the Azure AD application that enables you to authenticate from your custom C# application.</span></span> <span data-ttu-id="b0241-126">Později v tomto kurzu potřebujete následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="b0241-126">You need the following values later in this tutorial:</span></span>

* <span data-ttu-id="b0241-127">TenantId</span><span class="sxs-lookup"><span data-stu-id="b0241-127">TenantId</span></span>
* <span data-ttu-id="b0241-128">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="b0241-128">SubscriptionId</span></span>
* <span data-ttu-id="b0241-129">ApplicationId</span><span class="sxs-lookup"><span data-stu-id="b0241-129">ApplicationId</span></span>
* <span data-ttu-id="b0241-130">Heslo</span><span class="sxs-lookup"><span data-stu-id="b0241-130">Password</span></span>

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
