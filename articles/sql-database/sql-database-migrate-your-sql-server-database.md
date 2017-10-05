---
title: Migrace serveru SQL DB do Azure SQL Database | Microsoft Docs
description: "Naučte se migrovat databázi SQL serveru do Azure SQL Database."
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
ms.openlocfilehash: 375d3ea0230e7d3fd0fc02ca7e0b8a7a76c24a27
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="migrate-your-sql-server-database-to-azure-sql-database"></a>Migrovat databázi SQL serveru do Azure SQL Database

Přesunutí databázi SQL serveru do Azure SQL Database je proces tři části – nejdřív připravit, pak je exportovat a pak importování databáze. V tomto kurzu jste postup:

> [!div class="checklist"]
> * Příprava databáze v systému SQL Server pro migraci na Azure SQL Database pomocí [Data migrace pomocníka](https://www.microsoft.com/download/details.aspx?id=53595) (DMA).
> * Exportovat do souboru BACPAC souboru databáze
> * Souboru BACPAC soubor importovat do databáze SQL Azure

Pokud nemáte předplatné Azure, [vytvořit bezplatný účet](https://azure.microsoft.com/free/) před zahájením.

## <a name="prerequisites"></a>Požadavky

Předpokladem dokončení tohoto kurzu je splnění následujících požadavků:

- Nainstalovanou nejnovější verzi [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS). Instalace aplikace SSMS také nainstaluje nejnovější verzi SQLPackage, nástroje příkazového řádku, které je možné automatizovat řadu úloh vývoj databáze. 
- Nainstalována [asistent migrace dat](https://www.microsoft.com/download/details.aspx?id=53595) (DMA).
- Zjištění a mít přístup k databázi pro migraci. Tento kurz používá [SQL Server 2008 R2 databáze AdventureWorks OLTP](https://msftdbprodsamples.codeplex.com/releases/view/59211) v instanci systému SQL Server 2008 R2 nebo novější, ale můžete použít libovolnou databázi podle svého výběru. Chcete-li opravit problémy s kompatibilitou, použijte [SQL Server Data Tools](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)

## <a name="prepare-for-migration"></a>Příprava na migraci

Jste připraveni pro přípravu na migraci. Použijte následující postup použijte  **[Data migrace pomocníka](https://www.microsoft.com/download/details.aspx?id=53595)**  k vyhodnocení připravenosti databáze pro migraci do Azure SQL Database.

1. Otevřete **asistent migrace dat**. Přímý přístup do paměti lze spustit na libovolném počítači s připojením k instanci systému SQL Server obsahující databáze, která chcete migrovat, nemusíte jej nainstalovat na počítač, který je hostitelem instance systému SQL Server.

     ![Asistent migrace systému Open data](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-open.png)

2. V levé nabídce klikněte na tlačítko **+ nový** vytvořit **Assessment** projektu. Vyplňte formulář s **název projektu** (všechny ostatní hodnoty musí být ponechány na jejich výchozí hodnoty) a klikněte na tlačítko **vytvořit**. **Možnosti** otevře se stránka.

     ![Nový projekt asistenta migrace dat](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-new-project.png)

3. Na **možnosti** klikněte na tlačítko **Další**. **Vyberte zdroje** otevře se stránka.

     ![nové možnosti migrace dat](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-options.png) 

4. Na **vyberte zdroje** stránky, zadejte název instance systému SQL Server obsahující server, kterou chcete migrovat. V případě potřeby změňte jiné hodnoty na této stránce. Klikněte na **Připojit**.

     ![nové vyberte zdroje dat migrace](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-sources.png)

5. V **přidejte zdroje** část **vyberte zdroje** vyberte zaškrtávací políčka pro databáze, která má být testována pro kompatibilitu. Klikněte na tlačítko **Přidat**.

     ![nové vyberte zdroje dat migrace](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-sources-add.png)

6. Klikněte na tlačítko **spustit Assessment**.

     ![nové vyhodnocení zahájení migrace dat](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-start-assessment.png)

7. Po dokončení hodnocení první pohled, pokud je databáze dostatečně kompatibilní k migraci. Podívejte se na značku zaškrtnutí v zelená kruh.

     ![výsledky kompatibilní nové vyhodnocení migrace dat](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-assessment-results-compatible.png)

8. Zkontrolujte výsledky. **Parity funkcí systému SQL Server** výsledky zobrazeny jsou výsledky výchozí ke kontrole. Konkrétně zkontrolujte informace o nepodporované a částečně podporované funkce a zadané informace o doporučených akcí. 

     ![nové paritní assessment migrace dat](./media/sql-database-migrate-your-sql-server-database/data-migration-assistant-assessment-results-parity.png)

9. Zkontrolujte **problémy s kompatibilitou** kliknutím na tuto možnost v levé horní části. Konkrétně zkontrolujte informace o blokování migrace, změny chování a zastaralé funkce pro každou úroveň kompatibility. Pro databázi AdventureWorks2008R2 zkontrolujte změny Full-Text Search od SQL Server 2008 a změny SERVERPROPERTY('LCID') systémem SQL Server 2000. Podrobnosti o těchto změnách se poskytuje odkazy na další informace. Mnoho možnosti vyhledávání a změnili nastavení pro fulltextové vyhledávání 

   > [!IMPORTANT] 
   > Po dokončení migrace databáze do Azure SQL Database, můžete pracovat databázi na jeho aktuální úroveň kompatibility (úroveň 100 pro databázi AdventureWorks2008R2) nebo na vyšší úrovni. Další informace o důsledky a možnosti pro operační databázi na úrovni konkrétní kompatibility najdete v tématu [změnit úroveň kompatibility databáze](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level). Viz také [ALTER DATABASE OBOR CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) informace o další úroveň databáze nastavení související s úrovní kompatibility.
   >

10. Volitelně klikněte na **exportovat sestavu** pro uložení sestavy jako soubor ve formátu JSON.
11. Asistent migrace dat zavřete.

## <a name="export-to-bacpac-file"></a>Export souboru BACPAC souboru 

Soubor souboru BACPAC je soubor ZIP s příponou souboru BACPAC obsahující metadata a data z databáze systému SQL Server. Soubor souboru BACPAC lze uložit v úložišti objektů blob v Azure nebo v místní úložiště pro archivaci nebo migrace – například jako v systému SQL Server do Azure SQL Database. Pro export být stavu transakční konzistence, je nutné zajistit buď žádné zápisu aktivity dochází při exportu.

Použijte následující postup pomocí nástroje příkazového řádku SQLPackage se exportovat databázi AdventureWorks2008R2 do místního úložiště.

1. Otevřete příkazový řádek systému Windows a změňte adresáře na složku, ve které máte **130** verze SQLPackage – například **C:\Program Files (x86) \Microsoft SQL Server\130\DAC\bin**.

2. Spusťte následující příkaz SQLPackage příkazového řádku pro export **AdventureWorks2008R2** databáze z **localhost** k **AdventureWorks2008R2.bacpac**. Změna těchto hodnot v závislosti na vašem prostředí.

    ```SQLPackage
    sqlpackage.exe /Action:Export /ssn:localhost /sdn:AdventureWorks2008R2 /tf:AdventureWorks2008R2.bacpac
    ```

    ![sqlpackage export](./media/sql-database-migrate-your-sql-server-database/sqlpackage-export.png)

Po dokončení provádění generovaného souboru BACPAC soubor je uložen v adresáři, kde se nachází sqlpackage spustitelný soubor. V tomto příkladu C:\Program Files (x86) \Microsoft SQL Server\130\DAC\bin. 

## <a name="log-in-to-the-azure-portal"></a>Přihlášení k portálu Azure Portal

Přihlaste se k portálu [Azure Portal](https://portal.azure.com/). Přihlášení z počítače, ze kterého spouštíte nástroj příkazového řádku SQLPackage usnadňuje vytvoření pravidla brány firewall v kroku 5.

## <a name="create-a-sql-server-logical-server"></a>Vytvoření logického serveru SQL server

A [logickému serveru SQL server](sql-database-features.md) funguje jako centrální administrativní bod pro více databází. Postupujte podle těchto kroků k vytvoření logického serveru SQL server tak, aby obsahovala migrované databázi společnosti Adventure Works OLTP SQL Server. 

1. Klikněte na tlačítko **Nový** v levém horním rohu webu Azure Portal.

2. Typ **systému sql server** v okně hledání na **nový** a vyberte **databáze SQL (logický server)** ze seznamu Filtrované.

    ![výběr logického serveru](./media/sql-database-migrate-your-sql-server-database/logical-server.png)

3. Na **všechno, co** klikněte na tlačítko **systému SQL server (logický server)** a pak klikněte na **vytvořit** na **systému SQL Server (logický server)** stránky.

    ![vytvoření logického serveru](./media/sql-database-migrate-your-sql-server-database/logical-server-create.png)

4. Vyplňte formulář serveru SQL (logický server) pomocí následujících informací, jak je vidět na předchozím obrázku:     

   | Nastavení       | Navrhovaná hodnota | Popis | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Název serveru** | Zadejte všechny globálně jedinečného názvu | Platné názvy serverů najdete v tématu [Pravidla a omezení pojmenování](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). | 
   | **Přihlašovací jméno správce serveru** | Zadat libovolný platný název | Platná přihlašovací jména najdete v tématu [Identifikátory databází](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers). |
   | **Heslo** | Zadejte všechny platné heslo | Heslo musí mít aspoň 8 znaků a musí obsahovat znaky ze tří z následujících kategorií: velká písmena, malá písmena, číslice a jiné než alfanumerické znaky. |
   | **Předplatné** | Vyberte předplatné. | Podrobnosti o vašich předplatných najdete v tématu [Předplatná](https://account.windowsazure.com/Subscriptions). |
   | **Skupina prostředků** | Vyberte existující skupinu prostředků nebo vytvořte novou skupinu, například **myResourceGroup** |  Platné názvy skupin prostředků najdete v tématu [Pravidla a omezení pojmenování](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). |
   | **Umístění** | Zadejte všechny platné umístění pro nový server | Informace o oblastech najdete v tématu [Oblasti služeb Azure](https://azure.microsoft.com/regions/). |

   ![vytvoření logického serveru byla dokončena formuláře](./media/sql-database-migrate-your-sql-server-database/logical-server-create-completed.png)

5. Klikněte na tlačítko **vytvořit** ke zřízení logického serveru. Zřizování trvá několik minut. 

> [!IMPORTANT]
> Mějte na paměti, váš název serveru, serveru správce přihlašovací jméno a heslo. Je nutné tyto hodnoty později v tomto kurzu.
>

## <a name="create-a-server-level-firewall-rule"></a>Vytvoření pravidla brány firewall na úrovni serveru

Vytvoří služba SQL Database [brány firewall na úrovni serveru](sql-database-firewall-configure.md) , který brání externí aplikace a nástroje pro připojení k serveru nebo kterékoli databázi na serveru, pokud není vytvořená pravidla brány firewall pro otevření brány firewall pro konkrétní IP adresy. Postupujte podle těchto kroků k vytvoření pravidla brány firewall na úrovni serveru, databáze SQL pro IP adresu počítače, ze kterého spouštíte nástroj příkazového řádku SQLPackage. To umožňuje SQLPackage pro připojení k logickému serveru SQL serverDatabase přes bránu firewall databáze Azure SQL Database. 

1. Klikněte na tlačítko **všechny prostředky** z nabídky na levé straně a klikněte na nový server **všechny prostředky** stránky. Na stránce Přehled pro váš server otevře a poskytuje možnosti pro další konfiguraci.

     ![Přehled logický server](./media/sql-database-migrate-your-sql-server-database/logical-server-overview.png)

2. Klikněte na tlačítko **brány Firewall** v levé nabídce v části **nastavení** na stránce Přehled. Otevře se stránka **Nastavení brány firewall** pro server služby SQL Database. 

3. Klikněte na tlačítko **přidat IP adresu klienta** na panelu nástrojů přidat IP adresu počítače jsou aktuálně používá a potom klikněte na **Uložit**. Pravidlo brány firewall na úrovni serveru se vytvoří pro tuto IP adresu.

     ![nastavení pravidla brány firewall serveru](./media/sql-database-migrate-your-sql-server-database/server-firewall-rule-set.png)

4. Klikněte na **OK**.

Nyní můžete připojit pro všechny databáze na tomto serveru pomocí SQL Server Management Studio nebo jiný nástroj podle svého výběru z tuto IP adresu pomocí účtu správce serveru, který jste vytvořili dříve.

> [!NOTE]
> SQL Database komunikuje přes port 1433. Pokud se pokoušíte připojit z podnikové sítě, nemusí být odchozí provoz přes port 1433 bránou firewall vaší sítě povolený. Pokud je to tak, nebudete se moct připojit k serveru Azure SQL Database, dokud vaše IT oddělení neotevře port 1433.
>

## <a name="import-a-bacpac-file-to-azure-sql-database"></a>Importovat soubor souboru BACPAC do Azure SQL Database 

Nejnovější verze nástroje příkazového řádku SQLPackage poskytovat podporu pro vytvoření Azure SQL database v zadané [služby úroveň a úroveň výkonu](sql-database-service-tiers.md). Pro nejlepší výkon při importu vyberte vysokou úroveň služby a výkonu úroveň a pak vertikálně snížit kapacitu po importu Pokud úrovně služby a výkonu je vyšší, než je nutné okamžitě.

Použijte tyto kroky použijte nástroj příkazového řádku SQLPackage k importování databáze AdventureWorks2008R2 do Azure SQL Database. Když používáte SQL Server Management Studio pro tuto úlohu je SQLPackage upřednostňovanou metodou pro většinu produkčního prostředí pro maximální flexibilitu a nejlepší výkon. V tématu [migrace ze systému SQL Server do Azure SQL Database pomocí souboru BACPAC souborů](https://blogs.msdn.microsoft.com/sqlcat/2016/10/20/migrating-from-sql-server-to-azure-sql-database-using-bacpac-files/).

- Spusťte následující příkaz SQLPackage příkazového řádku k importu **AdventureWorks2008R2** databáze z místního úložiště k logickému serveru služby SQL server, který jste dříve vytvořili pro novou databázi, úroveň služby **Premium**a cíl služby z **P6**. Nahraďte hodnoty v lomených závorkách s příslušnými hodnotami pro logický server SQL server a zadejte název pro novou databázi (taky nahradit lomené závorky). Můžete také změnit hodnoty edici databáze a objectgive služby v závislosti na vašem prostředí. Pro účely tohoto kurzu migrované databázi se říká **myMigratedDatabase**.

    ```
    SqlPackage.exe /a:import /tcs:"Data Source=<your_server_name>.database.windows.net;Initial Catalog=<your_new_database_name>;User Id=<change_to_your_admin_user_account>;Password=<change_to_your_password>" /sf:AdventureWorks2008R2.bacpac /p:DatabaseEdition=Premium /p:DatabaseServiceObjective=P6
    ```

   ![sqlpackage import](./media/sql-database-migrate-your-sql-server-database/sqlpackage-import.png)

> [!IMPORTANT]
> Logickému serveru SQL server naslouchá na portu 1433. Pokud se pokoušíte připojit k serveru logickému serveru SQL z v rámci podnikové brány firewall, je třeba otevřít v podnikové brány firewall můžete úspěšně připojit tento port.
>

## <a name="connect-using-sql-server-management-studio-ssms"></a>Připojení pomocí SQL Server Management Studio (SSMS)

Použít SQL Server Management Studio k navázání připojení k serveru Azure SQL Database a nově migrovat databázi, nazývá **myMigratedDatabase** v tomto kurzu. Pokud používáte systém SQL Server Management Studio do jiného počítače, z níž jste spustili SQLPackage, vytvořte pravidlo brány firewall pro tento počítač pomocí kroků v předchozím postupu.

1. Otevřete SQL Server Management Studio.

2. V dialogovém okně **Připojení k serveru** zadejte následující informace:
   - **Typ serveru:** Zadejte Databázový stroj.
   - **Název serveru**: Zadejte název plně kvalifikovaný serveru, jako například **mynewserver20170403.database.windows.net**
   - **Ověřování:** Zadejte Ověřování SQL Serveru.
   - **Přihlášení:** Zadejte účet správce serveru.
   - **Heslo:** Zadejte heslo pro účet správce serveru.
 
    ![připojit pomocí ssms](./media/sql-database-migrate-your-sql-server-database/connect-ssms.png)

3. Klikněte na **Připojit**. Otevře se okno Průzkumník objektů. 

4. V Průzkumníku objektů rozbalte **databáze** a potom rozbalte **myMigratedDatabase** zobrazit objekty v ukázkové databázi.

## <a name="change-database-properties"></a>Změnit vlastnosti databáze

Můžete změnit úroveň služby, úroveň výkonu a úroveň kompatibility pomocí SQL Server Management Studio. Během fáze importu doporučujeme je importovat do databáze vyšší úroveň výkonu pro nejlepší výkon, ale po dokončení importu jak ušetřit peníze, dokud nebudete připraveni aktivně používá databázi importované škálování směrem dolů. Změna úrovně kompatibility mohou vést k vyšší výkon a přístup k nejnovější funkcím služby Azure SQL Database. Když provádíte migraci starší databázi, její úroveň kompatibility databáze je udržován na úrovni nejnižší podporovaná úroveň, který je kompatibilní s databází importována. Další informace najdete v tématu [vylepšuje výkon dotazů 130 úroveň kompatibility v Azure SQL Database](sql-database-compatibility-level-query-performance-130.md).

1. V Průzkumníku objektů, klikněte pravým tlačítkem na **myMigratedDatabase** a klikněte na tlačítko **nový dotaz**. Otevře se okno dotazu připojené k vaší databázi.

2. Spusťte následující příkaz pro danou vrstvu služeb **standardní** a na úroveň výkonu **S1**.

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

3. Spusťte následující příkaz, chcete-li změnit úroveň kompatibility databáze **130**.

    ```
    ALTER DATABASE myMigratedDatabase  
    SET COMPATIBILITY_LEVEL = 130;
    ```

   ![Změňte úroveň kompatibility](./media/sql-database-migrate-your-sql-server-database/compat-level.png)

## <a name="next-steps"></a>Další kroky 
V tomto kurzu připravená, exportovat a importovat vaší databáze. Jste se dozvěděli, do:

> [!div class="checklist"]
> * Příprava databáze v systému SQL Server pro migraci do Azure SQL Database
> * Exportovat do souboru BACPAC souboru databáze
> * Souboru BACPAC soubor importovat do databáze SQL Azure

Přechodu na v dalším kurzu se dozvíte, jak zabezpečit vaši databázi.

> [!div class="nextstepaction"]
> [Zabezpečení databáze Azure SQL](sql-database-security-tutorial.md).


