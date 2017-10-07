---
title: "Databáze serveru SQL tooAzure aaaMigrate databáze SQL | Microsoft Docs"
description: "Přečtěte si toomigrate vaše tooAzure databáze systému SQL Server databáze SQL."
services: sql-database
documentationcenter: 
author: janeng
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,load & move data
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 06/27/2017
ms.author: janeng
ms.openlocfilehash: d10ad1d26576194f1dd6858bae5c3e7c1ec4fb91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-sql-server-database-tooazure-sql-database"></a>Migrace vaší tooAzure databáze systému SQL Server databáze SQL

Přesunutí serveru SQL Server tooAzure databáze SQL Database je proces tři části – nejdřív připravit, pak exportovat a potom importovat hello databáze. V tomto kurzu jste postup:

> [!div class="checklist"]
> * Příprava databáze v systému SQL Server pro migraci tooAzure SQL Database pomocí hello [Data migrace pomocníka](https://www.microsoft.com/download/details.aspx?id=53595) (DMA).
> * Exportovat soubor souboru BACPAC tooa databáze hello
> * Hello souboru BACPAC soubor importovat do databáze SQL Azure

Pokud nemáte předplatné Azure, [vytvořit bezplatný účet](https://azure.microsoft.com/free/) před zahájením.

## <a name="prerequisites"></a>Požadavky

toocomplete dokončení tohoto kurzu, ujistěte se, hello následující požadavky:

- Nejnovější verze nainstalované hello [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS). Instalace aplikace SSMS nainstaluje taky hello nejnovější verzi SQLPackage, nástroje příkazového řádku, které můžou být použité tooautomate řadu úloh vývoj databáze. 
- Nainstalované hello [Data migrace pomocníka](https://www.microsoft.com/download/details.aspx?id=53595) (DMA).
- Zjištění a mít přístup tooa databáze toomigrate. Tento kurz používá hello [SQL Server 2008 R2 databáze AdventureWorks OLTP](https://msftdbprodsamples.codeplex.com/releases/view/59211) v instanci systému SQL Server 2008 R2 nebo novější, ale můžete použít libovolnou databázi podle svého výběru. problémy s kompatibilitou toofix, použijte [SQL Server Data Tools](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)

## <a name="prepare-for-migration"></a>Příprava na migraci

Jste tooprepare připravené pro migraci. Postupujte podle těchto kroků toouse hello  **[Data migrace pomocníka](https://www.microsoft.com/download/details.aspx?id=53595)**  tooassess hello připravenosti databáze pro tooAzure migrace databáze SQL.

1. Otevřete hello **Data migrace pomocníka**. Na libovolném počítači s připojením toohello databáze systému SQL Server instance obsahující hello můžete spustit přímý přístup do paměti, že máte v plánu toomigrate, není nutné tooinstall ho na hostování počítače hello hello instance systému SQL Server.

     ![Asistent migrace systému Open data](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-open.png)

2. V levé nabídce hello, klikněte na tlačítko **+ nový** toocreate **Assessment** projektu. Vyplňte formulář hello **název projektu** (všechny ostatní hodnoty musí být ponechány na jejich výchozí hodnoty) a klikněte na tlačítko **vytvořit**. Hello **možnosti** otevře se stránka.

     ![Nový projekt asistenta migrace dat](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-new-project.png)

3. Na hello **možnosti** klikněte na tlačítko **Další**. Hello **vyberte zdroje** otevře se stránka.

     ![nové možnosti migrace dat](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-options.png) 

4. Na hello **vyberte zdroje** stránky, zadejte název instance systému SQL Server obsahující server hello plánování toomigrate hello. Změna hello ostatní hodnoty na této stránce, v případě potřeby. Klikněte na **Připojit**.

     ![nové vyberte zdroje dat migrace](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-sources.png)

5. V hello **přidejte zdroje** část hello **vyberte zdroje** vyberte zaškrtávací políčka hello toobe databáze hello testovány z hlediska kompatibility. Klikněte na tlačítko **Přidat**.

     ![nové vyberte zdroje dat migrace](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-sources-add.png)

6. Klikněte na tlačítko **spustit Assessment**.

     ![nové vyhodnocení zahájení migrace dat](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-start-assessment.png)

7. Po dokončení hello assessment první pohled toosee Pokud je dostatečně kompatibilní toomigrate hello databáze. Vyhledejte hello zaškrtnutí v zelená kruh.

     ![výsledky kompatibilní nové vyhodnocení migrace dat](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-assessment-results-compatible.png)

8. Zkontrolujte výsledky hello. Hello **parity funkcí systému SQL Server** hello výchozí výsledky tooreview jsou výsledky zobrazeny. Konkrétně zkontrolujte hello informace o nepodporované a částečně podporované funkce a hello poskytuje informace o doporučených akcí. 

     ![nové paritní assessment migrace dat](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-assessment-results-parity.png)

9. Zkontrolujte hello **problémy s kompatibilitou** kliknutím na tuto možnost v levé horní části hello. Konkrétně zkontrolujte hello informace o migraci blokování, změny chování a zastaralé funkce pro každou úroveň kompatibility. Pro databázi hello AdventureWorks2008R2, zkontrolujte změny hello tooFull-Text Search od SQL Server 2008 a hello změn tooSERVERPROPERTY('LCID') od verze systému SQL Server 2000. Podrobnosti o těchto změnách se poskytuje odkazy na další informace. Mnoho možnosti vyhledávání a změnili nastavení pro fulltextové vyhledávání 

   > [!IMPORTANT] 
   > Po dokončení migrace tooAzure vaší databáze SQL Database, můžete toooperate hello databáze na jeho aktuální úroveň kompatibility (úroveň 100 pro databázi AdventureWorks2008R2 hello) nebo na vyšší úrovni. Další informace o hello důsledky a možnosti pro operační databázi na úrovni konkrétní kompatibility najdete v tématu [změnit úroveň kompatibility databáze](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Viz také [ALTER DATABASE OBOR CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) informace o další úroveň databáze nastavení související s toocompatibility úrovně.
   >

10. Volitelně klikněte na **exportovat sestavu** toosave hello sestavy jako soubor ve formátu JSON.
11. Pomocník pro migraci dat zavřít hello.

## <a name="export-toobacpac-file"></a>TooBACPAC souboru exportu 

Soubor souboru BACPAC je soubor ZIP s příponou souboru BACPAC obsahující hello metadata a data z databáze systému SQL Server. Soubor souboru BACPAC může být uložená v Azure blob storage nebo v místní úložiště pro archivaci nebo pro migraci -, jako z tooAzure systému SQL Server databáze SQL. Pro export toobe, stavu transakční konzistence, je nutné zajistit buď žádné zápisu aktivity dochází při exportu hello.

Postupujte podle těchto kroků toouse hello SQLPackage nástroj příkazového řádku tooexport hello AdventureWorks2008R2 toolocal úložiště databáze.

1. Otevřete příkazový řádek systému Windows a změňte složce tooa adresář, ve kterém máte hello **130** verze SQLPackage – například **C:\Program Files (x86) \Microsoft SQL Server\130\DAC\bin**.

2. Spustit následující příkaz SQLPackage v hello příkazového řádku tooexport hello hello **AdventureWorks2008R2** databáze z **localhost** příliš**AdventureWorks2008R2.bacpac**. Všechny tyto hodnoty změňte jako odpovídající tooyour prostředí.

    ```SQLPackage
    sqlpackage.exe /Action:Export /ssn:localhost /sdn:AdventureWorks2008R2 /tf:AdventureWorks2008R2.bacpac
    ```

    ![sqlpackage export](./media/sql-database-migrate-your-sql-server-database/sqlpackage-export.png)

Po dokončení provádění hello hello generované souboru BACPAC soubor je uložen v adresáři hello, kde se nachází hello sqlpackage spustitelný soubor. V tomto příkladu C:\Program Files (x86) \Microsoft SQL Server\130\DAC\bin. 

## <a name="log-in-toohello-azure-portal"></a>Přihlaste se toohello portálu Azure

Přihlaste se toohello [portál Azure](https://portal.azure.com/). Přihlášení z hello počítače, ze kterého spouštíte nástroj příkazového řádku SQLPackage hello usnadňuje hello vytvoření pravidla brány firewall hello v kroku 5.

## <a name="create-a-sql-server-logical-server"></a>Vytvoření logického serveru SQL server

A [logickému serveru SQL server](sql-database-features.md) funguje jako centrální administrativní bod pro více databází. Postupujte podle těchto kroků toocreate SQL server logický server toocontain hello migrovat databázi společnosti Adventure Works OLTP SQL serveru. 

1. Klikněte na tlačítko hello **nový** nalezeno tlačítko na hello levém horním rohu hello portálu Azure.

2. Typ **systému sql server** v okně hledání hello na hello **nový** a vyberte **databáze SQL (logický server)** z hello filtrovaný seznam.

    ![výběr logického serveru](./media/sql-database-migrate-your-sql-server-database/logical-server.png)

3. Na hello **všechno, co** klikněte na tlačítko **systému SQL server (logický server)** a pak klikněte na **vytvořit** na hello **systému SQL Server (logický server)** stránky .

    ![vytvoření logického serveru](./media/sql-database-migrate-your-sql-server-database/logical-server-create.png)

4. Vyplňte hello SQL server (logický server) formulář s hello následující informace, jak je znázorněno na hello předcházející bitové kopie:     

   | Nastavení       | Navrhovaná hodnota | Popis | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Název serveru** | Zadejte všechny globálně jedinečného názvu | Platné názvy serverů najdete v tématu [Pravidla a omezení pojmenování](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). | 
   | **Přihlašovací jméno správce serveru** | Zadat libovolný platný název | Platná přihlašovací jména najdete v tématu [Identifikátory databází](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers). |
   | **Heslo** | Zadejte všechny platné heslo | Heslo musí mít aspoň 8 znaků a musí obsahovat znaky ze tří z následujících kategorií hello: velká písmena, malá písmena, číslice a jiné než alfanumerické znaky. |
   | **Předplatné** | Vyberte předplatné. | Podrobnosti o vašich předplatných najdete v tématu [Předplatná](https://account.windowsazure.com/Subscriptions). |
   | **Skupina prostředků** | Vyberte existující skupinu prostředků nebo vytvořte novou skupinu, například **myResourceGroup** |  Platné názvy skupin prostředků najdete v tématu [Pravidla a omezení pojmenování](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). |
   | **Umístění** | Zadejte všechny platné umístění pro nový server hello | Informace o oblastech najdete v tématu [Oblasti služeb Azure](https://azure.microsoft.com/regions/). |

   ![vytvoření logického serveru byla dokončena formuláře](./media/sql-database-migrate-your-sql-server-database/logical-server-create-completed.png)

5. Klikněte na tlačítko **vytvořit** tooprovision hello logického serveru. Zřizování trvá několik minut. 

> [!IMPORTANT]
> Mějte na paměti, váš název serveru, serveru správce přihlašovací jméno a heslo. Je nutné tyto hodnoty později v tomto kurzu.
>

## <a name="create-a-server-level-firewall-rule"></a>Vytvoření pravidla brány firewall na úrovni serveru

Vytvoří Hello služba SQL Database [brány firewall na úrovni serveru hello](sql-database-firewall-configure.md) , který brání externí aplikace a nástroje pro připojení toohello serveru nebo kterékoli databázi na serveru hello, pokud je vytvořeno pravidlo brány firewall tooopen hello brány firewall pro konkrétní IP adresy. Postupujte podle těchto kroků toocreate pravidlo brány firewall na úrovni serveru SQL Database pro IP adresu hello hello počítače, ze kterého spouštíte nástroj příkazového řádku SQLPackage hello. To umožňuje SQLPackage tooconnect toohello serverDatabase logickému serveru SQL přes bránu firewall databáze Azure SQL Database hello. 

1. Klikněte na tlačítko **všechny prostředky** z nabídky na levé straně hello a klikněte na nový server na hello **všechny prostředky** stránky. stránka s přehledem Hello pro váš server otevře a poskytuje možnosti pro další konfiguraci.

     ![Přehled logický server](./media/sql-database-migrate-your-sql-server-database/logical-server-overview.png)

2. Klikněte na tlačítko **brány Firewall** v levé nabídce hello pod **nastavení** na stránce Přehled hello. Hello **nastavení brány Firewall** otevře se stránka pro hello databáze SQL server. 

3. Klikněte na tlačítko **přidat IP adresu klienta** na hello nástrojů tooadd hello IP adresu počítače hello aktuálně používáte a potom klikněte na **Uložit**. Pravidlo brány firewall na úrovni serveru se vytvoří pro tuto IP adresu.

     ![nastavení pravidla brány firewall serveru](./media/sql-database-migrate-your-sql-server-database/server-firewall-rule-set.png)

4. Klikněte na **OK**.

Teď se můžete připojit tooall databáze na tomto serveru pomocí SQL Server Management Studio nebo jiný nástroj podle svého výběru z tuto IP adresu pomocí účtu správce serveru hello vytvořili dříve.

> [!NOTE]
> SQL Database komunikuje přes port 1433. Pokud se pokoušíte tooconnect z podnikové sítě, odchozí provoz přes port 1433 nemusí mít povolený bránou firewall vaší sítě. Pokud ano, nemůžete připojit tooyour serveru Azure SQL Database, pokud vaše IT oddělení otevře port 1433.
>

## <a name="import-a-bacpac-file-tooazure-sql-database"></a>Importovat souboru BACPAC souboru tooAzure databáze SQL 

nejnovější verze Hello hello nástroj příkazového řádku SQLPackage poskytovat podporu pro vytvoření Azure SQL database v zadané [služby úroveň a úroveň výkonu](sql-database-service-tiers.md). Pro nejlepší výkon při importu vyberte vysokou úroveň služby a výkonu úroveň a pak vertikálně snížit kapacitu po importu Pokud hello úroveň služby a výkonu úroveň je vyšší, než je nutné okamžitě.

Postupujte podle těchto kroků použití hello SQLPackage nástroj příkazového řádku tooimport hello AdventureWorks2008R2 databáze tooAzure databáze SQL. Když používáte SQL Server Management Studio pro tuto úlohu je SQLPackage hello preferované metoda pro většinu produkčního prostředí pro maximální flexibilitu a zlepšení výkonu. V tématu [migrace ze systému SQL Server tooAzure databáze SQL pomocí souboru BACPAC souborů](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

- Spustit následující příkaz SQLPackage v hello příkazového řádku tooimport hello hello **AdventureWorks2008R2** databáze z místního úložiště toohello SQL serveru logického serveru této jste dříve vytvořenou tooa novou databázi, služby úroveň **Premium**a cíl služby z **P6**. Nahraďte příslušnými hodnotami pro logický server SQL server hello hodnoty v lomených závorkách a zadejte název pro novou databázi hello (také nahradit hello lomené závorky). Můžete také toochange hello hodnoty pro edici databáze a služba objectgive jako odpovídající tooyour prostředí. Za účelem hello tohoto kurzu, se nazývá migrované databáze hello **myMigratedDatabase**.

    ```
    SqlPackage.exe /a:import /tcs:"Data Source=<your_server_name>.database.windows.net;Initial Catalog=<your_new_database_name>;User Id=<change_to_your_admin_user_account>;Password=<change_to_your_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
    ```

   ![sqlpackage import](./media/sql-database-migrate-your-sql-server-database/sqlpackage-import.png)

> [!IMPORTANT]
> Logickému serveru SQL server naslouchá na portu 1433. Pokud se pokoušíte tooconnect tooa serveru SQL server logické z v rámci podnikové brány firewall, musí být tento port otevřít v hello podniková brána firewall pro připojení je toosuccessfully.
>

## <a name="connect-using-sql-server-management-studio-ssms"></a>Připojení pomocí SQL Server Management Studio (SSMS)

Používat SQL Server Management Studio tooestablish serveru Azure SQL Database tooyour připojení a nově migrovaných databázi názvem **myMigratedDatabase** v tomto kurzu. Pokud používáte systém SQL Server Management Studio do jiného počítače, z níž jste spustili SQLPackage, vytvořte pravidlo brány firewall pro tento počítač pomocí hello kroků v předchozím postupu hello.

1. Otevřete SQL Server Management Studio.

2. V hello **připojit tooServer** dialogovém okně zadejte hello následující informace:
   - **Typ serveru:** Zadejte Databázový stroj.
   - **Název serveru**: Zadejte název plně kvalifikovaný serveru, jako například **mynewserver20170403.database.windows.net**
   - **Ověřování:** Zadejte Ověřování SQL Serveru.
   - **Přihlášení:** Zadejte účet správce serveru.
   - **Heslo**: Zadejte hello heslo pro váš účet správce serveru
 
    ![připojit pomocí ssms](./media/sql-database-migrate-your-sql-server-database/connect-ssms.png)

3. Klikněte na **Připojit**. Otevře se okno Průzkumník objektů Hello. 

4. V Průzkumníku objektů rozbalte **databáze** a potom rozbalte **myMigratedDatabase** tooview hello objekty v ukázkové databázi hello.

## <a name="change-database-properties"></a>Změnit vlastnosti databáze

Můžete změnit úroveň služby hello úroveň výkonu a úroveň kompatibility pomocí SQL Server Management Studio. Během fáze importu hello doporučujeme, abyste importu tooa vyšší výkon vrstvy databáze pro nejlepší výkon, ale po dokončení importu hello toosave money tak, abyste byli připravené tooactively použijte hello importovaných database škálování směrem dolů. Změna úrovně kompatibility hello mohou vést k vyšší výkon a přístup toohello nejnovější schopnosti hello služby Azure SQL Database. Když provádíte migraci starší databázi, její úroveň kompatibility databáze je udržován na úrovni hello nejnižší podporované úrovně, který je kompatibilní s databází hello importovaných. Další informace najdete v tématu [vylepšuje výkon dotazů 130 úroveň kompatibility v Azure SQL Database](sql-database-compatibility-level-query-performance-130.md).

1. V Průzkumníku objektů, klikněte pravým tlačítkem na **myMigratedDatabase** a klikněte na tlačítko **nový dotaz**. Otevře se okno dotazu připojeného tooyour databáze.

2. Spustit následující příkaz tooset hello služby vrstvě příliš hello**standardní** a úroveň výkonu hello příliš**S1**.

    ```
    ALTER DATABASE myMigratedDatabase 
    MODIFY 
        (
        EDITION = 'Standard'
        , MAXSIZE = 250 GB
        , SERVICE_OBJECTIVE = 'S1'
    );
    ```

    ![změnit úroveň služby](./media/sql-database-migrate-your-sql-server-database/service-tier.png)

3. Provést následující úroveň kompatibility databáze hello toochange příkaz příliš hello**130**.

    ```
    ALTER DATABASE myMigratedDatabase  
    SET COMPATIBILITY_LEVEL = 130;
    ```

   ![Změňte úroveň kompatibility](./media/sql-database-migrate-your-sql-server-database/compat-level.png)

## <a name="next-steps"></a>Další kroky 
V tomto kurzu připravená, exportovat a importovat vaší databáze. Jste se dozvěděli, do:

> [!div class="checklist"]
> * Příprava databáze v systému SQL Server pro tooAzure migrace databáze SQL
> * Exportovat soubor souboru BACPAC tooa databáze hello
> * Hello souboru BACPAC soubor importovat do databáze SQL Azure

Jak zálohy další kurz toolearn toohello toosecure vaší databáze.

> [!div class="nextstepaction"]
> [Zabezpečení databáze Azure SQL](sql-database-security-tutorial.md).


