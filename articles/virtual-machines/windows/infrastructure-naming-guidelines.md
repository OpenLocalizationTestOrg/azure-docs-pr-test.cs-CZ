---
title: "pokyny – Windows pro pojmenování infrastruktury aaaAzure | Microsoft Docs"
description: "Další informace o hello klíče návrhu a implementace pravidla pro vytváření názvů v služby infrastruktury Azure."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 660765fa-4d42-49cb-a9c6-8c596d26d221
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9b4a16ce99cf1cac5804c77675e24590ac2e2b33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-infrastructure-naming-guidelines-for-windows-vms"></a><span data-ttu-id="04b47-103">Pokyny pro pojmenování pro virtuální počítače Windows Azure infrastruktury</span><span class="sxs-lookup"><span data-stu-id="04b47-103">Azure infrastructure naming guidelines for Windows VMs</span></span>

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

<span data-ttu-id="04b47-104">Tento článek se zaměřuje na pochopení, jak nastavit tooapproach zásady vytváření názvů pro všechny vaše různých prostředků Azure toobuild a logické a snadno identifikovat prostředků ve vašem prostředí.</span><span class="sxs-lookup"><span data-stu-id="04b47-104">This article focuses on understanding how tooapproach naming conventions for all your various Azure resources toobuild a logical and easily identifiable set of resources across your environment.</span></span>

## <a name="implementation-guidelines-for-naming-conventions"></a><span data-ttu-id="04b47-105">Postup implementace pro zásady vytváření názvů</span><span class="sxs-lookup"><span data-stu-id="04b47-105">Implementation guidelines for naming conventions</span></span>
<span data-ttu-id="04b47-106">Rozhodnutí:</span><span class="sxs-lookup"><span data-stu-id="04b47-106">Decisions:</span></span>

* <span data-ttu-id="04b47-107">Jaké jsou vaše zásady vytváření názvů pro prostředky Azure?</span><span class="sxs-lookup"><span data-stu-id="04b47-107">What are your naming conventions for Azure resources?</span></span>

<span data-ttu-id="04b47-108">Úlohy:</span><span class="sxs-lookup"><span data-stu-id="04b47-108">Tasks:</span></span>

* <span data-ttu-id="04b47-109">Definujte hello přípony toouse napříč konzistence toomaintain vaše prostředky.</span><span class="sxs-lookup"><span data-stu-id="04b47-109">Define hello affixes toouse across your resources toomaintain consistency.</span></span>
* <span data-ttu-id="04b47-110">Zadejte účet úložiště, že názvy zadané hello požadavek pro ně toobe globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="04b47-110">Define storage account names given hello requirement for them toobe globally unique.</span></span>
* <span data-ttu-id="04b47-111">Dokument hello naming convention toobe používá a distribuovat tooall strany související se situací tooensure konzistence do nasazení.</span><span class="sxs-lookup"><span data-stu-id="04b47-111">Document hello naming convention toobe used and distribute tooall parties involved tooensure consistency across deployments.</span></span>

## <a name="naming-conventions"></a><span data-ttu-id="04b47-112">Zásady vytváření názvů</span><span class="sxs-lookup"><span data-stu-id="04b47-112">Naming conventions</span></span>
<span data-ttu-id="04b47-113">Dobrý zásady vytváření názvů v místě byste měli mít před vytvořením nic v Azure.</span><span class="sxs-lookup"><span data-stu-id="04b47-113">You should have a good naming convention in place before creating anything in Azure.</span></span> <span data-ttu-id="04b47-114">Zásady vytváření názvů zajistí, že všechny prostředky hello předvídatelný název, který pomáhá nižší hello administrativní zátěž související se správou těchto prostředků.</span><span class="sxs-lookup"><span data-stu-id="04b47-114">A naming convention ensures that all hello resources have a predictable name, which helps lower hello administrative burden associated with managing those resources.</span></span>

