---
title: "aaaGetting začít s Azure synchronizaci dat SQL (Preview) | Microsoft Docs"
description: "Tento kurz vám pomůže začít pracovat s synchronizaci dat SQL Azure (Preview)."
services: sql-database
documentationcenter: 
author: douglaslms
manager: craigg
editor: 
ms.assetid: a295a768-7ff2-4a86-a253-0090281c8efa
ms.service: sql-database
ms.custom: load & move data
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: douglasl
ms.openlocfilehash: 666d09237e42acc23ae3c8c81e60734a413f5949
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-sql-data-sync-preview"></a>Začínáme s synchronizaci dat Azure SQL (Preview)
V tomto kurzu zjistíte, jak tooset až synchronizaci dat SQL Azure vytvořit skupinu hybridních synchronizace, který obsahuje instance Azure SQL Database a SQL Server. Hello nové synchronizace skupiny plně konfigurována a synchronizuje na hello nastaveného plánu.

Tento kurz předpokládá, že máte alespoň zkušenosti s SQL Database a SQL Server. 

Přehled synchronizaci dat SQL najdete v tématu [synchronizaci dat](sql-database-sync-data.md).

Pro dokončení příklady prostředí PowerShell, které ukazují, jak tooconfigure synchronizaci dat SQL, najdete v části hello následující články:
-   [Použití prostředí PowerShell toosync mezi více databází Azure SQL](scripts/sql-database-sync-data-between-sql-databases.md)
-   [Použití prostředí PowerShell toosync mezi databáze SQL Azure a místní databáze SQL serveru](scripts/sql-database-sync-data-between-azure-onprem.md)

> [!NOTE]
> Hello dokončení technická dokumentace pro synchronizaci dat SQL Azure, byly dříve umístěny na webu MSDN, nastavit je jako. Dokument PDF. Stažení [zde](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true).

## <a name="step-1---create-sync-group"></a>Krok 1 – Vytvoření skupiny synchronizace

### <a name="locate-hello-data-sync-settings"></a>Najít nastavení synchronizace dat hello

1.  V prohlížeči přejděte toohello portálu Azure.

2.  Hello portálu vyhledejte vaší databáze SQL z řídicího panelu nebo hello databází SQL ikonu na panelu nástrojů hello.

    ![Seznam databází Azure SQL](media/sql-database-get-started-sql-data-sync/datasync-preview-sqldbs.png)

3.  Na hello **databází SQL** okně, vyberte hello stávající má toouse jako databáze hello rozbočovače pro synchronizace dat. hello SQL databáze okno otevře databáze SQL.

4.  V okně databáze SQL hello hello zvolené databázi, vyberte **synchronizaci databázích tooother**. Otevře se okno synchronizaci dat Hello.

    ![Možnosti databáze tooother synchronizace](media/sql-database-get-started-sql-data-sync/datasync-preview-newsyncgroup.png)

### <a name="create-a-new-sync-group"></a>Vytvořit novou skupinu synchronizace

1.  V okně hello synchronizaci dat, vyberte **nové synchronizace skupiny**. Hello **nové synchronizace skupiny** otevře se okno s krokem 1 **vytvořit skupinu synchronizace**, zvýrazněné. Hello **vytvořit skupinu synchronizace dat** také otevře se okno.

2.  Na hello **vytvořit skupinu synchronizace dat** okně hello následující věci:

    1.  V hello **název skupiny synchronizace** pole, zadejte název nové skupiny synchronizace hello.

    2.  V hello **Metadata databáze Sync** zvolte, zda toocreate novou databázi (doporučeno) nebo toouse existující databáze.

        > [!NOTE]
        > Společnost Microsoft doporučuje, abyste vytvořili novou prázdnou databázi toouse jako hello Metadata databáze Sync. Synchronizaci dat vytváří tabulky v této databázi a spustí Časté úlohy. Tato databáze je automaticky sdílen jako hello databáze Metadata synchronizace pro všechny skupiny synchronizace v hello vybrané oblasti. Hello Metadata databáze Sync, jeho název nebo jeho úrovně služby nelze změnit bez vyřadíte.

        Pokud jste zvolili **novou databázi**, vyberte **vytvořit novou databázi.** Hello **SQL Database** otevře se okno. Na hello **SQL Database** okno, název a nakonfigurujte novou databázi hello. Potom vyberte **OK**.

        Pokud jste zvolili **použít existující databázi**, vyberte databázi hello hello seznamu.

    3.  V hello **automatické synchronizace** část, vyberte nejdřív **na** nebo **vypnout**.

        Pokud jste zvolili **na**, v hello **frekvence synchronizace** , zadejte číslo a vyberte sekund, minut, hodin nebo dnů.

        ![Zadejte četnost synchronizace](media/sql-database-get-started-sql-data-sync/datasync-preview-syncfreq.png)

    4.  V hello **řešení konfliktů** vyberte "Rozbočovače wins" nebo "Člen wins."

        ![Zadejte způsob řešení konfliktů](media/sql-database-get-started-sql-data-sync/datasync-preview-conflictres.png)

    5.  Vyberte **OK** a počkejte hello nové synchronizace skupiny toobe vytvoření a nasazení.

