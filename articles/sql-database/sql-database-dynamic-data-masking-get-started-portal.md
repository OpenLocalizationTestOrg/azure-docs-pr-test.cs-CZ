---
title: "Portál Azure: maskování dynamická data databáze SQL | Microsoft Docs"
description: "Jak tooget pracovat s dynamickými daty SQL Database maskování v hello portálu Azure"
services: sql-database
documentationcenter: 
author: ronitr
manager: jhubbard
editor: 
ms.assetid: "2"
ms.service: sql-database
ms.custom: security
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 11/22/2016
ms.author: ronitr; ronmat
ms.openlocfilehash: 5c5f74682a15d42eceb78c3ca0705401e1ac0ae7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-sql-database-dynamic-data-masking-with-hello-azure-portal"></a>Začínáme s dynamickými daty SQL Database maskování s hello portálu Azure

Toto téma ukazuje, jak tooimplement [dynamická data maskování](sql-database-dynamic-data-masking-get-started.md) s hello portálu Azure. Můžete taky implementovat pomocí maskování dynamická data [rutiny Azure SQL Database](https://msdn.microsoft.com/library/azure/mt574084.aspx) nebo hello [REST API](https://msdn.microsoft.com/library/dn505719.aspx).


## <a name="set-up-dynamic-data-masking-for-your-database-using-hello-azure-portal"></a>Nastavit maskování dynamická data pro vaši databázi pomocí hello portálu Azure
1. Spuštění hello Azure Portal na [https://portal.azure.com](https://portal.azure.com).
2. Přejděte okno nastavení toohello hello databáze, která zahrnuje hello citlivá data, která chcete toomask.
3. Klikněte na tlačítko hello **dynamického maskování dat** dlaždice, který spouští hello **dynamického maskování dat** okno konfigurace.
   
   * Alternativně můžete přejděte dolů toohello **operace** části a klikněte na tlačítko **dynamického maskování dat**.
     
     ![Navigační podokno](./media/sql-database-dynamic-data-masking-get-started/4_ddm_settings_tile.png)<br/><br/>
4. V hello **dynamického maskování dat** okno konfigurace, mohou se zobrazit některé sloupce databáze tento motor doporučení hello má označeny k maskování. V pořadí tooaccept hello doporučení, stačí kliknout na **přidat masku** pro jeden nebo více sloupců a masky bude vytvořena podle hello výchozí typ daného sloupce. Můžete změnit hello funkci maskování tak, že kliknete na pravidlo hello úpravy hello maskování pole formátu tooa jiného formátu podle svého výběru. Být jisti tooclick **Uložit** toosave nastavení.
   
    ![Navigační podokno](./media/sql-database-dynamic-data-masking-get-started/5_ddm_recommendations.png)<br/><br/>
5. tooadd masku pro všechny sloupce v databázi, hello horní části hello **dynamického maskování dat** konfigurace okna klikněte na tlačítko **přidat masku** tooopen hello **přidat pravidlo maskování** okno Konfigurace
   
    ![Navigační podokno](./media/sql-database-dynamic-data-masking-get-started/6_ddm_add_mask.png)<br/><br/>
6. Vyberte hello **schématu**, **tabulky** a **sloupec** toodefine hello určené pole, které se maskování.
7. Vyberte **maskování formátu pole** hello seznamu citlivých dat maskování kategorií.
   
    ![Navigační podokno](./media/sql-database-dynamic-data-masking-get-started/7_ddm_mask_field_format.png)<br/><br/>        
8. Klikněte na tlačítko **Uložit** v datech hello maskování pravidlo okno tooupdate hello sadu maskování pravidla v dynamické maskování zásad dat hello.
9. Uživatelé SQL hello typu nebo AAD identity, které mají být vyloučeny z maskování a mají přístup toohello odmaskováno citlivá data. To by měl být středníky oddělený seznam uživatelů. Všimněte si, že uživatelé s oprávněním správce vždy mají přístup toohello původní odmaskovaná data.
   
    ![Navigační podokno](./media/sql-database-dynamic-data-masking-get-started/8_ddm_excluded_users.png)
   
   > [!TIP]
   > toomake ji proto hello aplikační vrstvu můžete zobrazit citlivá data pro uživatele privilegovaný aplikací, přidejte hello uživatele SQL nebo AAD identity hello aplikace používá tooquery hello databáze. Důrazně doporučujeme, aby tento seznam obsahovat minimální počet mohou uživatelé s oprávněním toominimize ohrožení citlivých dat hello.
   > 
   > 
10. Klikněte na tlačítko **Uložit** v datech hello maskování konfigurace okna toosave hello maskování nové nebo aktualizované zásady.


## <a name="next-steps"></a>Další kroky

* Přehled maskování dynamickými daty naleznete v tématu [dynamická data maskování](sql-database-dynamic-data-masking-get-started.md).
* Můžete taky implementovat pomocí maskování dynamická data [rutiny Azure SQL Database](https://msdn.microsoft.com/library/azure/mt574084.aspx) nebo hello [REST API](https://msdn.microsoft.com/library/dn505719.aspx).
