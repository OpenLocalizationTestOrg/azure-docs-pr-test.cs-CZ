---
title: "Vytvoření funkce aktivované webhookem GitHubu v Azure | Dokumentace Microsoftu"
description: "Pomocí služby Azure Functions vytvoříte funkci bez serveru, která je vyvolána webhookem GitHubu."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 36ef34b8-3729-4940-86d2-cb8e176fcc06
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 038bb4cf0a9278416261c05ddaa0ee97d83b63c5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-function-triggered-by-a-github-webhook"></a><span data-ttu-id="d93b7-103">Vytvoření funkce aktivované webhookem GitHubu</span><span class="sxs-lookup"><span data-stu-id="d93b7-103">Create a function triggered by a GitHub webhook</span></span>

<span data-ttu-id="d93b7-104">Naučte se vytvořit funkci, která se aktivuje požadavkem webhooku HTTP s datovou částí specifickou pro GitHub.</span><span class="sxs-lookup"><span data-stu-id="d93b7-104">Learn how to create a function that is triggered by an HTTP webhook request with a GitHub-specific payload.</span></span>

![Funkce aktivovaná webhookem GitHubu na webu Azure Portal](./media/functions-create-github-webhook-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a><span data-ttu-id="d93b7-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d93b7-106">Prerequisites</span></span>

+ <span data-ttu-id="d93b7-107">Účet GitHubu s alespoň jedním projektem.</span><span class="sxs-lookup"><span data-stu-id="d93b7-107">A GitHub account with at least one project.</span></span>
+ <span data-ttu-id="d93b7-108">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="d93b7-108">An Azure subscription.</span></span> <span data-ttu-id="d93b7-109">Pokud ho nemáte, než začnete, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="d93b7-109">If you don't have one, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a><span data-ttu-id="d93b7-110">Vytvoření aplikace Azure Function App</span><span class="sxs-lookup"><span data-stu-id="d93b7-110">Create an Azure Function app</span></span>

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Aplikace Function App byla úspěšně vytvořena.](./media/functions-create-first-azure-function/function-app-create-success.png)

<span data-ttu-id="d93b7-112">Dál vytvoříte v nové aplikaci Function App funkci.</span><span class="sxs-lookup"><span data-stu-id="d93b7-112">Next, you create a function in the new function app.</span></span>

<a name="create-function"></a>

## <a name="create-a-github-webhook-triggered-function"></a><span data-ttu-id="d93b7-113">Vytvoření funkce aktivované webhookem GitHubu</span><span class="sxs-lookup"><span data-stu-id="d93b7-113">Create a GitHub webhook triggered function</span></span>

1. <span data-ttu-id="d93b7-114">Rozbalte aplikaci Function App a klikněte na tlačítko **+** vedle položky **Funkce**.</span><span class="sxs-lookup"><span data-stu-id="d93b7-114">Expand your function app and click the **+** button next to **Functions**.</span></span> <span data-ttu-id="d93b7-115">Pokud jde o první funkci ve vaší aplikaci Function App, vyberte možnost **Vlastní funkce**.</span><span class="sxs-lookup"><span data-stu-id="d93b7-115">If this is the first function in your function app, select **Custom function**.</span></span> <span data-ttu-id="d93b7-116">Zobrazí se kompletní sada šablon funkcí.</span><span class="sxs-lookup"><span data-stu-id="d93b7-116">This displays the complete set of function templates.</span></span>

    ![Stručný úvod do služby Functions na webu Azure Portal](./media/functions-create-github-webhook-triggered-function/add-first-function.png)

2. <span data-ttu-id="d93b7-118">Vyberte **WebHook Githubu** šablonu pro požadovaný jazyk.</span><span class="sxs-lookup"><span data-stu-id="d93b7-118">Select the **GitHub WebHook** template for your desired language.</span></span> <span data-ttu-id="d93b7-119">**Pojmenujte funkci** a pak vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d93b7-119">**Name your function**, then select **Create**.</span></span>

     ![Vytvoření funkce aktivované webhookem GitHubu na webu Azure Portal](./media/functions-create-github-webhook-triggered-function/functions-create-github-webhook-trigger.png) 

