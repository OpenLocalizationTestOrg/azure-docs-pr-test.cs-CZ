---
title: "Použít k trénování Azure DevTest Labs | Microsoft Docs"
description: "Naučte se používat Azure DevTest Labs pro scénáře školení."
services: devtest-lab,virtual-machines
documentationcenter: na
author: steved0x
manager: douge
editor: 
ms.assetid: 57ff4e30-7e33-453f-9867-e19b3fdb9fe2
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2016
ms.author: sdanie
ms.openlocfilehash: a85999b7963e9a07d3f91ec47f298f91439c0808
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-devtest-labs-for-training"></a><span data-ttu-id="b7ff0-103">Použití Azure DevTest Labs pro školení</span><span class="sxs-lookup"><span data-stu-id="b7ff0-103">Use Azure DevTest Labs for training</span></span>
<span data-ttu-id="b7ff0-104">Azure DevTest Labs lze použít k implementaci mnoho klíčových scénářů, kromě vývoje/testování.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-104">Azure DevTest Labs can be used to implement many key scenarios in addition to dev/test.</span></span> <span data-ttu-id="b7ff0-105">Jedním z těchto scénářů je nastavit testovací prostředí pro školení.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-105">One of those scenarios is to set up a lab for training.</span></span> <span data-ttu-id="b7ff0-106">Azure DevTest Labs umožňuje vytvoření testovacího prostředí, kde můžete zadat vlastní šablony, které každý praktikanta můžete použít k vytvoření identické a izolované prostředí pro školení.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-106">Azure DevTest Labs allows you to create a lab where you can provide custom templates that each trainee can use to create identical and isolated environments for training.</span></span> <span data-ttu-id="b7ff0-107">Můžete použít zásady, abyste zajistili, že školení prostředí jsou k dispozici pro každý praktikanta jenom v případě, že je potřebují a obsahovat dostatek prostředků – jako jsou virtuální počítače - požadované pro školení.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-107">You can apply policies to ensure that training environments are available to each trainee only when they need them and contain enough resources - such as virtual machines - required for the training.</span></span> <span data-ttu-id="b7ff0-108">Nakonec můžete snadno sdílet testovací prostředí s povolání, které mají přístup k jedním kliknutím.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-108">Finally, you can easily share the lab with trainees, which they can access in one click.</span></span>

![Používá DevTest Labs pro školení](./media/devtest-lab-training-lab/devtest-lab-training.png)

<span data-ttu-id="b7ff0-110">Azure DevTest Labs splňuje následující požadavky, které jsou nutné k provádějte školení v jakémkoli virtuální prostředí:</span><span class="sxs-lookup"><span data-stu-id="b7ff0-110">Azure DevTest Labs meets the following requirements that are required to conduct training in any virtual environment:</span></span> 

* <span data-ttu-id="b7ff0-111">Povolání neuvidí virtuální počítače vytvořené jiných povolání</span><span class="sxs-lookup"><span data-stu-id="b7ff0-111">Trainees cannot see VMs created by other trainees</span></span>
* <span data-ttu-id="b7ff0-112">Každý počítač školení by měly být shodné</span><span class="sxs-lookup"><span data-stu-id="b7ff0-112">Every training machine should be identical</span></span>
* <span data-ttu-id="b7ff0-113">Povolání můžete rychle zřídit jejich prostředí školení</span><span class="sxs-lookup"><span data-stu-id="b7ff0-113">Trainees can quickly provision their training environments</span></span>
* <span data-ttu-id="b7ff0-114">Řízení nákladů tím zajistí, že povolání nelze získat více virtuálních počítačů, než potřebují pro školení a také vypnutí virtuálních počítačů, když nejsou na jejich používání</span><span class="sxs-lookup"><span data-stu-id="b7ff0-114">Control cost by ensuring that trainees cannot get more VMs than they need for the training and also shutdown VMs when they are not using them</span></span>
* <span data-ttu-id="b7ff0-115">Snadno sdílet s každou praktikanta školení testovacího prostředí</span><span class="sxs-lookup"><span data-stu-id="b7ff0-115">Easily share the training lab with each trainee</span></span>
* <span data-ttu-id="b7ff0-116">Znovu a znovu použít testovací prostředí školení</span><span class="sxs-lookup"><span data-stu-id="b7ff0-116">Reuse the training lab again and again</span></span>

