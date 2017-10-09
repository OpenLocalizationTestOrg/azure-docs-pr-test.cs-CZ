---
title: "aaaAdd zpoždění při aplikace logiky | Microsoft Docs"
description: "Přehled hello zpoždění a prodlevu – dokud akce a jak toouse je s Azure logiku aplikace."
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 915f48bf-3bd8-4656-be73-91a941d0afcd
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: e5bc9d639adbddc01ee0f6a4c68716f586d4344a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-delay-and-delay-until-actions"></a><span data-ttu-id="e0e37-103">Začínáme s hello zpoždění a prodlevu – dokud akce</span><span class="sxs-lookup"><span data-stu-id="e0e37-103">Get started with hello delay and delay-until actions</span></span>
<span data-ttu-id="e0e37-104">Pomocí hello zpoždění a "zpoždění – dokud" akce, můžete dokončit scénáře pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="e0e37-104">By using hello delay and "delay-until" actions, you can complete workflow scenarios.</span></span>

<span data-ttu-id="e0e37-105">Můžete například provést následující věci:</span><span class="sxs-lookup"><span data-stu-id="e0e37-105">For example, you can:</span></span>

* <span data-ttu-id="e0e37-106">Počkejte, dokud toosend den v týdnu stav aktualizace prostřednictvím e-mailu.</span><span class="sxs-lookup"><span data-stu-id="e0e37-106">Wait until a weekday toosend a status update over email.</span></span>
* <span data-ttu-id="e0e37-107">Pracovní postup hello zpoždění až volání protokolu HTTP má čas toofinish před obnovení a načítání hello výsledek.</span><span class="sxs-lookup"><span data-stu-id="e0e37-107">Delay hello workflow until an HTTP call has time toofinish before resuming and retrieving hello result.</span></span>