3. <span data-ttu-id="d93b7-121">V nové funkci klikněte na **</> Získat adresu URL funkce** a potom zkopírujte a uložte příslušné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d93b7-121">In your new function, click **</> Get function URL**, then copy and save the values.</span></span> <span data-ttu-id="d93b7-122">To samé udělejte s možností **</> Získat tajný klíč GitHubu**.</span><span class="sxs-lookup"><span data-stu-id="d93b7-122">Do the same thing for **</> Get GitHub secret**.</span></span> <span data-ttu-id="d93b7-123">Tyto hodnoty použijete ke konfiguraci webhooku na GitHubu.</span><span class="sxs-lookup"><span data-stu-id="d93b7-123">You use these values to configure the webhook in GitHub.</span></span>

    ![Kontrola kódu funkce](./media/functions-create-github-webhook-triggered-function/functions-copy-function-url-github-secret.png)

<span data-ttu-id="d93b7-125">Dále vytvoříte webhook ve vašem úložišti GitHub.</span><span class="sxs-lookup"><span data-stu-id="d93b7-125">Next, you create a webhook in your GitHub repository.</span></span>

## <a name="configure-the-webhook"></a><span data-ttu-id="d93b7-126">Konfigurace webhooku</span><span class="sxs-lookup"><span data-stu-id="d93b7-126">Configure the webhook</span></span>

1. <span data-ttu-id="d93b7-127">V GitHubu přejděte do úložiště, které vlastníte.</span><span class="sxs-lookup"><span data-stu-id="d93b7-127">In GitHub, navigate to a repository that you own.</span></span> <span data-ttu-id="d93b7-128">Můžete také použít libovolné úložiště, které máte rozvětvené.</span><span class="sxs-lookup"><span data-stu-id="d93b7-128">You can also use any repository that you have forked.</span></span> <span data-ttu-id="d93b7-129">Pokud potřebujete úložiště rozvětvit, použijte stránku <https://github.com/Azure-Samples/functions-quickstart>.</span><span class="sxs-lookup"><span data-stu-id="d93b7-129">If you need to fork a repository, use <https://github.com/Azure-Samples/functions-quickstart>.</span></span>

1. <span data-ttu-id="d93b7-130">Klikněte na **Settings** (Nastavení), pak na **Webhooks** (Webhooky) a **Add webhook** (Přidat webhook).</span><span class="sxs-lookup"><span data-stu-id="d93b7-130">Click **Settings**, then click **Webhooks**, and  **Add webhook**.</span></span>

    ![Přidání webhooku GitHubu](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-2.png)

1. <span data-ttu-id="d93b7-132">Použijte nastavení uvedené v tabulce a potom klikněte na **Přidat webhook**.</span><span class="sxs-lookup"><span data-stu-id="d93b7-132">Use settings as specified in the table, then click **Add webhook**.</span></span>

    ![Nastavení adresy URL a tajného klíče webhooku](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-3.png)

| <span data-ttu-id="d93b7-134">Nastavení</span><span class="sxs-lookup"><span data-stu-id="d93b7-134">Setting</span></span> | <span data-ttu-id="d93b7-135">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="d93b7-135">Suggested value</span></span> | <span data-ttu-id="d93b7-136">Popis</span><span class="sxs-lookup"><span data-stu-id="d93b7-136">Description</span></span> |
|---|---|---|
| <span data-ttu-id="d93b7-137">**Datová část adresy URL**</span><span class="sxs-lookup"><span data-stu-id="d93b7-137">**Payload URL**</span></span> | <span data-ttu-id="d93b7-138">Zkopírovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="d93b7-138">Copied value</span></span> | <span data-ttu-id="d93b7-139">Použijte hodnotu vrácenou příkazem **</> Získat adresu URL funkce**.</span><span class="sxs-lookup"><span data-stu-id="d93b7-139">Use the value returned by  **</> Get function URL**.</span></span> |
| <span data-ttu-id="d93b7-140">**Tajný kód**</span><span class="sxs-lookup"><span data-stu-id="d93b7-140">**Secret**</span></span>   | <span data-ttu-id="d93b7-141">Zkopírovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="d93b7-141">Copied value</span></span> | <span data-ttu-id="d93b7-142">Použijte hodnotu vrácenou příkazem **</> Získat tajný kód GitHubu**.</span><span class="sxs-lookup"><span data-stu-id="d93b7-142">Use the value returned by  **</> Get GitHub secret**.</span></span> |
| <span data-ttu-id="d93b7-143">**Typ obsahu**</span><span class="sxs-lookup"><span data-stu-id="d93b7-143">**Content type**</span></span> | <span data-ttu-id="d93b7-144">application/json</span><span class="sxs-lookup"><span data-stu-id="d93b7-144">application/json</span></span> | <span data-ttu-id="d93b7-145">Funkce očekává datovou část JSON.</span><span class="sxs-lookup"><span data-stu-id="d93b7-145">The function expects a JSON payload.</span></span> |
| <span data-ttu-id="d93b7-146">Aktivační události</span><span class="sxs-lookup"><span data-stu-id="d93b7-146">Event triggers</span></span> | <span data-ttu-id="d93b7-147">Nechat mě vybrat jednotlivé události</span><span class="sxs-lookup"><span data-stu-id="d93b7-147">Let me select individual events</span></span> | <span data-ttu-id="d93b7-148">Chceme provést aktivaci jenom při událostech komentářů k problémům.</span><span class="sxs-lookup"><span data-stu-id="d93b7-148">We only want to trigger on issue comment events.</span></span>  |
| | <span data-ttu-id="d93b7-149">Komentář k problému</span><span class="sxs-lookup"><span data-stu-id="d93b7-149">Issue comment</span></span> |  |

