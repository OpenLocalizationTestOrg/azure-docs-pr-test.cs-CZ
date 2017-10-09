---
title: "použití tras (.Net) zpráv typu zařízení cloud aaaProcess Azure IoT Hub | Microsoft Docs"
description: "Jak tooprocess zprávy typu zařízení cloud IoT Hub pomocí pravidel směrování a vlastní koncové body toodispatch zpráv tooother back endové služby."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 5177bac9-722f-47ef-8a14-b201142ba4bc
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: c1dd5be04ca30c65af2be466ba6c8c1858333154
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="process-iot-hub-device-to-cloud-messages-using-routes-net"></a>Zpracování zpráv typu zařízení cloud IoT Hub pomocí tras (.NET)

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

V tomto kurzu vychází hello [Začínáme se službou IoT Hub] kurzu. kurz Hello:

* Ukazuje, jak směrování toouse pravidla zpráv typu zařízení cloud toodispatch způsobem snadno, založené na konfiguraci.
* Ukazuje, jak tooisolate interaktivní zprávy, které vyžadují okamžitou akci z řešení hello back-end pro další zpracování. Zařízení může například odešle zprávu výstrahy, která aktivuje vkládání lístek do systému CRM. Naproti tomu datového bodu zprávy, jako je například teplotní telemetrie informačního kanálu do modul analytics.

Na konci hello tohoto kurzu můžete spustit tři aplikace konzoly .NET:

* **SimulatedDevice**, upravenou verzi hello aplikace vytvořená v hello [Začínáme se službou IoT Hub] kurzu se odešle zprávy typu zařízení cloud datového bodu za sekundu a interaktivní zařízení cloud zprávy každých 10 sekund.
* **ReadDeviceToCloudMessages** , zobrazí hello nekritické telemetrické zprávy odesílané aplikace zařízení.
* **ReadCriticalQueue** zrušte fronty hello kritické zprávy odeslané aplikace zařízení z fronty Service Bus. Tato fronta je připojený toohello IoT hub.

> [!NOTE]
> IoT Hub je podpora v sadě SDK pro mnoho zařízení platformy a jazyky, včetně C, Java a JavaScript. toolearn způsobu tooreplace hello simulované zařízení v tomto kurzu se fyzické zařízení, najdete v hello [Azure střediska pro vývojáře IoT].

toocomplete tohoto kurzu budete potřebovat hello následující:

