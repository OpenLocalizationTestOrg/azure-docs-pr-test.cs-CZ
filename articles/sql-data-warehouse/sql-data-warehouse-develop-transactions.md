---
title: Transakce v SQL Data Warehouse | Microsoft Docs
description: "Tipy pro provádění transakcí v Azure SQL Data Warehouse na vývoj řešení."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: ae621788-e575-41f5-8bfe-fa04dc4b0b53
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 29d53e18539f2c24dd64090b2ac6f9dd4c783961
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="transactions-in-sql-data-warehouse"></a><span data-ttu-id="f1573-103">Transakce v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="f1573-103">Transactions in SQL Data Warehouse</span></span>
<span data-ttu-id="f1573-104">Jak jste zvyklí, SQL Data Warehouse podporuje transakce v rámci úlohy datového skladu.</span><span class="sxs-lookup"><span data-stu-id="f1573-104">As you would expect, SQL Data Warehouse supports transactions as part of the data warehouse workload.</span></span> <span data-ttu-id="f1573-105">Ale zajistit, že výkon služby SQL Data Warehouse je udržován na úrovni škálování některé funkce omezeny ve srovnání s systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f1573-105">However, to ensure the performance of SQL Data Warehouse is maintained at scale some features are limited when compared to SQL Server.</span></span> <span data-ttu-id="f1573-106">V tomto článku klade důraz rozdíly a uvádí ostatní.</span><span class="sxs-lookup"><span data-stu-id="f1573-106">This article highlights the differences and lists the others.</span></span> 

## <a name="transaction-isolation-levels"></a><span data-ttu-id="f1573-107">Úrovně izolace transakce</span><span class="sxs-lookup"><span data-stu-id="f1573-107">Transaction isolation levels</span></span>
<span data-ttu-id="f1573-108">SQL Data Warehouse implementuje transakce ACID.</span><span class="sxs-lookup"><span data-stu-id="f1573-108">SQL Data Warehouse implements ACID transactions.</span></span> <span data-ttu-id="f1573-109">Izolaci podpora transakcí je však omezená na `READ UNCOMMITTED` a toto nastavení nelze změnit.</span><span class="sxs-lookup"><span data-stu-id="f1573-109">However, the Isolation of the transactional support is limited to `READ UNCOMMITTED` and this cannot be changed.</span></span> <span data-ttu-id="f1573-110">Můžete implementovat řadu kódování metody, aby se zabránilo nekonzistence čtení dat, pokud se jedná o problém pro vás.</span><span class="sxs-lookup"><span data-stu-id="f1573-110">You can implement a number of coding methods to prevent dirty reads of data if this is a concern for you.</span></span> <span data-ttu-id="f1573-111">Nejčastěji používané metody využít uživatelům zabránit v dotazování na data, která stále připraveném funkce CTAS a přepínání oddílů tabulky (často označované jako posuvné okno vzor).</span><span class="sxs-lookup"><span data-stu-id="f1573-111">The most popular methods leverage both CTAS and table partition switching (often known as the sliding window pattern) to prevent users from querying data that is still being prepared.</span></span> <span data-ttu-id="f1573-112">Zobrazení, které předem filtrovat data je také oblíbených přístup.</span><span class="sxs-lookup"><span data-stu-id="f1573-112">Views that pre-filter the data is also a popular approach.</span></span>  

