---
title: aaaTransactions v SQL Data Warehouse | Microsoft Docs
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
ms.openlocfilehash: 7c541648553238443b407666612561918096eb61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="transactions-in-sql-data-warehouse"></a><span data-ttu-id="d528e-103">Transakce v SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="d528e-103">Transactions in SQL Data Warehouse</span></span>
<span data-ttu-id="d528e-104">Jak jste zvyklí, SQL Data Warehouse podporuje transakce v rámci úlohy datového skladu hello.</span><span class="sxs-lookup"><span data-stu-id="d528e-104">As you would expect, SQL Data Warehouse supports transactions as part of hello data warehouse workload.</span></span> <span data-ttu-id="d528e-105">Ale tooensure hello výkon služby SQL Data Warehouse je udržován na úrovni škálování některé funkce jsou omezené při porovnání tooSQL serveru.</span><span class="sxs-lookup"><span data-stu-id="d528e-105">However, tooensure hello performance of SQL Data Warehouse is maintained at scale some features are limited when compared tooSQL Server.</span></span> <span data-ttu-id="d528e-106">V tomto článku klade důraz hello rozdíly a seznamy hello ostatní.</span><span class="sxs-lookup"><span data-stu-id="d528e-106">This article highlights hello differences and lists hello others.</span></span> 

## <a name="transaction-isolation-levels"></a><span data-ttu-id="d528e-107">Úrovně izolace transakce</span><span class="sxs-lookup"><span data-stu-id="d528e-107">Transaction isolation levels</span></span>
<span data-ttu-id="d528e-108">SQL Data Warehouse implementuje transakce ACID.</span><span class="sxs-lookup"><span data-stu-id="d528e-108">SQL Data Warehouse implements ACID transactions.</span></span> <span data-ttu-id="d528e-109">Hello izolace podpora transakcí hello je však omezená příliš`READ UNCOMMITTED` a toto nastavení nelze změnit.</span><span class="sxs-lookup"><span data-stu-id="d528e-109">However, hello Isolation of hello transactional support is limited too`READ UNCOMMITTED` and this cannot be changed.</span></span> <span data-ttu-id="d528e-110">Můžete implementovat řadu kódování metod, které tooprevent nekonzistence čte dat, pokud se jedná o problém pro vás.</span><span class="sxs-lookup"><span data-stu-id="d528e-110">You can implement a number of coding methods tooprevent dirty reads of data if this is a concern for you.</span></span> <span data-ttu-id="d528e-111">využít funkce CTAS a přepnutí oddílu tabulky (často označované jako posuvné okno vzor hello) zprostředkovatele Hello nejoblíbenější metody tooprevent uživatele z dotazování na data, která stále připraveném.</span><span class="sxs-lookup"><span data-stu-id="d528e-111">hello most popular methods leverage both CTAS and table partition switching (often known as hello sliding window pattern) tooprevent users from querying data that is still being prepared.</span></span> <span data-ttu-id="d528e-112">Zobrazení, které předem filtrování dat hello je také oblíbených přístup.</span><span class="sxs-lookup"><span data-stu-id="d528e-112">Views that pre-filter hello data is also a popular approach.</span></span>  

## <a name="transaction-size"></a><span data-ttu-id="d528e-113">Velikost transakce</span><span class="sxs-lookup"><span data-stu-id="d528e-113">Transaction size</span></span>
<span data-ttu-id="d528e-114">Transakce úpravy jednoho datového je omezen velikostí.</span><span class="sxs-lookup"><span data-stu-id="d528e-114">A single data modification transaction is limited in size.</span></span> <span data-ttu-id="d528e-115">Hello limit dnes platí "za distribuční".</span><span class="sxs-lookup"><span data-stu-id="d528e-115">hello limit today is applied "per distribution".</span></span> <span data-ttu-id="d528e-116">Proto lze vypočítat celkové přidělení hello vynásobením hello limit počtu distribučních hello.</span><span class="sxs-lookup"><span data-stu-id="d528e-116">Therefore, hello total allocation can be calculated by multiplying hello limit by hello distribution count.</span></span> <span data-ttu-id="d528e-117">tooapproximate hello maximální počet řádků v transakci hello dělení hello distribuční cap hello celková velikost každý řádek.</span><span class="sxs-lookup"><span data-stu-id="d528e-117">tooapproximate hello maximum number of rows in hello transaction divide hello distribution cap by hello total size of each row.</span></span> <span data-ttu-id="d528e-118">Pro proměnnou délkou zvažte trvá délka průměrná sloupce, nikoli pomocí hello maximální velikost.</span><span class="sxs-lookup"><span data-stu-id="d528e-118">For variable length columns consider taking an average column length rather than using hello maximum size.</span></span>

