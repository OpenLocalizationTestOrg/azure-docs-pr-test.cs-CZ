---
title: "aaaCreate funkce v Azure aktivovány webhook Githubu | Microsoft Docs"
description: "Pomocí Azure Functions toocreate bez serveru funkci, která je vyvolat webhook Githubu."
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
ms.openlocfilehash: 8ffcde82c9310d749159ed53eab113658e38a030
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-function-triggered-by-a-github-webhook"></a><span data-ttu-id="67945-103">Vytvoření funkce aktivované webhookem GitHubu</span><span class="sxs-lookup"><span data-stu-id="67945-103">Create a function triggered by a GitHub webhook</span></span>

<span data-ttu-id="67945-104">Zjistěte, jak toocreate funkci, která se aktivuje požadavkem HTTP webhooku s datovou část specifický pro GitHub.</span><span class="sxs-lookup"><span data-stu-id="67945-104">Learn how toocreate a function that is triggered by an HTTP webhook request with a GitHub-specific payload.</span></span>

![Webhook Githubu aktivuje funkce v hello portálu Azure](./media/functions-create-github-webhook-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a><span data-ttu-id="67945-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="67945-106">Prerequisites</span></span>

+ <span data-ttu-id="67945-107">Účet GitHubu s alespoň jedním projektem.</span><span class="sxs-lookup"><span data-stu-id="67945-107">A GitHub account with at least one project.</span></span>
+ <span data-ttu-id="67945-108">Předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="67945-108">An Azure subscription.</span></span> <span data-ttu-id="67945-109">Pokud ho nemáte, než začnete, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="67945-109">If you don't have one, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a><span data-ttu-id="67945-110">Vytvoření aplikace Azure Function App</span><span class="sxs-lookup"><span data-stu-id="67945-110">Create an Azure Function app</span></span>

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Aplikace Function App byla úspěšně vytvořena.](./media/functions-create-first-azure-function/function-app-create-success.png)

<span data-ttu-id="67945-112">Dál vytvořte funkci v nové funkce aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="67945-112">Next, you create a function in hello new function app.</span></span>

<a name="create-function"></a>

## <a name="create-a-github-webhook-triggered-function"></a><span data-ttu-id="67945-113">Vytvoření funkce aktivované webhookem GitHubu</span><span class="sxs-lookup"><span data-stu-id="67945-113">Create a GitHub webhook triggered function</span></span>

1. <span data-ttu-id="67945-114">Rozšířit funkce aplikace a klikněte na tlačítko hello  **+**  tlačítko vedle příliš**funkce**.</span><span class="sxs-lookup"><span data-stu-id="67945-114">Expand your function app and click hello **+** button next too**Functions**.</span></span> <span data-ttu-id="67945-115">Pokud je to první funkce hello ve vaší aplikaci funkce, vyberte **vlastní funkce**.</span><span class="sxs-lookup"><span data-stu-id="67945-115">If this is hello first function in your function app, select **Custom function**.</span></span> <span data-ttu-id="67945-116">Zobrazí se hello kompletní sada šablon funkcí.</span><span class="sxs-lookup"><span data-stu-id="67945-116">This displays hello complete set of function templates.</span></span>

    ![Funkce Rychlé spuštění stránku hello portálu Azure](./media/functions-create-github-webhook-triggered-function/add-first-function.png)

2. <span data-ttu-id="67945-118">Vyberte hello **WebHook Githubu** šablonu pro požadovaný jazyk.</span><span class="sxs-lookup"><span data-stu-id="67945-118">Select hello **GitHub WebHook** template for your desired language.</span></span> <span data-ttu-id="67945-119">**Pojmenujte funkci** a pak vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="67945-119">**Name your function**, then select **Create**.</span></span>

     ![Vytvoření webhooku se aktivuje funkce Githubu v hello portálu Azure](./media/functions-create-github-webhook-triggered-function/functions-create-github-webhook-trigger.png) 

