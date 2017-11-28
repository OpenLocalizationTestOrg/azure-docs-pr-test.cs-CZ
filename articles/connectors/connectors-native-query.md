---
title: aaaAdd hello dotazu akce v aplikace logiky | Microsoft Docs
description: "Přehled hello dotazu akce pro provádění akcí, jako je pole filtru."
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
ms.openlocfilehash: 3d4be901e7e6bf1b644057648930667ab34f2124
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-query-action"></a><span data-ttu-id="1e2eb-103">Začínáme s hello dotazu akce</span><span class="sxs-lookup"><span data-stu-id="1e2eb-103">Get started with hello query action</span></span>
<span data-ttu-id="1e2eb-104">Pomocí akce dotazu hello můžete pracovat s dávky a pole tooaccomplish pracovní postupy:</span><span class="sxs-lookup"><span data-stu-id="1e2eb-104">By using hello query action, you can work with batches and arrays tooaccomplish workflows to:</span></span>

* <span data-ttu-id="1e2eb-105">Vytvořte úlohu pro všechny záznamy s vysokou prioritou z databáze.</span><span class="sxs-lookup"><span data-stu-id="1e2eb-105">Create a task for all high-priority records from a database.</span></span>
* <span data-ttu-id="1e2eb-106">Uložte všechny přílohy PDF pro e-mailů do objektu blob Azure.</span><span class="sxs-lookup"><span data-stu-id="1e2eb-106">Save all PDF attachments for emails into an Azure blob.</span></span>