<span data-ttu-id="d528e-119">V tabulce hello hello byly provedeny následující předpoklady:</span><span class="sxs-lookup"><span data-stu-id="d528e-119">In hello table below hello following assumptions have been made:</span></span>

* <span data-ttu-id="d528e-120">Rovnoměrné rozdělení dat došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="d528e-120">An even distribution of data has occurred</span></span> 
* <span data-ttu-id="d528e-121">délka řádku průměrná Hello je 250 bajtů</span><span class="sxs-lookup"><span data-stu-id="d528e-121">hello average row length is 250 bytes</span></span>

| <span data-ttu-id="d528e-122">[DWU][DWU]</span><span class="sxs-lookup"><span data-stu-id="d528e-122">[DWU][DWU]</span></span> | <span data-ttu-id="d528e-123">Cap za distribuční (GiB)</span><span class="sxs-lookup"><span data-stu-id="d528e-123">Cap per distribution (GiB)</span></span> | <span data-ttu-id="d528e-124">Počet distribuce</span><span class="sxs-lookup"><span data-stu-id="d528e-124">Number of Distributions</span></span> | <span data-ttu-id="d528e-125">MAXIMÁLNÍ velikost transakce (GiB)</span><span class="sxs-lookup"><span data-stu-id="d528e-125">MAX transaction size (GiB)</span></span> | <span data-ttu-id="d528e-126"># Řádků na jeden distribuce</span><span class="sxs-lookup"><span data-stu-id="d528e-126"># Rows per distribution</span></span> | <span data-ttu-id="d528e-127">Maximální počet řádků na transakci</span><span class="sxs-lookup"><span data-stu-id="d528e-127">Max Rows per transaction</span></span> |
| --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="d528e-128">OD DW100</span><span class="sxs-lookup"><span data-stu-id="d528e-128">DW100</span></span> |<span data-ttu-id="d528e-129">1</span><span class="sxs-lookup"><span data-stu-id="d528e-129">1</span></span> |<span data-ttu-id="d528e-130">60</span><span class="sxs-lookup"><span data-stu-id="d528e-130">60</span></span> |<span data-ttu-id="d528e-131">60</span><span class="sxs-lookup"><span data-stu-id="d528e-131">60</span></span> |<span data-ttu-id="d528e-132">4,000,000</span><span class="sxs-lookup"><span data-stu-id="d528e-132">4,000,000</span></span> |<span data-ttu-id="d528e-133">240,000,000</span><span class="sxs-lookup"><span data-stu-id="d528e-133">240,000,000</span></span> |
| <span data-ttu-id="d528e-134">DW200</span><span class="sxs-lookup"><span data-stu-id="d528e-134">DW200</span></span> |<span data-ttu-id="d528e-135">1.5</span><span class="sxs-lookup"><span data-stu-id="d528e-135">1.5</span></span> |<span data-ttu-id="d528e-136">60</span><span class="sxs-lookup"><span data-stu-id="d528e-136">60</span></span> |<span data-ttu-id="d528e-137">90</span><span class="sxs-lookup"><span data-stu-id="d528e-137">90</span></span> |<span data-ttu-id="d528e-138">6 000 000</span><span class="sxs-lookup"><span data-stu-id="d528e-138">6,000,000</span></span> |<span data-ttu-id="d528e-139">360,000,000</span><span class="sxs-lookup"><span data-stu-id="d528e-139">360,000,000</span></span> |
| <span data-ttu-id="d528e-140">DW300</span><span class="sxs-lookup"><span data-stu-id="d528e-140">DW300</span></span> |<span data-ttu-id="d528e-141">2.25</span><span class="sxs-lookup"><span data-stu-id="d528e-141">2.25</span></span> |<span data-ttu-id="d528e-142">60</span><span class="sxs-lookup"><span data-stu-id="d528e-142">60</span></span> |<span data-ttu-id="d528e-143">135</span><span class="sxs-lookup"><span data-stu-id="d528e-143">135</span></span> |<span data-ttu-id="d528e-144">9,000,000</span><span class="sxs-lookup"><span data-stu-id="d528e-144">9,000,000</span></span> |<span data-ttu-id="d528e-145">540,000,000</span><span class="sxs-lookup"><span data-stu-id="d528e-145">540,000,000</span></span> |
| <span data-ttu-id="d528e-146">DW400</span><span class="sxs-lookup"><span data-stu-id="d528e-146">DW400</span></span> |<span data-ttu-id="d528e-147">3</span><span class="sxs-lookup"><span data-stu-id="d528e-147">3</span></span> |<span data-ttu-id="d528e-148">60</span><span class="sxs-lookup"><span data-stu-id="d528e-148">60</span></span> |<span data-ttu-id="d528e-149">180</span><span class="sxs-lookup"><span data-stu-id="d528e-149">180</span></span> |<span data-ttu-id="d528e-150">12,000,000</span><span class="sxs-lookup"><span data-stu-id="d528e-150">12,000,000</span></span> |<span data-ttu-id="d528e-151">720,000,000</span><span class="sxs-lookup"><span data-stu-id="d528e-151">720,000,000</span></span> |
| <span data-ttu-id="d528e-152">DW500</span><span class="sxs-lookup"><span data-stu-id="d528e-152">DW500</span></span> |<span data-ttu-id="d528e-153">3.75</span><span class="sxs-lookup"><span data-stu-id="d528e-153">3.75</span></span> |<span data-ttu-id="d528e-154">60</span><span class="sxs-lookup"><span data-stu-id="d528e-154">60</span></span> |<span data-ttu-id="d528e-155">225</span><span class="sxs-lookup"><span data-stu-id="d528e-155">225</span></span> |<span data-ttu-id="d528e-156">15,000,000</span><span class="sxs-lookup"><span data-stu-id="d528e-156">15,000,000</span></span> |<span data-ttu-id="d528e-157">900,000,000</span><span class="sxs-lookup"><span data-stu-id="d528e-157">900,000,000</span></span> |
| <span data-ttu-id="d528e-158">DW600</span><span class="sxs-lookup"><span data-stu-id="d528e-158">DW600</span></span> |<span data-ttu-id="d528e-159">4.5</span><span class="sxs-lookup"><span data-stu-id="d528e-159">4.5</span></span> |<span data-ttu-id="d528e-160">60</span><span class="sxs-lookup"><span data-stu-id="d528e-160">60</span></span> |<span data-ttu-id="d528e-161">270</span><span class="sxs-lookup"><span data-stu-id="d528e-161">270</span></span> |<span data-ttu-id="d528e-162">18,000,000</span><span class="sxs-lookup"><span data-stu-id="d528e-162">18,000,000</span></span> |<span data-ttu-id="d528e-163">1,080,000,000</span><span class="sxs-lookup"><span data-stu-id="d528e-163">1,080,000,000</span></span> |
| <span data-ttu-id="d528e-164">DW1000</span><span class="sxs-lookup"><span data-stu-id="d528e-164">DW1000</span></span> |<span data-ttu-id="d528e-165">7.5</span><span class="sxs-lookup"><span data-stu-id="d528e-165">7.5</span></span> |<span data-ttu-id="d528e-166">60</span><span class="sxs-lookup"><span data-stu-id="d528e-166">60</span></span> |<span data-ttu-id="d528e-167">450</span><span class="sxs-lookup"><span data-stu-id="d528e-167">450</span></span> |<span data-ttu-id="d528e-168">30,000,000</span><span class="sxs-lookup"><span data-stu-id="d528e-168">30,000,000</span></span> |<span data-ttu-id="d528e-169">1,800,000,000</span><span class="sxs-lookup"><span data-stu-id="d528e-169">1,800,000,000</span></span> |
| <span data-ttu-id="d528e-170">DW1200</span><span class="sxs-lookup"><span data-stu-id="d528e-170">DW1200</span></span> |<span data-ttu-id="d528e-171">9</span><span class="sxs-lookup"><span data-stu-id="d528e-171">9</span></span> |<span data-ttu-id="d528e-172">60</span><span class="sxs-lookup"><span data-stu-id="d528e-172">60</span></span> |<span data-ttu-id="d528e-173">540</span><span class="sxs-lookup"><span data-stu-id="d528e-173">540</span></span> |<span data-ttu-id="d528e-174">36,000,000</span><span class="sxs-lookup"><span data-stu-id="d528e-174">36,000,000</span></span> |<span data-ttu-id="d528e-175">2,160,000,000</span><span class="sxs-lookup"><span data-stu-id="d528e-175">2,160,000,000</span></span> |
| <span data-ttu-id="d528e-176">DW1500</span><span class="sxs-lookup"><span data-stu-id="d528e-176">DW1500</span></span> |<span data-ttu-id="d528e-177">11.25</span><span class="sxs-lookup"><span data-stu-id="d528e-177">11.25</span></span> |<span data-ttu-id="d528e-178">60</span><span class="sxs-lookup"><span data-stu-id="d528e-178">60</span></span> |<span data-ttu-id="d528e-179">675</span><span class="sxs-lookup"><span data-stu-id="d528e-179">675</span></span> |<span data-ttu-id="d528e-180">45,000,000</span><span class="sxs-lookup"><span data-stu-id="d528e-180">45,000,000</span></span> |<span data-ttu-id="d528e-181">2,700,000,000</span><span class="sxs-lookup"><span data-stu-id="d528e-181">2,700,000,000</span></span> |
| <span data-ttu-id="d528e-182">DW2000</span><span class="sxs-lookup"><span data-stu-id="d528e-182">DW2000</span></span> |<span data-ttu-id="d528e-183">15</span><span class="sxs-lookup"><span data-stu-id="d528e-183">15</span></span> |<span data-ttu-id="d528e-184">60</span><span class="sxs-lookup"><span data-stu-id="d528e-184">60</span></span> |<span data-ttu-id="d528e-185">900</span><span class="sxs-lookup"><span data-stu-id="d528e-185">900</span></span> |<span data-ttu-id="d528e-186">60,000,000</span><span class="sxs-lookup"><span data-stu-id="d528e-186">60,000,000</span></span> |<span data-ttu-id="d528e-187">3,600,000,000</span><span class="sxs-lookup"><span data-stu-id="d528e-187">3,600,000,000</span></span> |
| <span data-ttu-id="d528e-188">DW3000</span><span class="sxs-lookup"><span data-stu-id="d528e-188">DW3000</span></span> |<span data-ttu-id="d528e-189">22.5</span><span class="sxs-lookup"><span data-stu-id="d528e-189">22.5</span></span> |<span data-ttu-id="d528e-190">60</span><span class="sxs-lookup"><span data-stu-id="d528e-190">60</span></span> |<span data-ttu-id="d528e-191">1,350</span><span class="sxs-lookup"><span data-stu-id="d528e-191">1,350</span></span> |<span data-ttu-id="d528e-192">90,000,000</span><span class="sxs-lookup"><span data-stu-id="d528e-192">90,000,000</span></span> |<span data-ttu-id="d528e-193">5,400,000,000</span><span class="sxs-lookup"><span data-stu-id="d528e-193">5,400,000,000</span></span> |
| <span data-ttu-id="d528e-194">DW6000</span><span class="sxs-lookup"><span data-stu-id="d528e-194">DW6000</span></span> |<span data-ttu-id="d528e-195">45</span><span class="sxs-lookup"><span data-stu-id="d528e-195">45</span></span> |<span data-ttu-id="d528e-196">60</span><span class="sxs-lookup"><span data-stu-id="d528e-196">60</span></span> |<span data-ttu-id="d528e-197">2,700</span><span class="sxs-lookup"><span data-stu-id="d528e-197">2,700</span></span> |<span data-ttu-id="d528e-198">180,000,000</span><span class="sxs-lookup"><span data-stu-id="d528e-198">180,000,000</span></span> |<span data-ttu-id="d528e-199">10,800,000,000</span><span class="sxs-lookup"><span data-stu-id="d528e-199">10,800,000,000</span></span> |

