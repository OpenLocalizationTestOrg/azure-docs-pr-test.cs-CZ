## <a name="view-hello-solution-dashboard"></a><span data-ttu-id="1ece5-101">Zobrazení řídicí panel řešení hello</span><span class="sxs-lookup"><span data-stu-id="1ece5-101">View hello solution dashboard</span></span>

<span data-ttu-id="1ece5-102">řídicí panel řešení Hello umožňuje toomanage hello nasazené řešení.</span><span class="sxs-lookup"><span data-stu-id="1ece5-102">hello solution dashboard enables you toomanage hello deployed solution.</span></span> <span data-ttu-id="1ece5-103">Můžete například zobrazit telemetrická data, přidat zařízení a volat metody.</span><span class="sxs-lookup"><span data-stu-id="1ece5-103">For example, you can view telemetry, add devices, and invoke methods.</span></span>

1. <span data-ttu-id="1ece5-104">Když je hello zřizování dokončeno a dlaždice hello pro předkonfigurované řešení označuje **připravené**, zvolte **spusťte** tooopen vzdálené monitorování řešení portálu na nové kartě.</span><span class="sxs-lookup"><span data-stu-id="1ece5-104">When hello provisioning is complete and hello tile for your preconfigured solution indicates **Ready**, choose **Launch** tooopen your remote monitoring solution portal in a new tab.</span></span>

    ![Spusťte hello předkonfigurované řešení][img-launch-solution]

1. <span data-ttu-id="1ece5-106">Ve výchozím nastavení, hello portálu řešení zobrazuje hello *řídicí panel*.</span><span class="sxs-lookup"><span data-stu-id="1ece5-106">By default, hello solution portal shows hello *dashboard*.</span></span> <span data-ttu-id="1ece5-107">Můžete procházet tooother oblasti hello portál řešení pomocí hello nabídky na levé straně stránky hello hello.</span><span class="sxs-lookup"><span data-stu-id="1ece5-107">You can navigate tooother areas of hello solution portal using hello menu on hello left-hand side of hello page.</span></span>

    ![Řídicí panel předkonfigurovaného řešení vzdáleného monitorování][img-menu]

## <a name="add-a-device"></a><span data-ttu-id="1ece5-109">Přidání zařízení</span><span class="sxs-lookup"><span data-stu-id="1ece5-109">Add a device</span></span>

<span data-ttu-id="1ece5-110">Pro zařízení tooconnect toohello předkonfigurované řešení, se musí identifikovat tooIoT centra pomocí platných přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="1ece5-110">For a device tooconnect toohello preconfigured solution, it must identify itself tooIoT Hub using valid credentials.</span></span> <span data-ttu-id="1ece5-111">Přihlašovací údaje hello zařízení můžete načíst z řídicí panel řešení hello.</span><span class="sxs-lookup"><span data-stu-id="1ece5-111">You can retrieve hello device credentials from hello solution dashboard.</span></span> <span data-ttu-id="1ece5-112">Přihlašovací údaje zařízení hello zahrnete do klientské aplikace později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="1ece5-112">You include hello device credentials in your client application later in this tutorial.</span></span>

<span data-ttu-id="1ece5-113">Pokud jste tak již neučinili, přidejte vlastní zařízení tooyour řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="1ece5-113">If you haven't already done so, add a custom device tooyour remote monitoring solution.</span></span> <span data-ttu-id="1ece5-114">Proveďte následující kroky v řídicí panel řešení hello hello:</span><span class="sxs-lookup"><span data-stu-id="1ece5-114">Complete hello following steps in hello solution dashboard:</span></span>

1. <span data-ttu-id="1ece5-115">V hello levém dolním rohu hello řídicí panel, klikněte na **přidání zařízení**.</span><span class="sxs-lookup"><span data-stu-id="1ece5-115">In hello lower left-hand corner of hello dashboard, click **Add a device**.</span></span>

   ![Přidání zařízení][1]

1. <span data-ttu-id="1ece5-117">V hello **vlastní zařízení** panelu, klikněte na tlačítko **přidat nový**.</span><span class="sxs-lookup"><span data-stu-id="1ece5-117">In hello **Custom Device** panel, click **Add new**.</span></span>

   ![Přidání vlastního zařízení][2]

1. <span data-ttu-id="1ece5-119">Vyberte možnost **Definovat vlastní ID zařízení**.</span><span class="sxs-lookup"><span data-stu-id="1ece5-119">Choose **Let me define my own Device ID**.</span></span> <span data-ttu-id="1ece5-120">Zadejte ID zařízení, jako **rasppi**, klikněte na tlačítko **Zkontrolujte ID** tooverify již nepoužili hello název ve vašem řešení a potom klikněte na **vytvořit** tooprovision hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="1ece5-120">Enter a Device ID such as **rasppi**, click **Check ID** tooverify you haven't already used hello name in your solution, and then click **Create** tooprovision hello device.</span></span>

   ![Přidání ID zařízení][3]

1. <span data-ttu-id="1ece5-122">Zkontrolujte zařízení hello Poznámka: přihlašovací údaje (**ID zařízení**, **název hostitele centra IoT**, a **klíč zařízení**).</span><span class="sxs-lookup"><span data-stu-id="1ece5-122">Make a note hello device credentials (**Device ID**, **IoT Hub Hostname**, and **Device Key**).</span></span> <span data-ttu-id="1ece5-123">Klientské aplikace na hello malin platformy musí tyto hodnoty tooconnect toohello řešení vzdáleného sledování.</span><span class="sxs-lookup"><span data-stu-id="1ece5-123">Your client application on hello Raspberry Pi needs these values tooconnect toohello remote monitoring solution.</span></span> <span data-ttu-id="1ece5-124">Potom klikněte na **Done** (Hotovo).</span><span class="sxs-lookup"><span data-stu-id="1ece5-124">Then click **Done**.</span></span>

    ![Zobrazení přihlašovacích údajů zařízení][4]

1. <span data-ttu-id="1ece5-126">Vyberte zařízení v seznamu zařízení hello v řídicí panel řešení hello.</span><span class="sxs-lookup"><span data-stu-id="1ece5-126">Select your device in hello device list in hello solution dashboard.</span></span> <span data-ttu-id="1ece5-127">Potom v hello **podrobnosti o zařízení** panelu, klikněte na tlačítko **povolit zařízení**.</span><span class="sxs-lookup"><span data-stu-id="1ece5-127">Then, in hello **Device Details** panel, click **Enable Device**.</span></span> <span data-ttu-id="1ece5-128">Hello stav zařízení je nyní **systémem**.</span><span class="sxs-lookup"><span data-stu-id="1ece5-128">hello status of your device is now **Running**.</span></span> <span data-ttu-id="1ece5-129">řešení vzdáleného monitorování Hello teď můžete přijímat telemetrická data ze zařízení a volat metody na hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="1ece5-129">hello remote monitoring solution can now receive telemetry from your device and invoke methods on hello device.</span></span>

[img-launch-solution]: media/iot-suite-raspberry-pi-kit-view-solution/launch.png
[img-menu]: media/iot-suite-raspberry-pi-kit-view-solution/menu.png
[1]: media/iot-suite-raspberry-pi-kit-view-solution/suite0.png
[2]: media/iot-suite-raspberry-pi-kit-view-solution/suite1.png
[3]: media/iot-suite-raspberry-pi-kit-view-solution/suite2.png
[4]: media/iot-suite-raspberry-pi-kit-view-solution/suite3.png