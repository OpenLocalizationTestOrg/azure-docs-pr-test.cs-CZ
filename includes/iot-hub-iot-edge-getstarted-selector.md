> [!div class="op_single_selector"]
> * [Linux](../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md)
> * [Windows](../articles/iot-hub/iot-hub-windows-iot-edge-get-started.md)
> 
> 

Tento článek obsahuje podrobný návod k hello [Hello, World ukázkový kód] [ lnk-helloworld-sample] tooillustrate hello základní součástí hello [Azure IoT Edge] [ lnk-iot-edge] architektura. Hello ukázce se používá toobuild hello Azure IoT hraniční bránu jednoduché protokoly každých pět sekund soubor tooa zprávu "hello, world".

Tento návod ilustruje:

* **Hello World ukázková architektura**: Popisuje, jak [architektury koncepty Azure IoT Edge] [ lnk-edge-concepts] použít toohello Hello, World vzorek a jak hello součásti zapadají.
* **Jak toobuild hello ukázka**: hello kroky požadované toobuild hello ukázka.
* **Jak toorun hello ukázka**: hello kroky požadované toorun hello ukázka. 
* **Typické výstup**: Příklad hello výstup tooexpect při spuštění ukázkové hello.
* **Fragmenty kódu**: kolekce tooshow fragmenty kódu jak ukázka programu hello Hello World implementuje klíče IoT hraniční brány součásti.


## <a name="hello-world-sample-architecture"></a>Architektura ukázky Hello World
Ukázka programu Hello Hello World znázorňuje hello konceptů popsaných v předchozí části hello. Ukázka programu Hello Hello World implementuje IoT hraniční bránu, která se skládá ze dvou IoT Edge moduly kanál:

* Hello *hello, world* modulu vytvoří zprávu každých pět sekund a předává je modul toohello protokolovacího nástroje.
* Hello *protokolovač* zprávy hello zápisy modulu obdrží tooa souboru.

![Architektura ukázky Hello World vytvořené s použitím služby Azure IoT Edge][4]

Jak je popsáno v předchozí části hello, hello World Hello modulu nepředává zprávy přímo toohello protokoly modulu každých pět sekund. Místo toho se publikuje zprostředkovatel toohello zpráva každých pět sekund.

Hello protokolovacího nástroje modul přijímá uvítací zprávu z hello zprostředkovatele a spustí se při, zápis hello obsah souboru tooa zpráva hello.

Hello protokoly modulu využívá pouze zprávy z hello zprostředkovatele, se nikdy publikuje nový zprostředkovatel toohello zprávy.

![Jak hello zprostředkovatele směrování zpráv mezi moduly v Azure IoT Edge][5]

Hello výše uvedené schéma ukazuje architekturu hello ukázka programu hello Hello World a relativní cesty hello toohello zdrojové soubory, které implementují různé části hello ukázka v hello [úložiště][lnk-iot-edge]. Prozkoumejte hello kód vlastní, nebo hello fragmenty kódu níže, použijte jako vodítko.

<!-- Images -->
[4]: media/iot-hub-iot-edge-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-iot-edge-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/iot-edge/tree/master/samples/hello_world
[lnk-iot-edge]: https://github.com/Azure/iot-edge
[lnk-edge-concepts]: ../articles/iot-hub/iot-hub-iot-edge-overview.md