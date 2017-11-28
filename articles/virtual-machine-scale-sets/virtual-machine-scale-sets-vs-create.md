---
title: "aaaDeploy sadu škálování virtuálního počítače pomocí sady Visual Studio | Microsoft Docs"
description: "Nasazení sady škálování virtuálního počítače pomocí sady Visual Studio a šablony Resource Manageru"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: ed0786b8-34b2-49a8-85b5-2a628128ead6
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/13/2017
ms.author: guybo
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c89a9f2478ccc3d22989aea604a4273bcc46df82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-virtual-machine-scale-set-with-visual-studio"></a><span data-ttu-id="734f9-103">Jak toocreate Škálovací sady virtuálních počítačů pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="734f9-103">How toocreate a Virtual Machine Scale Set with Visual Studio</span></span>
<span data-ttu-id="734f9-104">Tento článek ukazuje, jak toodeploy Azure Škálovací sady virtuálních počítačů pomocí Visual Studio nasazení skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="734f9-104">This article shows you how toodeploy an Azure Virtual Machine Scale Set using a Visual Studio Resource Group Deployment.</span></span>

<span data-ttu-id="734f9-105">[Azure sady škálování virtuálního počítače](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) Azure Compute toodeploy prostředků a spravovat kolekce podobně jako virtuální počítače s automatickému škálování a vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="734f9-105">[Azure Virtual Machine Scale Sets](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview/) is an Azure Compute resource toodeploy and manage a collection of similar virtual machines with auto-scale and load balancing.</span></span> <span data-ttu-id="734f9-106">Můžete zřídit a nasadit sady škálování virtuálního počítače pomocí [šablon Azure Resource Manageru](https://github.com/Azure/azure-quickstart-templates).</span><span class="sxs-lookup"><span data-stu-id="734f9-106">You can provision and deploy Virtual Machine Scale Sets using [Azure Resource Manager Templates](https://github.com/Azure/azure-quickstart-templates).</span></span> <span data-ttu-id="734f9-107">Šablony Azure Resource Manageru můžete nasadit pomocí rozhraní příkazového řádku Azure, prostředí PowerShell, REST a také přímo ze sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="734f9-107">Azure Resource Manager Templates can be deployed using Azure CLI, PowerShell, REST and also directly from Visual Studio.</span></span> <span data-ttu-id="734f9-108">Visual Studio poskytuje sadu příklad šablony, které můžete nasadit jako součást projektu nasazení skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="734f9-108">Visual Studio provides a set of example templates, which can be deployed as part of an Azure Resource Group Deployment project.</span></span>

<span data-ttu-id="734f9-109">Nasazení skupiny prostředků Azure jsou toogroup způsob a publikovat sadu souvisejících prostředků Azure v rámci operace jedno nasazení.</span><span class="sxs-lookup"><span data-stu-id="734f9-109">Azure Resource Group deployments are a way toogroup and publish a set of related Azure resources in a single deployment operation.</span></span> <span data-ttu-id="734f9-110">Další informace o nich zde: [vytvoření a nasazení skupin prostředků Azure pomocí sady Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="734f9-110">You can learn more about them here: [Creating and deploying Azure resource groups through Visual Studio](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="734f9-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="734f9-111">Pre-requisites</span></span>
<span data-ttu-id="734f9-112">tooget začít s nasazením sady škálování virtuálního počítače v sadě Visual Studio, potřebujete následující hello:</span><span class="sxs-lookup"><span data-stu-id="734f9-112">tooget started deploying Virtual Machine Scale Sets in Visual Studio, you need hello following:</span></span>

* <span data-ttu-id="734f9-113">Visual Studio 2013 nebo novější</span><span class="sxs-lookup"><span data-stu-id="734f9-113">Visual Studio 2013 or later</span></span>
* <span data-ttu-id="734f9-114">Azure SDK 2.7, 2.8 nebo 2.9</span><span class="sxs-lookup"><span data-stu-id="734f9-114">Azure SDK 2.7, 2.8 or 2.9</span></span>

>[!NOTE]
><span data-ttu-id="734f9-115">Tyto pokyny předpokládají, že používáte Visual Studio s [Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).</span><span class="sxs-lookup"><span data-stu-id="734f9-115">These instructions assume you are using Visual Studio with [Azure SDK 2.8](https://azure.microsoft.com/blog/announcing-the-azure-sdk-2-8-for-net/).</span></span>

## <a name="creating-a-project"></a><span data-ttu-id="734f9-116">Vytvoření projektu</span><span class="sxs-lookup"><span data-stu-id="734f9-116">Creating a Project</span></span>
1. <span data-ttu-id="734f9-117">Vytvořte nový projekt v sadě Visual Studio výběrem **souboru | Nové | Projekt**.</span><span class="sxs-lookup"><span data-stu-id="734f9-117">Create a new project in Visual Studio by choosing **File | New | Project**.</span></span>
   
    ![Nový soubor][file_new]

2. <span data-ttu-id="734f9-119">V části **Visual C# | Cloud**, zvolte **Azure Resource Manager** toocreate projekt pro nasazení šablonu Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="734f9-119">Under **Visual C# | Cloud**, choose **Azure Resource Manager** toocreate a project for deploying an Azure Resource Manager Template.</span></span>
   
    ![Vytvoření projektu][create_project]

3. <span data-ttu-id="734f9-121">Hello seznam šablon vyberte hello Linux nebo Windows nastavte šablony škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="734f9-121">From hello list of Templates, select either hello Linux or Windows Virtual Machine Scale Set Template.</span></span>
   
   ![Vyberte šablonu][select_Template]

4. <span data-ttu-id="734f9-123">Po vytvoření projektu zobrazí nasazení skriptů prostředí PowerShell, šablonu Azure Resource Manager a soubor parametrů pro hello sadu škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="734f9-123">Once your project is created you see PowerShell deployment scripts, an Azure Resource Manager Template, and a parameter file for hello Virtual Machine Scale Set.</span></span>
   
    ![Průzkumník řešení][solution_explorer]

## <a name="customize-your-project"></a><span data-ttu-id="734f9-125">Přizpůsobení projektu</span><span class="sxs-lookup"><span data-stu-id="734f9-125">Customize your project</span></span>
<span data-ttu-id="734f9-126">Teď můžete upravit hello šablony toocustomize pro potřeby vaší aplikace, například přidávání vlastnosti rozšíření virtuálního počítače nebo úprava pravidla Vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="734f9-126">Now you can edit hello Template toocustomize it for your application's needs, such as adding VM extension properties or editing load balancing rules.</span></span> <span data-ttu-id="734f9-127">Ve výchozím nastavení jsou šablony sad škálování hello virtuální počítač nakonfigurovaný toodeploy hello AzureDiagnostics rozšíření, které umožňuje snadno tooadd škálování pravidla.</span><span class="sxs-lookup"><span data-stu-id="734f9-127">By default hello Virtual Machine Scale Set Templates are configured toodeploy hello AzureDiagnostics extension, which makes it easy tooadd autoscale rules.</span></span> <span data-ttu-id="734f9-128">Kromě toho nasadí nástroj pro vyrovnávání zatížení s veřejnou IP adresu, nakonfigurované příchozích pravidel NAT.</span><span class="sxs-lookup"><span data-stu-id="734f9-128">It also deploys a load balancer with a public IP address, configured with inbound NAT rules.</span></span> 

<span data-ttu-id="734f9-129">Nástroj pro vyrovnávání zatížení Hello vám umožní připojit se toohello instance virtuálních počítačů pomocí protokolu SSH (Linux) nebo protokolu RDP (Windows).</span><span class="sxs-lookup"><span data-stu-id="734f9-129">hello load balancer lets you connect toohello VM instances with SSH (Linux) or RDP (Windows).</span></span> <span data-ttu-id="734f9-130">rozsah portů front-endu Hello začíná na 50000.</span><span class="sxs-lookup"><span data-stu-id="734f9-130">hello front-end port range starts at 50000.</span></span> <span data-ttu-id="734f9-131">Pro linux to znamená, že pokud jste SSH tooport 50000, jsou směrované tooport 22 z hello první virtuální počítač v hello sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="734f9-131">For linux this means that if you SSH tooport 50000, you are routed tooport 22 of hello first VM in hello Scale Set.</span></span> <span data-ttu-id="734f9-132">Připojení tooport 50001 je směrované tooport 22 Dobrý den sekundu virtuálních počítačů, a tak dále.</span><span class="sxs-lookup"><span data-stu-id="734f9-132">Connecting tooport 50001 is routed tooport 22 of hello second VM and so on.</span></span>

 <span data-ttu-id="734f9-133">Dobře tooedit vaše šablony v prostředí Visual Studio je toouse hello osnovy JSON tooorganize hello parametry, proměnné a prostředky.</span><span class="sxs-lookup"><span data-stu-id="734f9-133">A good way tooedit your Templates with Visual Studio is toouse hello JSON Outline tooorganize hello parameters, variables, and resources.</span></span> <span data-ttu-id="734f9-134">Se seznámíte s hello můžete před nasazením schématu sady Visual Studio upozorníme na chyby ve vaší šabloně.</span><span class="sxs-lookup"><span data-stu-id="734f9-134">With an understanding of hello schema Visual Studio can point out errors in your Template before you deploy it.</span></span>

![Průzkumník JSON][json_explorer]

## <a name="deploy-hello-project"></a><span data-ttu-id="734f9-136">Nasazení projektu hello</span><span class="sxs-lookup"><span data-stu-id="734f9-136">Deploy hello project</span></span>
1. <span data-ttu-id="734f9-137">Nasaďte hello šablony Azure Resource Manageru toocreate hello sadu škálování virtuálního počítače prostředků.</span><span class="sxs-lookup"><span data-stu-id="734f9-137">Deploy hello Azure Resource Manager Template toocreate hello Virtual Machine Scale Set resource.</span></span> <span data-ttu-id="734f9-138">Klikněte pravým tlačítkem na uzel projektu hello a zvolte **nasadit | Nové nasazení**.</span><span class="sxs-lookup"><span data-stu-id="734f9-138">Right-click on hello project node and choose **Deploy | New Deployment**.</span></span>
   
    ![Nasazení šablony][5deploy_Template]
    
2. <span data-ttu-id="734f9-140">V dialogovém okně "Nasadit tooResource skupiny" hello, vyberte své předplatné.</span><span class="sxs-lookup"><span data-stu-id="734f9-140">Select your subscription in hello “Deploy tooResource Group” dialog.</span></span>
   
    ![Nasazení šablony][6deploy_Template]

3. <span data-ttu-id="734f9-142">Tady můžete vytvořit šablonu, aby toodeploy skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="734f9-142">From here, you can create an Azure Resource Group toodeploy your Template to.</span></span>
   
    ![Novou skupinu prostředků][new_resource]

4. <span data-ttu-id="734f9-144">Klikněte na tlačítko **upravit parametry** tooenter parametry, které se předávají tooyour šablony.</span><span class="sxs-lookup"><span data-stu-id="734f9-144">Next, click **Edit Parameters** tooenter parameters that are passed tooyour Template.</span></span> <span data-ttu-id="734f9-145">Zadejte hello uživatelské jméno a heslo pro hello operačního systému, což je nasazení požadované toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="734f9-145">Provide hello username and password for hello OS, which is required toocreate hello deployment.</span></span> <span data-ttu-id="734f9-146">Pokud nemáte prostředí PowerShell nástroje pro sadu Visual Studio, je vhodné toocheck **ukládat hesla** tooavoid skrytá příkazového řádku prostředí PowerShell vyzve, nebo použijte [keyvault podporu](https://azure.microsoft.com/blog/keyvault-support-for-arm-templates/).</span><span class="sxs-lookup"><span data-stu-id="734f9-146">If you don't have PowerShell Tools for Visual Studio installed, it is recommended toocheck **Save passwords** tooavoid a hidden PowerShell command-line prompt, or use [keyvault support](https://azure.microsoft.com/blog/keyvault-support-for-arm-templates/).</span></span>
   
    ![Upravit parametry][edit_parameters]

5. <span data-ttu-id="734f9-148">Klikněte na tlačítko **nasadit**.</span><span class="sxs-lookup"><span data-stu-id="734f9-148">Now click **Deploy**.</span></span> <span data-ttu-id="734f9-149">Hello **výstup** v okně se zobrazí průběh nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="734f9-149">hello **Output** window shows hello deployment progress.</span></span> <span data-ttu-id="734f9-150">Všimněte si, že akce hello provádí hello **nasadit AzureResourceGroup.ps1** skriptu.</span><span class="sxs-lookup"><span data-stu-id="734f9-150">Note that hello action is executing hello **Deploy-AzureResourceGroup.ps1** script.</span></span>
   
   ![Výstup – okno][output_window]

## <a name="exploring-your-virtual-machine-scale-set"></a><span data-ttu-id="734f9-152">Prohlížení vaší Škálovací sadu virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="734f9-152">Exploring your Virtual Machine Scale Set</span></span>
<span data-ttu-id="734f9-153">Po dokončení nasazení hello můžete zobrazit hello novou sadu škálování virtuálního počítače v sadě Visual Studio hello **Průzkumník cloudu** (aktualizace seznamu hello).</span><span class="sxs-lookup"><span data-stu-id="734f9-153">Once hello deployment completes, you can view hello new Virtual Machine Scale Set in hello Visual Studio **Cloud Explorer** (refresh hello list).</span></span> <span data-ttu-id="734f9-154">Průzkumník cloudu umožňuje spravovat prostředky Azure v sadě Visual Studio při vývoji aplikace.</span><span class="sxs-lookup"><span data-stu-id="734f9-154">Cloud Explorer lets you manage Azure resources in Visual Studio while developing applications.</span></span> <span data-ttu-id="734f9-155">Můžete také zobrazit sadu škálování virtuálního počítače v hello [portál Azure](https://portal.azure.com) a [Průzkumníka prostředků Azure](https://resources.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="734f9-155">You can also view your Virtual Machine Scale Set in hello [Azure portal](https://portal.azure.com) and [Azure Resource Explorer](https://resources.azure.com/).</span></span>

![Průzkumník cloudu][cloud_explorer]

 <span data-ttu-id="734f9-157">portál Hello poskytuje nejlepší způsob hello toovisually spravovat infrastrukturu Azure s webovým prohlížečem, zatímco Průzkumníka prostředků Azure poskytuje tooexplore snadný způsob a ladění prostředky Azure, udělíte okno do hello "zobrazit instance" a také zobrazuje prostředí PowerShell příkazy pro hello prostředky, který hledáte v.</span><span class="sxs-lookup"><span data-stu-id="734f9-157">hello portal provides hello best way toovisually manage your Azure infrastructure with a web browser, while Azure Resource Explorer provides an easy way tooexplore and debug Azure resources, giving a window into hello "instance view" and also showing PowerShell commands for hello resources you are looking at.</span></span>

## <a name="next-steps"></a><span data-ttu-id="734f9-158">Další kroky</span><span class="sxs-lookup"><span data-stu-id="734f9-158">Next steps</span></span>
<span data-ttu-id="734f9-159">Jakmile jste úspěšně nasazena sady škálování virtuálního počítače pomocí sady Visual Studio, můžete dále přizpůsobit vašeho projektu toosuit vaše požadavky aplikací.</span><span class="sxs-lookup"><span data-stu-id="734f9-159">Once you’ve successfully deployed Virtual Machine Scale Sets through Visual Studio, you can further customize your project toosuit your application requirements.</span></span> <span data-ttu-id="734f9-160">Můžete například nakonfigurovat automatické škálování přidáním **Insights** prostředků, přidávání infrastruktury tooyour šablony (jako samostatné virtuální počítače) nebo nasazování aplikací pomocí rozšíření vlastních skriptů hello.</span><span class="sxs-lookup"><span data-stu-id="734f9-160">For example, configure auto-scale by adding an **Insights** resource, adding infrastructure tooyour Template (like standalone VMs), or deploying applications using hello custom script extension.</span></span> <span data-ttu-id="734f9-161">Dobrým příkladem šablony lze nalézt v hello [šablon Azure rychlý Start](https://github.com/Azure/azure-quickstart-templates) úložiště GitHub (vyhledejte "vmss").</span><span class="sxs-lookup"><span data-stu-id="734f9-161">Good example templates can be found in hello [Azure Quickstart Templates](https://github.com/Azure/azure-quickstart-templates) GitHub repository (search for "vmss").</span></span>

[file_new]: ./media/virtual-machine-scale-sets-vs-create/1-FileNew.png
[create_project]: ./media/virtual-machine-scale-sets-vs-create/2-CreateProject.png
[select_Template]: ./media/virtual-machine-scale-sets-vs-create/3b-SelectTemplateLin.png
[solution_explorer]: ./media/virtual-machine-scale-sets-vs-create/4-SolutionExplorer.png
[json_explorer]: ./media/virtual-machine-scale-sets-vs-create/10-JsonExplorer.png
[5deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/5-DeployTemplate.png
[6deploy_Template]: ./media/virtual-machine-scale-sets-vs-create/6-DeployTemplate.png
[new_resource]: ./media/virtual-machine-scale-sets-vs-create/7-NewResourceGroup.png
[edit_parameters]: ./media/virtual-machine-scale-sets-vs-create/8-EditParameter.png
[output_window]: ./media/virtual-machine-scale-sets-vs-create/9-Output.png
[cloud_explorer]: ./media/virtual-machine-scale-sets-vs-create/12-CloudExplorer.png
