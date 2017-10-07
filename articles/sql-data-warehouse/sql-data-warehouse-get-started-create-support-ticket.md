---
title: "aaaHow toocreate lístek podpory pro SQL Data Warehouse | Microsoft Docs"
description: "Jak toocreate podporu lístek v Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: a91d1f53-03cb-464b-9d5b-4a9c1a194ed3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: 72f7eac82112fb7f1bfb05abca4ce40aeb3c828c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-support-ticket-for-sql-data-warehouse"></a>Jak toocreate podporu lístek pro datový sklad SQL
Pokud máte se službou SQL Data Warehouse nějaké problémy, můžete si vytvořit lístek podpory, aby vám mohl náš technický tým pomoct.

> [!NOTE] 
> Od 12/20/2016 není přesný hello Kontrola stavu prostředků v hello portálu Azure. Aktivně pracujeme toofix tento problém. 


## <a name="create-a-support-ticket"></a>Vytvoření lístku podpory
1. Otevřete hello [portál Azure][Azure portal].
2. Na domovské obrazovce hello, klikněte na tlačítko hello **Nápověda a podpora** dlaždici.
   
    ![Nápověda a podpora](./media/sql-data-warehouse-get-started-create-support-ticket/help-support.png)
3. Na hello nápovědy + podporu okno, klikněte na tlačítko **vytvořit žádost o podporu**.
   
    ![Nová žádost o podporu](./media/sql-data-warehouse-get-started-create-support-ticket/create-support-request.png)
   
    <a name="request-quota-change"></a> 
4. Vyberte hello **typ žádosti**.
   
    ![Typ žádosti](./media/sql-data-warehouse-get-started-create-support-ticket/request-type.png)
   
   > [!NOTE]
   > Ve výchozím nastavení obsahuje každý server SQL (např. myserver.database.windows.net) **kvóty DTU** o hodnotě 45 000. Tato kvóta je jednoduše bezpečnostní omezení. Vytvoření lístku podpory a výběrem může zvýšení kvóty *kvóty* jako typ požadavku hello. toocalculate potřebuje, vynásobte vaší DTU hello 7.5 pomocí hello celkový [DWU] [ DWU] potřeby. Například byste chtěli toohost dva DW6000s na jednom SQL serveru a pak měli požádat o kvótě DTU z 90,000.  Vaše aktuální spotřeba DTU z okna hello SQL serveru můžete zobrazit na portálu hello. Databáze pozastavený a zrušení pozastavený započítávat hello kvóty DTU. 
   > 
   > 
5. Vyberte hello **předplatné** , hostitelé hello databáze s problémem hello podávají zprávy.
   
    ![Předplatné](./media/sql-data-warehouse-get-started-create-support-ticket/subscription.png)
6. Vyberte **SQL Data Warehouse** jako hello prostředků.
   
    ![Prostředek](./media/sql-data-warehouse-get-started-create-support-ticket/resource.png)
7. Vyberte svůj [plán podpory Azure][Azure support plan].
   
   * **Podpora k fakturaci, správě kvót a předplatného** je k dispozici na všech úrovních podpory.
   * Podpora k problémům vyžadujícím **opravu** je poskytována prostřednictvím plánů podpory [Developer Support][Developer], [Standard Support][Standard], [Professional Direct][Professional Direct] nebo [Premier Support][Premier]. Opravu jsou problémy, které mají zákazníci při použití Azure tam, kde existuje se dá předpokládat tohoto problému hello způsobila společnosti Microsoft.
   * **Mentorování vývojářů** a **poradenské služby** jsou k dispozici na hello [Professional Direct] [ Professional Direct] a [úrovně Premier] [ Premier] úrovních podpory. 
     
     Pokud máte plán podpory Premier Support, můžete SQL Data Warehouse nahlásit na hello problémy související s [online portálu Microsoft Premier][Microsoft Premier online portal].  V tématu [plánům podpory Azure] [ Azure support plan] toolearn Další informace o různých podporovat plány, včetně rozsahu, doby odezvy, cen, hello atd.  Nejčastější dotazy týkající se podpory Azure najdete v [nejčastějších dotazech týkajících se podpory k Azure][Azure support FAQs].  
     
     ![Plán podpory](./media/sql-data-warehouse-get-started-create-support-ticket/support-plan.png)
8. Vyberte hello **typ problému** a **kategorie**. V tomto příkladu jsme vybrali "Nástroje" jako hello typ problému a "Client tools" jako hello kategorie. 
   
    ![Kategorie typu problému](./media/sql-data-warehouse-get-started-create-support-ticket/problem-type-category.png)
9. Popište problém hello a zvolte hello úroveň dopadu na firmu.
   
    ![Popis problému](./media/sql-data-warehouse-get-started-create-support-ticket/problem-description.png)
10. Předvyplní se vaše **kontaktní údaje** pro tento lístek podpory. V případě potřeby je aktualizujte.
    
    ![Kontaktní údaje](./media/sql-data-warehouse-get-started-create-support-ticket/contact-info.png)
11. Klikněte na tlačítko **vytvořit** žádost o podporu toosubmit hello.

## <a name="monitor-a-support-ticket"></a>Monitorování lístku podpory
Po odeslání žádosti o podporu hello tým podpory Azure hello vás kontaktovat. toocheck vaše stav a podrobnosti žádosti, klikněte na tlačítko **spravovat žádosti o podporu** na řídicím panelu hello.

![Zkontrolování stavu](./media/sql-data-warehouse-get-started-create-support-ticket/check-status.png)

## <a name="other-resources"></a>Další prostředky
Kromě toho můžete propojit s hello komunitou SQL Data Warehouse na [Stack Overflow] [ Stack Overflow] nebo na hello [fórum Azure SQL Data Warehouse na MSDN] [ Azure SQL Data Warehouse MSDN forum].

<!--Image references--> 

<!--Article references--> 
[DWU]: ./sql-data-warehouse-overview-what-is.md

<!--MSDN references--> 

<!--Other web references--> 
[Azure portal]: https://portal.azure.com/
[Azure support plan]: https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/  
[Developer]: https://azure.microsoft.com/support/plans/developer/  
[Standard]: https://azure.microsoft.com/support/plans/standard/  
[Professional Direct]: https://azure.microsoft.com/support/plans/prodirect/  
[Premier]: https://azure.microsoft.com/support/plans/premier/  
[Azure support FAQs]: https://azure.microsoft.com/support/faq/
[Microsoft Premier online portal]: https://premier.microsoft.com/
[Stack Overflow]: https://stackoverflow.com/questions/tagged/azure-sqldw/
[Azure SQL Data Warehouse MSDN forum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/

