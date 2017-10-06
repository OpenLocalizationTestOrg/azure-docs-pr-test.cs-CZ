---
title: "aaaProcess zpráv typu zařízení cloud Azure IoT Hub (Java) | Microsoft Docs"
description: "Jak tooprocess zprávy typu zařízení cloud IoT Hub pomocí pravidel směrování a vlastní koncové body toodispatch zpráv tooother back endové služby."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: bd9af5f9-a740-4780-a2a6-8c0e2752cf48
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: dobett
ms.openlocfilehash: 084e84e721ca4297c4d7d6cb06a43b0bed9bce85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="process-iot-hub-device-to-cloud-messages-java"></a>Zpracování zpráv typu zařízení cloud IoT Hub (Java)

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

Azure IoT Hub je plně spravovaná služba, která umožňuje spolehlivou a zabezpečené obousměrnou komunikaci mezi miliony zařízení a řešením back-end. Ostatní kurzy ([Začínáme se službou IoT Hub] a [odesílání zpráv typu cloud zařízení s centrem IoT][lnk-c2d]) ukazují, jak toouse hello základní zařízení cloud a z cloudu do zařízení funkce služby IoT Hub pro zasílání zpráv.

V tomto kurzu vychází hello kód ukazuje hello [Začínáme se službou IoT Hub] kurzu a ukazuje, jak toouse zprávy směrování zpráv typu zařízení cloud tooprocess škálovatelné způsobem. Hello kurz ukazuje, jak tooprocess zprávy, které vyžadují okamžitou akci z řešení hello back-end. Zařízení může například odešle zprávu výstrahy, která aktivuje vkládání lístek do systému CRM. Naopak datového bodu zprávy jednoduše informačního kanálu do modul analytics. Například teplotní telemetrie ze zařízení, které se uloží pro pozdější analýzu toobe je zpráva datového bodu.

Na konci hello tohoto kurzu můžete spustit tři aplikace konzoly v jazyce Java:

* **simulated-device**, upravenou verzi hello aplikace vytvořená v hello [Začínáme se službou IoT Hub] kurzu posílání zpráv typu zařízení cloud datového bodu za sekundu a interaktivní zařízení cloud zprávy každých 10 sekund. Tato aplikace používá toocommunicate protokol AMQP hello službou IoT Hub.
* **Read-d2c-messages** zobrazí hello telemetrické zprávy odesílané aplikace zařízení.
* **čtení kritické fronty** zrušte fronty hello kritické zprávy z toohello připojené fronty Service Bus hello služby IoT hub.

> [!NOTE]
> IoT Hub je podpora v sadě SDK pro mnoho zařízení platformy a jazyky, včetně C, Java a JavaScript. Pokyny, jak tooreplace hello zařízení v tomto kurzu se fyzické zařízení a jak tooan tooconnect zařízení IoT Hub, najdete v části hello [Azure střediska pro vývojáře IoT].

toocomplete tohoto kurzu budete potřebovat hello následující:

