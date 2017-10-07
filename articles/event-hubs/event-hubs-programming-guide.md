---
title: "Průvodce aaaProgramming pro Azure Event Hubs | Microsoft Docs"
description: "Zapište kód pro Azure Event Hubs pomocí hello Azure .NET SDK."
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 64cbfd3d-4a0e-4455-a90a-7f3d4f080323
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/17/2017
ms.author: sethm
ms.openlocfilehash: 43bebd126c2311af9e3daeb52324132b66cf0884
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-programming-guide"></a>Průvodce programováním pro službu Event Hubs

Tento článek popisuje některé běžné scénáře v psaní kódu pomocí služby Azure Event Hubs a hello Azure .NET SDK. Předpokládá se předběžná znalost služby Event Hubs. Koncepční přehled služby Event Hubs naleznete v části hello [Přehled služby Event Hubs](event-hubs-what-is-event-hubs.md).

## <a name="event-publishers"></a>Zdroje událostí

Odešlete centra událostí tooan události buď pomocí HTTP POST, nebo prostřednictvím připojení protokolu AMQP 1.0. Hello volbu které toouse a kdy závisí na konkrétním adresovaném scénáři hello. Připojení protokolu AMQP 1.0 se měří jako zprostředkovaná připojení ve službě Service Bus. Díky tomu, že poskytují trvalý kanál pro zasílání zpráv, jsou vhodnější ve scénářích, kde se počítá s častými vysokými objemy zpráv a vyžaduje se nižší latence.

Můžete vytvořit a spravovat služby Event Hubs pomocí hello [NamespaceManager][] třídy. Když pomocí rozhraní .NET hello spravovaná rozhraní API, hello primární vytvoří pro publikování dat tooEvent rozbočovače jsou hello [EventHubClient](/dotnet/api/microsoft.servicebus.messaging.eventhubclient) a [EventData][] třídy. [EventHubClient][] poskytuje komunikační kanál AMQP hello za které se události posílají toohello centra událostí. Hello [EventData][] třída představuje událost a je centra událostí tooan použité toopublish zprávy. Tato třída obsahuje hello tělo, některá metadata a záhlaví s informacemi o události hello. Další vlastnosti se přidají toohello [EventData][] objektu při průchodu centra událostí.

## <a name="get-started"></a>Začínáme

Hello rozhraní .NET třídy, které podporují službu Event Hubs jsou uvedeny v hello Microsoft.ServiceBus.dll sestavení. Hello nejjednodušší způsob, jak tooreference hello rozhraní API služby Service Bus a tooconfigure aplikaci se všemi závislostmi služby Service Bus hello služby je toodownload hello [balíček Service Bus NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus). Alternativně můžete použít hello [Konzola správce balíčků](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) v sadě Visual Studio. toodo tedy vydat hello následující příkaz v hello [Konzola správce balíčků](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) okno:

```
Install-Package WindowsAzure.ServiceBus
```

## <a name="create-an-event-hub"></a>Vytvoření centra událostí
Můžete použít hello [NamespaceManager][] třídy toocreate Event Hubs. Například:

```csharp
var manager = new Microsoft.ServiceBus.NamespaceManager("mynamespace.servicebus.windows.net");
var description = manager.CreateEventHub("MyEventHub");
```

Ve většině případů je doporučeno používat hello [CreateEventHubIfNotExists][] metody tooavoid generování výjimek v případě restartování služby hello. Například:

```csharp
var description = manager.CreateEventHubIfNotExists("MyEventHub");
```

Všechny operace vytvoření služby Event Hubs, včetně [CreateEventHubIfNotExists][], vyžadují **spravovat** oprávnění hello oboru názvů. Pokud chcete toolimit hello oprávnění vydavatele nebo přijímajících aplikací, můžete zamezit volání operací vytváření v produkčním kódu použitím přihlašovacích údajů s omezenými oprávněními.

Hello [EventHubDescription](/dotnet/api/microsoft.servicebus.messaging.eventhubdescription) třída obsahuje podrobnosti o Centru událostí, včetně hello autorizační pravidla, intervalu uchování zpráv hello, ID oddílu, stav a cestu. Tato třída tooupdate hello metadata můžete použít v Centru událostí.