3. <span data-ttu-id="67945-121">V nové funkce, klikněte na **URL funkce <> / Get**, zkopírujte a uložte hello hodnoty.</span><span class="sxs-lookup"><span data-stu-id="67945-121">In your new function, click **</> Get function URL**, then copy and save hello values.</span></span> <span data-ttu-id="67945-122">Hello samé pro **tajný klíč <> / získat Githubu**.</span><span class="sxs-lookup"><span data-stu-id="67945-122">Do hello same thing for **</> Get GitHub secret**.</span></span> <span data-ttu-id="67945-123">Použijte tyto hodnoty tooconfigure hello webhooku v Githubu.</span><span class="sxs-lookup"><span data-stu-id="67945-123">You use these values tooconfigure hello webhook in GitHub.</span></span>

    ![Zkontrolujte kód funkce hello](./media/functions-create-github-webhook-triggered-function/functions-copy-function-url-github-secret.png)

<span data-ttu-id="67945-125">Dále vytvoříte webhook ve vašem úložišti GitHub.</span><span class="sxs-lookup"><span data-stu-id="67945-125">Next, you create a webhook in your GitHub repository.</span></span>

## <a name="configure-hello-webhook"></a><span data-ttu-id="67945-126">Konfigurace webhooku hello</span><span class="sxs-lookup"><span data-stu-id="67945-126">Configure hello webhook</span></span>

1. <span data-ttu-id="67945-127">V Githubu přejděte tooa úložiště, který vlastníte.</span><span class="sxs-lookup"><span data-stu-id="67945-127">In GitHub, navigate tooa repository that you own.</span></span> <span data-ttu-id="67945-128">Můžete také použít libovolné úložiště, které máte rozvětvené.</span><span class="sxs-lookup"><span data-stu-id="67945-128">You can also use any repository that you have forked.</span></span> <span data-ttu-id="67945-129">Pokud potřebujete toofork úložiště, použijte <https://github.com/Azure-Samples/functions-quickstart>.</span><span class="sxs-lookup"><span data-stu-id="67945-129">If you need toofork a repository, use <https://github.com/Azure-Samples/functions-quickstart>.</span></span>

1. <span data-ttu-id="67945-130">Klikněte na **Settings** (Nastavení), pak na **Webhooks** (Webhooky) a **Add webhook** (Přidat webhook).</span><span class="sxs-lookup"><span data-stu-id="67945-130">Click **Settings**, then click **Webhooks**, and  **Add webhook**.</span></span>

    ![Přidání webhooku GitHubu](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-2.png)

1. <span data-ttu-id="67945-132">Použití nastavení uvedeného v tabulce hello a pak klikněte na **přidat webhooku**.</span><span class="sxs-lookup"><span data-stu-id="67945-132">Use settings as specified in hello table, then click **Add webhook**.</span></span>

    ![Adresa URL webhooku hello sady a tajný klíč](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-3.png)

| <span data-ttu-id="67945-134">Nastavení</span><span class="sxs-lookup"><span data-stu-id="67945-134">Setting</span></span> | <span data-ttu-id="67945-135">Navrhovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="67945-135">Suggested value</span></span> | <span data-ttu-id="67945-136">Popis</span><span class="sxs-lookup"><span data-stu-id="67945-136">Description</span></span> |
|---|---|---|
| <span data-ttu-id="67945-137">**Datová část adresy URL**</span><span class="sxs-lookup"><span data-stu-id="67945-137">**Payload URL**</span></span> | <span data-ttu-id="67945-138">Zkopírovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="67945-138">Copied value</span></span> | <span data-ttu-id="67945-139">Použít hello hodnoty vrácené **URL funkce <> / Get**.</span><span class="sxs-lookup"><span data-stu-id="67945-139">Use hello value returned by  **</> Get function URL**.</span></span> |
| <span data-ttu-id="67945-140">**Tajný kód**</span><span class="sxs-lookup"><span data-stu-id="67945-140">**Secret**</span></span>   | <span data-ttu-id="67945-141">Zkopírovaná hodnota</span><span class="sxs-lookup"><span data-stu-id="67945-141">Copied value</span></span> | <span data-ttu-id="67945-142">Použít hello hodnoty vrácené **tajný klíč <> / získat Githubu**.</span><span class="sxs-lookup"><span data-stu-id="67945-142">Use hello value returned by  **</> Get GitHub secret**.</span></span> |
| <span data-ttu-id="67945-143">**Typ obsahu**</span><span class="sxs-lookup"><span data-stu-id="67945-143">**Content type**</span></span> | <span data-ttu-id="67945-144">application/json</span><span class="sxs-lookup"><span data-stu-id="67945-144">application/json</span></span> | <span data-ttu-id="67945-145">Funkce Hello očekává datové části JSON.</span><span class="sxs-lookup"><span data-stu-id="67945-145">hello function expects a JSON payload.</span></span> |
| <span data-ttu-id="67945-146">Aktivační události</span><span class="sxs-lookup"><span data-stu-id="67945-146">Event triggers</span></span> | <span data-ttu-id="67945-147">Nechat mě vybrat jednotlivé události</span><span class="sxs-lookup"><span data-stu-id="67945-147">Let me select individual events</span></span> | <span data-ttu-id="67945-148">Chceme jenom tootrigger na události komentář problém.</span><span class="sxs-lookup"><span data-stu-id="67945-148">We only want tootrigger on issue comment events.</span></span>  |
| | <span data-ttu-id="67945-149">Komentář k problému</span><span class="sxs-lookup"><span data-stu-id="67945-149">Issue comment</span></span> |  |

