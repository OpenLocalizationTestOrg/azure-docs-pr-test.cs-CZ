---
title: aaaScheduler vysokou dostupnost a spolehlivost
description: Scheduler vysokou dostupnost a spolehlivost
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 5ec78e60-a9b9-405a-91a8-f010f3872d50
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/16/2016
ms.author: deli
ms.openlocfilehash: 5c9efb333eb42b393adc5deea657ca99206d425e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scheduler-high-availability-and-reliability"></a>Scheduler vysokou dostupnost a spolehlivost
## <a name="azure-scheduler-high-availability"></a>Vysoká dostupnost Azure Scheduler
Jako základní platformu Azure služby Azure Scheduler je vysoce dostupný a funkce nasazení geograficky redundantní služby a úlohy místní geografické replikace.

### <a name="geo-redundant-service-deployment"></a>Nasazení služby geograficky redundantní
Azure Scheduler je k dispozici prostřednictvím hello uživatelského rozhraní v téměř každé geografické oblasti, která je v Azure ještě dnes. Hello seznam oblastí, které jsou k dispozici ve službě Azure Scheduler je [tady](https://azure.microsoft.com/regions/#services). Pokud datového centra v hostované oblasti vykresleno není k dispozici, jsou hello převzetí služeb při selhání služby Azure Scheduler tak, aby služba hello je dostupná z jiného datového centra.

### <a name="geo-regional-job-replication"></a>Geograficky místní úlohy replikace
Je nejen hello Azure Scheduler front-end pro požadavky na správu, ale vlastní úlohu k dispozici je také geograficky replikované. Po výpadku v jedné oblasti Azure Scheduler převezme a zajišťuje, že spuštění této úlohy hello z jiného datového centra v hello spárované geografické oblasti.

Například pokud jste vytvořili úlohu v jihu USA, Azure Scheduler automaticky replikuje této úlohy v Severní jihu USA. Když dojde k selhání v jihu USA, Azure Scheduler zajistí, že spuštění této úlohy hello z Sever střední USA. 

![][1]

V důsledku toho Azure Scheduler zajistí, že vaše data zůstane v rámci hello širší stejné zeměpisné oblasti v případě selhání Azure. V důsledku toho nemusí duplicitní vysokou dostupnost vaší úlohy jenom tooadd – Azure Scheduler automaticky poskytuje vysokou dostupnost funkcí pro úlohy.

## <a name="azure-scheduler-reliability"></a>Spolehlivost Azure Scheduler
Azure Scheduler zaručuje vlastní vysokou dostupnost a přistupují toouser vytvoření úlohy. Například vaší práce může vyvolat koncový bod protokolu HTTP, který je k dispozici. Azure Scheduler přesto pokusí tooexecute úlohu úspěšně, tím, že jste toodeal alternativní možnosti s chybou. Azure Scheduler k tomu dvěma způsoby:

### <a name="configurable-retry-policy-via-retrypolicy"></a>Konfigurovat zásady opakujte prostřednictvím "retryPolicy"
Azure Scheduler vám umožní tooconfigure zásady opakování. Ve výchozím nastavení pokud úloha selže, Scheduler pokusí hello úlohy znovu čtyři vícekrát v intervalech 30 sekund. Můžete znovu nakonfigurovat tato opakování zásad toobe agresivnější (například desetkrát, v intervalech 30 sekund) nebo volnější (například dvakrát, v denních intervalech.)

Jako příklad když to může pomoct můžete vytvořit úlohu, která spouští jednou za týden a vyvolá koncový bod HTTP. Pokud koncový bod hello HTTP je vypnutý po několik hodin, když vaše úloha běží, nemusí chcete toowait jeden další týden v případě hello úlohy toorun znovu vzhledem k tomu, že selžou i hello zásady opakování výchozí. V takových případech může překonfigurujete hello standardní opakování zásad tooretry každé tři hodiny (například) namísto každých 30 sekund.

jak tooconfigure zásady opakovaných pokusů, odkazovat příliš toolearn[retryPolicy](scheduler-concepts-terms.md#retrypolicy).

### <a name="alternate-endpoint-configurability-via-erroraction"></a>Alternativní koncový bod možnosti konfigurace: prostřednictvím "errorAction"
Pokud hello cílového koncového bodu pro úlohu Azure Scheduler zůstane nedostupný, Azure Scheduler přejde toohello alternativní koncový bod zpracování chyb po následující jeho zásady opakování. Pokud je nakonfigurovaný koncový bod alternativní zpracování chyb, Azure Scheduler jej spustí. Alternativní koncový bod vlastní úlohy jsou vysoce dostupné v hello vzhled selhání.

Jako příklad hello obrázku níže Azure Scheduler odpovídá jeho toohit zásady opakování New Yorku webové služby. Po hello opakuje pokus selže, zkontroluje, jestli je alternativním. Potom přejde dopředu a spustí provádění požadavků toohello alternativní s hello stejné zásady opakování.

![][2]

Všimněte si, že hello stejné zásady opakování platí tooboth hello původní akce a akce alternativní chyby hello. Typ akce jeho také možné toohave hello alternativní Chyba akce být odlišný od typu akce hello hlavní akce. Například při hello hlavní akce může být vyvolání koncový bod protokolu HTTP, akce chyby hello místo pravděpodobně fronty úložiště, fronty service bus nebo akce témata sběrnice služby, která provádí protokolování chyb.

jak tooconfigure alternativní koncový bod, najdete příliš toolearn[errorAction](scheduler-concepts-terms.md#action-and-erroraction).

## <a name="see-also"></a>Viz také
 [Co je Scheduler?](scheduler-intro.md)

 [Koncepty, terminologie a hierarchie entit Azure Scheduleru](scheduler-concepts-terms.md)

 [Začněte používat plánovače v hello portálu Azure](scheduler-get-started-portal.md)

 [Plány a fakturace v Azure Scheduleru](scheduler-plans-billing.md)

 [Jak toobuild komplexní plány a pokročilé opakování ve službě Azure Scheduler](scheduler-advanced-complexity.md)

 [REST API Azure Scheduleru – referenční informace](https://msdn.microsoft.com/library/mt629143)

 [Rutiny PowerShellu pro Azure Scheduler – referenční informace](scheduler-powershell-reference.md)

 [Omezení, výchozí hodnoty a chybové kódy Azure Scheduleru](scheduler-limits-defaults-errors.md)

 [Odchozí ověření Azure Scheduleru](scheduler-outbound-authentication.md)

[1]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png

[2]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png
