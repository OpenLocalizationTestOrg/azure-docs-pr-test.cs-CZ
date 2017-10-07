---
title: "aaaCreate první fungovat v Azure pomocí sady Visual Studio | Microsoft Docs"
description: "Vytvoření a publikování jednoduché tooAzure funkce protokolu HTTP aktivované pomocí nástrojů funkce Azure pro sadu Visual Studio."
services: functions
documentationcenter: na
author: rachelappel
manager: erikre
editor: 
tags: 
keywords: azure functions, functions, event processing, compute, serverless architecture
ms.assetid: 82db1177-2295-4e39-bd42-763f6082e796
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 07/05/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 851e5b98dcc2da00334620896a0ea31f566589f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-function-using-visual-studio"></a><span data-ttu-id="96c5a-104">Vytvoření první funkce pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96c5a-104">Create your first function using Visual Studio</span></span>

<span data-ttu-id="96c5a-105">Azure Functions umožňuje spuštění kódu v prostředí bez serveru bez nutnosti toofirst vytvoření virtuálního počítače nebo publikování webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="96c5a-105">Azure Functions lets you execute your code in a serverless environment without having toofirst create a VM or publish a web application.</span></span>

<span data-ttu-id="96c5a-106">V tomto tématu se dozvíte, jak toouse hello Visual Studio 2017 tools pro Azure Functions toocreate a testování místně funkci "hello, world".</span><span class="sxs-lookup"><span data-stu-id="96c5a-106">In this topic, you learn how toouse hello Visual Studio 2017 tools for Azure Functions toocreate and test a "hello world" function locally.</span></span> <span data-ttu-id="96c5a-107">Potom budete publikovat tooAzure kód funkce hello.</span><span class="sxs-lookup"><span data-stu-id="96c5a-107">You will then publish hello function code tooAzure.</span></span> <span data-ttu-id="96c5a-108">Tyto nástroje jsou k dispozici jako součást pracovního vytížení Azure development hello ve Visual Studio 2017 verze 15.3 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="96c5a-108">These tools are available as part of hello Azure development workload in Visual Studio 2017 version 15.3, or a later version.</span></span>

![Kód služby Azure Functions v projektu sady Visual Studio](./media/functions-create-your-first-function-visual-studio/functions-vstools-intro.png)

## <a name="prerequisites"></a><span data-ttu-id="96c5a-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="96c5a-110">Prerequisites</span></span>

<span data-ttu-id="96c5a-111">toocomplete tento kurz, instalace:</span><span class="sxs-lookup"><span data-stu-id="96c5a-111">toocomplete this tutorial, install:</span></span>

