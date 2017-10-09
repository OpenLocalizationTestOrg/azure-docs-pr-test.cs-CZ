## <a name="create-a-device-identity"></a><span data-ttu-id="044b7-101">Vytvoření identity zařízení</span><span class="sxs-lookup"><span data-stu-id="044b7-101">Create a device identity</span></span>

<span data-ttu-id="044b7-102">V této části můžete použít nástroj Node.js názvem [iothub-explorer] [ iot-hub-explorer] toocreate identitu zařízení v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="044b7-102">In this section, you use a Node.js tool called [iothub-explorer][iot-hub-explorer] toocreate a device identity for this tutorial.</span></span> <span data-ttu-id="044b7-103">V ID zařízení se rozlišují malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="044b7-103">Device IDs are case sensitive.</span></span>

1. <span data-ttu-id="044b7-104">V prostředí příkazového řádku spusťte následující hello:</span><span class="sxs-lookup"><span data-stu-id="044b7-104">Run hello following in your command-line environment:</span></span>

    `npm install -g iothub-explorer@latest`

1. <span data-ttu-id="044b7-105">Potom spusťte následující příkaz toologin tooyour rozbočovače hello.</span><span class="sxs-lookup"><span data-stu-id="044b7-105">Then, run hello following command toologin tooyour hub.</span></span> <span data-ttu-id="044b7-106">SUBSTITUTE `{iot hub connection string}` s hello připojovací řetězec služby IoT Hub jste zkopírovali dříve:</span><span class="sxs-lookup"><span data-stu-id="044b7-106">Substitute `{iot hub connection string}` with hello IoT Hub connection string you previously copied:</span></span>

    `iothub-explorer login "{iot hub connection string}"`

1. <span data-ttu-id="044b7-107">Nakonec vytvořte novou identitu zařízení s názvem `myDeviceId` příkazem hello:</span><span class="sxs-lookup"><span data-stu-id="044b7-107">Finally, create a new device identity called `myDeviceId` with hello command:</span></span>

    `iothub-explorer create myDeviceId --connection-string`

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

<span data-ttu-id="044b7-108">Poznamenejte si řetězec připojení zařízení hello z výsledku hello.</span><span class="sxs-lookup"><span data-stu-id="044b7-108">Make a note of hello device connection string from hello result.</span></span> <span data-ttu-id="044b7-109">Tento připojovací řetězec zařízení slouží hello zařízení aplikaci tooconnect tooyour IoT Hub jako zařízení.</span><span class="sxs-lookup"><span data-stu-id="044b7-109">This device connection string is used by hello device app tooconnect tooyour IoT Hub as a device.</span></span>

![][img-identity]

<span data-ttu-id="044b7-110">Odkazovat příliš[Začínáme se službou IoT Hub] [ lnk-getstarted] tooprogrammatically vytvoření identity zařízení.</span><span class="sxs-lookup"><span data-stu-id="044b7-110">Refer too[Getting started with IoT Hub][lnk-getstarted] tooprogrammatically create device identities.</span></span>

<!-- images and links -->
[img-identity]: media/iot-hub-get-started-create-device-identity/devidentity.png

[iot-hub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md

[lnk-getstarted]: ../articles/iot-hub/iot-hub-csharp-csharp-getstarted.md
