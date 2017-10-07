---
title: "výkon aaaTroubleshoot problémy a optimalizovat databázi | Microsoft Docs"
description: "Použít tooyour doporučení výkonu databáze SQL a také mazat jak hello toogain přehled o výkonu dotazů hello spuštěným pro vaši databázi"
metakeywords: azure sql database performance monitoring recommendation
services: sql-database
documentationcenter: 
manager: jhubbard
author: jan-eng
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,monitor & tune
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: janeng
ms.openlocfilehash: e948d30ac74eecf45420d5d77ef55e3c0b6f3f47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-performance-issues-and-optimize-your-database"></a>Řešení potíží s výkonem a optimalizací databáze

Častým důvodem toho, že databáze je pomalá, jsou chybějící indexy a nedostatečně optimalizované dotazy. V tomto kurzu jste postup:
> [!div class="checklist"]
> * Zkontrolujte, použití a vracet doporučení pro zlepšení výkonu
> * Najít dotazy s využitím vysoké prostředků
> * Najít dlouho běžící dotazy

> Je nutné nepřetržité úlohy na databázi s problémy s výkonem – chybí například tooreceive doporučení indexu.
>

## <a name="log-in-toohello-azure-portal"></a>Přihlaste se toohello portálu Azure

Přihlaste se toohello [portál Azure](https://portal.azure.com/).

## <a name="review-and-apply-a-recommendation"></a>Zkontrolovat a použít doporučení

Postupujte podle těchto kroků tooapply doporučení z hello systému pro vaši databázi:

1. Klikněte na tlačítko hello **výkonu doporučení** nabídky v okně databáze hello.

    ![doporučení výkonu](./media/sql-database-performance-tutorial/perf_recommendations.png)

2. Hello seznam doporučení vyberte active doporučení. V tomto příkladu Create Index.

    ![Vyberte doporučení](./media/sql-database-performance-tutorial/create_index.png)

3. Kliknutím na hello použít hello doporučení **použít** tlačítko. Volitelně můžete zkontrolovat podrobnosti o doporučení hello a zjistit hello T-SQL skriptu příliš provést kliknutím na **zobrazit skript** tlačítko.

    ![použít doporučení](./media/sql-database-performance-tutorial/apply.png)

4. [Nepovinné] Povolte automatické ladění pro toobe doporučení použít automaticky.

    ![Automatické ladění](./media/sql-database-performance-tutorial/auto_tuning.png)

## <a name="revert-a-recommendation"></a>Vrátit doporučení

Hello databáze Advisor monitoruje každé doporučení implementována. Pokud doporučení nedojde ke zlepšení hello zatížení se budou automaticky zrušeny. Ručně vrácení doporučení je možné, ale ve většině případů není nezbytné. toorevert doporučení:

1. Přejděte toohello výkonu doporučení nabídku a vyberte jednu z hello použít doporučení.

    ![Vyberte doporučení](./media/sql-database-performance-tutorial/select.png)

2. V zobrazení Podrobnosti o hello, klikněte na tlačítko **vrátit**.

    ![vrátit doporučení](./media/sql-database-performance-tutorial/revert.png)

## <a name="find-hello-query-that-consumes-hello-most-resources"></a>Najít hello dotazu, který využívá hello nejvíce zdrojů

Postupujte podle těchto kroků dotazu hello toofind využívání hello nejvíce zdrojů:

1. Klikněte na hello **Query Performance Insight** nabídky v okně databáze hello.

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/query_perf_insights.png)

2. Vyberte typ prostředku.

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/select_resource_type.png)

3. Vyberte první dotaz hello v tabulce hello.

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/select_query.png)

4. Zkontrolujte podrobnosti hello dotazu.

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/query_details.png)

## <a name="find-hello-longest-running-query"></a>Vyhledat nejdelší spuštěné dotaz hello

1. Přejděte tooQuery informace o výkonu a vyberte možnost hello **dlouho běžící dotazy** kartě.

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/long_running.png)

3. Vyberte první dotaz hello v tabulce hello.

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/select_first_query.png)

4. Zkontrolujte podrobnosti hello dotazu.

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/review_query_details.png)



## <a name="next-steps"></a>Další kroky 
Častým důvodem toho, že databáze je pomalá, jsou chybějící indexy a nedostatečně optimalizované dotazy. V tomto kurzu jste se dozvěděli na:
> [!div class="checklist"]
> * Zkontrolujte, použití a vracet doporučení pro zlepšení výkonu
> * Najít dotazy s využitím vysoké prostředků
> * Najít dlouho běžící dotazy

[Tipy pro optimalizaci výkonu databáze SQL](https://docs.microsoft.com/azure/sql-database/sql-database-troubleshoot-performance)