## <a name="create-an-event-hubs-client"></a>Vytvoření klienta pro centra událostí (Event Hubs)
Hello primární třídou pro interakci s centry událostí je [Microsoft.ServiceBus.Messaging.EventHubClient][EventHubClient]. Tato třída poskytuje možnosti jak pro odesílatele, tak pro příjemce. Můžete vytvořit instanci této třídy pomocí hello [vytvořit](/dotnet/api/microsoft.servicebus.messaging.eventhubclient.create) metoda, jak ukazuje následující příklad hello.

```csharp
var client = EventHubClient.Create(description.Path);
```

Tato metoda používá informace o připojení k Service Bus hello v souboru App.config hello ve hello `appSettings` části. Příklad hello `appSettings` XML používá informace o připojení k Service Bus hello toostore, naleznete v dokumentaci k hello hello [Microsoft.ServiceBus.Messaging.EventHubClient.Create(System.String)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Create_System_String_) metoda.

Další možností je toocreate hello klienta z připojovacího řetězce. Tato možnost dobře funguje při použití rolí pracovního procesu systému Azure, protože řetězec hello můžete uložit ve vlastnosti konfigurace hello hello pracovního procesu. Například:

```csharp
EventHubClient.CreateFromConnectionString("your_connection_string");
```

Hello připojovací řetězec bude mít stejný formát, jak se objevuje v souboru App.config hello u předchozích metod hello hello:

```
Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[key]
```

Nakonec je také možné toocreate [EventHubClient][] objektu z [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) instance, jak ukazuje následující příklad hello.

```csharp
var factory = MessagingFactory.CreateFromConnectionString("your_connection_string");
var client = factory.CreateEventHubClient("MyEventHub");
```

Je důležité toonote, že další [EventHubClient][] objekty vytvořené z zasílání zpráv instance objektu factory bude používat hello stejné základní připojení protokolu TCP. Tyto objekty tudíž mají omezení propustnosti na straně klienta. Hello [vytvořit](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Create_System_String_) metoda opětovně používá jedinou instanci messagingfactory. Pokud u jednoho odesílatele potřebujete vysokou propustnost, tak můžete vytvořit více instancí MessagingFactory a jeden objekt [EventHubClient][] z každé z nich.

## <a name="send-events-tooan-event-hub"></a>Odeslat centra událostí tooan události
Odeslání události centra událostí tooan vytvořením [EventData][] instance a odesláním prostřednictvím hello [odeslat](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Send_Microsoft_ServiceBus_Messaging_EventData_) metoda. Tato metoda přebírá jediný [EventData][] parametr instance a synchronně ho odesílá tooan centra událostí.