* Visual Studio 2015 nebo Visual Studio 2017.
* Aktivní účet Azure. <br/>Pokud účet nemáte, můžete vytvořit [bezplatný účet](https://azure.microsoft.com/free/) si během několika minut.

Měli byste některé základní znalosti o [Azure Storage] a [Azure Service Bus].

## <a name="send-interactive-messages"></a>Odesílat interaktivní zprávy

Upravit hello zařízení aplikaci, kterou jste vytvořili v hello [Začínáme se službou IoT Hub] kurz toooccasionally odesílat interaktivní zprávy.

V sadě Visual Studio v hello **SimulatedDevice** projektu, nahraďte hello `SendDeviceToCloudMessagesAsync` metoda s hello následující kód:

```csharp
private static async void SendDeviceToCloudMessagesAsync()
{
    double minTemperature = 20;
    double minHumidity = 60;
    Random rand = new Random();

    while (true)
    {
        double currentTemperature = minTemperature + rand.NextDouble() * 15;
        double currentHumidity = minHumidity + rand.NextDouble() * 20;

        var telemetryDataPoint = new
        {
            deviceId = "myFirstDevice",
            temperature = currentTemperature,
            humidity = currentHumidity
        };
        var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
        string levelValue;

        if (rand.NextDouble() > 0.7)
        {
            messageString = "This is a critical message";
            levelValue = "critical";
        }
        else
        {
            levelValue = "normal";
        }
        
        var message = new Message(Encoding.ASCII.GetBytes(messageString));
        message.Properties.Add("level", levelValue);
        
        await deviceClient.SendEventAsync(message);
        Console.WriteLine("{0} > Sent message: {1}", DateTime.Now, messageString);

        await Task.Delay(1000);
    }
}
```

Tato metoda náhodně přidá vlastnost hello `"level": "critical"` toomessages poslal hello zařízení, která simuluje zprávu, která vyžaduje okamžitý zásah pomocí back-end řešení hello. aplikace Hello zařízení předá tyto informace ve vlastnostech zprávy hello místo v textu zprávy hello tak této služby IoT Hub může směrovat hello zpráva toohello správné zpráva cílový.

> [!NOTE]
> Zpráv vlastnosti tooroute zprávy můžete použít pro různé scénáře, včetně studený path zpracování, kromě příklad horkou cesty toohello zobrazeny zde.

> [!NOTE]
> Pro hello zájmu jednoduchost tento kurz neimplementuje žádné zásady opakování. V produkčním kódu, měli byste implementovat zásady opakování například exponenciálního omezení rychlosti dle pokynů v článku na webu MSDN hello [přechodných chyb].

## <a name="route-messages-tooa-queue-in-your-iot-hub"></a>Směrování zprávy tooa fronty ve službě IoT hub

V této části:

* Vytvoření fronty Service Bus.
* Připojí tooyour IoT hub.
* Konfigurace vaší IoT hub toosend zprávy toohello frontu na základě přítomnosti hello vlastnosti na uvítací zprávu.

Další informace o tom, jak tooprocess zpráv z fronty služby Service Bus najdete v tématu [začít pracovat s fronty][Service Bus queue].

1. Vytvoření fronty Service Bus, jak je popsáno v [začít pracovat s fronty][Service Bus queue]. Hello fronty musí být v hello stejném předplatném, oblasti jako službu IoT hub. Poznamenejte si název oboru názvů a fronty hello.

    > [!NOTE]
    > Fronty sběrnice a témata použít jako koncové body centra IoT nesmí mít **relací** nebo **duplicitní detekce** povolena. Pokud některá z těchto možností jsou povolené, hello koncový bod se zobrazí jako **Unreachable** v hello portálu Azure.

2. V hello portálu Azure, otevřete své služby IoT hub a klikněte na tlačítko **koncové body**.
    
    ![Koncové body v centru IoT][30]

3. V hello **koncové body** okně klikněte na tlačítko **přidat** na hello nejvyšší tooadd fronty tooyour IoT hub. Koncový bod hello název **CriticalQueue** a použijte rozevírací seznamy tooselect hello **fronty Service Bus**hello oboru názvů Service Bus, ve které je umístěn fronty a hello název fronty. Až budete hotovi, klikněte na tlačítko **Uložit** dolnímu hello.
    
    ![Přidání koncového bodu][31]
    
4. Klikněte na tlačítko **trasy** ve službě IoT Hub. Klikněte na tlačítko **přidat** hello horní části toocreate hello okno Přidat pravidlo směrování, který směruje zprávy toohello fronty je právě. Vyberte **DeviceTelemetry** jako zdroj dat hello. Zadejte `level="critical"` jako hello podmínka a zvolte hello fronty, které jste právě přidali jako vlastní koncový bod jako hello směrování pravidlo koncového bodu. Až budete hotovi, klikněte na tlačítko **Uložit** dolnímu hello.
    
    ![Přidání trasu][32]
    
    Zajistěte, aby záložní trasy hello je nastaven příliš**ON**. Tato hodnota je hello výchozí konfiguraci pro Centrum IoT.
    
    ![Záložní trasy][33]

## <a name="read-from-hello-queue-endpoint"></a>Čtení z fronty hello koncového bodu

V této části číst hello zprávy z fronty hello koncového bodu.

1. V sadě Visual Studio, přidejte Visual C# Windows klasický desktopový projekt toohello aktuální řešení, pomocí hello **konzolovou aplikaci (rozhraní .NET Framework)** šablona projektu. Název projektu hello **ReadCriticalQueue**.

2. V Průzkumníku řešení klikněte pravým tlačítkem na hello **ReadCriticalQueue** projektu a pak klikněte na **spravovat balíčky NuGet**. Tato operace zobrazí hello **Správce balíčků NuGet** okno.

3. Vyhledejte **WindowsAzure.ServiceBus**, klikněte na tlačítko **nainstalovat**a přijměte podmínky použití hello. Tato operace stáhne, nainstaluje a přidá odkaz toohello Azure Service Bus se všemi jeho závislostmi.

4. Přidejte následující hello **pomocí** příkazy hello horní části hello **Program.cs** souboru:
   
    ```csharp
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Nakonec přidejte následující řádky toohello hello **hlavní** metoda. Nahraďte hello připojovací řetězec s **naslouchání** oprávnění pro frontu hello:
   
    ```csharp
    Console.WriteLine("Receive critical messages. Ctrl-C tooexit.\n");
    var connectionString = "{service bus listen string}";
    var queueName = "{queue name}";
    
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);

    client.OnMessage(message =>
        {
            Stream stream = message.GetBody<Stream>();
            StreamReader reader = new StreamReader(stream, Encoding.ASCII);
            string s = reader.ReadToEnd();
            Console.WriteLine(String.Format("Message body: {0}", s));
        });
        
    Console.ReadLine();
    ```

## <a name="run-hello-applications"></a>Spouštění aplikací hello
Teď je připraven toorun hello aplikace.

1. V sadě Visual Studio v Průzkumníku řešení klikněte pravým tlačítkem na řešení a vyberte **nastavit projekty po spuštění**. Vyberte **více projektů po spuštění**, pak vyberte **spustit** jako hello akce pro hello **ReadDeviceToCloudMessages**, **SimulatedDevice**, a **ReadCriticalQueue** projekty.
2. Stiskněte klávesu **F5** toostart hello tři aplikace konzoly. Hello **ReadDeviceToCloudMessages** aplikace má pouze nekritické zpráv odeslaných z hello **SimulatedDevice** aplikace a hello **ReadCriticalQueue** aplikace má pouze kritické zprávy.
   
   ![Tři aplikace konzoly][50]

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se dozvěděli, jak tooreliably odesílání zpráv typu zařízení cloud pomocí hello funkci směrování zpráv služby IoT Hub.

Hello [jak toosend cloud zařízení zpráv službou IoT Hub] [ lnk-c2d] ukazuje, jak toosend zprávy tooyour zařízení z back end vašeho řešení.

Příklady toosee dokončení začátku do konce řešení, které pomocí služby IoT Hub, viz [Azure IoT Suite][lnk-suite].

toolearn Další informace o vývoji řešení službou IoT Hub, najdete v části hello [Příručka vývojáře pro službu IoT Hub].

toolearn Další informace o směrování zpráv do služby IoT Hub, najdete v části [odesílat a přijímat zprávy službou IoT Hub][lnk-devguide-messaging].

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[30]: ./media/iot-hub-csharp-csharp-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/route-creation.png
[33]: ./media/iot-hub-csharp-csharp-process-d2c/fallback-route.png

<!-- Links -->
[Service Bus queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/
[Příručka vývojáře pro službu IoT Hub]: iot-hub-devguide.md
[Začínáme se službou IoT Hub]: iot-hub-csharp-csharp-getstarted.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[Azure střediska pro vývojáře IoT]: https://azure.microsoft.com/develop/iot
[přechodných chyb]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-c2d]: iot-hub-csharp-csharp-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
