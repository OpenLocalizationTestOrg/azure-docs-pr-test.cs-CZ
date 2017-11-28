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
ms.openlocfilehash: 31d0aa67b6ca58b75b432ae94f93ebcf6d730380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-about-azure-iaas-vm-disks-and-managed-and-unmanaged-premium-disks"></a><span data-ttu-id="fdb7b-103">Nejčastější dotazy týkající se disky virtuálního počítače Azure IaaS a spravovanými a nespravovanými prémiové disky</span><span class="sxs-lookup"><span data-stu-id="fdb7b-103">Frequently asked questions about Azure IaaS VM disks and managed and unmanaged premium disks</span></span>

<span data-ttu-id="fdb7b-104">Tento článek obsahuje odpovědi na některé nejčastější dotazy týkající se Azure spravované disky a Azure Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-104">This article answers some frequently asked questions about Azure Managed Disks and Azure Premium Storage.</span></span>

## <a name="managed-disks"></a><span data-ttu-id="fdb7b-105">Managed Disks</span><span class="sxs-lookup"><span data-stu-id="fdb7b-105">Managed Disks</span></span>

<span data-ttu-id="fdb7b-106">**Co je Azure spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-106">**What is Azure Managed Disks?**</span></span>

<span data-ttu-id="fdb7b-107">Spravované disků je funkce, která zjednodušuje Správa disku pro virtuální počítače Azure IaaS tím, že zpracování Správa účtů úložiště pro vás.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-107">Managed Disks is a feature that simplifies disk management for Azure IaaS VMs by handling storage account management for you.</span></span> <span data-ttu-id="fdb7b-108">Další informace najdete v tématu hello [spravované disky přehled](storage-managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fdb7b-108">For more information, see hello [Managed Disks overview](storage-managed-disks-overview.md).</span></span>

<span data-ttu-id="fdb7b-109">**Když vytvořím standardní se spravovaným diskem z existujícího VHD, který je 80 GB, jaké budou který náklady mi?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-109">**If I create a standard managed disk from an existing VHD that's 80 GB, how much will that cost me?**</span></span>

<span data-ttu-id="fdb7b-110">Standardní se spravovaným diskem vytvořen z disku VHD 80 GB je považována za hello další dostupné standardní velikost disku, který je k dispozici S10 disk.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-110">A standard managed disk created from an 80-GB VHD is treated as hello next available standard disk size, which is an S10 disk.</span></span> <span data-ttu-id="fdb7b-111">Se vám účtovat podle toohello S10 disku ceny.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-111">You're charged according toohello S10 disk pricing.</span></span> <span data-ttu-id="fdb7b-112">Další informace najdete v tématu hello [stránce s cenami](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="fdb7b-112">For more information, see hello [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="fdb7b-113">**Existují žádné transakce náklady na standardní spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-113">**Are there any transaction costs for standard managed disks?**</span></span>

<span data-ttu-id="fdb7b-114">Ano.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-114">Yes.</span></span> <span data-ttu-id="fdb7b-115">Vám účtovat každou transakci.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-115">You're charged for each transaction.</span></span> <span data-ttu-id="fdb7b-116">Další informace najdete v tématu hello [stránce s cenami](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="fdb7b-116">For more information, see hello [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="fdb7b-117">**Pro standardní se spravovaným diskem I odečte hello skutečná velikost hello dat na disku hello nebo hello zřídit kapacitu disku hello?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-117">**For a standard managed disk, will I be charged for hello actual size of hello data on hello disk or for hello provisioned capacity of hello disk?**</span></span>

<span data-ttu-id="fdb7b-118">Že se vám účtovat podle hello zřídit kapacitu disku hello.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-118">You're charged based on hello provisioned capacity of hello disk.</span></span> <span data-ttu-id="fdb7b-119">Další informace najdete v tématu hello [stránce s cenami](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="fdb7b-119">For more information, see hello [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="fdb7b-120">**Jak je ceny disků premium spravované liší od nespravované disky?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-120">**How is pricing of premium managed disks different from unmanaged disks?**</span></span>

<span data-ttu-id="fdb7b-121">Hello ceny disků spravované premium je hello stejné jako nespravované prémiové disky.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-121">hello pricing of premium managed disks is hello same as unmanaged premium disks.</span></span>

<span data-ttu-id="fdb7b-122">**Můžete změnit hello typ účtu úložiště (Standard nebo Premium) Moje spravovaných disků?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-122">**Can I change hello storage account type (Standard or Premium) of my managed disks?**</span></span>

