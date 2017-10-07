---
title: "aaaCreate tabulkový model pomocí návrháře Azure Analysis Services – webové hello | Microsoft Docs"
description: "Zjistěte, jak toocreate tabulkový model pomocí Azure Analysis Services hello Návrhář na portálu Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/21/2017
ms.author: owend
ms.openlocfilehash: a37b326b76c84fc3a4300827bc1c8706b0584701
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-model-in-azure-portal"></a>Vytvoření modelu na portálu Azure

Hello Azure Analysis Services webové návrháře (preview) funkce na portálu Azure poskytuje rychlý a snadný způsob toocreate a upravit tabulkové modely a dotaz modelu dat vpravo v prohlížeči. 

Mějte na paměti, Návrhář hello **preview**. Když se přidává nové funkce všechny hello čas, ve verzi preview, funkce je omezena. Pro pokročilejší model vývoje a testování je nejlepší toouse Visual Studio (SSDT) a SQL Server Management Studio (SSMS).

## <a name="prerequisites"></a>Požadavky

- Serveru Azure Analysis Services na úroveň Standard nebo vývojáře hello. Nové modelů vytvořených pomocí návrháře webové hello jsou DirectQuery, podporuje pouze tyto vrstvy.
- Databáze SQL Azure, Azure SQL Data Warehouse nebo souboru Power BI Desktop (.pbix) jako zdroj dat. Nové modely vytvořené z Power BI Desktop podporu soubory databáze SQL Azure, Azure SQL Data Warehouse, Oracle a Teradata datové zdroje.
- Účet systému SQL Server a heslo pro připojení zdroje dat tooAzure SQL Database nebo Azure SQL Data Warehouse.

## <a name="toocreate-a-new-tabular-model"></a>toocreate nový tabulkový model.

1. Na vašem serveru **přehled** okno > **Návrhář**, klikněte na tlačítko **otevřete**.

    ![Vytvoření modelu na portálu Azure](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. V **Návrhář** > **modely**, klikněte na tlačítko **+ přidat**.

    ![Vytvoření modelu na portálu Azure](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. V **nový model**, zadejte název modelu a potom vyberte zdroj dat.

    ![Dialogové okno Nový model na portálu Azure](./media/analysis-services-create-model-portal/aas-create-portal-new-model.png)

4. V **Connect**, zadejte vlastnosti připojení hello. Uživatelské jméno a heslo musí být účet systému SQL Server.

     ![Připojit dialogové okno na portálu Azure](./media/analysis-services-create-model-portal/aas-create-portal-connect.png)

5. V **tabulek a zobrazení**, vyberte tooinclude hello tabulky v modelu a pak klikněte na tlačítko **vytvořit**. Jsou automaticky vytvářet relace mezi tabulkami s pár klíčů.

     ![Vyberte tabulky a zobrazení](./media/analysis-services-create-model-portal/aas-create-portal-tables.png)

Nový model se zobrazí v prohlížeči. Tady můžete:   

- Dotaz na data modelu tak, že Návrháře dotazů toohello pole přetažením přidáte filtry.
- Vytvořte nové míry v tabulkách.
- Úpravy metadat modelu s použitím hello json editor.
- Visual Studio (SSDT), Power BI Desktop nebo aplikace Excel otevřít hello model.

![Vyberte tabulky a zobrazení](./media/analysis-services-create-model-portal/aas-create-portal-query.png)

> [!NOTE]
> Když budete upravovat metadata modelu nebo vytvářet nové míry v prohlížeči, ukládáte modelu tooyour tyto změny v Azure. Také při práci na modelu SSDT, Power BI Desktop nebo aplikace Excel, můžete získat váš model synchronizován.


## <a name="next-steps"></a>Další kroky 
[Spravovat role databáze a uživatele](analysis-services-database-users.md)  
[Propojení s Excelem](analysis-services-connect-excel.md)  


