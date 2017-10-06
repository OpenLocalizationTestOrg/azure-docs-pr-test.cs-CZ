---
title: "aaaUse Azure DevTest Labs pro virtuální počítač a PaaS testovací prostředí | Microsoft Docs"
description: "Zjistěte, jak toouse Azure DevTest Labs pro virtuální počítač a PaaS scénáře prostředí otestovat."
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
ms.openlocfilehash: 9285090da768491e1275942318b094fae89e3273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-devtest-labs-for-vm-and-paas-test-environments"></a><span data-ttu-id="460c9-103">Použití Azure DevTest Labs pro virtuální počítač a PaaS testovací prostředí</span><span class="sxs-lookup"><span data-stu-id="460c9-103">Use Azure DevTest Labs for VM and PaaS test environments</span></span>

<span data-ttu-id="460c9-104">Můžete použít Azure DevTest Labs tooimplement mnoho klíčových scénářů, ale primární scénář vyžaduje, abyste použili počítače toohost DevTest Labs pro testerům, sada.</span><span class="sxs-lookup"><span data-stu-id="460c9-104">You can use Azure DevTest Labs tooimplement many key scenarios, but a primary scenario involves using DevTest Labs toohost machines for testers.</span></span> 

<span data-ttu-id="460c9-105">V tomto scénáři DevTest Labs poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="460c9-105">In this scenario, DevTest Labs provides these benefits:</span></span>

- <span data-ttu-id="460c9-106">Testery můžete otestovat hello nejnovější verzi jejich aplikace rychle zřizování prostředí systému Windows a Linux pomocí opakovaně použitelné šablony a artefaktů.</span><span class="sxs-lookup"><span data-stu-id="460c9-106">Testers can test hello latest version of their application by quickly provisioning Windows and Linux environments using reusable templates and artifacts.</span></span>
- <span data-ttu-id="460c9-107">Testery můžete postupně škálovat jejich zatížení testování zřizování více testovacích agentů.</span><span class="sxs-lookup"><span data-stu-id="460c9-107">Testers can scale up their load testing by provisioning multiple test agents.</span></span>
- <span data-ttu-id="460c9-108">Správci můžou řídit náklady, přičemž zajistí, aby:</span><span class="sxs-lookup"><span data-stu-id="460c9-108">Administrators can control costs by ensuring that:</span></span>
  - <span data-ttu-id="460c9-109">Testery nelze získat více virtuálních počítačů, než potřebují.</span><span class="sxs-lookup"><span data-stu-id="460c9-109">Testers cannot get more VMs than they need.</span></span>
  - <span data-ttu-id="460c9-110">Virtuální počítače jsou vypnutí při není používán.</span><span class="sxs-lookup"><span data-stu-id="460c9-110">VMs are shut down when not in use.</span></span>

![Používá DevTest Labs pro školení](./media/devtest-lab-developer-lab/devtest-lab-developer-lab.png)

<span data-ttu-id="460c9-112">V tomto článku Další informace o různých Azure DevTest Labs funkce, které používá toomeet tester požadavcích a hello podrobné kroky toofollow tooset laboratoře.</span><span class="sxs-lookup"><span data-stu-id="460c9-112">In this article, you learn about various Azure DevTest Labs features used toomeet tester requirements and hello detailed steps toofollow tooset up a lab.</span></span>

## <a name="implementing-test-environments-with-azure-devtest-labs"></a><span data-ttu-id="460c9-113">Implementace testovacích prostředích s Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="460c9-113">Implementing test environments with Azure DevTest Labs</span></span>
1. <span data-ttu-id="460c9-114">**Vytvoření testovacího prostředí hello**</span><span class="sxs-lookup"><span data-stu-id="460c9-114">**Create hello lab**</span></span> 
   
    <span data-ttu-id="460c9-115">Labs jsou výchozí bod v Azure DevTest Labs hello.</span><span class="sxs-lookup"><span data-stu-id="460c9-115">Labs are hello starting point in Azure DevTest Labs.</span></span> <span data-ttu-id="460c9-116">Po vytvoření testovacího prostředí, můžete provádět úlohy, jako je například přidávání toohello lab uživatelů (testerům, sada), nastavení zásady toocontrol náklady, definování Image virtuálních počítačů, které můžete rychle vytvořit a další.</span><span class="sxs-lookup"><span data-stu-id="460c9-116">Once you create a lab, you can perform tasks such as adding users (testers) toohello lab, setting policies toocontrol costs, defining VM images that can create quickly, and more.</span></span>  
   
    <span data-ttu-id="460c9-117">Další informace kliknutím na odkazy hello v hello následující tabulka:</span><span class="sxs-lookup"><span data-stu-id="460c9-117">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="460c9-118">Úkol</span><span class="sxs-lookup"><span data-stu-id="460c9-118">Task</span></span> | <span data-ttu-id="460c9-119">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="460c9-119">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="460c9-120">Vytvoření testovacího prostředí v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="460c9-120">Create a lab in Azure DevTest Labs</span></span>](devtest-lab-create-lab.md) |<span data-ttu-id="460c9-121">Zjistěte, jak hello toocreate a testovacího prostředí v Azure DevTest Labs v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="460c9-121">Learn how toocreate a lab in Azure DevTest Labs in hello Azure portal.</span></span> |
