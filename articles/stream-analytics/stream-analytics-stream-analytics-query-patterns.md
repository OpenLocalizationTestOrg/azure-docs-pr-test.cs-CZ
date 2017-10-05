---
title: "Dotaz příkladů běžných vzorů využití v Stream Analytics | Microsoft Docs"
description: "Běžné typy dotazů Azure Stream Analytics"
keywords: "Příklady dotazů"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jenniehubbard
editor: cgronlun
ms.assetid: 6b9a7d00-fbcc-42f6-9cbb-8bbf0bbd3d0e
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/08/2017
ms.author: jenniehubbard
ms.openlocfilehash: a00855c200b3fb365073bad4c5773b02c4c2c7fe
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="query-examples-for-common-stream-analytics-usage-patterns"></a><span data-ttu-id="1e47c-104">Dotaz příkladů běžných vzorů využití Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="1e47c-104">Query examples for common Stream Analytics usage patterns</span></span>
## <a name="introduction"></a><span data-ttu-id="1e47c-105">Úvod</span><span class="sxs-lookup"><span data-stu-id="1e47c-105">Introduction</span></span>
<span data-ttu-id="1e47c-106">Dotazy v Azure Stream Analytics jsou vyjádřeny v jazyce SQL jako dotaz.</span><span class="sxs-lookup"><span data-stu-id="1e47c-106">Queries in Azure Stream Analytics are expressed in a SQL-like query language.</span></span> <span data-ttu-id="1e47c-107">Tyto dotazy jsou dokumentovány v článku [Stream Analytics query referenční informace k jazyku](https://msdn.microsoft.com/library/azure/dn834998.aspx) průvodce.</span><span class="sxs-lookup"><span data-stu-id="1e47c-107">These queries are documented in the [Stream Analytics query language reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) guide.</span></span> <span data-ttu-id="1e47c-108">Tento článek popisuje řešení několik běžné typy dotazů, na základě reálného scénářů.</span><span class="sxs-lookup"><span data-stu-id="1e47c-108">This article outlines solutions to several common query patterns, based on real-world scenarios.</span></span> <span data-ttu-id="1e47c-109">Je pracuje a bude aktualizován s novou vzory průběžně.</span><span class="sxs-lookup"><span data-stu-id="1e47c-109">It is a work in progress and continues to be updated with new patterns on an ongoing basis.</span></span>

## <a name="query-example-convert-data-types"></a><span data-ttu-id="1e47c-110">Příklad dotazu: Převést datové typy</span><span class="sxs-lookup"><span data-stu-id="1e47c-110">Query example: Convert data types</span></span>
<span data-ttu-id="1e47c-111">**Popis**: definování typů vlastností v vstupního datového proudu.</span><span class="sxs-lookup"><span data-stu-id="1e47c-111">**Description**: Define the types of properties on the input stream.</span></span>
<span data-ttu-id="1e47c-112">Například váhy car pochází na vstupního datového proudu jako řetězce a je třeba má být převeden na **INT** k provedení **součet** ho nahoru.</span><span class="sxs-lookup"><span data-stu-id="1e47c-112">For example, the car weight is coming on the input stream as strings and needs to be converted to **INT** to perform **SUM** it up.</span></span>

<span data-ttu-id="1e47c-113">**Vstup**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-113">**Input**:</span></span>

| <span data-ttu-id="1e47c-114">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="1e47c-114">Make</span></span> | <span data-ttu-id="1e47c-115">Čas</span><span class="sxs-lookup"><span data-stu-id="1e47c-115">Time</span></span> | <span data-ttu-id="1e47c-116">Váha</span><span class="sxs-lookup"><span data-stu-id="1e47c-116">Weight</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1e47c-117">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-117">Honda</span></span> |<span data-ttu-id="1e47c-118">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-118">2015-01-01T00:00:01.0000000Z</span></span> |<span data-ttu-id="1e47c-119">"1000"</span><span class="sxs-lookup"><span data-stu-id="1e47c-119">"1000"</span></span> |
| <span data-ttu-id="1e47c-120">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-120">Honda</span></span> |<span data-ttu-id="1e47c-121">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-121">2015-01-01T00:00:02.0000000Z</span></span> |<span data-ttu-id="1e47c-122">"2000"</span><span class="sxs-lookup"><span data-stu-id="1e47c-122">"2000"</span></span> |

<span data-ttu-id="1e47c-123">**Výstup**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-123">**Output**:</span></span>

| <span data-ttu-id="1e47c-124">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="1e47c-124">Make</span></span> | <span data-ttu-id="1e47c-125">Váha</span><span class="sxs-lookup"><span data-stu-id="1e47c-125">Weight</span></span> |
| --- | --- |
| <span data-ttu-id="1e47c-126">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-126">Honda</span></span> |<span data-ttu-id="1e47c-127">3000</span><span class="sxs-lookup"><span data-stu-id="1e47c-127">3000</span></span> |

<span data-ttu-id="1e47c-128">**Řešení**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-128">**Solution**:</span></span>

    SELECT
        Make,
        SUM(CAST(Weight AS BIGINT)) AS Weight
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

<span data-ttu-id="1e47c-129">**Vysvětlení**: použití **PŘETYPOVÁNÍ** příkaz v **váhy** pro zadání jeho datového typu pole.</span><span class="sxs-lookup"><span data-stu-id="1e47c-129">**Explanation**: Use a **CAST** statement in the **Weight** field to specify its data type.</span></span> <span data-ttu-id="1e47c-130">Zobrazit seznam podporované datové typy v [datové typy (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835065.aspx).</span><span class="sxs-lookup"><span data-stu-id="1e47c-130">See the list of supported data types in [Data types (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835065.aspx).</span></span>

## <a name="query-example-use-likenot-like-to-do-pattern-matching"></a><span data-ttu-id="1e47c-131">Příklad dotazu: použijte jako nebo nebyla chtěli vzor odpovídající</span><span class="sxs-lookup"><span data-stu-id="1e47c-131">Query example: Use Like/Not like to do pattern matching</span></span>
<span data-ttu-id="1e47c-132">**Popis**: Zkontrolujte, jestli hodnotu pole na události odpovídá určitého.</span><span class="sxs-lookup"><span data-stu-id="1e47c-132">**Description**: Check that a field value on the event matches a certain pattern.</span></span>
<span data-ttu-id="1e47c-133">Například zkontrolujte, že výsledek vrátí desky licencí, které se začínat a končit 9.</span><span class="sxs-lookup"><span data-stu-id="1e47c-133">For example, check that the result returns license plates that start with A and end with 9.</span></span>

<span data-ttu-id="1e47c-134">**Vstup**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-134">**Input**:</span></span>

| <span data-ttu-id="1e47c-135">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="1e47c-135">Make</span></span> | <span data-ttu-id="1e47c-136">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="1e47c-136">LicensePlate</span></span> | <span data-ttu-id="1e47c-137">Čas</span><span class="sxs-lookup"><span data-stu-id="1e47c-137">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1e47c-138">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-138">Honda</span></span> |<span data-ttu-id="1e47c-139">ABC 123</span><span class="sxs-lookup"><span data-stu-id="1e47c-139">ABC-123</span></span> |<span data-ttu-id="1e47c-140">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-140">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="1e47c-141">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-141">Toyota</span></span> |<span data-ttu-id="1e47c-142">AAA-999</span><span class="sxs-lookup"><span data-stu-id="1e47c-142">AAA-999</span></span> |<span data-ttu-id="1e47c-143">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-143">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="1e47c-144">Nissan</span><span class="sxs-lookup"><span data-stu-id="1e47c-144">Nissan</span></span> |<span data-ttu-id="1e47c-145">ABC 369</span><span class="sxs-lookup"><span data-stu-id="1e47c-145">ABC-369</span></span> |<span data-ttu-id="1e47c-146">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-146">2015-01-01T00:00:03.0000000Z</span></span> |

<span data-ttu-id="1e47c-147">**Výstup**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-147">**Output**:</span></span>

| <span data-ttu-id="1e47c-148">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="1e47c-148">Make</span></span> | <span data-ttu-id="1e47c-149">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="1e47c-149">LicensePlate</span></span> | <span data-ttu-id="1e47c-150">Čas</span><span class="sxs-lookup"><span data-stu-id="1e47c-150">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1e47c-151">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-151">Toyota</span></span> |<span data-ttu-id="1e47c-152">AAA-999</span><span class="sxs-lookup"><span data-stu-id="1e47c-152">AAA-999</span></span> |<span data-ttu-id="1e47c-153">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-153">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="1e47c-154">Nissan</span><span class="sxs-lookup"><span data-stu-id="1e47c-154">Nissan</span></span> |<span data-ttu-id="1e47c-155">ABC 369</span><span class="sxs-lookup"><span data-stu-id="1e47c-155">ABC-369</span></span> |<span data-ttu-id="1e47c-156">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-156">2015-01-01T00:00:03.0000000Z</span></span> |

<span data-ttu-id="1e47c-157">**Řešení**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-157">**Solution**:</span></span>

    SELECT
        *
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LicensePlate LIKE 'A%9'

<span data-ttu-id="1e47c-158">**Vysvětlení**: použití **jako** příkazu ke kontrole **LicensePlate** pole hodnota.</span><span class="sxs-lookup"><span data-stu-id="1e47c-158">**Explanation**: Use the **LIKE** statement to check the **LicensePlate** field value.</span></span> <span data-ttu-id="1e47c-159">By měl začínat A potom mít libovolný řetězec nula nebo více znaků a pak končit 9.</span><span class="sxs-lookup"><span data-stu-id="1e47c-159">It should start with an A, then have any string of zero or more characters, and then end with a 9.</span></span> 

## <a name="query-example-specify-logic-for-different-casesvalues-case-statements"></a><span data-ttu-id="1e47c-160">Příklad dotazu: Zadejte logiku pro různé případech nebo hodnoty (CASE – příkazy)</span><span class="sxs-lookup"><span data-stu-id="1e47c-160">Query example: Specify logic for different cases/values (CASE statements)</span></span>
<span data-ttu-id="1e47c-161">**Popis**: Zadejte jiný výpočet pro pole, v závislosti na konkrétní kritérium.</span><span class="sxs-lookup"><span data-stu-id="1e47c-161">**Description**: Provide a different computation for a field, based on a particular criterion.</span></span>
<span data-ttu-id="1e47c-162">Zadejte například, že řetězec popis Ujistěte se, kolik aut stejného byla dokončena s ve speciálním případě pro 1.</span><span class="sxs-lookup"><span data-stu-id="1e47c-162">For example, provide a string description for how many cars of the same make passed, with a special case for 1.</span></span>

<span data-ttu-id="1e47c-163">**Vstup**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-163">**Input**:</span></span>

| <span data-ttu-id="1e47c-164">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="1e47c-164">Make</span></span> | <span data-ttu-id="1e47c-165">Čas</span><span class="sxs-lookup"><span data-stu-id="1e47c-165">Time</span></span> |
| --- | --- |
| <span data-ttu-id="1e47c-166">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-166">Honda</span></span> |<span data-ttu-id="1e47c-167">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-167">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="1e47c-168">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-168">Toyota</span></span> |<span data-ttu-id="1e47c-169">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-169">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="1e47c-170">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-170">Toyota</span></span> |<span data-ttu-id="1e47c-171">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-171">2015-01-01T00:00:03.0000000Z</span></span> |

<span data-ttu-id="1e47c-172">**Výstup**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-172">**Output**:</span></span>

| <span data-ttu-id="1e47c-173">CarsPassed</span><span class="sxs-lookup"><span data-stu-id="1e47c-173">CarsPassed</span></span> | <span data-ttu-id="1e47c-174">Čas</span><span class="sxs-lookup"><span data-stu-id="1e47c-174">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1e47c-175">1 Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-175">1 Honda</span></span> |<span data-ttu-id="1e47c-176">2015-01-01T00:00:10.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-176">2015-01-01T00:00:10.0000000Z</span></span> |
| <span data-ttu-id="1e47c-177">2 Toyotas</span><span class="sxs-lookup"><span data-stu-id="1e47c-177">2 Toyotas</span></span> |<span data-ttu-id="1e47c-178">2015-01-01T00:00:10.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-178">2015-01-01T00:00:10.0000000Z</span></span> |

<span data-ttu-id="1e47c-179">**Řešení**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-179">**Solution**:</span></span>

    SELECT
        CASE
            WHEN COUNT(*) = 1 THEN CONCAT('1 ', Make)
            ELSE CONCAT(CAST(COUNT(*) AS NVARCHAR(MAX)), ' ', Make, 's')
        END AS CarsPassed,
        System.TimeStamp AS Time
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

<span data-ttu-id="1e47c-180">**Vysvětlení**: **případ** klauzule umožňuje, abychom mohli poskytovat různé výpočtů, na základě některé kritérií (v našem případě počet aut v okně agregační).</span><span class="sxs-lookup"><span data-stu-id="1e47c-180">**Explanation**: The **CASE** clause allows us to provide a different computation, based on some criteria (in our case, the count of the cars in the aggregate window).</span></span>

## <a name="query-example-send-data-to-multiple-outputs"></a><span data-ttu-id="1e47c-181">Příklad dotazu: odesílání dat do několik výstupů</span><span class="sxs-lookup"><span data-stu-id="1e47c-181">Query example: Send data to multiple outputs</span></span>
<span data-ttu-id="1e47c-182">**Popis**: posílat data do více cílů výstup z jediné úlohy.</span><span class="sxs-lookup"><span data-stu-id="1e47c-182">**Description**: Send data to multiple output targets from a single job.</span></span>
<span data-ttu-id="1e47c-183">Například analyzovat data pro upozornění na základě prahové hodnoty a archivaci všechny události do úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="1e47c-183">For example, analyze data for a threshold-based alert and archive all events to blob storage.</span></span>

<span data-ttu-id="1e47c-184">**Vstup**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-184">**Input**:</span></span>

| <span data-ttu-id="1e47c-185">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="1e47c-185">Make</span></span> | <span data-ttu-id="1e47c-186">Čas</span><span class="sxs-lookup"><span data-stu-id="1e47c-186">Time</span></span> |
| --- | --- |
| <span data-ttu-id="1e47c-187">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-187">Honda</span></span> |<span data-ttu-id="1e47c-188">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-188">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="1e47c-189">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-189">Honda</span></span> |<span data-ttu-id="1e47c-190">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-190">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="1e47c-191">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-191">Toyota</span></span> |<span data-ttu-id="1e47c-192">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-192">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="1e47c-193">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-193">Toyota</span></span> |<span data-ttu-id="1e47c-194">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-194">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="1e47c-195">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-195">Toyota</span></span> |<span data-ttu-id="1e47c-196">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-196">2015-01-01T00:00:03.0000000Z</span></span> |

<span data-ttu-id="1e47c-197">**Output1**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-197">**Output1**:</span></span>

| <span data-ttu-id="1e47c-198">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="1e47c-198">Make</span></span> | <span data-ttu-id="1e47c-199">Čas</span><span class="sxs-lookup"><span data-stu-id="1e47c-199">Time</span></span> |
| --- | --- |
| <span data-ttu-id="1e47c-200">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-200">Honda</span></span> |<span data-ttu-id="1e47c-201">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-201">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="1e47c-202">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-202">Honda</span></span> |<span data-ttu-id="1e47c-203">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-203">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="1e47c-204">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-204">Toyota</span></span> |<span data-ttu-id="1e47c-205">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-205">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="1e47c-206">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-206">Toyota</span></span> |<span data-ttu-id="1e47c-207">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-207">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="1e47c-208">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-208">Toyota</span></span> |<span data-ttu-id="1e47c-209">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-209">2015-01-01T00:00:03.0000000Z</span></span> |

<span data-ttu-id="1e47c-210">**Output2**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-210">**Output2**:</span></span>

| <span data-ttu-id="1e47c-211">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="1e47c-211">Make</span></span> | <span data-ttu-id="1e47c-212">Čas</span><span class="sxs-lookup"><span data-stu-id="1e47c-212">Time</span></span> | <span data-ttu-id="1e47c-213">Počet</span><span class="sxs-lookup"><span data-stu-id="1e47c-213">Count</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1e47c-214">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-214">Toyota</span></span> |<span data-ttu-id="1e47c-215">2015-01-01T00:00:10.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-215">2015-01-01T00:00:10.0000000Z</span></span> |<span data-ttu-id="1e47c-216">3</span><span class="sxs-lookup"><span data-stu-id="1e47c-216">3</span></span> |

<span data-ttu-id="1e47c-217">**Řešení**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-217">**Solution**:</span></span>

    SELECT
        *
    INTO
        ArchiveOutput
    FROM
        Input TIMESTAMP BY Time

    SELECT
        Make,
        System.TimeStamp AS Time,
        COUNT(*) AS [Count]
    INTO
        AlertOutput
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)
    HAVING
        [Count] >= 3

<span data-ttu-id="1e47c-218">**Vysvětlení**: **INTO** klauzule informuje Stream Analytics který výstupy zapsat data do tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="1e47c-218">**Explanation**: The **INTO** clause tells Stream Analytics which of the outputs to write the data to from this statement.</span></span>
<span data-ttu-id="1e47c-219">První dotazu je průchozí dat jsme dostali výstupu, který jsme pojmenovali **ArchiveOutput**.</span><span class="sxs-lookup"><span data-stu-id="1e47c-219">The first query is a pass-through of the data we received to an output that we named **ArchiveOutput**.</span></span>
<span data-ttu-id="1e47c-220">Druhý dotaz nemá některé jednoduché agregace a filtrování a odesílá výsledky na příjem dat výstrah systém.</span><span class="sxs-lookup"><span data-stu-id="1e47c-220">The second query does some simple aggregation and filtering, and it sends the results to a downstream alerting system.</span></span>

<span data-ttu-id="1e47c-221">Všimněte si, že můžete také znovu použít výsledky běžných výrazech tabulky (odkazu Cte) (například **WITH** příkazy) ve více příkazů výstup.</span><span class="sxs-lookup"><span data-stu-id="1e47c-221">Note that you can also reuse the results of the common table expressions (CTEs) (such as **WITH** statements) in multiple output statements.</span></span> <span data-ttu-id="1e47c-222">Tato možnost má výhodu v podobě otevírání méně uživatelům vstupního zdroje.</span><span class="sxs-lookup"><span data-stu-id="1e47c-222">This option has the added benefit of opening fewer readers to the input source.</span></span>
<span data-ttu-id="1e47c-223">Například:</span><span class="sxs-lookup"><span data-stu-id="1e47c-223">For example:</span></span> 

    WITH AllRedCars AS (
        SELECT
            *
        FROM
            Input TIMESTAMP BY Time
        WHERE
            Color = 'red'
    )
    SELECT * INTO HondaOutput FROM AllRedCars WHERE Make = 'Honda'
    SELECT * INTO ToyotaOutput FROM AllRedCars WHERE Make = 'Toyota'

## <a name="query-example-count-unique-values"></a><span data-ttu-id="1e47c-224">Příklad dotazu: počet jedinečné hodnoty</span><span class="sxs-lookup"><span data-stu-id="1e47c-224">Query example: Count unique values</span></span>
<span data-ttu-id="1e47c-225">**Popis**: počet jedinečné pole hodnoty, které se zobrazují v datovém proudu v rámci časové okno.</span><span class="sxs-lookup"><span data-stu-id="1e47c-225">**Description**: Count the number of unique field values that appear in the stream within a time window.</span></span>
<span data-ttu-id="1e47c-226">Například kolik jedinečný díky automobilů předána stánek projedou v okně sekundu 2?</span><span class="sxs-lookup"><span data-stu-id="1e47c-226">For example, how many unique makes of cars passed through the toll booth in a 2-second window?</span></span>

<span data-ttu-id="1e47c-227">**Vstup**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-227">**Input**:</span></span>

| <span data-ttu-id="1e47c-228">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="1e47c-228">Make</span></span> | <span data-ttu-id="1e47c-229">Čas</span><span class="sxs-lookup"><span data-stu-id="1e47c-229">Time</span></span> |
| --- | --- |
| <span data-ttu-id="1e47c-230">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-230">Honda</span></span> |<span data-ttu-id="1e47c-231">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-231">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="1e47c-232">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-232">Honda</span></span> |<span data-ttu-id="1e47c-233">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-233">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="1e47c-234">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-234">Toyota</span></span> |<span data-ttu-id="1e47c-235">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-235">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="1e47c-236">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-236">Toyota</span></span> |<span data-ttu-id="1e47c-237">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-237">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="1e47c-238">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-238">Toyota</span></span> |<span data-ttu-id="1e47c-239">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-239">2015-01-01T00:00:03.0000000Z</span></span> |

<span data-ttu-id="1e47c-240">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="1e47c-240">**Output:**</span></span>

| <span data-ttu-id="1e47c-241">Počet</span><span class="sxs-lookup"><span data-stu-id="1e47c-241">Count</span></span> | <span data-ttu-id="1e47c-242">Čas</span><span class="sxs-lookup"><span data-stu-id="1e47c-242">Time</span></span> |
| --- | --- |
| <span data-ttu-id="1e47c-243">2</span><span class="sxs-lookup"><span data-stu-id="1e47c-243">2</span></span> |<span data-ttu-id="1e47c-244">2015-01-01T00:00:02.000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-244">2015-01-01T00:00:02.000Z</span></span> |
| <span data-ttu-id="1e47c-245">1</span><span class="sxs-lookup"><span data-stu-id="1e47c-245">1</span></span> |<span data-ttu-id="1e47c-246">2015-01-01T00:00:04.000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-246">2015-01-01T00:00:04.000Z</span></span> |

<span data-ttu-id="1e47c-247">**Řešení:**</span><span class="sxs-lookup"><span data-stu-id="1e47c-247">**Solution:**</span></span>

````
SELECT
     COUNT(DISTINCT Make) AS CountMake,
     System.TIMESTAMP AS TIME
FROM Input TIMESTAMP BY TIME
GROUP BY 
     TumblingWindow(second, 2)
````


<span data-ttu-id="1e47c-248">**Vysvětlení:**
**COUNT (odlišné ověřte)** vrátí počet jedinečných hodnot ve **zkontrolujte** sloupec v rámci časové okno.</span><span class="sxs-lookup"><span data-stu-id="1e47c-248">**Explanation:**
**COUNT(DISTINCT Make)** returns the number of distinct values in the **Make** column within a time window.</span></span>

## <a name="query-example-determine-if-a-value-has-changed"></a><span data-ttu-id="1e47c-249">Příklad dotazu: určení, pokud byla hodnota změněna</span><span class="sxs-lookup"><span data-stu-id="1e47c-249">Query example: Determine if a value has changed</span></span>
<span data-ttu-id="1e47c-250">**Popis**: Podívejte se na předchozí hodnotu k určení, pokud se liší od aktuální hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1e47c-250">**Description**: Look at a previous value to determine if it is different than the current value.</span></span>
<span data-ttu-id="1e47c-251">Předchozí car na cestách projedou je třeba vytvořit stejný jako aktuální Auto?</span><span class="sxs-lookup"><span data-stu-id="1e47c-251">For example, is the previous car on the toll road the same make as the current car?</span></span>

<span data-ttu-id="1e47c-252">**Vstup**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-252">**Input**:</span></span>

| <span data-ttu-id="1e47c-253">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="1e47c-253">Make</span></span> | <span data-ttu-id="1e47c-254">Čas</span><span class="sxs-lookup"><span data-stu-id="1e47c-254">Time</span></span> |
| --- | --- |
| <span data-ttu-id="1e47c-255">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-255">Honda</span></span> |<span data-ttu-id="1e47c-256">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-256">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="1e47c-257">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-257">Toyota</span></span> |<span data-ttu-id="1e47c-258">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-258">2015-01-01T00:00:02.0000000Z</span></span> |

<span data-ttu-id="1e47c-259">**Výstup**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-259">**Output**:</span></span>

| <span data-ttu-id="1e47c-260">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="1e47c-260">Make</span></span> | <span data-ttu-id="1e47c-261">Čas</span><span class="sxs-lookup"><span data-stu-id="1e47c-261">Time</span></span> |
| --- | --- |
| <span data-ttu-id="1e47c-262">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-262">Toyota</span></span> |<span data-ttu-id="1e47c-263">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-263">2015-01-01T00:00:02.0000000Z</span></span> |

<span data-ttu-id="1e47c-264">**Řešení**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-264">**Solution**:</span></span>

    SELECT
        Make,
        Time
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(minute, 1)) <> Make

<span data-ttu-id="1e47c-265">**Vysvětlení**: použití **PRODLEVA** prohlížet do jedné události vstupního datového proudu zpět a získat **zkontrolujte** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1e47c-265">**Explanation**: Use **LAG** to peek into the input stream one event back and get the **Make** value.</span></span> <span data-ttu-id="1e47c-266">Pak porovnávají ho do **zkontrolujte** hodnotu na aktuální událost a výstupní událost, pokud se liší.</span><span class="sxs-lookup"><span data-stu-id="1e47c-266">Then compare it to the **Make** value on the current event and output the event if they are different.</span></span>

## <a name="query-example-find-the-first-event-in-a-window"></a><span data-ttu-id="1e47c-267">Příklad dotazu: Najít první událost v okně</span><span class="sxs-lookup"><span data-stu-id="1e47c-267">Query example: Find the first event in a window</span></span>
<span data-ttu-id="1e47c-268">**Popis**: Najít první auto v intervalu každých 10 minut.</span><span class="sxs-lookup"><span data-stu-id="1e47c-268">**Description**: Find the first car in every 10-minute interval.</span></span>

<span data-ttu-id="1e47c-269">**Vstup**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-269">**Input**:</span></span>

| <span data-ttu-id="1e47c-270">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="1e47c-270">LicensePlate</span></span> | <span data-ttu-id="1e47c-271">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="1e47c-271">Make</span></span> | <span data-ttu-id="1e47c-272">Čas</span><span class="sxs-lookup"><span data-stu-id="1e47c-272">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1e47c-273">DXE 5291</span><span class="sxs-lookup"><span data-stu-id="1e47c-273">DXE 5291</span></span> |<span data-ttu-id="1e47c-274">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-274">Honda</span></span> |<span data-ttu-id="1e47c-275">2015-07-27T00:00:05.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-275">2015-07-27T00:00:05.0000000Z</span></span> |
| <span data-ttu-id="1e47c-276">YZK 5704</span><span class="sxs-lookup"><span data-stu-id="1e47c-276">YZK 5704</span></span> |<span data-ttu-id="1e47c-277">Ford</span><span class="sxs-lookup"><span data-stu-id="1e47c-277">Ford</span></span> |<span data-ttu-id="1e47c-278">2015-07-27T00:02:17.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-278">2015-07-27T00:02:17.0000000Z</span></span> |
| <span data-ttu-id="1e47c-279">RMV 8282</span><span class="sxs-lookup"><span data-stu-id="1e47c-279">RMV 8282</span></span> |<span data-ttu-id="1e47c-280">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-280">Honda</span></span> |<span data-ttu-id="1e47c-281">2015-07-27T00:05:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-281">2015-07-27T00:05:01.0000000Z</span></span> |
| <span data-ttu-id="1e47c-282">YHN 6970</span><span class="sxs-lookup"><span data-stu-id="1e47c-282">YHN 6970</span></span> |<span data-ttu-id="1e47c-283">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-283">Toyota</span></span> |<span data-ttu-id="1e47c-284">2015-07-27T00:06:00.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-284">2015-07-27T00:06:00.0000000Z</span></span> |
| <span data-ttu-id="1e47c-285">VFE 1616</span><span class="sxs-lookup"><span data-stu-id="1e47c-285">VFE 1616</span></span> |<span data-ttu-id="1e47c-286">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-286">Toyota</span></span> |<span data-ttu-id="1e47c-287">2015-07-27T00:09:31.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-287">2015-07-27T00:09:31.0000000Z</span></span> |
| <span data-ttu-id="1e47c-288">QYF 9358</span><span class="sxs-lookup"><span data-stu-id="1e47c-288">QYF 9358</span></span> |<span data-ttu-id="1e47c-289">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-289">Honda</span></span> |<span data-ttu-id="1e47c-290">2015-07-27T00:12:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-290">2015-07-27T00:12:02.0000000Z</span></span> |
| <span data-ttu-id="1e47c-291">MDR 6128</span><span class="sxs-lookup"><span data-stu-id="1e47c-291">MDR 6128</span></span> |<span data-ttu-id="1e47c-292">BMW</span><span class="sxs-lookup"><span data-stu-id="1e47c-292">BMW</span></span> |<span data-ttu-id="1e47c-293">2015-07-27T00:13:45.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-293">2015-07-27T00:13:45.0000000Z</span></span> |

<span data-ttu-id="1e47c-294">**Výstup**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-294">**Output**:</span></span>

| <span data-ttu-id="1e47c-295">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="1e47c-295">LicensePlate</span></span> | <span data-ttu-id="1e47c-296">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="1e47c-296">Make</span></span> | <span data-ttu-id="1e47c-297">Čas</span><span class="sxs-lookup"><span data-stu-id="1e47c-297">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1e47c-298">DXE 5291</span><span class="sxs-lookup"><span data-stu-id="1e47c-298">DXE 5291</span></span> |<span data-ttu-id="1e47c-299">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-299">Honda</span></span> |<span data-ttu-id="1e47c-300">2015-07-27T00:00:05.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-300">2015-07-27T00:00:05.0000000Z</span></span> |
| <span data-ttu-id="1e47c-301">QYF 9358</span><span class="sxs-lookup"><span data-stu-id="1e47c-301">QYF 9358</span></span> |<span data-ttu-id="1e47c-302">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-302">Honda</span></span> |<span data-ttu-id="1e47c-303">2015-07-27T00:12:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-303">2015-07-27T00:12:02.0000000Z</span></span> |

<span data-ttu-id="1e47c-304">**Řešení**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-304">**Solution**:</span></span>

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) = 1

