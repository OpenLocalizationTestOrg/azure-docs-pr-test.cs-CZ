---
title: "aaaQuery příkladů běžných vzorů využití v Stream Analytics | Microsoft Docs"
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
ms.openlocfilehash: c8f7a8ac661eaf0281f4140b02c42141b73040fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="query-examples-for-common-stream-analytics-usage-patterns"></a><span data-ttu-id="a7f4b-104">Dotaz příkladů běžných vzorů využití Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="a7f4b-104">Query examples for common Stream Analytics usage patterns</span></span>
## <a name="introduction"></a><span data-ttu-id="a7f4b-105">Úvod</span><span class="sxs-lookup"><span data-stu-id="a7f4b-105">Introduction</span></span>
<span data-ttu-id="a7f4b-106">Dotazy v Azure Stream Analytics jsou vyjádřeny v jazyce SQL jako dotaz.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-106">Queries in Azure Stream Analytics are expressed in a SQL-like query language.</span></span> <span data-ttu-id="a7f4b-107">Tyto dotazy jsou popsané v hello [Stream Analytics query referenční informace k jazyku](https://msdn.microsoft.com/library/azure/dn834998.aspx) průvodce.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-107">These queries are documented in hello [Stream Analytics query language reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) guide.</span></span> <span data-ttu-id="a7f4b-108">Tento článek obsahuje přehled řešení tooseveral běžné typy dotazů, na základě reálných scénářů.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-108">This article outlines solutions tooseveral common query patterns, based on real-world scenarios.</span></span> <span data-ttu-id="a7f4b-109">Je pracuje a pokračuje toobe nové vzory průběžně aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-109">It is a work in progress and continues toobe updated with new patterns on an ongoing basis.</span></span>

## <a name="query-example-convert-data-types"></a><span data-ttu-id="a7f4b-110">Příklad dotazu: Převést datové typy</span><span class="sxs-lookup"><span data-stu-id="a7f4b-110">Query example: Convert data types</span></span>
<span data-ttu-id="a7f4b-111">**Popis**: definování typů hello vlastností na hello vstupního datového proudu.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-111">**Description**: Define hello types of properties on hello input stream.</span></span>
<span data-ttu-id="a7f4b-112">Například hello car váhy pochází na hello vstupního datového proudu jako řetězce a je třeba toobe převést příliš**INT** tooperform **součet** ho nahoru.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-112">For example, hello car weight is coming on hello input stream as strings and needs toobe converted too**INT** tooperform **SUM** it up.</span></span>

<span data-ttu-id="a7f4b-113">**Vstup**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-113">**Input**:</span></span>

| <span data-ttu-id="a7f4b-114">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="a7f4b-114">Make</span></span> | <span data-ttu-id="a7f4b-115">Čas</span><span class="sxs-lookup"><span data-stu-id="a7f4b-115">Time</span></span> | <span data-ttu-id="a7f4b-116">Váha</span><span class="sxs-lookup"><span data-stu-id="a7f4b-116">Weight</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a7f4b-117">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-117">Honda</span></span> |<span data-ttu-id="a7f4b-118">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-118">2015-01-01T00:00:01.0000000Z</span></span> |<span data-ttu-id="a7f4b-119">"1000"</span><span class="sxs-lookup"><span data-stu-id="a7f4b-119">"1000"</span></span> |
| <span data-ttu-id="a7f4b-120">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-120">Honda</span></span> |<span data-ttu-id="a7f4b-121">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-121">2015-01-01T00:00:02.0000000Z</span></span> |<span data-ttu-id="a7f4b-122">"2000"</span><span class="sxs-lookup"><span data-stu-id="a7f4b-122">"2000"</span></span> |

<span data-ttu-id="a7f4b-123">**Výstup**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-123">**Output**:</span></span>

| <span data-ttu-id="a7f4b-124">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="a7f4b-124">Make</span></span> | <span data-ttu-id="a7f4b-125">Váha</span><span class="sxs-lookup"><span data-stu-id="a7f4b-125">Weight</span></span> |
| --- | --- |
| <span data-ttu-id="a7f4b-126">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-126">Honda</span></span> |<span data-ttu-id="a7f4b-127">3000</span><span class="sxs-lookup"><span data-stu-id="a7f4b-127">3000</span></span> |

<span data-ttu-id="a7f4b-128">**Řešení**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-128">**Solution**:</span></span>

    SELECT
        Make,
        SUM(CAST(Weight AS BIGINT)) AS Weight
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

<span data-ttu-id="a7f4b-129">**Vysvětlení**: použití **PŘETYPOVÁNÍ** příkaz v hello **váhy** pole toospecify jeho datového typu.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-129">**Explanation**: Use a **CAST** statement in hello **Weight** field toospecify its data type.</span></span> <span data-ttu-id="a7f4b-130">Zobrazit seznam hello podporované datové typy v [datové typy (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835065.aspx).</span><span class="sxs-lookup"><span data-stu-id="a7f4b-130">See hello list of supported data types in [Data types (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835065.aspx).</span></span>

## <a name="query-example-use-likenot-like-toodo-pattern-matching"></a><span data-ttu-id="a7f4b-131">Příklad dotazu: použijte jako nebo nebyla jako toodo shoda vzoru</span><span class="sxs-lookup"><span data-stu-id="a7f4b-131">Query example: Use Like/Not like toodo pattern matching</span></span>
<span data-ttu-id="a7f4b-132">**Popis**: Zkontrolujte, jestli hodnotu pole na hello události odpovídá určitého.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-132">**Description**: Check that a field value on hello event matches a certain pattern.</span></span>
<span data-ttu-id="a7f4b-133">Například zkontrolujte, že hello výsledek vrátí desky licencí, které se začínat a končit 9.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-133">For example, check that hello result returns license plates that start with A and end with 9.</span></span>

<span data-ttu-id="a7f4b-134">**Vstup**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-134">**Input**:</span></span>

| <span data-ttu-id="a7f4b-135">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="a7f4b-135">Make</span></span> | <span data-ttu-id="a7f4b-136">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="a7f4b-136">LicensePlate</span></span> | <span data-ttu-id="a7f4b-137">Čas</span><span class="sxs-lookup"><span data-stu-id="a7f4b-137">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a7f4b-138">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-138">Honda</span></span> |<span data-ttu-id="a7f4b-139">ABC 123</span><span class="sxs-lookup"><span data-stu-id="a7f4b-139">ABC-123</span></span> |<span data-ttu-id="a7f4b-140">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-140">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-141">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-141">Toyota</span></span> |<span data-ttu-id="a7f4b-142">AAA-999</span><span class="sxs-lookup"><span data-stu-id="a7f4b-142">AAA-999</span></span> |<span data-ttu-id="a7f4b-143">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-143">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-144">Nissan</span><span class="sxs-lookup"><span data-stu-id="a7f4b-144">Nissan</span></span> |<span data-ttu-id="a7f4b-145">ABC 369</span><span class="sxs-lookup"><span data-stu-id="a7f4b-145">ABC-369</span></span> |<span data-ttu-id="a7f4b-146">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-146">2015-01-01T00:00:03.0000000Z</span></span> |

<span data-ttu-id="a7f4b-147">**Výstup**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-147">**Output**:</span></span>

