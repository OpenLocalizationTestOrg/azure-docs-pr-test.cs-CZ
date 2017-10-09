---
title: "aaaDevelop funkce Azure pomocí sady Visual Studio | Microsoft Docs"
description: "Zjistěte, jak Azure Functions toodevelop a testování pomocí nástroje Azure funkce pro Visual Studio 2017."
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
ms.openlocfilehash: c9baf882bf58068cb9a8930bea337fe51b2a77ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-tools-for-visual-studio"></a><span data-ttu-id="ad7ca-103">Azure Functions Tools pro Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ad7ca-103">Azure Functions Tools for Visual Studio</span></span>  

<span data-ttu-id="ad7ca-104">Azure funkce Tools for Visual Studio 2017 představuje rozšíření pro Visual Studio, která umožňuje vývoj, testování a nasazení tooAzure funkcí jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-104">Azure Functions Tools for Visual Studio 2017 is an extension for Visual Studio that lets you develop, test, and deploy C# functions tooAzure.</span></span> <span data-ttu-id="ad7ca-105">Pokud je vaše první zkušenosti s Azure Functions, další informace najdete v [Úvod tooAzure funkce](functions-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ad7ca-105">If this is your first experience with Azure Functions, you can learn more at [An introduction tooAzure Functions](functions-overview.md).</span></span>

<span data-ttu-id="ad7ca-106">Hello nástroje funkce Azure poskytuje hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="ad7ca-106">hello Azure Functions Tools provides hello following benefits:</span></span> 

* <span data-ttu-id="ad7ca-107">Upravit, sestavte a spusťte funkce ve svém místním vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-107">Edit, build, and run functions on your local development computer.</span></span> 
* <span data-ttu-id="ad7ca-108">Publikování projektu Azure Functions přímo tooAzure.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-108">Publish your Azure Functions project directly tooAzure.</span></span> 
* <span data-ttu-id="ad7ca-109">Pomocí WebJobs atributy toodeclare funkce vazby přímo v kódu jazyka C# místo zachování samostatné function.json pro vazby definice hello.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-109">Use WebJobs attributes toodeclare function bindings directly in hello C# code instead of maintaining a separate function.json for binding definitions.</span></span>
* <span data-ttu-id="ad7ca-110">Vývoj a nasazení předem kompilované funkce jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-110">Develop and deploy pre-compiled C# functions.</span></span> <span data-ttu-id="ad7ca-111">Předem dodržela funkce zajistit lepší studený start výkonu než založených na skriptech funkcí jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-111">Pre-complied functions provide a better cold-start performance than C# script-based functions.</span></span> 
* <span data-ttu-id="ad7ca-112">Přitom má všechny výhody hello vývoje v sadě Visual Studio Code funkcí v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-112">Code your functions in C# while having all of hello benefits of Visual Studio development.</span></span> 

<span data-ttu-id="ad7ca-113">Toto téma ukazuje, jak toouse hello nástroje funkce Azure pro Visual Studio 2017 toodevelop funkcí v jazyce C#.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-113">This topic shows you how toouse hello Azure Functions Tools for Visual Studio 2017 toodevelop your functions in C#.</span></span> <span data-ttu-id="ad7ca-114">Také zjistíte, jak toopublish vašeho projektu tooAzure jako sestavení rozhraní .NET.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-114">You also learn how toopublish your project tooAzure as a .NET assembly.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ad7ca-115">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ad7ca-115">Prerequisites</span></span>