<span data-ttu-id="1e47c-305">Teď umožňuje změnit problém a najít první auto konkrétní značky v intervalu každých 10 minut.</span><span class="sxs-lookup"><span data-stu-id="1e47c-305">Now let’s change the problem and find the first car of a particular make in every 10-minute interval.</span></span>

| <span data-ttu-id="1e47c-306">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="1e47c-306">LicensePlate</span></span> | <span data-ttu-id="1e47c-307">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="1e47c-307">Make</span></span> | <span data-ttu-id="1e47c-308">Čas</span><span class="sxs-lookup"><span data-stu-id="1e47c-308">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1e47c-309">DXE 5291</span><span class="sxs-lookup"><span data-stu-id="1e47c-309">DXE 5291</span></span> |<span data-ttu-id="1e47c-310">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-310">Honda</span></span> |<span data-ttu-id="1e47c-311">2015-07-27T00:00:05.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-311">2015-07-27T00:00:05.0000000Z</span></span> |
| <span data-ttu-id="1e47c-312">YZK 5704</span><span class="sxs-lookup"><span data-stu-id="1e47c-312">YZK 5704</span></span> |<span data-ttu-id="1e47c-313">Ford</span><span class="sxs-lookup"><span data-stu-id="1e47c-313">Ford</span></span> |<span data-ttu-id="1e47c-314">2015-07-27T00:02:17.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-314">2015-07-27T00:02:17.0000000Z</span></span> |
| <span data-ttu-id="1e47c-315">YHN 6970</span><span class="sxs-lookup"><span data-stu-id="1e47c-315">YHN 6970</span></span> |<span data-ttu-id="1e47c-316">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-316">Toyota</span></span> |<span data-ttu-id="1e47c-317">2015-07-27T00:06:00.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-317">2015-07-27T00:06:00.0000000Z</span></span> |
| <span data-ttu-id="1e47c-318">QYF 9358</span><span class="sxs-lookup"><span data-stu-id="1e47c-318">QYF 9358</span></span> |<span data-ttu-id="1e47c-319">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-319">Honda</span></span> |<span data-ttu-id="1e47c-320">2015-07-27T00:12:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-320">2015-07-27T00:12:02.0000000Z</span></span> |
| <span data-ttu-id="1e47c-321">MDR 6128</span><span class="sxs-lookup"><span data-stu-id="1e47c-321">MDR 6128</span></span> |<span data-ttu-id="1e47c-322">BMW</span><span class="sxs-lookup"><span data-stu-id="1e47c-322">BMW</span></span> |<span data-ttu-id="1e47c-323">2015-07-27T00:13:45.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-323">2015-07-27T00:13:45.0000000Z</span></span> |

