---
title: "Přidání aktivační událost opakování v aplikace logiky | Microsoft Docs"
description: "Přehled aktivační událost opakování a způsobu jeho použití s aplikací Azure logiku."
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
ms.openlocfilehash: fe558958c316c8dba42163e277ae01451f712e5a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-recurrence-trigger"></a><span data-ttu-id="71b2a-103">Začínáme s aktivační událost opakování</span><span class="sxs-lookup"><span data-stu-id="71b2a-103">Get started with the recurrence trigger</span></span>
<span data-ttu-id="71b2a-104">Pomocí aktivační událost opakování, můžete vytvořit výkonné pracovní postupy v cloudu.</span><span class="sxs-lookup"><span data-stu-id="71b2a-104">By using the recurrence trigger, you can create powerful workflows in the cloud.</span></span>

<span data-ttu-id="71b2a-105">Můžete například provést následující věci:</span><span class="sxs-lookup"><span data-stu-id="71b2a-105">For example, you can:</span></span>

* <span data-ttu-id="71b2a-106">Naplánujte pracovní postup spustit uloženou proceduru SQL každý den.</span><span class="sxs-lookup"><span data-stu-id="71b2a-106">Schedule a workflow to run a SQL stored procedure every day.</span></span>
* <span data-ttu-id="71b2a-107">Během posledního týdne o určitých hashtag e-mailu souhrn všech tweetů.</span><span class="sxs-lookup"><span data-stu-id="71b2a-107">Email a summary of all tweets within the last week about a certain hashtag.</span></span>

