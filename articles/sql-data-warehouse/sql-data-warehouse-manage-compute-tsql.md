---
title: "aaaPause, obnovit, škálovat pomocí T-SQL v Azure SQL Data Warehouse | Microsoft Docs"
description: "Příkaz Transact-SQL (T-SQL) úlohy se na více systémů tooscale výkonu úpravou Dwu. Uložte náklady škálování zpět v době mimo špičku."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: a970d939-2adf-4856-8a78-d4fe8ab2cceb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 03/30/2017
ms.author: elbutter;barbkess
ms.openlocfilehash: 84c6868acb673221d8853319ac9a05bb98b2b7c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-t-sql"></a>Spravovat výpočetní výkon v Azure SQL Data Warehouse (T-SQL)
> [!div class="op_single_selector"]
> * [Přehled](sql-data-warehouse-manage-compute-overview.md)
> * [Azure Portal](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>

<a name="current-dwu-bk"></a>

## <a name="view-current-dwu-settings"></a>Aktuální nastavení DWU zobrazení
tooview hello aktuální DWU nastavení pro vaše databáze:

1. Otevřete Průzkumník objektů systému SQL Server v sadě Visual Studio.
2. Připojte toohello přidružené k logickému serveru SQL Database hello hlavní databázi.
3. Vyberte zobrazení dynamické správy sys.database_service_objectives hello. Zde naleznete příklad: 

```sql
SELECT
    db.name [Database]
,   ds.edition [Edition]
,   ds.service_objective [Service Objective]
FROM
    sys.database_service_objectives ds
JOIN
    sys.databases db ON ds.database_id = db.database_id
```

<a name="scale-dwu-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a>Škálování výpočetní kapacity
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

hello toochange Dwu:

1. Připojte toohello přidružené k databázi SQL serveru logické hlavní databázi.
2. Použití hello [ALTER DATABASE] [ ALTER DATABASE] příkaz TSQL. Hello následující příklad ilustruje hello služby úrovně cíle tooDW1000 pro databázi hello MySQLDW. 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

<a name="check-database-state-bk"></a>

## <a name="check-database-state-and-operation-progress"></a>Kontrola stavu a operaci průběhu databáze

1. Připojte toohello přidružené k databázi SQL serveru logické hlavní databázi.
2. Odeslání stav databáze toocheck dotazu

```sql
SELECT *
FROM
sys.databases
```

3. Odeslat dotaz toocheck stav operace

```sql
SELECT *
FROM
    sys.dm_operation_status
WHERE
    resource_type_desc = 'Database'
AND 
    major_resource_id = 'MySQLDW'
```

Tato DMV vrátí informace o různé operace správy v SQL Data Warehouse jako je například stav operace a hello hello hello operace, který bude buď IN_PROGRESS nebo byla DOKONČENA.



<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Další kroky
Další úlohy správy, najdete v části [přehled správy][Management overview].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Management overview]: ./sql-data-warehouse-overview-manage.md
[Manage compute power overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->

[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