| <span data-ttu-id="a7f4b-148">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="a7f4b-148">Make</span></span> | <span data-ttu-id="a7f4b-149">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="a7f4b-149">LicensePlate</span></span> | <span data-ttu-id="a7f4b-150">Čas</span><span class="sxs-lookup"><span data-stu-id="a7f4b-150">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a7f4b-151">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-151">Toyota</span></span> |<span data-ttu-id="a7f4b-152">AAA-999</span><span class="sxs-lookup"><span data-stu-id="a7f4b-152">AAA-999</span></span> |<span data-ttu-id="a7f4b-153">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-153">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-154">Nissan</span><span class="sxs-lookup"><span data-stu-id="a7f4b-154">Nissan</span></span> |<span data-ttu-id="a7f4b-155">ABC 369</span><span class="sxs-lookup"><span data-stu-id="a7f4b-155">ABC-369</span></span> |<span data-ttu-id="a7f4b-156">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-156">2015-01-01T00:00:03.0000000Z</span></span> |

<span data-ttu-id="a7f4b-157">**Řešení**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-157">**Solution**:</span></span>

    SELECT
        *
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LicensePlate LIKE 'A%9'

<span data-ttu-id="a7f4b-158">**Vysvětlení**: použití hello **jako** příkaz toocheck hello **LicensePlate** pole hodnota.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-158">**Explanation**: Use hello **LIKE** statement toocheck hello **LicensePlate** field value.</span></span> <span data-ttu-id="a7f4b-159">By měl začínat A potom mít libovolný řetězec nula nebo více znaků a pak končit 9.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-159">It should start with an A, then have any string of zero or more characters, and then end with a 9.</span></span> 

## <a name="query-example-specify-logic-for-different-casesvalues-case-statements"></a><span data-ttu-id="a7f4b-160">Příklad dotazu: Zadejte logiku pro různé případech nebo hodnoty (CASE – příkazy)</span><span class="sxs-lookup"><span data-stu-id="a7f4b-160">Query example: Specify logic for different cases/values (CASE statements)</span></span>
<span data-ttu-id="a7f4b-161">**Popis**: Zadejte jiný výpočet pro pole, v závislosti na konkrétní kritérium.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-161">**Description**: Provide a different computation for a field, based on a particular criterion.</span></span>
<span data-ttu-id="a7f4b-162">Například zadejte popis řetězce, pro kolik aut hello stejné zkontrolujte byla dokončena s ve speciálním případě pro 1.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-162">For example, provide a string description for how many cars of hello same make passed, with a special case for 1.</span></span>

<span data-ttu-id="a7f4b-163">**Vstup**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-163">**Input**:</span></span>

| <span data-ttu-id="a7f4b-164">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="a7f4b-164">Make</span></span> | <span data-ttu-id="a7f4b-165">Čas</span><span class="sxs-lookup"><span data-stu-id="a7f4b-165">Time</span></span> |
| --- | --- |
| <span data-ttu-id="a7f4b-166">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-166">Honda</span></span> |<span data-ttu-id="a7f4b-167">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-167">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-168">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-168">Toyota</span></span> |<span data-ttu-id="a7f4b-169">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-169">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-170">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-170">Toyota</span></span> |<span data-ttu-id="a7f4b-171">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-171">2015-01-01T00:00:03.0000000Z</span></span> |

<span data-ttu-id="a7f4b-172">**Výstup**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-172">**Output**:</span></span>

| <span data-ttu-id="a7f4b-173">CarsPassed</span><span class="sxs-lookup"><span data-stu-id="a7f4b-173">CarsPassed</span></span> | <span data-ttu-id="a7f4b-174">Čas</span><span class="sxs-lookup"><span data-stu-id="a7f4b-174">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a7f4b-175">1 Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-175">1 Honda</span></span> |<span data-ttu-id="a7f4b-176">2015-01-01T00:00:10.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-176">2015-01-01T00:00:10.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-177">2 Toyotas</span><span class="sxs-lookup"><span data-stu-id="a7f4b-177">2 Toyotas</span></span> |<span data-ttu-id="a7f4b-178">2015-01-01T00:00:10.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-178">2015-01-01T00:00:10.0000000Z</span></span> |

<span data-ttu-id="a7f4b-179">**Řešení**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-179">**Solution**:</span></span>

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

<span data-ttu-id="a7f4b-180">**Vysvětlení**: hello **případ** klauzule umožňuje tooprovide různých výpočtů, na základě některé kritérií (v našem případě hello počet aut hello v okně agregační hello).</span><span class="sxs-lookup"><span data-stu-id="a7f4b-180">**Explanation**: hello **CASE** clause allows us tooprovide a different computation, based on some criteria (in our case, hello count of hello cars in hello aggregate window).</span></span>

## <a name="query-example-send-data-toomultiple-outputs"></a><span data-ttu-id="a7f4b-181">Příklad dotazu: Odeslat data toomultiple výstupy</span><span class="sxs-lookup"><span data-stu-id="a7f4b-181">Query example: Send data toomultiple outputs</span></span>
<span data-ttu-id="a7f4b-182">**Popis**: odesílání dat toomultiple výstup cíle z jediné úlohy.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-182">**Description**: Send data toomultiple output targets from a single job.</span></span>
<span data-ttu-id="a7f4b-183">Například analyzovat data pro upozornění na základě prahové hodnoty a všechny události tooblob úložiště archivu.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-183">For example, analyze data for a threshold-based alert and archive all events tooblob storage.</span></span>

<span data-ttu-id="a7f4b-184">**Vstup**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-184">**Input**:</span></span>

| <span data-ttu-id="a7f4b-185">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="a7f4b-185">Make</span></span> | <span data-ttu-id="a7f4b-186">Čas</span><span class="sxs-lookup"><span data-stu-id="a7f4b-186">Time</span></span> |
| --- | --- |
| <span data-ttu-id="a7f4b-187">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-187">Honda</span></span> |<span data-ttu-id="a7f4b-188">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-188">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-189">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-189">Honda</span></span> |<span data-ttu-id="a7f4b-190">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-190">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-191">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-191">Toyota</span></span> |<span data-ttu-id="a7f4b-192">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-192">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-193">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-193">Toyota</span></span> |<span data-ttu-id="a7f4b-194">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-194">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-195">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-195">Toyota</span></span> |<span data-ttu-id="a7f4b-196">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-196">2015-01-01T00:00:03.0000000Z</span></span> |

<span data-ttu-id="a7f4b-197">**Output1**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-197">**Output1**:</span></span>

| <span data-ttu-id="a7f4b-198">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="a7f4b-198">Make</span></span> | <span data-ttu-id="a7f4b-199">Čas</span><span class="sxs-lookup"><span data-stu-id="a7f4b-199">Time</span></span> |
| --- | --- |
| <span data-ttu-id="a7f4b-200">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-200">Honda</span></span> |<span data-ttu-id="a7f4b-201">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-201">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-202">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-202">Honda</span></span> |<span data-ttu-id="a7f4b-203">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-203">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-204">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-204">Toyota</span></span> |<span data-ttu-id="a7f4b-205">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-205">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-206">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-206">Toyota</span></span> |<span data-ttu-id="a7f4b-207">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-207">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-208">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-208">Toyota</span></span> |<span data-ttu-id="a7f4b-209">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-209">2015-01-01T00:00:03.0000000Z</span></span> |

<span data-ttu-id="a7f4b-210">**Output2**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-210">**Output2**:</span></span>

| <span data-ttu-id="a7f4b-211">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="a7f4b-211">Make</span></span> | <span data-ttu-id="a7f4b-212">Čas</span><span class="sxs-lookup"><span data-stu-id="a7f4b-212">Time</span></span> | <span data-ttu-id="a7f4b-213">Počet</span><span class="sxs-lookup"><span data-stu-id="a7f4b-213">Count</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a7f4b-214">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-214">Toyota</span></span> |<span data-ttu-id="a7f4b-215">2015-01-01T00:00:10.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-215">2015-01-01T00:00:10.0000000Z</span></span> |<span data-ttu-id="a7f4b-216">3</span><span class="sxs-lookup"><span data-stu-id="a7f4b-216">3</span></span> |

