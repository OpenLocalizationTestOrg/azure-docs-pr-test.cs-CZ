---
title: "Vytvoření první funkce na webu Azure Portal | Dokumentace Microsoftu"
description: "Naučíte se postup vytvoření první funkce Azure Function pro provádění pomocí webu Azure Portal bez serveru."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 96cf87b9-8db6-41a8-863a-abb828e3d06d
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/07/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 3ec1f278f21d89782137625aff200f07f15fd9fb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-your-first-function-in-the-azure-portal"></a><span data-ttu-id="ed94f-103">Vytvoření první funkce na webu Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ed94f-103">Create your first function in the Azure portal</span></span>

<span data-ttu-id="ed94f-104">Služba Azure Functions umožňuje spuštění kódu v prostředí bez serveru, aniž byste nejdřív museli vytvořit virtuální počítač nebo publikovat webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ed94f-104">Azure Functions lets you execute your code in a serverless environment without having to first create a VM or publish a web application.</span></span> <span data-ttu-id="ed94f-105">V tomto tématu se dozvíte, jak pomocí služby Functions vytvořit na webu Azure Portal funkci Hello World.</span><span class="sxs-lookup"><span data-stu-id="ed94f-105">In this topic, learn how to use Functions to create a "hello world" function in the Azure portal.</span></span>