## <a name="step-2---add-sync-members"></a>Krok 2 – přidání členů synchronizace

Po hello nové synchronizace skupiny se vytvoří a nasadí, krok 2, **přidat členy synchronizace**, je označený na hello **nové synchronizace skupiny** okno.

V hello **rozbočovače databáze** zadejte hello existující přihlašovací údaje pro hello databáze SQL server, na které hello je umístěna databáze rozbočovače. Nezadávejte *nové* přihlašovacích údajů v této části.

![Centrum databáze byla přidána toosync skupiny](media/sql-database-get-started-sql-data-sync/datasync-preview-hubadded.png)

## <a name="add-an-azure-sql-database"></a>Přidat k Azure SQL Database

V hello **databázi člena** část, volitelně přidat skupinu Azure SQL Database toohello synchronizaci výběrem **přidat databázi Azure**. Hello **konfigurace databáze Azure** otevře se okno.

Na hello **konfigurace databáze Azure** okně hello následující věci:

1.  V hello **název člena synchronizace** pole, zadejte název nového člena synchronizace hello. Tento název se liší od názvu hello samotná databáze hello.

2.  V hello **předplatné** pole, vyberte hello přidružené předplatné Azure pro účely fakturace.

3.  V hello **Azure SQL Server** hello pole, vyberte existující databázový server SQL.

4.  V hello **Azure SQL Database** hello pole, vyberte existující databázi SQL.

5.  V hello **synchronizace pokynů** vyberte obousměrné synchronizace, toohello rozbočovače, nebo z hello rozbočovače.

    ![Přidání nového člena synchronizace databáze SQL](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadding.png)

6.  V hello **uživatelské jméno** a **heslo** pole, zadejte hello existující přihlašovací údaje pro hello databáze SQL server, na které hello je umístěna databáze člena. Nezadávejte *nové* přihlašovacích údajů v této části.

7.  Vyberte **OK** a počkejte hello nové synchronizace člen toobe vytvoření a nasazení.

    ![Byl přidán nový člen synchronizace databáze SQL](media/sql-database-get-started-sql-data-sync/datasync-preview-memberadded.png)

## <a name="add-an-on-premises-sql-server-database"></a>Přidat místní databázi systému SQL Server

V hello **databázi člena** část, volitelně přidat skupinu místní toohello systému SQL Server synchronizace výběrem **přidat místní databázi**. Hello **nakonfigurovat místní** otevře se okno.

Na hello **nakonfigurovat místní** okně hello následující věci:

1.  Vyberte **zvolte hello brány agenta synchronizace**. Hello **vyberte agenta synchronizace** otevře se okno.

    ![Zvolte bránu agenta synchronizace hello](media/sql-database-get-started-sql-data-sync/datasync-preview-choosegateway.png)

