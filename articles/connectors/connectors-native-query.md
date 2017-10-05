---
title: "Přidání akce dotazu v aplikace logiky | Microsoft Docs"
description: "Přehled akce dotazu pro provádění akcí, jako je pole filtru."
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 34e702c7-f9e5-4885-9266-fc7404adecfe
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/20/2016
ms.author: jehollan
ms.openlocfilehash: a992fa17a07d6167297c4cf5fe9fb3b58181d7df
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-query-action"></a><span data-ttu-id="57b94-103">Začínáme s akce dotazu</span><span class="sxs-lookup"><span data-stu-id="57b94-103">Get started with the query action</span></span>
<span data-ttu-id="57b94-104">Pomocí akce dotazu, můžete pracovat se dávek a aby provést pracovní postupy pro pole:</span><span class="sxs-lookup"><span data-stu-id="57b94-104">By using the query action, you can work with batches and arrays to accomplish workflows to:</span></span>

* <span data-ttu-id="57b94-105">Vytvořte úlohu pro všechny záznamy s vysokou prioritou z databáze.</span><span class="sxs-lookup"><span data-stu-id="57b94-105">Create a task for all high-priority records from a database.</span></span>
* <span data-ttu-id="57b94-106">Uložte všechny přílohy PDF pro e-mailů do objektu blob Azure.</span><span class="sxs-lookup"><span data-stu-id="57b94-106">Save all PDF attachments for emails into an Azure blob.</span></span>

