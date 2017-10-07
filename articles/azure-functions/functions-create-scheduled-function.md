---
title: "aaaCreate funkci, která spouští podle plánu v Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate funkce v Azure, který běží podle plánu, který definujete."
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
ms.openlocfilehash: 793b06a65a154466dfd4c121bcc88082227cd597
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-in-azure-that-is-triggered-by-a-timer"></a><span data-ttu-id="80f0d-103">Vytvoření funkce v Azure aktivované časovačem</span><span class="sxs-lookup"><span data-stu-id="80f0d-103">Create a function in Azure that is triggered by a timer</span></span>

<span data-ttu-id="80f0d-104">Zjistěte, jak Azure Functions toocreate toouse funkci, která běží na základě plánu, který definujete.</span><span class="sxs-lookup"><span data-stu-id="80f0d-104">Learn how toouse Azure Functions toocreate a function that runs based a schedule that you define.</span></span>

![Vytvoření aplikace pro funkce v hello portálu Azure](./media/functions-create-scheduled-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a><span data-ttu-id="80f0d-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="80f0d-106">Prerequisites</span></span>

<span data-ttu-id="80f0d-107">toocomplete v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="80f0d-107">toocomplete this tutorial:</span></span>

+ <span data-ttu-id="80f0d-108">Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) před tím, než začnete.</span><span class="sxs-lookup"><span data-stu-id="80f0d-108">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a><span data-ttu-id="80f0d-109">Vytvoření aplikace Azure Function App</span><span class="sxs-lookup"><span data-stu-id="80f0d-109">Create an Azure Function app</span></span>

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Aplikace Function App byla úspěšně vytvořena.](./media/functions-create-first-azure-function/function-app-create-success.png)

<span data-ttu-id="80f0d-111">Dál vytvořte funkci v nové funkce aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="80f0d-111">Next, you create a function in hello new function app.</span></span>

<a name="create-function"></a>

## <a name="create-a-timer-triggered-function"></a><span data-ttu-id="80f0d-112">Vytvoření funkce aktivované časovačem</span><span class="sxs-lookup"><span data-stu-id="80f0d-112">Create a timer triggered function</span></span>

1. <span data-ttu-id="80f0d-113">Rozšířit funkce aplikace a klikněte na tlačítko hello  **+**  tlačítko vedle příliš**funkce**.</span><span class="sxs-lookup"><span data-stu-id="80f0d-113">Expand your function app and click hello **+** button next too**Functions**.</span></span> <span data-ttu-id="80f0d-114">Pokud je to první funkce hello ve vaší aplikaci funkce, vyberte **vlastní funkce**.</span><span class="sxs-lookup"><span data-stu-id="80f0d-114">If this is hello first function in your function app, select **Custom function**.</span></span> <span data-ttu-id="80f0d-115">Zobrazí se hello kompletní sada šablon funkcí.</span><span class="sxs-lookup"><span data-stu-id="80f0d-115">This displays hello complete set of function templates.</span></span>

    ![Funkce Rychlé spuštění stránku hello portálu Azure](./media/functions-create-scheduled-function/add-first-function.png)

