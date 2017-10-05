---
title: "Vývoj pomocí sady Visual Studio Azure Functions | Microsoft Docs"
description: "Zjistěte, jak pro vývoj a testování pomocí nástroje Azure funkce pro Visual Studio 2017 Azure Functions."
services: functions
documentationcenter: .net
author: ggailey777
manager: erikre
editor: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: glenga
ms.openlocfilehash: 1e0568bc58e8879cabe409cf8e9b5866f922e7c9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-functions-tools-for-visual-studio"></a><span data-ttu-id="5b713-103">Azure Functions Tools pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5b713-103">Azure Functions Tools for Visual Studio</span></span>  

<span data-ttu-id="5b713-104">Azure funkce Tools for Visual Studio 2017 představuje rozšíření pro Visual Studio, která umožňuje vývoj, testování a nasazení funkce jazyka C# do Azure.</span><span class="sxs-lookup"><span data-stu-id="5b713-104">Azure Functions Tools for Visual Studio 2017 is an extension for Visual Studio that lets you develop, test, and deploy C# functions to Azure.</span></span> <span data-ttu-id="5b713-105">Pokud je vaše první zkušenosti s Azure Functions, další informace najdete v [Úvod do Azure Functions](functions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5b713-105">If this is your first experience with Azure Functions, you can learn more at [An introduction to Azure Functions](functions-overview.md).</span></span>

<span data-ttu-id="5b713-106">Funkce nástroje Azure poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="5b713-106">The Azure Functions Tools provides the following benefits:</span></span> 

* <span data-ttu-id="5b713-107">Upravit, sestavte a spusťte funkce ve svém místním vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="5b713-107">Edit, build, and run functions on your local development computer.</span></span> 
* <span data-ttu-id="5b713-108">Publikování projektu Azure Functions přímo do Azure.</span><span class="sxs-lookup"><span data-stu-id="5b713-108">Publish your Azure Functions project directly to Azure.</span></span> 
* <span data-ttu-id="5b713-109">Pomocí atributů WebJobs deklarovat vazby funkcí přímo v kódu jazyka C# místo zachování samostatné function.json pro vazby definice.</span><span class="sxs-lookup"><span data-stu-id="5b713-109">Use WebJobs attributes to declare function bindings directly in the C# code instead of maintaining a separate function.json for binding definitions.</span></span>
* <span data-ttu-id="5b713-110">Vývoj a nasazení předem kompilované funkce jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="5b713-110">Develop and deploy pre-compiled C# functions.</span></span> <span data-ttu-id="5b713-111">Předem dodržela funkce zajistit lepší studený start výkonu než založených na skriptech funkcí jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="5b713-111">Pre-complied functions provide a better cold-start performance than C# script-based functions.</span></span> 
* <span data-ttu-id="5b713-112">Přitom má všechny výhody vývoje v sadě Visual Studio Code funkcí v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="5b713-112">Code your functions in C# while having all of the benefits of Visual Studio development.</span></span> 

<span data-ttu-id="5b713-113">Toto téma ukazuje, jak pomocí nástroje Azure funkce pro Visual Studio 2017 vyvíjet funkcí v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="5b713-113">This topic shows you how to use the Azure Functions Tools for Visual Studio 2017 to develop your functions in C#.</span></span> <span data-ttu-id="5b713-114">Také zjistíte, jak publikovat projekt do Azure jako sestavení rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="5b713-114">You also learn how to publish your project to Azure as a .NET assembly.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5b713-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5b713-115">Prerequisites</span></span>

