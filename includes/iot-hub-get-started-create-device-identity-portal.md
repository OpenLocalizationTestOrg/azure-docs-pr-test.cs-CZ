## <a name="create-a-device-identity"></a><span data-ttu-id="77b1f-101">Vytvoření identity zařízení</span><span class="sxs-lookup"><span data-stu-id="77b1f-101">Create a device identity</span></span>

<span data-ttu-id="77b1f-102">V této části použijte hello [portál Azure] [ lnk-azure-portal] toocreate identitu zařízení v registru identit hello ve službě IoT hub.</span><span class="sxs-lookup"><span data-stu-id="77b1f-102">In this section, you use hello [Azure portal][lnk-azure-portal] toocreate a device identity in hello identity registry in your IoT hub.</span></span> <span data-ttu-id="77b1f-103">Pokud má záznam v registru identit hello se nemůže připojit zařízení tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="77b1f-103">A device cannot connect tooIoT hub unless it has an entry in hello identity registry.</span></span> <span data-ttu-id="77b1f-104">Další informace najdete v části "Registr identit" hello hello [Příručka vývojáře pro službu IoT Hub][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="77b1f-104">For more information, see hello "Identity registry" section of hello [IoT Hub developer guide][lnk-devguide-identity].</span></span> <span data-ttu-id="77b1f-105">Hello **Explorer zařízení** v hello portál umožňuje vygenerujte jedinečné ID zařízení a klíč, můžete zařízení při připojení tooIoT rozbočovače použít tooidentify sám sebe.</span><span class="sxs-lookup"><span data-stu-id="77b1f-105">hello **Device Explorer** in hello portal helps you generate a unique device ID and key that your device can use tooidentify itself when it connects tooIoT Hub.</span></span> <span data-ttu-id="77b1f-106">V ID zařízení se rozlišují malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="77b1f-106">Device IDs are case sensitive.</span></span>

1. <span data-ttu-id="77b1f-107">Zajistěte, aby se přihlásíte toohello [portál Azure][lnk-azure-portal].</span><span class="sxs-lookup"><span data-stu-id="77b1f-107">Make sure you are signed in toohello [Azure portal][lnk-azure-portal].</span></span>

1. <span data-ttu-id="77b1f-108">V hello vlevo, klikněte na **všechny prostředky** a najít prostředek centra IoT.</span><span class="sxs-lookup"><span data-stu-id="77b1f-108">In hello Jumpbar, click **All resources** and find your IoT hub resource.</span></span>

    ![Přejděte tooyour Iot hub][img-find-iothub]

1. <span data-ttu-id="77b1f-110">Po otevření prostředku centra IoT klikněte na tlačítko hello **Explorer zařízení** nástroje a potom klikněte na **přidat** v horní části hello.</span><span class="sxs-lookup"><span data-stu-id="77b1f-110">When your IoT hub resource is opened, click hello **Device Explorer** tool, and then click **Add** at hello top.</span></span> <span data-ttu-id="77b1f-111">Zadejte název hello nové zařízení, jako třeba **myDeviceId**a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="77b1f-111">Provide hello name for your new device, such as **myDeviceId**, and click **Save**.</span></span>

    ![Vytvoření identity zařízení na portálu][img-create-device]

   <span data-ttu-id="77b1f-113">Tím se vytvoří novou identitu zařízení pro službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="77b1f-113">This creates a new device identity for your IoT hub.</span></span>

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

1. <span data-ttu-id="77b1f-114">V hello **Explorer zařízení**na seznam zařízení, klikněte na nově vytvořený hello zařízení a poznamenejte si hello **připojovací řetězec, primární klíč**.</span><span class="sxs-lookup"><span data-stu-id="77b1f-114">In hello **Device Explorer**'s device list, click hello newly created device and make note of hello **Connection string---primary key**.</span></span> 

    ![Řetězec připojení zařízení][img-connection-string]

> [!NOTE]
> <span data-ttu-id="77b1f-116">Hello registru identit služby IoT Hub ukládá jenom zařízení identity tooenable zabezpečený přístup toohello IoT hub.</span><span class="sxs-lookup"><span data-stu-id="77b1f-116">hello IoT Hub identity registry only stores device identities tooenable secure access toohello IoT hub.</span></span> <span data-ttu-id="77b1f-117">Ukládá ID a klíče toouse zařízení jako zabezpečovací pověření a příznak povoleno/zakázáno, které můžete toodisable přístup k jednotlivým zařízením.</span><span class="sxs-lookup"><span data-stu-id="77b1f-117">It stores device IDs and keys toouse as security credentials, and an enabled/disabled flag that you can use toodisable access for an individual device.</span></span> <span data-ttu-id="77b1f-118">Pokud aplikace potřebuje toostore další metadata specifická pro zařízení, měla by používat úložiště pro konkrétní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="77b1f-118">If your application needs toostore other device-specific metadata, it should use an application-specific store.</span></span> <span data-ttu-id="77b1f-119">Další informace najdete v [Příručce pro vývojáře pro službu IoT Hub][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="77b1f-119">For more information, see [IoT Hub developer guide][lnk-devguide-identity].</span></span>

<!-- Images. -->
[img-find-iothub]: ./media/iot-hub-get-started-create-device-identity-portal/find-iothub.png
[img-create-device]: ./media/iot-hub-get-started-create-device-identity-portal/create-identity-portal.png
[img-connection-string]: ./media/iot-hub-get-started-create-device-identity-portal/device-connection-string.png


<!-- Links -->
[lnk-azure-portal]: https://portal.azure.com
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md

