---
title: "Vytvoření kanálu CI/CD v Azure pomocí Team Services | Microsoft Docs"
description: "Naučte se vytvořit kanál Visual Studio Team Services pro průběžnou integraci a doručení, které nasadí webové aplikace do služby IIS na virtuální počítač s Windows"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: a587f58fad2ec74c7633823c4d34f900e7c01f7e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-continuous-integration-pipeline-with-visual-studio-team-services-and-iis"></a><span data-ttu-id="52229-103">Vytvoření kanálu průběžnou integraci s Visual Studio Team Services a služby IIS</span><span class="sxs-lookup"><span data-stu-id="52229-103">Create a continuous integration pipeline with Visual Studio Team Services and IIS</span></span>
<span data-ttu-id="52229-104">K automatizaci sestavení, testování a fáze nasazení pro vývoj aplikací, můžete použít průběžnou integraci a nasazení (CI/CD) kanálu.</span><span class="sxs-lookup"><span data-stu-id="52229-104">To automate the build, test, and deployment phases of application development, you can use a continuous integration and deployment (CI/CD) pipeline.</span></span> <span data-ttu-id="52229-105">V tomto kurzu vytvoříte kanál CI/CD pomocí Visual Studio Team Services a virtuálního počítače (VM) s Windows v Azure, který spouští IIS.</span><span class="sxs-lookup"><span data-stu-id="52229-105">In this tutorial, you create a CI/CD pipeline using Visual Studio Team Services and a Windows virtual machine (VM) in Azure that runs IIS.</span></span> <span data-ttu-id="52229-106">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="52229-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="52229-107">Publikovat webovou aplikaci ASP.NET do projektu Team Services</span><span class="sxs-lookup"><span data-stu-id="52229-107">Publish an ASP.NET web application to a Team Services project</span></span>
> * <span data-ttu-id="52229-108">Vytvořit definici sestavení, která se spustí při potvrzení kódu</span><span class="sxs-lookup"><span data-stu-id="52229-108">Create a build definition that is triggered by code commits</span></span>
> * <span data-ttu-id="52229-109">Instalace a konfigurace služby IIS na virtuální počítač v Azure</span><span class="sxs-lookup"><span data-stu-id="52229-109">Install and configure IIS on a virtual machine in Azure</span></span>
> * <span data-ttu-id="52229-110">Instance služby IIS přidat do skupiny nasazení v Team Services</span><span class="sxs-lookup"><span data-stu-id="52229-110">Add the IIS instance to a deployment group in Team Services</span></span>
> * <span data-ttu-id="52229-111">Vytvořit verzi definice k publikování nové webové nasazení balíčků do služby IIS</span><span class="sxs-lookup"><span data-stu-id="52229-111">Create a release definition to publish new web deploy packages to IIS</span></span>
> * <span data-ttu-id="52229-112">Testování CI/CD kanálu</span><span class="sxs-lookup"><span data-stu-id="52229-112">Test the CI/CD pipeline</span></span>

