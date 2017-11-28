---
title: "aaaGet začít s Azure IoT Hub (Python) | Microsoft Docs"
description: "Zjistěte, jak toosend zařízení cloud zprávy tooAzure IoT Hub pro jazyk Python pomocí sady SDK služby IoT. Vytvoření simulovaného zařízení a služby aplikace tooregister zařízení, odesílání zpráv a čtení zpráv ze služby IoT hub."
services: iot-hub
author: dsk-2015
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: python
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: dkshir
ms.custom: na
ms.openlocfilehash: aa23e792fb144202e121274723bcfaeae0c04723
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-simulated-device-tooyour-iot-hub-using-python"></a><span data-ttu-id="1cccf-104">Připojení simulovaného zařízení IoT hub tooyour používá Python</span><span class="sxs-lookup"><span data-stu-id="1cccf-104">Connect your simulated device tooyour IoT hub using Python</span></span>
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

<span data-ttu-id="1cccf-105">Na konci hello tohoto kurzu budete mít dvě aplikace Python:</span><span class="sxs-lookup"><span data-stu-id="1cccf-105">At hello end of this tutorial, you will have two Python apps:</span></span>

* <span data-ttu-id="1cccf-106">**CreateDeviceIdentity.py**, který vytvoří identitu zařízení a přiřazený bezpečnostní klíč tooconnect aplikace simulovaného zařízení.</span><span class="sxs-lookup"><span data-stu-id="1cccf-106">**CreateDeviceIdentity.py**, which creates a device identity and associated security key tooconnect your simulated device app.</span></span>
* <span data-ttu-id="1cccf-107">**SimulatedDevice.py**, který připojí tooyour služby IoT hub s dříve vytvořenou identitou zařízení hello a pravidelně zasílá telemetrickou zprávu pomocí protokolu MQTT hello.</span><span class="sxs-lookup"><span data-stu-id="1cccf-107">**SimulatedDevice.py**, which connects tooyour IoT hub with hello device identity created earlier, and periodically sends a telemetry message using hello MQTT protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="1cccf-108">článek Hello [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o hello SDK služby Azure IoT, které můžete toobuild toorun obě aplikace na zařízení a back end vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="1cccf-108">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both applications toorun on devices and your solution back end.</span></span>
> 
> 

<span data-ttu-id="1cccf-109">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="1cccf-109">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="1cccf-110">[Python 2.x nebo 3.x][lnk-python-download].</span><span class="sxs-lookup"><span data-stu-id="1cccf-110">[Python 2.x or 3.x][lnk-python-download].</span></span> <span data-ttu-id="1cccf-111">Ujistěte se, že toouse hello 32bitové nebo 64bitové instalace podle požadavků vašeho nastavení.</span><span class="sxs-lookup"><span data-stu-id="1cccf-111">Make sure toouse hello 32-bit or 64-bit installation as required by your setup.</span></span> <span data-ttu-id="1cccf-112">Po zobrazení výzvy během instalace hello, ujistěte se, že tooadd – tooyour Python – proměnná prostředí specifické pro platformu.</span><span class="sxs-lookup"><span data-stu-id="1cccf-112">When prompted during hello installation, make sure tooadd Python tooyour platform-specific environment variable.</span></span> <span data-ttu-id="1cccf-113">Pokud používáte Python 2.x, může být nutné příliš[nainstalovat nebo upgradovat *pip*, hello Python balíček systém správy][lnk-install-pip].</span><span class="sxs-lookup"><span data-stu-id="1cccf-113">If you are using Python 2.x, you may need too[install or upgrade *pip*, hello Python package management system][lnk-install-pip].</span></span>
* <span data-ttu-id="1cccf-114">Pokud používáte operační systém Windows, potom [distribuovatelného balíčku Visual C++] [ lnk-visual-c-redist] tooallow hello použití nativních knihoven DLL z Pythonu.</span><span class="sxs-lookup"><span data-stu-id="1cccf-114">If you are using Windows OS, then [Visual C++ redistributable package][lnk-visual-c-redist] tooallow hello use of native DLLs from Python.</span></span>
* <span data-ttu-id="1cccf-115">[Node.js 4.0 nebo novější][lnk-node-download].</span><span class="sxs-lookup"><span data-stu-id="1cccf-115">[Node.js 4.0 or later][lnk-node-download].</span></span> <span data-ttu-id="1cccf-116">Ujistěte se, že toouse hello 32bitové nebo 64bitové instalace podle požadavků vašeho nastavení.</span><span class="sxs-lookup"><span data-stu-id="1cccf-116">Make sure toouse hello 32-bit or 64-bit installation as required by your setup.</span></span> <span data-ttu-id="1cccf-117">Toto je potřebné tooinstall hello [nástroji Průzkumník centra IoT][lnk-iot-hub-explorer].</span><span class="sxs-lookup"><span data-stu-id="1cccf-117">This is needed tooinstall hello [IoT Hub Explorer tool][lnk-iot-hub-explorer].</span></span>
* <span data-ttu-id="1cccf-118">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="1cccf-118">An active Azure account.</span></span> <span data-ttu-id="1cccf-119">Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].</span><span class="sxs-lookup"><span data-stu-id="1cccf-119">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>

