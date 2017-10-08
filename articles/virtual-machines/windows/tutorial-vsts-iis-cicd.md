---
title: "aaaCreate CI/CD kanál v Azure s Team Services | Microsoft Docs"
description: "Zjistěte, jak toocreate Visual Studio Team Services kanálu pro průběžnou integraci a doručení, která nasazuje tooIIS webové aplikace na virtuální počítač s Windows"
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
ms.openlocfilehash: b758a124c4742854dd3b543f747fd8700f954414
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-continuous-integration-pipeline-with-visual-studio-team-services-and-iis"></a><span data-ttu-id="d5de2-103">Vytvoření kanálu průběžnou integraci s Visual Studio Team Services a služby IIS</span><span class="sxs-lookup"><span data-stu-id="d5de2-103">Create a continuous integration pipeline with Visual Studio Team Services and IIS</span></span>
<span data-ttu-id="d5de2-104">tooautomate hello sestavení, testování a fáze nasazení pro vývoj aplikací, můžete použít průběžnou integraci a nasazení (CI/CD) kanálu.</span><span class="sxs-lookup"><span data-stu-id="d5de2-104">tooautomate hello build, test, and deployment phases of application development, you can use a continuous integration and deployment (CI/CD) pipeline.</span></span> <span data-ttu-id="d5de2-105">V tomto kurzu vytvoříte kanál CI/CD pomocí Visual Studio Team Services a virtuálního počítače (VM) s Windows v Azure, který spouští IIS.</span><span class="sxs-lookup"><span data-stu-id="d5de2-105">In this tutorial, you create a CI/CD pipeline using Visual Studio Team Services and a Windows virtual machine (VM) in Azure that runs IIS.</span></span> <span data-ttu-id="d5de2-106">Získáte informace o těchto tématech:</span><span class="sxs-lookup"><span data-stu-id="d5de2-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d5de2-107">Publikování projektu Team Services tooa webové aplikace ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d5de2-107">Publish an ASP.NET web application tooa Team Services project</span></span>
> * <span data-ttu-id="d5de2-108">Vytvořit definici sestavení, která se spustí při potvrzení kódu</span><span class="sxs-lookup"><span data-stu-id="d5de2-108">Create a build definition that is triggered by code commits</span></span>
> * <span data-ttu-id="d5de2-109">Instalace a konfigurace služby IIS na virtuální počítač v Azure</span><span class="sxs-lookup"><span data-stu-id="d5de2-109">Install and configure IIS on a virtual machine in Azure</span></span>
> * <span data-ttu-id="d5de2-110">Přidání skupiny nasazení tooa instance služby IIS hello v Team Services</span><span class="sxs-lookup"><span data-stu-id="d5de2-110">Add hello IIS instance tooa deployment group in Team Services</span></span>
> * <span data-ttu-id="d5de2-111">Vytvoření nové webové verzi definice toopublish nasadit balíčky tooIIS</span><span class="sxs-lookup"><span data-stu-id="d5de2-111">Create a release definition toopublish new web deploy packages tooIIS</span></span>
> * <span data-ttu-id="d5de2-112">Test hello CI/CD kanálu</span><span class="sxs-lookup"><span data-stu-id="d5de2-112">Test hello CI/CD pipeline</span></span>

