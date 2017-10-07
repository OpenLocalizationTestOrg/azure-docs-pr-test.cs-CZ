---
title: "zprávy aaaCloud zařízení s Azure IoT Hub (.NET) | Microsoft Docs"
description: "Jak toosend cloud zařízení zpráv tooa zařízení ze služby Azure IoT hub pomocí .NET SDK služby Azure IoT hello. Upravte zprávy typu cloud zařízení tooreceive aplikace zařízení a změny zpráv back-end aplikace hello toosend cloud zařízení."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: a31c05ed-6ec0-40f3-99ab-8fdd28b1a89a
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: elioda
ms.openlocfilehash: f6a7618b164d95c8ddaf28943f244aeeb568217f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="send-messages-from-hello-cloud-tooyour-device-with-iot-hub-net"></a>Odesílání zpráv z hello cloud tooyour zařízení s centra IoT (.NET)
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Úvod
Azure IoT Hub je plně spravovaná služba, která pomáhá povolit spolehlivou a zabezpečenou obousměrnou komunikaci mezi miliony zařízení a back-end řešení. Hello [Začínáme se službou IoT Hub] kurzu se dozvíte, jak zřídit identitu zařízení v ní toocreate služby IoT hub a kódu aplikaci ze zařízení, která odesílá zprávy typu zařízení cloud.

V tomto kurzu vychází [Začínáme se službou IoT Hub]. Jak ukazuje na:

* Z back-end vašeho řešení odesílání zpráv typu cloud zařízení tooa jedno zařízení prostřednictvím služby IoT Hub.
* Příjem zpráv typu cloud zařízení na zařízení.
* Z back-end vašeho řešení, žádosti o potvrzení o doručení (*zpětné vazby*) pro zprávy odeslané tooa zařízení ze služby IoT Hub.

Další informace o zprávy typu cloud zařízení můžete najít v hello [Příručka vývojáře pro službu IoT Hub][IoT Hub developer guide - C2D].

Na konci hello tohoto kurzu můžete spustit dvě aplikace konzoly .NET:

* **SimulatedDevice**, upravenou verzi hello aplikace vytvořená v [Začínáme se službou IoT Hub], který připojí tooyour IoT hub a přijímá zprávy typu cloud zařízení.
* **SendCloudToDevice**, který odesílá aplikaci pomocí zpráv typu cloud zařízení toohello zařízení prostřednictvím služby IoT Hub a pak přijímá jeho potvrzení o doručení.

> [!NOTE]
> IoT Hub je podpora v sadě SDK pro mnoho zařízení platformy a jazyky (například C, Javy a JavaScriptu) prostřednictvím [sady SDK pro zařízení Azure IoT]. Podrobné pokyny o tom, jak tooconnect kurzu vaše zařízení toothis kódu a obecně tooAzure IoT Hub, najdete v hello [Příručka vývojáře pro službu IoT Hub].
> 
> 

toocomplete tohoto kurzu budete potřebovat hello následující:

* Visual Studio 2015 nebo Visual Studio 2017
* Aktivní účet Azure. (Pokud účet nemáte, můžete si během několika minut vytvořit [bezplatný účet][lnk-free-trial].)

## <a name="receive-messages-in-hello-device-app"></a>Příjem zpráv v aplikaci zařízení hello
V této části upravíte hello zařízení aplikaci, kterou jste vytvořili v [Začínáme se službou IoT Hub] tooreceive zprávy typu cloud zařízení ze služby IoT hub hello.