2. <span data-ttu-id="460c9-122">**Vytvořit virtuální počítače během pár minut pomocí připravených marketplace Image a vlastních bitových kopií**</span><span class="sxs-lookup"><span data-stu-id="460c9-122">**Create VMs in minutes using ready-made marketplace images and custom images**</span></span> 
   
    <span data-ttu-id="460c9-123">Můžete vybrat připravených bitové kopie z široké škály bitové kopie v Azure Marketplace hello a zpřístupnit v testovacím prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="460c9-123">You can pick ready-made images from a wide variety of images in hello Azure Marketplace and make them available in hello lab.</span></span> <span data-ttu-id="460c9-124">Pokud připravených Image hello nevyhovují vašim požadavkům, můžete vytvořit vlastní image vytvořením virtuálního počítače pomocí připravených bitovou kopii z Azure Marketplace, instalace všech hello softwaru, které potřebujete, a ukládá hello virtuálního počítače jako vlastní image v testovacím prostředí hello testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="460c9-124">If hello ready-made images don't meet your requirements, you can create a custom image by creating a lab VM using a ready-made image from Azure Marketplace, installing all hello software that you need, and saving hello VM as a custom image in hello lab.</span></span>

    <span data-ttu-id="460c9-125">Pokud budete používat vlastní Image, zvažte použití toocreate objekt pro vytváření bitové kopie a distribuci bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="460c9-125">If you will be using custom images, consider using an image factory toocreate and distribute your images.</span></span> <span data-ttu-id="460c9-126">Objekt pro vytváření bitové kopie je jako kód konfigurace řešení, které pravidelně sestavení a automaticky distribuuje nakonfigurované obrázků.</span><span class="sxs-lookup"><span data-stu-id="460c9-126">An image factory is a configuration-as-code solution that regularly builds and distributes your configured images automatically.</span></span> <span data-ttu-id="460c9-127">Tato uloží hello čas potřebný toomanually konfiguraci systému hello po vytvoření virtuálního počítače s hello základní operační systém.</span><span class="sxs-lookup"><span data-stu-id="460c9-127">This saves hello time required toomanually configure hello system after a VM has been created with hello base OS.</span></span>
  
    <span data-ttu-id="460c9-128">Další informace kliknutím na odkazy hello v hello následující tabulka:</span><span class="sxs-lookup"><span data-stu-id="460c9-128">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="460c9-129">Úkol</span><span class="sxs-lookup"><span data-stu-id="460c9-129">Task</span></span> | <span data-ttu-id="460c9-130">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="460c9-130">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="460c9-131">Konfigurace Azure Marketplace obrázků</span><span class="sxs-lookup"><span data-stu-id="460c9-131">Configure Azure Marketplace images</span></span>](devtest-lab-configure-marketplace-images.md) |<span data-ttu-id="460c9-132">Přečtěte si, jak povolených Image Azure Marketplace, zpřístupnění pro výběr pouze hello bitové kopie, kterou chcete použít pro testery hello.</span><span class="sxs-lookup"><span data-stu-id="460c9-132">Learn how you can whitelist Azure Marketplace images, making available for selection only hello images you want for hello testers.</span></span>|
   | [<span data-ttu-id="460c9-133">Vytvořit vlastní image</span><span class="sxs-lookup"><span data-stu-id="460c9-133">Create a custom image</span></span>](devtest-lab-create-template.md) |<span data-ttu-id="460c9-134">Vytvořte vlastní image před instalací hello softwaru, které potřebujete, aby testery dokáže rychle vytvořit virtuální počítač pomocí vlastní image hello.</span><span class="sxs-lookup"><span data-stu-id="460c9-134">Create a custom image by pre-installing hello software you need so that testers can quickly create a VM using hello custom image.</span></span>|
   | [<span data-ttu-id="460c9-135">Další informace o vytváření bitové kopie</span><span class="sxs-lookup"><span data-stu-id="460c9-135">Learn about image factory</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/) |<span data-ttu-id="460c9-136">Přehrát video, které popisuje, jak tooset až a použít objekt pro vytváření bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="460c9-136">Watch a video that describes how tooset up and use an image factory.</span></span>|