<span data-ttu-id="1e47c-324">**Řešení**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-324">**Solution**:</span></span>

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) OVER (PARTITION BY Make) = 1

## <a name="query-example-find-the-last-event-in-a-window"></a><span data-ttu-id="1e47c-325">Příklad dotazu: najít poslední událost v okně</span><span class="sxs-lookup"><span data-stu-id="1e47c-325">Query example: Find the last event in a window</span></span>
<span data-ttu-id="1e47c-326">**Popis**: najít poslední car v intervalu každých 10 minut.</span><span class="sxs-lookup"><span data-stu-id="1e47c-326">**Description**: Find the last car in every 10-minute interval.</span></span>

<span data-ttu-id="1e47c-327">**Vstup**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-327">**Input**:</span></span>

| <span data-ttu-id="1e47c-328">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="1e47c-328">LicensePlate</span></span> | <span data-ttu-id="1e47c-329">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="1e47c-329">Make</span></span> | <span data-ttu-id="1e47c-330">Čas</span><span class="sxs-lookup"><span data-stu-id="1e47c-330">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1e47c-331">DXE 5291</span><span class="sxs-lookup"><span data-stu-id="1e47c-331">DXE 5291</span></span> |<span data-ttu-id="1e47c-332">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-332">Honda</span></span> |<span data-ttu-id="1e47c-333">2015-07-27T00:00:05.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-333">2015-07-27T00:00:05.0000000Z</span></span> |
| <span data-ttu-id="1e47c-334">YZK 5704</span><span class="sxs-lookup"><span data-stu-id="1e47c-334">YZK 5704</span></span> |<span data-ttu-id="1e47c-335">Ford</span><span class="sxs-lookup"><span data-stu-id="1e47c-335">Ford</span></span> |<span data-ttu-id="1e47c-336">2015-07-27T00:02:17.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-336">2015-07-27T00:02:17.0000000Z</span></span> |
| <span data-ttu-id="1e47c-337">RMV 8282</span><span class="sxs-lookup"><span data-stu-id="1e47c-337">RMV 8282</span></span> |<span data-ttu-id="1e47c-338">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-338">Honda</span></span> |<span data-ttu-id="1e47c-339">2015-07-27T00:05:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-339">2015-07-27T00:05:01.0000000Z</span></span> |
| <span data-ttu-id="1e47c-340">YHN 6970</span><span class="sxs-lookup"><span data-stu-id="1e47c-340">YHN 6970</span></span> |<span data-ttu-id="1e47c-341">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-341">Toyota</span></span> |<span data-ttu-id="1e47c-342">2015-07-27T00:06:00.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-342">2015-07-27T00:06:00.0000000Z</span></span> |
| <span data-ttu-id="1e47c-343">VFE 1616</span><span class="sxs-lookup"><span data-stu-id="1e47c-343">VFE 1616</span></span> |<span data-ttu-id="1e47c-344">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-344">Toyota</span></span> |<span data-ttu-id="1e47c-345">2015-07-27T00:09:31.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-345">2015-07-27T00:09:31.0000000Z</span></span> |
| <span data-ttu-id="1e47c-346">QYF 9358</span><span class="sxs-lookup"><span data-stu-id="1e47c-346">QYF 9358</span></span> |<span data-ttu-id="1e47c-347">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-347">Honda</span></span> |<span data-ttu-id="1e47c-348">2015-07-27T00:12:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-348">2015-07-27T00:12:02.0000000Z</span></span> |
| <span data-ttu-id="1e47c-349">MDR 6128</span><span class="sxs-lookup"><span data-stu-id="1e47c-349">MDR 6128</span></span> |<span data-ttu-id="1e47c-350">BMW</span><span class="sxs-lookup"><span data-stu-id="1e47c-350">BMW</span></span> |<span data-ttu-id="1e47c-351">2015-07-27T00:13:45.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-351">2015-07-27T00:13:45.0000000Z</span></span> |

