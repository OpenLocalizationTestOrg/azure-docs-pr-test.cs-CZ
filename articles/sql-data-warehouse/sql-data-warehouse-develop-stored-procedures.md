---
title: postupy aaaStored v SQL Data Warehouse | Microsoft Docs
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
ms.openlocfilehash: 416252dd3dea95c66aa5e886860b933b22578002
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="stored-procedures-in-sql-data-warehouse"></a><span data-ttu-id="f250f-103">Uložené procedury v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="f250f-103">Stored procedures in SQL Data Warehouse</span></span>
<span data-ttu-id="f250f-104">SQL Data Warehouse podporuje mnoho funkcí hello Transact-SQL, najít v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f250f-104">SQL Data Warehouse supports many of hello Transact-SQL features found in SQL Server.</span></span> <span data-ttu-id="f250f-105">Je důležité nejsou specifické funkce, že chceme tooleverage toomaximize hello výkon vašeho řešení s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="f250f-105">More importantly there are scale out specific features that we will want tooleverage toomaximize hello performance of your solution.</span></span>

<span data-ttu-id="f250f-106">Toomaintain hello škálování a výkonu, služby SQL Data Warehouse jsou však také některé funkce a funkce, které mají rozdíly v chování a ostatní uživatele, které nejsou podporovány.</span><span class="sxs-lookup"><span data-stu-id="f250f-106">However, toomaintain hello scale and performance of SQL Data Warehouse there are also some features and functionality that have behavioral differences and others that are not supported.</span></span>

<span data-ttu-id="f250f-107">Tento článek vysvětluje, jak tooimplement uložené procedury v rámci SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="f250f-107">This article explains how tooimplement stored procedures within SQL Data Warehouse.</span></span>

## <a name="introducing-stored-procedures"></a><span data-ttu-id="f250f-108">Představení uložené procedury</span><span class="sxs-lookup"><span data-stu-id="f250f-108">Introducing stored procedures</span></span>
<span data-ttu-id="f250f-109">Uložené procedury jsou vhodné pro zapouzdření kódu SQL; ukládání dat zavřít tooyour hello datového skladu.</span><span class="sxs-lookup"><span data-stu-id="f250f-109">Stored procedures are a great way for encapsulating your SQL code; storing it close tooyour data in hello data warehouse.</span></span> <span data-ttu-id="f250f-110">Zapouzdřením hello kódu do spravovatelných jednotek uložené procedury pomoci vývojářům rozčlenění moduly svá řešení; usnadnění větší znovuvyužití kódu.</span><span class="sxs-lookup"><span data-stu-id="f250f-110">By encapsulating hello code into manageable units stored procedures help developers modularize their solutions; facilitating greater re-usability of code.</span></span> <span data-ttu-id="f250f-111">Každý uložené procedury může přijmout také parametry toomake je i flexibilnější.</span><span class="sxs-lookup"><span data-stu-id="f250f-111">Each stored procedure can also accept parameters toomake them even more flexible.</span></span>

<span data-ttu-id="f250f-112">SQL Data Warehouse poskytuje zjednodušenou a efektivní uložené procedury implementaci.</span><span class="sxs-lookup"><span data-stu-id="f250f-112">SQL Data Warehouse provides a simplified and streamlined stored procedure implementation.</span></span> <span data-ttu-id="f250f-113">Hello největších rozdíl oproti tooSQL, které je Server, hello uložená procedura není předem zkompilovaný kód.</span><span class="sxs-lookup"><span data-stu-id="f250f-113">hello biggest difference compared tooSQL Server is that hello stored procedure is not pre-compiled code.</span></span> <span data-ttu-id="f250f-114">V datové sklady jsou obecně menší význam s časem kompilace hello.</span><span class="sxs-lookup"><span data-stu-id="f250f-114">In data warehouses we are generally less concerned with hello compilation time.</span></span> <span data-ttu-id="f250f-115">Je důležité hello uložené procedury kód je při fungování proti velkých objemů dat správně optimalizované.</span><span class="sxs-lookup"><span data-stu-id="f250f-115">It is more important that hello stored procedure code is correctly optimised when operating against large data volumes.</span></span> <span data-ttu-id="f250f-116">cílem Hello je toosave hodiny, minuty a sekundy není milisekundách.</span><span class="sxs-lookup"><span data-stu-id="f250f-116">hello goal is toosave hours, minutes and seconds not milliseconds.</span></span> <span data-ttu-id="f250f-117">Proto je užitečné toothink uložené procedury jako kontejnery pro logiku SQL.</span><span class="sxs-lookup"><span data-stu-id="f250f-117">It is therefore more helpful toothink of stored procedures as containers for SQL logic.</span></span>     