## <a name="transaction-size"></a><span data-ttu-id="f1573-113">Velikost transakce</span><span class="sxs-lookup"><span data-stu-id="f1573-113">Transaction size</span></span>
<span data-ttu-id="f1573-114">Transakce úpravy jednoho datového je omezen velikostí.</span><span class="sxs-lookup"><span data-stu-id="f1573-114">A single data modification transaction is limited in size.</span></span> <span data-ttu-id="f1573-115">Limit dnes platí "za distribuční".</span><span class="sxs-lookup"><span data-stu-id="f1573-115">The limit today is applied "per distribution".</span></span> <span data-ttu-id="f1573-116">Proto lze vypočítat celkové přidělení vynásobením limit počtu distribučních.</span><span class="sxs-lookup"><span data-stu-id="f1573-116">Therefore, the total allocation can be calculated by multiplying the limit by the distribution count.</span></span> <span data-ttu-id="f1573-117">Přibližná maximální počet řádků v transakci rozdělíte krytky distribuční celková velikost každý řádek.</span><span class="sxs-lookup"><span data-stu-id="f1573-117">To approximate the maximum number of rows in the transaction divide the distribution cap by the total size of each row.</span></span> <span data-ttu-id="f1573-118">Pro proměnnou délkou zvažte trvá délka průměrná sloupce, nikoli pomocí maximální velikosti.</span><span class="sxs-lookup"><span data-stu-id="f1573-118">For variable length columns consider taking an average column length rather than using the maximum size.</span></span>

<span data-ttu-id="f1573-119">V následující tabulce následující předpoklady byly provedeny:</span><span class="sxs-lookup"><span data-stu-id="f1573-119">In the table below the following assumptions have been made:</span></span>

* <span data-ttu-id="f1573-120">Rovnoměrné rozdělení dat došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="f1573-120">An even distribution of data has occurred</span></span> 
* <span data-ttu-id="f1573-121">Délka průměrná řádek je 250 bajtů</span><span class="sxs-lookup"><span data-stu-id="f1573-121">The average row length is 250 bytes</span></span>

