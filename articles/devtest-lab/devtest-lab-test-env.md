---
title: "Pro virtuální počítač a PaaS testovací prostředí použít Azure DevTest Labs | Microsoft Docs"
description: "Naučte se používat Azure DevTest Labs pro virtuální počítač a PaaS scénáře testovacího prostředí."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: d4e2c334-643a-40c9-9051-625b8f39fc86
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: tarcher
ms.openlocfilehash: a556cee9d7b665cd7df23c97e7e2c8c2afabbe58
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="use-azure-devtest-labs-for-vm-and-paas-test-environments"></a><span data-ttu-id="bd9ec-103">Použití Azure DevTest Labs pro virtuální počítač a PaaS testovací prostředí</span><span class="sxs-lookup"><span data-stu-id="bd9ec-103">Use Azure DevTest Labs for VM and PaaS test environments</span></span>

<span data-ttu-id="bd9ec-104">Azure DevTest Labs můžete použít k implementaci mnoho klíčových scénářů, ale primární scénář zahrnuje použití DevTest Labs pro hostování počítačů pro testerům, sada.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-104">You can use Azure DevTest Labs to implement many key scenarios, but a primary scenario involves using DevTest Labs to host machines for testers.</span></span> 

<span data-ttu-id="bd9ec-105">V tomto scénáři DevTest Labs poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="bd9ec-105">In this scenario, DevTest Labs provides these benefits:</span></span>

- <span data-ttu-id="bd9ec-106">Testery můžete otestovat nejnovější verze jejich aplikace rychle zřizování prostředí systému Windows a Linux pomocí opakovaně použitelné šablony a artefaktů.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-106">Testers can test the latest version of their application by quickly provisioning Windows and Linux environments using reusable templates and artifacts.</span></span>
- <span data-ttu-id="bd9ec-107">Testery můžete postupně škálovat jejich zatížení testování zřizování více testovacích agentů.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-107">Testers can scale up their load testing by provisioning multiple test agents.</span></span>
- <span data-ttu-id="bd9ec-108">Správci můžou řídit náklady, přičemž zajistí, aby:</span><span class="sxs-lookup"><span data-stu-id="bd9ec-108">Administrators can control costs by ensuring that:</span></span>
  - <span data-ttu-id="bd9ec-109">Testery nelze získat více virtuálních počítačů, než potřebují.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-109">Testers cannot get more VMs than they need.</span></span>
  - <span data-ttu-id="bd9ec-110">Virtuální počítače jsou vypnutí při není používán.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-110">VMs are shut down when not in use.</span></span>

![Používá DevTest Labs pro školení](./media/devtest-lab-developer-lab/devtest-lab-developer-lab.png)

<span data-ttu-id="bd9ec-112">V tomto článku se dozvíte o různých funkcí Azure DevTest Labs použít ke splnění tester požadavky a podrobné kroky použijte k nastavení testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-112">In this article, you learn about various Azure DevTest Labs features used to meet tester requirements and the detailed steps to follow to set up a lab.</span></span>

## <a name="implementing-test-environments-with-azure-devtest-labs"></a><span data-ttu-id="bd9ec-113">Implementace testovacích prostředích s Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="bd9ec-113">Implementing test environments with Azure DevTest Labs</span></span>
1. <span data-ttu-id="bd9ec-114">**Vytvořit testovací prostředí**</span><span class="sxs-lookup"><span data-stu-id="bd9ec-114">**Create the lab**</span></span> 
   
    <span data-ttu-id="bd9ec-115">Labs jsou výchozím bodem v Azure DevTest Labs.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-115">Labs are the starting point in Azure DevTest Labs.</span></span> <span data-ttu-id="bd9ec-116">Po vytvoření testovacího prostředí, můžete provádět úlohy, jako je například přidávání uživatelů (testery) do testovacího prostředí, nastavení zásad pro řízení nákladů, definování Image virtuálních počítačů, které můžete rychle vytvořit a další.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-116">Once you create a lab, you can perform tasks such as adding users (testers) to the lab, setting policies to control costs, defining VM images that can create quickly, and more.</span></span>  
   
    <span data-ttu-id="bd9ec-117">Další informace kliknutím na odkazy v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="bd9ec-117">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="bd9ec-118">Úkol</span><span class="sxs-lookup"><span data-stu-id="bd9ec-118">Task</span></span> | <span data-ttu-id="bd9ec-119">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="bd9ec-119">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="bd9ec-120">Vytvoření testovacího prostředí v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="bd9ec-120">Create a lab in Azure DevTest Labs</span></span>](devtest-lab-create-lab.md) |<span data-ttu-id="bd9ec-121">Postup vytvoření testovacího prostředí v Azure DevTest Labs na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-121">Learn how to create a lab in Azure DevTest Labs in the Azure portal.</span></span> |