<span data-ttu-id="f250f-118">Když SQL Data Warehouse provede jsou příkazy SQL hello vaše uložené procedury analyzovat, přeložit a optimalizované v době běhu.</span><span class="sxs-lookup"><span data-stu-id="f250f-118">When SQL Data Warehouse executes your stored procedure hello SQL statements are parsed, translated and optimized at run time.</span></span> <span data-ttu-id="f250f-119">Během tohoto procesu je každý příkaz převeden na distribuované dotazy.</span><span class="sxs-lookup"><span data-stu-id="f250f-119">During this process each statement is converted into distributed queries.</span></span> <span data-ttu-id="f250f-120">Hello SQL kód, který je ve skutečnosti spustit pro hello data je jiný toohello dotazu odeslána.</span><span class="sxs-lookup"><span data-stu-id="f250f-120">hello SQL code that is actually executed against hello data is different toohello query submitted.</span></span>

## <a name="nesting-stored-procedures"></a><span data-ttu-id="f250f-121">Vnoření uložené procedury</span><span class="sxs-lookup"><span data-stu-id="f250f-121">Nesting stored procedures</span></span>
<span data-ttu-id="f250f-122">Když uložené procedury volat jiné uložené procedury nebo spuštění dynamické sql pak hello vnitřní uložené procedury nebo kód volání říká, že je toobe vnořený.</span><span class="sxs-lookup"><span data-stu-id="f250f-122">When stored procedures call other stored procedures or execute dynamic sql then hello inner stored procedure or code invocation is said toobe nested.</span></span>

<span data-ttu-id="f250f-123">SQL Data Warehouse podporují maximálně 8 vnořených úrovní.</span><span class="sxs-lookup"><span data-stu-id="f250f-123">SQL Data Warehouse support a maximum of 8 nesting levels.</span></span> <span data-ttu-id="f250f-124">Toto je mírně odlišný tooSQL serveru.</span><span class="sxs-lookup"><span data-stu-id="f250f-124">This is slightly different tooSQL Server.</span></span> <span data-ttu-id="f250f-125">úroveň vnoření Hello v systému SQL Server je 32.</span><span class="sxs-lookup"><span data-stu-id="f250f-125">hello nest level in SQL Server is 32.</span></span>

<span data-ttu-id="f250f-126">volání uložené procedury nejvyšší úrovně Hello znamená zároveň toonest úroveň 1</span><span class="sxs-lookup"><span data-stu-id="f250f-126">hello top level stored procedure call equates toonest level 1</span></span>

```sql
EXEC prc_nesting
```
<span data-ttu-id="f250f-127">Pokud hello uložené procedury navíc využívá další EXEC volání pak to zvýší too2 úrovně vnoření hello</span><span class="sxs-lookup"><span data-stu-id="f250f-127">If hello stored procedure also makes another EXEC call then this will increase hello nest level too2</span></span>

```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
<span data-ttu-id="f250f-128">Pokud se druhý postup hello pak provede některé dynamické sql pak to zvýší too3 úrovně vnoření hello</span><span class="sxs-lookup"><span data-stu-id="f250f-128">If hello second procedure then executes some dynamic sql then this will increase hello nest level too3</span></span>

```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

<span data-ttu-id="f250f-129">Poznámka: SQL Data Warehouse nepodporuje aktuálně@NESTLEVEL.</span><span class="sxs-lookup"><span data-stu-id="f250f-129">Note SQL Data Warehouse does not currently support @@NESTLEVEL.</span></span> <span data-ttu-id="f250f-130">Budete potřebovat tookeep sledovat vaše úrovně vnoření sami.</span><span class="sxs-lookup"><span data-stu-id="f250f-130">You will need tookeep a track of your nest level yourself.</span></span> <span data-ttu-id="f250f-131">Nepravděpodobné, se setkají limit úrovně vnoření hello 8, ale pokud uděláte budete potřebovat toore pracovní kódu a "zploštění" jej tak, aby odpovídal v rámci tohoto limitu.</span><span class="sxs-lookup"><span data-stu-id="f250f-131">It is unlikely you will hit hello 8 nest level limit but if you do you will need toore-work your code and "flatten" it so that it fits within this limit.</span></span>

