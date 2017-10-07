---
title: "aaaGet spuštění se Schedulerem na portálu Azure | Microsoft Docs"
description: "Začínáme se Schedulerem na portálu Azure"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: e69542ec-d10f-4f17-9b7a-2ee441ee7d68
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/10/2016
ms.author: deli
ms.openlocfilehash: 58255c0ad19da65932f8b1d36cb8fef1ff6e651b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-scheduler-in-azure-portal"></a>Začínáme se Schedulerem na portálu Azure
Je snadno toocreate naplánované úlohy ve službě Azure Scheduler. V tomto kurzu se dozvíte jak toocreate úlohu. Taky poznáte možnosti Scheduleru pro sledování a správu.

## <a name="create-a-job"></a>Vytvoření úlohy
1. Přihlaste se příliš[portál Azure](https://portal.azure.com/).  
2. Klikněte na tlačítko **+ nový** > typ *Plánovač* hello vyhledávacího pole > vyberte **Plánovač** ve výsledcích > klikněte na tlačítko **vytvořit**.
   
    ![][marketplace-create]
3. Teď vytvoříme úlohu, která jednoduše vyšle požadavek GET na http://www.microsoft.com/. V hello **Plánovač úloh** obrazovky, zadejte hello následující informace:
   
   1. **Název:**`getmicrosoft`  
   2. **Předplatné:** Vaše předplatné Azure   
   3. **Kolekce úloh:** Vyberte existující kolekci úloh nebo klikněte na **Vytvořit novou** &gt; zadejte název.
4. Vedle **nastavení akce**, definovat hello následující hodnoty:
   
   1. **Typ akce:**` HTTP`  
   2. **Metoda:**`GET`  
   3. **Adresa URL:**` http://www.microsoft.com`  
      
      ![][action-settings]
5. Nakonec nastavíme plán. Hello úlohy by se mohl definovat jako jednorázová, ale My vytvoříme plán opakování:
   
   1. **Opakování**: `Recurring`
   2. **Začátek**: Dnešní datum
   3. **Opakovat každý**: `12 Hours`
   4. **Konec**: Dva dny po dnešním datu  
      
      ![][recurrence-schedule]
6. Klikněte na **Vytvořit**

## <a name="manage-and-monitor-jobs"></a>Správa a sledování úloh
Po vytvoření úlohy se zobrazí v hello hlavním řídicím panelu Azure. Klikněte na úlohu hello a nový otevře se okno s hello následující karty:

1. Vlastnosti  
2. Nastavení akce  
3. Plán  
4. Historie
5. Uživatelé
   
   ![][job-overview]

### <a name="properties"></a>Vlastnosti
Tyto vlastnosti jen pro čtení popisují metadata správy hello pro úlohu Scheduleru hello.

   ![][job-properties]

### <a name="action-settings"></a>Nastavení akce
Kliknutím na úlohu v hello **úlohy** obrazovce můžete tooconfigure, které úlohy. Díky tomu můžete nakonfigurovat upřesňující nastavení, pokud jste nenakonfigurovali v hello Průvodci rychlým vytvořením.

Pro všechny typy akcí můžete změnit zásady opakovaných pokusů hello a hello akci chyby.

Pro akce úlohy typy HTTP a HTTPS můžete změnit tooany hello metoda akce HTTP povolena. Můžete také přidat, odstranit nebo změnit hello hlavičky a základní ověřovací údaje.

Pro akce fronty úložiště můžete změnit účet úložiště hello, název fronty, SAS token a text.

Pro akce sběrnice můžete změnit hello obor názvů, cestu k tématu/frontě, nastavení ověřování, typ přenosu, vlastnosti zprávy a text zprávy.

   ![][job-action-settings]

### <a name="schedule"></a>Plán
Díky tomu můžete znovu nakonfigurovat plán hello, pokud chcete toochange hello plán, který jste vytvořili v hello Průvodci rychlým vytvořením.

Jedná se možnost toobuild [komplexní plány a pokročilé opakování ve vaší úloze](scheduler-advanced-complexity.md)

Můžete změnit hello počáteční datum a čas, plán opakování a hello koncové datum a čas (Pokud hello úloha opakuje).

   ![][job-schedule]

### <a name="history"></a>Historie
Hello **historie** karta zobrazené vybrané metriky pro každé provedení úlohy v systému hello hello vybrané úlohy. Tyto metriky poskytují v reálném čase hodnoty ohledně kondice vašeho Scheduleru hello:

1. Status  
2. Podrobnosti  
3. Opakované pokusy
4. Výskyt: 1., 2., 3. atd.
5. Čas zahájení provedení  
6. Čas ukončení provedení
   
   ![][job-history]

Kliknutím na spustit tooview jeho **podrobnosti historie**, včetně kompletní odpovědi hello na každé provedení. Toto dialogové okno vám umožní toocopy hello odpovědi toohello schránky.

   ![][job-history-details]

### <a name="users"></a>Uživatelé
Řízení přístupu na základě role ve službě Azure Scheduler umožňuje přesnou správu přístupu. jak toouse hello kartě Uživatelé odkazovat příliš toolearn[řízení přístupu](../active-directory/role-based-access-control-configure.md)

## <a name="see-also"></a>Viz také
 [Co je Scheduler?](scheduler-intro.md)

 [Koncepty, terminologie a hierarchie entit Scheduleru](scheduler-concepts-terms.md)

 [Plány a fakturace v Azure Scheduleru](scheduler-plans-billing.md)

 [Jak toobuild komplexní plány a pokročilé opakování ve službě Azure Scheduler](scheduler-advanced-complexity.md)

 [Referenční materiály k rozhraní REST API Scheduleru](https://msdn.microsoft.com/library/mt629143)

 [Rutiny PowerShellu pro Scheduler – referenční informace](scheduler-powershell-reference.md)

 [Vysoká dostupnost a spolehlivost Scheduleru](scheduler-high-availability-reliability.md)

 [Omezení, výchozí hodnoty a kódy chyb Scheduleru](scheduler-limits-defaults-errors.md)

 [Odchozí ověření Scheduleru](scheduler-outbound-authentication.md)

[marketplace-create]: ./media/scheduler-get-started-portal/scheduler-v2-portal-marketplace-create.png
[action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-action-settings.png
[recurrence-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-recurrence-schedule.png
[job-properties]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-properties.png
[job-overview]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-overview-1.png
[job-action-settings]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-action-settings.png
[job-schedule]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-schedule.png
[job-history]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history.png
[job-history-details]: ./media/scheduler-get-started-portal/scheduler-v2-portal-job-history-details.png


[1]: ./media/scheduler-get-started-portal/scheduler-get-started-portal001.png
[2]: ./media/scheduler-get-started-portal/scheduler-get-started-portal002.png
[3]: ./media/scheduler-get-started-portal/scheduler-get-started-portal003.png
[4]: ./media/scheduler-get-started-portal/scheduler-get-started-portal004.png
[5]: ./media/scheduler-get-started-portal/scheduler-get-started-portal005.png
[6]: ./media/scheduler-get-started-portal/scheduler-get-started-portal006.png
[7]: ./media/scheduler-get-started-portal/scheduler-get-started-portal007.png
[8]: ./media/scheduler-get-started-portal/scheduler-get-started-portal008.png
[9]: ./media/scheduler-get-started-portal/scheduler-get-started-portal009.png
[10]: ./media/scheduler-get-started-portal/scheduler-get-started-portal010.png
[11]: ./media/scheduler-get-started-portal/scheduler-get-started-portal011.png
[12]: ./media/scheduler-get-started-portal/scheduler-get-started-portal012.png
[13]: ./media/scheduler-get-started-portal/scheduler-get-started-portal013.png
[14]: ./media/scheduler-get-started-portal/scheduler-get-started-portal014.png
[15]: ./media/scheduler-get-started-portal/scheduler-get-started-portal015.png
