---
title: "Použití Azure DevTest Labs pro vývojáře | Microsoft Docs"
description: "Naučte se používat Azure DevTest Labs pro vývojáře scénáře."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 22e070e5-3d1a-49fe-9d4c-5e07cb0b7fe2
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: tarcher
ms.openlocfilehash: c187819e9392908c8979556f80e8c94739eb14d5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-devtest-labs-for-developers"></a><span data-ttu-id="d11f0-103">Použití Azure DevTest Labs pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="d11f0-103">Use Azure DevTest Labs for developers</span></span>
<span data-ttu-id="d11f0-104">Azure DevTest Labs lze použít k implementaci mnoho klíčových scénářů, ale jeden primární scénářů, která využívá DevTest Labs hostovat vývoj počítače pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="d11f0-104">Azure DevTest Labs can be used to implement many key scenarios, but one of the primary scenarios involves using DevTest Labs to host development machines for developers.</span></span> <span data-ttu-id="d11f0-105">V tomto scénáři DevTest Labs poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="d11f0-105">In this scenario, DevTest Labs provides these benefits:</span></span>

- <span data-ttu-id="d11f0-106">Vývojáři mohou rychle zřizovat jejich vývoj počítačů na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="d11f0-106">Developers can quickly provision their development machines on demand.</span></span>
- <span data-ttu-id="d11f0-107">Vývojáři mohou snadno přizpůsobit své počítače vývoj vždy, když je potřeba.</span><span class="sxs-lookup"><span data-stu-id="d11f0-107">Developers can easily customize their development machines whenever needed.</span></span>
- <span data-ttu-id="d11f0-108">Správci můžou řídit náklady, přičemž zajistí, aby:</span><span class="sxs-lookup"><span data-stu-id="d11f0-108">Administrators can control costs by ensuring that:</span></span>
  - <span data-ttu-id="d11f0-109">Vývojáři nelze získat více virtuálních počítačů, než potřebují pro vývoj.</span><span class="sxs-lookup"><span data-stu-id="d11f0-109">Developers cannot get more VMs than they need for development.</span></span>
  - <span data-ttu-id="d11f0-110">Virtuální počítače jsou vypnutí při není používán.</span><span class="sxs-lookup"><span data-stu-id="d11f0-110">VMs are shut down when not in use.</span></span> 

![Používá DevTest Labs pro školení](./media/devtest-lab-developer-lab/devtest-lab-developer-lab.png)

<span data-ttu-id="d11f0-112">V tomto článku se dozvíte o různých funkcí Azure DevTest Labs, které lze použít ke splnění požadavků na vývojáře a podrobné kroky, které vám pomůžou nastavit testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="d11f0-112">In this article, you learn about various Azure DevTest Labs features that can be used to meet developer requirements and the detailed steps that you can follow to set up a lab.</span></span>

## <a name="implementing-developer-environments-with-azure-devtest-labs"></a><span data-ttu-id="d11f0-113">Implementace vývojáře prostředí s Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="d11f0-113">Implementing developer environments with Azure DevTest Labs</span></span>
1. <span data-ttu-id="d11f0-114">**Vytvořit testovací prostředí**</span><span class="sxs-lookup"><span data-stu-id="d11f0-114">**Create the lab**</span></span> 
   
    <span data-ttu-id="d11f0-115">Labs jsou výchozím bodem v Azure DevTest Labs.</span><span class="sxs-lookup"><span data-stu-id="d11f0-115">Labs are the starting point in Azure DevTest Labs.</span></span> <span data-ttu-id="d11f0-116">Po vytvoření testovacího prostředí, můžete provádět úlohy, jako je například přidávání uživatelů (vývojáři) do testovacího prostředí, nastavení zásad pro řízení nákladů, definování Image virtuálních počítačů, které můžete rychle vytvořit a další.</span><span class="sxs-lookup"><span data-stu-id="d11f0-116">Once you create a lab, you can perform tasks such as adding users (developers) to the lab, setting policies to control costs, defining VM images that can create quickly, and more.</span></span>  
   
    <span data-ttu-id="d11f0-117">Další informace kliknutím na odkazy v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="d11f0-117">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="d11f0-118">Úkol</span><span class="sxs-lookup"><span data-stu-id="d11f0-118">Task</span></span> | <span data-ttu-id="d11f0-119">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="d11f0-119">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="d11f0-120">Vytvoření testovacího prostředí v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="d11f0-120">Create a lab in Azure DevTest Labs</span></span>](devtest-lab-create-lab.md) |<span data-ttu-id="d11f0-121">Postup vytvoření testovacího prostředí v Azure DevTest Labs na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d11f0-121">Learn how to create a lab in Azure DevTest Labs in the Azure portal.</span></span> |