<span data-ttu-id="e0e37-108">tooget spuštění pomocí hello zpoždění akce v aplikaci logiky, aplikaci najdete v části [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="e0e37-108">tooget started using hello delay action in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-hello-delay-actions"></a><span data-ttu-id="e0e37-109">Pomocí akcí zpoždění hello</span><span class="sxs-lookup"><span data-stu-id="e0e37-109">Use hello delay actions</span></span>
<span data-ttu-id="e0e37-110">Akce je operace, která se provádí v pracovním postupu hello, která je definována v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="e0e37-110">An action is an operation that is carried out by hello workflow that is defined in a logic app.</span></span> <span data-ttu-id="e0e37-111">[Další informace o akcích](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e0e37-111">[Learn more about actions](connectors-overview.md).</span></span>

<span data-ttu-id="e0e37-112">Tady je příklad posloupnosti jak toouse zpoždění krok v aplikaci logiky:</span><span class="sxs-lookup"><span data-stu-id="e0e37-112">Here’s an example sequence of how toouse a delay step in a logic app:</span></span>

1. <span data-ttu-id="e0e37-113">Po přidání aktivační událost, klikněte na tlačítko **nový krok** tooadd akce.</span><span class="sxs-lookup"><span data-stu-id="e0e37-113">After adding a trigger, click **New Step** tooadd an action.</span></span>
2. <span data-ttu-id="e0e37-114">Vyhledejte **zpoždění** toobring až hello zpoždění akce.</span><span class="sxs-lookup"><span data-stu-id="e0e37-114">Search for **delay** toobring up hello delay actions.</span></span> <span data-ttu-id="e0e37-115">V tomto příkladu jsme vyberte **zpoždění**.</span><span class="sxs-lookup"><span data-stu-id="e0e37-115">In this example, we will select **Delay**.</span></span>
   
    ![Zpoždění akce](./media/connectors-native-delay/using-action-1.png)
3. <span data-ttu-id="e0e37-117">Dokončete všechny hello akce vlastnosti tooconfigure hello zpoždění.</span><span class="sxs-lookup"><span data-stu-id="e0e37-117">Complete any of hello action properties tooconfigure hello delay.</span></span>
   
    ![Konfigurace zpoždění](./media/connectors-native-delay/using-action-2.png)
4. <span data-ttu-id="e0e37-119">Klikněte na tlačítko **Uložit** toopublish a aktivovat aplikaci logiky hello.</span><span class="sxs-lookup"><span data-stu-id="e0e37-119">Click **Save** toopublish and activate hello logic app.</span></span>

## <a name="action-details"></a><span data-ttu-id="e0e37-120">Podrobnosti akce</span><span class="sxs-lookup"><span data-stu-id="e0e37-120">Action details</span></span>
<span data-ttu-id="e0e37-121">aktivační událost Hello opakování má následující vlastnosti, které lze nakonfigurovat hello.</span><span class="sxs-lookup"><span data-stu-id="e0e37-121">hello recurrence trigger has hello following properties that can be configured.</span></span>

### <a name="delay-action"></a><span data-ttu-id="e0e37-122">Zpoždění akce</span><span class="sxs-lookup"><span data-stu-id="e0e37-122">Delay action</span></span>
<span data-ttu-id="e0e37-123">Tato akce hello zpoždění spuštění pro určitý časový interval.</span><span class="sxs-lookup"><span data-stu-id="e0e37-123">This action delays hello run for a certain time interval.</span></span>
<span data-ttu-id="e0e37-124">A * znamená, že je povinné pole.</span><span class="sxs-lookup"><span data-stu-id="e0e37-124">A * means that it is a required field.</span></span>

| <span data-ttu-id="e0e37-125">Zobrazované jméno</span><span class="sxs-lookup"><span data-stu-id="e0e37-125">Display name</span></span> | <span data-ttu-id="e0e37-126">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="e0e37-126">Property name</span></span> | <span data-ttu-id="e0e37-127">Popis</span><span class="sxs-lookup"><span data-stu-id="e0e37-127">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e0e37-128">Počet *</span><span class="sxs-lookup"><span data-stu-id="e0e37-128">Count*</span></span> |<span data-ttu-id="e0e37-129">Počet</span><span class="sxs-lookup"><span data-stu-id="e0e37-129">count</span></span> |<span data-ttu-id="e0e37-130">Hello počet jednotek toodelay čas</span><span class="sxs-lookup"><span data-stu-id="e0e37-130">hello number of time units toodelay</span></span> |
| <span data-ttu-id="e0e37-131">Jednotka *</span><span class="sxs-lookup"><span data-stu-id="e0e37-131">Unit*</span></span> |<span data-ttu-id="e0e37-132">jednotka</span><span class="sxs-lookup"><span data-stu-id="e0e37-132">unit</span></span> |<span data-ttu-id="e0e37-133">jednotka času Hello: `Second`, `Minute`, `Hour`, nebo`Day`</span><span class="sxs-lookup"><span data-stu-id="e0e37-133">hello unit of time: `Second`, `Minute`, `Hour`, or `Day`</span></span> |

<br>

### <a name="delay-until-action"></a><span data-ttu-id="e0e37-134">Zpoždění – dokud akce</span><span class="sxs-lookup"><span data-stu-id="e0e37-134">Delay-until action</span></span>
<span data-ttu-id="e0e37-135">Tato akce zpozdí hello běžet, dokud zadané datum a čas.</span><span class="sxs-lookup"><span data-stu-id="e0e37-135">This action delays hello run until a specified date/time.</span></span>
<span data-ttu-id="e0e37-136">A * znamená, že je povinné pole.</span><span class="sxs-lookup"><span data-stu-id="e0e37-136">A * means that it is a required field.</span></span>

| <span data-ttu-id="e0e37-137">Zobrazované jméno</span><span class="sxs-lookup"><span data-stu-id="e0e37-137">Display name</span></span> | <span data-ttu-id="e0e37-138">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="e0e37-138">Property name</span></span> | <span data-ttu-id="e0e37-139">Popis</span><span class="sxs-lookup"><span data-stu-id="e0e37-139">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e0e37-140">Rok *</span><span class="sxs-lookup"><span data-stu-id="e0e37-140">Year*</span></span> |<span data-ttu-id="e0e37-141">časové razítko</span><span class="sxs-lookup"><span data-stu-id="e0e37-141">timestamp</span></span> |<span data-ttu-id="e0e37-142">Hello roku toodelay dokud (GMT)</span><span class="sxs-lookup"><span data-stu-id="e0e37-142">hello year toodelay until (GMT)</span></span> |
| <span data-ttu-id="e0e37-143">Měsíc *</span><span class="sxs-lookup"><span data-stu-id="e0e37-143">Month*</span></span> |<span data-ttu-id="e0e37-144">časové razítko</span><span class="sxs-lookup"><span data-stu-id="e0e37-144">timestamp</span></span> |<span data-ttu-id="e0e37-145">Hello toodelay měsíce až (GMT)</span><span class="sxs-lookup"><span data-stu-id="e0e37-145">hello month toodelay until (GMT)</span></span> |
| <span data-ttu-id="e0e37-146">Den *</span><span class="sxs-lookup"><span data-stu-id="e0e37-146">Day*</span></span> |<span data-ttu-id="e0e37-147">časové razítko</span><span class="sxs-lookup"><span data-stu-id="e0e37-147">timestamp</span></span> |<span data-ttu-id="e0e37-148">Hello den toodelay dokud (GMT)</span><span class="sxs-lookup"><span data-stu-id="e0e37-148">hello day toodelay until (GMT)</span></span> |

<br>

## <a name="next-steps"></a><span data-ttu-id="e0e37-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e0e37-149">Next steps</span></span>
<span data-ttu-id="e0e37-150">Teď vyzkoušet hello platformy a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="e0e37-150">Now, try out hello platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="e0e37-151">Můžete prozkoumat hello dalších dostupných konektorů v aplikacích logiky pohledem na našem [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="e0e37-151">You can explore hello other available connectors in logic apps by looking at our [APIs list](apis-list.md).</span></span>