<span data-ttu-id="1e2eb-107">tooget spuštění pomocí akce hello dotazu v aplikaci logiky, aplikaci najdete v části [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="1e2eb-107">tooget started using hello query action in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-hello-query-action"></a><span data-ttu-id="1e2eb-108">Pomocí akce dotazu hello</span><span class="sxs-lookup"><span data-stu-id="1e2eb-108">Use hello query action</span></span>
<span data-ttu-id="1e2eb-109">Akce je operace, která se provádí v pracovním postupu hello, která je definována v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="1e2eb-109">An action is an operation that is carried out by hello workflow that is defined in a logic app.</span></span> <span data-ttu-id="1e2eb-110">[Další informace o akcích](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1e2eb-110">[Learn more about actions](connectors-overview.md).</span></span>  

<span data-ttu-id="1e2eb-111">Akce dotazu Hello aktuálně používá jednu operaci, názvem hello pole filtru, který je zveřejněný v Návrháři hello.</span><span class="sxs-lookup"><span data-stu-id="1e2eb-111">hello query action currently has one operation, called hello filter array, that is exposed in hello designer.</span></span> <span data-ttu-id="1e2eb-112">To vám umožní tooquery pole a vrátí sadu filtrované výsledky.</span><span class="sxs-lookup"><span data-stu-id="1e2eb-112">This allows you tooquery an array and return a set of filtered results.</span></span>

<span data-ttu-id="1e2eb-113">Zde je, jak můžete přidat logiku aplikace:</span><span class="sxs-lookup"><span data-stu-id="1e2eb-113">Here's how you can add it in a logic app:</span></span>

1. <span data-ttu-id="1e2eb-114">Vyberte hello **nový krok** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="1e2eb-114">Select hello **New Step** button.</span></span>
2. <span data-ttu-id="1e2eb-115">Zvolte **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="1e2eb-115">Choose **Add an action**.</span></span>
3. <span data-ttu-id="1e2eb-116">Hello akce vyhledávacího pole zadejte **filtru** toolist hello **pole filtru** akce.</span><span class="sxs-lookup"><span data-stu-id="1e2eb-116">In hello action search box, type **filter** toolist hello **Filter array** action.</span></span>
   
    ![Vyberte akci dotazu hello](./media/connectors-native-query/using-action-1.png)
4. <span data-ttu-id="1e2eb-118">Vyberte toofilter pole.</span><span class="sxs-lookup"><span data-stu-id="1e2eb-118">Select an array toofilter.</span></span> <span data-ttu-id="1e2eb-119">(hello následující snímek obrazovky ukazuje hello pole výsledky vyhledávání na Twitteru.)</span><span class="sxs-lookup"><span data-stu-id="1e2eb-119">(hello following screenshot shows hello array of results from a Twitter search.)</span></span>
5. <span data-ttu-id="1e2eb-120">Vytvořte podmínky tooevaluate pro každou položku.</span><span class="sxs-lookup"><span data-stu-id="1e2eb-120">Create a condition tooevaluate on each item.</span></span> <span data-ttu-id="1e2eb-121">(hello následující snímek obrazovky filtruje tweetů od uživatelů, kteří mají více než 100 délky.)</span><span class="sxs-lookup"><span data-stu-id="1e2eb-121">(hello following screenshot filters tweets from users who have more than 100 followers.)</span></span>
   
    ![Dokončení hello dotazu akce](./media/connectors-native-query/using-action-2.png)
   
    <span data-ttu-id="1e2eb-123">Akce Hello výstup nové pole, která obsahuje jenom výsledky, které splnit požadavky filtru hello.</span><span class="sxs-lookup"><span data-stu-id="1e2eb-123">hello action will output a new array that contains only results that met hello filter requirements.</span></span>
6. <span data-ttu-id="1e2eb-124">Klikněte na tlačítko hello levého horního rohu toosave hello panelu nástrojů a vaše logiku aplikace uloží obou a publikování (aktivovat).</span><span class="sxs-lookup"><span data-stu-id="1e2eb-124">Click hello upper-left corner of hello toolbar toosave, and your logic app will both save and publish (activate).</span></span>

## <a name="query-action"></a><span data-ttu-id="1e2eb-125">Akce dotazu</span><span class="sxs-lookup"><span data-stu-id="1e2eb-125">Query action</span></span>
<span data-ttu-id="1e2eb-126">Tady jsou hello podrobnosti hello akce, který podporuje tento konektor.</span><span class="sxs-lookup"><span data-stu-id="1e2eb-126">Here are hello details for hello action that this connector supports.</span></span> <span data-ttu-id="1e2eb-127">konektor Hello má jednu možné akci.</span><span class="sxs-lookup"><span data-stu-id="1e2eb-127">hello connector has one possible action.</span></span>

| <span data-ttu-id="1e2eb-128">Akce</span><span class="sxs-lookup"><span data-stu-id="1e2eb-128">Action</span></span> | <span data-ttu-id="1e2eb-129">Popis</span><span class="sxs-lookup"><span data-stu-id="1e2eb-129">Description</span></span> |
| --- | --- |
| <span data-ttu-id="1e2eb-130">Pole filtru</span><span class="sxs-lookup"><span data-stu-id="1e2eb-130">Filter array</span></span> |<span data-ttu-id="1e2eb-131">Vyhodnocuje podmínku pro každou položku v pole a vrátí výsledky hello</span><span class="sxs-lookup"><span data-stu-id="1e2eb-131">Evaluates a condition for each item in an array and returns hello results</span></span> |

## <a name="action-details"></a><span data-ttu-id="1e2eb-132">Podrobnosti akce</span><span class="sxs-lookup"><span data-stu-id="1e2eb-132">Action details</span></span>
<span data-ttu-id="1e2eb-133">Akce dotazu Hello se dodává s možné jednu akci.</span><span class="sxs-lookup"><span data-stu-id="1e2eb-133">hello query action comes with one possible action.</span></span> <span data-ttu-id="1e2eb-134">Hello následující tabulky popisují hello požadované a volitelné vstupních polí pro akce hello a hello odpovídající výstup podrobností, které jsou spojené s použitím akce hello.</span><span class="sxs-lookup"><span data-stu-id="1e2eb-134">hello following tables describe hello required and optional input fields for hello action and hello corresponding output details that are associated with using hello action.</span></span>

### <a name="filter-array"></a><span data-ttu-id="1e2eb-135">Pole filtru</span><span class="sxs-lookup"><span data-stu-id="1e2eb-135">Filter array</span></span>
<span data-ttu-id="1e2eb-136">Hello následují vstupní pole pro hello akce, která umožňuje odchozí požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="1e2eb-136">hello following are input fields for hello action, which makes an HTTP outbound request.</span></span>
<span data-ttu-id="1e2eb-137">A * znamená, že je povinné pole.</span><span class="sxs-lookup"><span data-stu-id="1e2eb-137">A * means that it is a required field.</span></span>

| <span data-ttu-id="1e2eb-138">Zobrazované jméno</span><span class="sxs-lookup"><span data-stu-id="1e2eb-138">Display name</span></span> | <span data-ttu-id="1e2eb-139">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="1e2eb-139">Property name</span></span> | <span data-ttu-id="1e2eb-140">Popis</span><span class="sxs-lookup"><span data-stu-id="1e2eb-140">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1e2eb-141">Z *</span><span class="sxs-lookup"><span data-stu-id="1e2eb-141">From*</span></span> |<span data-ttu-id="1e2eb-142">Z</span><span class="sxs-lookup"><span data-stu-id="1e2eb-142">from</span></span> |<span data-ttu-id="1e2eb-143">pole toofilter Hello</span><span class="sxs-lookup"><span data-stu-id="1e2eb-143">hello array toofilter</span></span> |
| <span data-ttu-id="1e2eb-144">Podmínka *</span><span class="sxs-lookup"><span data-stu-id="1e2eb-144">Condition*</span></span> |<span data-ttu-id="1e2eb-145">kde</span><span class="sxs-lookup"><span data-stu-id="1e2eb-145">where</span></span> |<span data-ttu-id="1e2eb-146">Hello tooevaluate podmínku pro každou položku</span><span class="sxs-lookup"><span data-stu-id="1e2eb-146">hello condition tooevaluate for each item</span></span> |

<br>

### <a name="output-details"></a><span data-ttu-id="1e2eb-147">Podrobnosti o výstupu</span><span class="sxs-lookup"><span data-stu-id="1e2eb-147">Output details</span></span>
<span data-ttu-id="1e2eb-148">Hello následují výstup podrobnosti hello odpověď HTTP.</span><span class="sxs-lookup"><span data-stu-id="1e2eb-148">hello following are output details for hello HTTP response.</span></span>

| <span data-ttu-id="1e2eb-149">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="1e2eb-149">Property name</span></span> | <span data-ttu-id="1e2eb-150">Datový typ</span><span class="sxs-lookup"><span data-stu-id="1e2eb-150">Data type</span></span> | <span data-ttu-id="1e2eb-151">Popis</span><span class="sxs-lookup"><span data-stu-id="1e2eb-151">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1e2eb-152">Filtrované pole</span><span class="sxs-lookup"><span data-stu-id="1e2eb-152">Filtered array</span></span> |<span data-ttu-id="1e2eb-153">Pole</span><span class="sxs-lookup"><span data-stu-id="1e2eb-153">array</span></span> |<span data-ttu-id="1e2eb-154">Pole, které obsahuje objekt pro každý filtrované výsledek</span><span class="sxs-lookup"><span data-stu-id="1e2eb-154">An array that contains an object for each filtered result</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1e2eb-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1e2eb-155">Next steps</span></span>
<span data-ttu-id="1e2eb-156">Teď vyzkoušet hello platformy a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="1e2eb-156">Now, try out hello platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="1e2eb-157">Můžete prozkoumat hello dalších dostupných konektorů v aplikacích logiky pohledem na našem [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="1e2eb-157">You can explore hello other available connectors in logic apps by looking at our [APIs list](apis-list.md).</span></span>

