---
title: "výjimky zasílání zpráv Service Bus aaaAzure | Microsoft Docs"
description: "Seznam výjimky zasílání zpráv Service Bus a doporučované akce."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 3d8526fe-6e47-4119-9f3e-c56d916a98f9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2017
ms.author: sethm
ms.openlocfilehash: 0a206b7bbc808c1190044c1dfd6ffd47b9d454fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-messaging-exceptions"></a>Výjimky zasílání zpráv Service Bus
V tomto článku jsou uvedené některé výjimky generované hello Microsoft Azure Service Bus rozhraní API pro zasílání zpráv. Tento odkaz je toochange subjektu, tak to zkuste znovu aktualizací.

## <a name="exception-categories"></a>Výjimka kategorie
Hello API pro přenos zpráv generovat výjimky, které můžete spadají do následujících kategorií hello, společně s hello přidružené akce může trvat tootry toofix je. Všimněte si, že text hello, což znamená, že výjimky a se může lišit v závislosti na typu hello entity zasílání zpráv (fronty nebo témata nebo Event Hubs):

1. Uživatel kódování chyby ([System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx)). Obecné akce: Zkuste toofix hello kódu než budete pokračovat.
2. Chyba instalace/konfigurace ([Microsoft.ServiceBus.Messaging.MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception), [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). Obecné akce: Zkontrolujte konfiguraci a v případě potřeby změnit.
3. Přechodný výjimky ([Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception), [Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception)). Obecné akce: operaci hello nebo oznámit uživatelům.
4. Ostatní výjimky ([System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx), [Microsoft.ServiceBus.Messaging.MessageLockLostException](/dotnet/api/microsoft.servicebus.messaging.messagelocklostexception), [Microsoft.ServiceBus.Messaging.SessionLockLostException](/dotnet/api/microsoft.servicebus.messaging.sessionlocklostexception)). Obecné akce: typ výjimky konkrétní toohello; naleznete v tabulce toohello v následující části hello. 

## <a name="exception-types"></a>Typy výjimek
Hello následující tabulka uvádí typy výjimek zasílání zpráv a jejich příčiny a poznámky navrhovaná akce, které můžete provést.

| **Typ výjimky** | **Popis nebo příčina/příklady** | **Navrhovaná akce** | **Poznámka: na automatické, okamžitou opakování** |
| --- | --- | --- | --- |
| [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |Hello serveru nereagovala toohello požadovaná operace v rámci hello zadaný čas, který je kontrolován [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout). Hello server může dokončili hello požadovanou operaci. K tomu dochází z důvodu toonetwork nebo jiné infrastruktury zpoždění. |Zkontrolujte stav systému hello konzistence a opakujte v případě potřeby. V tématu [výjimkám časového limitu](#timeoutexception). |Opakování vám může pomoci v některých případech; Přidejte logiku toocode opakování. |
| [InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |Hello požadovaného uživatele operace není povolena v rámci hello server nebo službu. Zobrazí se zpráva o výjimce hello podrobnosti. Například [Complete](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) vygeneruje výjimku, pokud byla přijata zpráva hello v [ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode) režimu. |Zkontrolujte hello kód a dokumentace hello. Zkontrolujte, zda text hello požadovaná operace je platná. |Opakování nepomůže. |
| [OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Pokus o je provedené tooinvoke operace u objektu, který již byl uzavřen, byl zrušen nebo zlikvidován. Ve výjimečných případech je již zveřejněn hello vedlejším transakce. |Zkontrolujte hello kódu a ujistěte se, že nevyvolá operací na objekt uvolněné. |Opakování nepomůže. |
| [UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |Hello [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) objekt se nedá získat token, hello token je neplatný nebo hello token neobsahuje hello deklarace identity požadované tooperform hello operaci. |Ujistěte se, že se vytvoří zprostředkovatel tokenu hello s hello správné hodnoty. Zkontrolujte konfiguraci hello hello služby Řízení přístupu. |Opakování vám může pomoci v některých případech; Přidejte logiku toocode opakování. |
| [ArgumentException –](https://msdn.microsoft.com/library/system.argumentexception.aspx)<br /> [ArgumentNullException](https://msdn.microsoft.com/library/system.argumentnullexception.aspx)<br />[Výjimka ArgumentOutOfRangeException](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |Jeden nebo více metoda toohello argumenty jsou neplatné.<br /> Hello URI zadaný příliš[NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) nebo [vytvořit](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_Create_System_Collections_Generic_IEnumerable_System_Uri__) obsahuje segment(s) cesta.<br /> zadané schéma identifikátoru URI Hello příliš[NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) nebo [vytvořit](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_Create_System_Collections_Generic_IEnumerable_System_Uri__) je neplatný. <br />Hodnota vlastnosti Hello je větší než 32KB. |Volání kódu hello a ujistěte se, že hello argumenty jsou správné. |Opakování nepomůže. |
| [MessagingEntityNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagingentitynotfoundexception) |Entity přidružené hello operaci neexistuje nebo byla odstraněna. |Zkontrolujte, zda text hello entita existuje. |Opakování nepomůže. |
| [MessageNotFoundException](/dotnet/api/microsoft.servicebus.messaging.messagenotfoundexception) |Byl proveden pokus tooreceive zprávu s číslem konkrétní pořadí. Tato zpráva nebyla nalezena. |Ujistěte se, že již nebyla přijata zpráva hello. Toosee fronty nedoručených zpráv hello zkontrolujte, zda byl deadlettered uvítací zprávu. |Opakování nepomůže. |
| [MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception) |Klient není možné tooestablish tooService připojení sběrnice. |Zajistěte, aby hello zadaný název hostitele je správný, a hostitel hello je dostupný. |Opakování mohou pomoci, pokud existují problémy s občasným připojením. |
| [ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) |Služba není možné tooprocess hello žádosti v tuto chvíli. |Klienta můžete čekat na určitou dobu a potom opakujte operaci hello. |Klient může po určité době opakujte. Pokud opakování výsledkem různé výjimky, zkontrolujte opakování chování této výjimky. |
| [MessageLockLostException](/dotnet/api/microsoft.servicebus.messaging.messagelocklostexception) |Vypršela platnost tokenu zámku přidružené k uvítací zprávu nebo hello zámku token nebyl nalezen. |Dispose uvítací zprávu. |Opakování nepomůže. |
| [SessionLockLostException](/dotnet/api/microsoft.servicebus.messaging.sessionlocklostexception) |Zámek přidružené k této relaci se ztratí. |Abort – hello [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) objektu. |Opakování nepomůže. |
| [MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception) |Obecná výjimka, která může být vyvolána v následujících případech hello zasílání zpráv:<br /> Je proveden pokus o toocreate [QueueClient](/dotnet/api/microsoft.servicebus.messaging.queueclient) pomocí názvu nebo cestu, která patří tooa jiné entity typu (například téma).<br />  Pokus o je provedené toosend zprávu větší než 256KB. Hello server nebo služby došlo k chybě během zpracování požadavku hello. Podrobnosti viz zpráva o výjimce hello podrobnosti. Obvykle se jedná o přechodný výjimku. |Zkontrolujte hello kódu a ujistěte se, že pouze serializovatelné objekty se používají pro tělo zprávy hello (nebo použít vlastní serializátor). Podívejte se do dokumentace hello hello podporované typy hodnot vlastností hello a pouze pomocí podporované typy. Zkontrolujte hello [IsTransient](/dotnet/api/microsoft.servicebus.messaging.messagingexception#Microsoft_ServiceBus_Messaging_MessagingException_IsTransient) vlastnost. Pokud je **true**, můžete opakovat operace hello. |Postup pro opakované není definován a nemusí pomoci. |
| [MessagingEntityAlreadyExistsException](/dotnet/api/microsoft.servicebus.messaging.messagingentityalreadyexistsexception) |Byl proveden pokus toocreate entitu s názvem, který je již využíván jinou entitou v daném oboru názvů služby. |Odstraňte stávající entity hello nebo zvolte jiný název pro toobe entity hello vytvořili. |Opakování nepomůže. |
| [Quotaexceededexception –](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) |Hello entity pro zasílání zpráv bylo dosaženo maximální povolené velikosti nebo byl překročen maximální počet připojení tooa názvů hello. |Vytvoření prostoru v entitě hello příjmu zpráv ze hello entity nebo jeho dílčí fronty. V tématu [quotaexceededexception –](#quotaexceededexception). |Pokud zprávy byla odebrána v hello té doby, mohou pomoci opakování. |
| [RuleActionException](/dotnet/api/microsoft.servicebus.messaging.ruleactionexception) |Service Bus vrací výjimku, pokud pokusíte toocreate neplatné pravidlo akce. Service Bus připojí tato zpráva o výjimce tooa deadlettered, pokud dojde k chybě při zpracování hello akce pravidla pro tuto zprávu. |Zkontrolujte akce pravidla hello správnost. |Opakování nepomůže. |
| [FilterException](/dotnet/api/microsoft.servicebus.messaging.filterexception) |Service Bus vrátí výjimku, pokud se pokusíte toocreate neplatný filtr. Service Bus připojí tato zpráva o výjimce tooa deadlettered, pokud došlo k chybě při zpracování hello filtr pro tuto zprávu. |Zkontrolujte správnost hello filtr. |Opakování nepomůže. |
| [SessionCannotBeLockedException](/dotnet/api/microsoft.servicebus.messaging.sessioncannotbelockedexception) |Byl proveden pokus tooaccept, který relaci s ID konkrétní relace, ale relace hello je aktuálně uzamčen jiným klientem. |Zajistěte, aby se ostatní klienti odemkne hello relace. |Opakování mohou pomoci, pokud hello relace byla vydána v mezičase hello. |
| [TransactionSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.transactionsizeexceededexception) |Příliš mnoho operace jsou součástí hello transakce. |Snižte počet hello operací, které jsou součástí této transakce. |Opakování nepomůže. |
| [MessagingEntityDisabledException](/dotnet/api/microsoft.servicebus.messaging.messagingentitydisabledexception) |Žádost o operaci modul runtime na zakázané entity. |Aktivujte hello entity. |Opakování mohou pomoci, pokud byla aktivována hello entity v mezičase hello. |
| [NoMatchingSubscriptionException](/dotnet/api/microsoft.servicebus.messaging.nomatchingsubscriptionexception) |Service Bus vrací výjimku při odesílání zprávy tooa téma, které má povolené předem filtrování a hello filtry neodpovídají. |Ujistěte se, že odpovídá alespoň jeden filtr. |Opakování nepomůže. |
| [MessageSizeExceededException](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) |Datovou část zprávy překračuje limit 256 KB hello. Všimněte si, že je tento limit 256 KB hello hello celková velikost zpráv, které mohou zahrnovat vlastnosti systému a všechny režijní náklady na rozhraní .NET. |Snížení velikosti hello datovou část zprávy hello a opakujte operaci hello. |Opakování nepomůže. |
| [TransactionException –](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx) |Hello vedlejším transakce (*Transaction.Current*) je neplatný. Může být dokončit nebo přerušena. Další informace mohou poskytnout informacích o vnitřní výjimce. | |Opakování nepomůže. |
| [Transactionindoubtexception –](https://msdn.microsoft.com/library/system.transactions.transactionindoubtexception.aspx) |Pokus o operaci v transakci, která je nejistá, nebo je proveden pokus o toocommit hello transakce a hello transakce stane nejistých. |Aplikace musí zpracovávat výjimku (jako ve speciálním případě), jak hello transakce může již byly potvrzeny. |- |

## <a name="quotaexceededexception"></a>Quotaexceededexception –
[Quotaexceededexception –](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) označuje, že byla překročena kvóta pro konkrétní entitu.

### <a name="queues-and-topics"></a>Fronty a témata
Pro fronty a témata je to často hello velikost fronty hello. Vlastnost zprávy o chybě Hello bude obsahovat další podrobnosti, jako hello následující ukázka:

```
Microsoft.ServiceBus.Messaging.QuotaExceededException
Message: hello maximum entity size has been reached or exceeded for Topic: ‘xxx-xxx-xxx’. 
    Size of entity in bytes:1073742326, Max entity size in bytes:
1073741824..TrackingId:xxxxxxxxxxxxxxxxxxxxxxxxxx, TimeStamp:3/15/2013 7:50:18 AM
```

uvítací zprávu s oznámením, že tohoto tématu hello překročil limit velikosti v této případu 1 GB (hello výchozí limit velikosti). 

### <a name="namespaces"></a>obory názvů

Pro obory názvů [quotaexceededexception –](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) může naznačovat, že aplikace byla překročena maximální počet připojení tooa názvů hello. Například:

```
Microsoft.ServiceBus.Messaging.QuotaExceededException: ConnectionsQuotaExceeded for namespace xxx.
<tracking-id-guid>_G12 ---> 
System.ServiceModel.FaultException`1[System.ServiceModel.ExceptionDetail]: 
ConnectionsQuotaExceeded for namespace xxx.
```

#### <a name="common-causes"></a>Běžné příčiny
Existují dvě běžné příčiny této chyby: hello frontu nedoručených zpráv a nefunkční příjemci zprávy.

1. **Frontu nedoručených zpráv** čtečku selhává toocomplete zprávy a hello zprávy jsou vráceny toohello fronta nebo téma, když vyprší platnost hello zámku. K tomu může dojít, pokud čtečka hello zaznamená výjimku, která brání jeho volání [BrokeredMessage.Complete](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx). Jakmile zprávu byl načten 10krát, přesune fronty nedoručených zpráv toohello ve výchozím nastavení. Toto chování je řízeno hello [QueueDescription.MaxDeliveryCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.maxdeliverycount.aspx) vlastnost a výchozí hodnota je 10. Jako zprávy hromadí do fronty nedoručených zpráv hello, zabíraly místo.
   
    tooresolve hello problém, čtení a dokončení hello zpráv z fronty zpráv nedoručených zpráv hello, jako je by z jiné fronty. Hello [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx) i obsahuje třídy [FormatDeadLetterPath](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.formatdeadletterpath.aspx) metoda toohelp formátu hello frontu nedoručených zpráv cestu.
2. **Příjemce zastavena** příjemce byla zastavena, přijímání zpráv z fronty nebo předplatné. Hello tooidentify způsob, jak jde toolook v hello [QueueDescription.MessageCountDetails](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.messagecountdetails.aspx) vlastnosti, která ukazuje rozpis úplné hello hello zpráv. Pokud hello [ActiveMessageCount](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagecountdetails.activemessagecount.aspx) vlastnost je vysoká nebo rostoucí pak tak rychle, jak jste zapisovaný nejsou při čtení zprávy hello.

### <a name="event-hubs"></a>Event Hubs
Centra událostí může obsahovat maximálně 20 skupiny příjemců za centra událostí. Když zkusíte toocreate další, zobrazí se [quotaexceededexception –](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception). 

## <a name="timeoutexception"></a>TimeoutException
A [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) označuje, že operace se uživatel spustil trvá déle, než časový limit operace hello. 

Je potřeba zkontrolovat hello hodnotu hello [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit) vlastnost jako stiskne tento limit, může také způsobit [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx).

### <a name="queues-and-topics"></a>Fronty a témata
Pro fronty a témata hello limitu buď v hello [MessagingFactorySettings.OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings#Microsoft_ServiceBus_Messaging_MessagingFactorySettings_OperationTimeout) vlastnost jako součást hello připojovací řetězec, nebo prostřednictvím [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.servicebus.servicebusconnectionstringbuilder). chybová zpráva Hello samotné se můžou lišit, ale vždy obsahuje hodnotu časového limitu hello zadaný pro aktuální operaci hello. 

### <a name="event-hubs"></a>Event Hubs
Pro službu Event Hubs je hello časový limit buď jako součást hello připojovací řetězec, nebo prostřednictvím [ServiceBusConnectionStringBuilder](/dotnet/api/microsoft.servicebus.servicebusconnectionstringbuilder). chybová zpráva Hello samotné se můžou lišit, ale vždy obsahuje hodnotu časového limitu hello zadaný pro aktuální operaci hello. 

### <a name="common-causes"></a>Běžné příčiny
Existují dvě běžné příčiny této chyby: nesprávné konfigurace nebo k přechodné chybě.

1. **Nesprávná konfigurace** hello časový limit operace může být příliš malá pro provozní podmínky hello. časový limit operace hello v klientovi hello SDK Hello výchozí hodnota je 60 sekund. Zkontrolujte, zda toosee hello hodnota má váš kód nastavit toosomething příliš malá. Všimněte si, že podmínka hello hello sítě a využití procesoru může ovlivnit hello doba potřebná pro toocomplete určité operace, by neměl být nastavený časový limit operace hello tooa velmi nízkou hodnotu.
2. **Přechodné chybě** někdy hello služby Service Bus můžete zaznamenat zpoždění při zpracování požadavků, například během období intenzivní provoz. V takových případech může pokus zopakovat operaci po prodlevě, dokud nebude úspěšná operace hello. Jestliže hello stejné operace stále po několika pokusech, navštivte hello [služba Azure stav lokality](https://azure.microsoft.com/status/) toosee, pokud nejsou žádné známé výpadky.

## <a name="next-steps"></a>Další kroky

Hello úplný odkaz rozhraní API .NET Service Bus, najdete v části hello [referenční dokumentace rozhraní API .NET Azure](/dotnet/api/overview/azure/servicebus).

Další informace o toolearn [Service Bus](https://azure.microsoft.com/services/service-bus/), najdete v následujících tématech hello.

* [Přehled přenosu zpráv ve službě Service Bus](service-bus-messaging-overview.md)
* [Základy služby Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Architektura služby Service Bus](service-bus-architecture.md)

