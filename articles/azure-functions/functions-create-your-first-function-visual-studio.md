---
title: "Vytvoření první funkce v Azure pomocí sady Visual Studio | Dokumentace Microsoftu"
description: "Vytvořte a publikujte do Azure jednoduchou funkci aktivovanou protokolem HTTP pomocí Azure Functions Tools for Visual Studio."
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
ms.openlocfilehash: 04370558725d76ffe83d8aaf5d16c88fd2803ba9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-your-first-function-using-visual-studio"></a><span data-ttu-id="4f9b8-104">Vytvoření první funkce pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f9b8-104">Create your first function using Visual Studio</span></span>

<span data-ttu-id="4f9b8-105">Služba Azure Functions umožňuje spuštění kódu v prostředí bez serveru, aniž byste nejdřív museli vytvořit virtuální počítač nebo publikovat webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-105">Azure Functions lets you execute your code in a serverless environment without having to first create a VM or publish a web application.</span></span>

<span data-ttu-id="4f9b8-106">V tomto tématu se dozvíte, jak používat nástroje Visual Studio 2017 pro Azure Functions k vytvoření a otestování místně funkci "hello, world".</span><span class="sxs-lookup"><span data-stu-id="4f9b8-106">In this topic, you learn how to use the Visual Studio 2017 tools for Azure Functions to create and test a "hello world" function locally.</span></span> <span data-ttu-id="4f9b8-107">Kód funkce potom budete publikovat do Azure.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-107">You will then publish the function code to Azure.</span></span> <span data-ttu-id="4f9b8-108">Tyto nástroje jsou dostupné jako součást sady funkcí Vývoj pro Azure v sadě Visual Studio 2017 verze 15.3 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-108">These tools are available as part of the Azure development workload in Visual Studio 2017 version 15.3, or a later version.</span></span>

![Kód služby Azure Functions v projektu sady Visual Studio](./media/functions-create-your-first-function-visual-studio/functions-vstools-intro.png)

## <a name="prerequisites"></a><span data-ttu-id="4f9b8-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4f9b8-110">Prerequisites</span></span>

<span data-ttu-id="4f9b8-111">Pro absolvování tohoto kurzu nainstalujte:</span><span class="sxs-lookup"><span data-stu-id="4f9b8-111">To complete this tutorial, install:</span></span>

