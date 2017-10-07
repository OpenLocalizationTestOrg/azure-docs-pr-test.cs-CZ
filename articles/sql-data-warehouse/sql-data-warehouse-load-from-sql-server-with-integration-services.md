---
title: aaaLoad dat z SQL serveru do Azure SQL Data Warehouse (SSIS) | Microsoft Docs
description: "Ukazuje, jak toocreate toomove data balíčku integrační služby SSIS (SQL Server) ze široké škály data zdroje tooSQL datového skladu."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: e2c252e9-0828-47c2-a808-e3bea46c134a
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 03/30/2017
ms.author: cakarst;douglasl;barbkess
ms.openlocfilehash: bb28a08807a5b07832b85f2f074c2acf912c1dc3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-ssis"></a>Načtení dat z SQL serveru do Azure SQL Data Warehouse služby SSIS)
> [!div class="op_single_selector"]
> * [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
> * [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
> * [bcp](sql-data-warehouse-load-from-sql-server-with-bcp.md)
> 
> 

Vytvořte data tooload balíčku integrační služby SSIS (SQL Server) ze serveru SQL Server do Azure SQL Data Warehouse. Volitelně můžete změny struktury, transformace a čistí hello data při průchodu hello SSIS datového toku.

V tomto kurzu provedete následující:

* V sadě Visual Studio vytvořte nový projekt integrační služby.
* Připojte toodata zdrojů, včetně systému SQL Server (jako zdroj) a SQL Data Warehouse (jako cíl).
* Návrh služby SSIS balíček, který načte data ze zdroje hello do cílové hello.
* Spuštění hello data hello tooload balíčku služby SSIS.

Tento kurz používá SQL Server jako zdroj dat hello. SQL Server může být spuštěn místně nebo v virtuální počítač Azure.

## <a name="basic-concepts"></a>Základní koncepty
balíček Hello je hello jednotky práce v SSIS. Související balíčky jsou seskupené v projektech. Vytváření projektů a balíčků návrhu v sadě Visual Studio s SQL Server Data Tools. Hello návrhu, že je visual proces, ve kterém můžete přetáhnout myší součásti z hello sady nástrojů toohello návrhovou plochu, je připojení a nastavení jejich vlastnosti. Po dokončení vašeho balíčku, můžete volitelně nasadit tooSQL serveru pro komplexní správu, monitorování a zabezpečení.

## <a name="options-for-loading-data-with-ssis"></a>Možnosti pro načítání dat pomocí služby SSIS
Integrace služby SSIS (SQL Server) je flexibilní sadu nástrojů, který poskytuje celou řadu možností pro připojení k a načítání dat do SQL Data Warehouse.

1. Použijte určení NET ADO tooconnect tooSQL datového skladu. Tento kurz používá určení NET ADO, protože má nejmenší počet možností konfigurace hello.
2. Použijte určení technologie OLE DB tooconnect tooSQL datového skladu. Tato možnost může poskytovat mírně lepší výkon než hello ADO NET cílový.
3. Použití hello úloh nahrát objekt Blob Azure toostage hello dat v Azure Blob Storage. Potom pomocí hello SSIS provést SQL úloh toolaunch Polybase skript, který načte hello data do SQL Data Warehouse. Tato možnost poskytuje nejlepší výkon hello hello tři možnosti uvedené v tomto poli. tooget hello nahrát objekt Blob Azure úloh, stáhnout hello [Microsoft SQL Server 2016 integrační služby Feature Pack pro Azure][Microsoft SQL Server 2016 Integration Services Feature Pack for Azure]. toolearn Další informace o používání funkce Polybase, najdete v části [Průvodce funkcí PolyBase][PolyBase Guide].

## <a name="before-you-start"></a>Než začnete
toostep prostřednictvím tohoto kurzu potřebujete:

1. **SQL Server Integration Services (služby SSIS)**. Služby SSIS je součástí systému SQL Server a vyžaduje zkušební verze nebo verze na licencovanou verzi systému SQL Server. tooget zkušební verze systému SQL Server 2016 ve verzi Preview, najdete v části [SQL serveru hodnocení][SQL Server Evaluations].
2. Sadu **Visual Studio**. tooget hello bezplatná edice Community Visual Studio najdete v tématu [Visual Studio Community][Visual Studio Community].
3. **Nástroje SQL Server Data Tools pro sadu Visual Studio (SSDT)**. tooget SQL Server Data Tools pro sadu Visual Studio, najdete v části [stáhnout SQL Server Data Tools (SSDT)][Download SQL Server Data Tools (SSDT)].
4. **Ukázková data**. Tento kurz používá ukázková data uložena v systému SQL Server v ukázkovou databázi AdventureWorks hello, protože hello zdroje dat toobe načíst do SQL Data Warehouse. tooget hello ukázkovou databázi AdventureWorks, najdete v části [AdventureWorks 2014 ukázkové databáze][AdventureWorks 2014 Sample Databases].
5. **Databáze SQL Data Warehouse a oprávnění**. V tomto kurzu připojí tooa instanci SQL Data Warehouse a načte data do ní. Máte toohave oprávnění toocreate tabulky a tooload data.
6. **Pravidlo brány firewall**. Máte toocreate pravidlo brány firewall v SQL Data Warehouse s hello IP adresu místního počítače předtím, než můžete nahrát data toohello SQL Data Warehouse.

## <a name="step-1-create-a-new-integration-services-project"></a>Krok 1: Vytvoření nového projektu integrační služby
1. Spusťte sadu Visual Studio.
2. Na hello **soubor** nabídce vyberte možnost **nový | Projekt**.
3. Přejděte toohello **nainstalovaná | Šablony | Business Intelligence | Integrační služby** typy projektů.
4. Vyberte **integračních služeb projektu**. Zadejte hodnoty pro **název** a **umístění**a potom vyberte **OK**.

Visual Studio otevře a vytvoří nový projekt Integration Services (SSIS). Pak Visual Studio otevře hello designer pro hello jednoho nového SSIS balíčku (Package.dtsx) v projektu hello. Zobrazí následující obrazovka oblasti hello:

* Na levé straně hello hello **sada nástrojů** součásti služby SSIS.
* Uprostřed hello hello návrhovou plochu, s několik karet. Obvykle se používá minimálně hello **tok řízení** a hello **toku dat** karty.
* Na pravém hello, hello **Průzkumníku řešení** a hello **vlastnosti** podokna.
  
    ![][01]

## <a name="step-2-create-hello-basic-data-flow"></a>Krok 2: Vytvoření základní datový tok hello
1. Přetáhněte úlohu toku dat z centra toohello sady nástrojů hello hello návrhové plochy aplikace (na hello **tok řízení** karta).
   
    ![][02]
2. Dvakrát klikněte na hello úlohy toku dat tooswitch toohello toku dat kartě.
3. Z jiných zdrojů seznamu hello v hello sada nástrojů přetáhněte návrhovou plochu toohello zdroje ADO.NET. Adaptérem hello zdroj vybraný, změňte její název příliš**zdroje systému SQL Server** v hello **vlastnosti** podokně.
4. Hello jiné cíle seznamu hello sada nástrojů přetáhněte ADO.NET cílové toohello návrhová plocha pod hello zdroje ADO.NET. Cílový adaptér hello stále vybrán, změnit její název příliš**SQL DW cílové** v hello **vlastnosti** podokně.
   
    ![][09]

## <a name="step-3-configure-hello-source-adapter"></a>Krok 3: Konfigurace adaptér zdroje hello
1. Klikněte dvakrát na hello zdroj adaptér tooopen hello **Editor zdroje ADO.NET**.
   
    ![][03]
2. Na hello **Správce připojení** kartě hello **Editor zdroje ADO.NET**, klikněte na tlačítko hello **nový** tlačítko Další toohello **Správce připojení ADO.NET**seznamu tooopen hello **nakonfigurovat správce připojení ADO.NET** dialogové okno a vytvořte nastavení připojení pro databázi systému SQL Server hello, ze kterého v tomto kurzu načte data.
   
    ![][04]
3. V hello **nakonfigurovat správce připojení ADO.NET** dialogové okno pole, klikněte na tlačítko hello **nový** tlačítko tooopen hello **Správce připojení** dialogové okno a vytvořte nové datové připojení.
   
    ![][05]
4. V hello **Správce připojení** dialogové okno pole, hello následující věci.
   
   1. Pro **zprostředkovatele**, vyberte hello zprostředkovatele dat SqlClient.
   2. Pro **název serveru**, zadejte název serveru SQL Server hello.
   3. V hello **protokolu na serveru toohello** vyberte nebo zadejte informace o ověřování.
   4. V hello **Connect tooa databáze** vyberte ukázkovou databázi AdventureWorks hello.
   5. Klikněte na tlačítko **testovací připojení**.
      
       ![][06]
   6. V hello dialogové okno sestav hello výsledky testu hello připojení, klikněte na **OK** tooreturn toohello **Správce připojení** dialogové okno.
   7. V hello **Správce připojení** dialogové okno, klikněte na tlačítko **OK** tooreturn toohello **nakonfigurovat správce připojení ADO.NET** dialogové okno.
5. V hello **nakonfigurovat správce připojení ADO.NET** dialogové okno, klikněte na tlačítko **OK** tooreturn toohello **Editor zdroje ADO.NET**.
6. V hello **Editor zdroje ADO.NET**, v hello **název hello tabulky nebo zobrazení hello** seznamu, vyberte hello **Sales.SalesOrderDetail** tabulky.
   
    ![][07]
7. Klikněte na tlačítko **Preview** toosee hello prvních 200 řádků dat ve zdrojové tabulce hello v hello **náhled výsledků dotazu** dialogové okno.
   
    ![][08]
8. V hello **náhled výsledků dotazu** dialogové okno, klikněte na tlačítko **Zavřít** tooreturn toohello **Editor zdroje ADO.NET**.
9. V hello **Editor zdroje ADO.NET**, klikněte na tlačítko **OK** toofinish konfiguraci zdroje dat hello.

## <a name="step-4-connect-hello-source-adapter-toohello-destination-adapter"></a>Krok 4: Připojení hello zdroj adaptér toohello cílový adaptér
1. Vyberte zdroj adaptér hello hello návrhové ploše.
2. Vyberte hello blue šipku, která rozšiřuje z adaptéru zdroj hello a přetáhněte ji toohello cílové editor, dokud ho přichytí.
   
    ![][10]
   
    V typické SSIS balíčku použít několik dalších součástí ze hello sada nástrojů služby SSIS mezi hello zdrojové a cílové toorestructure hello, transformace a čistí data při průchodu hello SSIS datového toku. tookeep co nejjednodušší například jsme se chcete připojit hello zdroje přímo toohello cílový.

## <a name="step-5-configure-hello-destination-adapter"></a>Krok 5: Konfigurace hello cílový adaptér
1. Klikněte dvakrát na hello cílový adaptér tooopen hello **ADO.NET cílové Editor**.
   
    ![][11]
2. Na hello **Správce připojení** kartě hello **ADO.NET cílové Editor**, klikněte na tlačítko hello **nový** tlačítko Další toohello **Správce připojení**seznamu tooopen hello **nakonfigurovat správce připojení ADO.NET** dialogové okno a vytvořte nastavení připojení pro databázi Azure SQL Data Warehouse hello, do kterého v tomto kurzu načte data.
3. V hello **nakonfigurovat správce připojení ADO.NET** dialogové okno pole, klikněte na tlačítko hello **nový** tlačítko tooopen hello **Správce připojení** dialogové okno a vytvořte nové datové připojení.
4. V hello **Správce připojení** dialogové okno pole, hello následující věci.
   1. Pro **zprostředkovatele**, vyberte hello zprostředkovatele dat SqlClient.
   2. Pro **název serveru**, zadejte název hello SQL Data Warehouse.
   3. V hello **protokolu na serveru toohello** vyberte **ověřování použít SQL Server** a zadejte informace o ověřování.
   4. V hello **Connect tooa databáze** vyberte existující databázi SQL Data Warehouse.
   5. Klikněte na tlačítko **testovací připojení**.
   6. V hello dialogové okno sestav hello výsledky testu hello připojení, klikněte na **OK** tooreturn toohello **Správce připojení** dialogové okno.
   7. V hello **Správce připojení** dialogové okno, klikněte na tlačítko **OK** tooreturn toohello **nakonfigurovat správce připojení ADO.NET** dialogové okno.
5. V hello **nakonfigurovat správce připojení ADO.NET** dialogové okno, klikněte na tlačítko **OK** tooreturn toohello **ADO.NET cílové Editor**.
6. V hello **ADO.NET cílové Editor**, klikněte na tlačítko **nový** další toohello **použití tabulky nebo zobrazení** seznamu tooopen hello **Create Table** dialogové okno toocreate nové cílové tabulky se seznam sloupců, která odpovídá hello zdrojové tabulky.
   
    ![][12a]
7. V hello **Create Table** dialogové okno pole, hello následující věci.
   
   1. Změňte název hello hello cílové tabulky příliš**podrobnosti prodejní objednávky**.
   2. Odebrat hello **rowguid** sloupce. Hello **uniqueidentifier** datový typ není podporován v SQL Data Warehouse.
   3. Změnit datový typ hello hello **LineTotal** sloupec příliš**peníze**. Hello **decimal** datový typ není podporován v SQL Data Warehouse. Informace o podporované datové typy najdete v tématu [CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)][CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)].
      
       ![][12b]
   4. Klikněte na tlačítko **OK** toocreate hello tabulky a vraťte se toohello **ADO.NET cílové Editor**.
