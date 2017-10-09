## <a name="create-an-iot-hub"></a><span data-ttu-id="71695-101">Vytvoření centra IoT</span><span class="sxs-lookup"><span data-stu-id="71695-101">Create an IoT hub</span></span>

1. <span data-ttu-id="71695-102">V hello [portál Azure](https://portal.azure.com/), klikněte na tlačítko **nový** > **Internet věcí** > **IoT Hub**.</span><span class="sxs-lookup"><span data-stu-id="71695-102">In hello [Azure portal](https://portal.azure.com/), click **New** > **Internet of Things** > **IoT Hub**.</span></span>

   ![Vytvoření služby IoT hub v portálu Azure hello](../articles/iot-hub/media/iot-hub-create-hub-and-device/1_create-azure-iot-hub-portal.png)
2. <span data-ttu-id="71695-104">V hello **služby IoT hub** podokně zadejte následující informace pro službu IoT hub hello:</span><span class="sxs-lookup"><span data-stu-id="71695-104">In hello **IoT hub** pane, enter hello following information for your IoT hub:</span></span>

     <span data-ttu-id="71695-105">**Název**: Zadejte název hello služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="71695-105">**Name**: Enter hello name of your IoT hub.</span></span> <span data-ttu-id="71695-106">Pokud zadáte název hello je platný, se zobrazí zelená značka zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="71695-106">If hello name you enter is valid, a green check mark appears.</span></span>

     <span data-ttu-id="71695-107">**Úroveň ceny a škálování**: Vyberte hello **F1 - volné** vrstvy.</span><span class="sxs-lookup"><span data-stu-id="71695-107">**Pricing and scale tier**: Select hello **F1 - Free** tier.</span></span> <span data-ttu-id="71695-108">Tato možnost je pro tuto ukázku dostačující.</span><span class="sxs-lookup"><span data-stu-id="71695-108">This option is sufficient for this demo.</span></span> <span data-ttu-id="71695-109">Další informace najdete v tématu hello [cenovou a škálovací úroveň](https://azure.microsoft.com/pricing/details/iot-hub/).</span><span class="sxs-lookup"><span data-stu-id="71695-109">For more information, see hello [Pricing and scale tier](https://azure.microsoft.com/pricing/details/iot-hub/).</span></span>

     <span data-ttu-id="71695-110">**Skupina prostředků**: vytvoření IoT hub prostředků skupiny toohost hello nebo použijte existující.</span><span class="sxs-lookup"><span data-stu-id="71695-110">**Resource group**: Create a resource group toohost hello IoT hub or use an existing one.</span></span> <span data-ttu-id="71695-111">Další informace najdete v tématu [skupiny zdrojů použití toomanage vašich prostředků Azure](../articles/azure-resource-manager/resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="71695-111">For more information, see [Use resource groups toomanage your Azure resources](../articles/azure-resource-manager/resource-group-portal.md).</span></span>

     <span data-ttu-id="71695-112">**Umístění**: Vyberte hello nejbližší tooyou umístění, kde se má vytvořit hello IoT hub.</span><span class="sxs-lookup"><span data-stu-id="71695-112">**Location**: Select hello closest location tooyou where hello IoT hub is created.</span></span>

     <span data-ttu-id="71695-113">**PIN kód toodashboard**: tuto možnost vyberte pro Centrum IoT tooyour snadný přístup z řídicího panelu hello.</span><span class="sxs-lookup"><span data-stu-id="71695-113">**Pin toodashboard**: Select this option for easy access tooyour IoT hub from hello dashboard.</span></span>

   ![Zadejte informace o toocreate služby IoT hub](../articles/iot-hub/media/iot-hub-create-hub-and-device/2_fill-in-fields-for-azure-iot-hub-portal.png)

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

3. <span data-ttu-id="71695-115">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="71695-115">Click **Create**.</span></span> <span data-ttu-id="71695-116">Služby IoT hub může trvat několik minut toocreate.</span><span class="sxs-lookup"><span data-stu-id="71695-116">Your IoT hub might take a few minutes toocreate.</span></span> <span data-ttu-id="71695-117">Zobrazí se průběh hello **oznámení** podokně.</span><span class="sxs-lookup"><span data-stu-id="71695-117">You can see progress in hello **Notifications** pane.</span></span>

   ![Zobrazení oznámení o průběhu pro centrum IoT](../articles/iot-hub/media/iot-hub-create-hub-and-device/3_notification-azure-iot-hub-creation-progress-portal.png)

4. <span data-ttu-id="71695-119">Po vytvoření služby IoT hub na ni kliknete na řídicím panelu hello.</span><span class="sxs-lookup"><span data-stu-id="71695-119">After your IoT hub is created, click it on hello dashboard.</span></span> <span data-ttu-id="71695-120">Poznamenejte si hello **Hostname**a potom klikněte na **zásady sdíleného přístupu**.</span><span class="sxs-lookup"><span data-stu-id="71695-120">Make a note of hello **Hostname**, and then click **Shared access policies**.</span></span>

   ![Získat název hostitele hello služby IoT hub](../articles/iot-hub/media/iot-hub-create-hub-and-device/4_get-azure-iot-hub-hostname-portal.png)

5. <span data-ttu-id="71695-122">V hello **zásady sdíleného přístupu** podokně klikněte na tlačítko hello **iothubowner** zásady a pak kopírování a zaznamenání hello **připojovací řetězec** služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="71695-122">In hello **Shared access policies** pane, click hello **iothubowner** policy, and then copy and make a note of hello **Connection string** of your IoT hub.</span></span> <span data-ttu-id="71695-123">Další informace najdete v tématu [řízení přístupu tooIoT rozbočovače](../articles/iot-hub/iot-hub-devguide-security.md).</span><span class="sxs-lookup"><span data-stu-id="71695-123">For more information, see [Control access tooIoT Hub](../articles/iot-hub/iot-hub-devguide-security.md).</span></span>

> [!NOTE] 
<span data-ttu-id="71695-124">Pro tento kurz nastavení nebudete připojovací řetězec iothubowner potřebovat.</span><span class="sxs-lookup"><span data-stu-id="71695-124">You will not need this iothubowner connection string for this set-up tutorial.</span></span> <span data-ttu-id="71695-125">Ale musíte ho pro některé hello kurzy o různé scénáře IoT po dokončení tohoto nastavení.</span><span class="sxs-lookup"><span data-stu-id="71695-125">However, you may need it for some of hello tutorials on different IoT scenarios after you complete this set-up.</span></span>

   ![Získání připojovacího řetězce centra IoT](../articles/iot-hub/media/iot-hub-create-hub-and-device/5_get-azure-iot-hub-connection-string-portal.png)

## <a name="register-a-device-in-hello-iot-hub-for-your-device"></a><span data-ttu-id="71695-127">Registrovat zařízení ve hello IoT hub pro vaše zařízení</span><span class="sxs-lookup"><span data-stu-id="71695-127">Register a device in hello IoT hub for your device</span></span>

1. <span data-ttu-id="71695-128">V hello [portál Azure](https://portal.azure.com/), otevřete své služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="71695-128">In hello [Azure portal](https://portal.azure.com/), open your IoT hub.</span></span>

2. <span data-ttu-id="71695-129">Klikněte na **Průzkumník zařízení**.</span><span class="sxs-lookup"><span data-stu-id="71695-129">Click **Device Explorer**.</span></span>
3. <span data-ttu-id="71695-130">V podokně Průzkumník zařízení hello, klikněte na **přidat** tooadd zařízení tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="71695-130">In hello Device Explorer pane, click **Add** tooadd a device tooyour IoT hub.</span></span> <span data-ttu-id="71695-131">Potom hello následující:</span><span class="sxs-lookup"><span data-stu-id="71695-131">Then do hello following:</span></span>

   <span data-ttu-id="71695-132">**ID zařízení**: Zadejte ID hello hello nového zařízení.</span><span class="sxs-lookup"><span data-stu-id="71695-132">**Device ID**: Enter hello ID of hello new device.</span></span> <span data-ttu-id="71695-133">V ID zařízení se rozlišují malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="71695-133">Device IDs are case sensitive.</span></span>

   <span data-ttu-id="71695-134">**Typ ověřování:** Vyberte **Symetrický klíč**.</span><span class="sxs-lookup"><span data-stu-id="71695-134">**Authentication Type**: Select **Symmetric Key**.</span></span>

   <span data-ttu-id="71695-135">**Automaticky generovat klíče:** Zaškrtněte toto políčko.</span><span class="sxs-lookup"><span data-stu-id="71695-135">**Auto Generate Keys**: Select this check box.</span></span>

   <span data-ttu-id="71695-136">**Připojení zařízení tooIoT rozbočovače**: klikněte na tlačítko **povolit**.</span><span class="sxs-lookup"><span data-stu-id="71695-136">**Connect device tooIoT Hub**: Click **Enable**.</span></span>

   ![Přidání zařízení v hello Explorer zařízení služby IoT hub](../articles/iot-hub/media/iot-hub-create-hub-and-device/6_add-device-in-azure-iot-hub-device-explorer-portal.png)

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

4. <span data-ttu-id="71695-138">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="71695-138">Click **Save**.</span></span>
5. <span data-ttu-id="71695-139">Po vytvoření hello zařízení otevřete hello zařízení v hello **Explorer zařízení** podokně.</span><span class="sxs-lookup"><span data-stu-id="71695-139">After hello device is created, open hello device in hello **Device Explorer** pane.</span></span>
6. <span data-ttu-id="71695-140">Poznamenejte si primární klíč hello hello připojovacího řetězce.</span><span class="sxs-lookup"><span data-stu-id="71695-140">Make a note of hello primary key of hello connection string.</span></span>

   ![Získat hello zařízení připojovací řetězec](../articles/iot-hub/media/iot-hub-create-hub-and-device/7_get-device-connection-string-in-device-explorer-portal.png)