<span data-ttu-id="a7f4b-217">**Řešení**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-217">**Solution**:</span></span>

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

<span data-ttu-id="a7f4b-218">**Vysvětlení**: hello **INTO** klauzule informuje Stream Analytics, které hello výstupy toowrite hello data toofrom tohoto prohlášení.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-218">**Explanation**: hello **INTO** clause tells Stream Analytics which of hello outputs toowrite hello data toofrom this statement.</span></span>
<span data-ttu-id="a7f4b-219">první dotaz Hello je průchozí hello dat dostali jsme tooan výstupu, který jsme pojmenovali **ArchiveOutput**.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-219">hello first query is a pass-through of hello data we received tooan output that we named **ArchiveOutput**.</span></span>
<span data-ttu-id="a7f4b-220">druhý dotaz Hello nepodporuje některé jednoduché agregace a filtrování a odešle výsledky hello tooa podřízené výstrahy systému.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-220">hello second query does some simple aggregation and filtering, and it sends hello results tooa downstream alerting system.</span></span>

<span data-ttu-id="a7f4b-221">Všimněte si, že můžete také znovu použít hello výsledky hello běžných výrazech tabulky (odkazu Cte) (například **WITH** příkazy) ve více příkazů výstup.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-221">Note that you can also reuse hello results of hello common table expressions (CTEs) (such as **WITH** statements) in multiple output statements.</span></span> <span data-ttu-id="a7f4b-222">Tato možnost je hello dodatečná výhoda otevření méně čtečky toohello vstupní zdroj.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-222">This option has hello added benefit of opening fewer readers toohello input source.</span></span>
<span data-ttu-id="a7f4b-223">Například:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-223">For example:</span></span> 

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

## <a name="query-example-count-unique-values"></a><span data-ttu-id="a7f4b-224">Příklad dotazu: počet jedinečné hodnoty</span><span class="sxs-lookup"><span data-stu-id="a7f4b-224">Query example: Count unique values</span></span>
<span data-ttu-id="a7f4b-225">**Popis**: počet hello jedinečné pole hodnoty, které se zobrazují v datovém proudu hello v rámci časové okno.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-225">**Description**: Count hello number of unique field values that appear in hello stream within a time window.</span></span>
<span data-ttu-id="a7f4b-226">Například kolik jedinečný díky automobilů předána hello projedou stánek v okně sekundu 2?</span><span class="sxs-lookup"><span data-stu-id="a7f4b-226">For example, how many unique makes of cars passed through hello toll booth in a 2-second window?</span></span>

<span data-ttu-id="a7f4b-227">**Vstup**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-227">**Input**:</span></span>

| <span data-ttu-id="a7f4b-228">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="a7f4b-228">Make</span></span> | <span data-ttu-id="a7f4b-229">Čas</span><span class="sxs-lookup"><span data-stu-id="a7f4b-229">Time</span></span> |
| --- | --- |
| <span data-ttu-id="a7f4b-230">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-230">Honda</span></span> |<span data-ttu-id="a7f4b-231">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-231">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-232">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-232">Honda</span></span> |<span data-ttu-id="a7f4b-233">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-233">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-234">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-234">Toyota</span></span> |<span data-ttu-id="a7f4b-235">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-235">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-236">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-236">Toyota</span></span> |<span data-ttu-id="a7f4b-237">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-237">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-238">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-238">Toyota</span></span> |<span data-ttu-id="a7f4b-239">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-239">2015-01-01T00:00:03.0000000Z</span></span> |

<span data-ttu-id="a7f4b-240">**Výstup:**</span><span class="sxs-lookup"><span data-stu-id="a7f4b-240">**Output:**</span></span>

| <span data-ttu-id="a7f4b-241">Počet</span><span class="sxs-lookup"><span data-stu-id="a7f4b-241">Count</span></span> | <span data-ttu-id="a7f4b-242">Čas</span><span class="sxs-lookup"><span data-stu-id="a7f4b-242">Time</span></span> |
| --- | --- |
| <span data-ttu-id="a7f4b-243">2</span><span class="sxs-lookup"><span data-stu-id="a7f4b-243">2</span></span> |<span data-ttu-id="a7f4b-244">2015-01-01T00:00:02.000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-244">2015-01-01T00:00:02.000Z</span></span> |
| <span data-ttu-id="a7f4b-245">1</span><span class="sxs-lookup"><span data-stu-id="a7f4b-245">1</span></span> |<span data-ttu-id="a7f4b-246">2015-01-01T00:00:04.000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-246">2015-01-01T00:00:04.000Z</span></span> |

<span data-ttu-id="a7f4b-247">**Řešení:**</span><span class="sxs-lookup"><span data-stu-id="a7f4b-247">**Solution:**</span></span>

````
SELECT
     COUNT(DISTINCT Make) AS CountMake,
     System.TIMESTAMP AS TIME
FROM Input TIMESTAMP BY TIME
GROUP BY 
     TumblingWindow(second, 2)
````


<span data-ttu-id="a7f4b-248">**Vysvětlení:**
**COUNT (odlišné ověřte)** hello vrátí počet jedinečných hodnot v hello **zkontrolujte** sloupec v rámci časové okno.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-248">**Explanation:**
**COUNT(DISTINCT Make)** returns hello number of distinct values in hello **Make** column within a time window.</span></span>

## <a name="query-example-determine-if-a-value-has-changed"></a><span data-ttu-id="a7f4b-249">Příklad dotazu: určení, pokud byla hodnota změněna</span><span class="sxs-lookup"><span data-stu-id="a7f4b-249">Query example: Determine if a value has changed</span></span>
<span data-ttu-id="a7f4b-250">**Popis**: Podívejte se na předchozí toodetermine hodnotu, pokud se liší od aktuální hodnota hello.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-250">**Description**: Look at a previous value toodetermine if it is different than hello current value.</span></span>
<span data-ttu-id="a7f4b-251">Je třeba předchozí car hello na hello projedou silniční hello, které stejná jako aktuální car hello provést?</span><span class="sxs-lookup"><span data-stu-id="a7f4b-251">For example, is hello previous car on hello toll road hello same make as hello current car?</span></span>

<span data-ttu-id="a7f4b-252">**Vstup**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-252">**Input**:</span></span>

| <span data-ttu-id="a7f4b-253">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="a7f4b-253">Make</span></span> | <span data-ttu-id="a7f4b-254">Čas</span><span class="sxs-lookup"><span data-stu-id="a7f4b-254">Time</span></span> |
| --- | --- |
| <span data-ttu-id="a7f4b-255">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-255">Honda</span></span> |<span data-ttu-id="a7f4b-256">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-256">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-257">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-257">Toyota</span></span> |<span data-ttu-id="a7f4b-258">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-258">2015-01-01T00:00:02.0000000Z</span></span> |

<span data-ttu-id="a7f4b-259">**Výstup**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-259">**Output**:</span></span>

| <span data-ttu-id="a7f4b-260">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="a7f4b-260">Make</span></span> | <span data-ttu-id="a7f4b-261">Čas</span><span class="sxs-lookup"><span data-stu-id="a7f4b-261">Time</span></span> |
| --- | --- |
| <span data-ttu-id="a7f4b-262">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-262">Toyota</span></span> |<span data-ttu-id="a7f4b-263">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-263">2015-01-01T00:00:02.0000000Z</span></span> |

<span data-ttu-id="a7f4b-264">**Řešení**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-264">**Solution**:</span></span>

    SELECT
        Make,
        Time
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(minute, 1)) <> Make

