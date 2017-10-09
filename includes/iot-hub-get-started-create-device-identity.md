## <a name="create-a-device-identity"></a>Vytvoření identity zařízení

V této části můžete použít nástroj Node.js názvem [iothub-explorer] [ iot-hub-explorer] toocreate identitu zařízení v tomto kurzu. V ID zařízení se rozlišují malá a velká písmena.

1. V prostředí příkazového řádku spusťte následující hello:

    `npm install -g iothub-explorer@latest`

1. Potom spusťte následující příkaz toologin tooyour rozbočovače hello. SUBSTITUTE `{iot hub connection string}` s hello připojovací řetězec služby IoT Hub jste zkopírovali dříve:

    `iothub-explorer login "{iot hub connection string}"`

1. Nakonec vytvořte novou identitu zařízení s názvem `myDeviceId` příkazem hello:

    `iothub-explorer create myDeviceId --connection-string`

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

Poznamenejte si řetězec připojení zařízení hello z výsledku hello. Tento připojovací řetězec zařízení slouží hello zařízení aplikaci tooconnect tooyour IoT Hub jako zařízení.

![][img-identity]

Odkazovat příliš[Začínáme se službou IoT Hub] [ lnk-getstarted] tooprogrammatically vytvoření identity zařízení.

<!-- images and links -->
[img-identity]: media/iot-hub-get-started-create-device-identity/devidentity.png

[iot-hub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md

[lnk-getstarted]: ../articles/iot-hub/iot-hub-csharp-csharp-getstarted.md
