---
title: "Nástrojů Azure Data Lake: U-SQL místní spuštění a místní ladění s Visual Studio Code | Microsoft Docs"
description: "Další informace o použití nástroje Azure Data Lake pro Visual Studio Code místní spuštění a místní ladění."
Keywords: "VScode, nástroje Azure Data Lake, místní spuštění souboru úložiště místní ladění, místní ladění preview, odešlete do cestu k úložišti"
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: DJ
editor: jejiang
tags: azure-portal
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: big-data
ms.date: 07/14/2017
ms.author: jejiang
ms.openlocfilehash: 367e4ba792f83d6ee246208306e4c09b69cb49ef
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="u-sql-local-run-and-local-debug-with-visual-studio-code"></a><span data-ttu-id="551ed-104">U-SQL místní spuštění a místní ladění s kódem jazyka Visual Studio</span><span class="sxs-lookup"><span data-stu-id="551ed-104">U-SQL local run and local debug with Visual Studio Code</span></span>

## <a name="prerequisites"></a><span data-ttu-id="551ed-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="551ed-105">Prerequisites</span></span>
<span data-ttu-id="551ed-106">Ujistěte se, že máte následující požadavky na místě před zahájením těchto postupů:</span><span class="sxs-lookup"><span data-stu-id="551ed-106">Make sure you have the following prerequisites in place before you start these procedures:</span></span>
- <span data-ttu-id="551ed-107">Nástroj Azure Data Lake pro Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="551ed-107">Azure Data Lake Tool for Visual Studio Code.</span></span> <span data-ttu-id="551ed-108">Pokyny najdete v tématu [nástrojů pomocí Azure Data Lake pro Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span><span class="sxs-lookup"><span data-stu-id="551ed-108">For instructions, see [Use Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span></span>
- <span data-ttu-id="551ed-109">C# pro Visual Studio Code (Pokud chcete provést místní ladicího U-SQL).</span><span class="sxs-lookup"><span data-stu-id="551ed-109">C# for Visual Studio Code (if you want to perform a U-SQL local debug).</span></span>

   ![Instalace jazyka C# v nástrojů Data Lake pro Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-install-ms-vscodecsharp.png)
   
   > [!NOTE]
   > <span data-ttu-id="551ed-111">Místní spuštění U-SQL a ladění funkcí aktuálně podporují pouze uživatelé s Windows.</span><span class="sxs-lookup"><span data-stu-id="551ed-111">The U-SQL local run and debug features currently only support Windows users.</span></span> 


## <a name="set-up-the-u-sql-local-run-environment"></a><span data-ttu-id="551ed-112">Nastavení prostředí pro místní spuštění U-SQL</span><span class="sxs-lookup"><span data-stu-id="551ed-112">Set up the U-SQL local run environment</span></span>

1. <span data-ttu-id="551ed-113">Vyberte Ctrl + Shift + P otevřete paletu příkaz, a potom zadejte **ADL: stažení závislostí LocalRun** stáhnout balíčky.</span><span class="sxs-lookup"><span data-stu-id="551ed-113">Select Ctrl+Shift+P to open the command palette, and then enter **ADL: Download LocalRun Dependency** to download the packages.</span></span>  

   ![Stáhnout balíčky ADL LocalRun závislostí](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/DownloadLocalRun.png)

2. <span data-ttu-id="551ed-115">Vyhledejte závislosti balíčků z cesty uvedené v **výstup** podokně a pak nainstalujte BuildTools a Win10SDK 10240.</span><span class="sxs-lookup"><span data-stu-id="551ed-115">Locate the dependency packages from the path shown in the **Output** pane, and then install BuildTools and Win10SDK 10240.</span></span> <span data-ttu-id="551ed-116">Tady je příklad cesty:</span><span class="sxs-lookup"><span data-stu-id="551ed-116">Here is an example path:</span></span>  
`C:\Users\xxx\.vscode\extensions\usqlextpublisher.usql-vscode-ext-x.x.x\LocalRunDependency
`  
  <span data-ttu-id="551ed-117">![Vyhledejte závislosti balíčků](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/LocateDependencyPath.png)</span><span class="sxs-lookup"><span data-stu-id="551ed-117">![Locate the dependency packages](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/LocateDependencyPath.png)</span></span>

   <span data-ttu-id="551ed-118">a.</span><span class="sxs-lookup"><span data-stu-id="551ed-118">a.</span></span> <span data-ttu-id="551ed-119">Chcete-li nainstalovat BuildTools, postupujte podle pokynů průvodce.</span><span class="sxs-lookup"><span data-stu-id="551ed-119">To install BuildTools, follow the wizard instructions.</span></span>   

  ![Nainstalujte BuildTools](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallBuildTools.png)

   <span data-ttu-id="551ed-121">b.</span><span class="sxs-lookup"><span data-stu-id="551ed-121">b.</span></span> <span data-ttu-id="551ed-122">Chcete-li nainstalovat Win10SDK 10240, postupujte podle pokynů průvodce.</span><span class="sxs-lookup"><span data-stu-id="551ed-122">To install Win10SDK 10240, follow the wizard instructions.</span></span>  

  ![Nainstalujte Win10SDK 10240](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallWin10SDK.png)

3. <span data-ttu-id="551ed-124">Nastavte proměnnou prostředí.</span><span class="sxs-lookup"><span data-stu-id="551ed-124">Set up the environment variable.</span></span> <span data-ttu-id="551ed-125">Nastavte **SCOPE_CPP_SDK** proměnnou prostředí:</span><span class="sxs-lookup"><span data-stu-id="551ed-125">Set the **SCOPE_CPP_SDK** environment variable to:</span></span>  
`C:\Users\xxx\.vscode\extensions\usqlextpublisher.usql-vscode-ext-x.x.x\LocalRunDependency\CppSDK_3rdparty
`  
4. <span data-ttu-id="551ed-126">Restartujte operačního systému a ujistěte se, že nastavení proměnné prostředí se projeví.</span><span class="sxs-lookup"><span data-stu-id="551ed-126">Restart the OS to make sure that the environment variable settings take effect.</span></span>  

   ![Zkontrolujte, jestli že je nainstalované SCOPE_CPP_SDK proměnné prostředí](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/ConfigScopeCppSDk.png)

## <a name="start-the-local-run-service-and-submit-the-u-sql-job-to-a-local-account"></a><span data-ttu-id="551ed-128">Spusťte službu místní spuštění a odeslání úlohy U-SQL pro místní účet</span><span class="sxs-lookup"><span data-stu-id="551ed-128">Start the local run service and submit the U-SQL job to a local account</span></span> 
<span data-ttu-id="551ed-129">Pro uživatele poprvé, budete vyzváni ke stažení ADL: stažení LocalRun závislosti balíčků, pokud již nejsou nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="551ed-129">For the first-time user, you are prompted to download the ADL: Download LocalRun Dependency packages if they are not already installed.</span></span>
1. <span data-ttu-id="551ed-130">Vyberte Ctrl + Shift + P otevřete paletu příkaz, a potom zadejte **ADL: spustit místní spustit službu**.</span><span class="sxs-lookup"><span data-stu-id="551ed-130">Select Ctrl+Shift+P to open the command palette, and then enter **ADL: Start Local Run Service**.</span></span>
2. <span data-ttu-id="551ed-131">Vyberte **přijmout** poprvé přijmout licenční podmínky pro Software společnosti Microsoft.</span><span class="sxs-lookup"><span data-stu-id="551ed-131">Select **Accept** to accept the Microsoft Software License Terms for the first time.</span></span> 

   ![Přijmout licenční podmínky pro Software společnosti Microsoft](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/AcceptEULA.png)   
3. <span data-ttu-id="551ed-133">Otevře se konzola cmd.</span><span class="sxs-lookup"><span data-stu-id="551ed-133">The cmd console opens.</span></span> <span data-ttu-id="551ed-134">Pro uživatele, musíte zadat **3**a potom vyhledejte cestu místní složky pro vaše data vstup a výstup.</span><span class="sxs-lookup"><span data-stu-id="551ed-134">For first-time users, you need to enter **3**, and then locate the local folder path for your data input and output.</span></span> <span data-ttu-id="551ed-135">Pro další možnosti můžete použít výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="551ed-135">For other options, you can use the default values.</span></span> 

   ![Nástroje data Lake pro Visual Studio Code místní spuštění cmd](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-cmd.png)
4. <span data-ttu-id="551ed-137">Vyberte Ctrl + Shift + P otevřít palety příkazu, zadejte **ADL: odeslat úlohu**a potom vyberte **místní** se odeslat úlohu místní účet.</span><span class="sxs-lookup"><span data-stu-id="551ed-137">Select Ctrl+Shift+P to open the command palette, enter **ADL: Submit Job**, and then select **Local** to submit the job to your local account.</span></span>

   ![Vyberte místní nástrojů data Lake pro Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-select-local.png)
5. <span data-ttu-id="551ed-139">Po odeslání úlohy můžete zobrazit podrobnosti o odeslání.</span><span class="sxs-lookup"><span data-stu-id="551ed-139">After you submit the job, you can view the submission details.</span></span> <span data-ttu-id="551ed-140">K zobrazení podrobností vyberte odesílání **jobUrl** v **výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="551ed-140">To view the submission details select **jobUrl** in the **Output** window.</span></span> <span data-ttu-id="551ed-141">Můžete také zobrazit stav úlohy odeslání z konzoly cmd.</span><span class="sxs-lookup"><span data-stu-id="551ed-141">You can also view the job submission status from the cmd console.</span></span> <span data-ttu-id="551ed-142">Zadejte **7** v konzole cmd, pokud chcete zjistit podrobnosti úlohy.</span><span class="sxs-lookup"><span data-stu-id="551ed-142">Enter **7** in the cmd console if you want to know more job details.</span></span>

   <span data-ttu-id="551ed-143">![Nástroje data Lake pro Visual Studio Code místní spuštění výstupu](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-result.png)
   ![nástroje Data Lake pro Visual Studio Code místní spuštění cmd stav](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-localrun-cmd-status.png)</span><span class="sxs-lookup"><span data-stu-id="551ed-143">![Data Lake Tools for Visual Studio Code local run output](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-result.png)
![Data Lake Tools for Visual Studio Code local run cmd status](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-localrun-cmd-status.png)</span></span> 


## <a name="start-a-local-debug-for-the-u-sql-job"></a><span data-ttu-id="551ed-144">Spustit místní ladění pro projekt U-SQL</span><span class="sxs-lookup"><span data-stu-id="551ed-144">Start a local debug for the U-SQL job</span></span>  
<span data-ttu-id="551ed-145">Pro uživatele poprvé, budete vyzváni ke stažení ADL: stažení LocalRun závislosti balíčků, pokud již nejsou nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="551ed-145">For the first-time user, you are prompted to download the ADL: Download LocalRun Dependency packages if they are not already installed.</span></span>
  
1. <span data-ttu-id="551ed-146">Vyberte Ctrl + Shift + P otevřete paletu příkaz, a potom zadejte **ADL: spustit místní spustit službu**.</span><span class="sxs-lookup"><span data-stu-id="551ed-146">Select Ctrl+Shift+P to open the command palette, and then enter **ADL: Start Local Run Service**.</span></span> <span data-ttu-id="551ed-147">Otevře se konzola cmd.</span><span class="sxs-lookup"><span data-stu-id="551ed-147">The cmd console opens.</span></span> <span data-ttu-id="551ed-148">Ujistěte se, že **DataRoot** nastavena.</span><span class="sxs-lookup"><span data-stu-id="551ed-148">Make sure that the **DataRoot** is set.</span></span>
3. <span data-ttu-id="551ed-149">Nastavte zarážky v vaší C# kódu.</span><span class="sxs-lookup"><span data-stu-id="551ed-149">Set a breakpoint in your C# code-behind.</span></span>
4. <span data-ttu-id="551ed-150">Zpět v editoru skriptu vyberte Ctrl + Shift + P pro otevření konzoly příkazu a potom zadejte **místní ladění** na místní ladění službu spustit.</span><span class="sxs-lookup"><span data-stu-id="551ed-150">Back in the script editor, select Ctrl+Shift+P to open the command console, and then enter **Local Debug** to start your local debug service.</span></span>

![Nástroje data Lake pro Visual Studio Code výsledek místní ladění](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-debug-result.png)


## <a name="next-steps"></a><span data-ttu-id="551ed-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="551ed-152">Next steps</span></span>
- <span data-ttu-id="551ed-153">Pomocí nástrojů Azure Data Lake pro Visual Studio Code, najdete v části [nástrojů pomocí Azure Data Lake pro Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span><span class="sxs-lookup"><span data-stu-id="551ed-153">For using Azure Data Lake Tools for Visual Studio Code, see [Use Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span></span>
- <span data-ttu-id="551ed-154">Získávání Začínáme informací o Data Lake Analytics, najdete v části [kurz: Začínáme s Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="551ed-154">For getting started information on Data Lake Analytics, see [Tutorial: Get started with Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).</span></span>
- <span data-ttu-id="551ed-155">Informace o nástrojů Data Lake pro Visual Studio najdete v tématu [kurz: vývoj U-SQL skriptů pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="551ed-155">For information about Data Lake Tools for Visual Studio, see [Tutorial: Develop U-SQL scripts by using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
- <span data-ttu-id="551ed-156">Informace týkající se vývoje sestavení najdete v tématu [sestavení vyvíjet U-SQL pro úlohy Azure Data Lake Analytics](data-lake-analytics-u-sql-develop-assemblies.md).</span><span class="sxs-lookup"><span data-stu-id="551ed-156">For the information on developing assemblies, see [Develop U-SQL assemblies for Azure Data Lake Analytics jobs](data-lake-analytics-u-sql-develop-assemblies.md).</span></span>