<span data-ttu-id="d5de2-113">Tento kurz vyžaduje hello prostředí Azure PowerShell verze modulu 3,6 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="d5de2-113">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="d5de2-114">Spustit `Get-Module -ListAvailable AzureRM` toofind hello verze.</span><span class="sxs-lookup"><span data-stu-id="d5de2-114">Run `Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="d5de2-115">Pokud potřebujete tooupgrade, přečtěte si [modul nainstalovat Azure PowerShell](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="d5de2-115">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>


## <a name="create-project-in-team-services"></a><span data-ttu-id="d5de2-116">Vytvoření projektu v Team Services</span><span class="sxs-lookup"><span data-stu-id="d5de2-116">Create project in Team Services</span></span>
<span data-ttu-id="d5de2-117">Visual Studio Team Services umožňuje snadno spolupráce a vývoj bez zachování do řešení pro správu kódu na místě.</span><span class="sxs-lookup"><span data-stu-id="d5de2-117">Visual Studio Team Services allows for easy collaboration and development without maintaining an on-premises code management solution.</span></span> <span data-ttu-id="d5de2-118">Team Services poskytuje testování kódu cloudu, sestavení a službu application insights.</span><span class="sxs-lookup"><span data-stu-id="d5de2-118">Team Services provides cloud code testing, build, and application insights.</span></span> <span data-ttu-id="d5de2-119">Můžete IDE, které nejlépe vyhovuje vaší vývoj kódu a verzi ovládací prvek úložiště.</span><span class="sxs-lookup"><span data-stu-id="d5de2-119">You can choose a version control repo and IDE that best fits your code development.</span></span> <span data-ttu-id="d5de2-120">V tomto kurzu můžete použít bezplatný účet toocreate základní webové aplikace ASP.NET a CI/CD kanálu.</span><span class="sxs-lookup"><span data-stu-id="d5de2-120">For this tutorial, you can use a free account toocreate a basic ASP.NET web app and CI/CD pipeline.</span></span> <span data-ttu-id="d5de2-121">Pokud jste již nemají účet Team Services [vytvořit](http://go.microsoft.com/fwlink/?LinkId=307137).</span><span class="sxs-lookup"><span data-stu-id="d5de2-121">If you do not already have a Team Services account, [create one](http://go.microsoft.com/fwlink/?LinkId=307137).</span></span>

<span data-ttu-id="d5de2-122">toomanage hello kód potvrzení procesu sestavení definice a verze definice, vytvoření projektu v Team Services následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d5de2-122">toomanage hello code commit process, build definitions, and release definitions, create a project in Team Services as follows:</span></span>

1. <span data-ttu-id="d5de2-123">Otevřete řídicí panel Team Services ve webovém prohlížeči a zvolte **nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-123">Open your Team Services dashboard in a web browser and choose **New project**.</span></span>
2. <span data-ttu-id="d5de2-124">Zadejte *myWebApp* pro hello **název projektu**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-124">Enter *myWebApp* for hello **Project name**.</span></span> <span data-ttu-id="d5de2-125">Nechte všechny ostatní výchozí hodnoty toouse *Git* verzí a *Agile* zpracování pracovní položky.</span><span class="sxs-lookup"><span data-stu-id="d5de2-125">Leave all other default values toouse *Git* version control and *Agile* work item process.</span></span>
3. <span data-ttu-id="d5de2-126">Vyberte možnost hello příliš**sdílet s** *členové týmu*, pak vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-126">Choose hello option too**Share with** *Team Members*, then select **Create**.</span></span>
5. <span data-ttu-id="d5de2-127">Po vytvoření projektu, zvolte možnost hello příliš**inicializovat README nebo gitignore**, pak **inicializovat**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-127">Once your project has been created, choose hello option too**Initialize with a README or gitignore**, then **Initialize**.</span></span>
6. <span data-ttu-id="d5de2-128">Uvnitř nový projekt, vyberte **řídicí panely** napříč hello nahoře, pak vyberte **otevřete v sadě Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-128">Inside your new project, choose **Dashboards** across hello top, then select **Open in Visual Studio**.</span></span>


## <a name="create-aspnet-web-application"></a><span data-ttu-id="d5de2-129">Vytvoření webové aplikace ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d5de2-129">Create ASP.NET web application</span></span>
<span data-ttu-id="d5de2-130">V předchozím kroku hello jste vytvořili projektu v Team Services.</span><span class="sxs-lookup"><span data-stu-id="d5de2-130">In hello previous step, you created a project in Team Services.</span></span> <span data-ttu-id="d5de2-131">posledním krokem Hello otevře nový projekt v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d5de2-131">hello final step opens your new project in Visual Studio.</span></span> <span data-ttu-id="d5de2-132">Spravovat vaše potvrzení kódu v hello **Team Explorer** okno.</span><span class="sxs-lookup"><span data-stu-id="d5de2-132">You manage your code commits in hello **Team Explorer** window.</span></span> <span data-ttu-id="d5de2-133">Vytvořit místní kopii nový projekt, a poté vytvořit webovou aplikaci ASP.NET ze šablony takto:</span><span class="sxs-lookup"><span data-stu-id="d5de2-133">Create a local copy of your new project, then create an ASP.NET web application from a template as follows:</span></span>

1. <span data-ttu-id="d5de2-134">Vyberte **klon** toocreate úložiště místní git projektu Team Services.</span><span class="sxs-lookup"><span data-stu-id="d5de2-134">Select **Clone** toocreate a local git repo of your Team Services project.</span></span>
    
    ![Klonování úložiště z projektu Team Services](media/tutorial-vsts-iis-cicd/clone_repo.png)

2. <span data-ttu-id="d5de2-136">V části **řešení**, vyberte **nový**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-136">Under **Solutions**, select **New**.</span></span>

    ![Vytvoření řešení webové aplikace](media/tutorial-vsts-iis-cicd/new_solution.png)

3. <span data-ttu-id="d5de2-138">Vyberte **webové** šablony a potom zvolte hello **webové aplikace ASP.NET** šablony.</span><span class="sxs-lookup"><span data-stu-id="d5de2-138">Select **Web** templates, and then choose hello **ASP.NET Web Application** template.</span></span>
    1. <span data-ttu-id="d5de2-139">Zadejte název vaší aplikace, jako například *myWebApp*a zrušte zaškrtnutí políčka hello **vytvořit adresář pro řešení**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-139">Enter a name for your application, such as *myWebApp*, and uncheck hello box for **Create directory for solution**.</span></span>
    2. <span data-ttu-id="d5de2-140">Pokud je k dispozici možnost hello, zrušte zaškrtnutí políčka hello příliš**přidat službu Application Insights tooproject**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-140">If hello option is available, uncheck hello box too**Add Application Insights tooproject**.</span></span> <span data-ttu-id="d5de2-141">Application Insights vyžaduje jste tooauthorize webové aplikace pomocí služby Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="d5de2-141">Application Insights requires you tooauthorize your web application with Azure Application Insights.</span></span> <span data-ttu-id="d5de2-142">tookeep to jednoduché v tomto kurzu, tento proces přeskočit.</span><span class="sxs-lookup"><span data-stu-id="d5de2-142">tookeep it simple in this tutorial, skip this process.</span></span>
    3. <span data-ttu-id="d5de2-143">Vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-143">Select **OK**.</span></span>
4. <span data-ttu-id="d5de2-144">Zvolte **MVC** hello šablony seznamu.</span><span class="sxs-lookup"><span data-stu-id="d5de2-144">Choose **MVC** from hello template list.</span></span>
    1. <span data-ttu-id="d5de2-145">Vyberte **změna ověřování**, zvolte **bez ověřování**, pak vyberte **OK**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-145">Select **Change Authentication**, choose **No Authentication**, then select **OK**.</span></span>
    2. <span data-ttu-id="d5de2-146">Vyberte **OK** toocreate řešení.</span><span class="sxs-lookup"><span data-stu-id="d5de2-146">Select **OK** toocreate your solution.</span></span>
5. <span data-ttu-id="d5de2-147">V hello **Team Explorer** okně zvolte **změny**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-147">In hello **Team Explorer** window, choose **Changes**.</span></span>

    ![Potvrdit úložiště git služby tooTeam místní změny](media/tutorial-vsts-iis-cicd/commit_changes.png)

6. <span data-ttu-id="d5de2-149">Do textového pole hello potvrzení, zadejte zprávu *počáteční potvrzení*.</span><span class="sxs-lookup"><span data-stu-id="d5de2-149">In hello commit text box, enter a message such as *Initial commit*.</span></span> <span data-ttu-id="d5de2-150">Zvolte **potvrdit všechny a synchronizace** z rozevírací nabídky hello.</span><span class="sxs-lookup"><span data-stu-id="d5de2-150">Choose **Commit All and Sync** from hello drop-down menu.</span></span>


## <a name="create-build-definition"></a><span data-ttu-id="d5de2-151">Vytvořit definici sestavení</span><span class="sxs-lookup"><span data-stu-id="d5de2-151">Create build definition</span></span>
<span data-ttu-id="d5de2-152">V Team Services použijte toooutline definice sestavení, jak by měly být vytvořeny vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="d5de2-152">In Team Services, you use a build definition toooutline how your application should be built.</span></span> <span data-ttu-id="d5de2-153">V tomto kurzu vytvoříme základní definice, trvá sestavení řešení hello naše zdrojový kód, a potom vytvoří web nasazení balíčku používáme toorun hello webové aplikace na serveru IIS.</span><span class="sxs-lookup"><span data-stu-id="d5de2-153">In this tutorial, we create a basic definition that takes our source code, builds hello solution, then creates web deploy package we can use toorun hello web app on an IIS server.</span></span>

1. <span data-ttu-id="d5de2-154">V projektu Team Services, zvolte **sestavení a verze** napříč hello nahoře, pak vyberte **sestavení**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-154">In your Team Services project, choose **Build & Release** across hello top, then select **Builds**.</span></span>
3. <span data-ttu-id="d5de2-155">Vyberte **+ novou definici**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-155">Select **+ New definition**.</span></span>
4. <span data-ttu-id="d5de2-156">Zvolte hello **ASP.NET (PREVIEW)** šablony a vyberte **použít**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-156">Choose hello **ASP.NET (PREVIEW)** template and select **Apply**.</span></span>
5. <span data-ttu-id="d5de2-157">Ponechte výchozí všechny hello hodnoty úkolů.</span><span class="sxs-lookup"><span data-stu-id="d5de2-157">Leave all hello default task values.</span></span> <span data-ttu-id="d5de2-158">V části **získat zdroje**, ujistěte se, že hello *myWebApp* úložiště a *hlavní* jsou vybrat větev.</span><span class="sxs-lookup"><span data-stu-id="d5de2-158">Under **Get sources**, ensure that hello *myWebApp* repository and *master* branch are selected.</span></span>

    ![Vytvořit definici sestavení v projektu Team Services](media/tutorial-vsts-iis-cicd/create_build_definition.png)

6. <span data-ttu-id="d5de2-160">Na hello **aktivační události** kartě, posuvník hello pro **povolení této aktivační události** příliš*povoleno*.</span><span class="sxs-lookup"><span data-stu-id="d5de2-160">On hello **Triggers** tab, move hello slider for **Enable this trigger** too*Enabled*.</span></span>
7. <span data-ttu-id="d5de2-161">Uložit definici sestavení hello a fronty nového sestavení tak, že vyberete **Uložit & fronty** , pak **Uložit & fronty** znovu.</span><span class="sxs-lookup"><span data-stu-id="d5de2-161">Save hello build definition and queue a new build by selecting **Save & queue** , then **Save & queue** again.</span></span> <span data-ttu-id="d5de2-162">Ponechte výchozí hodnoty hello a vyberte možnost **fronty**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-162">Leave hello defaults and select **Queue**.</span></span>

<span data-ttu-id="d5de2-163">Kukátko jako hello sestavení je naplánováno na hostovaného agenta, pak začne toobuild.</span><span class="sxs-lookup"><span data-stu-id="d5de2-163">Watch as hello build is scheduled on a hosted agent, then begins toobuild.</span></span> <span data-ttu-id="d5de2-164">Hello výstup je podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="d5de2-164">hello output is similar toohello following example:</span></span>

![Úspěšné sestavení projektu Team Services](media/tutorial-vsts-iis-cicd/successful_build.png)


## <a name="create-virtual-machine"></a><span data-ttu-id="d5de2-166">Vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="d5de2-166">Create virtual machine</span></span>
<span data-ttu-id="d5de2-167">tooprovide toorun platformy vaší webové aplikace ASP.NET, je nutné virtuálního počítače s Windows, který spouští IIS.</span><span class="sxs-lookup"><span data-stu-id="d5de2-167">tooprovide a platform toorun your ASP.NET web app, you need a Windows virtual machine that runs IIS.</span></span> <span data-ttu-id="d5de2-168">Team Services používá toointeract agenta s instancí služby IIS hello potvrdíte nějaký kód a aktivaci sestavení.</span><span class="sxs-lookup"><span data-stu-id="d5de2-168">Team Services uses an agent toointeract with hello IIS instance as you commit code and builds are triggered.</span></span>

<span data-ttu-id="d5de2-169">Vytvoření virtuálního počítače Windows serveru 2016 pomocí [tento ukázkový skript](../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d5de2-169">Create a Windows Server 2016 VM using [this script sample](../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json).</span></span> <span data-ttu-id="d5de2-170">Ho trvá několik minut, než hello skriptu toorun a vytvořte hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d5de2-170">It takes a few minutes for hello script toorun and create hello VM.</span></span> <span data-ttu-id="d5de2-171">Po vytvoření hello virtuálních počítačů, otevřete port 80 pro webový provoz s [přidat AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.resources/new-azurermresourcegroup) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d5de2-171">Once hello VM has been created, open port 80 for web traffic with [Add-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.resources/new-azurermresourcegroup) as follows:</span></span>

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

<span data-ttu-id="d5de2-172">tooconnect tooyour virtuálních počítačů, získat hello veřejnou IP adresu s [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d5de2-172">tooconnect tooyour VM, obtain hello public IP address with [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) as follows:</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName $resourceGroup | Select IpAddress
```

