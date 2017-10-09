## <a name="view-device-telemetry-in-hello-dashboard"></a><span data-ttu-id="6fad7-101">Zobrazení telemetrie zařízení na řídicím panelu hello</span><span class="sxs-lookup"><span data-stu-id="6fad7-101">View device telemetry in hello dashboard</span></span>
<span data-ttu-id="6fad7-102">řídicí panel Hello v hello vzdálené monitorování umožňuje řešení můžete tooview hello telemetrie zařízení odesílat tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="6fad7-102">hello dashboard in hello remote monitoring solution enables you tooview hello telemetry your devices send tooIoT Hub.</span></span>

1. <span data-ttu-id="6fad7-103">V prohlížeči, návratový toohello vzdálené monitorování řídicí panel řešení, klikněte na tlačítko **zařízení** v hello levém panelu toonavigate toohello **seznam zařízení**.</span><span class="sxs-lookup"><span data-stu-id="6fad7-103">In your browser, return toohello remote monitoring solution dashboard, click **Devices** in hello left-hand panel toonavigate toohello **Devices list**.</span></span>
2. <span data-ttu-id="6fad7-104">V hello **seznam zařízení**, měli byste vidět, že hello stav zařízení je **systémem**.</span><span class="sxs-lookup"><span data-stu-id="6fad7-104">In hello **Devices list**, you should see that hello status of your device is **Running**.</span></span> <span data-ttu-id="6fad7-105">Pokud ne, klikněte na tlačítko **povolit zařízení** v hello **podrobnosti o zařízení** panelu.</span><span class="sxs-lookup"><span data-stu-id="6fad7-105">If not, click **Enable Device** in hello **Device Details** panel.</span></span>
   
    ![Zobrazení stavu zařízení][18]
3. <span data-ttu-id="6fad7-107">Klikněte na tlačítko **řídicí panel** tooreturn toohello řídicí panel, vyberte příslušné zařízení v hello **zařízení tooView** tooview rozevíracího seznamu jeho telemetrie.</span><span class="sxs-lookup"><span data-stu-id="6fad7-107">Click **Dashboard** tooreturn toohello dashboard, select your device in hello **Device tooView** drop-down tooview its telemetry.</span></span> <span data-ttu-id="6fad7-108">Hello telemetrie z ukázkové aplikace hello je 50 jednotky pro interní teploty, 55 jednotky pro externí teplotu a 50 jednotky pro vlhkosti.</span><span class="sxs-lookup"><span data-stu-id="6fad7-108">hello telemetry from hello sample application is 50 units for internal temperature, 55 units for external temperature, and 50 units for humidity.</span></span>
   
    ![Zobrazení telemetrie zařízení][img-telemetry]

## <a name="invoke-a-method-on-your-device"></a><span data-ttu-id="6fad7-110">Volání metody na zařízení</span><span class="sxs-lookup"><span data-stu-id="6fad7-110">Invoke a method on your device</span></span>
<span data-ttu-id="6fad7-111">Hello řídicího panelu řešení vzdáleného monitorování hello umožňuje tooinvoke metody zařízení prostřednictvím služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="6fad7-111">hello dashboard in hello remote monitoring solution enables you tooinvoke methods on your devices through IoT Hub.</span></span> <span data-ttu-id="6fad7-112">Například v hello řešení vzdáleného sledování můžete vyvolat metodu toosimulate, restartování zařízení.</span><span class="sxs-lookup"><span data-stu-id="6fad7-112">For example, in hello remote monitoring solution you can invoke a method toosimulate rebooting a device.</span></span>

1. <span data-ttu-id="6fad7-113">V hello vzdálené monitorování řídicí panel řešení, klikněte na **zařízení** v hello levém panelu toonavigate toohello **seznam zařízení**.</span><span class="sxs-lookup"><span data-stu-id="6fad7-113">In hello remote monitoring solution dashboard, click **Devices** in hello left-hand panel toonavigate toohello **Devices list**.</span></span>
2. <span data-ttu-id="6fad7-114">Klikněte na tlačítko **ID zařízení** pro zařízení v hello **seznam zařízení**.</span><span class="sxs-lookup"><span data-stu-id="6fad7-114">Click **Device ID** for your device in hello **Devices list**.</span></span>
3. <span data-ttu-id="6fad7-115">V hello **podrobnosti o zařízení** panelu, klikněte na tlačítko **metody**.</span><span class="sxs-lookup"><span data-stu-id="6fad7-115">In hello **Device details** panel, click **Methods**.</span></span>
   
    ![Metody zařízení][13]
4. <span data-ttu-id="6fad7-117">V hello **metoda** rozevíracího seznamu, vyberte **InitiateFirmwareUpdate**a potom v **FWPACKAGEURI** zadejte fiktivní adresu URL.</span><span class="sxs-lookup"><span data-stu-id="6fad7-117">In hello **Method** drop-down, select **InitiateFirmwareUpdate**, and then in **FWPACKAGEURI** enter a dummy URL.</span></span> <span data-ttu-id="6fad7-118">Klikněte na tlačítko **vyvolání metody** toocall hello metodu hello zařízení.</span><span class="sxs-lookup"><span data-stu-id="6fad7-118">Click **Invoke Method** toocall hello method on hello device.</span></span>
   
    ![Vyvolání metody zařízení][14]
   

5. <span data-ttu-id="6fad7-120">Zobrazí se zpráva v konzole hello spuštění kódu vašeho zařízení, když zařízení hello zpracovává metoda hello.</span><span class="sxs-lookup"><span data-stu-id="6fad7-120">You see a message in hello console running your device code when hello device handles hello method.</span></span> <span data-ttu-id="6fad7-121">výsledky Hello hello metody přidají toohello historie hello řešení portálu:</span><span class="sxs-lookup"><span data-stu-id="6fad7-121">hello results of hello method are added toohello history in hello solution portal:</span></span>

    ![Zobrazení historie metod][img-method-history]

## <a name="next-steps"></a><span data-ttu-id="6fad7-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6fad7-123">Next steps</span></span>
<span data-ttu-id="6fad7-124">článek Hello [přizpůsobení předkonfigurovaných řešení] [ lnk-customize] popisuje několik způsobů, jak můžete rozšířit této ukázce.</span><span class="sxs-lookup"><span data-stu-id="6fad7-124">hello article [Customizing preconfigured solutions][lnk-customize] describes some ways you can extend this sample.</span></span> <span data-ttu-id="6fad7-125">Mezi možná rozšíření patří skutečné senzory a implementace dalších příkazů.</span><span class="sxs-lookup"><span data-stu-id="6fad7-125">Possible extensions include using real sensors and implementing additional commands.</span></span>

<span data-ttu-id="6fad7-126">Další informace o hello [oprávnění na webu azureiotsuite.com hello][lnk-permissions].</span><span class="sxs-lookup"><span data-stu-id="6fad7-126">You can learn more about hello [permissions on hello azureiotsuite.com site][lnk-permissions].</span></span>

[13]: ./media/iot-suite-visualize-connecting/suite4.png
[14]: ./media/iot-suite-visualize-connecting/suite7-1.png
[18]: ./media/iot-suite-visualize-connecting/suite10.png
[img-telemetry]: ./media/iot-suite-visualize-connecting/telemetry.png
[img-method-history]: ./media/iot-suite-visualize-connecting/history.png
[lnk-customize]: ../articles/iot-suite/iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-permissions]: ../articles/iot-suite/iot-suite-permissions.md
