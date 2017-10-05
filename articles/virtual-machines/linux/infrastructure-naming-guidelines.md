---
title: "Pojmenování pokyny - Linux infrastrukturu Azure | Microsoft Docs"
description: "Další informace o klíčových návrhu a implementace pokyny pro pojmenování ve službách infrastruktury Azure."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 18ee4fb1-8297-49a1-8d3f-097880be67c7
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1b086f0972c02d569a7219820a3d596960b6014b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-infrastructure-naming-guidelines-for-linux-vms"></a><span data-ttu-id="569c2-103">Pokyny pro pojmenování pro virtuální počítače s Linuxem infrastrukturu Azure</span><span class="sxs-lookup"><span data-stu-id="569c2-103">Azure infrastructure naming guidelines for Linux VMs</span></span> 

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

<span data-ttu-id="569c2-104">Tento článek se zaměřuje na pochopení postupy, dosahují zásady vytváření názvů pro všechny různých prostředků Azure k vytvoření logické a snadno identifikovat sadu prostředků ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="569c2-104">This article focuses on understanding how to approach naming conventions for all your various Azure resources to build a logical and easily identifiable set of resources across your environment.</span></span>

## <a name="implementation-guidelines-for-naming-conventions"></a><span data-ttu-id="569c2-105">Postup implementace pro zásady vytváření názvů</span><span class="sxs-lookup"><span data-stu-id="569c2-105">Implementation guidelines for naming conventions</span></span>
<span data-ttu-id="569c2-106">Rozhodnutí:</span><span class="sxs-lookup"><span data-stu-id="569c2-106">Decisions:</span></span>

* <span data-ttu-id="569c2-107">Jaké jsou vaše zásady vytváření názvů pro prostředky Azure?</span><span class="sxs-lookup"><span data-stu-id="569c2-107">What are your naming conventions for Azure resources?</span></span>

<span data-ttu-id="569c2-108">Úlohy:</span><span class="sxs-lookup"><span data-stu-id="569c2-108">Tasks:</span></span>

* <span data-ttu-id="569c2-109">Definujte přípony k zachování konzistence použít ve vašich prostředků.</span><span class="sxs-lookup"><span data-stu-id="569c2-109">Define the affixes to use across your resources to maintain consistency.</span></span>
* <span data-ttu-id="569c2-110">Zadejte názvy účtů úložiště zadaný požadavek mohly být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="569c2-110">Define storage account names given the requirement for them to be globally unique.</span></span>
* <span data-ttu-id="569c2-111">Zdokumentujte zásady vytváření názvů, který chcete použít a distribuovat do všech k zachování konzistence napříč nasazení zúčastněných stran.</span><span class="sxs-lookup"><span data-stu-id="569c2-111">Document the naming convention to be used and distribute to all parties involved to ensure consistency across deployments.</span></span>

## <a name="naming-conventions"></a><span data-ttu-id="569c2-112">Zásady vytváření názvů</span><span class="sxs-lookup"><span data-stu-id="569c2-112">Naming conventions</span></span>
<span data-ttu-id="569c2-113">Dobrý zásady vytváření názvů v místě byste měli mít před vytvořením nic v Azure.</span><span class="sxs-lookup"><span data-stu-id="569c2-113">You should have a good naming convention in place before creating anything in Azure.</span></span> <span data-ttu-id="569c2-114">Zásady vytváření názvů zajistí, že všechny prostředky předvídatelný název, což pomáhá snížit administrativní zátěž související se správou těchto prostředků.</span><span class="sxs-lookup"><span data-stu-id="569c2-114">A naming convention ensures that all the resources have a predictable name, which helps lower the administrative burden associated with managing those resources.</span></span>

<span data-ttu-id="569c2-115">Můžete zvolit podle konkrétní sadu zásady vytváření názvů definované pro celou organizaci nebo pro konkrétní předplatného Azure nebo účtu.</span><span class="sxs-lookup"><span data-stu-id="569c2-115">You might choose to follow a specific set of naming conventions defined for your entire organization or for a specific Azure subscription or account.</span></span> <span data-ttu-id="569c2-116">I když je snadné pro jednotlivce v rámci organizace vytvořit implicitní pravidla při práci s prostředky Azure, budete muset být škálovat pro týmy společně v Azure.</span><span class="sxs-lookup"><span data-stu-id="569c2-116">Although it is easy for individuals within organizations to establish implicit rules when working with Azure resources, you need to be able to scale for teams working together in Azure.</span></span>

<span data-ttu-id="569c2-117">Odsouhlasení sadu předem zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="569c2-117">Agree on a set of naming conventions up front.</span></span> <span data-ttu-id="569c2-118">Existují některé aspekty týkající se zásadami vytváření názvů které vyjmout napříč nastaví pravidel.</span><span class="sxs-lookup"><span data-stu-id="569c2-118">There are some considerations regarding naming conventions that cut across that sets of rules.</span></span>

