---
title: "Přidání zpoždění v aplikace logiky | Microsoft Docs"
description: "Přehled zpoždění a zpoždění – dokud akcí a jejich použití s aplikací Azure logiku."
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
ms.openlocfilehash: 5f4f7052d48b4ca4ed91212d970551141e78e852
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-delay-and-delay-until-actions"></a><span data-ttu-id="58179-103">Začínáme s zpoždění a zpoždění – dokud akce</span><span class="sxs-lookup"><span data-stu-id="58179-103">Get started with the delay and delay-until actions</span></span>
<span data-ttu-id="58179-104">Pomocí zpoždění a "zpoždění – dokud" akce, můžete dokončit scénáře pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="58179-104">By using the delay and "delay-until" actions, you can complete workflow scenarios.</span></span>

<span data-ttu-id="58179-105">Můžete například provést následující věci:</span><span class="sxs-lookup"><span data-stu-id="58179-105">For example, you can:</span></span>

* <span data-ttu-id="58179-106">Počkejte na jeden den v týdnu pro odeslání e-mailu aktualizace stavu.</span><span class="sxs-lookup"><span data-stu-id="58179-106">Wait until a weekday to send a status update over email.</span></span>
* <span data-ttu-id="58179-107">Pracovní postup počkat, až budou volání protokolu HTTP má čas na dokončení obnovení a načítání výsledek.</span><span class="sxs-lookup"><span data-stu-id="58179-107">Delay the workflow until an HTTP call has time to finish before resuming and retrieving the result.</span></span>

