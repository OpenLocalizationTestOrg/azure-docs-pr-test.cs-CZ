---
title: "proměnné aaaAssign v SQL Data Warehouse | Microsoft Docs"
description: "Tipy pro přiřazení proměnné jazyka Transact-SQL v Azure SQL Data Warehouse na vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 81ddc7cf-a6ba-4585-91a3-b6ea50f49227
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 9de48739bb0af80ff2a117704b31512c680f78d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="assign-variables-in-sql-data-warehouse"></a><span data-ttu-id="38c25-103">Přiřazení proměnné v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="38c25-103">Assign variables in SQL Data Warehouse</span></span>
<span data-ttu-id="38c25-104">Jsou nastaveny proměnné v SQL Data Warehouse pomocí hello `DECLARE` příkaz nebo hello `SET` příkaz.</span><span class="sxs-lookup"><span data-stu-id="38c25-104">Variables in SQL Data Warehouse are set using hello `DECLARE` statement or hello `SET` statement.</span></span>

<span data-ttu-id="38c25-105">Jsou všechny následující hello perfektně platné způsoby tooset hodnotu proměnné:</span><span class="sxs-lookup"><span data-stu-id="38c25-105">All of hello following are perfectly valid ways tooset a variable value:</span></span>

## <a name="setting-variables-with-declare"></a><span data-ttu-id="38c25-106">Nastavení proměnné s DECLARE</span><span class="sxs-lookup"><span data-stu-id="38c25-106">Setting variables with DECLARE</span></span>
<span data-ttu-id="38c25-107">Inicializace proměnných s DECLARE je jedním z hello nejpružnější způsoby tooset hodnotu proměnné v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="38c25-107">Initializing variables with DECLARE is one of hello most flexible ways tooset a variable value in SQL Data Warehouse.</span></span>

```sql
DECLARE @v  int = 0
;
```

<span data-ttu-id="38c25-108">Současně můžete také použít DECLARE tooset více než jednu proměnnou.</span><span class="sxs-lookup"><span data-stu-id="38c25-108">You can also use DECLARE tooset more than one variable at a time.</span></span> <span data-ttu-id="38c25-109">Nemůžete použít `SELECT` nebo `UPDATE` toodo toto:</span><span class="sxs-lookup"><span data-stu-id="38c25-109">You cannot use `SELECT` or `UPDATE` toodo this:</span></span>

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

<span data-ttu-id="38c25-110">Nelze inicializovat a použití proměnné v hello stejné příkazu DECLARE.</span><span class="sxs-lookup"><span data-stu-id="38c25-110">You cannot initialise and use a variable in hello same DECLARE statement.</span></span> <span data-ttu-id="38c25-111">tooillustrate hello bodu hello následující příklad je **není** povolena jako @p1 jak inicializovat a použít v hello stejné příkazu DECLARE.</span><span class="sxs-lookup"><span data-stu-id="38c25-111">tooillustrate hello point hello example below is **not** allowed as @p1 is both initialized and used in hello same DECLARE statement.</span></span> <span data-ttu-id="38c25-112">To bude mít za následek chybu.</span><span class="sxs-lookup"><span data-stu-id="38c25-112">This will result in an error.</span></span>

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a><span data-ttu-id="38c25-113">Nastavení hodnot sadou</span><span class="sxs-lookup"><span data-stu-id="38c25-113">Setting values with SET</span></span>
<span data-ttu-id="38c25-114">Sada je velmi běžné metody pro nastavení jednu proměnnou.</span><span class="sxs-lookup"><span data-stu-id="38c25-114">Set is a very common method for setting a single variable.</span></span>

<span data-ttu-id="38c25-115">Všechny hello příklady jsou platné způsoby nastavení proměnné sadou:</span><span class="sxs-lookup"><span data-stu-id="38c25-115">All of hello examples below are valid ways of setting a variable with SET:</span></span>

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

<span data-ttu-id="38c25-116">Najednou můžete sadou nastavit pouze jednu proměnnou.</span><span class="sxs-lookup"><span data-stu-id="38c25-116">You can only set one variable at a time with SET.</span></span> <span data-ttu-id="38c25-117">Je ale, jak je vidět výše složené operátory povolená.</span><span class="sxs-lookup"><span data-stu-id="38c25-117">However, as can be seen above compound operators are permissable.</span></span>

## <a name="limitations"></a><span data-ttu-id="38c25-118">Omezení</span><span class="sxs-lookup"><span data-stu-id="38c25-118">Limitations</span></span>
<span data-ttu-id="38c25-119">Vyberte nebo aktualizaci nelze použít pro přiřazení proměnné.</span><span class="sxs-lookup"><span data-stu-id="38c25-119">You cannot use SELECT or UPDATE for variable assignment.</span></span>

## <a name="next-steps"></a><span data-ttu-id="38c25-120">Další kroky</span><span class="sxs-lookup"><span data-stu-id="38c25-120">Next steps</span></span>
<span data-ttu-id="38c25-121">Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].</span><span class="sxs-lookup"><span data-stu-id="38c25-121">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
