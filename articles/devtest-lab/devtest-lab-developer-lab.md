---
title: "aaaUse Azure DevTest Labs pro vývojáře | Microsoft Docs"
description: "Zjistěte, jak toouse Azure DevTest Labs pro vývojáře scénáře."
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
ms.openlocfilehash: 16a3ef47c9fcbca3050dd50db5b472a9a1e3c62c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-devtest-labs-for-developers"></a><span data-ttu-id="c2346-103">Použití Azure DevTest Labs pro vývojáře</span><span class="sxs-lookup"><span data-stu-id="c2346-103">Use Azure DevTest Labs for developers</span></span>
<span data-ttu-id="c2346-104">Azure DevTest Labs lze použít tooimplement mnoho klíčových scénářů, ale jeden primární scénářů hello zahrnuje použití DevTest Labs toohost vývoj počítače pro vývojáře.</span><span class="sxs-lookup"><span data-stu-id="c2346-104">Azure DevTest Labs can be used tooimplement many key scenarios, but one of hello primary scenarios involves using DevTest Labs toohost development machines for developers.</span></span> <span data-ttu-id="c2346-105">V tomto scénáři DevTest Labs poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="c2346-105">In this scenario, DevTest Labs provides these benefits:</span></span>

- <span data-ttu-id="c2346-106">Vývojáři mohou rychle zřizovat jejich vývoj počítačů na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="c2346-106">Developers can quickly provision their development machines on demand.</span></span>
- <span data-ttu-id="c2346-107">Vývojáři mohou snadno přizpůsobit své počítače vývoj vždy, když je potřeba.</span><span class="sxs-lookup"><span data-stu-id="c2346-107">Developers can easily customize their development machines whenever needed.</span></span>
- <span data-ttu-id="c2346-108">Správci můžou řídit náklady, přičemž zajistí, aby:</span><span class="sxs-lookup"><span data-stu-id="c2346-108">Administrators can control costs by ensuring that:</span></span>
  - <span data-ttu-id="c2346-109">Vývojáři nelze získat více virtuálních počítačů, než potřebují pro vývoj.</span><span class="sxs-lookup"><span data-stu-id="c2346-109">Developers cannot get more VMs than they need for development.</span></span>
  - <span data-ttu-id="c2346-110">Virtuální počítače jsou vypnutí při není používán.</span><span class="sxs-lookup"><span data-stu-id="c2346-110">VMs are shut down when not in use.</span></span> 

![Používá DevTest Labs pro školení](./media/devtest-lab-developer-lab/devtest-lab-developer-lab.png)

<span data-ttu-id="c2346-112">V tomto článku informace o různých funkcí Azure DevTest Labs, které se dají použít toomeet vývojáře požadavky a hello podrobné kroky, které vám pomůžou tooset laboratoře.</span><span class="sxs-lookup"><span data-stu-id="c2346-112">In this article, you learn about various Azure DevTest Labs features that can be used toomeet developer requirements and hello detailed steps that you can follow tooset up a lab.</span></span>

## <a name="implementing-developer-environments-with-azure-devtest-labs"></a><span data-ttu-id="c2346-113">Implementace vývojáře prostředí s Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="c2346-113">Implementing developer environments with Azure DevTest Labs</span></span>
1. <span data-ttu-id="c2346-114">**Vytvoření testovacího prostředí hello**</span><span class="sxs-lookup"><span data-stu-id="c2346-114">**Create hello lab**</span></span> 
   
    <span data-ttu-id="c2346-115">Labs jsou výchozí bod v Azure DevTest Labs hello.</span><span class="sxs-lookup"><span data-stu-id="c2346-115">Labs are hello starting point in Azure DevTest Labs.</span></span> <span data-ttu-id="c2346-116">Po vytvoření testovacího prostředí, můžete provádět úlohy, jako je například přidávání toohello lab uživatelů (vývojáři), nastavení zásady toocontrol náklady, definování Image virtuálních počítačů, které můžete rychle vytvořit a další.</span><span class="sxs-lookup"><span data-stu-id="c2346-116">Once you create a lab, you can perform tasks such as adding users (developers) toohello lab, setting policies toocontrol costs, defining VM images that can create quickly, and more.</span></span>  
   
    <span data-ttu-id="c2346-117">Další informace kliknutím na odkazy hello v hello následující tabulka:</span><span class="sxs-lookup"><span data-stu-id="c2346-117">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="c2346-118">Úkol</span><span class="sxs-lookup"><span data-stu-id="c2346-118">Task</span></span> | <span data-ttu-id="c2346-119">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="c2346-119">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="c2346-120">Vytvoření testovacího prostředí v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="c2346-120">Create a lab in Azure DevTest Labs</span></span>](devtest-lab-create-lab.md) |<span data-ttu-id="c2346-121">Zjistěte, jak hello toocreate a testovacího prostředí v Azure DevTest Labs v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c2346-121">Learn how toocreate a lab in Azure DevTest Labs in hello Azure portal.</span></span> |
