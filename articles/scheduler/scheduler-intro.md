---
title: aaaWhat je Azure Scheduler? | Dokumentace Microsoftu
description: "Azure Scheduler vám umožní toodeclaratively popisují akce toorun v cloudu hello. Potom naplánuje a automaticky spustí tyto akce."
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 52aa6ae1-4c3d-43fb-81b0-6792c84bcfae
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 062e25ae473510264dc0038198c05e7ac1e86210
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-scheduler"></a>Co je Azure Scheduler?
Azure Scheduler vám umožní toodeclaratively popisují akce toorun v cloudu hello. Potom naplánuje a automaticky spustí tyto akce.  Scheduler k tomu pomocí [hello portál Azure](scheduler-get-started-portal.md), kód, [REST API](https://msdn.microsoft.com/library/mt629143.aspx), nebo Azure PowerShell.

Scheduler vytváří, udržuje a vyvolává naplánovanou práci.  Scheduler nehostuje žádné úlohy ani nespouští žádný kód. On *vyvolává* kód hostovaný jinde – v Azure, v lokálním umístění nebo u jiného poskytovatele. Vyvolává přes HTTP, frontu úložiště, frontu sběrnice nebo téma sběrnice.

Scheduler plánuje [úlohy](scheduler-concepts-terms.md), uchovává historii výsledků provedení úloh, která jeden můžete zkontrolovat, a deterministicky a spolehlivě spuštění toobe plány úloh. Azure WebJobs (součást hello funkce Web Apps v Azure App Service) a další možnosti plánování Azure používají Scheduler hello pozadí. Hello [REST API Scheduleru](https://msdn.microsoft.com/library/mt629143.aspx) pomáhá spravovat hello komunikaci pro tyto akce. Scheduler jako takový bez problémů podporuje [komplexní plánování a pokročilé opakování](scheduler-advanced-complexity.md).

Existuje několik situací, se samo toohello použití Scheduleru. Například:

* *Opakující se akce aplikace:* Pravidelné shromažďování dat z Twitteru do informačního kanálu.
* *Každodenní údržba:* Každodenní vyřazování protokolů, zálohování a další úkoly týkající se údržby. Například může správce zvolit tooback hello databáze v 1:00 každý den pro hello dalších devět měsíců.

Scheduler vám umožní toocreate, aktualizovat, odstranit, zobrazit a spravovat úlohy a [kolekce úloh](scheduler-concepts-terms.md) programově pomocí skriptů a hello portálu.

## <a name="see-also"></a>Viz také
 [Koncepty, terminologie a hierarchie entit Azure Scheduleru](scheduler-concepts-terms.md)

 [Začněte používat plánovače v hello portálu Azure](scheduler-get-started-portal.md)

 [Plány a fakturace v Azure Scheduleru](scheduler-plans-billing.md)

 [Jak toobuild komplexní plány a pokročilé opakování ve službě Azure Scheduler](scheduler-advanced-complexity.md)

 [REST API Azure Scheduleru – referenční informace](https://msdn.microsoft.com/library/mt629143)

 [Rutiny PowerShellu pro Azure Scheduler – referenční informace](scheduler-powershell-reference.md)

 [Vysoká dostupnost a spolehlivost Azure Scheduleru](scheduler-high-availability-reliability.md)

 [Omezení, výchozí hodnoty a chybové kódy Azure Scheduleru](scheduler-limits-defaults-errors.md)

 [Odchozí ověření Azure Scheduleru](scheduler-outbound-authentication.md)