3. <span data-ttu-id="460c9-137">**Vytvoření šablon opakovaně použitelné pro testovacích počítačů**</span><span class="sxs-lookup"><span data-stu-id="460c9-137">**Create reusable templates for test machines**</span></span> 
   
    <span data-ttu-id="460c9-138">Vzorec v Azure DevTest Labs je, že seznam výchozí hodnoty vlastností použít toocreate virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="460c9-138">A formula in Azure DevTest Labs is a list of default property values used toocreate a VM.</span></span> <span data-ttu-id="460c9-139">Vzorce můžete vytvořit v testovacím prostředí hello výběrem obrázku, velikost virtuálního počítače (kombinaci procesor a paměť RAM) a virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="460c9-139">You can create a formula in hello lab by picking an image, a VM size (a combination of CPU and RAM), and a virtual network.</span></span> <span data-ttu-id="460c9-140">Každý tester můžete najdete v části hello vzorec v testovacím prostředí hello a použít ho toocreate virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="460c9-140">Each tester can see hello formula in hello lab and use it toocreate a VM.</span></span> 
   
    <span data-ttu-id="460c9-141">Další informace kliknutím na odkazy hello v hello následující tabulka:</span><span class="sxs-lookup"><span data-stu-id="460c9-141">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="460c9-142">Úkol</span><span class="sxs-lookup"><span data-stu-id="460c9-142">Task</span></span> | <span data-ttu-id="460c9-143">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="460c9-143">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="460c9-144">Spravovat DevTest Labs vzorce toocreate virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="460c9-144">Manage DevTest Labs formulas toocreate VMs</span></span>](devtest-lab-manage-formulas.md) |<span data-ttu-id="460c9-145">Zjistěte, jak můžete vytvořit vzorec podle vyzvednutí obrázku, velikost virtuálního počítače (kombinaci procesor a paměť RAM) a virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="460c9-145">Learn how you can create a formula by picking up an image, VM size (combination of CPU and RAM), and a virtual network.</span></span>|

3. <span data-ttu-id="460c9-146">**Vytvoření více virtuálních počítačů testovací prostředí**</span><span class="sxs-lookup"><span data-stu-id="460c9-146">**Create multi-VM test environments**</span></span> 
   
    <span data-ttu-id="460c9-147">Můžete použít infrastrukturu hello toodefine šablony Azure Resource Manager a konfigurace řešení Azure a opakovaně nasadit více testovací virtuální počítače v konzistentním stavu.</span><span class="sxs-lookup"><span data-stu-id="460c9-147">You can use Azure Resource Manager templates toodefine hello infrastructure and configuration of your Azure solution and repeatedly deploy multiple test VMs in a consistent state.</span></span>

    <span data-ttu-id="460c9-148">Prostředky Azure PaaS může být zřízen v prostředí z šablony Resource Manageru a zobrazí v sledování nákladů.</span><span class="sxs-lookup"><span data-stu-id="460c9-148">Azure PaaS resources can be provisioned in an environment from a Resource Manager template and appear in cost tracking.</span></span> <span data-ttu-id="460c9-149">Vypnutí virtuálního počítače automaticky nevztahuje tooPaaS prostředky.</span><span class="sxs-lookup"><span data-stu-id="460c9-149">However, VM auto shutdown does not apply tooPaaS resources.</span></span>

    <span data-ttu-id="460c9-150">Další informace kliknutím na odkazy hello v hello následující tabulka:</span><span class="sxs-lookup"><span data-stu-id="460c9-150">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="460c9-151">Úkol</span><span class="sxs-lookup"><span data-stu-id="460c9-151">Task</span></span> | <span data-ttu-id="460c9-152">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="460c9-152">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="460c9-153">Vytvoření prostředí více virtuálních počítačů a prostředků PaaS pomocí šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="460c9-153">Create multi-VM environments and PaaS resources with Azure Resource Manager templates</span></span>](devtest-lab-create-environment-from-arm.md) |<span data-ttu-id="460c9-154">Zjistěte, jak můžete nasadit víc virtuálních počítačů v konzistentním stavu pro testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="460c9-154">Learn how you can deploy multiple VMs in a consistent state for your test environment.</span></span>|

