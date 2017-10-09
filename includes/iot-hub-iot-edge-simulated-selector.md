> [!div class="op_single_selector"]
> * [Linux](../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md)
> * [Windows](../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md)

Tento návod hello [simulované zařízení cloudu nahrát ukázková] ukazuje, jak toouse [Azure IoT Edge] [ lnk-sdk] toosend telemetrie zařízení cloud tooIoT rozbočovače ze simulovaného zařízení.

Tento návod ilustruje:

* **Architektura**: architektury informace o hello [simulované zařízení cloudu nahrát ukázková].
* **Sestavení a spuštění**: požadované toobuild hello kroky a spusťte hello ukázková.

## <a name="architecture"></a>Architektura

Hello [simulované zařízení cloudu nahrát ukázková] ukazuje, jak toocreate bránu, která odesílá telemetrii z simulované zařízení tooan IoT hub. Zařízení nemusí být možné tooconnect přímo tooIoT rozbočovače protože hello zařízení:

* Nepoužívá komunikační protokol podporovaných službou IoT Hub.
* Není dostatečně inteligentní tooremember hello identity přiřazené tooit službou IoT Hub.

IoT vstupní brána může vyřešit tyto problémy v hello následující způsoby:

* Hello brány rozumí hello protokol používaný zařízením hello získává telemetrická data zařízení cloud ze zařízení hello a předává tyto zprávy tooIoT použití protokolu podporovaných službou hello IoT hub rozbočovače.

* Brána Hello mapuje toodevices identit služby IoT Hub a funguje jako proxy server, když zařízení odesílá zprávy tooIoT rozbočovače.

Hello následující diagram znázorňuje hlavní komponenty hello ukázkový text hello, včetně hello moduly IoT Edge:

![][1]

moduly Hello nepředávejte zprávy přímo tooeach jiné. moduly Hello publikovat tooan zprávy interní se zprostředkovatel, který doručí toohello hello zprávy z ostatních modulů pomocí mechanismu předplatného. Další informace najdete v tématu [Začínáme s Azure IoT Edge][lnk-gw-getstarted].

### <a name="protocol-ingestion-module"></a>Modul ingestování protokolu

Tento modul je hello výchozí bod pro příjem dat ze zařízení, prostřednictvím brány hello a do cloudu hello. V ukázce hello hello modul:

1. Vytvoří simulované teplotní data. Pokud používáte fyzické zařízení, hello modul čte data z těchto fyzických zařízení.
1. Vytvoří zprávu.
1. Hello simulované teplotní data umístí do hello obsah zprávy.
1. Přidá vlastnost s zprávu toohello falešné adresy MAC.
1. V řetězci hello umožňuje hello zprávy k dispozici toohello další modul.

Hello modul s názvem **protokol X přijímání** v hello předchozí diagram nazývá **simulované zařízení** ve zdrojovém kódu hello.

### <a name="mac-lt-gt-iot-hub-id-module"></a>Adresa MAC &lt; - &gt; Modul ID služby IoT Hub

Tento modul slouží k vyhledání zprávy, které mají vlastnost adresu Mac. V ukázce hello hello protokol přijímání modul přidává vlastnost adresy MAC hello. Pokud modul hello najde taková vlastnost, přidá jinou vlastnost s zprávou klíče toohello zařízení IoT Hub. modul Hello vytváří v řetězu hello hello zprávy k dispozici toohello další modul.

Hello vývojáře nastaví mapování mezi adresy MAC a IoT Hub identity tooassociate hello simulované zařízení pomocí identity zařízení IoT Hub. mapování hello Hello vývojáře ručně přidá jako součást konfigurace modulu hello.

> [!NOTE]
> Tato ukázka používá jako jedinečný identifikátor zařízení adresu MAC a koreluje ji s identitou zařízení služby IoT Hub. Můžete si ale napsat vlastní modul, který bude používat jiný jedinečný identifikátor. Například zařízení může mít jedinečné sériová čísla nebo hello telemetrická data může zahrnovat název jedinečný vložená zařízení.

### <a name="iot-hub-communication-module"></a>Komunikační modul služby IoT Hub

Tento modul přijímá zprávy pomocí služby IoT Hub zařízení klíčovou vlastnost, kterému byla přiřazena předchozí modulem hello. modul Hello pošle uvítací zprávu pomocí centra obsahu tooIoT hello protokolu HTTP. HTTP je jedním z hello tří protokolů rozumí službou IoT Hub.

Tento modul místo otevřením připojení pro každé simulované zařízení, otevře jednoho připojení protokolu HTTP z hello brány toohello IoT hub. modul Hello pak multiplexes připojení ze všech hello simulované zařízení přes toto připojení. Tento přístup umožňuje velké tooconnect mnoho další zařízení.

## <a name="before-you-get-started"></a>Než začnete

Než začnete, musíte provést tyto akce:

* [Vytvoření služby IoT hub] [ lnk-create-hub] ve vašem předplatném Azure, potřebujete hello název vašeho centra toocomplete tento návod. Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].
* Přidejte dva zařízení tooyour IoT hub a poznamenejte si jeho ID a klíče zařízení. Můžete použít hello [explorer zařízení] [ lnk-device-explorer] nebo [iothub-explorer] [ lnk-iothub-explorer] nástroj tooadd zařízení IoT hub toohello jste vytvořili v hello předchozí krok a načíst jejich klíče.

![][2]

<!-- Images -->
[1]: media/iot-hub-iot-edge-simulated-selector/image1.png
[2]: media/iot-hub-iot-edge-simulated-selector/image2.png

<!-- Links -->
[simulované zařízení cloudu nahrát ukázková]: https://github.com/Azure/iot-edge/blob/master/samples/simulated_device_cloud_upload/README.md
[lnk-sdk]: https://github.com/Azure/iot-edge
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md
[lnk-create-hub]: ../articles/iot-hub/iot-hub-create-through-portal.md