<span data-ttu-id="b7ff0-117">V tomto článku informace o různých funkcí Azure DevTest Labs, které můžete použít ke splnění požadavků bylo popsáno dříve školení a podrobné kroky, které vám pomůžou nastavit testovací prostředí pro školení.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-117">In this article, you learn about various Azure DevTest Labs features that can be used to meet the previously described training requirements and detailed steps that you can follow to set up a lab for training.</span></span>  

## <a name="implementing-training-with-azure-devtest-labs"></a><span data-ttu-id="b7ff0-118">Implementace školení s Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="b7ff0-118">Implementing training with Azure DevTest Labs</span></span>
1. <span data-ttu-id="b7ff0-119">**Vytvořit testovací prostředí**</span><span class="sxs-lookup"><span data-stu-id="b7ff0-119">**Create the lab**</span></span> 
   
    <span data-ttu-id="b7ff0-120">Labs jsou výchozím bodem v Azure DevTest Labs.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-120">Labs are the starting point in Azure DevTest Labs.</span></span> <span data-ttu-id="b7ff0-121">Po vytvoření testovacího prostředí, můžete provádět úlohy tak, jak přidat uživatele (povolání) do testovacího prostředí, nastavit zásady, které řídit náklady, definování Image virtuálních počítačů, které lze rychle vytvořit a další.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-121">Once you create a lab, you can perform tasks such as add users (trainees) to the lab, set policies to control costs, define VM images that can create quickly, and more.</span></span>   
   
    <span data-ttu-id="b7ff0-122">Další informace kliknutím na odkazy v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="b7ff0-122">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="b7ff0-123">Úkol</span><span class="sxs-lookup"><span data-stu-id="b7ff0-123">Task</span></span> | <span data-ttu-id="b7ff0-124">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="b7ff0-124">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="b7ff0-125">Vytvoření testovacího prostředí v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="b7ff0-125">Create a lab in Azure DevTest Labs</span></span>](devtest-lab-create-lab.md) |<span data-ttu-id="b7ff0-126">Postup vytvoření testovacího prostředí v Azure DevTest Labs na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-126">Learn how to create a lab in Azure DevTest Labs in the Azure portal.</span></span> |
2. <span data-ttu-id="b7ff0-127">**Vytvořit virtuální počítače školení během pár minut pomocí připravených marketplace Image a vlastních bitových kopií**</span><span class="sxs-lookup"><span data-stu-id="b7ff0-127">**Create training VMs in minutes using ready-made marketplace images and custom images**</span></span> 
   
    <span data-ttu-id="b7ff0-128">Můžete vybrat připravených bitové kopie z široké škály bitové kopie v Azure Marketplace a zpřístupnit na povolání v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-128">You can pick ready-made images from a wide variety of images in the Azure Marketplace and make them available for the trainees in the lab.</span></span> <span data-ttu-id="b7ff0-129">Jestliže připravených bitové kopie nevyhovují vašim požadavkům, můžete vytvořit vlastní image vytvořením testovacího prostředí virtuálních počítačů pomocí připravených bitové kopie z Azure Marketplace, instalace veškerý software, který budete potřebovat pro školení a uložení virtuálního počítače jako vlastní image v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-129">If the ready-made images don't meet your requirements, you can create a custom image by creating a lab VM using a ready-made image from Azure Marketplace, installing all the software that you need for the training, and saving the VM as custom image in the lab.</span></span> 
   
    <span data-ttu-id="b7ff0-130">Další informace kliknutím na odkazy v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="b7ff0-130">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="b7ff0-131">Úkol</span><span class="sxs-lookup"><span data-stu-id="b7ff0-131">Task</span></span> | <span data-ttu-id="b7ff0-132">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="b7ff0-132">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="b7ff0-133">Konfigurace Azure Marketplace obrázků</span><span class="sxs-lookup"><span data-stu-id="b7ff0-133">Configure Azure Marketplace images</span></span>](devtest-lab-configure-marketplace-images.md) |<span data-ttu-id="b7ff0-134">Přečtěte si, jak Image Azure Marketplace povolených; zpřístupnění pro výběr pouze Image chcete použít pro školení.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-134">Learn how you can whitelist Azure Marketplace images; making available for selection only the images you want for the training.</span></span> |
   | [<span data-ttu-id="b7ff0-135">Vytvořit vlastní image</span><span class="sxs-lookup"><span data-stu-id="b7ff0-135">Create a custom image</span></span>](devtest-lab-create-template.md) |<span data-ttu-id="b7ff0-136">Vytvořte vlastní image před instalací softwaru, které potřebujete k školení tak, aby povolání dokáže rychle vytvořit virtuální počítač pomocí vlastní image.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-136">Create a custom image by pre-installing the software you need for the training so that trainees can quickly create a VM using the custom image.</span></span> |