<span data-ttu-id="d528e-200">omezení velikosti transakce Hello se použije pro transakci nebo operace.</span><span class="sxs-lookup"><span data-stu-id="d528e-200">hello transaction size limit is applied per transaction or operation.</span></span> <span data-ttu-id="d528e-201">Nebude použito v rámci všech souběžných transakcí.</span><span class="sxs-lookup"><span data-stu-id="d528e-201">It is not applied across all concurrent transactions.</span></span> <span data-ttu-id="d528e-202">Proto každou transakci je povolené toowrite toto množství dat toohello protokolu.</span><span class="sxs-lookup"><span data-stu-id="d528e-202">Therefore each transaction is permitted toowrite this amount of data toohello log.</span></span> 

<span data-ttu-id="d528e-203">toooptimize a minimalizovat hello množství dat zapsaných toohello protokolu naleznete toohello [transakce osvědčené postupy] [ Transactions best practices] článku.</span><span class="sxs-lookup"><span data-stu-id="d528e-203">toooptimize and minimize hello amount of data written toohello log please refer toohello [Transactions best practices][Transactions best practices] article.</span></span>

> [!WARNING]
> <span data-ttu-id="d528e-204">maximální velikost transakce lze dosáhnout pouze pro hodnoty HASH nebo kde hello šířit z tabulky ROUND_ROBIN distribuované hello data Hello je i.</span><span class="sxs-lookup"><span data-stu-id="d528e-204">hello maximum transaction size can only be achieved for HASH or ROUND_ROBIN distributed tables where hello spread of hello data is even.</span></span> <span data-ttu-id="d528e-205">Pokud transakce hello je zápis dat zkreslilo způsobem toohello distribuce hello limit je pravděpodobně toobe nedosáhla předchozí toohello transakce maximální velikosti.</span><span class="sxs-lookup"><span data-stu-id="d528e-205">If hello transaction is writing data in a skewed fashion toohello distributions then hello limit is likely toobe reached prior toohello maximum transaction size.</span></span>
> <!--REPLICATED_TABLE-->
> 
> 