## <a name="affixes"></a><span data-ttu-id="569c2-119">Přípony</span><span class="sxs-lookup"><span data-stu-id="569c2-119">Affixes</span></span>
<span data-ttu-id="569c2-120">Při pohledu definovat zásady vytváření názvů, je jeden rozhodnutí, jestli předpona je na:</span><span class="sxs-lookup"><span data-stu-id="569c2-120">As you look to define a naming convention, one decision is whether the affix is at:</span></span>

* <span data-ttu-id="569c2-121">Na začátek názvu (předpona)</span><span class="sxs-lookup"><span data-stu-id="569c2-121">The beginning of the name (prefix)</span></span>
* <span data-ttu-id="569c2-122">Konci názvu (přípona)</span><span class="sxs-lookup"><span data-stu-id="569c2-122">The end of the name (suffix)</span></span>

<span data-ttu-id="569c2-123">Například tady jsou dvě možné názvy pro skupinu prostředků pomocí `rg` opatří:</span><span class="sxs-lookup"><span data-stu-id="569c2-123">For instance, here are two possible names for a Resource Group using the `rg` affix:</span></span>

* <span data-ttu-id="569c2-124">Rg-WebApp (předpona)</span><span class="sxs-lookup"><span data-stu-id="569c2-124">Rg-WebApp (prefix)</span></span>
* <span data-ttu-id="569c2-125">WebApp-Rg (přípona)</span><span class="sxs-lookup"><span data-stu-id="569c2-125">WebApp-Rg (suffix)</span></span>

<span data-ttu-id="569c2-126">Přípony najdete různé aspekty, které popisují konkrétní prostředky.</span><span class="sxs-lookup"><span data-stu-id="569c2-126">Affixes can refer to different aspects that describe the particular resources.</span></span> <span data-ttu-id="569c2-127">Následující tabulka uvádí některé příklady obvykle používá.</span><span class="sxs-lookup"><span data-stu-id="569c2-127">The following table shows some examples typically used.</span></span>

| <span data-ttu-id="569c2-128">Aspekt</span><span class="sxs-lookup"><span data-stu-id="569c2-128">Aspect</span></span> | <span data-ttu-id="569c2-129">Příklady</span><span class="sxs-lookup"><span data-stu-id="569c2-129">Examples</span></span> | <span data-ttu-id="569c2-130">Poznámky</span><span class="sxs-lookup"><span data-stu-id="569c2-130">Notes</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="569c2-131">Prostředí</span><span class="sxs-lookup"><span data-stu-id="569c2-131">Environment</span></span> |<span data-ttu-id="569c2-132">vývoj, stg, produkčního</span><span class="sxs-lookup"><span data-stu-id="569c2-132">dev, stg, prod</span></span> |<span data-ttu-id="569c2-133">V závislosti na účelu a název každé prostředí.</span><span class="sxs-lookup"><span data-stu-id="569c2-133">Depending on the purpose and name of each environment.</span></span> |
| <span data-ttu-id="569c2-134">Umístění</span><span class="sxs-lookup"><span data-stu-id="569c2-134">Location</span></span> |<span data-ttu-id="569c2-135">usw (západní USA), použijte (východní USA 2)</span><span class="sxs-lookup"><span data-stu-id="569c2-135">usw (West US), use (East US 2)</span></span> |<span data-ttu-id="569c2-136">V závislosti na oblasti datovém centru nebo oblasti organizace.</span><span class="sxs-lookup"><span data-stu-id="569c2-136">Depending on the region of the datacenter or the region of the organization.</span></span> |
| <span data-ttu-id="569c2-137">Komponenta Azure, služby nebo produktu</span><span class="sxs-lookup"><span data-stu-id="569c2-137">Azure component, service, or product</span></span> |<span data-ttu-id="569c2-138">Rg pro skupinu prostředků, virtuální sítě pro virtuální síť</span><span class="sxs-lookup"><span data-stu-id="569c2-138">Rg for resource group, VNet for virtual network</span></span> |<span data-ttu-id="569c2-139">V závislosti na produkt, pro kterou prostředek podporuje.</span><span class="sxs-lookup"><span data-stu-id="569c2-139">Depending on the product for which the resource provides support.</span></span> |
| <span data-ttu-id="569c2-140">Role</span><span class="sxs-lookup"><span data-stu-id="569c2-140">Role</span></span> |<span data-ttu-id="569c2-141">DB, aplikace, webová aplikace</span><span class="sxs-lookup"><span data-stu-id="569c2-141">db, app, web</span></span> |<span data-ttu-id="569c2-142">V závislosti na roli virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="569c2-142">Depending on the role of the virtual machine.</span></span> |
| <span data-ttu-id="569c2-143">Instance</span><span class="sxs-lookup"><span data-stu-id="569c2-143">Instance</span></span> |<span data-ttu-id="569c2-144">01, 02, 03, atd.</span><span class="sxs-lookup"><span data-stu-id="569c2-144">01, 02, 03, etc.</span></span> |<span data-ttu-id="569c2-145">Pro prostředky, které mají více než jednu instanci.</span><span class="sxs-lookup"><span data-stu-id="569c2-145">For resources that have more than one instance.</span></span> <span data-ttu-id="569c2-146">Například s vyrovnáváním zatížení se webové servery v cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="569c2-146">For example, load balanced web servers in a cloud service.</span></span> |

