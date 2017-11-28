## <a name="view-the-telemetry"></a><span data-ttu-id="73125-101">Zobrazení telemetrie</span><span class="sxs-lookup"><span data-stu-id="73125-101">View the telemetry</span></span>

<span data-ttu-id="73125-102">Pi malin nyní odesílá telemetrii do řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="73125-102">The Raspberry Pi is now sending telemetry to the remote monitoring solution.</span></span> <span data-ttu-id="73125-103">Můžete zobrazit telemetrii na řídicí panel řešení.</span><span class="sxs-lookup"><span data-stu-id="73125-103">You can view the telemetry on the solution dashboard.</span></span> <span data-ttu-id="73125-104">Na řídicím panelu řešení také mohou zasílat zprávy do vaší malin pí.</span><span class="sxs-lookup"><span data-stu-id="73125-104">You can also send messages to your Raspberry Pi from the solution dashboard.</span></span>

- <span data-ttu-id="73125-105">Přejděte na řídicí panel řešení.</span><span class="sxs-lookup"><span data-stu-id="73125-105">Navigate to the solution dashboard.</span></span>
- <span data-ttu-id="73125-106">Vyberte zařízení v **zařízení do zobrazení** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="73125-106">Select your device in the **Device to View** dropdown.</span></span>
- <span data-ttu-id="73125-107">Telemetrie z platformy malin zobrazí na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="73125-107">The telemetry from the Raspberry Pi displays on the dashboard.</span></span>

![Zobrazení telemetrie z malin platformy][img-telemetry-display]

## <a name="act-on-the-device"></a><span data-ttu-id="73125-109">Fungovat na zařízení</span><span class="sxs-lookup"><span data-stu-id="73125-109">Act on the device</span></span>

<span data-ttu-id="73125-110">Na řídicím panelu řešení můžete volat metody na vaše malin pí.</span><span class="sxs-lookup"><span data-stu-id="73125-110">From the solution dashboard, you can invoke methods on your Raspberry Pi.</span></span> <span data-ttu-id="73125-111">Když pí malin připojí k řešení vzdáleného monitorování, odešle informace o metodách, které podporuje.</span><span class="sxs-lookup"><span data-stu-id="73125-111">When the Raspberry Pi connects to the remote monitoring solution, it sends information about the methods it supports.</span></span>

- <span data-ttu-id="73125-112">Na řídicím panelu řešení, klikněte na tlačítko **zařízení** přejděte **zařízení** stránky.</span><span class="sxs-lookup"><span data-stu-id="73125-112">In the solution dashboard, click **Devices** to visit the **Devices** page.</span></span> <span data-ttu-id="73125-113">Vyberte vaše Malinová platformy v **seznam zařízení**.</span><span class="sxs-lookup"><span data-stu-id="73125-113">Select your Raspberry Pi in the **Device List**.</span></span> <span data-ttu-id="73125-114">Zvolte **metody**:</span><span class="sxs-lookup"><span data-stu-id="73125-114">Then choose **Methods**:</span></span>

    ![Seznam zařízení na řídicím panelu][img-list-devices]

- <span data-ttu-id="73125-116">Na **vyvolání metody** vyberte **LightBlink** v **metoda** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="73125-116">On the **Invoke Method** page, choose **LightBlink** in the **Method** dropdown.</span></span>

- <span data-ttu-id="73125-117">Zvolte **InvokeMethod**.</span><span class="sxs-lookup"><span data-stu-id="73125-117">Choose **InvokeMethod**.</span></span> <span data-ttu-id="73125-118">Indikátor LED připojený k blikání malin pí několikrát.</span><span class="sxs-lookup"><span data-stu-id="73125-118">The LED connected to the Raspberry Pi flashes several times.</span></span> <span data-ttu-id="73125-119">Aplikace na platformy malin odešle na potvrzení zpět na řídicí panel řešení:</span><span class="sxs-lookup"><span data-stu-id="73125-119">The app on the Raspberry Pi sends an acknowledgment back to the solution dashboard:</span></span>

    ![Zobrazit historii – metoda][img-method-history]

- <span data-ttu-id="73125-121">Můžete přepnout Indikátor zapnout a vypnout pomocí **ChangeLightStatus** metoda s **LightStatusValue** nastavena na **1** pro na nebo **0** pro vypnout.</span><span class="sxs-lookup"><span data-stu-id="73125-121">You can switch the LED on and off using the **ChangeLightStatus** method with a **LightStatusValue** set to **1** for on or **0** for off.</span></span>

> [!WARNING]
> <span data-ttu-id="73125-122">Pokud necháte řešení vzdáleného monitorování spuštěné v účtu Azure, se vám účtuje v době spuštění.</span><span class="sxs-lookup"><span data-stu-id="73125-122">If you leave the remote monitoring solution running in your Azure account, you are billed for the time it runs.</span></span> <span data-ttu-id="73125-123">Další informace o snížení spotřeby průběhu řešení vzdáleného monitorování najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="73125-123">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span> <span data-ttu-id="73125-124">Odstraňte předkonfigurované řešení z účtu Azure, když přestanete používat.</span><span class="sxs-lookup"><span data-stu-id="73125-124">Delete the preconfigured solution from your Azure account when you have finished using it.</span></span>


[img-telemetry-display]: media/iot-suite-raspberry-pi-kit-view-telemetry/telemetry.png
[img-list-devices]: media/iot-suite-raspberry-pi-kit-view-telemetry/listdevices.png
[img-method-history]: media/iot-suite-raspberry-pi-kit-view-telemetry/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md