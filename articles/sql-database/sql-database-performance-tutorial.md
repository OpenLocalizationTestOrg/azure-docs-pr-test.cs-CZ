---
title: "Řešení potíží s výkonem a optimalizací databáze | Microsoft Docs"
description: "Platí doporučení výkonu pro vaše databáze SQL a také mazat získáte přehled o výkonu dotazů spuštěným pro vaši databázi"
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
ms.openlocfilehash: f9ae96cdc80c347593f229cb2fce3f2d4d8e7caf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-performance-issues-and-optimize-your-database"></a>Řešení potíží s výkonem a optimalizací databáze

Častým důvodem toho, že databáze je pomalá, jsou chybějící indexy a nedostatečně optimalizované dotazy. V tomto kurzu jste postup:
> [!div class="checklist"]
> * Zkontrolujte, použití a vracet doporučení pro zlepšení výkonu
> * Najít dotazy s využitím vysoké prostředků
> * Najít dlouho běžící dotazy

> Je nutné nepřetržité úlohy na databázi s problémy s výkonem – chybí indexu třeba přijímat doporučení.
>

## <a name="log-in-to-the-azure-portal"></a>Přihlášení k portálu Azure Portal

Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).

## <a name="review-and-apply-a-recommendation"></a>Zkontrolovat a použít doporučení

Použijte následující postup platí doporučení ze systému pro vaši databázi:

1. Klikněte **výkonu doporučení** nabídky v okně databáze.

    ![doporučení výkonu](./media/sql-database-performance-tutorial/perf_recommendations.png)

2. Seznam doporučení vyberte active doporučení. V tomto příkladu Create Index.

    ![Vyberte doporučení](./media/sql-database-performance-tutorial/create_index.png)

3. Doporučení použít kliknutím **použít** tlačítko. Volitelně můžete zkontrolovat podrobnosti o doporučení a zjistit skriptu T-SQL, které by šlo spustit kliknutím na **zobrazit skript** tlačítko.

    ![použít doporučení](./media/sql-database-performance-tutorial/apply.png)

4. [Nepovinné] Povolte automatické ladění pro doporučení automaticky použije.

    ![Automatické ladění](./media/sql-database-performance-tutorial/auto_tuning.png)

## <a name="revert-a-recommendation"></a>Vrátit doporučení

Databáze Advisor monitoruje každé doporučení implementována. Pokud doporučení nedojde ke zvýšení zatížení se budou automaticky zrušeny. Ručně vrácení doporučení je možné, ale ve většině případů není nezbytné. Chcete-li vrátit doporučení:

1. Přejděte do nabídky doporučení výkonu a vyberte jednu z použité doporučení.

    ![Vyberte doporučení](./media/sql-database-performance-tutorial/select.png)

2. V podrobném zobrazení, klikněte na tlačítko **vrátit**.

    ![vrátit doporučení](./media/sql-database-performance-tutorial/revert.png)

## <a name="find-the-query-that-consumes-the-most-resources"></a>Vyhledat dotaz, který využívá nejvíce zdrojů

Použijte následující postup vyhledat dotaz využívání nejvíce zdrojů:

1. Klikněte na **Query Performance Insight** nabídky v okně databáze.

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/query_perf_insights.png)

2. Vyberte typ prostředku.

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/select_resource_type.png)

3. Vyberte první dotaz v tabulce.

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/select_query.png)

4. Zkontrolujte podrobnosti o dotazu.

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/query_details.png)

## <a name="find-the-longest-running-query"></a>Vyhledat nejdéle probíhající dotaz

1. Přejděte na Query Performance Insight a vyberte **dlouho běžící dotazy** kartě.

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/long_running.png)

3. Vyberte první dotaz v tabulce.

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/select_first_query.png)

4. Zkontrolujte podrobnosti o dotazu.

    ![Přehled o dotazu](./media/sql-database-performance-tutorial/review_query_details.png)



## <a name="next-steps"></a>Další kroky 
Častým důvodem toho, že databáze je pomalá, jsou chybějící indexy a nedostatečně optimalizované dotazy. V tomto kurzu jste se dozvěděli na:
> [!div class="checklist"]
> * Zkontrolujte, použití a vracet doporučení pro zlepšení výkonu
> * Najít dotazy s využitím vysoké prostředků
> * Najít dlouho běžící dotazy

[Tipy pro optimalizaci výkonu databáze SQL](https://docs.microsoft.com/azure/sql-database/sql-database-troubleshoot-performance)
