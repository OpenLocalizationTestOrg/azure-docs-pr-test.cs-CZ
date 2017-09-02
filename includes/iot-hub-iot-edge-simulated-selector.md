> [!div class="op_single_selector"]
> * [Linux](../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md)
> * [Windows](../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md)

Tento návod [simulované zařízení cloudu nahrát ukázková] ukazuje, jak používat [Azure IoT Edge] [ lnk-sdk] k odesílání telemetrie zařízení cloud do služby IoT Hub ze simulovaného zařízení .

Tento návod ilustruje:

* **Architektura**: architektury informace o [simulované zařízení cloudu nahrát ukázková].
* **Sestavení a spuštění:** Kroky potřebné k sestavení a spuštění ukázky.

## <a name="architecture"></a>Architektura

[simulované zařízení cloudu nahrát ukázková] ukazuje, jak vytvořit bránu, která odesílá telemetrii z Simulovaná zařízení do služby IoT hub. Zařízení nemusí být schopni připojit přímo do služby IoT Hub, protože zařízení:

* Nepoužívá komunikační protokol podporovaných službou IoT Hub.
* Není dostatečně inteligentní pamatovat identity přiřazené službou IoT Hub.

IoT vstupní brána může vyřešit tyto problémy následujícími způsoby:

* Brána rozumí protokol zařízení, použije získává telemetrická data zařízení cloud ze zařízení a předá tyto zprávy do služby IoT Hub použití protokolu podporovaných službou IoT hub.

* Brána mapuje identit služby IoT Hub na zařízení a funguje jako proxy server, když zařízení odesílá zprávy do služby IoT Hub.

Následující diagram znázorňuje hlavní komponenty ukázku, včetně modulů hraniční IoT:

![][1]

Moduly si mezi sebou nepředávají zprávy přímo. Moduly publikování zpráv interní zprostředkovatele, který doručí zpráv na jiné moduly pomocí mechanismu předplatného. Další informace najdete v tématu [Začínáme s Azure IoT Edge][lnk-gw-getstarted].

### <a name="protocol-ingestion-module"></a>Modul ingestování protokolu

Tento modul je výchozím bodem pro příjem dat ze zařízení, prostřednictvím brány a do cloudu. V ukázce modul:

1. Vytvoří simulované teplotní data. Pokud používáte fyzické zařízení, modul čte data z těchto fyzických zařízení.
1. Vytvoří zprávu.
1. Simulované teplotní data umístí do obsahu zprávy.
1. Přidá vlastnost s falešných adresu MAC na zprávu.
1. Zpráva zpřístupňuje další modul v řetězu.

Názvem modulu **protokol X přijímání** na předchozím obrázku se nazývá **simulované zařízení** ve zdrojovém kódu.

### <a name="mac-lt-gt-iot-hub-id-module"></a>Adresa MAC &lt; - &gt; Modul ID služby IoT Hub

Tento modul slouží k vyhledání zprávy, které mají vlastnost adresu Mac. V ukázce modul přijímání protokol přidá vlastnost adresy MAC. Pokud modul najde taková vlastnost, přidá jinou vlastnost s klíčem zařízení IoT Hub ke zprávě. Modul poté zpřístupní zprávu další modul v řetězu.

Vývojář nastaví mapování mezi adresy MAC i identit služby IoT Hub tak, aby Simulovaná zařízení přidružit identit zařízení IoT Hub. Vývojář přidá mapování ručně jako součást konfigurace modulu.

> [!NOTE]
> Tato ukázka používá jako jedinečný identifikátor zařízení adresu MAC a koreluje ji s identitou zařízení služby IoT Hub. Můžete si ale napsat vlastní modul, který bude používat jiný jedinečný identifikátor. Například zařízení může mít jedinečné sériová čísla nebo telemetrická data může zahrnovat název jedinečný vložená zařízení.

### <a name="iot-hub-communication-module"></a>Komunikační modul služby IoT Hub

Tento modul přijímá zprávy pomocí služby IoT Hub zařízení klíčovou vlastnost, kterému byla přiřazena předchozí modulem. Modul odesílá obsah zprávy do služby IoT Hub pomocí protokolu HTTP. HTTP je jedním ze tří protokolů, kterým služba IoT Hub rozumí.

Tento modul místo otevřením připojení pro každé simulované zařízení, otevře jednoho připojení protokolu HTTP z brány do služby IoT hub. Modul multiplexes připojení ze simulovaného zařízení pak přes toto připojení. Tento přístup umožňuje jednu bránu pro připojení mnoho další zařízení.

## <a name="before-you-get-started"></a>Než začnete

Než začnete, musíte provést tyto akce:

* [Vytvoření služby IoT hub] [ lnk-create-hub] ve vašem předplatném Azure, je třeba název centra pro dokončení tohoto návodu. Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].
* Přidejte dva zařízení do služby IoT hub a poznamenejte si jeho ID a klíče zařízení. Můžete použít [explorer zařízení] [ lnk-device-explorer] nebo [iothub-explorer] [ lnk-iothub-explorer] nástroje pro přidání zařízení ke službě IoT hub, který jste vytvořili v předchozím kroku a Načtěte jejich klíče.

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