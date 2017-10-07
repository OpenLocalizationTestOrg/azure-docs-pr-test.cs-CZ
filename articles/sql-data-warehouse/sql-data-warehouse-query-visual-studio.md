---
title: "aaaConnect tooAzure SQL Data Warehouse - služby VSTS | Microsoft Docs"
description: "Dotazování SQL Data Warehouse pomocí sady Visual Studio."
services: sql-data-warehouse
documentationcenter: NA
author: antvgski
manager: jhubbard
editor: 
ms.assetid: daace889-95e5-4826-b2fc-047eac9d6d95
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
ms.date: 10/31/2016
ms.author: anvang;barbkess
ms.openlocfilehash: 55eef4dff3e0647be5a735295bc89b43eb456079
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toosql-data-warehouse-with-visual-studio-and-ssdt"></a>Připojení k sadě Visual Studio a rozšíření SSDT tooSQL datového skladu
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Pomocí sady Visual Studio tooquery Azure SQL Data Warehouse za několik minut. Tato metoda používá hello rozšíření SQL Server Data Tools (SSDT) v sadě Visual Studio. 

## <a name="prerequisites"></a>Požadavky
toouse tohoto kurzu potřebujete:

* Existující SQL Data Warehouse. toocreate, najdete v části [vytvořit SQL Data Warehouse][Create a SQL Data Warehouse].
* SSDT pro Visual Studio. Pokud máte aplikaci Visual Studio, pravděpodobně již databázi máte. Pokyny k instalaci a možnosti najdete v tématu věnovaném [instalaci sady Visual Studio a rozšíření SSDT][Installing Visual Studio and SSDT].
* Hello plně kvalifikovaný název serveru SQL server. toofind, najdete v tématu [připojit tooSQL datového skladu][Connect tooSQL Data Warehouse].

## <a name="1-connect-tooyour-sql-data-warehouse"></a>1. Připojit tooyour SQL Data Warehouse
1. Otevřete Visual Studio 2013 nebo 2015.
2. Otevřete Průzkumník objektů SQL Serveru. toodo se vyberte **zobrazení** > **Průzkumník objektů systému SQL Server**.
   
    ![Průzkumník objektů systému SQL Server][1]
3. Klikněte na tlačítko hello **přidat SQL Server** ikonu.
   
    ![Přidat SQL Server][2]
4. Vyplňte pole hello v okně tooServer hello připojit.
   
    ![Připojit tooServer][3]
   
   * **Název serveru**: Zadejte hello **název serveru** dříve identifikovat.
   * **Ověřování**: Vyberte **Ověřování serveru SQL Server** nebo **Integrované ověřování Active Directory**.
   * **Uživatelské jméno** a **Heslo**: Pokud jste výše vybrali možnost Ověřování serveru SQL Server, zadejte uživatelské jméno a heslo.
   * Klikněte na **Připojit**.
5. tooexplore, rozbalte položku serveru Azure SQL. Můžete zobrazit hello databáze přidružené k serveru hello. Rozbalte položku AdventureWorksDW toosee hello tabulky v ukázkové databázi.
   
    ![Prozkoumejte AdventureWorksDW.][4]

## <a name="2-run-a-sample-query"></a>2. Spuštění ukázkového dotazu
Teď, když připojení bylo navázáno tooyour databáze, můžete napsat dotaz.

1. Pravým tlačítkem myši klikněte na databázi v Průzkumníku objektů systému SQL Server.
2. Vyberte **Nový dotaz**. Otevře se nové okno dotazu.
   
    ![Nový dotaz][5]
3. Zkopírujte tento dotaz TSQL do okna dotazu hello:
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. Spusťte dotaz hello. toodo, klikněte na tlačítko hello zelenou šipku nebo použijte následující zástupce hello: `CTRL` + `SHIFT` + `E`.
   
    ![Spuštění dotazu][6]
5. Podívejte se na výsledky dotazu hello. V tomto příkladu má hello tabulka FactInternetSales 60 398 řádků.
   
    ![Výsledky dotazu][7]

## <a name="next-steps"></a>Další kroky
Teď, když se můžete připojit a dotazu, zkuste [vizualizace hello dat pomocí PowerBI][visualizing hello data with PowerBI].

tooconfigure prostředí pro ověřování Azure Active Directory, najdete v části [ověření tooSQL datového skladu][Authenticate tooSQL Data Warehouse].

<!--Arcticles-->
[Connect tooSQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[Authenticate tooSQL Data Warehouse]: sql-data-warehouse-authentication.md
[visualizing hello data with PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md  

<!--Other-->
[Azure portal]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-query-visual-studio/open-ssdt.png
[2]: media/sql-data-warehouse-query-visual-studio/add-server.png
[3]: media/sql-data-warehouse-query-visual-studio/connection-dialog.png
[4]: media/sql-data-warehouse-query-visual-studio/explore-sample.png
[5]: media/sql-data-warehouse-query-visual-studio/new-query2.png
[6]: media/sql-data-warehouse-query-visual-studio/run-query.png
[7]: media/sql-data-warehouse-query-visual-studio/query-results.png
