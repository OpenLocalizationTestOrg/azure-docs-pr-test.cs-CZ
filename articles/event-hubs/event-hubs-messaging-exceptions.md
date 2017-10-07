---
title: "zasílání zpráv výjimky aaaAzure Event Hubs | Microsoft Docs"
description: "Seznam výjimky a doporučené postupy pro zasílání zpráv Azure Event Hubs."
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 2c6273de-0106-47e5-b45d-59040e51f2c5
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: 9c164e76612c26607219f08407f689aaab4050a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-messaging-exceptions"></a>Výjimky zasílání zpráv služby Event Hubs
V tomto článku jsou uvedeny některé hello výjimky generované hello Azure Service Bus zasílání zpráv na úrovni rozhraní API, které zahrnují Event Hubs. Tento odkaz je toochange subjektu, tak to zkuste znovu aktualizací.

## <a name="exception-categories"></a>Výjimka kategorie
Hello rozhraní API centra událostí generovat výjimky, které můžete spadají do následujících kategorií hello, společně s hello přidružené akce může trvat tootry toofix je.

1. Uživatel kódování Chyba: [System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [ System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx). Obecné akce: Zkuste toofix hello kódu než budete pokračovat.
2. Chyba instalace/konfigurace: [Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception), [Microsoft.Azure.EventHubs.MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception), [ System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). Obecné akce: Zkontrolujte konfiguraci a v případě potřeby změnit.
3. Přechodný výjimkami: [Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](#serverbusyexception), [ Microsoft.Azure.EventHubs.ServerBusyException](#serverbusyexception), [Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception). Obecné akce: operaci hello nebo oznámit uživatelům.
4. Ostatní výjimky: [System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](#timeoutexception), [Microsoft.ServiceBus.Messaging.MessageLockLostException](/dotnet/api/microsoft.servicebus.messaging.messagelocklostexception), [Microsoft.ServiceBus.Messaging.SessionLockLostException](/dotnet/api/microsoft.servicebus.messaging.sessionlocklostexception). Obecné akce: typ výjimky konkrétní toohello; naleznete v tabulce toohello v následující části hello. 

## <a name="exception-types"></a>Typy výjimek
Hello následující tabulka uvádí typy výjimek zasílání zpráv a jejich příčiny a poznámky navrhovaná akce, které můžete provést.

| **Typ výjimky** | **Popis nebo příčina/příklady** | **Navrhovaná akce** | **Poznámka: na automatické, okamžitou opakování** |
| --- | --- | --- | --- |
| [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |Hello serveru nereagovala toohello požadovaná operace v rámci hello zadaný čas, který je kontrolován [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout). Hello server může dokončili hello požadovanou operaci. K tomu dochází z důvodu toonetwork nebo jiné infrastruktury zpoždění. |Zkontrolujte stav systému hello konzistence a opakujte v případě potřeby.<br /> V tématu [TimeoutException](#timeoutexception). |Opakování vám může pomoci v některých případech; Přidejte logiku toocode opakování. |
| [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |Hello požadovaného uživatele operace není povolena v rámci hello server nebo službu. Zobrazí se zpráva o výjimce hello podrobnosti. Například [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) vygeneruje výjimku, pokud byla přijata zpráva hello v [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) režimu. |Zkontrolujte hello kód a dokumentace hello. Zkontrolujte, zda text hello požadovaná operace je platná. |Opakování nepomůže. |
| [OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Pokus o je provedené tooinvoke operace u objektu, který již byl uzavřen, byl zrušen nebo zlikvidován. Ve výjimečných případech je již zveřejněn hello vedlejším transakce. |Zkontrolujte hello kódu a ujistěte se, že nevyvolá operací na objekt uvolněné. |Opakování nepomůže. |
| [UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |Hello [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) objekt se nedá získat token, hello token je neplatný nebo hello token neobsahuje hello deklarace identity požadované tooperform hello operaci. |Ujistěte se, že se vytvoří zprostředkovatel tokenu hello s hello správné hodnoty. Zkontrolujte konfiguraci hello hello služby Řízení přístupu. |Opakování vám může pomoci v některých případech; Přidejte logiku toocode opakování. |
| [ArgumentException –](https://msdn.microsoft.com/library/system.argumentexception.aspx)<br /> [ArgumentNullException](https://msdn.microsoft.com/library/system.argumentnullexception.aspx)<br />[Výjimka ArgumentOutOfRangeException](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |Jeden nebo více metoda toohello argumenty jsou neplatné. Hello URI zadaný příliš[NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) nebo [vytvořit](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_Create_System_Collections_Generic_IEnumerable_System_Uri__) obsahuje segment(s) cesta. zadané schéma identifikátoru URI Hello příliš[NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) nebo [vytvořit](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_Create_System_Collections_Generic_IEnumerable_System_Uri__) je neplatný. Hodnota vlastnosti Hello je větší než 32KB. |Volání kódu hello a ujistěte se, že hello argumenty jsou správné. |Opakování nepomůže. |
| [Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception) <br /> [Microsoft.Azure.EventHubs.MessagingEntityNotFoundException](/dotnet/api/microsoft.azure.eventhubs.messagingentitynotfoundexception) |Entity přidružené hello operaci neexistuje nebo byla odstraněna. |Zkontrolujte, zda text hello entita existuje. |Opakování nepomůže. |
| [MessageNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagenotfoundexception) |Byl proveden pokus tooreceive zprávu s číslem konkrétní pořadí. Tato zpráva nebyla nalezena. |Ujistěte se, že již nebyla přijata zpráva hello. Toosee fronty nedoručených zpráv hello zkontrolujte, zda byl deadlettered uvítací zprávu. |Opakování nepomůže. |
| [MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) |Klient není možné tooestablish tooEvent připojení rozbočovače. |Zajistěte, aby hello zadaný název hostitele je správný, a hostitel hello je dostupný. |Opakování mohou pomoci, pokud existují problémy s občasným připojením. |
| [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) <br /> [Microsoft.Azure.EventHubs.ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) |Služba není možné tooprocess hello žádosti v tuto chvíli. |Klienta můžete čekat na určitou dobu a potom opakujte operaci hello. <br /> V tématu [ServerBusyException](#serverbusyexception). |Klient může po určité době opakujte. Pokud opakování výsledkem různé výjimky, zkontrolujte opakování chování této výjimky. |
| [MessageLockLostException](/dotnet/api/microsoft.servicebus.messaging.messagelocklostexception) |Vypršela platnost tokenu zámku přidružené k uvítací zprávu nebo hello zámku token nebyl nalezen. |Dispose uvítací zprávu. |Opakování nepomůže. |
| [SessionLockLostException](/dotnet/api/microsoft.servicebus.messaging.sessionlocklostexception) |Zámek přidružené k této relaci se ztratí. | Abort – hello [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) objektu. |Opakování nepomůže. |
| [MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception) |Obecná zasílání zpráv výjimka, která může být vyvolána v následujících případech hello: je proveden pokus o toocreate [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) pomocí názvu nebo cestu, která patří tooa jiné entity typu (například téma). Pokus o je provedené toosend zprávu větší než 256KB. Hello server nebo služby došlo k chybě během zpracování požadavku hello. Podrobnosti viz zpráva o výjimce hello podrobnosti. Obvykle se jedná o přechodný výjimku. |Zkontrolujte hello kódu a ujistěte se, že pouze serializovatelné objekty se používají pro tělo zprávy hello (nebo použít vlastní serializátor). Podívejte se do dokumentace hello hello podporované typy hodnot vlastností hello a pouze pomocí podporované typy. Zkontrolujte hello [IsTransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception#Microsoft_ServiceBus_Messaging_MessagingException_IsTransient) vlastnost. Pokud je **true**, můžete opakovat operace hello. |Postup pro opakované není definován a nemusí pomoci. |
| [MessagingEntityAlreadyExistsException](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) |Byl proveden pokus toocreate entitu s názvem, který je již využíván jinou entitou v daném oboru názvů služby. |Odstraňte stávající entity hello nebo zvolte jiný název pro toobe entity hello vytvořili. |Opakování nepomůže. |
| [Quotaexceededexception –](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) |Hello entity pro zasílání zpráv bylo dosaženo jeho maximální povolenou velikost. To může dojít, pokud již byla otevřena hello maximální počet příjemců (což je 5) na úrovni skupiny-příjemce. |Vytvoření prostoru v entitě hello příjmu zpráv ze hello entity nebo jeho dílčí fronty. <br /> V tématu [quotaexceededexception –](#quotaexceededexception) |Pokud zprávy byla odebrána v hello té doby, mohou pomoci opakování. |
|  | | | |
| [SessionCannotBeLockedException](/dotnet/api/microsoft.servicebus.messaging.sessioncannotbelockedexception) |Byl proveden pokus tooaccept, který relaci s ID konkrétní relace, ale relace hello je aktuálně uzamčen jiným klientem. |Zajistěte, aby se ostatní klienti odemkne hello relace. |Opakování mohou pomoci, pokud hello relace byla vydána v mezičase hello. |
| [TransactionSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.transactionsizeexceededexception) |Příliš mnoho operace jsou součástí hello transakce. |Snižte počet hello operací, které jsou součástí této transakce. |Opakování nepomůže. |
| [MessagingEntityDisabledException](/dotnet/api/microsoft.servicebus.messaging.messagingentitydisabledexception) |Žádost o operaci modul runtime na zakázané entity. |Aktivujte hello entity. |Opakování mohou pomoci, pokud byla aktivována hello entity v mezičase hello. |
| [Microsoft.ServiceBus.Messaging.MessageSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) <br /> [Microsoft.Azure.EventHubs.MessageSizeExceededException](/dotnet/api/microsoft.azure.eventhubs.messagesizeexceededexception) | Datovou část zprávy překračuje limit hello 256 kB. Všimněte si, že je tento limit hello 256 kB hello celková velikost zpráv, které mohou zahrnovat vlastnosti systému a všechny režijní náklady na rozhraní .NET. |Snížení velikosti hello datovou část zprávy hello a opakujte operaci hello. |Opakování nepomůže. |
| [TransactionException –](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx) |Hello vedlejším transakce (*Transaction.Current*) je neplatný. Může být dokončit nebo přerušena. Další informace mohou poskytnout informacích o vnitřní výjimce. | |Opakování nepomůže. |
| [Transactionindoubtexception –](https://msdn.microsoft.com/library/system.transactions.transactionindoubtexception.aspx) |Pokus o operaci v transakci, která je nejistá, nebo je proveden pokus o toocommit hello transakce a hello transakce stane nejistých. |Aplikace musí zpracovávat výjimku (jako ve speciálním případě), jak hello transakce může již byly potvrzeny. |- |

## <a name="quotaexceededexception"></a>Quotaexceededexception –
[Quotaexceededexception –](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) označuje, že byla překročena kvóta pro konkrétní entitu.

To může dojít, pokud již byla otevřena hello maximální počet příjemců (5) na úrovni skupiny-příjemce.

### <a name="event-hubs"></a>Event Hubs
Centra událostí může obsahovat maximálně 20 skupiny příjemců za centra událostí. Když zkusíte toocreate další, zobrazí se [quotaexceededexception –](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception). 

## <a name="timeoutexception"></a>TimeoutException
A [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) označuje, že operace se uživatel spustil trvá déle, než časový limit operace hello. 

Pro službu Event Hubs je hello časový limit buď jako součást hello připojovací řetězec, nebo prostřednictvím [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.servicebus.servicebusconnectionstringbuilder). chybová zpráva Hello samotné se můžou lišit, ale vždy obsahuje hodnotu časového limitu hello zadaný pro aktuální operaci hello. 

### <a name="common-causes"></a>Běžné příčiny
Existují dvě běžné příčiny této chyby: nesprávné konfigurace nebo k přechodné chybě.

1. **Nesprávná konfigurace** hello časový limit operace může být příliš malá pro provozní podmínky hello. časový limit operace hello v klientovi hello SDK Hello výchozí hodnota je 60 sekund. Zkontrolujte, zda toosee hello hodnota má váš kód nastavit toosomething příliš malá. Všimněte si, že podmínka hello hello sítě a využití procesoru může ovlivnit hello doba potřebná pro toocomplete určité operace, by neměl být nastavený časový limit operace hello tooa velmi nízkou hodnotu.
2. **Přechodné chybě** někdy hello služby Event Hubs můžete zaznamenat zpoždění při zpracování požadavků, například během období intenzivní provoz. V takových případech může pokus zopakovat operaci po prodlevě, dokud nebude úspěšná operace hello. Jestliže hello stejné operace stále po několika pokusech, navštivte hello [služba Azure stav lokality](https://azure.microsoft.com/status/) toosee, pokud nejsou žádné známé výpadky.

## <a name="serverbusyexception"></a>ServerBusyException

A [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) nebo [Microsoft.Azure.EventHubs.ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) označuje, že je serveru přetížena. Existují dva kódy chyb relevantní pro tuto výjimku.

### <a name="error-code-50002"></a>Kód chyby 50002

Této chybě může dojít pro jednu ze dvou důvodů:

1. Hello zatížení není rovnoměrně mezi všechny oddíly v hello centra událostí a jeden oddíl přístupů hello místní propustnost jednotky omezení.
    
    Řešení: Úprava hello oddílu distribuční strategie nebo pokusu o [EventHubClient.Send(eventDataWithOutPartitionKey)](/dotnet/api/microsoft.servicebus.messaging.eventhubclient#Microsoft_ServiceBus_Messaging_EventHubClient_Send_Microsoft_ServiceBus_Messaging_EventData_) vám může pomoci.

2. obor názvů služby Event Hubs Hello nemá dostatečná jednotky propustnosti (můžete zkontrolovat hello **metriky** okno v okně oboru názvů služby Event Hubs v hello [portál Azure](https://portal.azure.com) tooconfirm). Všimněte si, že hello portálu zobrazují informace agregované (1 min), ale jsme měření propustnosti hello v reálném čase – tak, aby byl pouze odhad.

    Řešení: Zvýšení propustnosti hello, které může pomoci jednotky v oboru názvů hello. Můžete to provést u hello portálu hello **škálování** okno okna oboru názvů služby Event Hubs hello.

### <a name="error-code-50001"></a>Kód chyby 50001

Tato chyba by měl dochází jen zřídka. Ho se stane, když hello kontejneru spuštění kódu pro obor názvů je nedostatek procesoru – více než několik sekund před hello načíst centra událostí, že začne vyrovnávání.


## <a name="next-steps"></a>Další kroky
Další informace o službě Event Hubs návštěvou hello následující odkazy:

* [Přehled služby Event Hubs](event-hubs-what-is-event-hubs.md)
* [Vytvoření centra událostí](event-hubs-create.md)
* [Nejčastější dotazy k Event Hubs](event-hubs-faq.md)
