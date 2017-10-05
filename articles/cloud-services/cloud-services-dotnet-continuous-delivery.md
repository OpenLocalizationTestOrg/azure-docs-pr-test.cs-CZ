---
title: "Nastavené průběžné doručování pro cloudové služby se sadou TFS v Azure | Microsoft Docs"
description: "Zjistěte, jak nastavit průběžné doručování Azure cloudových aplikací. Ukázky kódu pro MSBuild příkazy příkazového řádku a skripty prostředí PowerShell."
services: cloud-services
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 4f3c93c6-5c82-4779-9d19-7404a01e82ca
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/12/2017
ms.author: kraigb
ms.openlocfilehash: 0979722b9ec715e91825c7aba74657451df6e83f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="continuous-delivery-for-cloud-services-in-azure"></a><span data-ttu-id="06956-104">Nastavené průběžné doručování pro cloudové služby v Azure</span><span class="sxs-lookup"><span data-stu-id="06956-104">Continuous Delivery for Cloud Services in Azure</span></span>
<span data-ttu-id="06956-105">Proces popsaný v tomto článku se dozvíte, jak nastavit průběžné doručování Azure cloudových aplikací.</span><span class="sxs-lookup"><span data-stu-id="06956-105">The process described in this article shows you how to set up continuous delivery for Azure cloud apps.</span></span> <span data-ttu-id="06956-106">Po každém vrácení kódu se změnami vám tento proces umožní automaticky vytvořit balíčky a nasadit balíček do Azure.</span><span class="sxs-lookup"><span data-stu-id="06956-106">This process enables you to automatically create packages and deploy the package to Azure after every code check-in.</span></span> <span data-ttu-id="06956-107">Proces sestavení balíčku, který je popsaný v tomto článku je ekvivalentní **balíček** příkaz v sadě Visual Studio a publikování kroky jsou rovnocenná **publikovat** příkazu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="06956-107">The package build process described in this article is equivalent to the **Package** command in Visual Studio, and the publishing steps are equivalent to the **Publish** command in Visual Studio.</span></span>
<span data-ttu-id="06956-108">Článek vysvětluje metod, které byste použili k vytvoření serveru sestavení s příkazy příkazového řádku nástroje MSBuild a skriptů prostředí Windows PowerShell a také ukazuje, jak Volitelně lze konfigurovat Visual Studio Team Foundation Server – definice sestavení Team použití nástroje MSBuild příkazy a skripty prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="06956-108">The article covers the methods you would use to create a build server with MSBuild command-line statements and Windows PowerShell scripts, and it also demonstrates how to optionally configure Visual Studio Team Foundation Server - Team Build definitions to use the MSBuild commands and PowerShell scripts.</span></span> <span data-ttu-id="06956-109">Proces je přizpůsobitelné prostředí pro sestavení a prostředí Azure cíl.</span><span class="sxs-lookup"><span data-stu-id="06956-109">The process is customizable for your build environment and Azure target environments.</span></span>

<span data-ttu-id="06956-110">Můžete také použít Visual Studio Team Services, verzi sady TFS, která je hostovaná v Azure k tomu snadněji.</span><span class="sxs-lookup"><span data-stu-id="06956-110">You can also use Visual Studio Team Services, a version of TFS that is hosted in Azure, to do this more easily.</span></span> 

<span data-ttu-id="06956-111">Než začnete, byste měli publikovat aplikace ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="06956-111">Before you start, you should publish your application from Visual Studio.</span></span>
<span data-ttu-id="06956-112">To zajistí, že všechny prostředky, které jsou dostupné a inicializovaného při pokusu automatizovat proces publikace.</span><span class="sxs-lookup"><span data-stu-id="06956-112">This will ensure that all the resources are available and initialized when you attempt to automate the publication process.</span></span>

## <a name="1-configure-the-build-server"></a><span data-ttu-id="06956-113">1: Konfigurace serveru sestavení</span><span class="sxs-lookup"><span data-stu-id="06956-113">1: Configure the Build Server</span></span>
<span data-ttu-id="06956-114">Než vytvoříte balíček Azure pomocí nástroje MSBuild je na serveru sestavení nainstalovat požadovaný software a nástroje.</span><span class="sxs-lookup"><span data-stu-id="06956-114">Before you can create an Azure package by using MSBuild, you must install the required software and tools on the build server.</span></span>

<span data-ttu-id="06956-115">Visual Studio není musí být nainstalovaný na serveru sestavení.</span><span class="sxs-lookup"><span data-stu-id="06956-115">Visual Studio is not required to be installed on the build server.</span></span> <span data-ttu-id="06956-116">Pokud chcete použít ke správě serveru sestavení služby sestavení Team Foundation, postupujte podle pokynů [služby sestavení Team Foundation] [ Team Foundation Build Service] dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="06956-116">If you want to use Team Foundation Build Service to manage your build server, follow the instructions in the [Team Foundation Build Service][Team Foundation Build Service] documentation.</span></span>

