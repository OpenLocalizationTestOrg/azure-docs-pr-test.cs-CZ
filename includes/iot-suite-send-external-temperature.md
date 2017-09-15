## <a name="configure-the-nodejs-simulated-device"></a><span data-ttu-id="9b9cd-101">Konfigurace simulované zařízení Node.js</span><span class="sxs-lookup"><span data-stu-id="9b9cd-101">Configure the Node.js simulated device</span></span>
1. <span data-ttu-id="9b9cd-102">Na řídicím panelu vzdáleného monitorování, klikněte na tlačítko **+ přidat zařízení** a pak přidejte *vlastní zařízení*.</span><span class="sxs-lookup"><span data-stu-id="9b9cd-102">On the remote monitoring dashboard, click **+ Add a device** and then add a *custom device*.</span></span> <span data-ttu-id="9b9cd-103">Poznamenejte IoT Hub, název hostitele, id zařízení a klíč zařízení.</span><span class="sxs-lookup"><span data-stu-id="9b9cd-103">Make a note of the IoT Hub hostname, device id, and device key.</span></span> <span data-ttu-id="9b9cd-104">Musíte je později v tomto kurzu při přípravě remote_monitoring.js zařízení klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="9b9cd-104">You need them later in this tutorial when you prepare the remote_monitoring.js device client application.</span></span>
2. <span data-ttu-id="9b9cd-105">Ujistěte se, že Node.js verze 0.12.x nebo novější je nainstalován na vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="9b9cd-105">Ensure that Node.js version 0.12.x or later is installed on your development machine.</span></span> <span data-ttu-id="9b9cd-106">Spustit `node --version` na příkazovém řádku nebo v prostředí pro kontrolu verze.</span><span class="sxs-lookup"><span data-stu-id="9b9cd-106">Run `node --version` at a command prompt or in a shell to check the version.</span></span> <span data-ttu-id="9b9cd-107">Informace o používání Správce balíčků instalace Node.js na platformě Linux najdete v tématu [instalací Node.js prostřednictvím Správce balíčků][node-linux].</span><span class="sxs-lookup"><span data-stu-id="9b9cd-107">For information about using a package manager to install Node.js on Linux, see [Installing Node.js via package manager][node-linux].</span></span>
3. <span data-ttu-id="9b9cd-108">Pokud jste nainstalovali Node.js, klonování nejnovější verzi [azure-iot-sdk uzlu] [ lnk-github-repo] úložiště na vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="9b9cd-108">When you have installed Node.js, clone the latest version of the [azure-iot-sdk-node][lnk-github-repo] repository to your development machine.</span></span> <span data-ttu-id="9b9cd-109">Vždy nutné použít **hlavní** větve pro nejnovější verzi knihovny a ukázky.</span><span class="sxs-lookup"><span data-stu-id="9b9cd-109">Always use the **master** branch for the latest version of the libraries and samples.</span></span>
4. <span data-ttu-id="9b9cd-110">Z vaší místní kopii [azure-iot-sdk uzlu] [ lnk-github-repo] úložiště, kopírovat následující dva soubory ze složky uzlu, zařízení nebo ukázky k prázdné složce na vývojovém počítači:</span><span class="sxs-lookup"><span data-stu-id="9b9cd-110">From your local copy of the [azure-iot-sdk-node][lnk-github-repo] repository, copy the following two files from the node/device/samples folder to an empty folder on your development machine:</span></span>
   
   * <span data-ttu-id="9b9cd-111">Packages.JSON</span><span class="sxs-lookup"><span data-stu-id="9b9cd-111">packages.json</span></span>
   * <span data-ttu-id="9b9cd-112">remote_monitoring.js</span><span class="sxs-lookup"><span data-stu-id="9b9cd-112">remote_monitoring.js</span></span>
5. <span data-ttu-id="9b9cd-113">Otevřete soubor remote_monitoring.js a vyhledejte definici následující proměnné:</span><span class="sxs-lookup"><span data-stu-id="9b9cd-113">Open the remote_monitoring.js file and look for the following variable definition:</span></span>
   
    ```
    var connectionString = "[IoT Hub device connection string]";
    ```
6. <span data-ttu-id="9b9cd-114">Nahraďte **[připojovací řetězec služby IoT Hub zařízení]** připojovacím řetězcem zařízení.</span><span class="sxs-lookup"><span data-stu-id="9b9cd-114">Replace **[IoT Hub device connection string]** with your device connection string.</span></span> <span data-ttu-id="9b9cd-115">Použijte hodnoty pro název hostitele služby IoT Hub, id zařízení a klíč zařízení, který jste si poznamenali v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="9b9cd-115">Use the values for your IoT Hub hostname, device id, and device key that you made a note of in step 1.</span></span> <span data-ttu-id="9b9cd-116">Zařízení připojovací řetězec má následující formát:</span><span class="sxs-lookup"><span data-stu-id="9b9cd-116">A device connection string has the following format:</span></span>
   
    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```
   
    <span data-ttu-id="9b9cd-117">Pokud je váš název hostitele služby IoT Hub **contoso** a je id zařízení **mydevice**, připojovací řetězec vypadá jako následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="9b9cd-117">If your IoT Hub hostname is **contoso** and your device id is **mydevice**, your connection string looks like the following snippet:</span></span>
   
    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```
7. Uložte soubor. <span data-ttu-id="9b9cd-119">Spusťte následující příkazy v prostředí nebo příkazového řádku ve složce, která obsahuje tyto soubory k instalaci nezbytných balíčků a pak spusťte ukázkovou aplikaci:</span><span class="sxs-lookup"><span data-stu-id="9b9cd-119">Run the following commands in a shell or command prompt in the folder that contains these files to install the necessary packages and then run the sample application:</span></span>
   
    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a><span data-ttu-id="9b9cd-120">Sledovat dynamické telemetrie v akci</span><span class="sxs-lookup"><span data-stu-id="9b9cd-120">Observe dynamic telemetry in action</span></span>
<span data-ttu-id="9b9cd-121">Řídicí panel zobrazuje teploty a vlhkosti telemetrie z existující Simulovaná zařízení:</span><span class="sxs-lookup"><span data-stu-id="9b9cd-121">The dashboard shows the temperature and humidity telemetry from the existing simulated devices:</span></span>

![Výchozí řídicí panel][image1]

<span data-ttu-id="9b9cd-123">Pokud vyberete Node.js simulované zařízení, které jste spustili v předchozí části, uvidíte teploty a vlhkosti, externí teplotní telemetrie:</span><span class="sxs-lookup"><span data-stu-id="9b9cd-123">If you select the Node.js simulated device you ran in the previous section, you see temperature, humidity, and external temperature telemetry:</span></span>

![Přidat externí teplotu na řídicí panel][image2]

<span data-ttu-id="9b9cd-125">Řešení vzdáleného monitorování automaticky zjišťuje typ další externí teplotní telemetrie a přidá do grafu na řídicím panelu.</span><span class="sxs-lookup"><span data-stu-id="9b9cd-125">The remote monitoring solution automatically detects the additional external temperature telemetry type and adds it to the chart on the dashboard.</span></span>

[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdk-node
[image1]: media/iot-suite-send-external-temperature/image1.png
[image2]: media/iot-suite-send-external-temperature/image2.png