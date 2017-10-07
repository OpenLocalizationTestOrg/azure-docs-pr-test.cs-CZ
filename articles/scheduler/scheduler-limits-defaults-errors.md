---
title: "Omezení aaaScheduler a výchozích hodnot"
description: "Omezení a výchozích hodnot"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 88f4a3e9-6dbd-4943-8543-f0649d423061
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 6fe0600d3ce3249d5aab1b877369b175316b5437
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scheduler-limits-and-defaults"></a>Omezení a výchozích hodnot
## <a name="scheduler-quotas-limits-defaults-and-throttles"></a>Scheduler kvóty, omezení, výchozí hodnoty a omezení
[!INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="hello-x-ms-request-id-header"></a>Hello x-ms-request-id hlavičky
Každý požadavek směřovaný na hello služba Plánovač vrátí hlavičky odpovědi s názvem**x-ms-request-id**. Tuto hlavičku obsahuje neprůhledné hodnoty, která jednoznačně identifikuje hello požadavku.

Pokud je požadavek konzistentně selhání a mít ověříte, že je správně formulovali hello požadavku, můžete použít tuto hodnotu tooreport hello chyba tooMicrosoft. V sestavě, zahrnují hello hodnotu x-ms-request-id hello Přibližná doba této hello požadavek, hello identifikátor hello předplatné, kolekci úloh nebo úlohy a hello typ operace, která hello pokus o žádost.

## <a name="see-also"></a>Viz také
 [Co je Scheduler?](scheduler-intro.md)

 [Koncepty, terminologie a hierarchie entit Azure Scheduleru](scheduler-concepts-terms.md)

 [Začněte používat plánovače v hello portálu Azure](scheduler-get-started-portal.md)

 [Plány a fakturace v Azure Scheduleru](scheduler-plans-billing.md)

 [REST API Azure Scheduleru – referenční informace](https://msdn.microsoft.com/library/mt629143)

 [Rutiny PowerShellu pro Azure Scheduler – referenční informace](scheduler-powershell-reference.md)

 [Vysoká dostupnost a spolehlivost Azure Scheduleru](scheduler-high-availability-reliability.md)

 [Odchozí ověření Azure Scheduleru](scheduler-outbound-authentication.md)