<span data-ttu-id="d5de2-173">Vytvoření relace vzdálené plochy tooyour virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="d5de2-173">Create a remote desktop session tooyour VM:</span></span>

```cmd
mstsc /v:<publicIpAddress>
```

<span data-ttu-id="d5de2-174">Na hello virtuálních počítačů, otevřete **Powershellu správce** příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="d5de2-174">On hello VM, open an **Administrator PowerShell** command prompt.</span></span> <span data-ttu-id="d5de2-175">Instalace IIS a požadované funkce .NET následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d5de2-175">Install IIS and required .NET features as follows:</span></span>

```powershell
Install-WindowsFeature Web-Server,Web-Asp-Net45,NET-Framework-Features
```


## <a name="create-deployment-group"></a><span data-ttu-id="d5de2-176">Vytvoření skupiny nasazení</span><span class="sxs-lookup"><span data-stu-id="d5de2-176">Create deployment group</span></span>
<span data-ttu-id="d5de2-177">toopush out hello webové nasadit server služby IIS toohello balíček, můžete definovat skupiny nasazení v Team Services.</span><span class="sxs-lookup"><span data-stu-id="d5de2-177">toopush out hello web deploy package toohello IIS server, you define a deployment group in Team Services.</span></span> <span data-ttu-id="d5de2-178">Tato skupina umožňuje toospecify serverů, které jsou cílem hello nových sestavení automaticky jako potvrzení tooTeam kódu služby a sestavení jsou dokončeny.</span><span class="sxs-lookup"><span data-stu-id="d5de2-178">This group allows you toospecify which servers are hello target of new builds as you commit code tooTeam Services and builds are completed.</span></span>

