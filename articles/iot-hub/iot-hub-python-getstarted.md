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
# <a name="connect-your-simulated-device-tooyour-iot-hub-using-python"></a>Připojení simulovaného zařízení IoT hub tooyour používá Python
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Na konci hello tohoto kurzu budete mít dvě aplikace Python:

* **CreateDeviceIdentity.py**, který vytvoří identitu zařízení a přiřazený bezpečnostní klíč tooconnect aplikace simulovaného zařízení.
* **SimulatedDevice.py**, který připojí tooyour služby IoT hub s dříve vytvořenou identitou zařízení hello a pravidelně zasílá telemetrickou zprávu pomocí protokolu MQTT hello.

> [!NOTE]
> článek Hello [SDK služby Azure IoT] [ lnk-hub-sdks] poskytuje informace o hello SDK služby Azure IoT, které můžete toobuild toorun obě aplikace na zařízení a back end vašeho řešení.
> 
> 

toocomplete tohoto kurzu budete potřebovat hello následující:

* [Python 2.x nebo 3.x][lnk-python-download]. Ujistěte se, že toouse hello 32bitové nebo 64bitové instalace podle požadavků vašeho nastavení. Po zobrazení výzvy během instalace hello, ujistěte se, že tooadd – tooyour Python – proměnná prostředí specifické pro platformu. Pokud používáte Python 2.x, může být nutné příliš[nainstalovat nebo upgradovat *pip*, hello Python balíček systém správy][lnk-install-pip].
* Pokud používáte operační systém Windows, potom [distribuovatelného balíčku Visual C++] [ lnk-visual-c-redist] tooallow hello použití nativních knihoven DLL z Pythonu.
* [Node.js 4.0 nebo novější][lnk-node-download]. Ujistěte se, že toouse hello 32bitové nebo 64bitové instalace podle požadavků vašeho nastavení. Toto je potřebné tooinstall hello [nástroji Průzkumník centra IoT][lnk-iot-hub-explorer].
* Aktivní účet Azure. Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].

> [!NOTE]
> Hello *pip* balíčky pro `azure-iothub-service-client` a `azure-iothub-device-client` jsou nyní k dispozici pouze pro operační systém Windows. Linux nebo Mac OS, naleznete toohello Linux a Mac OS konkrétní části hello [Příprava vývojového prostředí pro jazyk Python] [ lnk-python-devbox] post.
> 

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Nyní jste vytvořili svůj IoT Hub. Použijte hello název hostitele služby IoT Hub a hello připojovací řetězec služby IoT Hub v hello zbytek tohoto kurzu.

> [!NOTE]
> Můžete také snadno vytvořit své služby IoT hub na příkazovém řádku pomocí hello Pythonu nebo Node.js na základě rozhraní příkazového řádku Azure. článek Hello [vytvoření služby IoT hub pomocí hello Azure CLI 2.0] [ lnk-azure-cli-hub] ukazuje tak hello toodo rychlé kroky. 
> 

## <a name="create-a-device-identity"></a>Vytvoření identity zařízení
V této části jsou uvedené kroky toocreate hello konzolovou aplikaci Python, která vytvoří identitu zařízení v registru identit hello služby IoT hub. Zařízení lze připojit pouze tooIoT rozbočovače, pokud má záznam v registru identit hello. Další informace najdete v tématu hello **registru identit** části hello [Příručka vývojáře pro službu IoT Hub][lnk-devguide-identity]. Při spuštění této konzolové aplikace vygeneruje jedinečné ID zařízení a klíč, že vaše zařízení použít tooidentify samotné při odešle zařízení cloud zprávy tooIoT rozbočovače.

1. Otevřete příkazový řádek a nainstalujte hello **SDK služby Azure IoT Hub pro jazyk Python**. Zavřete příkazový řádek hello po instalaci hello SDK.

    ```
    pip install azure-iothub-service-client
    ```

2. Vytvořte soubor Pythonu s názvem **CreateDeviceIdentity.py**. Otevře se v [Python editor/IDE zvoleného][lnk-python-ide-list], například hello výchozí [nečinnosti][lnk-idle].

3. Přidejte následující kód tooimport hello požadované moduly ze sady SDK služby hello hello:

    ```python
    import sys
    import iothub_service_client
    from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod
    from iothub_service_client import IoTHubDeviceStatus, IoTHubError
    ```
