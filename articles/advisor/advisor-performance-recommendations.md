---
title: "Poradce při hodnocení výkonu doporučení aaaAzure | Microsoft Docs"
description: "Použití Advisor toooptimize hello výkon vašich Azure nasazení."
services: advisor
documentationcenter: NA
author: kumudd
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: eb3d928664717f6f322132ac740f42015f56b76e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="advisor-performance-recommendations"></a>Poradce při hodnocení výkonu doporučení

Doporučení pro optimální výkon Azure Advisor k vylepšování hello rychlost a reakce důležitých podnikových aplikací. Výkon doporučení služby Advisor můžete získat na hello **výkonu** kartě hello Advisor řídicího panelu.

![Karta výkonu Advisor](./media/advisor-performance-recommendations/advisor-performance-tab.png)

## <a name="improve-database-performance-with-sql-db-advisor"></a>Zlepšení výkonu databáze službou SQL DB Advisor

Advisor vám poskytne konzistentní, konsolidované zobrazení doporučení pro všechny prostředky Azure. Se integruje toobring Poradce pro databáze SQL můžete doporučení pro zlepšení výkonu hello vaší databáze SQL Azure. Poradce pro funkci SQL Database vyhodnocuje analýzou historii využití hello výkon vaší databáze SQL Azure. Potom nabízí doporučení, které jsou nejvhodnější pro spuštění databáze hello typické zatížení. 

> [!NOTE]
> doporučení tooget databáze musí mít o týden využití, a v daném týdnu musí být některé konzistentní aktivity. Poradce pro databáze SQL můžete optimalizovat snadněji konzistentní dotazu v případě vzorů než pro náhodné shluky aktivity.

Další informace o službě Advisor databáze SQL najdete v tématu [Poradce pro funkci SQL Database](https://azure.microsoft.com/en-us/documentation/articles/sql-database-advisor/).

![Doporučení k databázi SQL](./media/advisor-performance-recommendations/advisor-performance-sql.png)

## <a name="improve-redis-cache-performance-and-reliability"></a>Zlepšení výkonu Redis Cache a spolehlivosti

Advisor identifikuje instance služby Redis Cache kde výkon může být nepříznivě ovlivněn velké množství paměti, zatížení serveru, šířky pásma sítě nebo velký počet připojení klientů. Služba Advisor navíc poskytuje nejlepší postupy předejít problémům toohelp doporučení. Další informace o doporučení Redis Cache najdete v tématu [Redis Cache Advisor](https://azure.microsoft.com/en-us/documentation/articles/cache-configure/#redis-cache-advisor).


## <a name="improve-app-service-performance-and-reliability"></a>Zlepšení výkonu služby App Service a spolehlivosti

Azure Advisor integruje doporučení pro zlepšení prostředí aplikační služby a zjišťování možnosti příslušné platformy. Příklady App Services doporučení:
* Zjišťování instancí, kde se pomocí aplikace moduly runtime s možnostmi zmírnění vyčerpání paměti nebo prostředků procesoru.
* Zjišťování instancí, které tyto prostředky jako webové aplikace a databáze může zvýšit výkon a nižší náklady. 

Další informace o App Services doporučení najdete v tématu [osvědčené postupy pro službu Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/app-service-best-practices/).
![Doporučení služby aplikace](./media/advisor-performance-recommendations/advisor-performance-app-service.png)

## <a name="how-tooaccess-performance-recommendations-in-advisor"></a>Jak tooaccess výkonu doporučení v Advisor

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).

2. V levém podokně hello, klikněte na **další služby**.

3. V hello služby nabídky podokně v části **monitorování a správu**, klikněte na tlačítko **Azure Advisor**.  
 se zobrazí řídicí panel Advisor Hello.

4. Na řídicím panelu hello Advisor, klikněte na tlačítko hello **výkonu** kartě.

5. Vyberte hello předplatné, pro který chcete tooreceive doporučení a pak klikněte na tlačítko **získat doporučení**.

> [!NOTE]
> tooaccess doporučení služby Advisor, musíte nejdřív *zaregistrovat předplatné* službou Advisor. Předplatné je zaregistrován při *předplatné vlastníka* spustí hello Advisor řídicí panel a klikne na tlačítko hello **získat doporučení** tlačítko. Toto je *jednorázovou operaci*. Po registraci předplatného hello dostanete doporučení služby Advisor jako *vlastníka*, *Přispěvatel*, nebo *čtečky* pro předplatné, skupinu prostředků nebo konkrétní prostředek.

## <a name="next-steps"></a>Další kroky

toolearn Další informace o doporučení služby Advisor, najdete v části:

* [TooAdvisor Úvod](advisor-overview.md)
* [Začínáme se službou Advisor](advisor-get-started.md)
* [Náklady na doporučení služby Advisor](advisor-performance-recommendations.md)
* [Doporučení pro vysokou dostupnost služby Advisor](advisor-high-availability-recommendations.md)
* [Doporučení zabezpečení Advisor](advisor-security-recommendations.md)