2. <span data-ttu-id="c2346-122">**Vytvořit virtuální počítače během pár minut pomocí připravených marketplace Image a vlastních bitových kopií**</span><span class="sxs-lookup"><span data-stu-id="c2346-122">**Create VMs in minutes using ready-made marketplace images and custom images**</span></span> 
   
    <span data-ttu-id="c2346-123">Můžete vybrat připravených bitové kopie z široké škály bitové kopie v Azure Marketplace hello a zpřístupnit v testovacím prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="c2346-123">You can pick ready-made images from a wide variety of images in hello Azure Marketplace and make them available in hello lab.</span></span> <span data-ttu-id="c2346-124">Pokud připravených Image hello nevyhovují vašim požadavkům, můžete vytvořit vlastní image vytvořením virtuálního počítače pomocí připravených bitovou kopii z Azure Marketplace, instalace všech hello softwaru, které potřebujete, a ukládá hello virtuálního počítače jako vlastní image v testovacím prostředí hello testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c2346-124">If hello ready-made images don't meet your requirements, you can create a custom image by creating a lab VM using a ready-made image from Azure Marketplace, installing all hello software that you need, and saving hello VM as a custom image in hello lab.</span></span>

    <span data-ttu-id="c2346-125">Pokud budete používat vlastní Image, zvažte použití toocreate objekt pro vytváření bitové kopie a distribuci bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="c2346-125">If you will be using custom images, consider using an image factory toocreate and distribute your images.</span></span> <span data-ttu-id="c2346-126">Objekt pro vytváření bitové kopie je jako kód konfigurace řešení, které pravidelně sestavení a automaticky distribuuje nakonfigurované obrázků.</span><span class="sxs-lookup"><span data-stu-id="c2346-126">An image factory is a configuration-as-code solution that regularly builds and distributes your configured images automatically.</span></span> <span data-ttu-id="c2346-127">Tato uloží hello čas potřebný toomanually konfiguraci systému hello po vytvoření virtuálního počítače s hello základní operační systém.</span><span class="sxs-lookup"><span data-stu-id="c2346-127">This saves hello time required toomanually configure hello system after a VM has been created with hello base OS.</span></span>
  
    <span data-ttu-id="c2346-128">Další informace kliknutím na odkazy hello v hello následující tabulka:</span><span class="sxs-lookup"><span data-stu-id="c2346-128">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="c2346-129">Úkol</span><span class="sxs-lookup"><span data-stu-id="c2346-129">Task</span></span> | <span data-ttu-id="c2346-130">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="c2346-130">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="c2346-131">Konfigurace Azure Marketplace obrázků</span><span class="sxs-lookup"><span data-stu-id="c2346-131">Configure Azure Marketplace images</span></span>](devtest-lab-configure-marketplace-images.md) |<span data-ttu-id="c2346-132">Přečtěte si, jak povolených Image Azure Marketplace, zpřístupnění pro výběr pouze hello bitové kopie, které chcete pro vývojáře hello.</span><span class="sxs-lookup"><span data-stu-id="c2346-132">Learn how you can whitelist Azure Marketplace images, making available for selection only hello images you want for hello developers.</span></span>|
   | [<span data-ttu-id="c2346-133">Vytvořit vlastní image</span><span class="sxs-lookup"><span data-stu-id="c2346-133">Create a custom image</span></span>](devtest-lab-create-template.md) |<span data-ttu-id="c2346-134">Vytvořte vlastní image před instalací hello softwaru, které potřebujete, aby vývojáři mohou rychle vytvořit virtuální počítač pomocí vlastní image hello.</span><span class="sxs-lookup"><span data-stu-id="c2346-134">Create a custom image by pre-installing hello software you need so that developers can quickly create a VM using hello custom image.</span></span>|
   | [<span data-ttu-id="c2346-135">Další informace o vytváření bitové kopie</span><span class="sxs-lookup"><span data-stu-id="c2346-135">Learn about image factory</span></span>](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/) |<span data-ttu-id="c2346-136">Přehrát video, které popisuje, jak tooset až a použít objekt pro vytváření bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="c2346-136">Watch a video that describes how tooset up and use an image factory.</span></span>|

