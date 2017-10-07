---
title: "Nástroje Azure Data Lake: Nástroje použití Azure Data Lake pro Visual Studio Code | Microsoft Docs"
description: "Zjistěte, jak nástrojů toouse Azure Data Lake pro Visual Studio Code toocreate, testovat a spouštět skripty U-SQL. "
Keywords: "VScode, nástroje Azure Data Lake, místní spuštění souboru úložiště místní ladění, místní ladění preview, nahrajte toostorage cesta"
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/14/2017
ms.author: jejiang
ms.openlocfilehash: 77771c5d5dae3bfce4ad2df240ea6c6ef848f288
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-data-lake-tools-for-visual-studio-code"></a><span data-ttu-id="b8970-104">Pomocí nástroje Azure Data Lake pro Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b8970-104">Use Azure Data Lake Tools for Visual Studio Code</span></span>

<span data-ttu-id="b8970-105">Zjistěte, jak nástrojů toouse Azure Data Lake pro Visual Studio Code (kód VS) toocreate, testovat a spouštět skripty U-SQL.</span><span class="sxs-lookup"><span data-stu-id="b8970-105">Learn how toouse Azure Data Lake Tools for Visual Studio Code (VS Code) toocreate, test, and run U-SQL scripts.</span></span> <span data-ttu-id="b8970-106">Hello informace jsou také obsaženy v hello následující video:</span><span class="sxs-lookup"><span data-stu-id="b8970-106">hello information is also covered in hello following video:</span></span>

<a href="https://www.youtube.com/watch?v=J_gWuyFnaGA&feature=youtu.be"><img src="./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-video.png"></a>

## <a name="prerequisites"></a><span data-ttu-id="b8970-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="b8970-107">Prerequisites</span></span>

<span data-ttu-id="b8970-108">Nástroje data Lake lze nainstalovat na hello platformách podporovaných aplikací VS Code.</span><span class="sxs-lookup"><span data-stu-id="b8970-108">Data Lake Tools can be installed on hello platforms supported by VS Code.</span></span> <span data-ttu-id="b8970-109">jsou následující Hello podporované platformy Windows, Linux a systému MacOS.</span><span class="sxs-lookup"><span data-stu-id="b8970-109">hello supported platforms include Windows, Linux, and MacOS.</span></span> <span data-ttu-id="b8970-110">různé platformy Hello mít hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="b8970-110">hello different platforms have hello following prerequisites:</span></span>

