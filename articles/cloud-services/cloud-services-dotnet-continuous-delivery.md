---
title: "aaaContinuous doručení pro cloudové služby se sadou TFS v Azure | Microsoft Docs"
description: "Zjistěte, jak tooset až nastavené průběžné doručování pro Azure cloudových aplikací. Ukázky kódu pro MSBuild příkazy příkazového řádku a skripty prostředí PowerShell."
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
ms.openlocfilehash: c0e5e72ffbd3c05b84ce1733068e92c528bcc4b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-delivery-for-cloud-services-in-azure"></a><span data-ttu-id="8c072-104">Nastavené průběžné doručování pro cloudové služby v Azure</span><span class="sxs-lookup"><span data-stu-id="8c072-104">Continuous Delivery for Cloud Services in Azure</span></span>
<span data-ttu-id="8c072-105">Hello proces popsaný v tomto článku se dozvíte, jak tooset až nastavené průběžné doručování Azure cloudových aplikací.</span><span class="sxs-lookup"><span data-stu-id="8c072-105">hello process described in this article shows you how tooset up continuous delivery for Azure cloud apps.</span></span> <span data-ttu-id="8c072-106">Tento proces umožňuje automatické vytvoření balíčků a nasadit balíček tooAzure hello po každé změnami kódu.</span><span class="sxs-lookup"><span data-stu-id="8c072-106">This process enables you to automatically create packages and deploy hello package tooAzure after every code check-in.</span></span> <span data-ttu-id="8c072-107">Hello procesu sestavení balíčku popsané v tomto článku je ekvivalentní toohello **balíček** příkaz v sadě Visual Studio a publikování kroky jsou ekvivalentní toohello **publikovat** příkazu v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8c072-107">hello package build process described in this article is equivalent toohello **Package** command in Visual Studio, and the publishing steps are equivalent toohello **Publish** command in Visual Studio.</span></span>
<span data-ttu-id="8c072-108">Hello článek obsahuje hello metody toocreate sestavení serveru by používat s příkazy příkazového řádku nástroje MSBuild a skriptů prostředí Windows PowerShell a také ukazuje, jak nakonfigurovat toooptionally Visual Studio Team Foundation Server – definice sestavení Team toouse hello MSBuild příkazy a skripty prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8c072-108">hello article covers hello methods you would use toocreate a build server with MSBuild command-line statements and Windows PowerShell scripts, and it also demonstrates how toooptionally configure Visual Studio Team Foundation Server - Team Build definitions toouse hello MSBuild commands and PowerShell scripts.</span></span> <span data-ttu-id="8c072-109">proces Hello je přizpůsobitelné prostředí pro sestavení a prostředí Azure cíl.</span><span class="sxs-lookup"><span data-stu-id="8c072-109">hello process is customizable for your build environment and Azure target environments.</span></span>

<span data-ttu-id="8c072-110">Můžete také použít Visual Studio Team Services, verzi sady TFS, která je hostovaná v Azure, toodo to snadněji.</span><span class="sxs-lookup"><span data-stu-id="8c072-110">You can also use Visual Studio Team Services, a version of TFS that is hosted in Azure, toodo this more easily.</span></span> 

<span data-ttu-id="8c072-111">Než začnete, byste měli publikovat aplikace ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8c072-111">Before you start, you should publish your application from Visual Studio.</span></span>
<span data-ttu-id="8c072-112">To zajistí, že jsou všechny prostředky hello dostupné a inicializovaného když pokusí tooautomate hello publikace procesu.</span><span class="sxs-lookup"><span data-stu-id="8c072-112">This will ensure that all hello resources are available and initialized when you attempt tooautomate hello publication process.</span></span>

## <a name="1-configure-hello-build-server"></a><span data-ttu-id="8c072-113">1: Konfigurace hello serveru sestavení</span><span class="sxs-lookup"><span data-stu-id="8c072-113">1: Configure hello Build Server</span></span>
<span data-ttu-id="8c072-114">Než vytvoříte balíček Azure pomocí nástroje MSBuild je na serveru sestavení hello nainstalovat hello požadované softwaru a nástroje.</span><span class="sxs-lookup"><span data-stu-id="8c072-114">Before you can create an Azure package by using MSBuild, you must install hello required software and tools on hello build server.</span></span>

<span data-ttu-id="8c072-115">Visual Studio není požadovaná toobe nainstalován na serveru sestavení hello.</span><span class="sxs-lookup"><span data-stu-id="8c072-115">Visual Studio is not required toobe installed on hello build server.</span></span> <span data-ttu-id="8c072-116">Pokud chcete službu sestavení Team Foundation toomanage toouse sestavení serveru, postupujte podle pokynů hello v hello [služby sestavení Team Foundation] [ Team Foundation Build Service] dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="8c072-116">If you want toouse Team Foundation Build Service toomanage your build server, follow hello instructions in hello [Team Foundation Build Service][Team Foundation Build Service] documentation.</span></span>

