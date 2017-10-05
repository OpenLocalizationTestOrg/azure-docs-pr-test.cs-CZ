---
title: "Uložené procedury v SQL Data Warehouse | Microsoft Docs"
description: "Tipy pro implementaci uložené procedury v Azure SQL Data Warehouse na vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 9b238789-6efe-4820-bf77-5a5da2afa0e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: e42d80f0ca35f3fbb67389c66d072bc40d8a8d2c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="stored-procedures-in-sql-data-warehouse"></a><span data-ttu-id="3441b-103">Uložené procedury v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="3441b-103">Stored procedures in SQL Data Warehouse</span></span>
<span data-ttu-id="3441b-104">SQL Data Warehouse podporuje mnoho funkcí jazyka Transact-SQL v SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="3441b-104">SQL Data Warehouse supports many of the Transact-SQL features found in SQL Server.</span></span> <span data-ttu-id="3441b-105">Je důležité nejsou specifické funkce, které bude chceme využít také k maximalizovat výkon vašeho řešení s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="3441b-105">More importantly there are scale out specific features that we will want to leverage to maximize the performance of your solution.</span></span>

<span data-ttu-id="3441b-106">Zachování škálování a výkonu, služby SQL Data Warehouse jsou však také některé funkce a funkce, které mají rozdíly v chování a ostatní uživatele, které nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="3441b-106">However, to maintain the scale and performance of SQL Data Warehouse there are also some features and functionality that have behavioral differences and others that are not supported.</span></span>

<span data-ttu-id="3441b-107">Tento článek vysvětluje, jak implementovat uložené procedury v rámci SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3441b-107">This article explains how to implement stored procedures within SQL Data Warehouse.</span></span>

## <a name="introducing-stored-procedures"></a><span data-ttu-id="3441b-108">Představení uložené procedury</span><span class="sxs-lookup"><span data-stu-id="3441b-108">Introducing stored procedures</span></span>
<span data-ttu-id="3441b-109">Uložené procedury jsou vhodné pro zapouzdření kódu SQL; ukládání blízko data v datovém skladu.</span><span class="sxs-lookup"><span data-stu-id="3441b-109">Stored procedures are a great way for encapsulating your SQL code; storing it close to your data in the data warehouse.</span></span> <span data-ttu-id="3441b-110">Zapouzdřením kód do spravovatelných jednotek uložené procedury pomoci vývojářům rozčlenění moduly svá řešení; usnadnění větší znovuvyužití kódu.</span><span class="sxs-lookup"><span data-stu-id="3441b-110">By encapsulating the code into manageable units stored procedures help developers modularize their solutions; facilitating greater re-usability of code.</span></span> <span data-ttu-id="3441b-111">Každý uložené procedury mohou také přijímat parametry tak, aby byly i flexibilnější.</span><span class="sxs-lookup"><span data-stu-id="3441b-111">Each stored procedure can also accept parameters to make them even more flexible.</span></span>

<span data-ttu-id="3441b-112">SQL Data Warehouse poskytuje zjednodušenou a efektivní uložené procedury implementaci.</span><span class="sxs-lookup"><span data-stu-id="3441b-112">SQL Data Warehouse provides a simplified and streamlined stored procedure implementation.</span></span> <span data-ttu-id="3441b-113">Největší rozdíl oproti systému SQL Server je, že uložená procedura není předem zkompilovaný kód.</span><span class="sxs-lookup"><span data-stu-id="3441b-113">The biggest difference compared to SQL Server is that the stored procedure is not pre-compiled code.</span></span> <span data-ttu-id="3441b-114">V datové sklady jsou obecně menší význam s časem kompilace.</span><span class="sxs-lookup"><span data-stu-id="3441b-114">In data warehouses we are generally less concerned with the compilation time.</span></span> <span data-ttu-id="3441b-115">Je důležité kód uložené procedury je při fungování proti velkých objemů dat správně optimalizované.</span><span class="sxs-lookup"><span data-stu-id="3441b-115">It is more important that the stored procedure code is correctly optimised when operating against large data volumes.</span></span> <span data-ttu-id="3441b-116">Cílem je uložit hodiny, minuty a sekundy není milisekundách.</span><span class="sxs-lookup"><span data-stu-id="3441b-116">The goal is to save hours, minutes and seconds not milliseconds.</span></span> <span data-ttu-id="3441b-117">Je proto více vhodné zamyslet nad uložené procedury jako kontejnery pro logiku SQL.</span><span class="sxs-lookup"><span data-stu-id="3441b-117">It is therefore more helpful to think of stored procedures as containers for SQL logic.</span></span>     