| <span data-ttu-id="f1573-122">[DWU][DWU]</span><span class="sxs-lookup"><span data-stu-id="f1573-122">[DWU][DWU]</span></span> | <span data-ttu-id="f1573-123">Cap za distribuční (GiB)</span><span class="sxs-lookup"><span data-stu-id="f1573-123">Cap per distribution (GiB)</span></span> | <span data-ttu-id="f1573-124">Počet distribuce</span><span class="sxs-lookup"><span data-stu-id="f1573-124">Number of Distributions</span></span> | <span data-ttu-id="f1573-125">MAXIMÁLNÍ velikost transakce (GiB)</span><span class="sxs-lookup"><span data-stu-id="f1573-125">MAX transaction size (GiB)</span></span> | <span data-ttu-id="f1573-126"># Řádků na jeden distribuce</span><span class="sxs-lookup"><span data-stu-id="f1573-126"># Rows per distribution</span></span> | <span data-ttu-id="f1573-127">Maximální počet řádků na transakci</span><span class="sxs-lookup"><span data-stu-id="f1573-127">Max Rows per transaction</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="f1573-128">OD DW100</span><span class="sxs-lookup"><span data-stu-id="f1573-128">DW100</span></span> |<span data-ttu-id="f1573-129">1</span><span class="sxs-lookup"><span data-stu-id="f1573-129">1</span></span> |<span data-ttu-id="f1573-130">60</span><span class="sxs-lookup"><span data-stu-id="f1573-130">60</span></span> |<span data-ttu-id="f1573-131">60</span><span class="sxs-lookup"><span data-stu-id="f1573-131">60</span></span> |<span data-ttu-id="f1573-132">4,000,000</span><span class="sxs-lookup"><span data-stu-id="f1573-132">4,000,000</span></span> |<span data-ttu-id="f1573-133">240,000,000</span><span class="sxs-lookup"><span data-stu-id="f1573-133">240,000,000</span></span> |
| <span data-ttu-id="f1573-134">DW200</span><span class="sxs-lookup"><span data-stu-id="f1573-134">DW200</span></span> |<span data-ttu-id="f1573-135">1.5</span><span class="sxs-lookup"><span data-stu-id="f1573-135">1.5</span></span> |<span data-ttu-id="f1573-136">60</span><span class="sxs-lookup"><span data-stu-id="f1573-136">60</span></span> |<span data-ttu-id="f1573-137">90</span><span class="sxs-lookup"><span data-stu-id="f1573-137">90</span></span> |<span data-ttu-id="f1573-138">6 000 000</span><span class="sxs-lookup"><span data-stu-id="f1573-138">6,000,000</span></span> |<span data-ttu-id="f1573-139">360,000,000</span><span class="sxs-lookup"><span data-stu-id="f1573-139">360,000,000</span></span> |
| <span data-ttu-id="f1573-140">DW300</span><span class="sxs-lookup"><span data-stu-id="f1573-140">DW300</span></span> |<span data-ttu-id="f1573-141">2.25</span><span class="sxs-lookup"><span data-stu-id="f1573-141">2.25</span></span> |<span data-ttu-id="f1573-142">60</span><span class="sxs-lookup"><span data-stu-id="f1573-142">60</span></span> |<span data-ttu-id="f1573-143">135</span><span class="sxs-lookup"><span data-stu-id="f1573-143">135</span></span> |<span data-ttu-id="f1573-144">9,000,000</span><span class="sxs-lookup"><span data-stu-id="f1573-144">9,000,000</span></span> |<span data-ttu-id="f1573-145">540,000,000</span><span class="sxs-lookup"><span data-stu-id="f1573-145">540,000,000</span></span> |
| <span data-ttu-id="f1573-146">DW400</span><span class="sxs-lookup"><span data-stu-id="f1573-146">DW400</span></span> |<span data-ttu-id="f1573-147">3</span><span class="sxs-lookup"><span data-stu-id="f1573-147">3</span></span> |<span data-ttu-id="f1573-148">60</span><span class="sxs-lookup"><span data-stu-id="f1573-148">60</span></span> |<span data-ttu-id="f1573-149">180</span><span class="sxs-lookup"><span data-stu-id="f1573-149">180</span></span> |<span data-ttu-id="f1573-150">12,000,000</span><span class="sxs-lookup"><span data-stu-id="f1573-150">12,000,000</span></span> |<span data-ttu-id="f1573-151">720,000,000</span><span class="sxs-lookup"><span data-stu-id="f1573-151">720,000,000</span></span> |
| <span data-ttu-id="f1573-152">DW500</span><span class="sxs-lookup"><span data-stu-id="f1573-152">DW500</span></span> |<span data-ttu-id="f1573-153">3.75</span><span class="sxs-lookup"><span data-stu-id="f1573-153">3.75</span></span> |<span data-ttu-id="f1573-154">60</span><span class="sxs-lookup"><span data-stu-id="f1573-154">60</span></span> |<span data-ttu-id="f1573-155">225</span><span class="sxs-lookup"><span data-stu-id="f1573-155">225</span></span> |<span data-ttu-id="f1573-156">15,000,000</span><span class="sxs-lookup"><span data-stu-id="f1573-156">15,000,000</span></span> |<span data-ttu-id="f1573-157">900,000,000</span><span class="sxs-lookup"><span data-stu-id="f1573-157">900,000,000</span></span> |
| <span data-ttu-id="f1573-158">DW600</span><span class="sxs-lookup"><span data-stu-id="f1573-158">DW600</span></span> |<span data-ttu-id="f1573-159">4.5</span><span class="sxs-lookup"><span data-stu-id="f1573-159">4.5</span></span> |<span data-ttu-id="f1573-160">60</span><span class="sxs-lookup"><span data-stu-id="f1573-160">60</span></span> |<span data-ttu-id="f1573-161">270</span><span class="sxs-lookup"><span data-stu-id="f1573-161">270</span></span> |<span data-ttu-id="f1573-162">18,000,000</span><span class="sxs-lookup"><span data-stu-id="f1573-162">18,000,000</span></span> |<span data-ttu-id="f1573-163">1,080,000,000</span><span class="sxs-lookup"><span data-stu-id="f1573-163">1,080,000,000</span></span> |
| <span data-ttu-id="f1573-164">DW1000</span><span class="sxs-lookup"><span data-stu-id="f1573-164">DW1000</span></span> |<span data-ttu-id="f1573-165">7.5</span><span class="sxs-lookup"><span data-stu-id="f1573-165">7.5</span></span> |<span data-ttu-id="f1573-166">60</span><span class="sxs-lookup"><span data-stu-id="f1573-166">60</span></span> |<span data-ttu-id="f1573-167">450</span><span class="sxs-lookup"><span data-stu-id="f1573-167">450</span></span> |<span data-ttu-id="f1573-168">30,000,000</span><span class="sxs-lookup"><span data-stu-id="f1573-168">30,000,000</span></span> |<span data-ttu-id="f1573-169">1,800,000,000</span><span class="sxs-lookup"><span data-stu-id="f1573-169">1,800,000,000</span></span> |
| <span data-ttu-id="f1573-170">DW1200</span><span class="sxs-lookup"><span data-stu-id="f1573-170">DW1200</span></span> |<span data-ttu-id="f1573-171">9</span><span class="sxs-lookup"><span data-stu-id="f1573-171">9</span></span> |<span data-ttu-id="f1573-172">60</span><span class="sxs-lookup"><span data-stu-id="f1573-172">60</span></span> |<span data-ttu-id="f1573-173">540</span><span class="sxs-lookup"><span data-stu-id="f1573-173">540</span></span> |<span data-ttu-id="f1573-174">36,000,000</span><span class="sxs-lookup"><span data-stu-id="f1573-174">36,000,000</span></span> |<span data-ttu-id="f1573-175">2,160,000,000</span><span class="sxs-lookup"><span data-stu-id="f1573-175">2,160,000,000</span></span> |
| <span data-ttu-id="f1573-176">DW1500</span><span class="sxs-lookup"><span data-stu-id="f1573-176">DW1500</span></span> |<span data-ttu-id="f1573-177">11.25</span><span class="sxs-lookup"><span data-stu-id="f1573-177">11.25</span></span> |<span data-ttu-id="f1573-178">60</span><span class="sxs-lookup"><span data-stu-id="f1573-178">60</span></span> |<span data-ttu-id="f1573-179">675</span><span class="sxs-lookup"><span data-stu-id="f1573-179">675</span></span> |<span data-ttu-id="f1573-180">45,000,000</span><span class="sxs-lookup"><span data-stu-id="f1573-180">45,000,000</span></span> |<span data-ttu-id="f1573-181">2,700,000,000</span><span class="sxs-lookup"><span data-stu-id="f1573-181">2,700,000,000</span></span> |
| <span data-ttu-id="f1573-182">DW2000</span><span class="sxs-lookup"><span data-stu-id="f1573-182">DW2000</span></span> |<span data-ttu-id="f1573-183">15</span><span class="sxs-lookup"><span data-stu-id="f1573-183">15</span></span> |<span data-ttu-id="f1573-184">60</span><span class="sxs-lookup"><span data-stu-id="f1573-184">60</span></span> |<span data-ttu-id="f1573-185">900</span><span class="sxs-lookup"><span data-stu-id="f1573-185">900</span></span> |<span data-ttu-id="f1573-186">60,000,000</span><span class="sxs-lookup"><span data-stu-id="f1573-186">60,000,000</span></span> |<span data-ttu-id="f1573-187">3,600,000,000</span><span class="sxs-lookup"><span data-stu-id="f1573-187">3,600,000,000</span></span> |
| <span data-ttu-id="f1573-188">DW3000</span><span class="sxs-lookup"><span data-stu-id="f1573-188">DW3000</span></span> |<span data-ttu-id="f1573-189">22.5</span><span class="sxs-lookup"><span data-stu-id="f1573-189">22.5</span></span> |<span data-ttu-id="f1573-190">60</span><span class="sxs-lookup"><span data-stu-id="f1573-190">60</span></span> |<span data-ttu-id="f1573-191">1,350</span><span class="sxs-lookup"><span data-stu-id="f1573-191">1,350</span></span> |<span data-ttu-id="f1573-192">90,000,000</span><span class="sxs-lookup"><span data-stu-id="f1573-192">90,000,000</span></span> |<span data-ttu-id="f1573-193">5,400,000,000</span><span class="sxs-lookup"><span data-stu-id="f1573-193">5,400,000,000</span></span> |
| <span data-ttu-id="f1573-194">DW6000</span><span class="sxs-lookup"><span data-stu-id="f1573-194">DW6000</span></span> |<span data-ttu-id="f1573-195">45</span><span class="sxs-lookup"><span data-stu-id="f1573-195">45</span></span> |<span data-ttu-id="f1573-196">60</span><span class="sxs-lookup"><span data-stu-id="f1573-196">60</span></span> |<span data-ttu-id="f1573-197">2,700</span><span class="sxs-lookup"><span data-stu-id="f1573-197">2,700</span></span> |<span data-ttu-id="f1573-198">180,000,000</span><span class="sxs-lookup"><span data-stu-id="f1573-198">180,000,000</span></span> |<span data-ttu-id="f1573-199">10,800,000,000</span><span class="sxs-lookup"><span data-stu-id="f1573-199">10,800,000,000</span></span> |

