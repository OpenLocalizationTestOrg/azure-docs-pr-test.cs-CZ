> [!div class="op_single_selector"]
> * [<span data-ttu-id="c39cf-101">C ve Windows</span><span class="sxs-lookup"><span data-stu-id="c39cf-101">C on Windows</span></span>](../articles/iot-suite/iot-suite-connecting-devices.md)
> * [<span data-ttu-id="c39cf-102">C v Linuxu</span><span class="sxs-lookup"><span data-stu-id="c39cf-102">C on Linux</span></span>](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
> * [<span data-ttu-id="c39cf-103">Node.js</span><span class="sxs-lookup"><span data-stu-id="c39cf-103">Node.js</span></span>](../articles/iot-suite/iot-suite-connecting-devices-node.md)
> 
> 

## <a name="scenario-overview"></a><span data-ttu-id="c39cf-104">Přehled scénáře</span><span class="sxs-lookup"><span data-stu-id="c39cf-104">Scenario overview</span></span>
<span data-ttu-id="c39cf-105">V tomto scénáři vytvoříte zařízení, které odesílá následující telemetrii do [předkonfigurovaného řešení][lnk-what-are-preconfig-solutions] vzdáleného monitorování:</span><span class="sxs-lookup"><span data-stu-id="c39cf-105">In this scenario, you create a device that sends the following telemetry to the remote monitoring [preconfigured solution][lnk-what-are-preconfig-solutions]:</span></span>

* <span data-ttu-id="c39cf-106">Venkovní teplota</span><span class="sxs-lookup"><span data-stu-id="c39cf-106">External temperature</span></span>
* <span data-ttu-id="c39cf-107">Vnitřní teplota</span><span class="sxs-lookup"><span data-stu-id="c39cf-107">Internal temperature</span></span>
* <span data-ttu-id="c39cf-108">Vlhkost</span><span class="sxs-lookup"><span data-stu-id="c39cf-108">Humidity</span></span>

<span data-ttu-id="c39cf-109">Kód v zařízení pro zjednodušení generuje ukázkové hodnoty, ale doporučujeme vám ukázku rozšířit připojením skutečných senzorů k zařízení a odesíláním skutečné telemetrie.</span><span class="sxs-lookup"><span data-stu-id="c39cf-109">For simplicity, the code on the device generates sample values, but we encourage you to extend the sample by connecting real sensors to your device and sending real telemetry.</span></span>

<span data-ttu-id="c39cf-110">Zařízení je také schopné odpovídat na metody vyvolané z řídicího panelu řešení a na požadované hodnoty vlastností nastavené na řídicím panelu řešení.</span><span class="sxs-lookup"><span data-stu-id="c39cf-110">The device is also able to respond to methods invoked from the solution dashboard and desired property values set in the solution dashboard.</span></span>

<span data-ttu-id="c39cf-111">K dokončení tohoto kurzu potřebujete mít aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="c39cf-111">To complete this tutorial, you need an active Azure account.</span></span> <span data-ttu-id="c39cf-112">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="c39cf-112">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="c39cf-113">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="c39cf-113">For details, see [Azure Free Trial][lnk-free-trial].</span></span>

## <a name="before-you-start"></a><span data-ttu-id="c39cf-114">Než začnete</span><span class="sxs-lookup"><span data-stu-id="c39cf-114">Before you start</span></span>
<span data-ttu-id="c39cf-115">Než začnete psát kód pro zařízení, je nutné zřídit předkonfigurované řešení vzdáleného monitorování a v něm zřídit nové vlastní zařízení.</span><span class="sxs-lookup"><span data-stu-id="c39cf-115">Before you write any code for your device, you must provision your remote monitoring preconfigured solution and provision a new custom device in that solution.</span></span>

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="c39cf-116">Zřízení předkonfigurovaného řešení vzdáleného monitorování</span><span class="sxs-lookup"><span data-stu-id="c39cf-116">Provision your remote monitoring preconfigured solution</span></span>
<span data-ttu-id="c39cf-117">Zařízení, které v tomto kurzu vytvoříte, odesílá data do instance předkonfigurovaného řešení [vzdáleného monitorování][lnk-remote-monitoring].</span><span class="sxs-lookup"><span data-stu-id="c39cf-117">The device you create in this tutorial sends data to an instance of the [remote monitoring][lnk-remote-monitoring] preconfigured solution.</span></span> <span data-ttu-id="c39cf-118">Pokud jste ve svém účtu Azure ještě nezřídili předkonfigurované řešení vzdáleného monitorování, použijte následující postup:</span><span class="sxs-lookup"><span data-stu-id="c39cf-118">If you haven't already provisioned the remote monitoring preconfigured solution in your Azure account, use the following steps:</span></span>

1. <span data-ttu-id="c39cf-119">Na stránce <https://www.azureiotsuite.com/> můžete vytvořit řešení kliknutím na **+**.</span><span class="sxs-lookup"><span data-stu-id="c39cf-119">On the <https://www.azureiotsuite.com/> page, click **+** to create a solution.</span></span>
2. <span data-ttu-id="c39cf-120">Kliknutím na **Vybrat** na panelu **Vzdálené monitorování** vytvořte řešení.</span><span class="sxs-lookup"><span data-stu-id="c39cf-120">Click **Select** on the **Remote monitoring** panel to create your solution.</span></span>
3. <span data-ttu-id="c39cf-121">Na stránce **Vytvořit řešení vzdáleného monitorování** zadejte **Název řešení** podle vašeho výběru, vyberte **Oblast**, do které chcete řešení nasadit, a vyberte předplatné Azure, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="c39cf-121">On the **Create Remote monitoring solution** page, enter a **Solution name** of your choice, select the **Region** you want to deploy to, and select the Azure subscription to want to use.</span></span> <span data-ttu-id="c39cf-122">Potom klikněte na **Vytvořit řešení**.</span><span class="sxs-lookup"><span data-stu-id="c39cf-122">Then click **Create solution**.</span></span>
4. <span data-ttu-id="c39cf-123">Počkejte, dokud proces zřizování neskončí.</span><span class="sxs-lookup"><span data-stu-id="c39cf-123">Wait until the provisioning process completes.</span></span>

> [!WARNING]
> <span data-ttu-id="c39cf-124">Předkonfigurovaná řešení využívají fakturovatelné služby Azure.</span><span class="sxs-lookup"><span data-stu-id="c39cf-124">The preconfigured solutions use billable Azure services.</span></span> <span data-ttu-id="c39cf-125">Abyste se vyhnuli zbytečným poplatkům, nezapomeňte předkonfigurované řešení odebrat ze svého předplatného, jakmile s ním budete hotovi.</span><span class="sxs-lookup"><span data-stu-id="c39cf-125">Be sure to remove the preconfigured solution from your subscription when you are done with it to avoid any unnecessary charges.</span></span> <span data-ttu-id="c39cf-126">Předkonfigurované řešení můžete ze svého předplatného úplně odebrat na stránce <https://www.azureiotsuite.com/>.</span><span class="sxs-lookup"><span data-stu-id="c39cf-126">You can completely remove a preconfigured solution from your subscription by visiting the <https://www.azureiotsuite.com/> page.</span></span>
> 
> 

<span data-ttu-id="c39cf-127">Po skončení procesu zřizování řešení vzdáleného monitorování klikněte na **Spustit**. Ve vašem prohlížeči se otevře řídicí panel řešení.</span><span class="sxs-lookup"><span data-stu-id="c39cf-127">When the provisioning process for the remote monitoring solution finishes, click **Launch** to open the solution dashboard in your browser.</span></span>

![Řídicí panel řešení][img-dashboard]

### <a name="provision-your-device-in-the-remote-monitoring-solution"></a><span data-ttu-id="c39cf-129">Zřízení zařízení v řešení vzdáleného monitorování</span><span class="sxs-lookup"><span data-stu-id="c39cf-129">Provision your device in the remote monitoring solution</span></span>
> [!NOTE]
> <span data-ttu-id="c39cf-130">Pokud jste už ve svém řešení zařízení zřídili, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="c39cf-130">If you have already provisioned a device in your solution, you can skip this step.</span></span> <span data-ttu-id="c39cf-131">Při vytváření klientské aplikace potřebujete znát přihlašovací údaje zařízení.</span><span class="sxs-lookup"><span data-stu-id="c39cf-131">You need to know the device credentials when you create the client application.</span></span>
> 
> 

<span data-ttu-id="c39cf-132">Aby se zařízení mohlo připojit k předkonfigurovanému řešení, musí se identifikovat ve službě IoT Hub pomocí platných přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="c39cf-132">For a device to connect to the preconfigured solution, it must identify itself to IoT Hub using valid credentials.</span></span> <span data-ttu-id="c39cf-133">Přihlašovací údaje zařízení můžete zjistit z řídicího panelu řešení.</span><span class="sxs-lookup"><span data-stu-id="c39cf-133">You can retrieve the device credentials from the solution dashboard.</span></span> <span data-ttu-id="c39cf-134">Přihlašovací údaje zařízení vložíte do klientské aplikace později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="c39cf-134">You include the device credentials in your client application later in this tutorial.</span></span>

<span data-ttu-id="c39cf-135">Pokud chcete přidat zařízení do řešení vzdáleného monitorování, proveďte na řídicím panelu řešení následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c39cf-135">To add a device to your remote monitoring solution, complete the following steps in the solution dashboard:</span></span>

1. <span data-ttu-id="c39cf-136">V levém dolním rohu řídicího panelu klikněte na **Přidat zařízení**.</span><span class="sxs-lookup"><span data-stu-id="c39cf-136">In the lower left-hand corner of the dashboard, click **Add a device**.</span></span>
   
   ![Přidání zařízení][1]
2. <span data-ttu-id="c39cf-138">Na panelu **Vlastní zařízení** klikněte na **Přidat nové**.</span><span class="sxs-lookup"><span data-stu-id="c39cf-138">In the **Custom Device** panel, click **Add new**.</span></span>
   
   ![Přidání vlastního zařízení][2]
3. <span data-ttu-id="c39cf-140">Vyberte možnost **Definovat vlastní ID zařízení**.</span><span class="sxs-lookup"><span data-stu-id="c39cf-140">Choose **Let me define my own Device ID**.</span></span> <span data-ttu-id="c39cf-141">Zadejte ID zařízení, třeba **mydevice**, klikněte na **Zkontrolovat ID** pro ověření, že se tento název ještě nepoužívá, a potom zřiďte zařízení kliknutím na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c39cf-141">Enter a Device ID such as **mydevice**, click **Check ID** to verify that name isn't already in use, and then click **Create** to provision the device.</span></span>
   
   ![Přidání ID zařízení][3]
4. <span data-ttu-id="c39cf-143">Poznamenejte si přihlašovací údaje zařízení (ID zařízení, název hostitele služby IoT Hub a Klíč zařízení).</span><span class="sxs-lookup"><span data-stu-id="c39cf-143">Make a note the device credentials (Device ID, IoT Hub Hostname, and Device Key).</span></span> <span data-ttu-id="c39cf-144">Klientská aplikace potřebuje tyto hodnoty pro připojení k řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="c39cf-144">Your client application needs these values to connect to the remote monitoring solution.</span></span> <span data-ttu-id="c39cf-145">Potom klikněte na **Done** (Hotovo).</span><span class="sxs-lookup"><span data-stu-id="c39cf-145">Then click **Done**.</span></span>
   
    ![Zobrazení přihlašovacích údajů zařízení][4]
5. <span data-ttu-id="c39cf-147">V seznamu zařízení na řídicím panelu řešení vyberte své zařízení.</span><span class="sxs-lookup"><span data-stu-id="c39cf-147">Select your device in the device list in the solution dashboard.</span></span> <span data-ttu-id="c39cf-148">Pak na panelu **Podrobnosti o zařízení** klikněte na **Povolit zařízení**.</span><span class="sxs-lookup"><span data-stu-id="c39cf-148">Then, in the **Device Details** panel, click **Enable Device**.</span></span> <span data-ttu-id="c39cf-149">Stav vašeho zařízení je teď **Spuštěno**.</span><span class="sxs-lookup"><span data-stu-id="c39cf-149">The status of your device is now **Running**.</span></span> <span data-ttu-id="c39cf-150">Řešení vzdáleného monitorování teď může z vašeho zařízení přijímat telemetrii a vyvolávat v něm metody.</span><span class="sxs-lookup"><span data-stu-id="c39cf-150">The remote monitoring solution can now receive telemetry from your device and invoke methods on the device.</span></span>

[img-dashboard]: ./media/iot-suite-selector-connecting/dashboard.png
[1]: ./media/iot-suite-selector-connecting/suite0.png
[2]: ./media/iot-suite-selector-connecting/suite1.png
[3]: ./media/iot-suite-selector-connecting/suite2.png
[4]: ./media/iot-suite-selector-connecting/suite3.png

[lnk-what-are-preconfig-solutions]: ../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: ../articles/iot-suite/iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/