## <a name="transaction-state"></a><span data-ttu-id="d528e-206">Stav transakce</span><span class="sxs-lookup"><span data-stu-id="d528e-206">Transaction state</span></span>
<span data-ttu-id="d528e-207">SQL Data Warehouse používá hello XACT_STATE() funkce tooreport selhání transakce pomocí hodnoty hello -2.</span><span class="sxs-lookup"><span data-stu-id="d528e-207">SQL Data Warehouse uses hello XACT_STATE() function tooreport a failed transaction using hello value -2.</span></span> <span data-ttu-id="d528e-208">To znamená, že hello transakce se nezdařilo a je označen pro vrácení zpět pouze</span><span class="sxs-lookup"><span data-stu-id="d528e-208">This means that hello transaction has failed and is marked for rollback only</span></span>

> [!NOTE]
> <span data-ttu-id="d528e-209">Hello použít-2 pomocí hello XACT_STATE funkce toodenote selhání transakce představuje různé chování tooSQL serveru.</span><span class="sxs-lookup"><span data-stu-id="d528e-209">hello use of -2 by hello XACT_STATE function toodenote a failed transaction represents different behavior tooSQL Server.</span></span> <span data-ttu-id="d528e-210">Používá systém SQL Server hello hodnota -1 toorepresent rámci transakce.</span><span class="sxs-lookup"><span data-stu-id="d528e-210">SQL Server uses hello value -1 toorepresent an un-committable transaction.</span></span> <span data-ttu-id="d528e-211">SQL Server může tolerovat chybami v transakci bez nutnosti toobe označena jako rámci.</span><span class="sxs-lookup"><span data-stu-id="d528e-211">SQL Server can tolerate some errors inside a transaction without it having toobe marked as un-committable.</span></span> <span data-ttu-id="d528e-212">Například `SELECT 1/0` by způsobit chybu, ale bez vynucení transakce v rámci stavu.</span><span class="sxs-lookup"><span data-stu-id="d528e-212">For example `SELECT 1/0` would cause an error but not force a transaction into an un-committable state.</span></span> <span data-ttu-id="d528e-213">Systému SQL Server také umožňuje čtení v rámci transakce hello.</span><span class="sxs-lookup"><span data-stu-id="d528e-213">SQL Server also permits reads in hello un-committable transaction.</span></span> <span data-ttu-id="d528e-214">Ale SQL Data Warehouse, nebudou vám to.</span><span class="sxs-lookup"><span data-stu-id="d528e-214">However, SQL Data Warehouse does not let you do this.</span></span> <span data-ttu-id="d528e-215">Pokud dojde k chybě v transakci SQL Data Warehouse zadá automaticky stavu hello -2 a nebude ji již možné toomake všechny příkazy další výběr, dokud příkaz hello byla vrácena zpět.</span><span class="sxs-lookup"><span data-stu-id="d528e-215">If an error occurs inside a SQL Data Warehouse transaction it will automatically enter hello -2 state and you will not be able toomake any further select statements until hello statement has been rolled back.</span></span> <span data-ttu-id="d528e-216">Proto je důležité toocheck, že vaše toosee kód aplikace, pokud používá XACT_STATE() podle může být nutné změny kódu toomake.</span><span class="sxs-lookup"><span data-stu-id="d528e-216">It is therefore important toocheck that your application code toosee if it uses  XACT_STATE() as you may need toomake code modifications.</span></span>
> 
> 