2. <span data-ttu-id="bd9ec-122">**Vytvořit virtuální počítače během pár minut pomocí připravených marketplace Image a vlastních bitových kopií**</span><span class="sxs-lookup"><span data-stu-id="bd9ec-122">**Create VMs in minutes using ready-made marketplace images and custom images**</span></span> 
   
    <span data-ttu-id="bd9ec-123">Můžete vybrat připravených bitové kopie z široké škály bitové kopie v Azure Marketplace a zpřístupnit v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-123">You can pick ready-made images from a wide variety of images in the Azure Marketplace and make them available in the lab.</span></span> <span data-ttu-id="bd9ec-124">Jestliže připravených bitové kopie nevyhovují vašim požadavkům, můžete vytvořit vlastní image vytvořením testovacího prostředí virtuálních počítačů pomocí připravených bitové kopie z Azure Marketplace, veškerý software, který budete potřebovat, instalace a uložení virtuálního počítače jako vlastní image v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-124">If the ready-made images don't meet your requirements, you can create a custom image by creating a lab VM using a ready-made image from Azure Marketplace, installing all the software that you need, and saving the VM as a custom image in the lab.</span></span>

    <span data-ttu-id="bd9ec-125">Pokud budete používat vlastní Image, zvažte použití objekt pro vytváření bitové kopie k vytvoření a distribuce obrázků.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-125">If you will be using custom images, consider using an image factory to create and distribute your images.</span></span> <span data-ttu-id="bd9ec-126">Objekt pro vytváření bitové kopie je jako kód konfigurace řešení, které pravidelně sestavení a automaticky distribuuje nakonfigurované obrázků.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-126">An image factory is a configuration-as-code solution that regularly builds and distributes your configured images automatically.</span></span> <span data-ttu-id="bd9ec-127">To umožňuje ušetřit čas potřebný k ručně konfigurovat systém po vytvoření virtuálního počítače se základní operačního systému.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-127">This saves the time required to manually configure the system after a VM has been created with the base OS.</span></span>
  
    <span data-ttu-id="bd9ec-128">Další informace kliknutím na odkazy v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="bd9ec-128">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="bd9ec-129">Úkol</span><span class="sxs-lookup"><span data-stu-id="bd9ec-129">Task</span></span> | <span data-ttu-id="bd9ec-130">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="bd9ec-130">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="bd9ec-131">Konfigurace Azure Marketplace obrázků</span><span class="sxs-lookup"><span data-stu-id="bd9ec-131">Configure Azure Marketplace images</span></span>](devtest-lab-configure-marketplace-images.md) |<span data-ttu-id="bd9ec-132">Přečtěte si, jak povolených Image Azure Marketplace, zpřístupnění pro výběr pouze obrázky, které chcete použít pro testery.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-132">Learn how you can whitelist Azure Marketplace images, making available for selection only the images you want for the testers.</span></span>|
   | [<span data-ttu-id="bd9ec-133">Vytvořit vlastní image</span><span class="sxs-lookup"><span data-stu-id="bd9ec-133">Create a custom image</span></span>](devtest-lab-create-template.md) |<span data-ttu-id="bd9ec-134">Vytvořte vlastní image před instalací softwaru, které potřebujete, aby testery dokáže rychle vytvořit virtuální počítač pomocí vlastní image.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-134">Create a custom image by pre-installing the software you need so that testers can quickly create a VM using the custom image.</span></span>|
   | [<span data-ttu-id="bd9ec-135">Další informace o vytváření bitové kopie</span><span class="sxs-lookup"><span data-stu-id="bd9ec-135">Learn about image factory</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/) |<span data-ttu-id="bd9ec-136">Přehrát video, které popisuje, jak nastavit a používat objekt pro vytváření bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-136">Watch a video that describes how to set up and use an image factory.</span></span>|

