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
ms.openlocfilehash: b27b78788614d32e6c0c4267fe0ce504ebd2f444
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="what-performance-options-are-available-for-an-azure-sql-database"></a>Jaké možnosti výkonu jsou k dispozici pro databáze SQL Azure

[Azure SQL Database](sql-database-technical-overview.md) nabízí čtyři úrovně služeb pro obě jedním a [ve fondu](sql-database-elastic-pool.md) databáze. Tyto úrovně služeb jsou: **základní**, **standardní**, **Premium**, a **Premium RS**. V každé úrovni služeb jsou různé úrovně výkonu ([Dtu](sql-database-what-is-a-dtu.md)) a možnosti úložiště toohandle různé úlohy a data velikosti. Vyšší úrovně výkonu zadejte další výpočetní a prostředky úložiště určené toodeliver stále vyšší výkon a kapacitu. Můžete změnit úrovně služeb, úrovně výkonu a úložiště dynamicky bez výpadků. 
- **Základní**, **standardní** a **Premium** mít úrovně služeb všechny dostupnost SLA 99.99 %, flexibilní možnosti kontinuity podnikových, funkce zabezpečení a fakturaci po hodinách. 
- Hello **Premium RS** vrstvy poskytuje hello stejné úrovně výkonu hello úroveň Premium s omezenou SLA vzhledem k tomu, že je spuštěna s menší počet redundantní kopie než databáze v hello jiné úrovně služeb. Ano v případě hello selhání služby, může být nutné toorecover databáze ze zálohy s až 5 minut prodleva tooa.

> [!IMPORTANT]
> Získá zaručenou sadu prostředků Azure SQL database a hello očekávat, že výkonové charakteristiky databáze nemá vliv jiné databáze v rámci Azure. 

## <a name="choosing-a-service-tier"></a>Výběr úrovně služeb
Hello následující tabulka obsahuje příklady hello úrovní služeb vhodné pro různé zátěže a aplikace.

| Úroveň služeb | Cílová zátěž |
| :--- | --- |
| **Basic** | Nejvhodnější pro malé databáze provádějící obvykle jednu aktivní operaci najednou. Patří sem například databáze používané pro vývoj a testování nebo méně rozsáhlé a zřídka používané aplikace. |
| **Standard** |Hello přejděte toooption pro cloudové aplikace s nízkou toomedium požadavky na výkon vstupně-výstupní operace, podpora více souběžných dotazů. Příkladem mohou být webové aplikace nebo aplikace pro pracovní týmy. |
| **Premium** | Tato varianta je určená pro vysoké objemy transakcí s vysokými požadavky na výkon V/V. Podporuje více souběžných uživatelů. Příkladem mohou být databáze podporující kritické podnikové procesy. |
| **Premium RS** | Určená pro úlohy náročné na vstupně-výstupní operace, které nevyžadují nejvyšší záruky dostupnosti hello. Mezi příklady patří testování vysoce výkonné úlohy, nebo analytické úlohy, kde hello databáze není hello systému záznamu. |
|||

Izolovaných databází můžete vytvořit pomocí vyhrazených prostředcích v rámci vrstvu služby na konkrétní [úroveň výkonu](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) nebo můžete také vytvořit databází v rámci [elastický fond SQL](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus). V elastický fond SQL hello výpočetní a úložnou kapacitu jsou sdíleny více databází v rámci jednoho logického serveru. 

Prostředky, které jsou k dispozici pro izolované databáze jsou vyjádřeny z hlediska jednotky transakcí databáze (Dtu) a prostředky pro elastické fondy SQL jsou vyjádřeny z hlediska jednotky transakcí elastické databáze (Edtu). Další informace o Dtu a Edtu najdete v tématu [co jsou Dtu a Edtu?](sql-database-what-is-a-dtu.md).