<span data-ttu-id="fdb7b-123">Ano.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-123">Yes.</span></span> <span data-ttu-id="fdb7b-124">Typ účtu úložiště hello spravované disky můžete změnit pomocí hello portálu Azure, PowerShell nebo hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-124">You can change hello storage account type of your managed disks by using hello Azure portal, PowerShell, or hello Azure CLI.</span></span>

<span data-ttu-id="fdb7b-125">**Existuje způsob, že umožní kopírování a exportovat účet spravovaných disků na tooa privátní úložiště?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-125">**Is there a way that I can copy or export a managed disk tooa private storage account?**</span></span>

<span data-ttu-id="fdb7b-126">Ano.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-126">Yes.</span></span> <span data-ttu-id="fdb7b-127">Vaše spravované disky můžete exportovat pomocí hello portálu Azure, PowerShell nebo hello rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-127">You can export your managed disks by using hello Azure portal, PowerShell, or hello Azure CLI.</span></span>

<span data-ttu-id="fdb7b-128">**Můžete použít soubor virtuálního pevného disku v toocreate účet úložiště Azure se spravovaným diskem s jiného předplatného?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-128">**Can I use a VHD file in an Azure storage account toocreate a managed disk with a different subscription?**</span></span>

<span data-ttu-id="fdb7b-129">Ne.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-129">No.</span></span>

<span data-ttu-id="fdb7b-130">**Můžete použít soubor virtuálního pevného disku v toocreate účet úložiště Azure se spravovaným diskem v jiné oblasti.**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-130">**Can I use a VHD file in an Azure storage account toocreate a managed disk in a different region?**</span></span>

<span data-ttu-id="fdb7b-131">Ne.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-131">No.</span></span>

<span data-ttu-id="fdb7b-132">**Existují nějaká omezení škálování pro zákazníky, kteří používají spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-132">**Are there any scale limitations for customers that use managed disks?**</span></span>

<span data-ttu-id="fdb7b-133">Spravované disky eliminuje hello omezení spojená s účty úložiště.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-133">Managed Disks eliminates hello limits associated with storage accounts.</span></span> <span data-ttu-id="fdb7b-134">Hello počet spravovaných disků na jedno předplatné, je však omezená too2 000 ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-134">However, hello number of managed disks per subscription is limited too2,000 by default.</span></span> <span data-ttu-id="fdb7b-135">Podpora tooincrease můžete volat toto číslo.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-135">You can call support tooincrease this number.</span></span>

<span data-ttu-id="fdb7b-136">**Může trvat přírůstkový snímek spravovaného disku?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-136">**Can I take an incremental snapshot of a managed disk?**</span></span>

<span data-ttu-id="fdb7b-137">Ne.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-137">No.</span></span> <span data-ttu-id="fdb7b-138">aktuální vytváření snímků Hello provede úplnou kopii se spravovaným diskem.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-138">hello current snapshot capability makes a full copy of a managed disk.</span></span> <span data-ttu-id="fdb7b-139">Jsme ale plánování toosupport přírůstkové snímky v budoucnu hello.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-139">However, we are planning toosupport incremental snapshots in hello future.</span></span>

<span data-ttu-id="fdb7b-140">**Virtuální počítače v nastavení dostupnosti se může skládat z kombinace spravovanými a nespravovanými disky?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-140">**Can VMs in an availability set consist of a combination of managed and unmanaged disks?**</span></span>

<span data-ttu-id="fdb7b-141">Ne.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-141">No.</span></span> <span data-ttu-id="fdb7b-142">Hello virtuálních počítačů v nastavení dostupnosti musí používat disky všechny spravované nebo nespravované všechny disky.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-142">hello VMs in an availability set must use either all managed disks or all unmanaged disks.</span></span> <span data-ttu-id="fdb7b-143">Když vytvoříte skupinu dostupnosti, můžete typu disky, kterého chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-143">When you create an availability set, you can choose which type of disks you want toouse.</span></span>

<span data-ttu-id="fdb7b-144">**Spravované disky hello výchozí možnost je ve hello portál Azure?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-144">**Is Managed Disks hello default option in hello Azure portal?**</span></span>

<span data-ttu-id="fdb7b-145">Aktuálně ale bude výchozí hello v budoucnu hello.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-145">Not currently, but it will become hello default in hello future.</span></span>

<span data-ttu-id="fdb7b-146">**Můžete vytvořit prázdný disk spravované?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-146">**Can I create an empty managed disk?**</span></span>