3. <span data-ttu-id="bd9ec-137">**Vytvoření šablon opakovaně použitelné pro testovacích počítačů**</span><span class="sxs-lookup"><span data-stu-id="bd9ec-137">**Create reusable templates for test machines**</span></span> 
   
    <span data-ttu-id="bd9ec-138">Vzorec v Azure DevTest Labs je seznam výchozí hodnoty vlastností použít k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-138">A formula in Azure DevTest Labs is a list of default property values used to create a VM.</span></span> <span data-ttu-id="bd9ec-139">Vzorec v testovacím prostředí vytvoříte výběr obrázku, velikost virtuálního počítače (kombinaci procesor a paměť RAM) a virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-139">You can create a formula in the lab by picking an image, a VM size (a combination of CPU and RAM), and a virtual network.</span></span> <span data-ttu-id="bd9ec-140">Každý tester můžete zobrazit vzorec v testovacím prostředí a ji použít k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-140">Each tester can see the formula in the lab and use it to create a VM.</span></span> 
   
    <span data-ttu-id="bd9ec-141">Další informace kliknutím na odkazy v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="bd9ec-141">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="bd9ec-142">Úkol</span><span class="sxs-lookup"><span data-stu-id="bd9ec-142">Task</span></span> | <span data-ttu-id="bd9ec-143">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="bd9ec-143">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="bd9ec-144">Spravovat DevTest Labs vzorce pro vytvoření virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="bd9ec-144">Manage DevTest Labs formulas to create VMs</span></span>](devtest-lab-manage-formulas.md) |<span data-ttu-id="bd9ec-145">Zjistěte, jak můžete vytvořit vzorec podle vyzvednutí obrázku, velikost virtuálního počítače (kombinaci procesor a paměť RAM) a virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-145">Learn how you can create a formula by picking up an image, VM size (combination of CPU and RAM), and a virtual network.</span></span>|

3. <span data-ttu-id="bd9ec-146">**Vytvoření více virtuálních počítačů testovací prostředí**</span><span class="sxs-lookup"><span data-stu-id="bd9ec-146">**Create multi-VM test environments**</span></span> 
   
    <span data-ttu-id="bd9ec-147">Šablony Azure Resource Manager můžete použít k definování infrastrukturu a konfiguraci řešení Azure a opakovaně nasadit více testovací virtuální počítače v konzistentním stavu.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-147">You can use Azure Resource Manager templates to define the infrastructure and configuration of your Azure solution and repeatedly deploy multiple test VMs in a consistent state.</span></span>

    <span data-ttu-id="bd9ec-148">Prostředky Azure PaaS může být zřízen v prostředí z šablony Resource Manageru a zobrazí v sledování nákladů.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-148">Azure PaaS resources can be provisioned in an environment from a Resource Manager template and appear in cost tracking.</span></span> <span data-ttu-id="bd9ec-149">Vypnutí automatického virtuálních počítačů však nevztahuje na PaaS prostředky.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-149">However, VM auto shutdown does not apply to PaaS resources.</span></span>

    <span data-ttu-id="bd9ec-150">Další informace kliknutím na odkazy v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="bd9ec-150">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="bd9ec-151">Úkol</span><span class="sxs-lookup"><span data-stu-id="bd9ec-151">Task</span></span> | <span data-ttu-id="bd9ec-152">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="bd9ec-152">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="bd9ec-153">Vytvoření prostředí více virtuálních počítačů a prostředků PaaS pomocí šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="bd9ec-153">Create multi-VM environments and PaaS resources with Azure Resource Manager templates</span></span>](devtest-lab-create-environment-from-arm.md) |<span data-ttu-id="bd9ec-154">Zjistěte, jak můžete nasadit víc virtuálních počítačů v konzistentním stavu pro testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-154">Learn how you can deploy multiple VMs in a consistent state for your test environment.</span></span>|