<span data-ttu-id="f1573-200">Limit velikosti transakce se použije pro transakci nebo operace.</span><span class="sxs-lookup"><span data-stu-id="f1573-200">The transaction size limit is applied per transaction or operation.</span></span> <span data-ttu-id="f1573-201">Nebude použito v rámci všech souběžných transakcí.</span><span class="sxs-lookup"><span data-stu-id="f1573-201">It is not applied across all concurrent transactions.</span></span> <span data-ttu-id="f1573-202">Každou transakci je proto oprávnění k zápisu takové množství dat do protokolu.</span><span class="sxs-lookup"><span data-stu-id="f1573-202">Therefore each transaction is permitted to write this amount of data to the log.</span></span> 

<span data-ttu-id="f1573-203">K a minimalizuje množství dat, zapíšou do protokolu naleznete [transakce osvědčené postupy] [ Transactions best practices] článku.</span><span class="sxs-lookup"><span data-stu-id="f1573-203">To optimize and minimize the amount of data written to the log please refer to the [Transactions best practices][Transactions best practices] article.</span></span>

> [!WARNING]
> <span data-ttu-id="f1573-204">Transakce maximální velikost lze dosáhnout pouze pro hodnoty HASH nebo dokonce ROUND_ROBIN distribuované tabulky, kde je šíření data.</span><span class="sxs-lookup"><span data-stu-id="f1573-204">The maximum transaction size can only be achieved for HASH or ROUND_ROBIN distributed tables where the spread of the data is even.</span></span> <span data-ttu-id="f1573-205">Pokud transakce je zápis dat zkreslilo způsobem do distribucí limit je pravděpodobně dosažitelná před transakce maximální velikost.</span><span class="sxs-lookup"><span data-stu-id="f1573-205">If the transaction is writing data in a skewed fashion to the distributions then the limit is likely to be reached prior to the maximum transaction size.</span></span>
> <!--REPLICATED_TABLE-->
> 
> 