<span data-ttu-id="a7f4b-265">**Vysvětlení**: použití **PRODLEVA** toopeek do hello vstupní datový proud zpět jedné události a získat hello **zkontrolujte** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-265">**Explanation**: Use **LAG** toopeek into hello input stream one event back and get hello **Make** value.</span></span> <span data-ttu-id="a7f4b-266">Porovnejte je toohello **zkontrolujte** hodnotu na aktuální a výstup hello události hello, pokud se liší.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-266">Then compare it toohello **Make** value on hello current event and output hello event if they are different.</span></span>

## <a name="query-example-find-hello-first-event-in-a-window"></a><span data-ttu-id="a7f4b-267">Příklad dotazu: najít hello první událost v okně</span><span class="sxs-lookup"><span data-stu-id="a7f4b-267">Query example: Find hello first event in a window</span></span>
<span data-ttu-id="a7f4b-268">**Popis**: najít hello prvního auta v intervalu každých 10 minut.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-268">**Description**: Find hello first car in every 10-minute interval.</span></span>

<span data-ttu-id="a7f4b-269">**Vstup**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-269">**Input**:</span></span>

| <span data-ttu-id="a7f4b-270">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="a7f4b-270">LicensePlate</span></span> | <span data-ttu-id="a7f4b-271">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="a7f4b-271">Make</span></span> | <span data-ttu-id="a7f4b-272">Čas</span><span class="sxs-lookup"><span data-stu-id="a7f4b-272">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a7f4b-273">DXE 5291</span><span class="sxs-lookup"><span data-stu-id="a7f4b-273">DXE 5291</span></span> |<span data-ttu-id="a7f4b-274">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-274">Honda</span></span> |<span data-ttu-id="a7f4b-275">2015-07-27T00:00:05.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-275">2015-07-27T00:00:05.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-276">YZK 5704</span><span class="sxs-lookup"><span data-stu-id="a7f4b-276">YZK 5704</span></span> |<span data-ttu-id="a7f4b-277">Ford</span><span class="sxs-lookup"><span data-stu-id="a7f4b-277">Ford</span></span> |<span data-ttu-id="a7f4b-278">2015-07-27T00:02:17.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-278">2015-07-27T00:02:17.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-279">RMV 8282</span><span class="sxs-lookup"><span data-stu-id="a7f4b-279">RMV 8282</span></span> |<span data-ttu-id="a7f4b-280">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-280">Honda</span></span> |<span data-ttu-id="a7f4b-281">2015-07-27T00:05:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-281">2015-07-27T00:05:01.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-282">YHN 6970</span><span class="sxs-lookup"><span data-stu-id="a7f4b-282">YHN 6970</span></span> |<span data-ttu-id="a7f4b-283">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-283">Toyota</span></span> |<span data-ttu-id="a7f4b-284">2015-07-27T00:06:00.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-284">2015-07-27T00:06:00.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-285">VFE 1616</span><span class="sxs-lookup"><span data-stu-id="a7f4b-285">VFE 1616</span></span> |<span data-ttu-id="a7f4b-286">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-286">Toyota</span></span> |<span data-ttu-id="a7f4b-287">2015-07-27T00:09:31.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-287">2015-07-27T00:09:31.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-288">QYF 9358</span><span class="sxs-lookup"><span data-stu-id="a7f4b-288">QYF 9358</span></span> |<span data-ttu-id="a7f4b-289">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-289">Honda</span></span> |<span data-ttu-id="a7f4b-290">2015-07-27T00:12:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-290">2015-07-27T00:12:02.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-291">MDR 6128</span><span class="sxs-lookup"><span data-stu-id="a7f4b-291">MDR 6128</span></span> |<span data-ttu-id="a7f4b-292">BMW</span><span class="sxs-lookup"><span data-stu-id="a7f4b-292">BMW</span></span> |<span data-ttu-id="a7f4b-293">2015-07-27T00:13:45.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-293">2015-07-27T00:13:45.0000000Z</span></span> |

<span data-ttu-id="a7f4b-294">**Výstup**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-294">**Output**:</span></span>

| <span data-ttu-id="a7f4b-295">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="a7f4b-295">LicensePlate</span></span> | <span data-ttu-id="a7f4b-296">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="a7f4b-296">Make</span></span> | <span data-ttu-id="a7f4b-297">Čas</span><span class="sxs-lookup"><span data-stu-id="a7f4b-297">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a7f4b-298">DXE 5291</span><span class="sxs-lookup"><span data-stu-id="a7f4b-298">DXE 5291</span></span> |<span data-ttu-id="a7f4b-299">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-299">Honda</span></span> |<span data-ttu-id="a7f4b-300">2015-07-27T00:00:05.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-300">2015-07-27T00:00:05.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-301">QYF 9358</span><span class="sxs-lookup"><span data-stu-id="a7f4b-301">QYF 9358</span></span> |<span data-ttu-id="a7f4b-302">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-302">Honda</span></span> |<span data-ttu-id="a7f4b-303">2015-07-27T00:12:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-303">2015-07-27T00:12:02.0000000Z</span></span> |

<span data-ttu-id="a7f4b-304">**Řešení**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-304">**Solution**:</span></span>

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) = 1

<span data-ttu-id="a7f4b-305">Teď vytvoříme změnu hello problému a najít hello prvního auta konkrétní třídy v intervalu každých 10 minut.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-305">Now let’s change hello problem and find hello first car of a particular make in every 10-minute interval.</span></span>

| <span data-ttu-id="a7f4b-306">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="a7f4b-306">LicensePlate</span></span> | <span data-ttu-id="a7f4b-307">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="a7f4b-307">Make</span></span> | <span data-ttu-id="a7f4b-308">Čas</span><span class="sxs-lookup"><span data-stu-id="a7f4b-308">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a7f4b-309">DXE 5291</span><span class="sxs-lookup"><span data-stu-id="a7f4b-309">DXE 5291</span></span> |<span data-ttu-id="a7f4b-310">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-310">Honda</span></span> |<span data-ttu-id="a7f4b-311">2015-07-27T00:00:05.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-311">2015-07-27T00:00:05.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-312">YZK 5704</span><span class="sxs-lookup"><span data-stu-id="a7f4b-312">YZK 5704</span></span> |<span data-ttu-id="a7f4b-313">Ford</span><span class="sxs-lookup"><span data-stu-id="a7f4b-313">Ford</span></span> |<span data-ttu-id="a7f4b-314">2015-07-27T00:02:17.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-314">2015-07-27T00:02:17.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-315">YHN 6970</span><span class="sxs-lookup"><span data-stu-id="a7f4b-315">YHN 6970</span></span> |<span data-ttu-id="a7f4b-316">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-316">Toyota</span></span> |<span data-ttu-id="a7f4b-317">2015-07-27T00:06:00.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-317">2015-07-27T00:06:00.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-318">QYF 9358</span><span class="sxs-lookup"><span data-stu-id="a7f4b-318">QYF 9358</span></span> |<span data-ttu-id="a7f4b-319">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-319">Honda</span></span> |<span data-ttu-id="a7f4b-320">2015-07-27T00:12:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-320">2015-07-27T00:12:02.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-321">MDR 6128</span><span class="sxs-lookup"><span data-stu-id="a7f4b-321">MDR 6128</span></span> |<span data-ttu-id="a7f4b-322">BMW</span><span class="sxs-lookup"><span data-stu-id="a7f4b-322">BMW</span></span> |<span data-ttu-id="a7f4b-323">2015-07-27T00:13:45.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-323">2015-07-27T00:13:45.0000000Z</span></span> |

<span data-ttu-id="a7f4b-324">**Řešení**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-324">**Solution**:</span></span>

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) OVER (PARTITION BY Make) = 1