<span data-ttu-id="5b713-116">Nástroje Azure funkce je součástí Azure development zatížení [Visual Studio 2017 verze 15.3](https://www.visualstudio.com/vs/), nebo novější.</span><span class="sxs-lookup"><span data-stu-id="5b713-116">Azure Functions Tools is included in the Azure development workload of [Visual Studio 2017 version 15.3](https://www.visualstudio.com/vs/), or a later version.</span></span> <span data-ttu-id="5b713-117">Zajistěte, aby zahrnete **Azure development** zatížení v instalaci sady Visual Studio 2017 verze 15.3:</span><span class="sxs-lookup"><span data-stu-id="5b713-117">Make sure you include the **Azure development** workload in your Visual Studio 2017 version 15.3 installation:</span></span>

![Instalace sady Visual Studio 2017 se sadou funkcí Vývoj pro Azure](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)

<span data-ttu-id="5b713-119">Pokud chcete vytvořit a nasadit funkce, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="5b713-119">To create and deploy functions, you also need:</span></span>

* <span data-ttu-id="5b713-120">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="5b713-120">An active Azure subscription.</span></span> <span data-ttu-id="5b713-121">Pokud nemáte předplatné Azure, [volné účty](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="5b713-121">If you don't have an Azure subscription, [free accounts](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) are available.</span></span>

* <span data-ttu-id="5b713-122">Účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="5b713-122">An Azure Storage account.</span></span> <span data-ttu-id="5b713-123">Pokud chcete vytvořit účet úložiště, najdete v části [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="5b713-123">To create a storage account, see [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span></span>  
## <a name="create-an-azure-functions-project"></a><span data-ttu-id="5b713-124">Vytvoření projektu Azure Functions</span><span class="sxs-lookup"><span data-stu-id="5b713-124">Create an Azure Functions project</span></span> 

[!INCLUDE [Create a project using the Azure Functions](../../includes/functions-vstools-create.md)]


## <a name="configure-the-project-for-local-development"></a><span data-ttu-id="5b713-125">Konfigurace projektu pro místní vývoj</span><span class="sxs-lookup"><span data-stu-id="5b713-125">Configure the project for local development</span></span>

<span data-ttu-id="5b713-126">Když vytvoříte nový projekt pomocí šablony Azure Functions, zobrazí prázdné C# projekt, který obsahuje následující soubory:</span><span class="sxs-lookup"><span data-stu-id="5b713-126">When you create a new project using the Azure Functions template, you get an empty C# project that contains the following files:</span></span>

* <span data-ttu-id="5b713-127">**Host.JSON**: vám umožní nakonfigurovat funkce hostitele.</span><span class="sxs-lookup"><span data-stu-id="5b713-127">**host.json**: Lets you configure the Functions host.</span></span> <span data-ttu-id="5b713-128">Toto nastavení se týká i při spuštění místně a v Azure.</span><span class="sxs-lookup"><span data-stu-id="5b713-128">These settings apply both when running locally and in Azure.</span></span> <span data-ttu-id="5b713-129">Další informace najdete v tématu [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) článku.</span><span class="sxs-lookup"><span data-stu-id="5b713-129">For more information, see [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) reference article.</span></span>
    
* <span data-ttu-id="5b713-130">**Local.Settings.JSON**: udržuje nastavení používané při místním spuštění funkce.</span><span class="sxs-lookup"><span data-stu-id="5b713-130">**local.settings.json**: Maintains settings used when running functions locally.</span></span> <span data-ttu-id="5b713-131">Tato nastavení nejsou používány nástrojem Azure, jsou používány [nástroje základní funkce Azure](functions-run-local.md).</span><span class="sxs-lookup"><span data-stu-id="5b713-131">These settings are not used by Azure, they are used by the [Azure Functions Core Tools](functions-run-local.md).</span></span> <span data-ttu-id="5b713-132">Tento soubor lze použijte k určení nastavení, jako jsou třeba řetězce připojení k jiným službám Azure.</span><span class="sxs-lookup"><span data-stu-id="5b713-132">Use this file to specify settings, such as connection strings to other Azure services.</span></span> <span data-ttu-id="5b713-133">Přidejte nový klíč k **hodnoty** pole pro každé připojení vyžaduje funkce ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="5b713-133">Add a new key to the **Values** array for each connection required by functions in your project.</span></span> <span data-ttu-id="5b713-134">Další informace najdete v tématu [nastavení místního souboru](functions-run-local.md#local-settings-file) v tématu nástroje základní funkce Azure.</span><span class="sxs-lookup"><span data-stu-id="5b713-134">For more information, see [Local settings file](functions-run-local.md#local-settings-file) in the Azure Functions Core Tools topic.</span></span>

<span data-ttu-id="5b713-135">Modul runtime funkce interně používá účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="5b713-135">The Functions runtime uses an Azure Storage account internally.</span></span> <span data-ttu-id="5b713-136">Pro všechny aktivovat jiného typu než HTTP a pomocí webhooků, musíte nastavit **Values.AzureWebJobsStorage** klíče na platný připojovací řetězec účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="5b713-136">For all trigger types other than HTTP and webhooks, you must set the **Values.AzureWebJobsStorage** key to a valid Azure Storage account connection string.</span></span>

[!INCLUDE [Note to not use local storage](../../includes/functions-local-settings-note.md)]

 <span data-ttu-id="5b713-137">Nastavení připojovacího řetězce pro účet úložiště:</span><span class="sxs-lookup"><span data-stu-id="5b713-137">To set the storage account connection string:</span></span>

1. <span data-ttu-id="5b713-138">V sadě Visual Studio otevřete **Průzkumník cloudu**, rozbalte položku **účet úložiště** > **váš účet úložiště**, pak vyberte **vlastnosti** a zkopírujte **primární připojovací řetězec** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5b713-138">In Visual Studio, open **Cloud Explorer**, expand **Storage Account** > **Your Storage Account**, then select **Properties** and copy the **Primary Connection String** value.</span></span>   

2. <span data-ttu-id="5b713-139">V projektu, otevřete soubor projektu local.settings.json a nastavte hodnotu **AzureWebJobsStorage** zkopírovanou klíče připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="5b713-139">In your project, open the local.settings.json project file and set the value of the **AzureWebJobsStorage** key to the connection string you copied.</span></span>

3. <span data-ttu-id="5b713-140">Opakujte předchozí krok pro přidání jedinečné klíče **hodnoty** pole pro všechna připojení, které vyžadují funkcí.</span><span class="sxs-lookup"><span data-stu-id="5b713-140">Repeat the previous step to add unique keys to the **Values** array for any other connections required by your functions.</span></span>  

## <a name="create-a-function"></a><span data-ttu-id="5b713-141">Vytvoření funkce</span><span class="sxs-lookup"><span data-stu-id="5b713-141">Create a function</span></span>

<span data-ttu-id="5b713-142">V předem kompilované funkce jsou definovány vazby používané funkce použití atributů v kódu.</span><span class="sxs-lookup"><span data-stu-id="5b713-142">In pre-compiled functions, the bindings used by the function are defined by applying attributes in the code.</span></span> <span data-ttu-id="5b713-143">Když pomocí funkce nástroje Azure vytvářet funkce ze zadaného šablon, použijí se tyto atributy pro vás.</span><span class="sxs-lookup"><span data-stu-id="5b713-143">When you use the Azure Functions Tools to create your functions from the provided templates, these attributes are applied for you.</span></span> 

1. <span data-ttu-id="5b713-144">V **Průzkumníku řešení** klikněte pravým tlačítkem na uzel projektu a vyberte **Přidat** > **Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="5b713-144">In **Solution Explorer**, right-click on your project node and select **Add** > **New Item**.</span></span> <span data-ttu-id="5b713-145">Vyberte **funkce Azure**, zadejte **název** pro třídu a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="5b713-145">Select **Azure Function**, type a **Name** for the class, and click **Add**.</span></span>

2. <span data-ttu-id="5b713-146">Zvolte aktivační událost, nastavte vlastnosti vazby a klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="5b713-146">Choose your trigger, set the binding properties, and click **Create**.</span></span> <span data-ttu-id="5b713-147">Následující příklad ukazuje nastavení při vytváření Queue storage aktivaci funkce.</span><span class="sxs-lookup"><span data-stu-id="5b713-147">The following example shows the settings when creating a Queue storage triggered function.</span></span> 

    ![](./media/functions-develop-vs/functions-vstools-create-queuetrigger.png)
    
    <span data-ttu-id="5b713-148">Klíč řetězce připojení s názvem **QueueStorage** zadaný, který je definován v souboru local.settings.json.</span><span class="sxs-lookup"><span data-stu-id="5b713-148">A connection string key named **QueueStorage** is supplied, which is defined in the local.settings.json file.</span></span> 
 
3. <span data-ttu-id="5b713-149">Zkontrolujte nově přidané třídy.</span><span class="sxs-lookup"><span data-stu-id="5b713-149">Examine the newly added class.</span></span> <span data-ttu-id="5b713-150">Zobrazí statického **spustit** metody s atributem **%{FunctionName/** atribut.</span><span class="sxs-lookup"><span data-stu-id="5b713-150">You see a static **Run** method, that is attributed with the **FunctionName** attribute.</span></span> <span data-ttu-id="5b713-151">Tento atribut označuje, že metoda je vstupní bod pro funkci.</span><span class="sxs-lookup"><span data-stu-id="5b713-151">This attribute indicates that the method is the entry point for the function.</span></span> 

    <span data-ttu-id="5b713-152">Například následující třídy jazyka C# představuje základní funkce úložiště aktivuje fronty:</span><span class="sxs-lookup"><span data-stu-id="5b713-152">For example, the following C# class represents a basic Queue storage triggered function:</span></span>

    ````csharp
    using System;
    using Microsoft.Azure.WebJobs;
    using Microsoft.Azure.WebJobs.Host;
    
    namespace FunctionApp1
    {
        public static class Function1
        {
            [FunctionName("QueueTriggerCSharp")]        
            public static void Run([QueueTrigger("myqueue-items", Connection = "QueueStorage")]string myQueueItem, TraceWriter log)
            {
                log.Info($"C# Queue trigger function processed: {myQueueItem}");
            }
        }
    } 
    ````
 
    <span data-ttu-id="5b713-153">Atribut specifické pro vazbu se použije pro každý zadaný pro vstupní bod metody parametr vazby.</span><span class="sxs-lookup"><span data-stu-id="5b713-153">A binding-specific attribute is applied to each binding parameter supplied to the entry point method.</span></span> <span data-ttu-id="5b713-154">Atribut načítá informace o vazbě jako parametry.</span><span class="sxs-lookup"><span data-stu-id="5b713-154">The attribute takes the binding information as parameters.</span></span> <span data-ttu-id="5b713-155">V předchozím příkladu má první parametr **QueueTrigger** atribut použité, s označením fronty aktivuje funkce.</span><span class="sxs-lookup"><span data-stu-id="5b713-155">In the previous example, The first parameter has a **QueueTrigger** attribute applied, indicating queue triggered function.</span></span> <span data-ttu-id="5b713-156">Název fronty a název nastavení připojovacího řetězce jsou předány jako parametry.</span><span class="sxs-lookup"><span data-stu-id="5b713-156">The queue name and connection string setting name are passed as parameters.</span></span>  

## <a name="testing-functions"></a><span data-ttu-id="5b713-157">Testování funkcí</span><span class="sxs-lookup"><span data-stu-id="5b713-157">Testing functions</span></span>

<span data-ttu-id="5b713-158">Nástroje Azure Functions Core umožňují spouštět projekt Azure Functions na místním počítači pro vývoj.</span><span class="sxs-lookup"><span data-stu-id="5b713-158">Azure Functions Core Tools lets you run Azure Functions project on your local development computer.</span></span> <span data-ttu-id="5b713-159">K instalaci těchto nástrojů budete vyzváni při prvním spuštění funkce ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5b713-159">You are prompted to install these tools the first time you start a function from Visual Studio.</span></span>  

<span data-ttu-id="5b713-160">Pokud chcete funkci otestovat, stiskněte F5.</span><span class="sxs-lookup"><span data-stu-id="5b713-160">To test your function, press F5.</span></span> <span data-ttu-id="5b713-161">Po výzvě přijměte požadavek ze sady Visual Studio na stažení a instalaci nástrojů Azure Functions Core (CLI).</span><span class="sxs-lookup"><span data-stu-id="5b713-161">If prompted, accept the request from Visual Studio to download and install Azure Functions Core (CLI) tools.</span></span>  <span data-ttu-id="5b713-162">Může být také potřeba povolit výjimku brány firewall, aby nástroje mohly zpracovávat požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="5b713-162">You may also need to enable a firewall exception so that the tools can handle HTTP requests.</span></span>

<span data-ttu-id="5b713-163">S projektem systémem můžete otestovat váš kód by při testování nasazené funkce.</span><span class="sxs-lookup"><span data-stu-id="5b713-163">With the project running, you can test your code as you would test deployed function.</span></span> <span data-ttu-id="5b713-164">Další informace najdete v tématu [strategie pro testování kódu v Azure Functions](functions-test-a-function.md).</span><span class="sxs-lookup"><span data-stu-id="5b713-164">For more information, see [Strategies for testing your code in Azure Functions](functions-test-a-function.md).</span></span> <span data-ttu-id="5b713-165">Při spuštění v režimu ladění, jsou zarážky v sadě Visual Studio podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="5b713-165">When running in debug mode, breakpoints are hit in Visual Studio as expected.</span></span> 

<span data-ttu-id="5b713-166">Příklad fronty aktivuje funkci otestovat, najdete v článku [fronty aktivuje funkce Rychlý úvodní kurz](functions-create-storage-queue-triggered-function.md#test-the-function).</span><span class="sxs-lookup"><span data-stu-id="5b713-166">For an example of how to test a queue triggered function, see the [queue triggered function quickstart tutorial](functions-create-storage-queue-triggered-function.md#test-the-function).</span></span>  

<span data-ttu-id="5b713-167">Další informace o použití nástrojů pro základní funkce Azure, najdete v části [kód a testovat místně na Azure functions](functions-run-local.md).</span><span class="sxs-lookup"><span data-stu-id="5b713-167">To learn more about using the Azure Functions Core Tools, see [Code and test Azure functions locally](functions-run-local.md).</span></span>

## <a name="publish-to-azure"></a><span data-ttu-id="5b713-168">Publikování aplikací do Azure</span><span class="sxs-lookup"><span data-stu-id="5b713-168">Publish to Azure</span></span>

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

>[!NOTE]  
><span data-ttu-id="5b713-169">Všechna nastavení, které jste přidali v local.settings.json je nutné také přidat do funkce aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="5b713-169">Any settings you added in the local.settings.json must be also added to the function app in Azure.</span></span> <span data-ttu-id="5b713-170">Tato nastavení nejsou automaticky přidáni.</span><span class="sxs-lookup"><span data-stu-id="5b713-170">These settings are not added automatically.</span></span> <span data-ttu-id="5b713-171">Požadovaná nastavení můžete přidat do aplikace pro funkce v jednom z těchto způsobů:</span><span class="sxs-lookup"><span data-stu-id="5b713-171">You can add required settings to your function app in one of these ways:</span></span>
>
>* <span data-ttu-id="5b713-172">[Pomocí portálu Azure](functions-how-to-use-azure-function-app-settings.md#settings).</span><span class="sxs-lookup"><span data-stu-id="5b713-172">[Using the Azure portal](functions-how-to-use-azure-function-app-settings.md#settings).</span></span>
>* <span data-ttu-id="5b713-173">[Pomocí `--publish-local-settings` možnost publikování v nástrojích pro základní funkce Azure](functions-run-local.md#publish).</span><span class="sxs-lookup"><span data-stu-id="5b713-173">[Using the `--publish-local-settings` publish option in the Azure Functions Core Tools](functions-run-local.md#publish).</span></span>
>* <span data-ttu-id="5b713-174">[Použití Azure CLI](/cli/azure/functionapp/config/appsettings#set).</span><span class="sxs-lookup"><span data-stu-id="5b713-174">[Using the Azure CLI](/cli/azure/functionapp/config/appsettings#set).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="5b713-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5b713-175">Next steps</span></span>

<span data-ttu-id="5b713-176">Další informace o nástrojích funkce Azure, najdete v části Nejčastější dotazy [2017 nástroje sady Visual Studio pro Azure Functions](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/) příspěvku na blogu.</span><span class="sxs-lookup"><span data-stu-id="5b713-176">For more information about Azure Functions Tools, see the Common Questions section of the [Visual Studio 2017 Tools for Azure Functions](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/) blog post.</span></span>

<span data-ttu-id="5b713-177">Další informace o nástrojích základní funkce Azure najdete v tématu [kód a testovat místně na Azure functions](functions-run-local.md).</span><span class="sxs-lookup"><span data-stu-id="5b713-177">To learn more about the Azure Functions Core Tools, see [Code and test Azure functions locally](functions-run-local.md).</span></span>  
<span data-ttu-id="5b713-178">Další informace o vývoji funkcí jako knihoven tříd .NET najdete v tématu [Použití knihoven tříd .NET se službou Azure Functions](functions-dotnet-class-library.md).</span><span class="sxs-lookup"><span data-stu-id="5b713-178">To learn more about developing functions as .NET class libraries, see [Using .NET class libraries with Azure Functions](functions-dotnet-class-library.md).</span></span> <span data-ttu-id="5b713-179">Toto téma obsahuje také příklady, jak použít atributy deklarovat různé typy vazeb Azure Functions podporuje.</span><span class="sxs-lookup"><span data-stu-id="5b713-179">This topic also provides examples of how to use attributes to declare the various types of bindings supported by Azure Functions.</span></span>    