1. <span data-ttu-id="d5de2-179">V Team Services, zvolte **sestavení a verze** a pak vyberte **nasazení skupiny**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-179">In Team Services, choose **Build & Release** and then select **Deployment groups**.</span></span>
2. <span data-ttu-id="d5de2-180">Zvolte **skupiny přidat nasazení**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-180">Choose **Add Deployment group**.</span></span>
3. <span data-ttu-id="d5de2-181">Například zadejte název pro skupinu hello *mojeiis*, pak vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-181">Enter a name for hello group, such as *myIIS*, then select **Create**.</span></span>
4. <span data-ttu-id="d5de2-182">V hello **registraci počítačů** se ujistěte, *Windows* je vybrána, a zaškrtněte políčko hello příliš**používat osobní přístupový token v hello skriptu pro ověřování**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-182">In hello **Register machines** section, ensure *Windows* is selected, then check hello box too**Use a personal access token in hello script for authentication**.</span></span>
5. <span data-ttu-id="d5de2-183">Vyberte **zkopírujte skript tooclipboard**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-183">Select **Copy script tooclipboard**.</span></span>


### <a name="add-iis-vm-toohello-deployment-group"></a><span data-ttu-id="d5de2-184">Přidat skupinu toohello nasazení virtuálních počítačů služby IIS</span><span class="sxs-lookup"><span data-stu-id="d5de2-184">Add IIS VM toohello deployment group</span></span>
<span data-ttu-id="d5de2-185">S hello nasazení skupina vytvořená přidejte každou skupinu toohello instance služby IIS.</span><span class="sxs-lookup"><span data-stu-id="d5de2-185">With hello deployment group created, add each IIS instance toohello group.</span></span> <span data-ttu-id="d5de2-186">Team Services vytváří skript, který se stáhne a nakonfiguruje agenta na hello nasadit virtuální počítač, který obdrží nové webové balíčky pak použije ji tooIIS.</span><span class="sxs-lookup"><span data-stu-id="d5de2-186">Team Services generates a script that downloads and configures an agent on hello VM that receives new web deploy packages then applies it tooIIS.</span></span>