## <a name="query-example-find-hello-last-event-in-a-window"></a><span data-ttu-id="a7f4b-325">Příklad dotazu: najít hello poslední událost v okně</span><span class="sxs-lookup"><span data-stu-id="a7f4b-325">Query example: Find hello last event in a window</span></span>
<span data-ttu-id="a7f4b-326">**Popis**: najít hello poslední car v intervalu každých 10 minut.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-326">**Description**: Find hello last car in every 10-minute interval.</span></span>

<span data-ttu-id="a7f4b-327">**Vstup**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-327">**Input**:</span></span>

| <span data-ttu-id="a7f4b-328">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="a7f4b-328">LicensePlate</span></span> | <span data-ttu-id="a7f4b-329">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="a7f4b-329">Make</span></span> | <span data-ttu-id="a7f4b-330">Čas</span><span class="sxs-lookup"><span data-stu-id="a7f4b-330">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a7f4b-331">DXE 5291</span><span class="sxs-lookup"><span data-stu-id="a7f4b-331">DXE 5291</span></span> |<span data-ttu-id="a7f4b-332">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-332">Honda</span></span> |<span data-ttu-id="a7f4b-333">2015-07-27T00:00:05.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-333">2015-07-27T00:00:05.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-334">YZK 5704</span><span class="sxs-lookup"><span data-stu-id="a7f4b-334">YZK 5704</span></span> |<span data-ttu-id="a7f4b-335">Ford</span><span class="sxs-lookup"><span data-stu-id="a7f4b-335">Ford</span></span> |<span data-ttu-id="a7f4b-336">2015-07-27T00:02:17.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-336">2015-07-27T00:02:17.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-337">RMV 8282</span><span class="sxs-lookup"><span data-stu-id="a7f4b-337">RMV 8282</span></span> |<span data-ttu-id="a7f4b-338">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-338">Honda</span></span> |<span data-ttu-id="a7f4b-339">2015-07-27T00:05:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-339">2015-07-27T00:05:01.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-340">YHN 6970</span><span class="sxs-lookup"><span data-stu-id="a7f4b-340">YHN 6970</span></span> |<span data-ttu-id="a7f4b-341">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-341">Toyota</span></span> |<span data-ttu-id="a7f4b-342">2015-07-27T00:06:00.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-342">2015-07-27T00:06:00.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-343">VFE 1616</span><span class="sxs-lookup"><span data-stu-id="a7f4b-343">VFE 1616</span></span> |<span data-ttu-id="a7f4b-344">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-344">Toyota</span></span> |<span data-ttu-id="a7f4b-345">2015-07-27T00:09:31.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-345">2015-07-27T00:09:31.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-346">QYF 9358</span><span class="sxs-lookup"><span data-stu-id="a7f4b-346">QYF 9358</span></span> |<span data-ttu-id="a7f4b-347">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-347">Honda</span></span> |<span data-ttu-id="a7f4b-348">2015-07-27T00:12:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-348">2015-07-27T00:12:02.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-349">MDR 6128</span><span class="sxs-lookup"><span data-stu-id="a7f4b-349">MDR 6128</span></span> |<span data-ttu-id="a7f4b-350">BMW</span><span class="sxs-lookup"><span data-stu-id="a7f4b-350">BMW</span></span> |<span data-ttu-id="a7f4b-351">2015-07-27T00:13:45.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-351">2015-07-27T00:13:45.0000000Z</span></span> |

<span data-ttu-id="a7f4b-352">**Výstup**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-352">**Output**:</span></span>

| <span data-ttu-id="a7f4b-353">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="a7f4b-353">LicensePlate</span></span> | <span data-ttu-id="a7f4b-354">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="a7f4b-354">Make</span></span> | <span data-ttu-id="a7f4b-355">Čas</span><span class="sxs-lookup"><span data-stu-id="a7f4b-355">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a7f4b-356">VFE 1616</span><span class="sxs-lookup"><span data-stu-id="a7f4b-356">VFE 1616</span></span> |<span data-ttu-id="a7f4b-357">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-357">Toyota</span></span> |<span data-ttu-id="a7f4b-358">2015-07-27T00:09:31.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-358">2015-07-27T00:09:31.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-359">MDR 6128</span><span class="sxs-lookup"><span data-stu-id="a7f4b-359">MDR 6128</span></span> |<span data-ttu-id="a7f4b-360">BMW</span><span class="sxs-lookup"><span data-stu-id="a7f4b-360">BMW</span></span> |<span data-ttu-id="a7f4b-361">2015-07-27T00:13:45.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-361">2015-07-27T00:13:45.0000000Z</span></span> |

<span data-ttu-id="a7f4b-362">**Řešení**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-362">**Solution**:</span></span>

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

<span data-ttu-id="a7f4b-363">**Vysvětlení**: existují dva kroky v dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-363">**Explanation**: There are two steps in hello query.</span></span> <span data-ttu-id="a7f4b-364">Hello první jeden najde hello nejnovější časové razítko v systému windows 10 minut.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-364">hello first one finds hello latest time stamp in 10-minute windows.</span></span> <span data-ttu-id="a7f4b-365">druhý krok spojení Hello hello výsledky hello první dotaz s hello původní datový proud toofind hello události, které odpovídají hello poslední časová razítka v každém okně.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-365">hello second step joins hello results of hello first query with hello original stream toofind hello events that match hello last time stamps in each window.</span></span> 

## <a name="query-example-detect-hello-absence-of-events"></a><span data-ttu-id="a7f4b-366">Příklad dotazu: zjištění hello absence událostí</span><span class="sxs-lookup"><span data-stu-id="a7f4b-366">Query example: Detect hello absence of events</span></span>
<span data-ttu-id="a7f4b-367">**Popis**: Zkontrolujte, že datový proud má žádná hodnota, která odpovídá určité kritérium.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-367">**Description**: Check that a stream has no value that matches a certain criterion.</span></span>
<span data-ttu-id="a7f4b-368">Například 2 po sobě jdoucích automobily od hello stejné provedený zadali hello projedou silniční v rámci hello posledních 90 sekund?</span><span class="sxs-lookup"><span data-stu-id="a7f4b-368">For example, have 2 consecutive cars from hello same make entered hello toll road within hello last 90 seconds?</span></span>

<span data-ttu-id="a7f4b-369">**Vstup**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-369">**Input**:</span></span>

| <span data-ttu-id="a7f4b-370">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="a7f4b-370">Make</span></span> | <span data-ttu-id="a7f4b-371">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="a7f4b-371">LicensePlate</span></span> | <span data-ttu-id="a7f4b-372">Čas</span><span class="sxs-lookup"><span data-stu-id="a7f4b-372">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a7f4b-373">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-373">Honda</span></span> |<span data-ttu-id="a7f4b-374">ABC 123</span><span class="sxs-lookup"><span data-stu-id="a7f4b-374">ABC-123</span></span> |<span data-ttu-id="a7f4b-375">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-375">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-376">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-376">Honda</span></span> |<span data-ttu-id="a7f4b-377">AAA-999</span><span class="sxs-lookup"><span data-stu-id="a7f4b-377">AAA-999</span></span> |<span data-ttu-id="a7f4b-378">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-378">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-379">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-379">Toyota</span></span> |<span data-ttu-id="a7f4b-380">DEF 987</span><span class="sxs-lookup"><span data-stu-id="a7f4b-380">DEF-987</span></span> |<span data-ttu-id="a7f4b-381">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-381">2015-01-01T00:00:03.0000000Z</span></span> |
| <span data-ttu-id="a7f4b-382">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-382">Honda</span></span> |<span data-ttu-id="a7f4b-383">GHI 345</span><span class="sxs-lookup"><span data-stu-id="a7f4b-383">GHI-345</span></span> |<span data-ttu-id="a7f4b-384">2015-01-01T00:00:04.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-384">2015-01-01T00:00:04.0000000Z</span></span> |

