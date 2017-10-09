## <a name="create-an-iot-hub"></a><span data-ttu-id="35417-101">Vytvoření centra IoT</span><span class="sxs-lookup"><span data-stu-id="35417-101">Create an IoT hub</span></span>
<span data-ttu-id="35417-102">Vytvoření služby IoT hub pro vaše aplikace tooconnect simulované zařízení k.</span><span class="sxs-lookup"><span data-stu-id="35417-102">Create an IoT hub for your simulated device app tooconnect to.</span></span> <span data-ttu-id="35417-103">Hello následující kroky vám ukážou, jak toocomplete tato úloha pomocí portálu Azure pomocí hello.</span><span class="sxs-lookup"><span data-stu-id="35417-103">hello following steps show you how toocomplete this task by using hello Azure portal.</span></span>

1. <span data-ttu-id="35417-104">Přihlaste se toohello [portál Azure][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="35417-104">Sign in toohello [Azure portal][lnk-portal].</span></span>
1. <span data-ttu-id="35417-105">V hello vlevo, klikněte na **nový** > **Internet věcí** > **IoT Hub**.</span><span class="sxs-lookup"><span data-stu-id="35417-105">In hello Jumpbar, click **New** > **Internet of Things** > **IoT Hub**.</span></span>
   
    ![Panel portálu Azure][1]
1. <span data-ttu-id="35417-107">V hello **služby IoT hub** okně zvolte hello konfiguraci pro službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="35417-107">In hello **IoT hub** blade, choose hello configuration for your IoT hub.</span></span>
   
    ![Okno centra IoT][2]
   
   1. <span data-ttu-id="35417-109">V hello **název** pole, zadejte název své služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="35417-109">In hello **Name** box, enter a name for your IoT hub.</span></span> <span data-ttu-id="35417-110">Pokud hello **název** je platný a dostupný, zobrazí se zelená značka zaškrtnutí v hello **název** pole.</span><span class="sxs-lookup"><span data-stu-id="35417-110">If hello **Name** is valid and available, a green check mark appears in hello **Name** box.</span></span>
    [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]
   
   1. <span data-ttu-id="35417-111">Vyberte [cenovou a škálovací úroveň][lnk-pricing].</span><span class="sxs-lookup"><span data-stu-id="35417-111">Select a [pricing and scale tier][lnk-pricing].</span></span> <span data-ttu-id="35417-112">Tento kurz nevyžaduje konkrétní úroveň.</span><span class="sxs-lookup"><span data-stu-id="35417-112">This tutorial does not require a specific tier.</span></span> <span data-ttu-id="35417-113">V tomto kurzu použijte bezplatnou úroveň F1 hello.</span><span class="sxs-lookup"><span data-stu-id="35417-113">For this tutorial, use hello free F1 tier.</span></span>
   1. <span data-ttu-id="35417-114">V části **Skupina prostředků** vytvořte skupinu prostředků nebo vyberte existující skupinu.</span><span class="sxs-lookup"><span data-stu-id="35417-114">In **Resource group**, either create a resource group, or select an existing one.</span></span> <span data-ttu-id="35417-115">Další informace najdete v tématu [pomocí prostředků skupiny prostředků Azure toomanage][lnk-resource-groups].</span><span class="sxs-lookup"><span data-stu-id="35417-115">For more information, see [Using resource groups toomanage your Azure resources][lnk-resource-groups].</span></span>
   1. <span data-ttu-id="35417-116">V **umístění**, vyberte hello toohost umístění služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="35417-116">In **Location**, select hello location toohost your IoT hub.</span></span> <span data-ttu-id="35417-117">V tomto kurzu zvolte nejbližší umístění.</span><span class="sxs-lookup"><span data-stu-id="35417-117">For this tutorial, choose your nearest location.</span></span>
1. <span data-ttu-id="35417-118">Po výběru možností konfigurace centra IoT klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="35417-118">When you have chosen your IoT hub configuration options, click **Create**.</span></span>  <span data-ttu-id="35417-119">Může trvat několik minut, než Azure toocreate IoT hub.</span><span class="sxs-lookup"><span data-stu-id="35417-119">It can take a few minutes for Azure toocreate your IoT hub.</span></span> <span data-ttu-id="35417-120">Stav hello toocheck, můžete sledovat průběh hello hello úvodním panelu nebo panelu oznámení hello.</span><span class="sxs-lookup"><span data-stu-id="35417-120">toocheck hello status, you can monitor hello progress on hello Startboard or in hello Notifications panel.</span></span>
   
    ![Stav nového centra IoT][3]
1. <span data-ttu-id="35417-122">Po úspěšném vytvoření služby IoT hub hello, klikněte na hello dlaždici nového centra IoT hello Azure portálu tooopen hello okně hello novou službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="35417-122">When hello IoT hub has been created successfully, click hello new tile for your IoT hub in hello Azure portal tooopen hello blade for hello new IoT hub.</span></span> <span data-ttu-id="35417-123">Poznamenejte si hello **Hostname**a potom klikněte na **zásady sdíleného přístupu**.</span><span class="sxs-lookup"><span data-stu-id="35417-123">Make a note of hello **Hostname**, and then click **Shared access policies**.</span></span>
   
    ![Okno nového centra IoT][4]
1. <span data-ttu-id="35417-125">V hello **zásady sdíleného přístupu** okně klikněte na tlačítko hello **iothubowner** zásady a pak zkopírujte a poznamenejte připojovací řetězec služby IoT Hub hello hello **iothubowner** okno.</span><span class="sxs-lookup"><span data-stu-id="35417-125">In hello **Shared access policies** blade, click hello **iothubowner** policy, and then copy and make note of hello IoT Hub connection string in hello **iothubowner** blade.</span></span> <span data-ttu-id="35417-126">Další informace najdete v tématu [řízení přístupu] [ lnk-access-control] v hello "Příručka vývojáře IoT Hub."</span><span class="sxs-lookup"><span data-stu-id="35417-126">For more information, see [Access control][lnk-access-control] in hello "IoT Hub developer guide."</span></span>
   
    ![Okno zásad sdíleného přístupu][5]

<!-- Images. -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
[3]: ./media/iot-hub-get-started-create-hub/create-iot-hub3.png
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
