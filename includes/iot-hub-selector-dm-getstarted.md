> [!div class="op_single_selector"]
> * [Zařízení: Node.js Služba: Node.js](../articles/iot-hub/iot-hub-node-node-device-management-get-started.md)
> * [Zařízení: Node.js Služba: C#](../articles/iot-hub/iot-hub-csharp-node-device-management-get-started.md)
> * [Zařízení: Služba Java: Java](../articles/iot-hub/iot-hub-java-java-device-management-getstarted.md)

Back-end aplikace můžete použít Azure IoT Hub primitiv, jako například [dvojče zařízení] [ lnk-devtwin] a [přímé metody][lnk-c2dmethod], tooremotely spuštění a monitorování akce správy zařízení na zařízení. Tento kurz ukazuje, jak můžete spolupracovat tooinitiate a monitorovat restartování vzdáleného zařízení pomocí služby IoT Hub back-end aplikace a aplikace na zařízení.

Použijte přímá metoda tooinitiate zařízení akce správy (například restartování, obnovení továrního nastavení a aktualizaci firmwaru) z back-end aplikace v cloudu hello. Hello zařízení je zodpovědná za:

* Zpracování hello metoda požadavek odeslaný ze služby IoT Hub.
* Probíhá inicializace hello odpovídající akce konkrétní zařízení na zařízení hello.
* Poskytuje stav aktualizací prostřednictvím *hlášené vlastnosti* tooIoT rozbočovače.

Back-end aplikace můžete použít v hello cloudu toorun zařízení twin dotazy tooreport na průběh hello vaše akce správy zařízení.

[lnk-devtwin]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