<span data-ttu-id="a7f4b-385">**Výstup**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-385">**Output**:</span></span>

| <span data-ttu-id="a7f4b-386">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="a7f4b-386">Make</span></span> | <span data-ttu-id="a7f4b-387">Čas</span><span class="sxs-lookup"><span data-stu-id="a7f4b-387">Time</span></span> | <span data-ttu-id="a7f4b-388">CurrentCarLicensePlate</span><span class="sxs-lookup"><span data-stu-id="a7f4b-388">CurrentCarLicensePlate</span></span> | <span data-ttu-id="a7f4b-389">FirstCarLicensePlate</span><span class="sxs-lookup"><span data-stu-id="a7f4b-389">FirstCarLicensePlate</span></span> | <span data-ttu-id="a7f4b-390">FirstCarTime</span><span class="sxs-lookup"><span data-stu-id="a7f4b-390">FirstCarTime</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="a7f4b-391">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-391">Honda</span></span> |<span data-ttu-id="a7f4b-392">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-392">2015-01-01T00:00:02.0000000Z</span></span> |<span data-ttu-id="a7f4b-393">AAA-999</span><span class="sxs-lookup"><span data-stu-id="a7f4b-393">AAA-999</span></span> |<span data-ttu-id="a7f4b-394">ABC 123</span><span class="sxs-lookup"><span data-stu-id="a7f4b-394">ABC-123</span></span> |<span data-ttu-id="a7f4b-395">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-395">2015-01-01T00:00:01.0000000Z</span></span> |

<span data-ttu-id="a7f4b-396">**Řešení**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-396">**Solution**:</span></span>

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

<span data-ttu-id="a7f4b-397">**Vysvětlení**: použití **PRODLEVA** toopeek do hello vstupní datový proud zpět jedné události a získat hello **zkontrolujte** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-397">**Explanation**: Use **LAG** toopeek into hello input stream one event back and get hello **Make** value.</span></span> <span data-ttu-id="a7f4b-398">Porovnejte je toohello **ZKONTROLUJTE** hodnota hello aktuální událost a potom výstupní hello událost, pokud jejich jsou stejné hello.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-398">Compare it toohello **MAKE** value in hello current event, and then output hello event if they are hello same.</span></span> <span data-ttu-id="a7f4b-399">Můžete také použít **PRODLEVA** tooget data o předchozí car hello.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-399">You can also use **LAG** tooget data about hello previous car.</span></span>

## <a name="query-example-detect-hello-duration-between-events"></a><span data-ttu-id="a7f4b-400">Příklad dotazu: zjistit dobu trvání hello mezi událostmi</span><span class="sxs-lookup"><span data-stu-id="a7f4b-400">Query example: Detect hello duration between events</span></span>
<span data-ttu-id="a7f4b-401">**Popis**: najít hello trvání dané události.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-401">**Description**: Find hello duration of a given event.</span></span> <span data-ttu-id="a7f4b-402">Zadané webové clickstream, například určete hello čas strávený na funkce.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-402">For example, given a web clickstream, determine hello time spent on a feature.</span></span>

<span data-ttu-id="a7f4b-403">**Vstup**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-403">**Input**:</span></span>  

| <span data-ttu-id="a7f4b-404">Uživatel</span><span class="sxs-lookup"><span data-stu-id="a7f4b-404">User</span></span> | <span data-ttu-id="a7f4b-405">Funkce</span><span class="sxs-lookup"><span data-stu-id="a7f4b-405">Feature</span></span> | <span data-ttu-id="a7f4b-406">Událost</span><span class="sxs-lookup"><span data-stu-id="a7f4b-406">Event</span></span> | <span data-ttu-id="a7f4b-407">Čas</span><span class="sxs-lookup"><span data-stu-id="a7f4b-407">Time</span></span> |
| --- | --- | --- | --- |
| user@location.com |<span data-ttu-id="a7f4b-408">RightMenu</span><span class="sxs-lookup"><span data-stu-id="a7f4b-408">RightMenu</span></span> |<span data-ttu-id="a7f4b-409">Start</span><span class="sxs-lookup"><span data-stu-id="a7f4b-409">Start</span></span> |<span data-ttu-id="a7f4b-410">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-410">2015-01-01T00:00:01.0000000Z</span></span> |
| user@location.com |<span data-ttu-id="a7f4b-411">RightMenu</span><span class="sxs-lookup"><span data-stu-id="a7f4b-411">RightMenu</span></span> |<span data-ttu-id="a7f4b-412">End</span><span class="sxs-lookup"><span data-stu-id="a7f4b-412">End</span></span> |<span data-ttu-id="a7f4b-413">2015-01-01T00:00:08.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-413">2015-01-01T00:00:08.0000000Z</span></span> |

<span data-ttu-id="a7f4b-414">**Výstup**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-414">**Output**:</span></span>  

| <span data-ttu-id="a7f4b-415">Uživatel</span><span class="sxs-lookup"><span data-stu-id="a7f4b-415">User</span></span> | <span data-ttu-id="a7f4b-416">Funkce</span><span class="sxs-lookup"><span data-stu-id="a7f4b-416">Feature</span></span> | <span data-ttu-id="a7f4b-417">Doba trvání</span><span class="sxs-lookup"><span data-stu-id="a7f4b-417">Duration</span></span> |
| --- | --- | --- |
| user@location.com |<span data-ttu-id="a7f4b-418">RightMenu</span><span class="sxs-lookup"><span data-stu-id="a7f4b-418">RightMenu</span></span> |<span data-ttu-id="a7f4b-419">7</span><span class="sxs-lookup"><span data-stu-id="a7f4b-419">7</span></span> |

<span data-ttu-id="a7f4b-420">**Řešení**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-420">**Solution**:</span></span>

````
    SELECT
        [user], feature, DATEDIFF(second, LAST(Time) OVER (PARTITION BY [user], feature LIMIT DURATION(hour, 1) WHEN Event = 'start'), Time) as duration
    FROM input TIMESTAMP BY Time
    WHERE
        Event = 'end'
````