<span data-ttu-id="fdb7b-147">Ano.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-147">Yes.</span></span> <span data-ttu-id="fdb7b-148">Můžete vytvořit prázdný disk.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-148">You can create an empty disk.</span></span> <span data-ttu-id="fdb7b-149">Spravovaný disk lze vytvořit nezávisle na virtuální počítač, například bez jeho připojení tooa virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-149">A managed disk can be created independently of a VM, for example, without attaching it tooa VM.</span></span>

<span data-ttu-id="fdb7b-150">**Co je podporováno hello počet domén selhání pro skupinu dostupnosti, který používá disky spravované?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-150">**What is hello supported fault domain count for an availability set that uses Managed Disks?**</span></span>

<span data-ttu-id="fdb7b-151">V závislosti na hello oblasti, kde se nachází skupina dostupnosti hello, který používá disky spravovat je počet domén selhání hello podporované 2 nebo 3.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-151">Depending on hello region where hello availability set that uses Managed Disks is located, hello supported fault domain count is 2 or 3.</span></span>

<span data-ttu-id="fdb7b-152">**Jak je hello standardní účet úložiště pro diagnostiku nastavení?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-152">**How is hello standard storage account for diagnostics set up?**</span></span>

<span data-ttu-id="fdb7b-153">Nastavit účet privátní úložiště pro diagnostiku virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-153">You set up a private storage account for VM diagnostics.</span></span> <span data-ttu-id="fdb7b-154">V budoucích hello, plánujeme tooswitch diagnostiky i tooManaged disky.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-154">In hello future, we plan tooswitch diagnostics tooManaged Disks as well.</span></span>

<span data-ttu-id="fdb7b-155">**Jaký druh řízení přístupu na základě Role podpora je k dispozici pro spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-155">**What kind of Role-Based Access Control support is available for Managed Disks?**</span></span>

<span data-ttu-id="fdb7b-156">Spravované disky podporuje tři klíčové výchozích rolí:</span><span class="sxs-lookup"><span data-stu-id="fdb7b-156">Managed Disks supports three key default roles:</span></span>

* <span data-ttu-id="fdb7b-157">Vlastník: Můžou spravovat všechno včetně přístupu</span><span class="sxs-lookup"><span data-stu-id="fdb7b-157">Owner: Can manage everything, including access</span></span>
* <span data-ttu-id="fdb7b-158">Přispěvatel: Můžou spravovat všechno kromě přístupu</span><span class="sxs-lookup"><span data-stu-id="fdb7b-158">Contributor: Can manage everything except access</span></span>
* <span data-ttu-id="fdb7b-159">Čtečka: Zobrazit vše, co, ale nelze provádět změny</span><span class="sxs-lookup"><span data-stu-id="fdb7b-159">Reader: Can view everything, but can't make changes</span></span>

<span data-ttu-id="fdb7b-160">**Existuje způsob, že umožní kopírování a exportovat účet spravovaných disků na tooa privátní úložiště?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-160">**Is there a way that I can copy or export a managed disk tooa private storage account?**</span></span>

<span data-ttu-id="fdb7b-161">Jen pro čtení sdíleného přístupového podpisu identifikátor URI pro hello spravované disk a použít ho můžete získat toocopy hello obsah tooa privátní úložiště účet nebo místní úložiště.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-161">You can get a read-only shared access signature URI for hello managed disk and use it toocopy hello contents tooa private storage account or on-premises storage.</span></span>

<span data-ttu-id="fdb7b-162">**Můžete vytvořit kopii Moje spravovaného disku?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-162">**Can I create a copy of my managed disk?**</span></span>

<span data-ttu-id="fdb7b-163">Zákazníci můžete pořízení snímku jejich spravované disky a pak použít hello snímku toocreate spravovaných disků na jiný.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-163">Customers can take a snapshot of their managed disks and then use hello snapshot toocreate another managed disk.</span></span>

<span data-ttu-id="fdb7b-164">**Jsou nespravované disky stále podporovány?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-164">**Are unmanaged disks still supported?**</span></span>

<span data-ttu-id="fdb7b-165">Ano.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-165">Yes.</span></span> <span data-ttu-id="fdb7b-166">Podporujeme nespravované a spravované disky.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-166">We support unmanaged and managed disks.</span></span> <span data-ttu-id="fdb7b-167">Doporučujeme používat spravované disky pro nové úlohy a vaše aktuální toomanaged disky úlohy migrace.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-167">We recommend that you use managed disks for new workloads and migrate your current workloads toomanaged disks.</span></span>


<span data-ttu-id="fdb7b-168">**Je-li vytvořit 128 GB disk a poté zvýšit hello velikost too130 GB, bude I vám účtována hello další velikost disku (512 GB)?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-168">**If I create a 128-GB disk and then increase hello size too130 GB, will I be charged for hello next disk size (512 GB)?**</span></span>

