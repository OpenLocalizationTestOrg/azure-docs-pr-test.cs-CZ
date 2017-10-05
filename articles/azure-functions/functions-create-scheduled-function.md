---
title: "Vytvoření funkce v Azure, která se spouští podle plánu | Dokumentace Microsoftu"
description: "Zjistěte, jak v Azure vytvořit funkci, která se spouští na základě vámi definovaného plánu."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: ba50ee47-58e0-4972-b67b-828f2dc48701
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 03cc5e71e8eb20002cf58e713fc0fc92a9129874
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-function-in-azure-that-is-triggered-by-a-timer"></a><span data-ttu-id="32cbe-103">Vytvoření funkce v Azure aktivované časovačem</span><span class="sxs-lookup"><span data-stu-id="32cbe-103">Create a function in Azure that is triggered by a timer</span></span>

<span data-ttu-id="32cbe-104">Zjistěte, jak ve službě Azure Functions vytvořit funkci, která se spouští na základě vámi definovaného plánu.</span><span class="sxs-lookup"><span data-stu-id="32cbe-104">Learn how to use Azure Functions to create a function that runs based a schedule that you define.</span></span>

![Vytvoření aplikace Function App na webu Azure Portal](./media/functions-create-scheduled-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a><span data-ttu-id="32cbe-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="32cbe-106">Prerequisites</span></span>

<span data-ttu-id="32cbe-107">K provedení kroků v tomto kurzu je potřeba:</span><span class="sxs-lookup"><span data-stu-id="32cbe-107">To complete this tutorial:</span></span>

+ <span data-ttu-id="32cbe-108">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="32cbe-108">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a><span data-ttu-id="32cbe-109">Vytvoření aplikace Azure Function App</span><span class="sxs-lookup"><span data-stu-id="32cbe-109">Create an Azure Function app</span></span>

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Aplikace Function App byla úspěšně vytvořena.](./media/functions-create-first-azure-function/function-app-create-success.png)

<span data-ttu-id="32cbe-111">Dál vytvoříte v nové aplikaci Function App funkci.</span><span class="sxs-lookup"><span data-stu-id="32cbe-111">Next, you create a function in the new function app.</span></span>

<a name="create-function"></a>

## <a name="create-a-timer-triggered-function"></a><span data-ttu-id="32cbe-112">Vytvoření funkce aktivované časovačem</span><span class="sxs-lookup"><span data-stu-id="32cbe-112">Create a timer triggered function</span></span>

1. <span data-ttu-id="32cbe-113">Rozbalte aplikaci Function App a klikněte na tlačítko **+** vedle položky **Funkce**.</span><span class="sxs-lookup"><span data-stu-id="32cbe-113">Expand your function app and click the **+** button next to **Functions**.</span></span> <span data-ttu-id="32cbe-114">Pokud jde o první funkci ve vaší aplikaci Function App, vyberte možnost **Vlastní funkce**.</span><span class="sxs-lookup"><span data-stu-id="32cbe-114">If this is the first function in your function app, select **Custom function**.</span></span> <span data-ttu-id="32cbe-115">Zobrazí se kompletní sada šablon funkcí.</span><span class="sxs-lookup"><span data-stu-id="32cbe-115">This displays the complete set of function templates.</span></span>

    ![Stručný úvod do služby Functions na webu Azure Portal](./media/functions-create-scheduled-function/add-first-function.png)