<span data-ttu-id="04b47-115">Můžete zvolit, toofollow konkrétní sadu zásady vytváření názvů definované pro celou organizaci nebo pro konkrétní předplatného Azure nebo účtu.</span><span class="sxs-lookup"><span data-stu-id="04b47-115">You might choose toofollow a specific set of naming conventions defined for your entire organization or for a specific Azure subscription or account.</span></span> <span data-ttu-id="04b47-116">I když je snadné pro jednotlivce v rámci organizace tooestablish implicitní pravidla při práci s prostředky Azure, když tým potřebuje toowork na projektu v Azure, že model nedá se jednoduše škálovat.</span><span class="sxs-lookup"><span data-stu-id="04b47-116">Although it is easy for individuals within organizations tooestablish implicit rules when working with Azure resources, when a team needs toowork on a project on Azure, that model does not scale well.</span></span>

<span data-ttu-id="04b47-117">Odsouhlasení sadu předem zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="04b47-117">Agree on a set of naming conventions up front.</span></span> <span data-ttu-id="04b47-118">Existují některé aspekty týkající se zásadami vytváření názvů vyjmout napříč této sady pravidel.</span><span class="sxs-lookup"><span data-stu-id="04b47-118">There are some considerations regarding naming conventions that cut across this set of rules.</span></span>

## <a name="affixes"></a><span data-ttu-id="04b47-119">Přípony</span><span class="sxs-lookup"><span data-stu-id="04b47-119">Affixes</span></span>
<span data-ttu-id="04b47-120">Při pohledu toodefine zásady vytváření názvů, jeden rozhodnutí obsahuje informaci, jestli hello připojí na:</span><span class="sxs-lookup"><span data-stu-id="04b47-120">As you look toodefine a naming convention, one decision comes whether hello affix is at:</span></span>

* <span data-ttu-id="04b47-121">začátek Hello hello názvu (předpona)</span><span class="sxs-lookup"><span data-stu-id="04b47-121">hello beginning of hello name (prefix)</span></span>
* <span data-ttu-id="04b47-122">Hello konci názvu hello (přípona)</span><span class="sxs-lookup"><span data-stu-id="04b47-122">hello end of hello name (suffix)</span></span>

<span data-ttu-id="04b47-123">Například tady jsou dvě možné názvy pro skupinu prostředků pomocí hello `rg` opatří:</span><span class="sxs-lookup"><span data-stu-id="04b47-123">For instance, here are two possible names for a Resource Group using hello `rg` affix:</span></span>

* <span data-ttu-id="04b47-124">Rg-WebApp (předpona)</span><span class="sxs-lookup"><span data-stu-id="04b47-124">Rg-WebApp (prefix)</span></span>
* <span data-ttu-id="04b47-125">WebApp-Rg (přípona)</span><span class="sxs-lookup"><span data-stu-id="04b47-125">WebApp-Rg (suffix)</span></span>

<span data-ttu-id="04b47-126">Přípony najdete toodifferent aspekty, které popisují hello konkrétní prostředky.</span><span class="sxs-lookup"><span data-stu-id="04b47-126">Affixes can refer toodifferent aspects that describe hello particular resources.</span></span> <span data-ttu-id="04b47-127">Hello následující tabulka uvádí některé příklady obvykle používá.</span><span class="sxs-lookup"><span data-stu-id="04b47-127">hello following table shows some examples typically used.</span></span>

