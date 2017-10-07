---
title: "aaaLoad terabajtů dat do SQL Data Warehouse | Microsoft Docs"
description: "Ukazuje, jak 1 TB dat je možné načíst do Azure SQL Data Warehouse pomocí Azure Data Factory v části 15 minut"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: a6c133c0-ced2-463c-86f0-a07b00c9e37f
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jingwang
ms.openlocfilehash: e63f60461b082b0e3979004cb631dbf4a6710de3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-1-tb-into-azure-sql-data-warehouse-under-15-minutes-with-data-factory"></a>Načtení 1 TB do Azure SQL Data Warehouse pomocí služby Data Factory v části 15 minut
[Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) je databáze založená na cloudu, škálování dokáže zpracovávat ohromné objemy dat, relačních i nerelačních.  Postavená na architektuře massively parallel processing (MPP), SQL Data Warehouse je optimalizovaná pro podnikové datového skladu úlohy.  Nabízí cloud pružnost s hello flexibilitu tooscale úložiště a výpočetní nezávisle.

Začínáme s Azure SQL Data Warehouse je teď jednodušší než kdy použití **Azure Data Factory**.  Azure Data Factory je plně spravovaná Cloudová datová služba integrace, který lze použít toopopulate SQL Data Warehouse s hello data z vašeho stávajícího systému a ukládání je hodně času při vyhodnocení SQL Data Warehouse a sestavování analytické údaje řešení. Tady jsou klíčové výhody hello načítání dat do Azure SQL Data Warehouse pomocí Azure Data Factory:

* **Snadno tooset až**: krok 5 intuitivní průvodce bez potřeby skriptování.
* **Podpora úložiště bohaté dat**: integrovanou podporu pro bohatou sadu místní a Cloudová datová úložiště.
* **Zabezpečení a dodržování**: data se přenáší prostřednictvím protokolu HTTPS nebo ExpressRoute a přítomnost globální služby zajišťuje vaše data nikdy neopustí hranice zeměpisné hello
* **Bezkonkurenční výkonu pomocí funkce PolyBase** – je hello nejúčinnější způsob, jak toomove data do Azure SQL Data Warehouse pomocí Polybase. Pomocí hello pracovní funkce objektů blob, můžete dosáhnout vysokého zatížení rychlosti ze všech typů úložišť dat kromě úložiště objektů Azure Blob, které hello Polybase podporuje ve výchozím nastavení.

Tento článek ukazuje, jak toouse Průvodce kopírováním služby Data Factory data tooload 1 TB z Azure Blob Storage do Azure SQL Data Warehouse v části 15 minut, při propustnost přes 1,2 GB/s.

Tento článek obsahuje podrobné pokyny pro přesun dat do Azure SQL Data Warehouse pomocí Průvodce kopírováním hello.

> [!NOTE]
>  Obecné informace o možnostech objektu pro vytváření dat při přesunu dat do/z Azure SQL Data Warehouse najdete v tématu [přesunout tooand dat z Azure SQL Data Warehouse pomocí Azure Data Factory](data-factory-azure-sql-data-warehouse-connector.md) článku.
>
> Můžete také vytvořit pomocí portálu Azure, Visual Studio, prostředí PowerShell, kanály atd. V tématu [kurz: kopírování dat z tooAzure objektů Blob v Azure SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) rychlé návod s podrobnými pokyny pro používání hello aktivitu kopírování v Azure Data Factory.  
>
>