<span data-ttu-id="58179-108">Chcete-li začít používat zpoždění akce v aplikaci logiky, přečtěte si téma [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="58179-108">To get started using the delay action in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-the-delay-actions"></a><span data-ttu-id="58179-109">Pomocí akcí zpoždění</span><span class="sxs-lookup"><span data-stu-id="58179-109">Use the delay actions</span></span>
<span data-ttu-id="58179-110">Akce je operace, která se provádí v pracovním postupu, který je definován v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="58179-110">An action is an operation that is carried out by the workflow that is defined in a logic app.</span></span> <span data-ttu-id="58179-111">[Další informace o akcích](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="58179-111">[Learn more about actions](connectors-overview.md).</span></span>

<span data-ttu-id="58179-112">Tady je v sekvenci příklad použití kroku zpoždění v aplikaci logiky:</span><span class="sxs-lookup"><span data-stu-id="58179-112">Here’s an example sequence of how to use a delay step in a logic app:</span></span>

1. <span data-ttu-id="58179-113">Po přidání aktivační událost, klikněte na tlačítko **nový krok** a přidejte akci.</span><span class="sxs-lookup"><span data-stu-id="58179-113">After adding a trigger, click **New Step** to add an action.</span></span>
2. <span data-ttu-id="58179-114">Vyhledejte **zpoždění** se zprovoznit akce zpoždění.</span><span class="sxs-lookup"><span data-stu-id="58179-114">Search for **delay** to bring up the delay actions.</span></span> <span data-ttu-id="58179-115">V tomto příkladu jsme vyberte **zpoždění**.</span><span class="sxs-lookup"><span data-stu-id="58179-115">In this example, we will select **Delay**.</span></span>
   
    ![Zpoždění akce](./media/connectors-native-delay/using-action-1.png)
3. <span data-ttu-id="58179-117">Dokončete všechny vlastnosti Akce konfigurace zpoždění.</span><span class="sxs-lookup"><span data-stu-id="58179-117">Complete any of the action properties to configure the delay.</span></span>
   
    ![Konfigurace zpoždění](./media/connectors-native-delay/using-action-2.png)
4. <span data-ttu-id="58179-119">Klikněte na tlačítko **Uložit** k publikování a aktivovat aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="58179-119">Click **Save** to publish and activate the logic app.</span></span>

## <a name="action-details"></a><span data-ttu-id="58179-120">Podrobnosti akce</span><span class="sxs-lookup"><span data-stu-id="58179-120">Action details</span></span>
<span data-ttu-id="58179-121">Aktivační událost opakování má následující vlastnosti, které lze konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="58179-121">The recurrence trigger has the following properties that can be configured.</span></span>

### <a name="delay-action"></a><span data-ttu-id="58179-122">Zpoždění akce</span><span class="sxs-lookup"><span data-stu-id="58179-122">Delay action</span></span>
<span data-ttu-id="58179-123">Tato akce zpozdí spuštění pro určitý časový interval.</span><span class="sxs-lookup"><span data-stu-id="58179-123">This action delays the run for a certain time interval.</span></span>
<span data-ttu-id="58179-124">A * znamená, že je povinné pole.</span><span class="sxs-lookup"><span data-stu-id="58179-124">A * means that it is a required field.</span></span>

| <span data-ttu-id="58179-125">Zobrazované jméno</span><span class="sxs-lookup"><span data-stu-id="58179-125">Display name</span></span> | <span data-ttu-id="58179-126">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="58179-126">Property name</span></span> | <span data-ttu-id="58179-127">Popis</span><span class="sxs-lookup"><span data-stu-id="58179-127">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="58179-128">Počet *</span><span class="sxs-lookup"><span data-stu-id="58179-128">Count*</span></span> |<span data-ttu-id="58179-129">Počet</span><span class="sxs-lookup"><span data-stu-id="58179-129">count</span></span> |<span data-ttu-id="58179-130">Počet jednotek doba zpoždění před</span><span class="sxs-lookup"><span data-stu-id="58179-130">The number of time units to delay</span></span> |
| <span data-ttu-id="58179-131">Jednotka *</span><span class="sxs-lookup"><span data-stu-id="58179-131">Unit*</span></span> |<span data-ttu-id="58179-132">jednotka</span><span class="sxs-lookup"><span data-stu-id="58179-132">unit</span></span> |<span data-ttu-id="58179-133">Jednotka času: `Second`, `Minute`, `Hour`, nebo`Day`</span><span class="sxs-lookup"><span data-stu-id="58179-133">The unit of time: `Second`, `Minute`, `Hour`, or `Day`</span></span> |

<br>

### <a name="delay-until-action"></a><span data-ttu-id="58179-134">Zpoždění – dokud akce</span><span class="sxs-lookup"><span data-stu-id="58179-134">Delay-until action</span></span>
<span data-ttu-id="58179-135">Tato akce zpozdí spustit až do zadaného data a času.</span><span class="sxs-lookup"><span data-stu-id="58179-135">This action delays the run until a specified date/time.</span></span>
<span data-ttu-id="58179-136">A * znamená, že je povinné pole.</span><span class="sxs-lookup"><span data-stu-id="58179-136">A * means that it is a required field.</span></span>

| <span data-ttu-id="58179-137">Zobrazované jméno</span><span class="sxs-lookup"><span data-stu-id="58179-137">Display name</span></span> | <span data-ttu-id="58179-138">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="58179-138">Property name</span></span> | <span data-ttu-id="58179-139">Popis</span><span class="sxs-lookup"><span data-stu-id="58179-139">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="58179-140">Rok *</span><span class="sxs-lookup"><span data-stu-id="58179-140">Year*</span></span> |<span data-ttu-id="58179-141">časové razítko</span><span class="sxs-lookup"><span data-stu-id="58179-141">timestamp</span></span> |<span data-ttu-id="58179-142">Roku zpoždění až (GMT)</span><span class="sxs-lookup"><span data-stu-id="58179-142">The year to delay until (GMT)</span></span> |
| <span data-ttu-id="58179-143">Měsíc *</span><span class="sxs-lookup"><span data-stu-id="58179-143">Month*</span></span> |<span data-ttu-id="58179-144">časové razítko</span><span class="sxs-lookup"><span data-stu-id="58179-144">timestamp</span></span> |<span data-ttu-id="58179-145">V měsíci pro zpoždění až (GMT)</span><span class="sxs-lookup"><span data-stu-id="58179-145">The month to delay until (GMT)</span></span> |
| <span data-ttu-id="58179-146">Den *</span><span class="sxs-lookup"><span data-stu-id="58179-146">Day*</span></span> |<span data-ttu-id="58179-147">časové razítko</span><span class="sxs-lookup"><span data-stu-id="58179-147">timestamp</span></span> |<span data-ttu-id="58179-148">Den zpoždění až (GMT)</span><span class="sxs-lookup"><span data-stu-id="58179-148">The day to delay until (GMT)</span></span> |

<br>

## <a name="next-steps"></a><span data-ttu-id="58179-149">Další kroky</span><span class="sxs-lookup"><span data-stu-id="58179-149">Next steps</span></span>
<span data-ttu-id="58179-150">Teď vyzkoušet platformu a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="58179-150">Now, try out the platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="58179-151">Ostatní konektory k dispozici v aplikace logiky můžete prozkoumat pohledem na našem [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="58179-151">You can explore the other available connectors in logic apps by looking at our [APIs list](apis-list.md).</span></span>