<span data-ttu-id="52229-113">Tento kurz vyžaduje modul Azure PowerShell verze 3.6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="52229-113">This tutorial requires the Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="52229-114">Verzi zjistíte spuštěním příkazu `Get-Module -ListAvailable AzureRM`.</span><span class="sxs-lookup"><span data-stu-id="52229-114">Run `Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="52229-115">Pokud je třeba upgradovat, přečtěte si téma [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="52229-115">If you need to upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>


## <a name="create-project-in-team-services"></a><span data-ttu-id="52229-116">Vytvoření projektu v Team Services</span><span class="sxs-lookup"><span data-stu-id="52229-116">Create project in Team Services</span></span>
<span data-ttu-id="52229-117">Visual Studio Team Services umožňuje snadno spolupráce a vývoj bez zachování do řešení pro správu kódu na místě.</span><span class="sxs-lookup"><span data-stu-id="52229-117">Visual Studio Team Services allows for easy collaboration and development without maintaining an on-premises code management solution.</span></span> <span data-ttu-id="52229-118">Team Services poskytuje testování kódu cloudu, sestavení a službu application insights.</span><span class="sxs-lookup"><span data-stu-id="52229-118">Team Services provides cloud code testing, build, and application insights.</span></span> <span data-ttu-id="52229-119">Můžete IDE, které nejlépe vyhovuje vaší vývoj kódu a verzi ovládací prvek úložiště.</span><span class="sxs-lookup"><span data-stu-id="52229-119">You can choose a version control repo and IDE that best fits your code development.</span></span> <span data-ttu-id="52229-120">V tomto kurzu slouží k vytvoření základní webové aplikace ASP.NET a CI/CD kanálu bezplatný účet.</span><span class="sxs-lookup"><span data-stu-id="52229-120">For this tutorial, you can use a free account to create a basic ASP.NET web app and CI/CD pipeline.</span></span> <span data-ttu-id="52229-121">Pokud jste již nemají účet Team Services [vytvořit](http://go.microsoft.com/fwlink/?LinkId=307137).</span><span class="sxs-lookup"><span data-stu-id="52229-121">If you do not already have a Team Services account, [create one](http://go.microsoft.com/fwlink/?LinkId=307137).</span></span>

<span data-ttu-id="52229-122">Spravovat proces potvrzení kódu, definice sestavení a verze definice, vytvoření projektu v Team Services následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="52229-122">To manage the code commit process, build definitions, and release definitions, create a project in Team Services as follows:</span></span>

1. <span data-ttu-id="52229-123">Otevřete řídicí panel Team Services ve webovém prohlížeči a zvolte **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="52229-123">Open your Team Services dashboard in a web browser and choose **New project**.</span></span>
2. <span data-ttu-id="52229-124">Zadejte *myWebApp* pro **název projektu**.</span><span class="sxs-lookup"><span data-stu-id="52229-124">Enter *myWebApp* for the **Project name**.</span></span> <span data-ttu-id="52229-125">Nechte všechny ostatní výchozí hodnoty pro použití *Git* verzí a *Agile* zpracování pracovní položky.</span><span class="sxs-lookup"><span data-stu-id="52229-125">Leave all other default values to use *Git* version control and *Agile* work item process.</span></span>
3. <span data-ttu-id="52229-126">Zvolte možnost **sdílet s** *členové týmu*, pak vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="52229-126">Choose the option to **Share with** *Team Members*, then select **Create**.</span></span>
5. <span data-ttu-id="52229-127">Po vytvoření projektu, zvolte možnost **inicializovat README nebo gitignore**, pak **inicializovat**.</span><span class="sxs-lookup"><span data-stu-id="52229-127">Once your project has been created, choose the option to **Initialize with a README or gitignore**, then **Initialize**.</span></span>
6. <span data-ttu-id="52229-128">Uvnitř nový projekt, vyberte **řídicí panely** v horní části, pak vyberte **otevřete v sadě Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="52229-128">Inside your new project, choose **Dashboards** across the top, then select **Open in Visual Studio**.</span></span>


## <a name="create-aspnet-web-application"></a><span data-ttu-id="52229-129">Vytvoření webové aplikace ASP.NET</span><span class="sxs-lookup"><span data-stu-id="52229-129">Create ASP.NET web application</span></span>
<span data-ttu-id="52229-130">V předchozím kroku jste vytvořili projektu v Team Services.</span><span class="sxs-lookup"><span data-stu-id="52229-130">In the previous step, you created a project in Team Services.</span></span> <span data-ttu-id="52229-131">V posledním kroku otevře nový projekt v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="52229-131">The final step opens your new project in Visual Studio.</span></span> <span data-ttu-id="52229-132">Spravovat vaše potvrzení kódu v **Team Explorer** okno.</span><span class="sxs-lookup"><span data-stu-id="52229-132">You manage your code commits in the **Team Explorer** window.</span></span> <span data-ttu-id="52229-133">Vytvořit místní kopii nový projekt, a poté vytvořit webovou aplikaci ASP.NET ze šablony takto:</span><span class="sxs-lookup"><span data-stu-id="52229-133">Create a local copy of your new project, then create an ASP.NET web application from a template as follows:</span></span>

1. <span data-ttu-id="52229-134">Vyberte **klon** k vytvoření úložiště místní git projektu Team Services.</span><span class="sxs-lookup"><span data-stu-id="52229-134">Select **Clone** to create a local git repo of your Team Services project.</span></span>
    
    ![Klonování úložiště z projektu Team Services](media/tutorial-vsts-iis-cicd/clone_repo.png)

2. <span data-ttu-id="52229-136">V části **řešení**, vyberte **nový**.</span><span class="sxs-lookup"><span data-stu-id="52229-136">Under **Solutions**, select **New**.</span></span>

    ![Vytvoření řešení webové aplikace](media/tutorial-vsts-iis-cicd/new_solution.png)

3. <span data-ttu-id="52229-138">Vyberte **webové** šablony a potom zvolte **webové aplikace ASP.NET** šablony.</span><span class="sxs-lookup"><span data-stu-id="52229-138">Select **Web** templates, and then choose the **ASP.NET Web Application** template.</span></span>
    1. <span data-ttu-id="52229-139">Zadejte název vaší aplikace, jako například *myWebApp*a zrušte zaškrtnutí políčka pro **vytvořit adresář pro řešení**.</span><span class="sxs-lookup"><span data-stu-id="52229-139">Enter a name for your application, such as *myWebApp*, and uncheck the box for **Create directory for solution**.</span></span>
    2. <span data-ttu-id="52229-140">Pokud možnost je dostupná, zrušte zaškrtnutí políčka **přidat službu Application Insights do projektu**.</span><span class="sxs-lookup"><span data-stu-id="52229-140">If the option is available, uncheck the box to **Add Application Insights to project**.</span></span> <span data-ttu-id="52229-141">Application Insights, musíte k autorizaci webové aplikace pomocí služby Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="52229-141">Application Insights requires you to authorize your web application with Azure Application Insights.</span></span> <span data-ttu-id="52229-142">Chcete-li zachovat jednoduché v tomto kurzu, přeskočte tento proces.</span><span class="sxs-lookup"><span data-stu-id="52229-142">To keep it simple in this tutorial, skip this process.</span></span>
    3. <span data-ttu-id="52229-143">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="52229-143">Select **OK**.</span></span>
4. <span data-ttu-id="52229-144">Zvolte **MVC** ze seznamu šablony.</span><span class="sxs-lookup"><span data-stu-id="52229-144">Choose **MVC** from the template list.</span></span>
    1. <span data-ttu-id="52229-145">Vyberte **změna ověřování**, zvolte **bez ověřování**, pak vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="52229-145">Select **Change Authentication**, choose **No Authentication**, then select **OK**.</span></span>
    2. <span data-ttu-id="52229-146">Vyberte **OK** k vytvoření řešení.</span><span class="sxs-lookup"><span data-stu-id="52229-146">Select **OK** to create your solution.</span></span>
5. <span data-ttu-id="52229-147">V **Team Explorer** okně zvolte **změny**.</span><span class="sxs-lookup"><span data-stu-id="52229-147">In the **Team Explorer** window, choose **Changes**.</span></span>

    ![Potvrzení změn místní úložiště git Team Services](media/tutorial-vsts-iis-cicd/commit_changes.png)

6. <span data-ttu-id="52229-149">V textovém poli potvrzení, zadejte zprávu *počáteční potvrzení*.</span><span class="sxs-lookup"><span data-stu-id="52229-149">In the commit text box, enter a message such as *Initial commit*.</span></span> <span data-ttu-id="52229-150">Zvolte **potvrdit všechny a synchronizace** z rozevírací nabídky.</span><span class="sxs-lookup"><span data-stu-id="52229-150">Choose **Commit All and Sync** from the drop-down menu.</span></span>


## <a name="create-build-definition"></a><span data-ttu-id="52229-151">Vytvořit definici sestavení</span><span class="sxs-lookup"><span data-stu-id="52229-151">Create build definition</span></span>
<span data-ttu-id="52229-152">V Team Services je možné popisují, jak by měly být vytvořeny vaší aplikace pomocí definice sestavení.</span><span class="sxs-lookup"><span data-stu-id="52229-152">In Team Services, you use a build definition to outline how your application should be built.</span></span> <span data-ttu-id="52229-153">V tomto kurzu vytvoříme základní definice, který trvá naše zdrojového kódu sestavení řešení a potom vytvoří web nasadit balíček, který jsme můžete použít ke spuštění webové aplikace na serveru se službou IIS.</span><span class="sxs-lookup"><span data-stu-id="52229-153">In this tutorial, we create a basic definition that takes our source code, builds the solution, then creates web deploy package we can use to run the web app on an IIS server.</span></span>

1. <span data-ttu-id="52229-154">V projektu Team Services, zvolte **sestavení a verze** v horní části, pak vyberte **sestavení**.</span><span class="sxs-lookup"><span data-stu-id="52229-154">In your Team Services project, choose **Build & Release** across the top, then select **Builds**.</span></span>
3. <span data-ttu-id="52229-155">Vyberte **+ novou definici**.</span><span class="sxs-lookup"><span data-stu-id="52229-155">Select **+ New definition**.</span></span>
4. <span data-ttu-id="52229-156">Vyberte **ASP.NET (PREVIEW)** šablony a vyberte **použít**.</span><span class="sxs-lookup"><span data-stu-id="52229-156">Choose the **ASP.NET (PREVIEW)** template and select **Apply**.</span></span>
5. <span data-ttu-id="52229-157">Ponechejte všechny výchozí hodnoty úkolů.</span><span class="sxs-lookup"><span data-stu-id="52229-157">Leave all the default task values.</span></span> <span data-ttu-id="52229-158">V části **získat zdroje**, ujistěte se, že *myWebApp* úložiště a *hlavní* jsou vybrat větev.</span><span class="sxs-lookup"><span data-stu-id="52229-158">Under **Get sources**, ensure that the *myWebApp* repository and *master* branch are selected.</span></span>

    ![Vytvořit definici sestavení v projektu Team Services](media/tutorial-vsts-iis-cicd/create_build_definition.png)

6. <span data-ttu-id="52229-160">Na **aktivační události** kartě, přesuňte jezdec pro **povolení této aktivační události** k *povoleno*.</span><span class="sxs-lookup"><span data-stu-id="52229-160">On the **Triggers** tab, move the slider for **Enable this trigger** to *Enabled*.</span></span>
7. <span data-ttu-id="52229-161">Uložit definici sestavení a fronty nového sestavení tak, že vyberete **Uložit & fronty** , pak **Uložit & fronty** znovu.</span><span class="sxs-lookup"><span data-stu-id="52229-161">Save the build definition and queue a new build by selecting **Save & queue** , then **Save & queue** again.</span></span> <span data-ttu-id="52229-162">Ponechte výchozí nastavení a vyberte **fronty**.</span><span class="sxs-lookup"><span data-stu-id="52229-162">Leave the defaults and select **Queue**.</span></span>

<span data-ttu-id="52229-163">Kukátko jako sestavení je naplánováno na hostovaného agenta, pak začne sestavení.</span><span class="sxs-lookup"><span data-stu-id="52229-163">Watch as the build is scheduled on a hosted agent, then begins to build.</span></span> <span data-ttu-id="52229-164">Výstup se podobá následujícímu příkladu:</span><span class="sxs-lookup"><span data-stu-id="52229-164">The output is similar to the following example:</span></span>

![Úspěšné sestavení projektu Team Services](media/tutorial-vsts-iis-cicd/successful_build.png)


## <a name="create-virtual-machine"></a><span data-ttu-id="52229-166">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="52229-166">Create virtual machine</span></span>
<span data-ttu-id="52229-167">Pokud chcete zadat platformu pro spouštění vaší webové aplikace ASP.NET, je nutné virtuálního počítače s Windows, který spouští IIS.</span><span class="sxs-lookup"><span data-stu-id="52229-167">To provide a platform to run your ASP.NET web app, you need a Windows virtual machine that runs IIS.</span></span> <span data-ttu-id="52229-168">Team Services používá k interakci s instancí služby IIS jako potvrdíte nějaký kód a aktivaci sestavení agenta.</span><span class="sxs-lookup"><span data-stu-id="52229-168">Team Services uses an agent to interact with the IIS instance as you commit code and builds are triggered.</span></span>

<span data-ttu-id="52229-169">Vytvoření virtuálního počítače Windows serveru 2016 pomocí [tento ukázkový skript](../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="52229-169">Create a Windows Server 2016 VM using [this script sample](../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json).</span></span> <span data-ttu-id="52229-170">Jak dlouho trvá několik minut, než skript, který chcete spustit a vytvořit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="52229-170">It takes a few minutes for the script to run and create the VM.</span></span> <span data-ttu-id="52229-171">Po vytvoření virtuálního počítače, otevřete port 80 pro webový provoz s [přidat AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.resources/new-azurermresourcegroup) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="52229-171">Once the VM has been created, open port 80 for web traffic with [Add-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.resources/new-azurermresourcegroup) as follows:</span></span>

```powershell
Get-AzureRmNetworkSecurityGroup `
  -ResourceGroupName $resourceGroup `
  -Name "myNetworkSecurityGroup" | `
Add-AzureRmNetworkSecurityRuleConfig `
  -Name "myNetworkSecurityGroupRuleWeb" `
  -Protocol "Tcp" `
  -Direction "Inbound" `
  -Priority "1001" `
  -SourceAddressPrefix "*" `
  -SourcePortRange "*" `
  -DestinationAddressPrefix "*" `
  -DestinationPortRange "80" `
  -Access "Allow" | `
Set-AzureRmNetworkSecurityGroup
```