4. <span data-ttu-id="460c9-155">**Vytvoření artefakty tooenable flexibilní přizpůsobení virtuálního počítače**</span><span class="sxs-lookup"><span data-stu-id="460c9-155">**Create artifacts tooenable flexible VM customization**</span></span>

   <span data-ttu-id="460c9-156">Artefakty jsou použité toodeploy a nakonfigurovat svoji aplikaci po zřízení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="460c9-156">Artifacts are used toodeploy and configure your application after a VM is provisioned.</span></span> <span data-ttu-id="460c9-157">Artefakty mohou být:</span><span class="sxs-lookup"><span data-stu-id="460c9-157">Artifacts can be:</span></span>

   - <span data-ttu-id="460c9-158">Nástroje, které chcete tooinstall na hello virtuálního počítače – například agentů, Fiddler a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="460c9-158">Tools that you want tooinstall on hello VM - such as agents, Fiddler, and Visual Studio.</span></span>
   - <span data-ttu-id="460c9-159">Akce, že chcete toorun na hello virtuálního počítače – například klonování úložišti.</span><span class="sxs-lookup"><span data-stu-id="460c9-159">Actions that you want toorun on hello VM - such as cloning a repo.</span></span>
   - <span data-ttu-id="460c9-160">Aplikace, které chcete tootest.</span><span class="sxs-lookup"><span data-stu-id="460c9-160">Applications that you want tootest.</span></span>

   <span data-ttu-id="460c9-161">Mnoho artefakty je již k dispozici out-of-the-box.</span><span class="sxs-lookup"><span data-stu-id="460c9-161">Many artifacts are already available out-of-the-box.</span></span> <span data-ttu-id="460c9-162">Ale pokud chcete další přizpůsobení svých konkrétních potřeb, můžete vytvořit vlastní artefakty.</span><span class="sxs-lookup"><span data-stu-id="460c9-162">But if you want more customization for your specific needs, you can create your own custom artifacts.</span></span>

   <span data-ttu-id="460c9-163">Další informace kliknutím na odkazy hello v hello následující tabulka:</span><span class="sxs-lookup"><span data-stu-id="460c9-163">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="460c9-164">Úkol</span><span class="sxs-lookup"><span data-stu-id="460c9-164">Task</span></span> | <span data-ttu-id="460c9-165">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="460c9-165">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="460c9-166">Vytvoření vlastních artefaktů pro virtuální počítač DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="460c9-166">Create custom artifacts for your DevTest Labs VM</span></span>](devtest-lab-artifact-author.md) |<span data-ttu-id="460c9-167">Vytvoření vlastních artefaktů pro virtuální počítače hello ve svém testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="460c9-167">Create your own custom artifacts for hello virtual machines in your lab.</span></span>|
   | [<span data-ttu-id="460c9-168">Přidání vlastních artefaktů Git úložiště toostore a šablon Azure Resource Manageru pro použití v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="460c9-168">Add a Git repository toostore custom artifacts and Azure Resource Manager templates for use in Azure DevTest Labs</span></span>](devtest-lab-add-artifact-repo.md) |<span data-ttu-id="460c9-169">Zjistěte, jak toostore vlastní artefakty na vlastní privátní úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="460c9-169">Learn how toostore your custom artifacts in your own private Git repo.</span></span>|