2. Přidejte následující kód, nahraďte zástupný symbol hello pro hello `[IoTHub Connection String]` s hello připojovací řetězec pro hello IoT hub, které jste vytvořili v předchozí části hello. Můžete použít libovolný název jako hello `DEVICE_ID`.
   
    ```python
    CONNECTION_STRING = "[IoTHub Connection String]"
    DEVICE_ID = "MyFirstPythonDevice"
    ```
   [!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

3. Přidat hello následující funkce tooprint některé informace o zařízení hello.

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
3. Přidejte následující funkce toocreate hello zařízení identifikace pomocí hello registru Správce hello. 

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
4. Nakonec přidejte hlavní funkce hello následujícím způsobem a uložte soubor hello.

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
5. Na příkazovém řádku hello spusťte hello **CreateDeviceIdentity.py** následujícím způsobem:

    ```python
    python CreateDeviceIdentity.py
    ```
6. Měli byste vidět hello získávání vytvořit simulované zařízení. Zapište hello **deviceId** a hello **primaryKey** tohoto zařízení. Budete potřebovat tyto hodnoty později při vytváření aplikace, která se připojuje tooIoT rozbočovače jako zařízení.

    ![Úspěšné vytvoření zařízení][1]

> [!NOTE]
> Hello registru identit služby IoT Hub ukládá jenom zařízení identity tooenable zabezpečený přístup toohello IoT hub. Ukládá ID a klíče toouse zařízení jako zabezpečovací přihlašovací údaje a příznak povoleno/zakázáno, které můžete toodisable přístup k jednotlivým zařízením. Pokud aplikace potřebuje toostore další metadata specifická pro zařízení, měla by používat úložiště pro konkrétní aplikaci. Další informace najdete v tématu hello [Příručka vývojáře pro službu IoT Hub][lnk-devguide-identity].
> 
> 


## <a name="create-a-simulated-device-app"></a>Vytvoření aplikace simulovaného zařízení
V této části jsou uvedené kroky toocreate hello konzolovou aplikaci Python, která simuluje zařízení a odešle zprávy typu zařízení cloud tooyour IoT hub.

1. Otevřete nový příkazový řádek a nainstalujte hello sady SDK zařízení Azure IoT Hub pro jazyk Python následujícím způsobem. Zavřete příkazový řádek hello po instalaci hello.

    ```
    pip install azure-iothub-device-client
    ```
2. Vytvořte soubor s názvem **SimulatedDevice.py**. Otevřete tento soubor v editoru Pythonu nebo integrovaném vývojovém prostředí (IDE) podle vašeho výběru (například IDLE).

3. Přidejte následující kód tooimport hello požadované moduly ze zařízení hello SDK hello.

    ```python
    import random
    import time
    import sys
    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult
    from iothub_client import IoTHubMessage, IoTHubMessageDispositionResult, IoTHubError, DeviceMethodReturnValue
    ```
4. Přidat hello následující kód a nahraďte zástupný symbol hello pro `[IoTHub Device Connection String]` s hello připojovací řetězec pro vaše zařízení. Hello zařízení připojovací řetězec je obvykle ve formátu hello `HostName=<hostName>;DeviceId=<deviceId>;SharedAccessKey=<primaryKey>`. Použití hello **deviceId** a **primaryKey** hello zařízení, které jste vytvořili v hello předchozí část tooreplace hello `<deviceId>` a `<primaryKey>` v uvedeném pořadí. Nahraďte `<hostName>` názvem hostitele služby IoT Hub, který je obvykle ve formátu `<IoT hub name>.azure-devices.net`.

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
5. Přidejte následující kód toodefine zpětné volání potvrzení odeslání hello. 

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
6. Přidejte následující kód tooinitialize hello zařízení klienta hello.

    ```python
    def iothub_client_init():
        # prepare iothub client
        client = IoTHubClient(CONNECTION_STRING, PROTOCOL)
        # set hello time until a message times out
        client.set_option("messageTimeout", MESSAGE_TIMEOUT)
        client.set_option("logtrace", 0)
        return client
    ```
7. Přidejte následující hello funkce tooformat a odeslání zprávy ze simulovaného zařízení služby IoT hub tooyour.

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
8. Nakonec přidejte hlavní funkce hello. 

    ```python
    if __name__ == '__main__':
        print ( "Simulating a device using hello Azure IoT Hub Device SDK for Python" )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_client_telemetry_sample_run()
    ```
9. Uložte a zavřete hello **SimulatedDevice.py** souboru. Můžete je nyní připraven toorun tuto aplikaci.

> [!NOTE]
> věcí tookeep jednoduchý, tento kurz neimplementuje žádné zásady opakování. V produkčním kódu, měli byste implementovat zásady opakování (například exponenciální zdvojnásobení) dle pokynů v článku na webu MSDN hello [přechodných chyb][lnk-transient-faults].
> 
> 

## <a name="receive-messages-from-your-simulated-device"></a>Příjem zpráv ze simulovaného zařízení
tooreceive telemetrických zpráv ze zařízení, je nutné toouse [Event Hubs][lnk-event-hubs-overview]-koncový bod kompatibilní vystavené hello IoT Hub, která čte zprávy typu zařízení cloud hello. Čtení hello [Začínáme se službou Event Hubs] [ lnk-eventhubs-tutorial] kurz informace o tom, jak tooprocess zpráv ze služby Event Hubs pro koncový bod kompatibilní s centrem událostí služby IoT hub. Služby Event Hubs nepodporuje telemetrie v Pythonu ještě, takže si můžete vytvořit buď [Node.js](iot-hub-node-node-getstarted.md#D2C_node) nebo [.NET](iot-hub-csharp-csharp-getstarted.md#D2C_csharp) zpráv typu zařízení cloud hello tooread aplikace Konzola služby Event Hubs ze služby IoT Hub. Tento kurz ukazuje, jak je možné používat hello [nástroji Průzkumník centra IoT] [ lnk-iot-hub-explorer] tooread zprávy těchto zařízení.

1. Otevřete příkazový řádek a nainstalujte hello Explorer centra IoT. 

    ```
    npm install -g iothub-explorer
    ```

2. Spusťte následující příkaz na příkazovém řádku hello hello, monitorování toobegin hello zpráv typu zařízení cloud ze zařízení. Použít připojovací řetězec služby IoT hub v zástupný symbol hello po `--login`.

    ```
    iothub-explorer monitor-events MyFirstPythonDevice --login "[IoTHub connection string]"
    ```

3. Otevřete nový příkazový řádek a přejděte toohello adresář obsahující hello **SimulatedDevice.py** souboru.

4. Spustit hello **SimulatedDevice.py** souboru, který pravidelně odesílá telemetrická data tooyour IoT hub. 
   
    ```
    python SimulatedDevice.py
    ```
5. Sledujte zprávy hello zařízení na spuštění hello IoT Hub Explorer z předchozí části hello hello příkazového řádku. 

    ![Zprávy typu zařízení-cloud v Pythonu][2]

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste nakonfigurovali novou službu IoT hub v hello portál Azure a poté jste vytvořili identitu zařízení v registru identit služby IoT hub hello. Použili jste toto zařízení identity tooenable hello simulované zařízení aplikaci toosend zpráv typu zařízení cloud toohello Centrum IoT. Jste zaznamenali hello zprávy přijaté službou IoT hub hello hello pomocí nástroje Průzkumník centra IoT hello. 

tooexplore hello Python SDK pro Azure IoT Hub využití podrobněji, navštivte [tohoto úložiště Git rozbočovače][lnk-python-github]. možnosti zasílání zpráv tooreview hello Dobrý den SDK služby Azure IoT Hub pro jazyk Python, můžete stáhnout a spustit [iothub_messaging_sample.py][lnk-messaging-sample]. Pro simulaci straně zařízení pomocí hello sady SDK zařízení Azure IoT Hub pro jazyk Python, můžete stáhnout a spustit hello [iothub_client_sample.py][lnk-client-sample].

toocontinue Začínáme se službou IoT Hub a tooexplore najdete v dalších scénářů platformy IoT:

* [Připojení zařízení][lnk-connect-device]
* [Začínáme se správou zařízení][lnk-device-management]
* [Začínáme se službou Azure IoT Edge][lnk-iot-edge]

toolearn jak tooextend zpráv IoT řešení a proces zařízení cloud ve velkém měřítku, najdete v části hello [zpracování zpráv typu zařízení cloud] [ lnk-process-d2c-tutorial] kurzu.
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