<span data-ttu-id="3441b-118">Když SQL Data Warehouse provede uložené procedury jsou analyzovat příkazy SQL, přeložených a optimalizované za běhu.</span><span class="sxs-lookup"><span data-stu-id="3441b-118">When SQL Data Warehouse executes your stored procedure the SQL statements are parsed, translated and optimized at run time.</span></span> <span data-ttu-id="3441b-119">Během tohoto procesu je každý příkaz převeden na distribuované dotazy.</span><span class="sxs-lookup"><span data-stu-id="3441b-119">During this process each statement is converted into distributed queries.</span></span> <span data-ttu-id="3441b-120">Kód SQL, který je ve skutečnosti spustit pro data se liší na dotaz odeslána.</span><span class="sxs-lookup"><span data-stu-id="3441b-120">The SQL code that is actually executed against the data is different to the query submitted.</span></span>

## <a name="nesting-stored-procedures"></a><span data-ttu-id="3441b-121">Vnoření uložené procedury</span><span class="sxs-lookup"><span data-stu-id="3441b-121">Nesting stored procedures</span></span>
<span data-ttu-id="3441b-122">Uložené procedury při volání jiné uložené procedury nebo pak vnitřní uložená procedura nebo kód volání říká, že je možné provést dynamické sql.</span><span class="sxs-lookup"><span data-stu-id="3441b-122">When stored procedures call other stored procedures or execute dynamic sql then the inner stored procedure or code invocation is said to be nested.</span></span>

<span data-ttu-id="3441b-123">SQL Data Warehouse podporují maximálně 8 vnořených úrovní.</span><span class="sxs-lookup"><span data-stu-id="3441b-123">SQL Data Warehouse support a maximum of 8 nesting levels.</span></span> <span data-ttu-id="3441b-124">To se mírně liší k systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3441b-124">This is slightly different to SQL Server.</span></span> <span data-ttu-id="3441b-125">Úroveň vnoření v systému SQL Server je 32.</span><span class="sxs-lookup"><span data-stu-id="3441b-125">The nest level in SQL Server is 32.</span></span>

<span data-ttu-id="3441b-126">Volání uložené procedury nejvyšší úrovně se rovná vnořit úroveň 1</span><span class="sxs-lookup"><span data-stu-id="3441b-126">The top level stored procedure call equates to nest level 1</span></span>

```sql
EXEC prc_nesting
```
<span data-ttu-id="3441b-127">Pokud uložená procedura také provede další EXEC volání pak to zvýší úroveň vnoření 2</span><span class="sxs-lookup"><span data-stu-id="3441b-127">If the stored procedure also makes another EXEC call then this will increase the nest level to 2</span></span>

```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
<span data-ttu-id="3441b-128">Pokud se druhý postup pak provede některé dynamické sql, pak to zvýší úroveň vnoření na 3</span><span class="sxs-lookup"><span data-stu-id="3441b-128">If the second procedure then executes some dynamic sql then this will increase the nest level to 3</span></span>

```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

<span data-ttu-id="3441b-129">Poznámka: SQL Data Warehouse nepodporuje aktuálně@NESTLEVEL.</span><span class="sxs-lookup"><span data-stu-id="3441b-129">Note SQL Data Warehouse does not currently support @@NESTLEVEL.</span></span> <span data-ttu-id="3441b-130">Musíte mít nainstalované sledovat vaše úrovně vnoření.</span><span class="sxs-lookup"><span data-stu-id="3441b-130">You will need to keep a track of your nest level yourself.</span></span> <span data-ttu-id="3441b-131">Nepravděpodobné, se setkají limit úrovně vnoření 8, ale v takovém případě budete muset znovu fungovat kódu a "zploštění" jej tak, aby odpovídal v rámci tohoto limitu.</span><span class="sxs-lookup"><span data-stu-id="3441b-131">It is unlikely you will hit the 8 nest level limit but if you do you will need to re-work your code and "flatten" it so that it fits within this limit.</span></span>

