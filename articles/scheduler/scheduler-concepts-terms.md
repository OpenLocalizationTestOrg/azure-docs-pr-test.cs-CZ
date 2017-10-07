---
title: aaaScheduler koncepty, pojmy a entity | Microsoft Docs
description: "Koncepty, terminologie a hierarchie entit služby Azure Scheduler včetně úloh a kolekcí úloh.  Zobrazí ucelený příklad naplánované úlohy."
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 3ef16fab-d18a-48ba-8e56-3f3e0a1bcb92
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: get-started-article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 73e7de7bfd2937e401aeab05e0e10fa292cf37b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scheduler-concepts-terminology--entity-hierarchy"></a>Koncepty, terminologie a hierarchie entit Scheduleru
## <a name="scheduler-entity-hierarchy"></a>Hierarchie entit Scheduleru
Hello následující tabulka popisuje hlavní prostředky, které hello vystavuje nebo používá hello API Scheduleru:

| Prostředek | Popis |
| --- | --- |
| **Kolekce úloh** |Kolekce úloh obsahuje skupinu úloh a zachovává nastavení, kvóty a omezení, které jsou sdílené mezi úlohami v kolekci hello. Kolekci úloh vytvoří vlastník předplatného a seskupuje úlohy podle použití nebo hranic aplikací. Je omezené tooone oblast. Také umožňuje vynucovat kvóty hello tooconstrain hello používání všech úloh v dané kolekci. Hello kvóty zahrnují hodnoty MaxJobs a MaxRecurrence. |
| **Úloha** |Úloha definuje jednu opakující se akci s jednoduchými nebo komplexními strategiemi provedení. Akce můžou zahrnovat požadavky HTTP, fronty úložiště, fronty sběrnice nebo témata sběrnice. |
| **Historie úlohy** |Historie úlohy obsahuje podrobnosti o provedení úlohy. Obsahuje podrobnosti o úspěchu/neúspěchu a jakékoli odezvě. |

## <a name="scheduler-entity-management"></a>Správa entit Scheduleru
Na vysoké úrovni hello Plánovač a rozhraní API pro správu služby hello vystavit hello následující operací s prostředky hello:

| Schopnost | Popis a adresa identifikátoru URI |
| --- | --- |
| **Správa kolekce úloh** |GET, PUT a DELETE podpora pro vytváření a úpravu kolekcí úloh a v nich obsažených úloh hello. Kolekce úloh je kontejner pro úlohy a mapy tooquotas a sdíleným nastavením. Příklady kvót popsané později jsou maximální počet úloh a nejmenší interval opakování. <p>PUT a DELETE: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p><p>GET: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}`</p> |
| **Správa úloh** |Podpora pro GET, PUT, POST, PATCH a DELETE pro vytváření a úpravu úloh. Všechny úlohy musí patřit tooa kolekce úloh, který již existuje, takže není vytváření nedochází. <p>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}`</p> |
| **Správa historie úloh** |Podpora pro GET pro načtení historie provádění úloh, jako je uplynulá doba úlohy a výsledky provedení úlohy, za posledních 60 dní. Přidá podporu parametru řetězce dotazu pro filtrování podle stavu a statusu. <P>`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Scheduler/jobCollections/{jobCollectionName}/jobs/{jobName}/history`</p> |

## <a name="job-types"></a>Typy úloh
Existují různé typy úloh: úlohy HTTP (a úlohy HTTPS, které podporují SSL), úlohy fronty úložiště, úlohy fronty sběrnice a úlohy témata sběrnice. Úlohy HTTP jsou ideální, pokud máte koncový bod existující úlohy nebo služby. Fronty úloh toopost zprávy toostorage front úložiště, můžete použít, takže jsou ideální pro úlohy, které používají fronty úložiště. Úlohy sběrnice služby jsou zase ideální pro úlohy, které používají témata a fronty sběrnice služby.

## <a name="hello-job-entity-in-detail"></a>Podrobnosti o entitě "úloha" Hello
Na základní úrovni má naplánovaná úloha několik částí:

* Hello akce tooperform při hello časovač úlohy sepne  
* (Volitelné) hello čase toorun hello úloha  
* (Volitelné) Kdy a jak často toorepeat hello úlohy  
* (Volitelné) Toofire akce, pokud selže primární akce hello  

Naplánovaná úloha interně taky obsahuje data poskytnutá systémem, jako je například hello čas příštího naplánovaného provedení.