<span data-ttu-id="57b94-107">Chcete-li začít používat akce dotazu v aplikaci logiky, přečtěte si téma [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="57b94-107">To get started using the query action in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-the-query-action"></a><span data-ttu-id="57b94-108">Pomocí akce dotazu</span><span class="sxs-lookup"><span data-stu-id="57b94-108">Use the query action</span></span>
<span data-ttu-id="57b94-109">Akce je operace, která se provádí v pracovním postupu, který je definován v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="57b94-109">An action is an operation that is carried out by the workflow that is defined in a logic app.</span></span> <span data-ttu-id="57b94-110">[Další informace o akcích](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="57b94-110">[Learn more about actions](connectors-overview.md).</span></span>  

<span data-ttu-id="57b94-111">Akce dotazu má aktuálně jednu operaci, názvem pole filtru, který je zveřejněný v návrháři.</span><span class="sxs-lookup"><span data-stu-id="57b94-111">The query action currently has one operation, called the filter array, that is exposed in the designer.</span></span> <span data-ttu-id="57b94-112">To umožňuje dotazování pole a vrátit sadu filtrované výsledky.</span><span class="sxs-lookup"><span data-stu-id="57b94-112">This allows you to query an array and return a set of filtered results.</span></span>

<span data-ttu-id="57b94-113">Zde je, jak můžete přidat logiku aplikace:</span><span class="sxs-lookup"><span data-stu-id="57b94-113">Here's how you can add it in a logic app:</span></span>

1. <span data-ttu-id="57b94-114">Vyberte **nový krok** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="57b94-114">Select the **New Step** button.</span></span>
2. <span data-ttu-id="57b94-115">Zvolte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="57b94-115">Choose **Add an action**.</span></span>
3. <span data-ttu-id="57b94-116">Zadejte do vyhledávacího pole Akce **filtru** do seznamu **pole filtru** akce.</span><span class="sxs-lookup"><span data-stu-id="57b94-116">In the action search box, type **filter** to list the **Filter array** action.</span></span>
   
    ![Vyberte akci, která dotazu](./media/connectors-native-query/using-action-1.png)
4. <span data-ttu-id="57b94-118">Vyberte pole filtrovat.</span><span class="sxs-lookup"><span data-stu-id="57b94-118">Select an array to filter.</span></span> <span data-ttu-id="57b94-119">(Následující snímek obrazovky ukazuje pole výsledky vyhledávání na Twitteru.)</span><span class="sxs-lookup"><span data-stu-id="57b94-119">(The following screenshot shows the array of results from a Twitter search.)</span></span>
5. <span data-ttu-id="57b94-120">Vytvořte podmínku, kterou chcete vyhodnotit na každou položku.</span><span class="sxs-lookup"><span data-stu-id="57b94-120">Create a condition to evaluate on each item.</span></span> <span data-ttu-id="57b94-121">(Následující snímek obrazovky filtruje tweetů od uživatelů, kteří mají více než 100 délky.)</span><span class="sxs-lookup"><span data-stu-id="57b94-121">(The following screenshot filters tweets from users who have more than 100 followers.)</span></span>
   
    ![Dokončení akce dotazu](./media/connectors-native-query/using-action-2.png)
   
    <span data-ttu-id="57b94-123">Akce bude výstup nové pole, které obsahuje pouze výsledky, které splnění požadavků filtru.</span><span class="sxs-lookup"><span data-stu-id="57b94-123">The action will output a new array that contains only results that met the filter requirements.</span></span>
6. <span data-ttu-id="57b94-124">Klikněte levém horním panelu nástrojů na Uložit a aplikace logiky jak uložíte a publikovat (aktivovat).</span><span class="sxs-lookup"><span data-stu-id="57b94-124">Click the upper-left corner of the toolbar to save, and your logic app will both save and publish (activate).</span></span>

## <a name="query-action"></a><span data-ttu-id="57b94-125">Akce dotazu</span><span class="sxs-lookup"><span data-stu-id="57b94-125">Query action</span></span>
<span data-ttu-id="57b94-126">Tady jsou uvedené podrobnosti pro akci, která tento konektor podporuje.</span><span class="sxs-lookup"><span data-stu-id="57b94-126">Here are the details for the action that this connector supports.</span></span> <span data-ttu-id="57b94-127">Konektor je možné jednu akci.</span><span class="sxs-lookup"><span data-stu-id="57b94-127">The connector has one possible action.</span></span>

| <span data-ttu-id="57b94-128">Akce</span><span class="sxs-lookup"><span data-stu-id="57b94-128">Action</span></span> | <span data-ttu-id="57b94-129">Popis</span><span class="sxs-lookup"><span data-stu-id="57b94-129">Description</span></span> |
| --- | --- |
| <span data-ttu-id="57b94-130">Pole filtru</span><span class="sxs-lookup"><span data-stu-id="57b94-130">Filter array</span></span> |<span data-ttu-id="57b94-131">Vyhodnocuje podmínku pro každou položku v pole a vrátí výsledky</span><span class="sxs-lookup"><span data-stu-id="57b94-131">Evaluates a condition for each item in an array and returns the results</span></span> |

## <a name="action-details"></a><span data-ttu-id="57b94-132">Podrobnosti akce</span><span class="sxs-lookup"><span data-stu-id="57b94-132">Action details</span></span>
<span data-ttu-id="57b94-133">Akce dotazu obsahuje jednu možné akci.</span><span class="sxs-lookup"><span data-stu-id="57b94-133">The query action comes with one possible action.</span></span> <span data-ttu-id="57b94-134">Následující tabulky popisují požadované a volitelné vstupních polí pro akce a odpovídající výstup podrobnosti, které jsou spojené s použitím akce.</span><span class="sxs-lookup"><span data-stu-id="57b94-134">The following tables describe the required and optional input fields for the action and the corresponding output details that are associated with using the action.</span></span>

### <a name="filter-array"></a><span data-ttu-id="57b94-135">Pole filtru</span><span class="sxs-lookup"><span data-stu-id="57b94-135">Filter array</span></span>
<span data-ttu-id="57b94-136">Níže jsou uvedeny vstupních polí pro akce, která umožňuje odchozí požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="57b94-136">The following are input fields for the action, which makes an HTTP outbound request.</span></span>
<span data-ttu-id="57b94-137">A * znamená, že je povinné pole.</span><span class="sxs-lookup"><span data-stu-id="57b94-137">A * means that it is a required field.</span></span>

| <span data-ttu-id="57b94-138">Zobrazované jméno</span><span class="sxs-lookup"><span data-stu-id="57b94-138">Display name</span></span> | <span data-ttu-id="57b94-139">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="57b94-139">Property name</span></span> | <span data-ttu-id="57b94-140">Popis</span><span class="sxs-lookup"><span data-stu-id="57b94-140">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="57b94-141">Z *</span><span class="sxs-lookup"><span data-stu-id="57b94-141">From*</span></span> |<span data-ttu-id="57b94-142">Z</span><span class="sxs-lookup"><span data-stu-id="57b94-142">from</span></span> |<span data-ttu-id="57b94-143">Pole pro filtrování</span><span class="sxs-lookup"><span data-stu-id="57b94-143">The array to filter</span></span> |
| <span data-ttu-id="57b94-144">Podmínka *</span><span class="sxs-lookup"><span data-stu-id="57b94-144">Condition*</span></span> |<span data-ttu-id="57b94-145">kde</span><span class="sxs-lookup"><span data-stu-id="57b94-145">where</span></span> |<span data-ttu-id="57b94-146">Podmínku, která má vyhodnotit pro každou položku</span><span class="sxs-lookup"><span data-stu-id="57b94-146">The condition to evaluate for each item</span></span> |

<br>

### <a name="output-details"></a><span data-ttu-id="57b94-147">Podrobnosti o výstupu</span><span class="sxs-lookup"><span data-stu-id="57b94-147">Output details</span></span>
<span data-ttu-id="57b94-148">Níže jsou uvedeny podrobnosti výstup pro odpověď HTTP.</span><span class="sxs-lookup"><span data-stu-id="57b94-148">The following are output details for the HTTP response.</span></span>

| <span data-ttu-id="57b94-149">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="57b94-149">Property name</span></span> | <span data-ttu-id="57b94-150">Datový typ</span><span class="sxs-lookup"><span data-stu-id="57b94-150">Data type</span></span> | <span data-ttu-id="57b94-151">Popis</span><span class="sxs-lookup"><span data-stu-id="57b94-151">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="57b94-152">Filtrované pole</span><span class="sxs-lookup"><span data-stu-id="57b94-152">Filtered array</span></span> |<span data-ttu-id="57b94-153">Pole</span><span class="sxs-lookup"><span data-stu-id="57b94-153">array</span></span> |<span data-ttu-id="57b94-154">Pole, které obsahuje objekt pro každý filtrované výsledek</span><span class="sxs-lookup"><span data-stu-id="57b94-154">An array that contains an object for each filtered result</span></span> |

## <a name="next-steps"></a><span data-ttu-id="57b94-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="57b94-155">Next steps</span></span>
<span data-ttu-id="57b94-156">Teď vyzkoušet platformu a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="57b94-156">Now, try out the platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="57b94-157">Ostatní konektory k dispozici v aplikace logiky můžete prozkoumat pohledem na našem [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="57b94-157">You can explore the other available connectors in logic apps by looking at our [APIs list](apis-list.md).</span></span>

