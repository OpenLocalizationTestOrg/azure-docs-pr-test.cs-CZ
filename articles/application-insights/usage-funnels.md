---
title: "aaaAzure nálevky Application Insights"
description: "Zjistěte, jak můžete pomocí nálevky toodiscover jak zákazníci komunikují s vaší aplikací."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: bwren
ms.openlocfilehash: 2a6125cf596570cfaee30bb3ff757916e90d7676
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="discover-how-customers-are-using-your-application-with-hello-application-insights-funnels"></a>Zjistit, jak zákazníci používají aplikace s hello nálevky Application Insights

Principy zkušeností zákazníků je hello velký důraz tooyour firmy. Pokud vaše aplikace zahrnuje několik fází, tooknow je nutné, pokud se většina zákazníků pokročíte prostřednictvím hello celý proces, nebo pokud jsou jejich ukončení procesu hello v určitém okamžiku. rozšiřování Hello prostřednictvím sérii kroků ve webové aplikaci se označuje jako "trychtýřového grafu". Můžete použít hello Application Insights nálevky toogain přehledy o vašich uživatelů a monitorování podrobné převod sazby. 

## <a name="get-started-with-hello-funnels-blade"></a>Začínáme s okno nálevky hello
Hello nejjednodušší způsob, jak toolearn o nálevky je toowalk, když příklad. Hello následující obrázky ukazují hello vlastníci kroky elektronické obchodování bude trvat, než se toolearn jak zákazníkům komunikovat s touto webovou aplikací.  

### <a name="create-your-funnel"></a>Vytvoření vaší trychtýřového grafu
Než vytvoříte vaší trychtýřového grafu, je třeba toodecide na otázku hello chcete tooanswer. Například můžete tooknow jak mnoho zákazníků zobrazení vaší domovské stránce kliknutím na oznámení o inzerovaném programu. V tomto příkladu má hello vlastníci hello společnosti Fabrikam Fiber tooknow hello podíl zákazníci, kteří nákup po přidání položky tootheir nákupní košík během hello poslední měsíc.

Tady jsou kroky hello jejich trvat toocreate jejich trychtýřového grafu.

1. Klikněte na tlačítko Nový hello v okně nálevky hello.
1. Vyberte časové rozmezí hello "Poslední měsíc" z hello **časový rozsah** rozevíracího seznamu. 
1. Vyberte hello **stránky produktu** událost z hello **kroku 1** rozevíracího seznamu. 
1. Vyberte hello **přidat tooshopping košíku** událost z hello **kroku 2** rozevíracího seznamu.
1. Vyberte hello **kliknutím na nákup** událost z hello **krok 3** rozevíracího seznamu.
1. Přidat trychtýřového grafu s názvem toohello a klikněte na **Uložit**.

Hello následující obrázek ukazuje, že generuje okno nálevky hello dat hello. Z hello sem můžete zobrazit vlastníci společnosti Fabrikam, během posledního týdne hello, 22.7 % zákazníků, kteří přidáni tootheir položky nákupního košíku dokončené hello nákupu. Můžete také zobrazit, 1 % hello zákazníků před hostujících stránku hello produktů a 20 % zákazníkům odhlášení po dokončení jejich nákup kliknutí na oznámení o inzerovaném programu.


![Okno nálevky s daty](./media/app-insights-understand-usage-patterns/funnel1.png)

## <a name="next-steps"></a>Další kroky
  * [Přehled využití](app-insights-usage-overview.md)
  * [Uživatelů, relací a události](app-insights-usage-segmentation.md)
  * [Uchování](app-insights-usage-retention.md)
  * [Workbooks](app-insights-usage-workbooks.md)
  * [Přidat uživatelský kontext](app-insights-usage-send-user-context.md)