2. <span data-ttu-id="d11f0-122">**Vytvořit virtuální počítače během pár minut pomocí připravených marketplace Image a vlastních bitových kopií**</span><span class="sxs-lookup"><span data-stu-id="d11f0-122">**Create VMs in minutes using ready-made marketplace images and custom images**</span></span> 
   
    <span data-ttu-id="d11f0-123">Můžete vybrat připravených bitové kopie z široké škály bitové kopie v Azure Marketplace a zpřístupnit v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d11f0-123">You can pick ready-made images from a wide variety of images in the Azure Marketplace and make them available in the lab.</span></span> <span data-ttu-id="d11f0-124">Jestliže připravených bitové kopie nevyhovují vašim požadavkům, můžete vytvořit vlastní image vytvořením testovacího prostředí virtuálních počítačů pomocí připravených bitové kopie z Azure Marketplace, veškerý software, který budete potřebovat, instalace a uložení virtuálního počítače jako vlastní image v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d11f0-124">If the ready-made images don't meet your requirements, you can create a custom image by creating a lab VM using a ready-made image from Azure Marketplace, installing all the software that you need, and saving the VM as a custom image in the lab.</span></span>

    <span data-ttu-id="d11f0-125">Pokud budete používat vlastní Image, zvažte použití objekt pro vytváření bitové kopie k vytvoření a distribuce obrázků.</span><span class="sxs-lookup"><span data-stu-id="d11f0-125">If you will be using custom images, consider using an image factory to create and distribute your images.</span></span> <span data-ttu-id="d11f0-126">Objekt pro vytváření bitové kopie je jako kód konfigurace řešení, které pravidelně sestavení a automaticky distribuuje nakonfigurované obrázků.</span><span class="sxs-lookup"><span data-stu-id="d11f0-126">An image factory is a configuration-as-code solution that regularly builds and distributes your configured images automatically.</span></span> <span data-ttu-id="d11f0-127">To umožňuje ušetřit čas potřebný k ručně konfigurovat systém po vytvoření virtuálního počítače se základní operačního systému.</span><span class="sxs-lookup"><span data-stu-id="d11f0-127">This saves the time required to manually configure the system after a VM has been created with the base OS.</span></span>
  
    <span data-ttu-id="d11f0-128">Další informace kliknutím na odkazy v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="d11f0-128">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="d11f0-129">Úkol</span><span class="sxs-lookup"><span data-stu-id="d11f0-129">Task</span></span> | <span data-ttu-id="d11f0-130">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="d11f0-130">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="d11f0-131">Konfigurace Azure Marketplace obrázků</span><span class="sxs-lookup"><span data-stu-id="d11f0-131">Configure Azure Marketplace images</span></span>](devtest-lab-configure-marketplace-images.md) |<span data-ttu-id="d11f0-132">Přečtěte si, jak povolených Image Azure Marketplace, zpřístupnění pro výběr pouze obrázky, které chcete použít pro vývojáři.</span><span class="sxs-lookup"><span data-stu-id="d11f0-132">Learn how you can whitelist Azure Marketplace images, making available for selection only the images you want for the developers.</span></span>|
   | [<span data-ttu-id="d11f0-133">Vytvořit vlastní image</span><span class="sxs-lookup"><span data-stu-id="d11f0-133">Create a custom image</span></span>](devtest-lab-create-template.md) |<span data-ttu-id="d11f0-134">Vytvořte vlastní image před instalací softwaru, které potřebujete, aby vývojáři mohou rychle vytvořit virtuální počítač pomocí vlastní image.</span><span class="sxs-lookup"><span data-stu-id="d11f0-134">Create a custom image by pre-installing the software you need so that developers can quickly create a VM using the custom image.</span></span>|
   | [<span data-ttu-id="d11f0-135">Další informace o vytváření bitové kopie</span><span class="sxs-lookup"><span data-stu-id="d11f0-135">Learn about image factory</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/) |<span data-ttu-id="d11f0-136">Přehrát video, které popisuje, jak nastavit a používat objekt pro vytváření bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="d11f0-136">Watch a video that describes how to set up and use an image factory.</span></span>|