4. <span data-ttu-id="bd9ec-155">**Vytvoří artefakty za účelem flexibilní přizpůsobení virtuálního počítače**</span><span class="sxs-lookup"><span data-stu-id="bd9ec-155">**Create artifacts to enable flexible VM customization**</span></span>

   <span data-ttu-id="bd9ec-156">Artefakty slouží k nasazení a konfiguraci aplikace po zřízení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-156">Artifacts are used to deploy and configure your application after a VM is provisioned.</span></span> <span data-ttu-id="bd9ec-157">Artefakty mohou být:</span><span class="sxs-lookup"><span data-stu-id="bd9ec-157">Artifacts can be:</span></span>

   - <span data-ttu-id="bd9ec-158">Nástroje, které chcete nainstalovat do virtuálního počítače – například agentů, Fiddler a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-158">Tools that you want to install on the VM - such as agents, Fiddler, and Visual Studio.</span></span>
   - <span data-ttu-id="bd9ec-159">Akce, které chcete spustit na virtuálním počítači – například klonování úložišti.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-159">Actions that you want to run on the VM - such as cloning a repo.</span></span>
   - <span data-ttu-id="bd9ec-160">Aplikace, které chcete otestovat</span><span class="sxs-lookup"><span data-stu-id="bd9ec-160">Applications that you want to test.</span></span>

   <span data-ttu-id="bd9ec-161">Mnoho artefakty je již k dispozici out-of-the-box.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-161">Many artifacts are already available out-of-the-box.</span></span> <span data-ttu-id="bd9ec-162">Ale pokud chcete další přizpůsobení svých konkrétních potřeb, můžete vytvořit vlastní artefakty.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-162">But if you want more customization for your specific needs, you can create your own custom artifacts.</span></span>

   <span data-ttu-id="bd9ec-163">Další informace kliknutím na odkazy v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="bd9ec-163">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="bd9ec-164">Úkol</span><span class="sxs-lookup"><span data-stu-id="bd9ec-164">Task</span></span> | <span data-ttu-id="bd9ec-165">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="bd9ec-165">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="bd9ec-166">Vytvoření vlastních artefaktů pro virtuální počítač DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="bd9ec-166">Create custom artifacts for your DevTest Labs VM</span></span>](devtest-lab-artifact-author.md) |<span data-ttu-id="bd9ec-167">Vytvoření vlastních artefaktů pro virtuální počítače ve vašem testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-167">Create your own custom artifacts for the virtual machines in your lab.</span></span>|
   | [<span data-ttu-id="bd9ec-168">Přidejte úložiště Git pro uložení vlastních artefaktů a šablon Azure Resource Manageru pro použití v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="bd9ec-168">Add a Git repository to store custom artifacts and Azure Resource Manager templates for use in Azure DevTest Labs</span></span>](devtest-lab-add-artifact-repo.md) |<span data-ttu-id="bd9ec-169">Zjistěte, jak k uložení vlastních artefaktů v vlastní privátní úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-169">Learn how to store your custom artifacts in your own private Git repo.</span></span>|