## <a name="prerequisites"></a>Požadavky
* Azure Blob Storage: Tento experiment používá Azure Blob úložiště (GRS) pro ukládání TPC-H testování datovou sadu.  Pokud nemáte účet úložiště Azure, přečtěte si [jak toocreate účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account).
* [TPC-H](http://www.tpc.org/tpch/) data: přidáme toouse TPC-H jako hello testování datovou sadu.  toodo, je nutné, aby toouse `dbgen` z TPC-H nástrojů, který umožňuje generovat datovou sadu hello.  Můžete buď stáhnout zdrojového kódu pro `dbgen` z [TPC nástroje](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) a jeho kompilace sami nebo stažení hello kompilovány binární z [Githubu](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools).  Spuštění dbgen.exe s hello následující příkazy toogenerate 1 TB plochý soubor pro `lineitem` tabulky šíření mezi 10 souborů:

  * `Dbgen -s 1000 -S **1** -C 10 -T L -v`
  * `Dbgen -s 1000 -S **2** -C 10 -T L -v`
  * …
  * `Dbgen -s 1000 -S **10** -C 10 -T L -v`

    Nyní kopie hello generování souborů tooAzure objektů Blob.  Odkazovat příliš[přesunutí data tooand ze v místním systému souborů pomocí Azure Data Factory](data-factory-onprem-file-system-connector.md) jak toodo, pomocí kopírování ADF.    
* Azure SQL Data Warehouse: tohoto experimentu načte data do Azure SQL Data Warehouse vytvořené pomocí 6000 Dwu

    Odkazovat příliš[vytvoření Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) podrobné pokyny, jak server toocreate SQL datového skladu databáze.  tooget hello nejlepší možný zatížení výkonu do SQL Data Warehouse pomocí Polybase, jsme vyberte maximální počet povolen v nastavení hello výkonu, které je 6000 Dwu jednotky datového skladu (Dwu).

  > [!NOTE]
  > Při načítání z Azure Blob, načítání výkonu hello dat je přímo úměrná toohello počet Dwu nakonfigurujete na hello SQL Data Warehouse:
  >
  > Načtení 1 TB do 1000 DWU SQL Data Warehouse přebírá načítání 1 TB 87 minut (propustnost ~ 200 MB/s) do 2 000 DWU SQL Data Warehouse trvá 46 minut (propustnost ~ 380 MB/s) načítání 1 TB do 6000 DWU SQL Data Warehouse přebírá 14 minut (propustnost ~1.2 GB/s)
  >
  >

    toocreate SQL Data Warehouse s 6000 Dwu, posuvník hello výkonu všechny toohello způsob hello práva:

    ![Výkon posuvníku](media/data-factory-load-sql-data-warehouse/performance-slider.png)

    Pro existující databázi, který není konfigurován s 6000 Dwu můžete postupně škálovat ji pomocí portálu Azure.  Přejděte toohello databáze na portálu Azure a je **škálování** tlačítka na hello **přehled** panelu ukazuje následující obrázek hello:

    ![Stupnice – tlačítko](media/data-factory-load-sql-data-warehouse/scale-button.png)    

    Klikněte na tlačítko hello **škálování** panelu, přesunout hello posuvníku toohello maximální hodnotu a klikněte na tlačítko tooopen hello následující **Uložit** tlačítko.

    ![Dialogové okno škálování](media/data-factory-load-sql-data-warehouse/scale-dialog.png)

    Tento experiment načte data do Azure SQL Data Warehouse pomocí `xlargerc` Třída prostředků.

    nejlepší možné propustnost tooachieve kopie musí toobe provádí pomocí SQL Data Warehouse uživatel patřící příliš`xlargerc` Třída prostředků.  Zjistěte, jak toodo, pomocí následujících [změnit v příkladu třída prostředků uživatele](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).  
* Vytvoření schématu cílové tabulky v databázi Azure SQL Data Warehouse, tak, že spustíte následující příkaz DDL hello:

    ```SQL  
    CREATE TABLE [dbo].[lineitem]
    (
        [L_ORDERKEY] [bigint] NOT NULL,
        [L_PARTKEY] [bigint] NOT NULL,
        [L_SUPPKEY] [bigint] NOT NULL,
        [L_LINENUMBER] [int] NOT NULL,
        [L_QUANTITY] [decimal](15, 2) NULL,
        [L_EXTENDEDPRICE] [decimal](15, 2) NULL,
        [L_DISCOUNT] [decimal](15, 2) NULL,
        [L_TAX] [decimal](15, 2) NULL,
        [L_RETURNFLAG] [char](1) NULL,
        [L_LINESTATUS] [char](1) NULL,
        [L_SHIPDATE] [date] NULL,
        [L_COMMITDATE] [date] NULL,
        [L_RECEIPTDATE] [date] NULL,
        [L_SHIPINSTRUCT] [char](25) NULL,
        [L_SHIPMODE] [char](10) NULL,
        [L_COMMENT] [varchar](44) NULL
    )
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    ```
Dokončení požadovaných krocích hello jsme jsou nyní aktivitou kopírování hello připravené tooconfigure pomocí Průvodce kopírováním hello.

## <a name="launch-copy-wizard"></a>Spuštění průvodce kopírováním
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. Klikněte na tlačítko **+ nový** z levého horního rohu hello, klikněte na tlačítko **Intelligence + analýzy**a klikněte na tlačítko **Data Factory**.
3. V hello **nový objekt pro vytváření dat** okno:

   1. Zadejte **LoadIntoSQLDWDataFactory** pro hello **název**.
       Hello název objektu pro vytváření dat Azure hello musí být globálně jedinečný. Pokud se zobrazí chyba hello: **název objektu pro vytváření dat "LoadIntoSQLDWDataFactory" není k dispozici**, změňte hello název objektu pro vytváření dat hello (například yournameLoadIntoSQLDWDataFactory) a zkuste to znovu. V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.  
   2. Vyberte své **předplatné** Azure.
   3. Pro skupinu prostředků proveďte jednu z následujících kroků hello:
      1. Vyberte **použít existující** tooselect existující skupinu prostředků.
      2. Vyberte **vytvořit nový** tooenter název pro skupinu prostředků.
   4. Vyberte **umístění** hello data Factory.
   5. Vyberte **Pin toodashboard** políčko v hello dolní části okna hello.  
   6. Klikněte na možnost **Vytvořit**.
4. Po dokončení vytvoření hello uvidíte hello **Data Factory** jak je uvedeno v hello následující bitové kopie:

   ![Domovská stránka objektu pro vytváření dat](media/data-factory-load-sql-data-warehouse/data-factory-home-page-copy-data.png)
5. Na domovské stránce objektu pro vytváření dat hello, klikněte na tlačítko hello **kopírování dat** dlaždici toolaunch **Průvodce kopírováním**.

   > [!NOTE]
   > Pokud se zobrazí, že hello webový prohlížeč zasekl ve fázi "autorizace …", zakažte/zrušte zaškrtnutí políčka **blokovat soubory cookie třetích stran a data lokality** nastavení (nebo) zachovat povolené a vytvořte výjimku pro **login.microsoftonline.com**a poté se pokuste spustit hello průvodce znovu.
   >
   >

## <a name="step-1-configure-data-loading-schedule"></a>Krok 1: Konfigurace plánu pro načítání dat
prvním krokem Hello je tooconfigure hello data načítání plánu.  

V hello **vlastnosti** stránky:

1. Zadejte **CopyFromBlobToAzureSqlDataWarehouse** pro **název úlohy**
2. Vyberte **spustit jednou nyní** možnost.   
3. Klikněte na **Další**.  

    ![Průvodce kopírováním – stránky vlastností](media/data-factory-load-sql-data-warehouse/copy-wizard-properties-page.png)

## <a name="step-2-configure-source"></a>Krok 2: Konfigurace zdroje
Tato část obsahuje hello kroky tooconfigure hello zdroj: Azure Blob obsahující hello 1 TB TPC-H položky na řádku soubory.

1. Vyberte hello **Azure Blob Storage** jako hello data ukládat a klikněte na tlačítko **Další**.

    ![Průvodce kopírováním - vyberte zdroj stránky](media/data-factory-load-sql-data-warehouse/select-source-connection.png)

2. Vyplňte hello informace o připojení pro hello účtu úložiště Azure Blob a klikněte na tlačítko **Další**.

    ![Průvodce kopírováním - informace o připojení zdroje](media/data-factory-load-sql-data-warehouse/source-connection-info.png)

3. Zvolte hello **složky** obsahující hello TPC-H řádku položky souborů a klikněte na tlačítko **Další**.

    ![Průvodce kopírováním – vyberte vstupní složky](media/data-factory-load-sql-data-warehouse/select-input-folder.png)

4. Po kliknutí na tlačítko **Další**, nastavení formátu souboru hello se rozpoznávají automaticky.  Zkontrolujte, zda je tento sloupec oddělovač toomake ' | 'místo hello čárkami výchozí','.  Klikněte na tlačítko **Další** po mít zobrazení náhledu dat hello.

    ![Průvodce kopírováním – nastavení formátu souboru](media/data-factory-load-sql-data-warehouse/file-format-settings.png)

## <a name="step-3-configure-destination"></a>Krok 3: Konfigurace cílového
V této části se dozvíte, jak tooconfigure hello cílové: `lineitem` tabulky v databázi Azure SQL Data Warehouse hello.

1. Zvolte **Azure SQL Data Warehouse** jako hello cíl uložení a klikněte na tlačítko **Další**.

    ![Průvodce kopírováním - vyberte cílového úložiště dat](media/data-factory-load-sql-data-warehouse/select-destination-data-store.png)

2. Zadejte informace o připojení hello pro Azure SQL Data Warehouse.  Je nutné zadat hello uživatele, který je členem hello role `xlargerc` (viz hello **požadavky** části Podrobné pokyny) a klikněte na tlačítko **Další**.

    ![Průvodce kopírováním - informace o cílovém připojení](media/data-factory-load-sql-data-warehouse/destination-connection-info.png)

3. Zvolte hello cílové tabulky a klikněte na **Další**.

    ![Zkopírujte - tabulky mapování stránku průvodce](media/data-factory-load-sql-data-warehouse/table-mapping-page.png)

4. Schéma mapování stránce, nechte nezaškrtnuté možnost "Použijí mapování sloupce" a klikněte na tlačítko **Další**.

## <a name="step-4-performance-settings"></a>Krok 4: Nastavení výkonu

**Povolit polybase** je ve výchozím nastavení zaškrtnuto.  Klikněte na **Další**.

![Zkopírujte – schéma mapování stránku průvodce](media/data-factory-load-sql-data-warehouse/performance-settings-page.png)

## <a name="step-5-deploy-and-monitor-load-results"></a>Krok 5: Nasazení a monitorování zatížení výsledky
1. Klikněte na tlačítko **Dokončit** toodeploy tlačítko.

    ![Průvodce kopírováním - souhrnná stránka](media/data-factory-load-sql-data-warehouse/summary-page.png)

2. Po dokončení nasazení hello klikněte na tlačítko `Click here toomonitor copy pipeline` kopie hello toomonitor spustit průběh. Vyberte hello kanál kopírování jste vytvořili v hello **aktivity Windows** seznamu.

    ![Průvodce kopírováním - souhrnná stránka](media/data-factory-load-sql-data-warehouse/select-pipeline-monitor-manage-app.png)

    Můžete zobrazit podrobnosti o spuštění v hello kopie hello **aktivity okno Průzkumníka** v pravém panelu hello, včetně hello datový svazek číst ze zdroje a zapsána do cílového, doba trvání a hello průměrnou propustností pro hello spustit.

    Jak vidíte z hello následující snímek obrazovky, kopírování 1 TB z Azure Blob Storage do SQL Data Warehouse trvalo 14 minut, efektivně dosahování 1.22 GB/s propustnosti!

    ![Průvodce kopírováním – dialogové okno úspěšně vytvořila.](media/data-factory-load-sql-data-warehouse/succeeded-info.png)

## <a name="best-practices"></a>Osvědčené postupy
Tady je několik osvědčených postupů pro spuštění databáze Azure SQL Data Warehouse:

* Při načítání do CLUSTEROVANÝ INDEX COLUMNSTORE, použijte větší Třída prostředků.
* Pro efektivnější spojení zvažte použití algoritmu hash distribuce podle vyberte sloupce místo výchozí round robin distribuční.
* Vyšší rychlost zatížení zvažte použití haldy pro přechodný data.
* Vytvoření statistiky po dokončení načítání Azure SQL Data Warehouse.

V tématu [osvědčené postupy pro Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md) podrobnosti.

## <a name="next-steps"></a>Další kroky
* [Průvodce kopírováním služby Data Factory](data-factory-copy-wizard.md) – Tento článek obsahuje podrobné informace o hello Průvodce kopírováním.
* [Zkopírujte aktivity výkonu a vyladění průvodce](data-factory-copy-activity-performance.md) – Tento článek obsahuje vyladění průvodce a měření výkonu odkaz hello.