3. <span data-ttu-id="d11f0-137">**Vytváření opakovaně použitelných šablon pro vývojáře počítače**</span><span class="sxs-lookup"><span data-stu-id="d11f0-137">**Create reusable templates for developer machines**</span></span> 
   
    <span data-ttu-id="d11f0-138">Vzorec v Azure DevTest Labs je seznam výchozí hodnoty vlastností použít k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d11f0-138">A formula in Azure DevTest Labs is a list of default property values used to create a VM.</span></span> <span data-ttu-id="d11f0-139">Vzorec v testovacím prostředí vytvoříte výběr obrázku, velikost virtuálního počítače (kombinaci procesor a paměť RAM) a virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d11f0-139">You can create a formula in the lab by picking an image, a VM size (a combination of CPU and RAM), and a virtual network.</span></span> <span data-ttu-id="d11f0-140">Každý vývojář může zobrazit vzorec v testovacím prostředí a ji použít k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d11f0-140">Each developer can see the formula in the lab and use it to create a VM.</span></span> 
   
    <span data-ttu-id="d11f0-141">Další informace kliknutím na odkazy v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="d11f0-141">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="d11f0-142">Úkol</span><span class="sxs-lookup"><span data-stu-id="d11f0-142">Task</span></span> | <span data-ttu-id="d11f0-143">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="d11f0-143">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="d11f0-144">Spravovat DevTest Labs vzorce pro vytvoření virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="d11f0-144">Manage DevTest Labs formulas to create VMs</span></span>](devtest-lab-manage-formulas.md) |<span data-ttu-id="d11f0-145">Zjistěte, jak můžete vytvořit vzorec podle vyzvednutí obrázku, velikost virtuálního počítače (kombinaci procesor a paměť RAM) a virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d11f0-145">Learn how you can create a formula by picking up an image, VM size (combination of CPU and RAM), and a virtual network.</span></span>|

4. <span data-ttu-id="d11f0-146">**Vytvoří artefakty za účelem flexibilní přizpůsobení virtuálního počítače**</span><span class="sxs-lookup"><span data-stu-id="d11f0-146">**Create artifacts to enable flexible VM customization**</span></span>

   <span data-ttu-id="d11f0-147">Artefakty slouží k nasazení a konfiguraci aplikace po zřízení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d11f0-147">Artifacts are used to deploy and configure your application after a VM is provisioned.</span></span> <span data-ttu-id="d11f0-148">Artefakty mohou být:</span><span class="sxs-lookup"><span data-stu-id="d11f0-148">Artifacts can be:</span></span>

   - <span data-ttu-id="d11f0-149">Nástroje, které chcete nainstalovat do virtuálního počítače – například agentů, Fiddler a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d11f0-149">Tools that you want to install on the VM - such as agents, Fiddler, and Visual Studio.</span></span>
   - <span data-ttu-id="d11f0-150">Akce, které chcete spustit na virtuálním počítači – například klonování úložišti.</span><span class="sxs-lookup"><span data-stu-id="d11f0-150">Actions that you want to run on the VM - such as cloning a repo.</span></span>
   - <span data-ttu-id="d11f0-151">Aplikace, které chcete otestovat</span><span class="sxs-lookup"><span data-stu-id="d11f0-151">Applications that you want to test.</span></span>

   <span data-ttu-id="d11f0-152">Mnoho artefakty je již k dispozici out-of-the-box.</span><span class="sxs-lookup"><span data-stu-id="d11f0-152">Many artifacts are already available out-of-the-box.</span></span> <span data-ttu-id="d11f0-153">Pokud chcete další přizpůsobení svých konkrétních potřeb můžete vytvořit vlastní artefakty.</span><span class="sxs-lookup"><span data-stu-id="d11f0-153">You can create your own custom artifacts if you want more customization for your specific needs.</span></span>

   <span data-ttu-id="d11f0-154">Další informace kliknutím na odkazy v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="d11f0-154">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="d11f0-155">Úkol</span><span class="sxs-lookup"><span data-stu-id="d11f0-155">Task</span></span> | <span data-ttu-id="d11f0-156">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="d11f0-156">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="d11f0-157">Vytvoření vlastních artefaktů pro virtuální počítač DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="d11f0-157">Create custom artifacts for your DevTest Labs VM</span></span>](devtest-lab-artifact-author.md) |<span data-ttu-id="d11f0-158">Vytvoření vlastních artefaktů pro virtuální počítače ve vašem testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d11f0-158">Create your own custom artifacts for the virtual machines in your lab.</span></span>|
   | [<span data-ttu-id="d11f0-159">Přidejte úložiště Git pro uložení vlastních artefaktů a šablon Azure Resource Manageru pro použití v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="d11f0-159">Add a Git repository to store custom artifacts and Azure Resource Manager templates for use in Azure DevTest Labs</span></span>](devtest-lab-add-artifact-repo.md) |<span data-ttu-id="d11f0-160">Zjistěte, jak k uložení vlastních artefaktů v vlastní privátní úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="d11f0-160">Learn how to store your custom artifacts in your own private Git repo.</span></span>|