1. <span data-ttu-id="06956-117">Na serveru, sestavení, nainstalujte [rozhraní .NET Framework 4.5.2][.NET Framework 4.5.2], což zahrnuje MSBuild.</span><span class="sxs-lookup"><span data-stu-id="06956-117">On the build server, install the [.NET Framework 4.5.2][.NET Framework 4.5.2], which includes MSBuild.</span></span>
2. <span data-ttu-id="06956-118">Nainstalujte si nejnovější verzi [nástroje pro tvorbu Azure pro .NET](https://azure.microsoft.com/develop/net/).</span><span class="sxs-lookup"><span data-stu-id="06956-118">Install the latest [Azure Authoring Tools for .NET](https://azure.microsoft.com/develop/net/).</span></span>
3. <span data-ttu-id="06956-119">Nainstalujte [knihovny Azure pro .NET](http://go.microsoft.com/fwlink/?LinkId=623519).</span><span class="sxs-lookup"><span data-stu-id="06956-119">Install the [Azure Libraries for .NET](http://go.microsoft.com/fwlink/?LinkId=623519).</span></span>
4. <span data-ttu-id="06956-120">Zkopírujte soubor Microsoft.WebApplication.targets z instalace sady Visual Studio k serveru sestavení.</span><span class="sxs-lookup"><span data-stu-id="06956-120">Copy the Microsoft.WebApplication.targets file from a Visual Studio installation to the build server.</span></span>

   <span data-ttu-id="06956-121">Na počítači s nainstalovanou sadu Visual Studio, tento soubor je umístěný v adresáři C:\\Program Files(x86)\\MSBuild\\Microsoft\\Visual Studio\\v14.0\\WebApplications.</span><span class="sxs-lookup"><span data-stu-id="06956-121">On a computer with Visual Studio installed, this file is located in the directory C:\\Program Files(x86)\\MSBuild\\Microsoft\\VisualStudio\\v14.0\\WebApplications.</span></span> <span data-ttu-id="06956-122">Měli byste ho zkopírovat do stejného adresáře na serveru sestavení.</span><span class="sxs-lookup"><span data-stu-id="06956-122">You should copy it to the same directory on the build server.</span></span>
5. <span data-ttu-id="06956-123">Nainstalujte [nástroje Azure pro sadu Visual Studio](https://www.visualstudio.com/features/azure-tools-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="06956-123">Install the [Azure Tools for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs.aspx).</span></span>

## <a name="2-build-a-package-using-msbuild-commands"></a><span data-ttu-id="06956-124">2: vytvoření balíčku pomocí příkazů nástroje MSBuild</span><span class="sxs-lookup"><span data-stu-id="06956-124">2: Build a Package using MSBuild Commands</span></span>
<span data-ttu-id="06956-125">Tato část popisuje, jak můžete vytvořit pomocí nástroje MSBuild příkazu, který vytvoří balíček Azure.</span><span class="sxs-lookup"><span data-stu-id="06956-125">This section describes how to construct an MSBuild command that builds an Azure package.</span></span> <span data-ttu-id="06956-126">Tento krok spusťte na serveru sestavení ověření všechno správně nakonfigurován a že příkaz MSBuild provést požadované na práci.</span><span class="sxs-lookup"><span data-stu-id="06956-126">Run this step on the build server to verify that everything is configured correctly and that the MSBuild command does what you want it to do.</span></span> <span data-ttu-id="06956-127">Tento příkaz můžete přidat buď existující sestavení skriptů na serveru sestavení nebo příkazového řádku v definici sestavení TFS můžete použít, jak je popsáno v následující části.</span><span class="sxs-lookup"><span data-stu-id="06956-127">You can either add this command line to existing build scripts on the build server, or you can use the command line in a TFS Build Definition, as described in the next section.</span></span> <span data-ttu-id="06956-128">Další informace o parametry příkazového řádku a nástroje MSBuild najdete v tématu [příkazového řádku MSBuild – Reference](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="06956-128">For more information about command-line parameters and MSBuild, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).</span></span>

1. <span data-ttu-id="06956-129">Pokud Visual Studio je nainstalovaná na serveru sestavení, vyhledejte a vyberte **Visual Studio – příkazový řádek** v **nástroje sady Visual Studio** složky v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="06956-129">If Visual Studio is installed on the build server, locate and choose **Visual Studio Command Prompt** in the **Visual Studio Tools** folder in Windows.</span></span>

   <span data-ttu-id="06956-130">Pokud Visual Studio není nainstalována na serveru sestavení, otevřete příkazový řádek a ujistěte se, že je dostupný na cestě MSBuild.exe.</span><span class="sxs-lookup"><span data-stu-id="06956-130">If Visual Studio is not installed on the build server, open a command prompt and make sure that MSBuild.exe is accessible on the path.</span></span> <span data-ttu-id="06956-131">MSBuild je nainstalován s rozhraním .NET Framework v cestě % WINDIR %\\Microsoft.NET\\Framework\\*verze*.</span><span class="sxs-lookup"><span data-stu-id="06956-131">MSBuild is installed with the .NET Framework in the path %WINDIR%\\Microsoft.NET\\Framework\\*Version*.</span></span> <span data-ttu-id="06956-132">Například MSBuild.exe přidat do proměnné prostředí PATH v případě, že máte nainstalované rozhraní .NET Framework 4, zadejte na příkazovém řádku následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="06956-132">For example, to add MSBuild.exe to the PATH environment variable when you have .NET Framework 4 installed, type the following command at the command prompt:</span></span>

       set PATH=%PATH%;"C:\Windows\Microsoft.NET\Framework\v4.0.30319"
2. <span data-ttu-id="06956-133">Na příkazovém řádku přejděte do složky obsahující soubor projektu Azure, který chcete vytvořit.</span><span class="sxs-lookup"><span data-stu-id="06956-133">At the command prompt, navigate to the folder containing the Azure project file that you want to build.</span></span>
3. <span data-ttu-id="06956-134">Spuštění nástroje MSBuild pomocí / target: publikování možnost jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="06956-134">Run MSBuild with the /target:Publish option as in the following example:</span></span>

       MSBuild /target:Publish

   <span data-ttu-id="06956-135">Tuto možnost lze zkrátit na /t: publikování.</span><span class="sxs-lookup"><span data-stu-id="06956-135">This option can be abbreviated as /t:Publish.</span></span> <span data-ttu-id="06956-136">Možnost /t:Publish v nástroji MSBuild Nezaměňovat s příkazy publikování, který je k dispozici v sadě Visual Studio Pokud máte sadu Azure SDK nainstalována.</span><span class="sxs-lookup"><span data-stu-id="06956-136">The /t:Publish option in MSBuild should not be confused with the Publish commands available in Visual Studio when you have the Azure SDK installed.</span></span> <span data-ttu-id="06956-137">/T: publikovat pouze sestavení možnost Azure balíčky.</span><span class="sxs-lookup"><span data-stu-id="06956-137">The /t:Publish option only builds the Azure packages.</span></span> <span data-ttu-id="06956-138">Stejně jako příkazy Publikovat v sadě Visual Studio Nenasazuje balíčky.</span><span class="sxs-lookup"><span data-stu-id="06956-138">It does not deploy the packages as the Publish commands in Visual Studio do.</span></span>

   <span data-ttu-id="06956-139">Volitelně můžete zadat název projektu jako parametr MSBuild.</span><span class="sxs-lookup"><span data-stu-id="06956-139">Optionally, you can specify the project name as an MSBuild parameter.</span></span> <span data-ttu-id="06956-140">Pokud není zadaný, použije se aktuální adresář.</span><span class="sxs-lookup"><span data-stu-id="06956-140">If not specified, the current directory is used.</span></span> <span data-ttu-id="06956-141">Další informace o možnostech příkazového řádku nástroje MSBuild najdete v tématu [příkazového řádku MSBuild – Reference](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="06956-141">For more information about MSBuild command line options, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).</span></span>
4. <span data-ttu-id="06956-142">Vyhledejte výstup.</span><span class="sxs-lookup"><span data-stu-id="06956-142">Locate the output.</span></span> <span data-ttu-id="06956-143">Ve výchozím nastavení, tento příkaz vytvoří adresář ve vztahu k kořenové složce projektu, například *ProjectDir*\\bin\\*konfigurace*\\app.publish\\.</span><span class="sxs-lookup"><span data-stu-id="06956-143">By default, this command creates a directory in relation to the root folder for the project, such as *ProjectDir*\\bin\\*Configuration*\\app.publish\\.</span></span> <span data-ttu-id="06956-144">Při sestavování projektu Azure generovat dva soubory, samotný soubor balíčku a doprovodné konfigurační soubor:</span><span class="sxs-lookup"><span data-stu-id="06956-144">When you build an Azure project, you generate two files, the package file itself and the accompanying configuration file:</span></span>

   * <span data-ttu-id="06956-145">Project.cspkg</span><span class="sxs-lookup"><span data-stu-id="06956-145">Project.cspkg</span></span>
   * <span data-ttu-id="06956-146">Objekt ServiceConfiguration. *TargetProfile*.cscfg</span><span class="sxs-lookup"><span data-stu-id="06956-146">ServiceConfiguration.*TargetProfile*.cscfg</span></span>

   <span data-ttu-id="06956-147">Ve výchozím nastavení každý projekt, Azure zahrnuje jednu konfigurační soubor služby (soubor .cscfg) pro místní sestavení (ladění) a druhou pro sestavení cloudu (pracovním nebo produkčním), ale můžete přidat nebo odebrat soubory konfigurace služby podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="06956-147">By default, each Azure project includes one service configuration file (.cscfg file) for local (debugging) builds and another for cloud (staging or production) builds, but you can add or remove service configuration files as needed.</span></span> <span data-ttu-id="06956-148">Když vytvoříte balíček Visual Studia, zobrazí se výzva které konfigurační soubor služby zahrnout společně se balíček.</span><span class="sxs-lookup"><span data-stu-id="06956-148">When you build a package within Visual Studio, you will be asked which service configuration file to include alongside the package.</span></span>
5. <span data-ttu-id="06956-149">Zadejte konfigurační soubor služby.</span><span class="sxs-lookup"><span data-stu-id="06956-149">Specify the service configuration file.</span></span> <span data-ttu-id="06956-150">Když vytvoříte balíček pomocí nástroje MSBuild, místní služby konfigurační soubor je zahrnuté ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="06956-150">When you build a package by using MSBuild, the local service configuration file is included by default.</span></span> <span data-ttu-id="06956-151">K zahrnutí konfigurační soubor různé služby, nastavte vlastnost TargetProfile MSBuild příkazu, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="06956-151">To include a different service configuration file, set the TargetProfile property of the MSBuild command, as in the following example:</span></span>

       MSBuild /t:Publish /p:TargetProfile=Cloud
6. <span data-ttu-id="06956-152">Zadejte umístění pro výstup.</span><span class="sxs-lookup"><span data-stu-id="06956-152">Specify the location for the output.</span></span> <span data-ttu-id="06956-153">Nastavit cestu, pomocí /p:PublishDir =*Directory* \\ možnost, včetně koncové oddělovače zpětné lomítko, jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="06956-153">Set the path by using the /p:PublishDir=*Directory*\\ option, including the trailing backslash separator, as in the following example:</span></span>

       MSBuild /target:Publish /p:PublishDir=\\myserver\drops\

   <span data-ttu-id="06956-154">Jakmile jste sestavený a otestovat příslušné nástroje MSBuild příkazového řádku k vytvoření vašich projektů a zkombinovat do balíčku s Azure, můžete přidat tento příkazový řádek pro skripty sestavení.</span><span class="sxs-lookup"><span data-stu-id="06956-154">Once you've constructed and tested an appropriate MSBuild command line to build your projects and combine them into an Azure package, you can add this command line to your build scripts.</span></span> <span data-ttu-id="06956-155">Pokud váš server sestavení používá vlastní skripty, tento proces bude záviset na jaké jsou specifikace svůj vlastní sestavovací proces.</span><span class="sxs-lookup"><span data-stu-id="06956-155">If your build server uses custom scripts, this process will depend on the specifics of your custom build process.</span></span> <span data-ttu-id="06956-156">Pokud používáte jako prostředí pro sestavování sady TFS, můžete podle pokynů v dalším kroku přidejte balíček Azure sestavení do vašeho procesu sestavení.</span><span class="sxs-lookup"><span data-stu-id="06956-156">If you are using TFS as a build environment, then you can follow the instructions in the next step to add the Azure package build to your build process.</span></span>

## <a name="3-build-a-package-using-tfs-team-build"></a><span data-ttu-id="06956-157">3: vytvoření balíčku pomocí TFS Team Build</span><span class="sxs-lookup"><span data-stu-id="06956-157">3: Build a Package using TFS Team Build</span></span>
<span data-ttu-id="06956-158">Pokud máte nastavený jako řadič sestavení a sestavení serveru Team Foundation Server (TFS) nastavit jako počítač sestavení sady TFS a potom automatizované sestavení Volitelně můžete nastavit pro balíčku Azure.</span><span class="sxs-lookup"><span data-stu-id="06956-158">If you have Team Foundation Server (TFS) set up as a build controller and the build server set up as a TFS build machine, then you can optionally set up an automated build for your Azure package.</span></span> <span data-ttu-id="06956-159">Informace o tom, jak nastavit a používat jako systém sestavení Team Foundation server najdete v tématu [škálování systém sestavení][Scale out your build system].</span><span class="sxs-lookup"><span data-stu-id="06956-159">For information on how to set up and use Team Foundation server as a build system, see [Scale out your build system][Scale out your build system].</span></span> <span data-ttu-id="06956-160">Zejména následující postup předpokládá, že jste nakonfigurovali vašem serveru sestavení, jak je popsáno v [nasadit a nakonfigurovat server sestavení][Deploy and configure a build server], a že jste vytvořili týmového projektu, vytvoří projekt cloudové služby v týmový projekt.</span><span class="sxs-lookup"><span data-stu-id="06956-160">In particular, the following procedure assumes that you have configured your build server as described in [Deploy and configure a build server][Deploy and configure a build server], and that you have created a team project, created a cloud service project in the team project.</span></span>

<span data-ttu-id="06956-161">Pokud chcete konfigurovat TFS sestavovat balíčky, Azure, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="06956-161">To configure TFS to build Azure packages, perform the following steps:</span></span>

1. <span data-ttu-id="06956-162">V sadě Visual Studio ve svém vývojovém počítači, v nabídce Zobrazit zvolte **Team Explorer**, nebo zvolte Ctrl +\\, Ctrl + M.</span><span class="sxs-lookup"><span data-stu-id="06956-162">In Visual Studio on your development computer, on the View menu, choose **Team Explorer**, or choose Ctrl+\\, Ctrl+M.</span></span> <span data-ttu-id="06956-163">V okně Team Explorer rozbalte **sestavení** uzlu, nebo zvolte **sestavení** stránce a vyberte **novou definici sestavení**.</span><span class="sxs-lookup"><span data-stu-id="06956-163">In the Team Explorer window, expand the **Builds** node or choose the **Builds** page, and choose **New Build Definition**.</span></span>

   ![Nová možnost definice sestavení][0]
2. <span data-ttu-id="06956-165">Vyberte **aktivační událost** a zadejte požadované podmínek, když chcete, aby balíček, který má být sestaven.</span><span class="sxs-lookup"><span data-stu-id="06956-165">Choose the **Trigger** tab, and specify the desired conditions for when you want the package to be built.</span></span> <span data-ttu-id="06956-166">Můžete například zadat **průběžnou integraci** dojde k vytvoření balíčku vždy, když zdroj řídí změnami.</span><span class="sxs-lookup"><span data-stu-id="06956-166">For example, specify **Continuous Integration** to build the package whenever a source control check-in occurs.</span></span>
3. <span data-ttu-id="06956-167">Vyberte **nastavení zdroje** kartě a zajistěte, aby složky projektu, je uvedena ve **zdrojové složky řízení** sloupce a stav je **Active**.</span><span class="sxs-lookup"><span data-stu-id="06956-167">Choose the **Source Settings** tab, and make sure your project folder is listed in the **Source Control Folder** column, and the status is **Active**.</span></span>
4. <span data-ttu-id="06956-168">Vyberte **sestavení výchozí** a v části sestavení řadič, ověřte název serveru sestavení.</span><span class="sxs-lookup"><span data-stu-id="06956-168">Choose the **Build Defaults** tab, and under Build controller, verify the name of the build server.</span></span>  <span data-ttu-id="06956-169">Také, zvolte možnost **kopie sestavení výstup do následující složky, vyřaďte** a zadejte požadované rozevírací umístění.</span><span class="sxs-lookup"><span data-stu-id="06956-169">Also, choose the option **Copy build output to the following drop folder** and specify the desired drop location.</span></span>
5. <span data-ttu-id="06956-170">Vyberte **proces** kartě. Na kartě procesu zvolit výchozí šablonu, v části **sestavení**, zvolte projekt, pokud není vybrána a rozbalte **Upřesnit** tématu **sestavení** části mřížky.</span><span class="sxs-lookup"><span data-stu-id="06956-170">Choose the **Process** tab. On the Process tab, choose the default template, under **Build**, choose the project if it is not already selected, and expand the **Advanced** section in the **Build** section of the grid.</span></span>
6. <span data-ttu-id="06956-171">Zvolte **argumenty MSBuild**a nastavte příslušné argumenty příkazového řádku nástroje MSBuild, jak je popsáno v kroku 2 výše.</span><span class="sxs-lookup"><span data-stu-id="06956-171">Choose **MSBuild Arguments**, and set the appropriate MSBuild command line arguments as described in Step 2 above.</span></span> <span data-ttu-id="06956-172">Zadejte například **/t: publikování /p:PublishDir =\\\\myserver\\zahodí\\**  k vytvoření balíčku a zkopírujte soubory balíčku do umístění \\ \\myserver\\zahodí\\:</span><span class="sxs-lookup"><span data-stu-id="06956-172">For example, enter **/t:Publish /p:PublishDir=\\\\myserver\\drops\\** to build a package and copy the package files to the location \\\\myserver\\drops\\:</span></span>

   ![Argumenty MSBuild][2]

   > [!NOTE]
   > <span data-ttu-id="06956-174">Kopírování souborů do veřejného umístění usnadňuje ručně nasadit balíčky z vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="06956-174">Copying the files to a public share makes it easier to manually deploy the packages from your development computer.</span></span>
7. <span data-ttu-id="06956-175">Kontrola v změn do projektu test úspěch kroku sestavení nebo fronty, nové sestavení.</span><span class="sxs-lookup"><span data-stu-id="06956-175">Test the success of your build step by checking in a change to your project, or queue up a new build.</span></span> <span data-ttu-id="06956-176">Fronty, nové sestavení, v Team Exploreru kliknete pravým tlačítkem na **všechny definice sestavení,** a potom zvolte **fronty nové sestavení**.</span><span class="sxs-lookup"><span data-stu-id="06956-176">To queue up a new build, in the Team Explorer, right-click **All Build Definitions,** and then choose **Queue New Build**.</span></span>

## <a name="4-publish-a-package-using-a-powershell-script"></a><span data-ttu-id="06956-177">4: publikování balíčku pomocí skriptu prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="06956-177">4: Publish a Package using a PowerShell Script</span></span>
<span data-ttu-id="06956-178">Tato část popisuje, jak vytvořit skript prostředí Windows PowerShell, která bude publikovat balíček výstup cloudové aplikace do Azure pomocí volitelné parametry.</span><span class="sxs-lookup"><span data-stu-id="06956-178">This section describes how to construct a Windows PowerShell script that will publish the Cloud app package output to Azure using optional parameters.</span></span> <span data-ttu-id="06956-179">Tento skript je možné volat po kroku sestavení ve vaší vlastní sestavovací automation.</span><span class="sxs-lookup"><span data-stu-id="06956-179">This script can be called after the build step in your custom build automation.</span></span> <span data-ttu-id="06956-180">Také může být volána z aktivit pracovního postupu šablonu procesu v sestavení Team TFS s Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="06956-180">It can also be called from Process Template workflow activities in Visual Studio TFS Team Build.</span></span>

1. <span data-ttu-id="06956-181">Nainstalujte [rutin prostředí Azure PowerShell] [ Azure PowerShell cmdlets] (v0.6.1 nebo vyšší).</span><span class="sxs-lookup"><span data-stu-id="06956-181">Install the [Azure PowerShell cmdlets][Azure PowerShell cmdlets] (v0.6.1 or higher).</span></span>
   <span data-ttu-id="06956-182">Během fáze instalace rutiny zvolte instalaci jako modul snap-in.</span><span class="sxs-lookup"><span data-stu-id="06956-182">During the cmdlet setup phase, choose to install as a snap-in.</span></span> <span data-ttu-id="06956-183">Všimněte si, že tato oficiálně podporované verze nahrazuje starší verze nabízeným přes webu CodePlex, i když bylo předchozí verze číslované 2.x.x.</span><span class="sxs-lookup"><span data-stu-id="06956-183">Note that this officially supported version replaces the older version offered through CodePlex, although the previous versions were numbered 2.x.x.</span></span>
2. <span data-ttu-id="06956-184">Otevřete prostředí Azure PowerShell pomocí nabídky Start nebo úvodní stránka.</span><span class="sxs-lookup"><span data-stu-id="06956-184">Start Azure PowerShell using the Start menu or Start page.</span></span> <span data-ttu-id="06956-185">Pokud spustíte tímto způsobem, budou načteny rutin prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="06956-185">If you start in this way, the Azure PowerShell cmdlets will be loaded.</span></span>
3. <span data-ttu-id="06956-186">Do příkazového řádku Powershellu, ověřte, zda jsou načteny rutin prostředí PowerShell zadáním příkazu částečné `Get-Azure` a stisknutím klávesy tabulátor pro dokončování.</span><span class="sxs-lookup"><span data-stu-id="06956-186">At the PowerShell prompt, verify that the PowerShell cmdlets are loaded by entering the partial command `Get-Azure` and then pressing the Tab key for statement completion.</span></span>

   <span data-ttu-id="06956-187">Pokud opakovaně stiskněte klávesu Tabulátor, měli byste vidět různé příkazy prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="06956-187">If you press the Tab key repeatedly, you should see various Azure PowerShell commands.</span></span>
4. <span data-ttu-id="06956-188">Ověřte, že se můžete připojit k předplatnému Azure pomocí Import ze souboru .publishsettings informace o vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="06956-188">Verify that you can connect to your Azure subscription by importing your subscription information from the .publishsettings file.</span></span>

   `Import-AzurePublishSettingsFile c:\scripts\WindowsAzure\default.publishsettings`

   <span data-ttu-id="06956-189">Potom zadejte příkaz</span><span class="sxs-lookup"><span data-stu-id="06956-189">Then enter the command</span></span>

   `Get-AzureSubscription`

   <span data-ttu-id="06956-190">Ukazuje to informace o vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="06956-190">This shows information about your subscription.</span></span> <span data-ttu-id="06956-191">Ověřte, že je vše v pořádku.</span><span class="sxs-lookup"><span data-stu-id="06956-191">Verify that everything is correct.</span></span>
5. <span data-ttu-id="06956-192">Uložit šablonu skript zadaná na konci tohoto článku do složky skriptů jako c:\\skripty\\WindowsAzure\\**PublishCloudService.ps1**.</span><span class="sxs-lookup"><span data-stu-id="06956-192">Save the script template provided at the end of this article to your scripts folder as c:\\scripts\\WindowsAzure\\**PublishCloudService.ps1**.</span></span>
6. <span data-ttu-id="06956-193">Projděte si část parametry skriptu.</span><span class="sxs-lookup"><span data-stu-id="06956-193">Review the parameters section of the script.</span></span> <span data-ttu-id="06956-194">Přidat nebo upravit všechny výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="06956-194">Add or modify any default values.</span></span> <span data-ttu-id="06956-195">Tyto hodnoty mohou být přepsány vždy explicitní parametrů.</span><span class="sxs-lookup"><span data-stu-id="06956-195">These values can always be overridden by passing in explicit parameters.</span></span>
7. <span data-ttu-id="06956-196">Zajistí existuje, platný cloudové služby a úložiště účtů v rámci vašeho předplatného, které mohou být zaměřeny skriptem publikovat vytvoření.</span><span class="sxs-lookup"><span data-stu-id="06956-196">Ensure there are valid cloud service and storage accounts created in your subscription that can be targeted by the publish script.</span></span> <span data-ttu-id="06956-197">Účet úložiště (úložiště objektů blob) se použije k nahrání a nasazení balíčku a konfigurační soubor dočasně uložit, je při vytváření nasazení.</span><span class="sxs-lookup"><span data-stu-id="06956-197">The storage account (blob storage) will be used to upload and temporarily store the deployment package and config file while the deployment is being created.</span></span>

   * <span data-ttu-id="06956-198">Pokud chcete vytvořit novou cloudovou službu, můžete volat tento skript nebo použít [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="06956-198">To create a new cloud service, you can call this script or use the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="06956-199">Název cloudové služby se použije jako předponu v platný plně kvalifikovaný název domény, a proto musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="06956-199">The cloud service name will be used as a prefix in a fully qualified domain name and hence it must be unique.</span></span>

         New-AzureService -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
   * <span data-ttu-id="06956-200">Pokud chcete vytvořit nový účet úložiště, můžete volat tento skript nebo použít [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="06956-200">To create a new storage account, you can call this script or use the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="06956-201">Název účtu úložiště se použije jako předponu v platný plně kvalifikovaný název domény, a proto musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="06956-201">The storage account name will be used as a prefix in a fully qualified domain name and hence it must be unique.</span></span> <span data-ttu-id="06956-202">Můžete se pokusit použít stejný název jako cloudová služba.</span><span class="sxs-lookup"><span data-stu-id="06956-202">You can try using the same name as the cloud service.</span></span>

         New-AzureStorageAccount -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
8. <span data-ttu-id="06956-203">Volání skript přímo z prostředí Azure PowerShell nebo propojit se tento skript k vaší automatizace sestavení hostitele dochází po sestavení balíčku.</span><span class="sxs-lookup"><span data-stu-id="06956-203">Call the script directly from Azure PowerShell, or wire up this script to your host build automation to occur after the package build.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="06956-204">Skript bude vždy odstranit nebo nahradit existující nasazení ve výchozím nastavení, pokud jsou zjišťovány.</span><span class="sxs-lookup"><span data-stu-id="06956-204">The script will always delete or replace your existing deployments by default if they are detected.</span></span> <span data-ttu-id="06956-205">Toto je nutná pro povolení průběžné doručování z automatizace, kde je možné použít bez vyzvání uživatele.</span><span class="sxs-lookup"><span data-stu-id="06956-205">This is necessary to enable continuous delivery from automation where no user prompting is possible.</span></span>
   >
   >

   <span data-ttu-id="06956-206">**Příklad scénáře 1:** průběžné nasazování pro pracovní prostředí služby:</span><span class="sxs-lookup"><span data-stu-id="06956-206">**Example scenario 1:** continuous deployment to the staging environment of a service:</span></span>

       PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Staging -serviceName mycloudservice -storageAccountName mystoragesaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

   <span data-ttu-id="06956-207">To je obvykle následuje test spusťte ověření a prohození virtuálních IP adres.</span><span class="sxs-lookup"><span data-stu-id="06956-207">This is typically followed up by test run verification and a VIP swap.</span></span> <span data-ttu-id="06956-208">Prohození virtuálních IP adres, můžete to udělat pomocí [portál Azure](https://portal.azure.com) nebo pomocí rutiny přesunutí nasazení.</span><span class="sxs-lookup"><span data-stu-id="06956-208">The VIP swap can be done via the [Azure portal](https://portal.azure.com) or by using the Move-Deployment cmdlet.</span></span>

   <span data-ttu-id="06956-209">**Příklad scénáře 2:** průběžné nasazování do produkčního prostředí služby vyhrazené testu</span><span class="sxs-lookup"><span data-stu-id="06956-209">**Example scenario 2:** continuous deployment to the production environment of a dedicated test service</span></span>

       PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Production -enableDeploymentUpgrade 1 -serviceName mycloudservice -storageAccountName mystorageaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

   <span data-ttu-id="06956-210">**Vzdálená plocha:**</span><span class="sxs-lookup"><span data-stu-id="06956-210">**Remote Desktop:**</span></span>

   <span data-ttu-id="06956-211">Pokud v projektu Azure, budete muset provést další kroky jednorázové zajistit, že je načtený správný certifikát služby Cloud ke všem cloudovým službám, které jsou cílem tento skript je povolené vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="06956-211">If Remote Desktop is enabled in your Azure project you will need to perform additional one-time steps to ensure the correct Cloud Service Certificate is uploaded to all cloud services targeted by this script.</span></span>

   <span data-ttu-id="06956-212">Vyhledejte hodnoty kryptografického otisku certifikátu očekávanou role.</span><span class="sxs-lookup"><span data-stu-id="06956-212">Locate the certificate thumbprint values expected by your roles.</span></span> <span data-ttu-id="06956-213">Kryptografický otisk hodnoty jsou viditelné v části certifikáty do cloudu konfiguračního souboru (tj. ServiceConfiguration.Cloud.cscfg).</span><span class="sxs-lookup"><span data-stu-id="06956-213">The thumbprint values are visible in the Certificates section of the cloud config file (i.e. ServiceConfiguration.Cloud.cscfg).</span></span> <span data-ttu-id="06956-214">Je také viditelné v dialogovém okně Konfigurace vzdálené plochy v sadě Visual Studio při zobrazení možností a zobrazení vybraný certifikát.</span><span class="sxs-lookup"><span data-stu-id="06956-214">It is also visible in the Remote Desktop Configuration dialog in Visual Studio when you Show Options and view the selected certificate.</span></span>

       <Certificates>
             <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="C33B6C432C25581601B84C80F86EC2809DC224E8" thumbprintAlgorithm="sha1" />
       </Certificates>

   <span data-ttu-id="06956-215">Nahrání certifikátů vzdálené plochy jako krok jednorázové instalace pomocí následující rutiny skriptu:</span><span class="sxs-lookup"><span data-stu-id="06956-215">Upload Remote Desktop certificates as a one-time setup step using the following cmdlet script:</span></span>

       Add-AzureCertificate -serviceName <CLOUDSERVICENAME> -certToDeploy (get-item cert:\CurrentUser\MY\<THUMBPRINT>)

   <span data-ttu-id="06956-216">Například:</span><span class="sxs-lookup"><span data-stu-id="06956-216">For example:</span></span>

       Add-AzureCertificate -serviceName 'mytestcloudservice' -certToDeploy (get-item cert:\CurrentUser\MY\C33B6C432C25581601B84C80F86EC2809DC224E8

   <span data-ttu-id="06956-217">Případně můžete exportovat soubor PFX certifikátu s privátním klíčem a nahrát certifikáty na každý cílový cloud služby pomocí [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="06956-217">Alternatively you can export the certificate file PFX with private key and upload certificates to each target cloud service using the [Azure portal](https://portal.azure.com).</span></span>

   <!---
   Fixing broken links for Azure content migration from ACOM to DOCS. I'm unable to find a replacement links, so I'm commenting out this reference for now. The author can investigate in the future. "Read the following article to learn more: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx.
   -->
   <span data-ttu-id="06956-218">**Upgrade a nasazení. Odstranění nasazení -\> nové nasazení**</span><span class="sxs-lookup"><span data-stu-id="06956-218">**Upgrade Deployment vs. Delete Deployment -\> New Deployment**</span></span>

   <span data-ttu-id="06956-219">Skript bude ve výchozím nastavení provádět nasazení upgradu ($enableDeploymentUpgrade = 1) Pokud žádný parametr se předává v nebo hodnota 1, je předaná explicitně.</span><span class="sxs-lookup"><span data-stu-id="06956-219">The script will by default perform an Upgrade Deployment ($enableDeploymentUpgrade = 1) when no parameter is passed in or the value 1 is passed explicitly.</span></span> <span data-ttu-id="06956-220">Pro jeden instance to nabízí výhodu v podobě trvá kratší dobu, než úplné nasazení.</span><span class="sxs-lookup"><span data-stu-id="06956-220">For single instances this has the advantage of taking less time than a full deployment.</span></span> <span data-ttu-id="06956-221">Pro instance, které vyžadují vysokou dostupnost, to také nabízí výhodu v podobě ponechat některé instancí spuštěných zatímco jiné jsou upgradovány (proti vaší doméně aktualizace), plus vaše virtuální IP adresy se neodstraní.</span><span class="sxs-lookup"><span data-stu-id="06956-221">For instances that require high availability this also has the advantage of leaving some instances running while others are upgraded (walking your update domain), plus your VIP will not be deleted.</span></span>

   <span data-ttu-id="06956-222">Nasazení upgradu lze zakázat ve skriptu ($enableDeploymentUpgrade = 0) nebo pomocí předání *- enableDeploymentUpgrade 0* jako parametr, který mění chování skriptu nejprve odstranit všechna stávající nasazení a pak vytvořte nové nasazení.</span><span class="sxs-lookup"><span data-stu-id="06956-222">Upgrade Deployment can be disabled in the script ($enableDeploymentUpgrade = 0) or by passing *-enableDeploymentUpgrade 0* as a parameter, which alters the script behavior to first delete any existing deployment and then create a new deployment.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="06956-223">Skript bude vždy odstranit nebo nahradit existující nasazení ve výchozím nastavení, pokud jsou zjišťovány.</span><span class="sxs-lookup"><span data-stu-id="06956-223">The script will always delete or replace your existing deployments by default if they are detected.</span></span> <span data-ttu-id="06956-224">Toto je nutná pro povolení nastavené průběžné doručování ze služby automation, kde je možné použít bez vyzvání uživatele / – operátor.</span><span class="sxs-lookup"><span data-stu-id="06956-224">This is necessary to enable continuous delivery from automation where no user/operator prompting is possible.</span></span>
   >
   >

## <a name="5-publish-a-package-using-tfs-team-build"></a><span data-ttu-id="06956-225">5: publikování balíčku pomocí TFS Team Build</span><span class="sxs-lookup"><span data-stu-id="06956-225">5: Publish a Package using TFS Team Build</span></span>
<span data-ttu-id="06956-226">Tento volitelný krok připojí TFS Team od sestavení k skript vytvořený v kroku 4, která zpracovává publikování sestavení balíčku do Azure.</span><span class="sxs-lookup"><span data-stu-id="06956-226">This optional step connects TFS Team Build to the script created in step 4, which handles publishing of the package build to Azure.</span></span> <span data-ttu-id="06956-227">To zahrnuje, úprava šablony procesu, který používá svou definici sestavení tak, aby běžel aktivitu publikovat na konci pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="06956-227">This entails modifying the Process Template used by your build definition so that it runs a Publish activity at the end of the workflow.</span></span> <span data-ttu-id="06956-228">Aktivita publikování provede příkazu prostředí PowerShell předávání v parametry z buildu.</span><span class="sxs-lookup"><span data-stu-id="06956-228">The Publish activity will execute your PowerShell command passing in parameters from the build.</span></span> <span data-ttu-id="06956-229">Výstup MSBuild cílí a publikovat skriptu se přesměruje do výstupu standardní sestavení.</span><span class="sxs-lookup"><span data-stu-id="06956-229">Output of the MSBuild targets and publish script will be piped into the standard build output.</span></span>

1. <span data-ttu-id="06956-230">Upravit definici sestavení zodpovědná za průběžné nasazování.</span><span class="sxs-lookup"><span data-stu-id="06956-230">Edit the Build Definition responsible for continuous deploy.</span></span>
2. <span data-ttu-id="06956-231">Vyberte **proces** kartě.</span><span class="sxs-lookup"><span data-stu-id="06956-231">Select the **Process** tab.</span></span>
3. <span data-ttu-id="06956-232">Postupujte podle [tyto pokyny](http://msdn.microsoft.com/library/dd647551.aspx) Přidání aktivity projektu pro šablonu procesu sestavení, stáhnout výchozí šablonu, přidejte do projektu a vrácení se změnami.</span><span class="sxs-lookup"><span data-stu-id="06956-232">Follow [these instructions](http://msdn.microsoft.com/library/dd647551.aspx) to add an Activity project for the build process template, download the default template, add it to the project and check it in.</span></span> <span data-ttu-id="06956-233">Zadejte nový název, jako je například AzureBuildProcessTemplate šablony procesu sestavení.</span><span class="sxs-lookup"><span data-stu-id="06956-233">Give the build process template a new name, such as AzureBuildProcessTemplate.</span></span>
4. <span data-ttu-id="06956-234">Vraťte se do **proces** kartě a používat **zobrazit podrobnosti** zobrazíte seznam šablony procesu sestavení k dispozici.</span><span class="sxs-lookup"><span data-stu-id="06956-234">Return to the **Process** tab, and use **Show Details** to show a list of available build process templates.</span></span> <span data-ttu-id="06956-235">Vyberte **nové...**  tlačítko a přejděte do projektu, stačí přidat a vrátit se změnami.</span><span class="sxs-lookup"><span data-stu-id="06956-235">Choose the **New...** button, and navigate to the project you just added and checked in.</span></span> <span data-ttu-id="06956-236">Vyhledejte šablonu, kterou jste právě vytvořili a zvolte **OK**.</span><span class="sxs-lookup"><span data-stu-id="06956-236">Locate the template you just created and choose **OK**.</span></span>
5. <span data-ttu-id="06956-237">Otevřete vybrané šablony procesu pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="06956-237">Open the selected Process Template for editing.</span></span> <span data-ttu-id="06956-238">Můžete otevřít přímo v Návrháři pracovních postupů, nebo v editoru XML pro práci s XAML.</span><span class="sxs-lookup"><span data-stu-id="06956-238">You can open directly in the Workflow designer or in the XML editor to work with the XAML.</span></span>
6. <span data-ttu-id="06956-239">Následující seznam argumentů, nové přidáte jako samostatný řádek položky na kartě argumenty v Návrháři pracovních postupů.</span><span class="sxs-lookup"><span data-stu-id="06956-239">Add the following list of new arguments as separate line items in the arguments tab of the workflow designer.</span></span> <span data-ttu-id="06956-240">Všechny argumenty by měl mít směr = v a typ = řetězec.</span><span class="sxs-lookup"><span data-stu-id="06956-240">All arguments should have direction=In and type=String.</span></span> <span data-ttu-id="06956-241">Tyto se použije tok parametry z definice sestavení do pracovního postupu, které pak získat používá k volání skriptu publikovat.</span><span class="sxs-lookup"><span data-stu-id="06956-241">These will be used to flow parameters from the build definition into the workflow, which then get used to call the publish script.</span></span>

       SubscriptionName
       StorageAccountName
       CloudConfigLocation
       PackageLocation
       Environment
       SubscriptionDataFileLocation
       PublishScriptLocation
       ServiceName

   ![Seznam argumentů][3]

   <span data-ttu-id="06956-243">Odpovídající XAML vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="06956-243">The corresponding XAML looks like this:</span></span>

       <Activity  _ />
         <x:Members>
           <x:Property Name="BuildSettings" Type="InArgument(mtbwa:BuildSettings)" />
           <x:Property Name="TestSpecs" Type="InArgument(mtbwa:TestSpecList)" />
           <x:Property Name="BuildNumberFormat" Type="InArgument(x:String)" />
           <x:Property Name="CleanWorkspace" Type="InArgument(mtbwa:CleanWorkspaceOption)" />
           <x:Property Name="RunCodeAnalysis" Type="InArgument(mtbwa:CodeAnalysisOption)" />
           <x:Property Name="SourceAndSymbolServerSettings" Type="InArgument(mtbwa:SourceAndSymbolServerSettings)" />
           <x:Property Name="AgentSettings" Type="InArgument(mtbwa:AgentSettings)" />
           <x:Property Name="AssociateChangesetsAndWorkItems" Type="InArgument(x:Boolean)" />
           <x:Property Name="CreateWorkItem" Type="InArgument(x:Boolean)" />
           <x:Property Name="DropBuild" Type="InArgument(x:Boolean)" />
           <x:Property Name="MSBuildArguments" Type="InArgument(x:String)" />
           <x:Property Name="MSBuildPlatform" Type="InArgument(mtbwa:ToolPlatform)" />
           <x:Property Name="PerformTestImpactAnalysis" Type="InArgument(x:Boolean)" />
           <x:Property Name="CreateLabel" Type="InArgument(x:Boolean)" />
           <x:Property Name="DisableTests" Type="InArgument(x:Boolean)" />
           <x:Property Name="GetVersion" Type="InArgument(x:String)" />
           <x:Property Name="PrivateDropLocation" Type="InArgument(x:String)" />
           <x:Property Name="Verbosity" Type="InArgument(mtbw:BuildVerbosity)" />
           <x:Property Name="Metadata" Type="mtbw:ProcessParameterMetadataCollection" />
           <x:Property Name="SupportedReasons" Type="mtbc:BuildReason" />
           <x:Property Name="SubscriptionName" Type="InArgument(x:String)" />
           <x:Property Name="StorageAccountName" Type="InArgument(x:String)" />
           <x:Property Name="CloudConfigLocation" Type="InArgument(x:String)" />
           <x:Property Name="PackageLocation" Type="InArgument(x:String)" />
           <x:Property Name="Environment" Type="InArgument(x:String)" />
           <x:Property Name="SubscriptionDataFileLocation" Type="InArgument(x:String)" />
           <x:Property Name="PublishScriptLocation" Type="InArgument(x:String)" />
           <x:Property Name="ServiceName" Type="InArgument(x:String)" />
         </x:Members>

         <this:Process.MSBuildArguments>
7. <span data-ttu-id="06956-244">Přidejte nové pořadí na konci spustit na agenta:</span><span class="sxs-lookup"><span data-stu-id="06956-244">Add a new sequence at the end of Run On Agent:</span></span>

   1. <span data-ttu-id="06956-245">Začněte přidáním aktivitu Pokud příkaz zkontrolujte soubor platný skriptu.</span><span class="sxs-lookup"><span data-stu-id="06956-245">Start by adding an If Statement activity to check for a valid script file.</span></span> <span data-ttu-id="06956-246">Podmínka nastavená na tuto hodnotu:</span><span class="sxs-lookup"><span data-stu-id="06956-246">Set the condition to this value:</span></span>

          Not String.IsNullOrEmpty(PublishScriptLocation)
   2. <span data-ttu-id="06956-247">Pak případ Pokud příkaz Přidat novou aktivitu pořadí.</span><span class="sxs-lookup"><span data-stu-id="06956-247">In the Then case of the If Statement, add a new Sequence activity.</span></span> <span data-ttu-id="06956-248">Nastavit počáteční publikovat zobrazovaný název</span><span class="sxs-lookup"><span data-stu-id="06956-248">Set the display name to 'Start publish'</span></span>
   3. <span data-ttu-id="06956-249">S spuštění publikování pořadí při výběru, přidejte následující seznam nové proměnné jako samostatné řádku položky v kartě proměnné v Návrháři pracovních postupů.</span><span class="sxs-lookup"><span data-stu-id="06956-249">With the Start publish sequence still selected, add the following list of new variables as separate line items in the variables tab of the workflow designer.</span></span> <span data-ttu-id="06956-250">Všechny proměnné, měl by být proměnné typ = řetězec a obor = počáteční publikovat.</span><span class="sxs-lookup"><span data-stu-id="06956-250">All variables should have Variable type =String and Scope=Start publish.</span></span> <span data-ttu-id="06956-251">Tyto se použije tok parametry z definice sestavení do pracovního postupu, které pak získat používá k volání skriptu publikovat.</span><span class="sxs-lookup"><span data-stu-id="06956-251">These will be used to flow parameters from the build definition into the workflow, which then get used to call the publish script.</span></span>

      * <span data-ttu-id="06956-252">SubscriptionDataFilePath typu řetězec.</span><span class="sxs-lookup"><span data-stu-id="06956-252">SubscriptionDataFilePath, of type String</span></span>
      * <span data-ttu-id="06956-253">PublishScriptFilePath typu řetězec.</span><span class="sxs-lookup"><span data-stu-id="06956-253">PublishScriptFilePath, of type String</span></span>

        ![Nové proměnné][4]
   4. <span data-ttu-id="06956-255">Pokud používáte sady TFS 2012 nebo starším, přidejte aktivitu ConvertWorkspaceItem na začátku nového pořadí.</span><span class="sxs-lookup"><span data-stu-id="06956-255">If you are using TFS 2012 or earlier, add a ConvertWorkspaceItem activity at the beginning of the new Sequence.</span></span> <span data-ttu-id="06956-256">Pokud používáte sady TFS 2013 nebo novější, přidejte aktivitu GetLocalPath na začátku nového pořadí.</span><span class="sxs-lookup"><span data-stu-id="06956-256">If you are using TFS 2013 or later, add a GetLocalPath activity at the beginning of the new sequence.</span></span> <span data-ttu-id="06956-257">Pro ConvertWorkspaceItem, nastavte vlastnosti takto: směr = ServerToLocal, DisplayName = název souboru skriptu publikovat převést, vstup = PublishScriptLocation, výsledek = PublishScriptFilePath, pracovní prostor = 'Prostoru'.</span><span class="sxs-lookup"><span data-stu-id="06956-257">For a ConvertWorkspaceItem, set the properties as follows: Direction=ServerToLocal, DisplayName='Convert publish script filename', Input=' PublishScriptLocation', Result='PublishScriptFilePath', Workspace='Workspace'.</span></span> <span data-ttu-id="06956-258">Pro aktivitu GetLocalPath nastavte vlastnost IncomingPath na 'PublishScriptLocation' a výsledek, který má 'PublishScriptFilePath'.</span><span class="sxs-lookup"><span data-stu-id="06956-258">For a GetLocalPath activity, set the property IncomingPath to 'PublishScriptLocation', and the Result to 'PublishScriptFilePath'.</span></span> <span data-ttu-id="06956-259">Tato aktivita převede cestu do skriptu publikování z TFS umístění serveru (Pokud je k dispozici) na místní disk standardní cestu.</span><span class="sxs-lookup"><span data-stu-id="06956-259">This activity converts the path to the publish script from TFS server locations (if applicable) to a standard local disk path.</span></span>
   5. <span data-ttu-id="06956-260">Pokud používáte sady TFS 2012 nebo starším, přidejte další aktivitu ConvertWorkspaceItem na konci tohoto nového pořadí.</span><span class="sxs-lookup"><span data-stu-id="06956-260">If you are using TFS 2012 or earlier, add another ConvertWorkspaceItem activity at the end of the new Sequence.</span></span> <span data-ttu-id="06956-261">Směr ServerToLocal, DisplayName = = převést předplatné filename, vstup = SubscriptionDataFileLocation, výsledek = SubscriptionDataFilePath, pracovní prostor = 'Prostoru'.</span><span class="sxs-lookup"><span data-stu-id="06956-261">Direction=ServerToLocal, DisplayName='Convert subscription filename', Input=' SubscriptionDataFileLocation', Result= 'SubscriptionDataFilePath', Workspace='Workspace'.</span></span> <span data-ttu-id="06956-262">Pokud používáte sady TFS 2013 nebo novější, přidejte další GetLocalPath.</span><span class="sxs-lookup"><span data-stu-id="06956-262">If you are using TFS 2013 or later, add another GetLocalPath.</span></span> <span data-ttu-id="06956-263">IncomingPath = 'SubscriptionDataFileLocation' a výsledek = "SubscriptionDataFilePath."</span><span class="sxs-lookup"><span data-stu-id="06956-263">IncomingPath='SubscriptionDataFileLocation', and Result='SubscriptionDataFilePath.'</span></span>
   6. <span data-ttu-id="06956-264">Přidejte aktivitu, InvokeProcess na konci tohoto nového pořadí.</span><span class="sxs-lookup"><span data-stu-id="06956-264">Add an InvokeProcess activity at the end of the new Sequence.</span></span>
      <span data-ttu-id="06956-265">Tato aktivita volá PowerShell.exe s argumenty předaná v definici sestavení.</span><span class="sxs-lookup"><span data-stu-id="06956-265">This activity calls PowerShell.exe with the arguments passed in by the Build Definition.</span></span>

      + <span data-ttu-id="06956-266">Argumenty = String.Format ("-""{0}" "- serviceName {1} - storageAccountName {2} - packageLocation""{3}" "– cloudConfigLocation""{4}" "– subscriptionDataFile""{5}" "- selectedSubscription {6} souboru-prostředí""{7}" "", PublishScriptFilePath, ServiceName, StorageAccountName, PackageLocation, CloudConfigLocation, SubscriptionDataFilePath, Název_předplatného, prostředí)</span><span class="sxs-lookup"><span data-stu-id="06956-266">Arguments = String.Format(" -File ""{0}"" -serviceName {1}  -storageAccountName {2} -packageLocation ""{3}""  -cloudConfigLocation ""{4}"" -subscriptionDataFile ""{5}""  -selectedSubscription {6} -environment ""{7}""",  PublishScriptFilePath, ServiceName, StorageAccountName,  PackageLocation, CloudConfigLocation,  SubscriptionDataFilePath, SubscriptionName, Environment)</span></span>
      + <span data-ttu-id="06956-267">DisplayName = Execute publikování skriptu</span><span class="sxs-lookup"><span data-stu-id="06956-267">DisplayName = Execute publish script</span></span>
      + <span data-ttu-id="06956-268">Název souboru = "PowerShell" (použít uvozovky)</span><span class="sxs-lookup"><span data-stu-id="06956-268">FileName = "PowerShell" (include the quotes)</span></span>
      + <span data-ttu-id="06956-269">OutputEncoding = System.Text.Encoding.GetEncoding(System.Globalization.CultureInfo.InstalledUICulture.TextInfo.OEMCodePage)</span><span class="sxs-lookup"><span data-stu-id="06956-269">OutputEncoding=  System.Text.Encoding.GetEncoding(System.Globalization.CultureInfo.InstalledUICulture.TextInfo.OEMCodePage)</span></span>
   7. <span data-ttu-id="06956-270">V **zpracování standardní výstup** část textového InvokeProcess, nastavte hodnotu textové pole na "data".</span><span class="sxs-lookup"><span data-stu-id="06956-270">In the **Handle Standard Output** section textbox of the InvokeProcess, set the textbox value to 'data'.</span></span> <span data-ttu-id="06956-271">Toto je proměnnou pro uložení standardní výstupní data.</span><span class="sxs-lookup"><span data-stu-id="06956-271">This is a variable to store the standard output data.</span></span>
   8. <span data-ttu-id="06956-272">Přidat aktivitu WriteBuildMessage právě níže **zpracování standardní výstup** části.</span><span class="sxs-lookup"><span data-stu-id="06956-272">Add a WriteBuildMessage activity just below the **Handle Standard Output** section.</span></span> <span data-ttu-id="06956-273">Nastavit význam = 'Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High' a zpráva = "data".</span><span class="sxs-lookup"><span data-stu-id="06956-273">Set the Importance = 'Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High' and the Message='data'.</span></span> <span data-ttu-id="06956-274">Tím se zajistí ve standardním výstupu skriptu získat zapíšou do výstupu sestavení.</span><span class="sxs-lookup"><span data-stu-id="06956-274">This ensures the standard output of the script will get written to the build output.</span></span>
   9. <span data-ttu-id="06956-275">V **zpracovávat chyby výstupu** část textového InvokeProcess, nastavte hodnotu textové pole na "data".</span><span class="sxs-lookup"><span data-stu-id="06956-275">In the **Handle Error Output** section textbox of the InvokeProcess, set the textbox value to 'data'.</span></span> <span data-ttu-id="06956-276">Toto je proměnnou pro uložení dat standardní chyba.</span><span class="sxs-lookup"><span data-stu-id="06956-276">This is a variable to store the standard error data.</span></span>
   10. <span data-ttu-id="06956-277">Přidat aktivitu WriteBuildError právě níže **zpracovávat chyby výstupu** části.</span><span class="sxs-lookup"><span data-stu-id="06956-277">Add a WriteBuildError activity just below the **Handle Error Output** section.</span></span> <span data-ttu-id="06956-278">Nastavit zprávu = "data".</span><span class="sxs-lookup"><span data-stu-id="06956-278">Set the Message='data'.</span></span> <span data-ttu-id="06956-279">Tím se zajistí, že standard chyby skriptu získat zapíšou do výstupu chyby sestavení.</span><span class="sxs-lookup"><span data-stu-id="06956-279">This ensures the standard errors of the script will get written to the build error output.</span></span>
   11. <span data-ttu-id="06956-280">Opravte všechny chyby, indikován blue vykřičníku značky.</span><span class="sxs-lookup"><span data-stu-id="06956-280">Correct any errors, indicated by blue exclamation marks.</span></span> <span data-ttu-id="06956-281">Najeďte myší značky vykřičníku získat nápovědu o této chybě.</span><span class="sxs-lookup"><span data-stu-id="06956-281">Hover over the exclamation marks to get a hint about the error.</span></span> <span data-ttu-id="06956-282">Uložení pracovního postupu vymazat chyby.</span><span class="sxs-lookup"><span data-stu-id="06956-282">Save the workflow to clear errors.</span></span>

   <span data-ttu-id="06956-283">Konečný výsledek aktivit pracovního postupu publikovat bude v Návrháři vypadat například takto:</span><span class="sxs-lookup"><span data-stu-id="06956-283">The final result of the publish workflow activities will look like this in the designer:</span></span>

   ![Aktivity pracovního postupu][5]

   <span data-ttu-id="06956-285">Konečný výsledek aktivit pracovního postupu publikovat bude vypadat takto v jazyce XAML:</span><span class="sxs-lookup"><span data-stu-id="06956-285">The final result of the publish workflow activities will look like this in XAML:</span></span>

       <If Condition="[Not String.IsNullOrEmpty(PublishScriptLocation)]" sap2010:WorkflowViewState.IdRef="If_1">
           <If.Then>
             <Sequence DisplayName="Start Publish" sap2010:WorkflowViewState.IdRef="Sequence_4">
               <Sequence.Variables>
                 <Variable x:TypeArguments="x:String" Name="SubscriptionDataFilePath" />
                 <Variable x:TypeArguments="x:String" Name="PublishScriptFilePath" />
               </Sequence.Variables>
               <mtbwa:ConvertWorkspaceItem DisplayName="Convert publish script filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_1" Input="[PublishScriptLocation]" Result="[PublishScriptFilePath]" Workspace="[Workspace]" />
               <mtbwa:ConvertWorkspaceItem DisplayName="Convert subscription filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_2" Input="[SubscriptionDataFileLocation]" Result="[SubscriptionDataFilePath]" Workspace="[Workspace]" />
               <mtbwa:InvokeProcess Arguments="[String.Format(&quot; -File &quot;&quot;{0}&quot;&quot; -serviceName {1}&#xD;&#xA;            -storageAccountName {2} -packageLocation &quot;&quot;{3}&quot;&quot;&#xD;&#xA;            -cloudConfigLocation &quot;&quot;{4}&quot;&quot; -subscriptionDataFile &quot;&quot;{5}&quot;&quot;&#xD;&#xA;            -selectedSubscription {6} -environment &quot;&quot;{7}&quot;&quot;&quot;,&#xD;&#xA;            PublishScriptFilePath, ServiceName, StorageAccountName,&#xD;&#xA;            PackageLocation, CloudConfigLocation,&#xD;&#xA;            SubscriptionDataFilePath, SubscriptionName, Environment)]" DisplayName="'Execute Publish Script'" FileName="[PowerShell]" sap2010:WorkflowViewState.IdRef="InvokeProcess_1">
                 <mtbwa:InvokeProcess.ErrorDataReceived>
                   <ActivityAction x:TypeArguments="x:String">
                     <ActivityAction.Argument>
                       <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                     </ActivityAction.Argument>
                     <mtbwa:WriteBuildError Message="{x:Null}" sap2010:WorkflowViewState.IdRef="WriteBuildError_1" />
                   </ActivityAction>
                 </mtbwa:InvokeProcess.ErrorDataReceived>
                 <mtbwa:InvokeProcess.OutputDataReceived>
                   <ActivityAction x:TypeArguments="x:String">
                     <ActivityAction.Argument>
                       <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                     </ActivityAction.Argument>
                     <mtbwa:WriteBuildMessage sap2010:WorkflowViewState.IdRef="WriteBuildMessage_2" Importance="[Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High]" Message="[data]" mva:VisualBasic.Settings="Assembly references and imported namespaces serialized as XML namespaces" />
                   </ActivityAction>
                 </mtbwa:InvokeProcess.OutputDataReceived>
               </mtbwa:InvokeProcess>
             </Sequence>
           </If.Then>
         </If>
       </Sequence>
8. <span data-ttu-id="06956-286">Uložte tento soubor pracovní postup šablony procesu sestavení a vrátit se změnami.</span><span class="sxs-lookup"><span data-stu-id="06956-286">Save the build process template workflow and Check In this file.</span></span>
9. <span data-ttu-id="06956-287">Upravit definici sestavení (zavřete ho Pokud už je otevřený) a vyberte **nový** tlačítko, pokud ještě nevidíte nové šablony v seznamu šablon procesů.</span><span class="sxs-lookup"><span data-stu-id="06956-287">Edit the build definition (close it if it is already open), and select the **New** button if you do not yet see the new template in the list of Process Templates.</span></span>
10. <span data-ttu-id="06956-288">Nastavte parametr hodnoty vlastností v části různé takto:</span><span class="sxs-lookup"><span data-stu-id="06956-288">Set the parameter property values in the Misc section as follows:</span></span>

    1. <span data-ttu-id="06956-289">CloudConfigLocation = "c:\\zahodí\\app.publish\\ServiceConfiguration.Cloud.cscfg' *tato hodnota se odvozuje od: ($PublishDir)ServiceConfiguration.Cloud.cscfg*</span><span class="sxs-lookup"><span data-stu-id="06956-289">CloudConfigLocation ='c:\\drops\\app.publish\\ServiceConfiguration.Cloud.cscfg' *This value is derived from: ($PublishDir)ServiceConfiguration.Cloud.cscfg*</span></span>
    2. <span data-ttu-id="06956-290">PackageLocation = "c:\\zahodí\\app.publish\\ContactManager.Azure.cspkg' *tato hodnota se odvozuje od: ($PublishDir)($ProjectName) .cspkg*</span><span class="sxs-lookup"><span data-stu-id="06956-290">PackageLocation = 'c:\\drops\\app.publish\\ContactManager.Azure.cspkg' *This value is derived from: ($PublishDir)($ProjectName).cspkg*</span></span>
    3. <span data-ttu-id="06956-291">PublishScriptLocation = "c:\\skripty\\WindowsAzure\\PublishCloudService.ps1.</span><span class="sxs-lookup"><span data-stu-id="06956-291">PublishScriptLocation = 'c:\\scripts\\WindowsAzure\\PublishCloudService.ps1'</span></span>
    4. <span data-ttu-id="06956-292">ServiceName = 'mycloudservicename' *zde použít název příslušné cloudové služby*</span><span class="sxs-lookup"><span data-stu-id="06956-292">ServiceName = 'mycloudservicename' *Use the appropriate cloud service name here*</span></span>
    5. <span data-ttu-id="06956-293">Prostředí = "pracovní.</span><span class="sxs-lookup"><span data-stu-id="06956-293">Environment = 'Staging'</span></span>
    6. <span data-ttu-id="06956-294">StorageAccountName = 'mystorageaccountname' *zde použít název účtu příslušné úložiště*</span><span class="sxs-lookup"><span data-stu-id="06956-294">StorageAccountName = 'mystorageaccountname' *Use the appropriate storage account name here*</span></span>
    7. <span data-ttu-id="06956-295">SubscriptionDataFileLocation = "c:\\skripty\\WindowsAzure\\Subscription.xml.</span><span class="sxs-lookup"><span data-stu-id="06956-295">SubscriptionDataFileLocation = 'c:\\scripts\\WindowsAzure\\Subscription.xml'</span></span>
    8. <span data-ttu-id="06956-296">Název_předplatného = "default"</span><span class="sxs-lookup"><span data-stu-id="06956-296">SubscriptionName = 'default'</span></span>

    ![Vlastnost hodnoty parametru][6]
11. <span data-ttu-id="06956-298">Uložte změny do definice sestavení.</span><span class="sxs-lookup"><span data-stu-id="06956-298">Save the changes to the Build Definition.</span></span>
12. <span data-ttu-id="06956-299">Fronta sestavení ke spouštění balíčku sestavení a publikování.</span><span class="sxs-lookup"><span data-stu-id="06956-299">Queue a Build to execute both the package build and publish.</span></span> <span data-ttu-id="06956-300">Pokud je nastavena na průběžnou integraci aktivační událost, provede toto chování na každý vrácení se změnami.</span><span class="sxs-lookup"><span data-stu-id="06956-300">If you have a trigger set to Continuous Integration, you will execute this behavior on every check-in.</span></span>

### <a name="publishcloudserviceps1-script-template"></a><span data-ttu-id="06956-301">Šablona PublishCloudService.ps1 skriptu</span><span class="sxs-lookup"><span data-stu-id="06956-301">PublishCloudService.ps1 script template</span></span>
```
Param(  $serviceName = "",
        $storageAccountName = "",
        $packageLocation = "",
        $cloudConfigLocation = "",
        $environment = "Staging",
        $deploymentLabel = "ContinuousDeploy to $servicename",
        $timeStampFormat = "g",
        $alwaysDeleteExistingDeployments = 1,
        $enableDeploymentUpgrade = 1,
        $selectedsubscription = "default",
        $subscriptionDataFile = ""
     )


function Publish()
{
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot -ErrorVariable a -ErrorAction silentlycontinue
    if ($a[0] -ne $null)
    {
        Write-Output "$(Get-Date -f $timeStampFormat) - No deployment is detected. Creating a new deployment. "
    }
    #check for existing deployment and then either upgrade, delete + deploy, or cancel according to $alwaysDeleteExistingDeployments and $enableDeploymentUpgrade boolean variables
    if ($deployment.Name -ne $null)
    {
        switch ($alwaysDeleteExistingDeployments)
        {
            1
            {
                switch ($enableDeploymentUpgrade)
                {
                    1  #Update deployment inplace (usually faster, cheaper, won't destroy VIP)
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Upgrading deployment."
                        UpgradeDeployment
                    }
                    0  #Delete then create new deployment
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Deleting deployment."
                        DeleteDeployment
                        CreateNewDeployment

                    }
                } # switch ($enableDeploymentUpgrade)
            }
            0
            {
                Write-Output "$(Get-Date -f $timeStampFormat) - ERROR: Deployment exists in $servicename.  Script execution cancelled."
                exit
            }
        } #switch ($alwaysDeleteExistingDeployments)
    } else {
            CreateNewDeployment
    }
}

function CreateNewDeployment()
{
    write-progress -id 3 -activity "Creating New Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: In progress"

    $opstat = New-AzureDeployment -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Creating New Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: Complete, Deployment ID: $completeDeploymentID"

    StartInstances
}

function UpgradeDeployment()
{
    write-progress -id 3 -activity "Upgrading Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: In progress"

    # perform Update-Deployment
    $setdeployment = Set-AzureDeployment -Upgrade -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName -Force

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Upgrading Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: Complete, Deployment ID: $completeDeploymentID"
}

function DeleteDeployment()
{

    write-progress -id 2 -activity "Deleting Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: In progress"

    #WARNING - always deletes with force
    $removeDeployment = Remove-AzureDeployment -Slot $slot -ServiceName $serviceName -Force

    write-progress -id 2 -activity "Deleting Deployment: Complete" -completed -Status $removeDeployment
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: Complete"

}

function StartInstances()
{
    write-progress -id 4 -activity "Starting Instances" -status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: In progress"

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $runstatus = $deployment.Status

    if ($runstatus -ne 'Running')
    {
        $run = Set-AzureDeployment -Slot $slot -ServiceName $serviceName -Status Running
    }
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $oldStatusStr = @("") * $deployment.RoleInstanceList.Count

    while (-not(AllInstancesRunning($deployment.RoleInstanceList)))
    {
        $i = 1
        foreach ($roleInstance in $deployment.RoleInstanceList)
        {
            $instanceName = $roleInstance.InstanceName
            $instanceStatus = $roleInstance.InstanceStatus

            if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
            {
                $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
                Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
            }

            write-progress -id (4 + $i) -activity "Starting Instance '$instanceName'" -status "$instanceStatus"
            $i = $i + 1
        }

        sleep -Seconds 1

        $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    }

    $i = 1
    foreach ($roleInstance in $deployment.RoleInstanceList)
    {
        $instanceName = $roleInstance.InstanceName
        $instanceStatus = $roleInstance.InstanceStatus

        if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
        {
            $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
            Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
        }

        $i = $i + 1
    }

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $opstat = $deployment.Status

    write-progress -id 4 -activity "Starting Instances" -completed -status $opstat
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: $opstat"
}

function AllInstancesRunning($roleInstanceList)
{
    foreach ($roleInstance in $roleInstanceList)
    {
        if ($roleInstance.InstanceStatus -ne "ReadyRole")
        {
            return $false
        }
    }

    return $true
}

#configure powershell with Azure 1.7 modules
Import-Module Azure

#configure powershell with publishsettings for your subscription
$pubsettings = $subscriptionDataFile
Import-AzurePublishSettingsFile $pubsettings
Set-AzureSubscription -CurrentStorageAccountName $storageAccountName -SubscriptionName $selectedsubscription
Select-AzureSubscription $selectedsubscription

#set remaining environment variables for Azure cmdlets
$subscription = Get-AzureSubscription $selectedsubscription
$subscriptionname = $subscription.subscriptionname
$subscriptionid = $subscription.subscriptionid
$slot = $environment

#main driver - publish & write progress to activity log
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script started."
Write-Output "$(Get-Date -f $timeStampFormat) - Preparing deployment of $deploymentLabel for $subscriptionname with Subscription ID $subscriptionid."

Publish

$deployment = Get-AzureDeployment -slot $slot -serviceName $servicename
$deploymentUrl = $deployment.Url

Write-Output "$(Get-Date -f $timeStampFormat) - Created Cloud Service with URL $deploymentUrl."
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script finished."
```

## <a name="next-steps"></a><span data-ttu-id="06956-302">Další kroky</span><span class="sxs-lookup"><span data-stu-id="06956-302">Next steps</span></span>
<span data-ttu-id="06956-303">Chcete-li povolit vzdálené ladění při použití nastavené průběžné doručování, přečtěte si téma [povolení vzdáleného ladění při publikování v Azure pomocí nastavené průběžné doručování](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md).</span><span class="sxs-lookup"><span data-stu-id="06956-303">To enable remote debugging when using continuous delivery, see [Enable remote debugging when using continuous delivery to publish to Azure](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md).</span></span>

[Team Foundation Build Service]: https://msdn.microsoft.com/library/ee259687.aspx
[.NET Framework 4]: https://www.microsoft.com/download/details.aspx?id=17851
[.NET Framework 4.5]: https://www.microsoft.com/download/details.aspx?id=30653
[.NET Framework 4.5.2]: https://www.microsoft.com/download/details.aspx?id=42643
[Scale out your build system]: https://msdn.microsoft.com/library/dd793166.aspx
[Deploy and configure a build server]: https://msdn.microsoft.com/library/ms181712.aspx
[Azure PowerShell cmdlets]: /powershell/azureps-cmdlets-docs
[the .publishsettings file]: https://manage.windowsazure.com/download/publishprofile.aspx?wa=wsignin1.0
[0]: ./media/cloud-services-dotnet-continuous-delivery/tfs-01bc.png
[2]: ./media/cloud-services-dotnet-continuous-delivery/tfs-02.png
[3]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-03.png
[4]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-04.png
[5]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-05.png
[6]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-06.png