<span data-ttu-id="fdb7b-169">Ano.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-169">Yes.</span></span>

<span data-ttu-id="fdb7b-170">**Můžete vytvořit místně redundantní úložiště, geograficky redundantní úložiště, a zónově redundantní úložiště spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-170">**Can I create locally redundant storage, geo-redundant storage, and zone-redundant storage managed disks?**</span></span>

<span data-ttu-id="fdb7b-171">Disky systému Azure spravované aktuálně podporuje pouze místně redundantního úložiště spravované disky.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-171">Azure Managed Disks currently supports only locally redundant storage managed disks.</span></span>

<span data-ttu-id="fdb7b-172">**Můžete zmenšit nebo downsize Moje spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-172">**Can I shrink or downsize my managed disks?**</span></span>

<span data-ttu-id="fdb7b-173">Ne.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-173">No.</span></span> <span data-ttu-id="fdb7b-174">Tato funkce není aktuálně podporován.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-174">This feature is not supported currently.</span></span> 

<span data-ttu-id="fdb7b-175">**Můžete změnit vlastnost název počítače hello, pokud specializované (ne vytvořili pomocí nástroje pro přípravu systému hello nebo zobecněn) operačního systému disku jsou použité tooprovision virtuálního počítače?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-175">**Can I change hello computer name property when a specialized (not created by using hello System Preparation tool or generalized) operating system disk is used tooprovision a VM?**</span></span>

<span data-ttu-id="fdb7b-176">Ne.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-176">No.</span></span> <span data-ttu-id="fdb7b-177">Nelze aktualizovat vlastnosti název počítače hello.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-177">You can't update hello computer name property.</span></span> <span data-ttu-id="fdb7b-178">Hello nový virtuální počítač zdědí ho hello nadřazený virtuální počítač, který byl disk operačního systému použít toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-178">hello new VM inherits it from hello parent VM, which was used toocreate hello operating system disk.</span></span> 