3. <span data-ttu-id="b7ff0-137">**Vytvořit opakovaně použitelný šablony pro školení počítače**</span><span class="sxs-lookup"><span data-stu-id="b7ff0-137">**Create reusable templates for training machines**</span></span> 
   
    <span data-ttu-id="b7ff0-138">Vzorec v Azure DevTest Labs je seznam výchozí hodnoty vlastností použít k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-138">A formula in Azure DevTest Labs is a list of default property values used to create a VM.</span></span> <span data-ttu-id="b7ff0-139">Vzorec v testovacím prostředí vytvoříte výběr obrázku, velikost virtuálního počítače (kombinaci procesor a paměť RAM) a virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-139">You can create a formula in the lab by picking an image, a VM size (a combination of CPU and RAM), and a virtual network.</span></span> <span data-ttu-id="b7ff0-140">Každý praktikanta můžete zobrazit vzorec v testovacím prostředí a ji použít k vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-140">Each trainee can see the formula in the lab and use it to create a VM.</span></span> 
   
    <span data-ttu-id="b7ff0-141">Další informace kliknutím na odkazy v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="b7ff0-141">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="b7ff0-142">Úkol</span><span class="sxs-lookup"><span data-stu-id="b7ff0-142">Task</span></span> | <span data-ttu-id="b7ff0-143">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="b7ff0-143">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="b7ff0-144">Spravovat DevTest Labs vzorce pro vytvoření virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="b7ff0-144">Manage DevTest Labs formulas to create VMs</span></span>](devtest-lab-manage-formulas.md) |<span data-ttu-id="b7ff0-145">Zjistěte, jak můžete vytvořit vzorec podle vyzvednutí obrázku, velikost virtuálního počítače (kombinaci procesor a paměť RAM) a virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-145">Learn how you can create a formula by picking up an image, VM size (combination of CPU and RAM), and a virtual network.</span></span> |
