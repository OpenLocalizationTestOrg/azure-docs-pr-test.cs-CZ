## <a name="create-a-device-identity"></a>Vytvoření identity zařízení
V této části vytvoříte konzolovou aplikaci .NET, která vytvoří identitu zařízení v registru identit hello ve službě IoT hub. Pokud má záznam v registru identit hello se nemůže připojit zařízení tooIoT rozbočovače. Další informace najdete v části "Registr identit" hello hello [Příručka vývojáře pro službu IoT Hub][lnk-devguide-identity]. Při spuštění této konzolové aplikace vygeneruje jedinečné ID zařízení a klíč, že vaše zařízení použít tooidentify samotné při odešle zařízení cloud zprávy tooIoT rozbočovače. V ID zařízení se rozlišují malá a velká písmena.

1. V sadě Visual Studio, přidejte nové řešení Visual C# Windows klasický desktopový projekt tooa pomocí hello **konzolovou aplikaci (rozhraní .NET Framework)** šablona projektu. Zkontrolujte, zda je hello verze rozhraní .NET Framework 4.5.1 nebo novější. Název projektu hello **CreateDeviceIdentity** a název hello řešení **IoTHubGetStarted**.
   
    ![Nový klasický desktopový projekt Visual C# pro systém Windows][10]
2. V Průzkumníku řešení klikněte pravým tlačítkem na hello **CreateDeviceIdentity** projektu a pak klikněte na **spravovat balíčky NuGet**.
3. V hello **Správce balíčků NuGet** vyberte **Procházet**, vyhledejte **microsoft.azure.devices**, vyberte **nainstalovat** tooinstall Hello **Microsoft.Azure.Devices** balíček a přijměte podmínky použití hello. Tento postup stáhne, nainstaluje a přidá odkaz toohello [sady SDK služby Azure IoT] [ lnk-nuget-service-sdk] NuGet balíček a jeho závislé součásti.
   
    ![Okno Správce balíčků NuGet][11]
4. Přidejte následující hello `using` příkazy hello horní části hello **Program.cs** souboru:
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;
5. Přidejte následující pole toohello hello **Program** třídy. Nahraďte hodnotu zástupného symbolu hello hello připojovací řetězec služby IoT Hub pro hello rozbočovače, který jste vytvořili v předchozí části hello.
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
6. Přidejte následující metodu toohello hello **Program** třídy:
   
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
   
    Tato metoda vytvoří identitu zařízení s ID **myFirstDevice**. (Pokud toto ID zařízení již existuje v registru identit hello, hello kód jednoduše načte hello stávající informace o zařízení.) Hello aplikace pak zobrazí hello primární klíč pro danou identitu. Tento klíč se používá hello simulované zařízení aplikaci tooconnect tooyour IoT hub.
[!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

7. Nakonec přidejte následující řádky toohello hello **hlavní** metoda:
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();
8. Tuto aplikaci spustit a poznamenejte si klíč zařízení hello.
   
    ![Klíč zařízení generovaný aplikací hello][12]

> [!NOTE]
> Hello registru identit služby IoT Hub ukládá jenom zařízení identity tooenable zabezpečený přístup toohello IoT hub. Ukládá ID a klíče toouse zařízení jako zabezpečovací pověření a příznak povoleno/zakázáno, které můžete toodisable přístup k jednotlivým zařízením. Pokud aplikace potřebuje toostore další metadata specifická pro zařízení, měla by používat úložiště pro konkrétní aplikaci. Další informace najdete v [Příručce pro vývojáře pro službu IoT Hub][lnk-devguide-identity].
> 
> 

<!-- Images. -->
[10]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp1.png
[11]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp2.png
[12]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp3.png


<!-- Links -->
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