<span data-ttu-id="67945-150">Nyní, hello webhooku je funkce nakonfigurovaná tootrigger při přidání nové komentář problém.</span><span class="sxs-lookup"><span data-stu-id="67945-150">Now, hello webhook is configured tootrigger your function when a new issue comment is added.</span></span>

## <a name="test-hello-function"></a><span data-ttu-id="67945-151">Testování funkce hello</span><span class="sxs-lookup"><span data-stu-id="67945-151">Test hello function</span></span>

1. <span data-ttu-id="67945-152">V úložišti GitHub, otevřete hello **problémy** ve nové okno prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="67945-152">In your GitHub repository, open hello **Issues** tab in a new browser window.</span></span>

1. <span data-ttu-id="67945-153">V novém okně hello, klikněte na tlačítko **nový problém**, zadejte název a potom klikněte na **odeslání nové problém**.</span><span class="sxs-lookup"><span data-stu-id="67945-153">In hello new window, click **New Issue**, type a title, and then click **Submit new issue**.</span></span>

1. <span data-ttu-id="67945-154">V hello problém, zadejte komentář a klikněte na tlačítko **komentář**.</span><span class="sxs-lookup"><span data-stu-id="67945-154">In hello issue, type a comment and click **Comment**.</span></span>

    ![Přidejte komentář k problému Githubu.](./media/functions-create-github-webhook-triggered-function/functions-github-webhook-add-comment.png)

1. <span data-ttu-id="67945-156">Přejděte zpět toohello portál a zobrazit protokoly hello.</span><span class="sxs-lookup"><span data-stu-id="67945-156">Go back toohello portal and view hello logs.</span></span> <span data-ttu-id="67945-157">Měli byste vidět položku trasování s nový komentář textem hello.</span><span class="sxs-lookup"><span data-stu-id="67945-157">You should see a trace entry with hello new comment text.</span></span>

     ![Zobrazit text komentáře hello v protokolech hello.](./media/functions-create-github-webhook-triggered-function/function-app-view-logs.png)

## <a name="clean-up-resources"></a><span data-ttu-id="67945-159">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="67945-159">Clean up resources</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="67945-160">Další kroky</span><span class="sxs-lookup"><span data-stu-id="67945-160">Next steps</span></span>

<span data-ttu-id="67945-161">Vytvořili jste funkci, která se spustí při přijetí požadavku z webhooku Githubu.</span><span class="sxs-lookup"><span data-stu-id="67945-161">You have created a function that runs when a request is received from a GitHub webhook.</span></span>

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="67945-162">Další informace o aktivačních událostech webhooků najdete v tématu [Vazby protokolu HTTP služby Azure Functions a vazby webhooku](functions-bindings-http-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="67945-162">For more information about webhook triggers, see [Azure Functions HTTP and webhook bindings](functions-bindings-http-webhook.md).</span></span>