<span data-ttu-id="fdb7b-179">**Kde najdu ukázka Azure Resource Manager šablony toocreate virtuálních počítačů s spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-179">**Where can I find sample Azure Resource Manager templates toocreate VMs with managed disks?**</span></span>
* [<span data-ttu-id="fdb7b-180">Seznam šablon pomocí spravovaných disků</span><span class="sxs-lookup"><span data-stu-id="fdb7b-180">List of templates using Managed Disks</span></span>](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* <span data-ttu-id="fdb7b-181">https://github.com/chagarw/MDPP</span><span class="sxs-lookup"><span data-stu-id="fdb7b-181">https://github.com/chagarw/MDPP</span></span>

## <a name="managed-disks-and-storage-service-encryption"></a><span data-ttu-id="fdb7b-182">Spravované disky a šifrování služby úložiště</span><span class="sxs-lookup"><span data-stu-id="fdb7b-182">Managed Disks and Storage Service Encryption</span></span> 

<span data-ttu-id="fdb7b-183">**Šifrování služby úložiště Azure ve výchozím nastavení zapnutá po vytvoření se spravovaným diskem?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-183">**Is Azure Storage Service Encryption enabled by default when I create a managed disk?**</span></span>

<span data-ttu-id="fdb7b-184">Ano.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-184">Yes.</span></span>

<span data-ttu-id="fdb7b-185">**Kdo spravuje hello šifrovacích klíčů?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-185">**Who manages hello encryption keys?**</span></span>

<span data-ttu-id="fdb7b-186">Spravuje Microsoft hello šifrovací klíče.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-186">Microsoft manages hello encryption keys.</span></span>

<span data-ttu-id="fdb7b-187">**Můžete zakázat šifrování služby úložiště pro moje spravované disky?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-187">**Can I disable Storage Service Encryption for my managed disks?**</span></span>

<span data-ttu-id="fdb7b-188">Ne.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-188">No.</span></span>

<span data-ttu-id="fdb7b-189">**Je šifrování služby úložiště k dispozici pouze v určitých oblastí?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-189">**Is Storage Service Encryption only available in specific regions?**</span></span>

<span data-ttu-id="fdb7b-190">Ne.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-190">No.</span></span> <span data-ttu-id="fdb7b-191">Je k dispozici ve všech oblastech hello, kde je k dispozici spravované disky.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-191">It's available in all hello regions where Managed Disks is available.</span></span> <span data-ttu-id="fdb7b-192">Spravované disků je k dispozici ve všech veřejných oblastí a Německu.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-192">Managed Disks is available in all public regions and Germany.</span></span>

<span data-ttu-id="fdb7b-193">**Jak můžete zjistit, pokud je zašifrovaná Moje spravovaných disků?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-193">**How can I find out if my managed disk is encrypted?**</span></span>

<span data-ttu-id="fdb7b-194">Můžete zjistit hello čas vytvoření spravovaného disku z hello portálu Azure, hello rozhraní příkazového řádku Azure a prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-194">You can find out hello time when a managed disk was created from hello Azure portal, hello Azure CLI, and PowerShell.</span></span> <span data-ttu-id="fdb7b-195">Pokud je doba hello po 9. června 2017, se šifrují na disku.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-195">If hello time is after June 9, 2017, then your disk is encrypted.</span></span> 

<span data-ttu-id="fdb7b-196">**Jak můžete šifrovat Můj stávající disky, které byly vytvořeny před 10 června 2017?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-196">**How can I encrypt my existing disks that were created before June 10, 2017?**</span></span>

<span data-ttu-id="fdb7b-197">Od verze 10 června 2017 je nová data tooexisting spravované disky šifrují automaticky.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-197">As of June 10, 2017, new data written tooexisting managed disks is automatically encrypted.</span></span> <span data-ttu-id="fdb7b-198">Můžeme také plánování tooencrypt stávající data a šifrování hello proběhne asynchronně hello pozadí.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-198">We are also planning tooencrypt existing data, and hello encryption will happen asynchronously in hello background.</span></span> <span data-ttu-id="fdb7b-199">Pokud je nutné šifrovat všechna existující data, vytvoří se kopie disku.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-199">If you must encrypt existing data now, create a copy of your disk.</span></span> <span data-ttu-id="fdb7b-200">Bude se šifrovat nové disky.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-200">New disks will be encrypted.</span></span>

* [<span data-ttu-id="fdb7b-201">Kopírovat spravovaných disků pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="fdb7b-201">Copy managed disks by using hello Azure CLI</span></span>](https://docs.microsoft.com/en-us/azure/storage/scripts/storage-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription?toc=%2fcli%2fmodule%2ftoc.json)
* [<span data-ttu-id="fdb7b-202">Zkopírujte spravované disky pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="fdb7b-202">Copy managed disks by using PowerShell</span></span>](https://docs.microsoft.com/en-us/azure/storage/scripts/storage-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription?toc=%2fcli%2fmodule%2ftoc.json)

<span data-ttu-id="fdb7b-203">**Jsou spravované snímky a bitové kopie zašifrovaná?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-203">**Are managed snapshots and images encrypted?**</span></span>

<span data-ttu-id="fdb7b-204">Ano.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-204">Yes.</span></span> <span data-ttu-id="fdb7b-205">Všechny spravované snímky a bitové kopie vytvořené po 9. června 2017, se šifrují automaticky.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-205">All managed snapshots and images created after June 9, 2017, are automatically encrypted.</span></span> 

<span data-ttu-id="fdb7b-206">**Můžete převést virtuální počítače s nespravované disky, které jsou umístěny v účtech úložiště, které jsou nebo byly dříve šifrované toomanaged disky?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-206">**Can I convert VMs with unmanaged disks that are located on storage accounts that are or were previously encrypted toomanaged disks?**</span></span>

<span data-ttu-id="fdb7b-207">Ano</span><span class="sxs-lookup"><span data-stu-id="fdb7b-207">Yes</span></span>

<span data-ttu-id="fdb7b-208">**Budou exportovaného virtuálního pevného disku z spravovaného disku nebo snímek také zašifrovaná?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-208">**Will an exported VHD from a managed disk or a snapshot also be encrypted?**</span></span>

<span data-ttu-id="fdb7b-209">Ne.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-209">No.</span></span> <span data-ttu-id="fdb7b-210">Pokud exportujete tooan virtuálního pevného disku, ale šifrované účet úložiště z šifrované spravovaného disku nebo snímek a potom je zašifrovaná.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-210">But if you export a VHD tooan encrypted storage account from an encrypted managed disk or snapshot, then it's encrypted.</span></span> 

## <a name="premium-disks-managed-and-unmanaged"></a><span data-ttu-id="fdb7b-211">Pro prémiové disky: spravovaných a nespravovaných</span><span class="sxs-lookup"><span data-stu-id="fdb7b-211">Premium disks: Managed and unmanaged</span></span>

<span data-ttu-id="fdb7b-212">**Pokud virtuální počítač používá velikost série, která podporuje službu Premium Storage, jako je například DSv2, můžu připojit premium a standard datové disky?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-212">**If a VM uses a size series that supports Premium Storage, such as a DSv2, can I attach both premium and standard data disks?**</span></span> 

<span data-ttu-id="fdb7b-213">Ano.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-213">Yes.</span></span>

<span data-ttu-id="fdb7b-214">**Můžete připojit premium a standard data disky tooa velikost řady, která nepodporuje Storage úrovně Premium, jako je například D Dv2, G nebo F řady?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-214">**Can I attach both premium and standard data disks tooa size series that doesn't support Premium Storage, such as D, Dv2, G, or F series?**</span></span>

<span data-ttu-id="fdb7b-215">Ne.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-215">No.</span></span> <span data-ttu-id="fdb7b-216">Můžete připojit pouze disky tooVMs standardní data, která nepoužívají velikost série, která podporuje službu Premium Storage.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-216">You can attach only standard data disks tooVMs that don't use a size series that supports Premium Storage.</span></span>

<span data-ttu-id="fdb7b-217">**Když vytvořím premium datový disk z existujícího VHD, který byl 80 GB, kolik bude která stojí?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-217">**If I create a premium data disk from an existing VHD that was 80 GB, how much will that cost?**</span></span>

<span data-ttu-id="fdb7b-218">Datový disk premium vytvořen z disku VHD 80 GB je považována za hello k dispozici další premium disku velikost, která je P10 disk.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-218">A premium data disk created from an 80-GB VHD is treated as hello next-available premium disk size, which is a P10 disk.</span></span> <span data-ttu-id="fdb7b-219">Se vám účtovat podle toohello P10 disku ceny.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-219">You're charged according toohello P10 disk pricing.</span></span>

<span data-ttu-id="fdb7b-220">**Existují transakce náklady toouse Premium Storage?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-220">**Are there transaction costs toouse Premium Storage?**</span></span>

<span data-ttu-id="fdb7b-221">Není opravené náklady pro každou velikost disku, která pochází zřízené pomocí omezení na IOPS a propustnosti.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-221">There is a fixed cost for each disk size, which comes provisioned with specific limits on IOPS and throughput.</span></span> <span data-ttu-id="fdb7b-222">Hello další náklady jsou šířky odchozího pásma a kapacity snímku, pokud je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-222">hello other costs are outbound bandwidth and snapshot capacity, if applicable.</span></span> <span data-ttu-id="fdb7b-223">Další informace najdete v tématu hello [stránce s cenami](https://azure.microsoft.com/pricing/details/storage).</span><span class="sxs-lookup"><span data-stu-id="fdb7b-223">For more information, see hello [pricing page](https://azure.microsoft.com/pricing/details/storage).</span></span>

<span data-ttu-id="fdb7b-224">**Jaká jsou omezení hello IOPS a propustnosti, kterou můžete dostat z hello diskové mezipaměti?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-224">**What are hello limits for IOPS and throughput that I can get from hello disk cache?**</span></span>

<span data-ttu-id="fdb7b-225">Hello kombinované omezení pro mezipaměť a místní SSD pro řady DS 4000 IOPS na základní a 33 MB za sekundu za jádra.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-225">hello combined limits for cache and local SSD for a DS series are 4,000 IOPS per core and 33 MB per second per core.</span></span> <span data-ttu-id="fdb7b-226">Hello GS řady nabízí 5 000 IOPS na základní a 50 MB za sekundu za jádra.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-226">hello GS series offers 5,000 IOPS per core and 50 MB per second per core.</span></span>

<span data-ttu-id="fdb7b-227">**Je hello nepodporuje místní SSD pro virtuální počítač spravovaný disky?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-227">**Is hello local SSD supported for a Managed Disks VM?**</span></span>

<span data-ttu-id="fdb7b-228">Hello místní SSD je dočasné úložiště, která je součástí spravované virtuální disky.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-228">hello local SSD is temporary storage that is included with a Managed Disks VM.</span></span> <span data-ttu-id="fdb7b-229">Existuje nejsou zpoplatněné pro toto dočasný úložiště.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-229">There is no extra cost for this temporary storage.</span></span> <span data-ttu-id="fdb7b-230">Doporučujeme vám, že nepoužijete tento místní SSD toostore data aplikací protože není trvalý v Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-230">We recommend that you do not use this local SSD toostore your application data because it isn't persisted in Azure Blob storage.</span></span>

<span data-ttu-id="fdb7b-231">**Jsou existuje žádné dopady na hello použít Trim na prémiové disky?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-231">**Are there any repercussions for hello use of TRIM on premium disks?**</span></span>

<span data-ttu-id="fdb7b-232">Neexistuje žádné nevýhodou použití toohello TRIM na disky Azure na buď premium nebo standardní disky.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-232">There is no downside toohello use of TRIM on Azure disks on either premium or standard disks.</span></span>

## <a name="new-disk-sizes-managed-and-unmanaged"></a><span data-ttu-id="fdb7b-233">Nové velikosti disků: spravovaných a nespravovaných</span><span class="sxs-lookup"><span data-stu-id="fdb7b-233">New disk sizes: Managed and unmanaged</span></span>

<span data-ttu-id="fdb7b-234">**Co je největší velikost disku hello podporovaná pro operační systém a datové disky?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-234">**What is hello largest disk size supported for operating system and data disks?**</span></span>

<span data-ttu-id="fdb7b-235">Typ Hello oddílu, který podporuje Azure pro disk operačního systému je hello hlavní spouštěcí záznam (MBR).</span><span class="sxs-lookup"><span data-stu-id="fdb7b-235">hello partition type that Azure supports for an operating system disk is hello master boot record (MBR).</span></span> <span data-ttu-id="fdb7b-236">formát MBR Hello podporuje velikost disku až too2 TB.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-236">hello MBR format supports a disk size up too2 TB.</span></span> <span data-ttu-id="fdb7b-237">Hello největší velikost, který podporuje Azure pro disk operačního systému je 2 TB.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-237">hello largest size that Azure supports for an operating system disk is 2 TB.</span></span> <span data-ttu-id="fdb7b-238">Azure podporuje až too4 TB pro datové disky.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-238">Azure supports up too4 TB for data disks.</span></span> 

<span data-ttu-id="fdb7b-239">**Co je hello největší stránky velikost objektu blob podporovanou?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-239">**What is hello largest page blob size that's supported?**</span></span>

<span data-ttu-id="fdb7b-240">Hello největší stránky velikost objektu blob podporující Azure je 8 TB (8 191 GB).</span><span class="sxs-lookup"><span data-stu-id="fdb7b-240">hello largest page blob size that Azure supports is 8 TB (8,191 GB).</span></span> <span data-ttu-id="fdb7b-241">Nepodporujeme objekty BLOB stránky větší než 4 TB (4095 GB) připojené tooa virtuálního počítače jako data nebo disky operačního systému.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-241">We don't support page blobs larger than 4 TB (4,095 GB) attached tooa VM as data or operating system disks.</span></span>

<span data-ttu-id="fdb7b-242">**Potřebuji toouse novou verzí z nástroje Azure toocreate připojit, přizpůsobit a odeslat disky, které jsou větší než 1 TB?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-242">**Do I need toouse a new version of Azure tools toocreate, attach, resize, and upload disks larger than 1 TB?**</span></span>

<span data-ttu-id="fdb7b-243">Nepotřebujete tooupgrade vaší existující toocreate nástroje Azure, připojit nebo změna velikosti disků, které jsou větší než 1 TB.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-243">You don't need tooupgrade your existing Azure tools toocreate, attach, or resize disks larger than 1 TB.</span></span> <span data-ttu-id="fdb7b-244">tooupload svůj disk VHD souboru z místní přímo tooAzure jako objekt blob stránky nebo nespravované disku, bude nutné toouse hello nejnovější sady nástrojů:</span><span class="sxs-lookup"><span data-stu-id="fdb7b-244">tooupload your VHD file from on-premises directly tooAzure as a page blob or unmanaged disk, you need toouse hello latest tool sets:</span></span>

|<span data-ttu-id="fdb7b-245">Nástroje Azure</span><span class="sxs-lookup"><span data-stu-id="fdb7b-245">Azure tools</span></span>      | <span data-ttu-id="fdb7b-246">Podporované verze</span><span class="sxs-lookup"><span data-stu-id="fdb7b-246">Supported versions</span></span>                                |
|-----------------|---------------------------------------------------|
|<span data-ttu-id="fdb7b-247">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="fdb7b-247">Azure PowerShell</span></span> | <span data-ttu-id="fdb7b-248">Číslo verze 4.1.0: červen 2017 verzi nebo novější</span><span class="sxs-lookup"><span data-stu-id="fdb7b-248">Version number 4.1.0: June 2017 release or later</span></span>|
|<span data-ttu-id="fdb7b-249">Rozhraní příkazového řádku Azure v1</span><span class="sxs-lookup"><span data-stu-id="fdb7b-249">Azure CLI v1</span></span>     | <span data-ttu-id="fdb7b-250">Číslo verze 0.10.13: pravděpodobně 2017 verzi nebo novější</span><span class="sxs-lookup"><span data-stu-id="fdb7b-250">Version number 0.10.13: May 2017 release or later</span></span>|
|<span data-ttu-id="fdb7b-251">AzCopy</span><span class="sxs-lookup"><span data-stu-id="fdb7b-251">AzCopy</span></span>           | <span data-ttu-id="fdb7b-252">Číslo verze 6.1.0: červen 2017 verzi nebo novější</span><span class="sxs-lookup"><span data-stu-id="fdb7b-252">Version number 6.1.0: June 2017 release or later</span></span>|

<span data-ttu-id="fdb7b-253">Podpora Hello v2 rozhraní příkazového řádku Azure a Azure Storage Explorer tu bude brzo dostupná.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-253">hello support for Azure CLI v2 and Azure Storage Explorer is coming soon.</span></span> 

<span data-ttu-id="fdb7b-254">**Jsou podporovány P4 a P6 velikosti disků pro nespravovaná disky nebo objekty BLOB stránky?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-254">**Are P4 and P6 disk sizes supported for unmanaged disks or page blobs?**</span></span>

<span data-ttu-id="fdb7b-255">Ne.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-255">No.</span></span> <span data-ttu-id="fdb7b-256">P4 (32 GB) a P6 velikosti disků (64 GB) jsou podporovány pouze u spravovaných disky.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-256">P4 (32 GB) and P6 (64 GB) disk sizes are supported only for managed disks.</span></span> <span data-ttu-id="fdb7b-257">Podpora pro nespravovaná disky a objekty BLOB stránky bude brzo.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-257">Support for unmanaged disks and page blobs is coming soon.</span></span>

<span data-ttu-id="fdb7b-258">**Pokud mé existující premium spravované disku menší než 64 GB byl vytvořen před povolením malý disk hello (přibližně 15. června 2017), jak se jeho fakturuje?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-258">**If my existing premium managed disk less than 64 GB was created before hello small disk was enabled (around June 15, 2017), how is it billed?**</span></span>

<span data-ttu-id="fdb7b-259">Stávající malých prémiové disky menší než 64 GB pokračovat toobe účtují podle toohello P10 cenovou úroveň.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-259">Existing small premium disks less than 64 GB continue toobe billed according toohello P10 pricing tier.</span></span> 

<span data-ttu-id="fdb7b-260">**Jak můžete přepnout hello disku úroveň malé prémiové disky menší než 64 GB z P10 tooP4 nebo P6?**</span><span class="sxs-lookup"><span data-stu-id="fdb7b-260">**How can I switch hello disk tier of small premium disks less than 64 GB from P10 tooP4 or P6?**</span></span>

<span data-ttu-id="fdb7b-261">Může pořízení snímku menší disky a poté vytvořit hello disku tooautomatically přepínač cenová úroveň tooP4 nebo P6 podle velikosti hello zřízený.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-261">You can take a snapshot of your small disks and then create a disk tooautomatically switch hello pricing tier tooP4 or P6 based on hello provisioned size.</span></span> 


## <a name="what-if-my-question-isnt-answered-here"></a><span data-ttu-id="fdb7b-262">Co když není zde odpovědi můj dotaz?</span><span class="sxs-lookup"><span data-stu-id="fdb7b-262">What if my question isn't answered here?</span></span>

<span data-ttu-id="fdb7b-263">Pokud váš dotaz není zde uvedeno, dejte nám vědět, a pomůžeme vám najít odpověď.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-263">If your question isn't listed here, let us know and we'll help you find an answer.</span></span> <span data-ttu-id="fdb7b-264">V komentářích hello můžete odeslat dotaz na konci hello tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="fdb7b-264">You can post a question at hello end of this article in hello comments.</span></span> <span data-ttu-id="fdb7b-265">tooengage týmem hello Azure Storage a ostatními členy komunity o tomto článku použít hello MSDN [fórum pro Azure Storage](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).</span><span class="sxs-lookup"><span data-stu-id="fdb7b-265">tooengage with hello Azure Storage team and other community members about this article, use hello MSDN [Azure Storage forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).</span></span>

<span data-ttu-id="fdb7b-266">Funkce toorequest odeslání vaší žádosti a nápady toohello [fóru pro zpětnou vazbu Azure Storage](https://feedback.azure.com/forums/217298-storage).</span><span class="sxs-lookup"><span data-stu-id="fdb7b-266">toorequest features, submit your requests and ideas toohello [Azure Storage feedback forum](https://feedback.azure.com/forums/217298-storage).</span></span>
