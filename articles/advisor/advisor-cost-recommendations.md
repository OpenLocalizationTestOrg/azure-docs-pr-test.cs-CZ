---
title: "doporučení služby Advisor náklady aaaAzure | Microsoft Docs"
description: "Pomocí Azure Advisor toooptimize hello náklady na nasazení Azure."
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
ms.openlocfilehash: 50f70c33a17f550c8753795435cdfddd51e409f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="advisor-cost-recommendations"></a>Náklady na doporučení služby Advisor

Pomáhá Advisor optimalizovat a snížit vaše celkové Azure tráví určením nečinnosti a nedostatečně prostředky. Můžete získat náklady doporučení z hello **náklady** karty na řídicím panelu Advisor hello.

![Karta náklady na Advisor](./media/advisor-cost-recommendations/advisor-cost-tab2.png)

## <a name="optimize-virtual-machine-spend-by-resizing-underutilized-instances"></a>Optimalizovat virtuální počítač tráví změnou velikosti nedostatečně instancí 
I když některé scénáře aplikací může být nízkou míru využívání návrhu, můžete často ušetřit peníze pomocí správy hello velikost a počet virtuálních počítačů. Advisor monitoruje vaše využití virtuálního počítače po dobu 14 dnů a pak identifikuje nízké využití virtuálních počítačů. Virtuální počítače, jejichž využití procesoru je 5 % nebo méně a využití sítě je 7 MB nebo méně čtyři nebo více dní jsou považovány za nízké využití virtuálních počítačů.

Zobrazuje Advisor hello odhadované náklady pokračováním toorun virtuálního počítače, takže můžete tooshut ho dolů nebo jeho velikost.  

![Advisor náklady doporučení pro změnu velikosti virtuálních počítačů](./media/advisor-cost-recommendations/advisor-cost-resizevms.png)

## <a name="use-a-cost-effective-solution-toomanage-performance-goals-of-multiple-sql-databases"></a>Použít výkonnostní cíle toomanage nákladově efektivní řešení více databází SQL
Advisor identifikuje instance serveru SQL, které můžete využít možnost vytvoření fondů elastické databáze. Fondy elastické databáze poskytují jednoduché a nákladově efektivní řešení toomanage hello výkonu cílů více databází, které mají různou vzorce používání. Další informace o Azure elastické fondy najdete v tématu [co je Azure Elastickém fondu?](https://azure.microsoft.com/en-us/documentation/articles/sql-database-elastic-pool/).

![Advisor náklady doporučení pro fondy elastické databáze](./media/advisor-cost-recommendations/advisor-cost-elasticdbpools.png)

## <a name="how-tooaccess-cost-recommendations-in-azure-advisor"></a>Jak tooaccess náklady doporučení v Azure Advisor

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).

2. V levém podokně hello, klikněte na **další služby**.

3. V hello služby nabídky podokně v části **monitorování a správu**, klikněte na tlačítko **Azure Advisor**.  
 se zobrazí řídicí panel Advisor Hello.

4. Na řídicím panelu hello Advisor, klikněte na tlačítko hello **náklady** kartě.

5. Vyberte hello předplatné, pro který chcete tooreceive doporučení a pak klikněte na tlačítko **získat doporučení**.

> [!NOTE]
> tooaccess doporučení služby Advisor, musíte nejdřív *zaregistrovat předplatné* službou Advisor. Předplatné je zaregistrován při *předplatné vlastníka* spustí hello Advisor řídicí panel a klikne na tlačítko hello **získat doporučení** tlačítko. Toto je *jednorázovou operaci*. Po registraci předplatného hello dostanete doporučení služby Advisor jako *vlastníka*, *Přispěvatel*, nebo *čtečky* pro předplatné, skupinu prostředků nebo konkrétní prostředek.

## <a name="next-steps"></a>Další kroky

toolearn Další informace o doporučení služby Advisor, najdete v části:
* [TooAdvisor Úvod](advisor-overview.md)
* [Začínáme](advisor-get-started.md)
* [Poradce při hodnocení výkonu doporučení](advisor-cost-recommendations.md)
* [Doporučení pro vysokou dostupnost služby Advisor](advisor-cost-recommendations.md)
* [Doporučení zabezpečení Advisor](advisor-cost-recommendations.md)