3. <span data-ttu-id="c2346-137">**Vytváření opakovaně použitelných šablon pro vývojáře počítače**</span><span class="sxs-lookup"><span data-stu-id="c2346-137">**Create reusable templates for developer machines**</span></span> 
   
    <span data-ttu-id="c2346-138">Vzorec v Azure DevTest Labs je, že seznam výchozí hodnoty vlastností použít toocreate virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c2346-138">A formula in Azure DevTest Labs is a list of default property values used toocreate a VM.</span></span> <span data-ttu-id="c2346-139">Vzorce můžete vytvořit v testovacím prostředí hello výběrem obrázku, velikost virtuálního počítače (kombinaci procesor a paměť RAM) a virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="c2346-139">You can create a formula in hello lab by picking an image, a VM size (a combination of CPU and RAM), and a virtual network.</span></span> <span data-ttu-id="c2346-140">Každý Vývojář můžete najdete v části hello vzorec v testovacím prostředí hello a použít ho toocreate virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c2346-140">Each developer can see hello formula in hello lab and use it toocreate a VM.</span></span> 
   
    <span data-ttu-id="c2346-141">Další informace kliknutím na odkazy hello v hello následující tabulka:</span><span class="sxs-lookup"><span data-stu-id="c2346-141">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="c2346-142">Úkol</span><span class="sxs-lookup"><span data-stu-id="c2346-142">Task</span></span> | <span data-ttu-id="c2346-143">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="c2346-143">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="c2346-144">Spravovat DevTest Labs vzorce toocreate virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="c2346-144">Manage DevTest Labs formulas toocreate VMs</span></span>](devtest-lab-manage-formulas.md) |<span data-ttu-id="c2346-145">Zjistěte, jak můžete vytvořit vzorec podle vyzvednutí obrázku, velikost virtuálního počítače (kombinaci procesor a paměť RAM) a virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="c2346-145">Learn how you can create a formula by picking up an image, VM size (combination of CPU and RAM), and a virtual network.</span></span>|