| <span data-ttu-id="04b47-128">Aspekt</span><span class="sxs-lookup"><span data-stu-id="04b47-128">Aspect</span></span> | <span data-ttu-id="04b47-129">Příklady</span><span class="sxs-lookup"><span data-stu-id="04b47-129">Examples</span></span> | <span data-ttu-id="04b47-130">Poznámky</span><span class="sxs-lookup"><span data-stu-id="04b47-130">Notes</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="04b47-131">Prostředí</span><span class="sxs-lookup"><span data-stu-id="04b47-131">Environment</span></span> |<span data-ttu-id="04b47-132">vývoj, stg, produkčního</span><span class="sxs-lookup"><span data-stu-id="04b47-132">dev, stg, prod</span></span> |<span data-ttu-id="04b47-133">V závislosti na účelu hello a název každé prostředí.</span><span class="sxs-lookup"><span data-stu-id="04b47-133">Depending on hello purpose and name of each environment.</span></span> |
| <span data-ttu-id="04b47-134">Umístění</span><span class="sxs-lookup"><span data-stu-id="04b47-134">Location</span></span> |<span data-ttu-id="04b47-135">usw (západní USA), použijte (východní USA 2)</span><span class="sxs-lookup"><span data-stu-id="04b47-135">usw (West US), use (East US 2)</span></span> |<span data-ttu-id="04b47-136">V závislosti na oblast hello hello datacenter nebo oblasti hello hello organizace.</span><span class="sxs-lookup"><span data-stu-id="04b47-136">Depending on hello region of hello datacenter or hello region of hello organization.</span></span> |
| <span data-ttu-id="04b47-137">Komponenta Azure, služby nebo produktu</span><span class="sxs-lookup"><span data-stu-id="04b47-137">Azure component, service, or product</span></span> |<span data-ttu-id="04b47-138">Rg pro skupinu prostředků, virtuální sítě pro virtuální síť</span><span class="sxs-lookup"><span data-stu-id="04b47-138">Rg for resource group, VNet for virtual network</span></span> |<span data-ttu-id="04b47-139">V závislosti na hello produktů, pro které hello prostředků poskytuje podporu.</span><span class="sxs-lookup"><span data-stu-id="04b47-139">Depending on hello product for which hello resource provides support.</span></span> |
| <span data-ttu-id="04b47-140">Role</span><span class="sxs-lookup"><span data-stu-id="04b47-140">Role</span></span> |<span data-ttu-id="04b47-141">SQL, ora, sp, služby iis</span><span class="sxs-lookup"><span data-stu-id="04b47-141">sql, ora, sp, iis</span></span> |<span data-ttu-id="04b47-142">V závislosti na roli hello hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="04b47-142">Depending on hello role of hello virtual machine.</span></span> |
| <span data-ttu-id="04b47-143">Instance</span><span class="sxs-lookup"><span data-stu-id="04b47-143">Instance</span></span> |<span data-ttu-id="04b47-144">01, 02, 03, atd.</span><span class="sxs-lookup"><span data-stu-id="04b47-144">01, 02, 03, etc.</span></span> |<span data-ttu-id="04b47-145">Pro prostředky, které mají více než jednu instanci.</span><span class="sxs-lookup"><span data-stu-id="04b47-145">For resources that have more than one instance.</span></span> <span data-ttu-id="04b47-146">Například s vyrovnáváním zatížení se webové servery v cloudové službě.</span><span class="sxs-lookup"><span data-stu-id="04b47-146">For example, load balanced web servers in a cloud service.</span></span> |

<span data-ttu-id="04b47-147">Při vytváření zásady vytváření názvů, ujistěte se, že v vyžadují, jasně uvádějí který opatří toouse pro každý typ prostředku a které pozice (přípona vs předponu).</span><span class="sxs-lookup"><span data-stu-id="04b47-147">When establishing your naming conventions, make sure that they clearly state which affixes toouse for each type of resource, and in which position (prefix vs suffix).</span></span>

## <a name="dates"></a><span data-ttu-id="04b47-148">kalendářní data</span><span class="sxs-lookup"><span data-stu-id="04b47-148">Dates</span></span>
<span data-ttu-id="04b47-149">Často je důležité toodetermine hello datum vytvoření z hello název prostředku.</span><span class="sxs-lookup"><span data-stu-id="04b47-149">It is often important toodetermine hello date of creation from hello name of a resource.</span></span> <span data-ttu-id="04b47-150">Doporučujeme formát data RRRRMMDD hello.</span><span class="sxs-lookup"><span data-stu-id="04b47-150">We recommend hello YYYYMMDD date format.</span></span> <span data-ttu-id="04b47-151">Tento formát zajišťuje, že pouze úplné datum hello zaznamená, ale také že dva prostředky, jejichž názvy se liší pouze na datum hello je seřazen abecedně a časovém pořadí v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="04b47-151">This format ensures that not only hello full date is recorded, but also that two resources whose names differ only on hello date is sorted alphabetically and chronologically at hello same time.</span></span>