5. <span data-ttu-id="d11f0-161">**Náklady na ovládací prvek**</span><span class="sxs-lookup"><span data-stu-id="d11f0-161">**Control costs**</span></span>
   
    <span data-ttu-id="d11f0-162">Azure DevTest Labs umožňuje nastavit zásady v testovacím prostředí, chcete-li určit maximální počet virtuálních počítačů, které lze vytvořit pomocí vývojář v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d11f0-162">Azure DevTest Labs allows you to set a policy in the lab to specify the maximum number of VMs that can be created by a developer in the lab.</span></span> 
   
    <span data-ttu-id="d11f0-163">Pokud má váš tým vývojáře sady pracovní plán a chcete zastavit všechny virtuální počítače v určitém čase dne a poté automaticky restartovat je následující den, který lze snadno provádět nastavením zásad, automatické ukončení a automaticky spouštěná v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d11f0-163">If your developer team has a set work schedule and you want to stop all the VMs at a particular time of the day and then automatically restart them the following day, you can easily accomplish that by setting auto-shutdown and auto-start policies in the lab.</span></span> 
   
    <span data-ttu-id="d11f0-164">Nakonec po dokončení vývoj aplikací můžete odstranit všechny virtuální počítače najednou spuštěním jednoho skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d11f0-164">Finally, when app development is complete, you can delete all the VMs at once by running a single PowerShell script.</span></span> 
   
    <span data-ttu-id="d11f0-165">Další informace kliknutím na odkazy v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="d11f0-165">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="d11f0-166">Úkol</span><span class="sxs-lookup"><span data-stu-id="d11f0-166">Task</span></span> | <span data-ttu-id="d11f0-167">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="d11f0-167">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="d11f0-168">Definice zásad testovacího prostředí</span><span class="sxs-lookup"><span data-stu-id="d11f0-168">Define lab policies</span></span>](devtest-lab-set-lab-policy.md) |<span data-ttu-id="d11f0-169">Řízení nákladů pomocí nastavení zásad v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d11f0-169">Control costs by setting policies in the lab.</span></span> |
   | [<span data-ttu-id="d11f0-170">Odstranit všechny testovací prostředí virtuálních počítačů pomocí skriptu prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d11f0-170">Delete all the lab VMs using a PowerShell script</span></span>](devtest-lab-faq.md#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |<span data-ttu-id="d11f0-171">Po dokončení vývoj, odstraňte všechny labs v rámci jedné operace.</span><span class="sxs-lookup"><span data-stu-id="d11f0-171">Delete all the labs in one operation when development is complete.</span></span>|

1. <span data-ttu-id="d11f0-172">**Přidání virtuální sítě k virtuálnímu počítači**</span><span class="sxs-lookup"><span data-stu-id="d11f0-172">**Add a virtual network to a VM**</span></span> 
   
    <span data-ttu-id="d11f0-173">DevTest Labs vytvoří novou virtuální síť (VNET) vždy, když je vytvořena testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="d11f0-173">DevTest Labs creates a new virtual network (VNET) whenever a lab is created.</span></span> <span data-ttu-id="d11f0-174">Pokud jste nakonfigurovali vlastní virtuální síť – například s použitím ExpressRoute nebo VPN typu site-to-site – přidáním této virtuální sítě na nastavení virtuální sítě testovacího prostředí tak, aby bylo dostupné při vytváření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d11f0-174">If you have configured your own VNET – for example, by using ExpressRoute or site-to-site VPN – you can add this VNET to your lab's virtual network settings so that it is available when creating VMs.</span></span>

    <span data-ttu-id="d11f0-175">Kromě toho je artefakt připojení k doméně služby Azure Active Directory k dispozici při vytvoření virtuálního počítače, který se připojí virtuální počítač k doméně.</span><span class="sxs-lookup"><span data-stu-id="d11f0-175">In addition, there is an Azure Active Directory domain join artifact available that will join a VM to a domain when the VM is being created.</span></span> 
   
    <span data-ttu-id="d11f0-176">Další informace kliknutím na odkazy v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="d11f0-176">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="d11f0-177">Úkol</span><span class="sxs-lookup"><span data-stu-id="d11f0-177">Task</span></span> | <span data-ttu-id="d11f0-178">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="d11f0-178">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="d11f0-179">Konfigurace virtuální sítě v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="d11f0-179">Configure a virtual network in Azure DevTest Labs</span></span>](devtest-lab-configure-vnet.md) |<span data-ttu-id="d11f0-180">Informace o konfiguraci virtuální sítě v Azure DevTest Labs pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d11f0-180">Learn how to configure a virtual network in Azure DevTest Labs using the Azure portal.</span></span>|