## <a name="transaction-state"></a><span data-ttu-id="f1573-206">Stav transakce</span><span class="sxs-lookup"><span data-stu-id="f1573-206">Transaction state</span></span>
<span data-ttu-id="f1573-207">SQL Data Warehouse používá funkci XACT_STATE() k hlášení selhání transakce pomocí hodnoty -2.</span><span class="sxs-lookup"><span data-stu-id="f1573-207">SQL Data Warehouse uses the XACT_STATE() function to report a failed transaction using the value -2.</span></span> <span data-ttu-id="f1573-208">To znamená, že transakce se nezdařilo a je označen pro vrácení zpět pouze</span><span class="sxs-lookup"><span data-stu-id="f1573-208">This means that the transaction has failed and is marked for rollback only</span></span>

> [!NOTE]
> <span data-ttu-id="f1573-209">Použití -2 funkcí XACT_STATE k označení selhání transakce představuje různé chování v systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f1573-209">The use of -2 by the XACT_STATE function to denote a failed transaction represents different behavior to SQL Server.</span></span> <span data-ttu-id="f1573-210">SQL Server používá k reprezentaci rámci transakce hodnota -1.</span><span class="sxs-lookup"><span data-stu-id="f1573-210">SQL Server uses the value -1 to represent an un-committable transaction.</span></span> <span data-ttu-id="f1573-211">SQL Server může tolerovat chybami v transakci bez nutnosti označit jako rámci.</span><span class="sxs-lookup"><span data-stu-id="f1573-211">SQL Server can tolerate some errors inside a transaction without it having to be marked as un-committable.</span></span> <span data-ttu-id="f1573-212">Například `SELECT 1/0` by způsobit chybu, ale bez vynucení transakce v rámci stavu.</span><span class="sxs-lookup"><span data-stu-id="f1573-212">For example `SELECT 1/0` would cause an error but not force a transaction into an un-committable state.</span></span> <span data-ttu-id="f1573-213">SQL Server také umožňuje čtení v rámci transakce.</span><span class="sxs-lookup"><span data-stu-id="f1573-213">SQL Server also permits reads in the un-committable transaction.</span></span> <span data-ttu-id="f1573-214">Ale SQL Data Warehouse, nebudou vám to.</span><span class="sxs-lookup"><span data-stu-id="f1573-214">However, SQL Data Warehouse does not let you do this.</span></span> <span data-ttu-id="f1573-215">Pokud dojde k chybě v transakci SQL Data Warehouse automaticky přejde do stavu-2 a nebudete moct provádět žádné další výběr příkazy, dokud příkaz byla vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="f1573-215">If an error occurs inside a SQL Data Warehouse transaction it will automatically enter the -2 state and you will not be able to make any further select statements until the statement has been rolled back.</span></span> <span data-ttu-id="f1573-216">Proto je důležité zkontrolovat, že kód aplikace zobrazíte, pokud používá XACT_STATE() jako je třeba provést změny kódu.</span><span class="sxs-lookup"><span data-stu-id="f1573-216">It is therefore important to check that your application code to see if it uses  XACT_STATE() as you may need to make code modifications.</span></span>
> 
> 