1. <span data-ttu-id="d5de2-187">Zpět v hello **Powershellu správce** relaci na vašem virtuálním počítači, vložte a spusťte skript hello zkopírovaných z Team Services.</span><span class="sxs-lookup"><span data-stu-id="d5de2-187">Back in hello **Administrator PowerShell** session on your VM, paste and run hello script copied from Team Services.</span></span>
2. <span data-ttu-id="d5de2-188">Když výzvami tooconfigure značky pro hello agenta, vyberte *Y* a zadejte *webové*.</span><span class="sxs-lookup"><span data-stu-id="d5de2-188">When prompted tooconfigure tags for hello agent, choose *Y* and enter *web*.</span></span>
3. <span data-ttu-id="d5de2-189">Po zobrazení výzvy k hello uživatelský účet, stiskněte klávesu *vrátit* tooaccept hello výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d5de2-189">When prompted for hello user account, press *Return* tooaccept hello defaults.</span></span>
4. <span data-ttu-id="d5de2-190">Počkejte hello skriptu toofinish zprávou *vstsagent.account.computername služby byla úspěšně spuštěna*.</span><span class="sxs-lookup"><span data-stu-id="d5de2-190">Wait for hello script toofinish with a message *Service vstsagent.account.computername started successfully*.</span></span>
5. <span data-ttu-id="d5de2-191">V hello **nasazení skupiny** stránku hello **sestavení a verze** nabídky, otevřete hello *mojeiis* skupiny nasazení.</span><span class="sxs-lookup"><span data-stu-id="d5de2-191">In hello **Deployment groups** page of hello **Build & Release** menu, open hello *myIIS* deployment group.</span></span> <span data-ttu-id="d5de2-192">Na hello **počítače** kartě, zkontrolujte, zda virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d5de2-192">On hello **Machines** tab, verify that your VM is listed.</span></span>

    ![Virtuální počítač úspěšně přidal skupinu nasazení služby tooTeam](media/tutorial-vsts-iis-cicd/deployment_group.png)


## <a name="create-release-definition"></a><span data-ttu-id="d5de2-194">Vytvoření definice verze</span><span class="sxs-lookup"><span data-stu-id="d5de2-194">Create release definition</span></span>
<span data-ttu-id="d5de2-195">toopublish sestavení, můžete vytvořit definici verze v Team Services.</span><span class="sxs-lookup"><span data-stu-id="d5de2-195">toopublish your builds, you create a release definition in Team Services.</span></span> <span data-ttu-id="d5de2-196">Tuto definici se automaticky spustí podle úspěšném sestavení vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="d5de2-196">This definition is triggered automatically by a successful build of your application.</span></span> <span data-ttu-id="d5de2-197">Zvolte hello nasazení skupiny toopush webového nasazení balíčku a zadejte odpovídající nastavení služby IIS hello.</span><span class="sxs-lookup"><span data-stu-id="d5de2-197">You choose hello deployment group toopush your web deploy package to, and define hello appropriate IIS settings.</span></span>

