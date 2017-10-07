---
title: "aaaConnect Excel tooSQL databáze | Microsoft Docs"
description: "Zjistěte, jak databázi aplikace Microsoft Excel tooAzure tooconnect SQL v cloudu hello. Naimportujte si data do Excelu, kde můžete data dále zkoumat a vytvářet z nich sestavy."
services: sql-database
keywords: "připojení aplikace excel toosql, naimportujte tooexcel dat"
documentationcenter: 
author: joseidz
manager: jhubbard
editor: 
ms.assetid: 906924bc-2707-48d3-bac6-397976a0409d
ms.service: sql-database
ms.custom: develop apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2017
ms.author: jhubbard
ms.openlocfilehash: 0048849432023145bd1009d45b6d9b64a9c7ac3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-excel-tooan-azure-sql-database-and-create-a-report"></a>Připojení aplikace Excel tooan Azure SQL database a vytvoření sestavy

Připojení aplikace Excel tooa databáze SQL v cloudu hello a importovat data a vytvářet tabulky a grafy na základě hodnot v databázi hello. V tomto kurzu nastavíte hello připojení mezi Excelem a databázovou tabulku uložte soubor hello, která ukládá data a hello informace o připojení pro Excel a pak vytvoříte kontingenční graf z hello hodnot v databázi.

Než začnete, budete potřebovat databázi SQL v Azure. Pokud nemáte, přečtěte si téma [vytvořit svoji první databázi SQL](sql-database-get-started-portal.md) tooget databázi s ukázkovými daty fungovaly za pár minut. V tomto článku budete importovat ukázková data do aplikace Excel z tohoto článku, ale můžete provést kroky pro podobné s vlastními daty.

Budete také potřebovat Excel. V tomto článku používáme [Microsoft Excel 2016](https://products.office.com/).

## <a name="connect-excel-tooa-sql-database-and-create-an-odc-file"></a>Připojení aplikace Excel tooa SQL database a vytvoření souboru odc
1. tooconnect Excel tooSQL databáze, otevřete aplikaci Excel a vytvoření nového sešitu nebo otevřít existující sešit aplikace Excel.
2. V řádku nabídek hello hello horní části stránky hello klikněte na **Data**, klikněte na tlačítko **z jiných zdrojů**a potom klikněte na **z SQL serveru**.
   
   ![Vyberte zdroj dat: připojení Excelu tooSQL databáze.](./media/sql-database-connect-excel/excel_data_source.png)
   
   Otevře se Průvodce datovým připojením Hello.
3. V hello **připojit tooDatabase Server** dialogové okno, hello typ SQL Database **název serveru** chcete tooconnect tooin hello formulář <*servername* > **. database.windows.net**. Například **adworkserver.database.windows.net**.
4. V části **přihlašovací pověření**, klikněte na tlačítko **hello použijte následující uživatelské jméno a heslo**, typ hello **uživatelské jméno** a **heslo** nastavil pro Databáze SQL serveru text Hello, pokud jste ji vytvořili a potom klikněte na **Další**.
   
   ![Zadejte název a přihlašovací údaje serveru hello](./media/sql-database-connect-excel/connect-to-server.png)
   
   > [!TIP]
   > V závislosti na vašem síťovém prostředí nemusí být možné tooconnect nebo můžete ztratit připojení hello, pokud server hello databáze SQL nepovoluje přenos dat z IP adresy vašeho klienta. Přejděte toohello [portál Azure](https://portal.azure.com/), klikněte na SQL servery, klikněte na svůj server, klikněte v části nastavení brány firewall a přidejte IP adresu svého klienta. V tématu [jak nastavení brány firewall tooconfigure](sql-database-configure-firewall-settings.md) podrobnosti.
   > 
   > 
5. V hello **vybrat databázi a tabulku** dialogové okno, vyberte hello databázi toowork s hello seznamu a pak klikněte na tlačítko hello tabulek nebo zobrazení, které chcete toowork s (jsme zvolili **vGetAllCategories**) a potom Klikněte na tlačítko **Další**.
   
    ![Vyberte databázi a tabulku.](./media/sql-database-connect-excel/select-database-and-table.png)
   
    Hello **uložit soubor datového připojení a dokončit** otevře se dialogové okno, kde zadáte informace o hello Office databáze připojení (*.odc) souboru, který používá Excel. Můžete ponechat výchozí nastavení hello nebo si vybrané možnosti přizpůsobit.
6. Můžete ponechat výchozí hodnoty hello, ale Poznámka hello **název souboru** na konkrétní. A **popis**, **popisný název**, a **hledaná klíčová slova** vám pomůže a ostatní uživatelé mějte na paměti, co se připojujete tooand najít hello připojení. Klikněte na tlačítko **vždy pokusí toouse tato data souboru toorefresh** Pokud chcete informace o připojení, které jsou uložené v souboru odc hello, aby se mohl aktualizovat při připojení tooit a pak klikněte na tlačítko **Dokončit**.
   
    ![Uložení souboru odc](./media/sql-database-connect-excel/save-odc-file.png)
   
    Hello **importovat data** zobrazí se dialogové okno.

## <a name="import-hello-data-into-excel-and-create-a-pivot-chart"></a>Umožňuje importovat hello data do Excelu a vytvoření kontingenčního grafu
Teď, když jste vytvořili připojení hello a soubor vytvořený hello se data a informace o připojení, při čtení dat tooimport hello.

1. V hello **importovat Data** dialogové okno, klikněte na možnost hello chcete použít pro prezentování vašich dat v listu hello a pak klikněte na tlačítko **OK**. Zvolili jsme možnost **Kontingenční graf**. Můžete také toocreate **nový list** nebo příliš**přidat tato data tooa datový Model**. Další informace o datových modelech najdete v tématu [Vytvoření datového modelu v Excelu](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B). Klikněte na tlačítko **vlastnosti** tooexplore informace o souboru odc hello jste vytvořili v hello předchozí krok a toochoose možnosti pro aktualizaci dat hello.
   
    ![Výběr hello formát dat v aplikaci Excel](./media/sql-database-connect-excel/import-data.png)
   
    Hello list má teď prázdnou kontingenční tabulku a graf.
2. V části **pole kontingenční tabulky**, vyberte všechny hello zaškrtněte políčka pro hello chcete tooview pole.
   
    ![Nakonfigurujte sestavu databáze.](./media/sql-database-connect-excel/power-pivot-results.png)

> [!TIP]
> Pokud chcete tooconnect jiné databáze pro toohello sešity a listy aplikace Excel, klikněte na tlačítko **Data**, klikněte na tlačítko **připojení**, klikněte na tlačítko **přidat**, zvolte hello připojení, které jste vytvořili ze seznamu hello a pak klikněte na tlačítko **otevřete**.
> ![Otevření připojení z jiného sešitu](./media/sql-database-connect-excel/open-from-another-workbook.png)
> 
> 

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[připojit tooSQL databáze s SQL Server Management Studio](sql-database-connect-query-ssms.md) pro pokročilé dotazy a analýzy.
* Další informace o výhodách hello [elastické fondy](sql-database-elastic-pool.md).
* Zjistěte, jak příliš[vytvořit webovou aplikaci, která se připojuje tooSQL databáze na hello back-end](../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).