5. <span data-ttu-id="bd9ec-170">**Náklady na ovládací prvek**</span><span class="sxs-lookup"><span data-stu-id="bd9ec-170">**Control costs**</span></span>
   
    <span data-ttu-id="bd9ec-171">Azure DevTest Labs umožňuje nastavit zásady v testovacím prostředí, chcete-li určit maximální počet virtuálních počítačů, které lze vytvořit pomocí nástroje testování v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-171">Azure DevTest Labs allows you to set a policy in the lab to specify the maximum number of VMs that can be created by a tester in the lab.</span></span> 
   
    <span data-ttu-id="bd9ec-172">Pokud má váš tým testovací sadu pracovní plán a chcete zastavit všechny virtuální počítače v určitém čase dne a poté automaticky restartovat je následující den, který lze snadno provádět nastavením zásad, automatické ukončení a automaticky spouštěná v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-172">If your test team has a set work schedule and you want to stop all the VMs at a particular time of the day and then automatically restart them the following day, you can easily accomplish that by setting auto-shutdown and auto-start policies in the lab.</span></span> 
   
    <span data-ttu-id="bd9ec-173">Nakonec po dokončení vývoj aplikací můžete odstranit všechny virtuální počítače najednou spuštěním jednoho skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-173">Finally, when app development is complete, you can delete all the VMs at once by running a single PowerShell script.</span></span> 
   
    <span data-ttu-id="bd9ec-174">Další informace kliknutím na odkazy v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="bd9ec-174">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="bd9ec-175">Úkol</span><span class="sxs-lookup"><span data-stu-id="bd9ec-175">Task</span></span> | <span data-ttu-id="bd9ec-176">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="bd9ec-176">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="bd9ec-177">Definice zásad testovacího prostředí</span><span class="sxs-lookup"><span data-stu-id="bd9ec-177">Define lab policies</span></span>](devtest-lab-set-lab-policy.md) |<span data-ttu-id="bd9ec-178">Řízení nákladů pomocí nastavení zásad v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-178">Control costs by setting policies in the lab.</span></span> |
   | [<span data-ttu-id="bd9ec-179">Odstranit všechny testovací prostředí virtuálních počítačů pomocí skriptu prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="bd9ec-179">Delete all the lab VMs using a PowerShell script</span></span>](devtest-lab-faq.md#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |<span data-ttu-id="bd9ec-180">Po dokončení testování, odstraňte všechny labs v rámci jedné operace.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-180">Delete all the labs in one operation when testing is complete.</span></span>|

1. <span data-ttu-id="bd9ec-181">**Přidat virtuální síť do testovacího prostředí**</span><span class="sxs-lookup"><span data-stu-id="bd9ec-181">**Add a virtual network to a Lab**</span></span> 
   
    <span data-ttu-id="bd9ec-182">DevTest Labs vytvoří novou virtuální síť (VNET) vždy, když je vytvořena testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-182">DevTest Labs creates a new virtual network (VNET) whenever a lab is created.</span></span> <span data-ttu-id="bd9ec-183">Pokud jste nakonfigurovali vlastní virtuální síť – například s použitím ExpressRoute nebo VPN typu site-to-site – přidáním této virtuální sítě na nastavení virtuální sítě testovacího prostředí tak, aby bylo dostupné při vytváření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-183">If you have configured your own VNET – for example, by using ExpressRoute or site-to-site VPN – you can add this VNET to your lab's virtual network settings so that it is available when creating VMs.</span></span>

    <span data-ttu-id="bd9ec-184">Kromě toho je Azure Active Directory domény spojení artefakt k dispozici, virtuální počítač připojí k doméně, při vytváření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-184">In addition, there is an Azure Active Directory domain join artifact available that joins a VM to a domain when the VM is being created.</span></span> 
   
    <span data-ttu-id="bd9ec-185">Další informace kliknutím na odkazy v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="bd9ec-185">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="bd9ec-186">Úkol</span><span class="sxs-lookup"><span data-stu-id="bd9ec-186">Task</span></span> | <span data-ttu-id="bd9ec-187">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="bd9ec-187">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="bd9ec-188">Konfigurace virtuální sítě v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="bd9ec-188">Configure a virtual network in Azure DevTest Labs</span></span>](devtest-lab-configure-vnet.md) |<span data-ttu-id="bd9ec-189">Informace o konfiguraci virtuální sítě v Azure DevTest Labs pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-189">Learn how to configure a virtual network in Azure DevTest Labs using the Azure portal.</span></span>|

