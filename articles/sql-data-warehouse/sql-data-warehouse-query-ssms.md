---
title: aaaConnect tooAzure SQL Data Warehouse - SSMS | Microsoft Docs
description: "Pomocí SQL Server Management Studio (SSMS) tooconnect tooand dotazu Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: 
author: antvgski
manager: jhubbard
editor: 
ms.assetid: 299e50b3-e68a-471c-8aee-b0b9874781bd
ms.service: sql-data-warehouse
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.custom: connect
ms.date: 10/31/2016
ms.author: anvang;barbkess
ms.openlocfilehash: bcbaf7139d2e5183b388b8d58c015cf5ad726722
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toosql-data-warehouse-with-sql-server-management-studio-ssms"></a>Připojit tooSQL datového skladu s SQL Server Management Studio (SSMS)
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Pomocí SQL Server Management Studio (SSMS) tooconnect tooand dotazu Azure SQL Data Warehouse. 

## <a name="prerequisites"></a>Požadavky
toouse tohoto kurzu potřebujete:

* Existující SQL Data Warehouse. toocreate, najdete v části [vytvořit SQL Data Warehouse][Create a SQL Data Warehouse].
* SQL Server Management Studio (SSMS) nainstalována. [Instalace aplikace SSMS] [ Install SSMS] zdarma, pokud ještě nemáte ho.
* Hello plně kvalifikovaný název serveru SQL server. toofind, najdete v tématu [připojit tooSQL datového skladu][Connect tooSQL Data Warehouse].

## <a name="1-connect-tooyour-sql-data-warehouse"></a>1. Připojit tooyour SQL Data Warehouse
1. Otevřete aplikaci SSMS.
2. Otevřete Průzkumník objektů. toodo se vyberte **soubor** > **připojit Průzkumník objektů**.
   
    ![Průzkumník objektů systému SQL Server][1]
3. Vyplňte pole hello v okně tooServer hello připojit.
   
    ![Připojit tooServer][2]
   
   * **Název serveru**: Zadejte hello **název serveru** dříve identifikovat.
   * **Ověřování**: Vyberte **Ověřování serveru SQL Server** nebo **Integrované ověřování Active Directory**.
   * **Uživatelské jméno** a **Heslo**: Pokud jste výše vybrali možnost Ověřování serveru SQL Server, zadejte uživatelské jméno a heslo.
   * Klikněte na **Připojit**.
4. tooexplore, rozbalte položku serveru Azure SQL. Můžete zobrazit hello databáze přidružené k serveru hello. Rozbalte položku AdventureWorksDW toosee hello tabulky v ukázkové databázi.
   
    ![Prozkoumejte AdventureWorksDW.][3]

## <a name="2-run-a-sample-query"></a>2. Spuštění ukázkového dotazu
Teď, když připojení bylo navázáno tooyour databáze, můžete napsat dotaz.

1. Pravým tlačítkem myši klikněte na databázi v Průzkumníku objektů systému SQL Server.
2. Vyberte **Nový dotaz**. Otevře se nové okno dotazu.
   
    ![Nový dotaz][4]
3. Zkopírujte tento dotaz TSQL do okna dotazu hello:
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. Spusťte dotaz hello. toodo tento, klikněte na tlačítko `Execute` nebo hello použijte následující klávesové: `F5`.
   
    ![Spuštění dotazu][5]
5. Podívejte se na výsledky dotazu hello. V tomto příkladu má hello tabulka FactInternetSales 60 398 řádků.
   
    ![Výsledky dotazu][6]

## <a name="next-steps"></a>Další kroky
Teď, když se můžete připojit a dotazu, zkuste [vizualizace hello dat pomocí PowerBI][visualizing hello data with PowerBI].

tooconfigure prostředí pro ověřování Azure Active Directory, najdete v části [ověření tooSQL datového skladu][Authenticate tooSQL Data Warehouse].

<!--Arcticles-->
[Connect tooSQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Authenticate tooSQL Data Warehouse]: sql-data-warehouse-authentication.md
[visualizing hello data with PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md 

<!--Other-->
[Azure portal]: https://portal.azure.com
[Install SSMS]: https://msdn.microsoft.com/en-US/library/hh213248.aspx


<!--Image references-->

[1]: media/sql-data-warehouse-query-ssms/connect-object-explorer.png
[2]: media/sql-data-warehouse-query-ssms/connect-object-explorer1.png
[3]: media/sql-data-warehouse-query-ssms/explore-tables.png
[4]: media/sql-data-warehouse-query-ssms/new-query.png
[5]: media/sql-data-warehouse-query-ssms/execute-query.png
[6]: media/sql-data-warehouse-query-ssms/results.png