## <a name="naming-resources"></a><span data-ttu-id="04b47-152">Názvy prostředků</span><span class="sxs-lookup"><span data-stu-id="04b47-152">Naming resources</span></span>
<span data-ttu-id="04b47-153">Definování každý typ prostředku v hello zásady vytváření názvů, které by měl mít pravidla, která definují, jak tooassign názvy prostředků tooeach který je vytvořen.</span><span class="sxs-lookup"><span data-stu-id="04b47-153">Define each type of resource in hello naming convention, which should have rules that define how tooassign names tooeach resource that is created.</span></span> <span data-ttu-id="04b47-154">Tato pravidla by se měly používat tooall typů prostředků, například:</span><span class="sxs-lookup"><span data-stu-id="04b47-154">These rules should apply tooall types of resources, for example:</span></span>

* <span data-ttu-id="04b47-155">Předplatná</span><span class="sxs-lookup"><span data-stu-id="04b47-155">Subscriptions</span></span>
* <span data-ttu-id="04b47-156">Účty</span><span class="sxs-lookup"><span data-stu-id="04b47-156">Accounts</span></span>
* <span data-ttu-id="04b47-157">Účty úložiště</span><span class="sxs-lookup"><span data-stu-id="04b47-157">Storage accounts</span></span>
* <span data-ttu-id="04b47-158">Virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="04b47-158">Virtual networks</span></span>
* <span data-ttu-id="04b47-159">Podsítě</span><span class="sxs-lookup"><span data-stu-id="04b47-159">Subnets</span></span>
* <span data-ttu-id="04b47-160">Skupiny dostupnosti</span><span class="sxs-lookup"><span data-stu-id="04b47-160">Availability sets</span></span>
* <span data-ttu-id="04b47-161">Skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="04b47-161">Resource groups</span></span>
* <span data-ttu-id="04b47-162">Virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="04b47-162">Virtual machines</span></span>
* <span data-ttu-id="04b47-163">Koncové body</span><span class="sxs-lookup"><span data-stu-id="04b47-163">Endpoints</span></span>
* <span data-ttu-id="04b47-164">Skupiny zabezpečení sítě</span><span class="sxs-lookup"><span data-stu-id="04b47-164">Network security groups</span></span>
* <span data-ttu-id="04b47-165">Role</span><span class="sxs-lookup"><span data-stu-id="04b47-165">Roles</span></span>

<span data-ttu-id="04b47-166">tooensure, který hello název poskytuje dostatek prostředků toowhich toodetermine informace, které se odkazuje, byste měli používat popisné názvy.</span><span class="sxs-lookup"><span data-stu-id="04b47-166">tooensure that hello name provides enough information toodetermine toowhich resource it refers, you should use descriptive names.</span></span>

## <a name="computer-names"></a><span data-ttu-id="04b47-167">Názvy počítačů</span><span class="sxs-lookup"><span data-stu-id="04b47-167">Computer names</span></span>
<span data-ttu-id="04b47-168">Když vytvoříte virtuální počítač (VM), Microsoft Azure vyžaduje název virtuálního počítače z až too15 znaků, který se používá pro název prostředku hello.</span><span class="sxs-lookup"><span data-stu-id="04b47-168">When you create a virtual machine (VM), Microsoft Azure requires a VM name of up too15 characters which is used for hello resource name.</span></span> <span data-ttu-id="04b47-169">Azure používá hello pro hello operační systém nainstalovaný v hello virtuálního počítače stejný název.</span><span class="sxs-lookup"><span data-stu-id="04b47-169">Azure uses hello same name for hello operating system installed in hello VM.</span></span> <span data-ttu-id="04b47-170">Ale tyto názvy nemusí vždy být hello stejné.</span><span class="sxs-lookup"><span data-stu-id="04b47-170">However, these names might not always be hello same.</span></span>