<span data-ttu-id="71b2a-108">Chcete-li začít používat aktivační událost opakování v aplikaci logiky, přečtěte si téma [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="71b2a-108">To get started using the recurrence trigger in a logic app, see [Create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span>

## <a name="use-a-recurrence-trigger"></a><span data-ttu-id="71b2a-109">Použít aktivační událost opakování</span><span class="sxs-lookup"><span data-stu-id="71b2a-109">Use a recurrence trigger</span></span>
<span data-ttu-id="71b2a-110">Aktivační událost je událost, která můžete použít ke spuštění pracovního postupu, který je definován v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="71b2a-110">A trigger is an event that can be used to start the workflow that is defined in a logic app.</span></span> <span data-ttu-id="71b2a-111">[Další informace o aktivační události](connectors-overview.md).</span><span class="sxs-lookup"><span data-stu-id="71b2a-111">[Learn more about triggers](connectors-overview.md).</span></span>

<span data-ttu-id="71b2a-112">Tady je v sekvenci příklad toho, jak nastavit opakování aktivační událost v aplikaci logiky:</span><span class="sxs-lookup"><span data-stu-id="71b2a-112">Here’s an example sequence of how to set up a recurrence trigger in a logic app:</span></span>

1. <span data-ttu-id="71b2a-113">Přidat **opakování** aktivační událost jako první krok v aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="71b2a-113">Add the **Recurrence** trigger as the first step in a logic app.</span></span>
2. <span data-ttu-id="71b2a-114">Zadejte parametry pro interval opakování.</span><span class="sxs-lookup"><span data-stu-id="71b2a-114">Fill in the parameters for the recurrence interval.</span></span>

<span data-ttu-id="71b2a-115">Aplikace logiky spustí spustit po každé časové období.</span><span class="sxs-lookup"><span data-stu-id="71b2a-115">The logic app now starts a run after each interval of time.</span></span>

![Trigger HTTP](./media/connectors-native-recurrence/using-trigger.png)

## <a name="trigger-details"></a><span data-ttu-id="71b2a-117">Podrobnosti o aktivační události</span><span class="sxs-lookup"><span data-stu-id="71b2a-117">Trigger details</span></span>
<span data-ttu-id="71b2a-118">Aktivační událost opakování má následující vlastnosti, které můžete konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="71b2a-118">The recurrence trigger has the following properties that you can configure.</span></span>

<span data-ttu-id="71b2a-119">Vyvolá aplikace logiky po zadaném časovém intervalu.</span><span class="sxs-lookup"><span data-stu-id="71b2a-119">It fires a logic app after a specified time interval.</span></span>
<span data-ttu-id="71b2a-120">A * znamená, že je povinné pole.</span><span class="sxs-lookup"><span data-stu-id="71b2a-120">A * means that it is a required field.</span></span>

| <span data-ttu-id="71b2a-121">Zobrazované jméno</span><span class="sxs-lookup"><span data-stu-id="71b2a-121">Display name</span></span> | <span data-ttu-id="71b2a-122">Název vlastnosti</span><span class="sxs-lookup"><span data-stu-id="71b2a-122">Property name</span></span> | <span data-ttu-id="71b2a-123">Popis</span><span class="sxs-lookup"><span data-stu-id="71b2a-123">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="71b2a-124">Frekvence *</span><span class="sxs-lookup"><span data-stu-id="71b2a-124">Frequency*</span></span> |<span data-ttu-id="71b2a-125">frekvence</span><span class="sxs-lookup"><span data-stu-id="71b2a-125">frequency</span></span> |<span data-ttu-id="71b2a-126">Jednotka času: `Second`, `Minute`, `Hour`, `Day`, nebo `Year`.</span><span class="sxs-lookup"><span data-stu-id="71b2a-126">The unit of time: `Second`, `Minute`, `Hour`, `Day`, or `Year`.</span></span> |
| <span data-ttu-id="71b2a-127">Interval *</span><span class="sxs-lookup"><span data-stu-id="71b2a-127">Interval*</span></span> |<span data-ttu-id="71b2a-128">Interval</span><span class="sxs-lookup"><span data-stu-id="71b2a-128">interval</span></span> |<span data-ttu-id="71b2a-129">Interval dané frekvenci opakování.</span><span class="sxs-lookup"><span data-stu-id="71b2a-129">The interval of the given frequency for the recurrence.</span></span> |
| <span data-ttu-id="71b2a-130">Časové pásmo</span><span class="sxs-lookup"><span data-stu-id="71b2a-130">Time Zone</span></span> |<span data-ttu-id="71b2a-131">Časové pásmo</span><span class="sxs-lookup"><span data-stu-id="71b2a-131">timeZone</span></span> |<span data-ttu-id="71b2a-132">Pokud je čas spuštění zadaný bez časový posun, použije se toto časové pásmo.</span><span class="sxs-lookup"><span data-stu-id="71b2a-132">If a start time is provided without a UTC offset, this time zone will be used.</span></span> |
| <span data-ttu-id="71b2a-133">Čas spuštění</span><span class="sxs-lookup"><span data-stu-id="71b2a-133">Start time</span></span> |<span data-ttu-id="71b2a-134">startTime</span><span class="sxs-lookup"><span data-stu-id="71b2a-134">startTime</span></span> |<span data-ttu-id="71b2a-135">Čas zahájení v [formátu ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations).</span><span class="sxs-lookup"><span data-stu-id="71b2a-135">The start time in [ISO 8601 format](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations).</span></span> |

<br>

## <a name="next-steps"></a><span data-ttu-id="71b2a-136">Další kroky</span><span class="sxs-lookup"><span data-stu-id="71b2a-136">Next steps</span></span>
<span data-ttu-id="71b2a-137">Teď vyzkoušet platformu a [vytvoření aplikace logiky](../logic-apps/logic-apps-create-a-logic-app.md).</span><span class="sxs-lookup"><span data-stu-id="71b2a-137">Now, try out the platform and [create a logic app](../logic-apps/logic-apps-create-a-logic-app.md).</span></span> <span data-ttu-id="71b2a-138">Ostatní konektory k dispozici v aplikace logiky můžete prozkoumat pohledem na našem [rozhraní API seznamu](apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="71b2a-138">You can explore the other available connectors in logic apps by looking at our [APIs list](apis-list.md).</span></span>