## <a name="event-serialization"></a>Serializace událostí
Hello [EventData][] třída má [čtyři přetížené konstruktory](/dotnet/api/microsoft.servicebus.messaging.eventdata#constructors_) které přebírají různé parametry, například objekt a serializátor, pole bajtů nebo datový proud. Je také možné tooinstantiate hello [EventData][] a nastavit datový proud obsahu hello později. Při použití formátu JSON s [EventData][], můžete použít **Encoding.UTF8.GetBytes()** tooretrieve hello pole bajtů řetězce kódovaného ve formátu JSON.

## <a name="partition-key"></a>Klíč oddílu
Hello [EventData][] třída má [PartitionKey][] vlastnost, která umožňuje toospecify odesílatele hello hodnotu, která je použita hodnota hash tooproduce přiřazení k oddílu. Použití klíče oddílu zajišťuje všechny hello události se stejným klíčem odešlou hello toohello stejné oddílu v Centru událostí hello. Běžné klíče oddílů zahrnují ID relace uživatele a jedinečné ID odesílatele. Hello [PartitionKey][] vlastnost je volitelná a lze zadat při použití hello [Microsoft.ServiceBus.Messaging.EventHubClient.Send(Microsoft.ServiceBus.Messaging.EventData)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Send_Microsoft_ServiceBus_Messaging_EventData_) nebo [Microsoft.ServiceBus.Messaging.EventHubClient.SendAsync(Microsoft.ServiceBus.Messaging.EventData)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_SendAsync_Microsoft_ServiceBus_Messaging_EventData_) metody. Pokud nezadáte hodnotu [PartitionKey][], odeslané události jsou distribuované toopartitions pomocí modelu kruhového dotazování.

### <a name="availability-considerations"></a>Aspekty dostupnosti

Použitím klíče oddílu je volitelný a měli byste pečlivě zvážit, jestli toouse jeden. V mnoha případech použitím klíče oddílu je vhodný, pokud řazení událostí je důležité. Pokud použijete klíč oddílu, tyto oddíly vyžadují dostupnosti v jednom uzlu, a můžete výpadků časem; například se při výpočetní uzly restartování a opravy. Jako takový Pokud nastavíte ID oddílu a oddílu z nějakého důvodu nedostupný, pokusu o tooaccess hello data v tomto oddílu se nezdaří. Pokud je nejdůležitější vysokou dostupnost, nezadávejte klíč oddílu; události v takovém případě budou odeslány toopartitions pomocí modelu kruhového dotazování hello popsané. V tomto scénáři provádíte explicitní volbu mezi dostupnosti (žádné ID oddílu) a konzistence (Připnutí ID oddílu tooa událostí).

Potřeba vzít v úvahu zpracovává zpoždění při zpracování události. V některých případech se může být lepší toodrop dat a opakujte než tootry a přečtěte si o zpracování, které mohou způsobit zpoždění další zpracování příjmu dat. Například s běžícími je lepší toowait pro dokončení aktuální data, ale v živé konverzace nebo VOIP scénář hello dat byste měli místo rychle, i když není úplný.

Zadaný těchto aspektů dostupnosti v těchto scénářích můžete vybrat jednu z následujících strategií zpracování chyb hello:

- Zastavení (Zastavit čtení ze služby Event Hubs, dokud se nevyřeší věcí)
- Vyřaďte (zprávy nejsou důležité, jejich umístění)
- Opakujte (hello opakování, které vložit jak můžete vidět, zprávy)
- [Nedoručených zpráv](../service-bus-messaging/service-bus-dead-letter-queues.md) (použití fronty nebo jiné toodead centra událostí písmeno jen hello zprávy, které nelze zpracovat)

Další informace a diskuzi o hello kompromis mezi dostupnosti a konzistence najdete v tématu [dostupnosti a konzistence v Event Hubs](event-hubs-availability-and-consistency.md). 

## <a name="batch-event-send-operations"></a>Dávkové operace odesílání událostí
Odesílání událostí v dávkách (batch) může výrazně zvýšit propustnost. Hello [SendBatch](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_SendBatch_System_Collections_Generic_IEnumerable_Microsoft_ServiceBus_Messaging_EventData__) metoda trvá **rozhraní IEnumerable** parametr typu [EventData][] a hello odesílá celý batch jako centra událostí toohello atomické operace.

```csharp
public void SendBatch(IEnumerable<EventData> eventDataList);
```

Všimněte si, že jeden batch nesmí překročit hello 256 KB omezení pro událost. Kromě toho každá zpráva v dávce hello používá hello stejnou identitu zdroje. Je zodpovědností hello tooensure hello odesílatele, který hello batch nepřekračuje hello maximální velikost události. V případě překročení se u klienta vygeneruje chyba odeslání (**Send**).

## <a name="send-asynchronously-and-send-at-scale"></a>Asynchronní odesílání a škálované odesílání
Můžete také odeslat centra událostí tooan události asynchronně. Asynchronní odesílání může zvýšit rychlost Dobrý den, kdy je klient schopný toosend události. Obě hello [odeslat](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Send_Microsoft_ServiceBus_Messaging_EventData_) a [SendBatch](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_SendBatch_System_Collections_Generic_IEnumerable_Microsoft_ServiceBus_Messaging_EventData__) metody jsou k dispozici v asynchronních verzích, které vracejí [úloh](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) objektu. Když tato technika může zvýšit propustnost, se může také způsobit hello události toosend toocontinue klienta i když ho po omezení ze strany hello služby Event Hubs a můžou vést k selhání hello klienta nebo ztrátě zpráv, pokud není implementovaná správně. Kromě toho můžete použít hello [RetryPolicy](/dotnet/api/microsoft.servicebus.messaging.cliententity#Microsoft_ServiceBus_Messaging_ClientEntity_RetryPolicy) vlastnost na možnosti opakování hello klienta toocontrol klienta.

## <a name="create-a-partition-sender"></a>Vytvoření odesílatele na oddíl
Přestože je nejběžnější toosend události tooan centra událostí bez klíče oddílu, v některých případech můžete toosend události přímo tooa zadaný oddíl. Například:

```csharp
var partitionedSender = client.CreatePartitionedSender(description.PartitionIds[0]);
```

[CreatePartitionedSender](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_CreatePartitionedSender_System_String_) vrátí [EventHubSender](/dotnet/api/microsoft.servicebus.messaging.eventhubsender) objektů, které můžete použít oddílu centra toopublish události tooa konkrétní události.

## <a name="event-consumers"></a>Příjemci událostí
Služba Event Hubs má dva primární modely příjmu událostí: přímé příjemce a abstrakce vyšší úrovně, jako je například [EventProcessorHost][]. Přímých příjemců zodpovídají za vlastní koordinaci toopartitions přístup v rámci skupiny příjemců.

### <a name="direct-consumer"></a>Přímý příjemce
Hello Nejpřímějším způsobem tooread z oddílu v rámci skupiny příjemců je toouse hello [EventHubReceiver](/dotnet/apie/microsoft.servicebus.messaging.eventhubreceiver) třídy. toocreate na instance této třídy, musíte použít instanci hello [EventHubConsumerGroup](/dotnet/api/microsoft.servicebus.messaging.eventhubconsumergroup) třídy. V následujícím příkladu hello ID oddílu hello je povinný při vytváření příjemce hello pro skupiny uživatelů hello.

```csharp
EventHubConsumerGroup group = client.GetDefaultConsumerGroup();
var receiver = group.CreateReceiver(client.GetRuntimeInformation().PartitionIds[0]);
```

Hello [CreateReceiver](/dotnet/api/microsoft.servicebus.messaging.eventhubconsumergroup#methods_summary) metoda má několik přetížení, které usnadňují kontrolu nad hello vytvářeného čtenáře. Tyto metody zahrnují určení posunu jako řetězce nebo časového razítka a hello možnost toospecify jestli tooinclude tento zadaný posun v hello datového proudu, nebo po jeho spuštění. Jakmile vytvoříte příjemce hello, můžete začít přijímat události prostřednictvím hello vrátil objekt. Hello [Receive](/dotnet/api/microsoft.servicebus.messaging.eventhubreceiver#methods_summary) metoda má čtyři přetížení tento ovládací prvek hello přijímat parametry operace, jako je například velikost dávky a doba čekání. Můžete použít asynchronní verze těchto metod tooincrease hello propustnost příjemce hello. Například:

```csharp
bool receive = true;
string myOffset;
while(receive)
{
    var message = receiver.Receive();
    myOffset = message.Offset;
    string body = Encoding.UTF8.GetString(message.GetBytes());
    Console.WriteLine(String.Format("Received message offset: {0} \nbody: {1}", myOffset, body));
}
```

S ohledem tooa konkrétního oddílu jsou přijaty zprávy hello v hello pořadí, ve které byly odeslány toohello centra událostí. Posun Hello je řetězec tokenu použité tooidentify zprávy v oddílu.

Všimněte si, že oddíl v rámci skupiny příjemců neumožňuje v žádné chvíli připojení více než pěti souběžných čtenářů. Jak se příjemci různě připojují a odpojují, mohou jejich relace zůstat aktivní pro několik minut, než služba hello rozpozná, že se odpojili. Během této doby může selhat připojení tooa oddílu. Kompletní příklad, jak napsat přímého příjemce pro službu Event Hubs, najdete v části hello [přímí příjemci centra událostí](https://code.msdn.microsoft.com/Event-Hub-Direct-Receivers-13fa95c6) ukázka.

### <a name="event-processor-host"></a>EventProcessorHost
Hello [EventProcessorHost][] třída zpracovává data ze služby Event Hubs. Tuto implementaci byste měli používat při vytváření čtenářů událostí na platformě .NET hello. Třída [EventProcessorHost][] poskytuje pro implementace zpracovatelů událostí bezpečné prostředí runtime, které umožňuje bezpečné použití vláken a více procesů. Taky poskytuje možnost vytváření kontrolních bodů a správy „půjčování“ oddílu.

toouse hello [EventProcessorHost][] třídu, můžete implementovat [IEventProcessor](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor). Toto rozhraní obsahuje tři metody:

* [OpenAsync](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor#Microsoft_ServiceBus_Messaging_IEventProcessor_OpenAsync_Microsoft_ServiceBus_Messaging_PartitionContext_)
* [CloseAsync](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor#Microsoft_ServiceBus_Messaging_IEventProcessor_CloseAsync_Microsoft_ServiceBus_Messaging_PartitionContext_Microsoft_ServiceBus_Messaging_CloseReason_)
* [ProcessEventsAsync](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor#Microsoft_ServiceBus_Messaging_IEventProcessor_ProcessEventsAsync_Microsoft_ServiceBus_Messaging_PartitionContext_System_Collections_Generic_IEnumerable_Microsoft_ServiceBus_Messaging_EventData__)

zpracování událostí toostart doložit [EventProcessorHost][], poskytuje hello vhodné parametry pro vaše Centrum událostí. Potom zavolejte [RegisterEventProcessorAsync](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost#Microsoft_ServiceBus_Messaging_EventProcessorHost_RegisterEventProcessorAsync__1) tooregister vaše [IEventProcessor](/dotnet/api/microsoft.servicebus.messaging.ieventprocessor) implementace s hello runtime. V tomto okamžiku hello hostitele pokusí tooacquire zapůjčení každý oddíl v Centru událostí hello použitím "chamtivého" algoritmu. Tato zapůjčení vydrží po stanovenou dobu a následně musí být obnovena. Jako nové uzly a instancí pracovního procesu v takovém případě režimu online, se umístí své rezervace zapůjčení a časem hello zatížení posune mezi uzly jako všechny pokusy o tooacquire další zapůjčení.

![Event Processor Host](./media/event-hubs-programming-guide/IC759863.png)

Postupem času se dosáhne rovnováhy. Tato dynamická funkce umožňuje tooconsumers toobe použít automatické škálování na základě využití procesoru pro škálování nahoru a dolů. Protože Event Hubs nemají přímým konceptem počítání zpráv, průměrné využití procesoru je často hello nejlepší mechanismus toomeasure back end nebo škálování příjemců. Když začnou zdroje toopublish, kterou může zpracovat další události, než příjemce, může být hello zvýšení využití procesoru na spotřebitele toocause použité k automatickému škálování počtu instancí pracovních procesů.

Hello [EventProcessorHost][] taky implementuje mechanismus Azure na základě úložiště vytváření kontrolních bodů. Tento mechanismus hello ukládá posun na bázi oddílu, tak, aby každý příjemce můžete určit, jaké hello poslední kontrolní bod předchozího příjemce hello se. Stejně jako oddíly přechod mezi uzly prostřednictvím zapůjčení, to je hello synchronizační mechanismus, který usnadňuje přesun zátěže.

## <a name="publisher-revocation"></a>Odvolání zdroje
Kromě toho toohello pokročilé funkce běhové [EventProcessorHost][], umožňuje Služba Event Hubs odvolání zdroje v pořadí tooblock konkrétním zdrojům možnost odesílat události tooan události rozbočovače. Tyto funkce jsou zvláště užitečné, pokud došlo k ohrožení zabezpečení token vydavatele, nebo aktualizace softwaru je příčinou je toobehave nesprávně. V těchto situacích může být hello vydavatele identitu, která je součástí jeho tokenu SAS, zablokováno publikování událostí.

Další informace o odvolání zdroje a o tom, jak toosend tooEvent Hubs jako vydavatel, najdete v části hello [události rozbočovače velké škálování zabezpečeného publikování](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab) ukázka.

## <a name="next-steps"></a>Další kroky
toolearn Další informace o scénářích služby Event Hubs naleznete pod těmito odkazy:

* [Přehled rozhraní API služby Event Hubs](event-hubs-api-overview.md)
* [Co je Služba Event Hubs](event-hubs-what-is-event-hubs.md)
* [Dostupnost a konzistence ve službě Event Hubs](event-hubs-availability-and-consistency.md)
* [Referenční dokumentace rozhraní API třídy EventProcessorHost](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)

[NamespaceManager]: /dotnet/api/microsoft.servicebus.namespacemanager
[EventHubClient]: /dotnet/api/microsoft.servicebus.messaging.eventhubclient
[EventData]: /dotnet/api/microsoft.servicebus.messaging.eventdata
[CreateEventHubIfNotExists]: /dotnet/api/microsoft.servicebus.namespacemanager.createeventhubifnotexists
[PartitionKey]: /dotnet/api/microsoft.servicebus.messaging.eventdata#Microsoft_ServiceBus_Messaging_EventData_PartitionKey
[EventProcessorHost]: /dotnet/api/microsoft.servicebus.messaging.eventprocessorhost