<span data-ttu-id="52229-172">Chcete-li připojit k virtuálnímu počítači, získat veřejnou IP adresu s [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="52229-172">To connect to your VM, obtain the public IP address with [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) as follows:</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup | Select IpAddress
```

<span data-ttu-id="52229-173">Vytvořte relaci vzdálené plochy k virtuálnímu počítači:</span><span class="sxs-lookup"><span data-stu-id="52229-173">Create a remote desktop session to your VM:</span></span>

```cmd
mstsc /v:<publicIpAddress>
```

<span data-ttu-id="52229-174">Na virtuální počítač, otevřete **Powershellu správce** příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="52229-174">On the VM, open an **Administrator PowerShell** command prompt.</span></span> <span data-ttu-id="52229-175">Instalace IIS a požadované funkce .NET následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="52229-175">Install IIS and required .NET features as follows:</span></span>

```powershell
Install-WindowsFeature Web-Server,Web-Asp-Net45,NET-Framework-Features
```


## <a name="create-deployment-group"></a><span data-ttu-id="52229-176">Vytvoření skupiny nasazení</span><span class="sxs-lookup"><span data-stu-id="52229-176">Create deployment group</span></span>
<span data-ttu-id="52229-177">K nabízení balíček nasazení webu pro server služby IIS, můžete definovat skupiny nasazení v Team Services.</span><span class="sxs-lookup"><span data-stu-id="52229-177">To push out the web deploy package to the IIS server, you define a deployment group in Team Services.</span></span> <span data-ttu-id="52229-178">Tato skupina umožňuje zadat, serverů, které jsou cílem nových sestavení automaticky jako potvrdíte nějaký kód do Team Services a sestavení se dokončí.</span><span class="sxs-lookup"><span data-stu-id="52229-178">This group allows you to specify which servers are the target of new builds as you commit code to Team Services and builds are completed.</span></span>

1. <span data-ttu-id="52229-179">V Team Services, zvolte **sestavení a verze** a pak vyberte **nasazení skupiny**.</span><span class="sxs-lookup"><span data-stu-id="52229-179">In Team Services, choose **Build & Release** and then select **Deployment groups**.</span></span>
2. <span data-ttu-id="52229-180">Zvolte **skupiny přidat nasazení**.</span><span class="sxs-lookup"><span data-stu-id="52229-180">Choose **Add Deployment group**.</span></span>
3. <span data-ttu-id="52229-181">Zadejte název skupiny, jako například *mojeiis*, pak vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="52229-181">Enter a name for the group, such as *myIIS*, then select **Create**.</span></span>
4. <span data-ttu-id="52229-182">V **registraci počítačů** se ujistěte, *Windows* je vybrána, a zaškrtněte políčko **používat osobní přístupový token ve skriptu pro ověřování**.</span><span class="sxs-lookup"><span data-stu-id="52229-182">In the **Register machines** section, ensure *Windows* is selected, then check the box to **Use a personal access token in the script for authentication**.</span></span>
5. <span data-ttu-id="52229-183">Vyberte **zkopírujte skript do schránky**.</span><span class="sxs-lookup"><span data-stu-id="52229-183">Select **Copy script to clipboard**.</span></span>


### <a name="add-iis-vm-to-the-deployment-group"></a><span data-ttu-id="52229-184">Přidat do skupiny nasazení virtuálních počítačů služby IIS</span><span class="sxs-lookup"><span data-stu-id="52229-184">Add IIS VM to the deployment group</span></span>
<span data-ttu-id="52229-185">Ke skupině nasazení vytvořen přidejte do skupiny každá instance služby IIS.</span><span class="sxs-lookup"><span data-stu-id="52229-185">With the deployment group created, add each IIS instance to the group.</span></span> <span data-ttu-id="52229-186">Team Services vytváří skript, který se stáhne a nakonfiguruje agenta na virtuálním počítači, který obdrží nové webové nasazení balíčků a použije ji do služby IIS.</span><span class="sxs-lookup"><span data-stu-id="52229-186">Team Services generates a script that downloads and configures an agent on the VM that receives new web deploy packages then applies it to IIS.</span></span>

1. <span data-ttu-id="52229-187">Zpět v **Powershellu správce** relaci na vašem virtuálním počítači, vložte a spusťte skript zkopírovali z Team Services.</span><span class="sxs-lookup"><span data-stu-id="52229-187">Back in the **Administrator PowerShell** session on your VM, paste and run the script copied from Team Services.</span></span>
2. <span data-ttu-id="52229-188">Po zobrazení výzvy ke konfiguraci značky pro agenta, vyberte *Y* a zadejte *webové*.</span><span class="sxs-lookup"><span data-stu-id="52229-188">When prompted to configure tags for the agent, choose *Y* and enter *web*.</span></span>
3. <span data-ttu-id="52229-189">Po zobrazení výzvy pro uživatelský účet, stiskněte klávesu *vrátit* přijměte výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="52229-189">When prompted for the user account, press *Return* to accept the defaults.</span></span>
4. <span data-ttu-id="52229-190">Počkejte na dokončení zprávou skriptu *vstsagent.account.computername služby byla úspěšně spuštěna*.</span><span class="sxs-lookup"><span data-stu-id="52229-190">Wait for the script to finish with a message *Service vstsagent.account.computername started successfully*.</span></span>
5. <span data-ttu-id="52229-191">V **nasazení skupiny** stránky **sestavení a verze** nabídky, otevřete *mojeiis* skupiny nasazení.</span><span class="sxs-lookup"><span data-stu-id="52229-191">In the **Deployment groups** page of the **Build & Release** menu, open the *myIIS* deployment group.</span></span> <span data-ttu-id="52229-192">Na **počítače** kartě, zkontrolujte, zda virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="52229-192">On the **Machines** tab, verify that your VM is listed.</span></span>

    ![Virtuální počítač se úspěšně přidal do skupiny nasazení Team Services](media/tutorial-vsts-iis-cicd/deployment_group.png)


## <a name="create-release-definition"></a><span data-ttu-id="52229-194">Vytvoření definice verze</span><span class="sxs-lookup"><span data-stu-id="52229-194">Create release definition</span></span>
<span data-ttu-id="52229-195">K publikování buildy, můžete vytvořit definici verze v Team Services.</span><span class="sxs-lookup"><span data-stu-id="52229-195">To publish your builds, you create a release definition in Team Services.</span></span> <span data-ttu-id="52229-196">Tuto definici se automaticky spustí podle úspěšném sestavení vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="52229-196">This definition is triggered automatically by a successful build of your application.</span></span> <span data-ttu-id="52229-197">Zvolte skupinu nasazení tak, aby nabízel webového nasazení balíčku a zadejte odpovídající nastavení služby IIS.</span><span class="sxs-lookup"><span data-stu-id="52229-197">You choose the deployment group to push your web deploy package to, and define the appropriate IIS settings.</span></span>

1. <span data-ttu-id="52229-198">Zvolte **sestavení a verze**, pak vyberte **sestavení**.</span><span class="sxs-lookup"><span data-stu-id="52229-198">Choose **Build & Release**, then select **Builds**.</span></span> <span data-ttu-id="52229-199">Zvolte definici sestavení vytvořené v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="52229-199">Choose the build definition created in a previous step.</span></span>
2. <span data-ttu-id="52229-200">V části **nedávném dokončení**, zvolte poslední sestavení a pak vyberte **verze**.</span><span class="sxs-lookup"><span data-stu-id="52229-200">Under **Recently completed**, choose the most recent build, then select **Release**.</span></span>
3. <span data-ttu-id="52229-201">Zvolte **Ano** k vytvoření definice verzí.</span><span class="sxs-lookup"><span data-stu-id="52229-201">Choose **Yes** to create a release definition.</span></span>
4. <span data-ttu-id="52229-202">Vyberte **prázdný** šablony, vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="52229-202">Choose the **Empty** template, then select **Next**.</span></span>
5. <span data-ttu-id="52229-203">Zkontrolujte definici sestavení projektu a zdroj se naplní projektu.</span><span class="sxs-lookup"><span data-stu-id="52229-203">Verify the project and source build definition are populated with your project.</span></span>
6. <span data-ttu-id="52229-204">Vyberte **průběžné nasazování** zaškrtněte políčko a potom vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="52229-204">Select the **Continuous deployment** check box, then select **Create**.</span></span>
7. <span data-ttu-id="52229-205">Vyberte pole rozevíracího seznamu vedle **+ přidat úlohy** a zvolte *přidat skupiny fáze nasazení*.</span><span class="sxs-lookup"><span data-stu-id="52229-205">Select the drop-down box next to **+ Add tasks** and choose *Add a deployment group phase*.</span></span>
    
    ![Přidat úloha uvolnění definice v Team Services](media/tutorial-vsts-iis-cicd/add_release_task.png)

8. <span data-ttu-id="52229-207">Zvolte **přidat** vedle **Deploy(Preview) aplikace webové služby IIS**, pak vyberte **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="52229-207">Choose **Add** next to **IIS Web App Deploy(Preview)**, then select **Close**.</span></span>
9. <span data-ttu-id="52229-208">Vyberte **spustit na skupiny nasazení** nadřazené úlohy.</span><span class="sxs-lookup"><span data-stu-id="52229-208">Select the **Run on deployment group** parent task.</span></span>
    1. <span data-ttu-id="52229-209">Pro **skupiny nasazení**, vyberte nasazení skupinu, kterou jste vytvořili dříve, jako například *mojeiis*.</span><span class="sxs-lookup"><span data-stu-id="52229-209">For **Deployment Group**, select the deployment group you created earlier, such as *myIIS*.</span></span>
    2. <span data-ttu-id="52229-210">V **počítač značky** vyberte **přidat** a zvolte *webové* značky.</span><span class="sxs-lookup"><span data-stu-id="52229-210">In the **Machine tags** box, select **Add** and choose the *web* tag.</span></span>
    
    ![Verze definice nasazení skupiny úloh pro službu IIS](media/tutorial-vsts-iis-cicd/release_definition_iis.png)
 
11. <span data-ttu-id="52229-212">Vyberte **nasadit: nasazení aplikace služby IIS webu** úlohy konfigurace instance nastavení služby IIS následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="52229-212">Select the **Deploy: IIS Web App Deploy** task to configure your IIS instance settings as follows:</span></span>
    1. <span data-ttu-id="52229-213">Pro **název webu**, zadejte *Default Web Site*.</span><span class="sxs-lookup"><span data-stu-id="52229-213">For **Website Name**, enter *Default Web Site*.</span></span>
    2. <span data-ttu-id="52229-214">Ponechte všechna ostatní výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="52229-214">Leave all the other default settings.</span></span>
12. <span data-ttu-id="52229-215">Zvolte **Uložit**, pak vyberte **OK** dvakrát.</span><span class="sxs-lookup"><span data-stu-id="52229-215">Choose **Save**, then select **OK** twice.</span></span>


## <a name="create-a-release-and-publish"></a><span data-ttu-id="52229-216">Vytvoření vydání a publikování</span><span class="sxs-lookup"><span data-stu-id="52229-216">Create a release and publish</span></span>
<span data-ttu-id="52229-217">Nyní můžete posouvat webového nasazení balíčku jako novou verzi.</span><span class="sxs-lookup"><span data-stu-id="52229-217">You can now push your web deploy package as a new release.</span></span> <span data-ttu-id="52229-218">Tento krok komunikuje s agentem na každou instanci, která je součástí skupiny nasazení, nabízených oznámení webu nasazení balíčku a nakonfiguruje službu IIS ke spuštění aktualizované webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="52229-218">This step communicates with the agent on each instance that is part of the deployment group, pushes the web deploy package, then configures IIS to run the updated web application.</span></span>

1. <span data-ttu-id="52229-219">Ve vaší verzi definice, vyberte **+ verze**, zvolte **vytvořit verzi**.</span><span class="sxs-lookup"><span data-stu-id="52229-219">In your release definition, select **+ Release**, then choose **Create Release**.</span></span>
2. <span data-ttu-id="52229-220">Ověřte, zda je na nejnovější verzi vybrali v rozevíracím seznamu, spolu s **automatizované nasazení: Po vytvoření verze**.</span><span class="sxs-lookup"><span data-stu-id="52229-220">Verify that the latest build is selected in the drop-down list, along with **Automated deployment: After release creation**.</span></span> <span data-ttu-id="52229-221">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="52229-221">Select **Create**.</span></span>
3. <span data-ttu-id="52229-222">Malé hlavička se zobrazí v horní části vaší definice vydání, například *vytvořil vydání verze 1*.</span><span class="sxs-lookup"><span data-stu-id="52229-222">A small banner appears across the top of your release definition, such as *Release 'Release-1' has been created*.</span></span> <span data-ttu-id="52229-223">Vyberte verzi odkaz.</span><span class="sxs-lookup"><span data-stu-id="52229-223">Select the release link.</span></span>
4. <span data-ttu-id="52229-224">Otevřete **protokoly** a sledovat průběh verze.</span><span class="sxs-lookup"><span data-stu-id="52229-224">Open the **Logs** tab to watch the release progress.</span></span>
    
    ![Úspěšné verze Team Services a webové nasadit balíček push](media/tutorial-vsts-iis-cicd/successful_release.png)

5. <span data-ttu-id="52229-226">Po dokončení verze otevřete webový prohlížeč a zadejte veřejnou adresu RP vašeho virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="52229-226">After the release is complete, open a web browser and enter the public IIP address of your VM.</span></span> <span data-ttu-id="52229-227">Je spuštěna webová aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="52229-227">Your ASP.NET web application is running.</span></span>

    ![Webová aplikace ASP.NET spuštěná na virtuálním počítači služby IIS](media/tutorial-vsts-iis-cicd/running_web_app.png)


## <a name="test-the-whole-cicd-pipeline"></a><span data-ttu-id="52229-229">Testování celého kanálu CI/CD</span><span class="sxs-lookup"><span data-stu-id="52229-229">Test the whole CI/CD pipeline</span></span>
<span data-ttu-id="52229-230">S vaší webové aplikace běžící v IIS zkuste teď celého kanálu CI/CD.</span><span class="sxs-lookup"><span data-stu-id="52229-230">With your web application running on IIS, now try the whole CI/CD pipeline.</span></span> <span data-ttu-id="52229-231">Po provedení změny v sadě Visual Studio a potvrzení spuštění kódu, sestavení, který potom aktivuje verzi aktualizované webového nasazení balíčku do služby IIS:</span><span class="sxs-lookup"><span data-stu-id="52229-231">After you make a change in Visual Studio and commit your code, a build is triggered which then triggers a release of your updated web deploy package to IIS:</span></span>

1. <span data-ttu-id="52229-232">V sadě Visual Studio, otevřete **Průzkumníku řešení** okno.</span><span class="sxs-lookup"><span data-stu-id="52229-232">In Visual Studio, open the **Solution Explorer** window.</span></span>
2. <span data-ttu-id="52229-233">Přejděte do a otevřete *myWebApp | Zobrazení | Domů | Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="52229-233">Navigate to and open *myWebApp | Views | Home | Index.cshtml*</span></span>
3. <span data-ttu-id="52229-234">Upravte řádek 6 ke čtení:</span><span class="sxs-lookup"><span data-stu-id="52229-234">Edit line 6 to read:</span></span>

    `<h1>ASP.NET with VSTS and CI/CD!</h1>`

4. <span data-ttu-id="52229-235">Uložte soubor.</span><span class="sxs-lookup"><span data-stu-id="52229-235">Save the file.</span></span>
5. <span data-ttu-id="52229-236">Otevřete **Team Explorer** vyberte *myWebApp* projektu, a potom vyberte **změny**.</span><span class="sxs-lookup"><span data-stu-id="52229-236">Open the **Team Explorer** window, select the *myWebApp* project, then choose **Changes**.</span></span>
6. <span data-ttu-id="52229-237">Zadejte zprávu o potvrzení, jako například *testování CI/CD kanálu*, zvolte **potvrzení všechny a synchronizace** z rozevírací nabídky.</span><span class="sxs-lookup"><span data-stu-id="52229-237">Enter a commit message, such as *Testing CI/CD pipeline*, then choose **Commit All and Sync** from the drop-down menu.</span></span>
7. <span data-ttu-id="52229-238">V pracovním prostoru Team Services aktivuje se z kódu potvrzení nového sestavení.</span><span class="sxs-lookup"><span data-stu-id="52229-238">In Team Services workspace, a new build is triggered from the code commit.</span></span> 
    - <span data-ttu-id="52229-239">Zvolte **sestavení a verze**, pak vyberte **sestavení**.</span><span class="sxs-lookup"><span data-stu-id="52229-239">Choose **Build & Release**, then select **Builds**.</span></span> 
    - <span data-ttu-id="52229-240">Vyberte svou definici sestavení a pak vyberte **zařazeno ve frontě & spuštěná** sestavení můžete sledovat v průběhu sestavení.</span><span class="sxs-lookup"><span data-stu-id="52229-240">Choose your build definition, then select the **Queued & running** build to watch as the build progresses.</span></span>
9. <span data-ttu-id="52229-241">Po úspěšném sestavení se aktivuje novou verzi.</span><span class="sxs-lookup"><span data-stu-id="52229-241">Once the build is successful, a new release is triggered.</span></span>
    - <span data-ttu-id="52229-242">Zvolte **sestavení a verze**, pak **verze** zobrazíte webu nasazení balíčku nabídnutých do virtuálního počítače služby IIS.</span><span class="sxs-lookup"><span data-stu-id="52229-242">Choose **Build & Release**, then **Releases** to see the web deploy package pushed to your IIS VM.</span></span> 
    - <span data-ttu-id="52229-243">Vyberte **aktualizovat** ikonu Aktualizovat stav.</span><span class="sxs-lookup"><span data-stu-id="52229-243">Select the **Refresh** icon to update the status.</span></span> <span data-ttu-id="52229-244">Když *prostředí* sloupci se zobrazuje zelená značka zaškrtnutí, vydání úspěšně nasadil do služby IIS.</span><span class="sxs-lookup"><span data-stu-id="52229-244">When the *Environments* column shows a green check mark, the release has successfully deployed to IIS.</span></span>
11. <span data-ttu-id="52229-245">Chcete-li zobrazit změny použity, aktualizujte váš web služby IIS v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="52229-245">To see your changes applied, refresh your IIS website in a browser.</span></span>

    ![Spuštění virtuálního počítače služby IIS z CI/CD kanálu webové aplikace ASP.NET](media/tutorial-vsts-iis-cicd/running_web_app_cicd.png)


## <a name="next-steps"></a><span data-ttu-id="52229-247">Další kroky</span><span class="sxs-lookup"><span data-stu-id="52229-247">Next steps</span></span>

<span data-ttu-id="52229-248">V tomto kurzu vytvoříte webovou aplikaci ASP.NET ve službě Team Services a nakonfigurujete sestavení a definice vydání k nasazení nové webové nasazení balíčků do služby IIS na každém potvrzení kódu.</span><span class="sxs-lookup"><span data-stu-id="52229-248">In this tutorial, you created an ASP.NET web application in Team Services and configured build and release definitions to deploy new web deploy packages to IIS on each code commit.</span></span> <span data-ttu-id="52229-249">Jste se dozvěděli, jak na:</span><span class="sxs-lookup"><span data-stu-id="52229-249">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="52229-250">Publikovat webovou aplikaci ASP.NET do projektu Team Services</span><span class="sxs-lookup"><span data-stu-id="52229-250">Publish an ASP.NET web application to a Team Services project</span></span>
> * <span data-ttu-id="52229-251">Vytvořit definici sestavení, která se spustí při potvrzení kódu</span><span class="sxs-lookup"><span data-stu-id="52229-251">Create a build definition that is triggered by code commits</span></span>
> * <span data-ttu-id="52229-252">Instalace a konfigurace služby IIS na virtuální počítač v Azure</span><span class="sxs-lookup"><span data-stu-id="52229-252">Install and configure IIS on a virtual machine in Azure</span></span>
> * <span data-ttu-id="52229-253">Instance služby IIS přidat do skupiny nasazení v Team Services</span><span class="sxs-lookup"><span data-stu-id="52229-253">Add the IIS instance to a deployment group in Team Services</span></span>
> * <span data-ttu-id="52229-254">Vytvořit verzi definice k publikování nové webové nasazení balíčků do služby IIS</span><span class="sxs-lookup"><span data-stu-id="52229-254">Create a release definition to publish new web deploy packages to IIS</span></span>
> * <span data-ttu-id="52229-255">Testování CI/CD kanálu</span><span class="sxs-lookup"><span data-stu-id="52229-255">Test the CI/CD pipeline</span></span>

<span data-ttu-id="52229-256">Přechodu na v dalším kurzu se dozvíte, jak zabezpečit webového serveru pomocí certifikátů SSL.</span><span class="sxs-lookup"><span data-stu-id="52229-256">Advance to the next tutorial to learn how to secure a web server with SSL certificates.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="52229-257">Zabezpečení webového serveru pomocí protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="52229-257">Secure web server with SSL</span></span>](tutorial-secure-web-server.md)