2.  Na hello **zvolte hello brány agenta synchronizace** okně zvolte zda toouse existujícího agenta nebo vytvořte nový agent.

    Pokud jste zvolili **existující agenty**, vyberte hello existujícího agenta ze seznamu hello.

    Pokud jste zvolili **vytvořit nový agent**, hello následující věci:

    1.  Stáhnout klientský software agenta synchronizace, hello z uvedeného odkazu hello a nainstalujte ji na hello počítači, kde se nachází hello systému SQL Server.
 
        > [!IMPORTANT]
        > Máte tooopen odchozí port TCP 1433 v hello brány firewall toolet hello Klientský agent komunikovat se serverem hello.


    2.  Zadejte název pro agenta hello.

    3.  Vyberte **vytvořit a vygenerovat klíč**.

    4.  Kopírování hello agenta klíče toohello schránky.
        
        ![Vytvoření nového agenta synchronizace](media/sql-database-get-started-sql-data-sync/datasync-preview-selectsyncagent.png)

    5.  Vyberte **OK** tooclose hello **vyberte agenta synchronizace** okno.

    6.  Na počítači systému SQL Server hello vyhledání a spuštění aplikace hello synchronizace agenta klienta.

        ![synchronizace dat Hello klientskou aplikaci pro agenta](media/sql-database-get-started-sql-data-sync/datasync-preview-clientagent.png)

    7.  V aplikaci agenta hello synchronizace, vyberte **odeslání klíč agenta**. Hello **konfigurace databáze synchronizace metadat** otevře se dialogové okno.

    8.  V hello **konfigurace databáze synchronizace metadat** dialogové okno, vložte klíč agenta hello zkopírovaných z hello portálu Azure. Zadejte také hello existující pověření pro server hello Azure SQL Database, na které hello je umístěna databáze metadat. (Pokud jste vytvořili novou databázi metadat, tato databáze je v hello stejný server jako databáze rozbočovače hello.) Vyberte **OK** a počkejte toofinish konfigurace hello.

        ![Zadejte pověření agenta klíč a server hello](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-enterkey.png)

        >   [!NOTE] 
        >   Pokud dojde k chybě brány firewall v tomto okamžiku, máte toocreate pravidlo brány firewall na Azure tooallow příchozí provoz z počítače serveru SQL Server hello. Můžete vytvořit pravidlo hello ručně hello portálu, ale možná bude snazší toocreate je v serveru SQL Server Management Studio (SSMS). V aplikaci SSMS zkuste tooconnect toohello rozbočovače databáze v Azure. Zadejte jeho název jako \<hub_database_name\>. database.windows.net. Postupujte podle kroků hello v hello dialogové okno pole tooconfigure hello brány Azure pravidlo. Pak se vraťte toohello agenta synchronizace klienta aplikace.

    9.  V aplikaci hello klientský Agent synchronizace, klikněte na tlačítko **zaregistrovat** tooregister databázi systému SQL Server s agentem hello. Hello **konfigurace serveru SQL Server** otevře se dialogové okno.

        ![Přidejte a nakonfigurujte databázi systému SQL Server](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-adddb.png)

    10. V hello **konfigurace serveru SQL Server** dialogovém okně vyberte zda tooconnect pomocí ověřování systému SQL Server nebo ověřování systému Windows. Pokud jste vybrali ověřování SQL serveru, zadejte existující přihlašovací údaje hello. Zadejte název serveru SQL Server hello a hello název hello databáze, které chcete toosync. Vyberte **testovací připojení** tootest nastavení. Potom vyberte **Uložit**. Hello registrované databáze se zobrazí v seznamu hello.

        ![Databáze systému SQL Server je teď zaregistrovaný.](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-dbadded.png)

    11. Teď můžete zavřít hello agenta synchronizace klienta aplikace.

    12. Hello portálu na hello **nakonfigurovat místní** vyberte **vyberte hello databáze.** Hello **vybrat databázi** otevře se okno.

    13. Na hello **vybrat databázi** okno, v hello **název člena synchronizace** pole, zadejte název nového člena synchronizace hello. Tento název se liší od názvu hello samotná databáze hello. Vyberte v seznamu hello hello databáze. V hello **synchronizace pokynů** vyberte obousměrné synchronizace, toohello rozbočovače, nebo z hello rozbočovače.

        ![Vyberte hello v místní databázi](media/sql-database-get-started-sql-data-sync/datasync-preview-selectdb.png)

    14. Vyberte **OK** tooclose hello **vybrat databázi** okno. Potom vyberte **OK** tooclose hello **nakonfigurovat místní** okno a počkejte hello nové synchronizace člen toobe vytvoření a nasazení. Nakonec klikněte na **OK** tooclose hello **vyberte synchronizace členů** okno.

        ![V místní databázi přidat toosync skupiny](media/sql-database-get-started-sql-data-sync/datasync-preview-onpremadded.png)

3.  tooconnect tooSQL synchronizaci dat a hello místní agent, přidejte vaše uživatelská role toohello název `DataSync_Executor`. Synchronizaci dat vytvoří této role v instanci systému SQL Server hello.

## <a name="step-3---configure-sync-group"></a>Krok 3: Konfigurace skupiny synchronizace

Po hello nové synchronizace členů skupiny se vytváří a nasazují, krok 3 **skupiny synchronizace konfigurace**, je označený na hello **nové synchronizace skupiny** okno.

1.  Na hello **tabulky** okně, vyberte databázi z hello seznam synchronizace členy skupiny a potom vyberte **aktualizace schématu**.

2.  Hello seznam dostupných tabulek vyberte hello tabulek, které chcete toosync.

    ![Vyberte tabulky toosync](media/sql-database-get-started-sql-data-sync/datasync-preview-tables.png)

3.  Ve výchozím nastavení jsou vybrané všechny sloupce v tabulce hello. Pokud nechcete, aby toosync všechny sloupce hello, zakažte hello zaškrtávací políčko pro hello sloupce, které nechcete toosync. Být vybrána tooleave hello sloupec primárního klíče.

    ![Vyberte pole toosync](media/sql-database-get-started-sql-data-sync/datasync-preview-tables2.png)

4.  Nakonec vyberte **Uložit**.

## <a name="next-steps"></a>Další kroky
Blahopřejeme. Vytvořili jste skupinu synchronizace, která obsahuje instance databáze SQL a databáze SQL serveru.

Další informace o SQL Database a synchronizaci dat SQL najdete v tématu:

-   [Stáhnout hello úplnou synchronizaci dat SQL technická dokumentace](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true)
-   [Stáhněte si dokumentaci k REST API služby SQL Data synchronizace hello](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)
-   [Databáze SQL – přehled](sql-database-technical-overview.md)
-   [Správa životního cyklu databáze](https://msdn.microsoft.com/library/jj907294.aspx)
