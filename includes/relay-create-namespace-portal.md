1. <span data-ttu-id="ee4b5-101">Přihlaste se toohello [portál Azure][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="ee4b5-101">Log on toohello [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="ee4b5-102">V levém navigačním podokně hello hello portálu, klikněte na **nový**, pak klikněte na tlačítko **Enterprise integrace**a potom klikněte na **předávání**.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-102">In hello left navigation pane of hello portal, click **New**, then click **Enterprise Integration**, and then click **Relay**.</span></span>
3. <span data-ttu-id="ee4b5-103">V hello **vytvoření oboru názvů** dialogové okno, zadejte název oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-103">In hello **Create namespace** dialog, enter a namespace name.</span></span> <span data-ttu-id="ee4b5-104">systém Hello toosee ověří okamžitě, pokud je k dispozici název hello.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-104">hello system immediately checks toosee if hello name is available.</span></span>
4. <span data-ttu-id="ee4b5-105">V hello **předplatné** pole, zvolte předplatné Azure v oboru názvů které toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-105">In hello **Subscription** field, choose an Azure subscription in which toocreate hello namespace.</span></span>
5. <span data-ttu-id="ee4b5-106">V hello  **[skupiny prostředků](../articles/azure-resource-manager/resource-group-portal.md)**  pole, vyberte existující skupinu prostředků v které hello obor názvů se za provozu nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-106">In hello **[Resource group](../articles/azure-resource-manager/resource-group-portal.md)** field, choose an existing resource group in which hello namespace will live, or create a new one.</span></span>      
6. <span data-ttu-id="ee4b5-107">V **umístění**, zvolte hello zemi nebo oblast, ve kterém by hostovat vašeho oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-107">In **Location**, choose hello country or region in which your namespace should be hosted.</span></span>
   
    ![Vytvoření oboru názvů][create-namespace]
7. <span data-ttu-id="ee4b5-109">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-109">Click **Create**.</span></span> <span data-ttu-id="ee4b5-110">Hello systém teď vytvoří obor názvů a povolí ho.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-110">hello system now creates your namespace and enables it.</span></span> <span data-ttu-id="ee4b5-111">Po několika minutách hello systém zřídí prostředky pro váš účet.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-111">After a few minutes, hello system provisions resources for your account.</span></span>

### <a name="obtain-hello-management-credentials"></a><span data-ttu-id="ee4b5-112">Získání pověření pro správu hello</span><span class="sxs-lookup"><span data-stu-id="ee4b5-112">Obtain hello management credentials</span></span>
1. <span data-ttu-id="ee4b5-113">V seznamu hello oborů názvů klikněte na tlačítko hello nově vytvořený obor názvů.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-113">In hello list of namespaces, click hello newly created namespace name.</span></span>
2. <span data-ttu-id="ee4b5-114">V okně hello obor názvů, klikněte na tlačítko **zásady sdíleného přístupu**.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-114">In hello namespace blade, click **Shared access policies**.</span></span>
3. <span data-ttu-id="ee4b5-115">V hello **zásady sdíleného přístupu** okně klikněte na tlačítko **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-115">In hello **Shared access policies** blade, click **RootManageSharedAccessKey**.</span></span>
   
    ![connection-info][connection-info]
4. <span data-ttu-id="ee4b5-117">V hello **zásady: RootManageSharedAccessKey** okně klikněte na tlačítko Kopírovat hello další příliš**připojovací řetězec – primární klíč**, toocopy hello připojovací řetězec tooyour schránky pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-117">In hello **Policy: RootManageSharedAccessKey** blade, click hello copy button next too**Connection string–primary key**, toocopy hello connection string tooyour clipboard for later use.</span></span> <span data-ttu-id="ee4b5-118">Vložte tuto hodnotu do Poznámkového bloku nebo jiného dočasného umístění.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-118">Paste this value into Notepad or some other temporary location.</span></span>
   
    ![connection-string][connection-string]

5. <span data-ttu-id="ee4b5-120">Předchozí krok zopakujte hello, kopírování a vkládání hello hodnotu **primární klíč** tooa dočasného umístění pro pozdější použití.</span><span class="sxs-lookup"><span data-stu-id="ee4b5-120">Repeat hello previous step, copying and pasting hello value of **Primary key** tooa temporary location for later use.</span></span>  

<!--Image references-->

[create-namespace]: ./media/relay-create-namespace-portal/create-namespace.png
[connection-info]: ./media/relay-create-namespace-portal/connection-info.png
[connection-string]: ./media/relay-create-namespace-portal/connection-string.png
[Azure portal]: https://portal.azure.com