<span data-ttu-id="569c2-147">Při vytváření zásady vytváření názvů, ujistěte se, že vyžadují, jasně uvádějí přípony, které chcete použít pro každý typ prostředku a které pozice (přípona vs předponu).</span><span class="sxs-lookup"><span data-stu-id="569c2-147">When establishing your naming conventions, make sure that they clearly state which affixes to use for each type of resource, and in which position (prefix vs suffix).</span></span>

## <a name="dates"></a><span data-ttu-id="569c2-148">kalendářní data</span><span class="sxs-lookup"><span data-stu-id="569c2-148">Dates</span></span>
<span data-ttu-id="569c2-149">Často je důležité určit datum vytvoření z názvu prostředku.</span><span class="sxs-lookup"><span data-stu-id="569c2-149">It is often important to determine the date of creation from the name of a resource.</span></span> <span data-ttu-id="569c2-150">Doporučujeme, abyste RRRRMMDD formát data.</span><span class="sxs-lookup"><span data-stu-id="569c2-150">We recommend the YYYYMMDD date format.</span></span> <span data-ttu-id="569c2-151">Tento formát zajišťuje, ne jenom celý se zaznamená datum, ale také že dva prostředky, jejichž názvy se liší pouze na data jsou seřazeny podle abecedy a časovém pořadí.</span><span class="sxs-lookup"><span data-stu-id="569c2-151">This format ensures that not only is the full date is recorded, but also that two resources whose names differ only on the date are sorted alphabetically and chronologically.</span></span>

## <a name="naming-resources"></a><span data-ttu-id="569c2-152">Názvy prostředků</span><span class="sxs-lookup"><span data-stu-id="569c2-152">Naming resources</span></span>
<span data-ttu-id="569c2-153">Definovat každý typ prostředku v zásady vytváření názvů, které by měl mít pravidla, která definují přiřazení názvy pro každý prostředek, který je vytvořen.</span><span class="sxs-lookup"><span data-stu-id="569c2-153">Define each type of resource in the naming convention, which should have rules that define how to assign names to each resource that is created.</span></span> <span data-ttu-id="569c2-154">Tato pravidla by se měly používat pro všechny typy prostředků, například:</span><span class="sxs-lookup"><span data-stu-id="569c2-154">These rules should apply to all types of resources, for example:</span></span>

* <span data-ttu-id="569c2-155">Předplatná</span><span class="sxs-lookup"><span data-stu-id="569c2-155">Subscriptions</span></span>
* <span data-ttu-id="569c2-156">Účty</span><span class="sxs-lookup"><span data-stu-id="569c2-156">Accounts</span></span>
* <span data-ttu-id="569c2-157">Účty úložiště</span><span class="sxs-lookup"><span data-stu-id="569c2-157">Storage accounts</span></span>
* <span data-ttu-id="569c2-158">Virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="569c2-158">Virtual networks</span></span>
* <span data-ttu-id="569c2-159">Podsítě</span><span class="sxs-lookup"><span data-stu-id="569c2-159">Subnets</span></span>
* <span data-ttu-id="569c2-160">Skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="569c2-160">Availability sets</span></span>
* <span data-ttu-id="569c2-161">Skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="569c2-161">Resource groups</span></span>
* <span data-ttu-id="569c2-162">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="569c2-162">Virtual machines</span></span>
* <span data-ttu-id="569c2-163">Koncové body</span><span class="sxs-lookup"><span data-stu-id="569c2-163">Endpoints</span></span>
* <span data-ttu-id="569c2-164">Skupiny zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="569c2-164">Network security groups</span></span>
* <span data-ttu-id="569c2-165">Role</span><span class="sxs-lookup"><span data-stu-id="569c2-165">Roles</span></span>

<span data-ttu-id="569c2-166">Abyste ověřili, že název poskytuje dostatek informací k určení, který prostředek se odkazuje, používejte popisné názvy.</span><span class="sxs-lookup"><span data-stu-id="569c2-166">To ensure that the name provides enough information to determine to which resource it refers, you should use descriptive names.</span></span>

