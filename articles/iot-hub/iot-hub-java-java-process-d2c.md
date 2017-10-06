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
# <a name="process-iot-hub-device-to-cloud-messages-java"></a><span data-ttu-id="fb8b6-103">Zpracování zpráv typu zařízení cloud IoT Hub (Java)</span><span class="sxs-lookup"><span data-stu-id="fb8b6-103">Process IoT Hub device-to-cloud messages (Java)</span></span>

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

<span data-ttu-id="fb8b6-104">Azure IoT Hub je plně spravovaná služba, která umožňuje spolehlivou a zabezpečené obousměrnou komunikaci mezi miliony zařízení a řešením back-end.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-104">Azure IoT Hub is a fully managed service that enables reliable and secure bi-directional communications between millions of devices and a solution back end.</span></span> <span data-ttu-id="fb8b6-105">Ostatní kurzy ([Začínáme se službou IoT Hub] a [odesílání zpráv typu cloud zařízení s centrem IoT][lnk-c2d]) ukazují, jak toouse hello základní zařízení cloud a z cloudu do zařízení funkce služby IoT Hub pro zasílání zpráv.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-105">Other tutorials ([Get started with IoT Hub] and [Send cloud-to-device messages with IoT Hub][lnk-c2d]) show you how toouse hello basic device-to-cloud and cloud-to-device messaging functionality of IoT Hub.</span></span>

<span data-ttu-id="fb8b6-106">V tomto kurzu vychází hello kód ukazuje hello [Začínáme se službou IoT Hub] kurzu a ukazuje, jak toouse zprávy směrování zpráv typu zařízení cloud tooprocess škálovatelné způsobem.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-106">This tutorial builds on hello code shown in hello [Get started with IoT Hub] tutorial, and shows you how toouse message routing tooprocess device-to-cloud messages in a scalable way.</span></span> <span data-ttu-id="fb8b6-107">Hello kurz ukazuje, jak tooprocess zprávy, které vyžadují okamžitou akci z řešení hello back-end.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-107">hello tutorial illustrates how tooprocess messages that require immediate action from hello solution back end.</span></span> <span data-ttu-id="fb8b6-108">Zařízení může například odešle zprávu výstrahy, která aktivuje vkládání lístek do systému CRM.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-108">For example, a device might send an alarm message that triggers inserting a ticket into a CRM system.</span></span> <span data-ttu-id="fb8b6-109">Naopak datového bodu zprávy jednoduše informačního kanálu do modul analytics.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-109">By contrast, data-point messages simply feed into an analytics engine.</span></span> <span data-ttu-id="fb8b6-110">Například teplotní telemetrie ze zařízení, které se uloží pro pozdější analýzu toobe je zpráva datového bodu.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-110">For example, temperature telemetry from a device that is toobe stored for later analysis is a data-point message.</span></span>

<span data-ttu-id="fb8b6-111">Na konci hello tohoto kurzu můžete spustit tři aplikace konzoly v jazyce Java:</span><span class="sxs-lookup"><span data-stu-id="fb8b6-111">At hello end of this tutorial, you run three Java console apps:</span></span>

* <span data-ttu-id="fb8b6-112">**simulated-device**, upravenou verzi hello aplikace vytvořená v hello [Začínáme se službou IoT Hub] kurzu posílání zpráv typu zařízení cloud datového bodu za sekundu a interaktivní zařízení cloud zprávy každých 10 sekund.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-112">**simulated-device**, a modified version of hello app created in hello [Get started with IoT Hub] tutorial, sends data-point device-to-cloud messages every second, and interactive device-to-cloud messages every 10 seconds.</span></span> <span data-ttu-id="fb8b6-113">Tato aplikace používá toocommunicate protokol AMQP hello službou IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-113">This app uses hello AMQP protocol toocommunicate with IoT Hub.</span></span>
* <span data-ttu-id="fb8b6-114">**Read-d2c-messages** zobrazí hello telemetrické zprávy odesílané aplikace zařízení.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-114">**read-d2c-messages** displays hello telemetry sent by your device app.</span></span>
* <span data-ttu-id="fb8b6-115">**čtení kritické fronty** zrušte fronty hello kritické zprávy z toohello připojené fronty Service Bus hello služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-115">**read-critical-queue** de-queues hello critical messages from hello Service Bus queue attached toohello IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="fb8b6-116">IoT Hub je podpora v sadě SDK pro mnoho zařízení platformy a jazyky, včetně C, Java a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-116">IoT Hub has SDK support for many device platforms and languages, including C, Java, and JavaScript.</span></span> <span data-ttu-id="fb8b6-117">Pokyny, jak tooreplace hello zařízení v tomto kurzu se fyzické zařízení a jak tooan tooconnect zařízení IoT Hub, najdete v části hello [Azure střediska pro vývojáře IoT].</span><span class="sxs-lookup"><span data-stu-id="fb8b6-117">For instructions on how tooreplace hello device in this tutorial with a physical device, and how tooconnect devices tooan IoT Hub, see hello [Azure IoT Developer Center].</span></span>