![Vytvoření aplikace Function App na webu Azure Portal](./media/functions-create-first-azure-function/function-app-in-portal-editor.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a><span data-ttu-id="ed94f-107">Přihlaste se k Azure.</span><span class="sxs-lookup"><span data-stu-id="ed94f-107">Log in to Azure</span></span>

<span data-ttu-id="ed94f-108">Přihlaste se k portálu [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="ed94f-108">Log in to the [Azure portal](https://portal.azure.com/).</span></span>

## <a name="create-a-function-app"></a><span data-ttu-id="ed94f-109">Vytvoření Function App</span><span class="sxs-lookup"><span data-stu-id="ed94f-109">Create a function app</span></span>

<span data-ttu-id="ed94f-110">K hostování provádění funkcí musíte mít aplikaci Function App.</span><span class="sxs-lookup"><span data-stu-id="ed94f-110">You must have a function app to host the execution of your functions.</span></span> <span data-ttu-id="ed94f-111">Aplikace Function App umožňuje seskupit funkce jako logickou jednotku pro snadnější správu, nasazování a sdílení prostředků.</span><span class="sxs-lookup"><span data-stu-id="ed94f-111">A function app lets you group functions as a logic unit for easier management, deployment, and sharing of resources.</span></span> 

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

<span data-ttu-id="ed94f-112">Dál vytvoříte v nové aplikaci Function App funkci.</span><span class="sxs-lookup"><span data-stu-id="ed94f-112">Next, you create a function in the new function app.</span></span>

## <span data-ttu-id="ed94f-113"><a name="create-function"></a>Vytvoření funkce aktivované protokolem HTTP</span><span class="sxs-lookup"><span data-stu-id="ed94f-113"><a name="create-function"></a>Create an HTTP triggered function</span></span>

1. <span data-ttu-id="ed94f-114">Rozbalte novou aplikaci Function App a potom klikněte na tlačítko  **+**  vedle položky **Funkce**.</span><span class="sxs-lookup"><span data-stu-id="ed94f-114">Expand your new function app, then click the **+** button next to **Functions**.</span></span>

2.  <span data-ttu-id="ed94f-115">Na stránce **Začněte rychle** vyberte **Webhook + API**, **Zvolte jazyk** funkce a klikněte na **Vytvořit tuto funkci**.</span><span class="sxs-lookup"><span data-stu-id="ed94f-115">In the **Get started quickly** page, select **WebHook + API**, **Choose a language** for your function, and click **Create this function**.</span></span> 
   
    ![Stručný úvod do služby Functions na webu Azure Portal.](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)

<span data-ttu-id="ed94f-117">Funkce ve vybraném jazyce se vytvoří pomocí šablony funkce aktivované protokolem HTTP.</span><span class="sxs-lookup"><span data-stu-id="ed94f-117">A function is created in your chosen language using the template for an HTTP triggered function.</span></span> <span data-ttu-id="ed94f-118">Novou funkci můžete spustit odesláním požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="ed94f-118">You can run the new function by sending an HTTP request.</span></span>

## <a name="test-the-function"></a><span data-ttu-id="ed94f-119">Testování funkce</span><span class="sxs-lookup"><span data-stu-id="ed94f-119">Test the function</span></span>

1. <span data-ttu-id="ed94f-120">V nové funkci klikněte na **</> Získat adresu URL funkce**, vyberte **výchozí (klíč funkce)** a pak na **Kopírovat**.</span><span class="sxs-lookup"><span data-stu-id="ed94f-120">In your new function, click **</> Get function URL**, select **default (Function key)**, and then click **Copy**.</span></span> 

    ![Kopírování adresy URL funkce z webu Azure Portal](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

2. <span data-ttu-id="ed94f-122">Vložte adresu URL funkce do panelu Adresa vašeho prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="ed94f-122">Paste the function URL into your browser's address bar.</span></span> <span data-ttu-id="ed94f-123">Připojte k této adrese URL řetězec dotazu `&name=<yourname>` a stisknutím klávesy `Enter` na klávesnici požadavek proveďte.</span><span class="sxs-lookup"><span data-stu-id="ed94f-123">Append the query string `&name=<yourname>` to this URL and press the `Enter` key on your keyboard to execute the request.</span></span> <span data-ttu-id="ed94f-124">Následuje příklad odpovědi vrácené funkcí v prohlížeči Edge:</span><span class="sxs-lookup"><span data-stu-id="ed94f-124">The following is an example of the response returned by the function in the Edge browser:</span></span>

    ![Odezva funkce v prohlížeči.](./media/functions-create-first-azure-function/function-app-browser-testing.png)

    <span data-ttu-id="ed94f-126">Adresa URL požadavku obsahuje klíč, který je ve výchozím nastavení nezbytný pro přístup k funkci přes protokol HTTP.</span><span class="sxs-lookup"><span data-stu-id="ed94f-126">The request URL includes a key that is required, by default, to access your function over HTTP.</span></span>   

3. <span data-ttu-id="ed94f-127">Při spuštění funkce se do protokolů zaznamenávají informace o trasování.</span><span class="sxs-lookup"><span data-stu-id="ed94f-127">When your function runs, trace information is written to the logs.</span></span> <span data-ttu-id="ed94f-128">Pokud chcete zobrazit výstup trasování z předchozího zpracování, vraťte se na funkce na portálu a kliknutím na šipku nahoru ve spodní části obrazovky rozbalte položku **Protokoly**.</span><span class="sxs-lookup"><span data-stu-id="ed94f-128">To see the trace output from the previous execution, return to your function in the portal and click the up arrow at the bottom of the screen to expand **Logs**.</span></span> 

   ![Prohlížeč protokolu funkcí na webu Azure Portal.](./media/functions-create-first-azure-function/function-view-logs.png)

## <a name="clean-up-resources"></a><span data-ttu-id="ed94f-130">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="ed94f-130">Clean up resources</span></span>

[!INCLUDE [Clean up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="ed94f-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ed94f-131">Next steps</span></span>

<span data-ttu-id="ed94f-132">Vytvořili jste aplikaci Function App s jednoduchou funkcí aktivovanou protokolem HTTP.</span><span class="sxs-lookup"><span data-stu-id="ed94f-132">You have created a function app with a simple HTTP triggered function.</span></span>  

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="ed94f-133">Další informace najdete v tématu [Vazby protokolu HTTP služby Azure Functions a vazby webhooku](functions-bindings-http-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="ed94f-133">For more information, see [Azure Functions HTTP and webhook bindings](functions-bindings-http-webhook.md).</span></span>