4. <span data-ttu-id="c2346-146">**Vytvoření artefakty tooenable flexibilní přizpůsobení virtuálního počítače**</span><span class="sxs-lookup"><span data-stu-id="c2346-146">**Create artifacts tooenable flexible VM customization**</span></span>

   <span data-ttu-id="c2346-147">Artefakty jsou použité toodeploy a nakonfigurovat svoji aplikaci po zřízení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c2346-147">Artifacts are used toodeploy and configure your application after a VM is provisioned.</span></span> <span data-ttu-id="c2346-148">Artefakty mohou být:</span><span class="sxs-lookup"><span data-stu-id="c2346-148">Artifacts can be:</span></span>

   - <span data-ttu-id="c2346-149">Nástroje, které chcete tooinstall na hello virtuálního počítače – například agentů, Fiddler a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c2346-149">Tools that you want tooinstall on hello VM - such as agents, Fiddler, and Visual Studio.</span></span>
   - <span data-ttu-id="c2346-150">Akce, že chcete toorun na hello virtuálního počítače – například klonování úložišti.</span><span class="sxs-lookup"><span data-stu-id="c2346-150">Actions that you want toorun on hello VM - such as cloning a repo.</span></span>
   - <span data-ttu-id="c2346-151">Aplikace, které chcete tootest.</span><span class="sxs-lookup"><span data-stu-id="c2346-151">Applications that you want tootest.</span></span>

   <span data-ttu-id="c2346-152">Mnoho artefakty je již k dispozici out-of-the-box.</span><span class="sxs-lookup"><span data-stu-id="c2346-152">Many artifacts are already available out-of-the-box.</span></span> <span data-ttu-id="c2346-153">Pokud chcete další přizpůsobení svých konkrétních potřeb můžete vytvořit vlastní artefakty.</span><span class="sxs-lookup"><span data-stu-id="c2346-153">You can create your own custom artifacts if you want more customization for your specific needs.</span></span>

   <span data-ttu-id="c2346-154">Další informace kliknutím na odkazy hello v hello následující tabulka:</span><span class="sxs-lookup"><span data-stu-id="c2346-154">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="c2346-155">Úkol</span><span class="sxs-lookup"><span data-stu-id="c2346-155">Task</span></span> | <span data-ttu-id="c2346-156">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="c2346-156">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="c2346-157">Vytvoření vlastních artefaktů pro virtuální počítač DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="c2346-157">Create custom artifacts for your DevTest Labs VM</span></span>](devtest-lab-artifact-author.md) |<span data-ttu-id="c2346-158">Vytvoření vlastních artefaktů pro virtuální počítače hello ve svém testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c2346-158">Create your own custom artifacts for hello virtual machines in your lab.</span></span>|
   | [<span data-ttu-id="c2346-159">Přidání vlastních artefaktů Git úložiště toostore a šablon Azure Resource Manageru pro použití v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="c2346-159">Add a Git repository toostore custom artifacts and Azure Resource Manager templates for use in Azure DevTest Labs</span></span>](devtest-lab-add-artifact-repo.md) |<span data-ttu-id="c2346-160">Zjistěte, jak toostore vlastní artefakty na vlastní privátní úložiště Git.</span><span class="sxs-lookup"><span data-stu-id="c2346-160">Learn how toostore your custom artifacts in your own private Git repo.</span></span>|

