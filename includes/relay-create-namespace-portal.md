1. <span data-ttu-id="7f2dd-101">Přihlaste se k webu [Azure Portal][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="7f2dd-101">Log on to the [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="7f2dd-102">V levém navigačním podokně portálu klikněte na **Nový**, pak klikněte na **Podniková integrace** a pak na **Relay**.</span><span class="sxs-lookup"><span data-stu-id="7f2dd-102">In the left navigation pane of the portal, click **New**, then click **Enterprise Integration**, and then click **Relay**.</span></span>
3. <span data-ttu-id="7f2dd-103">V dialogovém okně **Vytvořit obor názvů** zadejte název oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="7f2dd-103">In the **Create namespace** dialog, enter a namespace name.</span></span> <span data-ttu-id="7f2dd-104">Systém okamžitě kontroluje, jestli je název dostupný.</span><span class="sxs-lookup"><span data-stu-id="7f2dd-104">The system immediately checks to see if the name is available.</span></span>
4. <span data-ttu-id="7f2dd-105">V poli **Předplatné** zvolte předplatné Azure, ve které chcete vytvořit obor názvů.</span><span class="sxs-lookup"><span data-stu-id="7f2dd-105">In the **Subscription** field, choose an Azure subscription in which to create the namespace.</span></span>
5. <span data-ttu-id="7f2dd-106">V poli **[Skupina prostředků](../articles/azure-resource-manager/resource-group-portal.md)** zvolte existující skupinu prostředků, ve které bude obor názvů fungovat, nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="7f2dd-106">In the **[Resource group](../articles/azure-resource-manager/resource-group-portal.md)** field, choose an existing resource group in which the namespace will live, or create a new one.</span></span>      
6. <span data-ttu-id="7f2dd-107">V poli **Umístění**, vyberte zemi nebo oblast, ve které by měl být oboru názvů hostován.</span><span class="sxs-lookup"><span data-stu-id="7f2dd-107">In **Location**, choose the country or region in which your namespace should be hosted.</span></span>
   
    ![Vytvoření oboru názvů][create-namespace]
7. <span data-ttu-id="7f2dd-109">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="7f2dd-109">Click **Create**.</span></span> <span data-ttu-id="7f2dd-110">Systém teď vytvoří obor názvů a povolí ho.</span><span class="sxs-lookup"><span data-stu-id="7f2dd-110">The system now creates your namespace and enables it.</span></span> <span data-ttu-id="7f2dd-111">Po několika minutách systém zřídí prostředky pro váš účet.</span><span class="sxs-lookup"><span data-stu-id="7f2dd-111">After a few minutes, the system provisions resources for your account.</span></span>

### <a name="obtain-the-management-credentials"></a><span data-ttu-id="7f2dd-112">Získání přihlašovacích údajů pro správu</span><span class="sxs-lookup"><span data-stu-id="7f2dd-112">Obtain the management credentials</span></span>
1. <span data-ttu-id="7f2dd-113">V seznamu oborů názvů klikněte na nově vytvořený obor názvů.</span><span class="sxs-lookup"><span data-stu-id="7f2dd-113">In the list of namespaces, click the newly created namespace name.</span></span>
2. <span data-ttu-id="7f2dd-114">V okně oboru názvů klikněte na **Zásady sdíleného přístupu**.</span><span class="sxs-lookup"><span data-stu-id="7f2dd-114">In the namespace blade, click **Shared access policies**.</span></span>
3. <span data-ttu-id="7f2dd-115">V okně **Zásady sdíleného přístupu** klikněte na **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="7f2dd-115">In the **Shared access policies** blade, click **RootManageSharedAccessKey**.</span></span>
   
    ![connection-info][connection-info]
4. <span data-ttu-id="7f2dd-117">V okně **Zásada: RootManageSharedAccessKey** klikněte na tlačítko kopírování vedle položky **Připojovací řetězec – primární klíč**, abyste zkopírovali připojovací řetězec do vaší schránky pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="7f2dd-117">In the **Policy: RootManageSharedAccessKey** blade, click the copy button next to **Connection string–primary key**, to copy the connection string to your clipboard for later use.</span></span> <span data-ttu-id="7f2dd-118">Vložte tuto hodnotu do Poznámkového bloku nebo jiného dočasného umístění.</span><span class="sxs-lookup"><span data-stu-id="7f2dd-118">Paste this value into Notepad or some other temporary location.</span></span>
   
    ![connection-string][connection-string]

5. <span data-ttu-id="7f2dd-120">Opakujte předchozí krok, zkopírujte si hodnotu pro **primární klíč** a vložte ji do dočasného umístění pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="7f2dd-120">Repeat the previous step, copying and pasting the value of **Primary key** to a temporary location for later use.</span></span>  

<!--Image references-->

[create-namespace]: ./media/relay-create-namespace-portal/create-namespace.png
[connection-info]: ./media/relay-create-namespace-portal/connection-info.png
[connection-string]: ./media/relay-create-namespace-portal/connection-string.png
[Azure portal]: https://portal.azure.com