2. <span data-ttu-id="80f0d-117">Vyberte hello **TimerTrigger** šablonu pro požadovaný jazyk.</span><span class="sxs-lookup"><span data-stu-id="80f0d-117">Select hello **TimerTrigger** template for your desired language.</span></span> <span data-ttu-id="80f0d-118">Pak použijte hello nastavení uvedeného v tabulce hello:</span><span class="sxs-lookup"><span data-stu-id="80f0d-118">Then use hello settings as specified in hello table:</span></span>

    ![Vytvoření funkce časovače aktivuje v hello portálu Azure.](./media/functions-create-scheduled-function/functions-create-timer-trigger.png)

    | <span data-ttu-id="80f0d-120">Nastavení</span><span class="sxs-lookup"><span data-stu-id="80f0d-120">Setting</span></span> | <span data-ttu-id="80f0d-121">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="80f0d-121">Suggested value</span></span> | <span data-ttu-id="80f0d-122">Popis</span><span class="sxs-lookup"><span data-stu-id="80f0d-122">Description</span></span> |
    |---|---|---|
    | <span data-ttu-id="80f0d-123">**Pojmenujte svoji funkci**</span><span class="sxs-lookup"><span data-stu-id="80f0d-123">**Name your function**</span></span> | <span data-ttu-id="80f0d-124">TimerTriggerCSharp1</span><span class="sxs-lookup"><span data-stu-id="80f0d-124">TimerTriggerCSharp1</span></span> | <span data-ttu-id="80f0d-125">Definuje název hello funkce spustí časovač.</span><span class="sxs-lookup"><span data-stu-id="80f0d-125">Defines hello name of your timer triggered function.</span></span> |
    | <span data-ttu-id="80f0d-126">**[Plán](http://en.wikipedia.org/wiki/Cron#CRON_expression)**</span><span class="sxs-lookup"><span data-stu-id="80f0d-126">**[Schedule](http://en.wikipedia.org/wiki/Cron#CRON_expression)**</span></span> | <span data-ttu-id="80f0d-127">0 \*/1 \* \* \* \*</span><span class="sxs-lookup"><span data-stu-id="80f0d-127">0 \*/1 \* \* \* \*</span></span> | <span data-ttu-id="80f0d-128">Šest pole [výraz CRON](http://en.wikipedia.org/wiki/Cron#CRON_expression) která plánuje vaší funkce toorun každou minutu.</span><span class="sxs-lookup"><span data-stu-id="80f0d-128">A six field [CRON expression](http://en.wikipedia.org/wiki/Cron#CRON_expression) that schedules your function toorun every minute.</span></span> |

2. <span data-ttu-id="80f0d-129">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="80f0d-129">Click **Create**.</span></span> <span data-ttu-id="80f0d-130">Ve zvoleném jazyce se vytvoří funkce, která se bude spouštět každou minutu.</span><span class="sxs-lookup"><span data-stu-id="80f0d-130">A function is created in your chosen language that runs every minute.</span></span>

3. <span data-ttu-id="80f0d-131">Zobrazení informací trasování zapsat toohello protokoly a zkontrolujte provádění.</span><span class="sxs-lookup"><span data-stu-id="80f0d-131">Verify execution by viewing trace information written toohello logs.</span></span>

    ![Funkce přihlášení prohlížeč hello portálu Azure.](./media/functions-create-scheduled-function/functions-timer-trigger-view-logs2.png)

<span data-ttu-id="80f0d-133">Nyní můžete změnit plán hello funkce tak, aby běžel méně často, například jednou za hodinu.</span><span class="sxs-lookup"><span data-stu-id="80f0d-133">Now, you can change hello function's schedule so that it runs less often, such as once every hour.</span></span> 

## <a name="update-hello-timer-schedule"></a><span data-ttu-id="80f0d-134">Plán aktualizace hello časovače</span><span class="sxs-lookup"><span data-stu-id="80f0d-134">Update hello timer schedule</span></span>

1. <span data-ttu-id="80f0d-135">Rozbalte funkci a klikněte na **Integrace**.</span><span class="sxs-lookup"><span data-stu-id="80f0d-135">Expand your function and click **Integrate**.</span></span> <span data-ttu-id="80f0d-136">Toto je, kde můžete definovat vstup a výstup vazby pro funkce a také nastavit plán hello.</span><span class="sxs-lookup"><span data-stu-id="80f0d-136">This is where you define input and output bindings for your function and also set hello schedule.</span></span> 

2. <span data-ttu-id="80f0d-137">V poli **Plán** zadejte novou hodnotu `0 0 */1 * * *` a potom klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="80f0d-137">Enter a new **Schedule** value of `0 0 */1 * * *`, and then click **Save**.</span></span>  

![Funkce Aktualizovat plán časovače v hello portálu Azure.](./media/functions-create-scheduled-function/functions-timer-trigger-change-schedule.png)

<span data-ttu-id="80f0d-139">Teď máte funkci, která se spouští jednou za hodinu.</span><span class="sxs-lookup"><span data-stu-id="80f0d-139">You now have a function that runs once every hour.</span></span> 

## <a name="clean-up-resources"></a><span data-ttu-id="80f0d-140">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="80f0d-140">Clean up resources</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="80f0d-141">Další kroky</span><span class="sxs-lookup"><span data-stu-id="80f0d-141">Next steps</span></span>

<span data-ttu-id="80f0d-142">Vytvořili jste funkci, která se spouští na základě plánu.</span><span class="sxs-lookup"><span data-stu-id="80f0d-142">You have created a function that runs based on a schedule.</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="80f0d-143">Další informace o aktivačních časovačích najdete v tématu [Plánování spouštění kódu v Azure Functions](functions-bindings-timer.md).</span><span class="sxs-lookup"><span data-stu-id="80f0d-143">For more information timer triggers, see [Schedule code execution with Azure Functions](functions-bindings-timer.md).</span></span>