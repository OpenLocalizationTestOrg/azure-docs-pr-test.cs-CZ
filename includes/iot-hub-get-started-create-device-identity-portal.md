## <a name="create-a-device-identity"></a>Vytvoření identity zařízení

V této části použijte hello [portál Azure] [ lnk-azure-portal] toocreate identitu zařízení v registru identit hello ve službě IoT hub. Pokud má záznam v registru identit hello se nemůže připojit zařízení tooIoT rozbočovače. Další informace najdete v části "Registr identit" hello hello [Příručka vývojáře pro službu IoT Hub][lnk-devguide-identity]. Hello **Explorer zařízení** v hello portál umožňuje vygenerujte jedinečné ID zařízení a klíč, můžete zařízení při připojení tooIoT rozbočovače použít tooidentify sám sebe. V ID zařízení se rozlišují malá a velká písmena.

1. Zajistěte, aby se přihlásíte toohello [portál Azure][lnk-azure-portal].

1. V hello vlevo, klikněte na **všechny prostředky** a najít prostředek centra IoT.

    ![Přejděte tooyour Iot hub][img-find-iothub]

1. Po otevření prostředku centra IoT klikněte na tlačítko hello **Explorer zařízení** nástroje a potom klikněte na **přidat** v horní části hello. Zadejte název hello nové zařízení, jako třeba **myDeviceId**a klikněte na tlačítko **Uložit**.

    ![Vytvoření identity zařízení na portálu][img-create-device]

   Tím se vytvoří novou identitu zařízení pro službu IoT hub.

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

1. V hello **Explorer zařízení**na seznam zařízení, klikněte na nově vytvořený hello zařízení a poznamenejte si hello **připojovací řetězec, primární klíč**. 

    ![Řetězec připojení zařízení][img-connection-string]

> [!NOTE]
> Hello registru identit služby IoT Hub ukládá jenom zařízení identity tooenable zabezpečený přístup toohello IoT hub. Ukládá ID a klíče toouse zařízení jako zabezpečovací pověření a příznak povoleno/zakázáno, které můžete toodisable přístup k jednotlivým zařízením. Pokud aplikace potřebuje toostore další metadata specifická pro zařízení, měla by používat úložiště pro konkrétní aplikaci. Další informace najdete v [Příručce pro vývojáře pro službu IoT Hub][lnk-devguide-identity].

<!-- Images. -->
[img-find-iothub]: ./media/iot-hub-get-started-create-device-identity-portal/find-iothub.png
[img-create-device]: ./media/iot-hub-get-started-create-device-identity-portal/create-identity-portal.png
[img-connection-string]: ./media/iot-hub-get-started-create-device-identity-portal/device-connection-string.png


<!-- Links -->
[lnk-azure-portal]: https://portal.azure.com
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md

