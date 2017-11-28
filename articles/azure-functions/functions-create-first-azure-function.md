---
title: "aaaCreate první funkce z hello portálu Azure | Microsoft Docs"
description: "Zjistěte, jak hello toocreate vaše první Azure fungovat pro provádění bez serveru pomocí portálu Azure."
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
ms.openlocfilehash: 84283d7d4bc6015061946af4589f9a70ae61f36b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-function-in-hello-azure-portal"></a><span data-ttu-id="8131b-103">Vytvoření první funkce v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8131b-103">Create your first function in hello Azure portal</span></span>

<span data-ttu-id="8131b-104">Azure Functions umožňuje spuštění kódu v prostředí bez serveru bez nutnosti toofirst vytvoření virtuálního počítače nebo publikování webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="8131b-104">Azure Functions lets you execute your code in a serverless environment without having toofirst create a VM or publish a web application.</span></span> <span data-ttu-id="8131b-105">V tomto tématu se dozvíte, jak toouse funkce toocreate funkci "hello, world" v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8131b-105">In this topic, learn how toouse Functions toocreate a "hello world" function in hello Azure portal.</span></span>

![Vytvoření aplikace pro funkce v hello portálu Azure](./media/functions-create-first-azure-function/function-app-in-portal-editor.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-tooazure"></a><span data-ttu-id="8131b-107">Přihlaste se tooAzure</span><span class="sxs-lookup"><span data-stu-id="8131b-107">Log in tooAzure</span></span>

<span data-ttu-id="8131b-108">Přihlaste se toohello [portál Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="8131b-108">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>

## <a name="create-a-function-app"></a><span data-ttu-id="8131b-109">Vytvoření Function App</span><span class="sxs-lookup"><span data-stu-id="8131b-109">Create a function app</span></span>

<span data-ttu-id="8131b-110">Musíte mít funkce aplikace toohost hello provádění funkcí.</span><span class="sxs-lookup"><span data-stu-id="8131b-110">You must have a function app toohost hello execution of your functions.</span></span> <span data-ttu-id="8131b-111">Aplikace Function App umožňuje seskupit funkce jako logickou jednotku pro snadnější správu, nasazování a sdílení prostředků.</span><span class="sxs-lookup"><span data-stu-id="8131b-111">A function app lets you group functions as a logic unit for easier management, deployment, and sharing of resources.</span></span> 

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

<span data-ttu-id="8131b-112">Dál vytvořte funkci v nové funkce aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="8131b-112">Next, you create a function in hello new function app.</span></span>

## <span data-ttu-id="8131b-113"><a name="create-function"></a>Vytvoření funkce aktivované protokolem HTTP</span><span class="sxs-lookup"><span data-stu-id="8131b-113"><a name="create-function"></a>Create an HTTP triggered function</span></span>

1. <span data-ttu-id="8131b-114">Rozbalte nové funkce aplikace, a poté klikněte na tlačítko hello  **+**  tlačítko vedle příliš**funkce**.</span><span class="sxs-lookup"><span data-stu-id="8131b-114">Expand your new function app, then click hello **+** button next too**Functions**.</span></span>

2.  <span data-ttu-id="8131b-115">V hello **rychle začít** vyberte **WebHook + API**, **vybrat jazyk** pro svou funkci a klikněte na **vytvořit tuto funkci** .</span><span class="sxs-lookup"><span data-stu-id="8131b-115">In hello **Get started quickly** page, select **WebHook + API**, **Choose a language** for your function, and click **Create this function**.</span></span> 
   
    ![Funkce Rychlé spuštění v hello portálu Azure.](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)

<span data-ttu-id="8131b-117">Funkce je vytvořená ve zvoleného jazyka pomocí hello šablony pro HTTP aktivované funkce.</span><span class="sxs-lookup"><span data-stu-id="8131b-117">A function is created in your chosen language using hello template for an HTTP triggered function.</span></span> <span data-ttu-id="8131b-118">Můžete spustit novou funkci hello odesláním požadavku HTTP.</span><span class="sxs-lookup"><span data-stu-id="8131b-118">You can run hello new function by sending an HTTP request.</span></span>

## <a name="test-hello-function"></a><span data-ttu-id="8131b-119">Testování funkce hello</span><span class="sxs-lookup"><span data-stu-id="8131b-119">Test hello function</span></span>

1. <span data-ttu-id="8131b-120">V nové funkci klikněte na **</> Získat adresu URL funkce**, vyberte **výchozí (klíč funkce)** a pak na **Kopírovat**.</span><span class="sxs-lookup"><span data-stu-id="8131b-120">In your new function, click **</> Get function URL**, select **default (Function key)**, and then click **Copy**.</span></span> 

    ![Zkopírujte adresu URL funkce hello z hello portálu Azure](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

2. <span data-ttu-id="8131b-122">Vložte adresu URL funkce hello do panelu Adresa prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="8131b-122">Paste hello function URL into your browser's address bar.</span></span> <span data-ttu-id="8131b-123">Připojit řetězec dotazu hello `&name=<yourname>` toothis adresy URL a stiskněte klávesu hello `Enter` klíče na vaši žádost hello tooexecute klávesnice.</span><span class="sxs-lookup"><span data-stu-id="8131b-123">Append hello query string `&name=<yourname>` toothis URL and press hello `Enter` key on your keyboard tooexecute hello request.</span></span> <span data-ttu-id="8131b-124">Hello tady je příklad hello odpovědi vrácené funkcí hello v prohlížeči Edge hello:</span><span class="sxs-lookup"><span data-stu-id="8131b-124">hello following is an example of hello response returned by hello function in hello Edge browser:</span></span>

    ![Funkce odpověď v prohlížeči hello.](./media/functions-create-first-azure-function/function-app-browser-testing.png)

    <span data-ttu-id="8131b-126">požadavek Hello adresa URL obsahuje klíč, který je ve výchozím nastavení, tooaccess požadována funkce prostřednictvím protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="8131b-126">hello request URL includes a key that is required, by default, tooaccess your function over HTTP.</span></span>   

3. <span data-ttu-id="8131b-127">Když je funkce spuštěná, se zapisují informace o trasování toohello protokoly.</span><span class="sxs-lookup"><span data-stu-id="8131b-127">When your function runs, trace information is written toohello logs.</span></span> <span data-ttu-id="8131b-128">toosee hello výstup trasování z předchozí zpracování hello, vrátí funkce tooyour hello portálu a klikněte na tlačítko hello v hello dolní části obrazovky tooexpand hello šipka nahoru **protokoly**.</span><span class="sxs-lookup"><span data-stu-id="8131b-128">toosee hello trace output from hello previous execution, return tooyour function in hello portal and click hello up arrow at hello bottom of hello screen tooexpand **Logs**.</span></span> 

   ![Funkce přihlášení prohlížeč hello portálu Azure.](./media/functions-create-first-azure-function/function-view-logs.png)

## <a name="clean-up-resources"></a><span data-ttu-id="8131b-130">Vyčištění prostředků</span><span class="sxs-lookup"><span data-stu-id="8131b-130">Clean up resources</span></span>

[!INCLUDE [Clean up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a><span data-ttu-id="8131b-131">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8131b-131">Next steps</span></span>

<span data-ttu-id="8131b-132">Vytvořili jste aplikaci Function App s jednoduchou funkcí aktivovanou protokolem HTTP.</span><span class="sxs-lookup"><span data-stu-id="8131b-132">You have created a function app with a simple HTTP triggered function.</span></span>  

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

<span data-ttu-id="8131b-133">Další informace najdete v tématu [Vazby protokolu HTTP služby Azure Functions a vazby webhooku](functions-bindings-http-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="8131b-133">For more information, see [Azure Functions HTTP and webhook bindings](functions-bindings-http-webhook.md).</span></span>



