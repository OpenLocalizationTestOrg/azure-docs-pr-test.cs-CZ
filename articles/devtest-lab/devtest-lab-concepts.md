---
title: Koncepty DevTest Labs | Microsoft Docs
description: "Seznamte se základními koncepty DevTest Labs a jak ji můžete snadno vytvářet, spravovat a monitorovat virtuální počítače Azure"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 105919e8-3617-4ce3-a29f-a289fa608fb2
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
ms.openlocfilehash: 1caea59e71126e934e2e52a1ad7f533ffa7d4b03
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="devtest-labs-concepts"></a><span data-ttu-id="54762-103">Koncepce DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="54762-103">DevTest Labs concepts</span></span>
## <a name="overview"></a><span data-ttu-id="54762-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="54762-104">Overview</span></span>
<span data-ttu-id="54762-105">Následující seznam obsahuje klíčové koncepty DevTest Labs a definice:</span><span class="sxs-lookup"><span data-stu-id="54762-105">The following list contains key DevTest Labs concepts and definitions:</span></span>

## <a name="labs"></a><span data-ttu-id="54762-106">Testovací prostředí</span><span class="sxs-lookup"><span data-stu-id="54762-106">Labs</span></span>
<span data-ttu-id="54762-107">Testovacího prostředí je infrastrukturu, která zahrnuje skupinu prostředků, třeba virtuální počítače (VM), které vám umožní lépe spravovat tyto prostředky zadáním omezení a kvóty.</span><span class="sxs-lookup"><span data-stu-id="54762-107">A lab is the infrastructure that encompasses a group of resources, such as Virtual Machines (VMs), that lets you better manage those resources by specifying limits and quotas.</span></span>