2. <span data-ttu-id="32cbe-117">Vyberte šablonu **TimerTrigger** pro vámi požadovaný jazyk.</span><span class="sxs-lookup"><span data-stu-id="32cbe-117">Select the **TimerTrigger** template for your desired language.</span></span> <span data-ttu-id="32cbe-118">Potom použijte nastavení uvedená v tabulce:</span><span class="sxs-lookup"><span data-stu-id="32cbe-118">Then use the settings as specified in the table:</span></span>

    ![Vytvořte na portálu Azure Portal funkci aktivovanou časovačem.](./media/functions-create-scheduled-function/functions-create-timer-trigger.png)

    | <span data-ttu-id="32cbe-120">Nastavení</span><span class="sxs-lookup"><span data-stu-id="32cbe-120">Setting</span></span> | <span data-ttu-id="32cbe-121">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="32cbe-121">Suggested value</span></span> | <span data-ttu-id="32cbe-122">Popis</span><span class="sxs-lookup"><span data-stu-id="32cbe-122">Description</span></span> |
    |---|---|---|
    | <span data-ttu-id="32cbe-123">**Pojmenujte svoji funkci**</span><span class="sxs-lookup"><span data-stu-id="32cbe-123">**Name your function**</span></span> | <span data-ttu-id="32cbe-124">TimerTriggerCSharp1</span><span class="sxs-lookup"><span data-stu-id="32cbe-124">TimerTriggerCSharp1</span></span> | <span data-ttu-id="32cbe-125">Určuje název funkce aktivované časovačem.</span><span class="sxs-lookup"><span data-stu-id="32cbe-125">Defines the name of your timer triggered function.</span></span> |
    | <span data-ttu-id="32cbe-126">**[Plán](http://en.wikipedia.org/wiki/Cron#CRON_expression)**</span><span class="sxs-lookup"><span data-stu-id="32cbe-126">**[Schedule](http://en.wikipedia.org/wiki/Cron#CRON_expression)**</span></span> | <span data-ttu-id="32cbe-127">0 \*/1 \* \* \* \*</span><span class="sxs-lookup"><span data-stu-id="32cbe-127">0 \*/1 \* \* \* \*</span></span> | <span data-ttu-id="32cbe-128">Pole [Výraz CRON](http://en.wikipedia.org/wiki/Cron#CRON_expression) v šestkové soustavě, ve kterém naplánujete spouštění funkce každou minutu.</span><span class="sxs-lookup"><span data-stu-id="32cbe-128">A six field [CRON expression](http://en.wikipedia.org/wiki/Cron#CRON_expression) that schedules your function to run every minute.</span></span> |

2. <span data-ttu-id="32cbe-129">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="32cbe-129">Click **Create**.</span></span> <span data-ttu-id="32cbe-130">Ve zvoleném jazyce se vytvoří funkce, která se bude spouštět každou minutu.</span><span class="sxs-lookup"><span data-stu-id="32cbe-130">A function is created in your chosen language that runs every minute.</span></span>

3. <span data-ttu-id="32cbe-131">Podívejte se na informace o trasování zaznamenané v protokolech a ověřte provedení.</span><span class="sxs-lookup"><span data-stu-id="32cbe-131">Verify execution by viewing trace information written to the logs.</span></span>

    ![Prohlížeč protokolu funkcí na webu Azure Portal.](./media/functions-create-scheduled-function/functions-timer-trigger-view-logs2.png)

<span data-ttu-id="32cbe-133">Teď můžete změnit plán funkce tak, aby se funkce spouštěla méně často, třeba jednou za hodinu.</span><span class="sxs-lookup"><span data-stu-id="32cbe-133">Now, you can change the function's schedule so that it runs less often, such as once every hour.</span></span> 

## <a name="update-the-timer-schedule"></a><span data-ttu-id="32cbe-134">Aktualizace plánu časovače</span><span class="sxs-lookup"><span data-stu-id="32cbe-134">Update the timer schedule</span></span>

1. <span data-ttu-id="32cbe-135">Rozbalte funkci a klikněte na **Integrace**.</span><span class="sxs-lookup"><span data-stu-id="32cbe-135">Expand your function and click **Integrate**.</span></span> <span data-ttu-id="32cbe-136">Tady se určují vstupní a výstupní vazby funkce a nastavuje plán.</span><span class="sxs-lookup"><span data-stu-id="32cbe-136">This is where you define input and output bindings for your function and also set the schedule.</span></span> 

2. <span data-ttu-id="32cbe-137">V poli **Plán** zadejte novou hodnotu `0 0 */1 * * *` a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="32cbe-137">Enter a new **Schedule** value of `0 0 */1 * * *`, and then click **Save**.</span></span>  

![Aktualizace plánu časovače funkcí na webu Azure Portal.](./media/functions-create-scheduled-function/functions-timer-trigger-change-schedule.png)

<span data-ttu-id="32cbe-139">Teď máte funkci, která se spouští jednou za hodinu.</span><span class="sxs-lookup"><span data-stu-id="32cbe-139">You now have a function that runs once every hour.</span></span> 

## <a name="clean-up-resources"></a><span data-ttu-id="32cbe-140">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="32cbe-140">Clean up resources</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="32cbe-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="32cbe-141">Next steps</span></span>

<span data-ttu-id="32cbe-142">Vytvořili jste funkci, která se spouští na základě plánu.</span><span class="sxs-lookup"><span data-stu-id="32cbe-142">You have created a function that runs based on a schedule.</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="32cbe-143">Další informace o aktivačních časovačích najdete v tématu [Plánování spouštění kódu v Azure Functions](functions-bindings-timer.md).</span><span class="sxs-lookup"><span data-stu-id="32cbe-143">For more information timer triggers, see [Schedule code execution with Azure Functions](functions-bindings-timer.md).</span></span>