<span data-ttu-id="d528e-217">V systému SQL Server se může zobrazit transakci, která vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="d528e-217">For example, in SQL Server you might see a transaction that looks like this:</span></span>

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

<span data-ttu-id="d528e-218">Pokud necháte kódu, protože je nad obdržíte hello následující chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="d528e-218">If you leave your code as it is above then you will get hello following error message:</span></span>

<span data-ttu-id="d528e-219">Msg 111233, Level 16, State 1, řádek 1 111233; hello aktuální transakce byla přerušena a všechny neuložené změny byly vráceny zpět.</span><span class="sxs-lookup"><span data-stu-id="d528e-219">Msg 111233, Level 16, State 1, Line 1 111233;hello current transaction has aborted, and any pending changes have been rolled back.</span></span> <span data-ttu-id="d528e-220">Příčina: Transakce ve stavu jen pro vrácení zpět nebyla vrácena explicitně před DDL, DML nebo příkazu SELECT.</span><span class="sxs-lookup"><span data-stu-id="d528e-220">Cause: A transaction in a rollback-only state was not explicitly rolled back before a DDL, DML or SELECT statement.</span></span>

<span data-ttu-id="d528e-221">Také nebude získat výstup hello hello ERROR_ * funkcí.</span><span class="sxs-lookup"><span data-stu-id="d528e-221">You will also not get hello output of hello ERROR_* functions.</span></span>