## <a name="insertexecute"></a><span data-ttu-id="f250f-132">PŘÍKAZ INSERT... SPUŠTĚNÍ</span><span class="sxs-lookup"><span data-stu-id="f250f-132">INSERT..EXECUTE</span></span>
<span data-ttu-id="f250f-133">SQL Data Warehouse vám nepovoluje tooconsume hello sady výsledků dotazu uložené proceduře pomocí příkazu INSERT.</span><span class="sxs-lookup"><span data-stu-id="f250f-133">SQL Data Warehouse does not permit you tooconsume hello result set of a stored procedure with an INSERT statement.</span></span> <span data-ttu-id="f250f-134">Je však alternativní způsob, který můžete použít.</span><span class="sxs-lookup"><span data-stu-id="f250f-134">However, there is an alternative approach you can use.</span></span>

<span data-ttu-id="f250f-135">Naleznete v následujícím článku toohello [dočasných tabulek] pro příklad jak toodo to.</span><span class="sxs-lookup"><span data-stu-id="f250f-135">Please refer toohello following article on [temporary tables] for an example on how toodo this.</span></span>

## <a name="limitations"></a><span data-ttu-id="f250f-136">Omezení</span><span class="sxs-lookup"><span data-stu-id="f250f-136">Limitations</span></span>
<span data-ttu-id="f250f-137">Existují některé aspekty Transact-SQL uložené procedury, které nejsou implementované v SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="f250f-137">There are some aspects of Transact-SQL stored procedures that are not implemented in SQL Data Warehouse.</span></span>

<span data-ttu-id="f250f-138">Jsou:</span><span class="sxs-lookup"><span data-stu-id="f250f-138">They are:</span></span>

* <span data-ttu-id="f250f-139">dočasně uložených procedur</span><span class="sxs-lookup"><span data-stu-id="f250f-139">temporary stored procedures</span></span>
* <span data-ttu-id="f250f-140">číslované uložené procedury</span><span class="sxs-lookup"><span data-stu-id="f250f-140">numbered stored procedures</span></span>
* <span data-ttu-id="f250f-141">Rozšířené uložené procedury</span><span class="sxs-lookup"><span data-stu-id="f250f-141">extended stored procedures</span></span>
* <span data-ttu-id="f250f-142">CLR uložené procedury</span><span class="sxs-lookup"><span data-stu-id="f250f-142">CLR stored procedures</span></span>
* <span data-ttu-id="f250f-143">možnost šifrování</span><span class="sxs-lookup"><span data-stu-id="f250f-143">encryption option</span></span>
* <span data-ttu-id="f250f-144">možnost replikace</span><span class="sxs-lookup"><span data-stu-id="f250f-144">replication option</span></span>
* <span data-ttu-id="f250f-145">Parametry s hodnotou tabulky</span><span class="sxs-lookup"><span data-stu-id="f250f-145">table-valued parameters</span></span>
* <span data-ttu-id="f250f-146">parametry jen pro čtení</span><span class="sxs-lookup"><span data-stu-id="f250f-146">read-only parameters</span></span>
* <span data-ttu-id="f250f-147">výchozí parametry</span><span class="sxs-lookup"><span data-stu-id="f250f-147">default parameters</span></span>
* <span data-ttu-id="f250f-148">kontexty provádění</span><span class="sxs-lookup"><span data-stu-id="f250f-148">execution contexts</span></span>
* <span data-ttu-id="f250f-149">Return – příkaz</span><span class="sxs-lookup"><span data-stu-id="f250f-149">return statement</span></span>

## <a name="next-steps"></a><span data-ttu-id="f250f-150">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f250f-150">Next steps</span></span>
<span data-ttu-id="f250f-151">Další tipy pro vývoj, najdete v části [přehled vývoje][development overview].</span><span class="sxs-lookup"><span data-stu-id="f250f-151">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[dočasných tabulek]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[nest level]: https://msdn.microsoft.com/library/ms187371.aspx

<!--Other Web references-->
