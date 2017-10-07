---
title: "aaaUse Redgate tooload data tooyour Azure datového skladu | Microsoft Docs"
description: "Zjistěte, jak Studio platformy toouse Redgate dat pro scénáře datových skladů."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: 670aef98-31f7-4436-86c0-cc989a39fe7f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 6082390c07c8ffa73ebd8ab272ace00ba8bb1897
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-with-redgate-data-platform-studio"></a>Načtení dat pomocí nástroje Redgate Data Platform Studio
> [!div class="op_single_selector"]
> * [Redgate](sql-data-warehouse-load-with-redgate.md)
> * [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)
> * [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)
> * [BCP](sql-data-warehouse-load-with-bcp.md)
> 
> 

Tento kurz ukazuje, jak toouse [Redgate na Data platformy Studio](http://www.red-gate.com/products/azure-development/data-platform-studio/) (distribučních bodů) toomove data ze tooAzure místní systém SQL Server SQL Data Warehouse. Data platformy Studio platí hello nejvhodnější kompatibility opravy a optimalizace, takže má hello nejrychleji tooget způsob, jak začít s SQL Data Warehouse.

> [!NOTE]
> [Redgate](http://www.red-gate.com) je dlouhodobý partner Microsoftu, který poskytuje různé nástroje pro SQL Server. Tato funkce byla v nástroji Data Platform Studio zpřístupněna bezplatně pro komerční i nekomerční použití.
> 
> 

## <a name="before-you-begin"></a>Než začnete
### <a name="create-or-identify-resources"></a>Vytvoření nebo určení prostředků
Před zahájením tohoto kurzu potřebujete toohave:

* **místní databáze SQL serveru**: hello data, která chcete tooimport tooSQL datového skladu musí toocome z místního serveru SQL (verze 2008 R2 nebo novější). Data Platform Studio nemůže importovat data přímo z Azure SQL Database nebo z textových souborů.
* **Účet služby Azure Storage**: Studio platformy Data zpracuje hello data v Azure Blob Storage před načtením do SQL Data Warehouse. účet úložiště Hello musí používat model nasazení hello "Resource Manager" (výchozí hello) namísto hello "Classic" modelu nasazení. Pokud nemáte účet úložiště, zjistěte, jak tooCreate účet úložiště. 
* **SQL Data Warehouse**: v tomto kurzu přesune hello data z místního serveru SQL Server tooSQL datového skladu, proto musíte toohave online datový sklad. Pokud již datový sklad nemáte, zjistěte, jak tooCreate Azure SQL Data Warehouse.

> [!NOTE]
> Výkon je vyšší, pokud účet úložiště hello hello datového skladu jsou vytvořeny a hello stejné oblasti.
> 
> 

## <a name="step-1-sign-in-toodata-platform-studio-with-your-azure-account"></a>Krok 1: Zaregistrujte v tooData Studio platformy s vaším účtem Azure
Otevřete webový prohlížeč a přejděte toohello [Data platformy Studio](https://www.dataplatformstudio.com/) webu. Přihlášení s hello stejný účet Azure, že můžete použít toocreate hello úložiště účet a datový sklad. Pokud vaše e-mailová adresa je přidružen pracovní nebo školní účet a účet Microsoft, být jisti toochoose hello účet, který má přístup k prostředkům tooyour.

> [!NOTE]
> Pokud používáte Data platformy Studio poprvé, budete vyzváni toomanage oprávnění aplikace hello toogrant vašich prostředků Azure.
> 
> 

## <a name="step-2-start-hello-import-wizard"></a>Krok 2: Spuštění Průvodce importem hello
Z hlavní obrazovky hello distribučních bodů vyberte hello Import tooAzure SQL Data Warehouse odkaz toostart hello Průvodce importem.

![][1]

## <a name="step-3-install-hello-data-platform-studio-gateway"></a>Krok 3: Instalace hello brána Studio platformy dat
tooconnect tooyour databáze SQL serveru místní, musíte tooinstall hello brány distribučních bodů. Brána Hello je klientský agent, který poskytuje přístup tooyour místní prostředí, extrahuje hello data a odešle ho tooyour účet úložiště. Vaše data nikdy neprocházejí přes servery společnosti Redgate. tooinstall hello brány:

1. Klikněte na tlačítko hello **vytvořit bránu** odkaz
2. Stažení a instalace hello brány pomocí hello zadaný instalační program

![][2]

> [!NOTE]
> Hello brány lze nainstalovat z jakéhokoli počítače s databází SQL Server zdrojové toohello přístup k síti. Přistupuje k databázi systému SQL Server hello pomocí ověřování systému Windows hello přihlašovací údaje aktuálního uživatele hello.
> 
> 

Po instalaci hello tooConnected změny stavu brány a můžete vybrat další.

## <a name="step-4-identify-hello-source-database"></a>Krok 4: Určení hello zdrojové databáze
V hello *zadejte název serveru* textovému poli, zadejte název hello hello serveru, který je hostitelem databáze a vyberte **Další**. Potom vyberte z rozevírací nabídky hello, hello požadovaná tooimport data z databáze.

![][3]

Distribučních bodů zkontroluje hello vybrané databáze pro tooimport tabulky. Ve výchozím nastavení distribučních bodů importuje všechny hello tabulky v databázi hello. Můžete vybrat, nebo zrušte výběr tabulek rozšířením hello propojení všech tabulek. Vyberte hello další tlačítko toomove dál.

## <a name="step-5-choose-a-storage-account-toostage-hello-data"></a>Krok 5: Zvolte dat hello toostage účtu úložiště
Distribučních bodů vyzve k zadání umístění toostage hello data. Vyberte existující účet úložiště z vašeho předplatného a pak zvolte **Další**.

> [!NOTE]
> Vytvoříte nový kontejner objektů blob v hello vybrali účet úložiště a používat odlišné složku pro každý import distribučních bodů.
> 
> 

![][4]

## <a name="step-6-select-a-data-warehouse"></a>Krok 6: Výběr datového skladu
Potom vyberte online [Azure SQL Data Warehouse](http://aka.ms/sqldw) tooimport hello data do databáze. Po dokončení výběru vaší databáze, je nutné tooenter hello pověření tooconnect toohello databáze a vyberte **Další**.

![][5]

> [!NOTE]
> Distribučních bodů sloučí hello zdroje dat tabulky hello datového skladu. Pokud název tabulky hello vyžaduje toooverwrite existující tabulky v datovém skladu hello varuje distribučních bodů. Vám může volitelně můžete odstranit všechny existující objekty v datovém skladu hello zaškrtnutím odstranit všechny existující objekty před importem.
> 
> 

## <a name="step-7-import-hello-data"></a>Krok 7: Import dat hello
Distribučních bodů, potvrdí, které chcete tooimport hello data. Jednoduše klikněte na tlačítko import dat hello tlačítko hello zahájení importu toobegin.

![][6]

Distribučních bodů zobrazí vizualizace, který zobrazuje průběh hello extrahování a nahrání dat hello z místní hello systému SQL Server a hello průběh hello import do SQL Data Warehouse.

![][7]

Po dokončení importu hello distribučních bodů zobrazuje souhrn importu dat hello a změnit sestavu hello kompatibility opravy, které byly provedeny.

![][8]

## <a name="next-steps"></a>Další kroky
tooexplore svá data do SQL Data Warehouse, spusťte zobrazení:

* [Dotazování Azure SQL Data Warehouse (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)]
* [Vizualizace dat pomocí Power BI][Visualize data with Power BI]

Další informace o společnosti Redgate Data platformy Studio toolearn:

* [Navštěvovat domovskou stránku hello distribučních bodů](http://www.dataplatformstudio.com/)
* [Ukázka DPS na Channel 9](https://channel9.msdn.com/Blogs/cloud-with-a-silver-lining/Loading-data-into-Azure-SQL-Datawarehouse-with-Redgate-Data-Platform-Studio)

Vaše data v SQL Data Warehouse najdete přehled jiné způsoby toomigrate a zatížení:

* [Migrace vašeho řešení tooSQL datového skladu][Migrate your solution tooSQL Data Warehouse]
* [Načtení dat do Azure SQL Data Warehouse](sql-data-warehouse-overview-load.md)

Další tipy pro vývoj, najdete v části hello [přehled vývoje SQL Data Warehouse](sql-data-warehouse-overview-develop.md).

<!--Image references-->
[1]: media/sql-data-warehouse-redgate/2016-10-05_15-59-56.png
[2]: media/sql-data-warehouse-redgate/2016-10-05_11-16-07.png
[3]: media/sql-data-warehouse-redgate/2016-10-05_11-17-46.png
[4]: media/sql-data-warehouse-redgate/2016-10-05_11-20-41.png
[5]: media/sql-data-warehouse-redgate/2016-10-05_11-31-24.png
[6]: media/sql-data-warehouse-redgate/2016-10-05_11-32-20.png
[7]: media/sql-data-warehouse-redgate/2016-10-05_11-49-53.png
[8]: media/sql-data-warehouse-redgate/2016-10-05_12-57-10.png

<!--Article references-->
[Query Azure SQL Data Warehouse (Visual Studio)]: ./sql-data-warehouse-query-visual-studio.md
[Visualize data with Power BI]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md
[Migrate your solution tooSQL Data Warehouse]: ./sql-data-warehouse-overview-migrate.md
[Load data into Azure SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
