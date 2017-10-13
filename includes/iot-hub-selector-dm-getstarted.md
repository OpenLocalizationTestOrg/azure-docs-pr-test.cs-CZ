> [!div class="op_single_selector"]
> * [<span data-ttu-id="17cd0-101">Zařízení: Node.js Služba: Node.js</span><span class="sxs-lookup"><span data-stu-id="17cd0-101">Device: Node.js Service: Node.js</span></span>](../articles/iot-hub/iot-hub-node-node-device-management-get-started.md)
> * [<span data-ttu-id="17cd0-102">Zařízení: Node.js Služba: C#</span><span class="sxs-lookup"><span data-stu-id="17cd0-102">Device: Node.js Service: C#</span></span>](../articles/iot-hub/iot-hub-csharp-node-device-management-get-started.md)
> * [<span data-ttu-id="17cd0-103">Zařízení: Služba Java: Java</span><span class="sxs-lookup"><span data-stu-id="17cd0-103">Device: Java Service: Java</span></span>](../articles/iot-hub/iot-hub-java-java-device-management-getstarted.md)

<span data-ttu-id="17cd0-104">Back-end aplikace můžete použít Azure IoT Hub primitiv, jako například [dvojče zařízení] [ lnk-devtwin] a [přímé metody][lnk-c2dmethod], tooremotely spuštění a monitorování akce správy zařízení na zařízení.</span><span class="sxs-lookup"><span data-stu-id="17cd0-104">Back-end apps can use Azure IoT Hub primitives, such as [device twin][lnk-devtwin] and [direct methods][lnk-c2dmethod], tooremotely start and monitor device management actions on devices.</span></span> <span data-ttu-id="17cd0-105">Tento kurz ukazuje, jak můžete spolupracovat tooinitiate a monitorovat restartování vzdáleného zařízení pomocí služby IoT Hub back-end aplikace a aplikace na zařízení.</span><span class="sxs-lookup"><span data-stu-id="17cd0-105">This tutorial shows you how a back-end app and a device app can work together tooinitiate and monitor a remote device reboot using IoT Hub.</span></span>

<span data-ttu-id="17cd0-106">Použijte přímá metoda tooinitiate zařízení akce správy (například restartování, obnovení továrního nastavení a aktualizaci firmwaru) z back-end aplikace v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="17cd0-106">Use a direct method tooinitiate device management actions (such as reboot, factory reset, and firmware update) from a back-end app in hello cloud.</span></span> <span data-ttu-id="17cd0-107">Hello zařízení je zodpovědná za:</span><span class="sxs-lookup"><span data-stu-id="17cd0-107">hello device is responsible for:</span></span>

* <span data-ttu-id="17cd0-108">Zpracování hello metoda požadavek odeslaný ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="17cd0-108">Handling hello method request sent from IoT Hub.</span></span>
* <span data-ttu-id="17cd0-109">Probíhá inicializace hello odpovídající akce konkrétní zařízení na zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="17cd0-109">Initiating hello corresponding device-specific action on hello device.</span></span>
* <span data-ttu-id="17cd0-110">Poskytuje stav aktualizací prostřednictvím *hlášené vlastnosti* tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="17cd0-110">Providing status updates through *reported properties* tooIoT Hub.</span></span>

<span data-ttu-id="17cd0-111">Back-end aplikace můžete použít v hello cloudu toorun zařízení twin dotazy tooreport na průběh hello vaše akce správy zařízení.</span><span class="sxs-lookup"><span data-stu-id="17cd0-111">You can use a back-end app in hello cloud toorun device twin queries tooreport on hello progress of your device management actions.</span></span>

[lnk-devtwin]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md