<span data-ttu-id="ad7ca-116">Nástroje Azure funkce je součástí zatížení Azure development hello [Visual Studio 2017 verze 15.3](https://www.visualstudio.com/vs/), nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-116">Azure Functions Tools is included in hello Azure development workload of [Visual Studio 2017 version 15.3](https://www.visualstudio.com/vs/), or a later version.</span></span> <span data-ttu-id="ad7ca-117">Zajistěte, aby zahrnete hello **Azure development** zatížení v instalaci sady Visual Studio 2017 verze 15.3:</span><span class="sxs-lookup"><span data-stu-id="ad7ca-117">Make sure you include hello **Azure development** workload in your Visual Studio 2017 version 15.3 installation:</span></span>

![Nainstalovat Visual Studio 2017 zatížení Azure development hello](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)

<span data-ttu-id="ad7ca-119">toocreate a nasazení funkce, musíte taky:</span><span class="sxs-lookup"><span data-stu-id="ad7ca-119">toocreate and deploy functions, you also need:</span></span>

* <span data-ttu-id="ad7ca-120">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-120">An active Azure subscription.</span></span> <span data-ttu-id="ad7ca-121">Pokud nemáte předplatné Azure, [volné účty](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-121">If you don't have an Azure subscription, [free accounts](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) are available.</span></span>

* <span data-ttu-id="ad7ca-122">Účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-122">An Azure Storage account.</span></span> <span data-ttu-id="ad7ca-123">toocreate účet úložiště, najdete v části [vytvořit účet úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="ad7ca-123">toocreate a storage account, see [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span></span>  
## <a name="create-an-azure-functions-project"></a><span data-ttu-id="ad7ca-124">Vytvoření projektu Azure Functions</span><span class="sxs-lookup"><span data-stu-id="ad7ca-124">Create an Azure Functions project</span></span> 

[!INCLUDE [Create a project using hello Azure Functions](../../includes/functions-vstools-create.md)]


## <a name="configure-hello-project-for-local-development"></a><span data-ttu-id="ad7ca-125">Konfigurace projektu hello pro místní vývoj</span><span class="sxs-lookup"><span data-stu-id="ad7ca-125">Configure hello project for local development</span></span>

<span data-ttu-id="ad7ca-126">Když vytvoříte nový projekt pomocí šablony Azure Functions hello, získáte prázdný C# projekt, který obsahuje hello následující soubory:</span><span class="sxs-lookup"><span data-stu-id="ad7ca-126">When you create a new project using hello Azure Functions template, you get an empty C# project that contains hello following files:</span></span>

* <span data-ttu-id="ad7ca-127">**Host.JSON**: umožňuje nakonfigurovat hello funkce hostitele.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-127">**host.json**: Lets you configure hello Functions host.</span></span> <span data-ttu-id="ad7ca-128">Toto nastavení se týká i při spuštění místně a v Azure.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-128">These settings apply both when running locally and in Azure.</span></span> <span data-ttu-id="ad7ca-129">Další informace najdete v tématu [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) článku.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-129">For more information, see [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) reference article.</span></span>
    
* <span data-ttu-id="ad7ca-130">**Local.Settings.JSON**: udržuje nastavení používané při místním spuštění funkce.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-130">**local.settings.json**: Maintains settings used when running functions locally.</span></span> <span data-ttu-id="ad7ca-131">Tato nastavení nejsou používány nástrojem Azure, jsou používány hello [nástroje základní funkce Azure](functions-run-local.md).</span><span class="sxs-lookup"><span data-stu-id="ad7ca-131">These settings are not used by Azure, they are used by hello [Azure Functions Core Tools](functions-run-local.md).</span></span> <span data-ttu-id="ad7ca-132">Použijte tato nastavení toospecify souboru, například připojovací řetězce tooother Azure services.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-132">Use this file toospecify settings, such as connection strings tooother Azure services.</span></span> <span data-ttu-id="ad7ca-133">Přidání nového klíče toohello **hodnoty** pole pro každé připojení vyžaduje funkce ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-133">Add a new key toohello **Values** array for each connection required by functions in your project.</span></span> <span data-ttu-id="ad7ca-134">Další informace najdete v tématu [nastavení místního souboru](functions-run-local.md#local-settings-file) v tématu nástroje základní funkce Azure hello.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-134">For more information, see [Local settings file](functions-run-local.md#local-settings-file) in hello Azure Functions Core Tools topic.</span></span>

<span data-ttu-id="ad7ca-135">Hello Functions runtime interně používá účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-135">hello Functions runtime uses an Azure Storage account internally.</span></span> <span data-ttu-id="ad7ca-136">Pro všechny aktivovat jiného typu než HTTP a pomocí webhooků, musíte nastavit hello **Values.AzureWebJobsStorage** klíče tooa platný Azure Storage účet připojovací řetězec.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-136">For all trigger types other than HTTP and webhooks, you must set hello **Values.AzureWebJobsStorage** key tooa valid Azure Storage account connection string.</span></span>

[!INCLUDE [Note toonot use local storage](../../includes/functions-local-settings-note.md)]

 <span data-ttu-id="ad7ca-137">tooset hello účet připojovacího řetězce úložiště:</span><span class="sxs-lookup"><span data-stu-id="ad7ca-137">tooset hello storage account connection string:</span></span>

1. <span data-ttu-id="ad7ca-138">V sadě Visual Studio otevřete **Průzkumník cloudu**, rozbalte položku **účet úložiště** > **váš účet úložiště**, pak vyberte **vlastnosti**a kopírování hello **primární připojovací řetězec** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-138">In Visual Studio, open **Cloud Explorer**, expand **Storage Account** > **Your Storage Account**, then select **Properties** and copy hello **Primary Connection String** value.</span></span>   

2. <span data-ttu-id="ad7ca-139">V projektu, otevřete soubor projektu local.settings.json hello a nastavte hodnotu hello hello **AzureWebJobsStorage** klíče toohello připojovací řetězec, kterou jste zkopírovali.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-139">In your project, open hello local.settings.json project file and set hello value of hello **AzureWebJobsStorage** key toohello connection string you copied.</span></span>

3. <span data-ttu-id="ad7ca-140">Opakujte hello předchozí krok tooadd jedinečné klíče toohello **hodnoty** pole pro všechna připojení, které vyžadují funkcí.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-140">Repeat hello previous step tooadd unique keys toohello **Values** array for any other connections required by your functions.</span></span>  

## <a name="create-a-function"></a><span data-ttu-id="ad7ca-141">Vytvoření funkce</span><span class="sxs-lookup"><span data-stu-id="ad7ca-141">Create a function</span></span>

<span data-ttu-id="ad7ca-142">V předem kompilované funkce jsou definovány hello vazby používané funkce hello použití atributů v kódu hello.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-142">In pre-compiled functions, hello bindings used by hello function are defined by applying attributes in hello code.</span></span> <span data-ttu-id="ad7ca-143">Při použití nástroje funkce Azure toocreate hello funkcí ze šablon hello poskytuje tyto atributy se použijí pro vás.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-143">When you use hello Azure Functions Tools toocreate your functions from hello provided templates, these attributes are applied for you.</span></span> 

1. <span data-ttu-id="ad7ca-144">V **Průzkumníku řešení** klikněte pravým tlačítkem na uzel projektu a vyberte **Přidat** > **Nová položka**.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-144">In **Solution Explorer**, right-click on your project node and select **Add** > **New Item**.</span></span> <span data-ttu-id="ad7ca-145">Vyberte **funkce Azure**, zadejte **název** pro hello třídu a klikněte na tlačítko **přidat**.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-145">Select **Azure Function**, type a **Name** for hello class, and click **Add**.</span></span>

2. <span data-ttu-id="ad7ca-146">Zvolte aktivační událost, nastavte hello vlastnosti vazby a klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-146">Choose your trigger, set hello binding properties, and click **Create**.</span></span> <span data-ttu-id="ad7ca-147">Hello následující příklad ukazuje nastavení hello při vytvoření fronty úložiště aktivaci funkce.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-147">hello following example shows hello settings when creating a Queue storage triggered function.</span></span> 

    ![](./media/functions-develop-vs/functions-vstools-create-queuetrigger.png)
    
    <span data-ttu-id="ad7ca-148">Klíč řetězce připojení s názvem **QueueStorage** zadaný, který je definován v souboru local.settings.json hello.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-148">A connection string key named **QueueStorage** is supplied, which is defined in hello local.settings.json file.</span></span> 
 
3. <span data-ttu-id="ad7ca-149">Zkontrolujte hello nově přidaná třídy.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-149">Examine hello newly added class.</span></span> <span data-ttu-id="ad7ca-150">Zobrazí statického **spustit** metody s atributem hello **%{FunctionName/** atribut.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-150">You see a static **Run** method, that is attributed with hello **FunctionName** attribute.</span></span> <span data-ttu-id="ad7ca-151">Tento atribut označuje, že je metoda hello hello vstupní bod pro funkci hello.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-151">This attribute indicates that hello method is hello entry point for hello function.</span></span> 

    <span data-ttu-id="ad7ca-152">Například hello následující C# – třída představuje základní funkce úložiště aktivuje fronty:</span><span class="sxs-lookup"><span data-stu-id="ad7ca-152">For example, hello following C# class represents a basic Queue storage triggered function:</span></span>

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
 
    <span data-ttu-id="ad7ca-153">Atribut konkrétní vazbu je metody použité tooeach vazby zadaný parametr toohello vstupní bod.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-153">A binding-specific attribute is applied tooeach binding parameter supplied toohello entry point method.</span></span> <span data-ttu-id="ad7ca-154">atribut Hello načítá informace o vazbě hello jako parametry.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-154">hello attribute takes hello binding information as parameters.</span></span> <span data-ttu-id="ad7ca-155">V předchozím příkladu hello hello první parametr má **QueueTrigger** atribut použité, s označením fronty aktivuje funkce.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-155">In hello previous example, hello first parameter has a **QueueTrigger** attribute applied, indicating queue triggered function.</span></span> <span data-ttu-id="ad7ca-156">Název fronty Hello a název nastavení připojovacího řetězce jsou předány jako parametry.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-156">hello queue name and connection string setting name are passed as parameters.</span></span>  

## <a name="testing-functions"></a><span data-ttu-id="ad7ca-157">Testování funkcí</span><span class="sxs-lookup"><span data-stu-id="ad7ca-157">Testing functions</span></span>

<span data-ttu-id="ad7ca-158">Nástroje Azure Functions Core umožňují spouštět projekt Azure Functions na místním počítači pro vývoj.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-158">Azure Functions Core Tools lets you run Azure Functions project on your local development computer.</span></span> <span data-ttu-id="ad7ca-159">Jste výzvami tooinstall, které tyto nástroje hello při prvním spuštění funkce ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-159">You are prompted tooinstall these tools hello first time you start a function from Visual Studio.</span></span>  

<span data-ttu-id="ad7ca-160">tootest funkce, stiskněte klávesu F5.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-160">tootest your function, press F5.</span></span> <span data-ttu-id="ad7ca-161">Pokud se zobrazí výzva, přijímat žádosti o hello ze sady Visual Studio toodownload a nainstalujte nástroje pro základní funkce Azure (CLI).</span><span class="sxs-lookup"><span data-stu-id="ad7ca-161">If prompted, accept hello request from Visual Studio toodownload and install Azure Functions Core (CLI) tools.</span></span>  <span data-ttu-id="ad7ca-162">Také můžete potřebovat tooenable výjimku brány firewall tak, aby hello nástrojů může zpracovávat požadavky HTTP.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-162">You may also need tooenable a firewall exception so that hello tools can handle HTTP requests.</span></span>

<span data-ttu-id="ad7ca-163">S projektem hello systémem můžete otestovat váš kód by při testování nasazené funkce.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-163">With hello project running, you can test your code as you would test deployed function.</span></span> <span data-ttu-id="ad7ca-164">Další informace najdete v tématu [strategie pro testování kódu v Azure Functions](functions-test-a-function.md).</span><span class="sxs-lookup"><span data-stu-id="ad7ca-164">For more information, see [Strategies for testing your code in Azure Functions](functions-test-a-function.md).</span></span> <span data-ttu-id="ad7ca-165">Při spuštění v režimu ladění, jsou zarážky v sadě Visual Studio podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-165">When running in debug mode, breakpoints are hit in Visual Studio as expected.</span></span> 

<span data-ttu-id="ad7ca-166">Příklad jak tootest fronty aktivuje funkce, najdete v části hello [fronty aktivuje funkce Rychlý úvodní kurz](functions-create-storage-queue-triggered-function.md#test-the-function).</span><span class="sxs-lookup"><span data-stu-id="ad7ca-166">For an example of how tootest a queue triggered function, see hello [queue triggered function quickstart tutorial](functions-create-storage-queue-triggered-function.md#test-the-function).</span></span>  

<span data-ttu-id="ad7ca-167">toolearn Další informace o použití nástroje základní funkce hello Azure, najdete v části [kód a testovat místně na Azure functions](functions-run-local.md).</span><span class="sxs-lookup"><span data-stu-id="ad7ca-167">toolearn more about using hello Azure Functions Core Tools, see [Code and test Azure functions locally](functions-run-local.md).</span></span>

## <a name="publish-tooazure"></a><span data-ttu-id="ad7ca-168">Publikování tooAzure</span><span class="sxs-lookup"><span data-stu-id="ad7ca-168">Publish tooAzure</span></span>

[!INCLUDE [Publish hello project tooAzure](../../includes/functions-vstools-publish.md)]

>[!NOTE]  
><span data-ttu-id="ad7ca-169">Všechna nastavení, které jste přidali v hello local.settings.json je nutné také přidat toohello funkce aplikace v Azure.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-169">Any settings you added in hello local.settings.json must be also added toohello function app in Azure.</span></span> <span data-ttu-id="ad7ca-170">Tato nastavení nejsou automaticky přidáni.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-170">These settings are not added automatically.</span></span> <span data-ttu-id="ad7ca-171">Můžete přidat aplikaci funkce tooyour požadovaná nastavení v jednom z těchto způsobů:</span><span class="sxs-lookup"><span data-stu-id="ad7ca-171">You can add required settings tooyour function app in one of these ways:</span></span>
>
>* <span data-ttu-id="ad7ca-172">[Pomocí portálu Azure hello](functions-how-to-use-azure-function-app-settings.md#settings).</span><span class="sxs-lookup"><span data-stu-id="ad7ca-172">[Using hello Azure portal](functions-how-to-use-azure-function-app-settings.md#settings).</span></span>
>* <span data-ttu-id="ad7ca-173">[Pomocí hello `--publish-local-settings` možnost publikování v hello nástroje základní funkce Azure](functions-run-local.md#publish).</span><span class="sxs-lookup"><span data-stu-id="ad7ca-173">[Using hello `--publish-local-settings` publish option in hello Azure Functions Core Tools](functions-run-local.md#publish).</span></span>
>* <span data-ttu-id="ad7ca-174">[Pomocí rozhraní příkazového řádku Azure hello](/cli/azure/functionapp/config/appsettings#set).</span><span class="sxs-lookup"><span data-stu-id="ad7ca-174">[Using hello Azure CLI](/cli/azure/functionapp/config/appsettings#set).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="ad7ca-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ad7ca-175">Next steps</span></span>

<span data-ttu-id="ad7ca-176">Další informace o nástrojích funkce Azure, najdete v tématu hello běžné otázky části hello [2017 nástroje sady Visual Studio pro Azure Functions](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/) příspěvku na blogu.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-176">For more information about Azure Functions Tools, see hello Common Questions section of hello [Visual Studio 2017 Tools for Azure Functions](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/) blog post.</span></span>

<span data-ttu-id="ad7ca-177">toolearn Další informace o nástroje základní funkce hello Azure, najdete v části [kód a testovat místně na Azure functions](functions-run-local.md).</span><span class="sxs-lookup"><span data-stu-id="ad7ca-177">toolearn more about hello Azure Functions Core Tools, see [Code and test Azure functions locally](functions-run-local.md).</span></span>  
<span data-ttu-id="ad7ca-178">toolearn Další informace o vývoji funkce jako knihovny tříd rozhraní .NET, najdete v části [knihovny tříd pomocí rozhraní .NET s Azure Functions](functions-dotnet-class-library.md).</span><span class="sxs-lookup"><span data-stu-id="ad7ca-178">toolearn more about developing functions as .NET class libraries, see [Using .NET class libraries with Azure Functions](functions-dotnet-class-library.md).</span></span> <span data-ttu-id="ad7ca-179">Toto téma obsahuje také příklady jak toouse atributy toodeclare hello různých typů vazeb Azure Functions podporuje.</span><span class="sxs-lookup"><span data-stu-id="ad7ca-179">This topic also provides examples of how toouse attributes toodeclare hello various types of bindings supported by Azure Functions.</span></span>    