<span data-ttu-id="1e47c-352">**Výstup**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-352">**Output**:</span></span>

| <span data-ttu-id="1e47c-353">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="1e47c-353">LicensePlate</span></span> | <span data-ttu-id="1e47c-354">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="1e47c-354">Make</span></span> | <span data-ttu-id="1e47c-355">Čas</span><span class="sxs-lookup"><span data-stu-id="1e47c-355">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1e47c-356">VFE 1616</span><span class="sxs-lookup"><span data-stu-id="1e47c-356">VFE 1616</span></span> |<span data-ttu-id="1e47c-357">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-357">Toyota</span></span> |<span data-ttu-id="1e47c-358">2015-07-27T00:09:31.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-358">2015-07-27T00:09:31.0000000Z</span></span> |
| <span data-ttu-id="1e47c-359">MDR 6128</span><span class="sxs-lookup"><span data-stu-id="1e47c-359">MDR 6128</span></span> |<span data-ttu-id="1e47c-360">BMW</span><span class="sxs-lookup"><span data-stu-id="1e47c-360">BMW</span></span> |<span data-ttu-id="1e47c-361">2015-07-27T00:13:45.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-361">2015-07-27T00:13:45.0000000Z</span></span> |

<span data-ttu-id="1e47c-362">**Řešení**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-362">**Solution**:</span></span>

    WITH LastInWindow AS
    (
        SELECT 
            MAX(Time) AS LastEventTime
        FROM 
            Input TIMESTAMP BY Time
        GROUP BY 
            TumblingWindow(minute, 10)
    )
    SELECT 
        Input.LicensePlate,
        Input.Make,
        Input.Time
    FROM
        Input TIMESTAMP BY Time 
        INNER JOIN LastInWindow
        ON DATEDIFF(minute, Input, LastInWindow) BETWEEN 0 AND 10
        AND Input.Time = LastInWindow.LastEventTime

