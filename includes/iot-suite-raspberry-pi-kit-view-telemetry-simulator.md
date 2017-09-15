## <a name="view-the-telemetry"></a><span data-ttu-id="93b16-101">Zobrazení telemetrie</span><span class="sxs-lookup"><span data-stu-id="93b16-101">View the telemetry</span></span>

<span data-ttu-id="93b16-102">Pi malin nyní odesílá telemetrii do řešení vzdáleného monitorování.</span><span class="sxs-lookup"><span data-stu-id="93b16-102">The Raspberry Pi is now sending telemetry to the remote monitoring solution.</span></span> <span data-ttu-id="93b16-103">Můžete zobrazit telemetrii na řídicí panel řešení.</span><span class="sxs-lookup"><span data-stu-id="93b16-103">You can view the telemetry on the solution dashboard.</span></span> <span data-ttu-id="93b16-104">Na řídicím panelu řešení také mohou zasílat zprávy do vaší malin pí.</span><span class="sxs-lookup"><span data-stu-id="93b16-104">You can also send messages to your Raspberry Pi from the solution dashboard.</span></span>

- <span data-ttu-id="93b16-105">Přejděte na řídicí panel řešení.</span><span class="sxs-lookup"><span data-stu-id="93b16-105">Navigate to the solution dashboard.</span></span>
- <span data-ttu-id="93b16-106">Vyberte zařízení v **zařízení do zobrazení** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="93b16-106">Select your device in the **Device to View** dropdown.</span></span>
- <span data-ttu-id="93b16-107">Telemetrie z platformy malin zobrazí na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="93b16-107">The telemetry from the Raspberry Pi displays on the dashboard.</span></span>

![Zobrazení telemetrie z malin platformy][img-telemetry-display]

## <a name="act-on-the-device"></a><span data-ttu-id="93b16-109">Fungovat na zařízení</span><span class="sxs-lookup"><span data-stu-id="93b16-109">Act on the device</span></span>

<span data-ttu-id="93b16-110">Na řídicím panelu řešení můžete volat metody na vaše malin pí.</span><span class="sxs-lookup"><span data-stu-id="93b16-110">From the solution dashboard, you can invoke methods on your Raspberry Pi.</span></span> <span data-ttu-id="93b16-111">Když pí malin připojí k řešení vzdáleného monitorování, odešle informace o metodách, které podporuje.</span><span class="sxs-lookup"><span data-stu-id="93b16-111">When the Raspberry Pi connects to the remote monitoring solution, it sends information about the methods it supports.</span></span>

- <span data-ttu-id="93b16-112">Na řídicím panelu řešení, klikněte na tlačítko **zařízení** přejděte **zařízení** stránky.</span><span class="sxs-lookup"><span data-stu-id="93b16-112">In the solution dashboard, click **Devices** to visit the **Devices** page.</span></span> <span data-ttu-id="93b16-113">Vyberte vaše Malinová platformy v **seznam zařízení**.</span><span class="sxs-lookup"><span data-stu-id="93b16-113">Select your Raspberry Pi in the **Device List**.</span></span> <span data-ttu-id="93b16-114">Zvolte **metody**:</span><span class="sxs-lookup"><span data-stu-id="93b16-114">Then choose **Methods**:</span></span>

    ![Seznam zařízení na řídicím panelu][img-list-devices]

- <span data-ttu-id="93b16-116">Na **vyvolání metody** vyberte **LightBlink** v **metoda** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="93b16-116">On the **Invoke Method** page, choose **LightBlink** in the **Method** dropdown.</span></span>

- <span data-ttu-id="93b16-117">Zvolte **InvokeMethod**.</span><span class="sxs-lookup"><span data-stu-id="93b16-117">Choose **InvokeMethod**.</span></span> <span data-ttu-id="93b16-118">Simulátor vytiskne zprávy v konzole na malin pí.</span><span class="sxs-lookup"><span data-stu-id="93b16-118">The simulator prints a message in the console on the Raspberry Pi.</span></span> <span data-ttu-id="93b16-119">Aplikace na platformy malin odešle na potvrzení zpět na řídicí panel řešení:</span><span class="sxs-lookup"><span data-stu-id="93b16-119">The app on the Raspberry Pi sends an acknowledgment back to the solution dashboard:</span></span>

    ![Zobrazit historii – metoda][img-method-history]

- <span data-ttu-id="93b16-121">Můžete přepnout Indikátor zapnout a vypnout pomocí **ChangeLightStatus** metoda s **LightStatusValue** nastavena na **1** pro na nebo **0** pro vypnout.</span><span class="sxs-lookup"><span data-stu-id="93b16-121">You can switch the LED on and off using the **ChangeLightStatus** method with a **LightStatusValue** set to **1** for on or **0** for off.</span></span>

> [!WARNING]
> <span data-ttu-id="93b16-122">Pokud necháte řešení vzdáleného monitorování spuštěné v účtu Azure, se vám účtuje v době spuštění.</span><span class="sxs-lookup"><span data-stu-id="93b16-122">If you leave the remote monitoring solution running in your Azure account, you are billed for the time it runs.</span></span> <span data-ttu-id="93b16-123">Další informace o snížení spotřeby průběhu řešení vzdáleného monitorování najdete v tématu [konfigurace Azure IoT Suite předkonfigurovaných řešení pro účely ukázky][lnk-demo-config].</span><span class="sxs-lookup"><span data-stu-id="93b16-123">For more information about reducing consumption while the remote monitoring solution runs, see [Configuring Azure IoT Suite preconfigured solutions for demo purposes][lnk-demo-config].</span></span> <span data-ttu-id="93b16-124">Odstraňte předkonfigurované řešení z účtu Azure, když přestanete používat.</span><span class="sxs-lookup"><span data-stu-id="93b16-124">Delete the preconfigured solution from your Azure account when you have finished using it.</span></span>


[img-telemetry-display]: media/iot-suite-raspberry-pi-kit-view-telemetry-simulator/telemetry.png
[img-list-devices]: media/iot-suite-raspberry-pi-kit-view-telemetry-simulator/listdevices.png
[img-method-history]: media/iot-suite-raspberry-pi-kit-view-telemetry-simulator/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md