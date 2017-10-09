> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-direct-methods.md)
> * [C#/Node.js](../articles/iot-hub/iot-hub-csharp-node-direct-methods.md)
> * [C#](../articles/iot-hub/iot-hub-csharp-csharp-direct-methods.md)
> * [Java](../articles/iot-hub/iot-hub-java-java-direct-methods.md)

Azure IoT Hub je plně spravovaná služba, která umožňuje spolehlivou a zabezpečené obousměrnou komunikaci mezi miliony zařízení a řešením back-end. Předchozí kurzy ([Začínáme se službou IoT Hub] a [odesílání zpráv typu Cloud-zařízení s centrem IoT]) ilustrují hello základních zařízení cloud a z cloudu do zařízení zasílání zpráv funkcí služby IoT Hub. Centrum IoT také nabízí možnost tooinvoke netrvalý metody na zařízení z cloudu hello hello. Přímé metody představují požadavku a odpovědi interakci s zařízení podobné tooan HTTP volání v, že jejich úspěch nebo neúspěch okamžitě (po časový limit definované uživatelem) toolet hello uživatele vědět hello stav hello volání. [Volání metody přímé na zařízení] [ lnk-devguide-methods] popisuje přímé metody podrobněji a nabízí informace o při toouse přímé metody místo zprávy typu cloud zařízení nebo požadované vlastnosti.

V tomto kurzu získáte informace o následujících postupech:

* Použijte hello portálu toocreate Azure IoT hub a vytvoření identity zařízení ve službě IoT hub.
* Vytvoření aplikace simulovaného zařízení, která má přímý metodu, která je možné volat v cloudu hello.
* Vytvořte konzolovou aplikaci, která volá metodu přímé v hello aplikaci simulovaného zařízení prostřednictvím služby IoT hub.

> [!NOTE]
> V tomto okamžiku jsou přímé metody pouze podporovaná na zařízení, které se připojují prostřednictvím centra tooIoT hello MQTT protokolu. Naleznete toohello [MQTT podporu] [ lnk-devguide-mqtt] článku pokyny, jak tooconvert existující zařízení aplikaci toouse MQTT.


[lnk-devguide-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md

[odesílání zpráv typu Cloud-zařízení s centrem IoT]: ../articles/iot-hub/iot-hub-csharp-csharp-c2d.md
[Začínáme se službou IoT Hub]: ../articles/iot-hub/iot-hub-node-node-getstarted.md