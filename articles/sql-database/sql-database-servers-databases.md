---
title: "Vytvořit a spravovat servery Azure SQL & databáze | Microsoft Docs"
description: "Informace o serveru Azure SQL Database a databázových koncepcí a o vytváření a správě serverů a databází pomocí portálu Azure, prostředí PowerShell, rozhraní příkazového řádku Azure, Transact-SQL a rozhraní REST API."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/05/2017
ms.author: carlrab
ms.openlocfilehash: ef61aa610957024d85f4231d957869858fd545c5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-manage-azure-sql-database-servers-and-databases"></a>Vytvářet a spravovat servery Azure SQL Database a databáze

Azure SQL database je spravovaný databáze v Microsoft Azure, který je vytvořen v rámci [skupina prostředků Azure](../azure-resource-manager/resource-group-overview.md) s definovanou sadu [výpočetní a úložnou kapacitu pro různé úlohy](sql-database-service-tiers.md). Azure SQL database je přidružen logického serveru Azure SQL Database, která je vytvořena v rámci konkrétní oblasti Azure. 

## <a name="an-azure-sql-database-can-be-a-single-pooled-or-partitioned-database"></a>Azure SQL database může být jeden, ve fondu nebo oddílů databáze

Azure SQL database může být:

- Izolovaná databáze s [vlastní sadou prostředků](sql-database-what-is-a-dtu.md#what-are-database-transaction-units-dtus) (DTU)
- Součástí [elastický fond SQL](sql-database-elastic-pool.md) , [sdílí sadu prostředků](sql-database-what-is-a-dtu.md#what-are-elastic-database-transaction-units-edtus) (Edtu)
- Součástí [sady horizontálně dělených databází s horizontálním navýšením kapacity](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling); tyto databáze můžou být buď izolované, nebo součástí fondu
- Součástí sady databází, které se podílejí na [vzoru návrhu SaaS pro více klientů](sql-database-design-patterns-multi-tenancy-saas-applications.md); tyto databáze můžou být buď izolované, nebo součástí fondu (nebo obojí) 

> [!TIP]
> Platné názvy databází najdete v tématu [Identifikátory databází](https://docs.microsoft.com/en-us/sql/relational-databases/databases/database-identifiers). 
>
 
- Výchozí databázová kolace, kterou služba Microsoft Azure SQL Database používá, je **SQL_LATIN1_GENERAL_CP1_CI_AS**, kde **LATIN1_GENERAL** znamená jazyk Angličtina (Spojené státy), **CP1** značí znakovou sadu 1252, **CI** znamená, že se nerozlišují malá a velká písmena, a **AS** značí rozlišování diakritiky. Další informace o nastavení kolace najdete v článku [COLLATE (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx).
- Microsoft Azure SQL Database podporuje tabular data stream (TDS) protokol klienta verze 7.3 nebo novější.
- Jsou povoleny pouze připojení TCP/IP.

## <a name="what-is-an-azure-sql-logical-server"></a>Co je logickému serveru Azure SQL?

Logický server funguje jako centrální administrativní bod pro více databází, včetně [elastické fondy SQL](sql-database-elastic-pool.md) [přihlášení](sql-database-manage-logins.md), [pravidla brány firewall](sql-database-firewall-configure.md), [auditování pravidla](sql-database-auditing.md), [hrozby zásady detekce](sql-database-threat-detection.md), a [převzetí služeb při selhání skupiny](sql-database-geo-replication-overview.md). Logický server může být v jiné oblasti než jeho skupin prostředků. Logický server musí existovat, abyste mohli vytvořit databázi Azure SQL. Všechny databáze na serveru jsou vytvořeny v rámci stejné oblasti jako logického serveru. 


> [!IMPORTANT]
> Ve službě SQL Database je server logická konstrukce, která se liší od instance SQL Serveru, kterou můžete znát z místního prostředí. Služba SQL Database zejména neposkytuje žádnou záruku ohledně umístění databází ve vztahu k jejich logickým serverům a nezveřejňuje žádné funkce ani možnosti přístupu na úrovni instance.
> 

Když vytvoříte logického serveru, zadat server přihlašovací účet a heslo, které má oprávnění správce pro hlavní databázi na tomto serveru a všechny databáze na tomto serveru vytvořit. Tento počáteční účet je účet SQL přihlášení. Azure SQL Database podporuje ověřování SQL a ověřování Azure Active Directory pro ověřování. Informace o ověřování a přihlášení najdete v tématu [Správa databází a přihlašovacích údajů ve službě Azure SQL Database](sql-database-manage-logins.md). Ověřování systému Windows se nepodporuje. 

> [!TIP]
> Názvy platný prostředků skupiny a serveru, najdete v části [pojmenování pravidla a omezení](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).
>

Logický server Azure Database:

- Je vytvořen v rámci předplatného Azure, ale je možné ho i s obsaženými prostředky přenést do jiného předplatného.
- Je nadřazeným prostředkem pro databáze, elastické fondy a datové sklady.
- Poskytuje obor názvů pro databáze elastické fondy a datových skladů
- Je logický kontejner s silné životnost sémantiku - odstranění serveru a odstraní databázích s omezením, elastické fondy a datových skladů
- Účastní se [řízení přístupu Azure na základě rolí (RBAC)](/active-directory/role-based-access-control-what-is) -databází, elastické fondy a datových skladů v rámci serveru zdědí oprávnění ze serveru
- Je element horní identity databází, elastické fondy a datových skladů pro prostředků Azure pro účely správy (viz schéma adresy URL pro databáze a fondy)
- Uspořádává prostředky v oblasti.
- Poskytuje koncový bod připojení pro přístup k databázi (<serverName>.database.windows.net).
- Poskytuje přístup k metadatům, která se vztahují k obsaženým prostředkům, přes zobrazení dynamických zpráv díky připojení k hlavní databázi. 
- Poskytuje oboru pro zásady správy, které platí pro její databáze - přihlášení, brány firewall, audit, hrozby detekce atd. 
- Je omezené na základě kvótu v rámci nadřazeného předplatného (šesti serverů na jedno předplatné, ve výchozím nastavení - [najdete v části předplatné omezuje zde](../azure-subscription-service-limits.md))
- Poskytuje obor pro kvóty databáze a kvóty DTU pro prostředky, které obsahuje (jako je například 45 000 DTU)
- Je v rozsahu správy verzí pro možnosti zapnuta obsažených prostředků 
- Hlavní přihlášení na úrovni serveru můžou spravovat všechny databáze na serveru.
- Může obsahovat přihlašovací údaje podobné těm, která se místně používají v instancích SQL Serveru, a udělit jim přístup k jedné nebo několika databázím na serveru spolu s omezenými právy pro správu. Další informace najdete v tématu [Přihlašovací údaje](sql-database-manage-logins.md).

## <a name="azure-sql-databases-protected-by-sql-database-firewall"></a>Chráněné bránou firewall databáze SQL Azure databáze SQL

K ochraně vašich dat [brány firewall SQL Database](sql-database-firewall-configure.md) zabraňuje veškerý přístup k databázovému serveru ani žádnou její databáze z mimo připojení k serveru přímo prostřednictvím připojení k předplatnému Azure. Chcete-li povolit další připojení, je potřeba [vytvořit jeden nebo více pravidel brány firewall](sql-database-firewall-configure.md#creating-and-managing-firewall-rules). Vytváření a Správa SQL elastické fondy najdete v tématu [elastické fondy](sql-database-elastic-pool.md).

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-azure-portal"></a>Spravovat servery Azure SQL, databáze a brány firewall pomocí portálu Azure

Můžete vytvořit skupinu prostředků Azure SQL database předem nebo při vytváření samotný server. Existuje více metod pro získání nového formuláře SQL serveru tak, že vytvoříte nový server SQL nebo jako součást vytvoření nové databáze. 

### <a name="create-a-blank-sql-server-logical-server"></a>Vytvořit prázdný SQL server (logický server)

K vytvoření serveru (bez databáze) Azure SQL Database pomocí [portál Azure](https://portal.azure.com), přejděte do prázdného formuláře SQL server (logický server). Následující snímek obrazovky ukazuje jednu metodu pro otevření formuláře k vytvoření prázdného logického serveru SQL. 

   ![vytvoření logického serveru byla dokončena formuláře](./media/sql-database-migrate-your-sql-server-database/logical-server-create-completed.png)

Pokud dojde k tomuto formuláři pomocí jiné metody, je stejný jako informace na formuláři.

### <a name="create-a-blank-or-sample-sql-database"></a>Vytvoření databáze SQL prázdné nebo ukázku

Chcete-li vytvořit databázi Azure SQL pomocí [portál Azure](https://portal.azure.com), přejděte do prázdného formuláře databáze SQL a zadejte požadované informace. Můžete vytvořit skupinu prostředků a logický server předem nebo při vytváření samotná databáze Azure SQL database. Můžete vytvořit prázdnou databázi nebo vytvoření ukázkové databáze založené na společnosti Adventure Works LT. 

  ![create database-1](./media/sql-database-get-started-portal/create-database-1.png)

> [DŮLEŽITÉ] Informace o výběru cenovou úroveň pro vaši databázi najdete v tématu [úrovních služeb](sql-database-service-tiers.md).
>

### <a name="manage-an-existing-sql-server"></a>Spravovat existující server SQL

Chcete-li spravovat existující server, přejděte na server s počtem metody – například od konkrétní stránky databáze SQL, **servery SQL** stránky, nebo **všechny prostředky** stránky. Následující snímek obrazovky ukazuje, jak začít, nastavení brány firewall úrovni serveru z **přehled** stránky pro server. 

   ![Přehled logický server](./media/sql-database-migrate-your-sql-server-database/logical-server-overview.png)

Chcete-li spravovat stávající databázi, přejděte na **databází SQL** a klikněte na tlačítko databáze, které chcete spravovat. Následující snímek obrazovky ukazuje, jak začít, nastavení brány firewall úrovni serveru pro databázi z **přehled** stránky pro databázi. 

   ![pravidlo brány firewall serveru](./media/sql-database-get-started-portal/server-firewall-rule.png) 

> [!IMPORTANT]
> Konfigurace vlastnosti výkonu pro databázi najdete v tématu [úrovních služeb](sql-database-service-tiers.md).
>

> [!TIP]
> Azure portálu rychlý úvodní kurz, najdete v části [vytvoření Azure SQL database na portálu Azure](sql-database-get-started-portal.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-powershell"></a>Spravovat servery Azure SQL, databáze a brány firewall pomocí prostředí PowerShell

Chcete-li vytvořit a spravovat server Azure SQL, databáze a brány firewall pomocí prostředí Azure PowerShell, použijte následující rutiny prostředí PowerShell. Pokud je potřeba nainstalovat nebo upgradovat prostředí PowerShell najdete v tématu [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps). Vytváření a Správa SQL elastické fondy najdete v tématu [elastické fondy](sql-database-elastic-pool.md).

| Rutina | Popis |
| --- | --- |
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Vytvoří databázi |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Získá jednu nebo více databází|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Nastaví vlastnosti pro databázi nebo přesune existující databáze do pružného fondu|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Odebere databáze|
|[Nový AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)|Vytvoří skupinu prostředků.]
|[Nový AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver)|Vytvoří serveru|
|[Get-AzureRmSqlServer](/powershell/module/azurerm.sql/get-azurermsqlserver)|Vrátí informace o serverech|
|[Set-AzureRmSqlServer](https://docs.microsoft.com/en-us/powershell/module/azurerm.sql/set-azurermsqlserver)|Upraví vlastnosti serveru|
|[Remove-AzureRmSqlServer](/powershell/module/azurerm.sql/remove-azurermsqlserver)|Odebere server|
|[New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule)|Vytvoří pravidlo brány firewall na úrovni serveru |
|[Get-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/get-azurermsqlserverfirewallrule)|Získá pravidla brány firewall pro server|
|[Set-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/set-azurermsqlserverfirewallrule)|Upraví pravidlo brány firewall na serveru|
|[Remove-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/remove-azurermsqlserverfirewallrule)|Odstraní pravidlo brány firewall ze serveru.|

> [!TIP]
> Rychlý úvodní kurz prostředí PowerShell, najdete v části [vytvářet izolované databáze Azure SQL pomocí prostředí PowerShell](sql-database-get-started-portal.md). Příklad skriptů prostředí PowerShell, najdete v části [použít PowerShell k vytvoření jedné databáze Azure SQL a nakonfigurujte pravidlo brány firewall](scripts/sql-database-create-and-configure-database-powershell.md) a [sledování a škálování jeden SQL databáze pomocí prostředí PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-azure-cli"></a>Spravovat servery Azure SQL, databáze a brány firewall pomocí rozhraní příkazového řádku Azure

Vytvoření a Správa serveru Azure SQL, databáze a brány firewall se [rozhraní příkazového řádku Azure](/cli/azure/overview), použijte následující [databáze SQL Azure CLI](/cli/azure/sql/db) příkazy. Rozhraní příkazového řádku můžete spustit v prohlížeči pomocí [Cloud Shellu](/azure/cloud-shell/overview) nebo [nainstalovat](/cli/azure/install-azure-cli) v systémech macOS, Linux nebo Windows. Vytváření a Správa SQL elastické fondy najdete v tématu [elastické fondy](sql-database-elastic-pool.md).

| Rutina | Popis |
| --- | --- |
|[Vytvoření az sql db](/cli/azure/sql/db#create) |Vytvoří databázi|
|[AZ sql db seznamu](/cli/azure/sql/db#list)|Zobrazí všechny databáze a datových skladů na serveru, nebo všechny databáze v elastickém fondu|
|[seznam edicí az sql db](/cli/azure/sql/db#list-editions)|Seznamy, které jsou k dispozici služby cíle a limity úložiště|
|[db sql az seznamu – použití](/cli/azure/sql/db#list-usages)|Vrátí databáze použití|
|[AZ sql db zobrazit](/cli/azure/sql/db#show)|Získá databáze nebo datového skladu|
|[aktualizace databáze sql az](/cli/azure/sql/db#update)|Aktualizuje databázi|
|[Odstranění databáze sql az](/cli/azure/sql/db#delete)|Odebere databáze|
|[Vytvoření skupiny az](/cli/azure/group#create)|Vytvoří skupinu prostředků.|
|[Vytvoření az sql serveru](/cli/azure/sql/server#create)|Vytvoří serveru|
|[seznam serverů sql az](/cli/azure/sql/server#list)|Vytvoří seznam serverů|
|[server sql az seznamu – použití](/cli/azure/sql/server#list-usages)|Vrátí použití serveru|
|[AZ sql serveru zobrazit](/cli/azure/sql/server#show)|Získá serveru|
|[aktualizace az sql server](/cli/azure/sql/server#update)|Aktualizace serveru|
|[Odstranění serveru sql az](/cli/azure/sql/server#delete)|Odstraní server|
|[Vytvoření brány firewall pravidlo az sql serveru](/cli/azure/sql/server/firewall-rule#create)|Vytvoří pravidlo brány firewall serveru|
|[seznam az sql serverů pravidlo brány firewall](/cli/azure/sql/server/firewall-rule#list)|Zobrazí seznam pravidel brány firewall na serveru|
|[Zobrazit pravidlo brány firewall serveru sql az](/cli/azure/sql/server/firewall-rule#show)|Zobrazí podrobnosti pravidla brány firewall|
|[aktualizace pravidla brány firewall az sql server](/cli/azure/sql/server/firewall-rule#update)|Aktualizace pravidla brány firewall|
|[Odstranit pravidlo brány firewall serveru sql az](/cli/azure/sql/server/firewall-rule#delete)|Odstraní pravidlo brány firewall|

> [!TIP]
> Rychlý úvodní kurz pro Azure CLI, najdete v části [vytvářet izolované databáze Azure SQL pomocí rozhraní příkazového řádku Azure](sql-database-get-started-cli.md). Příklad skriptů příkazového řádku Azure CLI, najdete v části [použití rozhraní příkazového řádku k vytvoření jedné databáze Azure SQL a nakonfigurujte pravidlo brány firewall](scripts/sql-database-create-and-configure-database-cli.md) a [použití rozhraní příkazového řádku pro sledování a škálování jedné databáze SQL](scripts/sql-database-monitor-and-scale-database-cli.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-transact-sql"></a>Spravovat servery Azure SQL, databáze a brány firewall pomocí jazyka Transact-SQL

Vytvoření a Správa serveru Azure SQL, databáze a brány firewall pomocí jazyka Transact-SQL, pomocí následujících příkazů T-SQL. Můžete použít tyto příkazy pomocí portálu Azure [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), nebo jiný program, který se může připojit k serveru Azure SQL Database a předat Transact-SQL příkazy. Pro správu SQL elastické fondy, najdete v části [elastické fondy](sql-database-elastic-pool.md).

> [!IMPORTANT]
> Nelze vytvořit nebo odstranit serveru pomocí jazyka Transact-SQL.
>

| Příkaz | Popis |
| --- | --- |
|[Vytvoření databáze (Azure SQL Database)](/sql/t-sql/statements/create-database-azure-sql-database)|Vytvoří novou databázi. Musíte být připojení k hlavní databázi a vytvořte novou databázi.|
| [Příkaz ALTER DATABASE (databáze Azure SQL)](/sql/t-sql/statements/alter-database-azure-sql-database) |Upravuje Azure SQL database. |
|[Příkaz ALTER databáze (Azure SQL Data Warehouse)](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse)|Upravuje datovým skladem Azure SQL.|
|[ODPOJENÍ databáze (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Odstraní databázi.|
|[Sys.database_service_objectives (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Vrátí edition (vrstva služby), cíl služby (cenové úrovně) a název elastického fondu, pokud existuje, pro databázi Azure SQL nebo Azure SQL Data Warehouse. Pokud přihlášení k hlavní databázi serveru Azure SQL Database, vrátí informace na všechny databáze. Pro Azure SQL Data Warehouse musí být připojen k hlavní databázi.|
|[Sys.dm_db_resource_stats (Azure SQL Database)](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)| Vrátí spotřeby procesoru, vstupně-výstupních operací a paměti pro databázi Azure SQL Database. Jeden řádek existuje pro každých 15 sekund, i když je v databázi žádná aktivita.|
|[Sys.resource_stats (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)|Vrátí data o využití a úložiště procesoru pro Azure SQL Database. Data jsou nasbírána a shrnuta v pěti minutách.|
|[Sys.database_connection_stats (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-connection-stats-azure-sql-database)|Obsahuje statistiku pro události připojení databáze SQL Database, umožní získat přehled o databáze připojení úspěchy a selhání. |
|[Sys.event_log (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-event-log-azure-sql-database)|Vrátí úspěšné připojení databáze Azure SQL Database, připojení selhání a zablokování. Tyto informace můžete sledovat a řešit potíže aktivitu vaší databáze SQL Database.|
|[příkaz sp_set_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database)|Vytvoří nebo aktualizuje nastavení brány firewall na úrovni serveru pro server služby SQL Database. Tuto uloženou proceduru lze pouze v hlavní databázi, a hlavní přihlášení úrovni serveru. Pravidlo brány firewall na úrovni serveru lze vytvořit pouze po vytvoření první pravidlo brány firewall na úrovni serveru uživatel oprávnění na úrovni Azure pomocí jazyka Transact-SQL|
|[Sys.firewall_rules (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database)|Vrací informace o nastavení brány firewall na úrovni serveru přidružené k vaší databázi SQL Azure Microsoft.|
|[sp_delete_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-delete-firewall-rule-azure-sql-database)|Nastavení brány firewall na úrovni serveru odebere server služby SQL Database. Tuto uloženou proceduru lze pouze v hlavní databázi, a hlavní přihlášení úrovni serveru.|
|[sp_set_database_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database)|Vytvoří nebo aktualizuje pravidla brány firewall na úrovni databáze Azure SQL Database nebo SQL Data Warehouse. Pravidla firewallu databáze lze konfigurovat pro hlavní databázi a pro uživatele databáze pro službu SQL Database. Pravidla firewallu databáze jsou užitečné, pokud pomocí obsažené uživatele databáze. |
|[Sys.database_firewall_rules (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database)|Vrací informace o nastavení brány firewall na úrovni databáze přidružené k vaší databázi SQL Azure Microsoft. |
|[sp_delete_database_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database)|Nastavení brány firewall na úrovni databáze odebere z Azure SQL Database nebo SQL Data Warehouse. |


> [!TIP]
> Rychlý úvodní kurz pomocí SQL Server Management Studio v systému Windows, najdete v části [Azure SQL Database: pomocí SQL Server Management Studio k připojení a dotazování dat](sql-database-connect-query-ssms.md). Rychlý úvodní kurz pomocí kódu v jazyce Visual Studio v systému macOS, Linux nebo Windows, najdete v části [Azure SQL Database: použití Visual Studio Code k připojení a dotazování dat](sql-database-connect-query-vscode.md).

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-the-rest-api"></a>Spravovat servery Azure SQL, databáze a brány firewall pomocí rozhraní REST API

Vytvořit a spravovat server Azure SQL, databáze a brány firewall pomocí rozhraní REST API najdete v tématu [Azure SQL Database REST API](/rest/api/sql/).

## <a name="next-steps"></a>Další kroky

- Další informace o sdružování databází pomocí SQL elastické fondy najdete v tématu [elastické fondy](sql-database-elastic-pool.md).
- Informace o službě Azure SQL Database, najdete v článku [co je SQL Database?](sql-database-technical-overview.md).
- Další informace o migraci databáze SQL serveru do Azure najdete v tématu [migrací do Azure SQL Database](sql-database-cloud-migrate.md).
- Informace o podporovaných funkcích najdete v tématu [Funkce](sql-database-features.md).