1. V sadě Visual Studio v hello **SimulatedDevice** projekt, přidejte následující metodu toohello hello **Program** třídy.
   
        private static async void ReceiveC2dAsync()
        {
            Console.WriteLine("\nReceiving cloud toodevice messages from service");
            while (true)
            {
                Message receivedMessage = await deviceClient.ReceiveAsync();
                if (receivedMessage == null) continue;
   
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received message: {0}", Encoding.ASCII.GetString(receivedMessage.GetBytes()));
                Console.ResetColor();
   
                await deviceClient.CompleteAsync(receivedMessage);
            }
        }
   
    Hello `ReceiveAsync` metoda hello přijaté zprávy asynchronně vrátí v čase hello, který je přijatých hello zařízení. Vrátí *null* po specifiable časový limit (v takovém případě se používá výchozí hello minutu). Když aplikace hello obdrží *null*, i nadále toowait nových zpráv. Tento požadavek je hello důvod hello `if (receivedMessage == null) continue` řádku.
   
    Hello volání příliš`CompleteAsync()` IoT Hub upozorní, že hello zpráva byla úspěšně zpracována. uvítací zprávu lze bezpečně odebrat z fronty hello zařízení. Pokud tuto aplikaci zabránila hello zařízení z důvodu hello zpracování zprávy hello se něco stalo, IoT Hub zajišťuje ho znovu. Je důležité zpracování logiky v aplikaci zařízení hello této zprávy *idempotent*tak, aby přijímá hello vytváří vícekrát stejnou zprávu hello stejného výsledku. Aplikace můžete také dočasně abandon zprávu, což vede k zachování uvítací zprávu ve frontě hello pro budoucí používání služby IoT hub. Nebo můžete aplikace hello odmítnout zprávu, která trvale odstraní uvítací zprávu z fronty hello. Další informace o životní cyklus zpráv typu cloud zařízení hello najdete v tématu hello [Příručka vývojáře pro službu IoT Hub][IoT Hub developer guide - C2D].
   
   > [!NOTE]
   > Při použití protokolu HTTP místo MQTT nebo AMQP jako přenosového mechanismu, hello `ReceiveAsync` metoda vrátí okamžitě. vzor Hello podporované pro zprávy typu cloud zařízení s protokolem HTTP je občas připojená zařízení, které zkontrolujte zprávy zřídka (méně než každých 25 minut). Vydání další HTTP přijme výsledky v omezení hello požadavky služby IoT Hub. Další informace o hello rozdíly mezi podpora MQTT, AMQP a HTTP a omezení služby IoT Hub, najdete v části hello [Příručka vývojáře pro službu IoT Hub][IoT Hub developer guide - C2D].
   > 
   > 
2. Přidejte následující metodu v hello hello **hlavní** metoda bezprostředně před hello `Console.ReadLine()` řádku:
   
        ReceiveC2dAsync();

> [!NOTE]
> Pro saké na jednoduchost tento kurz neimplementuje žádné zásady opakování. V produkčním kódu, měli byste implementovat zásady opakování (například exponenciálního omezení rychlosti), dle pokynů v článku na webu MSDN hello [přechodných chyb].
> 
> 

## <a name="send-a-cloud-to-device-message"></a>Odeslání zprávy typu cloud zařízení
V této části napíšete konzolovou aplikaci .NET, která odešle aplikace zařízení toohello zprávy typu cloud zařízení.

1. V aktuálním řešení sady Visual Studio hello, vytvoření projektu Visual C# plochy aplikace pomocí hello **konzolové aplikace** šablona projektu. Název projektu hello **SendCloudToDevice**.
   
    ![Nový projekt v sadě Visual Studio][20]
2. V Průzkumníku řešení klikněte pravým tlačítkem na řešení hello a pak klikněte na tlačítko **spravovat balíčky NuGet pro řešení...** . 
   
    Tato akce otevře hello **spravovat balíčky NuGet** okno.
3. Vyhledejte **Microsoft.Azure.Devices**, klikněte na tlačítko **nainstalovat**a přijměte podmínky použití hello. 
   
    To stáhne, nainstaluje a přidá odkaz toohello [balíček NuGet sady SDK služby Azure IoT].

4. Přidejte následující hello `using` příkaz hello horní části hello **Program.cs** souboru:
   
        using Microsoft.Azure.Devices;
5. Přidejte následující pole toohello hello **Program** třídy. Nahraďte hodnotu zástupného symbolu hello s hello IoT hub připojovacího řetězce z [Začínáme se službou IoT Hub]:
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. Přidejte následující metodu toohello hello **Program** třídy:
   
        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud toodevice message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }
   
    Tato metoda odesílá nové zařízení toohello zpráv typu cloud zařízení s hello ID `myFirstDevice`. Tento parametr změnit, pouze v případě, že se změnil z hello používá v [Začínáme se službou IoT Hub].
