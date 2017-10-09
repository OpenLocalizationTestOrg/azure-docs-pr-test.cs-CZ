## <a name="configure-hello-nodejs-simulated-device"></a><span data-ttu-id="51062-101">Konfigurace simulované zařízení Node.js hello</span><span class="sxs-lookup"><span data-stu-id="51062-101">Configure hello Node.js simulated device</span></span>
1. <span data-ttu-id="51062-102">Na panelu vzdáleného monitorování hello, klikněte na tlačítko **+ přidat zařízení** a pak přidejte *vlastní zařízení*.</span><span class="sxs-lookup"><span data-stu-id="51062-102">On hello remote monitoring dashboard, click **+ Add a device** and then add a *custom device*.</span></span> <span data-ttu-id="51062-103">Poznamenejte hello IoT Hub, název hostitele, id zařízení a klíč zařízení.</span><span class="sxs-lookup"><span data-stu-id="51062-103">Make a note of hello IoT Hub hostname, device id, and device key.</span></span> <span data-ttu-id="51062-104">Musíte je později v tomto kurzu při přípravě hello remote_monitoring.js zařízení klientské aplikace.</span><span class="sxs-lookup"><span data-stu-id="51062-104">You need them later in this tutorial when you prepare hello remote_monitoring.js device client application.</span></span>
2. <span data-ttu-id="51062-105">Ujistěte se, že Node.js verze 0.12.x nebo novější je nainstalován na vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="51062-105">Ensure that Node.js version 0.12.x or later is installed on your development machine.</span></span> <span data-ttu-id="51062-106">Spustit `node --version` na příkazovém řádku nebo v prostředí toocheck hello verzi.</span><span class="sxs-lookup"><span data-stu-id="51062-106">Run `node --version` at a command prompt or in a shell toocheck hello version.</span></span> <span data-ttu-id="51062-107">Informace o používání balíček správce tooinstall Node.js v systému Linux najdete v tématu [instalací Node.js prostřednictvím Správce balíčků][node-linux].</span><span class="sxs-lookup"><span data-stu-id="51062-107">For information about using a package manager tooinstall Node.js on Linux, see [Installing Node.js via package manager][node-linux].</span></span>
3. <span data-ttu-id="51062-108">Pokud jste nainstalovali Node.js, klonování hello nejnovější verzi hello [azure-iot-sdk uzlu] [ lnk-github-repo] úložiště tooyour vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="51062-108">When you have installed Node.js, clone hello latest version of hello [azure-iot-sdk-node][lnk-github-repo] repository tooyour development machine.</span></span> <span data-ttu-id="51062-109">Vždy používat hello **hlavní** větve pro hello nejnovější verzi hello knihovny a ukázky.</span><span class="sxs-lookup"><span data-stu-id="51062-109">Always use hello **master** branch for hello latest version of hello libraries and samples.</span></span>
4. <span data-ttu-id="51062-110">Z vaší místní kopii hello [azure-iot-sdk uzlu] [ lnk-github-repo] úložiště, kopie hello následující dva soubory z hello uzlu, zařízení nebo ukázky tooan prázdná složka na vývojovém počítači:</span><span class="sxs-lookup"><span data-stu-id="51062-110">From your local copy of hello [azure-iot-sdk-node][lnk-github-repo] repository, copy hello following two files from hello node/device/samples folder tooan empty folder on your development machine:</span></span>
   
   * <span data-ttu-id="51062-111">Packages.JSON</span><span class="sxs-lookup"><span data-stu-id="51062-111">packages.json</span></span>
   * <span data-ttu-id="51062-112">remote_monitoring.js</span><span class="sxs-lookup"><span data-stu-id="51062-112">remote_monitoring.js</span></span>
5. <span data-ttu-id="51062-113">Otevřete soubor remote_monitoring.js hello a vyhledejte hello za definicí proměnné:</span><span class="sxs-lookup"><span data-stu-id="51062-113">Open hello remote_monitoring.js file and look for hello following variable definition:</span></span>
   
    ```
    var connectionString = "[IoT Hub device connection string]";
    ```
6. <span data-ttu-id="51062-114">Nahraďte **[připojovací řetězec služby IoT Hub zařízení]** připojovacím řetězcem zařízení.</span><span class="sxs-lookup"><span data-stu-id="51062-114">Replace **[IoT Hub device connection string]** with your device connection string.</span></span> <span data-ttu-id="51062-115">Použijte hello hodnoty pro název hostitele služby IoT Hub, id zařízení a klíč zařízení, který jste si poznamenali v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="51062-115">Use hello values for your IoT Hub hostname, device id, and device key that you made a note of in step 1.</span></span> <span data-ttu-id="51062-116">Připojovací řetězec zařízení má hello následující formát:</span><span class="sxs-lookup"><span data-stu-id="51062-116">A device connection string has hello following format:</span></span>
   
    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```
   
    <span data-ttu-id="51062-117">Pokud je váš název hostitele služby IoT Hub **contoso** a je id zařízení **mydevice**, připojovací řetězec vypadá jako hello následující fragment kódu:</span><span class="sxs-lookup"><span data-stu-id="51062-117">If your IoT Hub hostname is **contoso** and your device id is **mydevice**, your connection string looks like hello following snippet:</span></span>
   
    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```
7. Uložte soubor hello. <span data-ttu-id="51062-119">Spusťte následující příkazy v prostředí nebo příkazového řádku ve složce hello, který obsahuje soubory potřebné balíčky tooinstall hello tyto hello a pak spusťte hello ukázkovou aplikaci:</span><span class="sxs-lookup"><span data-stu-id="51062-119">Run hello following commands in a shell or command prompt in hello folder that contains these files tooinstall hello necessary packages and then run hello sample application:</span></span>
   
    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a><span data-ttu-id="51062-120">Sledovat dynamické telemetrie v akci</span><span class="sxs-lookup"><span data-stu-id="51062-120">Observe dynamic telemetry in action</span></span>
<span data-ttu-id="51062-121">řídicí panel Hello zobrazuje hello teploty a vlhkosti telemetrie z hello existující Simulovaná zařízení:</span><span class="sxs-lookup"><span data-stu-id="51062-121">hello dashboard shows hello temperature and humidity telemetry from hello existing simulated devices:</span></span>

![Hello výchozí řídicí panely][image1]

<span data-ttu-id="51062-123">Pokud vyberete Node.js hello simulované se zařízení, které jste spustili v předchozí části hello, uvidíte teploty a vlhkosti, externí teplotní telemetrie:</span><span class="sxs-lookup"><span data-stu-id="51062-123">If you select hello Node.js simulated device you ran in hello previous section, you see temperature, humidity, and external temperature telemetry:</span></span>

![Přidat externí teplotu toohello řídicí panel][image2]

<span data-ttu-id="51062-125">řešení vzdáleného monitorování Hello automaticky zjišťuje hello další externí teplotní telemetrie typu a přidá na řídicí panel hello toohello grafu.</span><span class="sxs-lookup"><span data-stu-id="51062-125">hello remote monitoring solution automatically detects hello additional external temperature telemetry type and adds it toohello chart on hello dashboard.</span></span>

[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdk-node
[image1]: media/iot-suite-send-external-temperature/image1.png
[image2]: media/iot-suite-send-external-temperature/image2.png