1. <span data-ttu-id="d5de2-198">Zvolte **sestavení a verze**, pak vyberte **sestavení**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-198">Choose **Build & Release**, then select **Builds**.</span></span> <span data-ttu-id="d5de2-199">Zvolte definici sestavení hello vytvořili v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="d5de2-199">Choose hello build definition created in a previous step.</span></span>
2. <span data-ttu-id="d5de2-200">V části **nedávném dokončení**, zvolte poslední sestavení hello a pak vyberte **verze**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-200">Under **Recently completed**, choose hello most recent build, then select **Release**.</span></span>
3. <span data-ttu-id="d5de2-201">Zvolte **Ano** toocreate definici verze.</span><span class="sxs-lookup"><span data-stu-id="d5de2-201">Choose **Yes** toocreate a release definition.</span></span>
4. <span data-ttu-id="d5de2-202">Zvolte hello **prázdný** šablony, vyberte **Další**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-202">Choose hello **Empty** template, then select **Next**.</span></span>
5. <span data-ttu-id="d5de2-203">Zkontrolujte definici sestavení projektu a zdroj hello se naplní projektu.</span><span class="sxs-lookup"><span data-stu-id="d5de2-203">Verify hello project and source build definition are populated with your project.</span></span>
6. <span data-ttu-id="d5de2-204">Vyberte hello **průběžné nasazování** zaškrtněte políčko a potom vyberte **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-204">Select hello **Continuous deployment** check box, then select **Create**.</span></span>
7. <span data-ttu-id="d5de2-205">Vyberte hello rozevíracího seznamu vedle příliš**+ přidat úlohy** a zvolte *přidat skupiny fáze nasazení*.</span><span class="sxs-lookup"><span data-stu-id="d5de2-205">Select hello drop-down box next too**+ Add tasks** and choose *Add a deployment group phase*.</span></span>
    
    ![Přidat úloha toorelease definice v Team Services](media/tutorial-vsts-iis-cicd/add_release_task.png)

8. <span data-ttu-id="d5de2-207">Zvolte **přidat** další příliš**Deploy(Preview) aplikace webové služby IIS**, pak vyberte **Zavřít**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-207">Choose **Add** next too**IIS Web App Deploy(Preview)**, then select **Close**.</span></span>
9. <span data-ttu-id="d5de2-208">Vyberte hello **spustit na skupiny nasazení** nadřazené úlohy.</span><span class="sxs-lookup"><span data-stu-id="d5de2-208">Select hello **Run on deployment group** parent task.</span></span>
    1. <span data-ttu-id="d5de2-209">Pro **skupiny nasazení**, vyberte nasazení hello skupiny, kterou jste vytvořený dříve, jako třeba *mojeiis*.</span><span class="sxs-lookup"><span data-stu-id="d5de2-209">For **Deployment Group**, select hello deployment group you created earlier, such as *myIIS*.</span></span>
    2. <span data-ttu-id="d5de2-210">V hello **počítač značky** vyberte **přidat** a zvolte hello *webové* značky.</span><span class="sxs-lookup"><span data-stu-id="d5de2-210">In hello **Machine tags** box, select **Add** and choose hello *web* tag.</span></span>
    
    ![Verze definice nasazení skupiny úloh pro službu IIS](media/tutorial-vsts-iis-cicd/release_definition_iis.png)
 
11. <span data-ttu-id="d5de2-212">Vyberte hello **nasadit: nasazení aplikace služby IIS webu** tooconfigure úloh vaší služby IIS instance nastavení následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="d5de2-212">Select hello **Deploy: IIS Web App Deploy** task tooconfigure your IIS instance settings as follows:</span></span>
    1. <span data-ttu-id="d5de2-213">Pro **název webu**, zadejte *Default Web Site*.</span><span class="sxs-lookup"><span data-stu-id="d5de2-213">For **Website Name**, enter *Default Web Site*.</span></span>
    2. <span data-ttu-id="d5de2-214">Ponechejte hello všechny ostatní výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="d5de2-214">Leave all hello other default settings.</span></span>
12. <span data-ttu-id="d5de2-215">Zvolte **Uložit**, pak vyberte **OK** dvakrát.</span><span class="sxs-lookup"><span data-stu-id="d5de2-215">Choose **Save**, then select **OK** twice.</span></span>


## <a name="create-a-release-and-publish"></a><span data-ttu-id="d5de2-216">Vytvoření vydání a publikování</span><span class="sxs-lookup"><span data-stu-id="d5de2-216">Create a release and publish</span></span>
<span data-ttu-id="d5de2-217">Nyní můžete posouvat webového nasazení balíčku jako novou verzi.</span><span class="sxs-lookup"><span data-stu-id="d5de2-217">You can now push your web deploy package as a new release.</span></span> <span data-ttu-id="d5de2-218">Tento krok komunikuje s agentem hello na každou instanci, která je součástí skupiny nasazení hello, nabízených oznámení hello webové nasazení balíčku a nakonfiguruje službu IIS toorun hello aktualizovat webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="d5de2-218">This step communicates with hello agent on each instance that is part of hello deployment group, pushes hello web deploy package, then configures IIS toorun hello updated web application.</span></span>