5. <span data-ttu-id="c2346-161">**Náklady na ovládací prvek**</span><span class="sxs-lookup"><span data-stu-id="c2346-161">**Control costs**</span></span>
   
    <span data-ttu-id="c2346-162">Azure DevTest Labs umožňuje tooset zásadu hello testovacím toospecify hello maximálního povoleného počtu virtuálních počítačů, které lze vytvořit pomocí vývojář v testovacím prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="c2346-162">Azure DevTest Labs allows you tooset a policy in hello lab toospecify hello maximum number of VMs that can be created by a developer in hello lab.</span></span> 
   
    <span data-ttu-id="c2346-163">Pokud má váš tým vývojáře sady pracovní plán a chcete toostop všechny hello virtuální počítače v určitém čase hello dne a pak automaticky je restartovat hello následující den, můžete snadno provést, pomocí nastavení automatické ukončení a automatického spouštění zásad v testovacím prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="c2346-163">If your developer team has a set work schedule and you want toostop all hello VMs at a particular time of hello day and then automatically restart them hello following day, you can easily accomplish that by setting auto-shutdown and auto-start policies in hello lab.</span></span> 
   
    <span data-ttu-id="c2346-164">Nakonec po dokončení vývoj aplikací můžete odstranit všechny virtuální počítače hello najednou spuštěním jednoho skriptu prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c2346-164">Finally, when app development is complete, you can delete all hello VMs at once by running a single PowerShell script.</span></span> 
   
    <span data-ttu-id="c2346-165">Další informace kliknutím na odkazy hello v hello následující tabulka:</span><span class="sxs-lookup"><span data-stu-id="c2346-165">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="c2346-166">Úkol</span><span class="sxs-lookup"><span data-stu-id="c2346-166">Task</span></span> | <span data-ttu-id="c2346-167">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="c2346-167">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="c2346-168">Definice zásad testovacího prostředí</span><span class="sxs-lookup"><span data-stu-id="c2346-168">Define lab policies</span></span>](devtest-lab-set-lab-policy.md) |<span data-ttu-id="c2346-169">Řízení nákladů pomocí nastavení zásad v testovacím prostředí hello.</span><span class="sxs-lookup"><span data-stu-id="c2346-169">Control costs by setting policies in hello lab.</span></span> |
   | [<span data-ttu-id="c2346-170">Odstranit všechny hello prostředí virtuálních počítačů pomocí skriptu prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c2346-170">Delete all hello lab VMs using a PowerShell script</span></span>](devtest-lab-faq.md#how-can-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |<span data-ttu-id="c2346-171">Po dokončení vývoj, odstraňte všechny labs hello v rámci jedné operace.</span><span class="sxs-lookup"><span data-stu-id="c2346-171">Delete all hello labs in one operation when development is complete.</span></span>|

1. <span data-ttu-id="c2346-172">**Přidat tooa virtuální sítě virtuálních počítačů**</span><span class="sxs-lookup"><span data-stu-id="c2346-172">**Add a virtual network tooa VM**</span></span> 
   
    <span data-ttu-id="c2346-173">DevTest Labs vytvoří novou virtuální síť (VNET) vždy, když je vytvořena testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="c2346-173">DevTest Labs creates a new virtual network (VNET) whenever a lab is created.</span></span> <span data-ttu-id="c2346-174">Pokud jste nakonfigurovali vlastní virtuální síť – například s použitím ExpressRoute nebo VPN typu site-to-site – můžete přidat nastavení virtuální sítě v této laboratoři tooyour virtuální sítě tak, aby bylo dostupné při vytváření virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c2346-174">If you have configured your own VNET – for example, by using ExpressRoute or site-to-site VPN – you can add this VNET tooyour lab's virtual network settings so that it is available when creating VMs.</span></span>

    <span data-ttu-id="c2346-175">Kromě toho je artefakt připojení k doméně služby Azure Active Directory k dispozici, když se vytváří hello virtuálních počítačů, bude připojení k doméně tooa virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="c2346-175">In addition, there is an Azure Active Directory domain join artifact available that will join a VM tooa domain when hello VM is being created.</span></span> 
   
    <span data-ttu-id="c2346-176">Další informace kliknutím na odkazy hello v hello následující tabulka:</span><span class="sxs-lookup"><span data-stu-id="c2346-176">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="c2346-177">Úkol</span><span class="sxs-lookup"><span data-stu-id="c2346-177">Task</span></span> | <span data-ttu-id="c2346-178">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="c2346-178">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="c2346-179">Konfigurace virtuální sítě v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="c2346-179">Configure a virtual network in Azure DevTest Labs</span></span>](devtest-lab-configure-vnet.md) |<span data-ttu-id="c2346-180">Zjistěte, jak hello tooconfigure virtuální sítě v Azure DevTest Labs pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="c2346-180">Learn how tooconfigure a virtual network in Azure DevTest Labs using hello Azure portal.</span></span>|

