<span data-ttu-id="a0cbb-101">fronty toobegin pomocí služby Service Bus v Azure, musíte nejdřív vytvořit obor názvů s názvem, který je v rámci Azure jedinečný.</span><span class="sxs-lookup"><span data-stu-id="a0cbb-101">toobegin using Service Bus queues in Azure, you must first create a namespace with a name that is unique across Azure.</span></span> <span data-ttu-id="a0cbb-102">Obor názvů poskytuje kontejner oboru pro adresování prostředků služby Service Bus v rámci vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="a0cbb-102">A namespace provides a scoping container for addressing Service Bus resources within your application.</span></span>

<span data-ttu-id="a0cbb-103">toocreate obor názvů:</span><span class="sxs-lookup"><span data-stu-id="a0cbb-103">toocreate a namespace:</span></span>

1. <span data-ttu-id="a0cbb-104">Přihlaste se toohello [portál Azure][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="a0cbb-104">Log on toohello [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="a0cbb-105">V levém navigačním podokně hello hello portálu, klikněte na **nový**, pak klikněte na tlačítko **Enterprise integrace**a potom klikněte na **Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="a0cbb-105">In hello left navigation pane of hello portal, click **New**, then click **Enterprise Integration**, and then click **Service Bus**.</span></span>
3. <span data-ttu-id="a0cbb-106">V hello **vytvoření oboru názvů** dialogové okno, zadejte název oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="a0cbb-106">In hello **Create namespace** dialog, enter a namespace name.</span></span> <span data-ttu-id="a0cbb-107">systém Hello toosee ověří okamžitě, pokud je k dispozici název hello.</span><span class="sxs-lookup"><span data-stu-id="a0cbb-107">hello system immediately checks toosee if hello name is available.</span></span>
4. <span data-ttu-id="a0cbb-108">Po provedení že název oboru názvů hello je k dispozici, zvolte hello cenová úroveň (Basic, Standard nebo Premium).</span><span class="sxs-lookup"><span data-stu-id="a0cbb-108">After making sure hello namespace name is available, choose hello pricing tier (Basic, Standard, or Premium).</span></span>
5. <span data-ttu-id="a0cbb-109">V hello **předplatné** pole, zvolte předplatné Azure v oboru názvů které toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="a0cbb-109">In hello **Subscription** field, choose an Azure subscription in which toocreate hello namespace.</span></span>
6. <span data-ttu-id="a0cbb-110">V hello **skupiny prostředků** pole, vyberte existující skupinu prostředků v které hello obor názvů se za provozu nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="a0cbb-110">In hello **Resource group** field, choose an existing resource group in which hello namespace will live, or create a new one.</span></span>      
7. <span data-ttu-id="a0cbb-111">V **umístění**, zvolte hello zemi nebo oblast, ve kterém by hostovat vašeho oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="a0cbb-111">In **Location**, choose hello country or region in which your namespace should be hosted.</span></span>
   
    ![Vytvoření oboru názvů][create-namespace]
8. <span data-ttu-id="a0cbb-113">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a0cbb-113">Click **Create**.</span></span> <span data-ttu-id="a0cbb-114">Hello systém teď vytvoří obor názvů a povolí ho.</span><span class="sxs-lookup"><span data-stu-id="a0cbb-114">hello system now creates your namespace and enables it.</span></span> <span data-ttu-id="a0cbb-115">Toowait může mít několik minut hello systém zřídí prostředky pro váš účet.</span><span class="sxs-lookup"><span data-stu-id="a0cbb-115">You might have toowait several minutes as hello system provisions resources for your account.</span></span>

### <a name="obtain-hello-management-credentials"></a><span data-ttu-id="a0cbb-116">Získání pověření pro správu hello</span><span class="sxs-lookup"><span data-stu-id="a0cbb-116">Obtain hello management credentials</span></span>

1. <span data-ttu-id="a0cbb-117">V seznamu hello oborů názvů klikněte na tlačítko hello nově vytvořený obor názvů.</span><span class="sxs-lookup"><span data-stu-id="a0cbb-117">In hello list of namespaces, click hello newly created namespace name.</span></span>
2. <span data-ttu-id="a0cbb-118">V okně hello obor názvů, klikněte na tlačítko **zásady sdíleného přístupu**.</span><span class="sxs-lookup"><span data-stu-id="a0cbb-118">In hello namespace blade, click **Shared access policies**.</span></span>
3. <span data-ttu-id="a0cbb-119">V hello **zásady sdíleného přístupu** okně klikněte na tlačítko **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="a0cbb-119">In hello **Shared access policies** blade, click **RootManageSharedAccessKey**.</span></span>
   
    ![connection-info][connection-info]
4. <span data-ttu-id="a0cbb-121">V hello **zásady: RootManageSharedAccessKey** okně klikněte na tlačítko Kopírovat hello další příliš**připojovací řetězec – primární klíč**, toocopy hello připojovací řetězec tooyour schránky pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="a0cbb-121">In hello **Policy: RootManageSharedAccessKey** blade, click hello copy button next too**Connection string–primary key**, toocopy hello connection string tooyour clipboard for later use.</span></span> <span data-ttu-id="a0cbb-122">Vložte tuto hodnotu do Poznámkového bloku nebo jiného dočasného umístění.</span><span class="sxs-lookup"><span data-stu-id="a0cbb-122">Paste this value into Notepad or some other temporary location.</span></span>
   
    ![connection-string][connection-string]

5. <span data-ttu-id="a0cbb-124">Předchozí krok zopakujte hello, kopírování a vkládání hello hodnotu **primární klíč** tooa dočasného umístění pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="a0cbb-124">Repeat hello previous step, copying and pasting hello value of **Primary key** tooa temporary location for later use.</span></span>

<!--Image references-->

[create-namespace]: ./media/service-bus-create-namespace-portal/create-namespace.png
[connection-info]: ./media/service-bus-create-namespace-portal/connection-info.png
[connection-string]: ./media/service-bus-create-namespace-portal/connection-string.png
[Azure portal]: https://portal.azure.com
