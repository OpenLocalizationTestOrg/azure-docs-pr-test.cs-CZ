---
title: "Úrovně služby a výkonu Azure SQL Database | Microsoft Docs"
description: "Porovnání úrovně služeb SQL Database a úrovně výkonu pro izolované databáze a způsobit elastické fondy SQL"
keywords: "možnosti databáze, výkon databáze"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: f5c5c596-cd1e-451f-92a7-b70d4916e974
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 06/30/2017
ms.author: carlrab
ms.openlocfilehash: b25ff5331f119efd44c61808f7d1d5decb226bd6
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="what-performance-options-are-available-for-an-azure-sql-database"></a>Jaké možnosti výkonu jsou k dispozici pro databáze SQL Azure

[Azure SQL Database](sql-database-technical-overview.md) nabízí čtyři úrovně služeb pro obě jedním a [ve fondu](sql-database-elastic-pool.md) databáze. Tyto úrovně služeb jsou: **základní**, **standardní**, **Premium**, a **Premium RS**. V každé úrovni služeb jsou různé úrovně výkonu ([Dtu](sql-database-what-is-a-dtu.md)) a možnosti úložiště pro zpracování různých velikostech úlohy a data. Vyšší úroveň výkonu poskytuje další výpočetní a úložnou kapacitu navržený tak, aby stále vyšší propustnost a kapacity. Můžete změnit úrovně služeb, úrovně výkonu a úložiště dynamicky bez výpadků. 
- **Základní**, **standardní** a **Premium** mít úrovně služeb všechny dostupnost SLA 99.99 %, flexibilní možnosti kontinuity podnikových, funkce zabezpečení a fakturaci po hodinách. 
- **Premium RS** úroveň nabízí stejné úrovně výkonu jako úroveň Premium s omezenou SLA, protože je spuštěna s menší počet redundantní kopie než databázi v jiných úrovně služeb. Ano v případě selhání služby, musíte obnovit databázi ze zálohy s až 5 minut prodleva.

> [!IMPORTANT]
> Azure SQL database získá zaručenou sadu prostředků a vlastnosti očekávaný výkon databáze nemá vliv jiné databáze v rámci Azure. 

## <a name="choosing-a-service-tier"></a>Výběr úrovně služeb
Následující tabulka obsahuje příklady úrovní služeb vhodných pro různé zátěže a aplikace.

| Úroveň služeb | Cílová zátěž |
| :--- | --- |
| **Basic** | Nejvhodnější pro malé databáze provádějící obvykle jednu aktivní operaci najednou. Patří sem například databáze používané pro vývoj a testování nebo méně rozsáhlé a zřídka používané aplikace. |
| **Standard** |Základní možnost pro cloudové aplikace s nízkými až středními požadavky na výkon V/V. Podporuje více současných dotazů. Příkladem mohou být webové aplikace nebo aplikace pro pracovní týmy. |
| **Premium** | Tato varianta je určená pro vysoké objemy transakcí s vysokými požadavky na výkon V/V. Podporuje více souběžných uživatelů. Příkladem mohou být databáze podporující kritické podnikové procesy. |
| **Premium RS** | Určená pro úlohy náročné na vstupně-výstupní operace, které nevyžadují, že zaručuje nejvyšší dostupnost. Mezi příklady patří testování vysoce výkonné úlohy, nebo analytické úlohy, kde databáze není systému záznamu. |
|||

Izolovaných databází můžete vytvořit pomocí vyhrazených prostředcích v rámci vrstvu služby na konkrétní [úroveň výkonu](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) nebo můžete také vytvořit databází v rámci [elastický fond SQL](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus). V elastický fond SQL výpočetní a úložnou kapacitu jsou sdíleny více databází v rámci jednoho logického serveru. 

Prostředky, které jsou k dispozici pro izolované databáze jsou vyjádřeny z hlediska jednotky transakcí databáze (Dtu) a prostředky pro elastické fondy SQL jsou vyjádřeny z hlediska jednotky transakcí elastické databáze (Edtu). Další informace o Dtu a Edtu najdete v tématu [co jsou Dtu a Edtu?](sql-database-what-is-a-dtu.md).