## <a name="insertexecute"></a><span data-ttu-id="3441b-132">PŘÍKAZ INSERT... SPUŠTĚNÍ</span><span class="sxs-lookup"><span data-stu-id="3441b-132">INSERT..EXECUTE</span></span>
<span data-ttu-id="3441b-133">SQL Data Warehouse nepovoluje využívat sady výsledků dotazu uložené proceduře pomocí příkazu INSERT.</span><span class="sxs-lookup"><span data-stu-id="3441b-133">SQL Data Warehouse does not permit you to consume the result set of a stored procedure with an INSERT statement.</span></span> <span data-ttu-id="3441b-134">Je však alternativní způsob, který můžete použít.</span><span class="sxs-lookup"><span data-stu-id="3441b-134">However, there is an alternative approach you can use.</span></span>

<span data-ttu-id="3441b-135">Naleznete v následujícím článku na [dočasných tabulek] příklad o tom, jak to udělat.</span><span class="sxs-lookup"><span data-stu-id="3441b-135">Please refer to the following article on [temporary tables] for an example on how to do this.</span></span>

## <a name="limitations"></a><span data-ttu-id="3441b-136">Omezení</span><span class="sxs-lookup"><span data-stu-id="3441b-136">Limitations</span></span>
<span data-ttu-id="3441b-137">Existují některé aspekty Transact-SQL uložené procedury, které nejsou implementované v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3441b-137">There are some aspects of Transact-SQL stored procedures that are not implemented in SQL Data Warehouse.</span></span>

<span data-ttu-id="3441b-138">Jsou:</span><span class="sxs-lookup"><span data-stu-id="3441b-138">They are:</span></span>

* <span data-ttu-id="3441b-139">dočasně uložených procedur</span><span class="sxs-lookup"><span data-stu-id="3441b-139">temporary stored procedures</span></span>
* <span data-ttu-id="3441b-140">číslované uložené procedury</span><span class="sxs-lookup"><span data-stu-id="3441b-140">numbered stored procedures</span></span>
* <span data-ttu-id="3441b-141">Rozšířené uložené procedury</span><span class="sxs-lookup"><span data-stu-id="3441b-141">extended stored procedures</span></span>
* <span data-ttu-id="3441b-142">CLR uložené procedury</span><span class="sxs-lookup"><span data-stu-id="3441b-142">CLR stored procedures</span></span>
* <span data-ttu-id="3441b-143">možnost šifrování</span><span class="sxs-lookup"><span data-stu-id="3441b-143">encryption option</span></span>
* <span data-ttu-id="3441b-144">možnost replikace</span><span class="sxs-lookup"><span data-stu-id="3441b-144">replication option</span></span>
* <span data-ttu-id="3441b-145">Parametry s hodnotou tabulky</span><span class="sxs-lookup"><span data-stu-id="3441b-145">table-valued parameters</span></span>
* <span data-ttu-id="3441b-146">parametry jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="3441b-146">read-only parameters</span></span>
* <span data-ttu-id="3441b-147">výchozí parametry</span><span class="sxs-lookup"><span data-stu-id="3441b-147">default parameters</span></span>
* <span data-ttu-id="3441b-148">kontexty provádění</span><span class="sxs-lookup"><span data-stu-id="3441b-148">execution contexts</span></span>
* <span data-ttu-id="3441b-149">Return – příkaz</span><span class="sxs-lookup"><span data-stu-id="3441b-149">return statement</span></span>

## <a name="next-steps"></a><span data-ttu-id="3441b-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3441b-150">Next steps</span></span>
<span data-ttu-id="3441b-151">Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].</span><span class="sxs-lookup"><span data-stu-id="3441b-151">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
<span data-ttu-id="3441b-152">[dočasných tabulek]: ./sql-data-warehouse-tables-temporary.md#modularizing-code</span><span class="sxs-lookup"><span data-stu-id="3441b-152">[temporary tables]: ./sql-data-warehouse-tables-temporary.md#modularizing-code</span></span>
[development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[nest level]: https://msdn.microsoft.com/library/ms187371.aspx

<!--Other Web references-->