<span data-ttu-id="fb8b6-118">toocomplete tohoto kurzu budete potřebovat hello následující:</span><span class="sxs-lookup"><span data-stu-id="fb8b6-118">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="fb8b6-119">Dokončení pracovní verzi hello [Začínáme se službou IoT Hub] kurzu.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-119">A complete working version of hello [Get started with IoT Hub] tutorial.</span></span>
* <span data-ttu-id="fb8b6-120">Hello nejnovější [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="fb8b6-120">hello latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span>
* [<span data-ttu-id="fb8b6-121">Maven 3</span><span class="sxs-lookup"><span data-stu-id="fb8b6-121">Maven 3</span></span>](https://maven.apache.org/install.html)
* <span data-ttu-id="fb8b6-122">Aktivní účet Azure.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-122">An active Azure account.</span></span> <span data-ttu-id="fb8b6-123">(Pokud nemáte účet, můžete vytvořit [bezplatný účet] [lnk-free-trial] si během několika minut.)</span><span class="sxs-lookup"><span data-stu-id="fb8b6-123">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

<span data-ttu-id="fb8b6-124">Měli byste některé základní znalosti o [Azure Storage] a [Azure Service Bus].</span><span class="sxs-lookup"><span data-stu-id="fb8b6-124">You should have some basic knowledge of [Azure Storage] and [Azure Service Bus].</span></span>

## <a name="send-interactive-messages-from-a-device-app"></a><span data-ttu-id="fb8b6-125">Odesílat interaktivní zprávy z aplikace na zařízení</span><span class="sxs-lookup"><span data-stu-id="fb8b6-125">Send interactive messages from a device app</span></span>
<span data-ttu-id="fb8b6-126">V této části upravíte hello zařízení aplikaci, kterou jste vytvořili v hello [Začínáme se službou IoT Hub] kurz toooccasionally odesílat zprávy, které vyžadují okamžitou zpracování.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-126">In this section, you modify hello device app you created in hello [Get started with IoT Hub] tutorial toooccasionally send messages that require immediate processing.</span></span>

1. <span data-ttu-id="fb8b6-127">Pomocí textového editoru tooopen hello simulated-device\src\main\java\com\mycompany\app\App.java souboru.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-127">Use a text editor tooopen hello simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span> <span data-ttu-id="fb8b6-128">Tento soubor obsahuje hello kód pro hello **simulated-device** aplikace, které jste vytvořili v hello [Začínáme se službou IoT Hub] kurzu.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-128">This file contains hello code for hello **simulated-device** app you created in hello [Get started with IoT Hub] tutorial.</span></span>

2. <span data-ttu-id="fb8b6-129">Nahraďte hello **MessageSender** se hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="fb8b6-129">Replace hello **MessageSender** class with hello following code:</span></span>

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
   
    <span data-ttu-id="fb8b6-130">Tato metoda náhodně přidá vlastnost hello `"level": "critical"` toomessages poslal hello zařízení, která simuluje zprávu, která vyžaduje okamžitý zásah pomocí aplikace hello back-end.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-130">This method randomly adds hello property `"level": "critical"` toomessages sent by hello device, which simulates a message that requires immediate action by hello application back-end.</span></span> <span data-ttu-id="fb8b6-131">aplikace Hello předá tyto informace ve vlastnostech zprávy hello místo v textu zprávy hello tak této služby IoT Hub může směrovat hello zpráva toohello správné zpráva cílový.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-131">hello application passes this information in hello message properties, instead of in hello message body, so that IoT Hub can route hello message toohello proper message destination.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="fb8b6-132">Zpráv vlastnosti tooroute zprávy můžete použít pro různé scénáře, včetně studený path zpracování, kromě toohello aktivní trase příkladu v tomto poli.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-132">You can use message properties tooroute messages for various scenarios including cold-path processing, in addition toohello hot path example shown here.</span></span>

2. <span data-ttu-id="fb8b6-133">Uložte a zavřete soubor simulated-device\src\main\java\com\mycompany\app\App.java hello.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-133">Save and close hello simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

    > [!NOTE]
    > <span data-ttu-id="fb8b6-134">Pro hello zájmu jednoduchost tento kurz neimplementuje žádné zásady opakování.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-134">For hello sake of simplicity, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="fb8b6-135">V produkčním kódu, měli byste implementovat zásady opakování například exponenciálního omezení rychlosti dle pokynů v článku na webu MSDN hello [přechodných chyb].</span><span class="sxs-lookup"><span data-stu-id="fb8b6-135">In production code, you should implement a retry policy such as exponential backoff, as suggested in hello MSDN article [Transient Fault Handling].</span></span>

3. <span data-ttu-id="fb8b6-136">toobuild hello **simulated-device** aplikace pomocí nástroje Maven, spustit následující příkaz na příkazovém řádku hello ve složce simulated-device hello hello:</span><span class="sxs-lookup"><span data-stu-id="fb8b6-136">toobuild hello **simulated-device** app using Maven, execute hello following command at hello command prompt in hello simulated-device folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="add-a-queue-tooyour-iot-hub-and-route-messages-tooit"></a><span data-ttu-id="fb8b6-137">Přidat fronty tooyour IoT hub a směrování zpráv tooit</span><span class="sxs-lookup"><span data-stu-id="fb8b6-137">Add a queue tooyour IoT hub and route messages tooit</span></span>

<span data-ttu-id="fb8b6-138">V této části Vytvoření fronty Service Bus, připojte ho tooyour IoT hub a konfigurace vašeho IoT hub toosend zprávy toohello frontu na základě přítomnosti hello vlastnosti na uvítací zprávu.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-138">In this section, you create a Service Bus queue, connect it tooyour IoT hub, and configure your IoT hub toosend messages toohello queue based on hello presence of a property on hello message.</span></span> <span data-ttu-id="fb8b6-139">Další informace o tom, jak tooprocess zpráv z fronty služby Service Bus najdete v tématu [začít pracovat s fronty][lnk-sb-queues-java].</span><span class="sxs-lookup"><span data-stu-id="fb8b6-139">For more information about how tooprocess messages from Service Bus queues, see [Get started with queues][lnk-sb-queues-java].</span></span>

1. <span data-ttu-id="fb8b6-140">Vytvoření fronty Service Bus, jak je popsáno v [začít pracovat s fronty][lnk-sb-queues-java].</span><span class="sxs-lookup"><span data-stu-id="fb8b6-140">Create a Service Bus queue as described in [Get started with queues][lnk-sb-queues-java].</span></span> <span data-ttu-id="fb8b6-141">Poznamenejte si název oboru názvů a fronty hello.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-141">Make a note of hello namespace and queue name.</span></span>

2. <span data-ttu-id="fb8b6-142">V hello portálu Azure, otevřete své služby IoT hub a klikněte na tlačítko **koncové body**.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-142">In hello Azure portal, open your IoT hub and click **Endpoints**.</span></span>

    ![Koncové body v centru IoT][30]

3. <span data-ttu-id="fb8b6-144">V hello **koncové body** okně klikněte na tlačítko **přidat** na hello nejvyšší tooadd fronty tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-144">In hello **Endpoints** blade, click **Add** at hello top tooadd your queue tooyour IoT hub.</span></span> <span data-ttu-id="fb8b6-145">Koncový bod hello název **CriticalQueue** a použijte rozevírací seznamy tooselect hello **fronty Service Bus**hello oboru názvů Service Bus, ve které je umístěn fronty a hello název fronty.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-145">Name hello endpoint **CriticalQueue** and use hello drop-downs tooselect **Service Bus queue**, hello Service Bus namespace in which your queue resides, and hello name of your queue.</span></span> <span data-ttu-id="fb8b6-146">Až budete hotovi, klikněte na tlačítko **Uložit** dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-146">When you are done, click **Save** at hello bottom.</span></span>

    ![Přidání koncového bodu][31]

4. <span data-ttu-id="fb8b6-148">Klikněte na tlačítko **trasy** ve službě IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-148">Now click **Routes** in your IoT Hub.</span></span> <span data-ttu-id="fb8b6-149">Klikněte na tlačítko **přidat** hello horní části toocreate hello okno Přidat pravidlo směrování, který směruje zprávy toohello fronty je právě.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-149">Click **Add** at hello top of hello blade toocreate a routing rule that routes messages toohello queue you just added.</span></span> <span data-ttu-id="fb8b6-150">Vyberte **DeviceTelemetry** jako zdroj dat hello.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-150">Select **DeviceTelemetry** as hello source of data.</span></span> <span data-ttu-id="fb8b6-151">Zadejte `level="critical"` jako hello podmínka a zvolte hello fronty, které jste právě přidali jako vlastní koncový bod jako hello směrování pravidlo koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-151">Enter `level="critical"` as hello condition, and choose hello queue you just added as a custom endpoint as hello routing rule endpoint.</span></span> <span data-ttu-id="fb8b6-152">Až budete hotovi, klikněte na tlačítko **Uložit** dolnímu hello.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-152">When you are done, click **Save** at hello bottom.</span></span>

    ![Přidání trasu][32]

    <span data-ttu-id="fb8b6-154">Zajistěte, aby záložní trasy hello je nastaven příliš**ON**.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-154">Make sure hello fallback route is set too**ON**.</span></span> <span data-ttu-id="fb8b6-155">Toto nastavení je hello výchozí konfiguraci služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-155">This setting is hello default configuration of an IoT hub.</span></span>

    ![Záložní trasy][33]

## <a name="optional-read-from-hello-queue-endpoint"></a><span data-ttu-id="fb8b6-157">(Volitelné) Čtení z fronty hello koncového bodu</span><span class="sxs-lookup"><span data-stu-id="fb8b6-157">(Optional) Read from hello queue endpoint</span></span>

<span data-ttu-id="fb8b6-158">Volitelně může číst hello zprávy z fronty hello koncového bodu podle následujících pokynů hello v [začít pracovat s fronty][lnk-sb-queues-java].</span><span class="sxs-lookup"><span data-stu-id="fb8b6-158">You can optionally read hello messages from hello queue endpoint by following hello instructions in [Get started with queues][lnk-sb-queues-java].</span></span> <span data-ttu-id="fb8b6-159">Název aplikace hello **čtení kritické fronty**.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-159">Name hello app **read-critical-queue**.</span></span>

## <a name="run-hello-applications"></a><span data-ttu-id="fb8b6-160">Spouštění aplikací hello</span><span class="sxs-lookup"><span data-stu-id="fb8b6-160">Run hello applications</span></span>

<span data-ttu-id="fb8b6-161">Teď je připraven toorun hello tři aplikace.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-161">Now you are ready toorun hello three applications.</span></span>

1. <span data-ttu-id="fb8b6-162">toorun hello **read-d2c-messages** aplikace, v příkazovém řádku nebo prostředí přejděte toohello read-d2c složky a spuštění hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fb8b6-162">toorun hello **read-d2c-messages** application, in a command prompt or shell navigate toohello read-d2c folder and execute hello following command:</span></span>

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```

   ![Spustit read-d2c-messages][readd2c]

2. <span data-ttu-id="fb8b6-164">toorun hello **čtení kritické fronty** aplikace, v příkazovém řádku nebo prostředí přejděte toohello čtení kritické fronty složky a spuštění hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="fb8b6-164">toorun hello **read-critical-queue** application, in a command prompt or shell navigate toohello read-critical-queue folder and execute hello following command:</span></span>

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```
   
   ![Spustit pro čtení kritické zprávy][readqueue]

3. <span data-ttu-id="fb8b6-166">toorun hello **simulated-device** aplikaci, v příkazovém řádku nebo prostředí přejděte složky simulated Devices toohello a spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="fb8b6-166">toorun hello **simulated-device** app, in a command prompt or shell navigate toohello simulated-device folder and execute hello following command:</span></span>

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```
   
   ![Spustit simulated-device][simulateddevice]

## <a name="next-steps"></a><span data-ttu-id="fb8b6-168">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fb8b6-168">Next steps</span></span>

<span data-ttu-id="fb8b6-169">V tomto kurzu jste se dozvěděli, jak tooreliably odesílání zpráv typu zařízení cloud pomocí hello funkci směrování zpráv služby IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-169">In this tutorial, you learned how tooreliably dispatch device-to-cloud messages by using hello message routing functionality of IoT Hub.</span></span>

<span data-ttu-id="fb8b6-170">Hello [jak toosend cloud zařízení zpráv službou IoT Hub] [ lnk-c2d] ukazuje, jak toosend zprávy tooyour zařízení z back end vašeho řešení.</span><span class="sxs-lookup"><span data-stu-id="fb8b6-170">hello [How toosend cloud-to-device messages with IoT Hub][lnk-c2d] shows you how toosend messages tooyour devices from your solution back end.</span></span>

<span data-ttu-id="fb8b6-171">Příklady toosee dokončení začátku do konce řešení, které pomocí služby IoT Hub, viz [Azure IoT Suite][lnk-suite].</span><span class="sxs-lookup"><span data-stu-id="fb8b6-171">toosee examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite][lnk-suite].</span></span>

<span data-ttu-id="fb8b6-172">toolearn Další informace o vývoji řešení službou IoT Hub, najdete v části hello [Příručka vývojáře pro službu IoT Hub].</span><span class="sxs-lookup"><span data-stu-id="fb8b6-172">toolearn more about developing solutions with IoT Hub, see hello [IoT Hub developer guide].</span></span>

<span data-ttu-id="fb8b6-173">toolearn Další informace o směrování zpráv do služby IoT Hub, najdete v části [odesílat a přijímat zprávy službou IoT Hub][lnk-devguide-messaging].</span><span class="sxs-lookup"><span data-stu-id="fb8b6-173">toolearn more about message routing in IoT Hub, see [Send and receive messages with IoT Hub][lnk-devguide-messaging].</span></span>

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