Při rozhodování o úrovni služeb začněte tak, že určíte minimální databázové funkce, které budete potřebovat:

| **Funkce úrovně služby** | **Basic** | **Standard** | **Premium** | **Premium RS**|
| :-- | --: | --: | --: | --: |
| Velikost maximální jedné databáze | 2 GB | 250 GB | 4 TB *  | 500 GB  |
| Elastický fond maximální velikost | 156 GB | 2,9 TB | 4 TB * | 750 GB |
| Maximální velikost databáze v elastickém fondu | 2 GB | 250 GB | 500 GB | 500 GB |
| Maximální počet databází na každý fond | 500  | 500 | 100 | 100 |
| Maximální počet jednotek Dtu jedné databáze | 5 | 100 | 4000 | 1000 |
| Maximální počet jednotek Dtu na databázi v elastickém fondu | 5 | 3000 | 4000 | 1000 |
| Doba uchovávání záloh databáze | 7 dní | 35 dní | 35 dní | 35 dní |
||||||

> [!IMPORTANT]
> Úložiště až 4 TB je nyní k dispozici v následujících oblastech: nám East2, západní USA, nám verze pro státní správu Virginia, západní Evropa, Německo centrální, Jižní východní Asie, Japonsko – východ, Austrálie – východ, Kanada centrální a Východní Kanada. V tématu [omezení aktuální 4 TB](sql-database-service-tiers.md#current-limitations-of-p11-and-p15-databases-with-4-tb-maxsize)
>

Jakmile zjistíte, vrstvě příslušnou službu, jste připraveni k určení úrovně výkonu (počet jednotek Dtu) a velikost úložiště pro databázi. 

- Výkon S2 a S3 úrovně v **standardní** vrstvy jsou často dobrý výchozí bod. 
- Pro databáze s vysoké využití procesoru nebo vstupně-výstupní požadavky úrovně výkonu v **Premium** vrstvy jsou právo výchozí bod. 
- **Premium** vrstvy nabízí další procesoru a začínající hodnotou 10 x více vstupně-výstupní operace ve srovnání s nejvyšší výkon v **standardní** vrstvy.
- **PremiumRS** úroveň nabízí výkon **Premium** vrstvy s nižšími náklady, ale s omezenou SLA.

> [!IMPORTANT]
> Zkontrolujte [SQL elastické fondy](sql-database-elastic-pool.md) téma podrobné informace o seskupování databáze do SQL elastické fondy sdílet výpočetní a úložnou kapacitu. Zbývající část tohoto tématu se zaměřuje na úrovně služeb a úrovně výkonu pro izolované databáze.
>

## <a name="single-database-service-tiers-and-performance-levels"></a>Úrovně služeb a úrovně výkonu pro izolované databáze
Pro izolované databáze jsou více úrovně výkonu a úložiště objemy v každé úrovni služeb. 

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

## <a name="scaling-up-or-scaling-down-a-single-database"></a>Vertikální navýšení nebo snížení kapacity izolované databáze

Po počátečním výběru úrovně služeb a úrovně výkonu můžete dynamicky vertikálně navyšovat nebo snižovat kapacitu izolované databáze na základě aktuálních zkušeností.  

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

Se změnou úrovně služeb nebo úrovně výkonu databáze se vytvoří replika původní databáze na nové úrovni výkonu a následně se přepnou připojení na repliku. Během tohoto procesu se neztratí žádná data, ale během krátké chvíle, kdy se přepíná na repliku, jsou zakázána připojení k databázi, takže může dojít k vrácení některých probíhajících transakcí zpět. Doba pro přepínač přes se liší, ale je obecně méně než 30 sekundách 99 % času je v části 4 sekundy. Pokud máte velké množství transakcí pohybující se v okamžiku připojení jsou zakázané, může být delší dobu pro přepínač přes.  

Délka trvání celého procesu vertikálního navyšování kapacity závisí na velikosti a úrovni služeb databáze před změnou a po ní. Například změna databáze o velikosti 250 GB na, z nebo v rámci úrovně služeb Standard, by se měla dokončit během 6 hodin. V případě databáze o stejné velikosti, u které se mění úrovně výkonu v rámci úrovně služeb Premium, by se změna měla dokončit během 3 hodin.

> [!TIP]
> Pokud chcete zkontrolovat stav probíhající operace škálování databáze SQL, můžete použít následující dotaz: ```select * from sys.dm_operation_status```.
>

* Pokud provádíte upgrade na vyšší úroveň výkonu a vrstvu služby, maximální velikost databáze se nezvyšuje, pokud nezadáte explicitně větší maximální velikost.
* Přejít na starší verzi databáze, musí být menší než povolená maximální velikost vrstvy služby s cílové databáze. 
* Při upgradování databáze s [geografická replikace](sql-database-geo-replication-portal.md) povoleno, upgradujte její sekundární databáze úrovně požadovaný výkon před upgradem primární databáze (Obecné pokyny). Při upgradu na jinou edici ugrading sekundární databázi nejprve je vyžadováno. 
* Když přechod na starší verzi databáze s [geografická replikace](sql-database-geo-replication-portal.md) povoleno, starší verzi její primární databáze k vrstvě požadovaný výkon před snížením sekundární databáze (Obecné pokyny). Když přechod na starší verzi na jinou edici nejprve Downgrade primární databáze je povinný. 

* Nabídky služeb pro obnovení se u různých úrovní služeb liší. Pokud jsou Downgrade k **základní** vrstvy, bude mít nižší dobu uchovávání záloh - najdete v části [zálohy databáze SQL Azure](sql-database-automated-backups.md).
* Nové vlastnosti databáze se nepoužijí, dokud nebudou změny dokončeny.


## <a name="current-limitations-of-p11-and-p15-databases-with-4-tb-maxsize"></a>Aktuální omezení P11 a P15 databází s maxsize 4 TB

Maximální velikost 4 TB P11 a P15 databáze je podporována v některé oblasti (dřív popsané). Následující požadavky a omezení platí pro databáze P11 a P15 s maxsize 4 TB:

- Pokud si zvolíte možnost maxsize 4 TB, když se vytváří databáze (pomocí hodnotu 4 TB nebo 4096 GB), provedení příkazu pro vytvoření selže s chybou, pokud databáze je zajištěna v nepodporované oblast.
- Pro existující P11 a P15 databáze nachází v jedné z podporovaných oblastí můžete zvýšit maxsize úložiště 4 TB. To lze ověřit pomocí [vyberte DATABASEPROPERTYEX](https://msdn.microsoft.com/library/ms186823.aspx) nebo zkontrolováním velikost databáze na portálu Azure. Upgrade na existující P11 nebo P15 databáze lze provést pouze pomocí hlavního přihlášení na úrovni serveru nebo pomocí členové databázové role dbmanager. 
- Pokud se operace upgradu se spustí v podporované oblasti konfigurace aktualizoval okamžitě. Během procesu upgradu zůstane online databáze. Nelze však využívat úplné 4 TB úložiště, dokud se neupgradují skutečné databázové soubory do nové maxsize. Délka čas potřebný závisí na velikosti databáze během upgradu.  
- Při vytváření nebo aktualizaci P11 nebo P15 databáze, můžete zvolit pouze 1 TB a 4 TB maxsize. Velikosti zprostředkující úložiště nejsou aktuálně podporovány. Při vytváření P11/P15, je předem vybranou výchozí možnost úložiště o velikosti 1 TB. Pro databáze nachází v jedné z podporovaných oblastí můžete zvýšit maximální úložiště 4 TB pro jednu databázi nové nebo existující. Všechny ostatní země nelze zvýšit maximální velikost, vyšší než 1 TB. Ceny nezmění, když vyberete 4 TB součástí úložiště.
- Maxsize 4 TB databáze nelze změnit na 1 TB, i když skutečné úložiště používané je menší než 1 TB. Proto není možné snížit P11 - 4 TB nebo P15 - 4 TB P11 až 1 TB nebo P15 - 1 TB nebo nižší úroveň výkonu například k P1 P6) dokud poskytujeme možnosti dodatečné úložiště pro zbytek úrovně výkonu. Toto omezení platí také pro obnovení a scénáře kopírováním včetně bodu v čase, geografické obnovení, dlouhodobý-dlouhodobé zálohování uchovávání a kopie databáze. Jakmile databáze je nastavena možnost 4 TB, je potřeba spustit všechny operace obnovení této databáze do P11 nebo P15 s 4 TB maxsize.
- Při vytváření nebo upgradu databázi P11/P15 v nepodporované oblasti, operace vytvoření nebo aktualizace, které se nezdaří se následující chybová zpráva: **P11 a P15 databáze s až 4 TB úložiště jsou k dispozici v nám East2, západ USA, Virginia nám verze pro státní správu, – západ Evropa, střední Německo, jihovýchodní Asie, Japonsko – východ, Austrálie – východ, Střední Kanada a Východní Kanada.**
- Pro aktivní geografickou replikací scénáře:
   - Nastavení vztahu geografická replikace: Pokud primární databáze je P11 nebo P15, secondary(ies) musí být také P11 nebo P15; nižší úrovně výkonu byly zamítnuty jako sekundární databáze, protože nejsou podporovat 4 TB.
   - Upgrade z primární databáze ve vztahu geografická replikace: Změna maxsize 4 TB v primární databázi aktivuje stejné změny v sekundární databázi. Obě upgrady musí být úspěšná změna na primárním vstoupily v platnost. Platí oblast omezení pro možnost 4TB (viz výše). Je-li sekundární v oblasti, která nepodporuje 4 TB, není aktualizován primární.
- Použití službu Import/Export načítání P11 až 4TB nebo P15 - 4TB databáze není podporováno. Použít SqlPackage.exe k [importovat](sql-database-import.md) a [exportovat](sql-database-export.md) data.

## <a name="manage-single-database-service-tiers-and-performance-levels-using-the-azure-portal"></a>Spravovat úrovně služeb izolovaných databází a úrovně výkonu pomocí portálu Azure

Chcete-li nastavit nebo změnit úroveň služby, úroveň výkonu nebo množství úložiště pro nový nebo existující databázi Azure SQL pomocí portálu Azure, otevřete **konfigurace výkonu** okno pro vaši databázi kliknutím **cenová úroveň ( škálování Dtu)** – jak je znázorněno na následujícím snímku obrazovky. 

- Nastavení nebo změna vrstvy služby výběrem vrstvy služeb pro vaši úlohu. 
- Nastavit nebo změnit úroveň výkonu (**Dtu**) v rámci vrstvy služby pomocí **DTU** posuvníku.
- Nastavit nebo změnit velikost úložiště pro úrovně výkonu pomocí **úložiště** posuvníku. 

  ![Konfigurace úrovně služeb a výkonu](./media/sql-database-service-tiers/service-tier-performance-level.png)

> [!IMPORTANT]
> Zkontrolujte [aktuální omezení P11 a P15 databází s 4 TB maxsize](sql-database-service-tiers.md#current-limitations-of-p11-and-p15-databases-with-4-tb-maxsize) při výběru P11 nebo P15 vrstvy služeb.
>

## <a name="manage-single-database-service-tiers-and-performance-levels-using-powershell"></a>Spravovat úrovně služeb izolovaných databází a úrovně výkonu pomocí prostředí PowerShell

Chcete-li nastavit nebo změnit úrovních služby databáze Azure SQL, úrovně výkonu a množství úložiště pomocí prostředí PowerShell, použijte následující rutiny prostředí PowerShell. Pokud je potřeba nainstalovat nebo upgradovat prostředí PowerShell najdete v tématu [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps). 

| Rutina | Popis |
| --- | --- |
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Vytvoří databázi |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Získá jednu nebo více databází|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Nastaví vlastnosti pro databázi nebo přesune existující databáze do pružného fondu|


> [!TIP]
> Ukázkový skript prostředí PowerShell, který monitoruje metriky výkonu databáze, škálovatelná pro vyšší úroveň výkonu a vytvoří pravidlo výstrahy na jednom metrik výkonu, najdete v části [sledování a škálování jeden SQL databáze pomocí prostředí PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md).

## <a name="manage-single-database-service-tiers-and-performance-levels-using-the-azure-cli"></a>Spravovat úrovně služeb izolovaných databází a úrovně výkonu pomocí rozhraní příkazového řádku Azure

Nastavit nebo změnit databáze Azure SQL úrovně služeb, úrovně výkonu a množství úložiště pomocí rozhraní příkazového řádku Azure používat následující [databáze SQL Azure CLI](/cli/azure/sql/db) příkazy. Rozhraní příkazového řádku můžete spustit v prohlížeči pomocí [Cloud Shellu](/azure/cloud-shell/overview) nebo [nainstalovat](/cli/azure/install-azure-cli) v systémech macOS, Linux nebo Windows. Vytváření a Správa SQL elastické fondy najdete v tématu [elastické fondy](sql-database-elastic-pool.md).

| Rutina | Popis |
| --- | --- |
|[Vytvoření az sql db](/cli/azure/sql/db#create) |Vytvoří databázi|
|[AZ sql db seznamu](/cli/azure/sql/db#list)|Zobrazí všechny databáze a datových skladů na serveru, nebo všechny databáze v elastickém fondu|
|[seznam edicí az sql db](/cli/azure/sql/db#list-editions)|Seznamy, které jsou k dispozici služby cíle a limity úložiště|
|[db sql az seznamu – použití](/cli/azure/sql/db#list-usages)|Vrátí databáze použití|
|[AZ sql db zobrazit](/cli/azure/sql/db#show)|Získá databáze nebo datového skladu|
|[aktualizace databáze sql az](/cli/azure/sql/db#update)|Aktualizuje databázi|

> [!TIP]
> Ukázkový skript příkazového řádku Azure CLI, která je škálovatelná jedné databáze Azure SQL na úroveň výkonu různých po dotaz na informace o velikosti databáze, najdete v části [použití rozhraní příkazového řádku pro sledování a škálování jedné databáze SQL](scripts/sql-database-monitor-and-scale-database-cli.md).
>

## <a name="manage-single-database-service-tiers-and-performance-levels-using-transact-sql"></a>Spravovat úrovně služeb izolovaných databází a úrovně výkonu pomocí jazyka Transact-SQL

Nastavení nebo změna úrovně služeb databáze Azure SQL, úrovně výkonu a množství úložiště pomocí jazyka Transact-SQL, pomocí následujících příkazů T-SQL. Můžete použít tyto příkazy pomocí portálu Azure [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), nebo jiný program, který se může připojit k serveru Azure SQL Database a předat Transact-SQL příkazy. 

| Příkaz | Popis |
| --- | --- |
|[Vytvoření databáze (Azure SQL Database)](/sql/t-sql/statements/create-database-azure-sql-database)|Vytvoří novou databázi. Musíte být připojení k hlavní databázi a vytvořte novou databázi.|
| [Příkaz ALTER DATABASE (databáze Azure SQL)](/sql/t-sql/statements/alter-database-azure-sql-database) |Upravuje Azure SQL database. |
|[Sys.database_service_objectives (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Vrátí edition (vrstva služby), cíl služby (cenové úrovně) a název elastického fondu, pokud existuje, pro databázi Azure SQL nebo Azure SQL Data Warehouse. Pokud přihlášení k hlavní databázi serveru Azure SQL Database, vrátí informace na všechny databáze. Pro Azure SQL Data Warehouse musí být připojen k hlavní databázi.|
|[Sys.database_usage (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-usage-azure-sql-database)|Uvádí počet, typ a doba trvání databází na serveru Azure SQL Database.|

Následující příklad ukazuje maxsize se změnit pomocí příkazu ALTER DATABASE:

 ```sql
ALTER DATABASE <myDatabaseName> 
   MODIFY (MAXSIZE = 4096 GB);
```

## <a name="manage-single-databases-using-the-rest-api"></a>Spravovat jedné databáze pomocí rozhraní REST API

Nastavení nebo změna úrovně služeb databáze Azure SQL, úrovně výkonu a množství úložiště pomocí rozhraní REST API, najdete v části [Azure SQL Database REST API](/rest/api/sql/).

## <a name="next-steps"></a>Další kroky

* Další informace o [Dtu](sql-database-what-is-a-dtu.md).
* Další informace o sledování využití v jednotkách DTU najdete v tématu [monitorování a optimalizace výkonu](sql-database-troubleshoot-performance.md).