* <span data-ttu-id="4f9b8-112">[Visual Studio 2017 verze 15.3](https://www.visualstudio.com/vs/preview/), včetně sady funkcí **Vývoj pro Azure**.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-112">[Visual Studio 2017 version 15.3](https://www.visualstudio.com/vs/preview/), including the **Azure development** workload.</span></span>

    ![Instalace sady Visual Studio 2017 se sadou funkcí Vývoj pro Azure](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)
    
    >[!NOTE]  
    <span data-ttu-id="4f9b8-114">Po instalaci nebo upgradu na Visual Studio 2017 verze 15.3, budete také muset ručně aktualizovat nástroje Visual Studio 2017 pro Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-114">After you install or upgrade to Visual Studio 2017 version 15.3, you might also need to manually update the Visual Studio 2017 tools for Azure Functions.</span></span> <span data-ttu-id="4f9b8-115">Můžete je aktualizovat z nástrojů **nástroje** nabídky v části **rozšíření a aktualizace...**   >  **Aktualizace** > **sady Visual Studio Marketplace** > **Azure Functions a webové úlohy nástroje**  >  **Aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-115">You can update the tools from the **Tools** menu under **Extensions and Updates...** > **Updates** > **Visual Studio Marketplace** > **Azure Functions and Web Jobs Tools** > **Update**.</span></span> 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 

## <a name="create-an-azure-functions-project-in-visual-studio"></a><span data-ttu-id="4f9b8-116">Vytvoření projektu Azure Functions v sadě Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f9b8-116">Create an Azure Functions project in Visual Studio</span></span>

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

<span data-ttu-id="4f9b8-117">Teď, když jste vytvořili projekt, můžete vytvořit svou první funkci.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-117">Now that you have created the project, you can create your first function.</span></span>

## <a name="create-the-function"></a><span data-ttu-id="4f9b8-118">Vytvoření funkce</span><span class="sxs-lookup"><span data-stu-id="4f9b8-118">Create the function</span></span>

1. <span data-ttu-id="4f9b8-119">V **Průzkumníku řešení** klikněte pravým tlačítkem na uzel projektu a vyberte **Přidat** > **Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-119">In **Solution Explorer**, right-click on your project node and select **Add** > **New Item**.</span></span> <span data-ttu-id="4f9b8-120">Vyberte **Funkce Azure Functions** a klikněte na **Přidat**.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-120">Select **Azure Function** and click **Add**.</span></span>

2. <span data-ttu-id="4f9b8-121">Vyberte **HttpTrigger**, zadejte **Název funkce**, jako **Přístupová práva** vyberte **Anonymní** a klikněte na **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-121">Select **HttpTrigger**, type a **Function Name**, select **Anonymous** for **Access Rights**, and click **Create**.</span></span> <span data-ttu-id="4f9b8-122">Vytvořená funkce je přístupná prostřednictvím požadavku HTTP z jakéhokoli klienta.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-122">The function created is accessed by an HTTP request from any client.</span></span> 

    ![Vytvoření nové funkce Azure Functions](./media/functions-create-your-first-function-visual-studio/functions-vstools-add-new-function-2.png)

    <span data-ttu-id="4f9b8-124">Soubor kódu se přidá do projektu, který obsahuje třídu, která implementuje funkce kódu.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-124">A code file is added to your project that contains a class that implements your function code.</span></span> <span data-ttu-id="4f9b8-125">Tento kód je založena na šablonu, která přijímá název hodnota a toto využití zpátky.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-125">This code is based on a template, which receives a name value and echos it back.</span></span> <span data-ttu-id="4f9b8-126">**%{FunctionName/** atribut nastaví název funkce.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-126">The **FunctionName** attribute sets the name of your function.</span></span> <span data-ttu-id="4f9b8-127">**HttpTrigger** atribut určuje zprávu, která aktivuje funkce.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-127">The **HttpTrigger** attribute indicates the message that triggers the function.</span></span> 

    ![Funkce souboru kódu](./media/functions-create-your-first-function-visual-studio/functions-code-page.png)

<span data-ttu-id="4f9b8-129">Teď máte vytvořenou funkci aktivovanou protokolem HTTP a můžete ji otestovat na místním počítači.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-129">Now that you have created an HTTP-triggered function, you can test it on your local computer.</span></span>

## <a name="test-the-function-locally"></a><span data-ttu-id="4f9b8-130">Místní testování funkce</span><span class="sxs-lookup"><span data-stu-id="4f9b8-130">Test the function locally</span></span>

<span data-ttu-id="4f9b8-131">Nástroje Azure Functions Core umožňují spouštět projekt Azure Functions na místním počítači pro vývoj.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-131">Azure Functions Core Tools lets you run Azure Functions project on your local development computer.</span></span> <span data-ttu-id="4f9b8-132">K instalaci těchto nástrojů budete vyzváni při prvním spuštění funkce ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-132">You are prompted to install these tools the first time you start a function from Visual Studio.</span></span>  

1. <span data-ttu-id="4f9b8-133">Pokud chcete funkci otestovat, stiskněte F5.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-133">To test your function, press F5.</span></span> <span data-ttu-id="4f9b8-134">Po výzvě přijměte požadavek ze sady Visual Studio na stažení a instalaci nástrojů Azure Functions Core (CLI).</span><span class="sxs-lookup"><span data-stu-id="4f9b8-134">If prompted, accept the request from Visual Studio to download and install Azure Functions Core (CLI) tools.</span></span>  <span data-ttu-id="4f9b8-135">Může být také potřeba povolit výjimku brány firewall, aby nástroje mohly zpracovávat požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-135">You may also need to enable a firewall exception so that the tools can handle HTTP requests.</span></span>

2. <span data-ttu-id="4f9b8-136">Zkopírujte adresu URL vaší funkce z výstupu modulu runtime služby Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-136">Copy the URL of your function from the Azure Functions runtime output.</span></span>  

    ![Místní modul runtime Azure](./media/functions-create-your-first-function-visual-studio/functions-vstools-f5.png)

3. <span data-ttu-id="4f9b8-138">Vložte adresu URL pro požadavek HTTP do panelu adresy prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-138">Paste the URL for the HTTP request into your browser's address bar.</span></span> <span data-ttu-id="4f9b8-139">K této adrese URL připojte řetězec dotazu `&name=<yourname>` a proveďte požadavek.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-139">Append the query string `&name=<yourname>` to this URL and execute the request.</span></span> <span data-ttu-id="4f9b8-140">Následuje ukázka odezvy na místní požadavek GET vrácené funkcí v prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="4f9b8-140">The following shows the response in the browser to the local GET request returned by the function:</span></span> 

    ![Odezva místního hostitele funkce v prohlížeči](./media/functions-create-your-first-function-visual-studio/functions-test-local-browser.png)

4. <span data-ttu-id="4f9b8-142">Pokud chcete zastavit ladění, klikněte na tlačítko **Zastavit** na panelu nástrojů sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-142">To stop debugging, click the **Stop** button on the Visual Studio toolbar.</span></span>

<span data-ttu-id="4f9b8-143">Po ověření správného fungování funkce na místním počítači je na čase publikovat projekt do Azure.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-143">After you have verified that the function runs correctly on your local computer, it's time to publish the project to Azure.</span></span>

## <a name="publish-the-project-to-azure"></a><span data-ttu-id="4f9b8-144">Publikování projektu do Azure</span><span class="sxs-lookup"><span data-stu-id="4f9b8-144">Publish the project to Azure</span></span>

<span data-ttu-id="4f9b8-145">Před publikováním projektu musíte mít v předplatném Azure aplikaci funkcí.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-145">You must have a function app in your Azure subscription before you can publish your project.</span></span> <span data-ttu-id="4f9b8-146">Aplikaci funkcí můžete vytvořit přímo v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-146">You can create a function app right from Visual Studio.</span></span>

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

## <a name="test-your-function-in-azure"></a><span data-ttu-id="4f9b8-147">Testování funkce v Azure</span><span class="sxs-lookup"><span data-stu-id="4f9b8-147">Test your function in Azure</span></span>

1. <span data-ttu-id="4f9b8-148">Zkopírujte základní adresu URL aplikace funkcí ze stránky Publikovat profil.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-148">Copy the base URL of the function app from the Publish profile page.</span></span> <span data-ttu-id="4f9b8-149">Nahraďte část adresy URL použité při místním testování funkce, která obsahuje `localhost:port`, novou základní adresou URL.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-149">Replace the `localhost:port` portion of the URL you used when testing the function locally with the new base URL.</span></span> <span data-ttu-id="4f9b8-150">Stejně jako dříve nezapomeňte k této adrese URL připojit řetězec dotazu `&name=<yourname>` a provést požadavek.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-150">As before, make sure to append the query string `&name=<yourname>` to this URL and execute the request.</span></span>

    <span data-ttu-id="4f9b8-151">Adresa URL, která volá vaši funkci aktivovanou protokolem HTTP, vypadá nějak takto:</span><span class="sxs-lookup"><span data-stu-id="4f9b8-151">The URL that calls your HTTP triggered function looks like this:</span></span>

        http://<functionappname>.azurewebsites.net/api/<functionname>?name=<yourname> 

2. <span data-ttu-id="4f9b8-152">Vložte tuto novou adresu URL pro požadavek HTTP do panelu Adresa prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-152">Paste this new URL for the HTTP request into your browser's address bar.</span></span> <span data-ttu-id="4f9b8-153">Následuje ukázka odezvy na vzdálený požadavek GET vrácené funkcí v prohlížeči:</span><span class="sxs-lookup"><span data-stu-id="4f9b8-153">The following shows the response in the browser to the remote GET request returned by the function:</span></span> 

    ![Odezva funkce v prohlížeči](./media/functions-create-your-first-function-visual-studio/functions-test-remote-browser.png)
 
## <a name="next-steps"></a><span data-ttu-id="4f9b8-155">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4f9b8-155">Next steps</span></span>

<span data-ttu-id="4f9b8-156">Pomocí sady Visual Studio jste vytvořili aplikaci funkcí jazyka C# s jednoduchou funkcí aktivovanou protokolem HTTP.</span><span class="sxs-lookup"><span data-stu-id="4f9b8-156">You have used Visual Studio to create a C# function app with a simple HTTP triggered function.</span></span> 

+ <span data-ttu-id="4f9b8-157">Pokud chcete zjistit, jak nakonfigurovat projekt tak, aby podporoval další typy triggerů a vazeb, přečtěte si část [Konfigurace projektu pro místní vývoj](functions-develop-vs.md#configure-the-project-for-local-development) v tématu [Azure Functions Tools for Visual Studio](functions-develop-vs.md).</span><span class="sxs-lookup"><span data-stu-id="4f9b8-157">To learn how to configure your project to support other types of triggers and bindings, see the [Configure the project for local development](functions-develop-vs.md#configure-the-project-for-local-development) section in [Azure Functions Tools for Visual Studio](functions-develop-vs.md).</span></span>
+ <span data-ttu-id="4f9b8-158">Další informace o místním testování a ladění pomocí základních funkcí služby Azure Functions najdete v tématu [Místní kódování a testování služby Azure Functions](functions-run-local.md).</span><span class="sxs-lookup"><span data-stu-id="4f9b8-158">To learn more about local testing and debugging using the Azure Functions Core Tools, see [Code and test Azure Functions locally](functions-run-local.md).</span></span> 
+ <span data-ttu-id="4f9b8-159">Další informace o vývoji funkcí jako knihoven tříd .NET najdete v tématu [Použití knihoven tříd .NET se službou Azure Functions](functions-dotnet-class-library.md).</span><span class="sxs-lookup"><span data-stu-id="4f9b8-159">To learn more about developing functions as .NET class libraries, see [Using .NET class libraries with Azure Functions](functions-dotnet-class-library.md).</span></span> 

