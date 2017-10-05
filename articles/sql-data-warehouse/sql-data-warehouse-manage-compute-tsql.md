---
title: "Pozastavit, obnovit a škálovat pomocí T-SQL v Azure SQL Data Warehouse | Microsoft Docs"
description: "Příkaz Transact-SQL (T-SQL) úlohy škálovatelný výkon úpravou Dwu. Uložte náklady škálování zpět v době mimo špičku."
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
ms.openlocfilehash: 9221d72ecf8ab2ba8b04e4bc97eeef7157817cca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-t-sql"></a><span data-ttu-id="66898-104">Spravovat výpočetní výkon v Azure SQL Data Warehouse (T-SQL)</span><span class="sxs-lookup"><span data-stu-id="66898-104">Manage compute power in Azure SQL Data Warehouse (T-SQL)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="66898-105">Přehled</span><span class="sxs-lookup"><span data-stu-id="66898-105">Overview</span></span>](sql-data-warehouse-manage-compute-overview.md)
> * [<span data-ttu-id="66898-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="66898-106">Portal</span></span>](sql-data-warehouse-manage-compute-portal.md)
> * [<span data-ttu-id="66898-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="66898-107">PowerShell</span></span>](sql-data-warehouse-manage-compute-powershell.md)
> * [<span data-ttu-id="66898-108">REST</span><span class="sxs-lookup"><span data-stu-id="66898-108">REST</span></span>](sql-data-warehouse-manage-compute-rest-api.md)
> * [<span data-ttu-id="66898-109">TSQL</span><span class="sxs-lookup"><span data-stu-id="66898-109">TSQL</span></span>](sql-data-warehouse-manage-compute-tsql.md)
>
>

<a name="current-dwu-bk"></a>

## <a name="view-current-dwu-settings"></a><span data-ttu-id="66898-110">Aktuální nastavení DWU zobrazení</span><span class="sxs-lookup"><span data-stu-id="66898-110">View current DWU settings</span></span>
<span data-ttu-id="66898-111">Chcete-li zobrazit aktuální nastavení DWU pro vaše databáze:</span><span class="sxs-lookup"><span data-stu-id="66898-111">To view the current DWU settings for your databases:</span></span>

1. <span data-ttu-id="66898-112">Otevřete Průzkumník objektů systému SQL Server v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="66898-112">Open SQL Server Object Explorer in Visual Studio.</span></span>
2. <span data-ttu-id="66898-113">Připojení k databázi hlavní přidružené k logickému serveru služby SQL Database.</span><span class="sxs-lookup"><span data-stu-id="66898-113">Connect to the master database associated with the logical SQL Database server.</span></span>
3. <span data-ttu-id="66898-114">Vyberte ze zobrazení dynamické správy sys.database_service_objectives.</span><span class="sxs-lookup"><span data-stu-id="66898-114">Select from the sys.database_service_objectives dynamic management view.</span></span> <span data-ttu-id="66898-115">Zde naleznete příklad:</span><span class="sxs-lookup"><span data-stu-id="66898-115">Here is an example:</span></span> 

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

## <a name="scale-compute"></a><span data-ttu-id="66898-116">Škálování výpočetní kapacity</span><span class="sxs-lookup"><span data-stu-id="66898-116">Scale compute</span></span>
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

<span data-ttu-id="66898-117">Chcete-li změnit jednotkami Dwu:</span><span class="sxs-lookup"><span data-stu-id="66898-117">To change the DWUs:</span></span>

1. <span data-ttu-id="66898-118">Připojení k databázi hlavní přidružené k logické databáze SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="66898-118">Connect to the master database associated with your logical SQL Database server.</span></span>
2. <span data-ttu-id="66898-119">Použití [ALTER DATABASE] [ ALTER DATABASE] příkaz TSQL.</span><span class="sxs-lookup"><span data-stu-id="66898-119">Use the [ALTER DATABASE][ALTER DATABASE] TSQL statement.</span></span> <span data-ttu-id="66898-120">Následující příklad nastaví DW1000 cíle na úrovni služby pro databázi MySQLDW.</span><span class="sxs-lookup"><span data-stu-id="66898-120">The following example sets the service level objective to DW1000 for the database MySQLDW.</span></span> 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

<a name="check-database-state-bk"></a>

## <a name="check-database-state-and-operation-progress"></a><span data-ttu-id="66898-121">Kontrola stavu a operaci průběhu databáze</span><span class="sxs-lookup"><span data-stu-id="66898-121">Check database state and operation progress</span></span>

1. <span data-ttu-id="66898-122">Připojení k databázi hlavní přidružené k logické databáze SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="66898-122">Connect to the master database associated with your logical SQL Database server.</span></span>
2. <span data-ttu-id="66898-123">Odesílání dotazu pro kontrolu stavu databáze</span><span class="sxs-lookup"><span data-stu-id="66898-123">Submit query to check database state</span></span>

```sql
SELECT *
FROM
sys.databases
```

3. <span data-ttu-id="66898-124">Odesílání dotazu a zkontrolujte stav operace</span><span class="sxs-lookup"><span data-stu-id="66898-124">Submit query to check status of operation</span></span>

```sql
SELECT *
FROM
    sys.dm_operation_status
WHERE
    resource_type_desc = 'Database'
AND 
    major_resource_id = 'MySQLDW'
```

<span data-ttu-id="66898-125">Tato DMV vrátí informace o různé operace správy v SQL Data Warehouse například operaci a stav operace, který bude buď IN_PROGRESS nebo byla DOKONČENA.</span><span class="sxs-lookup"><span data-stu-id="66898-125">This DMV will return information about various management operations on your SQL Data Warehouse such as the operation and the state of the operation, which will either be IN_PROGRESS or COMPLETED.</span></span>



<a name="next-steps-bk"></a>

## <a name="next-steps"></a><span data-ttu-id="66898-126">Další kroky</span><span class="sxs-lookup"><span data-stu-id="66898-126">Next steps</span></span>
<span data-ttu-id="66898-127">Další úlohy správy, najdete v části [přehled správy][Management overview].</span><span class="sxs-lookup"><span data-stu-id="66898-127">For other management tasks, see [Management overview][Management overview].</span></span>

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Management overview]: ./sql-data-warehouse-overview-manage.md
[Manage compute power overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->

[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