- <span data-ttu-id="b8970-111">Windows</span><span class="sxs-lookup"><span data-stu-id="b8970-111">Windows</span></span>

    - <span data-ttu-id="b8970-112">[Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="b8970-112">[Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).</span></span>
    - <span data-ttu-id="b8970-113">[Aktualizace verze 8 prostředí Java Runtime Administrátor 77 nebo novější](https://java.com/download/manual.jsp).</span><span class="sxs-lookup"><span data-stu-id="b8970-113">[Java SE Runtime Environment version 8 update 77 or later](https://java.com/download/manual.jsp).</span></span> <span data-ttu-id="b8970-114">Přidejte hello java.exe cesta toohello systému prostředí cestu k proměnné.</span><span class="sxs-lookup"><span data-stu-id="b8970-114">Add hello java.exe path toohello system environment variable path.</span></span> <span data-ttu-id="b8970-115">Pokyny ke konfiguraci, najdete v části [jak nastavit nebo změnit systémové proměnné Path hello?]( https://www.java.com/download/help/path.xml).</span><span class="sxs-lookup"><span data-stu-id="b8970-115">For configuration instructions, see [How do I set or change hello Path system variable?]( https://www.java.com/download/help/path.xml).</span></span> <span data-ttu-id="b8970-116">Cesta Hello je podobné tooC:\Program Files\Java\jdk1.8.0_77\jre\bin.</span><span class="sxs-lookup"><span data-stu-id="b8970-116">hello path is similar tooC:\Program Files\Java\jdk1.8.0_77\jre\bin.</span></span>
    - <span data-ttu-id="b8970-117">[.NET core SDK 1.0.3 nebo modul runtime .NET Core 1.1](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="b8970-117">[.NET Core SDK 1.0.3 or .NET Core 1.1 runtime](https://www.microsoft.com/net/download).</span></span>
    
- <span data-ttu-id="b8970-118">Linux (doporučujeme Ubuntu 14.04 LTS)</span><span class="sxs-lookup"><span data-stu-id="b8970-118">Linux (We recommend Ubuntu 14.04 LTS)</span></span>

    - <span data-ttu-id="b8970-119">[Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="b8970-119">[Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).</span></span> <span data-ttu-id="b8970-120">tooinstall hello kód, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="b8970-120">tooinstall hello code, enter hello following command:</span></span>

              sudo dpkg -i code_<version_number>_amd64.deb

    - <span data-ttu-id="b8970-121">[Monofonní 4.2.x](http://www.mono-project.com/docs/getting-started/install/linux/).</span><span class="sxs-lookup"><span data-stu-id="b8970-121">[Mono 4.2.x](http://www.mono-project.com/docs/getting-started/install/linux/).</span></span> 

        - <span data-ttu-id="b8970-122">balíček bázi deb hello tooupdate zdroje, zadejte hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="b8970-122">tooupdate hello deb package source, enter hello following commands:</span></span>

                sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
                echo "deb http://download.mono-project.com/repo/debian wheezy/snapshots 4.2.4.4/main" | sudo tee /etc/apt/sources.list.d/mono-xamarin.list
                sudo apt-get update

        - <span data-ttu-id="b8970-123">tooinstall Mono, zadejte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="b8970-123">tooinstall Mono, enter hello following command:</span></span>

                sudo apt-get install mono-complete

            > [!NOTE] 
            > <span data-ttu-id="b8970-124">Mono 4.6 není podporována.</span><span class="sxs-lookup"><span data-stu-id="b8970-124">Mono 4.6 is not supported.</span></span> <span data-ttu-id="b8970-125">Odinstalujte verzi 4.6 zcela, před instalací 4.2.x.</span><span class="sxs-lookup"><span data-stu-id="b8970-125">Uninstall version 4.6 entirely before you install 4.2.x.</span></span>  

        - <span data-ttu-id="b8970-126">[Aktualizace verze 8 prostředí Java Runtime Administrátor 77 nebo novější](https://java.com/download/manual.jsp).</span><span class="sxs-lookup"><span data-stu-id="b8970-126">[Java SE Runtime Environment version 8 update 77 or later](https://java.com/download/manual.jsp).</span></span> <span data-ttu-id="b8970-127">Pokyny k instalaci naleznete v tématu hello [Linux 64-bit pokyny k instalaci pro jazyk Java]( https://java.com/en/download/help/linux_x64_install.xml) stránky.</span><span class="sxs-lookup"><span data-stu-id="b8970-127">For instructions on installation, see hello [Linux 64-bit installation instructions for Java]( https://java.com/en/download/help/linux_x64_install.xml) page.</span></span>
        - <span data-ttu-id="b8970-128">[.NET core SDK 1.0.3 nebo modul runtime .NET Core 1.1](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="b8970-128">[.NET Core SDK 1.0.3 or .NET Core 1.1 runtime](https://www.microsoft.com/net/download).</span></span>
- <span data-ttu-id="b8970-129">MacOS</span><span class="sxs-lookup"><span data-stu-id="b8970-129">MacOS</span></span>

    - <span data-ttu-id="b8970-130">[Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="b8970-130">[Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).</span></span>
    - <span data-ttu-id="b8970-131">[Mono 4.2.4](http://download.mono-project.com/archive/4.2.4/macos-10-x86/).</span><span class="sxs-lookup"><span data-stu-id="b8970-131">[Mono 4.2.4](http://download.mono-project.com/archive/4.2.4/macos-10-x86/).</span></span> 
    - <span data-ttu-id="b8970-132">[Aktualizace verze 8 prostředí Java Runtime Administrátor 77 nebo novější](https://java.com/download/manual.jsp).</span><span class="sxs-lookup"><span data-stu-id="b8970-132">[Java SE Runtime Environment version 8 update 77 or later](https://java.com/download/manual.jsp).</span></span> <span data-ttu-id="b8970-133">Pokyny k instalaci naleznete v tématu hello [Linux 64-bit pokyny k instalaci pro jazyk Java](https://java.com/en/download/help/mac_install.xml) stránky.</span><span class="sxs-lookup"><span data-stu-id="b8970-133">For instructions on installation, see hello [Linux 64-bit installation instructions for Java](https://java.com/en/download/help/mac_install.xml) page.</span></span>
    - <span data-ttu-id="b8970-134">[.NET core SDK 1.0.3 nebo modul runtime .NET Core 1.1](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="b8970-134">[.NET Core SDK 1.0.3 or .NET Core 1.1 runtime](https://www.microsoft.com/net/download).</span></span>

## <a name="install-data-lake-tools"></a><span data-ttu-id="b8970-135">Nainstalovat nástroje Data Lake</span><span class="sxs-lookup"><span data-stu-id="b8970-135">Install Data Lake Tools</span></span>

<span data-ttu-id="b8970-136">Po instalaci hello požadavky, můžete nainstalovat nástroje Data Lake pro VS Code.</span><span class="sxs-lookup"><span data-stu-id="b8970-136">After you install hello prerequisites, you can install Data Lake Tools for VS Code.</span></span>

<span data-ttu-id="b8970-137">**tooinstall nástrojů Data Lake**</span><span class="sxs-lookup"><span data-stu-id="b8970-137">**tooinstall Data Lake Tools**</span></span>

1. <span data-ttu-id="b8970-138">Otevřete Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="b8970-138">Open Visual Studio Code.</span></span>
2. <span data-ttu-id="b8970-139">Vyberte Ctrl + P a potom zadejte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b8970-139">Select Ctrl+P, and then enter hello following command:</span></span>
```
ext install usql-vscode-ext
```
<span data-ttu-id="b8970-140">Zobrazí se seznam rozšíření kódu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b8970-140">You can see a list of Visual Studio code extensions.</span></span> <span data-ttu-id="b8970-141">Jeden z nich je **nástrojů Azure Data Lake**.</span><span class="sxs-lookup"><span data-stu-id="b8970-141">One of them is **Azure Data Lake Tools**.</span></span>

3. <span data-ttu-id="b8970-142">Vyberte **nainstalovat** další příliš**nástrojů Azure Data Lake**.</span><span class="sxs-lookup"><span data-stu-id="b8970-142">Select **Install** next too**Azure Data Lake Tools**.</span></span> <span data-ttu-id="b8970-143">Za několik sekund, hello **nainstalovat** tlačítko změny příliš**opětovného načtení**.</span><span class="sxs-lookup"><span data-stu-id="b8970-143">After a few seconds, hello **Install** button changes too**Reload**.</span></span>
4. <span data-ttu-id="b8970-144">Vyberte **opětovného načtení** tooactivate hello rozšíření.</span><span class="sxs-lookup"><span data-stu-id="b8970-144">Select **Reload** tooactivate hello extension.</span></span>
5. <span data-ttu-id="b8970-145">Vyberte **OK** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="b8970-145">Select **OK** tooconfirm.</span></span> <span data-ttu-id="b8970-146">Zobrazí nástroje Azure Data Lake v hello **rozšíření** podokně.</span><span class="sxs-lookup"><span data-stu-id="b8970-146">You can see Azure Data Lake Tools in hello **Extensions** pane.</span></span>
    <span data-ttu-id="b8970-147">![Nástroje data Lake pro Visual Studio rozšíření kódu podokno](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extensions.png)</span><span class="sxs-lookup"><span data-stu-id="b8970-147">![Data Lake Tools for Visual Studio Code Extensions pane](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extensions.png)</span></span>

## <a name="activate-azure-data-lake-tools"></a><span data-ttu-id="b8970-148">Aktivovat nástrojů Azure Data Lake</span><span class="sxs-lookup"><span data-stu-id="b8970-148">Activate Azure Data Lake Tools</span></span>
<span data-ttu-id="b8970-149">Vytvořit nový soubor .usql nebo otevřít existující tooactivate hello příponu souboru .usql.</span><span class="sxs-lookup"><span data-stu-id="b8970-149">Create a new .usql file or open an existing .usql file tooactivate hello extension.</span></span> 

## <a name="connect-tooazure"></a><span data-ttu-id="b8970-150">Připojit tooAzure</span><span class="sxs-lookup"><span data-stu-id="b8970-150">Connect tooAzure</span></span>

<span data-ttu-id="b8970-151">Předtím, než můžete zkompilování a spuštění skriptů U-SQL v Data Lake Analytics, je nutné připojit tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="b8970-151">Before you can compile and run U-SQL scripts in Data Lake Analytics, you must connect tooyour Azure account.</span></span>

<span data-ttu-id="b8970-152">**tooconnect tooAzure**</span><span class="sxs-lookup"><span data-stu-id="b8970-152">**tooconnect tooAzure**</span></span>

1.  <span data-ttu-id="b8970-153">Vyberte Ctrl + Shift + P tooopen hello příkaz palety.</span><span class="sxs-lookup"><span data-stu-id="b8970-153">Select Ctrl+Shift+P tooopen hello command palette.</span></span> 
2.  <span data-ttu-id="b8970-154">Zadejte **ADL: přihlášení**.</span><span class="sxs-lookup"><span data-stu-id="b8970-154">Enter **ADL: Login**.</span></span> <span data-ttu-id="b8970-155">Hello přihlašovací informace se zobrazí v hello **výstup** podokně.</span><span class="sxs-lookup"><span data-stu-id="b8970-155">hello login information appears in hello **Output** pane.</span></span>

    <span data-ttu-id="b8970-156">![Nástroje data Lake pro Visual Studio Code příkaz palety](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login.png)
    ![nástroje Data Lake pro Visual Studio Code zařízení přihlašovací informace](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-login-info.png)</span><span class="sxs-lookup"><span data-stu-id="b8970-156">![Data Lake Tools for Visual Studio Code command palette](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login.png)
![Data Lake Tools for Visual Studio Code device login information](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-login-info.png)</span></span>
3. <span data-ttu-id="b8970-157">Vyberte Ctrl + klikněte na adresu URL pro přihlášení k hello: https://aka.ms/devicelogin tooopen hello přihlašovací webovou stránku.</span><span class="sxs-lookup"><span data-stu-id="b8970-157">Select Ctrl+click on hello login URL: https://aka.ms/devicelogin tooopen hello login webpage.</span></span> <span data-ttu-id="b8970-158">Zadejte kód hello **G567LX42V** do hello textového pole a pak vyberte **pokračovat**.</span><span class="sxs-lookup"><span data-stu-id="b8970-158">Enter hello code **G567LX42V** into hello text box, and then select **Continue**.</span></span>

   ![Nástroje data Lake pro Visual Studio Code přihlášení vložte kód](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login-paste-code.png )   
4.  <span data-ttu-id="b8970-160">Postupujte podle pokynů toosign hello v z webovou stránku hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-160">Follow hello instructions toosign in from hello webpage.</span></span> <span data-ttu-id="b8970-161">Když jste připojení, název účtu Azure se zobrazí na stavovém řádku hello v levém dolním rohu hello hello **VS Code** okno.</span><span class="sxs-lookup"><span data-stu-id="b8970-161">When you're connected, your Azure account name appears on hello status bar in hello lower-left corner of hello **VS Code** window.</span></span> 

    > [!NOTE] 
    > <span data-ttu-id="b8970-162">Pokud váš účet má dvě úrovně povoleno, doporučujeme použít ověřování phone místo pomocí PIN kódu.</span><span class="sxs-lookup"><span data-stu-id="b8970-162">If your account has two factors enabled, we recommend that you use phone authentication rather than using a PIN.</span></span>

<span data-ttu-id="b8970-163">toosign limitu, zadejte příkaz hello **ADL: odhlášení**.</span><span class="sxs-lookup"><span data-stu-id="b8970-163">toosign out, enter hello command **ADL: Logout**.</span></span>

## <a name="list-your-data-lake-analytics-accounts"></a><span data-ttu-id="b8970-164">Seznam svých účtů Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="b8970-164">List your Data Lake Analytics accounts</span></span>

<span data-ttu-id="b8970-165">tootest hello připojení, get seznam svých účtů Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="b8970-165">tootest hello connection, get a list of your Data Lake Analytics accounts.</span></span>

<span data-ttu-id="b8970-166">**toolist hello účtů Data Lake Analytics v rámci vašeho předplatného Azure**</span><span class="sxs-lookup"><span data-stu-id="b8970-166">**toolist hello Data Lake Analytics accounts under your Azure subscription**</span></span>

1. <span data-ttu-id="b8970-167">Vyberte Ctrl + Shift + P tooopen hello příkaz palety.</span><span class="sxs-lookup"><span data-stu-id="b8970-167">Select Ctrl+Shift+P tooopen hello command palette.</span></span>
2. <span data-ttu-id="b8970-168">Zadejte **ADL: seznamu účtů**.</span><span class="sxs-lookup"><span data-stu-id="b8970-168">Enter **ADL: List Accounts**.</span></span> <span data-ttu-id="b8970-169">účty Hello se zobrazí v hello **výstup** podokně.</span><span class="sxs-lookup"><span data-stu-id="b8970-169">hello accounts appear in hello **Output** pane.</span></span>

## <a name="open-hello-sample-script"></a><span data-ttu-id="b8970-170">Otevřete hello ukázkový skript</span><span class="sxs-lookup"><span data-stu-id="b8970-170">Open hello sample script</span></span>
<span data-ttu-id="b8970-171">Spusťte příkaz palety hello (Ctrl + Shift + P) a zadejte **ADL: Otevřete ukázkový skript**.</span><span class="sxs-lookup"><span data-stu-id="b8970-171">Open hello command palette (Ctrl+Shift+P) and enter **ADL: Open Sample Script**.</span></span> <span data-ttu-id="b8970-172">Otevře další instanci této ukázce.</span><span class="sxs-lookup"><span data-stu-id="b8970-172">This opens another instance of this sample.</span></span> <span data-ttu-id="b8970-173">Můžete také upravit, konfigurovat a odeslat skript v této instanci.</span><span class="sxs-lookup"><span data-stu-id="b8970-173">You can also edit, configure, and submit script on this instance.</span></span>

## <a name="work-with-u-sql"></a><span data-ttu-id="b8970-174">Práce s jazykem U-SQL</span><span class="sxs-lookup"><span data-stu-id="b8970-174">Work with U-SQL</span></span>

<span data-ttu-id="b8970-175">U-SQL souboru nebo složky toowork třeba otevřete pomocí U-SQL.</span><span class="sxs-lookup"><span data-stu-id="b8970-175">You need open either a U-SQL file or a folder toowork with U-SQL.</span></span>

<span data-ttu-id="b8970-176">**tooopen do složky pro váš projekt U-SQL**</span><span class="sxs-lookup"><span data-stu-id="b8970-176">**tooopen a folder for your U-SQL project**</span></span>

1. <span data-ttu-id="b8970-177">Visual Studio Code, vyberte hello **soubor** nabídce a potom vyberte **otevřít složku**.</span><span class="sxs-lookup"><span data-stu-id="b8970-177">From Visual Studio Code, select hello **File** menu, and then select **Open Folder**.</span></span>
2. <span data-ttu-id="b8970-178">Zadejte složku a potom vyberte **vyberte složku**.</span><span class="sxs-lookup"><span data-stu-id="b8970-178">Specify a folder, and then select **Select Folder**.</span></span>
3. <span data-ttu-id="b8970-179">Vyberte hello **soubor** nabídce a potom vyberte **nový**.</span><span class="sxs-lookup"><span data-stu-id="b8970-179">Select hello **File** menu, and then select **New**.</span></span> <span data-ttu-id="b8970-180">Soubor bez názvu-1 se přidá toohello projektu.</span><span class="sxs-lookup"><span data-stu-id="b8970-180">An Untitled-1 file is added toohello project.</span></span>
4. <span data-ttu-id="b8970-181">Zadejte následující kód do souboru hello bez názvu-1 hello:</span><span class="sxs-lookup"><span data-stu-id="b8970-181">Enter hello following code into hello Untitled-1 file:</span></span>

        @departments  = 
            SELECT * FROM 
                (VALUES
                    (31,    "Sales"),
                    (33,    "Engineering"), 
                    (34,    "Clerical"),
                    (35,    "Marketing")
                ) AS 
                      D( DepID, DepName );
         
        OUTPUT @departments
            TO “/Output/departments.csv”

    <span data-ttu-id="b8970-182">skript Hello vytvoří soubor departments.csv s některé dat obsažených ve složce/Output hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-182">hello script creates a departments.csv file with some data included in hello /output folder.</span></span>

5. <span data-ttu-id="b8970-183">Uložte soubor hello jako **myUSQL.usql** v hello otevřít složku.</span><span class="sxs-lookup"><span data-stu-id="b8970-183">Save hello file as **myUSQL.usql** in hello opened folder.</span></span> <span data-ttu-id="b8970-184">Konfigurační soubor adltools_settings.json je taky přidaná toohello projektu.</span><span class="sxs-lookup"><span data-stu-id="b8970-184">A adltools_settings.json configuration file is also added toohello project.</span></span>
4. <span data-ttu-id="b8970-185">Otevřít a konfigurovat adltools_settings.json s hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="b8970-185">Open and configure adltools_settings.json with hello following properties:</span></span>

    - <span data-ttu-id="b8970-186">Účet: Účet Data Lake Analytics v rámci vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="b8970-186">Account:  A Data Lake Analytics account under your Azure subscription.</span></span>
    - <span data-ttu-id="b8970-187">Databáze: Databáze v rámci vašeho účtu.</span><span class="sxs-lookup"><span data-stu-id="b8970-187">Database: A database under your account.</span></span> <span data-ttu-id="b8970-188">Výchozí hodnota Hello je **hlavní**.</span><span class="sxs-lookup"><span data-stu-id="b8970-188">hello default is **master**.</span></span>
    - <span data-ttu-id="b8970-189">Schémat: Schéma v databázi.</span><span class="sxs-lookup"><span data-stu-id="b8970-189">Schema: A schema under your database.</span></span> <span data-ttu-id="b8970-190">Výchozí hodnota Hello je **dbo**.</span><span class="sxs-lookup"><span data-stu-id="b8970-190">hello default is **dbo**.</span></span>
    - <span data-ttu-id="b8970-191">Volitelné nastavení:</span><span class="sxs-lookup"><span data-stu-id="b8970-191">Optional settings:</span></span>
        - <span data-ttu-id="b8970-192">Priorita: rozsah priority hello je z 1 too1000 s 1 jako hello nejvyšší prioritou.</span><span class="sxs-lookup"><span data-stu-id="b8970-192">Priority: hello priority range is from 1 too1000 with 1 as hello highest priority.</span></span> <span data-ttu-id="b8970-193">Hello výchozí hodnota je **1000**.</span><span class="sxs-lookup"><span data-stu-id="b8970-193">hello default value is **1000**.</span></span>
        - <span data-ttu-id="b8970-194">Paralelismus: hello paralelismus rozsah je od 1 too150.</span><span class="sxs-lookup"><span data-stu-id="b8970-194">Parallelism: hello parallelism range is from 1 too150.</span></span> <span data-ttu-id="b8970-195">Hello výchozí hodnota je hello maximální paralelismus v účtu Azure Data Lake Analytics povolen.</span><span class="sxs-lookup"><span data-stu-id="b8970-195">hello default value is hello maximum parallelism allowed in your Azure Data Lake Analytics account.</span></span> 
        
        > [!NOTE] 
        > <span data-ttu-id="b8970-196">Pokud jsou neplatné nastavení hello, použijí se výchozí hodnoty hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-196">If hello settings are invalid, hello default values are used.</span></span>

    ![Nástroje data Lake pro Visual Studio Code konfiguračního souboru](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-configuration-file.png)

    <span data-ttu-id="b8970-198">Výpočetní, účet Data Lake Analytics je potřeba toocompile a spuštění úloh U-SQL.</span><span class="sxs-lookup"><span data-stu-id="b8970-198">A compute Data Lake Analytics account is needed toocompile and run U-SQL jobs.</span></span> <span data-ttu-id="b8970-199">Předtím, než můžete zkompilování a spuštění úloh U-SQL, je nutné nakonfigurovat účet počítače hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-199">You must configure hello computer account before you can compile and run U-SQL jobs.</span></span>
    
<span data-ttu-id="b8970-200">Po uložení konfigurace hello hello účet, databázi a schéma informace se zobrazí na stavovém řádku hello v hello okraje v levém dolním rohu odpovídající soubor .usql hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-200">After hello configuration is saved, hello account, database, and schema information appears on hello status bar at hello bottom-left corner of hello corresponding .usql file.</span></span> 
 
 
<span data-ttu-id="b8970-201">Porovnání tooopening soubor, když otevřete složku, kterou můžete:</span><span class="sxs-lookup"><span data-stu-id="b8970-201">Compared tooopening a file, when you open a folder you can:</span></span>

- <span data-ttu-id="b8970-202">Použijte soubor kódu.</span><span class="sxs-lookup"><span data-stu-id="b8970-202">Use a code-behind file.</span></span> <span data-ttu-id="b8970-203">V režimu jednoho souboru hello nepodporuje kódu.</span><span class="sxs-lookup"><span data-stu-id="b8970-203">In hello single-file mode, code-behind is not supported.</span></span>
- <span data-ttu-id="b8970-204">Pomocí konfiguračního souboru.</span><span class="sxs-lookup"><span data-stu-id="b8970-204">Use a configuration file.</span></span> <span data-ttu-id="b8970-205">Když otevřete složku, hello skripty v hello pracovní složky sdílet jednom konfiguračním souboru.</span><span class="sxs-lookup"><span data-stu-id="b8970-205">When you open a folder, hello scripts in hello working folder share a single configuration file.</span></span>


<span data-ttu-id="b8970-206">Hello skript U-SQL zkompiluje vzdáleně přes hello služby Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="b8970-206">hello U-SQL script compiles remotely through hello Data Lake Analytics service.</span></span> <span data-ttu-id="b8970-207">Pokud vydáte hello **zkompilovat** příkaz hello skript U-SQL je odeslán tooyour účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="b8970-207">When you issue hello **compile** command, hello U-SQL script is sent tooyour Data Lake Analytics account.</span></span> <span data-ttu-id="b8970-208">Visual Studio Code později, obdrží hello kompilace výsledek.</span><span class="sxs-lookup"><span data-stu-id="b8970-208">Later, Visual Studio Code receives hello compilation result.</span></span> <span data-ttu-id="b8970-209">Kvůli toohello vzdálené kompilace Visual Studio Code vyžaduje, že uvedete hello informace tooconnect tooyour účtu Data Lake Analytics v konfiguračním souboru hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-209">Due toohello remote compilation, Visual Studio Code requires that you list hello information tooconnect tooyour Data Lake Analytics account in hello configuration file.</span></span>

<span data-ttu-id="b8970-210">**skript toocompile U-SQL**</span><span class="sxs-lookup"><span data-stu-id="b8970-210">**toocompile a U-SQL script**</span></span>

1. <span data-ttu-id="b8970-211">Vyberte Ctrl + Shift + P tooopen hello příkaz palety.</span><span class="sxs-lookup"><span data-stu-id="b8970-211">Select Ctrl+Shift+P tooopen hello command palette.</span></span> 
2. <span data-ttu-id="b8970-212">Zadejte **ADL: kompilace skriptu**.</span><span class="sxs-lookup"><span data-stu-id="b8970-212">Enter **ADL: Compile Script**.</span></span> <span data-ttu-id="b8970-213">Hello kompilace výsledky se zobrazí v hello **výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="b8970-213">hello compile results appear in hello **Output** window.</span></span> <span data-ttu-id="b8970-214">Můžete také klikněte pravým tlačítkem na soubor skriptu a potom vyberte **ADL: kompilace skriptu** úlohy toocompile U-SQL.</span><span class="sxs-lookup"><span data-stu-id="b8970-214">You can also right-click a script file, and then select **ADL: Compile Script** toocompile a U-SQL job.</span></span> <span data-ttu-id="b8970-215">Výsledek kompilace Hello se zobrazí v hello **výstup** podokně.</span><span class="sxs-lookup"><span data-stu-id="b8970-215">hello compilation result appears in hello **Output** pane.</span></span>
 

<span data-ttu-id="b8970-216">**skript toosubmit U-SQL**</span><span class="sxs-lookup"><span data-stu-id="b8970-216">**toosubmit a U-SQL script**</span></span>

1. <span data-ttu-id="b8970-217">Vyberte Ctrl + Shift + P tooopen hello příkaz palety.</span><span class="sxs-lookup"><span data-stu-id="b8970-217">Select Ctrl+Shift+P tooopen hello command palette.</span></span> 
2. <span data-ttu-id="b8970-218">Zadejte **ADL: odeslat úlohu**.</span><span class="sxs-lookup"><span data-stu-id="b8970-218">Enter **ADL: Submit Job**.</span></span>  <span data-ttu-id="b8970-219">Můžete také klikněte pravým tlačítkem na soubor skriptu a potom vyberte **ADL: odeslat úlohu**.</span><span class="sxs-lookup"><span data-stu-id="b8970-219">You can also right-click a script file, and then select **ADL: Submit Job**.</span></span> 

<span data-ttu-id="b8970-220">Po odeslání úlohy U-SQL hello odeslání protokolů se objeví v hello **výstup** okno v produktu VS Code.</span><span class="sxs-lookup"><span data-stu-id="b8970-220">After you submit a U-SQL job, hello submission logs appear in hello **Output** window in VS Code.</span></span> <span data-ttu-id="b8970-221">Pokud hello odeslání úspěšné, se zobrazí také adresu URL hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="b8970-221">If hello submission is successful, hello job URL appears as well.</span></span> <span data-ttu-id="b8970-222">Adresa URL hello úlohy lze otevřít v stav v reálném čase úlohy hello tootrack webového prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="b8970-222">You can open hello job URL in a web browser tootrack hello real-time job status.</span></span>

<span data-ttu-id="b8970-223">výstup hello tooenable hello podrobnosti úlohy, nastavte **jobInformationOutputPath** v hello **vs kód hello u-sql_settings.json** souboru.</span><span class="sxs-lookup"><span data-stu-id="b8970-223">tooenable hello output of hello job details, set **jobInformationOutputPath** in hello **vs code for hello u-sql_settings.json** file.</span></span>
 
## <a name="use-a-code-behind-file"></a><span data-ttu-id="b8970-224">Použít soubor kódu</span><span class="sxs-lookup"><span data-stu-id="b8970-224">Use a code-behind file</span></span>

<span data-ttu-id="b8970-225">Soubor kódu je soubor C# přidružené jednoho skriptu U-SQL.</span><span class="sxs-lookup"><span data-stu-id="b8970-225">A code-behind file is a C# file associated with a single U-SQL script.</span></span> <span data-ttu-id="b8970-226">V souboru kódu na pozadí hello můžete definovat skriptu vyhrazené tooUDO, UDA, UDT a UDF.</span><span class="sxs-lookup"><span data-stu-id="b8970-226">You can define a script dedicated tooUDO, UDA, UDT, and UDF in hello code-behind file.</span></span> <span data-ttu-id="b8970-227">Hello UDO, UDA, UDT a UDF lze přímo ve skriptu hello bez registrace sestavení hello nejdřív.</span><span class="sxs-lookup"><span data-stu-id="b8970-227">hello UDO, UDA, UDT, and UDF can be used directly in hello script without registering hello assembly first.</span></span> <span data-ttu-id="b8970-228">Hello souboru kódu na pozadí uveden hello stejné složce jako jeho partnerského vztahu soubor skriptu U-SQL.</span><span class="sxs-lookup"><span data-stu-id="b8970-228">hello code-behind file is put in hello same folder as its peering U-SQL script file.</span></span> <span data-ttu-id="b8970-229">Pokud skript hello název xxx.usql, hello kódu je pojmenována jako xxx.usql.cs.</span><span class="sxs-lookup"><span data-stu-id="b8970-229">If hello script is named xxx.usql, hello code-behind is named as xxx.usql.cs.</span></span> <span data-ttu-id="b8970-230">Pokud odstraníte ručně hello souboru kódu na pozadí, hello kódu funkce je vypnutá pro jeho přidružené skript U-SQL.</span><span class="sxs-lookup"><span data-stu-id="b8970-230">If you manually delete hello code-behind file, hello code-behind feature is disabled for its associated U-SQL script.</span></span> <span data-ttu-id="b8970-231">Další informace o psaní kódu zákazníka pro skript U-SQL najdete v tématu [zápis a pomocí vlastní kód v U-SQL: funkce definované uživatelem]( https://blogs.msdn.microsoft.com/visualstudio/2015/10/28/writing-and-using-custom-code-in-u-sql-user-defined-functions/).</span><span class="sxs-lookup"><span data-stu-id="b8970-231">For more information about writing customer code for U-SQL script, see [Writing and Using Custom Code in U-SQL: User-Defined Functions]( https://blogs.msdn.microsoft.com/visualstudio/2015/10/28/writing-and-using-custom-code-in-u-sql-user-defined-functions/).</span></span>

<span data-ttu-id="b8970-232">toosupport kódu, je nutné otevřít pracovní složky.</span><span class="sxs-lookup"><span data-stu-id="b8970-232">toosupport code-behind, you must open a working folder.</span></span> 

<span data-ttu-id="b8970-233">**toogenerate souboru kódu na pozadí**</span><span class="sxs-lookup"><span data-stu-id="b8970-233">**toogenerate a code-behind file**</span></span>

1. <span data-ttu-id="b8970-234">Otevřete zdrojový soubor.</span><span class="sxs-lookup"><span data-stu-id="b8970-234">Open a source file.</span></span> 
2. <span data-ttu-id="b8970-235">Vyberte Ctrl + Shift + P tooopen hello příkaz palety.</span><span class="sxs-lookup"><span data-stu-id="b8970-235">Select Ctrl+Shift+P tooopen hello command palette.</span></span>
3. <span data-ttu-id="b8970-236">Zadejte **ADL: vygenerování kódu na pozadí**.</span><span class="sxs-lookup"><span data-stu-id="b8970-236">Enter **ADL: Generate Code Behind**.</span></span> <span data-ttu-id="b8970-237">Soubor kódu se vytvoří v hello stejné složce.</span><span class="sxs-lookup"><span data-stu-id="b8970-237">A code-behind file is created in hello same folder.</span></span> 

<span data-ttu-id="b8970-238">Můžete také klikněte pravým tlačítkem na soubor skriptu a potom vyberte **ADL: generování kódu za**.</span><span class="sxs-lookup"><span data-stu-id="b8970-238">You can also right-click a script file, and then select **ADL: Generate Code Behind**.</span></span> 

<span data-ttu-id="b8970-239">toocompile a odeslat skript U-SQL pomocí souboru kódu je hello stejná hodnota jako soubor skriptu hello samostatné U-SQL.</span><span class="sxs-lookup"><span data-stu-id="b8970-239">toocompile and submit a U-SQL script with a code-behind file is hello same as with hello standalone U-SQL script file.</span></span>

<span data-ttu-id="b8970-240">Hello následující dva snímky obrazovky ukazují souboru kódu na pozadí a jeho přidružený soubor skriptu U-SQL:</span><span class="sxs-lookup"><span data-stu-id="b8970-240">hello following two screenshots show a code-behind file and its associated U-SQL script file:</span></span>
 
![Nástroje data Lake pro Visual Studio Code kódu](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-behind.png)

![Soubor skriptu nástrojů data Lake pro Visual Studio Code kódu](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-behind-call.png) 

## <a name="use-assemblies"></a><span data-ttu-id="b8970-243">Použití sestavení</span><span class="sxs-lookup"><span data-stu-id="b8970-243">Use assemblies</span></span>

<span data-ttu-id="b8970-244">Informace týkající se vývoje sestavení najdete v tématu [sestavení vyvíjet U-SQL pro úlohy Azure Data Lake Analytics](data-lake-analytics-u-sql-develop-assemblies.md).</span><span class="sxs-lookup"><span data-stu-id="b8970-244">For information on developing assemblies, see [Develop U-SQL assemblies for Azure Data Lake Analytics jobs](data-lake-analytics-u-sql-develop-assemblies.md).</span></span>

<span data-ttu-id="b8970-245">Nástroje Data Lake tooregister vlastní kód sestavení můžete použít v katalogu Data Lake Analytics hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-245">You can use Data Lake Tools tooregister custom code assemblies in hello Data Lake Analytics catalog.</span></span>

<span data-ttu-id="b8970-246">**tooregister sestavení**</span><span class="sxs-lookup"><span data-stu-id="b8970-246">**tooregister an assembly**</span></span>

<span data-ttu-id="b8970-247">Můžete zaregistrovat sestavení hello prostřednictvím hello **ADL: registrace sestavení** nebo **ADL: registrace sestavení prostřednictvím konfigurace** příkazy.</span><span class="sxs-lookup"><span data-stu-id="b8970-247">You can register hello assembly through hello **ADL: Register Assembly** or **ADL: Register Assembly through Configuration** commands.</span></span>

<span data-ttu-id="b8970-248">**tooregister prostřednictvím hello ADL: příkaz registrace sestavení**</span><span class="sxs-lookup"><span data-stu-id="b8970-248">**tooregister through hello ADL: Register Assembly command**</span></span>
1.  <span data-ttu-id="b8970-249">Vyberte Ctrl + Shift + P tooopen hello příkaz palety.</span><span class="sxs-lookup"><span data-stu-id="b8970-249">Select Ctrl+Shift+P tooopen hello command palette.</span></span>
2.  <span data-ttu-id="b8970-250">Zadejte **ADL: registrace sestavení**.</span><span class="sxs-lookup"><span data-stu-id="b8970-250">Enter **ADL: Register Assembly**.</span></span> 
3.  <span data-ttu-id="b8970-251">Zadejte cestu k místní sestavení hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-251">Specify hello local assembly path.</span></span> 
4.  <span data-ttu-id="b8970-252">Vyberte účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="b8970-252">Select a Data Lake Analytics account.</span></span>
5.  <span data-ttu-id="b8970-253">Vyberte databázi.</span><span class="sxs-lookup"><span data-stu-id="b8970-253">Select a database.</span></span>

<span data-ttu-id="b8970-254">Výsledky: portál hello je otevřen v prohlížeči a zobrazí procesu registrace sestavení hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-254">Results: hello portal is opened in a browser and displays hello assembly registration process.</span></span>  

<span data-ttu-id="b8970-255">Jiné hello tootrigger pohodlný způsob **ADL: registrace sestavení** příkaz je soubor .dll hello tooright klikněte v Průzkumníku souborů.</span><span class="sxs-lookup"><span data-stu-id="b8970-255">Another convenient way tootrigger hello **ADL: Register Assembly** command is tooright-click hello .dll file in File Explorer.</span></span> 

<span data-ttu-id="b8970-256">**tooregister ale hello ADL: registrace sestavení pomocí příkazu konfigurace**</span><span class="sxs-lookup"><span data-stu-id="b8970-256">**tooregister though hello ADL: Register Assembly through Configuration command**</span></span>
1.  <span data-ttu-id="b8970-257">Vyberte Ctrl + Shift + P tooopen hello příkaz palety.</span><span class="sxs-lookup"><span data-stu-id="b8970-257">Select Ctrl+Shift+P tooopen hello command palette.</span></span>
2.  <span data-ttu-id="b8970-258">Zadejte **ADL: registrace sestavení prostřednictvím konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="b8970-258">Enter **ADL: Register Assembly through Configuration**.</span></span> 
3.  <span data-ttu-id="b8970-259">Zadejte cestu k místní sestavení hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-259">Specify hello local assembly path.</span></span> 
4.  <span data-ttu-id="b8970-260">Zobrazí se soubor JSON Hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-260">hello JSON file is displayed.</span></span> <span data-ttu-id="b8970-261">Zkontrolujte a v případě potřeby upravit hello sestavení závislosti a parametry prostředků.</span><span class="sxs-lookup"><span data-stu-id="b8970-261">Review and edit hello assembly dependencies and resource parameters, if needed.</span></span> <span data-ttu-id="b8970-262">Pokyny, které se zobrazují v hello **výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="b8970-262">Instructions are displayed in hello **Output** window.</span></span> <span data-ttu-id="b8970-263">tooproceed toohello sestavení registrace, uložte soubor JSON hello (Ctrl + S).</span><span class="sxs-lookup"><span data-stu-id="b8970-263">tooproceed toohello assembly registration, save (Ctrl+S) hello JSON file.</span></span>

![Nástroje data Lake pro Visual Studio Code kódu](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-register-assembly-advance.png)
>[!NOTE]
>- <span data-ttu-id="b8970-265">Závislosti sestavení: automatické nástrojů Azure Data Lake jestli hello DLL má všechny závislosti.</span><span class="sxs-lookup"><span data-stu-id="b8970-265">Assembly dependencies: Azure Data Lake Tools autodetects whether hello DLL has any dependencies.</span></span> <span data-ttu-id="b8970-266">Po zjištění, zobrazí se Hello závislosti v souboru JSON hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-266">hello dependencies are displayed in hello JSON file after they are detected.</span></span> 
>- <span data-ttu-id="b8970-267">Prostředky: Jako součást registrace sestavení hello můžete nahrát vašich prostředků knihovny DLL (například TXT, .png a CSV).</span><span class="sxs-lookup"><span data-stu-id="b8970-267">Resources: You can upload your DLL resources (for example, .txt, .png, and .csv) as part of hello assembly registration.</span></span> 

<span data-ttu-id="b8970-268">Jiný způsob tootrigger hello **ADL: registrace sestavení prostřednictvím konfigurace** příkaz je soubor .dll hello tooright klikněte v Průzkumníku souborů.</span><span class="sxs-lookup"><span data-stu-id="b8970-268">Another way tootrigger hello **ADL: Register Assembly through Configuration** command is tooright-click hello .dll file in File Explorer.</span></span> 

<span data-ttu-id="b8970-269">Hello následující U-SQL kód ukazuje, jak toocall sestavení.</span><span class="sxs-lookup"><span data-stu-id="b8970-269">hello following U-SQL code demonstrates how toocall an assembly.</span></span> <span data-ttu-id="b8970-270">V ukázce hello hello název sestavení je *testování*.</span><span class="sxs-lookup"><span data-stu-id="b8970-270">In hello sample, hello assembly name is *test*.</span></span>

```
REFERENCE ASSEMBLY [test];

@a = 
    EXTRACT 
        Iid int,
    Starts DateTime,
    Region string,
    Query string,
    DwellTime int,
    Results string,
    ClickedUrls string 
    FROM @"Sample/SearchLog.txt" 
    USING Extractors.Tsv();

@d =
    SELECT DISTINCT Region 
    FROM @a;

@d1 = 
    PROCESS @d
    PRODUCE 
        Region string,
    Mkt string
    USING new USQLApplication_codebehind.MyProcessor();

OUTPUT @d1 
    too@"Sample/SearchLogtest.txt" 
    USING Outputters.Tsv();
```


## <a name="access-hello-data-lake-analytics-catalog"></a><span data-ttu-id="b8970-271">Katalog Data Lake Analytics hello přístup</span><span class="sxs-lookup"><span data-stu-id="b8970-271">Access hello Data Lake Analytics catalog</span></span>

<span data-ttu-id="b8970-272">Po připojení tooAzure, můžete použít následující kroky tooaccess hello U-SQL katalogu hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-272">After you have connected tooAzure, you can use hello following steps tooaccess hello U-SQL catalog.</span></span>

<span data-ttu-id="b8970-273">**tooaccess hello metadata Azure Data Lake Analytics**</span><span class="sxs-lookup"><span data-stu-id="b8970-273">**tooaccess hello Azure Data Lake Analytics metadata**</span></span>

1.  <span data-ttu-id="b8970-274">Vyberte Ctrl + Shift + P a potom zadejte **ADL: seznamu tabulek**.</span><span class="sxs-lookup"><span data-stu-id="b8970-274">Select Ctrl+Shift+P, and then enter **ADL: List Tables**.</span></span>
2.  <span data-ttu-id="b8970-275">Vyberte jeden z účtů Data Lake Analytics hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-275">Select one of hello Data Lake Analytics accounts.</span></span>
3.  <span data-ttu-id="b8970-276">Vyberte jednu z databáze hello Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="b8970-276">Select one of hello Data Lake Analytics databases.</span></span>
4.  <span data-ttu-id="b8970-277">Vyberte jednu z hello schémat.</span><span class="sxs-lookup"><span data-stu-id="b8970-277">Select one of hello schemas.</span></span> <span data-ttu-id="b8970-278">Zobrazí seznam hello tabulek.</span><span class="sxs-lookup"><span data-stu-id="b8970-278">You can see hello list of tables.</span></span>

## <a name="view-data-lake-analytics-jobs"></a><span data-ttu-id="b8970-279">Úlohy zobrazení Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="b8970-279">View Data Lake Analytics jobs</span></span>

<span data-ttu-id="b8970-280">**tooview úloh Data Lake Analytics**</span><span class="sxs-lookup"><span data-stu-id="b8970-280">**tooview Data Lake Analytics jobs**</span></span>
1.  <span data-ttu-id="b8970-281">Otevřete hello příkaz palety (Ctrl + Shift + P) a vyberte **ADL: Zobrazit úlohy**.</span><span class="sxs-lookup"><span data-stu-id="b8970-281">Open hello command palette (Ctrl+Shift+P) and select **ADL: Show Job**.</span></span> 
2.  <span data-ttu-id="b8970-282">Vyberte Data Lake Analytics nebo místní účet.</span><span class="sxs-lookup"><span data-stu-id="b8970-282">Select a Data Lake Analytics or local account.</span></span> 
3.  <span data-ttu-id="b8970-283">Počkejte hello seznamu úloh pro účet tooappear hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-283">Wait for hello jobs list for hello account tooappear.</span></span>
4.  <span data-ttu-id="b8970-284">Vyberte ze seznamu úloh úlohu, nástrojů Data Lake podrobnosti úlohy hello se otevře v hello portál Azure a zobrazí soubor informací o úloze hello v produktu VS Code.</span><span class="sxs-lookup"><span data-stu-id="b8970-284">Select a job from job list, Data Lake Tools opens hello job details in hello Azure portal and displays hello JobInfo file in VS Code.</span></span>

![Typy objektů nástroje data Lake pro Visual Studio kódu IntelliSense](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-show-job.png)

## <a name="azure-data-lake-storage-integration"></a><span data-ttu-id="b8970-286">Integrace úložiště Azure Data Lake</span><span class="sxs-lookup"><span data-stu-id="b8970-286">Azure Data Lake Storage integration</span></span>

<span data-ttu-id="b8970-287">Můžete použít příkazy související s Azure Data Lake Storage na:</span><span class="sxs-lookup"><span data-stu-id="b8970-287">You can use Azure Data Lake Storage-related commands to:</span></span>
 - <span data-ttu-id="b8970-288">Procházejte prostředky Azure Data Lake Storage hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-288">Browse through hello Azure Data Lake Storage resources.</span></span> 
 - <span data-ttu-id="b8970-289">Náhled souboru Azure Data Lake Storage hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-289">Preview hello Azure Data Lake Storage file.</span></span>  
 - <span data-ttu-id="b8970-290">Nahrát hello tooAzure Data Lake Storage souboru přímo v kódu VS.</span><span class="sxs-lookup"><span data-stu-id="b8970-290">Upload hello file directly tooAzure Data Lake Storage in VS Code.</span></span> 

### <a name="list-hello-storage-path"></a><span data-ttu-id="b8970-291">Cestu k úložišti hello seznamu</span><span class="sxs-lookup"><span data-stu-id="b8970-291">List hello storage path</span></span> 
<span data-ttu-id="b8970-292">Můžete vytvořit seznam cestu k úložišti hello prostřednictvím hello příkaz palety nebo klikněte pravým tlačítkem.</span><span class="sxs-lookup"><span data-stu-id="b8970-292">You can list hello storage path through hello command palette or through right-click.</span></span>

<span data-ttu-id="b8970-293">**cestu k úložišti hello toolist prostřednictvím hello příkaz palety**</span><span class="sxs-lookup"><span data-stu-id="b8970-293">**toolist hello storage path through hello command palette**</span></span>

1.  <span data-ttu-id="b8970-294">Spusťte příkaz palety hello (Ctrl + Shift + P) a zadejte **ADL: cestu k úložišti seznamu**.</span><span class="sxs-lookup"><span data-stu-id="b8970-294">Open hello command palette (Ctrl+Shift+P) and enter **ADL: List Storage Path**.</span></span>

    ![Nástroje data Lake pro Visual Studio Code seznamu cestu k úložišti](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-storage.png)

2.  <span data-ttu-id="b8970-296">Vyberte vaše upřednostňovaný způsob pro výpis cestu k úložišti hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-296">Select your preferred way for listing hello storage path.</span></span> <span data-ttu-id="b8970-297">Používá tento průchod **zadejte cestu** jako příklad.</span><span class="sxs-lookup"><span data-stu-id="b8970-297">This passage uses **Enter a path** as an example.</span></span>

    ![Nástroje data Lake pro Visual Studio Code cestu k úložišti jedním ze způsobů toolist hello](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account-selectoneway.png)

    > [!NOTE]
    >- <span data-ttu-id="b8970-299">VS Code zajišťuje hello poslední navštívené cesta každý účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="b8970-299">VS Code keeps hello last-visited path in every Data Lake Analytics account.</span></span> <span data-ttu-id="b8970-300">Například: / tt/ss.</span><span class="sxs-lookup"><span data-stu-id="b8970-300">For example: /tt/ss.</span></span>
    >- <span data-ttu-id="b8970-301">Prohlížeči kořenovou cestu: hello seznamu kořenovou cestu z účtu Data Lake Analytics vybrané nebo místní cestu.</span><span class="sxs-lookup"><span data-stu-id="b8970-301">Browser from root path: hello list root path from your selected Data Lake Analytics account or a local path.</span></span>
    >- <span data-ttu-id="b8970-302">Zadejte cestu: seznam zadanou cestu z účtu Data Lake Analytics vybrané nebo místní cestu.</span><span class="sxs-lookup"><span data-stu-id="b8970-302">Enter a path: List a specified path from your selected Data Lake Analytics account or a local path.</span></span>
    
3. <span data-ttu-id="b8970-303">Vyberte účet ze hello místní cestu nebo účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="b8970-303">Select an account from hello local path or a Data Lake Analytics account.</span></span>

    ![Nástroje data Lake pro Visual Studio Code vyberte více](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

4. <span data-ttu-id="b8970-305">Vyberte **Další** toolist další účty Data Lake Analytics a potom vyberte účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="b8970-305">Select **more** toolist more Data Lake Analytics accounts, and then select a Data Lake Analytics account.</span></span>

    ![Nástroje data Lake pro Visual Studio Code vyberte účet](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-select-adla-account.png)

5.  <span data-ttu-id="b8970-307">Zadejte cestu k úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="b8970-307">Enter an Azure storage path.</span></span> <span data-ttu-id="b8970-308">Například/výstup.</span><span class="sxs-lookup"><span data-stu-id="b8970-308">For example, /output.</span></span>

    ![Nástroje data Lake pro Visual Studio Code zadejte cestu k úložišti](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-input-path.png)

6.  <span data-ttu-id="b8970-310">Výsledky: hello příkaz palety uvádí informace o cestě hello hodnot.</span><span class="sxs-lookup"><span data-stu-id="b8970-310">Results: hello command palette lists hello path information based on your entries.</span></span>

    ![Nástroje data Lake pro Visual Studio Code seznam výsledků cesta úložiště](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-path.png)

<span data-ttu-id="b8970-312">Pohodlnější způsob toolist hello relativní cesta je prostřednictvím hello klikněte pravým tlačítkem na místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="b8970-312">A more convenient way toolist hello relative path is through hello right-click context menu.</span></span>

<span data-ttu-id="b8970-313">**Klikněte pravým tlačítkem na cestu k úložišti hello toolist prostřednictvím**</span><span class="sxs-lookup"><span data-stu-id="b8970-313">**toolist hello storage path through right-click**</span></span>

1.  <span data-ttu-id="b8970-314">Klikněte pravým tlačítkem na hello cesta řetězec tooselect **cestu k úložišti seznamu**.</span><span class="sxs-lookup"><span data-stu-id="b8970-314">Right-click hello path string tooselect **List Storage Path**.</span></span>

       ![Nástroje data Lake pro Visual Studio Code, klikněte pravým tlačítkem na kontextové nabídky](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-path.png)

2. <span data-ttu-id="b8970-316">Vybraná cesta relativní Hello se zobrazí v palety příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-316">hello selected relative path appears in hello command palette.</span></span>

   ![Nástroje data Lake pro Visual Studio Code vybraná relativní cesta.](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-relative-path.png)

3.  <span data-ttu-id="b8970-318">Vyberte účet ze hello místní cestu nebo účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="b8970-318">Select an account from hello local path or a Data Lake Analytics account.</span></span>

       ![Nástroje data Lake pro Visual Studio Code vyberte účet](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

4.  <span data-ttu-id="b8970-320">Výsledky: hello příkaz palety uvádí hello složek a souborů pro aktuální cestu hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-320">Results: hello command palette lists hello folders and files for hello current path.</span></span>

       ![Nástroje data Lake pro Visual Studio Code seznam z aktuální cestě hello](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-current.png)

### <a name="preview-hello-storage-file"></a><span data-ttu-id="b8970-322">Soubor úložiště hello Preview</span><span class="sxs-lookup"><span data-stu-id="b8970-322">Preview hello storage file</span></span>
<span data-ttu-id="b8970-323">Můžete zobrazit náhled souboru hello úložiště prostřednictvím hello příkaz palety nebo klikněte pravým tlačítkem.</span><span class="sxs-lookup"><span data-stu-id="b8970-323">You can preview hello storage file through hello command palette or through right-click.</span></span>

<span data-ttu-id="b8970-324">**soubor úložiště hello toopreview prostřednictvím hello příkaz palety**</span><span class="sxs-lookup"><span data-stu-id="b8970-324">**toopreview hello storage file through hello command palette**</span></span>

1.  <span data-ttu-id="b8970-325">Spusťte příkaz palety hello (Ctrl + Shift + P) a zadejte **ADL: soubor úložiště Preview**.</span><span class="sxs-lookup"><span data-stu-id="b8970-325">Open hello command palette (Ctrl+Shift+P) and enter **ADL: Preview Storage File**.</span></span>

       ![Nástroje data Lake pro Visual Studio Code preview úložiště souborů](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-preview.png)

2.  <span data-ttu-id="b8970-327">Vyberte účet ze hello místní cestu nebo účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="b8970-327">Select an account from hello local path or a Data Lake Analytics account.</span></span>

       ![Nástroje data Lake pro Visual Studio Code seznamu účtu](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

3.  <span data-ttu-id="b8970-329">Vyberte **Další** toolist další účty Data Lake Analytics a potom vyberte účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="b8970-329">Select **more** toolist more Data Lake Analytics accounts, and then select a Data Lake Analytics account.</span></span>

       ![Nástroje data Lake pro Visual Studio Code vyberte účet](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-select-adla-account.png)

4.  <span data-ttu-id="b8970-331">Zadejte cestu k úložišti Azure nebo souboru.</span><span class="sxs-lookup"><span data-stu-id="b8970-331">Enter an Azure storage path or file.</span></span> <span data-ttu-id="b8970-332">Například /output/SearchLog.txt.</span><span class="sxs-lookup"><span data-stu-id="b8970-332">For example, /output/SearchLog.txt.</span></span>

       ![Nástroje data Lake pro Visual Studio Code zadejte cestu k úložišti a souborů](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-input-preview-file.png)

5.  <span data-ttu-id="b8970-334">Výsledky: hello příkaz palety uvádí informace o cestě hello hodnot.</span><span class="sxs-lookup"><span data-stu-id="b8970-334">Results: hello command palette lists hello path information based on your entries.</span></span>

       ![Nástroje data Lake pro Visual Studio Code náhled souboru výsledků](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-preview-results.png)

<span data-ttu-id="b8970-336">**Klikněte pravým tlačítkem na cestu k úložišti hello toolist prostřednictvím**</span><span class="sxs-lookup"><span data-stu-id="b8970-336">**toolist hello storage path through right-click**</span></span>

1.  <span data-ttu-id="b8970-337">Klikněte pravým tlačítkem toopreview souboru, cesta k souboru hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-337">toopreview a file, right-click hello file path.</span></span>

   ![Nástroje data Lake pro Visual Studio Code, klikněte pravým tlačítkem na kontextové nabídky](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-preview.png) 

2.  <span data-ttu-id="b8970-339">Vyberte účet ze hello místní cestu nebo účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="b8970-339">Select an account from hello local path or a Data Lake Analytics account.</span></span>

       ![Nástroje data Lake pro Visual Studio Code vyberte účet](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

3.  <span data-ttu-id="b8970-341">Výsledky: VS Code zobrazí výsledky náhledu hello hello souboru.</span><span class="sxs-lookup"><span data-stu-id="b8970-341">Results: VS Code displays hello preview results of hello file.</span></span>

       ![Nástroje data Lake pro Visual Studio Code náhled souboru výsledků](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-preview-results.png)

### <a name="upload-a-file"></a><span data-ttu-id="b8970-343">Nahrání souboru</span><span class="sxs-lookup"><span data-stu-id="b8970-343">Upload a file</span></span> 

<span data-ttu-id="b8970-344">Můžete nahrát soubory tak, že zadáte příkazy hello **ADL: nahrát soubor** nebo **ADL: nahrát soubor prostřednictvím konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="b8970-344">You can upload files by entering hello commands **ADL: Upload File** or **ADL: Upload File through Configuration**.</span></span>

<span data-ttu-id="b8970-345">**soubory tooupload ale hello ADL: nahrát soubor – příkaz**</span><span class="sxs-lookup"><span data-stu-id="b8970-345">**tooupload files though hello ADL: Upload File command**</span></span>
1. <span data-ttu-id="b8970-346">Vyberte Ctrl + Shift + P tooopen hello příkaz palety nebo klikněte pravým tlačítkem na editor skriptů hello a potom zadejte **nahrát soubor**.</span><span class="sxs-lookup"><span data-stu-id="b8970-346">Select Ctrl+Shift+P tooopen hello command palette or right-click hello script editor, and then enter **Upload File**.</span></span>
2.  <span data-ttu-id="b8970-347">tooupload hello souboru, zadejte místní cestu.</span><span class="sxs-lookup"><span data-stu-id="b8970-347">tooupload hello file, enter a local path.</span></span>

    ![Nástroje data Lake pro Visual Studio Code zadejte místní cestu.](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-input-local-path.png)

3. <span data-ttu-id="b8970-349">Vyberte některým z těchto způsobů hello výpis hello úložiště cesty.</span><span class="sxs-lookup"><span data-stu-id="b8970-349">Select one of hello ways of listing hello storage path.</span></span> <span data-ttu-id="b8970-350">Používá tento průchod **zadejte cestu** jako příklad.</span><span class="sxs-lookup"><span data-stu-id="b8970-350">This passage uses **Enter a path** as an example.</span></span>

    ![Nástroje data Lake pro Visual Studio Code seznamu cestu k úložišti](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account-selectoneway.png)
    >[!NOTE]
    >- <span data-ttu-id="b8970-352">VS Code zajišťuje hello poslední navštívené cesta každý účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="b8970-352">VS Code keeps hello last-visited path in every Data Lake Analytics account.</span></span> <span data-ttu-id="b8970-353">Například: / tt/ss.</span><span class="sxs-lookup"><span data-stu-id="b8970-353">For example: /tt/ss.</span></span>
    >- <span data-ttu-id="b8970-354">Prohlížeči kořenovou cestu: hello seznamu kořenovou cestu z účtu Data Lake Analytics vybrané nebo místní cestu.</span><span class="sxs-lookup"><span data-stu-id="b8970-354">Browser from root path: hello list root path from your selected Data Lake Analytics account or a local path.</span></span>
    >- <span data-ttu-id="b8970-355">Zadejte cestu: seznam zadanou cestu z účtu Data Lake Analytics vybrané nebo místní cestu.</span><span class="sxs-lookup"><span data-stu-id="b8970-355">Enter a path: List a specified path from your selected Data Lake Analytics account or a local path.</span></span>

4. <span data-ttu-id="b8970-356">Vyberte účet ze hello místní cestu nebo účtu Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="b8970-356">Select an account from hello local path or a Data Lake Analytics account.</span></span>

    ![Nástroje data Lake pro Visual Studio Code, klikněte pravým tlačítkem na úložiště](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

5. <span data-ttu-id="b8970-358">Zadejte cestu k úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="b8970-358">Enter an Azure storage path.</span></span> <span data-ttu-id="b8970-359">Například: / výstup.</span><span class="sxs-lookup"><span data-stu-id="b8970-359">For example: /output.</span></span>

       ![Data Lake Tools for Visual Studio Code enter storage path](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-input-preview-file.png)

6. <span data-ttu-id="b8970-360">Najít cestu k úložišti Azure.</span><span class="sxs-lookup"><span data-stu-id="b8970-360">Find your Azure storage path.</span></span> <span data-ttu-id="b8970-361">Vyberte **aktuální složce zvolte**.</span><span class="sxs-lookup"><span data-stu-id="b8970-361">Select **Choose current folder**.</span></span>

    ![Vyberte složku nástroje data Lake pro Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-choose-current-folder.png)

7.  <span data-ttu-id="b8970-363">Výsledky: hello **výstup** okně se zobrazí stav nahrávání souboru hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-363">Results: hello **Output** window displays hello file upload status.</span></span>

       ![Nástroje data Lake pro Visual Studio Code nahrát stav](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-status.png)    

<span data-ttu-id="b8970-365">**soubory tooupload ale hello ADL: nahrát soubor pomocí příkazu konfigurace**</span><span class="sxs-lookup"><span data-stu-id="b8970-365">**tooupload files though hello ADL: Upload File through Configuration command**</span></span>
1.  <span data-ttu-id="b8970-366">Vyberte Ctrl + Shift + P tooopen hello příkaz palety nebo klikněte pravým tlačítkem na editor skriptů hello a potom zadejte **nahrát soubor prostřednictvím konfigurace**.</span><span class="sxs-lookup"><span data-stu-id="b8970-366">Select Ctrl+Shift+P tooopen hello command palette or right-click hello script editor, and then enter **Upload File through Configuration**.</span></span>
2.  <span data-ttu-id="b8970-367">VS Code zobrazí soubor JSON.</span><span class="sxs-lookup"><span data-stu-id="b8970-367">VS Code displays a JSON file.</span></span> <span data-ttu-id="b8970-368">Můžete zadat cesty k souborům a uložení více souborů v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="b8970-368">You can enter file paths and upload multiple files at hello same time.</span></span> <span data-ttu-id="b8970-369">Pokyny, které se zobrazují v hello **výstup** okno.</span><span class="sxs-lookup"><span data-stu-id="b8970-369">Instructions are displayed in hello **Output** window.</span></span> <span data-ttu-id="b8970-370">tooproceed tooupload hello soubor, uložte soubor JSON hello (Ctrl + S).</span><span class="sxs-lookup"><span data-stu-id="b8970-370">tooproceed tooupload hello file, save (Ctrl+S) hello JSON file.</span></span>

       ![Cesta k souboru nástroje data Lake pro Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-file.png)

3.  <span data-ttu-id="b8970-372">Výsledky: hello **výstup** okně se zobrazí stav nahrávání souboru hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-372">Results: hello **Output** window displays hello file upload status.</span></span>

       ![Nástroje data Lake pro Visual Studio Code nahrát stav](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-status.png)     

<span data-ttu-id="b8970-374">Jiný způsob tooupload toostorage souboru je prostřednictvím hello klikněte pravým tlačítkem na nabídky na úplná cesta hello souboru nebo relativní cestu hello souboru v editoru skriptů hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-374">Another way tooupload a file toostorage is through hello right-click menu on hello file's full path or hello file's relative path in hello script editor.</span></span> <span data-ttu-id="b8970-375">Zadejte cestu k souboru místní hello a potom vyberte účet hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-375">Enter hello local file path, and then select hello account.</span></span> <span data-ttu-id="b8970-376">Hello **výstup** okně se zobrazí stav nahrávání hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-376">hello **Output** window displays hello upload status.</span></span> 

### <a name="open-azure-storage-explorer"></a><span data-ttu-id="b8970-377">Otevřete Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="b8970-377">Open Azure Storage Explorer</span></span>
<span data-ttu-id="b8970-378">Můžete otevřít **Azure Storage Explorer** zadáním příkazu hello **ADL: Otevřete Web Azure Storage Explorer** nebo výběrem hello klikněte pravým tlačítkem na místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="b8970-378">You can open **Azure Storage Explorer** by entering hello command **ADL: Open Web Azure Storage Explorer** or by selecting it from hello right-click context menu.</span></span>

<span data-ttu-id="b8970-379">**tooopen Azure Storage Explorer**</span><span class="sxs-lookup"><span data-stu-id="b8970-379">**tooopen Azure Storage Explorer**</span></span>

1. <span data-ttu-id="b8970-380">Vyberte Ctrl + Shift + P tooopen hello příkaz palety.</span><span class="sxs-lookup"><span data-stu-id="b8970-380">Select Ctrl+Shift+P tooopen hello command palette.</span></span>
2. <span data-ttu-id="b8970-381">Zadejte **otevřete Web Azure Storage Explorer** nebo klikněte pravým tlačítkem na relativní cestu nebo hello úplnou cestu v editoru hello skript a potom vyberte **otevřete Web Azure Storage Explorer**.</span><span class="sxs-lookup"><span data-stu-id="b8970-381">Enter **Open Web Azure Storage Explorer** or right-click on a relative path or hello full path in hello script editor, and then select **Open Web Azure Storage Explorer**.</span></span>
3. <span data-ttu-id="b8970-382">Vyberte účet Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="b8970-382">Select a Data Lake Analytics account.</span></span>

<span data-ttu-id="b8970-383">Nástroje data Lake otevře cestu k úložišti Azure hello v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b8970-383">Data Lake Tools opens hello Azure storage path in hello Azure portal.</span></span> <span data-ttu-id="b8970-384">Můžete najít hello cestu a náhled souboru hello z webové hello.</span><span class="sxs-lookup"><span data-stu-id="b8970-384">You can find hello path and preview hello file from hello web.</span></span>

### <a name="local-run-and-local-debug-for-windows-users"></a><span data-ttu-id="b8970-385">Místní spuštění a místní ladění pro systém Windows uživatele</span><span class="sxs-lookup"><span data-stu-id="b8970-385">Local run and local debug for Windows users</span></span>
<span data-ttu-id="b8970-386">U-SQL místní spuštění testů místní data a ověří váš skript místně, než váš kód je publikovaná tooData Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="b8970-386">U-SQL local run tests your local data and validates your script locally, before your code is published tooData Lake Analytics.</span></span> <span data-ttu-id="b8970-387">funkce umožňuje místní ladění Hello je hello toocomplete následující úkoly před váš kód je odeslána tooData Lake Analytics:</span><span class="sxs-lookup"><span data-stu-id="b8970-387">hello local debug feature enables you toocomplete hello following tasks before your code is submitted tooData Lake Analytics:</span></span> 
- <span data-ttu-id="b8970-388">Ladění vaší C# kódu.</span><span class="sxs-lookup"><span data-stu-id="b8970-388">Debug your C# code-behind.</span></span> 
- <span data-ttu-id="b8970-389">Projděte hello kódu.</span><span class="sxs-lookup"><span data-stu-id="b8970-389">Step through hello code.</span></span> 
- <span data-ttu-id="b8970-390">Ověřte váš skript místně.</span><span class="sxs-lookup"><span data-stu-id="b8970-390">Validate your script locally.</span></span>

<span data-ttu-id="b8970-391">Pokyny v místním spuštění a místní ladění najdete v tématu [U-SQL místní spuštění a místní ladění s Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).</span><span class="sxs-lookup"><span data-stu-id="b8970-391">For instructions on local run and local debug, see [U-SQL local run and local debug with Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).</span></span>

## <a name="additional-features"></a><span data-ttu-id="b8970-392">Další funkce</span><span class="sxs-lookup"><span data-stu-id="b8970-392">Additional features</span></span>

<span data-ttu-id="b8970-393">Nástroje data Lake pro VS Code podporuje hello následující funkce:</span><span class="sxs-lookup"><span data-stu-id="b8970-393">Data Lake Tools for VS Code supports hello following features:</span></span>

-   <span data-ttu-id="b8970-394">Automatické dokončování IntelliSense: návrhy zobrazí v automaticky otevíraná okna kolem položky, jako jsou klíčová slova, metod a proměnné.</span><span class="sxs-lookup"><span data-stu-id="b8970-394">IntelliSense auto-complete: Suggestions appear in pop-up windows around items, such as keywords, methods, and variables.</span></span> <span data-ttu-id="b8970-395">Různé ikony představují různé typy objektů hello:</span><span class="sxs-lookup"><span data-stu-id="b8970-395">Different icons represent different types of hello objects:</span></span>

    - <span data-ttu-id="b8970-396">Datový typ Scala</span><span class="sxs-lookup"><span data-stu-id="b8970-396">Scala data type</span></span>
    - <span data-ttu-id="b8970-397">Komplexní datový typ</span><span class="sxs-lookup"><span data-stu-id="b8970-397">Complex data type</span></span>
    - <span data-ttu-id="b8970-398">Předdefinované UDT</span><span class="sxs-lookup"><span data-stu-id="b8970-398">Built-in UDTs</span></span>
    - <span data-ttu-id="b8970-399">Třídy a .NET collection</span><span class="sxs-lookup"><span data-stu-id="b8970-399">.NET collection and classes</span></span>
    - <span data-ttu-id="b8970-400">Výrazy jazyka C#</span><span class="sxs-lookup"><span data-stu-id="b8970-400">C# expressions</span></span>
    - <span data-ttu-id="b8970-401">Předdefinované C# UDF, UDO a UDAAGs</span><span class="sxs-lookup"><span data-stu-id="b8970-401">Built-in C# UDFs, UDOs, and UDAAGs</span></span> 
    - <span data-ttu-id="b8970-402">Funkce U-SQL</span><span class="sxs-lookup"><span data-stu-id="b8970-402">U-SQL functions</span></span>
    - <span data-ttu-id="b8970-403">U-SQL oddílová funkce</span><span class="sxs-lookup"><span data-stu-id="b8970-403">U-SQL windowing function</span></span>
 
    ![Typy objektů nástroje data Lake pro Visual Studio kódu IntelliSense](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-objects.png)
 
-   <span data-ttu-id="b8970-405">IntelliSense automatického dokončování v Data Lake Analytics metadata: nástrojů Data Lake stáhne hello Data Lake Analytics informace metadat místně.</span><span class="sxs-lookup"><span data-stu-id="b8970-405">IntelliSense auto-complete on Data Lake Analytics metadata: Data Lake Tools downloads hello Data Lake Analytics metadata information locally.</span></span> <span data-ttu-id="b8970-406">Funkce IntelliSense Hello automaticky naplní objektů, včetně hello databáze, schéma, tabulka, zobrazení, funkce vracející tabulku, postupy a sestavení C#, z metadat hello Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="b8970-406">hello IntelliSense feature automatically populates objects, including hello database, schema, table, view, table-valued function, procedures, and C# assemblies, from hello Data Lake Analytics metadata.</span></span>
 
    ![Nástroje data Lake pro Visual Studio kódu IntelliSense metadat](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-metastore.png)

-   <span data-ttu-id="b8970-408">Chyba značky IntelliSense: nástrojů Data Lake podtrhne hello úpravy chyby U-SQL a C#.</span><span class="sxs-lookup"><span data-stu-id="b8970-408">IntelliSense error marker: Data Lake Tools underlines hello editing errors for U-SQL and C#.</span></span> 
-   <span data-ttu-id="b8970-409">Označuje syntaxe: nástrojů Data Lake pomocí různých barev toodifferentiate položky, jako jsou proměnné, klíčová slova, datový typ a funkce.</span><span class="sxs-lookup"><span data-stu-id="b8970-409">Syntax highlights: Data Lake Tools uses different colors toodifferentiate items, such as variables, keywords, data type, and functions.</span></span> 

    ![Nástroje data Lake pro Visual Studio Code syntaxe označuje](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a><span data-ttu-id="b8970-411">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b8970-411">Next steps</span></span>

- <span data-ttu-id="b8970-412">Místní spuštění U-SQL a místní ladění s kódem jazyka Visual Studio najdete v tématu [U-SQL místní spuštění a místní ladění s Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).</span><span class="sxs-lookup"><span data-stu-id="b8970-412">For U-SQL local run and local debug with Visual Studio Code, see [U-SQL local run and local debug with Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).</span></span>
- <span data-ttu-id="b8970-413">Začínáme informace o Data Lake Analytics najdete v tématu [kurz: Začínáme s Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b8970-413">For getting-started information on Data Lake Analytics, see [Tutorial: Get started with Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).</span></span>
- <span data-ttu-id="b8970-414">Informace o nástrojů Data Lake pro Visual Studio najdete v tématu [kurz: vývoj U-SQL skriptů pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="b8970-414">For information about Data Lake Tools for Visual Studio, see [Tutorial: Develop U-SQL scripts by using Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
- <span data-ttu-id="b8970-415">Informace týkající se vývoje sestavení najdete v tématu [sestavení vyvíjet U-SQL pro úlohy Azure Data Lake Analytics](data-lake-analytics-u-sql-develop-assemblies.md).</span><span class="sxs-lookup"><span data-stu-id="b8970-415">For information on developing assemblies, see [Develop U-SQL assemblies for Azure Data Lake Analytics jobs](data-lake-analytics-u-sql-develop-assemblies.md).</span></span>



