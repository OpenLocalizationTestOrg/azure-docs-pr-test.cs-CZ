## <a name="view-hello-telemetry"></a><span data-ttu-id="b5350-101">Zobrazení telemetrie hello</span><span class="sxs-lookup"><span data-stu-id="b5350-101">View hello telemetry</span></span>

<span data-ttu-id="b5350-102">Hello malin platformy je nyní odesílání telemetrie toohello řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="b5350-102">hello Raspberry Pi is now sending telemetry toohello remote monitoring solution.</span></span> <span data-ttu-id="b5350-103">Můžete zobrazit telemetrii hello na řídicí panel řešení hello.</span><span class="sxs-lookup"><span data-stu-id="b5350-103">You can view hello telemetry on hello solution dashboard.</span></span> <span data-ttu-id="b5350-104">Můžete také odeslat zprávy tooyour malin pí z řídicí panel řešení hello.</span><span class="sxs-lookup"><span data-stu-id="b5350-104">You can also send messages tooyour Raspberry Pi from hello solution dashboard.</span></span>

- <span data-ttu-id="b5350-105">Přejděte toohello řídicí panel řešení.</span><span class="sxs-lookup"><span data-stu-id="b5350-105">Navigate toohello solution dashboard.</span></span>
- <span data-ttu-id="b5350-106">Vyberte zařízení v hello **zařízení tooView** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="b5350-106">Select your device in hello **Device tooView** dropdown.</span></span>
- <span data-ttu-id="b5350-107">Hello telemetrie z hello malin platformy zobrazí na řídicím panelu hello.</span><span class="sxs-lookup"><span data-stu-id="b5350-107">hello telemetry from hello Raspberry Pi displays on hello dashboard.</span></span>

![Zobrazení telemetrie z hello malin platformy][img-telemetry-display]

## <a name="act-on-hello-device"></a><span data-ttu-id="b5350-109">Fungovat na zařízení hello</span><span class="sxs-lookup"><span data-stu-id="b5350-109">Act on hello device</span></span>

<span data-ttu-id="b5350-110">Z řídicího panelu řešení hello můžete volat metody na vaše malin pí.</span><span class="sxs-lookup"><span data-stu-id="b5350-110">From hello solution dashboard, you can invoke methods on your Raspberry Pi.</span></span> <span data-ttu-id="b5350-111">Když hello malin pí připojí toohello řešení vzdáleného monitorování, odešle informace o metodách hello, kterou podporuje.</span><span class="sxs-lookup"><span data-stu-id="b5350-111">When hello Raspberry Pi connects toohello remote monitoring solution, it sends information about hello methods it supports.</span></span>

- <span data-ttu-id="b5350-112">Na řídicím panelu řešení hello, klikněte na tlačítko **zařízení** toovisit hello **zařízení** stránky.</span><span class="sxs-lookup"><span data-stu-id="b5350-112">In hello solution dashboard, click **Devices** toovisit hello **Devices** page.</span></span> <span data-ttu-id="b5350-113">Vyberte platformy vaší malin v hello **seznam zařízení**.</span><span class="sxs-lookup"><span data-stu-id="b5350-113">Select your Raspberry Pi in hello **Device List**.</span></span> <span data-ttu-id="b5350-114">Zvolte **metody**:</span><span class="sxs-lookup"><span data-stu-id="b5350-114">Then choose **Methods**:</span></span>

    ![Seznam zařízení na řídicím panelu][img-list-devices]

- <span data-ttu-id="b5350-116">Na hello **vyvolání metody** vyberte **LightBlink** v hello **metoda** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="b5350-116">On hello **Invoke Method** page, choose **LightBlink** in hello **Method** dropdown.</span></span>

- <span data-ttu-id="b5350-117">Zvolte **InvokeMethod**.</span><span class="sxs-lookup"><span data-stu-id="b5350-117">Choose **InvokeMethod**.</span></span> <span data-ttu-id="b5350-118">Simulátor Hello vytiskne zprávy v konzole hello na hello malin pí.</span><span class="sxs-lookup"><span data-stu-id="b5350-118">hello simulator prints a message in hello console on hello Raspberry Pi.</span></span> <span data-ttu-id="b5350-119">aplikace Hello na hello malin pí odešle řídicí panel řešení back toohello potvrzení:</span><span class="sxs-lookup"><span data-stu-id="b5350-119">hello app on hello Raspberry Pi sends an acknowledgment back toohello solution dashboard:</span></span>

    ![Zobrazit historii – metoda][img-method-history]

- <span data-ttu-id="b5350-121">Můžete přepnout hello DIODU zapnout a vypnout pomocí hello **ChangeLightStatus** metoda s **LightStatusValue** nastavit příliš**1** pro na nebo **0** pro vypnout.</span><span class="sxs-lookup"><span data-stu-id="b5350-121">You can switch hello LED on and off using hello **ChangeLightStatus** method with a **LightStatusValue** set too**1** for on or **0** for off.</span></span>

> [!WARNING]
> <span data-ttu-id="b5350-122">Pokud necháte hello vzdálené monitorování řešení, které jsou spuštěné v účtu Azure, se vám účtuje hello, když ji spustí.</span><span class="sxs-lookup"><span data-stu-id="b5350-122">If you leave hello remote monitoring solution running in your Azure account, you are billed for hello time it runs.</span></span> <span data-ttu-id="b5350-123">Další informace o snížení spotřeby při hello vzdálené monitorování spustí řešení najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="b5350-123">For more information about reducing consumption while hello remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span> <span data-ttu-id="b5350-124">Po dokončení používat, odstraňte hello předkonfigurované řešení z účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="b5350-124">Delete hello preconfigured solution from your Azure account when you have finished using it.</span></span>


[img-telemetry-display]: media/iot-suite-raspberry-pi-kit-view-telemetry-simulator/telemetry.png
[img-list-devices]: media/iot-suite-raspberry-pi-kit-view-telemetry-simulator/listdevices.png
[img-method-history]: media/iot-suite-raspberry-pi-kit-view-telemetry-simulator/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md