> [!NOTE]
> <span data-ttu-id="1cccf-120">Hello *pip* balíčky pro `azure-iothub-service-client` a `azure-iothub-device-client` jsou nyní k dispozici pouze pro operační systém Windows.</span><span class="sxs-lookup"><span data-stu-id="1cccf-120">hello *pip* packages for `azure-iothub-service-client` and `azure-iothub-device-client` are currently available only for Windows OS.</span></span> <span data-ttu-id="1cccf-121">Linux nebo Mac OS, naleznete toohello Linux a Mac OS konkrétní části hello [Příprava vývojového prostředí pro jazyk Python] [ lnk-python-devbox] post.</span><span class="sxs-lookup"><span data-stu-id="1cccf-121">For Linux/Mac OS, please refer toohello Linux and Mac OS-specific sections on hello [Prepare your development environment for Python][lnk-python-devbox] post.</span></span>
> 

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

<span data-ttu-id="1cccf-122">Nyní jste vytvořili svůj IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="1cccf-122">You have now created your IoT hub.</span></span> <span data-ttu-id="1cccf-123">Použijte hello název hostitele služby IoT Hub a hello připojovací řetězec služby IoT Hub v hello zbytek tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="1cccf-123">Use hello IoT Hub host name and hello IoT Hub connection string in hello rest of this tutorial.</span></span>

> [!NOTE]
> <span data-ttu-id="1cccf-124">Můžete také snadno vytvořit své služby IoT hub na příkazovém řádku pomocí hello Pythonu nebo Node.js na základě rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="1cccf-124">You can also easily create your IoT hub on a command line, using hello Python or Node.js based Azure CLI.</span></span> <span data-ttu-id="1cccf-125">článek Hello [vytvoření služby IoT hub pomocí hello Azure CLI 2.0] [ lnk-azure-cli-hub] ukazuje tak hello toodo rychlé kroky.</span><span class="sxs-lookup"><span data-stu-id="1cccf-125">hello article [Create an IoT hub using hello Azure CLI 2.0][lnk-azure-cli-hub] shows you hello quick steps toodo so.</span></span> 
> 

