## <a name="create-a-device-identity"></a><span data-ttu-id="2b599-101">Vytvoření identity zařízení</span><span class="sxs-lookup"><span data-stu-id="2b599-101">Create a device identity</span></span>
<span data-ttu-id="2b599-102">V této části vytvoříte konzolovou aplikaci .NET, která vytvoří identitu zařízení v registru identit hello ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2b599-102">In this section, you create a .NET console app that creates a device identity in hello identity registry in your IoT hub.</span></span> <span data-ttu-id="2b599-103">Pokud má záznam v registru identit hello se nemůže připojit zařízení tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="2b599-103">A device cannot connect tooIoT hub unless it has an entry in hello identity registry.</span></span> <span data-ttu-id="2b599-104">Další informace najdete v části "Registr identit" hello hello [Příručka vývojáře pro službu IoT Hub][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="2b599-104">For more information, see hello "Identity registry" section of hello [IoT Hub developer guide][lnk-devguide-identity].</span></span> <span data-ttu-id="2b599-105">Při spuštění této konzolové aplikace vygeneruje jedinečné ID zařízení a klíč, že vaše zařízení použít tooidentify samotné při odešle zařízení cloud zprávy tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="2b599-105">When you run this console app, it generates a unique device ID and key that your device can use tooidentify itself when it sends device-to-cloud messages tooIoT Hub.</span></span> <span data-ttu-id="2b599-106">V ID zařízení se rozlišují malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="2b599-106">Device IDs are case sensitive.</span></span>

1. <span data-ttu-id="2b599-107">V sadě Visual Studio, přidejte nové řešení Visual C# Windows klasický desktopový projekt tooa pomocí hello **konzolovou aplikaci (rozhraní .NET Framework)** šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="2b599-107">In Visual Studio, add a Visual C# Windows Classic Desktop project tooa new solution by using hello **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="2b599-108">Zkontrolujte, zda je hello verze rozhraní .NET Framework 4.5.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="2b599-108">Make sure hello .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="2b599-109">Název projektu hello **CreateDeviceIdentity** a název hello řešení **IoTHubGetStarted**.</span><span class="sxs-lookup"><span data-stu-id="2b599-109">Name hello project **CreateDeviceIdentity** and name hello solution **IoTHubGetStarted**.</span></span>
   
    ![Nový klasický desktopový projekt Visual C# pro systém Windows][10]
2. <span data-ttu-id="2b599-111">V Průzkumníku řešení klikněte pravým tlačítkem na hello **CreateDeviceIdentity** projektu a pak klikněte na **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="2b599-111">In Solution Explorer, right-click hello **CreateDeviceIdentity** project, and then click **Manage NuGet Packages**.</span></span>
3. <span data-ttu-id="2b599-112">V hello **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte **nainstalovat** tooinstall Hello **Microsoft.Azure.Devices** balíček a přijměte podmínky použití hello.</span><span class="sxs-lookup"><span data-stu-id="2b599-112">In hello **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="2b599-113">Tento postup stáhne, nainstaluje a přidá odkaz toohello [sady SDK služby Azure IoT] [ lnk-nuget-service-sdk] NuGet balíček a jeho závislé součásti.</span><span class="sxs-lookup"><span data-stu-id="2b599-113">This procedure downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![Okno Správce balíčků NuGet][11]
4. <span data-ttu-id="2b599-115">Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:</span><span class="sxs-lookup"><span data-stu-id="2b599-115">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;
5. <span data-ttu-id="2b599-116">Přidejte následující pole toohello hello **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="2b599-116">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="2b599-117">Nahraďte hodnotu zástupného symbolu hello hello připojovací řetězec služby IoT Hub pro hello rozbočovače, který jste vytvořili v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="2b599-117">Replace hello placeholder value with hello IoT Hub connection string for hello hub that you created in hello previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
6. <span data-ttu-id="2b599-118">Přidejte následující metodu toohello hello **Program** třídy:</span><span class="sxs-lookup"><span data-stu-id="2b599-118">Add hello following method toohello **Program** class:</span></span>
   
        private static async Task AddDeviceAsync()
        {
            string deviceId = "myFirstDevice";
            Device device;
            try
            {
                device = await registryManager.AddDeviceAsync(new Device(deviceId));
            }
            catch (DeviceAlreadyExistsException)
            {
                device = await registryManager.GetDeviceAsync(deviceId);
            }
            Console.WriteLine("Generated device key: {0}", device.Authentication.SymmetricKey.PrimaryKey);
        }
   
    <span data-ttu-id="2b599-119">Tato metoda vytvoří identitu zařízení s ID **myFirstDevice**.</span><span class="sxs-lookup"><span data-stu-id="2b599-119">This method creates a device identity with ID **myFirstDevice**.</span></span> <span data-ttu-id="2b599-120">(Pokud toto ID zařízení již existuje v registru identit hello, hello kód jednoduše načte hello stávající informace o zařízení.) Hello aplikace pak zobrazí hello primární klíč pro danou identitu.</span><span class="sxs-lookup"><span data-stu-id="2b599-120">(If that device ID already exists in hello identity registry, hello code simply retrieves hello existing device information.) hello app then displays hello primary key for that identity.</span></span> <span data-ttu-id="2b599-121">Tento klíč se používá hello simulované zařízení aplikaci tooconnect tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2b599-121">You use this key in hello simulated device app tooconnect tooyour IoT hub.</span></span>
[!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

7. <span data-ttu-id="2b599-122">Nakonec přidejte následující řádky toohello hello **hlavní** metoda:</span><span class="sxs-lookup"><span data-stu-id="2b599-122">Finally, add hello following lines toohello **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();
8. <span data-ttu-id="2b599-123">Tuto aplikaci spustit a poznamenejte si klíč zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="2b599-123">Run this application, and make a note of hello device key.</span></span>
   
    ![Klíč zařízení generovaný aplikací hello][12]

> [!NOTE]
> <span data-ttu-id="2b599-125">Hello registru identit služby IoT Hub ukládá jenom zařízení identity tooenable zabezpečený přístup toohello IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2b599-125">hello IoT Hub identity registry only stores device identities tooenable secure access toohello IoT hub.</span></span> <span data-ttu-id="2b599-126">Ukládá ID a klíče toouse zařízení jako zabezpečovací pověření a příznak povoleno/zakázáno, které můžete toodisable přístup k jednotlivým zařízením.</span><span class="sxs-lookup"><span data-stu-id="2b599-126">It stores device IDs and keys toouse as security credentials, and an enabled/disabled flag that you can use toodisable access for an individual device.</span></span> <span data-ttu-id="2b599-127">Pokud aplikace potřebuje toostore další metadata specifická pro zařízení, měla by používat úložiště pro konkrétní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2b599-127">If your application needs toostore other device-specific metadata, it should use an application-specific store.</span></span> <span data-ttu-id="2b599-128">Další informace najdete v [Příručce pro vývojáře pro službu IoT Hub][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="2b599-128">For more information, see [IoT Hub developer guide][lnk-devguide-identity].</span></span>
> 
> 

<!-- Images. -->
[10]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp1.png
[11]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp2.png
[12]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp3.png


<!-- Links -->
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