4. <span data-ttu-id="b7ff0-146">**Náklady na ovládací prvek**</span><span class="sxs-lookup"><span data-stu-id="b7ff0-146">**Control costs**</span></span>
   
    <span data-ttu-id="b7ff0-147">Azure DevTest Labs umožňuje nastavit zásady v testovacím prostředí, chcete-li určit maximální počet virtuálních počítačů, které lze vytvořit pomocí praktikanta v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-147">Azure DevTest Labs allows you to set a policy in the lab to specify the maximum number of VMs that can be created by a trainee in the lab.</span></span> 
   
    <span data-ttu-id="b7ff0-148">Pokud jsou provádění Vícedenní školení a chcete zastavit všechny virtuální počítače v určitém čase dne a pak automaticky je znovu spustit následující den, můžete snadno provést nastavením automatické ukončení a automatického spouštění zásad v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-148">If you are conducting multi-day training and want to stop all the VMs at a particular time of the day and then automatically restart them the following day, you can easily accomplish that by setting auto-shutdown and auto-start policies in the lab.</span></span> 
   
    <span data-ttu-id="b7ff0-149">Nakonec po dokončení školení můžete odstranit všechny virtuální počítače najednou spuštěním jednoho skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-149">Finally, when training is complete you can delete all the VMs at once by running a single PowerShell script.</span></span> 
   
    <span data-ttu-id="b7ff0-150">Další informace kliknutím na odkazy v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="b7ff0-150">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="b7ff0-151">Úkol</span><span class="sxs-lookup"><span data-stu-id="b7ff0-151">Task</span></span> | <span data-ttu-id="b7ff0-152">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="b7ff0-152">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="b7ff0-153">Definice zásad testovacího prostředí</span><span class="sxs-lookup"><span data-stu-id="b7ff0-153">Define lab policies</span></span>](devtest-lab-set-lab-policy.md) |<span data-ttu-id="b7ff0-154">Řízení nákladů pomocí nastavení zásad v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-154">Control costs by setting policies in the lab.</span></span> |
   | [<span data-ttu-id="b7ff0-155">Odstranit všechny testovací prostředí virtuálních počítačů pomocí skriptu prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="b7ff0-155">Delete all the lab VMs using a PowerShell script</span></span>](devtest-lab-faq.md#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |<span data-ttu-id="b7ff0-156">Po dokončení školení, odstraňte všechny labs v rámci jedné operace.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-156">Delete all the labs in one operation when the training is complete.</span></span> |
5. <span data-ttu-id="b7ff0-157">**Sdílet testovací prostředí s každou praktikanta**</span><span class="sxs-lookup"><span data-stu-id="b7ff0-157">**Share the lab with each trainee**</span></span>
   
    <span data-ttu-id="b7ff0-158">Labs lze přímo přistupovat pomocí odkazu, který sdílíte s vaší povolání.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-158">Labs can be directly accessed using a link that you share with your trainees.</span></span> <span data-ttu-id="b7ff0-159">Vaše povolání dokonce ani nemusí mít účet Azure, tak dlouho, dokud mají [účtu Microsoft](devtest-lab-faq.md#what-is-a-microsoft-account).</span><span class="sxs-lookup"><span data-stu-id="b7ff0-159">Your trainees don't even have to have an Azure account, as long as they have a [Microsoft account](devtest-lab-faq.md#what-is-a-microsoft-account).</span></span> <span data-ttu-id="b7ff0-160">Povolání neuvidí virtuální počítače vytvořené jiných povolání.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-160">Trainees cannot see VMs created by other trainees.</span></span>  
   
    <span data-ttu-id="b7ff0-161">Další informace kliknutím na odkazy v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="b7ff0-161">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="b7ff0-162">Úkol</span><span class="sxs-lookup"><span data-stu-id="b7ff0-162">Task</span></span> | <span data-ttu-id="b7ff0-163">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="b7ff0-163">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="b7ff0-164">Přidání praktikanta do testovacího prostředí v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="b7ff0-164">Add a trainee to a lab in Azure DevTest Labs</span></span>](devtest-lab-add-devtest-user.md) |<span data-ttu-id="b7ff0-165">Použití portálu Azure přidat povolání do testovacího prostředí školení.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-165">Use the Azure portal to add trainees to your training lab.</span></span> |
   | [<span data-ttu-id="b7ff0-166">Přidat povolání do testovacího prostředí pomocí skriptu prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="b7ff0-166">Add trainees to the lab using a PowerShell script</span></span>](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |<span data-ttu-id="b7ff0-167">Použití Powershellu k automatizaci přidání povolání do testovacího prostředí školení.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-167">Use PowerShell to automate adding trainees to your training lab.</span></span> |
   | [<span data-ttu-id="b7ff0-168">Získejte odkaz do testovacího prostředí</span><span class="sxs-lookup"><span data-stu-id="b7ff0-168">Get a link to the lab</span></span>](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |<span data-ttu-id="b7ff0-169">Zjistěte, jak testovacího prostředí je přímo přístupný prostřednictvím hypertextový odkaz.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-169">Learn how a lab can be directly accessed via a hyperlink.</span></span> |
6. <span data-ttu-id="b7ff0-170">**Znovu a znovu použít testovací prostředí**</span><span class="sxs-lookup"><span data-stu-id="b7ff0-170">**Reuse the lab again and again**</span></span> 
   
    <span data-ttu-id="b7ff0-171">Je možné automatizovat vytvoření testovacího prostředí, včetně vlastních nastavení, vytvořením šablony Resource Manageru a jej použijete k vytvoření identické labs znovu a znovu.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-171">You can automate lab creation, including custom settings, by creating a Resource Manager template and using it to create identical labs again and again.</span></span> 
   
    <span data-ttu-id="b7ff0-172">Další informace kliknutím na odkazy v následující tabulce:</span><span class="sxs-lookup"><span data-stu-id="b7ff0-172">Learn more by clicking on the links in the following table:</span></span>
   
   | <span data-ttu-id="b7ff0-173">Úkol</span><span class="sxs-lookup"><span data-stu-id="b7ff0-173">Task</span></span> | <span data-ttu-id="b7ff0-174">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="b7ff0-174">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="b7ff0-175">Vytvoření testovacího prostředí pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="b7ff0-175">Create a lab using a Resource Manager template</span></span>](devtest-lab-faq.md#how-do-i-create-a-lab-from-an-azure-resource-manager-template) |<span data-ttu-id="b7ff0-176">Vytvoření prostředí v Azure DevTest Labs pomocí šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="b7ff0-176">Create labs in Azure DevTest Labs using Resource Manager templates.</span></span> |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