1. <span data-ttu-id="d5de2-219">Ve vaší verzi definice, vyberte **+ verze**, zvolte **vytvořit verzi**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-219">In your release definition, select **+ Release**, then choose **Create Release**.</span></span>
2. <span data-ttu-id="d5de2-220">Ověřte, zda je sestavení nejnovější hello vybrali v rozevíracím seznamu hello, spolu s **automatizované nasazení: Po vytvoření verze**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-220">Verify that hello latest build is selected in hello drop-down list, along with **Automated deployment: After release creation**.</span></span> <span data-ttu-id="d5de2-221">Vyberte **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-221">Select **Create**.</span></span>
3. <span data-ttu-id="d5de2-222">Nápis informující o malé se zobrazí napříč hello horní části vaší definice vydání, jako například *vytvořil vydání verze 1*.</span><span class="sxs-lookup"><span data-stu-id="d5de2-222">A small banner appears across hello top of your release definition, such as *Release 'Release-1' has been created*.</span></span> <span data-ttu-id="d5de2-223">Vyberte hello verze odkaz.</span><span class="sxs-lookup"><span data-stu-id="d5de2-223">Select hello release link.</span></span>
4. <span data-ttu-id="d5de2-224">Otevřete hello **protokoly** kartě toowatch hello verze průběh.</span><span class="sxs-lookup"><span data-stu-id="d5de2-224">Open hello **Logs** tab toowatch hello release progress.</span></span>
    
    ![Úspěšné verze Team Services a webové nasadit balíček push](media/tutorial-vsts-iis-cicd/successful_release.png)

5. <span data-ttu-id="d5de2-226">Po dokončení hello verze otevřete webový prohlížeč a zadejte hello veřejnou adresu RP vašeho virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d5de2-226">After hello release is complete, open a web browser and enter hello public IIP address of your VM.</span></span> <span data-ttu-id="d5de2-227">Je spuštěna webová aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d5de2-227">Your ASP.NET web application is running.</span></span>

    ![Webová aplikace ASP.NET spuštěná na virtuálním počítači služby IIS](media/tutorial-vsts-iis-cicd/running_web_app.png)


## <a name="test-hello-whole-cicd-pipeline"></a><span data-ttu-id="d5de2-229">Test hello celou CI/CD kanálu</span><span class="sxs-lookup"><span data-stu-id="d5de2-229">Test hello whole CI/CD pipeline</span></span>
<span data-ttu-id="d5de2-230">S vaší webové aplikace běžící v IIS zkuste teď hello celou CI/CD kanálu.</span><span class="sxs-lookup"><span data-stu-id="d5de2-230">With your web application running on IIS, now try hello whole CI/CD pipeline.</span></span> <span data-ttu-id="d5de2-231">Po provedení změny v sadě Visual Studio a potvrzení spuštění kódu, sestavení, který potom aktivuje verzi aktualizované webového nasaďte balíček tooIIS:</span><span class="sxs-lookup"><span data-stu-id="d5de2-231">After you make a change in Visual Studio and commit your code, a build is triggered which then triggers a release of your updated web deploy package tooIIS:</span></span>

1. <span data-ttu-id="d5de2-232">V sadě Visual Studio otevřete hello **Průzkumníku řešení** okno.</span><span class="sxs-lookup"><span data-stu-id="d5de2-232">In Visual Studio, open hello **Solution Explorer** window.</span></span>
2. <span data-ttu-id="d5de2-233">Přejděte otevřete tooand *myWebApp | Zobrazení | Domů | Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d5de2-233">Navigate tooand open *myWebApp | Views | Home | Index.cshtml*</span></span>
3. <span data-ttu-id="d5de2-234">Upravte tooread řádek 6:</span><span class="sxs-lookup"><span data-stu-id="d5de2-234">Edit line 6 tooread:</span></span>

    `<h1>ASP.NET with VSTS and CI/CD!</h1>`

4. <span data-ttu-id="d5de2-235">Uložte soubor hello.</span><span class="sxs-lookup"><span data-stu-id="d5de2-235">Save hello file.</span></span>
5. <span data-ttu-id="d5de2-236">Otevřete hello **Team Explorer** okno, vyberte hello *myWebApp* projektu, a potom vyberte **změny**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-236">Open hello **Team Explorer** window, select hello *myWebApp* project, then choose **Changes**.</span></span>
6. <span data-ttu-id="d5de2-237">Zadejte zprávu o potvrzení, jako například *testování CI/CD kanálu*, zvolte **potvrzení všechny a synchronizace** z rozevírací nabídky hello.</span><span class="sxs-lookup"><span data-stu-id="d5de2-237">Enter a commit message, such as *Testing CI/CD pipeline*, then choose **Commit All and Sync** from hello drop-down menu.</span></span>
7. <span data-ttu-id="d5de2-238">V pracovním prostoru Team Services aktivuje se z hello kód potvrzení nového sestavení.</span><span class="sxs-lookup"><span data-stu-id="d5de2-238">In Team Services workspace, a new build is triggered from hello code commit.</span></span> 
    - <span data-ttu-id="d5de2-239">Zvolte **sestavení a verze**, pak vyberte **sestavení**.</span><span class="sxs-lookup"><span data-stu-id="d5de2-239">Choose **Build & Release**, then select **Builds**.</span></span> 
    - <span data-ttu-id="d5de2-240">Vyberte svou definici sestavení a pak vyberte hello **zařazeno ve frontě & spuštěná** toowatch sestavení jako hello sestavení postupuje.</span><span class="sxs-lookup"><span data-stu-id="d5de2-240">Choose your build definition, then select hello **Queued & running** build toowatch as hello build progresses.</span></span>