5. <span data-ttu-id="460c9-170">**Náklady na ovládací prvek**</span><span class="sxs-lookup"><span data-stu-id="460c9-170">**Control costs**</span></span>
   
    <span data-ttu-id="460c9-171">Azure DevTest Labs umožňuje tooset zásadu hello testovacím toospecify hello maximálního povoleného počtu virtuálních počítačů, které lze vytvořit pomocí nástroje testování v testovacím prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="460c9-171">Azure DevTest Labs allows you tooset a policy in hello lab toospecify hello maximum number of VMs that can be created by a tester in hello lab.</span></span> 
   
    <span data-ttu-id="460c9-172">Pokud má váš tým testovací sadu pracovní plán a chcete toostop všechny hello virtuální počítače v určitém čase hello dne a pak automaticky je restartovat hello následující den, můžete snadno dosáhnete pomocí nastavení automatické ukončení a automatického spouštění zásad v testovacím prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="460c9-172">If your test team has a set work schedule and you want toostop all hello VMs at a particular time of hello day and then automatically restart them hello following day, you can easily accomplish that by setting auto-shutdown and auto-start policies in hello lab.</span></span> 
   
    <span data-ttu-id="460c9-173">Nakonec po dokončení vývoj aplikací můžete odstranit všechny virtuální počítače hello najednou spuštěním jednoho skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="460c9-173">Finally, when app development is complete, you can delete all hello VMs at once by running a single PowerShell script.</span></span> 
   
    <span data-ttu-id="460c9-174">Další informace kliknutím na odkazy hello v hello následující tabulka:</span><span class="sxs-lookup"><span data-stu-id="460c9-174">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="460c9-175">Úkol</span><span class="sxs-lookup"><span data-stu-id="460c9-175">Task</span></span> | <span data-ttu-id="460c9-176">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="460c9-176">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="460c9-177">Definice zásad testovacího prostředí</span><span class="sxs-lookup"><span data-stu-id="460c9-177">Define lab policies</span></span>](devtest-lab-set-lab-policy.md) |<span data-ttu-id="460c9-178">Řízení nákladů pomocí nastavení zásad v testovacím prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="460c9-178">Control costs by setting policies in hello lab.</span></span> |
   | [<span data-ttu-id="460c9-179">Odstranit všechny hello prostředí virtuálních počítačů pomocí skriptu prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="460c9-179">Delete all hello lab VMs using a PowerShell script</span></span>](devtest-lab-faq.md#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |<span data-ttu-id="460c9-180">Po dokončení testování, odstraňte všechny labs hello v rámci jedné operace.</span><span class="sxs-lookup"><span data-stu-id="460c9-180">Delete all hello labs in one operation when testing is complete.</span></span>|

1. <span data-ttu-id="460c9-181">**Přidat virtuální síť tooa testovacího prostředí**</span><span class="sxs-lookup"><span data-stu-id="460c9-181">**Add a virtual network tooa Lab**</span></span> 
   
    <span data-ttu-id="460c9-182">DevTest Labs vytvoří novou virtuální síť (VNET) vždy, když je vytvořena testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="460c9-182">DevTest Labs creates a new virtual network (VNET) whenever a lab is created.</span></span> <span data-ttu-id="460c9-183">Pokud jste nakonfigurovali vlastní virtuální síť – například s použitím ExpressRoute nebo VPN typu site-to-site – můžete přidat nastavení virtuální sítě v této laboratoři tooyour virtuální sítě tak, aby bylo dostupné při vytváření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="460c9-183">If you have configured your own VNET – for example, by using ExpressRoute or site-to-site VPN – you can add this VNET tooyour lab's virtual network settings so that it is available when creating VMs.</span></span>

    <span data-ttu-id="460c9-184">Kromě toho je Azure Active Directory domény spojení artefakt k dispozici, připojen do domény tooa virtuálních počítačů, když se vytváří hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="460c9-184">In addition, there is an Azure Active Directory domain join artifact available that joins a VM tooa domain when hello VM is being created.</span></span> 
   
    <span data-ttu-id="460c9-185">Další informace kliknutím na odkazy hello v hello následující tabulka:</span><span class="sxs-lookup"><span data-stu-id="460c9-185">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="460c9-186">Úkol</span><span class="sxs-lookup"><span data-stu-id="460c9-186">Task</span></span> | <span data-ttu-id="460c9-187">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="460c9-187">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="460c9-188">Konfigurace virtuální sítě v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="460c9-188">Configure a virtual network in Azure DevTest Labs</span></span>](devtest-lab-configure-vnet.md) |<span data-ttu-id="460c9-189">Zjistěte, jak hello tooconfigure virtuální sítě v Azure DevTest Labs pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="460c9-189">Learn how tooconfigure a virtual network in Azure DevTest Labs using hello Azure portal.</span></span>|

