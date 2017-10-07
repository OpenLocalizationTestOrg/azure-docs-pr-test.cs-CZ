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
# <a name="u-sql-local-run-and-local-debug-with-visual-studio-code"></a>U-SQL místní spuštění a místní ladění s kódem jazyka Visual Studio

## <a name="prerequisites"></a>Požadavky
Ujistěte se, že máte hello následující požadavky splněny, než začnete těchto postupů:
- Nástroj Azure Data Lake pro Visual Studio Code. Pokyny najdete v tématu [nástrojů pomocí Azure Data Lake pro Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).
- C# pro Visual Studio Code (Pokud chcete, aby místní ladění tooperform U-SQL).

   ![Instalace jazyka C# v nástrojů Data Lake pro Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-install-ms-vscodecsharp.png)
   
   > [!NOTE]
   > Hello místní spuštění U-SQL a funkcí ladění aktuálně podporují pouze uživatelé s Windows. 


## <a name="set-up-hello-u-sql-local-run-environment"></a>Nastavení hello U-SQL místní spuštění prostředí

1. Vyberte Ctrl + Shift + P tooopen hello příkaz palety a potom zadejte **ADL: stažení závislostí LocalRun** toodownload hello balíčky.  

   ![Stáhnout balíčky ADL LocalRun závislostí hello](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/DownloadLocalRun.png)

2. Vyhledejte hello závislosti balíčků z cesty hello ukazuje hello **výstup** podokně a pak nainstalujte BuildTools a Win10SDK 10240. Tady je příklad cesty:  
`C:\Users\xxx\.vscode\extensions\usqlextpublisher.usql-vscode-ext-x.x.x\LocalRunDependency
`  
  ![Vyhledejte hello závislosti balíčků](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/LocateDependencyPath.png)

   a. tooinstall BuildTools, postupujte podle pokynů Průvodce hello.   

  ![Nainstalujte BuildTools](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallBuildTools.png)

   b. tooinstall Win10SDK 10240, postupujte podle pokynů Průvodce hello.  

  ![Nainstalujte Win10SDK 10240](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallWin10SDK.png)

3. Nastavte proměnnou prostředí hello. Sada hello **SCOPE_CPP_SDK** proměnnou prostředí:  
`C:\Users\xxx\.vscode\extensions\usqlextpublisher.usql-vscode-ext-x.x.x\LocalRunDependency\CppSDK_3rdparty
`  
4. Restartujte toomake hello operačního systému se, že nastavení proměnné prostředí hello projeví.  

   ![Zkontrolujte, jestli je nainstalované hello SCOPE_CPP_SDK – proměnná prostředí](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/ConfigScopeCppSDk.png)

## <a name="start-hello-local-run-service-and-submit-hello-u-sql-job-tooa-local-account"></a>Spusťte místní spuštění služby hello a odeslání hello U-SQL úlohy tooa místní účet 
Pro hello prvního uživatele, jsou výzvami toodownload hello ADL: stažení LocalRun závislosti balíčků, pokud již nejsou nainstalovány.
1. Vyberte Ctrl + Shift + P tooopen hello příkaz palety a potom zadejte **ADL: spustit místní spustit službu**.
2. Vyberte **přijmout** tooaccept hello licenční podmínky softwaru společnosti Microsoft pro hello poprvé. 

   ![Přijměte licenční podmínky softwaru společnosti Microsoft hello](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/AcceptEULA.png)   
3. Otevře se konzola cmd Hello. Pro uživatele, je třeba tooenter **3**a vyhledejte hello místní složky cesta pro vstupní a výstupní data. Pro další možnosti můžete použít výchozí hodnoty hello. 

   ![Nástroje data Lake pro Visual Studio Code místní spuštění cmd](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-cmd.png)
4. Vyberte Ctrl + Shift + P tooopen hello příkaz palety, zadejte **ADL: odeslat úlohu**a potom vyberte **místní** toosubmit hello úlohy tooyour místní účet.

   ![Vyberte místní nástrojů data Lake pro Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-select-local.png)
5. Po odeslání úlohy hello můžete zobrazit podrobnosti o odeslání hello. odeslání hello tooview podrobnosti, vyberte **jobUrl** v hello **výstup** okno. Můžete také zobrazit stav odeslání úlohy hello z konzoly cmd hello. Zadejte **7** v konzole cmd hello Pokud chcete, aby tooknow další podrobnosti úlohy.

   ![Nástroje data Lake pro Visual Studio Code místní spuštění výstupu](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-result.png)
   ![nástroje Data Lake pro Visual Studio Code místní spuštění cmd stav](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-localrun-cmd-status.png) 


## <a name="start-a-local-debug-for-hello-u-sql-job"></a>Spustit místní ladění pro projekt U-SQL hello  
Pro hello prvního uživatele, jsou výzvami toodownload hello ADL: stažení LocalRun závislosti balíčků, pokud již nejsou nainstalovány.
  
1. Vyberte Ctrl + Shift + P tooopen hello příkaz palety a potom zadejte **ADL: spustit místní spustit službu**. Otevře se konzola cmd Hello. Ujistěte se, že hello **DataRoot** nastavena.
3. Nastavte zarážky v vaší C# kódu.
4. V hello editor skriptů, vyberte Ctrl + Shift + P tooopen hello příkaz konzoly a pak zadejte **místní ladění** toostart služby místní ladění.

![Nástroje data Lake pro Visual Studio Code výsledek místní ladění](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-debug-result.png)


## <a name="next-steps"></a>Další kroky
- Pomocí nástrojů Azure Data Lake pro Visual Studio Code, najdete v části [nástrojů pomocí Azure Data Lake pro Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).
- Získávání Začínáme informací o Data Lake Analytics, najdete v části [kurz: Začínáme s Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).
- Informace o nástrojů Data Lake pro Visual Studio najdete v tématu [kurz: vývoj U-SQL skriptů pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Hello informace týkající se vývoje sestavení najdete v tématu [sestavení vyvíjet U-SQL pro úlohy Azure Data Lake Analytics](data-lake-analytics-u-sql-develop-assemblies.md).