<span data-ttu-id="a7f4b-421">**Vysvětlení**: použití hello **poslední** funkce tooretrieve hello poslední **čas** hodnota při typ události hello **spustit**.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-421">**Explanation**: Use hello **LAST** function tooretrieve hello last **TIME** value when hello event type was **Start**.</span></span> <span data-ttu-id="a7f4b-422">Hello **poslední** využívá **PARTITION BY [user]** tooindicate, který hello výsledek se počítá na jedinečný uživatele.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-422">hello **LAST** function uses **PARTITION BY [user]** tooindicate that hello result is computed per unique user.</span></span> <span data-ttu-id="a7f4b-423">dotaz Hello má maximální prahovou hodnotu 1 hodina hello časový rozdíl mezi **spustit** a **Zastavit** události, ale je možné konfigurovat podle potřeby **(LIMIT DURATION(hour, 1)**.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-423">hello query has a 1-hour maximum threshold for hello time difference between **Start** and **Stop** events, but is configurable as needed **(LIMIT DURATION(hour, 1)**.</span></span>

## <a name="query-example-detect-hello-duration-of-a-condition"></a><span data-ttu-id="a7f4b-424">Příklad dotazu: zjistit dobu trvání hello podmínku</span><span class="sxs-lookup"><span data-stu-id="a7f4b-424">Query example: Detect hello duration of a condition</span></span>
<span data-ttu-id="a7f4b-425">**Popis**: Najít se na jak dlouho podmínku došlo k chybě.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-425">**Description**: Find out how long a condition occurred.</span></span>
<span data-ttu-id="a7f4b-426">Předpokládejme například, že chyby výsledkem všechny aut, které mají nesprávné váhu (nad 20 000 libra).</span><span class="sxs-lookup"><span data-stu-id="a7f4b-426">For example, suppose that a bug resulted in all cars having an incorrect weight (above 20,000 pounds).</span></span> <span data-ttu-id="a7f4b-427">Chceme toocompute hello trvání hello chyb.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-427">We want toocompute hello duration of hello bug.</span></span>

<span data-ttu-id="a7f4b-428">**Vstup**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-428">**Input**:</span></span>

| <span data-ttu-id="a7f4b-429">Ujistěte se</span><span class="sxs-lookup"><span data-stu-id="a7f4b-429">Make</span></span> | <span data-ttu-id="a7f4b-430">Čas</span><span class="sxs-lookup"><span data-stu-id="a7f4b-430">Time</span></span> | <span data-ttu-id="a7f4b-431">Váha</span><span class="sxs-lookup"><span data-stu-id="a7f4b-431">Weight</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a7f4b-432">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-432">Honda</span></span> |<span data-ttu-id="a7f4b-433">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-433">2015-01-01T00:00:01.0000000Z</span></span> |<span data-ttu-id="a7f4b-434">2000</span><span class="sxs-lookup"><span data-stu-id="a7f4b-434">2000</span></span> |
| <span data-ttu-id="a7f4b-435">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-435">Toyota</span></span> |<span data-ttu-id="a7f4b-436">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-436">2015-01-01T00:00:02.0000000Z</span></span> |<span data-ttu-id="a7f4b-437">25000</span><span class="sxs-lookup"><span data-stu-id="a7f4b-437">25000</span></span> |
| <span data-ttu-id="a7f4b-438">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-438">Honda</span></span> |<span data-ttu-id="a7f4b-439">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-439">2015-01-01T00:00:03.0000000Z</span></span> |<span data-ttu-id="a7f4b-440">26000</span><span class="sxs-lookup"><span data-stu-id="a7f4b-440">26000</span></span> |
| <span data-ttu-id="a7f4b-441">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-441">Toyota</span></span> |<span data-ttu-id="a7f4b-442">2015-01-01T00:00:04.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-442">2015-01-01T00:00:04.0000000Z</span></span> |<span data-ttu-id="a7f4b-443">25000</span><span class="sxs-lookup"><span data-stu-id="a7f4b-443">25000</span></span> |
| <span data-ttu-id="a7f4b-444">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-444">Honda</span></span> |<span data-ttu-id="a7f4b-445">2015-01-01T00:00:05.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-445">2015-01-01T00:00:05.0000000Z</span></span> |<span data-ttu-id="a7f4b-446">26000</span><span class="sxs-lookup"><span data-stu-id="a7f4b-446">26000</span></span> |
| <span data-ttu-id="a7f4b-447">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-447">Toyota</span></span> |<span data-ttu-id="a7f4b-448">2015-01-01T00:00:06.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-448">2015-01-01T00:00:06.0000000Z</span></span> |<span data-ttu-id="a7f4b-449">25000</span><span class="sxs-lookup"><span data-stu-id="a7f4b-449">25000</span></span> |
| <span data-ttu-id="a7f4b-450">Honda</span><span class="sxs-lookup"><span data-stu-id="a7f4b-450">Honda</span></span> |<span data-ttu-id="a7f4b-451">2015-01-01T00:00:07.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-451">2015-01-01T00:00:07.0000000Z</span></span> |<span data-ttu-id="a7f4b-452">26000</span><span class="sxs-lookup"><span data-stu-id="a7f4b-452">26000</span></span> |
| <span data-ttu-id="a7f4b-453">Toyota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-453">Toyota</span></span> |<span data-ttu-id="a7f4b-454">2015-01-01T00:00:08.0000000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-454">2015-01-01T00:00:08.0000000Z</span></span> |<span data-ttu-id="a7f4b-455">2000</span><span class="sxs-lookup"><span data-stu-id="a7f4b-455">2000</span></span> |

<span data-ttu-id="a7f4b-456">**Výstup**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-456">**Output**:</span></span>

| <span data-ttu-id="a7f4b-457">StartFault</span><span class="sxs-lookup"><span data-stu-id="a7f4b-457">StartFault</span></span> | <span data-ttu-id="a7f4b-458">EndFault</span><span class="sxs-lookup"><span data-stu-id="a7f4b-458">EndFault</span></span> |
| --- | --- |
| <span data-ttu-id="a7f4b-459">2015-01-01T00:00:02.000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-459">2015-01-01T00:00:02.000Z</span></span> |<span data-ttu-id="a7f4b-460">2015-01-01T00:00:07.000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-460">2015-01-01T00:00:07.000Z</span></span> |

<span data-ttu-id="a7f4b-461">**Řešení**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-461">**Solution**:</span></span>

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

<span data-ttu-id="a7f4b-462">**Vysvětlení**: použití **PRODLEVA** tooview hello vstupního datového proudu pro 24 hodin a podívejte se na instance, kde **StartFault** a **StopFault** jsou předané hello Váha < 20000.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-462">**Explanation**: Use **LAG** tooview hello input stream for 24 hours and look for instances where **StartFault** and **StopFault** are spanned by hello weight < 20000.</span></span>

## <a name="query-example-fill-missing-values"></a><span data-ttu-id="a7f4b-463">Příklad dotazu: vyplnění chybějící hodnoty</span><span class="sxs-lookup"><span data-stu-id="a7f4b-463">Query example: Fill missing values</span></span>
<span data-ttu-id="a7f4b-464">**Popis**: pro hello datový proud události, které mají chybějící hodnoty, vytvořit datový proud událostí s pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-464">**Description**: For hello stream of events that have missing values, produce a stream of events with regular intervals.</span></span>
<span data-ttu-id="a7f4b-465">Například generovat událost každých 5 sekund, který ohlašuje hello naposledy vidět datového bodu.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-465">For example, generate an event every 5 seconds that reports hello most recently seen data point.</span></span>

<span data-ttu-id="a7f4b-466">**Vstup**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-466">**Input**:</span></span>

| <span data-ttu-id="a7f4b-467">T</span><span class="sxs-lookup"><span data-stu-id="a7f4b-467">t</span></span> | <span data-ttu-id="a7f4b-468">hodnota</span><span class="sxs-lookup"><span data-stu-id="a7f4b-468">value</span></span> |
| --- | --- |
| <span data-ttu-id="a7f4b-469">"2014-01-01T06:01:00"</span><span class="sxs-lookup"><span data-stu-id="a7f4b-469">"2014-01-01T06:01:00"</span></span> |<span data-ttu-id="a7f4b-470">1</span><span class="sxs-lookup"><span data-stu-id="a7f4b-470">1</span></span> |
| <span data-ttu-id="a7f4b-471">"2014-01-01T06:01:05"</span><span class="sxs-lookup"><span data-stu-id="a7f4b-471">"2014-01-01T06:01:05"</span></span> |<span data-ttu-id="a7f4b-472">2</span><span class="sxs-lookup"><span data-stu-id="a7f4b-472">2</span></span> |
| <span data-ttu-id="a7f4b-473">"2014-01-01T06:01:10"</span><span class="sxs-lookup"><span data-stu-id="a7f4b-473">"2014-01-01T06:01:10"</span></span> |<span data-ttu-id="a7f4b-474">3</span><span class="sxs-lookup"><span data-stu-id="a7f4b-474">3</span></span> |
| <span data-ttu-id="a7f4b-475">"2014-01-01T06:01:15"</span><span class="sxs-lookup"><span data-stu-id="a7f4b-475">"2014-01-01T06:01:15"</span></span> |<span data-ttu-id="a7f4b-476">4</span><span class="sxs-lookup"><span data-stu-id="a7f4b-476">4</span></span> |
| <span data-ttu-id="a7f4b-477">"2014-01-01T06:01:30"</span><span class="sxs-lookup"><span data-stu-id="a7f4b-477">"2014-01-01T06:01:30"</span></span> |<span data-ttu-id="a7f4b-478">5</span><span class="sxs-lookup"><span data-stu-id="a7f4b-478">5</span></span> |
| <span data-ttu-id="a7f4b-479">"2014-01-01T06:01:35"</span><span class="sxs-lookup"><span data-stu-id="a7f4b-479">"2014-01-01T06:01:35"</span></span> |<span data-ttu-id="a7f4b-480">6</span><span class="sxs-lookup"><span data-stu-id="a7f4b-480">6</span></span> |

<span data-ttu-id="a7f4b-481">**Výstup (prvních 10 řádků)**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-481">**Output (first 10 rows)**:</span></span>

| <span data-ttu-id="a7f4b-482">windowend</span><span class="sxs-lookup"><span data-stu-id="a7f4b-482">windowend</span></span> | <span data-ttu-id="a7f4b-483">lastevent.t</span><span class="sxs-lookup"><span data-stu-id="a7f4b-483">lastevent.t</span></span> | <span data-ttu-id="a7f4b-484">lastevent.Value</span><span class="sxs-lookup"><span data-stu-id="a7f4b-484">lastevent.value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a7f4b-485">2014-01-01T14:01:00.000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-485">2014-01-01T14:01:00.000Z</span></span> |<span data-ttu-id="a7f4b-486">2014-01-01T14:01:00.000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-486">2014-01-01T14:01:00.000Z</span></span> |<span data-ttu-id="a7f4b-487">1</span><span class="sxs-lookup"><span data-stu-id="a7f4b-487">1</span></span> |
| <span data-ttu-id="a7f4b-488">2014-01-01T14:01:05.000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-488">2014-01-01T14:01:05.000Z</span></span> |<span data-ttu-id="a7f4b-489">2014-01-01T14:01:05.000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-489">2014-01-01T14:01:05.000Z</span></span> |<span data-ttu-id="a7f4b-490">2</span><span class="sxs-lookup"><span data-stu-id="a7f4b-490">2</span></span> |
| <span data-ttu-id="a7f4b-491">2014-01-01T14:01:10.000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-491">2014-01-01T14:01:10.000Z</span></span> |<span data-ttu-id="a7f4b-492">2014-01-01T14:01:10.000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-492">2014-01-01T14:01:10.000Z</span></span> |<span data-ttu-id="a7f4b-493">3</span><span class="sxs-lookup"><span data-stu-id="a7f4b-493">3</span></span> |
| <span data-ttu-id="a7f4b-494">2014-01-01T14:01:15.000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-494">2014-01-01T14:01:15.000Z</span></span> |<span data-ttu-id="a7f4b-495">2014-01-01T14:01:15.000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-495">2014-01-01T14:01:15.000Z</span></span> |<span data-ttu-id="a7f4b-496">4</span><span class="sxs-lookup"><span data-stu-id="a7f4b-496">4</span></span> |
| <span data-ttu-id="a7f4b-497">2014-01-01T14:01:20.000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-497">2014-01-01T14:01:20.000Z</span></span> |<span data-ttu-id="a7f4b-498">2014-01-01T14:01:15.000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-498">2014-01-01T14:01:15.000Z</span></span> |<span data-ttu-id="a7f4b-499">4</span><span class="sxs-lookup"><span data-stu-id="a7f4b-499">4</span></span> |
| <span data-ttu-id="a7f4b-500">2014-01-01T14:01:25.000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-500">2014-01-01T14:01:25.000Z</span></span> |<span data-ttu-id="a7f4b-501">2014-01-01T14:01:15.000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-501">2014-01-01T14:01:15.000Z</span></span> |<span data-ttu-id="a7f4b-502">4</span><span class="sxs-lookup"><span data-stu-id="a7f4b-502">4</span></span> |
| <span data-ttu-id="a7f4b-503">2014-01-01T14:01:30.000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-503">2014-01-01T14:01:30.000Z</span></span> |<span data-ttu-id="a7f4b-504">2014-01-01T14:01:30.000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-504">2014-01-01T14:01:30.000Z</span></span> |<span data-ttu-id="a7f4b-505">5</span><span class="sxs-lookup"><span data-stu-id="a7f4b-505">5</span></span> |
| <span data-ttu-id="a7f4b-506">2014-01-01T14:01:35.000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-506">2014-01-01T14:01:35.000Z</span></span> |<span data-ttu-id="a7f4b-507">2014-01-01T14:01:35.000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-507">2014-01-01T14:01:35.000Z</span></span> |<span data-ttu-id="a7f4b-508">6</span><span class="sxs-lookup"><span data-stu-id="a7f4b-508">6</span></span> |
| <span data-ttu-id="a7f4b-509">2014-01-01T14:01:40.000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-509">2014-01-01T14:01:40.000Z</span></span> |<span data-ttu-id="a7f4b-510">2014-01-01T14:01:35.000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-510">2014-01-01T14:01:35.000Z</span></span> |<span data-ttu-id="a7f4b-511">6</span><span class="sxs-lookup"><span data-stu-id="a7f4b-511">6</span></span> |
| <span data-ttu-id="a7f4b-512">2014-01-01T14:01:45.000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-512">2014-01-01T14:01:45.000Z</span></span> |<span data-ttu-id="a7f4b-513">2014-01-01T14:01:35.000Z</span><span class="sxs-lookup"><span data-stu-id="a7f4b-513">2014-01-01T14:01:35.000Z</span></span> |<span data-ttu-id="a7f4b-514">6</span><span class="sxs-lookup"><span data-stu-id="a7f4b-514">6</span></span> |

<span data-ttu-id="a7f4b-515">**Řešení**:</span><span class="sxs-lookup"><span data-stu-id="a7f4b-515">**Solution**:</span></span>

    SELECT
        System.Timestamp AS windowEnd,
        TopOne() OVER (ORDER BY t DESC) AS lastEvent
    FROM
        input TIMESTAMP BY t
    GROUP BY HOPPINGWINDOW(second, 300, 5)


<span data-ttu-id="a7f4b-516">**Vysvětlení**: Tento dotaz vygeneruje události každých 5 sekund a výstupy hello poslední událost, který jste získali dříve.</span><span class="sxs-lookup"><span data-stu-id="a7f4b-516">**Explanation**: This query generates events every 5 seconds and outputs hello last event that was received previously.</span></span> <span data-ttu-id="a7f4b-517">Hello [Hopping okno](https://msdn.microsoft.com/library/dn835041.aspx "Hopping okno – Azure Stream Analytics") doba trvání Určuje, jak daleko back hello dotazu vypadá toofind hello nejnovější událost (v tomto příkladu je 300 sekund).</span><span class="sxs-lookup"><span data-stu-id="a7f4b-517">hello [Hopping window](https://msdn.microsoft.com/library/dn835041.aspx "Hopping window--Azure Stream Analytics") duration determines how far back hello query looks toofind hello latest event (300 seconds in this example).</span></span>

## <a name="get-help"></a><span data-ttu-id="a7f4b-518">Podpora</span><span class="sxs-lookup"><span data-stu-id="a7f4b-518">Get help</span></span>
<span data-ttu-id="a7f4b-519">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="a7f4b-519">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a7f4b-520">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a7f4b-520">Next steps</span></span>
* [<span data-ttu-id="a7f4b-521">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="a7f4b-521">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="a7f4b-522">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="a7f4b-522">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="a7f4b-523">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="a7f4b-523">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="a7f4b-524">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="a7f4b-524">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="a7f4b-525">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="a7f4b-525">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