<span data-ttu-id="1e47c-363">**Vysvětlení**: existují dva kroky v dotazu.</span><span class="sxs-lookup"><span data-stu-id="1e47c-363">**Explanation**: There are two steps in the query.</span></span> <span data-ttu-id="1e47c-364">První z nich vyhledá nejnovější časové razítko v systému windows 10 minut.</span><span class="sxs-lookup"><span data-stu-id="1e47c-364">The first one finds the latest time stamp in 10-minute windows.</span></span> <span data-ttu-id="1e47c-365">Druhý krok spojí výsledky první dotaz s původní datového proudu a vyhledejte události, které odpovídají poslední časová razítka v každém okně.</span><span class="sxs-lookup"><span data-stu-id="1e47c-365">The second step joins the results of the first query with the original stream to find the events that match the last time stamps in each window.</span></span> 

## <a name="query-example-detect-the-absence-of-events"></a><span data-ttu-id="1e47c-366">Příklad dotazu: zjištění absence událostí</span><span class="sxs-lookup"><span data-stu-id="1e47c-366">Query example: Detect the absence of events</span></span>
<span data-ttu-id="1e47c-367">**Popis**: Zkontrolujte, že datový proud má žádná hodnota, která odpovídá určité kritérium.</span><span class="sxs-lookup"><span data-stu-id="1e47c-367">**Description**: Check that a stream has no value that matches a certain criterion.</span></span>
<span data-ttu-id="1e47c-368">Například 2 po sobě jdoucích aut ze stejné zkontrolujte zadali silniční projedou v posledních 90 sekund?</span><span class="sxs-lookup"><span data-stu-id="1e47c-368">For example, have 2 consecutive cars from the same make entered the toll road within the last 90 seconds?</span></span>

<span data-ttu-id="1e47c-369">**Vstup**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-369">**Input**:</span></span>