6. <span data-ttu-id="bd9ec-190">**Sdílet testovací prostředí s každou tester**</span><span class="sxs-lookup"><span data-stu-id="bd9ec-190">**Share the lab with each tester**</span></span>
   
    <span data-ttu-id="bd9ec-191">Labs lze přímo přistupovat pomocí odkazu, který sdílíte s vaší testerům, sada.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-191">Labs can be directly accessed using a link that you share with your testers.</span></span> <span data-ttu-id="bd9ec-192">Se dokonce ani nemusí mít účet Azure, tak dlouho, dokud mají [účtu Microsoft](devtest-lab-faq.md#what-is-a-microsoft-account).</span><span class="sxs-lookup"><span data-stu-id="bd9ec-192">They don't even have to have an Azure account, as long as they have a [Microsoft account](devtest-lab-faq.md#what-is-a-microsoft-account).</span></span> <span data-ttu-id="bd9ec-193">Testery neuvidí vytvořit další testerům, sada virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-193">Testers cannot see VMs created by other testers.</span></span>  
   
    <span data-ttu-id="bd9ec-194">Další informace kliknutím na odkazy v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="bd9ec-194">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="bd9ec-195">Úkol</span><span class="sxs-lookup"><span data-stu-id="bd9ec-195">Task</span></span> | <span data-ttu-id="bd9ec-196">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="bd9ec-196">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="bd9ec-197">Přidání tester do testovacího prostředí v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="bd9ec-197">Add a tester to a lab in Azure DevTest Labs</span></span>](devtest-lab-add-devtest-user.md) |<span data-ttu-id="bd9ec-198">Pomocí portálu Azure přidejte testery do vašeho testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-198">Use the Azure portal to add testers to your lab.</span></span>|
   | [<span data-ttu-id="bd9ec-199">Přidat testery do testovacího prostředí pomocí skriptu prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="bd9ec-199">Add testers to the lab using a PowerShell script</span></span>](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |<span data-ttu-id="bd9ec-200">Použití Powershellu k automatizaci přidání testery do vašeho testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-200">Use PowerShell to automate adding testers to your lab.</span></span> |
   | [<span data-ttu-id="bd9ec-201">Získejte odkaz do testovacího prostředí</span><span class="sxs-lookup"><span data-stu-id="bd9ec-201">Get a link to the lab</span></span>](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |<span data-ttu-id="bd9ec-202">Zjistěte, jak testery přímo přistupovat k testovacím prostředí pomocí hypertextový odkaz.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-202">Learn how testers can directly access a lab via a hyperlink.</span></span>|

7. <span data-ttu-id="bd9ec-203">**Automatizovat vytvoření testovacího prostředí pro více týmů**</span><span class="sxs-lookup"><span data-stu-id="bd9ec-203">**Automate lab creation for more teams**</span></span> 
   
    <span data-ttu-id="bd9ec-204">Je možné automatizovat vytvoření testovacího prostředí, včetně vlastních nastavení, vytvořením šablony Resource Manageru a jej použijete k vytvoření identické labs znovu a znovu.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-204">You can automate lab creation, including custom settings, by creating a Resource Manager template and using it to create identical labs again and again.</span></span> 
   
    <span data-ttu-id="bd9ec-205">Další informace kliknutím na odkazy v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="bd9ec-205">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="bd9ec-206">Úkol</span><span class="sxs-lookup"><span data-stu-id="bd9ec-206">Task</span></span> | <span data-ttu-id="bd9ec-207">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="bd9ec-207">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="bd9ec-208">Vytvoření testovacího prostředí pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="bd9ec-208">Create a lab using a Resource Manager template</span></span>](devtest-lab-faq.md#how-do-i-create-a-lab-from-an-azure-resource-manager-template) |<span data-ttu-id="bd9ec-209">Vytvoření prostředí v Azure DevTest Labs pomocí šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="bd9ec-209">Create labs in Azure DevTest Labs using Resource Manager templates.</span></span> |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