8. V hello **ADO.NET cílové Editor**, vyberte hello **mapování** kartě toosee jak sloupce ve zdroji hello jsou namapované toocolumns v cílovém umístění hello.
   
    ![][13]
9. Klikněte na tlačítko **OK** toofinish konfiguraci zdroje dat hello.

## <a name="step-6-run-hello-package-tooload-hello-data"></a>Krok 6: Spuštění data hello tooload balíčku hello
Spuštění hello balíček kliknutím hello **spustit** tlačítka na panelu nástrojů hello nebo výběrem jedné z hello **spustit** možnosti na hello **ladění** nabídky.

Jako balíček hello začne toorun, zobrazí žlutý souborů Wheel roztočený tooindicate aktivity, jakož i hello počet řádků, pokud zpracovat.

![][14]

Po dokončení spuštění balíčku hello je najdete v části úspěch tooindicate zelené značky zaškrtnutí a také hello celkový počet řádků dat načtených z hello zdroj toohello cíl.

![][15]

Blahopřejeme! Úspěšně jste použili službu SQL Server Integration Services tooload data do Azure SQL Data Warehouse.

## <a name="next-steps"></a>Další kroky
* Další informace o hello SSIS datového toku. Začněte zde: [toku dat][Data Flow].
* Zjistěte, jak toodebug a řešení potíží s vaše právo balíčky v prostředí návrhu hello. Začněte zde: [řešení potíží s nástrojů pro vývoj balíček][Troubleshooting Tools for Package Development].
* Zjistěte, jak toodeploy vaší služby SSIS projekty a balíčky tooIntegration služby serveru nebo tooanother umístění úložiště. Začněte zde: [projekty nasazení a balíčky][Deployment of Projects and Packages].