6. <span data-ttu-id="460c9-190">**Laboratoř hello sdílenou složku s každou tester**</span><span class="sxs-lookup"><span data-stu-id="460c9-190">**Share hello lab with each tester**</span></span>
   
    <span data-ttu-id="460c9-191">Labs lze přímo přistupovat pomocí odkazu, který sdílíte s vaší testerům, sada.</span><span class="sxs-lookup"><span data-stu-id="460c9-191">Labs can be directly accessed using a link that you share with your testers.</span></span> <span data-ttu-id="460c9-192">I nemají toohave účet Azure, tak dlouho, dokud mají [účtu Microsoft](devtest-lab-faq.md#what-is-a-microsoft-account).</span><span class="sxs-lookup"><span data-stu-id="460c9-192">They don't even have toohave an Azure account, as long as they have a [Microsoft account](devtest-lab-faq.md#what-is-a-microsoft-account).</span></span> <span data-ttu-id="460c9-193">Testery neuvidí vytvořit další testerům, sada virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="460c9-193">Testers cannot see VMs created by other testers.</span></span>  
   
    <span data-ttu-id="460c9-194">Další informace kliknutím na odkazy hello v hello následující tabulka:</span><span class="sxs-lookup"><span data-stu-id="460c9-194">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="460c9-195">Úkol</span><span class="sxs-lookup"><span data-stu-id="460c9-195">Task</span></span> | <span data-ttu-id="460c9-196">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="460c9-196">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="460c9-197">Přidání testovacího prostředí tooa tester v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="460c9-197">Add a tester tooa lab in Azure DevTest Labs</span></span>](devtest-lab-add-devtest-user.md) |<span data-ttu-id="460c9-198">Použijte hello Azure portálu tooadd testery tooyour testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="460c9-198">Use hello Azure portal tooadd testers tooyour lab.</span></span>|
   | [<span data-ttu-id="460c9-199">Přidání prostředí toohello testery pomocí skriptu prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="460c9-199">Add testers toohello lab using a PowerShell script</span></span>](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |<span data-ttu-id="460c9-200">Pomocí prostředí PowerShell tooautomate přidání testery tooyour testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="460c9-200">Use PowerShell tooautomate adding testers tooyour lab.</span></span> |
   | [<span data-ttu-id="460c9-201">Získání testovacího prostředí toohello odkaz</span><span class="sxs-lookup"><span data-stu-id="460c9-201">Get a link toohello lab</span></span>](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |<span data-ttu-id="460c9-202">Zjistěte, jak testery přímo přistupovat k testovacím prostředí pomocí hypertextový odkaz.</span><span class="sxs-lookup"><span data-stu-id="460c9-202">Learn how testers can directly access a lab via a hyperlink.</span></span>|

7. <span data-ttu-id="460c9-203">**Automatizovat vytvoření testovacího prostředí pro více týmů**</span><span class="sxs-lookup"><span data-stu-id="460c9-203">**Automate lab creation for more teams**</span></span> 
   
    <span data-ttu-id="460c9-204">Je možné automatizovat vytvoření testovacího prostředí, včetně vlastních nastavení, vytváření šablony Resource Manageru a použitím ho znovu a znovu toocreate identické laboratoře.</span><span class="sxs-lookup"><span data-stu-id="460c9-204">You can automate lab creation, including custom settings, by creating a Resource Manager template and using it toocreate identical labs again and again.</span></span> 
   
    <span data-ttu-id="460c9-205">Další informace kliknutím na odkazy hello v hello následující tabulka:</span><span class="sxs-lookup"><span data-stu-id="460c9-205">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="460c9-206">Úkol</span><span class="sxs-lookup"><span data-stu-id="460c9-206">Task</span></span> | <span data-ttu-id="460c9-207">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="460c9-207">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="460c9-208">Vytvoření testovacího prostředí pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="460c9-208">Create a lab using a Resource Manager template</span></span>](devtest-lab-faq.md#how-do-i-create-a-lab-from-an-azure-resource-manager-template) |<span data-ttu-id="460c9-209">Vytvoření prostředí v Azure DevTest Labs pomocí šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="460c9-209">Create labs in Azure DevTest Labs using Resource Manager templates.</span></span> |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