<span data-ttu-id="04b47-171">V případě, že virtuální počítač je vytvořen ze souboru bitové kopie VHD, který již obsahuje operační systém, hello název virtuálního počítače v Azure se může lišit od hello název počítače operační systém Virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="04b47-171">In case a VM is created from a .vhd image file that already contains an operating system, hello VM name in Azure can differ from hello VM's operating system computer name.</span></span> <span data-ttu-id="04b47-172">Tuto situaci můžete přidat určitý stupeň potíže tooVM správu, který proto nedoporučujeme.</span><span class="sxs-lookup"><span data-stu-id="04b47-172">This situation can add a degree of difficulty tooVM management, which we therefore do not recommend.</span></span> <span data-ttu-id="04b47-173">Přiřaďte hello hello prostředků virtuálního počítače Azure stejný název jako název počítače hello přiřadit toohello operačního systému tohoto virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="04b47-173">Assign hello Azure VM resource hello same name as hello computer name that you assign toohello operating system of that VM.</span></span>

<span data-ttu-id="04b47-174">Doporučujeme vám, že název virtuálního počítače Azure hello je hello stejné jako hello základní název operačního systému počítače.</span><span class="sxs-lookup"><span data-stu-id="04b47-174">We recommend that hello Azure VM name is hello same as hello underlying operating system computer name.</span></span>

## <a name="storage-account-names"></a><span data-ttu-id="04b47-175">Názvy účtů úložiště</span><span class="sxs-lookup"><span data-stu-id="04b47-175">Storage account names</span></span>
<span data-ttu-id="04b47-176">Tato část se nevztahuje příliš[Azure spravované disky](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), jak vytvářet účet samostatného úložiště.</span><span class="sxs-lookup"><span data-stu-id="04b47-176">This section does not apply too[Azure Managed Disks](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), as you do not create a separate storage account.</span></span> <span data-ttu-id="04b47-177">Pro nespravovaná disky účty úložiště mají zvláštní pravidla, kterými se řídí jejich názvy.</span><span class="sxs-lookup"><span data-stu-id="04b47-177">For unmanaged disks, storage accounts have special rules governing their names.</span></span> <span data-ttu-id="04b47-178">Můžete použít pouze malá písmena a číslice.</span><span class="sxs-lookup"><span data-stu-id="04b47-178">You can only use lowercase letters and numbers.</span></span> <span data-ttu-id="04b47-179">Další informace najdete v tématu [vytvořit účet úložiště](../../storage/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="04b47-179">For more information, see [Create a storage account](../../storage/storage-create-storage-account.md#create-a-storage-account).</span></span> <span data-ttu-id="04b47-180">Kromě toho hello název účtu úložiště, společně s core.windows.net, musí být globálně platný jedinečný název DNS.</span><span class="sxs-lookup"><span data-stu-id="04b47-180">Additionally, hello storage account name, along with core.windows.net, should be a globally valid, unique DNS name.</span></span> <span data-ttu-id="04b47-181">Například pokud účet úložiště hello nazývá můj_účet_úložiště, hello následující výsledné názvy DNS musí být jedinečné:</span><span class="sxs-lookup"><span data-stu-id="04b47-181">For instance, if hello storage account is called mystorageaccount, hello following resulting DNS names should be unique:</span></span>

* <span data-ttu-id="04b47-182">mystorageaccount.BLOB.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="04b47-182">mystorageaccount.blob.core.windows.net</span></span>
* <span data-ttu-id="04b47-183">mystorageaccount.Table.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="04b47-183">mystorageaccount.table.core.windows.net</span></span>
* <span data-ttu-id="04b47-184">mystorageaccount.Queue.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="04b47-184">mystorageaccount.queue.core.windows.net</span></span>

## <a name="next-steps"></a><span data-ttu-id="04b47-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="04b47-185">Next steps</span></span>
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]