| <span data-ttu-id="1e47c-370">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="1e47c-370">Make</span></span> | <span data-ttu-id="1e47c-371">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="1e47c-371">LicensePlate</span></span> | <span data-ttu-id="1e47c-372">Čas</span><span class="sxs-lookup"><span data-stu-id="1e47c-372">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1e47c-373">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-373">Honda</span></span> |<span data-ttu-id="1e47c-374">ABC 123</span><span class="sxs-lookup"><span data-stu-id="1e47c-374">ABC-123</span></span> |<span data-ttu-id="1e47c-375">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-375">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="1e47c-376">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-376">Honda</span></span> |<span data-ttu-id="1e47c-377">AAA-999</span><span class="sxs-lookup"><span data-stu-id="1e47c-377">AAA-999</span></span> |<span data-ttu-id="1e47c-378">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-378">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="1e47c-379">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-379">Toyota</span></span> |<span data-ttu-id="1e47c-380">DEF 987</span><span class="sxs-lookup"><span data-stu-id="1e47c-380">DEF-987</span></span> |<span data-ttu-id="1e47c-381">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-381">2015-01-01T00:00:03.0000000Z</span></span> |
| <span data-ttu-id="1e47c-382">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-382">Honda</span></span> |<span data-ttu-id="1e47c-383">GHI 345</span><span class="sxs-lookup"><span data-stu-id="1e47c-383">GHI-345</span></span> |<span data-ttu-id="1e47c-384">2015-01-01T00:00:04.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-384">2015-01-01T00:00:04.0000000Z</span></span> |

<span data-ttu-id="1e47c-385">**Výstup**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-385">**Output**:</span></span>

| <span data-ttu-id="1e47c-386">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="1e47c-386">Make</span></span> | <span data-ttu-id="1e47c-387">Čas</span><span class="sxs-lookup"><span data-stu-id="1e47c-387">Time</span></span> | <span data-ttu-id="1e47c-388">CurrentCarLicensePlate</span><span class="sxs-lookup"><span data-stu-id="1e47c-388">CurrentCarLicensePlate</span></span> | <span data-ttu-id="1e47c-389">FirstCarLicensePlate</span><span class="sxs-lookup"><span data-stu-id="1e47c-389">FirstCarLicensePlate</span></span> | <span data-ttu-id="1e47c-390">FirstCarTime</span><span class="sxs-lookup"><span data-stu-id="1e47c-390">FirstCarTime</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="1e47c-391">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-391">Honda</span></span> |<span data-ttu-id="1e47c-392">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-392">2015-01-01T00:00:02.0000000Z</span></span> |<span data-ttu-id="1e47c-393">AAA-999</span><span class="sxs-lookup"><span data-stu-id="1e47c-393">AAA-999</span></span> |<span data-ttu-id="1e47c-394">ABC 123</span><span class="sxs-lookup"><span data-stu-id="1e47c-394">ABC-123</span></span> |<span data-ttu-id="1e47c-395">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-395">2015-01-01T00:00:01.0000000Z</span></span> |