<span data-ttu-id="f1573-217">V systému SQL Server se může zobrazit transakci, která vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="f1573-217">For example, in SQL Server you might see a transaction that looks like this:</span></span>

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;

        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

<span data-ttu-id="f1573-218">Pokud necháte kódu, protože je výše a zobrazí se následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="f1573-218">If you leave your code as it is above then you will get the following error message:</span></span>

<span data-ttu-id="f1573-219">Msg 111233, Level 16, stav 1, řádek 1 111233; Aktuální transakce byla přerušena a všechny čekající změny byly vráceny zpět.</span><span class="sxs-lookup"><span data-stu-id="f1573-219">Msg 111233, Level 16, State 1, Line 1 111233;The current transaction has aborted, and any pending changes have been rolled back.</span></span> <span data-ttu-id="f1573-220">Příčina: Transakce ve stavu jen pro vrácení zpět nebyla vrácena explicitně před DDL, DML nebo příkazu SELECT.</span><span class="sxs-lookup"><span data-stu-id="f1573-220">Cause: A transaction in a rollback-only state was not explicitly rolled back before a DDL, DML or SELECT statement.</span></span>

<span data-ttu-id="f1573-221">Také nebude získat výstup funkce ERROR_ *.</span><span class="sxs-lookup"><span data-stu-id="f1573-221">You will also not get the output of the ERROR_* functions.</span></span>

