---
title: "aaaAzure předávání výjimky a jak tooresolve je | Microsoft Docs"
description: "Získání seznamu výjimek předávání přes Azure a doporučované akce může trvat toohelp jejich řešení."
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 5f9dd02c-cce0-43b3-8eb8-744f0c27f38c
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: de417c8e9e43407ef355fd44f6170cf2cdc46d6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-relay-exceptions"></a>Azure předávání výjimky

V tomto článku jsou uvedené některé výjimky, které může být generována hello Azure předávání přes rozhraní API. Tento odkaz je toochange subjektu, tak to zkuste znovu aktualizací.

## <a name="exception-categories"></a>Výjimka kategorie

Hello předávání přes rozhraní API pro generování výjimek, které může spadají do následujících kategorií hello. Navrhovaná akce, můžete provést toohelp vyřešte hello výjimky jsou také uvedeny.

*   **Uživatel kódování chyba**: [System.ArgumentException](https://msdn.microsoft.com/library/system.argumentexception.aspx), [System.InvalidOperationException](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx), [System.OperationCanceledException](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx), [System.Runtime.Serialization.SerializationException](https://msdn.microsoft.com/library/system.runtime.serialization.serializationexception.aspx). 

    **Obecné akce**: Zkuste toofix hello kódu, než budete pokračovat.
*   **Chyba instalace/konfigurace**: [System.UnauthorizedAccessException](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx). 

    **Obecné akce**: Zkontrolujte konfiguraci. V případě potřeby změňte konfiguraci hello.
*   **Přechodný výjimky**: [Microsoft.ServiceBus.Messaging.MessagingException](/dotnet/api/microsoft.servicebus.messaging.messagingexception), [Microsoft.ServiceBus.Messaging.ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception), [Microsoft.ServiceBus.Messaging.MessagingCommunicationException](/dotnet/api/microsoft.servicebus.messaging.messagingcommunicationexception). 

    **Obecné akce**: operaci hello nebo oznámit uživatelům.
*   **Ostatní výjimky**: [System.Transactions.TransactionException](https://msdn.microsoft.com/library/system.transactions.transactionexception.aspx), [System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx). 

    **Obecné akce**: konkrétní toohello typ výjimky. Viz tabulka hello v následující části hello. 

## <a name="exception-types"></a>Typy výjimek

Hello následující tabulka uvádí typy zasílání zpráv výjimky a jejich příčiny. Zaznamenává taky doporučované akce může trvat vyřešení toohelp hello výjimky.

| **Typ výjimky** | **Popis** | **Navrhovaná akce** | **Poznámka: na automatické nebo okamžitou opakování** |
| --- | --- | --- | --- |
| [Časový limit](https://msdn.microsoft.com/library/system.timeoutexception.aspx) |Hello serveru nereagovala toohello požadovaná operace v rámci hello zadaný čas, který je kontrolován [OperationTimeout](/dotnet/api/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout). Hello server může mít dokončené hello požadovanou operaci. K tomu dochází z důvodu toonetwork nebo jiné infrastruktury zpoždění. |Zkontrolujte stav systému hello konzistence a opakujte, v případě potřeby. V tématu [TimeoutException](#timeoutexception). |Opakování vám může pomoci v některých případech; Přidejte logiku toocode opakování. |
| [Neplatná operace](https://msdn.microsoft.com/library/system.invalidoperationexception.aspx) |Hello požadovaného uživatele operace není povolena v rámci hello server nebo službu. Zobrazí se zpráva o výjimce hello podrobnosti. |Zkontrolujte hello kód a dokumentace hello. Ujistěte se, že tento hello požadovaná operace je platná. |Opakování nepomůže. |
| [Operace byla zrušena](https://msdn.microsoft.com/library/system.operationcanceledexception.aspx) |Pokus o je provedené tooinvoke operace u objektu, který již byl uzavřen, byl zrušen nebo zlikvidován. Ve výjimečných případech je již zveřejněn hello vedlejším transakce. |Zkontrolujte hello kódu a ujistěte se, že nevyvolá operací na objekt uvolněné. |Opakování nepomůže. |
| [Neoprávněný přístup](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) |Hello [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) objekt se nedá získat token, hello token je neplatný nebo hello token neobsahuje hello deklarace identity požadované tooperform hello operaci. |Ujistěte se, že tento zprostředkovatel tokenu hello je vytvořen s hello správné hodnoty. Zkontrolujte konfiguraci hello hello služby Řízení přístupu. |Opakování vám může pomoci v některých případech; Přidejte logiku toocode opakování. |
| [Argument výjimka](https://msdn.microsoft.com/library/system.argumentexception.aspx),<br /> [Argument Null](https://msdn.microsoft.com/library/system.argumentnullexception.aspx),<br />[Argument mimo rozsah.](https://msdn.microsoft.com/library/system.argumentoutofrangeexception.aspx) |Jeden nebo více následujících hello došlo k chybě:<br />Jeden nebo více metoda toohello argumenty jsou neplatné.<br /> Hello URI zadaný příliš[NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) nebo [vytvořit](/dotnet/api/microsoft.servicebus.messaging.messagingfactory.create) obsahuje jeden nebo více segmenty cesty.<br />zadané schéma identifikátoru URI Hello příliš[NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) nebo [vytvořit](/dotnet/api/microsoft.servicebus.messaging.messagingfactory.create) je neplatný. <br />Hodnota vlastnosti Hello je větší než 32 KB. |Volání kódu hello a ujistěte se, že hello argumenty jsou správné. |Opakování nepomůže. |
| [Server je zaneprázdněný](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) |Služba není možné tooprocess hello žádosti v tuto chvíli. |Hello klienta můžete čekat na určitou dobu a potom opakujte operaci hello. |Hello klienta mohou zkuste v určitých intervalech. Pokud výsledky opakování v různých výjimek, zkontrolujte hello opakování chování této výjimky. |
| [Kvóta byla překročena.](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) |Hello entity pro zasílání zpráv bylo dosaženo jeho maximální povolenou velikost. |Vytvoření prostoru v entitě hello příjmu zpráv ze hello entity nebo jeho podfronty. V tématu [quotaexceededexception –](#quotaexceededexception). |Pokud zprávy byla odebrána v hello té doby, mohou pomoci opakování. |
| [Velikost zprávy překročen](/dotnet/api/microsoft.servicebus.messaging.messagesizeexceededexception) |Datovou část zprávy překračuje limit 256 KB hello. Všimněte si, že je tento limit 256KB hello hello celková velikost zpráv. Hello celková velikost zpráv mohou zahrnovat vlastnosti systému a všechny režijní náklady na rozhraní Microsoft .NET. |Snížení velikosti hello datovou část zprávy hello a opakujte operaci hello. |Opakování nepomůže. |

## <a name="quotaexceededexception"></a>Quotaexceededexception –

[Quotaexceededexception –](/dotnet/api/microsoft.servicebus.messaging.quotaexceededexception) označuje, že byla překročena kvóta pro konkrétní entitu.

Pro předávání, tato výjimka zabalí hello [System.ServiceModel.QuotaExceededException](https://msdn.microsoft.com/library/system.servicemodel.quotaexceededexception.aspx), což znamená, že maximální počet hello naslouchacího procesu byla překročena pro tento koncový bod. To je uvedené v hello **MaximumListenersPerEndpoint** hodnotu hello zpráva o výjimce.

## <a name="timeoutexception"></a>TimeoutException
A [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx) označuje, že operace se uživatel spustil trvá déle, než časový limit operace hello. 

Zkontrolujte hodnotu hello hello [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit) vlastnost. Stiskne tohoto limitu může také způsobit [TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx).

Pro předávání se může zobrazit výjimkám časového limitu při prvním otevření předávacího spojení odesílatele. Existují dvě běžné příčiny této výjimky:

*   Hello [OpenTimeout](https://msdn.microsoft.com/library/wcf.opentimeout.aspx) hodnota může být příliš malý (Pokud i za zlomek sekundy).
* Naslouchací proces předávání místní může být reagovat (nebo setkat problémů pravidla brány firewall, které moduly pro naslouchání přijímat nová připojení klienta) a hello [OpenTimeout](https://msdn.microsoft.com/library/wcf.opentimeout.aspx) hodnota je menší než přibližně 20 sekund.

Příklad:

```
'System.TimeoutException’: hello operation did not complete within hello allotted timeout of 00:00:10.
hello time allotted toothis operation may have been a portion of a longer timeout.
```

### <a name="common-causes"></a>Běžné příčiny
Existují dvě běžné příčiny této chyby:

*   **Nesprávná konfigurace**
    
    časový limit operace Hello může být příliš malá pro provozní podmínky hello. časový limit operace hello v klientovi hello SDK Hello výchozí hodnota je 60 sekund. Toosee zkontrolujte, zda hodnota hello ve vašem kódu nastavená toosomething příliš malá. Všimněte si, že podmínka hello a využití procesoru hello sítě může ovlivnit hello doba potřebná pro toocomplete operaci. Je vhodné není tooset hello operaci hodnotu časového limitu tooa velmi malé.
*   **Přechodné chybě**

    V některých případech může dojít k hello předávací služba zpoždění při zpracování požadavků. K tomu může dojít například během období intenzivní provoz. Pokud k tomu dojde, opakujte operaci po prodlevě, dokud nebude úspěšná operace hello. Pokud hello stejné operace pokračovat toofail po několika pokusech, zkuste hello [služba Azure stav lokality](https://azure.microsoft.com/status/) toosee, pokud jsou známy výpadky.

## <a name="next-steps"></a>Další kroky
* [Nejčastější dotazy k Azure předávání](relay-faq.md)
* [Vytvoření oboru názvů předávání](relay-create-namespace-portal.md)
* [Začínáme s Azure předávání a rozhraní .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Začínáme s Azure předávání a uzlu](relay-hybrid-connections-node-get-started.md)

