---
title: "aktivační události opakování hello aaaAdd do aplikace logiky | Microsoft Docs"
description: "Přehled hello opakování aktivační události a jak toouse se službou Azure logiku aplikace."
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 51dd4f22-7dc5-41af-a0a9-e7148378cd50
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: e7c625c382a88a1e7cdfff4ddc0caf55727232bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-recurrence-trigger"></a><span data-ttu-id="1b443-103">Začínáme s aktivační událost opakování hello</span><span class="sxs-lookup"><span data-stu-id="1b443-103">Get started with hello recurrence trigger</span></span>
<span data-ttu-id="1b443-104">Pomocí aktivační hello opakování, můžete vytvořit výkonné pracovní postupy v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="1b443-104">By using hello recurrence trigger, you can create powerful workflows in hello cloud.</span></span>

<span data-ttu-id="1b443-105">Můžete například provést následující věci:</span><span class="sxs-lookup"><span data-stu-id="1b443-105">For example, you can:</span></span>

* <span data-ttu-id="1b443-106">Plánování pracovního postupu toorun uloženou proceduru SQL každý den.</span><span class="sxs-lookup"><span data-stu-id="1b443-106">Schedule a workflow toorun a SQL stored procedure every day.</span></span>
* <span data-ttu-id="1b443-107">E-mailu souhrn všech tweetů v rámci hello minulého týdne o určitých hashtag.</span><span class="sxs-lookup"><span data-stu-id="1b443-107">Email a summary of all tweets within hello last week about a certain hashtag.</span></span>

<span data-ttu-id="1b443-108">tooget spuštěna v aktivační události opakování hello aplikace logiky, najdete v části [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="1b443-108">tooget started using hello recurrence trigger in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-a-recurrence-trigger"></a><span data-ttu-id="1b443-109">Použít aktivační událost opakování</span><span class="sxs-lookup"><span data-stu-id="1b443-109">Use a recurrence trigger</span></span>
<span data-ttu-id="1b443-110">Aktivační událost je událost, která lze použít toostart hello workflow, který je definován v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="1b443-110">A trigger is an event that can be used toostart hello workflow that is defined in a logic app.</span></span> <span data-ttu-id="1b443-111">[Další informace o aktivační události](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1b443-111">[Learn more about triggers](connectors-overview.md).</span></span>

<span data-ttu-id="1b443-112">Tady je příklad posloupnosti jak aktivovat tooset až opakování v aplikaci logiky:</span><span class="sxs-lookup"><span data-stu-id="1b443-112">Here’s an example sequence of how tooset up a recurrence trigger in a logic app:</span></span>

1. <span data-ttu-id="1b443-113">Přidat hello **opakování** aktivační událost jako první krok hello v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="1b443-113">Add hello **Recurrence** trigger as hello first step in a logic app.</span></span>
2. <span data-ttu-id="1b443-114">Vyplňte hello parametry pro interval opakování hello.</span><span class="sxs-lookup"><span data-stu-id="1b443-114">Fill in hello parameters for hello recurrence interval.</span></span>

<span data-ttu-id="1b443-115">aplikace logiky Hello spustí spustit po každé časové období.</span><span class="sxs-lookup"><span data-stu-id="1b443-115">hello logic app now starts a run after each interval of time.</span></span>

![Trigger HTTP](./media/connectors-native-recurrence/using-trigger.png)

## <a name="trigger-details"></a><span data-ttu-id="1b443-117">Podrobnosti o aktivační události</span><span class="sxs-lookup"><span data-stu-id="1b443-117">Trigger details</span></span>
<span data-ttu-id="1b443-118">aktivační událost Hello opakování má následující vlastnosti, které můžete konfigurovat hello.</span><span class="sxs-lookup"><span data-stu-id="1b443-118">hello recurrence trigger has hello following properties that you can configure.</span></span>

<span data-ttu-id="1b443-119">Vyvolá aplikace logiky po zadaném časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="1b443-119">It fires a logic app after a specified time interval.</span></span>
<span data-ttu-id="1b443-120">A * znamená, že je povinné pole.</span><span class="sxs-lookup"><span data-stu-id="1b443-120">A * means that it is a required field.</span></span>

| <span data-ttu-id="1b443-121">Zobrazované jméno</span><span class="sxs-lookup"><span data-stu-id="1b443-121">Display name</span></span> | <span data-ttu-id="1b443-122">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="1b443-122">Property name</span></span> | <span data-ttu-id="1b443-123">Popis</span><span class="sxs-lookup"><span data-stu-id="1b443-123">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1b443-124">Frekvence *</span><span class="sxs-lookup"><span data-stu-id="1b443-124">Frequency*</span></span> |<span data-ttu-id="1b443-125">frequency</span><span class="sxs-lookup"><span data-stu-id="1b443-125">frequency</span></span> |<span data-ttu-id="1b443-126">jednotka času Hello: `Second`, `Minute`, `Hour`, `Day`, nebo `Year`.</span><span class="sxs-lookup"><span data-stu-id="1b443-126">hello unit of time: `Second`, `Minute`, `Hour`, `Day`, or `Year`.</span></span> |
| <span data-ttu-id="1b443-127">Interval *</span><span class="sxs-lookup"><span data-stu-id="1b443-127">Interval*</span></span> |<span data-ttu-id="1b443-128">interval</span><span class="sxs-lookup"><span data-stu-id="1b443-128">interval</span></span> |<span data-ttu-id="1b443-129">interval Hello hello zadané frekvence opakování hello.</span><span class="sxs-lookup"><span data-stu-id="1b443-129">hello interval of hello given frequency for hello recurrence.</span></span> |
| <span data-ttu-id="1b443-130">Časové pásmo</span><span class="sxs-lookup"><span data-stu-id="1b443-130">Time Zone</span></span> |<span data-ttu-id="1b443-131">Časové pásmo</span><span class="sxs-lookup"><span data-stu-id="1b443-131">timeZone</span></span> |<span data-ttu-id="1b443-132">Pokud je čas spuštění zadaný bez časový posun, použije se toto časové pásmo.</span><span class="sxs-lookup"><span data-stu-id="1b443-132">If a start time is provided without a UTC offset, this time zone will be used.</span></span> |
| <span data-ttu-id="1b443-133">Čas spuštění</span><span class="sxs-lookup"><span data-stu-id="1b443-133">Start time</span></span> |<span data-ttu-id="1b443-134">startTime</span><span class="sxs-lookup"><span data-stu-id="1b443-134">startTime</span></span> |<span data-ttu-id="1b443-135">Hello počáteční čas v [formátu ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations).</span><span class="sxs-lookup"><span data-stu-id="1b443-135">hello start time in [ISO 8601 format](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations).</span></span> |

<br>

## <a name="next-steps"></a><span data-ttu-id="1b443-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1b443-136">Next steps</span></span>
<span data-ttu-id="1b443-137">Teď vyzkoušet hello platformy a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="1b443-137">Now, try out hello platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="1b443-138">Můžete prozkoumat hello dalších dostupných konektorů v aplikacích logiky pohledem na našem [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="1b443-138">You can explore hello other available connectors in logic apps by looking at our [APIs list](apis-list.md).</span></span>