* <span data-ttu-id="96c5a-112">[Visual Studio 2017 verze 15.3](https://www.visualstudio.com/vs/preview/), včetně hello **Azure development** zatížení.</span><span class="sxs-lookup"><span data-stu-id="96c5a-112">[Visual Studio 2017 version 15.3](https://www.visualstudio.com/vs/preview/), including hello **Azure development** workload.</span></span>

    ![Nainstalovat Visual Studio 2017 zatížení Azure development hello](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)
    
    >[!NOTE]  
    <span data-ttu-id="96c5a-114">Po instalaci nebo upgradu tooVisual Studio 2017 verze 15.3, budete pravděpodobně potřebovat toomanually aktualizace hello Visual Studio 2017 tools pro Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="96c5a-114">After you install or upgrade tooVisual Studio 2017 version 15.3, you might also need toomanually update hello Visual Studio 2017 tools for Azure Functions.</span></span> <span data-ttu-id="96c5a-115">Můžete je aktualizovat z hello nástrojů hello **nástroje** nabídky v části **rozšíření a aktualizace...**   >  **Aktualizace** > **sady Visual Studio Marketplace** > **Azure Functions a webové úlohy nástroje**  >  **Aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="96c5a-115">You can update hello tools from hello **Tools** menu under **Extensions and Updates...** > **Updates** > **Visual Studio Marketplace** > **Azure Functions and Web Jobs Tools** > **Update**.</span></span> 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 

## <a name="create-an-azure-functions-project-in-visual-studio"></a><span data-ttu-id="96c5a-116">Vytvoření projektu Azure Functions v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96c5a-116">Create an Azure Functions project in Visual Studio</span></span>

[!INCLUDE [Create a project using hello Azure Functions template](../../includes/functions-vstools-create.md)]

<span data-ttu-id="96c5a-117">Teď, když jste vytvořili projekt hello, můžete vytvořit svoji první funkci.</span><span class="sxs-lookup"><span data-stu-id="96c5a-117">Now that you have created hello project, you can create your first function.</span></span>

## <a name="create-hello-function"></a><span data-ttu-id="96c5a-118">Vytvoření funkce hello</span><span class="sxs-lookup"><span data-stu-id="96c5a-118">Create hello function</span></span>

1. <span data-ttu-id="96c5a-119">V **Průzkumníku řešení** klikněte pravým tlačítkem na uzel projektu a vyberte **Přidat** > **Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="96c5a-119">In **Solution Explorer**, right-click on your project node and select **Add** > **New Item**.</span></span> <span data-ttu-id="96c5a-120">Vyberte **Funkce Azure Functions** a klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="96c5a-120">Select **Azure Function** and click **Add**.</span></span>

2. <span data-ttu-id="96c5a-121">Vyberte **HttpTrigger**, zadejte **Název funkce**, jako **Přístupová práva** vyberte **Anonymní** a klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="96c5a-121">Select **HttpTrigger**, type a **Function Name**, select **Anonymous** for **Access Rights**, and click **Create**.</span></span> <span data-ttu-id="96c5a-122">Funkce Hello vytvořit přístup k požadavku HTTP z libovolného klienta.</span><span class="sxs-lookup"><span data-stu-id="96c5a-122">hello function created is accessed by an HTTP request from any client.</span></span> 

    ![Vytvoření nové funkce Azure Functions](./media/functions-create-your-first-function-visual-studio/functions-vstools-add-new-function-2.png)

    <span data-ttu-id="96c5a-124">Soubor kódu se přidá tooyour projekt, který obsahuje třídu, která implementuje funkce kódu.</span><span class="sxs-lookup"><span data-stu-id="96c5a-124">A code file is added tooyour project that contains a class that implements your function code.</span></span> <span data-ttu-id="96c5a-125">Tento kód je založena na šablonu, která přijímá název hodnota a toto využití zpátky.</span><span class="sxs-lookup"><span data-stu-id="96c5a-125">This code is based on a template, which receives a name value and echos it back.</span></span> <span data-ttu-id="96c5a-126">Hello **%{FunctionName/** atribut nastaví hello název funkce.</span><span class="sxs-lookup"><span data-stu-id="96c5a-126">hello **FunctionName** attribute sets hello name of your function.</span></span> <span data-ttu-id="96c5a-127">Hello **HttpTrigger** atribut označuje uvítací zprávu, která aktivuje funkce hello.</span><span class="sxs-lookup"><span data-stu-id="96c5a-127">hello **HttpTrigger** attribute indicates hello message that triggers hello function.</span></span> 

    ![Funkce souboru kódu](./media/functions-create-your-first-function-visual-studio/functions-code-page.png)

<span data-ttu-id="96c5a-129">Teď máte vytvořenou funkci aktivovanou protokolem HTTP a můžete ji otestovat na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="96c5a-129">Now that you have created an HTTP-triggered function, you can test it on your local computer.</span></span>

## <a name="test-hello-function-locally"></a><span data-ttu-id="96c5a-130">Testování funkce hello místně</span><span class="sxs-lookup"><span data-stu-id="96c5a-130">Test hello function locally</span></span>

<span data-ttu-id="96c5a-131">Nástroje Azure Functions Core umožňují spouštět projekt Azure Functions na místním počítači pro vývoj.</span><span class="sxs-lookup"><span data-stu-id="96c5a-131">Azure Functions Core Tools lets you run Azure Functions project on your local development computer.</span></span> <span data-ttu-id="96c5a-132">Jste výzvami tooinstall, které tyto nástroje hello při prvním spuštění funkce ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="96c5a-132">You are prompted tooinstall these tools hello first time you start a function from Visual Studio.</span></span>  

1. <span data-ttu-id="96c5a-133">tootest funkce, stiskněte klávesu F5.</span><span class="sxs-lookup"><span data-stu-id="96c5a-133">tootest your function, press F5.</span></span> <span data-ttu-id="96c5a-134">Pokud se zobrazí výzva, přijímat žádosti o hello ze sady Visual Studio toodownload a nainstalujte nástroje pro základní funkce Azure (CLI).</span><span class="sxs-lookup"><span data-stu-id="96c5a-134">If prompted, accept hello request from Visual Studio toodownload and install Azure Functions Core (CLI) tools.</span></span>  <span data-ttu-id="96c5a-135">Také můžete potřebovat tooenable výjimku brány firewall tak, aby hello nástrojů může zpracovávat požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="96c5a-135">You may also need tooenable a firewall exception so that hello tools can handle HTTP requests.</span></span>

2. <span data-ttu-id="96c5a-136">Kopírování hello URL funkce z modulu runtime Azure Functions hello výstup.</span><span class="sxs-lookup"><span data-stu-id="96c5a-136">Copy hello URL of your function from hello Azure Functions runtime output.</span></span>  

    ![Místní modul runtime Azure](./media/functions-create-your-first-function-visual-studio/functions-vstools-f5.png)

3. <span data-ttu-id="96c5a-138">Vložte hello adresu URL pro požadavek HTTP hello do panelu Adresa prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="96c5a-138">Paste hello URL for hello HTTP request into your browser's address bar.</span></span> <span data-ttu-id="96c5a-139">Připojit řetězec dotazu hello `&name=<yourname>` toothis adresy URL a provedení hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="96c5a-139">Append hello query string `&name=<yourname>` toothis URL and execute hello request.</span></span> <span data-ttu-id="96c5a-140">Následující Hello zobrazuje hello odpovědi v hello prohlížeče toohello místní požadavek GET vrácené funkcí hello:</span><span class="sxs-lookup"><span data-stu-id="96c5a-140">hello following shows hello response in hello browser toohello local GET request returned by hello function:</span></span> 

    ![Funkce localhost odpovědi v prohlížeči hello](./media/functions-create-your-first-function-visual-studio/functions-test-local-browser.png)

4. <span data-ttu-id="96c5a-142">toostop ladění, klikněte na tlačítko hello **Zastavit** tlačítka na panelu nástrojů Visual Studio hello.</span><span class="sxs-lookup"><span data-stu-id="96c5a-142">toostop debugging, click hello **Stop** button on hello Visual Studio toolbar.</span></span>

<span data-ttu-id="96c5a-143">Po ověření, že funkce hello běží správně v místním počítači, je čas toopublish hello projektu tooAzure.</span><span class="sxs-lookup"><span data-stu-id="96c5a-143">After you have verified that hello function runs correctly on your local computer, it's time toopublish hello project tooAzure.</span></span>

## <a name="publish-hello-project-tooazure"></a><span data-ttu-id="96c5a-144">Publikování projektu tooAzure hello</span><span class="sxs-lookup"><span data-stu-id="96c5a-144">Publish hello project tooAzure</span></span>

<span data-ttu-id="96c5a-145">Před publikováním projektu musíte mít v předplatném Azure aplikaci funkcí.</span><span class="sxs-lookup"><span data-stu-id="96c5a-145">You must have a function app in your Azure subscription before you can publish your project.</span></span> <span data-ttu-id="96c5a-146">Aplikaci funkcí můžete vytvořit přímo v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="96c5a-146">You can create a function app right from Visual Studio.</span></span>

[!INCLUDE [Publish hello project tooAzure](../../includes/functions-vstools-publish.md)]

## <a name="test-your-function-in-azure"></a><span data-ttu-id="96c5a-147">Testování funkce v Azure</span><span class="sxs-lookup"><span data-stu-id="96c5a-147">Test your function in Azure</span></span>

1. <span data-ttu-id="96c5a-148">Zkopírujte hello základní adresu URL aplikace hello funkce ze stránky profilu publikování hello.</span><span class="sxs-lookup"><span data-stu-id="96c5a-148">Copy hello base URL of hello function app from hello Publish profile page.</span></span> <span data-ttu-id="96c5a-149">Nahraďte hello `localhost:port` část hello adresy URL, které jste použili při testování hello funkce místně s hello nové základní adresu URL.</span><span class="sxs-lookup"><span data-stu-id="96c5a-149">Replace hello `localhost:port` portion of hello URL you used when testing hello function locally with hello new base URL.</span></span> <span data-ttu-id="96c5a-150">Jako dříve, ujistěte se, řetězce dotazu hello tooappend `&name=<yourname>` toothis adresy URL a provedení hello požadavku.</span><span class="sxs-lookup"><span data-stu-id="96c5a-150">As before, make sure tooappend hello query string `&name=<yourname>` toothis URL and execute hello request.</span></span>

    <span data-ttu-id="96c5a-151">Hello adresu URL, která volá protokolu HTTP aktivované funkce vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="96c5a-151">hello URL that calls your HTTP triggered function looks like this:</span></span>

        http://<functionappname>.azurewebsites.net/api/<functionname>?name=<yourname> 

2. <span data-ttu-id="96c5a-152">Vložte tuto novou adresu URL pro požadavek hello HTTP do panelu Adresa prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="96c5a-152">Paste this new URL for hello HTTP request into your browser's address bar.</span></span> <span data-ttu-id="96c5a-153">Následující Hello zobrazuje hello odpovědi v hello prohlížeče toohello vzdálené požadavek GET vrácené funkcí hello:</span><span class="sxs-lookup"><span data-stu-id="96c5a-153">hello following shows hello response in hello browser toohello remote GET request returned by hello function:</span></span> 

    ![Funkce odpovědi v prohlížeči hello](./media/functions-create-your-first-function-visual-studio/functions-test-remote-browser.png)
 
## <a name="next-steps"></a><span data-ttu-id="96c5a-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="96c5a-155">Next steps</span></span>

<span data-ttu-id="96c5a-156">Použili jste aplikaci funkce sady Visual Studio toocreate C# pomocí jednoduché funkce protokolu HTTP aktivované.</span><span class="sxs-lookup"><span data-stu-id="96c5a-156">You have used Visual Studio toocreate a C# function app with a simple HTTP triggered function.</span></span> 

+ <span data-ttu-id="96c5a-157">toolearn jak tooconfigure vašeho projektu toosupport jiné typy triggerů a vazeb, najdete v části hello [projektu hello konfigurace pro místní vývoj](functions-develop-vs.md#configure-the-project-for-local-development) kapitoly [nástroje funkce Azure pro sadu Visual Studio](functions-develop-vs.md).</span><span class="sxs-lookup"><span data-stu-id="96c5a-157">toolearn how tooconfigure your project toosupport other types of triggers and bindings, see hello [Configure hello project for local development](functions-develop-vs.md#configure-the-project-for-local-development) section in [Azure Functions Tools for Visual Studio](functions-develop-vs.md).</span></span>
+ <span data-ttu-id="96c5a-158">toolearn Další informace o místní testování a ladění pomocí nástroje základní funkce hello Azure, najdete v části [kódu a testování Azure Functions místně](functions-run-local.md).</span><span class="sxs-lookup"><span data-stu-id="96c5a-158">toolearn more about local testing and debugging using hello Azure Functions Core Tools, see [Code and test Azure Functions locally](functions-run-local.md).</span></span> 
+ <span data-ttu-id="96c5a-159">toolearn Další informace o vývoji funkce jako knihovny tříd rozhraní .NET, najdete v části [knihovny tříd pomocí rozhraní .NET s Azure Functions](functions-dotnet-class-library.md).</span><span class="sxs-lookup"><span data-stu-id="96c5a-159">toolearn more about developing functions as .NET class libraries, see [Using .NET class libraries with Azure Functions](functions-dotnet-class-library.md).</span></span> 