<!-- Image references -->
[01]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-designer-01.png
[02]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-data-flow-task-02.png
[03]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-03.png
[04]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-manager-04.png
[05]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-05.png
[06]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/test-connection-06.png
[07]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-07.png
[08]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/preview-data-08.png
[09]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/source-destination-09.png
[10]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/connect-source-destination-10.png
[11]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-destination-11.png
[12a]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-before-12a.png
[12b]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-after-12b.png
[13]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/column-mapping-13.png
[14]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-running-14.png
[15]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-success-15.png

<!-- Article references -->

<!-- MSDN references -->
[PolyBase Guide]: https://msdn.microsoft.com/library/mt143171.aspx
[Download SQL Server Data Tools (SSDT)]: https://msdn.microsoft.com/library/mt204009.aspx
[CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)]: https://msdn.microsoft.com/library/mt203953.aspx
[Data Flow]: https://msdn.microsoft.com/library/ms140080.aspx
[Troubleshooting Tools for Package Development]: https://msdn.microsoft.com/library/ms137625.aspx
[Deployment of Projects and Packages]: https://msdn.microsoft.com/library/hh213290.aspx

<!--Other Web references-->
[Microsoft SQL Server 2016 Integration Services Feature Pack for Azure]: http://go.microsoft.com/fwlink/?LinkID=626967
[SQL Server Evaluations]: https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2016
[Visual Studio Community]: https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx
[AdventureWorks 2014 Sample Databases]: https://msftdbprodsamples.codeplex.com/releases/view/125550
