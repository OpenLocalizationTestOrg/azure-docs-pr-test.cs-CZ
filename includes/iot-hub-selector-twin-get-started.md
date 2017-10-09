> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
> * [C#/Node.js](../articles/iot-hub/iot-hub-csharp-node-twin-getstarted.md)
> * [C#](../articles/iot-hub/iot-hub-csharp-csharp-twin-getstarted.md)
> * [Java](../articles/iot-hub/iot-hub-java-java-twin-getstarted.md)

Dvojčata zařízení jsou dokumenty JSON, které obsahují informace o stavu zařízení (metadata, konfigurace a podmínky). IoT Hub trvá dvojče zařízení pro každé zařízení, která se připojuje tooit.

Použijte dvojčata zařízení na:

* Ukládání metadat ze zařízení z back end vašeho řešení.
* Sestava aktuální informace o stavu, jako jsou k dispozici funkce a podmínky (třeba hello připojení metoda se používá) z vaší aplikací.
* Synchronizujte hello stav pracovních dlouho běžící (například aktualizací firmwaru a konfigurace) mezi aplikací zařízení a back-end aplikace.
* Dotaz na vaše zařízení metadata, konfigurace nebo stavu.

> [!NOTE]
> Dvojčata zařízení jsou navrženy pro synchronizaci a pro dotazování konfigurací zařízení a podmínky. – Další informace na při toouse dvojčata zařízení naleznete v [pochopit dvojčata zařízení][lnk-twins].

Dvojčata zařízení se ukládají do služby IoT hub a obsahovat:

* *značky*, metadat zařízení, které jsou přístupné pouze hello back-end řešení;
* *požadovaného vlastnosti*, objekty JSON upravitelnými řešením hello back end a lze zobrazit aplikace hello zařízení; a
* *hlášené vlastnosti*, objekty JSON upravitelnými aplikace hello zařízení a přečíst back-end hello řešení. Značky a vlastností nesmí obsahovat pole, ale mohou být vnořené objekty.

![][img-twin]

Kromě toho se můžete dotazovat dvojčata zařízení založené na všechny hello výše data back-end hello řešení.
Odkazovat příliš[pochopit dvojčata zařízení] [ lnk-twins] Další informace o dvojčata zařízení a toohello [IoT Hub dotazovací jazyk] [ lnk-query] odkaz pro zadávání dotazů.

> [!NOTE]
> V tomto okamžiku jsou přístupné pouze ze zařízení, která se připojují tooIoT rozbočovače dvojčata zařízení pomocí protokolu MQTT hello. Naleznete toohello [MQTT podporu] [ lnk-devguide-mqtt] článku pokyny, jak tooconvert existující zařízení aplikaci toouse MQTT.

V tomto kurzu získáte informace o následujících postupech:

* Vytvoření aplikace back-end, který přidává *značky* tooa dvojče zařízení a aplikaci simulovaného zařízení, která generuje sestavy funkčnost připojení kanálu jako *hlášené vlastnost* na dvojče zařízení hello.
* Dotaz na zařízení z back-end aplikace pomocí filtrů o vlastnostech dříve vytvořili a hello značky.

<!-- images -->
[img-twin]: media/iot-hub-selector-twin-get-started/twin.png

<!-- links -->
[lnk-query]: ../articles/iot-hub/iot-hub-devguide-query-language.md
[lnk-twins]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-d2c]: ../articles/iot-hub/iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md