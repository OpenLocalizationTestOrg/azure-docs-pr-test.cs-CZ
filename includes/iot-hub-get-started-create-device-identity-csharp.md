## <a name="create-a-device-identity"></a><span data-ttu-id="b34b8-101">Vytvoření identity zařízení</span><span class="sxs-lookup"><span data-stu-id="b34b8-101">Create a device identity</span></span>
<span data-ttu-id="b34b8-102">V této části vytvoříte konzolovou aplikaci .NET, která v registru identit ve službě IoT Hub vytvoří identitu zařízení.</span><span class="sxs-lookup"><span data-stu-id="b34b8-102">In this section, you create a .NET console app that creates a device identity in the identity registry in your IoT hub.</span></span> <span data-ttu-id="b34b8-103">Zařízení lze připojit ke službě IoT Hub, pouze pokud má záznam v registru identit.</span><span class="sxs-lookup"><span data-stu-id="b34b8-103">A device cannot connect to IoT hub unless it has an entry in the identity registry.</span></span> <span data-ttu-id="b34b8-104">Další informace najdete v části Registr identit v [Příručce pro vývojáře pro službu IoT Hub][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="b34b8-104">For more information, see the "Identity registry" section of the [IoT Hub developer guide][lnk-devguide-identity].</span></span> <span data-ttu-id="b34b8-105">Tato konzolová aplikace po spuštění vygeneruje jedinečné ID zařízení a klíč, s jehož pomocí se zařízení může identifikovat při posílání zpráv typu zařízení-cloud do služby IoT Hub. </span><span class="sxs-lookup"><span data-stu-id="b34b8-105">When you run this console app, it generates a unique device ID and key that your device can use to identify itself when it sends device-to-cloud messages to IoT Hub.</span></span> <span data-ttu-id="b34b8-106">V ID zařízení se rozlišují malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="b34b8-106">Device IDs are case sensitive.</span></span>

1. <span data-ttu-id="b34b8-107">V sadě Visual Studio přidejte k novému řešení klasický desktopový projekt Visual C# pro systém Windows pomocí šablony projektu **Konzolová aplikace (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="b34b8-107">In Visual Studio, add a Visual C# Windows Classic Desktop project to a new solution by using the **Console App (.NET Framework)** project template.</span></span> <span data-ttu-id="b34b8-108">Zkontrolujte, zda máte verzi rozhraní .NET Framework 4.5.1 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="b34b8-108">Make sure the .NET Framework version is 4.5.1 or later.</span></span> <span data-ttu-id="b34b8-109">Projekt nazvěte **CreateDeviceIdentity** a řešení nazvěte **IoTHubGetStarted**.</span><span class="sxs-lookup"><span data-stu-id="b34b8-109">Name the project **CreateDeviceIdentity** and name the solution **IoTHubGetStarted**.</span></span>
   
    ![Nový klasický desktopový projekt Visual C# pro systém Windows][10]
2. <span data-ttu-id="b34b8-111">V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt **CreateDeviceIdentity** a potom klikněte na tlačítko **Spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b34b8-111">In Solution Explorer, right-click the **CreateDeviceIdentity** project, and then click **Manage NuGet Packages**.</span></span>
3. <span data-ttu-id="b34b8-112">V okně **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte možnost **Instalovat**, nainstalujte balíček  **Microsoft.Azure.Devices** a přijměte podmínky používání.</span><span class="sxs-lookup"><span data-stu-id="b34b8-112">In the **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** to install the **Microsoft.Azure.Devices** package, and accept the terms of use.</span></span> <span data-ttu-id="b34b8-113">Tímto postupem se stáhne a nainstaluje [balíček NuGet sady SDK pro službu Azure IoT][lnk-nuget-service-sdk] a jeho závislosti a přidá se na něj odkaz.</span><span class="sxs-lookup"><span data-stu-id="b34b8-113">This procedure downloads, installs, and adds a reference to the [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>
   
    ![Okno Správce balíčků NuGet][11]
4. <span data-ttu-id="b34b8-115">Do horní části souboru **Program.cs** přidejte následující příkazy `using`:</span><span class="sxs-lookup"><span data-stu-id="b34b8-115">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;
5. <span data-ttu-id="b34b8-116">Do třídy **Program** přidejte následující pole.</span><span class="sxs-lookup"><span data-stu-id="b34b8-116">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="b34b8-117">Nahraďte hodnotu zástupného symbolu připojovacím řetězcem pro službu IoT Hub, kterou jste vytvořili v předchozí části.</span><span class="sxs-lookup"><span data-stu-id="b34b8-117">Replace the placeholder value with the IoT Hub connection string for the hub that you created in the previous section.</span></span>
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
6. <span data-ttu-id="b34b8-118">Přidejte následující metodu do třídy **Program**:</span><span class="sxs-lookup"><span data-stu-id="b34b8-118">Add the following method to the **Program** class:</span></span>
   
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
   
    <span data-ttu-id="b34b8-119">Tato metoda vytvoří identitu zařízení s ID **myFirstDevice**.</span><span class="sxs-lookup"><span data-stu-id="b34b8-119">This method creates a device identity with ID **myFirstDevice**.</span></span> <span data-ttu-id="b34b8-120">(Pokud toto ID zařízení již v registru identit existuje, kód jednoduše načte informace o stávajícím zařízení.) Aplikace pak zobrazí primární klíč pro danou identitu.</span><span class="sxs-lookup"><span data-stu-id="b34b8-120">(If that device ID already exists in the identity registry, the code simply retrieves the existing device information.) The app then displays the primary key for that identity.</span></span> <span data-ttu-id="b34b8-121">Tento klíč v aplikaci simulovaného zařízení slouží k připojení ke službě IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="b34b8-121">You use this key in the simulated device app to connect to your IoT hub.</span></span>
[!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

7. <span data-ttu-id="b34b8-122">Nakonec do metody **Main** přidejte následující řádky:</span><span class="sxs-lookup"><span data-stu-id="b34b8-122">Finally, add the following lines to the **Main** method:</span></span>
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();
8. <span data-ttu-id="b34b8-123">Spusťte aplikaci a poznamenejte si klíč zařízení.</span><span class="sxs-lookup"><span data-stu-id="b34b8-123">Run this application, and make a note of the device key.</span></span>
   
    ![Klíč zařízení generovaný aplikací][12]

> [!NOTE]
> <span data-ttu-id="b34b8-125">V registru identit služby IoT Hub se uchovávají pouze identity zařízení za účelem bezpečného přístupu ke službě IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="b34b8-125">The IoT Hub identity registry only stores device identities to enable secure access to the IoT hub.</span></span> <span data-ttu-id="b34b8-126">Ukládají se zde ID zařízení a jejich klíče, které slouží jako zabezpečené přihlašovací údaje, a příznak povoleno/zakázáno, s jehož pomocí můžete zakázat přístup k jednotlivým zařízením.</span><span class="sxs-lookup"><span data-stu-id="b34b8-126">It stores device IDs and keys to use as security credentials, and an enabled/disabled flag that you can use to disable access for an individual device.</span></span> <span data-ttu-id="b34b8-127">Pokud aplikace potřebuje pro zařízení ukládat další metadata, měla by používat úložiště pro konkrétní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b34b8-127">If your application needs to store other device-specific metadata, it should use an application-specific store.</span></span> <span data-ttu-id="b34b8-128">Další informace najdete v [Příručce pro vývojáře pro službu IoT Hub][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="b34b8-128">For more information, see [IoT Hub developer guide][lnk-devguide-identity].</span></span>
> 
> 

<!-- Images. -->
[10]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp1.png
[11]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp2.png
[12]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp3.png


<!-- Links -->
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
