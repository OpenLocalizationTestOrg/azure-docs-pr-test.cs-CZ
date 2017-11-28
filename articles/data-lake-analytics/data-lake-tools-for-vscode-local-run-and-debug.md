---
title: "Nástrojů Azure Data Lake: U-SQL místní spuštění a místní ladění s Visual Studio Code | Microsoft Docs"
description: "Zjistěte, jak ladit nástrojů toouse Azure Data Lake pro Visual Studio Code toolocal spuštění a místní."
Keywords: "VScode, nástroje Azure Data Lake, místní spuštění souboru úložiště místní ladění, místní ladění preview, nahrajte toostorage cesta"
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
ms.openlocfilehash: fb152f07fe8c4b03dde8fb8e62c7475eccda0578
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="u-sql-local-run-and-local-debug-with-visual-studio-code"></a><span data-ttu-id="95c02-104">U-SQL místní spuštění a místní ladění s kódem jazyka Visual Studio</span><span class="sxs-lookup"><span data-stu-id="95c02-104">U-SQL local run and local debug with Visual Studio Code</span></span>

## <a name="prerequisites"></a><span data-ttu-id="95c02-105">Požadavky</span><span class="sxs-lookup"><span data-stu-id="95c02-105">Prerequisites</span></span>
<span data-ttu-id="95c02-106">Ujistěte se, že máte hello následující požadavky splněny, než začnete těchto postupů:</span><span class="sxs-lookup"><span data-stu-id="95c02-106">Make sure you have hello following prerequisites in place before you start these procedures:</span></span>
- <span data-ttu-id="95c02-107">Nástroj Azure Data Lake pro Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="95c02-107">Azure Data Lake Tool for Visual Studio Code.</span></span> <span data-ttu-id="95c02-108">Pokyny najdete v tématu [nástrojů pomocí Azure Data Lake pro Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span><span class="sxs-lookup"><span data-stu-id="95c02-108">For instructions, see [Use Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span></span>
- <span data-ttu-id="95c02-109">C# pro Visual Studio Code (Pokud chcete, aby místní ladění tooperform U-SQL).</span><span class="sxs-lookup"><span data-stu-id="95c02-109">C# for Visual Studio Code (if you want tooperform a U-SQL local debug).</span></span>

   ![Instalace jazyka C# v nástrojů Data Lake pro Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-install-ms-vscodecsharp.png)
   
   > [!NOTE]
   > <span data-ttu-id="95c02-111">Hello místní spuštění U-SQL a funkcí ladění aktuálně podporují pouze uživatelé s Windows.</span><span class="sxs-lookup"><span data-stu-id="95c02-111">hello U-SQL local run and debug features currently only support Windows users.</span></span> 


## <a name="set-up-hello-u-sql-local-run-environment"></a><span data-ttu-id="95c02-112">Nastavení hello U-SQL místní spuštění prostředí</span><span class="sxs-lookup"><span data-stu-id="95c02-112">Set up hello U-SQL local run environment</span></span>

1. <span data-ttu-id="95c02-113">Vyberte Ctrl + Shift + P tooopen hello příkaz palety a potom zadejte **ADL: stažení závislostí LocalRun** toodownload hello balíčky.</span><span class="sxs-lookup"><span data-stu-id="95c02-113">Select Ctrl+Shift+P tooopen hello command palette, and then enter **ADL: Download LocalRun Dependency** toodownload hello packages.</span></span>  

   ![Stáhnout balíčky ADL LocalRun závislostí hello](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/DownloadLocalRun.png)

2. <span data-ttu-id="95c02-115">Vyhledejte hello závislosti balíčků z cesty hello ukazuje hello **výstup** podokně a pak nainstalujte BuildTools a Win10SDK 10240.</span><span class="sxs-lookup"><span data-stu-id="95c02-115">Locate hello dependency packages from hello path shown in hello **Output** pane, and then install BuildTools and Win10SDK 10240.</span></span> <span data-ttu-id="95c02-116">Tady je příklad cesty:</span><span class="sxs-lookup"><span data-stu-id="95c02-116">Here is an example path:</span></span>  
`C:\Users\xxx\.vscode\extensions\usqlextpublisher.usql-vscode-ext-x.x.x\LocalRunDependency
`  
  <span data-ttu-id="95c02-117">![Vyhledejte hello závislosti balíčků](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/LocateDependencyPath.png)</span><span class="sxs-lookup"><span data-stu-id="95c02-117">![Locate hello dependency packages](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/LocateDependencyPath.png)</span></span>

   <span data-ttu-id="95c02-118">a.</span><span class="sxs-lookup"><span data-stu-id="95c02-118">a.</span></span> <span data-ttu-id="95c02-119">tooinstall BuildTools, postupujte podle pokynů Průvodce hello.</span><span class="sxs-lookup"><span data-stu-id="95c02-119">tooinstall BuildTools, follow hello wizard instructions.</span></span>   

  ![Nainstalujte BuildTools](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallBuildTools.png)

   <span data-ttu-id="95c02-121">b.</span><span class="sxs-lookup"><span data-stu-id="95c02-121">b.</span></span> <span data-ttu-id="95c02-122">tooinstall Win10SDK 10240, postupujte podle pokynů Průvodce hello.</span><span class="sxs-lookup"><span data-stu-id="95c02-122">tooinstall Win10SDK 10240, follow hello wizard instructions.</span></span>  

  ![Nainstalujte Win10SDK 10240](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallWin10SDK.png)

3. <span data-ttu-id="95c02-124">Nastavte proměnnou prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="95c02-124">Set up hello environment variable.</span></span> <span data-ttu-id="95c02-125">Sada hello **SCOPE_CPP_SDK** proměnnou prostředí:</span><span class="sxs-lookup"><span data-stu-id="95c02-125">Set hello **SCOPE_CPP_SDK** environment variable to:</span></span>  
`C:\Users\xxx\.vscode\extensions\usqlextpublisher.usql-vscode-ext-x.x.x\LocalRunDependency\CppSDK_3rdparty
`  
4. <span data-ttu-id="95c02-126">Restartujte toomake hello operačního systému se, že nastavení proměnné prostředí hello projeví.</span><span class="sxs-lookup"><span data-stu-id="95c02-126">Restart hello OS toomake sure that hello environment variable settings take effect.</span></span>  

   ![Zkontrolujte, jestli je nainstalované hello SCOPE_CPP_SDK – proměnná prostředí](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/ConfigScopeCppSDk.png)

## <a name="start-hello-local-run-service-and-submit-hello-u-sql-job-tooa-local-account"></a><span data-ttu-id="95c02-128">Spusťte místní spuštění služby hello a odeslání hello U-SQL úlohy tooa místní účet</span><span class="sxs-lookup"><span data-stu-id="95c02-128">Start hello local run service and submit hello U-SQL job tooa local account</span></span> 
<span data-ttu-id="95c02-129">Pro hello prvního uživatele, jsou výzvami toodownload hello ADL: stažení LocalRun závislosti balíčků, pokud již nejsou nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="95c02-129">For hello first-time user, you are prompted toodownload hello ADL: Download LocalRun Dependency packages if they are not already installed.</span></span>
1. <span data-ttu-id="95c02-130">Vyberte Ctrl + Shift + P tooopen hello příkaz palety a potom zadejte **ADL: spustit místní spustit službu**.</span><span class="sxs-lookup"><span data-stu-id="95c02-130">Select Ctrl+Shift+P tooopen hello command palette, and then enter **ADL: Start Local Run Service**.</span></span>
2. <span data-ttu-id="95c02-131">Vyberte **přijmout** tooaccept hello licenční podmínky softwaru společnosti Microsoft pro hello poprvé.</span><span class="sxs-lookup"><span data-stu-id="95c02-131">Select **Accept** tooaccept hello Microsoft Software License Terms for hello first time.</span></span> 

   ![Přijměte licenční podmínky softwaru společnosti Microsoft hello](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/AcceptEULA.png)   
3. <span data-ttu-id="95c02-133">Otevře se konzola cmd Hello.</span><span class="sxs-lookup"><span data-stu-id="95c02-133">hello cmd console opens.</span></span> <span data-ttu-id="95c02-134">Pro uživatele, je třeba tooenter **3**a vyhledejte hello místní složky cesta pro vstupní a výstupní data.</span><span class="sxs-lookup"><span data-stu-id="95c02-134">For first-time users, you need tooenter **3**, and then locate hello local folder path for your data input and output.</span></span> <span data-ttu-id="95c02-135">Pro další možnosti můžete použít výchozí hodnoty hello.</span><span class="sxs-lookup"><span data-stu-id="95c02-135">For other options, you can use hello default values.</span></span> 

   ![Nástroje data Lake pro Visual Studio Code místní spuštění cmd](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-cmd.png)
4. <span data-ttu-id="95c02-137">Vyberte Ctrl + Shift + P tooopen hello příkaz palety, zadejte **ADL: odeslat úlohu**a potom vyberte **místní** toosubmit hello úlohy tooyour místní účet.</span><span class="sxs-lookup"><span data-stu-id="95c02-137">Select Ctrl+Shift+P tooopen hello command palette, enter **ADL: Submit Job**, and then select **Local** toosubmit hello job tooyour local account.</span></span>

   ![Vyberte místní nástrojů data Lake pro Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-select-local.png)
5. <span data-ttu-id="95c02-139">Po odeslání úlohy hello můžete zobrazit podrobnosti o odeslání hello.</span><span class="sxs-lookup"><span data-stu-id="95c02-139">After you submit hello job, you can view hello submission details.</span></span> <span data-ttu-id="95c02-140">odeslání hello tooview podrobnosti, vyberte **jobUrl** v hello **výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="95c02-140">tooview hello submission details select **jobUrl** in hello **Output** window.</span></span> <span data-ttu-id="95c02-141">Můžete také zobrazit stav odeslání úlohy hello z konzoly cmd hello.</span><span class="sxs-lookup"><span data-stu-id="95c02-141">You can also view hello job submission status from hello cmd console.</span></span> <span data-ttu-id="95c02-142">Zadejte **7** v konzole cmd hello Pokud chcete, aby tooknow další podrobnosti úlohy.</span><span class="sxs-lookup"><span data-stu-id="95c02-142">Enter **7** in hello cmd console if you want tooknow more job details.</span></span>

   <span data-ttu-id="95c02-143">![Nástroje data Lake pro Visual Studio Code místní spuštění výstupu](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-result.png)
   ![nástroje Data Lake pro Visual Studio Code místní spuštění cmd stav](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-localrun-cmd-status.png)</span><span class="sxs-lookup"><span data-stu-id="95c02-143">![Data Lake Tools for Visual Studio Code local run output](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-result.png)
![Data Lake Tools for Visual Studio Code local run cmd status](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-localrun-cmd-status.png)</span></span> 


## <a name="start-a-local-debug-for-hello-u-sql-job"></a><span data-ttu-id="95c02-144">Spustit místní ladění pro projekt U-SQL hello</span><span class="sxs-lookup"><span data-stu-id="95c02-144">Start a local debug for hello U-SQL job</span></span>  
<span data-ttu-id="95c02-145">Pro hello prvního uživatele, jsou výzvami toodownload hello ADL: stažení LocalRun závislosti balíčků, pokud již nejsou nainstalovány.</span><span class="sxs-lookup"><span data-stu-id="95c02-145">For hello first-time user, you are prompted toodownload hello ADL: Download LocalRun Dependency packages if they are not already installed.</span></span>
  
1. <span data-ttu-id="95c02-146">Vyberte Ctrl + Shift + P tooopen hello příkaz palety a potom zadejte **ADL: spustit místní spustit službu**.</span><span class="sxs-lookup"><span data-stu-id="95c02-146">Select Ctrl+Shift+P tooopen hello command palette, and then enter **ADL: Start Local Run Service**.</span></span> <span data-ttu-id="95c02-147">Otevře se konzola cmd Hello.</span><span class="sxs-lookup"><span data-stu-id="95c02-147">hello cmd console opens.</span></span> <span data-ttu-id="95c02-148">Ujistěte se, že hello **DataRoot** nastavena.</span><span class="sxs-lookup"><span data-stu-id="95c02-148">Make sure that hello **DataRoot** is set.</span></span>
3. <span data-ttu-id="95c02-149">Nastavte zarážky v vaší C# kódu.</span><span class="sxs-lookup"><span data-stu-id="95c02-149">Set a breakpoint in your C# code-behind.</span></span>
4. <span data-ttu-id="95c02-150">V hello editor skriptů, vyberte Ctrl + Shift + P tooopen hello příkaz konzoly a pak zadejte **místní ladění** toostart služby místní ladění.</span><span class="sxs-lookup"><span data-stu-id="95c02-150">Back in hello script editor, select Ctrl+Shift+P tooopen hello command console, and then enter **Local Debug** toostart your local debug service.</span></span>

![Nástroje data Lake pro Visual Studio Code výsledek místní ladění](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-debug-result.png)


## <a name="next-steps"></a><span data-ttu-id="95c02-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="95c02-152">Next steps</span></span>
- <span data-ttu-id="95c02-153">Pomocí nástrojů Azure Data Lake pro Visual Studio Code, najdete v části [nástrojů pomocí Azure Data Lake pro Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span><span class="sxs-lookup"><span data-stu-id="95c02-153">For using Azure Data Lake Tools for Visual Studio Code, see [Use Azure Data Lake Tools for Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).</span></span>
- <span data-ttu-id="95c02-154">Získávání Začínáme informací o Data Lake Analytics, najdete v části [kurz: Začínáme s Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="95c02-154">For getting started information on Data Lake Analytics, see [Tutorial: Get started with Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).</span></span>
- <span data-ttu-id="95c02-155">Informace o nástrojů Data Lake pro Visual Studio najdete v tématu [kurz: vývoj U-SQL skriptů pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="95c02-155">For information about Data Lake Tools for Visual Studio, see [Tutorial: Develop U-SQL scripts by using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
- <span data-ttu-id="95c02-156">Hello informace týkající se vývoje sestavení najdete v tématu [sestavení vyvíjet U-SQL pro úlohy Azure Data Lake Analytics](data-lake-analytics-u-sql-develop-assemblies.md).</span><span class="sxs-lookup"><span data-stu-id="95c02-156">For hello information on developing assemblies, see [Develop U-SQL assemblies for Azure Data Lake Analytics jobs](data-lake-analytics-u-sql-develop-assemblies.md).</span></span>