Následující kód Hello poskytuje ucelený příklad naplánované úlohy. Podrobnější informace jsou uvedené v dalších částech.

    {
        "startTime": "2012-08-04T00:00Z",               // optional
        "action":
        {
            "type": "http",
            "retryPolicy": { "retryType":"none" },
            "request":
            {
                "uri": "http://contoso.com/foo",        // required
                "method": "PUT",                        // required
                "body": "Posting from a timer",         // optional
                "headers":                              // optional

                {
                    "Content-Type": "application/json"
                },
            },
           "errorAction":
           {
               "type": "http",
               "request":
               {
                   "uri": "http://contoso.com/notifyError",
                   "method": "POST",
               },
           },
        },
        "recurrence":                                   // optional
        {
            "frequency": "week",                        // can be "year" "month" "day" "week" "minute"
            "interval": 1,                              // optional, how often toofire (default too1)
            "schedule":                                 // optional (advanced scheduling specifics)
            {
                "weekDays": ["monday", "wednesday", "friday"],
                "hours": [10, 22]
            },
            "count": 10,                                 // optional (default toorecur infinitely)
            "endTime": "2012-11-04",                     // optional (default toorecur infinitely)
        },
        "state": "disabled",                           // enabled or disabled
        "status":                                       // controlled by Scheduler service
        {
            "lastExecutionTime": "2007-03-01T13:00:00Z",
            "nextExecutionTime": "2007-03-01T14:00:00Z ",
            "executionCount": 3,
                                                "failureCount": 0,
                                                "faultedCount": 0
        },
    }

Jak je vidět v hello předešlé ukázce naplánované úlohy, má definice úlohy několik částí:

* Čas spuštění („startTime“)  
* Akce („action“), která zahrnuje i akci při chybě („errorAction“)
* Opakování („recurrence“)  
* Stav („state“)  
* Status („status“)  
* Zásady opakovaných pokusů („retryPolicy“)  

Podívejme se na každou podrobněji:

