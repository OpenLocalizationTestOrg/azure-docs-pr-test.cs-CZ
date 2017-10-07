---
title: "aaaLoad data do Azure SQL Data Warehouse – Data Factory | Microsoft Docs"
description: "V tomto kurzu načte data do Azure SQL Data Warehouse pomocí Azure Data Factory a používá databázi systému SQL Server jako zdroj dat hello."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse;azure-data-factory
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: loading
ms.date: 02/08/2017
ms.author: cakarst;barbkess
ms.openlocfilehash: 471871d3ee00ab34cc84bb63fbd13a323d14c2b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-into-sql-data-warehouse-with-data-factory"></a>Načtení dat do SQL Data Warehouse pomocí služby Data Factory

Můžete použít Azure Data Factory tooload data do Azure SQL Data Warehouse z jakéhokoli z hello [podporovanými úložišti dat zdroj](../data-factory/data-factory-data-movement-activities.md#supported-data-stores-and-formats). Například můžete načtení dat z Azure SQL database nebo databáze Oracle do SQL data warehouse pomocí služby Data Factory. Kurzu v tomto článku se dozvíte, jak tooload dat z SQL serveru místní databáze do SQL data warehouse.

**Odhad času**: v tomto kurzu, trvá asi 10 až 15 minut toocomplete po splnění předpokladů hello.

## <a name="prerequisites"></a>Požadavky

- Je nutné **databáze systému SQL Server** s tabulkami, které obsahují hello data toobe zkopírovali toohello SQL datového skladu.  

- Je třeba online **SQL Data Warehouse**. Pokud již datový sklad nemáte, zjistěte, jak příliš[vytvoření Azure SQL Data Warehouse](sql-data-warehouse-get-started-provision.md).

- Budete potřebovat **účet úložiště Azure**. Pokud již nemáte účet úložiště, zjistěte, jak příliš[vytvořit účet úložiště](../storage/common/storage-create-storage-account.md). Pro nejlepší výkon, vyhledejte účet úložiště hello a hello datový sklad v hello stejné oblasti Azure.

## <a name="configure-a-data-factory"></a>Konfigurace služby data factory
1. Přihlaste se toohello [portál Azure][].
2. Vyhledejte datového skladu a klikněte na tlačítko tooopen ho.
3. V hlavním okně hello, klikněte na **načítání dat** > **Azure Data Factory**.

    ![Spustí Průvodce načítání dat](media/sql-data-warehouse-load-with-data-factory/launch-load-data-wizard.png)

4. Pokud nemáte objekt pro vytváření dat ve vašem předplatném Azure, uvidíte **nový objekt pro vytváření dat** dialogové okno na samostatné kartě hello prohlížeče. Vyplňte hello požadované informace a klikněte na tlačítko **vytvořit**. Po vytvoření objektu pro vytváření dat hello hello **nový objekt pro vytváření dat** dialog se zavře a zobrazí hello **vyberte objekt pro vytváření dat** dialogové okno.

    Pokud máte jeden nebo více datové továrny už v hello předplatné Azure, uvidíte hello **vyberte objekt pro vytváření dat** dialogové okno. V tomto dialogovém můžete vybrat existující datovou továrnu nebo klikněte na tlačítko **vytvořit nový objekt pro vytváření dat** toocreate novou.

    ![Konfigurace objektu pro vytváření dat](media/sql-data-warehouse-load-with-data-factory/configure-data-factory.png)

5. V hello **vyberte objekt pro vytváření dat** dialogové okno, hello **načíst data** ve výchozím nastavení je vybraná možnost. Klikněte na tlačítko **Další** toostart vytváření dat načítání úloh.

## <a name="configure-hello-data-factory-properties"></a>Konfigurovat vlastnosti objektu pro vytváření dat hello
Teď, když jste vytvořili objekt pro vytváření dat, hello dalším krokem je tooconfigure hello data načítání plánu.

1. Pro **název úlohy**, zadejte **DWLoadData fromSQLServer**.
2. Použít výchozí hello **spustit jednou nyní** možnost, klikněte na tlačítko **Další**.

    ![Nakonfigurujte plán zatížení](media/sql-data-warehouse-load-with-data-factory/configure-load-schedule.png)

## <a name="configure-hello-source-data-store-and-gateway"></a>Konfigurace hello zdrojového úložiště dat a brány
Objekt pro vytváření dat se teď říct o databázi systému SQL Server místní hello, ze kterého chcete tooload data.

1. Zvolte **systému SQL Server** z hello podporované zdroje dat ukládání katalogu a klikněte na tlačítko **Další**.

    ![Vyberte zdroj systému SQL Server](media/sql-data-warehouse-load-with-data-factory/choose-sql-server-source.png)

2. A **databáze systému SQL Server místní hello zadejte** otevře se dialogové okno. nejprve Hello **název připojení** je automaticky vyplněno pole. druhé pole Hello požádá o hello název hello **brány**. Pokud používáte existující datovou továrnu, která již má bránu, můžete znovu použít bránu hello vyberete v rozevíracím seznamu hello. Klikněte na tlačítko hello **vytvořit bránu** toocreate Brána pro správu dat na odkaz.  

    > [!NOTE]
    > Pokud hello zdrojového úložiště dat je místně nebo na virtuálním počítači Azure IaaS, brána pro správu dat je požadovaná. Brána je v relaci 1-1 pomocí služby data factory. Nedá se použít z jiného objektu pro vytváření dat, ale mohou být využívána načítání úlohy s v hello více dat stejné služby data factory. Při spuštění úlohy načítání dat, může být bránu použité tooconnect toomultiple datová úložiště.
    >
    > Podrobné informace o bráně hello najdete v tématu [Brána pro správu dat](../data-factory/data-factory-data-management-gateway.md) článku.

3. A **vytvořit bránu** zobrazí se dialogové okno. Název zadejte **GatewayForDWLoading**a klikněte na tlačítko **vytvořit**.

4. A **konfigurace brány** zobrazí se dialogové okno. Klikněte na tlačítko **spusťte Expresní instalace na tomto počítači** tooautomatically stáhnout, nainstalovat a zaregistrovat brány pro správu dat na počítači pro aktuální. průběh Hello je zobrazen v místním okně. Pokud počítač hello se nemůže připojit toohello úložiště dat, můžete ručně [stáhnout a nainstalovat bránu hello](https://www.microsoft.com/download/details.aspx?id=39717) na počítači, který může připojit toohello data ukládat a potom pomocí klíče tooregister hello.
    > [!NOTE]
    > Expresní instalace Hello nativně pracuje s Microsoft Edge a prohlížeče Internet Explorer. Pokud používáte Google Chrome, nejprve nainstalujte rozšíření hello ClickOnce z internetového obchodu Chrome.

    ![Spusťte instalační program Express](media/sql-data-warehouse-load-with-data-factory/launch-express-setup.png)

5. Počkejte toocomplete instalační program brány hello. Jakmile hello brány se úspěšně zaregistroval a je online, hello automaticky otevírané okno se zavře a hello nové brány se zobrazí v poli brána hello. Potom vyplňte hello rest požadovaná pole následujícím způsobem, pak klikněte na tlačítko **Další**.
    - **Název serveru**: název hello místní systém SQL Server.
    - **Název databáze**: databáze systému SQL Server.
    - **Přihlašovací údaje šifrování**: použijte výchozí hello "ve webovém prohlížeči".
    - **Typ ověřování**: Zvolte hello typ ověřování, kterou používáte.
    - **Uživatelské jméno** a **heslo**: Zadejte hello uživatelské jméno a heslo pro uživatele, který má oprávnění toocopy hello data.

    ![Spusťte instalační program Express](media/sql-data-warehouse-load-with-data-factory/configure-sql-server.png)

6. dalším krokem Hello je toochoose hello tabulky z datových toocopy hello. Hello tabulky můžete filtrovat pomocí klíčových slov. A můžete zobrazit náhled hello dat a tabulka schématu dolním panelu hello. Po dokončení váš výběr, klikněte na tlačítko **Další**.

    ![Výběr tabulek](media/sql-data-warehouse-load-with-data-factory/select-tables.png)

## <a name="configure-hello-destination-your-sql-data-warehouse"></a>Nakonfigurujte cíl hello, datový sklad SQL

Objekt pro vytváření dat se teď říct o informace o cíli hello.

1. Informace o připojení SQL Data Warehouse se vyplní automaticky. Zadejte heslo hello hello uživatelské jméno. a klikněte na tlačítko **Další**.

    ![Konfigurace cílového](media/sql-data-warehouse-load-with-data-factory/configure-destination.png)

2. Zobrazí se mapování inteligentního tabulky, která mapuje zdrojové tabulky toodestination podle názvů tabulek. Pokud tabulka hello neexistuje v cílovém umístění hello, ve výchozím nastavení ADF vytvoří s hello stejným názvem (platí tooSQL serveru nebo Azure SQL Database jako zdroj). Můžete také toomap tooan existující tabulce. Zkontrolujte a klikněte na **Další**.

    ![Mapování tabulek](media/sql-data-warehouse-load-with-data-factory/table-mapping.png)

3. Zkontrolujte mapování hello schématu a vyhledejte chyby nebo upozornění. Inteligentní mapování je založena na název sloupce. Pokud konverzi nepodporovaný datový typ mezi zdrojovým a cílovým sloupcem hello se zobrazí chybová zpráva spolu s odpovídající tabulky hello. Pokud se rozhodnete toolet Data Factory automaticky vytvářet tabulky hello, převod typu správné dat může dojít v případě potřeby toofix hello nekompatibilita mezi zdrojovým a cílovým úložišti.

    ![Schéma mapování](media/sql-data-warehouse-load-with-data-factory/schema-mapping.png)

4. Klikněte na **Další**.

## <a name="configure-hello-performance-settings"></a>Konfigurovat nastavení výkonu hello
V konfiguracích hello výkonu, nakonfigurujte účet úložiště Azure používají pro pracovní hello data předtím, než se načte do SQL Data Warehouse pomocí performantly [PolyBase](sql-data-warehouse-best-practices.md#use-polybase-to-load-and-export-data-quickly). Po dokončení kopírování hello hello průběžných dat v úložišti se automaticky vyčistí.

Vyberte stávající účet úložiště Azure a klikněte na **Další**.

![Konfigurovat pracovní objektů blob](media/sql-data-warehouse-load-with-data-factory/configure-staging-blob.png)

## <a name="review-summary-information-and-deploy-hello-pipeline"></a>Zkontrolujte souhrnné informace a nasaďte kanál hello

Zkontrolujte konfiguraci hello a klikněte na tlačítko **Dokončit** tlačítko toodeploy hello kanálu.

![Nasazení služby data factory](media/sql-data-warehouse-load-with-data-factory/deploy-data-factory.png)

## <a name="monitor-data-loading-progress"></a>Probíhá načítání dat monitorování

Zobrazí se průběh nasazení hello a má za následek hello **nasazení** stránky.

1. Po dokončení nasazení hello kliknutím na odkaz hello, která uvádí, že **kliknutím sem kanál kopírování toomonitor** toomonitor data průběhu načítání.

    ![Monitorování kanálu](media/sql-data-warehouse-load-with-data-factory/monitor-pipeline.png)

2. nově vytvořený Hello **DWLoadData fromSQLServer** kanálu načítání dat se automaticky vybere ze hello levé **Průzkumníka prostředků**.

    ![Zobrazení kanálu](media/sql-data-warehouse-load-with-data-factory/view-pipeline.png)

3. Klikněte na tlačítko do kanálu hello uprostřed hello panely toosee hello podrobný stav pro každou tabulku, která mapuje tooan aktivity.

    ![Zobrazení tabulky aktivity](media/sql-data-warehouse-load-with-data-factory/view-table-activity.png)

4. Klikněte na další na aktivity a zobrazí data hello načítání podrobností o v pravém panelu hello včetně velikost dat, řádky, propustnost, atd.

    ![Zobrazit podrobnosti o aktivitě tabulky](media/sql-data-warehouse-load-with-data-factory/view-table-activity-details.png)

5. toolaunch toto monitorování zobrazit později, přejděte tooyour SQL Data Warehouse, klikněte na tlačítko **načítání dat > Azure Data Factory**, vyberete továrnu a zvolit **monitorování existující načítání úlohy**.

## <a name="next-steps"></a>Další kroky

toomigrate tooSQL vaše databáze datového skladu, najdete v části [Přehled migrace](sql-data-warehouse-overview-migrate.md).

toolearn informace o Azure Data Factory a možnosti přesun dat najdete v části hello následující články:

- [Úvod tooAzure objekt pro vytváření dat](../data-factory/data-factory-introduction.md)
- [Přesun dat pomocí aktivity kopírování](../data-factory/data-factory-data-movement-activities.md)
- [Přesun dat tooand z Azure SQL Data Warehouse pomocí Azure Data Factory](../data-factory/data-factory-azure-sql-data-warehouse-connector.md)

tooexplore vaše data v SQL Data Warehouse, najdete v části hello následující články:

- [Připojení k sadě Visual Studio a rozšíření SSDT tooSQL datového skladu](sql-data-warehouse-query-visual-studio.md)
- [Vizuální data s Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md).

<!-- Azure references -->
[portál Azure]: https://portal.azure.com