7. Nakonec přidejte následující řádky toohello hello **hlavní** metoda:
   
        Console.WriteLine("Send Cloud-to-Device message\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
   
        Console.WriteLine("Press any key toosend a C2D message.");
        Console.ReadLine();
        SendCloudToDeviceMessageAsync().Wait();
        Console.ReadLine();
8. V sadě Visual Studio, klikněte pravým tlačítkem na řešení a vyberte **nastavit projekty po spuštění...** . Vyberte **více projektů po spuštění**, pak vyberte hello **spustit** akce pro **ReadDeviceToCloudMessages**, **SimulatedDevice**, a **SendCloudToDevice**.
9. Stiskněte klávesu **F5**. Všechny tři aplikace by se měl spustit. Vyberte hello **SendCloudToDevice** windows a stiskněte klávesu **Enter**. Měli byste vidět uvítací zprávu přijímání hello zařízení aplikací.
   
   ![Zprávy přijímající aplikace][21]

## <a name="receive-delivery-feedback"></a>Přijímat zpětnou vazbu o doručení
Je možné toorequest doručení (nebo vypršení platnosti) potvrzení ze služby IoT Hub pro každou zprávu cloud zařízení. Tato možnost umožňuje hello řešení back-end tooeasily informujte opakovat, nebo kompenzace logiku. Další informace o zpětné vazbě z cloudu do zařízení, najdete v části hello [Příručka vývojáře pro službu IoT Hub][IoT Hub developer guide - C2D].

V této části upravíte hello **SendCloudToDevice** toorequest zpětné vazby aplikace a přijímat ze služby IoT Hub.

1. V sadě Visual Studio v hello **SendCloudToDevice** projekt, přidejte následující metodu toohello hello **Program** třídy.
   
        private async static void ReceiveFeedbackAsync()
        {
            var feedbackReceiver = serviceClient.GetFeedbackReceiver();
   
            Console.WriteLine("\nReceiving c2d feedback from service");
            while (true)
            {
                var feedbackBatch = await feedbackReceiver.ReceiveAsync();
                if (feedbackBatch == null) continue;
   
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received feedback: {0}", string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
                Console.ResetColor();
   
                await feedbackReceiver.CompleteAsync(feedbackBatch);
            }
        }
   
    Poznámka: Tento vzor receive je hello stejné jeden použité tooreceive zprávy typu cloud zařízení z aplikace hello zařízení.
2. Přidejte následující metodu v hello hello **hlavní** metoda hned po hello `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` řádku:
   
        ReceiveFeedbackAsync();
3. zpětná vazba toorequest hello doručení zprávy typu cloud zařízení, máte toospecify vlastností v hello **SendCloudToDeviceMessageAsync** metoda. Přidejte následující řádek, přímo po hello hello `var commandMessage = new Message(...);` řádku:
   
        commandMessage.Ack = DeliveryAcknowledgement.Full;
4. Spuštění aplikace hello stisknutím **F5**. Měli byste vidět všechny tři aplikace spustit. Vyberte hello **SendCloudToDevice** windows a stiskněte klávesu **Enter**. Měli byste vidět hello zprávy přijímání aplikace hello zařízení a za několik sekund, hello přijímá zprávy zpětnou vazbu vaše **SendCloudToDevice** aplikace.
   
   ![Zprávy přijímající aplikace][22]

> [!NOTE]
> Pro saké na jednoduchost tento kurz neimplementuje žádné zásady opakování. V produkčním kódu, měli byste implementovat zásady opakování (například exponenciálního omezení rychlosti), dle pokynů v článku na webu MSDN hello [přechodných chyb].
> 
> 

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste se naučili jak toosend a příjem zpráv typu cloud zařízení. 

Příklady toosee dokončení začátku do konce řešení, které pomocí služby IoT Hub, viz [Azure IoT Suite].

toolearn Další informace o vývoji řešení službou IoT Hub, najdete v části hello [Příručka vývojáře pro službu IoT Hub].

<!-- Images -->
[20]: ./media/iot-hub-csharp-csharp-c2d/create-identity-csharp1.png
[21]: ./media/iot-hub-csharp-csharp-c2d/sendc2d1.png
[22]: ./media/iot-hub-csharp-csharp-c2d/sendc2d2.png

<!-- Links -->

[balíček NuGet sady SDK služby Azure IoT]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[přechodných chyb]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md

[Příručka vývojáře pro službu IoT Hub]: iot-hub-devguide.md
[Začínáme se službou IoT Hub]: iot-hub-csharp-csharp-getstarted.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure IoT Suite]: https://docs.microsoft.com/en-us/azure/iot-suite/
[sady SDK pro zařízení Azure IoT]: iot-hub-devguide-sdks.md