## <a name="virtual-machine"></a><span data-ttu-id="54762-108">Virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="54762-108">Virtual machine</span></span>
<span data-ttu-id="54762-109">Virtuální počítač Azure je jedním z několika typů [na vyžádání, škálovatelných výpočetních prostředků](https://docs.microsoft.com/azure/app-service-web/choose-web-site-cloud-service-vm) Azure nabízí.</span><span class="sxs-lookup"><span data-stu-id="54762-109">An Azure VM is one of several types of [on-demand, scalable computing resources](https://docs.microsoft.com/azure/app-service-web/choose-web-site-cloud-service-vm) that Azure offers.</span></span> <span data-ttu-id="54762-110">Virtuální počítače Azure poskytují flexibilitu virtualizace bez nutnosti zakoupení a údržby fyzického hardwaru, který spouští, i když stále je třeba zachovat virtuální počítač tak, že provádění určitých úloh, jako je například konfigurace, opravy a instalace softwaru, který běží na ho.</span><span class="sxs-lookup"><span data-stu-id="54762-110">Azure VMs give you the flexibility of virtualization without having to buy and maintain the physical hardware that runs it, although you still need to maintain the VM by performing certain tasks, such as configuring, patching, and installing the software that runs on it.</span></span>

<span data-ttu-id="54762-111">[Přehled o virtuálních počítačích s Windows v Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-overview) poskytuje informace o co byste měli zvážit před vytvořit virtuální počítač, jak vytvořit a jak spravovat.</span><span class="sxs-lookup"><span data-stu-id="54762-111">[Overview of Windows virtual machines in Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-overview) gives you information about what you should consider before you create a VM, how you create it, and how you manage it.</span></span>

## <a name="claimable-vm"></a><span data-ttu-id="54762-112">Vymahatelných virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="54762-112">Claimable VM</span></span>
<span data-ttu-id="54762-113">Virtuální počítač Azure vymahatelných je virtuální počítač, který je k dispozici pro použití jakéhokoli testovacího prostředí uživatele s oprávněními.</span><span class="sxs-lookup"><span data-stu-id="54762-113">An Azure Claimable VM is a virtual machine that is available for use by any lab user with permissions.</span></span> <span data-ttu-id="54762-114">Správce testovacího prostředí můžete připravit virtuální počítače s konkrétním základní bitové kopie a artefaktů a uložit je ke sdílenému fondu.</span><span class="sxs-lookup"><span data-stu-id="54762-114">A lab admin can prepare VMs with specific base images and artifacts and save them to a shared pool.</span></span> <span data-ttu-id="54762-115">Testovacího prostředí můžete pak deklarace identity uživatele funkční virtuální počítač z fondu když potřebují jednu s konfigurací pro tuto konkrétní.</span><span class="sxs-lookup"><span data-stu-id="54762-115">A lab user can then claim a working VM from the pool when they need one with that specific configuration.</span></span>

<span data-ttu-id="54762-116">Virtuální počítač, který je vymahatelných není původně přiřazen k žádné konkrétní uživatele, ale se zobrazí v seznamu každého uživatele v části "Vymahatelných virtuální počítače".</span><span class="sxs-lookup"><span data-stu-id="54762-116">A VM that is claimable is not initially assigned to any particular user, but will show up in every user's list under "Claimable virtual machines".</span></span> <span data-ttu-id="54762-117">Po virtuálních počítačů je požadována uživatelem, se přesune až jejich "Moje virtuální počítače" oblasti a nadále již není vymahatelných jiným uživatelem.</span><span class="sxs-lookup"><span data-stu-id="54762-117">After a VM is claimed by a user, it is moved up to their "My virtual machines" area and is no longer claimable by any other user.</span></span>

## <a name="environment"></a><span data-ttu-id="54762-118">Prostředí</span><span class="sxs-lookup"><span data-stu-id="54762-118">Environment</span></span>
<span data-ttu-id="54762-119">Prostředí v DevTest Labs, odkazuje na kolekci prostředků Azure v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="54762-119">In DevTest Labs, an environment refers to a collection of Azure resources in a lab.</span></span> <span data-ttu-id="54762-120">[Tento příspěvek blogu](https://blogs.msdn.microsoft.com/devtestlab/2016/11/16/connect-2016-news-for-azure-devtest-labs-azure-resource-manager-template-based-environments-vm-auto-shutdown-and-more/) popisuje, jak vytvořit prostředí více virtuálních počítačů z šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="54762-120">[This blog post](https://blogs.msdn.microsoft.com/devtestlab/2016/11/16/connect-2016-news-for-azure-devtest-labs-azure-resource-manager-template-based-environments-vm-auto-shutdown-and-more/) discusses how to create multi-VM environments from your Azure Resource Manager templates.</span></span>

## <a name="base-images"></a><span data-ttu-id="54762-121">Základní bitové kopie</span><span class="sxs-lookup"><span data-stu-id="54762-121">Base images</span></span>
<span data-ttu-id="54762-122">Základní Image jsou Image virtuálních počítačů s všechny nástroje a nastavení předem nainstalován a nakonfigurován k rychlému vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="54762-122">Base images are VM images with all the tools and settings preinstalled and configured to quickly create a VM.</span></span> <span data-ttu-id="54762-123">Výběr existující základní a přidáním artefakt k instalaci agenta test agent můžete zřídit virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="54762-123">You can provision a VM by picking an existing base and adding an artifact to install your test agent.</span></span> <span data-ttu-id="54762-124">Pak můžete uložit zřízeného virtuálního počítače jako základ tak, aby základní lze použít bez nutnosti znovu nainstalujte agenta test pro každý zřizování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="54762-124">You can then save the provisioned VM as a base so that the base can be used without having to reinstall the test agent for each provisioning of the VM.</span></span>

## <a name="artifacts"></a><span data-ttu-id="54762-125">Artefakty</span><span class="sxs-lookup"><span data-stu-id="54762-125">Artifacts</span></span>
<span data-ttu-id="54762-126">Artefakty slouží k nasazení a konfiguraci aplikace po zřízení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="54762-126">Artifacts are used to deploy and configure your application after a VM is provisioned.</span></span> <span data-ttu-id="54762-127">Artefakty mohou být:</span><span class="sxs-lookup"><span data-stu-id="54762-127">Artifacts can be:</span></span>

* <span data-ttu-id="54762-128">Nástroje, které chcete nainstalovat do virtuálního počítače – například agentů, Fiddler a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="54762-128">Tools that you want to install on the VM - such as agents, Fiddler, and Visual Studio.</span></span>
* <span data-ttu-id="54762-129">Akce, které chcete spustit na virtuálním počítači – například klonování úložišti.</span><span class="sxs-lookup"><span data-stu-id="54762-129">Actions that you want to run on the VM - such as cloning a repo.</span></span>
* <span data-ttu-id="54762-130">Aplikace, které chcete otestovat</span><span class="sxs-lookup"><span data-stu-id="54762-130">Applications that you want to test.</span></span>

<span data-ttu-id="54762-131">Artefakty je [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) soubory JSON, které obsahují pokyny k provedení nasazení a konfiguraci použít.</span><span class="sxs-lookup"><span data-stu-id="54762-131">Artifacts are [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) JSON files that contain instructions to perform deployment and apply configuration.</span></span>

## <a name="artifact-repositories"></a><span data-ttu-id="54762-132">Úložiště artefaktů</span><span class="sxs-lookup"><span data-stu-id="54762-132">Artifact repositories</span></span>
<span data-ttu-id="54762-133">Úložiště artefaktů jsou úložiště git, kde se změnami artefakty.</span><span class="sxs-lookup"><span data-stu-id="54762-133">Artifact repositories are git repositories where artifacts are checked in.</span></span> <span data-ttu-id="54762-134">Úložiště artefaktů lze přidat do více labs ve vaší organizaci povolení opakované použití a sdílení.</span><span class="sxs-lookup"><span data-stu-id="54762-134">Artifact repositories can be added to multiple labs in your organization enabling reuse and sharing.</span></span>

## <a name="formulas"></a><span data-ttu-id="54762-135">Vzorce</span><span class="sxs-lookup"><span data-stu-id="54762-135">Formulas</span></span>
<span data-ttu-id="54762-136">Vzorce, kromě základní Image, poskytují mechanismus pro rychlé zřizování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="54762-136">Formulas, in addition to base images, provide a mechanism for fast VM provisioning.</span></span> <span data-ttu-id="54762-137">Vzorec v DevTest Labs je seznam výchozí hodnoty vlastností použít k vytvoření testovacího prostředí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="54762-137">A formula in DevTest Labs is a list of default property values used to create a lab VM.</span></span>
<span data-ttu-id="54762-138">Pomocí vzorce, virtuální počítače s jednou sadou vlastností – například základní bitovou kopii, velikost virtuálního počítače, virtuální sítě a artefakty – lze vytvořit bez nutnosti zadávat pokaždé, když tyto vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="54762-138">With formulas, VMs with the same set of properties - such as base image, VM size, virtual network, and artifacts - can be created without needing to specify those properties each time.</span></span> <span data-ttu-id="54762-139">Při vytváření virtuálního počítače ze vzorce, výchozí hodnoty lze použít jako-je nebo upravit.</span><span class="sxs-lookup"><span data-stu-id="54762-139">When creating a VM from a formula, the default values can be used as-is or modified.</span></span>

## <a name="policies"></a><span data-ttu-id="54762-140">Zásady</span><span class="sxs-lookup"><span data-stu-id="54762-140">Policies</span></span>
<span data-ttu-id="54762-141">Zásady pomáhají v řízení nákladů ve svém testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="54762-141">Policies help in controlling cost in your lab.</span></span> <span data-ttu-id="54762-142">Můžete například vytvořit zásadu, která automaticky vypnout virtuální počítače podle definovaného plánu.</span><span class="sxs-lookup"><span data-stu-id="54762-142">For example, you can create a policy to automatically shut down VMs based on a defined schedule.</span></span>

## <a name="caps"></a><span data-ttu-id="54762-143">CAP k vzdálené ploše</span><span class="sxs-lookup"><span data-stu-id="54762-143">Caps</span></span>
<span data-ttu-id="54762-144">CAP k vzdálené ploše je mechanismus pro minimalizaci odpady ve vašem testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="54762-144">Caps is a mechanism to minimize waste in your lab.</span></span> <span data-ttu-id="54762-145">Například můžete nastavit cap omezit počet virtuálních počítačů, které lze vytvořit na uživatele nebo v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="54762-145">For example, you can set a cap to restrict the number of VMs that can be created per user, or in a lab.</span></span>

## <a name="security-levels"></a><span data-ttu-id="54762-146">Úrovně zabezpečení</span><span class="sxs-lookup"><span data-stu-id="54762-146">Security levels</span></span>
<span data-ttu-id="54762-147">Zabezpečení přístupu je určen podle řízení řízení přístupu (RBAC).</span><span class="sxs-lookup"><span data-stu-id="54762-147">Security access is determined by Azure Role-Based Access Control (RBAC).</span></span> <span data-ttu-id="54762-148">Abyste pochopili, jak přístup funguje, pomáhá pochopit rozdíly mezi oprávnění, role a obor podle definice RBAC.</span><span class="sxs-lookup"><span data-stu-id="54762-148">To understand how access works, it helps to understand the differences between a permission, a role, and a scope as defined by RBAC.</span></span>

* <span data-ttu-id="54762-149">Oprávnění – oprávnění jsou definované přístup k určité akce (například-čtení pro všechny virtuální počítače).</span><span class="sxs-lookup"><span data-stu-id="54762-149">Permission - A permission is a defined access to a specific action (e.g. read-access to all virtual machines).</span></span>
* <span data-ttu-id="54762-150">Role - role je sadu oprávnění, které mohou být seskupeny a přiřazen k uživateli.</span><span class="sxs-lookup"><span data-stu-id="54762-150">Role - A role is a set of permissions that can be grouped and assigned to a user.</span></span> <span data-ttu-id="54762-151">Například *vlastník předplatného* má role přístup ke všem prostředkům v rámci předplatného.</span><span class="sxs-lookup"><span data-stu-id="54762-151">For example, the *subscription owner* role has access to all resources within a subscription.</span></span>
* <span data-ttu-id="54762-152">Rozsah - obor je úrovní v hierarchii prostředek služby Azure, jako je například skupinu prostředků, jeden testovacího prostředí nebo celé předplatné.</span><span class="sxs-lookup"><span data-stu-id="54762-152">Scope - A scope is a level within the hierarchy of an Azure resource, such as a resource group, a single lab, or the entire subscription.</span></span>

<span data-ttu-id="54762-153">V rámci oboru DevTest Labs, existují dva typy rolí k definování oprávnění uživatele: vlastníka testovacího prostředí a testovacího prostředí uživatele.</span><span class="sxs-lookup"><span data-stu-id="54762-153">Within the scope of DevTest Labs, there are two types of roles to define user permissions: lab owner and lab user.</span></span>

* <span data-ttu-id="54762-154">Laboratoř vlastníka – vlastník testovacího prostředí má přístup k žádným prostředkům v testovacím prostředí.</span><span class="sxs-lookup"><span data-stu-id="54762-154">Lab Owner - A lab owner has access to any resources within the lab.</span></span> <span data-ttu-id="54762-155">Proto vlastníka testovacího prostředí můžete upravit zásady, čtení a zápis všech virtuálních počítačů, změňte virtuální síť a tak dále.</span><span class="sxs-lookup"><span data-stu-id="54762-155">Therefore, a lab owner can modify policies, read and write any VMs, change the virtual network, and so on.</span></span>
* <span data-ttu-id="54762-156">Uživatel Lab - uživatel testovacího prostředí můžete zobrazit všechny prostředky testovacího prostředí, jako jsou virtuální počítače, zásady a virtuální sítě, ale nelze změnit zásady nebo všechny virtuální počítače vytvořené jinými uživateli.</span><span class="sxs-lookup"><span data-stu-id="54762-156">Lab User - A lab user can view all lab resources, such as VMs, policies, and virtual networks, but cannot modify policies or any VMs created by other users.</span></span>

<span data-ttu-id="54762-157">Chcete-li zjistit, jak vytvářet vlastní role v DevTest Labs, naleznete v článku [udělení uživatelských oprávnění k zásad konkrétní testovacího](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).</span><span class="sxs-lookup"><span data-stu-id="54762-157">To see how to create custom roles in DevTest Labs, refer to the article, [Grant user permissions to specific lab policies](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).</span></span>

<span data-ttu-id="54762-158">Vzhledem k tomu, že obory jsou hierarchické, když má uživatel oprávnění u určité oboru, jsou automaticky přiděleno těchto oprávnění v každé nižší úrovni oboru zahrnut.</span><span class="sxs-lookup"><span data-stu-id="54762-158">Since scopes are hierarchical, when a user has permissions at a certain scope, they are automatically granted those permissions at every lower-level scope encompassed.</span></span> <span data-ttu-id="54762-159">Například pokud uživatel je přiřazen k roli vlastník předplatného, pak mají přístup ke všem prostředkům v předplatném, včetně všech virtuálních počítačů, všechny virtuální sítě a všechny laboratoře.</span><span class="sxs-lookup"><span data-stu-id="54762-159">For instance, if a user is assigned to the role of subscription owner, then they have access to all resources in a subscription, which include all virtual machines, all virtual networks, and all labs.</span></span> <span data-ttu-id="54762-160">Proto vlastník předplatného automaticky převezme roli vlastníka testovacího prostředí.</span><span class="sxs-lookup"><span data-stu-id="54762-160">Therefore, a subscription owner automatically inherits the role of lab owner.</span></span> <span data-ttu-id="54762-161">Jako opak však není pravda.</span><span class="sxs-lookup"><span data-stu-id="54762-161">However, the opposite is not true.</span></span> <span data-ttu-id="54762-162">Vlastník testovacího prostředí má přístup k testovacím prostředí, které je obor nižší než úroveň předplatného.</span><span class="sxs-lookup"><span data-stu-id="54762-162">A lab owner has access to a lab, which is a lower scope than the subscription level.</span></span> <span data-ttu-id="54762-163">Vlastník testovacího prostředí proto nebudou moci zobrazit virtuální počítače nebo virtuální sítě nebo všechny prostředky, které jsou mimo testovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="54762-163">Therefore, a lab owner will not be able to see virtual machines or virtual networks or any resources that are outside of the lab.</span></span>

## <a name="azure-resource-manager-templates"></a><span data-ttu-id="54762-164">Šablony Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="54762-164">Azure Resource Manager templates</span></span>
<span data-ttu-id="54762-165">Všechny principy probírané v tomto článku můžete nakonfigurovat pomocí šablony Azure Resource Manager, které umožňují definovat infrastruktury nebo konfiguraci řešení Azure a opakovaně nasadit v konzistentním stavu.</span><span class="sxs-lookup"><span data-stu-id="54762-165">All of the concepts discussed in this article can be configured by using Azure Resource Manager templates, which let you define the infrastructure/configuration of your Azure solution and repeatedly deploy it in a consistent state.</span></span>

<span data-ttu-id="54762-166">[Pochopit strukturu a syntaxe šablon Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates#template-format) popisuje strukturu šablonu Azure Resource Manager a vlastnosti, které jsou k dispozici v různých částech šablony.</span><span class="sxs-lookup"><span data-stu-id="54762-166">[Understand the structure and syntax of Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates#template-format) describes the structure of an Azure Resource Manager template and the properties that are available in the different sections of a template.</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="54762-167">Další kroky</span><span class="sxs-lookup"><span data-stu-id="54762-167">Next steps</span></span>
[<span data-ttu-id="54762-168">Vytvoření testovacího prostředí v DevTest Labs</span><span class="sxs-lookup"><span data-stu-id="54762-168">Create a lab in DevTest Labs</span></span>](devtest-lab-create-lab.md)