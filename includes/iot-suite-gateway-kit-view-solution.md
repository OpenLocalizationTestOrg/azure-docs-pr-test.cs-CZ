## <a name="view-the-solution-dashboard"></a><span data-ttu-id="09658-101">Zobrazení řídicího panelu řešení</span><span class="sxs-lookup"><span data-stu-id="09658-101">View the solution dashboard</span></span>

<span data-ttu-id="09658-102">Přes řídicí panel řešení můžete spravovat nasazené řešení.</span><span class="sxs-lookup"><span data-stu-id="09658-102">The solution dashboard enables you to manage the deployed solution.</span></span> <span data-ttu-id="09658-103">Můžete například zobrazit telemetrická data, přidat zařízení a volat metody.</span><span class="sxs-lookup"><span data-stu-id="09658-103">For example, you can view telemetry, add devices, and invoke methods.</span></span>

1. <span data-ttu-id="09658-104">Až bude zřizování dokončeno a dlaždice předkonfigurovaného řešení bude hlásit **Připraveno**, zvolte **Spustit**. Na nové kartě se otevře portál předkonfigurovaného řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="09658-104">When the provisioning is complete and the tile for your preconfigured solution indicates **Ready**, choose **Launch** to open your remote monitoring solution portal in a new tab.</span></span>

    ![Spuštění předkonfigurovaného řešení][img-launch-solution]

1. <span data-ttu-id="09658-106">Ve výchozím nastavení se na portálu řešení zobrazuje *řídicí panel*.</span><span class="sxs-lookup"><span data-stu-id="09658-106">By default, the solution portal shows the *dashboard*.</span></span> <span data-ttu-id="09658-107">Přejděte do jiných oblastí portálu řešení pomocí nabídky na levé straně stránky.</span><span class="sxs-lookup"><span data-stu-id="09658-107">Navigate to other areas of the solution portal using the menu on the left-hand side of the page.</span></span>

    ![Řídicí panel předkonfigurovaného řešení vzdáleného monitorování][img-menu]

## <a name="add-a-device"></a><span data-ttu-id="09658-109">Přidání zařízení</span><span class="sxs-lookup"><span data-stu-id="09658-109">Add a device</span></span>

<span data-ttu-id="09658-110">Aby se zařízení mohlo připojit k předkonfigurovanému řešení, musí se identifikovat ve službě IoT Hub pomocí platných přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="09658-110">For a device to connect to the preconfigured solution, it must identify itself to IoT Hub using valid credentials.</span></span> <span data-ttu-id="09658-111">Přihlašovací údaje zařízení můžete zjistit z řídicího panelu řešení.</span><span class="sxs-lookup"><span data-stu-id="09658-111">You can retrieve the device credentials from the solution dashboard.</span></span> <span data-ttu-id="09658-112">Přihlašovací údaje zařízení vložíte do klientské aplikace později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="09658-112">You include the device credentials in your client application later in this tutorial.</span></span>

<span data-ttu-id="09658-113">Pokud chcete přidat zařízení do řešení vzdáleného monitorování, proveďte na řídicím panelu řešení následující kroky:</span><span class="sxs-lookup"><span data-stu-id="09658-113">To add a device to your remote monitoring solution, complete the following steps in the solution dashboard:</span></span>

1. <span data-ttu-id="09658-114">V levém dolním rohu řídicího panelu klikněte na **Přidat zařízení**.</span><span class="sxs-lookup"><span data-stu-id="09658-114">In the lower left-hand corner of the dashboard, click **Add a device**.</span></span>

   ![Přidání zařízení][1]

1. <span data-ttu-id="09658-116">Na panelu **Vlastní zařízení** klikněte na **Přidat nové**.</span><span class="sxs-lookup"><span data-stu-id="09658-116">In the **Custom Device** panel, click **Add new**.</span></span>

   ![Přidání vlastního zařízení][2]

1. <span data-ttu-id="09658-118">Vyberte možnost **Definovat vlastní ID zařízení**.</span><span class="sxs-lookup"><span data-stu-id="09658-118">Choose **Let me define my own Device ID**.</span></span> <span data-ttu-id="09658-119">Zadejte ID zařízení, jako **device01**, klikněte na tlačítko **Zkontrolujte ID** ověření již nepoužili název ve vašem řešení a potom klikněte na **vytvořit** ke zřízení zařízení.</span><span class="sxs-lookup"><span data-stu-id="09658-119">Enter a Device ID such as **device01**, click **Check ID** to verify you haven't already used the name in your solution, and then click **Create** to provision the device.</span></span>

   ![Přidání ID zařízení][3]

1. <span data-ttu-id="09658-121">Poznamenejte si přihlašovací údaje zařízení (**ID zařízení**, **název hostitele centra IoT**, a **klíč zařízení**).</span><span class="sxs-lookup"><span data-stu-id="09658-121">Make a note the device credentials (**Device ID**, **IoT Hub Hostname**, and **Device Key**).</span></span> <span data-ttu-id="09658-122">Klientské aplikace na Intel NUC musí tyto hodnoty pro připojení k řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="09658-122">Your client application on the Intel NUC needs these values to connect to the remote monitoring solution.</span></span> <span data-ttu-id="09658-123">Potom klikněte na **Done** (Hotovo).</span><span class="sxs-lookup"><span data-stu-id="09658-123">Then click **Done**.</span></span>

    ![Zobrazení přihlašovacích údajů zařízení][4]

1. <span data-ttu-id="09658-125">V seznamu zařízení na řídicím panelu řešení vyberte své zařízení.</span><span class="sxs-lookup"><span data-stu-id="09658-125">Select your device in the device list in the solution dashboard.</span></span> <span data-ttu-id="09658-126">Pak na panelu **Podrobnosti o zařízení** klikněte na **Povolit zařízení**.</span><span class="sxs-lookup"><span data-stu-id="09658-126">Then, in the **Device Details** panel, click **Enable Device**.</span></span> <span data-ttu-id="09658-127">Stav vašeho zařízení je teď **Spuštěno**.</span><span class="sxs-lookup"><span data-stu-id="09658-127">The status of your device is now **Running**.</span></span> <span data-ttu-id="09658-128">Řešení vzdáleného monitorování teď může z vašeho zařízení přijímat telemetrii a vyvolávat v něm metody.</span><span class="sxs-lookup"><span data-stu-id="09658-128">The remote monitoring solution can now receive telemetry from your device and invoke methods on the device.</span></span>

[img-launch-solution]: media/iot-suite-gateway-kit-view-solution/launch.png
[img-menu]: media/iot-suite-gateway-kit-view-solution/menu.png
[1]: media/iot-suite-gateway-kit-view-solution/suite0.png
[2]: media/iot-suite-gateway-kit-view-solution/suite1.png
[3]: media/iot-suite-gateway-kit-view-solution/suite2.png
[4]: media/iot-suite-gateway-kit-view-solution/suite3.png