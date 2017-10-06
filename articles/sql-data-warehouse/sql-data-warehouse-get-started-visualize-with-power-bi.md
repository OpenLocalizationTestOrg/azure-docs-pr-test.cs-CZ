---
title: "aaaVisualize dat SQL Data Warehouse pomocí Power BI – Microsoft Azure"
description: "Vizualizace dat SQL Data Warehouse pomocí Power BI"
services: sql-data-warehouse
documentationcenter: NA
author: mlee3gsd
manager: jhubbard
editor: 
ms.assetid: d7fb89d1-da1d-4788-a111-68d0e3fda799
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: martinle;barbkess
ms.openlocfilehash: 0425cf5abe7bc001b2a41df4d09bf5f2e42527e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-data-with-power-bi"></a>Vizualizace dat pomocí Power BI
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Tento kurz ukazuje, jak toouse Power BI tooconnect tooSQL datového skladu a vytvořit pár základních vizualizací.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Data-Warehouse-Sample-Data-and-PowerBI/player]
> 
> 

## <a name="prerequisites"></a>Požadavky
toostep prostřednictvím tohoto kurzu potřebujete:

* SQL Data Warehouse předem načtená hello databáze AdventureWorksDW. tooprovision, najdete v tématu [vytvořit SQL Data Warehouse] [ Create a SQL Data Warehouse] a zvolte tooload hello ukázková data. Pokud už Data Warehouse máte, ale nemáte ukázková data, můžete [ukázková data načíst ručně][load sample data manually].

## <a name="1-connect-tooyour-database"></a>1. Připojit databáze tooyour
tooopen Power BI a připojte databázi AdventureWorksDW tooyour:

1. Přihlaste se k hello [portál Azure][Azure portal].
2. Klikněte na **Databáze SQL** a zvolte databázi AdventureWorks služby SQL Data Warehouse.
   
    ![Vyhledání databáze][1]
3. Klikněte na tlačítko Otevřít v Power BI hello.
   
    ![Tlačítko Power BI][2]
4. Teď byste měli vidět hello SQL Data Warehouse připojení stránky zobrazení webovou adresu vaší databáze. Klikněte na tlačítko Další.
   
    ![Připojení Power BI][3]
5. Zadejte uživatelské jméno pro server Azure SQL a heslo a budete plně připojené tooyour databázi SQL Data Warehouse.
   
    ![Přihlášení k Power BI][4]
6. Až se zaregistrujete v Power BI, klikněte na datovou sadu AdventureWorksDW hello v levém okně hello. Tím se otevře databáze hello.
   
    ![Otevření databáze AdventureWorksDW v Power BI][5]

## <a name="2-create-a-report"></a>2. Vytvoření sestavy
Můžete je nyní připraven toouse Power BI tooanalyze ukázkových dat AdventureWorksDW. tooperform hello analýzy má databáze AdventureWorksDW zobrazení s názvem AggregateSales. Toto zobrazení obsahuje několik hello klíčových metrik pro analýzu prodeje společnosti hello hello.

1. toocreate mapu částek prodeje podle kódu toopostal, v podokně polí hello, klikněte na tlačítko hello tooexpand zobrazení AggregateSales ho. Klikněte na tlačítko tooselect sloupce PostalCode a SalesAmount hello je.
   
    ![Výběr sloupce AggregateSales v Power BI][6]
   
    Power BI automaticky rozpozná, že se jedná o zeměpisné údaje, a umístí vám je na mapu.
   
    ![Mapa Power BI][7]
2. Tento krok vytvoří pruhový graf, který zobrazí částku prodeje na příjem zákazníka. toocreate tento přejděte toohello rozšířené zobrazení AggregateSales. Klikněte na pole SalesAmount hello. Přetáhněte hello příjem zákazníka pole toohello doleva a umístěte jej do osy.
   
    ![Výběr osy v Power BI][8]
   
    Jsme hello pruhový graf nepřesouvají hello doleva.
   
    ![Pruhový graf v Power BI][9]
3. Tento krok vytvoří spojnicový graf zobrazující prodejní částku k datu objednávky. toocreate tento přejděte toohello rozšířené zobrazení AggregateSales. Klikněte na SalesAmount a OrderDate. Ve sloupci vizualizace hello klikněte na ikonu spojnicového grafu hello; Toto je první ikona hello hello druhém řádku pod vizualizacemi.
   
    ![Výběr spojnicového grafu v Power BI][10]
   
    Teď máte sestavu zobrazující tři různé vizualizace dat hello.
   
    ![Spojnicový graf v Power BI][11]

Kdykoli můžete rozdělanou práci uložit tak, že kliknete na **Soubor** a vyberete **Uložit**.

## <a name="next-steps"></a>Další kroky
Teď, když vyzkoušeli jste některé čas toowarm hello ukázková data, najdete v části Jak příliš[vyvíjet][develop], [načíst][load], nebo [ migrace][migrate]. Nebo se podívejte na hello [web Power BI][Power BI website].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png
[2]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png
[3]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png
[4]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png
[5]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png
[6]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png
[7]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png
[8]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png
[9]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png
[10]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png
[11]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png
[12]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-save.png

<!--Article references-->
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[load sample data manually]: sql-data-warehouse-load-sample-databases.md
[connecting tooSQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[Azure portal]: https://portal.azure.com/
[Power BI website]: http://www.powerbi.com/