* Dokončení pracovní verzi hello [Začínáme se službou IoT Hub] kurzu.
* Hello nejnovější [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Aktivní účet Azure. (Pokud nemáte účet, můžete vytvořit [bezplatný účet] [lnk-free-trial] si během několika minut.)

Měli byste některé základní znalosti o [Azure Storage] a [Azure Service Bus].

## <a name="send-interactive-messages-from-a-device-app"></a>Odesílat interaktivní zprávy z aplikace na zařízení
V této části upravíte hello zařízení aplikaci, kterou jste vytvořili v hello [Začínáme se službou IoT Hub] kurz toooccasionally odesílat zprávy, které vyžadují okamžitou zpracování.

1. Pomocí textového editoru tooopen hello simulated-device\src\main\java\com\mycompany\app\App.java souboru. Tento soubor obsahuje hello kód pro hello **simulated-device** aplikace, které jste vytvořili v hello [Začínáme se službou IoT Hub] kurzu.

2. Nahraďte hello **MessageSender** se hello následující kód:

    ```java
    private static class MessageSender implements Runnable {

        public void run()  {
            try {
                double minTemperature = 20;
                double minHumidity = 60;
                Random rand = new Random();

                while (true) {
                    String msgStr;
                    Message msg;
                    if (new Random().nextDouble() > 0.7) {
                        msgStr = "This is a critical message.";
                        msg = new Message(msgStr);
                        msg.setProperty("level", "critical");
                    } else {
                        double currentTemperature = minTemperature + rand.nextDouble() * 15;
                        double currentHumidity = minHumidity + rand.nextDouble() * 20; 
                        TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
                        telemetryDataPoint.deviceId = deviceId;
                        telemetryDataPoint.temperature = currentTemperature;
                        telemetryDataPoint.humidity = currentHumidity;

                        msgStr = telemetryDataPoint.serialize();
                        msg = new Message(msgStr);
                    }
                    
                    System.out.println("Sending: " + msgStr);

                    Object lockobj = new Object();
                    EventCallback callback = new EventCallback();
                    client.sendEventAsync(msg, callback, lockobj);

                    synchronized (lockobj) {
                        lockobj.wait();
                    }
                    Thread.sleep(1000);
                }
            } catch (InterruptedException e) {
                System.out.println("Finished.");
            }
        }
    }
    ```
   
    Tato metoda náhodně přidá vlastnost hello `"level": "critical"` toomessages poslal hello zařízení, která simuluje zprávu, která vyžaduje okamžitý zásah pomocí aplikace hello back-end. aplikace Hello předá tyto informace ve vlastnostech zprávy hello místo v textu zprávy hello tak této služby IoT Hub může směrovat hello zpráva toohello správné zpráva cílový.
   
   > [!NOTE]
   > Zpráv vlastnosti tooroute zprávy můžete použít pro různé scénáře, včetně studený path zpracování, kromě toohello aktivní trase příkladu v tomto poli.

2. Uložte a zavřete soubor simulated-device\src\main\java\com\mycompany\app\App.java hello.

    > [!NOTE]
    > Pro hello zájmu jednoduchost tento kurz neimplementuje žádné zásady opakování. V produkčním kódu, měli byste implementovat zásady opakování například exponenciálního omezení rychlosti dle pokynů v článku na webu MSDN hello [přechodných chyb].

3. toobuild hello **simulated-device** aplikace pomocí nástroje Maven, spustit následující příkaz na příkazovém řádku hello ve složce simulated-device hello hello:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="add-a-queue-tooyour-iot-hub-and-route-messages-tooit"></a>Přidat fronty tooyour IoT hub a směrování zpráv tooit

V této části Vytvoření fronty Service Bus, připojte ho tooyour IoT hub a konfigurace vašeho IoT hub toosend zprávy toohello frontu na základě přítomnosti hello vlastnosti na uvítací zprávu. Další informace o tom, jak tooprocess zpráv z fronty služby Service Bus najdete v tématu [začít pracovat s fronty][lnk-sb-queues-java].

1. Vytvoření fronty Service Bus, jak je popsáno v [začít pracovat s fronty][lnk-sb-queues-java]. Poznamenejte si název oboru názvů a fronty hello.

2. V hello portálu Azure, otevřete své služby IoT hub a klikněte na tlačítko **koncové body**.

    ![Koncové body v centru IoT][30]

3. V hello **koncové body** okně klikněte na tlačítko **přidat** na hello nejvyšší tooadd fronty tooyour IoT hub. Koncový bod hello název **CriticalQueue** a použijte rozevírací seznamy tooselect hello **fronty Service Bus**hello oboru názvů Service Bus, ve které je umístěn fronty a hello název fronty. Až budete hotovi, klikněte na tlačítko **Uložit** dolnímu hello.

    ![Přidání koncového bodu][31]

4. Klikněte na tlačítko **trasy** ve službě IoT Hub. Klikněte na tlačítko **přidat** hello horní části toocreate hello okno Přidat pravidlo směrování, který směruje zprávy toohello fronty je právě. Vyberte **DeviceTelemetry** jako zdroj dat hello. Zadejte `level="critical"` jako hello podmínka a zvolte hello fronty, které jste právě přidali jako vlastní koncový bod jako hello směrování pravidlo koncového bodu. Až budete hotovi, klikněte na tlačítko **Uložit** dolnímu hello.

    ![Přidání trasu][32]

    Zajistěte, aby záložní trasy hello je nastaven příliš**ON**. Toto nastavení je hello výchozí konfiguraci služby IoT hub.

    ![Záložní trasy][33]

## <a name="optional-read-from-hello-queue-endpoint"></a>(Volitelné) Čtení z fronty hello koncového bodu

Volitelně může číst hello zprávy z fronty hello koncového bodu podle následujících pokynů hello v [začít pracovat s fronty][lnk-sb-queues-java]. Název aplikace hello **čtení kritické fronty**.

## <a name="run-hello-applications"></a>Spouštění aplikací hello

Teď je připraven toorun hello tři aplikace.

1. toorun hello **read-d2c-messages** aplikace, v příkazovém řádku nebo prostředí přejděte toohello read-d2c složky a spuštění hello následující příkaz:

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```

   ![Spustit read-d2c-messages][readd2c]

2. toorun hello **čtení kritické fronty** aplikace, v příkazovém řádku nebo prostředí přejděte toohello čtení kritické fronty složky a spuštění hello následující příkaz:

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```
   
   ![Spustit pro čtení kritické zprávy][readqueue]

3. toorun hello **simulated-device** aplikaci, v příkazovém řádku nebo prostředí přejděte složky simulated Devices toohello a spusťte následující příkaz hello:

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```
   
   ![Spustit simulated-device][simulateddevice]

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se dozvěděli, jak tooreliably odesílání zpráv typu zařízení cloud pomocí hello funkci směrování zpráv služby IoT Hub.

Hello [jak toosend cloud zařízení zpráv službou IoT Hub] [ lnk-c2d] ukazuje, jak toosend zprávy tooyour zařízení z back end vašeho řešení.

Příklady toosee dokončení začátku do konce řešení, které pomocí služby IoT Hub, viz [Azure IoT Suite][lnk-suite].

toolearn Další informace o vývoji řešení službou IoT Hub, najdete v části hello [Příručka vývojáře pro službu IoT Hub].

toolearn Další informace o směrování zpráv do služby IoT Hub, najdete v části [odesílat a přijímat zprávy službou IoT Hub][lnk-devguide-messaging].

<!-- Images. -->
<!-- TODO: UPDATE PICTURES -->
[simulateddevice]: ./media/iot-hub-java-java-process-d2c/runsimulateddevice.png
[readd2c]: ./media/iot-hub-java-java-process-d2c/runprocessinteractive.png
[readqueue]: ./media/iot-hub-java-java-process-d2c/runprocessd2c.png

[30]: ./media/iot-hub-java-java-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-java-java-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-java-java-process-d2c/route-creation.png
[33]: ./media/iot-hub-java-java-process-d2c/fallback-route.png

<!-- Links -->

[lnk-sb-queues-java]: ../service-bus-messaging/service-bus-java-how-to-use-queues.md

[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/

[Příručka vývojáře pro službu IoT Hub]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[Začínáme se službou IoT Hub]: iot-hub-java-java-getstarted.md
[Azure střediska pro vývojáře IoT]: https://azure.microsoft.com/develop/iot
[přechodných chyb]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Přechodná chyba zpracování]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-c2d]: iot-hub-java-java-c2d.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