6. <span data-ttu-id="c2346-181">**Sdílet hello testovacího prostředí se každý vývojář**</span><span class="sxs-lookup"><span data-stu-id="c2346-181">**Share hello lab with each developer**</span></span>
   
    <span data-ttu-id="c2346-182">Labs lze přímo přistupovat pomocí odkazu, který sdílíte s vaší vývojáři.</span><span class="sxs-lookup"><span data-stu-id="c2346-182">Labs can be directly accessed using a link that you share with your developers.</span></span> <span data-ttu-id="c2346-183">I nemají toohave účet Azure, tak dlouho, dokud mají [účtu Microsoft](devtest-lab-faq.md#what-is-a-microsoft-account).</span><span class="sxs-lookup"><span data-stu-id="c2346-183">They don't even have toohave an Azure account, as long as they have a [Microsoft account](devtest-lab-faq.md#what-is-a-microsoft-account).</span></span> <span data-ttu-id="c2346-184">Vývojáři nelze zobrazit virtuální počítače vytvořené jinými vývojáři.</span><span class="sxs-lookup"><span data-stu-id="c2346-184">Developers cannot see VMs created by other developers.</span></span>  
   
    <span data-ttu-id="c2346-185">Další informace kliknutím na odkazy hello v hello následující tabulka:</span><span class="sxs-lookup"><span data-stu-id="c2346-185">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="c2346-186">Úkol</span><span class="sxs-lookup"><span data-stu-id="c2346-186">Task</span></span> | <span data-ttu-id="c2346-187">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="c2346-187">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="c2346-188">Přidání testovacího prostředí tooa vývojáře v Azure DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="c2346-188">Add a developer tooa lab in Azure DevTest Labs</span></span>](devtest-lab-add-devtest-user.md) |<span data-ttu-id="c2346-189">Použijte hello Azure portálu tooadd vývojáři tooyour testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="c2346-189">Use hello Azure portal tooadd developers tooyour lab.</span></span>|
   | [<span data-ttu-id="c2346-190">Přidání prostředí toohello vývojáři pomocí skriptu prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="c2346-190">Add developers toohello lab using a PowerShell script</span></span>](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |<span data-ttu-id="c2346-191">Pomocí prostředí PowerShell tooautomate přidání testovacího prostředí tooyour vývojáři.</span><span class="sxs-lookup"><span data-stu-id="c2346-191">Use PowerShell tooautomate adding developers tooyour lab.</span></span> |
   | [<span data-ttu-id="c2346-192">Získání testovacího prostředí toohello odkaz</span><span class="sxs-lookup"><span data-stu-id="c2346-192">Get a link toohello lab</span></span>](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |<span data-ttu-id="c2346-193">Zjistěte, jak vývojáři přímo přistupovat k testovacím prostředí pomocí hypertextový odkaz.</span><span class="sxs-lookup"><span data-stu-id="c2346-193">Learn how developers can directly access a lab via a hyperlink.</span></span>|

7. <span data-ttu-id="c2346-194">**Automatizovat vytvoření testovacího prostředí pro více týmů**</span><span class="sxs-lookup"><span data-stu-id="c2346-194">**Automate lab creation for more teams**</span></span> 
   
    <span data-ttu-id="c2346-195">Je možné automatizovat vytvoření testovacího prostředí, včetně vlastních nastavení, vytváření šablony Resource Manageru a použitím ho znovu a znovu toocreate identické laboratoře.</span><span class="sxs-lookup"><span data-stu-id="c2346-195">You can automate lab creation, including custom settings, by creating a Resource Manager template and using it toocreate identical labs again and again.</span></span> 
   
    <span data-ttu-id="c2346-196">Další informace kliknutím na odkazy hello v hello následující tabulka:</span><span class="sxs-lookup"><span data-stu-id="c2346-196">Learn more by clicking on hello links in hello following table:</span></span>
   
   | <span data-ttu-id="c2346-197">Úkol</span><span class="sxs-lookup"><span data-stu-id="c2346-197">Task</span></span> | <span data-ttu-id="c2346-198">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="c2346-198">What you learn</span></span> |
   | --- | --- |
   | [<span data-ttu-id="c2346-199">Vytvoření testovacího prostředí pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="c2346-199">Create a lab using a Resource Manager template</span></span>](devtest-lab-faq.md#how-do-i-create-a-lab-from-an-azure-resource-manager-template) |<span data-ttu-id="c2346-200">Vytvoření prostředí v Azure DevTest Labs pomocí šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="c2346-200">Create labs in Azure DevTest Labs using Resource Manager templates.</span></span> |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

