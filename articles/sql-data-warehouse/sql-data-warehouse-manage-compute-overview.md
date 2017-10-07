---
title: "aaaManage výpočetní výkon v Azure SQL Data Warehouse (přehled) | Microsoft Docs"
description: "Výkon škálování možnosti v Azure SQL Data Warehouse. Horizontální navýšení kapacity úpravou Dwu nebo pozastavení a obnovení toosave náklady na výpočetní prostředky."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: 
ms.assetid: e13a82b0-abfe-429f-ac3c-f2b6789a70c6
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 03/22/2017
ms.author: elbutter
ms.openlocfilehash: 1ffbe8d694ac181eaeb6f585a2cee87a570ed7d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-overview"></a>Spravovat výpočetní výkon v Azure SQL Data Warehouse (přehled)
> [!div class="op_single_selector"]
> * [Přehled](sql-data-warehouse-manage-compute-overview.md)
> * [Azure Portal](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>

Architektura Hello služby SQL Data Warehouse odděluje úložiště a výpočty, povolení jednotlivých tooscale nezávisle. Výpočetní v důsledku toho může být nezávislá hello množství dat, požadavky na výkon škálovat toomeet. Přirozený důsledek této architektury je, že [fakturace] [ billed] pro výpočetní prostředí a úložiště jsou oddělené. 

Tento přehled popisuje jak škálovat funguje s SQL Data Warehouse a jak tooutilize hello pozastavení, opětovné spuštění a možnosti škálování služby SQL Data Warehouse. Poraďte se hello [datového skladu (Dwu) jednotky] [ data warehouse units (DWUs)] jak Dwu a výkonu související s toolearn stránky. 

## <a name="how-compute-management-operations-work-in-sql-data-warehouse"></a>Jak výpočetní operace správy pracovat v SQL Data Warehouse
Architektura Hello pro SQL Data Warehouse se skládá z řídicí uzel, výpočetní uzly a vrstvy úložiště hello rozloženy 60 distribuce. 

Během normálního aktivní relace v SQL Data Warehouse váš systém hlavního uzlu spravuje hello metadata a obsahuje hello distribuované Optimalizátor dotazů. Pod tímto uzlem head jsou výpočetní uzly a vrstvě úložiště. Pro 400 DWU má váš systém jeden hlavní uzel, čtyři výpočetní uzly a vrstvy úložiště hello, který se skládá z 60 distribuce. 

Když podstoupit škálování nebo pozastavit operace, systém hello nejprve ukončí všechny příchozí dotazy a vrátí zpět transakce tooensure konzistentním stavu. Pro operace škálování škálování dojde pouze po dokončení této transakční vrácení zpět. U operace škálování hello systému zřizuje hello velmi požadovaný počet výpočetních uzlů a poté zahájí opětovné hello výpočetní uzly toohello úložiště vrstvy. Pro operaci vertikální snížení kapacity hello nepotřebné uzly jsou vydávány a hello zbývající výpočetní uzly připojte, sami toohello odpovídající počet distribuce. Operace pozastavení všechny výpočetní uzly jsou vydávány a váš systém určitým celou řadu metadata operations tooleave konečné systém stabilní.

| DWU  | \#výpočetních uzlů | \#distribucí na uzel |
| ---- | ------------------ | ---------------------------- |
| 100  | 1                  | 60                           |
| 200  | 2                  | 30                           |
| 300  | 3                  | 20                           |
| 400  | 4                  | 15                           |
| 500  | 5                  | 12                           |
| 600  | 6                  | 10                           |
| 1000 | 10                 | 6                            |
| 1200 | 12                 | 5                            |
| 1 500 | 15                 | 4                            |
| 2000 | 20                 | 3                            |
| 3000 | 30                 | 2                            |
| 6000 | 60                 | 1                            |

jsou tři primární funkce pro správu výpočetních Hello:

1. Pozastavení
2. Obnovit
3. Měřítko

Všechny tyto operace může trvat několik minut toocomplete. Pokud jste škálování nebo pozastavení nebo obnovení automaticky, můžete tooimplement logiku tooensure určitá operace byly dokončeny před pokračováním další akci. 

Kontroluje se stav databáze hello prostřednictvím různých koncových bodů vám umožní toocorrectly implementace automatizace těchto operací. portál Hello bude poskytovat oznámení po dokončení operaci a hello databáze aktuální stav, ale neumožňuje programový kontroly stavu. 

>  [!NOTE]
>
>  Výpočetní funkce správy neexistuje mezi všechny koncové body.
>
>  

|              | Pozastavení nebo obnovení | Měřítko | Zkontrolujte stav databáze |
| ------------ | ------------ | ----- | -------------------- |
| portál Azure | Ano          | Ano   | **Ne**               |
| PowerShell   | Ano          | Ano   | Ano                  |
| REST API     | Ano          | Ano   | Ano                  |
| T-SQL        | **Ne**       | Ano   | Ano                  |



<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a>Škálování výpočetní kapacity

Výkon v SQL Data Warehouse se měří v [datového skladu (Dwu) jednotky] [ data warehouse units (DWUs)] tedy abstraktní měr výpočetní prostředky, jako je například procesoru, paměti a vstupně-výstupní šířky pásma. Uživatel, který si přeje tooscale výkon jejich systému lze provést různé způsoby, například prostřednictvím portálu hello T-SQL a rozhraní REST API. 

### <a name="how-do-i-scale-compute"></a>Jak škálovat výpočetní?
Výpočetní výkon spravovaná SQL Data Warehouse změnou nastavení DWU hello. Zvýšení výkonu [lineárně] [ linearly] jako přidáte další DWU pro určité operace.  Nabízíme DWU nabídky, které zajišťují, že výkon změní výrazně když je systém škálovat nahoru nebo dolů. 

tooadjust Dwu, můžete použít některou z těchto jednotlivých metod.

* [Škálování výpočetního výkonu pomocí portálu Azure][Scale compute power with Azure portal]
* [Škálování výpočetního výkonu pomocí prostředí PowerShell][Scale compute power with PowerShell]
* [Škálování výpočetní výkon pomocí rozhraní REST API][Scale compute power with REST APIs]
* [Škálování výpočetního výkonu pomocí TSQL][Scale compute power with TSQL]

### <a name="how-many-dwus-should-i-use"></a>Kolik Dwu mám použít?

toounderstand jaké ideální hodnota DWU je, zkuste škálování nahoru a dolů a spustit pár dotazů po načtení dat. Škálování je rychlé, můžete zkusit různé úrovně výkonu za hodinu nebo méně. 

> [!Note] 
> Datový sklad SQL je navrženou tooprocess velkých objemů dat. toosee možnosti true pro škálování, zejména u větší Dwu, chcete toouse velké datové sady, který se blíží nebo je vyšší než 1 TB.

Doporučení pro vyhledání hello nejlepší DWU pro úlohy:

1. Pro datový sklad v vývoj Začněte výběrem menší úroveň výkonu DWU.  Vhodná výchozí hodnota je DW400 nebo DW200.
2. Monitorovat výkon aplikací, sledování hello počet Dwu vybrané porovnání výkonu toohello zjistíte.
3. Určují, jak daleko vyšší nebo nižší výkon by mělo být pro jste tooreach hello optimální úroveň výkonu pro vaše požadavky podle za předpokladu, že lineární stupnice.
4. Zvýšení nebo snížení počtu hello Dwu v poměru toohow mnohem vyšší nebo nižší, na které má vaše tooperform zatížení. 
5. Pokračujte v provádění úprav, dokud se nedostanete na úroveň optimálního výkonu pro vaše podnikové požadavky.

> [!NOTE]
>
> Výkon dotazů pouze hodnota se zvyšuje s další paralelizace Pokud hello pracovní můžete rozdělit mezi výpočetní uzly. Pokud zjistíte, že škálování není změna výkon, podrobnosti naleznete v našem ladění toocheck články, zda je vaše data nerovnoměrně distribuované nebo pokud jsou představení velké množství přesun dat výkonu. 

### <a name="when-should-i-scale-dwus"></a>Pokud by měl škálování Dwu?
Škálování Dwu mění hello následující důležité scénáře:

1. Změna lineárně výkon systému hello kontrol, agregace a funkce CTAS příkazy
2. Při načítání pomocí funkce PolyBase zvýšit počet hello čtení a zápis
3. Maximální počet souběžných dotazů a sloty souběžnosti

Doporučení pro případ tooscale Dwu:

1. Před provedením operace načítání nebo transformace dat těžká, škálovat Dwu tak, aby vaše data jsou k dispozici rychleji.
2. Během pracovní dobu ve špičce škálovat tooaccommodate velký počet souběžných dotazů. 

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Pozastavit výpočetní
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

toopause databázi, použijte některou z těchto jednotlivých metod.

* [Pozastavit výpočetní pomocí portálu Azure][Pause compute with Azure portal]
* [Pozastavit výpočetní pomocí prostředí PowerShell][Pause compute with PowerShell]
* [Pozastavit výpočetní pomocí rozhraní REST API][Pause compute with REST APIs]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Obnovit výpočetní
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

tooresume databázi, použijte některou z těchto jednotlivých metod.

* [Výpočetní obnovit pomocí portálu Azure][Resume compute with Azure portal]
* [Výpočetní obnovení pomocí prostředí PowerShell][Resume compute with PowerShell]
* [Obnovit výpočetní pomocí rozhraní REST API][Resume compute with REST APIs]

<a name="check-compute-bk"></a>

## <a name="check-database-state"></a>Zkontrolujte stav databáze 

tooresume databázi, použijte některou z těchto jednotlivých metod.

- [Zkontrolujte stav databáze s T-SQL][Check database state with T-SQL]
- [Zkontrolujte stav databáze pomocí prostředí PowerShell][Check database state with PowerShell]
- [Zkontrolujte stav databáze pomocí rozhraní REST API][Check database state with REST APIs]

## <a name="permissions"></a>Oprávnění

Škálování hello databáze vyžaduje hello oprávnění popsaná v [ALTER DATABASE][ALTER DATABASE].  Pozastavení a obnovení vyžadují hello [Přispěvatel databází SQL] [ SQL DB Contributor] oprávnění, konkrétně Microsoft.Sql/servers/databases/action.

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Další kroky
Odkažte toohello následující články toohelp základními pojmy některé další klíče výkonu:

* [Správa úloh a souběžnost][Workload and concurrency management]
* [Přehled návrhu tabulky][Table design overview]
* [Distribuce tabulky][Table distribution]
* [Tabulka indexování][Table indexing]
* [Vytváření oddílů tabulky][Table partitioning]
* [Statistiky tabulky][Table statistics]
* [Doporučené postupy][Best practices]

<!--Image reference-->

<!--Article references-->
[data warehouse units (DWUs)]: ./sql-data-warehouse-overview-what-is.md#predictable-and-scalable-performance-with-data-warehouse-units
[billed]: https://azure.microsoft.com/en-us/pricing/details/sql-data-warehouse/
[linearly]: ./sql-data-warehouse-overview-what-is.md#predictable-and-scalable-performance-with-data-warehouse-units
[Scale compute power with Azure portal]: ./sql-data-warehouse-manage-compute-portal.md#scale-compute-power
[Scale compute power with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#scale-compute-bk
[Scale compute power with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#scale-compute-bk
[Scale compute power with TSQL]: ./sql-data-warehouse-manage-compute-tsql.md#scale-compute-bk

[capacity limits]: ./sql-data-warehouse-service-capacity-limits.md

[Pause compute with Azure portal]:  ./sql-data-warehouse-manage-compute-portal.md#pause-compute-bk
[Pause compute with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#pause-compute-bk
[Pause compute with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#pause-compute-bk

[Resume compute with Azure portal]:  ./sql-data-warehouse-manage-compute-portal.md#resume-compute-bk
[Resume compute with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#resume-compute-bk
[Resume compute with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#resume-compute-bk

[Check database state with T-SQL]: ./sql-data-warehouse-manage-compute-tsql.md#check-database-state-and-operation-progress
[Check database state with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#check-database-state
[Check database state with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#check-database-state

[Workload and concurrency management]: ./sql-data-warehouse-develop-concurrency.md
[Table design overview]: ./sql-data-warehouse-tables-overview.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Table indexing]: ./sql-data-warehouse-tables-index.md
[Table partitioning]: ./sql-data-warehouse-tables-partition.md
[Table statistics]: ./sql-data-warehouse-tables-statistics.md
[Best practices]: ./sql-data-warehouse-best-practices.md
[development overview]: ./sql-data-warehouse-overview-develop.md

[SQL DB Contributor]: ../active-directory/role-based-access-built-in-roles.md#sql-db-contributor

<!--MSDN references-->
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Azure portal]: http://portal.azure.com/