<span data-ttu-id="f1573-222">V SQL Data Warehouse musí být mírně změněn kód:</span><span class="sxs-lookup"><span data-stu-id="f1573-222">In SQL Data Warehouse the code needs to be slightly altered:</span></span>

```sql
SET NOCOUNT ON;
DECLARE @xact_state smallint = 0;

BEGIN TRAN
    BEGIN TRY
        DECLARE @i INT;
        SET     @i = CONVERT(INT,'ABC');
    END TRY
    BEGIN CATCH
        SET @xact_state = XACT_STATE();

        IF @@TRANCOUNT > 0
        BEGIN
            PRINT 'ROLLBACK';
            ROLLBACK TRAN;
        END

        SELECT  ERROR_NUMBER()    AS ErrNumber
        ,       ERROR_SEVERITY()  AS ErrSeverity
        ,       ERROR_STATE()     AS ErrState
        ,       ERROR_PROCEDURE() AS ErrProcedure
        ,       ERROR_MESSAGE()   AS ErrMessage
        ;
    END CATCH;

IF @@TRANCOUNT >0
BEGIN
    PRINT 'COMMIT';
    COMMIT TRAN;
END

SELECT @xact_state AS TransactionState;
```

<span data-ttu-id="f1573-223">Nyní je zaznamenali očekávané chování.</span><span class="sxs-lookup"><span data-stu-id="f1573-223">The expected behavior is now observed.</span></span> <span data-ttu-id="f1573-224">Chyba v transakci je spravovat a funkce ERROR_ * zadejte hodnoty, podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="f1573-224">The error in the transaction is managed and the ERROR_* functions provide values as expected.</span></span>

<span data-ttu-id="f1573-225">Všechny, které se změnily je, že `ROLLBACK` transakce museli dojít před čtení informací o chybě v `CATCH` bloku.</span><span class="sxs-lookup"><span data-stu-id="f1573-225">All that has changed is that the `ROLLBACK` of the transaction had to happen before the read of the error information in the `CATCH` block.</span></span>

## <a name="errorline-function"></a><span data-ttu-id="f1573-226">Error_Line() – funkce</span><span class="sxs-lookup"><span data-stu-id="f1573-226">Error_Line() function</span></span>
<span data-ttu-id="f1573-227">Je také vhodné poznamenat, že SQL Data Warehouse implementovat nebo podporují funkci ERROR_LINE().</span><span class="sxs-lookup"><span data-stu-id="f1573-227">It is also worth noting that SQL Data Warehouse does not implement or support the ERROR_LINE() function.</span></span> <span data-ttu-id="f1573-228">Pokud máte ve vašem kódu, budete muset odebrat tak, aby vyhovoval s SQL Data Warehouse to.</span><span class="sxs-lookup"><span data-stu-id="f1573-228">If you have this in your code you will need to remove it to be compliant with SQL Data Warehouse.</span></span> <span data-ttu-id="f1573-229">Použijte místo toho popisky dotazu v kódu k implementaci ekvivalentní funkce.</span><span class="sxs-lookup"><span data-stu-id="f1573-229">Use query labels in your code instead to implement equivalent functionality.</span></span> <span data-ttu-id="f1573-230">Podrobnosti najdete [popisek] [ LABEL] další podrobnosti o této funkci najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="f1573-230">Please refer to the [LABEL][LABEL] article for more details on this feature.</span></span>

## <a name="using-throw-and-raiserror"></a><span data-ttu-id="f1573-231">Pomocí THROW a RAISERROR</span><span class="sxs-lookup"><span data-stu-id="f1573-231">Using THROW and RAISERROR</span></span>
<span data-ttu-id="f1573-232">THROW je více moderní implementaci pro vyvolávání výjimek v SQL Data Warehouse, ale RAISERROR je také podporována.</span><span class="sxs-lookup"><span data-stu-id="f1573-232">THROW is the more modern implementation for raising exceptions in SQL Data Warehouse but RAISERROR is also supported.</span></span> <span data-ttu-id="f1573-233">Existuje několik rozdílů, které jsou vhodné důrazem ale.</span><span class="sxs-lookup"><span data-stu-id="f1573-233">There are a few differences that are worth paying attention to however.</span></span>