## <a name="create-a-device-identity"></a><span data-ttu-id="1cccf-126">Vytvoření identity zařízení</span><span class="sxs-lookup"><span data-stu-id="1cccf-126">Create a device identity</span></span>
<span data-ttu-id="1cccf-127">V této části jsou uvedené kroky toocreate hello konzolovou aplikaci Python, která vytvoří identitu zařízení v registru identit hello služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="1cccf-127">This section lists hello steps toocreate a Python console app, that creates a device identity in hello identity registry of your IoT hub.</span></span> <span data-ttu-id="1cccf-128">Zařízení lze připojit pouze tooIoT rozbočovače, pokud má záznam v registru identit hello.</span><span class="sxs-lookup"><span data-stu-id="1cccf-128">A device can only connect tooIoT Hub if it has an entry in hello identity registry.</span></span> <span data-ttu-id="1cccf-129">Další informace najdete v tématu hello **registru identit** části hello [Příručka vývojáře pro službu IoT Hub][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="1cccf-129">For more information, see hello **Identity Registry** section of hello [IoT Hub developer guide][lnk-devguide-identity].</span></span> <span data-ttu-id="1cccf-130">Při spuštění této konzolové aplikace vygeneruje jedinečné ID zařízení a klíč, že vaše zařízení použít tooidentify samotné při odešle zařízení cloud zprávy tooIoT rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="1cccf-130">When you run this console app, it generates a unique device ID and key that your device can use tooidentify itself when it sends device-to-cloud messages tooIoT Hub.</span></span>

1. <span data-ttu-id="1cccf-131">Otevřete příkazový řádek a nainstalujte hello **SDK služby Azure IoT Hub pro jazyk Python**.</span><span class="sxs-lookup"><span data-stu-id="1cccf-131">Open a command prompt and install hello **Azure IoT Hub Service SDK for Python**.</span></span> <span data-ttu-id="1cccf-132">Zavřete příkazový řádek hello po instalaci hello SDK.</span><span class="sxs-lookup"><span data-stu-id="1cccf-132">Close hello command prompt after you install hello SDK.</span></span>

    ```
    pip install azure-iothub-service-client
    ```

2. <span data-ttu-id="1cccf-133">Vytvořte soubor Pythonu s názvem **CreateDeviceIdentity.py**.</span><span class="sxs-lookup"><span data-stu-id="1cccf-133">Create a Python file named **CreateDeviceIdentity.py**.</span></span> <span data-ttu-id="1cccf-134">Otevře se v [Python editor/IDE zvoleného][lnk-python-ide-list], například hello výchozí [nečinnosti][lnk-idle].</span><span class="sxs-lookup"><span data-stu-id="1cccf-134">Open it in [a Python editor/IDE of your choice][lnk-python-ide-list], for example, hello default [IDLE][lnk-idle].</span></span>

3. <span data-ttu-id="1cccf-135">Přidejte následující kód tooimport hello požadované moduly ze sady SDK služby hello hello:</span><span class="sxs-lookup"><span data-stu-id="1cccf-135">Add hello following code tooimport hello required modules from hello service SDK:</span></span>

    ```python
    import sys
    import iothub_service_client
    from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod
    from iothub_service_client import IoTHubDeviceStatus, IoTHubError
    ```
2. <span data-ttu-id="1cccf-136">Přidejte následující kód, nahraďte zástupný symbol hello pro hello `[IoTHub Connection String]` s hello připojovací řetězec pro hello IoT hub, které jste vytvořili v předchozí části hello.</span><span class="sxs-lookup"><span data-stu-id="1cccf-136">Add hello following code, replacing hello placeholder for `[IoTHub Connection String]` with hello connection string for hello IoT hub you created in hello previous section.</span></span> <span data-ttu-id="1cccf-137">Můžete použít libovolný název jako hello `DEVICE_ID`.</span><span class="sxs-lookup"><span data-stu-id="1cccf-137">You can use any name as hello `DEVICE_ID`.</span></span>
   
    ```python
    CONNECTION_STRING = "[IoTHub Connection String]"
    DEVICE_ID = "MyFirstPythonDevice"
    ```
   [!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

3. <span data-ttu-id="1cccf-138">Přidat hello následující funkce tooprint některé informace o zařízení hello.</span><span class="sxs-lookup"><span data-stu-id="1cccf-138">Add hello following function tooprint some of hello device information.</span></span>

    ```python
    def print_device_info(title, iothub_device):
        print ( title + ":" )
        print ( "iothubDevice.deviceId                    = {0}".format(iothub_device.deviceId) )
        print ( "iothubDevice.primaryKey                  = {0}".format(iothub_device.primaryKey) )
        print ( "iothubDevice.secondaryKey                = {0}".format(iothub_device.secondaryKey) )
        print ( "iothubDevice.connectionState             = {0}".format(iothub_device.connectionState) )
        print ( "iothubDevice.status                      = {0}".format(iothub_device.status) )
        print ( "iothubDevice.lastActivityTime            = {0}".format(iothub_device.lastActivityTime) )
        print ( "iothubDevice.cloudToDeviceMessageCount   = {0}".format(iothub_device.cloudToDeviceMessageCount) )
        print ( "iothubDevice.isManaged                   = {0}".format(iothub_device.isManaged) )
        print ( "iothubDevice.authMethod                  = {0}".format(iothub_device.authMethod) )
        print ( "" )
    ```
3. <span data-ttu-id="1cccf-139">Přidejte následující funkce toocreate hello zařízení identifikace pomocí hello registru Správce hello.</span><span class="sxs-lookup"><span data-stu-id="1cccf-139">Add hello following function toocreate hello device identification using hello Registry Manager.</span></span> 

    ```python
    def iothub_createdevice():
        try:
            iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)
            auth_method = IoTHubRegistryManagerAuthMethod.SHARED_PRIVATE_KEY
            new_device = iothub_registry_manager.create_device(DEVICE_ID, "", "", auth_method)
            print_device_info("CreateDevice", new_device)

        except IoTHubError as iothub_error:
            print ( "Unexpected error {0}".format(iothub_error) )
            return
        except KeyboardInterrupt:
            print ( "iothub_createdevice stopped" )
    ```
4. <span data-ttu-id="1cccf-140">Nakonec přidejte hlavní funkce hello následujícím způsobem a uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="1cccf-140">Finally, add hello main function as follows and save hello file.</span></span>

    ```python
    if __name__ == '__main__':
        print ( "" )
        print ( "Python {0}".format(sys.version) )
        print ( "Creating device using hello Azure IoT Hub Service SDK for Python" )
        print ( "" )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_createdevice()
    ```
5. <span data-ttu-id="1cccf-141">Na příkazovém řádku hello spusťte hello **CreateDeviceIdentity.py** následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="1cccf-141">On hello command prompt, run hello **CreateDeviceIdentity.py** as follows:</span></span>

    ```python
    python CreateDeviceIdentity.py
    ```
6. <span data-ttu-id="1cccf-142">Měli byste vidět hello získávání vytvořit simulované zařízení.</span><span class="sxs-lookup"><span data-stu-id="1cccf-142">You should see hello simulated device getting created.</span></span> <span data-ttu-id="1cccf-143">Zapište hello **deviceId** a hello **primaryKey** tohoto zařízení.</span><span class="sxs-lookup"><span data-stu-id="1cccf-143">Note down hello **deviceId** and hello **primaryKey** of this device.</span></span> <span data-ttu-id="1cccf-144">Budete potřebovat tyto hodnoty později při vytváření aplikace, která se připojuje tooIoT rozbočovače jako zařízení.</span><span class="sxs-lookup"><span data-stu-id="1cccf-144">You need these values later when you create an application that connects tooIoT Hub as a device.</span></span>

    ![Úspěšné vytvoření zařízení][1]

> [!NOTE]
> <span data-ttu-id="1cccf-146">Hello registru identit služby IoT Hub ukládá jenom zařízení identity tooenable zabezpečený přístup toohello IoT hub.</span><span class="sxs-lookup"><span data-stu-id="1cccf-146">hello IoT Hub identity registry only stores device identities tooenable secure access toohello IoT hub.</span></span> <span data-ttu-id="1cccf-147">Ukládá ID a klíče toouse zařízení jako zabezpečovací přihlašovací údaje a příznak povoleno/zakázáno, které můžete toodisable přístup k jednotlivým zařízením.</span><span class="sxs-lookup"><span data-stu-id="1cccf-147">It stores device IDs and keys toouse as security credentials and an enabled/disabled flag that you can use toodisable access for an individual device.</span></span> <span data-ttu-id="1cccf-148">Pokud aplikace potřebuje toostore další metadata specifická pro zařízení, měla by používat úložiště pro konkrétní aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1cccf-148">If your application needs toostore other device-specific metadata, it should use an application-specific store.</span></span> <span data-ttu-id="1cccf-149">Další informace najdete v tématu hello [Příručka vývojáře pro službu IoT Hub][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="1cccf-149">For more information, see hello [IoT Hub developer guide][lnk-devguide-identity].</span></span>
> 
> 


## <a name="create-a-simulated-device-app"></a><span data-ttu-id="1cccf-150">Vytvoření aplikace simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="1cccf-150">Create a simulated device app</span></span>
<span data-ttu-id="1cccf-151">V této části jsou uvedené kroky toocreate hello konzolovou aplikaci Python, která simuluje zařízení a odešle zprávy typu zařízení cloud tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="1cccf-151">This section lists hello steps toocreate a Python console app, that simulates a device and sends device-to-cloud messages tooyour IoT hub.</span></span>

1. <span data-ttu-id="1cccf-152">Otevřete nový příkazový řádek a nainstalujte hello sady SDK zařízení Azure IoT Hub pro jazyk Python následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="1cccf-152">Open a new command prompt and install hello Azure IoT Hub Device SDK for Python as follows.</span></span> <span data-ttu-id="1cccf-153">Zavřete příkazový řádek hello po instalaci hello.</span><span class="sxs-lookup"><span data-stu-id="1cccf-153">Close hello command prompt after hello installation.</span></span>

    ```
    pip install azure-iothub-device-client
    ```
2. <span data-ttu-id="1cccf-154">Vytvořte soubor s názvem **SimulatedDevice.py**.</span><span class="sxs-lookup"><span data-stu-id="1cccf-154">Create a file named **SimulatedDevice.py**.</span></span> <span data-ttu-id="1cccf-155">Otevřete tento soubor v editoru Pythonu nebo integrovaném vývojovém prostředí (IDE) podle vašeho výběru (například IDLE).</span><span class="sxs-lookup"><span data-stu-id="1cccf-155">Open this file in a Python editor/IDE of your choice (for example, IDLE).</span></span>

3. <span data-ttu-id="1cccf-156">Přidejte následující kód tooimport hello požadované moduly ze zařízení hello SDK hello.</span><span class="sxs-lookup"><span data-stu-id="1cccf-156">Add hello following code tooimport hello required modules from hello device SDK.</span></span>

    ```python
    import random
    import time
    import sys
    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult
    from iothub_client import IoTHubMessage, IoTHubMessageDispositionResult, IoTHubError, DeviceMethodReturnValue
    ```
4. <span data-ttu-id="1cccf-157">Přidat hello následující kód a nahraďte zástupný symbol hello pro `[IoTHub Device Connection String]` s hello připojovací řetězec pro vaše zařízení.</span><span class="sxs-lookup"><span data-stu-id="1cccf-157">Add hello following code and replace hello placeholder for `[IoTHub Device Connection String]` with hello connection string for your device.</span></span> <span data-ttu-id="1cccf-158">Hello zařízení připojovací řetězec je obvykle ve formátu hello `HostName=<hostName>;DeviceId=<deviceId>;SharedAccessKey=<primaryKey>`.</span><span class="sxs-lookup"><span data-stu-id="1cccf-158">hello device connection string is usually in hello format of `HostName=<hostName>;DeviceId=<deviceId>;SharedAccessKey=<primaryKey>`.</span></span> <span data-ttu-id="1cccf-159">Použití hello **deviceId** a **primaryKey** hello zařízení, které jste vytvořili v hello předchozí část tooreplace hello `<deviceId>` a `<primaryKey>` v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="1cccf-159">Use hello **deviceId** and **primaryKey** of hello device you created in hello previous section tooreplace hello `<deviceId>` and `<primaryKey>` respectively.</span></span> <span data-ttu-id="1cccf-160">Nahraďte `<hostName>` názvem hostitele služby IoT Hub, který je obvykle ve formátu `<IoT hub name>.azure-devices.net`.</span><span class="sxs-lookup"><span data-stu-id="1cccf-160">Replace `<hostName>` with your IoT hub's host name, usually as `<IoT hub name>.azure-devices.net`.</span></span>

    ```python
    # String containing Hostname, Device Id & Device Key in hello format
    CONNECTION_STRING = "[IoTHub Device Connection String]"
    # choose HTTP, AMQP or MQTT as transport protocol
    PROTOCOL = IoTHubTransportProvider.MQTT
    MESSAGE_TIMEOUT = 10000
    AVG_WIND_SPEED = 10.0
    SEND_CALLBACKS = 0
    MSG_TXT = "{\"deviceId\": \"MyFirstPythonDevice\",\"windSpeed\": %.2f}"    
    ```
5. <span data-ttu-id="1cccf-161">Přidejte následující kód toodefine zpětné volání potvrzení odeslání hello.</span><span class="sxs-lookup"><span data-stu-id="1cccf-161">Add hello following code toodefine a send confirmation callback.</span></span> 

    ```python
    def send_confirmation_callback(message, result, user_context):
        global SEND_CALLBACKS
        print ( "Confirmation[%d] received for message with result = %s" % (user_context, result) )
        map_properties = message.properties()
        print ( "    message_id: %s" % message.message_id )
        print ( "    correlation_id: %s" % message.correlation_id )
        key_value_pair = map_properties.get_internals()
        print ( "    Properties: %s" % key_value_pair )
        SEND_CALLBACKS += 1
        print ( "    Total calls confirmed: %d" % SEND_CALLBACKS )
    ```
6. <span data-ttu-id="1cccf-162">Přidejte následující kód tooinitialize hello zařízení klienta hello.</span><span class="sxs-lookup"><span data-stu-id="1cccf-162">Add hello following code tooinitialize hello device client.</span></span>

    ```python
    def iothub_client_init():
        # prepare iothub client
        client = IoTHubClient(CONNECTION_STRING, PROTOCOL)
        # set hello time until a message times out
        client.set_option("messageTimeout", MESSAGE_TIMEOUT)
        client.set_option("logtrace", 0)
        return client
    ```
7. <span data-ttu-id="1cccf-163">Přidejte následující hello funkce tooformat a odeslání zprávy ze simulovaného zařízení služby IoT hub tooyour.</span><span class="sxs-lookup"><span data-stu-id="1cccf-163">Add hello following function tooformat and send a message from your simulated device tooyour IoT hub.</span></span>

    ```python
    def iothub_client_telemetry_sample_run():

        try:
            client = iothub_client_init()
            print ( "IoT Hub device sending periodic messages, press Ctrl-C tooexit" )
            message_counter = 0

            while True:
                msg_txt_formatted = MSG_TXT % (AVG_WIND_SPEED + (random.random() * 4 + 2))
                # messages can be encoded as string or bytearray
                if (message_counter & 1) == 1:
                    message = IoTHubMessage(bytearray(msg_txt_formatted, 'utf8'))
                else:
                    message = IoTHubMessage(msg_txt_formatted)
                # optional: assign ids
                message.message_id = "message_%d" % message_counter
                message.correlation_id = "correlation_%d" % message_counter
                # optional: assign properties
                prop_map = message.properties()
                prop_text = "PropMsg_%d" % message_counter
                prop_map.add("Property", prop_text)

                client.send_event_async(message, send_confirmation_callback, message_counter)
                print ( "IoTHubClient.send_event_async accepted message [%d] for transmission tooIoT Hub." % message_counter )

                status = client.get_send_status()
                print ( "Send status: %s" % status )
                time.sleep(30)

                status = client.get_send_status()
                print ( "Send status: %s" % status )

                message_counter += 1

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )
    ```
8. <span data-ttu-id="1cccf-164">Nakonec přidejte hlavní funkce hello.</span><span class="sxs-lookup"><span data-stu-id="1cccf-164">Finally, add hello main function.</span></span> 

    ```python
    if __name__ == '__main__':
        print ( "Simulating a device using hello Azure IoT Hub Device SDK for Python" )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_client_telemetry_sample_run()
    ```
9. <span data-ttu-id="1cccf-165">Uložte a zavřete hello **SimulatedDevice.py** souboru.</span><span class="sxs-lookup"><span data-stu-id="1cccf-165">Save and close hello **SimulatedDevice.py** file.</span></span> <span data-ttu-id="1cccf-166">Můžete je nyní připraven toorun tuto aplikaci.</span><span class="sxs-lookup"><span data-stu-id="1cccf-166">You are now ready toorun this app.</span></span>

> [!NOTE]
> <span data-ttu-id="1cccf-167">věcí tookeep jednoduchý, tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="1cccf-167">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="1cccf-168">V produkčním kódu, měli byste implementovat zásady opakování (například exponenciální zdvojnásobení) dle pokynů v článku na webu MSDN hello [přechodných chyb][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="1cccf-168">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>
> 
> 

## <a name="receive-messages-from-your-simulated-device"></a><span data-ttu-id="1cccf-169">Příjem zpráv ze simulovaného zařízení</span><span class="sxs-lookup"><span data-stu-id="1cccf-169">Receive messages from your simulated device</span></span>
<span data-ttu-id="1cccf-170">tooreceive telemetrických zpráv ze zařízení, je nutné toouse [Event Hubs][lnk-event-hubs-overview]-koncový bod kompatibilní vystavené hello IoT Hub, která čte zprávy typu zařízení cloud hello.</span><span class="sxs-lookup"><span data-stu-id="1cccf-170">tooreceive telemetry messages from your device, you need toouse an [Event Hubs][lnk-event-hubs-overview]-compatible endpoint exposed by hello IoT Hub, which reads hello device-to-cloud messages.</span></span> <span data-ttu-id="1cccf-171">Čtení hello [Začínáme se službou Event Hubs] [ lnk-eventhubs-tutorial] kurz informace o tom, jak tooprocess zpráv ze služby Event Hubs pro koncový bod kompatibilní s centrem událostí služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="1cccf-171">Read hello [Get Started with Event Hubs][lnk-eventhubs-tutorial] tutorial for information on how tooprocess messages from Event Hubs for your IoT hub's Event Hub-compatible endpoint.</span></span> <span data-ttu-id="1cccf-172">Služby Event Hubs nepodporuje telemetrie v Pythonu ještě, takže si můžete vytvořit buď [Node.js](iot-hub-node-node-getstarted.md#D2C_node) nebo [.NET](iot-hub-csharp-csharp-getstarted.md#D2C_csharp) zpráv typu zařízení cloud hello tooread aplikace Konzola služby Event Hubs ze služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="1cccf-172">Event Hubs does not support telemetry in Python yet, so you can either create a [Node.js](iot-hub-node-node-getstarted.md#D2C_node) or a [.NET](iot-hub-csharp-csharp-getstarted.md#D2C_csharp) Event Hubs-based console app tooread hello device-to-cloud messages from IoT Hub.</span></span> <span data-ttu-id="1cccf-173">Tento kurz ukazuje, jak je možné používat hello [nástroji Průzkumník centra IoT] [ lnk-iot-hub-explorer] tooread zprávy těchto zařízení.</span><span class="sxs-lookup"><span data-stu-id="1cccf-173">This tutorial shows how you can use hello [IoT Hub Explorer tool][lnk-iot-hub-explorer] tooread these device messages.</span></span>

1. <span data-ttu-id="1cccf-174">Otevřete příkazový řádek a nainstalujte hello Explorer centra IoT.</span><span class="sxs-lookup"><span data-stu-id="1cccf-174">Open a command prompt and install hello IoT Hub Explorer.</span></span> 

    ```
    npm install -g iothub-explorer
    ```

2. <span data-ttu-id="1cccf-175">Spusťte následující příkaz na příkazovém řádku hello hello, monitorování toobegin hello zpráv typu zařízení cloud ze zařízení.</span><span class="sxs-lookup"><span data-stu-id="1cccf-175">Run hello following command on hello command prompt, toobegin monitoring hello device-to-cloud messages from your device.</span></span> <span data-ttu-id="1cccf-176">Použít připojovací řetězec služby IoT hub v zástupný symbol hello po `--login`.</span><span class="sxs-lookup"><span data-stu-id="1cccf-176">Use your IoT hub's connection string in hello placeholder after `--login`.</span></span>

    ```
    iothub-explorer monitor-events MyFirstPythonDevice --login "[IoTHub connection string]"
    ```

3. <span data-ttu-id="1cccf-177">Otevřete nový příkazový řádek a přejděte toohello adresář obsahující hello **SimulatedDevice.py** souboru.</span><span class="sxs-lookup"><span data-stu-id="1cccf-177">Open a new command prompt and navigate toohello directory containing hello **SimulatedDevice.py** file.</span></span>

4. <span data-ttu-id="1cccf-178">Spustit hello **SimulatedDevice.py** souboru, který pravidelně odesílá telemetrická data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="1cccf-178">Run hello **SimulatedDevice.py** file, which periodically sends telemetry data tooyour IoT hub.</span></span> 
   
    ```
    python SimulatedDevice.py
    ```
5. <span data-ttu-id="1cccf-179">Sledujte zprávy hello zařízení na spuštění hello IoT Hub Explorer z předchozí části hello hello příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="1cccf-179">Observe hello device messages on hello command prompt running hello IoT Hub Explorer from hello previous section.</span></span> 

    ![Zprávy typu zařízení-cloud v Pythonu][2]

## <a name="next-steps"></a><span data-ttu-id="1cccf-181">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1cccf-181">Next steps</span></span>
<span data-ttu-id="1cccf-182">V tomto kurzu jste nakonfigurovali novou službu IoT hub v hello portál Azure a poté jste vytvořili identitu zařízení v registru identit služby IoT hub hello.</span><span class="sxs-lookup"><span data-stu-id="1cccf-182">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="1cccf-183">Použili jste toto zařízení identity tooenable hello simulované zařízení aplikaci toosend zpráv typu zařízení cloud toohello Centrum IoT.</span><span class="sxs-lookup"><span data-stu-id="1cccf-183">You used this device identity tooenable hello simulated device app toosend device-to-cloud messages toohello IoT hub.</span></span> <span data-ttu-id="1cccf-184">Jste zaznamenali hello zprávy přijaté službou IoT hub hello hello pomocí nástroje Průzkumník centra IoT hello.</span><span class="sxs-lookup"><span data-stu-id="1cccf-184">You observed hello messages received by hello IoT hub with hello help of hello IoT Hub Explorer tool.</span></span> 

<span data-ttu-id="1cccf-185">tooexplore hello Python SDK pro Azure IoT Hub využití podrobněji, navštivte [tohoto úložiště Git rozbočovače][lnk-python-github].</span><span class="sxs-lookup"><span data-stu-id="1cccf-185">tooexplore hello Python SDK for Azure IoT Hub usage in depth, visit [this Git Hub repo][lnk-python-github].</span></span> <span data-ttu-id="1cccf-186">možnosti zasílání zpráv tooreview hello Dobrý den SDK služby Azure IoT Hub pro jazyk Python, můžete stáhnout a spustit [iothub_messaging_sample.py][lnk-messaging-sample].</span><span class="sxs-lookup"><span data-stu-id="1cccf-186">tooreview hello messaging capabilities of hello Azure IoT Hub Service SDK for Python, you can download and run [iothub_messaging_sample.py][lnk-messaging-sample].</span></span> <span data-ttu-id="1cccf-187">Pro simulaci straně zařízení pomocí hello sady SDK zařízení Azure IoT Hub pro jazyk Python, můžete stáhnout a spustit hello [iothub_client_sample.py][lnk-client-sample].</span><span class="sxs-lookup"><span data-stu-id="1cccf-187">For device side simulation using hello Azure IoT Hub Device SDK for Python, you can download and run hello [iothub_client_sample.py][lnk-client-sample].</span></span>

<span data-ttu-id="1cccf-188">toocontinue Začínáme se službou IoT Hub a tooexplore najdete v dalších scénářů platformy IoT:</span><span class="sxs-lookup"><span data-stu-id="1cccf-188">toocontinue getting started with IoT Hub and tooexplore other IoT scenarios, see:</span></span>

* <span data-ttu-id="1cccf-189">[Připojení zařízení][lnk-connect-device]</span><span class="sxs-lookup"><span data-stu-id="1cccf-189">[Connecting your device][lnk-connect-device]</span></span>
* <span data-ttu-id="1cccf-190">[Začínáme se správou zařízení][lnk-device-management]</span><span class="sxs-lookup"><span data-stu-id="1cccf-190">[Getting started with device management][lnk-device-management]</span></span>
* <span data-ttu-id="1cccf-191">[Začínáme se službou Azure IoT Edge][lnk-iot-edge]</span><span class="sxs-lookup"><span data-stu-id="1cccf-191">[Getting started with Azure IoT Edge][lnk-iot-edge]</span></span>

<span data-ttu-id="1cccf-192">toolearn jak tooextend zpráv IoT řešení a proces zařízení cloud ve velkém měřítku, najdete v části hello [zpracování zpráv typu zařízení cloud] [ lnk-process-d2c-tutorial] kurzu.</span><span class="sxs-lookup"><span data-stu-id="1cccf-192">toolearn how tooextend your IoT solution and process device-to-cloud messages at scale, see hello [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span>
[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!-- Images. -->
[1]: ./media/iot-hub-python-getstarted/createdevice.png
[2]: ./media/iot-hub-python-getstarted/sendd2cmessage.png

<!-- Links -->
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-node-download]: https://nodejs.org/en/download/
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
[lnk-azure-cli-hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-create-using-cli
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-idle]: https://docs.python.org/3/library/idle.html
[lnk-python-ide-list]: https://wiki.python.org/moin/IntegratedDevelopmentEnvironments
[lnk-iot-hub-explorer]: https://github.com/Azure/iothub-explorer
[lnk-python-github]: https://github.com/Azure/azure-iot-sdk-python
[lnk-messaging-sample]: https://github.com/Azure/azure-iot-sdk-python/blob/master/service/samples/iothub_messaging_sample.py
[lnk-client-sample]: https://github.com/Azure/azure-iot-sdk-python/blob/master/device/samples/iothub_client_sample.py

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md
[lnk-python-devbox]: https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md

[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