<span data-ttu-id="d93b7-150">Teď je webhook nakonfigurován tak, aby aktivoval vaši funkci v případě, že dojde k přidání nového komentáře k problému.</span><span class="sxs-lookup"><span data-stu-id="d93b7-150">Now, the webhook is configured to trigger your function when a new issue comment is added.</span></span>

## <a name="test-the-function"></a><span data-ttu-id="d93b7-151">Testování funkce</span><span class="sxs-lookup"><span data-stu-id="d93b7-151">Test the function</span></span>

1. <span data-ttu-id="d93b7-152">Ve vašem úložišti GitHub otevřete kartu **Issues** (Problémy) v novém okně prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="d93b7-152">In your GitHub repository, open the **Issues** tab in a new browser window.</span></span>

1. <span data-ttu-id="d93b7-153">V novém okně klikněte na **New Issue** (Nový problém), zadejte název a klikněte na **Submit new issue** (Odeslat nový problém).</span><span class="sxs-lookup"><span data-stu-id="d93b7-153">In the new window, click **New Issue**, type a title, and then click **Submit new issue**.</span></span>

1. <span data-ttu-id="d93b7-154">U problému zadejte komentář a klikněte na tlačítko **Comment** (Komentář).</span><span class="sxs-lookup"><span data-stu-id="d93b7-154">In the issue, type a comment and click **Comment**.</span></span>

    ![Přidejte komentář k problému Githubu.](./media/functions-create-github-webhook-triggered-function/functions-github-webhook-add-comment.png)

1. <span data-ttu-id="d93b7-156">Vraťte se na portál a podívejte se do protokolů.</span><span class="sxs-lookup"><span data-stu-id="d93b7-156">Go back to the portal and view the logs.</span></span> <span data-ttu-id="d93b7-157">Měli byste vidět položku trasování s textem nového komentáře.</span><span class="sxs-lookup"><span data-stu-id="d93b7-157">You should see a trace entry with the new comment text.</span></span>

     ![Zobrazte si text komentáře v protokolech.](./media/functions-create-github-webhook-triggered-function/function-app-view-logs.png)

## <a name="clean-up-resources"></a><span data-ttu-id="d93b7-159">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="d93b7-159">Clean up resources</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="d93b7-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d93b7-160">Next steps</span></span>

<span data-ttu-id="d93b7-161">Vytvořili jste funkci, která se spustí při přijetí požadavku z webhooku Githubu.</span><span class="sxs-lookup"><span data-stu-id="d93b7-161">You have created a function that runs when a request is received from a GitHub webhook.</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="d93b7-162">Další informace o aktivačních událostech webhooků najdete v tématu [Vazby protokolu HTTP služby Azure Functions a vazby webhooku](functions-bindings-http-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="d93b7-162">For more information about webhook triggers, see [Azure Functions HTTP and webhook bindings](functions-bindings-http-webhook.md).</span></span>