1. <span data-ttu-id="8c072-117">Na serveru hello sestavení, nainstalujte hello [rozhraní .NET Framework 4.5.2][.NET Framework 4.5.2], což zahrnuje MSBuild.</span><span class="sxs-lookup"><span data-stu-id="8c072-117">On hello build server, install hello [.NET Framework 4.5.2][.NET Framework 4.5.2], which includes MSBuild.</span></span>
2. <span data-ttu-id="8c072-118">Nainstalujte nejnovější hello [nástroje pro tvorbu Azure pro .NET](https://azure.microsoft.com/develop/net/).</span><span class="sxs-lookup"><span data-stu-id="8c072-118">Install hello latest [Azure Authoring Tools for .NET](https://azure.microsoft.com/develop/net/).</span></span>
3. <span data-ttu-id="8c072-119">Nainstalujte hello [knihovny Azure pro .NET](http://go.microsoft.com/fwlink/?LinkId=623519).</span><span class="sxs-lookup"><span data-stu-id="8c072-119">Install hello [Azure Libraries for .NET](http://go.microsoft.com/fwlink/?LinkId=623519).</span></span>
4. <span data-ttu-id="8c072-120">Kopírovat soubor Microsoft.WebApplication.targets hello z toohello instalace sady Visual Studio vytvořit server.</span><span class="sxs-lookup"><span data-stu-id="8c072-120">Copy hello Microsoft.WebApplication.targets file from a Visual Studio installation toohello build server.</span></span>

   <span data-ttu-id="8c072-121">Na počítači s nainstalovanou sadu Visual Studio, tento soubor je umístěný v adresáři hello C:\\Program Files(x86)\\MSBuild\\Microsoft\\Visual Studio\\v14.0\\WebApplications.</span><span class="sxs-lookup"><span data-stu-id="8c072-121">On a computer with Visual Studio installed, this file is located in hello directory C:\\Program Files(x86)\\MSBuild\\Microsoft\\VisualStudio\\v14.0\\WebApplications.</span></span> <span data-ttu-id="8c072-122">Měli byste ho zkopírovat toohello stejný adresář na serveru sestavení hello.</span><span class="sxs-lookup"><span data-stu-id="8c072-122">You should copy it toohello same directory on hello build server.</span></span>
5. <span data-ttu-id="8c072-123">Nainstalujte hello [nástroje Azure pro sadu Visual Studio](https://www.visualstudio.com/features/azure-tools-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="8c072-123">Install hello [Azure Tools for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs.aspx).</span></span>

## <a name="2-build-a-package-using-msbuild-commands"></a><span data-ttu-id="8c072-124">2: vytvoření balíčku pomocí příkazů nástroje MSBuild</span><span class="sxs-lookup"><span data-stu-id="8c072-124">2: Build a Package using MSBuild Commands</span></span>
<span data-ttu-id="8c072-125">Tato část popisuje, jak tooconstruct MSBuild příkaz, který vytvoří balíček Azure.</span><span class="sxs-lookup"><span data-stu-id="8c072-125">This section describes how tooconstruct an MSBuild command that builds an Azure package.</span></span> <span data-ttu-id="8c072-126">Tento krok lze spusťte na hello sestavení serveru tooverify že všechno správně nakonfigurován a že hello MSBuild příkaz provést požadované ho toodo.</span><span class="sxs-lookup"><span data-stu-id="8c072-126">Run this step on hello build server tooverify that everything is configured correctly and that hello MSBuild command does what you want it toodo.</span></span> <span data-ttu-id="8c072-127">Jak je popsáno v další části hello, můžete přidat buď tento příkazový řádek tooexisting skripty sestavení na hello sestavení serveru nebo můžete použít v definici sestavení TFS hello příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="8c072-127">You can either add this command line tooexisting build scripts on hello build server, or you can use hello command line in a TFS Build Definition, as described in hello next section.</span></span> <span data-ttu-id="8c072-128">Další informace o parametry příkazového řádku a nástroje MSBuild najdete v tématu [příkazového řádku MSBuild – Reference](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="8c072-128">For more information about command-line parameters and MSBuild, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).</span></span>

1. <span data-ttu-id="8c072-129">Pokud na serveru sestavení hello je nainstalovaná sada Visual Studio, vyhledejte a vyberte **Visual Studio – příkazový řádek** v hello **nástroje sady Visual Studio** složky v systému Windows.</span><span class="sxs-lookup"><span data-stu-id="8c072-129">If Visual Studio is installed on hello build server, locate and choose **Visual Studio Command Prompt** in hello **Visual Studio Tools** folder in Windows.</span></span>

   <span data-ttu-id="8c072-130">Pokud Visual Studio není nainstalována na serveru hello sestavení, otevřete příkazový řádek a ujistěte se, že MSBuild.exe je dostupný v cestě.</span><span class="sxs-lookup"><span data-stu-id="8c072-130">If Visual Studio is not installed on hello build server, open a command prompt and make sure that MSBuild.exe is accessible on the path.</span></span> <span data-ttu-id="8c072-131">MSBuild se instaluje s hello rozhraní .NET Framework v hello cesta % WINDIR %\\Microsoft.NET\\Framework\\*verze*.</span><span class="sxs-lookup"><span data-stu-id="8c072-131">MSBuild is installed with hello .NET Framework in hello path %WINDIR%\\Microsoft.NET\\Framework\\*Version*.</span></span> <span data-ttu-id="8c072-132">Například proměnné prostředí PATH toohello MSBuild.exe přidat v případě, že máte nainstalované rozhraní .NET Framework 4, zadejte následující příkaz na příkazovém řádku hello hello:</span><span class="sxs-lookup"><span data-stu-id="8c072-132">For example, to add MSBuild.exe toohello PATH environment variable when you have .NET Framework 4 installed, type hello following command at hello command prompt:</span></span>

       set PATH=%PATH%;"C:\Windows\Microsoft.NET\Framework\v4.0.30319"
2. <span data-ttu-id="8c072-133">Na příkazovém řádku hello přejděte toohello složce obsahující soubor projektu Azure, které chcete toobuild.</span><span class="sxs-lookup"><span data-stu-id="8c072-133">At hello command prompt, navigate toohello folder containing the Azure project file that you want toobuild.</span></span>
3. <span data-ttu-id="8c072-134">Spusťte nástroje MSBuild s hello/target: možnost jako hello následující ukázka publikování:</span><span class="sxs-lookup"><span data-stu-id="8c072-134">Run MSBuild with hello /target:Publish option as in hello following example:</span></span>

       MSBuild /target:Publish

   <span data-ttu-id="8c072-135">Tuto možnost lze zkrátit na /t: publikování.</span><span class="sxs-lookup"><span data-stu-id="8c072-135">This option can be abbreviated as /t:Publish.</span></span> <span data-ttu-id="8c072-136">možnost /t:Publish Hello v nástroji MSBuild Nezaměňovat s příkazy publikovat hello k dispozici v sadě Visual Studio až budete mít hello instalace Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="8c072-136">hello /t:Publish option in MSBuild should not be confused with hello Publish commands available in Visual Studio when you have hello Azure SDK installed.</span></span> <span data-ttu-id="8c072-137">Hello /t: možnost publikování jen sestavení hello Azure balíčky.</span><span class="sxs-lookup"><span data-stu-id="8c072-137">hello /t:Publish option only builds hello Azure packages.</span></span> <span data-ttu-id="8c072-138">Stejně jako hello publikovat příkazy v sadě Visual Studio Nenasazuje hello balíčky.</span><span class="sxs-lookup"><span data-stu-id="8c072-138">It does not deploy hello packages as hello Publish commands in Visual Studio do.</span></span>

   <span data-ttu-id="8c072-139">Volitelně můžete zadat název projektu hello jako parametr MSBuild.</span><span class="sxs-lookup"><span data-stu-id="8c072-139">Optionally, you can specify hello project name as an MSBuild parameter.</span></span> <span data-ttu-id="8c072-140">Pokud není zadaný, použije se hello aktuální adresář.</span><span class="sxs-lookup"><span data-stu-id="8c072-140">If not specified, hello current directory is used.</span></span> <span data-ttu-id="8c072-141">Další informace o možnostech příkazového řádku nástroje MSBuild najdete v tématu [příkazového řádku MSBuild – Reference](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="8c072-141">For more information about MSBuild command line options, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).</span></span>
4. <span data-ttu-id="8c072-142">Vyhledejte výstup hello.</span><span class="sxs-lookup"><span data-stu-id="8c072-142">Locate hello output.</span></span> <span data-ttu-id="8c072-143">Ve výchozím nastavení, tento příkaz vytvoří adresář vztah toohello kořenové složky pro projekt hello, jako například *ProjectDir*\\bin\\*konfigurace* \\ app.publish\\.</span><span class="sxs-lookup"><span data-stu-id="8c072-143">By default, this command creates a directory in relation toohello root folder for hello project, such as *ProjectDir*\\bin\\*Configuration*\\app.publish\\.</span></span> <span data-ttu-id="8c072-144">Při sestavování projektu Azure generovat ze dvou souborů, samotný soubor balíčku hello a hello doplňujícími konfigurační soubor:</span><span class="sxs-lookup"><span data-stu-id="8c072-144">When you build an Azure project, you generate two files, hello package file itself and hello accompanying configuration file:</span></span>

   * <span data-ttu-id="8c072-145">Project.cspkg</span><span class="sxs-lookup"><span data-stu-id="8c072-145">Project.cspkg</span></span>
   * <span data-ttu-id="8c072-146">Objekt ServiceConfiguration. *TargetProfile*.cscfg</span><span class="sxs-lookup"><span data-stu-id="8c072-146">ServiceConfiguration.*TargetProfile*.cscfg</span></span>

   <span data-ttu-id="8c072-147">Ve výchozím nastavení každý projekt, Azure zahrnuje jednu konfigurační soubor služby (soubor .cscfg) pro místní sestavení (ladění) a druhou pro sestavení cloudu (pracovním nebo produkčním), ale můžete přidat nebo odebrat soubory konfigurace služby podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="8c072-147">By default, each Azure project includes one service configuration file (.cscfg file) for local (debugging) builds and another for cloud (staging or production) builds, but you can add or remove service configuration files as needed.</span></span> <span data-ttu-id="8c072-148">Když vytvoříte balíček Visual Studia, zobrazí se výzva které služby konfigurační soubor tooinclude spolu s hello balíčku.</span><span class="sxs-lookup"><span data-stu-id="8c072-148">When you build a package within Visual Studio, you will be asked which service configuration file tooinclude alongside hello package.</span></span>
5. <span data-ttu-id="8c072-149">Zadejte konfigurační soubor služby hello.</span><span class="sxs-lookup"><span data-stu-id="8c072-149">Specify hello service configuration file.</span></span> <span data-ttu-id="8c072-150">Když vytvoříte balíček pomocí nástroje MSBuild, hello místní služby konfigurační soubor je zahrnuté ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="8c072-150">When you build a package by using MSBuild, hello local service configuration file is included by default.</span></span> <span data-ttu-id="8c072-151">tooinclude konfigurační soubor jinou službu, nastavte vlastnost TargetProfile hello MSBuild příkazu, stejně jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="8c072-151">tooinclude a different service configuration file, set the TargetProfile property of hello MSBuild command, as in hello following example:</span></span>

       MSBuild /t:Publish /p:TargetProfile=Cloud
6. <span data-ttu-id="8c072-152">Zadejte umístění hello pro výstup hello.</span><span class="sxs-lookup"><span data-stu-id="8c072-152">Specify hello location for hello output.</span></span> <span data-ttu-id="8c072-153">Cesta hello sady pomocí /p:PublishDir =*Directory* \\ možnost, včetně hello koncové oddělovače zpětné lomítko, stejně jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="8c072-153">Set hello path by using the /p:PublishDir=*Directory*\\ option, including hello trailing backslash separator, as in hello following example:</span></span>

       MSBuild /target:Publish /p:PublishDir=\\myserver\drops\

   <span data-ttu-id="8c072-154">Jakmile jste sestavený a otestovat příslušné nástroje MSBuild příkazového řádku toobuild vašich projektů a zkombinovat do balíčku s Azure, můžete přidat skripty sestavení tooyour Tento příkazový řádek.</span><span class="sxs-lookup"><span data-stu-id="8c072-154">Once you've constructed and tested an appropriate MSBuild command line toobuild your projects and combine them into an Azure package, you can add this command line tooyour build scripts.</span></span> <span data-ttu-id="8c072-155">Pokud váš server sestavení používá vlastní skripty, tento proces bude záviset na jaké jsou specifikace svůj vlastní sestavovací proces.</span><span class="sxs-lookup"><span data-stu-id="8c072-155">If your build server uses custom scripts, this process will depend on the specifics of your custom build process.</span></span> <span data-ttu-id="8c072-156">Pokud používáte jako prostředí pro sestavování sady TFS, můžete provést hello pokyny v hello další krok tooadd hello balíček Azure sestavení tooyour procesu sestavení.</span><span class="sxs-lookup"><span data-stu-id="8c072-156">If you are using TFS as a build environment, then you can follow hello instructions in hello next step tooadd hello Azure package build tooyour build process.</span></span>

## <a name="3-build-a-package-using-tfs-team-build"></a><span data-ttu-id="8c072-157">3: vytvoření balíčku pomocí TFS Team Build</span><span class="sxs-lookup"><span data-stu-id="8c072-157">3: Build a Package using TFS Team Build</span></span>
<span data-ttu-id="8c072-158">Máte-li nastavit jako řadič sestavení a hello sestavení serveru Team Foundation Server (TFS) nastavit jako počítač sestavení sady TFS a potom automatizované sestavení Volitelně můžete nastavit pro balíčku Azure.</span><span class="sxs-lookup"><span data-stu-id="8c072-158">If you have Team Foundation Server (TFS) set up as a build controller and hello build server set up as a TFS build machine, then you can optionally set up an automated build for your Azure package.</span></span> <span data-ttu-id="8c072-159">Informace o tom, jak tooset nahoru a použít Team Foundation server jako systém sestavení, najdete v části [škálování systém sestavení][Scale out your build system].</span><span class="sxs-lookup"><span data-stu-id="8c072-159">For information on how tooset up and use Team Foundation server as a build system, see [Scale out your build system][Scale out your build system].</span></span> <span data-ttu-id="8c072-160">Zejména následující postup předpokládá, že jste nakonfigurovali vašem serveru sestavení, jak je popsáno v [nasadit a nakonfigurovat server sestavení][Deploy and configure a build server], a že jste vytvořili týmového projektu, vytvoření cloudu projekt služby v hello týmového projektu.</span><span class="sxs-lookup"><span data-stu-id="8c072-160">In particular, the following procedure assumes that you have configured your build server as described in [Deploy and configure a build server][Deploy and configure a build server], and that you have created a team project, created a cloud service project in hello team project.</span></span>

<span data-ttu-id="8c072-161">tooconfigure TFS toobuild Azure balíčky, proveďte následující kroky hello:</span><span class="sxs-lookup"><span data-stu-id="8c072-161">tooconfigure TFS toobuild Azure packages, perform hello following steps:</span></span>

1. <span data-ttu-id="8c072-162">V sadě Visual Studio ve svém vývojovém počítači, v nabídce zobrazení hello zvolte **Team Explorer**, nebo zvolte Ctrl +\\, Ctrl + M.</span><span class="sxs-lookup"><span data-stu-id="8c072-162">In Visual Studio on your development computer, on hello View menu, choose **Team Explorer**, or choose Ctrl+\\, Ctrl+M.</span></span> <span data-ttu-id="8c072-163">V okně Team Explorer rozbalte hello **sestavení** uzlu, nebo zvolte hello **sestavení** stránce a vyberte **novou definici sestavení**.</span><span class="sxs-lookup"><span data-stu-id="8c072-163">In the Team Explorer window, expand hello **Builds** node or choose hello **Builds** page, and choose **New Build Definition**.</span></span>

   ![Nová možnost definice sestavení][0]
2. <span data-ttu-id="8c072-165">Zvolte hello **aktivační událost** a zadejte hello požadovaných podmínek, když chcete hello toobe balíček vytvořen.</span><span class="sxs-lookup"><span data-stu-id="8c072-165">Choose hello **Trigger** tab, and specify hello desired conditions for when you want hello package toobe built.</span></span> <span data-ttu-id="8c072-166">Můžete například zadat **průběžnou integraci** dojde k toobuild hello balíček vždy, když zdroj řídí změnami.</span><span class="sxs-lookup"><span data-stu-id="8c072-166">For example, specify **Continuous Integration** toobuild hello package whenever a source control check-in occurs.</span></span>
3. <span data-ttu-id="8c072-167">Zvolte hello **nastavení zdroje** kartě a zajistěte, aby složky projektu je uvedena v hello **zdrojové složky řízení** sloupce, a stav hello **Active**.</span><span class="sxs-lookup"><span data-stu-id="8c072-167">Choose hello **Source Settings** tab, and make sure your project folder is listed in hello **Source Control Folder** column, and hello status is **Active**.</span></span>
4. <span data-ttu-id="8c072-168">Zvolte hello **sestavení výchozí** a v kontroleru buildu, ověřte název hello hello sestavení serveru.</span><span class="sxs-lookup"><span data-stu-id="8c072-168">Choose hello **Build Defaults** tab, and under Build controller, verify hello name of hello build server.</span></span>  <span data-ttu-id="8c072-169">Také vyberte možnost hello **kopie sestavení výstupu toohello následující vyřadit složky** a zadejte umístění rozevírací hello potřeby.</span><span class="sxs-lookup"><span data-stu-id="8c072-169">Also, choose hello option **Copy build output toohello following drop folder** and specify hello desired drop location.</span></span>
5. <span data-ttu-id="8c072-170">Zvolte hello **proces** kartě. Na kartě hello procesu, zvolte hello výchozí šablony, v části **sestavení**, zvolte projekt hello, pokud není vybrána a rozbalte hello **Upřesnit** část v hello **sestavení**části hello mřížky.</span><span class="sxs-lookup"><span data-stu-id="8c072-170">Choose hello **Process** tab. On hello Process tab, choose hello default template, under **Build**, choose hello project if it is not already selected, and expand hello **Advanced** section in hello **Build** section of hello grid.</span></span>
6. <span data-ttu-id="8c072-171">Zvolte **argumenty MSBuild**a nastavte hello příslušné nástroje MSBuild argumenty příkazového řádku, jak je popsáno v kroku 2 výše.</span><span class="sxs-lookup"><span data-stu-id="8c072-171">Choose **MSBuild Arguments**, and set hello appropriate MSBuild command line arguments as described in Step 2 above.</span></span> <span data-ttu-id="8c072-172">Zadejte například **/t: publikování /p:PublishDir =\\\\myserver\\zahodí\\**  toobuild hello balíček balíčku a zkopírujte soubory toohello umístění \\ \\myserver\\zahodí\\:</span><span class="sxs-lookup"><span data-stu-id="8c072-172">For example, enter **/t:Publish /p:PublishDir=\\\\myserver\\drops\\** toobuild a package and copy hello package files toohello location \\\\myserver\\drops\\:</span></span>

   ![Argumenty MSBuild][2]

   > [!NOTE]
   > <span data-ttu-id="8c072-174">Kopírování souborů tooa hello veřejné složky umožňuje snazší toomanually nasadit balíčky hello z vývojovém počítači.</span><span class="sxs-lookup"><span data-stu-id="8c072-174">Copying hello files tooa public share makes it easier toomanually deploy hello packages from your development computer.</span></span>
7. <span data-ttu-id="8c072-175">Testování úspěšnosti hello kroku sestavení kontrolou v projektu tooyour změnu nebo fronty, nové sestavení.</span><span class="sxs-lookup"><span data-stu-id="8c072-175">Test hello success of your build step by checking in a change tooyour project, or queue up a new build.</span></span> <span data-ttu-id="8c072-176">Klikněte pravým tlačítkem na tooqueue až nového sestavení, v nástroji Team Explorer **všechny definice sestavení,** a potom zvolte **fronty nové sestavení**.</span><span class="sxs-lookup"><span data-stu-id="8c072-176">tooqueue up a new build, in the Team Explorer, right-click **All Build Definitions,** and then choose **Queue New Build**.</span></span>

## <a name="4-publish-a-package-using-a-powershell-script"></a><span data-ttu-id="8c072-177">4: publikování balíčku pomocí skriptu prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="8c072-177">4: Publish a Package using a PowerShell Script</span></span>
<span data-ttu-id="8c072-178">Tato část popisuje, jak tooconstruct skript prostředí Windows PowerShell, která bude publikovat balíček aplikace hello cloudu výstup tooAzure pomocí volitelné parametry.</span><span class="sxs-lookup"><span data-stu-id="8c072-178">This section describes how tooconstruct a Windows PowerShell script that will publish hello Cloud app package output tooAzure using optional parameters.</span></span> <span data-ttu-id="8c072-179">Tento skript je možné volat po kroku hello sestavení ve vaší vlastní sestavovací automation.</span><span class="sxs-lookup"><span data-stu-id="8c072-179">This script can be called after hello build step in your custom build automation.</span></span> <span data-ttu-id="8c072-180">Také může být volána z aktivit pracovního postupu šablonu procesu v sestavení Team TFS s Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8c072-180">It can also be called from Process Template workflow activities in Visual Studio TFS Team Build.</span></span>

1. <span data-ttu-id="8c072-181">Nainstalujte hello [rutin prostředí Azure PowerShell] [ Azure PowerShell cmdlets] (v0.6.1 nebo vyšší).</span><span class="sxs-lookup"><span data-stu-id="8c072-181">Install hello [Azure PowerShell cmdlets][Azure PowerShell cmdlets] (v0.6.1 or higher).</span></span>
   <span data-ttu-id="8c072-182">Během fáze instalace hello rutiny vyberte tooinstall jako modul snap-in.</span><span class="sxs-lookup"><span data-stu-id="8c072-182">During hello cmdlet setup phase, choose tooinstall as a snap-in.</span></span> <span data-ttu-id="8c072-183">Všimněte si, že tato oficiálně podporované verze nahrazuje starší verze hello nabízeným přes webu CodePlex, i když bylo předchozí verze hello číslované 2.x.x.</span><span class="sxs-lookup"><span data-stu-id="8c072-183">Note that this officially supported version replaces hello older version offered through CodePlex, although hello previous versions were numbered 2.x.x.</span></span>
2. <span data-ttu-id="8c072-184">Otevřete prostředí Azure PowerShell pomocí hello nabídky Start nebo úvodní stránky.</span><span class="sxs-lookup"><span data-stu-id="8c072-184">Start Azure PowerShell using hello Start menu or Start page.</span></span> <span data-ttu-id="8c072-185">Pokud spustíte tímto způsobem, budou načteny hello rutin prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8c072-185">If you start in this way, hello Azure PowerShell cmdlets will be loaded.</span></span>
3. <span data-ttu-id="8c072-186">Na příkazovém řádku prostředí PowerShell text hello, ověřte, zda jsou načteny hello rutiny prostředí PowerShell zadáním příkazu částečné hello `Get-Azure` a stisknutím klávesy tabulátor pro dokončování hello.</span><span class="sxs-lookup"><span data-stu-id="8c072-186">At hello PowerShell prompt, verify that hello PowerShell cmdlets are loaded by entering hello partial command `Get-Azure` and then pressing hello Tab key for statement completion.</span></span>

   <span data-ttu-id="8c072-187">Když stisknete klávesy Tab hello opakovaně, měli byste vidět různé příkazy prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8c072-187">If you press hello Tab key repeatedly, you should see various Azure PowerShell commands.</span></span>
4. <span data-ttu-id="8c072-188">Ověřte, že můžete importovat informace o vašem předplatném ze souboru .publishsettings hello připojit tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="8c072-188">Verify that you can connect tooyour Azure subscription by importing your subscription information from hello .publishsettings file.</span></span>

   `Import-AzurePublishSettingsFile c:\scripts\WindowsAzure\default.publishsettings`

   <span data-ttu-id="8c072-189">Potom zadejte příkaz hello</span><span class="sxs-lookup"><span data-stu-id="8c072-189">Then enter hello command</span></span>

   `Get-AzureSubscription`

   <span data-ttu-id="8c072-190">Ukazuje to informace o vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="8c072-190">This shows information about your subscription.</span></span> <span data-ttu-id="8c072-191">Ověřte, že je vše v pořádku.</span><span class="sxs-lookup"><span data-stu-id="8c072-191">Verify that everything is correct.</span></span>
5. <span data-ttu-id="8c072-192">Uložení šablony skriptu hello uvedených v hello konci tohoto článku do složky skriptů jako c:\\skripty\\WindowsAzure\\**PublishCloudService.ps1**.</span><span class="sxs-lookup"><span data-stu-id="8c072-192">Save hello script template provided at hello end of this article to your scripts folder as c:\\scripts\\WindowsAzure\\**PublishCloudService.ps1**.</span></span>
6. <span data-ttu-id="8c072-193">Projděte si část parametry hello hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="8c072-193">Review hello parameters section of hello script.</span></span> <span data-ttu-id="8c072-194">Přidat nebo upravit všechny výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="8c072-194">Add or modify any default values.</span></span> <span data-ttu-id="8c072-195">Tyto hodnoty mohou být přepsány vždy explicitní parametrů.</span><span class="sxs-lookup"><span data-stu-id="8c072-195">These values can always be overridden by passing in explicit parameters.</span></span>
7. <span data-ttu-id="8c072-196">Ujistěte se, nejsou platná cloudové služby a účty úložiště vytvořené v rámci vašeho předplatného, který může být cílem hello publikování skriptu.</span><span class="sxs-lookup"><span data-stu-id="8c072-196">Ensure there are valid cloud service and storage accounts created in your subscription that can be targeted by hello publish script.</span></span> <span data-ttu-id="8c072-197">Účet úložiště (úložiště objektů blob) bude použité tooupload a dočasně uložit hello nasazení balíčku a konfigurační soubor je při vytváření nasazení.</span><span class="sxs-lookup"><span data-stu-id="8c072-197">The storage account (blob storage) will be used tooupload and temporarily store hello deployment package and config file while the deployment is being created.</span></span>

   * <span data-ttu-id="8c072-198">toocreate novou cloudovou službu, můžete volat tento skript nebo použijte hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8c072-198">toocreate a new cloud service, you can call this script or use hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="8c072-199">Název cloudové služby Hello se použije jako předponu v platný plně kvalifikovaný název domény, a proto musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="8c072-199">hello cloud service name will be used as a prefix in a fully qualified domain name and hence it must be unique.</span></span>

         New-AzureService -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
   * <span data-ttu-id="8c072-200">toocreate nový účet úložiště, můžete volat tento skript nebo použijte hello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8c072-200">toocreate a new storage account, you can call this script or use hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="8c072-201">název účtu úložiště Hello se použije jako předponu v platný plně kvalifikovaný název domény, a proto musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="8c072-201">hello storage account name will be used as a prefix in a fully qualified domain name and hence it must be unique.</span></span> <span data-ttu-id="8c072-202">Můžete se pokusit hello stejný název jako cloudová služba.</span><span class="sxs-lookup"><span data-stu-id="8c072-202">You can try using hello same name as the cloud service.</span></span>

         New-AzureStorageAccount -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
8. <span data-ttu-id="8c072-203">Volání skriptu hello přímo z prostředí Azure PowerShell nebo propojit se tento skript tooyour hostitele sestavení automatizace toooccur po sestavení balíčku hello.</span><span class="sxs-lookup"><span data-stu-id="8c072-203">Call hello script directly from Azure PowerShell, or wire up this script tooyour host build automation toooccur after hello package build.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="8c072-204">Hello skript bude vždy odstranit nebo nahradit existující nasazení ve výchozím nastavení, pokud jsou zjišťovány.</span><span class="sxs-lookup"><span data-stu-id="8c072-204">hello script will always delete or replace your existing deployments by default if they are detected.</span></span> <span data-ttu-id="8c072-205">Toto je nutná pro povolení průběžné doručování z automatizace, kde je možné použít bez vyzvání uživatele.</span><span class="sxs-lookup"><span data-stu-id="8c072-205">This is necessary to enable continuous delivery from automation where no user prompting is possible.</span></span>
   >
   >

   <span data-ttu-id="8c072-206">**Příklad scénáře 1:** toohello průběžné nasazování pracovní prostředí služby:</span><span class="sxs-lookup"><span data-stu-id="8c072-206">**Example scenario 1:** continuous deployment toohello staging environment of a service:</span></span>

       PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Staging -serviceName mycloudservice -storageAccountName mystoragesaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

   <span data-ttu-id="8c072-207">To je obvykle následuje test spusťte ověření a prohození virtuálních IP adres.</span><span class="sxs-lookup"><span data-stu-id="8c072-207">This is typically followed up by test run verification and a VIP swap.</span></span> <span data-ttu-id="8c072-208">swap Hello VIP, můžete to udělat pomocí hello [portál Azure](https://portal.azure.com) nebo pomocí rutiny hello přesunutí nasazení.</span><span class="sxs-lookup"><span data-stu-id="8c072-208">hello VIP swap can be done via hello [Azure portal](https://portal.azure.com) or by using hello Move-Deployment cmdlet.</span></span>

   <span data-ttu-id="8c072-209">**Příklad scénáře 2:** průběžné nasazování toohello provozním prostředí služby vyhrazené testu</span><span class="sxs-lookup"><span data-stu-id="8c072-209">**Example scenario 2:** continuous deployment toohello production environment of a dedicated test service</span></span>

       PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Production -enableDeploymentUpgrade 1 -serviceName mycloudservice -storageAccountName mystorageaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

   <span data-ttu-id="8c072-210">**Vzdálená plocha:**</span><span class="sxs-lookup"><span data-stu-id="8c072-210">**Remote Desktop:**</span></span>

   <span data-ttu-id="8c072-211">Pokud je povoleno vzdálené plochy v projektu Azure budete potřebovat další jednorázové kroky tooperform tooensure hello je správný certifikát cloudové služby nahrán tooall cloudové služby, která je cílem tohoto skriptu.</span><span class="sxs-lookup"><span data-stu-id="8c072-211">If Remote Desktop is enabled in your Azure project you will need tooperform additional one-time steps tooensure hello correct Cloud Service Certificate is uploaded tooall cloud services targeted by this script.</span></span>

   <span data-ttu-id="8c072-212">Vyhledejte hodnoty kryptografického otisku certifikátu hello očekávanou role.</span><span class="sxs-lookup"><span data-stu-id="8c072-212">Locate hello certificate thumbprint values expected by your roles.</span></span> <span data-ttu-id="8c072-213">Kryptografický otisk hodnoty jsou viditelné v části certifikáty hello souboru konfigurace cloudu (tj. ServiceConfiguration.Cloud.cscfg).</span><span class="sxs-lookup"><span data-stu-id="8c072-213">The thumbprint values are visible in hello Certificates section of the cloud config file (i.e. ServiceConfiguration.Cloud.cscfg).</span></span> <span data-ttu-id="8c072-214">Je také v dialogovém okně hello konfigurace vzdálené plochy v sadě Visual Studio viditelné zobrazit možnosti a zobrazení hello výběru certifikátu.</span><span class="sxs-lookup"><span data-stu-id="8c072-214">It is also visible in hello Remote Desktop Configuration dialog in Visual Studio when you Show Options and view hello selected certificate.</span></span>

       <Certificates>
             <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="C33B6C432C25581601B84C80F86EC2809DC224E8" thumbprintAlgorithm="sha1" />
       </Certificates>

   <span data-ttu-id="8c072-215">Nahrání certifikátů vzdálené plochy jako krok jednorázové instalace pomocí následující rutiny skriptu hello:</span><span class="sxs-lookup"><span data-stu-id="8c072-215">Upload Remote Desktop certificates as a one-time setup step using hello following cmdlet script:</span></span>

       Add-AzureCertificate -serviceName <CLOUDSERVICENAME> -certToDeploy (get-item cert:\CurrentUser\MY\<THUMBPRINT>)

   <span data-ttu-id="8c072-216">Například:</span><span class="sxs-lookup"><span data-stu-id="8c072-216">For example:</span></span>

       Add-AzureCertificate -serviceName 'mytestcloudservice' -certToDeploy (get-item cert:\CurrentUser\MY\C33B6C432C25581601B84C80F86EC2809DC224E8

   <span data-ttu-id="8c072-217">Případně můžete exportovat soubor certifikátu PFX hello s privátní klíč a nahrávání certifikátů tooeach cílový cloud service pomocí [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8c072-217">Alternatively you can export hello certificate file PFX with private key and upload certificates tooeach target cloud service using the [Azure portal](https://portal.azure.com).</span></span>

   <!---
   Fixing broken links for Azure content migration from ACOM tooDOCS. I'm unable toofind a replacement links, so I'm commenting out this reference for now. hello author can investigate in hello future. "Read hello following article toolearn more: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx.
   -->
   <span data-ttu-id="8c072-218">**Upgrade a nasazení. Odstranění nasazení -\> nové nasazení**</span><span class="sxs-lookup"><span data-stu-id="8c072-218">**Upgrade Deployment vs. Delete Deployment -\> New Deployment**</span></span>

   <span data-ttu-id="8c072-219">Hello skript bude ve výchozím nastavení provádět nasazení upgradu ($enableDeploymentUpgrade = 1) Pokud žádný parametr se předává v nebo hodnota 1, je předaná explicitně.</span><span class="sxs-lookup"><span data-stu-id="8c072-219">hello script will by default perform an Upgrade Deployment ($enableDeploymentUpgrade = 1) when no parameter is passed in or the value 1 is passed explicitly.</span></span> <span data-ttu-id="8c072-220">Pro jeden instance to nabízí výhodu v podobě trvá kratší dobu, než úplné nasazení.</span><span class="sxs-lookup"><span data-stu-id="8c072-220">For single instances this has the advantage of taking less time than a full deployment.</span></span> <span data-ttu-id="8c072-221">Pro instance, které vyžadují vysokou dostupnost, to také obsahuje výhod hello ponechat některé instancí spuštěných zatímco jiné jsou upgradovány (proti vaší doméně aktualizace), plus vaše virtuální IP adresy se neodstraní.</span><span class="sxs-lookup"><span data-stu-id="8c072-221">For instances that require high availability this also has hello advantage of leaving some instances running while others are upgraded (walking your update domain), plus your VIP will not be deleted.</span></span>

   <span data-ttu-id="8c072-222">Nasazení upgradu můžete zakázat v hello skriptu ($enableDeploymentUpgrade = 0) nebo pomocí předání *- enableDeploymentUpgrade 0* jako parametr, který mění skriptu chování toofirst odstranit všechna stávající nasazení a pak vytvořte nové nasazení.</span><span class="sxs-lookup"><span data-stu-id="8c072-222">Upgrade Deployment can be disabled in hello script ($enableDeploymentUpgrade = 0) or by passing *-enableDeploymentUpgrade 0* as a parameter, which alters the script behavior toofirst delete any existing deployment and then create a new deployment.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="8c072-223">Hello skript bude vždy odstranit nebo nahradit existující nasazení ve výchozím nastavení, pokud jsou zjišťovány.</span><span class="sxs-lookup"><span data-stu-id="8c072-223">hello script will always delete or replace your existing deployments by default if they are detected.</span></span> <span data-ttu-id="8c072-224">Toto je nutná pro povolení nastavené průběžné doručování ze služby automation, kde je možné použít bez vyzvání uživatele / – operátor.</span><span class="sxs-lookup"><span data-stu-id="8c072-224">This is necessary to enable continuous delivery from automation where no user/operator prompting is possible.</span></span>
   >
   >

## <a name="5-publish-a-package-using-tfs-team-build"></a><span data-ttu-id="8c072-225">5: publikování balíčku pomocí TFS Team Build</span><span class="sxs-lookup"><span data-stu-id="8c072-225">5: Publish a Package using TFS Team Build</span></span>
<span data-ttu-id="8c072-226">Tento volitelný krok připojí TFS Team Build toohello skript vytvořený v kroku 4, která zpracovává publikování tooAzure sestavení balíčku hello.</span><span class="sxs-lookup"><span data-stu-id="8c072-226">This optional step connects TFS Team Build toohello script created in step 4, which handles publishing of hello package build tooAzure.</span></span> <span data-ttu-id="8c072-227">To má za následek změny hello používá svou definici sestavení tak, aby běžel aktivitu publikovat na konci hello hello pracovní postup šablony procesu.</span><span class="sxs-lookup"><span data-stu-id="8c072-227">This entails modifying hello Process Template used by your build definition so that it runs a Publish activity at hello end of hello workflow.</span></span> <span data-ttu-id="8c072-228">Hello publikovat aktivita provede předávání v parametrech ze sestavení hello příkazu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8c072-228">hello Publish activity will execute your PowerShell command passing in parameters from hello build.</span></span> <span data-ttu-id="8c072-229">Výstup hello MSBuild cílí a publikovat skript bude přesměrovat do výstupu standardní sestavení hello.</span><span class="sxs-lookup"><span data-stu-id="8c072-229">Output of hello MSBuild targets and publish script will be piped into hello standard build output.</span></span>

1. <span data-ttu-id="8c072-230">Upravit hello sestavení definice zodpovědná za průběžné nasazování.</span><span class="sxs-lookup"><span data-stu-id="8c072-230">Edit hello Build Definition responsible for continuous deploy.</span></span>
2. <span data-ttu-id="8c072-231">Vyberte hello **proces** kartě.</span><span class="sxs-lookup"><span data-stu-id="8c072-231">Select hello **Process** tab.</span></span>
3. <span data-ttu-id="8c072-232">Postupujte podle [tyto pokyny](http://msdn.microsoft.com/library/dd647551.aspx) tooadd projektu aktivity pro hello šablony procesu sestavení, stáhnout výchozí šablonu pro hello, přidejte ho do projektu hello a vrácení se změnami.</span><span class="sxs-lookup"><span data-stu-id="8c072-232">Follow [these instructions](http://msdn.microsoft.com/library/dd647551.aspx) tooadd an Activity project for hello build process template, download hello default template, add it to hello project and check it in.</span></span> <span data-ttu-id="8c072-233">Zadejte nový název, jako je například AzureBuildProcessTemplate šablony procesu sestavení hello.</span><span class="sxs-lookup"><span data-stu-id="8c072-233">Give hello build process template a new name, such as AzureBuildProcessTemplate.</span></span>
4. <span data-ttu-id="8c072-234">Vrátí toohello **proces** kartě a používat **zobrazit podrobnosti** tooshow seznam šablony procesu sestavení k dispozici.</span><span class="sxs-lookup"><span data-stu-id="8c072-234">Return toohello **Process** tab, and use **Show Details** tooshow a list of available build process templates.</span></span> <span data-ttu-id="8c072-235">Zvolte hello **nový...**  tlačítko a přejděte toohello projektu, stačí přidat a vrátit se změnami.</span><span class="sxs-lookup"><span data-stu-id="8c072-235">Choose hello **New...** button, and navigate toohello project you just added and checked in.</span></span> <span data-ttu-id="8c072-236">Vyhledejte hello šablonu, kterou jste právě vytvořili a zvolte **OK**.</span><span class="sxs-lookup"><span data-stu-id="8c072-236">Locate hello template you just created and choose **OK**.</span></span>
5. <span data-ttu-id="8c072-237">Otevřete hello vybrané šablony procesu pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="8c072-237">Open hello selected Process Template for editing.</span></span> <span data-ttu-id="8c072-238">Můžete otevřít přímo v Návrháři pracovních postupů hello nebo v toowork editor XML hello s hello XAML.</span><span class="sxs-lookup"><span data-stu-id="8c072-238">You can open directly in hello Workflow designer or in hello XML editor toowork with hello XAML.</span></span>
6. <span data-ttu-id="8c072-239">Přidejte následující seznam nových argumentů jako samostatné řádku položky v kartě argumenty hello Návrháře pracovního postupu hello hello.</span><span class="sxs-lookup"><span data-stu-id="8c072-239">Add hello following list of new arguments as separate line items in hello arguments tab of hello workflow designer.</span></span> <span data-ttu-id="8c072-240">Všechny argumenty by měl mít směr = v a typ = řetězec.</span><span class="sxs-lookup"><span data-stu-id="8c072-240">All arguments should have direction=In and type=String.</span></span> <span data-ttu-id="8c072-241">Budou tyto parametry použité tooflow z definice sestavení hello do hello pracovního postupu, který pak použít toocall hello get publikování skriptu.</span><span class="sxs-lookup"><span data-stu-id="8c072-241">These will be used tooflow parameters from hello build definition into hello workflow, which then get used toocall hello publish script.</span></span>

       SubscriptionName
       StorageAccountName
       CloudConfigLocation
       PackageLocation
       Environment
       SubscriptionDataFileLocation
       PublishScriptLocation
       ServiceName

   ![Seznam argumentů][3]

   <span data-ttu-id="8c072-243">Hello odpovídající XAML vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="8c072-243">hello corresponding XAML looks like this:</span></span>

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
7. <span data-ttu-id="8c072-244">Přidejte nové pořadí na konci hello spustit na agenta:</span><span class="sxs-lookup"><span data-stu-id="8c072-244">Add a new sequence at hello end of Run On Agent:</span></span>

   1. <span data-ttu-id="8c072-245">Začněte přidáním toocheck Pokud příkaz aktivity pro soubor platný skriptu.</span><span class="sxs-lookup"><span data-stu-id="8c072-245">Start by adding an If Statement activity toocheck for a valid script file.</span></span> <span data-ttu-id="8c072-246">Nastavte hodnotu toothis hello podmínku:</span><span class="sxs-lookup"><span data-stu-id="8c072-246">Set hello condition toothis value:</span></span>

          Not String.IsNullOrEmpty(PublishScriptLocation)
   2. <span data-ttu-id="8c072-247">Hello pak případ hello příkaz If přidejte nová aktivita pořadí.</span><span class="sxs-lookup"><span data-stu-id="8c072-247">In hello Then case of hello If Statement, add a new Sequence activity.</span></span> <span data-ttu-id="8c072-248">Sada hello zobrazovaný název too'Start publikovat.</span><span class="sxs-lookup"><span data-stu-id="8c072-248">Set hello display name too'Start publish'</span></span>
   3. <span data-ttu-id="8c072-249">S hello počáteční publikování pořadí při výběru přidejte jako samostatné položky řádek na kartě proměnné hello Návrháře pracovního postupu v následujícím seznamu nové proměnné.</span><span class="sxs-lookup"><span data-stu-id="8c072-249">With hello Start publish sequence still selected, add the following list of new variables as separate line items in the variables tab of hello workflow designer.</span></span> <span data-ttu-id="8c072-250">Všechny proměnné, měl by být proměnné typ = řetězec a obor = počáteční publikovat.</span><span class="sxs-lookup"><span data-stu-id="8c072-250">All variables should have Variable type =String and Scope=Start publish.</span></span> <span data-ttu-id="8c072-251">Budou tyto parametry použité tooflow z definice sestavení hello do pracovního postupu, který pak použít toocall hello get publikování skriptu.</span><span class="sxs-lookup"><span data-stu-id="8c072-251">These will be used tooflow parameters from hello build definition into the workflow, which then get used toocall hello publish script.</span></span>

      * <span data-ttu-id="8c072-252">SubscriptionDataFilePath typu řetězec.</span><span class="sxs-lookup"><span data-stu-id="8c072-252">SubscriptionDataFilePath, of type String</span></span>
      * <span data-ttu-id="8c072-253">PublishScriptFilePath typu řetězec.</span><span class="sxs-lookup"><span data-stu-id="8c072-253">PublishScriptFilePath, of type String</span></span>

        ![Nové proměnné][4]
   4. <span data-ttu-id="8c072-255">Pokud používáte TFS 2012 nebo starší, přidat aktivitu ConvertWorkspaceItem na začátku hello hello nové pořadí.</span><span class="sxs-lookup"><span data-stu-id="8c072-255">If you are using TFS 2012 or earlier, add a ConvertWorkspaceItem activity at hello beginning of hello new Sequence.</span></span> <span data-ttu-id="8c072-256">Pokud používáte sady TFS 2013 nebo novější, přidejte aktivitu GetLocalPath na začátku hello hello nové pořadí.</span><span class="sxs-lookup"><span data-stu-id="8c072-256">If you are using TFS 2013 or later, add a GetLocalPath activity at hello beginning of hello new sequence.</span></span> <span data-ttu-id="8c072-257">Pro ConvertWorkspaceItem, nastavte vlastnosti hello následujícím způsobem: směr = ServerToLocal, DisplayName = název souboru skriptu publikovat převést, vstup = PublishScriptLocation, výsledek = PublishScriptFilePath, pracovní prostor = 'Prostoru'.</span><span class="sxs-lookup"><span data-stu-id="8c072-257">For a ConvertWorkspaceItem, set hello properties as follows: Direction=ServerToLocal, DisplayName='Convert publish script filename', Input=' PublishScriptLocation', Result='PublishScriptFilePath', Workspace='Workspace'.</span></span> <span data-ttu-id="8c072-258">Pro aktivitu GetLocalPath nastavit hello vlastnost IncomingPath too'PublishScriptLocation', a výsledek too'PublishScriptFilePath hello ".</span><span class="sxs-lookup"><span data-stu-id="8c072-258">For a GetLocalPath activity, set hello property IncomingPath too'PublishScriptLocation', and hello Result too'PublishScriptFilePath'.</span></span> <span data-ttu-id="8c072-259">Tato aktivita převede hello cesta toohello publikování skript z umístění serveru TFS (pokud existuje) tooa disků na úrovni standard místní cesta.</span><span class="sxs-lookup"><span data-stu-id="8c072-259">This activity converts hello path toohello publish script from TFS server locations (if applicable) tooa standard local disk path.</span></span>
   5. <span data-ttu-id="8c072-260">Pokud používáte TFS 2012 nebo starší, přidejte další aktivitu ConvertWorkspaceItem na konci hello hello nové pořadí.</span><span class="sxs-lookup"><span data-stu-id="8c072-260">If you are using TFS 2012 or earlier, add another ConvertWorkspaceItem activity at hello end of hello new Sequence.</span></span> <span data-ttu-id="8c072-261">Směr ServerToLocal, DisplayName = = převést předplatné filename, vstup = SubscriptionDataFileLocation, výsledek = SubscriptionDataFilePath, pracovní prostor = 'Prostoru'.</span><span class="sxs-lookup"><span data-stu-id="8c072-261">Direction=ServerToLocal, DisplayName='Convert subscription filename', Input=' SubscriptionDataFileLocation', Result= 'SubscriptionDataFilePath', Workspace='Workspace'.</span></span> <span data-ttu-id="8c072-262">Pokud používáte sady TFS 2013 nebo novější, přidejte další GetLocalPath.</span><span class="sxs-lookup"><span data-stu-id="8c072-262">If you are using TFS 2013 or later, add another GetLocalPath.</span></span> <span data-ttu-id="8c072-263">IncomingPath = 'SubscriptionDataFileLocation' a výsledek = "SubscriptionDataFilePath."</span><span class="sxs-lookup"><span data-stu-id="8c072-263">IncomingPath='SubscriptionDataFileLocation', and Result='SubscriptionDataFilePath.'</span></span>
   6. <span data-ttu-id="8c072-264">Přidejte aktivitu, InvokeProcess na konci hello hello nové pořadí.</span><span class="sxs-lookup"><span data-stu-id="8c072-264">Add an InvokeProcess activity at hello end of hello new Sequence.</span></span>
      <span data-ttu-id="8c072-265">Toto volání aktivity PowerShell.exe s argumenty hello předaná v hello definice sestavení.</span><span class="sxs-lookup"><span data-stu-id="8c072-265">This activity calls PowerShell.exe with hello arguments passed in by hello Build Definition.</span></span>

      + <span data-ttu-id="8c072-266">Argumenty = String.Format ("-""{0}" "- serviceName {1} - storageAccountName {2} - packageLocation""{3}" "– cloudConfigLocation""{4}" "– subscriptionDataFile""{5}" "- selectedSubscription {6} souboru-prostředí""{7}" "", PublishScriptFilePath, ServiceName, StorageAccountName, PackageLocation, CloudConfigLocation, SubscriptionDataFilePath, Název_předplatného, prostředí)</span><span class="sxs-lookup"><span data-stu-id="8c072-266">Arguments = String.Format(" -File ""{0}"" -serviceName {1}  -storageAccountName {2} -packageLocation ""{3}""  -cloudConfigLocation ""{4}"" -subscriptionDataFile ""{5}""  -selectedSubscription {6} -environment ""{7}""",  PublishScriptFilePath, ServiceName, StorageAccountName,  PackageLocation, CloudConfigLocation,  SubscriptionDataFilePath, SubscriptionName, Environment)</span></span>
      + <span data-ttu-id="8c072-267">DisplayName = Execute publikování skriptu</span><span class="sxs-lookup"><span data-stu-id="8c072-267">DisplayName = Execute publish script</span></span>
      + <span data-ttu-id="8c072-268">Název souboru = "PowerShell" (včetně uvozovek hello)</span><span class="sxs-lookup"><span data-stu-id="8c072-268">FileName = "PowerShell" (include hello quotes)</span></span>
      + <span data-ttu-id="8c072-269">OutputEncoding = System.Text.Encoding.GetEncoding(System.Globalization.CultureInfo.InstalledUICulture.TextInfo.OEMCodePage)</span><span class="sxs-lookup"><span data-stu-id="8c072-269">OutputEncoding=  System.Text.Encoding.GetEncoding(System.Globalization.CultureInfo.InstalledUICulture.TextInfo.OEMCodePage)</span></span>
   7. <span data-ttu-id="8c072-270">V hello **zpracování standardní výstup** část textového InvokeProcess, nastavte too'data hodnota textbox hello ".</span><span class="sxs-lookup"><span data-stu-id="8c072-270">In hello **Handle Standard Output** section textbox of the InvokeProcess, set hello textbox value too'data'.</span></span> <span data-ttu-id="8c072-271">Toto je proměnné toostore hello standardní výstupní data.</span><span class="sxs-lookup"><span data-stu-id="8c072-271">This is a variable toostore hello standard output data.</span></span>
   8. <span data-ttu-id="8c072-272">Přidat aktivitu WriteBuildMessage pod hello **zpracování standardní výstup** části.</span><span class="sxs-lookup"><span data-stu-id="8c072-272">Add a WriteBuildMessage activity just below hello **Handle Standard Output** section.</span></span> <span data-ttu-id="8c072-273">Nastavit hello důležitosti = 'Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High' a hello zpráva = "data".</span><span class="sxs-lookup"><span data-stu-id="8c072-273">Set hello Importance = 'Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High' and hello Message='data'.</span></span> <span data-ttu-id="8c072-274">Tím se zajistí, že hello standardní výstup skriptu získat zapíšou toohello výstupu sestavení.</span><span class="sxs-lookup"><span data-stu-id="8c072-274">This ensures hello standard output of the script will get written toohello build output.</span></span>
   9. <span data-ttu-id="8c072-275">V hello **zpracovávat chyby výstupu** část textového InvokeProcess, nastavte too'data hodnota textbox hello ".</span><span class="sxs-lookup"><span data-stu-id="8c072-275">In hello **Handle Error Output** section textbox of the InvokeProcess, set hello textbox value too'data'.</span></span> <span data-ttu-id="8c072-276">Toto je dat proměnné toostore hello standardní chyba.</span><span class="sxs-lookup"><span data-stu-id="8c072-276">This is a variable toostore hello standard error data.</span></span>
   10. <span data-ttu-id="8c072-277">Přidat aktivitu WriteBuildError pod hello **zpracovávat chyby výstupu** části.</span><span class="sxs-lookup"><span data-stu-id="8c072-277">Add a WriteBuildError activity just below hello **Handle Error Output** section.</span></span> <span data-ttu-id="8c072-278">Nastavit hello zpráva = "data".</span><span class="sxs-lookup"><span data-stu-id="8c072-278">Set hello Message='data'.</span></span> <span data-ttu-id="8c072-279">Tím se zajistí, že hello standard chyby skriptu hello získat zapíšou toohello sestavení chyby výstupu.</span><span class="sxs-lookup"><span data-stu-id="8c072-279">This ensures hello standard errors of hello script will get written toohello build error output.</span></span>
   11. <span data-ttu-id="8c072-280">Opravte všechny chyby, indikován blue vykřičníku značky.</span><span class="sxs-lookup"><span data-stu-id="8c072-280">Correct any errors, indicated by blue exclamation marks.</span></span> <span data-ttu-id="8c072-281">Pozastavte ukazatel myši nad tooget vykřičníku značky nápovědu o chybě hello.</span><span class="sxs-lookup"><span data-stu-id="8c072-281">Hover over the exclamation marks tooget a hint about hello error.</span></span> <span data-ttu-id="8c072-282">Uložení pracovního postupu hello zrušte chyby.</span><span class="sxs-lookup"><span data-stu-id="8c072-282">Save hello workflow to clear errors.</span></span>

   <span data-ttu-id="8c072-283">Konečný výsledek Hello hello publikovat pracovní postup, který aktivity bude vypadat například takto v Návrháři hello:</span><span class="sxs-lookup"><span data-stu-id="8c072-283">hello final result of hello publish workflow activities will look like this in hello designer:</span></span>

   ![Aktivity pracovního postupu][5]

   <span data-ttu-id="8c072-285">Konečný výsledek Hello hello publikovat pracovní postup, který aktivity bude vypadat například takto v jazyce XAML:</span><span class="sxs-lookup"><span data-stu-id="8c072-285">hello final result of hello publish workflow activities will look like this in XAML:</span></span>

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
8. <span data-ttu-id="8c072-286">Uložte hello pracovní postup šablony procesu sestavení a kontrola v tomto souboru.</span><span class="sxs-lookup"><span data-stu-id="8c072-286">Save hello build process template workflow and Check In this file.</span></span>
9. <span data-ttu-id="8c072-287">Upravit definici sestavení hello (zavřete ho Pokud už je otevřený) a vyberte hello **nový** tlačítko, pokud ještě nevidíte hello nové šablony v seznamu hello šablon procesů.</span><span class="sxs-lookup"><span data-stu-id="8c072-287">Edit hello build definition (close it if it is already open), and select hello **New** button if you do not yet see hello new template in hello list of Process Templates.</span></span>
10. <span data-ttu-id="8c072-288">Nastavte hodnoty hello parametr vlastností v hello různé části takto:</span><span class="sxs-lookup"><span data-stu-id="8c072-288">Set hello parameter property values in hello Misc section as follows:</span></span>

    1. <span data-ttu-id="8c072-289">CloudConfigLocation = "c:\\zahodí\\app.publish\\ServiceConfiguration.Cloud.cscfg' *tato hodnota se odvozuje od: ($PublishDir)ServiceConfiguration.Cloud.cscfg*</span><span class="sxs-lookup"><span data-stu-id="8c072-289">CloudConfigLocation ='c:\\drops\\app.publish\\ServiceConfiguration.Cloud.cscfg' *This value is derived from: ($PublishDir)ServiceConfiguration.Cloud.cscfg*</span></span>
    2. <span data-ttu-id="8c072-290">PackageLocation = "c:\\zahodí\\app.publish\\ContactManager.Azure.cspkg' *tato hodnota se odvozuje od: ($PublishDir)($ProjectName) .cspkg*</span><span class="sxs-lookup"><span data-stu-id="8c072-290">PackageLocation = 'c:\\drops\\app.publish\\ContactManager.Azure.cspkg' *This value is derived from: ($PublishDir)($ProjectName).cspkg*</span></span>
    3. <span data-ttu-id="8c072-291">PublishScriptLocation = "c:\\skripty\\WindowsAzure\\PublishCloudService.ps1.</span><span class="sxs-lookup"><span data-stu-id="8c072-291">PublishScriptLocation = 'c:\\scripts\\WindowsAzure\\PublishCloudService.ps1'</span></span>
    4. <span data-ttu-id="8c072-292">ServiceName = 'mycloudservicename' *použití hello odpovídající název cloudové služby zde*</span><span class="sxs-lookup"><span data-stu-id="8c072-292">ServiceName = 'mycloudservicename' *Use hello appropriate cloud service name here*</span></span>
    5. <span data-ttu-id="8c072-293">Prostředí = "pracovní.</span><span class="sxs-lookup"><span data-stu-id="8c072-293">Environment = 'Staging'</span></span>
    6. <span data-ttu-id="8c072-294">StorageAccountName = 'mystorageaccountname' *název účtu úložiště odpovídající pomocí hello sem*</span><span class="sxs-lookup"><span data-stu-id="8c072-294">StorageAccountName = 'mystorageaccountname' *Use hello appropriate storage account name here*</span></span>
    7. <span data-ttu-id="8c072-295">SubscriptionDataFileLocation = "c:\\skripty\\WindowsAzure\\Subscription.xml.</span><span class="sxs-lookup"><span data-stu-id="8c072-295">SubscriptionDataFileLocation = 'c:\\scripts\\WindowsAzure\\Subscription.xml'</span></span>
    8. <span data-ttu-id="8c072-296">Název_předplatného = "default"</span><span class="sxs-lookup"><span data-stu-id="8c072-296">SubscriptionName = 'default'</span></span>

    ![Vlastnost hodnoty parametru][6]
11. <span data-ttu-id="8c072-298">Uložte změny toohello hello definice sestavení.</span><span class="sxs-lookup"><span data-stu-id="8c072-298">Save hello changes toohello Build Definition.</span></span>
12. <span data-ttu-id="8c072-299">Fronta sestavení tooexecute obě hello balíček sestavení a publikování.</span><span class="sxs-lookup"><span data-stu-id="8c072-299">Queue a Build tooexecute both hello package build and publish.</span></span> <span data-ttu-id="8c072-300">Pokud máte aktivační událost nastavit tooContinuous integrace, provede toto chování na každý vrácení se změnami.</span><span class="sxs-lookup"><span data-stu-id="8c072-300">If you have a trigger set tooContinuous Integration, you will execute this behavior on every check-in.</span></span>

### <a name="publishcloudserviceps1-script-template"></a><span data-ttu-id="8c072-301">Šablona PublishCloudService.ps1 skriptu</span><span class="sxs-lookup"><span data-stu-id="8c072-301">PublishCloudService.ps1 script template</span></span>
```
Param(  $serviceName = "",
        $storageAccountName = "",
        $packageLocation = "",
        $cloudConfigLocation = "",
        $environment = "Staging",
        $deploymentLabel = "ContinuousDeploy too$servicename",
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
    #check for existing deployment and then either upgrade, delete + deploy, or cancel according too$alwaysDeleteExistingDeployments and $enableDeploymentUpgrade boolean variables
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

#main driver - publish & write progress tooactivity log
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script started."
Write-Output "$(Get-Date -f $timeStampFormat) - Preparing deployment of $deploymentLabel for $subscriptionname with Subscription ID $subscriptionid."

Publish

$deployment = Get-AzureDeployment -slot $slot -serviceName $servicename
$deploymentUrl = $deployment.Url

Write-Output "$(Get-Date -f $timeStampFormat) - Created Cloud Service with URL $deploymentUrl."
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script finished."
```

## <a name="next-steps"></a><span data-ttu-id="8c072-302">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8c072-302">Next steps</span></span>
<span data-ttu-id="8c072-303">tooenable vzdálené ladění při použití nastavené průběžné doručování, najdete v části [povolení vzdáleného ladění při použití nastavené průběžné doručování toopublish tooAzure](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md).</span><span class="sxs-lookup"><span data-stu-id="8c072-303">tooenable remote debugging when using continuous delivery, see [Enable remote debugging when using continuous delivery toopublish tooAzure](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md).</span></span>

[Team Foundation Build Service]: https://msdn.microsoft.com/library/ee259687.aspx
[.NET Framework 4]: https://www.microsoft.com/download/details.aspx?id=17851
[.NET Framework 4.5]: https://www.microsoft.com/download/details.aspx?id=30653
[.NET Framework 4.5.2]: https://www.microsoft.com/download/details.aspx?id=42643
[Scale out your build system]: https://msdn.microsoft.com/library/dd793166.aspx
[Deploy and configure a build server]: https://msdn.microsoft.com/library/ms181712.aspx
[Azure PowerShell cmdlets]: /powershell/azureps-cmdlets-docs
[hello .publishsettings file]: https://manage.windowsazure.com/download/publishprofile.aspx?wa=wsignin1.0
[0]: ./media/cloud-services-dotnet-continuous-delivery/tfs-01bc.png
[2]: ./media/cloud-services-dotnet-continuous-delivery/tfs-02.png
[3]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-03.png
[4]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-04.png
[5]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-05.png
[6]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-06.png