* <span data-ttu-id="f1573-234">Uživatelem definované chybové zprávy, které čísla nemůže být v rozsahu 100 000-150 000 pro THROW</span><span class="sxs-lookup"><span data-stu-id="f1573-234">User defined error messages numbers cannot be in the 100,000 - 150,000 range for THROW</span></span>
* <span data-ttu-id="f1573-235">Chybové zprávy RAISERROR stanoví na 50 000</span><span class="sxs-lookup"><span data-stu-id="f1573-235">RAISERROR error messages are fixed at 50,000</span></span>
* <span data-ttu-id="f1573-236">Použití systémových není podporováno.</span><span class="sxs-lookup"><span data-stu-id="f1573-236">Use of sys.messages is not supported</span></span>

## <a name="limitiations"></a><span data-ttu-id="f1573-237">Limitiations</span><span class="sxs-lookup"><span data-stu-id="f1573-237">Limitiations</span></span>
<span data-ttu-id="f1573-238">SQL Data Warehouse má několik omezení, které se týkají transakce.</span><span class="sxs-lookup"><span data-stu-id="f1573-238">SQL Data Warehouse does have a few other restrictions that relate to transactions.</span></span>

<span data-ttu-id="f1573-239">Ty jsou následující:</span><span class="sxs-lookup"><span data-stu-id="f1573-239">They are as follows:</span></span>

* <span data-ttu-id="f1573-240">Žádné distribuovaných transakcí</span><span class="sxs-lookup"><span data-stu-id="f1573-240">No distributed transactions</span></span>
* <span data-ttu-id="f1573-241">Žádné vnořené transakce povolené</span><span class="sxs-lookup"><span data-stu-id="f1573-241">No nested transactions permitted</span></span>
* <span data-ttu-id="f1573-242">Bez uložení bodů povoleno</span><span class="sxs-lookup"><span data-stu-id="f1573-242">No save points allowed</span></span>
* <span data-ttu-id="f1573-243">Žádné pojmenované transakce</span><span class="sxs-lookup"><span data-stu-id="f1573-243">No named transactions</span></span>
* <span data-ttu-id="f1573-244">Žádné označené transakce</span><span class="sxs-lookup"><span data-stu-id="f1573-244">No marked transactions</span></span>
* <span data-ttu-id="f1573-245">Žádná podpora pro DDL jako `CREATE TABLE` uvnitř uživatele definované transakce</span><span class="sxs-lookup"><span data-stu-id="f1573-245">No support for DDL such as `CREATE TABLE` inside a user defined transaction</span></span>

## <a name="next-steps"></a><span data-ttu-id="f1573-246">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f1573-246">Next steps</span></span>
<span data-ttu-id="f1573-247">Další informace o optimalizaci transakce, najdete v části [transakce osvědčené postupy][Transactions best practices].</span><span class="sxs-lookup"><span data-stu-id="f1573-247">To learn more about optimizing transactions, see [Transactions best practices][Transactions best practices].</span></span>  <span data-ttu-id="f1573-248">Další informace o ostatní osvědčené postupy pro SQL Data Warehouse najdete v tématu [osvědčené postupy pro SQL Data Warehouse][SQL Data Warehouse best practices].</span><span class="sxs-lookup"><span data-stu-id="f1573-248">To learn about other SQL Data Warehouse best practices, see [SQL Data Warehouse best practices][SQL Data Warehouse best practices].</span></span>

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md
[development overview]: ./sql-data-warehouse-overview-develop.md
[Transactions best practices]: ./sql-data-warehouse-develop-best-practices-transactions.md
[SQL Data Warehouse best practices]: ./sql-data-warehouse-best-practices.md
[LABEL]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->

<!--Other Web references-->