9. <span data-ttu-id="d5de2-241">Po úspěšném sestavení hello se aktivuje novou verzi.</span><span class="sxs-lookup"><span data-stu-id="d5de2-241">Once hello build is successful, a new release is triggered.</span></span>
    - <span data-ttu-id="d5de2-242">Zvolte **sestavení a verze**, pak **verze** nasazení webu hello toosee balíček nabídnutých tooyour virtuálních počítačů služby IIS.</span><span class="sxs-lookup"><span data-stu-id="d5de2-242">Choose **Build & Release**, then **Releases** toosee hello web deploy package pushed tooyour IIS VM.</span></span> 
    - <span data-ttu-id="d5de2-243">Vyberte hello **aktualizovat** ikonu tooupdate hello stavu.</span><span class="sxs-lookup"><span data-stu-id="d5de2-243">Select hello **Refresh** icon tooupdate hello status.</span></span> <span data-ttu-id="d5de2-244">Když hello *prostředí* sloupci se zobrazuje zelená značka zaškrtnutí, verze hello úspěšně nasadil tooIIS.</span><span class="sxs-lookup"><span data-stu-id="d5de2-244">When hello *Environments* column shows a green check mark, hello release has successfully deployed tooIIS.</span></span>
11. <span data-ttu-id="d5de2-245">toosee vaše změny použít, aktualizujte váš web služby IIS v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="d5de2-245">toosee your changes applied, refresh your IIS website in a browser.</span></span>

    ![Spuštění virtuálního počítače služby IIS z CI/CD kanálu webové aplikace ASP.NET](media/tutorial-vsts-iis-cicd/running_web_app_cicd.png)


## <a name="next-steps"></a><span data-ttu-id="d5de2-247">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d5de2-247">Next steps</span></span>

<span data-ttu-id="d5de2-248">V tomto kurzu jste vytvořili webovou aplikaci ASP.NET v Team Services a nakonfigurované sestavení a verze definice toodeploy nové webové nasadit balíčky tooIIS na každém potvrzení kódu.</span><span class="sxs-lookup"><span data-stu-id="d5de2-248">In this tutorial, you created an ASP.NET web application in Team Services and configured build and release definitions toodeploy new web deploy packages tooIIS on each code commit.</span></span> <span data-ttu-id="d5de2-249">Naučili jste se tyto postupy:</span><span class="sxs-lookup"><span data-stu-id="d5de2-249">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d5de2-250">Publikování projektu Team Services tooa webové aplikace ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d5de2-250">Publish an ASP.NET web application tooa Team Services project</span></span>
> * <span data-ttu-id="d5de2-251">Vytvořit definici sestavení, která se spustí při potvrzení kódu</span><span class="sxs-lookup"><span data-stu-id="d5de2-251">Create a build definition that is triggered by code commits</span></span>
> * <span data-ttu-id="d5de2-252">Instalace a konfigurace služby IIS na virtuální počítač v Azure</span><span class="sxs-lookup"><span data-stu-id="d5de2-252">Install and configure IIS on a virtual machine in Azure</span></span>
> * <span data-ttu-id="d5de2-253">Přidání skupiny nasazení tooa instance služby IIS hello v Team Services</span><span class="sxs-lookup"><span data-stu-id="d5de2-253">Add hello IIS instance tooa deployment group in Team Services</span></span>
> * <span data-ttu-id="d5de2-254">Vytvoření nové webové verzi definice toopublish nasadit balíčky tooIIS</span><span class="sxs-lookup"><span data-stu-id="d5de2-254">Create a release definition toopublish new web deploy packages tooIIS</span></span>
> * <span data-ttu-id="d5de2-255">Test hello CI/CD kanálu</span><span class="sxs-lookup"><span data-stu-id="d5de2-255">Test hello CI/CD pipeline</span></span>

<span data-ttu-id="d5de2-256">Jak zálohy další kurz toolearn toohello toosecure webového serveru pomocí certifikátů SSL.</span><span class="sxs-lookup"><span data-stu-id="d5de2-256">Advance toohello next tutorial toolearn how toosecure a web server with SSL certificates.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d5de2-257">Zabezpečení webového serveru pomocí protokolu SSL</span><span class="sxs-lookup"><span data-stu-id="d5de2-257">Secure web server with SSL</span></span>](tutorial-secure-web-server.md)