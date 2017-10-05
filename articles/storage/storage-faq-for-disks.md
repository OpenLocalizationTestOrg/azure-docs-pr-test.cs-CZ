---
title: "Nejčastější dotazy (FAQ) o disky virtuálních počítačů Azure IaaS | Microsoft Docs"
description: "Nejčastější dotazy týkající se disky virtuálního počítače Azure IaaS a prémiové disky (spravovaných a nespravovaných)"
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: e2a20625-6224-4187-8401-abadc8f1de91
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: robinsh
ms.openlocfilehash: 9e5eed35334f1b95441b8181c8e90645be78b389
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="frequently-asked-questions-about-azure-iaas-vm-disks-and-managed-and-unmanaged-premium-disks"></a><span data-ttu-id="5240a-103">Nejčastější dotazy týkající se disky virtuálního počítače Azure IaaS a spravovanými a nespravovanými prémiové disky</span><span class="sxs-lookup"><span data-stu-id="5240a-103">Frequently asked questions about Azure IaaS VM disks and managed and unmanaged premium disks</span></span>

<span data-ttu-id="5240a-104">Tento článek obsahuje odpovědi na některé nejčastější dotazy týkající se Azure spravované disky a Azure Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="5240a-104">This article answers some frequently asked questions about Azure Managed Disks and Azure Premium Storage.</span></span>

## <a name="managed-disks"></a><span data-ttu-id="5240a-105">Managed Disks</span><span class="sxs-lookup"><span data-stu-id="5240a-105">Managed Disks</span></span>

<span data-ttu-id="5240a-106">**Co je Azure spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="5240a-106">**What is Azure Managed Disks?**</span></span>

