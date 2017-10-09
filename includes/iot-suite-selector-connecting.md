> [!div class="op_single_selector"]
> * [<span data-ttu-id="d36a3-101">C ve Windows</span><span class="sxs-lookup"><span data-stu-id="d36a3-101">C on Windows</span></span>](../articles/iot-suite/iot-suite-connecting-devices.md)
> * [<span data-ttu-id="d36a3-102">C v Linuxu</span><span class="sxs-lookup"><span data-stu-id="d36a3-102">C on Linux</span></span>](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
> * [<span data-ttu-id="d36a3-103">Node.js</span><span class="sxs-lookup"><span data-stu-id="d36a3-103">Node.js</span></span>](../articles/iot-suite/iot-suite-connecting-devices-node.md)
> 
> 

## <a name="scenario-overview"></a><span data-ttu-id="d36a3-104">Přehled scénáře</span><span class="sxs-lookup"><span data-stu-id="d36a3-104">Scenario overview</span></span>
<span data-ttu-id="d36a3-105">V tomto scénáři vytvoříte zařízení, které odesílá hello následující telemetrie toohello vzdálené monitorování [předkonfigurované řešení][lnk-what-are-preconfig-solutions]:</span><span class="sxs-lookup"><span data-stu-id="d36a3-105">In this scenario, you create a device that sends hello following telemetry toohello remote monitoring [preconfigured solution][lnk-what-are-preconfig-solutions]:</span></span>

* <span data-ttu-id="d36a3-106">Venkovní teplota</span><span class="sxs-lookup"><span data-stu-id="d36a3-106">External temperature</span></span>
* <span data-ttu-id="d36a3-107">Vnitřní teplota</span><span class="sxs-lookup"><span data-stu-id="d36a3-107">Internal temperature</span></span>
* <span data-ttu-id="d36a3-108">Vlhkost</span><span class="sxs-lookup"><span data-stu-id="d36a3-108">Humidity</span></span>

<span data-ttu-id="d36a3-109">Pro jednoduchost hello kódu na zařízení hello generuje ukázkové hodnoty, ale doporučujeme vám tooextend hello ukázka připojením zařízení tooyour skutečné senzory a odesláním skutečné telemetrie.</span><span class="sxs-lookup"><span data-stu-id="d36a3-109">For simplicity, hello code on hello device generates sample values, but we encourage you tooextend hello sample by connecting real sensors tooyour device and sending real telemetry.</span></span>

<span data-ttu-id="d36a3-110">Hello zařízení je také možné toorespond toomethods volat z řídicí panel řešení hello a potřeby vlastnost hodnotami nastavenými v řídicí panel řešení hello.</span><span class="sxs-lookup"><span data-stu-id="d36a3-110">hello device is also able toorespond toomethods invoked from hello solution dashboard and desired property values set in hello solution dashboard.</span></span>

<span data-ttu-id="d36a3-111">toocomplete tohoto kurzu potřebujete aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="d36a3-111">toocomplete this tutorial, you need an active Azure account.</span></span> <span data-ttu-id="d36a3-112">Pokud účet nemáte, můžete si během několika minut vytvořit bezplatný zkušební účet.</span><span class="sxs-lookup"><span data-stu-id="d36a3-112">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="d36a3-113">Podrobnosti najdete v článku [Bezplatná zkušební verze Azure][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="d36a3-113">For details, see [Azure Free Trial][lnk-free-trial].</span></span>

## <a name="before-you-start"></a><span data-ttu-id="d36a3-114">Než začnete</span><span class="sxs-lookup"><span data-stu-id="d36a3-114">Before you start</span></span>
<span data-ttu-id="d36a3-115">Než začnete psát kód pro zařízení, je nutné zřídit předkonfigurované řešení vzdáleného monitorování a v něm zřídit nové vlastní zařízení.</span><span class="sxs-lookup"><span data-stu-id="d36a3-115">Before you write any code for your device, you must provision your remote monitoring preconfigured solution and provision a new custom device in that solution.</span></span>

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a><span data-ttu-id="d36a3-116">Zřízení předkonfigurovaného řešení vzdáleného monitorování</span><span class="sxs-lookup"><span data-stu-id="d36a3-116">Provision your remote monitoring preconfigured solution</span></span>
<span data-ttu-id="d36a3-117">Hello zařízení v tomto kurzu vytvoříte odesílá data tooan instanci hello [vzdálené monitorování] [ lnk-remote-monitoring] předkonfigurované řešení.</span><span class="sxs-lookup"><span data-stu-id="d36a3-117">hello device you create in this tutorial sends data tooan instance of hello [remote monitoring][lnk-remote-monitoring] preconfigured solution.</span></span> <span data-ttu-id="d36a3-118">Pokud jste ještě nezřídili hello vzdálené monitorování předkonfigurované řešení v účtu Azure, použijte hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d36a3-118">If you haven't already provisioned hello remote monitoring preconfigured solution in your Azure account, use hello following steps:</span></span>

1. <span data-ttu-id="d36a3-119">Na hello <https://www.azureiotsuite.com/> klikněte na tlačítko  **+**  toocreate řešení.</span><span class="sxs-lookup"><span data-stu-id="d36a3-119">On hello <https://www.azureiotsuite.com/> page, click **+** toocreate a solution.</span></span>
2. <span data-ttu-id="d36a3-120">Klikněte na tlačítko **vyberte** na hello **vzdálené monitorování** panelu toocreate řešení.</span><span class="sxs-lookup"><span data-stu-id="d36a3-120">Click **Select** on hello **Remote monitoring** panel toocreate your solution.</span></span>
3. <span data-ttu-id="d36a3-121">Na hello **vytvořit řešení vzdáleného sledování** stránky, zadejte **název řešení** podle vaší volby, vyberte hello **oblast** toodeploy do mají a vyberte hello Azure toouse toowant předplatné.</span><span class="sxs-lookup"><span data-stu-id="d36a3-121">On hello **Create Remote monitoring solution** page, enter a **Solution name** of your choice, select hello **Region** you want toodeploy to, and select hello Azure subscription toowant toouse.</span></span> <span data-ttu-id="d36a3-122">Potom klikněte na **Vytvořit řešení**.</span><span class="sxs-lookup"><span data-stu-id="d36a3-122">Then click **Create solution**.</span></span>
4. <span data-ttu-id="d36a3-123">Počkejte na dokončení procesu zřizování hello.</span><span class="sxs-lookup"><span data-stu-id="d36a3-123">Wait until hello provisioning process completes.</span></span>

> [!WARNING]
> <span data-ttu-id="d36a3-124">Hello předkonfigurované řešení využívají fakturovatelný služby Azure.</span><span class="sxs-lookup"><span data-stu-id="d36a3-124">hello preconfigured solutions use billable Azure services.</span></span> <span data-ttu-id="d36a3-125">Ujistěte se, tooremove hello předkonfigurované řešení ze svého předplatného, když jste hotovi s ním tooavoid všechny nepotřebné poplatky.</span><span class="sxs-lookup"><span data-stu-id="d36a3-125">Be sure tooremove hello preconfigured solution from your subscription when you are done with it tooavoid any unnecessary charges.</span></span> <span data-ttu-id="d36a3-126">Předkonfigurované řešení můžete úplně odebrat ze svého předplatného návštěvou hello <https://www.azureiotsuite.com/> stránky.</span><span class="sxs-lookup"><span data-stu-id="d36a3-126">You can completely remove a preconfigured solution from your subscription by visiting hello <https://www.azureiotsuite.com/> page.</span></span>
> 
> 

<span data-ttu-id="d36a3-127">Po dokončení hello zřizování pro hello řešení vzdáleného sledování, klikněte na tlačítko **spusťte** řídicí panel řešení hello tooopen v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="d36a3-127">When hello provisioning process for hello remote monitoring solution finishes, click **Launch** tooopen hello solution dashboard in your browser.</span></span>

![Řídicí panel řešení][img-dashboard]

### <a name="provision-your-device-in-hello-remote-monitoring-solution"></a><span data-ttu-id="d36a3-129">Zřízení zařízení v řešení vzdáleného sledování hello</span><span class="sxs-lookup"><span data-stu-id="d36a3-129">Provision your device in hello remote monitoring solution</span></span>
> [!NOTE]
> <span data-ttu-id="d36a3-130">Pokud jste už ve svém řešení zařízení zřídili, můžete tento krok přeskočit.</span><span class="sxs-lookup"><span data-stu-id="d36a3-130">If you have already provisioned a device in your solution, you can skip this step.</span></span> <span data-ttu-id="d36a3-131">Přihlašovací údaje tooknow hello zařízení musíte při vytváření hello klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="d36a3-131">You need tooknow hello device credentials when you create hello client application.</span></span>
> 
> 

<span data-ttu-id="d36a3-132">Pro zařízení tooconnect toohello předkonfigurované řešení, se musí identifikovat tooIoT centra pomocí platných přihlašovacích údajů.</span><span class="sxs-lookup"><span data-stu-id="d36a3-132">For a device tooconnect toohello preconfigured solution, it must identify itself tooIoT Hub using valid credentials.</span></span> <span data-ttu-id="d36a3-133">Přihlašovací údaje hello zařízení můžete načíst z řídicí panel řešení hello.</span><span class="sxs-lookup"><span data-stu-id="d36a3-133">You can retrieve hello device credentials from hello solution dashboard.</span></span> <span data-ttu-id="d36a3-134">Přihlašovací údaje zařízení hello zahrnete do klientské aplikace později v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="d36a3-134">You include hello device credentials in your client application later in this tutorial.</span></span>

<span data-ttu-id="d36a3-135">tooadd zařízení tooyour řešení vzdáleného monitorování, dokončení hello následující kroky v řídicí panel řešení hello:</span><span class="sxs-lookup"><span data-stu-id="d36a3-135">tooadd a device tooyour remote monitoring solution, complete hello following steps in hello solution dashboard:</span></span>

1. <span data-ttu-id="d36a3-136">V hello levém dolním rohu hello řídicí panel, klikněte na **přidání zařízení**.</span><span class="sxs-lookup"><span data-stu-id="d36a3-136">In hello lower left-hand corner of hello dashboard, click **Add a device**.</span></span>
   
   ![Přidání zařízení][1]
2. <span data-ttu-id="d36a3-138">V hello **vlastní zařízení** panelu, klikněte na tlačítko **přidat nový**.</span><span class="sxs-lookup"><span data-stu-id="d36a3-138">In hello **Custom Device** panel, click **Add new**.</span></span>
   
   ![Přidání vlastního zařízení][2]
3. <span data-ttu-id="d36a3-140">Vyberte možnost **Definovat vlastní ID zařízení**.</span><span class="sxs-lookup"><span data-stu-id="d36a3-140">Choose **Let me define my own Device ID**.</span></span> <span data-ttu-id="d36a3-141">Zadejte ID zařízení, jako **mydevice**, klikněte na tlačítko **Zkontrolujte ID** tooverify tento název již není používána a pak klikněte na tlačítko **vytvořit** tooprovision hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="d36a3-141">Enter a Device ID such as **mydevice**, click **Check ID** tooverify that name isn't already in use, and then click **Create** tooprovision hello device.</span></span>
   
   ![Přidání ID zařízení][3]
4. <span data-ttu-id="d36a3-143">Nastavit hello zařízení Poznámka: přihlašovací údaje (ID zařízení, název hostitele centra IoT a klíč zařízení).</span><span class="sxs-lookup"><span data-stu-id="d36a3-143">Make a note hello device credentials (Device ID, IoT Hub Hostname, and Device Key).</span></span> <span data-ttu-id="d36a3-144">Klientské aplikace, musí tyto hodnoty tooconnect toohello řešení vzdáleného sledování.</span><span class="sxs-lookup"><span data-stu-id="d36a3-144">Your client application needs these values tooconnect toohello remote monitoring solution.</span></span> <span data-ttu-id="d36a3-145">Potom klikněte na **Done** (Hotovo).</span><span class="sxs-lookup"><span data-stu-id="d36a3-145">Then click **Done**.</span></span>
   
    ![Zobrazení přihlašovacích údajů zařízení][4]
5. <span data-ttu-id="d36a3-147">Vyberte zařízení v seznamu zařízení hello v řídicí panel řešení hello.</span><span class="sxs-lookup"><span data-stu-id="d36a3-147">Select your device in hello device list in hello solution dashboard.</span></span> <span data-ttu-id="d36a3-148">Potom v hello **podrobnosti o zařízení** panelu, klikněte na tlačítko **povolit zařízení**.</span><span class="sxs-lookup"><span data-stu-id="d36a3-148">Then, in hello **Device Details** panel, click **Enable Device**.</span></span> <span data-ttu-id="d36a3-149">Hello stav zařízení je nyní **systémem**.</span><span class="sxs-lookup"><span data-stu-id="d36a3-149">hello status of your device is now **Running**.</span></span> <span data-ttu-id="d36a3-150">řešení vzdáleného monitorování Hello teď můžete přijímat telemetrická data ze zařízení a volat metody na hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="d36a3-150">hello remote monitoring solution can now receive telemetry from your device and invoke methods on hello device.</span></span>

[img-dashboard]: ./media/iot-suite-selector-connecting/dashboard.png
[1]: ./media/iot-suite-selector-connecting/suite0.png
[2]: ./media/iot-suite-selector-connecting/suite1.png
[3]: ./media/iot-suite-selector-connecting/suite2.png
[4]: ./media/iot-suite-selector-connecting/suite3.png

[lnk-what-are-preconfig-solutions]: ../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: ../articles/iot-suite/iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/