## <a name="computer-names"></a><span data-ttu-id="569c2-167">Názvy počítačů</span><span class="sxs-lookup"><span data-stu-id="569c2-167">Computer names</span></span>
<span data-ttu-id="569c2-168">Při vytváření virtuálního počítače (VM) Azure vyžaduje název virtuálního počítače až 64 znaků, který se používá pro název prostředku.</span><span class="sxs-lookup"><span data-stu-id="569c2-168">When you create a virtual machine (VM), Azure requires a VM name of up to 64 characters that is used for the resource name.</span></span> <span data-ttu-id="569c2-169">Azure používá stejný název pro operační systém nainstalovaný ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="569c2-169">Azure uses the same name for the operating system installed in the VM.</span></span> <span data-ttu-id="569c2-170">Ale tyto názvy nemusí být vždy stejné.</span><span class="sxs-lookup"><span data-stu-id="569c2-170">However, these names might not always be the same.</span></span>

<span data-ttu-id="569c2-171">Pokud virtuální počítač je vytvořen ze souboru bitové kopie VHD, který již obsahuje operační systém, název virtuálního počítače v Azure se může lišit od názvu počítače operační systém Virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="569c2-171">If a VM is created from a .vhd image file that already contains an operating system, the VM name in Azure can differ from the VM's operating system computer name.</span></span> <span data-ttu-id="569c2-172">Tuto situaci můžete přidat určitý stupeň potíže ke správě virtuálních počítačů, které proto nedoporučujeme.</span><span class="sxs-lookup"><span data-stu-id="569c2-172">This situation can add a degree of difficulty to VM management, which we therefore do not recommend.</span></span> <span data-ttu-id="569c2-173">Přiřaďte prostředků Azure virtuálního počítače stejný název jako název počítače, který přiřadíte operačního systému tohoto virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="569c2-173">Assign the Azure VM resource the same name as the computer name that you assign to the operating system of that VM.</span></span>

<span data-ttu-id="569c2-174">Doporučujeme vám, že název virtuálního počítače Azure je stejný jako název počítače základní operační systém.</span><span class="sxs-lookup"><span data-stu-id="569c2-174">We recommend that the Azure VM name is the same as the underlying operating system computer name.</span></span>

## <a name="storage-account-names"></a><span data-ttu-id="569c2-175">Názvy účtů úložiště</span><span class="sxs-lookup"><span data-stu-id="569c2-175">Storage account names</span></span>
<span data-ttu-id="569c2-176">Tato část se nevztahuje na [Azure spravované disky](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), jak vytvářet účet samostatného úložiště.</span><span class="sxs-lookup"><span data-stu-id="569c2-176">This section does not apply to [Azure Managed Disks](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), as you do not create a separate storage account.</span></span> <span data-ttu-id="569c2-177">Pro nespravovaná disky účty úložiště mají zvláštní pravidla, kterými se řídí jejich názvy.</span><span class="sxs-lookup"><span data-stu-id="569c2-177">For unmanaged disks, storage accounts have special rules governing their names.</span></span> <span data-ttu-id="569c2-178">Můžete použít pouze malá písmena a číslice.</span><span class="sxs-lookup"><span data-stu-id="569c2-178">You can only use lowercase letters and numbers.</span></span> <span data-ttu-id="569c2-179">Další informace najdete v tématu [vytvořit účet úložiště](../../storage/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="569c2-179">For more information, see [Create a storage account](../../storage/storage-create-storage-account.md#create-a-storage-account).</span></span> <span data-ttu-id="569c2-180">Název účtu úložiště, s core.windows.net, kromě toho musí být globálně platný jedinečný název DNS.</span><span class="sxs-lookup"><span data-stu-id="569c2-180">Additionally, the storage account name, with core.windows.net, should be a globally valid, unique DNS name.</span></span> <span data-ttu-id="569c2-181">Například pokud účet úložiště, se nazývá můj_účet_úložiště, následující výsledné názvy DNS musí být jedinečné:</span><span class="sxs-lookup"><span data-stu-id="569c2-181">For instance, if the storage account is called mystorageaccount, the following resulting DNS names should be unique:</span></span>

* <span data-ttu-id="569c2-182">mystorageaccount.BLOB.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="569c2-182">mystorageaccount.blob.core.windows.net</span></span>
* <span data-ttu-id="569c2-183">mystorageaccount.Table.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="569c2-183">mystorageaccount.table.core.windows.net</span></span>
* <span data-ttu-id="569c2-184">mystorageaccount.Queue.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="569c2-184">mystorageaccount.queue.core.windows.net</span></span>

## <a name="next-steps"></a><span data-ttu-id="569c2-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="569c2-185">Next steps</span></span>
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