toodecide na vrstvu služby spusťte tak, že určíte hello minimální databázové funkce, které budete potřebovat:

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
> Je aktuálně k dispozici v následujících oblastech hello úložiště až too4 TB: nám East2, západní USA, nám verze pro státní správu Virginia, západní Evropa, Německo centrální, Jižní východní Asie, Japonsko – východ, Austrálie – východ, Kanada centrální a Východní Kanada. V tématu [omezení aktuální 4 TB](sql-database-service-tiers.md#current-limitations-of-p11-and-p15-databases-with-4-tb-maxsize)
>

Jakmile zjistíte hello příslušnou službu vrstvy, jsou připravené toodetermine úroveň výkonu hello (hello počet jednotek Dtu) a velikost úložiště hello hello databáze. 

- Hello S2 a S3 úrovně výkonu v hello **standardní** vrstvy jsou často dobrý výchozí bod. 
- Pro databáze s požadavky na vysoké využití procesoru nebo vstupně-výstupní operace hello úrovně výkonu v hello **Premium** vrstvy jsou hello správné počáteční bod. 
- Hello **Premium** úroveň nabízí další procesoru a spustí 10 x více vstupně-výstupní operace porovnání toohello nejvyšší úroveň výkonu v hello **standardní** vrstvy.
- Hello **PremiumRS** úroveň nabízí hello výkon hello **Premium** vrstvy s nižšími náklady, ale s omezenou SLA.

> [!IMPORTANT]
> Zkontrolujte hello [SQL elastické fondy](sql-database-elastic-pool.md) tématu hello podrobnosti o seskupování databází do elastického SQL fondy tooshare výpočetní a úložnou kapacitu. Hello zbývající část tohoto tématu se zaměřuje na úrovně služeb a úrovně výkonu pro izolované databáze.
>

## <a name="single-database-service-tiers-and-performance-levels"></a>Úrovně služeb a úrovně výkonu pro izolované databáze
Pro izolované databáze jsou více úrovně výkonu a úložiště objemy v každé úrovni služeb. 

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

## <a name="scaling-up-or-scaling-down-a-single-database"></a>Vertikální navýšení nebo snížení kapacity izolované databáze

Po počátečním výběru úrovně služeb a úrovně výkonu můžete dynamicky vertikálně navyšovat nebo snižovat kapacitu izolované databáze na základě aktuálních zkušeností.  

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

Změna úrovně vrstvy a výkonu služby hello databáze vytvoří repliku hello původní databáze na novou úroveň výkonu hello a pak přepne toohello repliky připojení. Během tohoto procesu je ztracena žádná data, ale během okamžiku stručný hello, když jsme přepínač přes toohello repliky, jsou zakázány připojení toohello databáze, takže některé transakce na cestě může být vrácena zpět. Hello dlouho hello přepněte se liší, ale je obecně méně než 30 sekund 99 % času hello je v části 4 sekundy. Pokud máte velké množství transakcí pohybující se v okamžiku připojení hello jsou zakázané, hello dlouho hello přepněte může být delší.  

Délka Hello hello celý proces škálování závisí na obou hello velikost a úroveň služby hello databáze před a po změně hello. Například změna databáze o velikosti 250 GB na, z nebo v rámci úrovně služeb Standard, by se měla dokončit během 6 hodin. Pro databázi hello stejná velikost tedy Změna úrovně výkonu v rámci hello úroveň služeb Premium, by se měla dokončit v rámci 3 hodiny.

> [!TIP]
> toocheck na dobrý stav probíhající operace škálování databáze SQL, můžete použít následující dotaz hello: ```select * from sys.dm_operation_status```.
>

* Pokud provádíte upgrade tooa vyšší výkon nebo vrstvě úroveň služby, hello maximální velikost databáze se nezvyšuje, pokud je explicitně zadat větší maximální velikost.
* toodowngrade databáze, musí být menší než maximální povolená velikost hello vrstvy služby s hello cílové databáze hello. 
* Při upgradování databáze s [geografická replikace](sql-database-geo-replication-portal.md) povoleno, upgrade touto vrstvou požadovaný výkon toohello sekundární databází před upgradem primární databáze hello (Obecné pokyny). Při upgradu tooa jinou edici ugrading hello sekundární nejprve je vyžadována databáze. 
* Když přechod na starší verzi databáze s [geografická replikace](sql-database-geo-replication-portal.md) povoleno, starší verzi jeho primární databází toohello požadovaný výkon vrstvy před snížením hello sekundární databáze (Obecné pokyny). Když přechod na starší verzi tooa jinou edici přechod na starší verzi hello primární nejprve je vyžadována databáze. 

* Hello obnovení nabídky služeb jsou pro jiné hello různé úrovně služeb. Pokud jsou Downgrade toohello **základní** vrstvy, bude mít nižší dobu uchovávání záloh - najdete v části [zálohy databáze SQL Azure](sql-database-automated-backups.md).
* Vlastnosti nového Hello hello databáze se nepoužívají, dokud nebudou dokončeny změny hello.


## <a name="current-limitations-of-p11-and-p15-databases-with-4-tb-maxsize"></a>Aktuální omezení P11 a P15 databází s maxsize 4 TB

Maximální velikost 4 TB P11 a P15 databáze je podporována v některé oblasti (dřív popsané). Hello následující požadavky a omezení použití tooP11 a P15 databáze s maxsize 4 TB:

- Pokud zvolíte možnost maxsize 4 TB hello při vytváření databáze (pomocí hodnotu 4 TB nebo 4096 GB), hello vytvořit příkaz selže s chybou, pokud je databáze hello zřízené v nepodporované oblasti.
- Pro existující P11 a P15 databáze nachází v jedné z hello podporované oblasti můžete zvýšit hello maxsize úložiště too4 TB. To lze ověřit pomocí hello [vyberte DATABASEPROPERTYEX](https://msdn.microsoft.com/library/ms186823.aspx) nebo zkontrolováním hello velikost databáze v hello portálu Azure. Upgrade na existující P11 nebo P15 databáze lze provést pouze hlavní přihlášení na úrovni serveru nebo členové role databáze dbmanager hello. 
- Pokud se operace upgradu se spouštějí v konfiguraci hello podporovanou oblast aktualizoval okamžitě. Hello databáze zůstane během procesu upgradu hello online. Nelze však využívat hello úplné 4 TB úložiště, dokud hello skutečné databázové soubory nebyly upgradovány toohello nové maxsize. Délka Hello doba potřebná závisí na velikosti hello hello databáze během upgradu.  
- Při vytváření nebo aktualizaci P11 nebo P15 databáze, můžete zvolit pouze 1 TB a 4 TB maxsize. Velikosti zprostředkující úložiště nejsou aktuálně podporovány. Při vytváření P11/P15, je předem vybrané hello výchozí možnost úložiště o velikosti 1 TB. Pro databáze nachází v jedné z hello podporované oblasti můžete zvýšit maximální too4TB hello úložiště pro nový nebo existující jednu databázi. Všechny ostatní země nelze zvýšit maximální velikost, vyšší než 1 TB. cena Hello nezmění, když vyberete 4 TB součástí úložiště.
- Hello 4 TB databáze maxsize nemůže být změněné too1 TB i v případě hello skutečné úložiště používané je menší než 1 TB. Proto není možné snížit P11 až 4 TB nebo P15 - 4 TB tooa P11 až 1 TB nebo P15 - 1 TB nebo nižší úroveň výkonu například tooP1 P6) dokud umožňujeme možnosti dodatečné úložiště pro hello zbytek hello úrovně výkonu. Toto omezení platí i toohello obnovení a scénáře kopírováním včetně bodu v čase, geografické obnovení, dlouhodobý-dlouhodobé zálohování uchovávání a kopie databáze. Jakmile databáze je nastavena možnost 4 TB hello, je potřeba spustit všechny operace obnovení této databáze do P11 nebo P15 s 4 TB maxsize.
- Při vytváření nebo upgradu databázi P11/P15 v oblast nepodporované hello vytvořit nebo operace upgradu nezdaří a zobrazí se následující chybová zpráva hello: **P11 a P15 databáze s až too4TB úložiště jsou k dispozici v East2 USA, západní USA, Virginia nám verze pro státní správu Západní Evropa, střední Německo, jihovýchodní Asie, Japonsko – východ, Austrálie – východ, Střední Kanada a Východní Kanada.**
- Pro aktivní geografickou replikací scénáře:
   - Nastavení vztahu geografická replikace: Pokud je primární databáze hello P11 nebo P15, hello secondary(ies) musí být také P11 nebo P15; nižší úrovně výkonu byly zamítnuty jako sekundární databáze, protože nejsou podporovat 4 TB.
   - Upgrade hello primární databázi v relaci geografická replikace: Změna hello maxsize too4 TB na primární databázi aktivuje hello stejné změnit na hello sekundární databáze. Obě upgrady musí být úspěšná změna hello na hello primární tootake vliv. Platí oblast omezení pro možnost 4TB hello (viz výše). Je-li hello sekundární v oblasti, která nepodporuje 4 TB, není aktualizován hello primární.
- Použití služby importu a exportu hello načítání P11 až 4TB nebo P15 - 4TB databáze není podporováno. Použít SqlPackage.exe příliš[importovat](sql-database-import.md) a [exportovat](sql-database-export.md) data.

## <a name="manage-single-database-service-tiers-and-performance-levels-using-hello-azure-portal"></a>Spravovat úrovně služeb izolovaných databází a úrovně výkonu pomocí hello portálu Azure

tooset nebo změňte hello vrstvy služeb, úroveň výkonu nebo množství úložiště pro nový nebo existující databázi Azure SQL pomocí hello portál Azure, otevřete hello **konfigurace výkonu** okno pro vaši databázi kliknutím  **Cenová úroveň (škálování Dtu)** – jak ukazuje následující snímek obrazovky hello. 

- Nastavit nebo změnit úroveň služby hello výběrem hello vrstvy služeb pro vaši úlohu. 
- Nastavit nebo změnit úroveň výkonu hello (**Dtu**) v rámci vrstvy služby pomocí hello **DTU** posuvníku.
- Nastavení nebo změna hello množství úložiště pro úroveň výkonu hello pomocí hello **úložiště** posuvníku. 

  ![Konfigurace úrovně služeb a výkonu](./media/sql-database-service-tiers/service-tier-performance-level.png)

> [!IMPORTANT]
> Zkontrolujte [aktuální omezení P11 a P15 databází s 4 TB maxsize](sql-database-service-tiers.md#current-limitations-of-p11-and-p15-databases-with-4-tb-maxsize) při výběru P11 nebo P15 vrstvy služeb.
>

## <a name="manage-single-database-service-tiers-and-performance-levels-using-powershell"></a>Spravovat úrovně služeb izolovaných databází a úrovně výkonu pomocí prostředí PowerShell

tooset nebo změnu úrovně služeb databáze Azure SQL, úrovně výkonu a množství úložiště pomocí prostředí PowerShell, použijte hello následující rutiny prostředí PowerShell. Pokud potřebujete tooinstall nebo upgrade prostředí PowerShell, přečtěte si téma [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps). 

| Rutina | Popis |
| --- | --- |
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Vytvoří databázi |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Získá jednu nebo více databází|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Nastaví vlastnosti pro databázi nebo přesune existující databáze do pružného fondu|


> [!TIP]
> Ukázkový skript prostředí PowerShell, který monitoruje hello metriky výkonu databáze, se škáluje tooa vyšší úroveň výkonu a vytvoří pravidlo výstrahy na jednom hello metrik výkonu, najdete v části [sledování a škálování jeden SQL databáze pomocí Prostředí PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md).

## <a name="manage-single-database-service-tiers-and-performance-levels-using-hello-azure-cli"></a>Spravovat úrovně služeb izolovaných databází a úrovně výkonu pomocí hello rozhraní příkazového řádku Azure

tooset nebo změnu úrovně služeb databáze Azure SQL, úrovně výkonu a množství úložiště pomocí hello rozhraní příkazového řádku Azure, použijte následující hello [databáze SQL Azure CLI](/cli/azure/sql/db) příkazy. Použití hello [cloudové prostředí](/azure/cloud-shell/overview) toorun hello rozhraní příkazového řádku v prohlížeči nebo [nainstalovat](/cli/azure/install-azure-cli) ji v systému macOS, Linux nebo Windows. Vytváření a Správa SQL elastické fondy najdete v tématu [elastické fondy](sql-database-elastic-pool.md).

| Rutina | Popis |
| --- | --- |
|[Vytvoření az sql db](/cli/azure/sql/db#create) |Vytvoří databázi|
|[AZ sql db seznamu](/cli/azure/sql/db#list)|Zobrazí všechny databáze a datových skladů na serveru, nebo všechny databáze v elastickém fondu|
|[seznam edicí az sql db](/cli/azure/sql/db#list-editions)|Seznamy, které jsou k dispozici služby cíle a limity úložiště|
|[db sql az seznamu – použití](/cli/azure/sql/db#list-usages)|Vrátí databáze použití|
|[AZ sql db zobrazit](/cli/azure/sql/db#show)|Získá databáze nebo datového skladu|
|[aktualizace databáze sql az](/cli/azure/sql/db#update)|Aktualizuje databázi|

> [!TIP]
> Ukázkový skript příkazového řádku Azure CLI, která je škálovatelná jeden Azure SQL databáze tooa jinou úroveň výkonu po dotaz na informace o velikosti hello hello databáze, najdete v části [toomonitor použití rozhraní příkazového řádku a škálování jedné databáze SQL](scripts/sql-database-monitor-and-scale-database-cli.md).
>

## <a name="manage-single-database-service-tiers-and-performance-levels-using-transact-sql"></a>Spravovat úrovně služeb izolovaných databází a úrovně výkonu pomocí jazyka Transact-SQL

tooset nebo změnu úrovně služeb databáze Azure SQL, úrovně výkonu a množství úložiště pomocí jazyka Transact-SQL, pomocí následujících příkazů T-SQL hello. Můžete použít tyto příkazy pomocí hello portál Azure, [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), nebo jiný program, který se můžete připojit server Azure SQL Database tooan a předat Transact-SQL příkazy. 

| Příkaz | Popis |
| --- | --- |
|[Vytvoření databáze (Azure SQL Database)](/sql/t-sql/statements/create-database-azure-sql-database)|Vytvoří novou databázi. Musí být připojené toohello hlavní databázi toocreate novou databázi.|
| [Příkaz ALTER DATABASE (databáze Azure SQL)](/sql/t-sql/statements/alter-database-azure-sql-database) |Upravuje Azure SQL database. |
|[Sys.database_service_objectives (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Vrátí hello edition (vrstva služby), cíl služby (cenové úrovně) a název elastického fondu, pokud existuje, pro databázi Azure SQL nebo Azure SQL Data Warehouse. Pokud přihlášení toohello hlavní databázi v serveru Azure SQL Database, vrátí informace na všechny databáze. Pro Azure SQL Data Warehouse musí být připojené toohello hlavní databázi.|
|[Sys.database_usage (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-usage-azure-sql-database)|Seznamy hello počet, typ a doba trvání databází na serveru Azure SQL Database.|

Hello následující příklad ukazuje hello maxsize mění pomocí příkazu ALTER DATABASE hello:

 ```sql
ALTER DATABASE <myDatabaseName> 
   MODIFY (MAXSIZE = 4096 GB);
```

## <a name="manage-single-databases-using-hello-rest-api"></a>Spravovat jedné databáze pomocí hello REST API

tooset nebo změnu úrovně služeb databáze Azure SQL, úrovně výkonu a množství úložiště pomocí hello rozhraní REST API, najdete v části [Azure SQL Database REST API](/rest/api/sql/).

## <a name="next-steps"></a>Další kroky

* Další informace o [Dtu](sql-database-what-is-a-dtu.md).
* toolearn o sledování využití v jednotkách DTU, najdete v části [monitorování a optimalizace výkonu](sql-database-troubleshoot-performance.md).