<span data-ttu-id="d528e-222">V SQL Data Warehouse potřebuje hello kód toobe mírně změnit:</span><span class="sxs-lookup"><span data-stu-id="d528e-222">In SQL Data Warehouse hello code needs toobe slightly altered:</span></span>

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

<span data-ttu-id="d528e-223">Hello očekává, že je nyní zjištěnými chování.</span><span class="sxs-lookup"><span data-stu-id="d528e-223">hello expected behavior is now observed.</span></span> <span data-ttu-id="d528e-224">Hello došlo k chybě v transakci hello je spravovat a hello ERROR_ * funkce zadejte hodnoty, podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="d528e-224">hello error in hello transaction is managed and hello ERROR_* functions provide values as expected.</span></span>

<span data-ttu-id="d528e-225">Všechno, co došlo ke změně této hello je `ROLLBACK` z hello transakce měl toohappen než hello čtení hello informací o chybách v hello `CATCH` bloku.</span><span class="sxs-lookup"><span data-stu-id="d528e-225">All that has changed is that hello `ROLLBACK` of hello transaction had toohappen before hello read of hello error information in hello `CATCH` block.</span></span>

## <a name="errorline-function"></a><span data-ttu-id="d528e-226">Error_Line() – funkce</span><span class="sxs-lookup"><span data-stu-id="d528e-226">Error_Line() function</span></span>
<span data-ttu-id="d528e-227">Je také vhodné poznamenat, že SQL Data Warehouse implementovat nebo podporují funkce ERROR_LINE() hello.</span><span class="sxs-lookup"><span data-stu-id="d528e-227">It is also worth noting that SQL Data Warehouse does not implement or support hello ERROR_LINE() function.</span></span> <span data-ttu-id="d528e-228">Pokud máte ve vašem kódu to budete potřebovat tooremove ho toobe kompatibilní s SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="d528e-228">If you have this in your code you will need tooremove it toobe compliant with SQL Data Warehouse.</span></span> <span data-ttu-id="d528e-229">Místo toho použijte popisky dotazu v kódu tooimplement ekvivalentní funkce.</span><span class="sxs-lookup"><span data-stu-id="d528e-229">Use query labels in your code instead tooimplement equivalent functionality.</span></span> <span data-ttu-id="d528e-230">Podrobnosti najdete toohello [popisek] [ LABEL] další podrobnosti o této funkci najdete v článku.</span><span class="sxs-lookup"><span data-stu-id="d528e-230">Please refer toohello [LABEL][LABEL] article for more details on this feature.</span></span>