## <a name="starttime"></a>startTime
Hello "startTime" je čas spuštění hello a umožňuje hello volající toospecify časové pásmo posun na hello přenosu v [formátu ISO 8601](http://en.wikipedia.org/wiki/ISO_8601).

## <a name="action-and-erroraction"></a>action a errorAction
Hello "action" je hello akce vyvolaná při každém výskytu a popisuje typ vyvolání služby. Akce Hello je, co se provede na hello zadaný plán. Scheduler podporuje akce HTTP, fronty úložiště, fronty sběrnice a témata sběrnice.

Akce Hello v předchozím příkladu hello je akce HTTP. Dole je příklad akce fronty úložiště:

    {
            "type": "storageQueue",
            "queueMessage":
            {
                "storageAccount": "myStorageAccount",  // required
                "queueName": "myqueue",                // required
                "sasToken": "TOKEN",                   // required
                "message":                             // required
                    "My message body",
            },
    }

Dole je příklad akce témata sběrnice.

  "action": { "type": "serviceBusTopic", "serviceBusTopicMessage": { "topicPath": "t1",  
      "namespace": "mySBNamespace", "transportType": "netMessaging", // Can be either netMessaging or AMQP "authentication": { "sasKeyName": "QPolicy", "type": "sharedAccessKey" }, "message": "Some message", "brokeredMessageProperties": {}, "customMessageProperties": { "appname": "FromScheduler" } }, }

Dole je příklad akce fronty sběrnice:

  "action": { "serviceBusQueueMessage": { "queueName": "q1",  
      "namespace": "mySBNamespace", "transportType": "netMessaging", // Can be either netMessaging or AMQP "authentication": {  
        "sasKeyName": "QPolicy", "type": "sharedAccessKey" }, "message": "Some message",  
      "brokeredMessageProperties": {}, "customMessageProperties": { "appname": "FromScheduler" } }, "type": "serviceBusQueue" }

Hello "errorAction" je obslužná rutina chyby hello, hello akce vyvolaná při selhání primární akce hello. Můžete použít tuto proměnnou toocall koncový bod zpracování chyb nebo odeslání oznámení pro uživatele. Dá se použít pro spojení se sekundárním koncovým bodem v případě hello této hello primární server není k dispozici (například v případě havárie v lokalitě koncového bodu hello hello) nebo můžete použít pro upozornění koncového bodu pro zpracování chyby. Stejně jako primární akce hello může být akce chyby hello jednoduchou nebo složenou logiku podle jiných akcí. jak toocreate SAS token naleznete příliš toolearn[vytváření a používání sdíleného přístupového podpisu](https://msdn.microsoft.com/library/azure/jj721951.aspx).

## <a name="recurrence"></a>recurrence
Opakování má několik částí:

* Frequency (frekvence): Minuta, hodina, den, týden, měsíc nebo rok  
* Interval: Interval při hello zadané frekvence opakování hello  
* Předepsaný plán: Zadejte minuty, hodiny, dny v týdnu, měsíce a prescribed hello opakování  
* Count (počet): Počet výskytů  
* Koncový čas: po hello zadaný čas ukončení se neprovedou žádné úlohy  

Úloha se opakuje, jestliže má ve své definici JSON opakující se objekt. Pokud jsou zadané count i endTime, je dodržení hello dokončení pravidlo, které nastane jako první.

## <a name="state"></a>state
Hello stav úlohy hello je jednu ze čtyř hodnot: povolena, zakázána, dokončení nebo došlo k chybě. Můžete vložit nebo oprava úlohy, jako tooupdate je toohello povolený nebo zakázaný stav. Pokud úloha byla dokončena nebo došlo k chybě, která je konečný stav, který nelze aktualizovat (i když stále můžete použít DELETE hello úlohy). Příklad vlastnosti stavu hello vypadá takto:

        "state": "disabled", // enabled, disabled, completed, or faulted
Dokončené úlohy a úlohy, které selhaly, se odstraní po 60 dnech.

## <a name="status"></a>status
Po zahájení úlohy Scheduleru se vrátí informace o aktuální stav úlohy hello hello. Tento objekt není nastavit uživatelem hello – je nastavena podle hello systému. Ale je součástí hello objektu úlohy (spíše než v samostatném propojeném prostředku), aby jeden můžete snadno získat hello stav úlohy.

Stav úlohy obsahuje čas hello hello předchozího spuštění (pokud existuje), hello čas dalšího naplánovaného spuštění hello (pro probíhající úlohy) a počet provedení hello hello úlohy.

## <a name="retrypolicy"></a>retryPolicy
Pokud úloha Scheduleru selže, je možné toospecify toodetermine zásady opakování, zda a jak se pokus o hello akce. To je dáno hello **retryType** objektu – je nastaven příliš**žádné** Pokud neexistuje žádné zásady opakovaných pokusů, jak je uvedeno výše. Nastavit také**pevné** v případě zásady opakování.

tooset zásady opakovaných pokusů, můžete zadat další dvě nastavení: interval opakování (**retryInterval**) a hello počet opakovaných pokusů (**retryCount**).

interval opakovaných Hello zadaným hello **retryInterval** objektu, je hello interval mezi opakovanými pokusy. Výchozí hodnota je 30 sekund, minimální nastavitelná hodnota je 15 sekund a maximální hodnota je 18 měsíců. Pro úlohy v kolekcích úloh Free je minimální nastavitelná hodnota 1 hodina.  Je definována ve formátu ISO 8601 hello. Podobně je zadána hodnota hello hello počet opakování s hello **retryCount** objektu; je hello počet, kolikrát má pokus opakovat. Výchozí hodnota je 4 a maximální hodnota je 20\. Obě **retryInterval** a **retryCount** jsou volitelné. Pokud jsou uvedené výchozí hodnoty **retryType** je nastaven příliš**pevné** a nejsou explicitně zadané žádné hodnoty.

## <a name="see-also"></a>Viz také
 [Co je Scheduler?](scheduler-intro.md)

 [Začněte používat plánovače v hello portálu Azure](scheduler-get-started-portal.md)

 [Plány a fakturace v Azure Scheduleru](scheduler-plans-billing.md)

 [Jak toobuild komplexní plány a pokročilé opakování ve službě Azure Scheduler](scheduler-advanced-complexity.md)

 [REST API Azure Scheduleru – referenční informace](https://msdn.microsoft.com/library/mt629143)

 [Rutiny PowerShellu pro Azure Scheduler – referenční informace](scheduler-powershell-reference.md)

 [Vysoká dostupnost a spolehlivost Azure Scheduleru](scheduler-high-availability-reliability.md)

 [Omezení, výchozí hodnoty a chybové kódy Azure Scheduleru](scheduler-limits-defaults-errors.md)

 [Odchozí ověření Azure Scheduleru](scheduler-outbound-authentication.md)