<span data-ttu-id="1e47c-396">**Řešení**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-396">**Solution**:</span></span>

    SELECT
        Make,
        Time,
        LicensePlate AS CurrentCarLicensePlate,
        LAG(LicensePlate, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarLicensePlate,
        LAG(Time, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarTime
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(second, 90)) = Make

<span data-ttu-id="1e47c-397">**Vysvětlení**: použití **PRODLEVA** prohlížet do jedné události vstupního datového proudu zpět a získat **zkontrolujte** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="1e47c-397">**Explanation**: Use **LAG** to peek into the input stream one event back and get the **Make** value.</span></span> <span data-ttu-id="1e47c-398">Porovnat s **ZKONTROLUJTE** hodnoty v aktuálním události a pak výstupní událost, pokud jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="1e47c-398">Compare it to the **MAKE** value in the current event, and then output the event if they are the same.</span></span> <span data-ttu-id="1e47c-399">Můžete také použít **PRODLEVA** k získání dat o předchozí Auto.</span><span class="sxs-lookup"><span data-stu-id="1e47c-399">You can also use **LAG** to get data about the previous car.</span></span>

## <a name="query-example-detect-the-duration-between-events"></a><span data-ttu-id="1e47c-400">Příklad dotazu: zjistit dobu trvání mezi událostmi</span><span class="sxs-lookup"><span data-stu-id="1e47c-400">Query example: Detect the duration between events</span></span>
<span data-ttu-id="1e47c-401">**Popis**: najít trvání dané události.</span><span class="sxs-lookup"><span data-stu-id="1e47c-401">**Description**: Find the duration of a given event.</span></span> <span data-ttu-id="1e47c-402">Například zadané webové clickstream, určete čas strávený na funkce.</span><span class="sxs-lookup"><span data-stu-id="1e47c-402">For example, given a web clickstream, determine the time spent on a feature.</span></span>

<span data-ttu-id="1e47c-403">**Vstup**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-403">**Input**:</span></span>  

| <span data-ttu-id="1e47c-404">Uživatel</span><span class="sxs-lookup"><span data-stu-id="1e47c-404">User</span></span> | <span data-ttu-id="1e47c-405">Funkce</span><span class="sxs-lookup"><span data-stu-id="1e47c-405">Feature</span></span> | <span data-ttu-id="1e47c-406">Událost</span><span class="sxs-lookup"><span data-stu-id="1e47c-406">Event</span></span> | <span data-ttu-id="1e47c-407">Čas</span><span class="sxs-lookup"><span data-stu-id="1e47c-407">Time</span></span> |
| --- | --- | --- | --- |
| user@location.com |<span data-ttu-id="1e47c-408">RightMenu</span><span class="sxs-lookup"><span data-stu-id="1e47c-408">RightMenu</span></span> |<span data-ttu-id="1e47c-409">Start</span><span class="sxs-lookup"><span data-stu-id="1e47c-409">Start</span></span> |<span data-ttu-id="1e47c-410">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-410">2015-01-01T00:00:01.0000000Z</span></span> |
| user@location.com |<span data-ttu-id="1e47c-411">RightMenu</span><span class="sxs-lookup"><span data-stu-id="1e47c-411">RightMenu</span></span> |<span data-ttu-id="1e47c-412">End</span><span class="sxs-lookup"><span data-stu-id="1e47c-412">End</span></span> |<span data-ttu-id="1e47c-413">2015-01-01T00:00:08.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-413">2015-01-01T00:00:08.0000000Z</span></span> |

<span data-ttu-id="1e47c-414">**Výstup**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-414">**Output**:</span></span>  

| <span data-ttu-id="1e47c-415">Uživatel</span><span class="sxs-lookup"><span data-stu-id="1e47c-415">User</span></span> | <span data-ttu-id="1e47c-416">Funkce</span><span class="sxs-lookup"><span data-stu-id="1e47c-416">Feature</span></span> | <span data-ttu-id="1e47c-417">Doba trvání</span><span class="sxs-lookup"><span data-stu-id="1e47c-417">Duration</span></span> |
| --- | --- | --- |
| user@location.com |<span data-ttu-id="1e47c-418">RightMenu</span><span class="sxs-lookup"><span data-stu-id="1e47c-418">RightMenu</span></span> |<span data-ttu-id="1e47c-419">7</span><span class="sxs-lookup"><span data-stu-id="1e47c-419">7</span></span> |

<span data-ttu-id="1e47c-420">**Řešení**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-420">**Solution**:</span></span>

````
    SELECT
        [user], feature, DATEDIFF(second, LAST(Time) OVER (PARTITION BY [user], feature LIMIT DURATION(hour, 1) WHEN Event = 'start'), Time) as duration
    FROM input TIMESTAMP BY Time
    WHERE
        Event = 'end'
````

<span data-ttu-id="1e47c-421">**Vysvětlení**: použití **poslední** funkce načíst poslední **čas** hodnota typu události při **spustit**.</span><span class="sxs-lookup"><span data-stu-id="1e47c-421">**Explanation**: Use the **LAST** function to retrieve the last **TIME** value when the event type was **Start**.</span></span> <span data-ttu-id="1e47c-422">**Poslední** využívá **PARTITION BY [user]** indikující, že výsledek se počítá na jedinečný uživatele.</span><span class="sxs-lookup"><span data-stu-id="1e47c-422">The **LAST** function uses **PARTITION BY [user]** to indicate that the result is computed per unique user.</span></span> <span data-ttu-id="1e47c-423">Dotaz musí maximální prahovou hodnotu 1 hodina časový rozdíl mezi **spustit** a **Zastavit** události, ale je možné konfigurovat podle potřeby **(LIMIT DURATION(hour, 1)**.</span><span class="sxs-lookup"><span data-stu-id="1e47c-423">The query has a 1-hour maximum threshold for the time difference between **Start** and **Stop** events, but is configurable as needed **(LIMIT DURATION(hour, 1)**.</span></span>

## <a name="query-example-detect-the-duration-of-a-condition"></a><span data-ttu-id="1e47c-424">Příklad dotazu: zjistit dobu trvání podmínku</span><span class="sxs-lookup"><span data-stu-id="1e47c-424">Query example: Detect the duration of a condition</span></span>
<span data-ttu-id="1e47c-425">**Popis**: Najít se na jak dlouho podmínku došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="1e47c-425">**Description**: Find out how long a condition occurred.</span></span>
<span data-ttu-id="1e47c-426">Předpokládejme například, že chyby výsledkem všechny aut, které mají nesprávné váhu (nad 20 000 libra).</span><span class="sxs-lookup"><span data-stu-id="1e47c-426">For example, suppose that a bug resulted in all cars having an incorrect weight (above 20,000 pounds).</span></span> <span data-ttu-id="1e47c-427">Chceme výpočetní trvání chybě.</span><span class="sxs-lookup"><span data-stu-id="1e47c-427">We want to compute the duration of the bug.</span></span>

<span data-ttu-id="1e47c-428">**Vstup**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-428">**Input**:</span></span>

| <span data-ttu-id="1e47c-429">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="1e47c-429">Make</span></span> | <span data-ttu-id="1e47c-430">Čas</span><span class="sxs-lookup"><span data-stu-id="1e47c-430">Time</span></span> | <span data-ttu-id="1e47c-431">Váha</span><span class="sxs-lookup"><span data-stu-id="1e47c-431">Weight</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1e47c-432">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-432">Honda</span></span> |<span data-ttu-id="1e47c-433">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-433">2015-01-01T00:00:01.0000000Z</span></span> |<span data-ttu-id="1e47c-434">2000</span><span class="sxs-lookup"><span data-stu-id="1e47c-434">2000</span></span> |
| <span data-ttu-id="1e47c-435">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-435">Toyota</span></span> |<span data-ttu-id="1e47c-436">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-436">2015-01-01T00:00:02.0000000Z</span></span> |<span data-ttu-id="1e47c-437">25000</span><span class="sxs-lookup"><span data-stu-id="1e47c-437">25000</span></span> |
| <span data-ttu-id="1e47c-438">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-438">Honda</span></span> |<span data-ttu-id="1e47c-439">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-439">2015-01-01T00:00:03.0000000Z</span></span> |<span data-ttu-id="1e47c-440">26000</span><span class="sxs-lookup"><span data-stu-id="1e47c-440">26000</span></span> |
| <span data-ttu-id="1e47c-441">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-441">Toyota</span></span> |<span data-ttu-id="1e47c-442">2015-01-01T00:00:04.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-442">2015-01-01T00:00:04.0000000Z</span></span> |<span data-ttu-id="1e47c-443">25000</span><span class="sxs-lookup"><span data-stu-id="1e47c-443">25000</span></span> |
| <span data-ttu-id="1e47c-444">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-444">Honda</span></span> |<span data-ttu-id="1e47c-445">2015-01-01T00:00:05.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-445">2015-01-01T00:00:05.0000000Z</span></span> |<span data-ttu-id="1e47c-446">26000</span><span class="sxs-lookup"><span data-stu-id="1e47c-446">26000</span></span> |
| <span data-ttu-id="1e47c-447">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-447">Toyota</span></span> |<span data-ttu-id="1e47c-448">2015-01-01T00:00:06.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-448">2015-01-01T00:00:06.0000000Z</span></span> |<span data-ttu-id="1e47c-449">25000</span><span class="sxs-lookup"><span data-stu-id="1e47c-449">25000</span></span> |
| <span data-ttu-id="1e47c-450">Honda</span><span class="sxs-lookup"><span data-stu-id="1e47c-450">Honda</span></span> |<span data-ttu-id="1e47c-451">2015-01-01T00:00:07.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-451">2015-01-01T00:00:07.0000000Z</span></span> |<span data-ttu-id="1e47c-452">26000</span><span class="sxs-lookup"><span data-stu-id="1e47c-452">26000</span></span> |
| <span data-ttu-id="1e47c-453">Toyota</span><span class="sxs-lookup"><span data-stu-id="1e47c-453">Toyota</span></span> |<span data-ttu-id="1e47c-454">2015-01-01T00:00:08.0000000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-454">2015-01-01T00:00:08.0000000Z</span></span> |<span data-ttu-id="1e47c-455">2000</span><span class="sxs-lookup"><span data-stu-id="1e47c-455">2000</span></span> |

<span data-ttu-id="1e47c-456">**Výstup**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-456">**Output**:</span></span>

| <span data-ttu-id="1e47c-457">StartFault</span><span class="sxs-lookup"><span data-stu-id="1e47c-457">StartFault</span></span> | <span data-ttu-id="1e47c-458">EndFault</span><span class="sxs-lookup"><span data-stu-id="1e47c-458">EndFault</span></span> |
| --- | --- |
| <span data-ttu-id="1e47c-459">2015-01-01T00:00:02.000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-459">2015-01-01T00:00:02.000Z</span></span> |<span data-ttu-id="1e47c-460">2015-01-01T00:00:07.000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-460">2015-01-01T00:00:07.000Z</span></span> |

<span data-ttu-id="1e47c-461">**Řešení**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-461">**Solution**:</span></span>

````
    WITH SelectPreviousEvent AS
    (
    SELECT
    *,
        LAG([time]) OVER (LIMIT DURATION(hour, 24)) as previousTime,
        LAG([weight]) OVER (LIMIT DURATION(hour, 24)) as previousWeight
    FROM input TIMESTAMP BY [time]
    )

    SELECT 
        LAG(time) OVER (LIMIT DURATION(hour, 24) WHEN previousWeight < 20000 ) [StartFault],
        previousTime [EndFault]
    FROM SelectPreviousEvent
    WHERE
        [weight] < 20000
        AND previousWeight > 20000
````

<span data-ttu-id="1e47c-462">**Vysvětlení**: použití **PRODLEVA** zobrazení vstupního datového proudu pro 24 hodin a vyhledejte instance kde **StartFault** a **StopFault** jsou předané váhu < 20000.</span><span class="sxs-lookup"><span data-stu-id="1e47c-462">**Explanation**: Use **LAG** to view the input stream for 24 hours and look for instances where **StartFault** and **StopFault** are spanned by the weight < 20000.</span></span>

## <a name="query-example-fill-missing-values"></a><span data-ttu-id="1e47c-463">Příklad dotazu: vyplnění chybějící hodnoty</span><span class="sxs-lookup"><span data-stu-id="1e47c-463">Query example: Fill missing values</span></span>
<span data-ttu-id="1e47c-464">**Popis**: pro datový proud události, které mají chybějící hodnoty, vytvořit datový proud událostí s pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="1e47c-464">**Description**: For the stream of events that have missing values, produce a stream of events with regular intervals.</span></span>
<span data-ttu-id="1e47c-465">Například generovat událost každých 5 sekund, která generuje sestavy naposledy zaznamenané datového bodu.</span><span class="sxs-lookup"><span data-stu-id="1e47c-465">For example, generate an event every 5 seconds that reports the most recently seen data point.</span></span>

<span data-ttu-id="1e47c-466">**Vstup**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-466">**Input**:</span></span>

| <span data-ttu-id="1e47c-467">T</span><span class="sxs-lookup"><span data-stu-id="1e47c-467">t</span></span> | <span data-ttu-id="1e47c-468">hodnota</span><span class="sxs-lookup"><span data-stu-id="1e47c-468">value</span></span> |
| --- | --- |
| <span data-ttu-id="1e47c-469">"2014-01-01T06:01:00"</span><span class="sxs-lookup"><span data-stu-id="1e47c-469">"2014-01-01T06:01:00"</span></span> |<span data-ttu-id="1e47c-470">1</span><span class="sxs-lookup"><span data-stu-id="1e47c-470">1</span></span> |
| <span data-ttu-id="1e47c-471">"2014-01-01T06:01:05"</span><span class="sxs-lookup"><span data-stu-id="1e47c-471">"2014-01-01T06:01:05"</span></span> |<span data-ttu-id="1e47c-472">2</span><span class="sxs-lookup"><span data-stu-id="1e47c-472">2</span></span> |
| <span data-ttu-id="1e47c-473">"2014-01-01T06:01:10"</span><span class="sxs-lookup"><span data-stu-id="1e47c-473">"2014-01-01T06:01:10"</span></span> |<span data-ttu-id="1e47c-474">3</span><span class="sxs-lookup"><span data-stu-id="1e47c-474">3</span></span> |
| <span data-ttu-id="1e47c-475">"2014-01-01T06:01:15"</span><span class="sxs-lookup"><span data-stu-id="1e47c-475">"2014-01-01T06:01:15"</span></span> |<span data-ttu-id="1e47c-476">4</span><span class="sxs-lookup"><span data-stu-id="1e47c-476">4</span></span> |
| <span data-ttu-id="1e47c-477">"2014-01-01T06:01:30"</span><span class="sxs-lookup"><span data-stu-id="1e47c-477">"2014-01-01T06:01:30"</span></span> |<span data-ttu-id="1e47c-478">5</span><span class="sxs-lookup"><span data-stu-id="1e47c-478">5</span></span> |
| <span data-ttu-id="1e47c-479">"2014-01-01T06:01:35"</span><span class="sxs-lookup"><span data-stu-id="1e47c-479">"2014-01-01T06:01:35"</span></span> |<span data-ttu-id="1e47c-480">6</span><span class="sxs-lookup"><span data-stu-id="1e47c-480">6</span></span> |

<span data-ttu-id="1e47c-481">**Výstup (prvních 10 řádků)**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-481">**Output (first 10 rows)**:</span></span>

| <span data-ttu-id="1e47c-482">windowend</span><span class="sxs-lookup"><span data-stu-id="1e47c-482">windowend</span></span> | <span data-ttu-id="1e47c-483">lastevent.t</span><span class="sxs-lookup"><span data-stu-id="1e47c-483">lastevent.t</span></span> | <span data-ttu-id="1e47c-484">lastevent.Value</span><span class="sxs-lookup"><span data-stu-id="1e47c-484">lastevent.value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1e47c-485">2014-01-01T14:01:00.000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-485">2014-01-01T14:01:00.000Z</span></span> |<span data-ttu-id="1e47c-486">2014-01-01T14:01:00.000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-486">2014-01-01T14:01:00.000Z</span></span> |<span data-ttu-id="1e47c-487">1</span><span class="sxs-lookup"><span data-stu-id="1e47c-487">1</span></span> |
| <span data-ttu-id="1e47c-488">2014-01-01T14:01:05.000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-488">2014-01-01T14:01:05.000Z</span></span> |<span data-ttu-id="1e47c-489">2014-01-01T14:01:05.000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-489">2014-01-01T14:01:05.000Z</span></span> |<span data-ttu-id="1e47c-490">2</span><span class="sxs-lookup"><span data-stu-id="1e47c-490">2</span></span> |
| <span data-ttu-id="1e47c-491">2014-01-01T14:01:10.000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-491">2014-01-01T14:01:10.000Z</span></span> |<span data-ttu-id="1e47c-492">2014-01-01T14:01:10.000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-492">2014-01-01T14:01:10.000Z</span></span> |<span data-ttu-id="1e47c-493">3</span><span class="sxs-lookup"><span data-stu-id="1e47c-493">3</span></span> |
| <span data-ttu-id="1e47c-494">2014-01-01T14:01:15.000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-494">2014-01-01T14:01:15.000Z</span></span> |<span data-ttu-id="1e47c-495">2014-01-01T14:01:15.000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-495">2014-01-01T14:01:15.000Z</span></span> |<span data-ttu-id="1e47c-496">4</span><span class="sxs-lookup"><span data-stu-id="1e47c-496">4</span></span> |
| <span data-ttu-id="1e47c-497">2014-01-01T14:01:20.000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-497">2014-01-01T14:01:20.000Z</span></span> |<span data-ttu-id="1e47c-498">2014-01-01T14:01:15.000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-498">2014-01-01T14:01:15.000Z</span></span> |<span data-ttu-id="1e47c-499">4</span><span class="sxs-lookup"><span data-stu-id="1e47c-499">4</span></span> |
| <span data-ttu-id="1e47c-500">2014-01-01T14:01:25.000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-500">2014-01-01T14:01:25.000Z</span></span> |<span data-ttu-id="1e47c-501">2014-01-01T14:01:15.000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-501">2014-01-01T14:01:15.000Z</span></span> |<span data-ttu-id="1e47c-502">4</span><span class="sxs-lookup"><span data-stu-id="1e47c-502">4</span></span> |
| <span data-ttu-id="1e47c-503">2014-01-01T14:01:30.000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-503">2014-01-01T14:01:30.000Z</span></span> |<span data-ttu-id="1e47c-504">2014-01-01T14:01:30.000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-504">2014-01-01T14:01:30.000Z</span></span> |<span data-ttu-id="1e47c-505">5</span><span class="sxs-lookup"><span data-stu-id="1e47c-505">5</span></span> |
| <span data-ttu-id="1e47c-506">2014-01-01T14:01:35.000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-506">2014-01-01T14:01:35.000Z</span></span> |<span data-ttu-id="1e47c-507">2014-01-01T14:01:35.000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-507">2014-01-01T14:01:35.000Z</span></span> |<span data-ttu-id="1e47c-508">6</span><span class="sxs-lookup"><span data-stu-id="1e47c-508">6</span></span> |
| <span data-ttu-id="1e47c-509">2014-01-01T14:01:40.000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-509">2014-01-01T14:01:40.000Z</span></span> |<span data-ttu-id="1e47c-510">2014-01-01T14:01:35.000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-510">2014-01-01T14:01:35.000Z</span></span> |<span data-ttu-id="1e47c-511">6</span><span class="sxs-lookup"><span data-stu-id="1e47c-511">6</span></span> |
| <span data-ttu-id="1e47c-512">2014-01-01T14:01:45.000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-512">2014-01-01T14:01:45.000Z</span></span> |<span data-ttu-id="1e47c-513">2014-01-01T14:01:35.000Z</span><span class="sxs-lookup"><span data-stu-id="1e47c-513">2014-01-01T14:01:35.000Z</span></span> |<span data-ttu-id="1e47c-514">6</span><span class="sxs-lookup"><span data-stu-id="1e47c-514">6</span></span> |

<span data-ttu-id="1e47c-515">**Řešení**:</span><span class="sxs-lookup"><span data-stu-id="1e47c-515">**Solution**:</span></span>

    SELECT
        System.Timestamp AS windowEnd,
        TopOne() OVER (ORDER BY t DESC) AS lastEvent
    FROM
        input TIMESTAMP BY t
    GROUP BY HOPPINGWINDOW(second, 300, 5)


<span data-ttu-id="1e47c-516">**Vysvětlení**: Tento dotaz vygeneruje události každých 5 sekund a výstupy poslední událost, který jste získali dříve.</span><span class="sxs-lookup"><span data-stu-id="1e47c-516">**Explanation**: This query generates events every 5 seconds and outputs the last event that was received previously.</span></span> <span data-ttu-id="1e47c-517">[Hopping okno](https://msdn.microsoft.com/library/dn835041.aspx "Hopping okno – Azure Stream Analytics") doba trvání Určuje, jak daleko zpětný dotaz vypadá najít nejnovější událost (v tomto příkladu je 300 sekund).</span><span class="sxs-lookup"><span data-stu-id="1e47c-517">The [Hopping window](https://msdn.microsoft.com/library/dn835041.aspx "Hopping window--Azure Stream Analytics") duration determines how far back the query looks to find the latest event (300 seconds in this example).</span></span>

## <a name="get-help"></a><span data-ttu-id="1e47c-518">Podpora</span><span class="sxs-lookup"><span data-stu-id="1e47c-518">Get help</span></span>
<span data-ttu-id="1e47c-519">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="1e47c-519">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1e47c-520">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1e47c-520">Next steps</span></span>
* [<span data-ttu-id="1e47c-521">Úvod do služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="1e47c-521">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="1e47c-522">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="1e47c-522">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="1e47c-523">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="1e47c-523">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="1e47c-524">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="1e47c-524">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="1e47c-525">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="1e47c-525">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