## <a name="using-throw-and-raiserror"></a><span data-ttu-id="d528e-231">Pomocí THROW a RAISERROR</span><span class="sxs-lookup"><span data-stu-id="d528e-231">Using THROW and RAISERROR</span></span>
<span data-ttu-id="d528e-232">THROW je hello více moderní implementace pro vyvolávání výjimek v SQL Data Warehouse, ale RAISERROR je také podporována.</span><span class="sxs-lookup"><span data-stu-id="d528e-232">THROW is hello more modern implementation for raising exceptions in SQL Data Warehouse but RAISERROR is also supported.</span></span> <span data-ttu-id="d528e-233">Existuje několik rozdílů, které jsou vhodné platícího toohowever pozornost.</span><span class="sxs-lookup"><span data-stu-id="d528e-233">There are a few differences that are worth paying attention toohowever.</span></span>

* <span data-ttu-id="d528e-234">Uživatelem definované chybové zprávy, že čísla nemůže být v hello 150 100 000 000 rozsah THROW</span><span class="sxs-lookup"><span data-stu-id="d528e-234">User defined error messages numbers cannot be in hello 100,000 - 150,000 range for THROW</span></span>
* <span data-ttu-id="d528e-235">Chybové zprávy RAISERROR stanoví na 50 000</span><span class="sxs-lookup"><span data-stu-id="d528e-235">RAISERROR error messages are fixed at 50,000</span></span>
* <span data-ttu-id="d528e-236">Použití systémových není podporováno.</span><span class="sxs-lookup"><span data-stu-id="d528e-236">Use of sys.messages is not supported</span></span>

## <a name="limitiations"></a><span data-ttu-id="d528e-237">Limitiations</span><span class="sxs-lookup"><span data-stu-id="d528e-237">Limitiations</span></span>
<span data-ttu-id="d528e-238">SQL Data Warehouse má několik omezení, které se týkají tootransactions.</span><span class="sxs-lookup"><span data-stu-id="d528e-238">SQL Data Warehouse does have a few other restrictions that relate tootransactions.</span></span>

<span data-ttu-id="d528e-239">Ty jsou následující:</span><span class="sxs-lookup"><span data-stu-id="d528e-239">They are as follows:</span></span>

* <span data-ttu-id="d528e-240">Žádné distribuovaných transakcí</span><span class="sxs-lookup"><span data-stu-id="d528e-240">No distributed transactions</span></span>
* <span data-ttu-id="d528e-241">Žádné vnořené transakce povolené</span><span class="sxs-lookup"><span data-stu-id="d528e-241">No nested transactions permitted</span></span>
* <span data-ttu-id="d528e-242">Bez uložení bodů povoleno</span><span class="sxs-lookup"><span data-stu-id="d528e-242">No save points allowed</span></span>
* <span data-ttu-id="d528e-243">Žádné pojmenované transakce</span><span class="sxs-lookup"><span data-stu-id="d528e-243">No named transactions</span></span>
* <span data-ttu-id="d528e-244">Žádné označené transakce</span><span class="sxs-lookup"><span data-stu-id="d528e-244">No marked transactions</span></span>
* <span data-ttu-id="d528e-245">Žádná podpora pro DDL jako `CREATE TABLE` uvnitř uživatele definované transakce</span><span class="sxs-lookup"><span data-stu-id="d528e-245">No support for DDL such as `CREATE TABLE` inside a user defined transaction</span></span>

## <a name="next-steps"></a><span data-ttu-id="d528e-246">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d528e-246">Next steps</span></span>
<span data-ttu-id="d528e-247">toolearn Další informace o optimalizaci transakce, najdete v části [transakce osvědčené postupy][Transactions best practices].</span><span class="sxs-lookup"><span data-stu-id="d528e-247">toolearn more about optimizing transactions, see [Transactions best practices][Transactions best practices].</span></span>  <span data-ttu-id="d528e-248">toolearn o ostatní osvědčené postupy SQL Data Warehouse, najdete v části [osvědčené postupy pro SQL Data Warehouse][SQL Data Warehouse best practices].</span><span class="sxs-lookup"><span data-stu-id="d528e-248">toolearn about other SQL Data Warehouse best practices, see [SQL Data Warehouse best practices][SQL Data Warehouse best practices].</span></span>

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md
[development overview]: ./sql-data-warehouse-overview-develop.md
[Transactions best practices]: ./sql-data-warehouse-develop-best-practices-transactions.md
[SQL Data Warehouse best practices]: ./sql-data-warehouse-best-practices.md
[LABEL]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->

<!--Other Web references-->