<span data-ttu-id="5240a-107">Spravované disků je funkce, která zjednodušuje Správa disku pro virtuální počítače Azure IaaS tím, že zpracování Správa účtů úložiště pro vás.</span><span class="sxs-lookup"><span data-stu-id="5240a-107">Managed Disks is a feature that simplifies disk management for Azure IaaS VMs by handling storage account management for you.</span></span> <span data-ttu-id="5240a-108">Další informace najdete v tématu [spravované disky přehled](storage-managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5240a-108">For more information, see the [Managed Disks overview](storage-managed-disks-overview.md).</span></span>

<span data-ttu-id="5240a-109">**Když vytvořím standardní se spravovaným diskem z existujícího VHD, který je 80 GB, jaké budou který náklady mi?**</span><span class="sxs-lookup"><span data-stu-id="5240a-109">**If I create a standard managed disk from an existing VHD that's 80 GB, how much will that cost me?**</span></span>

<span data-ttu-id="5240a-110">Standardní se spravovaným diskem vytvořen z disku VHD 80 GB je považována za další velikost standardní disku, která je k dispozici S10 disk.</span><span class="sxs-lookup"><span data-stu-id="5240a-110">A standard managed disk created from an 80-GB VHD is treated as the next available standard disk size, which is an S10 disk.</span></span> <span data-ttu-id="5240a-111">Že se vám účtovat podle S10 disku ceny.</span><span class="sxs-lookup"><span data-stu-id="5240a-111">You're charged according to the S10 disk pricing.</span></span> <span data-ttu-id="5240a-112">Další informace najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="5240a-112">For more information, see the [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="5240a-113">**Existují žádné transakce náklady na standardní spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="5240a-113">**Are there any transaction costs for standard managed disks?**</span></span>

<span data-ttu-id="5240a-114">Ano.</span><span class="sxs-lookup"><span data-stu-id="5240a-114">Yes.</span></span> <span data-ttu-id="5240a-115">Vám účtovat každou transakci.</span><span class="sxs-lookup"><span data-stu-id="5240a-115">You're charged for each transaction.</span></span> <span data-ttu-id="5240a-116">Další informace najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="5240a-116">For more information, see the [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="5240a-117">**Pro standardní se spravovaným diskem I odečte skutečná velikost dat na disku nebo zřízená kapacita disku?**</span><span class="sxs-lookup"><span data-stu-id="5240a-117">**For a standard managed disk, will I be charged for the actual size of the data on the disk or for the provisioned capacity of the disk?**</span></span>

<span data-ttu-id="5240a-118">Že se vám účtovat podle zřízená kapacita disku.</span><span class="sxs-lookup"><span data-stu-id="5240a-118">You're charged based on the provisioned capacity of the disk.</span></span> <span data-ttu-id="5240a-119">Další informace najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="5240a-119">For more information, see the [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="5240a-120">**Jak je ceny disků premium spravované liší od nespravované disky?**</span><span class="sxs-lookup"><span data-stu-id="5240a-120">**How is pricing of premium managed disks different from unmanaged disks?**</span></span>

<span data-ttu-id="5240a-121">Ceny spravované prémiové disky je stejný jako nespravované prémiové disky.</span><span class="sxs-lookup"><span data-stu-id="5240a-121">The pricing of premium managed disks is the same as unmanaged premium disks.</span></span>

<span data-ttu-id="5240a-122">**Můžete změnit typ účtu úložiště (Standard nebo Premium) Moje spravovaných disků?**</span><span class="sxs-lookup"><span data-stu-id="5240a-122">**Can I change the storage account type (Standard or Premium) of my managed disks?**</span></span>

<span data-ttu-id="5240a-123">Ano.</span><span class="sxs-lookup"><span data-stu-id="5240a-123">Yes.</span></span> <span data-ttu-id="5240a-124">Typ účtu úložiště spravovaného disky můžete změnit pomocí portálu Azure, PowerShell nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="5240a-124">You can change the storage account type of your managed disks by using the Azure portal, PowerShell, or the Azure CLI.</span></span>

<span data-ttu-id="5240a-125">**Existuje způsob, že umožní kopírování a exportovat spravovaných disků na účet privátní úložiště?**</span><span class="sxs-lookup"><span data-stu-id="5240a-125">**Is there a way that I can copy or export a managed disk to a private storage account?**</span></span>

<span data-ttu-id="5240a-126">Ano.</span><span class="sxs-lookup"><span data-stu-id="5240a-126">Yes.</span></span> <span data-ttu-id="5240a-127">Vaše spravované disky můžete exportovat pomocí portálu Azure, PowerShell nebo rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="5240a-127">You can export your managed disks by using the Azure portal, PowerShell, or the Azure CLI.</span></span>

<span data-ttu-id="5240a-128">**Můžete použít soubor virtuálního pevného disku v účtu úložiště Azure k vytvoření spravovaného disk pomocí jiné předplatné?**</span><span class="sxs-lookup"><span data-stu-id="5240a-128">**Can I use a VHD file in an Azure storage account to create a managed disk with a different subscription?**</span></span>

<span data-ttu-id="5240a-129">Ne.</span><span class="sxs-lookup"><span data-stu-id="5240a-129">No.</span></span>

<span data-ttu-id="5240a-130">**Můžete použít soubor virtuálního pevného disku v účtu úložiště Azure k vytvoření spravovaného disku v jiné oblasti?**</span><span class="sxs-lookup"><span data-stu-id="5240a-130">**Can I use a VHD file in an Azure storage account to create a managed disk in a different region?**</span></span>

<span data-ttu-id="5240a-131">Ne.</span><span class="sxs-lookup"><span data-stu-id="5240a-131">No.</span></span>

<span data-ttu-id="5240a-132">**Existují nějaká omezení škálování pro zákazníky, kteří používají spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="5240a-132">**Are there any scale limitations for customers that use managed disks?**</span></span>

<span data-ttu-id="5240a-133">Spravované disky eliminuje omezení spojená s účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="5240a-133">Managed Disks eliminates the limits associated with storage accounts.</span></span> <span data-ttu-id="5240a-134">Počet spravovaných disků na jedno předplatné je však omezená na 2 000 ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="5240a-134">However, the number of managed disks per subscription is limited to 2,000 by default.</span></span> <span data-ttu-id="5240a-135">Obraťte se na podporu, aby tento počet zvýšit.</span><span class="sxs-lookup"><span data-stu-id="5240a-135">You can call support to increase this number.</span></span>

<span data-ttu-id="5240a-136">**Může trvat přírůstkový snímek spravovaného disku?**</span><span class="sxs-lookup"><span data-stu-id="5240a-136">**Can I take an incremental snapshot of a managed disk?**</span></span>

<span data-ttu-id="5240a-137">Ne.</span><span class="sxs-lookup"><span data-stu-id="5240a-137">No.</span></span> <span data-ttu-id="5240a-138">Aktuální vytváření snímků provede úplnou kopii se spravovaným diskem.</span><span class="sxs-lookup"><span data-stu-id="5240a-138">The current snapshot capability makes a full copy of a managed disk.</span></span> <span data-ttu-id="5240a-139">Doporučujeme však hodláte podporují přírůstkové snímky v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="5240a-139">However, we are planning to support incremental snapshots in the future.</span></span>

<span data-ttu-id="5240a-140">**Virtuální počítače v nastavení dostupnosti se může skládat z kombinace spravovanými a nespravovanými disky?**</span><span class="sxs-lookup"><span data-stu-id="5240a-140">**Can VMs in an availability set consist of a combination of managed and unmanaged disks?**</span></span>

<span data-ttu-id="5240a-141">Ne.</span><span class="sxs-lookup"><span data-stu-id="5240a-141">No.</span></span> <span data-ttu-id="5240a-142">Virtuální počítače v nastavení dostupnosti musí používat disky všechny spravované nebo nespravované všechny disky.</span><span class="sxs-lookup"><span data-stu-id="5240a-142">The VMs in an availability set must use either all managed disks or all unmanaged disks.</span></span> <span data-ttu-id="5240a-143">Když vytvoříte skupinu dostupnosti, můžete typu disky, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="5240a-143">When you create an availability set, you can choose which type of disks you want to use.</span></span>

<span data-ttu-id="5240a-144">**Spravované disků je výchozí možnost na portálu Azure?**</span><span class="sxs-lookup"><span data-stu-id="5240a-144">**Is Managed Disks the default option in the Azure portal?**</span></span>

<span data-ttu-id="5240a-145">Není v současné době ale výchozí hodnota se stane v budoucnu.</span><span class="sxs-lookup"><span data-stu-id="5240a-145">Not currently, but it will become the default in the future.</span></span>

<span data-ttu-id="5240a-146">**Můžete vytvořit prázdný disk spravované?**</span><span class="sxs-lookup"><span data-stu-id="5240a-146">**Can I create an empty managed disk?**</span></span>

<span data-ttu-id="5240a-147">Ano.</span><span class="sxs-lookup"><span data-stu-id="5240a-147">Yes.</span></span> <span data-ttu-id="5240a-148">Můžete vytvořit prázdný disk.</span><span class="sxs-lookup"><span data-stu-id="5240a-148">You can create an empty disk.</span></span> <span data-ttu-id="5240a-149">Spravovaný disk můžete vytvořit nezávisle na virtuální počítač, například bez připojení k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="5240a-149">A managed disk can be created independently of a VM, for example, without attaching it to a VM.</span></span>

<span data-ttu-id="5240a-150">**Co je počet domén selhání podporované pro dostupnosti nastavit používající spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="5240a-150">**What is the supported fault domain count for an availability set that uses Managed Disks?**</span></span>

<span data-ttu-id="5240a-151">V závislosti na oblasti, kde se nachází skupiny dostupnosti, který používá disky spravovat je počet domén selhání podporované 2 nebo 3.</span><span class="sxs-lookup"><span data-stu-id="5240a-151">Depending on the region where the availability set that uses Managed Disks is located, the supported fault domain count is 2 or 3.</span></span>

<span data-ttu-id="5240a-152">**Jak je účet standardního úložiště pro diagnostiku nastavení?**</span><span class="sxs-lookup"><span data-stu-id="5240a-152">**How is the standard storage account for diagnostics set up?**</span></span>

<span data-ttu-id="5240a-153">Nastavit účet privátní úložiště pro diagnostiku virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5240a-153">You set up a private storage account for VM diagnostics.</span></span> <span data-ttu-id="5240a-154">V budoucnu plánujeme také přepnout diagnostiky spravované disky.</span><span class="sxs-lookup"><span data-stu-id="5240a-154">In the future, we plan to switch diagnostics to Managed Disks as well.</span></span>

<span data-ttu-id="5240a-155">**Jaký druh řízení přístupu na základě Role podpora je k dispozici pro spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="5240a-155">**What kind of Role-Based Access Control support is available for Managed Disks?**</span></span>

<span data-ttu-id="5240a-156">Spravované disky podporuje tři klíčové výchozích rolí:</span><span class="sxs-lookup"><span data-stu-id="5240a-156">Managed Disks supports three key default roles:</span></span>

* <span data-ttu-id="5240a-157">Vlastník: Můžou spravovat všechno včetně přístupu</span><span class="sxs-lookup"><span data-stu-id="5240a-157">Owner: Can manage everything, including access</span></span>
* <span data-ttu-id="5240a-158">Přispěvatel: Můžou spravovat všechno kromě přístupu</span><span class="sxs-lookup"><span data-stu-id="5240a-158">Contributor: Can manage everything except access</span></span>
* <span data-ttu-id="5240a-159">Čtečka: Zobrazit vše, co, ale nelze provádět změny</span><span class="sxs-lookup"><span data-stu-id="5240a-159">Reader: Can view everything, but can't make changes</span></span>

<span data-ttu-id="5240a-160">**Existuje způsob, že umožní kopírování a exportovat spravovaných disků na účet privátní úložiště?**</span><span class="sxs-lookup"><span data-stu-id="5240a-160">**Is there a way that I can copy or export a managed disk to a private storage account?**</span></span>

<span data-ttu-id="5240a-161">Můžete získat jen pro čtení sdíleného přístupového podpisu identifikátor URI pro spravované disk a použít ho zkopírovat obsah do privátní úložiště účet nebo místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="5240a-161">You can get a read-only shared access signature URI for the managed disk and use it to copy the contents to a private storage account or on-premises storage.</span></span>

<span data-ttu-id="5240a-162">**Můžete vytvořit kopii Moje spravovaného disku?**</span><span class="sxs-lookup"><span data-stu-id="5240a-162">**Can I create a copy of my managed disk?**</span></span>

<span data-ttu-id="5240a-163">Zákazníci můžete pořízení snímku jejich spravované disky a potom pomocí snímku vytvoření spravovaných disků na jiný.</span><span class="sxs-lookup"><span data-stu-id="5240a-163">Customers can take a snapshot of their managed disks and then use the snapshot to create another managed disk.</span></span>

<span data-ttu-id="5240a-164">**Jsou nespravované disky stále podporovány?**</span><span class="sxs-lookup"><span data-stu-id="5240a-164">**Are unmanaged disks still supported?**</span></span>

<span data-ttu-id="5240a-165">Ano.</span><span class="sxs-lookup"><span data-stu-id="5240a-165">Yes.</span></span> <span data-ttu-id="5240a-166">Podporujeme nespravované a spravované disky.</span><span class="sxs-lookup"><span data-stu-id="5240a-166">We support unmanaged and managed disks.</span></span> <span data-ttu-id="5240a-167">Doporučujeme používat spravované disky pro nové úlohy a migraci vašich aktuální zatížení na spravované disky.</span><span class="sxs-lookup"><span data-stu-id="5240a-167">We recommend that you use managed disks for new workloads and migrate your current workloads to managed disks.</span></span>


<span data-ttu-id="5240a-168">**Je-li vytvořit 128 GB disk a poté zvýšit velikost 130 GB, bude I vám účtována další velikost disku (512 GB)?**</span><span class="sxs-lookup"><span data-stu-id="5240a-168">**If I create a 128-GB disk and then increase the size to 130 GB, will I be charged for the next disk size (512 GB)?**</span></span>

<span data-ttu-id="5240a-169">Ano.</span><span class="sxs-lookup"><span data-stu-id="5240a-169">Yes.</span></span>

<span data-ttu-id="5240a-170">**Můžete vytvořit místně redundantní úložiště, geograficky redundantní úložiště, a zónově redundantní úložiště spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="5240a-170">**Can I create locally redundant storage, geo-redundant storage, and zone-redundant storage managed disks?**</span></span>

<span data-ttu-id="5240a-171">Disky systému Azure spravované aktuálně podporuje pouze místně redundantního úložiště spravované disky.</span><span class="sxs-lookup"><span data-stu-id="5240a-171">Azure Managed Disks currently supports only locally redundant storage managed disks.</span></span>

<span data-ttu-id="5240a-172">**Můžete zmenšit nebo downsize Moje spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="5240a-172">**Can I shrink or downsize my managed disks?**</span></span>

<span data-ttu-id="5240a-173">Ne.</span><span class="sxs-lookup"><span data-stu-id="5240a-173">No.</span></span> <span data-ttu-id="5240a-174">Tato funkce není aktuálně podporován.</span><span class="sxs-lookup"><span data-stu-id="5240a-174">This feature is not supported currently.</span></span> 

<span data-ttu-id="5240a-175">**Můžete změnit vlastnost název počítače při specializované (ne vytvořili pomocí nástroje pro přípravu systému nebo zobecněn) disk operačního systému slouží ke zřízení virtuálního počítače?**</span><span class="sxs-lookup"><span data-stu-id="5240a-175">**Can I change the computer name property when a specialized (not created by using the System Preparation tool or generalized) operating system disk is used to provision a VM?**</span></span>

<span data-ttu-id="5240a-176">Ne.</span><span class="sxs-lookup"><span data-stu-id="5240a-176">No.</span></span> <span data-ttu-id="5240a-177">Nelze aktualizovat vlastnosti název počítače.</span><span class="sxs-lookup"><span data-stu-id="5240a-177">You can't update the computer name property.</span></span> <span data-ttu-id="5240a-178">Nový virtuální počítač se dědí z nadřazený virtuální počítač, který byl použit k vytvoření disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="5240a-178">The new VM inherits it from the parent VM, which was used to create the operating system disk.</span></span> 

<span data-ttu-id="5240a-179">**Kde najdu ukázkových šablon Azure Resource Manageru k vytvoření virtuálních počítačů s spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="5240a-179">**Where can I find sample Azure Resource Manager templates to create VMs with managed disks?**</span></span>
* [<span data-ttu-id="5240a-180">Seznam šablon pomocí spravovaných disků</span><span class="sxs-lookup"><span data-stu-id="5240a-180">List of templates using Managed Disks</span></span>](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* <span data-ttu-id="5240a-181">https://github.com/chagarw/MDPP</span><span class="sxs-lookup"><span data-stu-id="5240a-181">https://github.com/chagarw/MDPP</span></span>

## <a name="managed-disks-and-storage-service-encryption"></a><span data-ttu-id="5240a-182">Spravované disky a šifrování služby úložiště</span><span class="sxs-lookup"><span data-stu-id="5240a-182">Managed Disks and Storage Service Encryption</span></span> 

<span data-ttu-id="5240a-183">**Šifrování služby úložiště Azure ve výchozím nastavení zapnutá po vytvoření se spravovaným diskem?**</span><span class="sxs-lookup"><span data-stu-id="5240a-183">**Is Azure Storage Service Encryption enabled by default when I create a managed disk?**</span></span>

<span data-ttu-id="5240a-184">Ano.</span><span class="sxs-lookup"><span data-stu-id="5240a-184">Yes.</span></span>

<span data-ttu-id="5240a-185">**Kdo spravuje šifrovacích klíčů?**</span><span class="sxs-lookup"><span data-stu-id="5240a-185">**Who manages the encryption keys?**</span></span>

<span data-ttu-id="5240a-186">Microsoft spravuje šifrovací klíče.</span><span class="sxs-lookup"><span data-stu-id="5240a-186">Microsoft manages the encryption keys.</span></span>

<span data-ttu-id="5240a-187">**Můžete zakázat šifrování služby úložiště pro moje spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="5240a-187">**Can I disable Storage Service Encryption for my managed disks?**</span></span>

<span data-ttu-id="5240a-188">Ne.</span><span class="sxs-lookup"><span data-stu-id="5240a-188">No.</span></span>

<span data-ttu-id="5240a-189">**Je šifrování služby úložiště k dispozici pouze v určitých oblastí?**</span><span class="sxs-lookup"><span data-stu-id="5240a-189">**Is Storage Service Encryption only available in specific regions?**</span></span>

<span data-ttu-id="5240a-190">Ne.</span><span class="sxs-lookup"><span data-stu-id="5240a-190">No.</span></span> <span data-ttu-id="5240a-191">Je k dispozici ve všech oblastech, kde je k dispozici spravované disky.</span><span class="sxs-lookup"><span data-stu-id="5240a-191">It's available in all the regions where Managed Disks is available.</span></span> <span data-ttu-id="5240a-192">Spravované disků je k dispozici ve všech veřejných oblastí a Německu.</span><span class="sxs-lookup"><span data-stu-id="5240a-192">Managed Disks is available in all public regions and Germany.</span></span>

<span data-ttu-id="5240a-193">**Jak můžete zjistit, pokud je zašifrovaná Moje spravovaných disků?**</span><span class="sxs-lookup"><span data-stu-id="5240a-193">**How can I find out if my managed disk is encrypted?**</span></span>

<span data-ttu-id="5240a-194">Můžete získat čas vytvoření spravovaného disku z portálu Azure, rozhraní příkazového řádku Azure a prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5240a-194">You can find out the time when a managed disk was created from the Azure portal, the Azure CLI, and PowerShell.</span></span> <span data-ttu-id="5240a-195">Pokud je doba po 9. června 2017, se šifrují na disku.</span><span class="sxs-lookup"><span data-stu-id="5240a-195">If the time is after June 9, 2017, then your disk is encrypted.</span></span> 

<span data-ttu-id="5240a-196">**Jak můžete šifrovat Můj stávající disky, které byly vytvořeny před 10 června 2017?**</span><span class="sxs-lookup"><span data-stu-id="5240a-196">**How can I encrypt my existing disks that were created before June 10, 2017?**</span></span>

<span data-ttu-id="5240a-197">Od verze 10 června 2017 je nová data stávající spravovaný disky šifrují automaticky.</span><span class="sxs-lookup"><span data-stu-id="5240a-197">As of June 10, 2017, new data written to existing managed disks is automatically encrypted.</span></span> <span data-ttu-id="5240a-198">Můžeme také plánování šifrování stávající data a šifrování asynchronně proběhne na pozadí.</span><span class="sxs-lookup"><span data-stu-id="5240a-198">We are also planning to encrypt existing data, and the encryption will happen asynchronously in the background.</span></span> <span data-ttu-id="5240a-199">Pokud je nutné šifrovat všechna existující data, vytvoří se kopie disku.</span><span class="sxs-lookup"><span data-stu-id="5240a-199">If you must encrypt existing data now, create a copy of your disk.</span></span> <span data-ttu-id="5240a-200">Bude se šifrovat nové disky.</span><span class="sxs-lookup"><span data-stu-id="5240a-200">New disks will be encrypted.</span></span>

* [<span data-ttu-id="5240a-201">Zkopírovat disky spravované pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="5240a-201">Copy managed disks by using the Azure CLI</span></span>](https://docs.microsoft.com/en-us/azure/storage/scripts/storage-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription?toc=%2fcli%2fmodule%2ftoc.json)
* [<span data-ttu-id="5240a-202">Zkopírujte spravované disky pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="5240a-202">Copy managed disks by using PowerShell</span></span>](https://docs.microsoft.com/en-us/azure/storage/scripts/storage-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription?toc=%2fcli%2fmodule%2ftoc.json)

<span data-ttu-id="5240a-203">**Jsou spravované snímky a bitové kopie zašifrovaná?**</span><span class="sxs-lookup"><span data-stu-id="5240a-203">**Are managed snapshots and images encrypted?**</span></span>

<span data-ttu-id="5240a-204">Ano.</span><span class="sxs-lookup"><span data-stu-id="5240a-204">Yes.</span></span> <span data-ttu-id="5240a-205">Všechny spravované snímky a bitové kopie vytvořené po 9. června 2017, se šifrují automaticky.</span><span class="sxs-lookup"><span data-stu-id="5240a-205">All managed snapshots and images created after June 9, 2017, are automatically encrypted.</span></span> 

<span data-ttu-id="5240a-206">**Můžete převést virtuální počítače s nespravované disky, které se nacházejí na účtech úložiště, které jsou nebo byly dříve šifrovaná na spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="5240a-206">**Can I convert VMs with unmanaged disks that are located on storage accounts that are or were previously encrypted to managed disks?**</span></span>

<span data-ttu-id="5240a-207">Ano</span><span class="sxs-lookup"><span data-stu-id="5240a-207">Yes</span></span>

<span data-ttu-id="5240a-208">**Budou exportovaného virtuálního pevného disku z spravovaného disku nebo snímek také zašifrovaná?**</span><span class="sxs-lookup"><span data-stu-id="5240a-208">**Will an exported VHD from a managed disk or a snapshot also be encrypted?**</span></span>

<span data-ttu-id="5240a-209">Ne.</span><span class="sxs-lookup"><span data-stu-id="5240a-209">No.</span></span> <span data-ttu-id="5240a-210">Pokud exportujete virtuální pevný disk k účtu úložiště šifrované z šifrované, ale spravované disku nebo snímek a potom je zašifrovaná.</span><span class="sxs-lookup"><span data-stu-id="5240a-210">But if you export a VHD to an encrypted storage account from an encrypted managed disk or snapshot, then it's encrypted.</span></span> 

## <a name="premium-disks-managed-and-unmanaged"></a><span data-ttu-id="5240a-211">Pro prémiové disky: spravovaných a nespravovaných</span><span class="sxs-lookup"><span data-stu-id="5240a-211">Premium disks: Managed and unmanaged</span></span>

<span data-ttu-id="5240a-212">**Pokud virtuální počítač používá velikost série, která podporuje službu Premium Storage, jako je například DSv2, můžu připojit premium a standard datové disky?**</span><span class="sxs-lookup"><span data-stu-id="5240a-212">**If a VM uses a size series that supports Premium Storage, such as a DSv2, can I attach both premium and standard data disks?**</span></span> 

<span data-ttu-id="5240a-213">Ano.</span><span class="sxs-lookup"><span data-stu-id="5240a-213">Yes.</span></span>

<span data-ttu-id="5240a-214">**Můžete připojit premium a standard datových disků pro velikost série, která nepodporuje Storage úrovně Premium, jako je například D Dv2, G nebo F řady?**</span><span class="sxs-lookup"><span data-stu-id="5240a-214">**Can I attach both premium and standard data disks to a size series that doesn't support Premium Storage, such as D, Dv2, G, or F series?**</span></span>

<span data-ttu-id="5240a-215">Ne.</span><span class="sxs-lookup"><span data-stu-id="5240a-215">No.</span></span> <span data-ttu-id="5240a-216">Pouze standardní datových disků můžete připojit k virtuálním počítačům, které nepoužívají velikost série, která podporuje službu Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="5240a-216">You can attach only standard data disks to VMs that don't use a size series that supports Premium Storage.</span></span>

<span data-ttu-id="5240a-217">**Když vytvořím premium datový disk z existujícího VHD, který byl 80 GB, kolik bude která stojí?**</span><span class="sxs-lookup"><span data-stu-id="5240a-217">**If I create a premium data disk from an existing VHD that was 80 GB, how much will that cost?**</span></span>

<span data-ttu-id="5240a-218">Datový disk premium vytvořen z disku VHD 80 GB je považována za velikost disku k dispozici další premium, což je P10 disku.</span><span class="sxs-lookup"><span data-stu-id="5240a-218">A premium data disk created from an 80-GB VHD is treated as the next-available premium disk size, which is a P10 disk.</span></span> <span data-ttu-id="5240a-219">Že se vám účtovat podle P10 disku ceny.</span><span class="sxs-lookup"><span data-stu-id="5240a-219">You're charged according to the P10 disk pricing.</span></span>

<span data-ttu-id="5240a-220">**Existují transakce náklady na použití služby Premium Storage?**</span><span class="sxs-lookup"><span data-stu-id="5240a-220">**Are there transaction costs to use Premium Storage?**</span></span>

<span data-ttu-id="5240a-221">Není opravené náklady pro každou velikost disku, která pochází zřízené pomocí omezení na IOPS a propustnosti.</span><span class="sxs-lookup"><span data-stu-id="5240a-221">There is a fixed cost for each disk size, which comes provisioned with specific limits on IOPS and throughput.</span></span> <span data-ttu-id="5240a-222">Další náklady jsou šířky odchozího pásma a kapacity snímku, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="5240a-222">The other costs are outbound bandwidth and snapshot capacity, if applicable.</span></span> <span data-ttu-id="5240a-223">Další informace najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="5240a-223">For more information, see the [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="5240a-224">**Jaké jsou limity pro IOPS a propustnosti, kterou můžete dostat z mezipaměti disku?**</span><span class="sxs-lookup"><span data-stu-id="5240a-224">**What are the limits for IOPS and throughput that I can get from the disk cache?**</span></span>

<span data-ttu-id="5240a-225">Kombinované omezení pro mezipaměť a místní SSD pro řady DS jsou 4 000 IOPS na základní a 33 MB za sekundu za jádra.</span><span class="sxs-lookup"><span data-stu-id="5240a-225">The combined limits for cache and local SSD for a DS series are 4,000 IOPS per core and 33 MB per second per core.</span></span> <span data-ttu-id="5240a-226">GS řady nabízí 5 000 IOPS na základní a 50 MB za sekundu za jádra.</span><span class="sxs-lookup"><span data-stu-id="5240a-226">The GS series offers 5,000 IOPS per core and 50 MB per second per core.</span></span>

<span data-ttu-id="5240a-227">**Je podporovaný na místní SSD pro virtuální počítač spravovaný disky?**</span><span class="sxs-lookup"><span data-stu-id="5240a-227">**Is the local SSD supported for a Managed Disks VM?**</span></span>

<span data-ttu-id="5240a-228">Na místní SSD je dočasné úložiště, která je součástí spravované virtuální disky.</span><span class="sxs-lookup"><span data-stu-id="5240a-228">The local SSD is temporary storage that is included with a Managed Disks VM.</span></span> <span data-ttu-id="5240a-229">Existuje nejsou zpoplatněné pro toto dočasný úložiště.</span><span class="sxs-lookup"><span data-stu-id="5240a-229">There is no extra cost for this temporary storage.</span></span> <span data-ttu-id="5240a-230">Doporučujeme vám, nepoužívejte tento místní SSD ukládat data aplikací, protože není trvalý v Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="5240a-230">We recommend that you do not use this local SSD to store your application data because it isn't persisted in Azure Blob storage.</span></span>

<span data-ttu-id="5240a-231">**Existují jakékoli následky pro použití operace TRIM na prémiové disky?**</span><span class="sxs-lookup"><span data-stu-id="5240a-231">**Are there any repercussions for the use of TRIM on premium disks?**</span></span>

<span data-ttu-id="5240a-232">Neexistuje žádné nevýhodou použití operace TRIM na disky Azure na buď premium nebo standardní disky.</span><span class="sxs-lookup"><span data-stu-id="5240a-232">There is no downside to the use of TRIM on Azure disks on either premium or standard disks.</span></span>

## <a name="new-disk-sizes-managed-and-unmanaged"></a><span data-ttu-id="5240a-233">Nové velikosti disků: spravovaných a nespravovaných</span><span class="sxs-lookup"><span data-stu-id="5240a-233">New disk sizes: Managed and unmanaged</span></span>

<span data-ttu-id="5240a-234">**Co je podporováno pro operační systém a datové disky největší velikost disku?**</span><span class="sxs-lookup"><span data-stu-id="5240a-234">**What is the largest disk size supported for operating system and data disks?**</span></span>

<span data-ttu-id="5240a-235">Typ oddílu, který podporuje Azure pro disk operačního systému je hlavní spouštěcí záznam (MBR).</span><span class="sxs-lookup"><span data-stu-id="5240a-235">The partition type that Azure supports for an operating system disk is the master boot record (MBR).</span></span> <span data-ttu-id="5240a-236">Podporuje formátu MBR disk velikost až 2 TB.</span><span class="sxs-lookup"><span data-stu-id="5240a-236">The MBR format supports a disk size up to 2 TB.</span></span> <span data-ttu-id="5240a-237">Největší velikost, kterou podporuje Azure pro disk operačního systému je 2 TB.</span><span class="sxs-lookup"><span data-stu-id="5240a-237">The largest size that Azure supports for an operating system disk is 2 TB.</span></span> <span data-ttu-id="5240a-238">Azure podporuje až 4 TB pro datové disky.</span><span class="sxs-lookup"><span data-stu-id="5240a-238">Azure supports up to 4 TB for data disks.</span></span> 

<span data-ttu-id="5240a-239">**Co je největší velikost objektu blob stránky, která je podporována?**</span><span class="sxs-lookup"><span data-stu-id="5240a-239">**What is the largest page blob size that's supported?**</span></span>

<span data-ttu-id="5240a-240">Největší velikost objektu blob stránky, který podporuje Azure je 8 TB (8 191 GB).</span><span class="sxs-lookup"><span data-stu-id="5240a-240">The largest page blob size that Azure supports is 8 TB (8,191 GB).</span></span> <span data-ttu-id="5240a-241">Nepodporujeme objekty BLOB stránky větší než 4 TB (4095 GB) připojené k virtuálnímu počítači jako data nebo disky operačního systému.</span><span class="sxs-lookup"><span data-stu-id="5240a-241">We don't support page blobs larger than 4 TB (4,095 GB) attached to a VM as data or operating system disks.</span></span>

<span data-ttu-id="5240a-242">**Je potřeba použít novou verzi nástroje Azure vytvořit, připojení, přizpůsobit a odeslat disky, které jsou větší než 1 TB?**</span><span class="sxs-lookup"><span data-stu-id="5240a-242">**Do I need to use a new version of Azure tools to create, attach, resize, and upload disks larger than 1 TB?**</span></span>

<span data-ttu-id="5240a-243">Není nutné upgradovat existující nástroje Azure k vytvoření, připojit nebo změna velikosti disků, které jsou větší než 1 TB.</span><span class="sxs-lookup"><span data-stu-id="5240a-243">You don't need to upgrade your existing Azure tools to create, attach, or resize disks larger than 1 TB.</span></span> <span data-ttu-id="5240a-244">K odeslání souboru virtuálního pevného disku z místního přímo do Azure jako objekt blob stránky nebo nespravované disku, budete muset použít nejnovější sady nástrojů:</span><span class="sxs-lookup"><span data-stu-id="5240a-244">To upload your VHD file from on-premises directly to Azure as a page blob or unmanaged disk, you need to use the latest tool sets:</span></span>

|<span data-ttu-id="5240a-245">Nástroje Azure</span><span class="sxs-lookup"><span data-stu-id="5240a-245">Azure tools</span></span>      | <span data-ttu-id="5240a-246">Podporované verze</span><span class="sxs-lookup"><span data-stu-id="5240a-246">Supported versions</span></span>                                |
|-----------------|---------------------------------------------------|
|<span data-ttu-id="5240a-247">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="5240a-247">Azure PowerShell</span></span> | <span data-ttu-id="5240a-248">Číslo verze 4.1.0: červen 2017 verzi nebo novější</span><span class="sxs-lookup"><span data-stu-id="5240a-248">Version number 4.1.0: June 2017 release or later</span></span>|
|<span data-ttu-id="5240a-249">Rozhraní příkazového řádku Azure v1</span><span class="sxs-lookup"><span data-stu-id="5240a-249">Azure CLI v1</span></span>     | <span data-ttu-id="5240a-250">Číslo verze 0.10.13: pravděpodobně 2017 verzi nebo novější</span><span class="sxs-lookup"><span data-stu-id="5240a-250">Version number 0.10.13: May 2017 release or later</span></span>|
|<span data-ttu-id="5240a-251">AzCopy</span><span class="sxs-lookup"><span data-stu-id="5240a-251">AzCopy</span></span>           | <span data-ttu-id="5240a-252">Číslo verze 6.1.0: červen 2017 verzi nebo novější</span><span class="sxs-lookup"><span data-stu-id="5240a-252">Version number 6.1.0: June 2017 release or later</span></span>|

<span data-ttu-id="5240a-253">Podpora rozhraní příkazového řádku Azure v2 a Azure Storage Explorer tu bude brzo dostupná.</span><span class="sxs-lookup"><span data-stu-id="5240a-253">The support for Azure CLI v2 and Azure Storage Explorer is coming soon.</span></span> 

<span data-ttu-id="5240a-254">**Jsou podporovány P4 a P6 velikosti disků pro nespravovaná disky nebo objekty BLOB stránky?**</span><span class="sxs-lookup"><span data-stu-id="5240a-254">**Are P4 and P6 disk sizes supported for unmanaged disks or page blobs?**</span></span>

<span data-ttu-id="5240a-255">Ne.</span><span class="sxs-lookup"><span data-stu-id="5240a-255">No.</span></span> <span data-ttu-id="5240a-256">P4 (32 GB) a P6 velikosti disků (64 GB) jsou podporovány pouze u spravovaných disky.</span><span class="sxs-lookup"><span data-stu-id="5240a-256">P4 (32 GB) and P6 (64 GB) disk sizes are supported only for managed disks.</span></span> <span data-ttu-id="5240a-257">Podpora pro nespravovaná disky a objekty BLOB stránky bude brzo.</span><span class="sxs-lookup"><span data-stu-id="5240a-257">Support for unmanaged disks and page blobs is coming soon.</span></span>

<span data-ttu-id="5240a-258">**Pokud mé existující premium spravované disku menší než 64 GB byl vytvořen před povolením malý disk (přibližně 15. června 2017), jak se jeho fakturuje?**</span><span class="sxs-lookup"><span data-stu-id="5240a-258">**If my existing premium managed disk less than 64 GB was created before the small disk was enabled (around June 15, 2017), how is it billed?**</span></span>

<span data-ttu-id="5240a-259">Stávající malých premium disky menší než 64 GB dál účtovat podle cenové úrovně P10.</span><span class="sxs-lookup"><span data-stu-id="5240a-259">Existing small premium disks less than 64 GB continue to be billed according to the P10 pricing tier.</span></span> 

<span data-ttu-id="5240a-260">**Jak můžete přepnout na úrovni disku malé prémiové disky menší než 64 GB z P10 P4 nebo P6?**</span><span class="sxs-lookup"><span data-stu-id="5240a-260">**How can I switch the disk tier of small premium disks less than 64 GB from P10 to P4 or P6?**</span></span>

<span data-ttu-id="5240a-261">Můžete pořízení snímku menší disky a potom vytvořit disk automaticky přepínat cenové úrovně P4 nebo P6 podle zřízené velikost.</span><span class="sxs-lookup"><span data-stu-id="5240a-261">You can take a snapshot of your small disks and then create a disk to automatically switch the pricing tier to P4 or P6 based on the provisioned size.</span></span> 


## <a name="what-if-my-question-isnt-answered-here"></a><span data-ttu-id="5240a-262">Co když není zde odpovědi můj dotaz?</span><span class="sxs-lookup"><span data-stu-id="5240a-262">What if my question isn't answered here?</span></span>

<span data-ttu-id="5240a-263">Pokud váš dotaz není zde uvedeno, dejte nám vědět, a pomůžeme vám najít odpověď.</span><span class="sxs-lookup"><span data-stu-id="5240a-263">If your question isn't listed here, let us know and we'll help you find an answer.</span></span> <span data-ttu-id="5240a-264">V komentářích můžete odeslat dotaz na konci tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="5240a-264">You can post a question at the end of this article in the comments.</span></span> <span data-ttu-id="5240a-265">Chcete-li se spojte s týmu Azure Storage a ostatními členy komunity o tomto článku, použijte webu MSDN [fórum pro Azure Storage](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).</span><span class="sxs-lookup"><span data-stu-id="5240a-265">To engage with the Azure Storage team and other community members about this article, use the MSDN [Azure Storage forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).</span></span>

<span data-ttu-id="5240a-266">Chcete-li požádat o funkce, odeslání požadavků a nápady, jak [fóru pro zpětnou vazbu Azure Storage](https://feedback.azure.com/forums/217298-storage).</span><span class="sxs-lookup"><span data-stu-id="5240a-266">To request features, submit your requests and ideas to the [Azure Storage feedback forum](https://feedback.azure.com/forums/217298-storage).</span></span>