6. <span data-ttu-id="d11f0-181">**Sdílet testovací prostředí se každý vývojář**</span><span class="sxs-lookup"><span data-stu-id="d11f0-181">**Share the lab with each developer**</span></span>
   
    <span data-ttu-id="d11f0-182">Labs lze přímo přistupovat pomocí odkazu, který sdílíte s vaší vývojáři.</span><span class="sxs-lookup"><span data-stu-id="d11f0-182">Labs can be directly accessed using a link that you share with your developers.</span></span> <span data-ttu-id="d11f0-183">Se dokonce ani nemusí mít účet Azure, tak dlouho, dokud mají [účtu Microsoft](devtest-lab-faq.md#what-is-a-microsoft-account).</span><span class="sxs-lookup"><span data-stu-id="d11f0-183">They don't even have to have an Azure account, as long as they have a [Microsoft account](devtest-lab-faq.md#what-is-a-microsoft-account).</span></span> <span data-ttu-id="d11f0-184">Vývojáři nelze zobrazit virtuální počítače vytvořené jinými vývojáři.</span><span class="sxs-lookup"><span data-stu-id="d11f0-184">Developers cannot see VMs created by other developers.</span></span>  
   
    <span data-ttu-id="d11f0-185">Další informace kliknutím na odkazy v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="d11f0-185">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="d11f0-186">Úkol</span><span class="sxs-lookup"><span data-stu-id="d11f0-186">Task</span></span> | <span data-ttu-id="d11f0-187">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="d11f0-187">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="d11f0-188">Přidání vývojář do testovacího prostředí v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="d11f0-188">Add a developer to a lab in Azure DevTest Labs</span></span>](devtest-lab-add-devtest-user.md) |<span data-ttu-id="d11f0-189">Pomocí portálu Azure přidejte vývojáři do vašeho testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="d11f0-189">Use the Azure portal to add developers to your lab.</span></span>|
   | [<span data-ttu-id="d11f0-190">Přidat vývojáři do testovacího prostředí pomocí skriptu prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="d11f0-190">Add developers to the lab using a PowerShell script</span></span>](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |<span data-ttu-id="d11f0-191">Použití Powershellu k automatizaci přidání vývojářům testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="d11f0-191">Use PowerShell to automate adding developers to your lab.</span></span> |
   | [<span data-ttu-id="d11f0-192">Získejte odkaz do testovacího prostředí</span><span class="sxs-lookup"><span data-stu-id="d11f0-192">Get a link to the lab</span></span>](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |<span data-ttu-id="d11f0-193">Zjistěte, jak vývojáři přímo přistupovat k testovacím prostředí pomocí hypertextový odkaz.</span><span class="sxs-lookup"><span data-stu-id="d11f0-193">Learn how developers can directly access a lab via a hyperlink.</span></span>|

7. <span data-ttu-id="d11f0-194">**Automatizovat vytvoření testovacího prostředí pro více týmů**</span><span class="sxs-lookup"><span data-stu-id="d11f0-194">**Automate lab creation for more teams**</span></span> 
   
    <span data-ttu-id="d11f0-195">Je možné automatizovat vytvoření testovacího prostředí, včetně vlastních nastavení, vytvořením šablony Resource Manageru a jej použijete k vytvoření identické labs znovu a znovu.</span><span class="sxs-lookup"><span data-stu-id="d11f0-195">You can automate lab creation, including custom settings, by creating a Resource Manager template and using it to create identical labs again and again.</span></span> 
   
    <span data-ttu-id="d11f0-196">Další informace kliknutím na odkazy v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="d11f0-196">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="d11f0-197">Úkol</span><span class="sxs-lookup"><span data-stu-id="d11f0-197">Task</span></span> | <span data-ttu-id="d11f0-198">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="d11f0-198">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="d11f0-199">Vytvoření testovacího prostředí pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="d11f0-199">Create a lab using a Resource Manager template</span></span>](devtest-lab-faq.md#how-do-i-create-a-lab-from-an-azure-resource-manager-template) |<span data-ttu-id="d11f0-200">Vytvoření prostředí v Azure DevTest Labs pomocí šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="d11f0-200">Create labs in Azure DevTest Labs using Resource Manager templates.</span></span> |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]
