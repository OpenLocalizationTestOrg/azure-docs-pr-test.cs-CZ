---
title: "Azure Disk Encryption pro systém Windows a virtuálních počítačů Linux IaaS | Microsoft Docs"
description: "Tento článek obsahuje přehled Microsoft Azure Disk Encryption pro systém Windows a virtuálních počítačů IaaS Linux."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: d3fac8bb-4829-405e-8701-fa7229fb1725
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2017
ms.author: kakhan
ms.openlocfilehash: a4f20fc19ae40561d042d5cff744a030014f75c7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-disk-encryption-for-windows-and-linux-iaas-vms"></a><span data-ttu-id="e6804-103">Azure Disk Encryption pro systém Windows a virtuálních počítačů Linux IaaS</span><span class="sxs-lookup"><span data-stu-id="e6804-103">Azure Disk Encryption for Windows and Linux IaaS VMs</span></span>
<span data-ttu-id="e6804-104">Microsoft Azure se důrazně zaměřuje na zajištění ochrany osobních údajů, suverenity data a umožňuje vám řízení vaší Azure hostované data prostřednictvím řadu pokročilých technologiích k šifrování, řídit a spravovat šifrovací klíče, řízení a audit přístupu k datům.</span><span class="sxs-lookup"><span data-stu-id="e6804-104">Microsoft Azure is strongly committed to ensuring your data privacy, data sovereignty and enables you to control your Azure hosted data through a range of advanced technologies to encrypt, control and manage encryption keys, control & audit access of data.</span></span> <span data-ttu-id="e6804-105">To poskytuje Azure zákazníkům flexibilitu zvolit si řešení, které nejlépe vyhovuje potřebám své firmy.</span><span class="sxs-lookup"><span data-stu-id="e6804-105">This provides Azure customers the flexibility to choose the solution that best meets their business needs.</span></span> <span data-ttu-id="e6804-106">V tomto dokumentu jsme vás seznámí s nové řešení technologie "Azure Disk Encryption pro systém Windows a Linux IaaS virtuálního počítače je" k ochraně a ochranu dat, aby splňovaly vaše organizace zabezpečení a dodržování předpisů závazky.</span><span class="sxs-lookup"><span data-stu-id="e6804-106">In this paper, we will introduce you to a new technology solution “Azure Disk Encryption for Windows and Linux IaaS VM’s” to help protect and safeguard your data to meet your organizational security and compliance commitments.</span></span> <span data-ttu-id="e6804-107">Dokumentu poskytuje podrobné pokyny k použití funkcí Azure disk encryption, včetně Podporované scénáře a uživatel dojde.</span><span class="sxs-lookup"><span data-stu-id="e6804-107">The paper provides detailed guidance on how to use the Azure disk encryption features including the supported scenarios and the user experiences.</span></span>

> [!NOTE]
> <span data-ttu-id="e6804-108">Některá doporučení může zvýšit data, síťové nebo využití výpočetních prostředků, což vede k další náklady na licence nebo předplatné.</span><span class="sxs-lookup"><span data-stu-id="e6804-108">Certain recommendations might increase data, network, or compute resource usage, resulting in additional license or subscription costs.</span></span>

## <a name="overview"></a><span data-ttu-id="e6804-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="e6804-109">Overview</span></span>
<span data-ttu-id="e6804-110">Azure Disk Encryption je novou funkci, která umožňuje šifrovat disky virtuálního počítače s Windows a Linux IaaS.</span><span class="sxs-lookup"><span data-stu-id="e6804-110">Azure Disk Encryption is a new capability that helps you encrypt your Windows and Linux IaaS virtual machine disks.</span></span> <span data-ttu-id="e6804-111">Azure Disk Encryption využívá oborový standard [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) funkce systému Windows a [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) funkce systému Linux zajistit šifrování svazku operačního systému a datové disky.</span><span class="sxs-lookup"><span data-stu-id="e6804-111">Azure Disk Encryption leverages the industry standard [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) feature of Windows and the [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) feature of Linux to provide volume encryption for the OS and the data disks.</span></span> <span data-ttu-id="e6804-112">Řešení je integrovaná s [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) můžete řídit a spravovat šifrování disku klíče a tajné klíče v rámci vašeho předplatného trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-112">The solution is integrated with [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) to help you control and manage the disk-encryption keys and secrets in your key vault subscription.</span></span> <span data-ttu-id="e6804-113">Řešení také zajistí, že všechna data na disky virtuálního počítače jsou zašifrovaná přinejmenším ve službě Azure storage.</span><span class="sxs-lookup"><span data-stu-id="e6804-113">The solution also ensures that all data on the virtual machine disks are encrypted at rest in your Azure storage.</span></span>

<span data-ttu-id="e6804-114">Azure disk encryption pro systém Windows a virtuálních počítačů IaaS Linux je nyní v **obecné dostupnosti** do všech veřejných oblastí Azure a oblasti AzureGov standardní virtuální počítače a virtuální počítače s storage úrovně premium.</span><span class="sxs-lookup"><span data-stu-id="e6804-114">Azure disk encryption for Windows and Linux IaaS VMs is now in **General Availability** in all Azure public regions and AzureGov regions for Standard  VMs and VMs with premium storage.</span></span>

### <a name="encryption-scenarios"></a><span data-ttu-id="e6804-115">Šifrování scénáře</span><span class="sxs-lookup"><span data-stu-id="e6804-115">Encryption scenarios</span></span>
<span data-ttu-id="e6804-116">Řešení Azure Disk Encryption podporuje následující scénáře zákazníka:</span><span class="sxs-lookup"><span data-stu-id="e6804-116">The Azure Disk Encryption solution supports the following customer scenarios:</span></span>

* <span data-ttu-id="e6804-117">Povolit šifrování na nové virtuální počítače IaaS vytvořené z předem šifrované virtuálního pevného disku a šifrovacích klíčů</span><span class="sxs-lookup"><span data-stu-id="e6804-117">Enable encryption on new IaaS VMs created from pre-encrypted VHD and encryption keys</span></span>
* <span data-ttu-id="e6804-118">Povolit šifrování na nové virtuální počítače IaaS vytvořené z podporovaných Azure Galerie obrázků</span><span class="sxs-lookup"><span data-stu-id="e6804-118">Enable encryption on new IaaS VMs created from the supported Azure Gallery images</span></span>
* <span data-ttu-id="e6804-119">Povolit šifrování na existující virtuální počítače IaaS běžící v Azure</span><span class="sxs-lookup"><span data-stu-id="e6804-119">Enable encryption on existing IaaS VMs running in Azure</span></span>
* <span data-ttu-id="e6804-120">Vypnout šifrování u virtuálních počítačů IaaS Windows</span><span class="sxs-lookup"><span data-stu-id="e6804-120">Disable encryption on Windows IaaS VMs</span></span>
* <span data-ttu-id="e6804-121">Zakázat šifrování na datových jednotkách pro virtuální počítače IaaS Linux</span><span class="sxs-lookup"><span data-stu-id="e6804-121">Disable encryption on data drives for Linux IaaS VMs</span></span>
* <span data-ttu-id="e6804-122">Povolit šifrování spravovaných disků na virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="e6804-122">Enable encryption of managed disk VMs</span></span>
* <span data-ttu-id="e6804-123">Aktualizovat nastavení šifrování existující úložiště šifrované neprémiové virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="e6804-123">Update encryption settings of an existing encrypted non-premium storage VM</span></span>
* <span data-ttu-id="e6804-124">Zálohování a obnovení šifrovaných virtuálních počítačů, šifrovaný klíč šifrovacího klíče</span><span class="sxs-lookup"><span data-stu-id="e6804-124">Backup and restore of encrypted VMs, encrypted with key encryption key</span></span>

<span data-ttu-id="e6804-125">Řešení podporuje následující scénáře pro virtuální počítače IaaS, pokud jsou povolené v Microsoft Azure:</span><span class="sxs-lookup"><span data-stu-id="e6804-125">The solution supports the following scenarios for IaaS VMs when they are enabled in Microsoft Azure:</span></span>

* <span data-ttu-id="e6804-126">Integrace s Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e6804-126">Integration with Azure Key Vault</span></span>
* <span data-ttu-id="e6804-127">Úroveň Standard virtuálních počítačů: [A, D, DS, G, GS, F a atd. řady virtuální počítače IaaS](https://azure.microsoft.com/pricing/details/virtual-machines/)</span><span class="sxs-lookup"><span data-stu-id="e6804-127">Standard tier VMs: [A, D, DS, G, GS, F, and so forth series IaaS VMs](https://azure.microsoft.com/pricing/details/virtual-machines/)</span></span>
* <span data-ttu-id="e6804-128">Povolit šifrování na Windows a virtuálních počítačů IaaS Linux spravovaných disků virtuálních počítačů z podporovaných Azure Galerie obrázků</span><span class="sxs-lookup"><span data-stu-id="e6804-128">Enable encryption on Windows and Linux IaaS VMs and managed disk VMs from the supported Azure Gallery images</span></span>
* <span data-ttu-id="e6804-129">Zakázat šifrování na jednotkách operačního systému a dat pro virtuální počítače IaaS Windows a spravovaných disků na virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="e6804-129">Disable encryption on OS and data drives for Windows IaaS VMs and managed disk VMs</span></span>
* <span data-ttu-id="e6804-130">Zakázat šifrování na datových jednotkách pro virtuální počítače IaaS Linux a spravovaných disků na virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="e6804-130">Disable encryption on data drives for Linux IaaS VMs and managed disk VMs</span></span>
* <span data-ttu-id="e6804-131">Povolit šifrování na virtuální počítače IaaS se systémem OS klienta Windows</span><span class="sxs-lookup"><span data-stu-id="e6804-131">Enable encryption on IaaS VMs running Windows Client OS</span></span>
* <span data-ttu-id="e6804-132">Povolit šifrování na svazcích s cestami připojení</span><span class="sxs-lookup"><span data-stu-id="e6804-132">Enable encryption on volumes with mount paths</span></span>
* <span data-ttu-id="e6804-133">Povolit šifrování na virtuální počítače s Linuxem nakonfigurovaný s diskem proložení (RAID) pomocí mdadm</span><span class="sxs-lookup"><span data-stu-id="e6804-133">Enable encryption on Linux VMs configured with disk striping (RAID) using mdadm</span></span>
* <span data-ttu-id="e6804-134">Povolit šifrování na virtuální počítače s Linuxem pomocí LVM pro datové disky</span><span class="sxs-lookup"><span data-stu-id="e6804-134">Enable encryption on Linux VMs using LVM for data disks</span></span>
* <span data-ttu-id="e6804-135">Povolit šifrování na virtuálních počítačích Windows nakonfigurovaný s prostory úložiště</span><span class="sxs-lookup"><span data-stu-id="e6804-135">Enable encryption on Windows VMs configured with Storage Spaces</span></span>
* <span data-ttu-id="e6804-136">Aktualizovat nastavení šifrování existující úložiště šifrované neprémiové virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="e6804-136">Update encryption settings of an existing encrypted non-premium storage VM</span></span>
* <span data-ttu-id="e6804-137">Jsou podporovány všechny veřejné Azure a AzureGov oblastí</span><span class="sxs-lookup"><span data-stu-id="e6804-137">All Azure Public and AzureGov regions are supported</span></span>

<span data-ttu-id="e6804-138">Řešení nepodporuje následující scénáře, funkce a technologie:</span><span class="sxs-lookup"><span data-stu-id="e6804-138">The solution does not support the following scenarios, features, and technology:</span></span>

* <span data-ttu-id="e6804-139">Úroveň Basic virtuálních počítačů IaaS</span><span class="sxs-lookup"><span data-stu-id="e6804-139">Basic tier IaaS VMs</span></span>
* <span data-ttu-id="e6804-140">Zakázáním šifrování na jednotce operačního systému pro virtuální počítače IaaS Linux</span><span class="sxs-lookup"><span data-stu-id="e6804-140">Disabling encryption on an OS drive for Linux IaaS VMs</span></span>
* <span data-ttu-id="e6804-141">Zakázáním šifrování na datová jednotka, pokud je pro virtuální počítače Iaas Linux šifrované jednotky operačního systému</span><span class="sxs-lookup"><span data-stu-id="e6804-141">Disabling encryption on a data drive if the OS drive is encrypted for Linux Iaas VMs</span></span>
* <span data-ttu-id="e6804-142">Virtuální počítače IaaS, která jsou vytvořená pomocí klasického metody vytvoření virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="e6804-142">IaaS VMs that are created by using the classic VM creation method</span></span>
* <span data-ttu-id="e6804-143">Povolte šifrování na systém Windows a virtuálních počítačů Linux IaaS zákazníka vlastních bitových kopií není podporován.</span><span class="sxs-lookup"><span data-stu-id="e6804-143">Enable encryption on Windows and Linux IaaS VMs customer custom images is NOT supported.</span></span> <span data-ttu-id="e6804-144">Povolit enccryption na operační systém Linux LVM disku aktuálně nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="e6804-144">Enable enccryption on Linux LVM OS disk is not supported currently.</span></span> <span data-ttu-id="e6804-145">Tato podpora bude brzy pocházet.</span><span class="sxs-lookup"><span data-stu-id="e6804-145">This support will come soon.</span></span>
* <span data-ttu-id="e6804-146">Integrace s vaší místní služby správy klíčů</span><span class="sxs-lookup"><span data-stu-id="e6804-146">Integration with your on-premises Key Management Service</span></span>
* <span data-ttu-id="e6804-147">Soubory Azure (systém souborů sdíleného), Network File System (NFS), dynamické svazky a virtuální počítače Windows, které jsou nakonfigurované s systémy na bázi softwaru diskového pole RAID</span><span class="sxs-lookup"><span data-stu-id="e6804-147">Azure Files (shared file system), Network File System (NFS), dynamic volumes, and Windows VMs that are configured with software-based RAID systems</span></span>
* <span data-ttu-id="e6804-148">Zálohování a obnovení šifrovaných virtuálních počítačů, šifrované bez klíče šifrovací klíč.</span><span class="sxs-lookup"><span data-stu-id="e6804-148">Backup and restore of encrypted VMs, encrypted without key encryption key.</span></span>
* <span data-ttu-id="e6804-149">Aktualizujte nastavení šifrování existující šifrované premium úložiště virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e6804-149">Update encryption settings of an existing encrypted premium storage VM.</span></span>

> [!NOTE]
> <span data-ttu-id="e6804-150">Zálohování a obnovení šifrovaných virtuálních počítačů je podporována pouze pro virtuální počítače, které jsou zašifrované s konfigurací KEK.</span><span class="sxs-lookup"><span data-stu-id="e6804-150">Backup and restore of encrypted VMs is supported only for VMs that are encrypted with the KEK configuration.</span></span> <span data-ttu-id="e6804-151">Není podporována u virtuálních počítačů, které jsou zašifrované bez KEK.</span><span class="sxs-lookup"><span data-stu-id="e6804-151">It is not supported on VMs that are encrypted without KEK.</span></span> <span data-ttu-id="e6804-152">KEK je volitelný parametr, který povoluje šifrování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e6804-152">KEK is an optional parameter that enables VM encryption.</span></span> <span data-ttu-id="e6804-153">Tato podpora se připravuje.</span><span class="sxs-lookup"><span data-stu-id="e6804-153">This support is coming soon.</span></span>
> <span data-ttu-id="e6804-154">Aktualizujte nastavení šifrování virtuálního počítače nejsou podporovány existující úložiště šifrované premium Storage.</span><span class="sxs-lookup"><span data-stu-id="e6804-154">Update encryption settings of an existing encrypted premium storage VM are not supported.</span></span> <span data-ttu-id="e6804-155">Tato podpora se připravuje.</span><span class="sxs-lookup"><span data-stu-id="e6804-155">This support is coming soon.</span></span>

### <a name="encryption-features"></a><span data-ttu-id="e6804-156">Funkce šifrování</span><span class="sxs-lookup"><span data-stu-id="e6804-156">Encryption features</span></span>
<span data-ttu-id="e6804-157">Když povolíte a nasadíte Azure Disk Encryption pro virtuální počítače Azure IaaS, jsou povolené následující možnosti, v závislosti na konfiguraci poskytuje:</span><span class="sxs-lookup"><span data-stu-id="e6804-157">When you enable and deploy Azure Disk Encryption for Azure IaaS VMs, the following capabilities are enabled, depending on the configuration provided:</span></span>

* <span data-ttu-id="e6804-158">Šifrování svazku operačního systému k ochraně spouštěcího svazku v klidovém stavu uložených v úložišti</span><span class="sxs-lookup"><span data-stu-id="e6804-158">Encryption of the OS volume to protect the boot volume at rest in your storage</span></span>
* <span data-ttu-id="e6804-159">Šifrování datové svazky k ochraně objemy dat v klidovém stavu uložených v úložišti</span><span class="sxs-lookup"><span data-stu-id="e6804-159">Encryption of data volumes to protect the data volumes at rest in your storage</span></span>
* <span data-ttu-id="e6804-160">Zakázáním šifrování na jednotkách operačního systému a dat pro virtuální počítače IaaS Windows</span><span class="sxs-lookup"><span data-stu-id="e6804-160">Disabling encryption on the OS and data drives for Windows IaaS VMs</span></span>
* <span data-ttu-id="e6804-161">Zakázáním šifrování dat (pouze v případě OS jednotka je není zašifrovaná) disky pro virtuální počítače IaaS Linux</span><span class="sxs-lookup"><span data-stu-id="e6804-161">Disabling encryption on the data drives for Linux IaaS VMs (only if OS drive IS NOT encrypted)</span></span>
* <span data-ttu-id="e6804-162">Zabezpečení šifrovacích klíčů a tajných klíčů ve vašem předplatném trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="e6804-162">Safeguarding the encryption keys and secrets in your key vault subscription</span></span>
* <span data-ttu-id="e6804-163">Vytváření sestav stav šifrování šifrovaný IaaS virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="e6804-163">Reporting the encryption status of the encrypted IaaS VM</span></span>
* <span data-ttu-id="e6804-164">Odebrání nastavení konfigurace šifrování disku z virtuálního počítače IaaS</span><span class="sxs-lookup"><span data-stu-id="e6804-164">Removal of disk-encryption configuration settings from the IaaS virtual machine</span></span>
* <span data-ttu-id="e6804-165">Zálohování a obnovení šifrovaných virtuálních počítačů pomocí služby zálohování Azure</span><span class="sxs-lookup"><span data-stu-id="e6804-165">Backup and restore of encrypted VMs by using the Azure Backup service</span></span>

> [!NOTE]
> <span data-ttu-id="e6804-166">Zálohování a obnovení šifrovaných virtuálních počítačů je podporována pouze pro virtuální počítače, které jsou zašifrované s konfigurací KEK.</span><span class="sxs-lookup"><span data-stu-id="e6804-166">Backup and restore of encrypted VMs is supported only for VMs that are encrypted with the KEK configuration.</span></span> <span data-ttu-id="e6804-167">Není podporována u virtuálních počítačů, které jsou zašifrované bez KEK.</span><span class="sxs-lookup"><span data-stu-id="e6804-167">It is not supported on VMs that are encrypted without KEK.</span></span> <span data-ttu-id="e6804-168">KEK je volitelný parametr, který povoluje šifrování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e6804-168">KEK is an optional parameter that enables VM encryption.</span></span>

<span data-ttu-id="e6804-169">Azure Disk Encryption pro virtuální počítače IaaS pro Windows a Linux řešení zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="e6804-169">Azure Disk Encryption for IaaS VMS for Windows and Linux solution includes:</span></span>

* <span data-ttu-id="e6804-170">Šifrování disku rozšíření pro Windows.</span><span class="sxs-lookup"><span data-stu-id="e6804-170">The disk-encryption extension for Windows.</span></span>
* <span data-ttu-id="e6804-171">Šifrování disku rozšíření pro Linux.</span><span class="sxs-lookup"><span data-stu-id="e6804-171">The disk-encryption extension for Linux.</span></span>
* <span data-ttu-id="e6804-172">Rutiny prostředí PowerShell šifrování disku.</span><span class="sxs-lookup"><span data-stu-id="e6804-172">The disk-encryption PowerShell cmdlets.</span></span>
* <span data-ttu-id="e6804-173">Disk šifrování rutiny rozhraní příkazového řádku Azure (CLI).</span><span class="sxs-lookup"><span data-stu-id="e6804-173">The disk-encryption Azure command-line interface (CLI) cmdlets.</span></span>
* <span data-ttu-id="e6804-174">Šablony Azure Resource Manager šifrování disku.</span><span class="sxs-lookup"><span data-stu-id="e6804-174">The disk-encryption Azure Resource Manager templates.</span></span>

<span data-ttu-id="e6804-175">Podporuje řešení Azure Disk Encryption na virtuální počítače IaaS se systémem Windows nebo operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="e6804-175">The Azure Disk Encryption solution is supported on IaaS VMs that are running Windows or Linux OS.</span></span> <span data-ttu-id="e6804-176">Další informace o podporovaných operačních systémů najdete v části "Požadavky".</span><span class="sxs-lookup"><span data-stu-id="e6804-176">For more information about the supported operating systems, see the "Prerequisites" section.</span></span>

> [!NOTE]
> <span data-ttu-id="e6804-177">Není k dispozici bez dalších poplatků pro šifrování disky virtuálních počítačů s Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="e6804-177">There is no additional charge for encrypting VM disks with Azure Disk Encryption.</span></span>

### <a name="value-proposition"></a><span data-ttu-id="e6804-178">Návrh hodnoty</span><span class="sxs-lookup"><span data-stu-id="e6804-178">Value proposition</span></span>
<span data-ttu-id="e6804-179">Při použití řešení Azure Disk Encryption správy se může stát odpovědí následující obchodních potřeb:</span><span class="sxs-lookup"><span data-stu-id="e6804-179">When you apply the Azure Disk Encryption-management solution, you can satisfy the following business needs:</span></span>

* <span data-ttu-id="e6804-180">Virtuální počítače IaaS jsou zabezpečeny v klidu, protože šifrování standardní technologii můžete použít k vyřešení organizační požadavky na zabezpečení a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="e6804-180">IaaS VMs are secured at rest, because you can use industry-standard encryption technology to address organizational security and compliance requirements.</span></span>
* <span data-ttu-id="e6804-181">Spouštění virtuálních počítačů IaaS v části klíče řídí zákazníka a zásady a auditovat jejich využití v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-181">IaaS VMs boot under customer-controlled keys and policies, and you can audit their usage in your key vault.</span></span>

### <a name="encryption-workflow"></a><span data-ttu-id="e6804-182">Pracovní postup šifrování</span><span class="sxs-lookup"><span data-stu-id="e6804-182">Encryption workflow</span></span>
<span data-ttu-id="e6804-183">Pokud chcete povolit šifrování disku pro systém Windows a virtuální počítače s Linuxem, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e6804-183">To enable disk encryption for Windows and Linux VMs, do the following:</span></span>

1. <span data-ttu-id="e6804-184">Zvolte scénářem šifrování některé z těchto scénářů předchozí šifrování.</span><span class="sxs-lookup"><span data-stu-id="e6804-184">Choose an encryption scenario from among the preceding encryption scenarios.</span></span>
2. <span data-ttu-id="e6804-185">Vyjádřit výslovný souhlas pro povolení šifrování pomocí šablony Azure Disk Encryption Resource Manageru, rutiny prostředí PowerShell nebo rozhraní příkazového řádku příkaz na disku a zadejte nastavení šifrování.</span><span class="sxs-lookup"><span data-stu-id="e6804-185">Opt in to enabling disk encryption via the Azure Disk Encryption Resource Manager template, PowerShell cmdlets, or CLI command, and specify the encryption configuration.</span></span>

   * <span data-ttu-id="e6804-186">Pro virtuální pevný disk scénář šifrované zákazníka nahrajte šifrované virtuální pevný disk do účtu úložiště a materiál šifrovací klíče do trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-186">For the customer-encrypted VHD scenario, upload the encrypted VHD to your storage account and the encryption key material to your key vault.</span></span> <span data-ttu-id="e6804-187">Potom zadejte konfiguraci šifrování chcete povolit šifrování na nový virtuální počítač IaaS.</span><span class="sxs-lookup"><span data-stu-id="e6804-187">Then, provide the encryption configuration to enable encryption on a new IaaS VM.</span></span>
   * <span data-ttu-id="e6804-188">Pro nové virtuální počítače, které jsou vytvořené z Marketplace a stávající virtuální počítače, které jsou již spuštěny v Azure zadejte konfiguraci šifrování chcete povolit šifrování na virtuální počítač IaaS.</span><span class="sxs-lookup"><span data-stu-id="e6804-188">For new VMs that are created from the Marketplace and existing VMs that are already running in Azure, provide the encryption configuration to enable encryption on the IaaS VM.</span></span>

3. <span data-ttu-id="e6804-189">Udělit přístup na platformu Azure ke čtení materiálu šifrovací klíč (šifrovací klíče nástroje BitLocker pro systémy Windows) a heslo pro Linux z trezoru klíčů na virtuální počítač IaaS povolit šifrování.</span><span class="sxs-lookup"><span data-stu-id="e6804-189">Grant access to the Azure platform to read the encryption-key material (BitLocker encryption keys for Windows systems and Passphrase for Linux) from your key vault to enable encryption on the IaaS VM.</span></span>

4. <span data-ttu-id="e6804-190">Zadejte identitu aplikace Azure Active Directory (Azure AD) k zápisu do trezoru klíčů podstatným šifrovací klíč.</span><span class="sxs-lookup"><span data-stu-id="e6804-190">Provide the Azure Active Directory (Azure AD) application identity to write the encryption key material to your key vault.</span></span> <span data-ttu-id="e6804-191">Díky tomu šifrování u virtuálních počítačů IaaS pro scénáře uveden v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="e6804-191">Doing so enables encryption on the IaaS VM for the scenarios mentioned in step 2.</span></span>

5. <span data-ttu-id="e6804-192">Azure aktualizace modelu služby virtuálních počítačů pomocí šifrování a konfigurace trezoru klíčů a nastaví šifrované virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e6804-192">Azure updates the VM service model with encryption and the key vault configuration, and sets up your encrypted VM.</span></span>

 ![Microsoft Antimalware v Azure](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

### <a name="decryption-workflow"></a><span data-ttu-id="e6804-194">Dešifrování pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="e6804-194">Decryption workflow</span></span>
<span data-ttu-id="e6804-195">Pokud chcete zakázat šifrování disku pro virtuální počítače IaaS, proveďte následující postup vysoké úrovně:</span><span class="sxs-lookup"><span data-stu-id="e6804-195">To disable disk encryption for IaaS VMs, complete the following high-level steps:</span></span>

1. <span data-ttu-id="e6804-196">Zvolte můžete zakázat šifrování (dešifrování) na spuštění virtuálního počítače IaaS v Azure pomocí šablony Azure Disk Encryption Resource Manageru nebo rutiny prostředí PowerShell a zadejte konfiguraci dešifrování.</span><span class="sxs-lookup"><span data-stu-id="e6804-196">Choose to disable encryption (decryption) on a running IaaS VM in Azure via the Azure Disk Encryption Resource Manager template or PowerShell cmdlets, and specify the decryption configuration.</span></span>

 <span data-ttu-id="e6804-197">Tento krok zakazuje šifrování operačního systému nebo datový svazek nebo obojí do spuštěného virtuálního počítače Windows IaaS.</span><span class="sxs-lookup"><span data-stu-id="e6804-197">This step disables encryption of the OS or the data volume or both on the running Windows IaaS VM.</span></span> <span data-ttu-id="e6804-198">Nicméně jak je uvedeno v předchozí části, zakázáním šifrování disku operačního systému Linux nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="e6804-198">However, as mentioned in the previous section, disabling OS disk encryption for Linux is not supported.</span></span> <span data-ttu-id="e6804-199">Krok dešifrování je povoleno pouze pro datové jednotky v virtuální počítače s Linuxem, dokud není šifrován disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="e6804-199">The decryption step is allowed only for data drives on Linux VMs as long as the OS disk is not encrypted.</span></span>
2. <span data-ttu-id="e6804-200">Aktualizace modelu služby virtuálních počítačů Azure a virtuální počítač IaaS je označený dešifrovaný.</span><span class="sxs-lookup"><span data-stu-id="e6804-200">Azure updates the VM service model, and the IaaS VM is marked decrypted.</span></span> <span data-ttu-id="e6804-201">Obsah virtuálního počítače jsou již v zašifrované podobě.</span><span class="sxs-lookup"><span data-stu-id="e6804-201">The contents of the VM are no longer encrypted at rest.</span></span>

> [!NOTE]
> <span data-ttu-id="e6804-202">Operace disable šifrování nedojde k odstranění trezoru klíčů a materiál klíče šifrování (šifrovací klíče nástroje BitLocker pro systémy Windows) nebo přístupové heslo pro Linux.</span><span class="sxs-lookup"><span data-stu-id="e6804-202">The disable-encryption operation does not delete your key vault and the encryption key material (BitLocker encryption keys for Windows systems or Passphrase for Linux).</span></span>
 > <span data-ttu-id="e6804-203">Zakázáním šifrování disku operačního systému Linux není podporována.</span><span class="sxs-lookup"><span data-stu-id="e6804-203">Disabling OS disk encryption for Linux is not supported.</span></span> <span data-ttu-id="e6804-204">Krok dešifrování je povoleno pouze pro datové jednotky v virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="e6804-204">The decryption step is allowed only for data drives on Linux VMs.</span></span>
<span data-ttu-id="e6804-205">Zakázáním šifrování disku data pro Linux není podporována, pokud je šifrované jednotky operačního systému.</span><span class="sxs-lookup"><span data-stu-id="e6804-205">Disabling data disk encryption for Linux is not supported if the OS drive is encrypted.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e6804-206">Požadavky</span><span class="sxs-lookup"><span data-stu-id="e6804-206">Prerequisites</span></span>
<span data-ttu-id="e6804-207">Než povolíte Azure Disk Encryption na virtuálních počítačích Azure IaaS pro podporované scénáře, které bylo popsané v části "Přehled", viz následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="e6804-207">Before you enable Azure Disk Encryption on Azure IaaS VMs for the supported scenarios that were discussed in the "Overview" section, see the following prerequisites:</span></span>

* <span data-ttu-id="e6804-208">Musí mít platné aktivní předplatné Azure k vytváření prostředků v Azure v podporovaných oblastí.</span><span class="sxs-lookup"><span data-stu-id="e6804-208">You must have a valid active Azure subscription to create resources in Azure in the supported regions.</span></span>
* <span data-ttu-id="e6804-209">Azure Disk Encryption je podporována v těchto verzích Windows serveru: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 a Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="e6804-209">Azure Disk Encryption is supported on the following Windows Server versions: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, and Windows Server 2016.</span></span>
* <span data-ttu-id="e6804-210">Azure Disk Encryption je podporována v následujících klientských verzí Windows: klienta systému Windows 8 a Windows 10 klienta.</span><span class="sxs-lookup"><span data-stu-id="e6804-210">Azure Disk Encryption is supported on the following Windows client versions: Windows 8 client and Windows 10 client.</span></span>

> [!NOTE]
> <span data-ttu-id="e6804-211">Pro Windows Server 2008 R2 musíte mít rozhraní .NET Framework 4.5 nainstalované dříve než povolíte šifrování v Azure.</span><span class="sxs-lookup"><span data-stu-id="e6804-211">For Windows Server 2008 R2, you must have .NET Framework 4.5 installed before you enable encryption in Azure.</span></span> <span data-ttu-id="e6804-212">Můžete ji nainstalovat z webu Windows Update nainstalováním volitelnou aktualizaci rozhraní Microsoft .NET Framework 4.5.2 na x64 systémů Windows Server 2008 R2 ([KB2901983](https://support.microsoft.com/kb/2901983)).</span><span class="sxs-lookup"><span data-stu-id="e6804-212">You can install it from Windows Update by installing the optional update Microsoft .NET Framework 4.5.2 for Windows Server 2008 R2 x64-based systems ([KB2901983](https://support.microsoft.com/kb/2901983)).</span></span>

* <span data-ttu-id="e6804-213">Azure Disk Encryption je podporována v následujících Galerie Azure na základě Linux serveru distribucích a verzích:</span><span class="sxs-lookup"><span data-stu-id="e6804-213">Azure Disk Encryption is supported on the following Azure Gallery based Linux server distributions and versions:</span></span>

| <span data-ttu-id="e6804-214">Distribuce systému Linux</span><span class="sxs-lookup"><span data-stu-id="e6804-214">Linux Distribution</span></span> | <span data-ttu-id="e6804-215">Verze</span><span class="sxs-lookup"><span data-stu-id="e6804-215">Version</span></span> | <span data-ttu-id="e6804-216">Typ svazku podporovaný pro šifrování</span><span class="sxs-lookup"><span data-stu-id="e6804-216">Volume Type Supported for Encryption</span></span>|
| --- | --- |--- |
| <span data-ttu-id="e6804-217">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="e6804-217">Ubuntu</span></span> | <span data-ttu-id="e6804-218">16.04. DENNĚ LTS</span><span class="sxs-lookup"><span data-stu-id="e6804-218">16.04-DAILY-LTS</span></span> | <span data-ttu-id="e6804-219">Disk operačního systému a dat</span><span class="sxs-lookup"><span data-stu-id="e6804-219">OS and Data disk</span></span> |
| <span data-ttu-id="e6804-220">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="e6804-220">Ubuntu</span></span> | <span data-ttu-id="e6804-221">14.04.5-DAILY-LTS</span><span class="sxs-lookup"><span data-stu-id="e6804-221">14.04.5-DAILY-LTS</span></span> | <span data-ttu-id="e6804-222">Disk operačního systému a dat</span><span class="sxs-lookup"><span data-stu-id="e6804-222">OS and Data disk</span></span> |
| <span data-ttu-id="e6804-223">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="e6804-223">Ubuntu</span></span> | <span data-ttu-id="e6804-224">12.10</span><span class="sxs-lookup"><span data-stu-id="e6804-224">12.10</span></span> | <span data-ttu-id="e6804-225">Datový disk</span><span class="sxs-lookup"><span data-stu-id="e6804-225">Data disk</span></span> |
| <span data-ttu-id="e6804-226">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="e6804-226">Ubuntu</span></span> | <span data-ttu-id="e6804-227">12.04</span><span class="sxs-lookup"><span data-stu-id="e6804-227">12.04</span></span> | <span data-ttu-id="e6804-228">Datový disk</span><span class="sxs-lookup"><span data-stu-id="e6804-228">Data disk</span></span> |
| <span data-ttu-id="e6804-229">RHEL</span><span class="sxs-lookup"><span data-stu-id="e6804-229">RHEL</span></span> | <span data-ttu-id="e6804-230">7.3</span><span class="sxs-lookup"><span data-stu-id="e6804-230">7.3</span></span> | <span data-ttu-id="e6804-231">Disk operačního systému a dat</span><span class="sxs-lookup"><span data-stu-id="e6804-231">OS and Data disk</span></span> |
| <span data-ttu-id="e6804-232">RHEL</span><span class="sxs-lookup"><span data-stu-id="e6804-232">RHEL</span></span> | <span data-ttu-id="e6804-233">7.2</span><span class="sxs-lookup"><span data-stu-id="e6804-233">7.2</span></span> | <span data-ttu-id="e6804-234">Disk operačního systému a dat</span><span class="sxs-lookup"><span data-stu-id="e6804-234">OS and Data disk</span></span> |
| <span data-ttu-id="e6804-235">RHEL</span><span class="sxs-lookup"><span data-stu-id="e6804-235">RHEL</span></span> | <span data-ttu-id="e6804-236">6.8</span><span class="sxs-lookup"><span data-stu-id="e6804-236">6.8</span></span> | <span data-ttu-id="e6804-237">Disk operačního systému a dat</span><span class="sxs-lookup"><span data-stu-id="e6804-237">OS and Data disk</span></span> |
| <span data-ttu-id="e6804-238">RHEL</span><span class="sxs-lookup"><span data-stu-id="e6804-238">RHEL</span></span> | <span data-ttu-id="e6804-239">6.7</span><span class="sxs-lookup"><span data-stu-id="e6804-239">6.7</span></span> | <span data-ttu-id="e6804-240">Datový disk</span><span class="sxs-lookup"><span data-stu-id="e6804-240">Data disk</span></span> |
| <span data-ttu-id="e6804-241">CentOS</span><span class="sxs-lookup"><span data-stu-id="e6804-241">CentOS</span></span> | <span data-ttu-id="e6804-242">7.3</span><span class="sxs-lookup"><span data-stu-id="e6804-242">7.3</span></span> | <span data-ttu-id="e6804-243">Disk operačního systému a dat</span><span class="sxs-lookup"><span data-stu-id="e6804-243">OS and Data disk</span></span> |
| <span data-ttu-id="e6804-244">CentOS</span><span class="sxs-lookup"><span data-stu-id="e6804-244">CentOS</span></span> | <span data-ttu-id="e6804-245">7.2N</span><span class="sxs-lookup"><span data-stu-id="e6804-245">7.2n</span></span> | <span data-ttu-id="e6804-246">Disk operačního systému a dat</span><span class="sxs-lookup"><span data-stu-id="e6804-246">OS and Data disk</span></span> |
| <span data-ttu-id="e6804-247">CentOS</span><span class="sxs-lookup"><span data-stu-id="e6804-247">CentOS</span></span> | <span data-ttu-id="e6804-248">6.8</span><span class="sxs-lookup"><span data-stu-id="e6804-248">6.8</span></span> | <span data-ttu-id="e6804-249">Disk operačního systému a dat</span><span class="sxs-lookup"><span data-stu-id="e6804-249">OS and Data disk</span></span> |
| <span data-ttu-id="e6804-250">CentOS</span><span class="sxs-lookup"><span data-stu-id="e6804-250">CentOS</span></span> | <span data-ttu-id="e6804-251">7.1</span><span class="sxs-lookup"><span data-stu-id="e6804-251">7.1</span></span> | <span data-ttu-id="e6804-252">Datový disk</span><span class="sxs-lookup"><span data-stu-id="e6804-252">Data disk</span></span> |
| <span data-ttu-id="e6804-253">CentOS</span><span class="sxs-lookup"><span data-stu-id="e6804-253">CentOS</span></span> | <span data-ttu-id="e6804-254">7.0</span><span class="sxs-lookup"><span data-stu-id="e6804-254">7.0</span></span> | <span data-ttu-id="e6804-255">Datový disk</span><span class="sxs-lookup"><span data-stu-id="e6804-255">Data disk</span></span> |
| <span data-ttu-id="e6804-256">CentOS</span><span class="sxs-lookup"><span data-stu-id="e6804-256">CentOS</span></span> | <span data-ttu-id="e6804-257">6.7</span><span class="sxs-lookup"><span data-stu-id="e6804-257">6.7</span></span> | <span data-ttu-id="e6804-258">Datový disk</span><span class="sxs-lookup"><span data-stu-id="e6804-258">Data disk</span></span> |
| <span data-ttu-id="e6804-259">CentOS</span><span class="sxs-lookup"><span data-stu-id="e6804-259">CentOS</span></span> | <span data-ttu-id="e6804-260">6.6</span><span class="sxs-lookup"><span data-stu-id="e6804-260">6.6</span></span> | <span data-ttu-id="e6804-261">Datový disk</span><span class="sxs-lookup"><span data-stu-id="e6804-261">Data disk</span></span> |
| <span data-ttu-id="e6804-262">CentOS</span><span class="sxs-lookup"><span data-stu-id="e6804-262">CentOS</span></span> | <span data-ttu-id="e6804-263">6.5</span><span class="sxs-lookup"><span data-stu-id="e6804-263">6.5</span></span> | <span data-ttu-id="e6804-264">Datový disk</span><span class="sxs-lookup"><span data-stu-id="e6804-264">Data disk</span></span> |
| <span data-ttu-id="e6804-265">openSUSE</span><span class="sxs-lookup"><span data-stu-id="e6804-265">openSUSE</span></span> | <span data-ttu-id="e6804-266">13.2</span><span class="sxs-lookup"><span data-stu-id="e6804-266">13.2</span></span> | <span data-ttu-id="e6804-267">Datový disk</span><span class="sxs-lookup"><span data-stu-id="e6804-267">Data disk</span></span> |
| <span data-ttu-id="e6804-268">SLES</span><span class="sxs-lookup"><span data-stu-id="e6804-268">SLES</span></span> | <span data-ttu-id="e6804-269">12 SP1</span><span class="sxs-lookup"><span data-stu-id="e6804-269">12 SP1</span></span> | <span data-ttu-id="e6804-270">Datový disk</span><span class="sxs-lookup"><span data-stu-id="e6804-270">Data disk</span></span> |
| <span data-ttu-id="e6804-271">SLES</span><span class="sxs-lookup"><span data-stu-id="e6804-271">SLES</span></span> | <span data-ttu-id="e6804-272">12-SP1 (Premium)</span><span class="sxs-lookup"><span data-stu-id="e6804-272">12-SP1 (Premium)</span></span> | <span data-ttu-id="e6804-273">Datový disk</span><span class="sxs-lookup"><span data-stu-id="e6804-273">Data disk</span></span> |
| <span data-ttu-id="e6804-274">SLES</span><span class="sxs-lookup"><span data-stu-id="e6804-274">SLES</span></span> | <span data-ttu-id="e6804-275">HPC 12</span><span class="sxs-lookup"><span data-stu-id="e6804-275">HPC 12</span></span> | <span data-ttu-id="e6804-276">Datový disk</span><span class="sxs-lookup"><span data-stu-id="e6804-276">Data disk</span></span> |
| <span data-ttu-id="e6804-277">SLES</span><span class="sxs-lookup"><span data-stu-id="e6804-277">SLES</span></span> | <span data-ttu-id="e6804-278">11-SP4 (Premium)</span><span class="sxs-lookup"><span data-stu-id="e6804-278">11-SP4 (Premium)</span></span> | <span data-ttu-id="e6804-279">Datový disk</span><span class="sxs-lookup"><span data-stu-id="e6804-279">Data disk</span></span> |
| <span data-ttu-id="e6804-280">SLES</span><span class="sxs-lookup"><span data-stu-id="e6804-280">SLES</span></span> | <span data-ttu-id="e6804-281">11 SP4</span><span class="sxs-lookup"><span data-stu-id="e6804-281">11 SP4</span></span> | <span data-ttu-id="e6804-282">Datový disk</span><span class="sxs-lookup"><span data-stu-id="e6804-282">Data disk</span></span> |

* <span data-ttu-id="e6804-283">Azure Disk Encryption vyžaduje, aby váš trezor klíčů a virtuální počítače jsou umístěny ve stejné oblasti Azure a předplatné.</span><span class="sxs-lookup"><span data-stu-id="e6804-283">Azure Disk Encryption requires that your key vault and VMs reside in the same Azure region and subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="e6804-284">Konfigurace prostředků v oblastech způsobí selhání při povolování funkce Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="e6804-284">Configuring the resources in separate regions causes a failure in enabling the Azure Disk Encryption feature.</span></span>

* <span data-ttu-id="e6804-285">Nastavení a konfigurace trezoru klíčů pro Azure Disk Encryption, najdete v tématu **nastavit registrace a konfigurace trezoru klíčů pro Azure Disk Encryption** v *požadavky* tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="e6804-285">To set up and configure your key vault for Azure Disk Encryption, see section **Set up and configure your key vault for Azure Disk Encryption** in the *Prerequisites* section of this article.</span></span>
* <span data-ttu-id="e6804-286">Nastavení a konfigurace aplikace Azure AD ve službě Azure Active directory pro Azure Disk Encryption, najdete v tématu **nastavit aplikaci Azure AD ve službě Azure Active Directory** v *požadavky* část v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="e6804-286">To set up and configure Azure AD application in Azure Active directory for Azure Disk Encryption, see section **Set up the Azure AD application in Azure Active Directory** in the *Prerequisites* section of this article.</span></span>
* <span data-ttu-id="e6804-287">Nastavit a konfigurovat zásady přístupu k trezoru klíčů pro aplikace Azure AD, najdete v tématu **nastavte zásady přístupu trezoru klíčů pro aplikace Azure AD** v *požadavky* tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="e6804-287">To set up and configure the key vault access policy for the Azure AD application, see section **Set up the key vault access policy for the Azure AD application** in the *Prerequisites* section of this article.</span></span>
* <span data-ttu-id="e6804-288">Příprava předem šifrované virtuálního pevného disku Windows, najdete v tématu **připravit předem šifrované virtuálního pevného disku Windows** v *příloha*.</span><span class="sxs-lookup"><span data-stu-id="e6804-288">To prepare a pre-encrypted Windows VHD, see section **Prepare a pre-encrypted Windows VHD** in the *Appendix*.</span></span>
* <span data-ttu-id="e6804-289">Příprava předem šifrované Linux virtuální pevný disk, najdete v tématu **připravit předem šifrované VHD Linux** v *příloha*.</span><span class="sxs-lookup"><span data-stu-id="e6804-289">To prepare a pre-encrypted Linux VHD, see section **Prepare a pre-encrypted Linux VHD** in the *Appendix*.</span></span>
* <span data-ttu-id="e6804-290">Platformy Azure potřebuje přístup k šifrovacích klíčů nebo tajných klíčů v trezoru klíčů, aby byly k dispozici pro virtuální počítač při spuštění a dešifruje svazek operačního systému virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e6804-290">The Azure platform needs access to the encryption keys or secrets in your key vault to make them available to the virtual machine when it boots and decrypts the virtual machine OS volume.</span></span> <span data-ttu-id="e6804-291">Chcete-li udělit oprávnění pro platformu Azure, nastavte **EnabledForDiskEncryption** vlastnost v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-291">To grant permissions to Azure platform, set the **EnabledForDiskEncryption** property in the key vault.</span></span> <span data-ttu-id="e6804-292">Další informace najdete v tématu **nastavit registrace a konfigurace trezoru klíčů pro Azure Disk Encryption** v příloze.</span><span class="sxs-lookup"><span data-stu-id="e6804-292">For more information, see **Set up and configure your key vault for Azure Disk Encryption** in the Appendix.</span></span>
* <span data-ttu-id="e6804-293">Tajný klíč trezoru klíčů a adresy URL KEK musí být verzí.</span><span class="sxs-lookup"><span data-stu-id="e6804-293">Your key vault secret and KEK URLs must be versioned.</span></span> <span data-ttu-id="e6804-294">Azure vynucuje toto omezení Správa verzí.</span><span class="sxs-lookup"><span data-stu-id="e6804-294">Azure enforces this restriction of versioning.</span></span> <span data-ttu-id="e6804-295">Platný tajný klíč a KEK adresy URL najdete v následujících příkladech:</span><span class="sxs-lookup"><span data-stu-id="e6804-295">For valid secret and KEK URLs, see the following examples:</span></span>

  * <span data-ttu-id="e6804-296">Příklad platnou adresu URL tajný: *https://contosovault.vault.azure.net/secrets/BitLockerEncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span><span class="sxs-lookup"><span data-stu-id="e6804-296">Example of a valid secret URL:   *https://contosovault.vault.azure.net/secrets/BitLockerEncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span></span>
  * <span data-ttu-id="e6804-297">Příklad platnou adresu URL KEK: *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span><span class="sxs-lookup"><span data-stu-id="e6804-297">Example of a valid KEK URL:   *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span></span>

* <span data-ttu-id="e6804-298">Azure Disk Encryption nepodporuje zadání čísla portu jako součást tajné klíče trezoru klíčů a adresy URL KEK.</span><span class="sxs-lookup"><span data-stu-id="e6804-298">Azure Disk Encryption does not support specifying port numbers as part of key vault secrets and KEK URLs.</span></span> <span data-ttu-id="e6804-299">Příkladem adresy URL není podporován a podporované trezoru klíčů naleznete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="e6804-299">For examples of non-supported and supported key vault URLs, see the following:</span></span>

  * <span data-ttu-id="e6804-300">Adresa URL nemůže být přijata trezoru klíčů *https://contosovault.vault.azure.net:443 nebo tajných klíčů nebo contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span><span class="sxs-lookup"><span data-stu-id="e6804-300">Unacceptable key vault URL  *https://contosovault.vault.azure.net:443/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span></span>
  * <span data-ttu-id="e6804-301">Adresa URL přijatelné trezoru klíčů: *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span><span class="sxs-lookup"><span data-stu-id="e6804-301">Acceptable key vault URL:   *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span></span>

* <span data-ttu-id="e6804-302">Povolit Azure Disk Encryption funkce, virtuální počítače IaaS musí splňovat následující požadavky konfigurace koncového bodu sítě:</span><span class="sxs-lookup"><span data-stu-id="e6804-302">To enable the Azure Disk Encryption feature, the IaaS VMs must meet the following network endpoint configuration requirements:</span></span>
  * <span data-ttu-id="e6804-303">Získá token pro připojení k trezoru klíčů, musí být možné se připojit k Azure Active Directory koncový bod, virtuálních počítačů IaaS \[login.microsoftonline.com\].</span><span class="sxs-lookup"><span data-stu-id="e6804-303">To get a token to connect to your key vault, the IaaS VM must be able to connect to an Azure Active Directory endpoint, \[login.microsoftonline.com\].</span></span>
  * <span data-ttu-id="e6804-304">Zapsat šifrovací klíče do trezoru klíčů, musí být virtuální počítač IaaS možné se připojit ke koncovému bodu trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-304">To write the encryption keys to your key vault, the IaaS VM must be able to connect to the key vault endpoint.</span></span>
  * <span data-ttu-id="e6804-305">Virtuální počítač IaaS musí být schopný se připojit k úložišti Azure koncový bod, který je hostitelem úložiště rozšíření Azure a účet úložiště Azure, který je hostitelem souborů virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="e6804-305">The IaaS VM must be able to connect to an Azure storage endpoint that hosts the Azure extension repository and an Azure storage account that hosts the VHD files.</span></span>

  > [!NOTE]
  > <span data-ttu-id="e6804-306">Pokud vaše zásady zabezpečení omezuje přístup z virtuálních počítačů Azure k Internetu, můžete předchozí URI vyřešit a nakonfigurovat konkrétní pravidlo povolení odchozí připojení k IP adresy.</span><span class="sxs-lookup"><span data-stu-id="e6804-306">If your security policy limits access from Azure VMs to the Internet, you can resolve the preceding URI and configure a specific rule to allow outbound connectivity to the IPs.</span></span>
  >
  ><span data-ttu-id="e6804-307">Ke konfiguraci a přístup k Azure Key Vault za bránou firewall (https://docs.microsoft.com/en-us/azure/key-vault/key-vault-access-behind-firewall)</span><span class="sxs-lookup"><span data-stu-id="e6804-307">To configure and access Azure Key Vault behind a firewall(https://docs.microsoft.com/en-us/azure/key-vault/key-vault-access-behind-firewall)</span></span>

* <span data-ttu-id="e6804-308">Použijte nejnovější verzi Azure PowerShell SDK verze konfigurovat Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="e6804-308">Use the latest version of Azure PowerShell SDK version to configure Azure Disk Encryption.</span></span> <span data-ttu-id="e6804-309">Stáhněte si nejnovější verzi [verzi prostředí Azure PowerShell](https://github.com/Azure/azure-powershell/releases)</span><span class="sxs-lookup"><span data-stu-id="e6804-309">Download the latest version of [Azure PowerShell release](https://github.com/Azure/azure-powershell/releases)</span></span>

 > [!NOTE]
  > <span data-ttu-id="e6804-310">Azure Disk Encryption není podporována na [Azure PowerShell SDK verze 1.1.0](https://github.com/Azure/azure-powershell/releases/tag/v1.1.0-January2016).</span><span class="sxs-lookup"><span data-stu-id="e6804-310">Azure Disk Encryption is not supported on [Azure PowerShell SDK version 1.1.0](https://github.com/Azure/azure-powershell/releases/tag/v1.1.0-January2016).</span></span> <span data-ttu-id="e6804-311">Pokud jste obdrželi chybu související s použitím prostředí Azure PowerShell 1.1.0, najdete v části [Azure Disk Encryption chyby související s Azure PowerShell 1.1.0](http://blogs.msdn.com/b/azuresecurity/archive/2016/02/10/azure-disk-encryption-error-related-to-azure-powershell-1-1-0.aspx).</span><span class="sxs-lookup"><span data-stu-id="e6804-311">If you are receiving an error related to using Azure PowerShell 1.1.0, see [Azure Disk Encryption Error Related to Azure PowerShell 1.1.0](http://blogs.msdn.com/b/azuresecurity/archive/2016/02/10/azure-disk-encryption-error-related-to-azure-powershell-1-1-0.aspx).</span></span>

* <span data-ttu-id="e6804-312">Pokud chcete spustit libovolný příkaz příkazového řádku Azure CLI a přidružit ho ke svému předplatnému Azure, musíte nejprve nainstalovat rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="e6804-312">To run any Azure CLI command and associate it with your Azure subscription, you must first install Azure CLI:</span></span>
  * <span data-ttu-id="e6804-313">Chcete-li nainstalovat rozhraní příkazového řádku Azure a přidružit ho ke svému předplatnému Azure, najdete v části [postup instalace a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="e6804-313">To install Azure CLI and associate it with your Azure subscription, see [How to install and configure Azure CLI](../cli-install-nodejs.md).</span></span>
  * <span data-ttu-id="e6804-314">Pomocí příkazového řádku Azure CLI pro Mac, Linux a Windows pomocí Azure Resource Manageru naleznete v části [rozhraní příkazového řádku Azure v režimu Resource Manager](../virtual-machines/azure-cli-arm-commands.md).</span><span class="sxs-lookup"><span data-stu-id="e6804-314">To use Azure CLI for Mac, Linux, and Windows with Azure Resource Manager, see [Azure CLI commands in Resource Manager mode](../virtual-machines/azure-cli-arm-commands.md).</span></span>

* <span data-ttu-id="e6804-315">Při šifrování se spravovaným diskem, je povinné požadavek na převedení snímek spravovaného disku nebo zálohování na disk mimo Azure Disk Encryption před povolením šifrování.</span><span class="sxs-lookup"><span data-stu-id="e6804-315">When encrypting a managed disk, it is mandatory prerequisite to take a snapshot of the managed disk or a backup of the disk outside of Azure Disk Encryption prior to enabling encryption.</span></span>  <span data-ttu-id="e6804-316">Bez předchozího provedení zálohy na místě jakákoli Neočekávaná chyba při šifrování může vykreslit disk a virtuální počítač nedostupný bez možnosti obnovení.</span><span class="sxs-lookup"><span data-stu-id="e6804-316">Without a backup in place, any unexpected failure during encryption may render the disk and VM inaccessible without a recovery option.</span></span>  <span data-ttu-id="e6804-317">Set-AzureRmVMDiskEncryptionExtension není aktuálně zálohovat spravovaných disků a bude chyba, je-li použít u se spravovaným diskem, pokud je zadán parametr - skipVmBackup.</span><span class="sxs-lookup"><span data-stu-id="e6804-317">Set-AzureRmVMDiskEncryptionExtension does not currently back up managed disks and will error if used against a managed disk unless the -skipVmBackup parameter has been specified.</span></span>  <span data-ttu-id="e6804-318">Tento parametr nebezpečné pro používání Pokud zálohu již byl změněn mimo Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="e6804-318">This parameter is unsafe to use unless a backup has already been made outside of Azure Disk Encryption.</span></span>   <span data-ttu-id="e6804-319">Pokud je zadán parametr - skipVmBackup, rutina nebude vytvořit záložní kopii spravovaných disků před šifrování.</span><span class="sxs-lookup"><span data-stu-id="e6804-319">When the -skipVmBackup parameter is specified, the cmdlet will not make a backup of the managed disk prior to encryption.</span></span>  <span data-ttu-id="e6804-320">Z tohoto důvodu bude považován za povinný předpokladem pro zajistěte, aby zálohování spravovaného disku virtuálního počítače je na místě před povolením Azure Disk Encryption, v případě, že je později potřeba obnovení.</span><span class="sxs-lookup"><span data-stu-id="e6804-320">For this reason, it is considered a mandatory prerequisite to make sure a backup of the managed disk VM is in place prior to enabling Azure Disk Encryption in case recovery is later needed.</span></span>  
> [!NOTE]
 > <span data-ttu-id="e6804-321">Parametr - skipVmBackup by nikdy nepoužívali, pokud snímek nebo zálohování již byl změněn mimo Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="e6804-321">The -skipVmBackup parameter should never be used unless a snapshot or backup has already been made outside of Azure Disk Encryption.</span></span> 

* <span data-ttu-id="e6804-322">Řešení Azure Disk Encryption používá ochrana externí klíče nástroje BitLocker pro virtuální počítače IaaS Windows.</span><span class="sxs-lookup"><span data-stu-id="e6804-322">The Azure Disk Encryption solution uses the BitLocker external key protector for Windows IaaS VMs.</span></span> <span data-ttu-id="e6804-323">Pro doménu připojené k virtuální počítače, nesmí push žádné zásady skupiny, které vynutit ochrany pomocí čipu TPM.</span><span class="sxs-lookup"><span data-stu-id="e6804-323">For domain joined VMs, DO NOT push any group policies that enforce TPM protectors.</span></span> <span data-ttu-id="e6804-324">Informace o zásadách skupiny pro "Povolit nástroj BitLocker bez kompatibilním čipem TPM" najdete v tématu [referenční příručka zásad skupiny Bitlockeru](https://technet.microsoft.com/library/ee706521).</span><span class="sxs-lookup"><span data-stu-id="e6804-324">For information about the group policy for “Allow BitLocker without a compatible TPM,” see [BitLocker Group Policy Reference](https://technet.microsoft.com/library/ee706521).</span></span>
* <span data-ttu-id="e6804-325">Zásady nástroje BitLocker na virtuální počítače připojené k doméně pomocí zásad skupiny vlastní musí zahrnovat následující nastavení: `Configure user storage of bitlocker recovery information -> Allow 256-bit recovery key` Azure Disk Encryption se nezdaří, pokud nejsou kompatibilní, nastavení zásad vlastní skupiny pro Bitlocker.</span><span class="sxs-lookup"><span data-stu-id="e6804-325">Bitlocker policy on domain joined virtual machines with custom group policy must include the following setting: `Configure user storage of bitlocker recovery information -> Allow 256-bit recovery key`  Azure Disk Encryption will fail when custom group policy settings for Bitlocker are incompatible.</span></span> <span data-ttu-id="e6804-326">Na počítačích, které nemá správné zásady může být nastavení, použít nové zásady, vynucení nové zásady aktualizace (gpupdate.exe/Force) a pak restartování požadováno.</span><span class="sxs-lookup"><span data-stu-id="e6804-326">On machines that did not have the correct policy setting, applying the new policy, forcing the new policy to update (gpupdate.exe /force), and then restarting may be required.</span></span>  
* <span data-ttu-id="e6804-327">Pokud chcete vytvořit aplikaci Azure AD, vytvoření trezoru klíčů, nebo nastavit existující trezor klíčů a povolit šifrování, najdete v článku [požadovaných skript prostředí PowerShell Azure Disk Encryption](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).</span><span class="sxs-lookup"><span data-stu-id="e6804-327">To create an Azure AD application, create a key vault, or set up an existing key vault and enable encryption, see the [Azure Disk Encryption prerequisite PowerShell script](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).</span></span>
* <span data-ttu-id="e6804-328">Konfigurace šifrování disku požadavků pomocí Azure CLI, najdete v části [tento skript Bash](https://github.com/ejarvi/ade-cli-getting-started).</span><span class="sxs-lookup"><span data-stu-id="e6804-328">To configure disk-encryption prerequisites using the Azure CLI, see [this Bash script](https://github.com/ejarvi/ade-cli-getting-started).</span></span>
* <span data-ttu-id="e6804-329">Pomocí služby Azure Backup k zálohování a obnovení virtuálních počítačů služby šifrovaná, pokud je povolené šifrování s Azure Disk Encryption, zašifrujte virtuální počítače pomocí klíče konfigurace Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="e6804-329">To use the Azure Backup service to back up and restore encrypted VMs, when encryption is enabled with Azure Disk Encryption, encrypt your VMs by using the Azure Disk Encryption key configuration.</span></span> <span data-ttu-id="e6804-330">Služba zálohování podporuje virtuální počítače, které jsou šifrované pomocí pouze KEK konfigurace.</span><span class="sxs-lookup"><span data-stu-id="e6804-330">The Backup service supports VMs that are encrypted using KEK configuration only.</span></span> <span data-ttu-id="e6804-331">V tématu [šifrované zálohování a obnovení virtuálních počítačů s šifrováním Azure Backup](https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-encryption).</span><span class="sxs-lookup"><span data-stu-id="e6804-331">See [How to back up and restore encrypted virtual machines with Azure Backup  encryption](https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-encryption).</span></span>

* <span data-ttu-id="e6804-332">Při šifrování svazku operačního systému Linux, je momentálně nevyžaduje na konci procesu Všimněte si, že virtuální počítač restartovat.</span><span class="sxs-lookup"><span data-stu-id="e6804-332">When encrypting a Linux OS volume, note that a VM restart is currently required at the end of the process.</span></span> <span data-ttu-id="e6804-333">To lze provést přes portál, prostředí powershell nebo rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e6804-333">This can be done via the portal, powershell, or CLI.</span></span>   <span data-ttu-id="e6804-334">Pokud chcete sledovat průběh šifrování, pravidelně dotazovala vrácený Get-AzureRmVMDiskEncryptionStatus https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus stavové zprávy.</span><span class="sxs-lookup"><span data-stu-id="e6804-334">To track the progress of encryption, periodically poll the status message returned by Get-AzureRmVMDiskEncryptionStatus https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus.</span></span>  <span data-ttu-id="e6804-335">Po dokončení šifrování stavové zprávy vrácené tento příkaz označí to.</span><span class="sxs-lookup"><span data-stu-id="e6804-335">Once encryption is complete, the the status message returned by this command will indicate this.</span></span>  <span data-ttu-id="e6804-336">Například "ProgressMessage: disk operačního systému úspěšně šifrování, restartujte virtuální počítač" v tomto okamžiku může být virtuální počítač restartovat a použita.</span><span class="sxs-lookup"><span data-stu-id="e6804-336">For example, "ProgressMessage: OS disk successfully encrypted, please reboot the VM"  At this point the VM can be restarted and used.</span></span>  

* <span data-ttu-id="e6804-337">Azure Disk Encryption pro Linux vyžaduje datové disky tak, aby měl systém připojeného souboru v systému Linux před šifrování</span><span class="sxs-lookup"><span data-stu-id="e6804-337">Azure Disk Encryption for Linux requires data disks to have a mounted file system in Linux prior to encryption</span></span>

* <span data-ttu-id="e6804-338">Rekurzivní připojených data, která nepodporuje disky Azure Disk Encryption pro Linux.</span><span class="sxs-lookup"><span data-stu-id="e6804-338">Recursively mounted data disks are not supported by the Azure Disk Encryption for Linux.</span></span> <span data-ttu-id="e6804-339">Například pokud cílovém systému má v řádku/foo/připojit disk a potom jiné na /foo/bar/baz, šifrování /foo/bar/baz bude úspěšné, ale šifrování s/foo nebo se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="e6804-339">For example, if the target system has mounted a disk on /foo/bar and then another on /foo/bar/baz, the encryption of /foo/bar/baz will succeed, but encryption of /foo/bar will fail.</span></span> 

* <span data-ttu-id="e6804-340">Azure Disk Encryption je podporován pouze v galerii Azure, které jsou podporované Image, které splňují zmíněnými požadavky.</span><span class="sxs-lookup"><span data-stu-id="e6804-340">Azure Disk Encryption is only supported on Azure gallery supported images that meet the aforementioned prerequisites.</span></span> <span data-ttu-id="e6804-341">Zákazník vlastními obrázky nejsou podporovány z důvodu vlastní oddíl schémata a proces chování, které mohou existovat na těchto bitových kopií.</span><span class="sxs-lookup"><span data-stu-id="e6804-341">Customer custom images are not supported due to custom partition schemes and process behaviors that may exist on these images.</span></span> <span data-ttu-id="e6804-342">Navíc založená i galerie image Virtuálního počítače, který původně splněny požadavky ale byly upraveny po vytvoření může být nekompatibilní.</span><span class="sxs-lookup"><span data-stu-id="e6804-342">Further, even gallery image based VM's that initially met prerequisites but have been modified after creation may be incompatible.</span></span>  <span data-ttu-id="e6804-343">Pro tento návrhy postupu pro šifrování virtuálního počítače s Linuxem z toho důvodu je spustit z Galerie vyčištění bitové kopie, šifrování virtuálního počítače a poté přidejte vlastní softwaru nebo data na virtuální počítač podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="e6804-343">For that reason, the suggested procedure for encrypting a Linux VM is to start from a clean gallery image, encrypt the VM, and then add custom software or data to the VM as needed.</span></span>  

> [!NOTE]
> <span data-ttu-id="e6804-344">Zálohování a obnovení šifrovaných virtuálních počítačů je podporována pouze pro virtuální počítače, které jsou zašifrované s konfigurací KEK.</span><span class="sxs-lookup"><span data-stu-id="e6804-344">Backup and restore of encrypted VMs is supported only for VMs that are encrypted with the KEK configuration.</span></span> <span data-ttu-id="e6804-345">Není podporována u virtuálních počítačů, které jsou zašifrované bez KEK.</span><span class="sxs-lookup"><span data-stu-id="e6804-345">It is not supported on VMs that are encrypted without KEK.</span></span> <span data-ttu-id="e6804-346">KEK je volitelný parametr, který umožňuje virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e6804-346">KEK is an optional parameter that enables VM.</span></span>

#### <a name="set-up-the-azure-ad-application-in-azure-active-directory"></a><span data-ttu-id="e6804-347">Nastavit aplikaci Azure AD ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e6804-347">Set up the Azure AD application in Azure Active Directory</span></span>
<span data-ttu-id="e6804-348">Pokud je třeba šifrování povolení na spuštění virtuálního počítače v Azure, Azure Disk Encryption generuje a zapíše šifrovací klíče do trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-348">When you need encryption to be enabled on a running VM in Azure, Azure Disk Encryption generates and writes the encryption keys to your key vault.</span></span> <span data-ttu-id="e6804-349">Správa šifrovacích klíčů v trezoru klíčů se vyžaduje ověřování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e6804-349">Managing encryption keys in your key vault requires Azure AD authentication.</span></span>

<span data-ttu-id="e6804-350">Pro tento účel vytvořte aplikaci Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e6804-350">For this purpose, create an Azure AD application.</span></span> <span data-ttu-id="e6804-351">Najdete podrobné pokyny pro registraci aplikace v části "Získat Identity pro aplikace" v příspěvku blogu [Azure Key Vault - krok za krokem](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx).</span><span class="sxs-lookup"><span data-stu-id="e6804-351">You can find detailed steps for registering an application in the “Get an Identity for the Application” section of the blog post [Azure Key Vault - Step by Step](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx).</span></span> <span data-ttu-id="e6804-352">Tento příspěvek také obsahuje řadu užitečné příklady pro nastavení a konfiguraci trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-352">This post also contains a number of helpful examples for setting up and configuring your key vault.</span></span> <span data-ttu-id="e6804-353">Pro účely ověření můžete použít buď ověřování na základě tajný klíč klienta nebo ověřování klientů na základě certifikátů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e6804-353">For authentication purposes, you can use either client secret-based authentication or client certificate-based Azure AD authentication.</span></span>

#### <a name="client-secret-based-authentication-for-azure-ad"></a><span data-ttu-id="e6804-354">Ověřování na základě tajný klíč klienta pro Azure AD</span><span class="sxs-lookup"><span data-stu-id="e6804-354">Client secret-based authentication for Azure AD</span></span>
<span data-ttu-id="e6804-355">Následující části můžete nakonfigurovat ověřování na základě tajný klíč klienta pro Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e6804-355">The sections that follow can help you configure a client secret-based authentication for Azure AD.</span></span>

##### <a name="create-an-azure-ad-application-by-using-azure-powershell"></a><span data-ttu-id="e6804-356">Vytvořit aplikaci Azure AD pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="e6804-356">Create an Azure AD application by using Azure PowerShell</span></span>
<span data-ttu-id="e6804-357">Pomocí následující rutiny prostředí PowerShell vytvořit aplikaci Azure AD:</span><span class="sxs-lookup"><span data-stu-id="e6804-357">Use the following PowerShell cmdlet to create an Azure AD application:</span></span>

    $aadClientSecret = "yourSecret"
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -Password $aadClientSecret
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

> [!NOTE]
> <span data-ttu-id="e6804-358">$azureAdApplication.ApplicationId Azure AD ClientID a $aadClientSecret je sdílený tajný klíč klienta, který by měl později použít k povolení Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="e6804-358">$azureAdApplication.ApplicationId is the Azure AD ClientID and $aadClientSecret is the client secret that you should use later to enable Azure Disk Encryption.</span></span> <span data-ttu-id="e6804-359">Správně zabezpečit tajný klíč klienta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e6804-359">Safeguard the Azure AD client secret appropriately.</span></span>

##### <a name="setting-up-the-azure-ad-client-id-and-secret-from-the-azure-classic-portal"></a><span data-ttu-id="e6804-360">Nastavení ID klienta Azure AD a tajný klíč z portálu Azure classic</span><span class="sxs-lookup"><span data-stu-id="e6804-360">Setting up the Azure AD client ID and secret from the Azure classic portal</span></span>
<span data-ttu-id="e6804-361">Můžete vytvořit také ID klienta Azure AD a tajný klíč pomocí [portál Azure classic]( https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="e6804-361">You can also set up your Azure AD client ID and secret by using the [Azure classic portal]( https://manage.windowsazure.com).</span></span> <span data-ttu-id="e6804-362">K provedení této úlohy, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e6804-362">To perform this task, do the following:</span></span>

1. <span data-ttu-id="e6804-363">Klikněte **služby Active Directory** kartě.</span><span class="sxs-lookup"><span data-stu-id="e6804-363">Click the **Active Directory** tab.</span></span>

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig3.png)

2. <span data-ttu-id="e6804-365">Klikněte na tlačítko **přidat aplikaci**a pak zadejte název aplikace.</span><span class="sxs-lookup"><span data-stu-id="e6804-365">Click **Add Application**, and then type the application name.</span></span>

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig4.png)

3. <span data-ttu-id="e6804-367">Klikněte na tlačítko se šipkou a pak nakonfigurujte vlastnosti aplikace.</span><span class="sxs-lookup"><span data-stu-id="e6804-367">Click the arrow button, and then configure the application properties.</span></span>

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig5.png)

4. <span data-ttu-id="e6804-369">Kliknutím na značku zaškrtnutí v levém dolním rohu na dokončení.</span><span class="sxs-lookup"><span data-stu-id="e6804-369">Click the check mark in the lower left corner to finish.</span></span> <span data-ttu-id="e6804-370">Zobrazí se stránka konfigurace aplikace a ID klienta Azure AD se zobrazí v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="e6804-370">The application configuration page appears, and the Azure AD client ID is displayed at the bottom of the page.</span></span>

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig6.png)

5. <span data-ttu-id="e6804-372">Sdílený tajný klíč klienta Azure AD uložte kliknutím **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="e6804-372">Save the Azure AD client secret by clicking the **Save** button.</span></span> <span data-ttu-id="e6804-373">Všimněte si, tajný klíč klienta Azure AD, do textového pole klíče.</span><span class="sxs-lookup"><span data-stu-id="e6804-373">Note the Azure AD client secret in the keys text box.</span></span> <span data-ttu-id="e6804-374">Zabezpečit správně.</span><span class="sxs-lookup"><span data-stu-id="e6804-374">Safeguard it appropriately.</span></span>

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig7.png)

 > [!NOTE]
 > <span data-ttu-id="e6804-376">Předchozí postup není podporována na portálu Azure classic.</span><span class="sxs-lookup"><span data-stu-id="e6804-376">The preceding flow is not supported on the Azure classic portal.</span></span>

##### <a name="use-an-existing-application"></a><span data-ttu-id="e6804-377">Použít stávající aplikaci</span><span class="sxs-lookup"><span data-stu-id="e6804-377">Use an existing application</span></span>
<span data-ttu-id="e6804-378">Pokud chcete spustit následující příkazy, získat a použít [modulu Azure AD PowerShell](https://technet.microsoft.com/library/jj151815.aspx).</span><span class="sxs-lookup"><span data-stu-id="e6804-378">To execute the following commands, obtain and use the [Azure AD PowerShell module](https://technet.microsoft.com/library/jj151815.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="e6804-379">Je třeba spustit následující příkazy z nové okno prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e6804-379">The following commands must be executed from a new PowerShell window.</span></span> <span data-ttu-id="e6804-380">Nepoužívejte provést příkazy prostředí Azure PowerShell nebo v okně Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="e6804-380">Do not use Azure PowerShell or the Azure Resource Manager window to execute the commands.</span></span> <span data-ttu-id="e6804-381">Doporučujeme vám tento přístup, protože jsou tyto rutiny v modulu MSOnline nebo Azure AD PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e6804-381">We recommend this approach because these cmdlets are in the MSOnline module or Azure AD PowerShell.</span></span>

    $clientSecret = ‘<yourAadClientSecret>’
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type password -Value $clientSecret

#### <a name="certificate-based-authentication-for-azure-ad"></a><span data-ttu-id="e6804-382">Ověřování pomocí certifikátů pro Azure AD</span><span class="sxs-lookup"><span data-stu-id="e6804-382">Certificate-based authentication for Azure AD</span></span>
> [!NOTE]
> <span data-ttu-id="e6804-383">Na virtuální počítače s Linuxem se aktuálně nepodporuje ověřování pomocí certifikátů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e6804-383">Azure AD certificate-based authentication is currently not supported on Linux VMs.</span></span>

<span data-ttu-id="e6804-384">V následujících ukazují, jak nakonfigurovat ověřování pomocí certifikátů pro Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e6804-384">The sections that follow show how to configure a certificate-based authentication for Azure AD.</span></span>

##### <a name="create-an-azure-ad-application"></a><span data-ttu-id="e6804-385">Vytvořit aplikaci Azure AD</span><span class="sxs-lookup"><span data-stu-id="e6804-385">Create an Azure AD application</span></span>
<span data-ttu-id="e6804-386">Pokud chcete vytvořit aplikaci Azure AD, spusťte následující rutiny prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="e6804-386">To create an Azure AD application, execute the following PowerShell cmdlets:</span></span>

> [!NOTE]
> <span data-ttu-id="e6804-387">Nahraďte následující `yourpassword` řetězec pomocí zabezpečeného hesla a zabezpečit heslo.</span><span class="sxs-lookup"><span data-stu-id="e6804-387">Replace the following `yourpassword` string with your secure password, and safeguard the password.</span></span>

    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate("C:\certificates\examplecert.pfx", "yourpassword")
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -KeyValue $keyValue -KeyType AsymmetricX509Cert
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

<span data-ttu-id="e6804-388">Po dokončení tohoto kroku můžete nahrát soubor PFX do trezoru klíčů a povolte zásady přístupu, které jsou potřeba k nasazení tohoto certifikátu pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e6804-388">After you finish this step, upload a PFX file to your key vault and enable the access policy needed to deploy that certificate to a VM.</span></span>

##### <a name="use-an-existing-azure-ad-application"></a><span data-ttu-id="e6804-389">Použít existující aplikaci Azure AD</span><span class="sxs-lookup"><span data-stu-id="e6804-389">Use an existing Azure AD application</span></span>
<span data-ttu-id="e6804-390">Pokud nakonfigurujete ověřování pomocí certifikátů pro existující aplikace, použijte rutiny prostředí PowerShell, které jsou tady uvedené.</span><span class="sxs-lookup"><span data-stu-id="e6804-390">If you are configuring certificate-based authentication for an existing application, use the PowerShell cmdlets shown here.</span></span> <span data-ttu-id="e6804-391">Ujistěte se, že je provést z nové okno prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e6804-391">Be sure to execute them from a new PowerShell window.</span></span>

    $certLocalPath = 'C:\certs\myaadapp.cer'
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate
    $cer.Import($certLocalPath)
    $binCert = $cer.GetRawCertData()
    $credValue = [System.Convert]::ToBase64String($binCert);
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type asymmetric -Value $credValue -Usage verify

<span data-ttu-id="e6804-392">Po dokončení tohoto kroku můžete nahrát soubor PFX do trezoru klíčů a povolte zásady přístupu, které je potřeba k nasazení certifikátu k virtuálnímu počítači.</span><span class="sxs-lookup"><span data-stu-id="e6804-392">After you finish this step, upload a PFX file to your key vault and enable the access policy that's needed to deploy the certificate to a VM.</span></span>

##### <a name="upload-a-pfx-file-to-your-key-vault"></a><span data-ttu-id="e6804-393">Nahrání souboru PFX do trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="e6804-393">Upload a PFX file to your key vault</span></span>
<span data-ttu-id="e6804-394">Podrobné vysvětlení tohoto procesu najdete v tématu [oficiální Azure Key Vault Blog týmu](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx).</span><span class="sxs-lookup"><span data-stu-id="e6804-394">For a detailed explanation of this process, see [The Official Azure Key Vault Team Blog](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx).</span></span> <span data-ttu-id="e6804-395">Následující rutiny prostředí PowerShell jsou však všechny, které jsou potřeba pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="e6804-395">However, the following PowerShell cmdlets are all you need for the task.</span></span> <span data-ttu-id="e6804-396">Ujistěte se, že je provést z konzoly Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e6804-396">Be sure to execute them from Azure PowerShell console.</span></span>

> [!NOTE]
> <span data-ttu-id="e6804-397">Nahraďte následující `yourpassword` řetězec pomocí zabezpečeného hesla a zabezpečit heslo.</span><span class="sxs-lookup"><span data-stu-id="e6804-397">Replace the following `yourpassword` string with your secure password, and safeguard the password.</span></span>

    $certLocalPath = 'C:\certs\myaadapp.pfx'
    $certPassword = "yourpassword"
    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’

    $fileContentBytes = get-content $certLocalPath -Encoding Byte
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)

    $jsonObject = @"
    {
    "data": "$filecontentencoded",
    "dataType" :"pfx",
    "password": "$certPassword"
    }
    "@

    $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
    $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)

    Switch-AzureMode -Name AzureResourceManager
    $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName -SecretValue $secret
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ResourceGroupName $resourceGroupName –EnabledForDeployment

##### <a name="deploy-a-certificate-in-your-key-vault-to-an-existing-vm"></a><span data-ttu-id="e6804-398">Nasaďte certifikát v trezoru klíčů na existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="e6804-398">Deploy a certificate in your key vault to an existing VM</span></span>
<span data-ttu-id="e6804-399">Po dokončení nahrávání soubor PFX, nasaďte certifikát v trezoru klíčů na existující virtuální počítač s následujícími službami:</span><span class="sxs-lookup"><span data-stu-id="e6804-399">After you finish uploading the PFX, deploy a certificate in the key vault to an existing VM with the following:</span></span>
 ```
    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’
    $vmName = ‘yourVMName’
    $certUrl = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName).Id
    $sourceVaultId = (Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $resourceGroupName).ResourceId
    $vm = Get-AzureRmVM -ResourceGroupName $resourceGroupName -Name $vmName
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore "My" -CertificateUrl $certUrl
    Update-AzureRmVM -VM $vm  -ResourceGroupName $resourceGroupName
 ```

#### <a name="set-up-the-key-vault-access-policy-for-the-azure-ad-application"></a><span data-ttu-id="e6804-400">Nastavte zásady přístupu trezoru klíčů pro aplikace Azure AD</span><span class="sxs-lookup"><span data-stu-id="e6804-400">Set up the key vault access policy for the Azure AD application</span></span>
<span data-ttu-id="e6804-401">Aplikace Azure AD musí práva pro přístup k klíčů nebo tajných klíčů v trezoru.</span><span class="sxs-lookup"><span data-stu-id="e6804-401">Your Azure AD application needs rights to access the keys or secrets in the vault.</span></span> <span data-ttu-id="e6804-402">Použití [ `Set-AzureKeyVaultAccessPolicy` ](/powershell/module/azure/set-azurekeyvaultaccesspolicy?view=azuresmps-3.7.0) rutiny udělit oprávnění k aplikaci, pomocí ID klienta (který byl vygenerován při aplikaci byl zaregistrován) jako _– ServicePrincipalName_ hodnota parametru.</span><span class="sxs-lookup"><span data-stu-id="e6804-402">Use the [`Set-AzureKeyVaultAccessPolicy`](/powershell/module/azure/set-azurekeyvaultaccesspolicy?view=azuresmps-3.7.0) cmdlet to grant permissions to the application, using the client ID (which was generated when the application was registered) as the _–ServicePrincipalName_ parameter value.</span></span> <span data-ttu-id="e6804-403">Další informace naleznete v příspěvku blogu [Azure Key Vault - krok za krokem](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx).</span><span class="sxs-lookup"><span data-stu-id="e6804-403">To learn more, see the blog post [Azure Key Vault - Step by Step](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx).</span></span> <span data-ttu-id="e6804-404">Tady je příklad toho, jak to provést pomocí prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="e6804-404">Here is an example of how to perform this task via PowerShell:</span></span>

    $keyVaultName = '<yourKeyVaultName>'
    $aadClientID = '<yourAadAppClientID>'
    $rgname = '<yourResourceGroup>'
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname

> [!NOTE]
> <span data-ttu-id="e6804-405">Azure Disk Encryption vyžaduje, abyste nakonfigurovali následující zásady přístupu k vaší aplikaci klienta Azure AD: _WrapKey_ a _nastavit_ oprávnění.</span><span class="sxs-lookup"><span data-stu-id="e6804-405">Azure Disk Encryption requires you to configure the following access policies to your Azure AD client application: _WrapKey_ and _Set_ permissions.</span></span>

## <a name="terminology"></a><span data-ttu-id="e6804-406">Terminologie</span><span class="sxs-lookup"><span data-stu-id="e6804-406">Terminology</span></span>
<span data-ttu-id="e6804-407">Abyste pochopili, některé z běžných podmínek používá tuto technologii, použijte následující tabulku terminologie:</span><span class="sxs-lookup"><span data-stu-id="e6804-407">To understand some of the common terms used by this technology, use the following terminology table:</span></span>

| <span data-ttu-id="e6804-408">Terminologie</span><span class="sxs-lookup"><span data-stu-id="e6804-408">Terminology</span></span> | <span data-ttu-id="e6804-409">Definice</span><span class="sxs-lookup"><span data-stu-id="e6804-409">Definition</span></span> |
| --- | --- |
| <span data-ttu-id="e6804-410">Azure AD</span><span class="sxs-lookup"><span data-stu-id="e6804-410">Azure AD</span></span> | <span data-ttu-id="e6804-411">Azure AD je [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="e6804-411">Azure AD is [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).</span></span> <span data-ttu-id="e6804-412">Účet Azure AD je předpokladem pro ověřování, ukládání a načítání tajné klíče z trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-412">An Azure AD account is a prerequisite for authenticating, storing, and retrieving secrets from a key vault.</span></span> |
| <span data-ttu-id="e6804-413">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e6804-413">Azure Key Vault</span></span> | <span data-ttu-id="e6804-414">Key Vault je služba kryptografické klíče správy, která je založena na moduly ověřit Federal Information Processing Standards FIPS hardwarového zabezpečení, které v zájmu ochrany kryptografické klíče a tajné klíče citlivé.</span><span class="sxs-lookup"><span data-stu-id="e6804-414">Key Vault is a cryptographic, key management service that's based on Federal Information Processing Standards (FIPS)-validated hardware security modules, which help safeguard your cryptographic keys and sensitive secrets.</span></span> <span data-ttu-id="e6804-415">Další informace najdete v tématu [Key Vault](https://azure.microsoft.com/services/key-vault/) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="e6804-415">For more information, see [Key Vault](https://azure.microsoft.com/services/key-vault/) documentation.</span></span> |
| <span data-ttu-id="e6804-416">ARM</span><span class="sxs-lookup"><span data-stu-id="e6804-416">ARM</span></span> | <span data-ttu-id="e6804-417">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="e6804-417">Azure Resource Manager</span></span> |
| <span data-ttu-id="e6804-418">Nástroj BitLocker</span><span class="sxs-lookup"><span data-stu-id="e6804-418">BitLocker</span></span> |<span data-ttu-id="e6804-419">[Nástroj BitLocker](https://technet.microsoft.com/library/hh831713.aspx) je rozpoznána odvětví Windows svazku šifrovací technologie, která slouží k povolení šifrování disku na virtuální počítače IaaS Windows.</span><span class="sxs-lookup"><span data-stu-id="e6804-419">[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) is an industry-recognized Windows volume encryption technology that's used to enable disk encryption on Windows IaaS VMs.</span></span> |
| <span data-ttu-id="e6804-420">BEK</span><span class="sxs-lookup"><span data-stu-id="e6804-420">BEK</span></span> | <span data-ttu-id="e6804-421">Nástroj BitLocker šifrovací klíče se používají k šifrování spouštěcí svazek operačního systému a datové svazky.</span><span class="sxs-lookup"><span data-stu-id="e6804-421">BitLocker encryption keys are used to encrypt the OS boot volume and data volumes.</span></span> <span data-ttu-id="e6804-422">Klíče Bitlockeru chráněna jako tajných klíčů v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-422">The BitLocker keys are safeguarded in a key vault as secrets.</span></span> |
| <span data-ttu-id="e6804-423">Rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="e6804-423">CLI</span></span> | <span data-ttu-id="e6804-424">V tématu [rozhraní příkazového řádku Azure](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="e6804-424">See [Azure command-line interface](../cli-install-nodejs.md).</span></span> |
| <span data-ttu-id="e6804-425">DM-Crypt</span><span class="sxs-lookup"><span data-stu-id="e6804-425">DM-Crypt</span></span> |<span data-ttu-id="e6804-426">[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) je podsystém systémem Linux, transparentní-šifrování disku, který slouží k povolení šifrování disku na virtuální počítače IaaS Linux.</span><span class="sxs-lookup"><span data-stu-id="e6804-426">[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) is the Linux-based, transparent disk-encryption subsystem that's used to enable disk encryption on Linux IaaS VMs.</span></span> |
| <span data-ttu-id="e6804-427">KEK</span><span class="sxs-lookup"><span data-stu-id="e6804-427">KEK</span></span> | <span data-ttu-id="e6804-428">Klíče šifrovací klíč je asymetrický klíč (RSA 2048), který můžete použít k ochraně nebo zabalení tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="e6804-428">Key encryption key is the asymmetric key (RSA 2048) that you can use to protect or wrap the secret.</span></span> <span data-ttu-id="e6804-429">Můžete zadat hardwaru zabezpečení (HSM) moduly-chráněný klíč, nebo softwarově chráněný klíč.</span><span class="sxs-lookup"><span data-stu-id="e6804-429">You can provide a hardware security modules (HSM)-protected key or software-protected key.</span></span> <span data-ttu-id="e6804-430">Další podrobnosti najdete v tématu [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="e6804-430">For more details, see [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) documentation.</span></span> |
| <span data-ttu-id="e6804-431">PS rutiny</span><span class="sxs-lookup"><span data-stu-id="e6804-431">PS cmdlets</span></span> | <span data-ttu-id="e6804-432">V tématu [rutin prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e6804-432">See [Azure PowerShell cmdlets](/powershell/azure/overview).</span></span> |

### <a name="set-up-and-configure-your-key-vault-for-azure-disk-encryption"></a><span data-ttu-id="e6804-433">Nastavení a konfigurace trezoru klíčů pro Azure Disk Encryption</span><span class="sxs-lookup"><span data-stu-id="e6804-433">Set up and configure your key vault for Azure Disk Encryption</span></span>
<span data-ttu-id="e6804-434">Azure Disk Encryption pomáhá chránit šifrování disku klíče a tajné klíče v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-434">Azure Disk Encryption helps safeguard the disk-encryption keys and secrets in your key vault.</span></span> <span data-ttu-id="e6804-435">Pro nastavení trezoru klíčů pro Azure Disk Encryption, proveďte kroky v každé z těchto částí.</span><span class="sxs-lookup"><span data-stu-id="e6804-435">To set up your key vault for Azure Disk Encryption, complete the steps in each of the following sections.</span></span>

#### <a name="create-a-key-vault"></a><span data-ttu-id="e6804-436">Vytvořte trezor klíčů</span><span class="sxs-lookup"><span data-stu-id="e6804-436">Create a key vault</span></span>
<span data-ttu-id="e6804-437">Vytvoření trezoru klíčů, použijte jednu z následujících možností:</span><span class="sxs-lookup"><span data-stu-id="e6804-437">To create a key vault, use one of the following options:</span></span>

* [<span data-ttu-id="e6804-438">"101-Key-trezoru-vytvořit" šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="e6804-438">"101-Key-Vault-Create" Resource Manager template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create)
* [<span data-ttu-id="e6804-439">Rutiny Azure PowerShell trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="e6804-439">Azure PowerShell key vault cmdlets</span></span>](/powershell/module/azurerm.keyvault/#key_vault)
* <span data-ttu-id="e6804-440">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="e6804-440">Azure Resource Manager</span></span>
* <span data-ttu-id="e6804-441">Postup [zabezpečit váš trezor klíčů](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-secure-your-key-vault)</span><span class="sxs-lookup"><span data-stu-id="e6804-441">How to [Secure your key vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-secure-your-key-vault)</span></span>

> [!NOTE]
> <span data-ttu-id="e6804-442">Pokud jste již nastavili trezoru klíčů pro vaše předplatné, přejděte k další části.</span><span class="sxs-lookup"><span data-stu-id="e6804-442">If you have already set up a key vault for your subscription, skip to the next section.</span></span>

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig1.png)

#### <a name="set-up-a-key-encryption-key-optional"></a><span data-ttu-id="e6804-444">Nastavení klíče šifrovací klíč (volitelné)</span><span class="sxs-lookup"><span data-stu-id="e6804-444">Set up a key encryption key (optional)</span></span>
<span data-ttu-id="e6804-445">Pokud chcete použít pro další úroveň zabezpečení pro šifrovací klíče nástroje BitLocker KEK, přidejte k trezoru klíčů KEK.</span><span class="sxs-lookup"><span data-stu-id="e6804-445">If you want to use a KEK for an additional layer of security for the BitLocker encryption keys, add a KEK to your key vault.</span></span> <span data-ttu-id="e6804-446">Použití [ `Add-AzureKeyVaultKey` ](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) vytvořte klíč šifrovacího klíče v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-446">Use the [`Add-AzureKeyVaultKey`](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) cmdlet to create a key encryption key in the key vault.</span></span> <span data-ttu-id="e6804-447">Můžete také importovat KEK z vaší místní správy k klíče HSM.</span><span class="sxs-lookup"><span data-stu-id="e6804-447">You can also import a KEK from your on-premises key management HSM.</span></span> <span data-ttu-id="e6804-448">Další podrobnosti najdete v tématu [klíč trezoru dokumentaci](https://azure.microsoft.com/documentation/services/key-vault/).</span><span class="sxs-lookup"><span data-stu-id="e6804-448">For more details, see [Key Vault Documentation](https://azure.microsoft.com/documentation/services/key-vault/).</span></span>

    Add-AzureKeyVaultKey [-VaultName] <string> [-Name] <string> -Destination <string> {HSM | Software}

<span data-ttu-id="e6804-449">Klíče KEK můžete přidat tak, že přejdete do Azure Resource Manageru nebo přes rozhraní váš trezor klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-449">You can add the KEK by going to Azure Resource Manager or by using your key vault interface.</span></span>

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig2.png)

#### <a name="set-key-vault-permissions"></a><span data-ttu-id="e6804-451">Nastavte oprávnění trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="e6804-451">Set key vault permissions</span></span>
<span data-ttu-id="e6804-452">Platformy Azure potřebuje přístup k šifrovacích klíčů nebo tajných klíčů v trezoru klíčů, aby je bylo možné virtuální počítač pro spouštění a dešifrování svazky.</span><span class="sxs-lookup"><span data-stu-id="e6804-452">The Azure platform needs access to the encryption keys or secrets in your key vault to make them available to the VM for booting and decrypting the volumes.</span></span> <span data-ttu-id="e6804-453">Chcete-li udělit oprávnění pro platformu Azure, nastavte **EnabledForDiskEncryption** vlastnost klíč trezoru pomocí rutiny prostředí PowerShell trezoru klíčů:</span><span class="sxs-lookup"><span data-stu-id="e6804-453">To grant permissions to the Azure platform, set the **EnabledForDiskEncryption** property in the key vault by using the key vault PowerShell cmdlet:</span></span>

    Set-AzureRmKeyVaultAccessPolicy -VaultName <yourVaultName> -ResourceGroupName <yourResourceGroup> -EnabledForDiskEncryption

<span data-ttu-id="e6804-454">Můžete také nastavit **EnabledForDiskEncryption** vlastnost navštivte stránky [Průzkumníka prostředků Azure](https://resources.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e6804-454">You can also set the **EnabledForDiskEncryption** property by visiting the [Azure Resource Explorer](https://resources.azure.com).</span></span>

<span data-ttu-id="e6804-455">Jak už bylo zmíněno dříve, je nutné nastavit **EnabledForDiskEncryption** vlastnost v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-455">As mentioned earlier, you must set the **EnabledForDiskEncryption** property on your key vault.</span></span> <span data-ttu-id="e6804-456">Jinak se nasazení nezdaří.</span><span class="sxs-lookup"><span data-stu-id="e6804-456">Otherwise, the deployment will fail.</span></span>

<span data-ttu-id="e6804-457">Můžete nastavit pro vaši aplikaci Azure AD z rozhraní trezoru klíčů, zásady přístupu, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="e6804-457">You can set up access policies for your Azure AD application from the key vault interface, as shown here:</span></span>

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3.png)

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3b.png)

<span data-ttu-id="e6804-460">Na **zásady rozšířené přístupu** kartě, ujistěte se, že váš trezor klíčů je povolena pro Azure Disk Encryption:</span><span class="sxs-lookup"><span data-stu-id="e6804-460">On the **Advanced access policies** tab, make sure that your key vault is enabled for Azure Disk Encryption:</span></span>

![Azure trezoru klíčů](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)

## <a name="disk-encryption-deployment-scenarios-and-user-experiences"></a><span data-ttu-id="e6804-462">Scénáře nasazení šifrování disku a koncových uživatelů</span><span class="sxs-lookup"><span data-stu-id="e6804-462">Disk-encryption deployment scenarios and user experiences</span></span>
<span data-ttu-id="e6804-463">Můžete povolit mnoho scénářů šifrování disku, a kroky mohou lišit v závislosti scénáři.</span><span class="sxs-lookup"><span data-stu-id="e6804-463">You can enable many disk-encryption scenarios, and the steps may vary according to the scenario.</span></span> <span data-ttu-id="e6804-464">Následující části se věnují scénáře podrobněji.</span><span class="sxs-lookup"><span data-stu-id="e6804-464">The following sections cover the scenarios in greater detail.</span></span>

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-the-marketplace"></a><span data-ttu-id="e6804-465">Povolit šifrování na nové virtuální počítače IaaS, vytvořený z Marketplace</span><span class="sxs-lookup"><span data-stu-id="e6804-465">Enable encryption on new IaaS VMs that are created from the Marketplace</span></span>
<span data-ttu-id="e6804-466">Můžete povolit šifrování disku na nový virtuální počítač IaaS Windows v Marketplace v Azure pomocí [šablony Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image).</span><span class="sxs-lookup"><span data-stu-id="e6804-466">You can enable disk encryption on new IaaS Windows VM from the Marketplace in Azure by using the  [Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image).</span></span>

1. <span data-ttu-id="e6804-467">V šabloně Azure rychlý start, klikněte na tlačítko **nasadit do Azure**, zadejte v konfiguraci šifrování na **parametry** okna a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e6804-467">On the Azure quick-start template, click **Deploy to Azure**, enter the encryption configuration on the **Parameters** blade, and then click **OK**.</span></span>

2. <span data-ttu-id="e6804-468">Vyberte předplatné, skupinu prostředků, umístění skupiny prostředků, právní podmínky a smlouvy a pak klikněte na tlačítko **vytvořit** chcete povolit šifrování na nový virtuální počítač IaaS.</span><span class="sxs-lookup"><span data-stu-id="e6804-468">Select the subscription, resource group, resource group location, legal terms, and agreement, and then click **Create** to enable encryption on a new IaaS VM.</span></span>

> [!NOTE]
> <span data-ttu-id="e6804-469">Tato šablona vytvoří nový virtuální počítač šifrované Windows, která využívá image systému Windows Server 2012 galerie.</span><span class="sxs-lookup"><span data-stu-id="e6804-469">This template creates a new encrypted Windows VM that uses the Windows Server 2012 gallery image.</span></span>

<span data-ttu-id="e6804-470">Můžete povolit šifrování disku na nový IaaS RedHat Linux 7.2 virtuální počítač s polem RAID-0 200 GB prostřednictvím tohoto [šablony Resource Manageru](https://aka.ms/fde-rhel).</span><span class="sxs-lookup"><span data-stu-id="e6804-470">You can enable disk encryption on a new IaaS RedHat Linux 7.2 VM with a 200-GB RAID-0 array by using this [Resource Manager template](https://aka.ms/fde-rhel).</span></span> <span data-ttu-id="e6804-471">Po nasazení šablony, ověřte stav šifrování virtuálních počítačů pomocí `Get-AzureRmVmDiskEncryptionStatus` rutiny, jak je popsáno v [OS šifrování jednotky na spuštěný virtuální počítač s Linuxem](#encrypting-os-drive-on-a-running-linux-vm).</span><span class="sxs-lookup"><span data-stu-id="e6804-471">After you deploy the template, verify the VM encryption status by using the `Get-AzureRmVmDiskEncryptionStatus` cmdlet, as described in [Encrypting OS drive on a running Linux VM](#encrypting-os-drive-on-a-running-linux-vm).</span></span> <span data-ttu-id="e6804-472">Když je počítač vrátí stav _VMRestartPending_, restartujte virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e6804-472">When the machine returns a status of _VMRestartPending_, restart the VM.</span></span>

<span data-ttu-id="e6804-473">Následující tabulka uvádí parametrů šablony Resource Manageru pro nové virtuální počítače z Marketplace scénáře pomocí ID klienta Azure AD:</span><span class="sxs-lookup"><span data-stu-id="e6804-473">The following table lists the Resource Manager template parameters for new VMs from the Marketplace scenario using Azure AD client ID:</span></span>

| <span data-ttu-id="e6804-474">Parametr</span><span class="sxs-lookup"><span data-stu-id="e6804-474">Parameter</span></span> | <span data-ttu-id="e6804-475">Popis</span><span class="sxs-lookup"><span data-stu-id="e6804-475">Description</span></span> |
| --- | --- |
| <span data-ttu-id="e6804-476">adminUserName</span><span class="sxs-lookup"><span data-stu-id="e6804-476">adminUserName</span></span> | <span data-ttu-id="e6804-477">Uživatelské jméno správce pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e6804-477">Admin user name for the virtual machine.</span></span> |
| <span data-ttu-id="e6804-478">adminPassword</span><span class="sxs-lookup"><span data-stu-id="e6804-478">adminPassword</span></span> | <span data-ttu-id="e6804-479">Uživatelské heslo správce pro virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e6804-479">Admin user password for the virtual machine.</span></span> |
| <span data-ttu-id="e6804-480">newStorageAccountName</span><span class="sxs-lookup"><span data-stu-id="e6804-480">newStorageAccountName</span></span> | <span data-ttu-id="e6804-481">Název účtu úložiště pro ukládání operačního systému a dat virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="e6804-481">Name of the storage account to store OS and data VHDs.</span></span> |
| <span data-ttu-id="e6804-482">vmSize</span><span class="sxs-lookup"><span data-stu-id="e6804-482">vmSize</span></span> | <span data-ttu-id="e6804-483">Velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e6804-483">Size of the VM.</span></span> <span data-ttu-id="e6804-484">V současné době jsou podporovány pouze standardní A, D a G řady.</span><span class="sxs-lookup"><span data-stu-id="e6804-484">Currently, only Standard A, D, and G series are supported.</span></span> |
| <span data-ttu-id="e6804-485">virtualNetworkName</span><span class="sxs-lookup"><span data-stu-id="e6804-485">virtualNetworkName</span></span> | <span data-ttu-id="e6804-486">Název sítě VNet, která na síťový adaptér virtuálního počítače by měly patřit do.</span><span class="sxs-lookup"><span data-stu-id="e6804-486">Name of the VNet that the VM NIC should belong to.</span></span> |
| <span data-ttu-id="e6804-487">subnetName</span><span class="sxs-lookup"><span data-stu-id="e6804-487">subnetName</span></span> | <span data-ttu-id="e6804-488">Název v síti VNet, která na síťový adaptér virtuálního počítače by měly patřit do podsítě.</span><span class="sxs-lookup"><span data-stu-id="e6804-488">Name of the subnet in the VNet that the VM NIC should belong to.</span></span> |
| <span data-ttu-id="e6804-489">AADClientID</span><span class="sxs-lookup"><span data-stu-id="e6804-489">AADClientID</span></span> | <span data-ttu-id="e6804-490">ID klienta aplikace Azure AD, který má oprávnění k zápisu tajných klíčů do trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-490">Client ID of the Azure AD application that has permissions to write secrets to your key vault.</span></span> |
| <span data-ttu-id="e6804-491">AADClientSecret</span><span class="sxs-lookup"><span data-stu-id="e6804-491">AADClientSecret</span></span> | <span data-ttu-id="e6804-492">Tajný klíč klienta aplikace Azure AD, který má oprávnění k zápisu tajných klíčů do trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-492">Client secret of the Azure AD application that has permissions to write secrets to your key vault.</span></span> |
| <span data-ttu-id="e6804-493">keyVaultURL</span><span class="sxs-lookup"><span data-stu-id="e6804-493">keyVaultURL</span></span> | <span data-ttu-id="e6804-494">Adresa URL trezoru klíčů, který klíč Bitlockeru, musí být nahrán do.</span><span class="sxs-lookup"><span data-stu-id="e6804-494">URL of the key vault that the BitLocker key should be uploaded to.</span></span> <span data-ttu-id="e6804-495">Můžete ho získat pomocí rutiny `(Get-AzureRmKeyVault -VaultName,-ResourceGroupName ).VaultURI`.</span><span class="sxs-lookup"><span data-stu-id="e6804-495">You can get it by using the cmdlet `(Get-AzureRmKeyVault -VaultName,-ResourceGroupName ).VaultURI`.</span></span> |
| <span data-ttu-id="e6804-496">keyEncryptionKeyURL</span><span class="sxs-lookup"><span data-stu-id="e6804-496">keyEncryptionKeyURL</span></span> | <span data-ttu-id="e6804-497">Adresa URL klíče šifrovací klíč, který se používá k šifrování generovaný klíč nástroje BitLocker (volitelné).</span><span class="sxs-lookup"><span data-stu-id="e6804-497">URL of the key encryption key that's used to encrypt the generated BitLocker key (optional).</span></span> |
| <span data-ttu-id="e6804-498">keyVaultResourceGroup</span><span class="sxs-lookup"><span data-stu-id="e6804-498">keyVaultResourceGroup</span></span> | <span data-ttu-id="e6804-499">Skupina prostředků služby key vault.</span><span class="sxs-lookup"><span data-stu-id="e6804-499">Resource group of the key vault.</span></span> |
| <span data-ttu-id="e6804-500">vmName</span><span class="sxs-lookup"><span data-stu-id="e6804-500">vmName</span></span> | <span data-ttu-id="e6804-501">Název virtuálního počítače, který operace šifrování je třeba provést na.</span><span class="sxs-lookup"><span data-stu-id="e6804-501">Name of the VM that the encryption operation is to be performed on.</span></span> |

> [!NOTE]
> <span data-ttu-id="e6804-502">_KeyEncryptionKeyURL_ je volitelný parametr.</span><span class="sxs-lookup"><span data-stu-id="e6804-502">_KeyEncryptionKeyURL_ is an optional parameter.</span></span> <span data-ttu-id="e6804-503">Můžete zahrnout vlastní KEK na další chránit datový šifrovací klíč (heslo tajný klíč) v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-503">You can bring your own KEK to further safeguard the data encryption key (Passphrase secret) in your key vault.</span></span>

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-customer-encrypted-vhd-and-encryption-keys"></a><span data-ttu-id="e6804-504">Povolit šifrování na nové virtuální počítače IaaS, vytvořený z šifrované zákazníka virtuálního pevného disku a šifrovacích klíčů</span><span class="sxs-lookup"><span data-stu-id="e6804-504">Enable encryption on new IaaS VMs that are created from customer-encrypted VHD and encryption keys</span></span>
<span data-ttu-id="e6804-505">V tomto scénáři můžete povolit šifrování pomocí šablony Resource Manageru, rutiny prostředí PowerShell nebo rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e6804-505">In this scenario, you can enable encrypting by using the Resource Manager template, PowerShell cmdlets, or CLI commands.</span></span> <span data-ttu-id="e6804-506">Následující části popisují podrobněji šablony Resource Manageru a rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e6804-506">The following sections explain in greater detail the Resource Manager template and CLI commands.</span></span>

<span data-ttu-id="e6804-507">Postupujte podle pokynů z jednoho z těchto částí pro přípravu předem šifrované bitové kopie, které lze použít v Azure.</span><span class="sxs-lookup"><span data-stu-id="e6804-507">Follow the instructions from one of these sections for preparing pre-encrypted images that can be used in Azure.</span></span> <span data-ttu-id="e6804-508">Po vytvoření image slouží k vytvoření šifrované virtuální počítač Azure podle kroků v další části.</span><span class="sxs-lookup"><span data-stu-id="e6804-508">After the image is created, you can use the steps in the next section to create an encrypted Azure VM.</span></span>

* [<span data-ttu-id="e6804-509">Příprava předem šifrované virtuálního pevného disku Windows</span><span class="sxs-lookup"><span data-stu-id="e6804-509">Prepare a pre-encrypted Windows VHD</span></span>](#preparing-a-pre-encrypted-windows-vhd)
* [<span data-ttu-id="e6804-510">Příprava předem šifrované Linux virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="e6804-510">Prepare a pre-encrypted Linux VHD</span></span>](#preparing-a-pre-encrypted-linux-vhd)

#### <a name="using-the-resource-manager-template"></a><span data-ttu-id="e6804-511">Pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="e6804-511">Using the Resource Manager template</span></span>
<span data-ttu-id="e6804-512">Můžete povolit šifrování disku na svůj šifrované disk VHD pomocí [šablony Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm).</span><span class="sxs-lookup"><span data-stu-id="e6804-512">You can enable disk encryption on your encrypted VHD by using the [Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm).</span></span>

1. <span data-ttu-id="e6804-513">V šabloně Azure rychlý start, klikněte na tlačítko **nasadit do Azure**, zadejte v konfiguraci šifrování na **parametry** okna a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e6804-513">On the Azure quick-start template, click **Deploy to Azure**, enter the encryption configuration on the **Parameters** blade, and then click **OK**.</span></span>

2. <span data-ttu-id="e6804-514">Vyberte předplatné, skupinu prostředků, umístění skupiny prostředků, právní podmínky a smlouvy a pak klikněte na tlačítko **vytvořit** chcete povolit šifrování na nový virtuální počítač IaaS.</span><span class="sxs-lookup"><span data-stu-id="e6804-514">Select the subscription, resource group, resource group location, legal terms, and agreement, and then click **Create** to enable encryption on the new IaaS VM.</span></span>

<span data-ttu-id="e6804-515">Následující tabulka uvádí parametrů šablony Resource Manageru pro vaše virtuální pevný disk šifrovaný:</span><span class="sxs-lookup"><span data-stu-id="e6804-515">The following table lists the Resource Manager template parameters for your encrypted VHD:</span></span>

| <span data-ttu-id="e6804-516">Parametr</span><span class="sxs-lookup"><span data-stu-id="e6804-516">Parameter</span></span> | <span data-ttu-id="e6804-517">Popis</span><span class="sxs-lookup"><span data-stu-id="e6804-517">Description</span></span> |
| --- | --- |
| <span data-ttu-id="e6804-518">newStorageAccountName</span><span class="sxs-lookup"><span data-stu-id="e6804-518">newStorageAccountName</span></span> | <span data-ttu-id="e6804-519">Název účtu úložiště pro ukládání šifrované virtuální pevný disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="e6804-519">Name of the storage account to store the encrypted OS VHD.</span></span> <span data-ttu-id="e6804-520">Tento účet úložiště by byl již vytvořen ve stejné skupině prostředků a stejné umístění jako virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e6804-520">This storage account should already have been created in the same resource group and same location as the VM.</span></span> |
| <span data-ttu-id="e6804-521">osVhdUri</span><span class="sxs-lookup"><span data-stu-id="e6804-521">osVhdUri</span></span> | <span data-ttu-id="e6804-522">Identifikátor URI virtuálního pevného disku operačního systému z účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e6804-522">URI of the OS VHD from the storage account.</span></span> |
| <span data-ttu-id="e6804-523">osType</span><span class="sxs-lookup"><span data-stu-id="e6804-523">osType</span></span> | <span data-ttu-id="e6804-524">Typ produktu operačního systému (Windows nebo Linuxem).</span><span class="sxs-lookup"><span data-stu-id="e6804-524">OS product type (Windows/Linux).</span></span> |
| <span data-ttu-id="e6804-525">virtualNetworkName</span><span class="sxs-lookup"><span data-stu-id="e6804-525">virtualNetworkName</span></span> | <span data-ttu-id="e6804-526">Název sítě VNet, která na síťový adaptér virtuálního počítače by měly patřit do.</span><span class="sxs-lookup"><span data-stu-id="e6804-526">Name of the VNet that the VM NIC should belong to.</span></span> <span data-ttu-id="e6804-527">Název by měl byl již vytvořen ve stejné skupině prostředků a stejné umístění jako virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e6804-527">The name should already have been created in the same resource group and same location as the VM.</span></span> |
| <span data-ttu-id="e6804-528">subnetName</span><span class="sxs-lookup"><span data-stu-id="e6804-528">subnetName</span></span> | <span data-ttu-id="e6804-529">Název v síti VNet, která na síťový adaptér virtuálního počítače by měly patřit do podsítě.</span><span class="sxs-lookup"><span data-stu-id="e6804-529">Name of the subnet on the VNet that the VM NIC should belong to.</span></span> |
| <span data-ttu-id="e6804-530">vmSize</span><span class="sxs-lookup"><span data-stu-id="e6804-530">vmSize</span></span> | <span data-ttu-id="e6804-531">Velikost virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e6804-531">Size of the VM.</span></span> <span data-ttu-id="e6804-532">V současné době jsou podporovány pouze standardní A, D a G řady.</span><span class="sxs-lookup"><span data-stu-id="e6804-532">Currently, only Standard A, D, and G series are supported.</span></span> |
| <span data-ttu-id="e6804-533">keyVaultResourceID</span><span class="sxs-lookup"><span data-stu-id="e6804-533">keyVaultResourceID</span></span> | <span data-ttu-id="e6804-534">ID prostředku, který identifikuje prostředek trezoru klíčů ve službě Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="e6804-534">The ResourceID that identifies the key vault resource in Azure Resource Manager.</span></span> <span data-ttu-id="e6804-535">Můžete ho získat pomocí rutiny prostředí PowerShell `(Get-AzureRmKeyVault -VaultName &lt;yourKeyVaultName&gt; -ResourceGroupName &lt;yourResourceGroupName&gt;).ResourceId`.</span><span class="sxs-lookup"><span data-stu-id="e6804-535">You can get it by using the PowerShell cmdlet `(Get-AzureRmKeyVault -VaultName &lt;yourKeyVaultName&gt; -ResourceGroupName &lt;yourResourceGroupName&gt;).ResourceId`.</span></span> |
| <span data-ttu-id="e6804-536">keyVaultSecretUrl</span><span class="sxs-lookup"><span data-stu-id="e6804-536">keyVaultSecretUrl</span></span> | <span data-ttu-id="e6804-537">Adresa URL disku šifrovací klíč, který je nastavený v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-537">URL of the disk-encryption key that's set up in the key vault.</span></span> |
| <span data-ttu-id="e6804-538">keyVaultKekUrl</span><span class="sxs-lookup"><span data-stu-id="e6804-538">keyVaultKekUrl</span></span> | <span data-ttu-id="e6804-539">Adresa URL klíče šifrovacího klíče pro šifrování klíče generované šifrování disku.</span><span class="sxs-lookup"><span data-stu-id="e6804-539">URL of the key encryption key for encrypting the generated disk-encryption key.</span></span> |
| <span data-ttu-id="e6804-540">vmName</span><span class="sxs-lookup"><span data-stu-id="e6804-540">vmName</span></span> | <span data-ttu-id="e6804-541">Název virtuálního počítače IaaS.</span><span class="sxs-lookup"><span data-stu-id="e6804-541">Name of the IaaS VM.</span></span> |

#### <a name="using-powershell-cmdlets"></a><span data-ttu-id="e6804-542">Pomocí rutin prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="e6804-542">Using PowerShell cmdlets</span></span>
<span data-ttu-id="e6804-543">Můžete povolit šifrování disku na svůj disk VHD šifrované pomocí rutiny prostředí PowerShell [ `Set-AzureRmVMOSDisk` ](/powershell/module/azurerm.compute/set-azurermvmosdisk).</span><span class="sxs-lookup"><span data-stu-id="e6804-543">You can enable disk encryption on your encrypted VHD by using the PowerShell cmdlet [`Set-AzureRmVMOSDisk`](/powershell/module/azurerm.compute/set-azurermvmosdisk).</span></span>  

#### <a name="using-cli-commands"></a><span data-ttu-id="e6804-544">Pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="e6804-544">Using CLI commands</span></span>
<span data-ttu-id="e6804-545">Pokud chcete povolit šifrování disku pro tento scénář pomocí rozhraní příkazového řádku, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e6804-545">To enable disk encryption for this scenario by using CLI commands, do the following:</span></span>

1. <span data-ttu-id="e6804-546">Nastavení zásad přístupu v trezoru klíčů:</span><span class="sxs-lookup"><span data-stu-id="e6804-546">Set access policies in your key vault:</span></span>

   * <span data-ttu-id="e6804-547">Nastavte **EnabledForDiskEncryption** příznak:</span><span class="sxs-lookup"><span data-stu-id="e6804-547">Set the **EnabledForDiskEncryption** flag:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * <span data-ttu-id="e6804-548">Nastavte oprávnění k zápisu tajných klíčů do trezoru klíčů aplikace Azure AD:</span><span class="sxs-lookup"><span data-stu-id="e6804-548">Set permissions to Azure AD application to write secrets to your key vault:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. <span data-ttu-id="e6804-549">Pokud chcete povolit šifrování na stávající nebo spuštěný virtuální počítač, zadejte:</span><span class="sxs-lookup"><span data-stu-id="e6804-549">To enable encryption on an existing or running VM, type:</span></span>

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. <span data-ttu-id="e6804-550">Načíst stav šifrování:</span><span class="sxs-lookup"><span data-stu-id="e6804-550">Get encryption status:</span></span>

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. <span data-ttu-id="e6804-551">Chcete-li povolit šifrování na nový virtuální počítač z vaší šifrované virtuálního pevného disku, použijte následující parametry s `azure vm create` příkaz:</span><span class="sxs-lookup"><span data-stu-id="e6804-551">To enable encryption on a new VM from your encrypted VHD, use the following parameters with the `azure vm create` command:</span></span>

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-existing-or-running-iaas-windows-vm-in-azure"></a><span data-ttu-id="e6804-552">Povolit šifrování na existující nebo běžící virtuální počítače IaaS Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="e6804-552">Enable encryption on existing or running IaaS Windows VM in Azure</span></span>
<span data-ttu-id="e6804-553">V tomto scénáři můžete povolit šifrování pomocí šablony Resource Manageru, rutiny prostředí PowerShell nebo rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e6804-553">In this scenario, you can enable encrypting by using the Resource Manager template, PowerShell cmdlets, or CLI commands.</span></span> <span data-ttu-id="e6804-554">Následující části popisují podrobněji povolení pomocí šablony Resource Manageru a rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e6804-554">The following sections explain in greater detail how to enable it by using the Resource Manager template and CLI commands.</span></span>

#### <a name="using-the-resource-manager-template"></a><span data-ttu-id="e6804-555">Pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="e6804-555">Using the Resource Manager template</span></span>
<span data-ttu-id="e6804-556">Můžete povolit šifrování disku na existující nebo spuštěných virtuálních počítačů IaaS Windows Azure pomocí [šablony Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm).</span><span class="sxs-lookup"><span data-stu-id="e6804-556">You can enable disk encryption on existing or running IaaS Windows VMs in Azure by using the [Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm).</span></span>

1. <span data-ttu-id="e6804-557">V šabloně Azure rychlý start, klikněte na tlačítko **nasadit do Azure**, zadejte v konfiguraci šifrování na **parametry** okna a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e6804-557">On the Azure quick-start template, click **Deploy to Azure**, enter the encryption configuration on the **Parameters** blade, and then click **OK**.</span></span>

2. <span data-ttu-id="e6804-558">Vyberte předplatné, skupinu prostředků, umístění skupiny prostředků, právní podmínky a smlouvy a pak klikněte na tlačítko **vytvořit** chcete povolit šifrování na stávající nebo spuštěných virtuálních počítačů IaaS.</span><span class="sxs-lookup"><span data-stu-id="e6804-558">Select the subscription, resource group, resource group location, legal terms, and agreement, and then click **Create** to enable encryption on the existing or running IaaS VM.</span></span>

<span data-ttu-id="e6804-559">V následující tabulce jsou uvedeny parametry šablony Resource Manageru pro existující nebo spuštěných virtuálních počítačů, které používají ID klienta Azure AD:</span><span class="sxs-lookup"><span data-stu-id="e6804-559">The following table lists the Resource Manager template parameters for existing or running VMs that use an Azure AD client ID:</span></span>

| <span data-ttu-id="e6804-560">Parametr</span><span class="sxs-lookup"><span data-stu-id="e6804-560">Parameter</span></span> | <span data-ttu-id="e6804-561">Popis</span><span class="sxs-lookup"><span data-stu-id="e6804-561">Description</span></span> |
| --- | --- |
| <span data-ttu-id="e6804-562">AADClientID</span><span class="sxs-lookup"><span data-stu-id="e6804-562">AADClientID</span></span> | <span data-ttu-id="e6804-563">ID klienta aplikace Azure AD, který má oprávnění k zápisu tajných klíčů do trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-563">Client ID of the Azure AD application that has permissions to write secrets to the key vault.</span></span> |
| <span data-ttu-id="e6804-564">AADClientSecret</span><span class="sxs-lookup"><span data-stu-id="e6804-564">AADClientSecret</span></span> | <span data-ttu-id="e6804-565">Tajný klíč klienta aplikace Azure AD, který má oprávnění k zápisu tajných klíčů do trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-565">Client secret of the Azure AD application that has permissions to write secrets to the key vault.</span></span> |
| <span data-ttu-id="e6804-566">keyVaultName</span><span class="sxs-lookup"><span data-stu-id="e6804-566">keyVaultName</span></span> | <span data-ttu-id="e6804-567">Název trezoru klíčů, který klíč Bitlockeru, musí být nahrán do.</span><span class="sxs-lookup"><span data-stu-id="e6804-567">Name of the key vault that the BitLocker key should be uploaded to.</span></span> <span data-ttu-id="e6804-568">Můžete ho získat pomocí rutiny `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`.</span><span class="sxs-lookup"><span data-stu-id="e6804-568">You can get it by using the cmdlet `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`.</span></span> |
|  <span data-ttu-id="e6804-569">keyEncryptionKeyURL</span><span class="sxs-lookup"><span data-stu-id="e6804-569">keyEncryptionKeyURL</span></span> | <span data-ttu-id="e6804-570">Adresa URL klíče šifrovací klíč, který se používá k šifrování generovaný klíč nástroje BitLocker.</span><span class="sxs-lookup"><span data-stu-id="e6804-570">URL of the key encryption key that's used to encrypt the generated BitLocker key.</span></span> <span data-ttu-id="e6804-571">Tento parametr je nepovinný, pokud vyberete **nokek** v rozevíracím seznamu UseExistingKek.</span><span class="sxs-lookup"><span data-stu-id="e6804-571">This parameter is optional if you select **nokek** in the UseExistingKek drop-down list.</span></span> <span data-ttu-id="e6804-572">Pokud vyberete **kek** v rozevíracím seznamu UseExistingKek, je nutné zadat _keyEncryptionKeyURL_ hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e6804-572">If you select **kek** in the UseExistingKek drop-down list, you must enter the _keyEncryptionKeyURL_ value.</span></span> |
| <span data-ttu-id="e6804-573">volumeType</span><span class="sxs-lookup"><span data-stu-id="e6804-573">volumeType</span></span> | <span data-ttu-id="e6804-574">Typ svazku, který operace šifrování.</span><span class="sxs-lookup"><span data-stu-id="e6804-574">Type of volume that the encryption operation is performed on.</span></span> <span data-ttu-id="e6804-575">Platné hodnoty jsou _OS_, _Data_, a _všechny_.</span><span class="sxs-lookup"><span data-stu-id="e6804-575">Valid values are _OS_, _Data_, and _All_.</span></span> |
| <span data-ttu-id="e6804-576">sequenceVersion</span><span class="sxs-lookup"><span data-stu-id="e6804-576">sequenceVersion</span></span> | <span data-ttu-id="e6804-577">Pořadí verze operace nástroje BitLocker.</span><span class="sxs-lookup"><span data-stu-id="e6804-577">Sequence version of the BitLocker operation.</span></span> <span data-ttu-id="e6804-578">Toto číslo verze zvýší pokaždé, když se operace šifrování disku provádí na stejného virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e6804-578">Increment this version number every time a disk-encryption operation is performed on the same VM.</span></span> |
| <span data-ttu-id="e6804-579">vmName</span><span class="sxs-lookup"><span data-stu-id="e6804-579">vmName</span></span> | <span data-ttu-id="e6804-580">Název virtuálního počítače, který operace šifrování je třeba provést na.</span><span class="sxs-lookup"><span data-stu-id="e6804-580">Name of the VM that the encryption operation is to be performed on.</span></span> |

> [!NOTE]
> <span data-ttu-id="e6804-581">_KeyEncryptionKeyURL_ je volitelný parametr.</span><span class="sxs-lookup"><span data-stu-id="e6804-581">_KeyEncryptionKeyURL_ is an optional parameter.</span></span> <span data-ttu-id="e6804-582">Vlastní KEK můžete zahrnout do další chránit datový šifrovací klíč (šifrování nástroje BitLocker tajný klíč) v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-582">You can bring your own KEK to further safeguard the data encryption key (BitLocker encryption secret) in the key vault.</span></span>

#### <a name="using-powershell-cmdlets"></a><span data-ttu-id="e6804-583">Pomocí rutin prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="e6804-583">Using PowerShell cmdlets</span></span>
<span data-ttu-id="e6804-584">Informace o povolení šifrování s Azure Disk Encryption pomocí rutin prostředí PowerShell, najdete v příspěvcích na blogu [prozkoumat Azure Disk Encryption s prostředím Azure PowerShell – část 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) a [prozkoumat Azure Disk Encryption v prostředí Azure PowerShell - část 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).</span><span class="sxs-lookup"><span data-stu-id="e6804-584">For information about enabling encryption with Azure Disk Encryption by using PowerShell cmdlets, see the blog posts [Explore Azure Disk Encryption with Azure PowerShell - Part 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) and [Explore Azure Disk Encryption with Azure PowerShell - Part 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).</span></span>

#### <a name="using-cli-commands"></a><span data-ttu-id="e6804-585">Pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="e6804-585">Using CLI commands</span></span>
<span data-ttu-id="e6804-586">Pokud chcete povolit šifrování na existující nebo spuštěné virtuální počítače IaaS Windows v Azure pomocí rozhraní příkazového řádku, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e6804-586">To enable encryption on existing or running IaaS Windows VM in Azure using CLI commands, do the following:</span></span>

1. <span data-ttu-id="e6804-587">Nastavení zásad přístupu v trezoru klíčů:</span><span class="sxs-lookup"><span data-stu-id="e6804-587">To set access policies in the key vault:</span></span>
   * <span data-ttu-id="e6804-588">Nastavte **EnabledForDiskEncryption** příznak:</span><span class="sxs-lookup"><span data-stu-id="e6804-588">Set the **EnabledForDiskEncryption** flag:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * <span data-ttu-id="e6804-589">Nastavte oprávnění k zápisu tajných klíčů do trezoru klíčů aplikace Azure AD:</span><span class="sxs-lookup"><span data-stu-id="e6804-589">Set permissions to Azure AD application to write secrets to your key vault:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`
2. <span data-ttu-id="e6804-590">Chcete-li povolit šifrování na stávající nebo spuštěný virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="e6804-590">To enable encryption on an existing or running VM:</span></span>

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`
3. <span data-ttu-id="e6804-591">Pokud chcete získat stav šifrování:</span><span class="sxs-lookup"><span data-stu-id="e6804-591">To get encryption status:</span></span>

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`
4. <span data-ttu-id="e6804-592">Chcete-li povolit šifrování na nový virtuální počítač z vaší šifrované virtuálního pevného disku, použijte následující parametry s `azure vm create` příkaz:</span><span class="sxs-lookup"><span data-stu-id="e6804-592">To enable encryption on a new VM from your encrypted VHD, use the following parameters with the `azure vm create` command:</span></span>

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-an-existing-or-running-iaas-linux-vm-in-azure"></a><span data-ttu-id="e6804-593">Povolit šifrování na existující nebo spuštěné IaaS Linux virtuální počítač v Azure</span><span class="sxs-lookup"><span data-stu-id="e6804-593">Enable encryption on an existing or running IaaS Linux VM in Azure</span></span>
<span data-ttu-id="e6804-594">Můžete povolit šifrování disku na existující nebo spuštěné IaaS Linux virtuální počítač v Azure pomocí [šablony Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm).</span><span class="sxs-lookup"><span data-stu-id="e6804-594">You can enable disk encryption on an existing or running IaaS Linux VM in Azure by using the [Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm).</span></span>

1. <span data-ttu-id="e6804-595">Klikněte na tlačítko **nasadit do Azure** v šabloně Azure rychlý start, zadejte konfiguraci šifrování **parametry** okna a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e6804-595">Click **Deploy to Azure** on the Azure quick-start template, enter the encryption configuration on the **Parameters** blade, and then click **OK**.</span></span>

2. <span data-ttu-id="e6804-596">Vyberte předplatné, skupinu prostředků, umístění skupiny prostředků, právní podmínky a smlouvy a pak klikněte na tlačítko **vytvořit** chcete povolit šifrování na stávající nebo spuštěných virtuálních počítačů IaaS.</span><span class="sxs-lookup"><span data-stu-id="e6804-596">Select the subscription, resource group, resource group location, legal terms, and agreement, and then click **Create** to enable encryption on the existing or running IaaS VM.</span></span>

<span data-ttu-id="e6804-597">V následující tabulce jsou uvedeny parametry šablony Resource Manageru pro existující nebo spuštěných virtuálních počítačů, které používají ID klienta Azure AD:</span><span class="sxs-lookup"><span data-stu-id="e6804-597">The following table lists Resource Manager template parameters for existing or running VMs that use an Azure AD client ID:</span></span>

| <span data-ttu-id="e6804-598">Parametr</span><span class="sxs-lookup"><span data-stu-id="e6804-598">Parameter</span></span> | <span data-ttu-id="e6804-599">Popis</span><span class="sxs-lookup"><span data-stu-id="e6804-599">Description</span></span> |
| --- | --- |
| <span data-ttu-id="e6804-600">AADClientID</span><span class="sxs-lookup"><span data-stu-id="e6804-600">AADClientID</span></span> | <span data-ttu-id="e6804-601">ID klienta aplikace Azure AD, který má oprávnění k zápisu tajných klíčů do trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-601">Client ID of the Azure AD application that has permissions to write secrets to the key vault.</span></span> |
| <span data-ttu-id="e6804-602">AADClientSecret</span><span class="sxs-lookup"><span data-stu-id="e6804-602">AADClientSecret</span></span> | <span data-ttu-id="e6804-603">Tajný klíč klienta aplikace Azure AD, který má oprávnění k zápisu tajných klíčů do trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-603">Client secret of the Azure AD application that has permissions to write secrets to your key vault.</span></span> |
| <span data-ttu-id="e6804-604">keyVaultName</span><span class="sxs-lookup"><span data-stu-id="e6804-604">keyVaultName</span></span> | <span data-ttu-id="e6804-605">Název trezoru klíčů, který klíč Bitlockeru, musí být nahrán do.</span><span class="sxs-lookup"><span data-stu-id="e6804-605">Name of the key vault that the BitLocker key should be uploaded to.</span></span> <span data-ttu-id="e6804-606">Můžete ho získat pomocí rutiny `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`.</span><span class="sxs-lookup"><span data-stu-id="e6804-606">You can get it by using the cmdlet `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`.</span></span> |
|  <span data-ttu-id="e6804-607">keyEncryptionKeyURL</span><span class="sxs-lookup"><span data-stu-id="e6804-607">keyEncryptionKeyURL</span></span> | <span data-ttu-id="e6804-608">Adresa URL klíče šifrovací klíč, který se používá k šifrování generovaný klíč nástroje BitLocker.</span><span class="sxs-lookup"><span data-stu-id="e6804-608">URL of the key encryption key that's used to encrypt the generated BitLocker key.</span></span> <span data-ttu-id="e6804-609">Tento parametr je nepovinný, pokud vyberete **nokek** v rozevíracím seznamu UseExistingKek.</span><span class="sxs-lookup"><span data-stu-id="e6804-609">This parameter is optional if you select **nokek** in the UseExistingKek drop-down list.</span></span> <span data-ttu-id="e6804-610">Pokud vyberete **kek** v rozevíracím seznamu UseExistingKek, je nutné zadat _keyEncryptionKeyURL_ hodnotu.</span><span class="sxs-lookup"><span data-stu-id="e6804-610">If you select **kek** in the UseExistingKek drop-down list, you must enter the _keyEncryptionKeyURL_ value.</span></span> |
| <span data-ttu-id="e6804-611">volumeType</span><span class="sxs-lookup"><span data-stu-id="e6804-611">volumeType</span></span> | <span data-ttu-id="e6804-612">Typ svazku, který operace šifrování.</span><span class="sxs-lookup"><span data-stu-id="e6804-612">Type of volume that the encryption operation is performed on.</span></span> <span data-ttu-id="e6804-613">Platné podporované hodnoty jsou _OS_ nebo _všechny_ (pro RHEL 7.2, CentOS 7.2 a Ubuntu 16.04), a _Data_ (pro všechny ostatní distribuce).</span><span class="sxs-lookup"><span data-stu-id="e6804-613">Valid supported values are _OS_ or _All_ (for RHEL 7.2, CentOS 7.2, and Ubuntu 16.04), and _Data_ (for all other distributions).</span></span> |
| <span data-ttu-id="e6804-614">sequenceVersion</span><span class="sxs-lookup"><span data-stu-id="e6804-614">sequenceVersion</span></span> | <span data-ttu-id="e6804-615">Pořadí verze operace nástroje BitLocker.</span><span class="sxs-lookup"><span data-stu-id="e6804-615">Sequence version of the BitLocker operation.</span></span> <span data-ttu-id="e6804-616">Toto číslo verze zvýší pokaždé, když se operace šifrování disku provádí na stejného virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e6804-616">Increment this version number every time a disk-encryption operation is performed on the same VM.</span></span> |
| <span data-ttu-id="e6804-617">vmName</span><span class="sxs-lookup"><span data-stu-id="e6804-617">vmName</span></span> | <span data-ttu-id="e6804-618">Název virtuálního počítače, který operace šifrování je třeba provést na.</span><span class="sxs-lookup"><span data-stu-id="e6804-618">Name of the VM that the encryption operation is to be performed on.</span></span> |
| <span data-ttu-id="e6804-619">přístupové heslo</span><span class="sxs-lookup"><span data-stu-id="e6804-619">passPhrase</span></span> | <span data-ttu-id="e6804-620">Zadejte silné heslo jako datový šifrovací klíč.</span><span class="sxs-lookup"><span data-stu-id="e6804-620">Type a strong passphrase as the data encryption key.</span></span> |

> [!NOTE]
> <span data-ttu-id="e6804-621">_KeyEncryptionKeyURL_ je volitelný parametr.</span><span class="sxs-lookup"><span data-stu-id="e6804-621">_KeyEncryptionKeyURL_ is an optional parameter.</span></span> <span data-ttu-id="e6804-622">Můžete zahrnout vlastní KEK na další chránit datový šifrovací klíč (heslo tajný klíč) v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-622">You can bring your own KEK to further safeguard the data encryption key (passphrase secret) in your key vault.</span></span>

#### <a name="cli-commands"></a><span data-ttu-id="e6804-623">Rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="e6804-623">CLI commands</span></span>
<span data-ttu-id="e6804-624">Můžete povolit šifrování disku na svůj disk VHD šifrované instalaci a použití [rozhraní příkazového řádku příkaz](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="e6804-624">You can enable disk encryption on your encrypted VHD by installing and using the [CLI command](../cli-install-nodejs.md).</span></span> <span data-ttu-id="e6804-625">Pokud chcete povolit šifrování na existující nebo běžící virtuální počítače IaaS Linux v Azure pomocí rozhraní příkazového řádku, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e6804-625">To enable encryption on existing or running IaaS Linux VMs in Azure by using CLI commands, do the following:</span></span>

1. <span data-ttu-id="e6804-626">Nastavit zásady přístupu v trezoru klíčů:</span><span class="sxs-lookup"><span data-stu-id="e6804-626">Set access policies in the key vault:</span></span>

 * <span data-ttu-id="e6804-627">Nastavte **EnabledForDiskEncryption** příznak:</span><span class="sxs-lookup"><span data-stu-id="e6804-627">Set the **EnabledForDiskEncryption** flag:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
 * <span data-ttu-id="e6804-628">Nastavte oprávnění k zápisu tajných klíčů do trezoru klíčů aplikace Azure AD:</span><span class="sxs-lookup"><span data-stu-id="e6804-628">Set permissions to Azure AD application to write secrets to your key vault:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. <span data-ttu-id="e6804-629">Chcete-li povolit šifrování na stávající nebo spuštěný virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="e6804-629">To enable encryption on an existing or running VM:</span></span>

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. <span data-ttu-id="e6804-630">Načíst stav šifrování:</span><span class="sxs-lookup"><span data-stu-id="e6804-630">Get encryption status:</span></span>

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. <span data-ttu-id="e6804-631">Chcete-li povolit šifrování na nový virtuální počítač z vaší šifrované virtuálního pevného disku, použijte následující parametry s `azure vm create` příkaz:</span><span class="sxs-lookup"><span data-stu-id="e6804-631">To enable encryption on a new VM from your encrypted VHD, use the following parameters with the `azure vm create` command:</span></span>
 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="get-the-encryption-status-of-an-encrypted-iaas-vm"></a><span data-ttu-id="e6804-632">Načíst stav šifrování virtuálního počítače šifrovaný IaaS</span><span class="sxs-lookup"><span data-stu-id="e6804-632">Get the encryption status of an encrypted IaaS VM</span></span>
<span data-ttu-id="e6804-633">Pomocí Azure Resource Manager, můžete získat stav šifrování [rutiny prostředí PowerShell](/powershell/azure/overview), nebo rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e6804-633">You can get the encryption status by using Azure Resource Manager, [PowerShell cmdlets](/powershell/azure/overview), or CLI commands.</span></span> <span data-ttu-id="e6804-634">Následující části popisují, jak používat portál Azure classic a rozhraní příkazového řádku a získat stav šifrování.</span><span class="sxs-lookup"><span data-stu-id="e6804-634">The following sections explain how to use the Azure classic portal and CLI commands to get the encryption status.</span></span>

#### <a name="get-the-encryption-status-of-an-encrypted-windows-vm-by-using-azure-resource-manager"></a><span data-ttu-id="e6804-635">Načíst stav šifrování šifrovaných virtuální počítač Windows pomocí Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="e6804-635">Get the encryption status of an encrypted Windows VM by using Azure Resource Manager</span></span>
<span data-ttu-id="e6804-636">Stav šifrování virtuálních počítačů IaaS můžete získat z Azure Resource Manager následujícím postupem:</span><span class="sxs-lookup"><span data-stu-id="e6804-636">You can get the encryption status of the IaaS VM from Azure Resource Manager by doing the following:</span></span>

1. <span data-ttu-id="e6804-637">Přihlaste se k [portál Azure classic](https://portal.azure.com/)a potom klikněte na **virtuální počítače** v levém podokně zobrazíte souhrnné zobrazení virtuálních počítačů v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="e6804-637">Sign in to the [Azure classic portal](https://portal.azure.com/), and then click **Virtual machines** in the left pane to see a summary view of the virtual machines in your subscription.</span></span> <span data-ttu-id="e6804-638">Můžete filtrovat zobrazení virtuálních počítačů tak, že vyberete název odběru v **předplatné** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="e6804-638">You can filter the virtual machines view by selecting the subscription name in the **Subscription** drop-down list.</span></span>

2. <span data-ttu-id="e6804-639">V horní části **virtuální počítače** klikněte na tlačítko **sloupce**.</span><span class="sxs-lookup"><span data-stu-id="e6804-639">At the top of the **Virtual machines** page, click **Columns**.</span></span>

3. <span data-ttu-id="e6804-640">Na **vyberte sloupec** vyberte **šifrování disku**a potom klikněte na **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="e6804-640">On the **Choose column** blade, select **Disk Encryption**, and then click **Update**.</span></span> <span data-ttu-id="e6804-641">Měli byste vidět šifrování disku sloupec zobrazuje stav šifrování _povoleno_ nebo _Nepovoleno_ pro každý virtuální počítač, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="e6804-641">You should see the disk-encryption column showing the encryption state _Enabled_ or _Not Enabled_ for each VM, as shown in the following figure:</span></span>

 ![Microsoft Antimalware v Azure](./media/azure-security-disk-encryption/disk-encryption-fig2.png)

#### <a name="get-the-encryption-status-of-an-encrypted-windowslinux-iaas-vm-by-using-the-disk-encryption-powershell-cmdlet"></a><span data-ttu-id="e6804-643">Načíst stav šifrování virtuálního počítače šifrovaný IaaS (Windows nebo Linuxem) pomocí rutiny prostředí PowerShell šifrování disku</span><span class="sxs-lookup"><span data-stu-id="e6804-643">Get the encryption status of an encrypted (Windows/Linux) IaaS VM by using the disk-encryption PowerShell cmdlet</span></span>
<span data-ttu-id="e6804-644">Stav šifrování virtuálních počítačů IaaS můžete získat z rutiny prostředí PowerShell šifrování disku `Get-AzureRmVMDiskEncryptionStatus`.</span><span class="sxs-lookup"><span data-stu-id="e6804-644">You can get the encryption status of the IaaS VM from the disk-encryption PowerShell cmdlet `Get-AzureRmVMDiskEncryptionStatus`.</span></span> <span data-ttu-id="e6804-645">Chcete-li získat nastavení šifrování pro virtuální počítač, zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e6804-645">To get the encryption settings for your VM, enter the following:</span></span>

    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : NotEncrypted
    DataVolumesEncrypted       : Encrypted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a

<span data-ttu-id="e6804-646">Si můžete prohlédnout výstup _Get-AzureRmVMDiskEncryptionStatus_ pro šifrování klíče adresy URL.</span><span class="sxs-lookup"><span data-stu-id="e6804-646">You can inspect the output of _Get-AzureRmVMDiskEncryptionStatus_ for encryption key URLs.</span></span>

    C:\> $status = Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName
    e $VMName -ExtensionName $ExtensionName
    C:\> $status.OsVolumeEncryptionSettings

    DiskEncryptionKey                                                 KeyEncryptionKey                                               Enabled
    -----------------                                                 ----------------                                               -------
    Microsoft.Azure.Management.Compute.Models.KeyVaultSecretReference Microsoft.Azure.Management.Compute.Models.KeyVaultKeyReference    True


    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey.SecretUrl
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a
    C:\> $status.OsVolumeEncryptionSettings.DiskEncryptionKey

    SecretUrl                                                                                                               SourceVault
    ---------                                                                                                               -----------
    https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a Microsoft.Azure.Management....

<span data-ttu-id="e6804-647">Nastavení hodnoty OSVolumeEncrypted a DataVolumesEncrypted jsou nastaveny na _šifrovaný_, který ukazuje, že jsou oba svazky šifrované pomocí Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="e6804-647">The OSVolumeEncrypted and DataVolumesEncrypted settings values are set to _Encrypted_, which shows that both volumes are encrypted using Azure Disk Encryption.</span></span> <span data-ttu-id="e6804-648">Informace o povolení šifrování s Azure Disk Encryption pomocí rutin prostředí PowerShell, najdete v příspěvcích na blogu [prozkoumat Azure Disk Encryption s prostředím Azure PowerShell – část 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) a [prozkoumat Azure Disk Encryption v prostředí Azure PowerShell - část 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).</span><span class="sxs-lookup"><span data-stu-id="e6804-648">For information about enabling encryption with Azure Disk Encryption by using PowerShell cmdlets, see the blog posts [Explore Azure Disk Encryption with Azure PowerShell - Part 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) and [Explore Azure Disk Encryption with Azure PowerShell - Part 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="e6804-649">Na virtuální počítače s Linuxem, trvá tři až čtyři minut `Get-AzureRmVMDiskEncryptionStatus` rutiny nahlásit stav šifrování.</span><span class="sxs-lookup"><span data-stu-id="e6804-649">On Linux VMs, it takes three to four minutes for the `Get-AzureRmVMDiskEncryptionStatus` cmdlet to report the encryption status.</span></span>

#### <a name="get-the-encryption-status-of-the-iaas-vm-from-the-disk-encryption-cli-command"></a><span data-ttu-id="e6804-650">Načíst stav šifrování virtuálních počítačů IaaS z rozhraní příkazového řádku příkaz šifrování disku</span><span class="sxs-lookup"><span data-stu-id="e6804-650">Get the encryption status of the IaaS VM from the disk-encryption CLI command</span></span>
<span data-ttu-id="e6804-651">Stav šifrování virtuálních počítačů IaaS můžete získat pomocí rozhraní příkazového řádku příkaz šifrování disku `azure vm show-disk-encryption-status`.</span><span class="sxs-lookup"><span data-stu-id="e6804-651">You can get the encryption status of the IaaS VM by using the disk-encryption CLI command `azure vm show-disk-encryption-status`.</span></span> <span data-ttu-id="e6804-652">Chcete-li získat nastavení šifrování pro virtuální počítač, zadejte relaci příkazového řádku Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="e6804-652">To get the encryption settings for your VM, enter your Azure CLI session:</span></span>

    azure vm show-disk-encryption-status --resource-group <yourResourceGroupName> --name <yourVMName> --json  

#### <a name="disable-encryption-on-running-windows-iaas-vm"></a><span data-ttu-id="e6804-653">Zakázat šifrování u spuštěných virtuálních počítačů IaaS Windows</span><span class="sxs-lookup"><span data-stu-id="e6804-653">Disable encryption on running Windows IaaS VM</span></span>
<span data-ttu-id="e6804-654">Můžete zakázat šifrování na spuštěný Windows nebo virtuálních počítačů IaaS Linux pomocí šablony Azure Disk Encryption Resource Manageru nebo rutiny prostředí PowerShell a zadejte konfiguraci dešifrování.</span><span class="sxs-lookup"><span data-stu-id="e6804-654">You can disable encryption on a running Windows or Linux IaaS VM via the Azure Disk Encryption Resource Manager template or PowerShell cmdlets and specify the decryption configuration.</span></span>

##### <a name="windows-vm"></a><span data-ttu-id="e6804-655">Virtuální počítač s Windows</span><span class="sxs-lookup"><span data-stu-id="e6804-655">Windows VM</span></span>
<span data-ttu-id="e6804-656">Zakázat šifrování krok zakazuje šifrování operačního systému, datový svazek nebo obojí do spuštěného virtuálního počítače Windows IaaS.</span><span class="sxs-lookup"><span data-stu-id="e6804-656">The disable-encryption step disables encryption of the OS, the data volume, or both on the running Windows IaaS VM.</span></span> <span data-ttu-id="e6804-657">Nelze zakázat svazku operačního systému a nechte datový svazek zašifrovaná.</span><span class="sxs-lookup"><span data-stu-id="e6804-657">You cannot disable the OS volume and leave the data volume encrypted.</span></span> <span data-ttu-id="e6804-658">Při provádění krok zakázat šifrování modelu nasazení Azure classic aktualizace modelu služby virtuálních počítačů a virtuálních počítačů IaaS Windows je označena dešifrovaný.</span><span class="sxs-lookup"><span data-stu-id="e6804-658">When the disable-encryption step is performed, the Azure classic deployment model updates the VM service model, and the Windows IaaS VM is marked decrypted.</span></span> <span data-ttu-id="e6804-659">Obsah virtuálního počítače jsou již v zašifrované podobě.</span><span class="sxs-lookup"><span data-stu-id="e6804-659">The contents of the VM are no longer encrypted at rest.</span></span> <span data-ttu-id="e6804-660">Dešifrování nedojde k odstranění trezoru klíčů a materiál klíče pro šifrování (šifrovací klíče nástroje BitLocker pro systém Windows a heslo pro Linux).</span><span class="sxs-lookup"><span data-stu-id="e6804-660">The decryption does not delete your key vault and the encryption key material (BitLocker encryption keys for Windows and Passphrase for Linux).</span></span>

##### <a name="linux-vm"></a><span data-ttu-id="e6804-661">Virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="e6804-661">Linux VM</span></span>
<span data-ttu-id="e6804-662">Zakázat šifrování krok zakazuje šifrování datový svazek na spuštěného virtuálního počítače s Linuxem IaaS.</span><span class="sxs-lookup"><span data-stu-id="e6804-662">The disable-encryption step disables encryption of the data volume on the running Linux IaaS VM.</span></span> <span data-ttu-id="e6804-663">Tento krok je funkční pouze v případě disk operačního systému není zašifrován.</span><span class="sxs-lookup"><span data-stu-id="e6804-663">This step only works if the OS disk is not encrypted.</span></span>

> [!NOTE]
> <span data-ttu-id="e6804-664">Zakázáním šifrování na disk operačního systému není povolená na virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="e6804-664">Disabling encryption on the OS disk is not allowed on Linux VMs.</span></span>

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a><span data-ttu-id="e6804-665">Zakázat šifrování na stávající nebo spuštěný virtuální počítač IaaS</span><span class="sxs-lookup"><span data-stu-id="e6804-665">Disable encryption on an existing or running IaaS VM</span></span>
<span data-ttu-id="e6804-666">Můžete zakázat šifrování disku na spuštěných virtuálních počítačů IaaS Windows pomocí [šablony Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm).</span><span class="sxs-lookup"><span data-stu-id="e6804-666">You can disable disk encryption on running Windows IaaS VMs by using the [Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm).</span></span>

1. <span data-ttu-id="e6804-667">V šabloně Azure rychlý start, klikněte na tlačítko **nasadit do Azure**, zadejte v konfiguraci dešifrování na **parametry** okna a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="e6804-667">On the Azure quick-start template, click **Deploy to Azure**, enter the decryption configuration on the **Parameters** blade, and then click **OK**.</span></span>

2. <span data-ttu-id="e6804-668">Vyberte předplatné, skupinu prostředků, umístění skupiny prostředků, právní podmínky a smlouvy a pak klikněte na tlačítko **vytvořit** chcete povolit šifrování na nový virtuální počítač IaaS.</span><span class="sxs-lookup"><span data-stu-id="e6804-668">Select the subscription, resource group, resource group location, legal terms, and agreement, and then click **Create** to enable encryption on a new IaaS VM.</span></span>

<span data-ttu-id="e6804-669">Pro virtuální počítače s Linuxem, můžete zakázat šifrování pomocí [vypnout šifrování u spuštěného virtuálního počítače s Linuxem](https://aka.ms/decrypt-linuxvm) šablony.</span><span class="sxs-lookup"><span data-stu-id="e6804-669">For Linux VMs, you can disable encryption by using the [Disable encryption on a running Linux VM](https://aka.ms/decrypt-linuxvm) template.</span></span>

<span data-ttu-id="e6804-670">V následující tabulce jsou uvedeny parametry šablony Resource Manageru pro zakázáním šifrování na spuštěný virtuální počítač IaaS:</span><span class="sxs-lookup"><span data-stu-id="e6804-670">The following table lists Resource Manager template parameters for disabling encryption on a running IaaS VM:</span></span>

| <span data-ttu-id="e6804-671">Parametr</span><span class="sxs-lookup"><span data-stu-id="e6804-671">Parameter</span></span> | <span data-ttu-id="e6804-672">Popis</span><span class="sxs-lookup"><span data-stu-id="e6804-672">Description</span></span> |
| --- | --- |
| <span data-ttu-id="e6804-673">vmName</span><span class="sxs-lookup"><span data-stu-id="e6804-673">vmName</span></span> | <span data-ttu-id="e6804-674">Název virtuálního počítače, který operace šifrování je třeba provést na.</span><span class="sxs-lookup"><span data-stu-id="e6804-674">Name of the VM that the encryption operation is to be performed on.</span></span>
| <span data-ttu-id="e6804-675">volumeType</span><span class="sxs-lookup"><span data-stu-id="e6804-675">volumeType</span></span> | <span data-ttu-id="e6804-676">Typ svazku, který na se provést operaci dešifrování.</span><span class="sxs-lookup"><span data-stu-id="e6804-676">Type of volume that a decryption operation is performed on.</span></span> <span data-ttu-id="e6804-677">Platné hodnoty jsou _OS_, _Data_, a _všechny_.</span><span class="sxs-lookup"><span data-stu-id="e6804-677">Valid values are _OS_, _Data_, and _All_.</span></span> <span data-ttu-id="e6804-678">Nelze zakázat šifrování u spuštěných virtuálních počítačů IaaS Windows operačního systému nebo spouštěcí svazek bez zakázáním šifrování na _Data_ svazku.</span><span class="sxs-lookup"><span data-stu-id="e6804-678">You cannot disable encryption on running Windows IaaS VM OS/boot volume without disabling encryption on the _Data_ volume.</span></span> <span data-ttu-id="e6804-679">Všimněte si také, že zakázáním šifrování na disk operačního systému není povolená na virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="e6804-679">Also note that disabling encryption on the OS disk is not allowed on Linux VMs.</span></span> |
| <span data-ttu-id="e6804-680">sequenceVersion</span><span class="sxs-lookup"><span data-stu-id="e6804-680">sequenceVersion</span></span> | <span data-ttu-id="e6804-681">Pořadí verze operace nástroje BitLocker.</span><span class="sxs-lookup"><span data-stu-id="e6804-681">Sequence version of the BitLocker operation.</span></span> <span data-ttu-id="e6804-682">Toto číslo verze zvýší pokaždé, když provádí operaci dešifrování disku na stejného virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e6804-682">Increment this version number every time a disk decryption operation is performed on the same VM.</span></span> |

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a><span data-ttu-id="e6804-683">Zakázat šifrování na stávající nebo spuštěný virtuální počítač IaaS</span><span class="sxs-lookup"><span data-stu-id="e6804-683">Disable encryption on an existing or running IaaS VM</span></span>
<span data-ttu-id="e6804-684">Můžete zakázat šifrování na stávající nebo spuštěný virtuální počítač IaaS pomocí rutiny prostředí PowerShell, najdete v části [ `Disable-AzureRmVMDiskEncryption` ](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption).</span><span class="sxs-lookup"><span data-stu-id="e6804-684">To disable encryption on an existing or running IaaS VM by using the PowerShell cmdlet, see [`Disable-AzureRmVMDiskEncryption`](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption).</span></span> <span data-ttu-id="e6804-685">Tato rutina podporuje systém Windows a virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="e6804-685">This cmdlet supports both Windows and Linux VMs.</span></span> <span data-ttu-id="e6804-686">Můžete zakázat šifrování, nainstaluje rozšíření ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="e6804-686">To disable encryption, it installs an extension on the virtual machine.</span></span> <span data-ttu-id="e6804-687">Pokud _název_ není zadán parametr, rozšíření s výchozím názvem _AzureDiskEncryption virtuálních počítačů pro Windows_ je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="e6804-687">If the _Name_ parameter is not specified, an extension with the default name _AzureDiskEncryption for Windows VMs_ is created.</span></span>

<span data-ttu-id="e6804-688">Na virtuální počítače s Linuxem je použít AzureDiskEncryptionForLinux rozšíření.</span><span class="sxs-lookup"><span data-stu-id="e6804-688">On Linux VMs, the AzureDiskEncryptionForLinux extension is used.</span></span>

> [!NOTE]
> <span data-ttu-id="e6804-689">Tato rutina restartuje virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="e6804-689">This cmdlet reboots the virtual machine.</span></span>

### <a name="enable-encryption-on-pre-encrypted-iaas-vm-with-azure-managed-disk"></a><span data-ttu-id="e6804-690">Povolit šifrování na předem šifrované virtuálních počítačů IaaS s diskem spravované Azure</span><span class="sxs-lookup"><span data-stu-id="e6804-690">Enable encryption on pre-encrypted IaaS VM with Azure Managed Disk</span></span>
<span data-ttu-id="e6804-691">Pomocí šablony Azure ARM disku spravované vytvoření šifrované virtuálního počítače z disku VHD předem šifrované pomocí šablony ARM nacházející se v</span><span class="sxs-lookup"><span data-stu-id="e6804-691">Use the Azure Managed Disk ARM template to create a encrypted VM from a pre-encrypted VHD using the ARM template located at</span></span>   
<span data-ttu-id="e6804-692">[Vytvořit nové šifrované spravovaným diskem z předem šifrované objektu blob virtuálního pevného disku nebo úložiště] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-create-encrypted-managed-disk)</span><span class="sxs-lookup"><span data-stu-id="e6804-692">[Create a new encrypted managed disk from a pre-encrypted VHD/storage blob] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-create-encrypted-managed-disk)</span></span>

### <a name="enable-encryption-on-a-new-linux-iaas-vm-with-azure-managed-disk"></a><span data-ttu-id="e6804-693">Povolit šifrování na nový virtuální počítač IaaS Linux s diskem spravované Azure</span><span class="sxs-lookup"><span data-stu-id="e6804-693">Enable encryption on a new Linux IaaS VM with Azure Managed Disk</span></span>
<span data-ttu-id="e6804-694">Pomocí šablony Azure ARM disku spravované k vytvoření nové šifrovaný IaaS virtuálního počítače s Linuxem pomocí šablony ARM nacházející se v</span><span class="sxs-lookup"><span data-stu-id="e6804-694">Use the Azure Managed Disk ARM template to create a new encrypted Linux IaaS VM using the ARM template located at</span></span>   
<span data-ttu-id="e6804-695">[Nasazení RHEL 7.2 s šifrování celého disku] (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-full-disk-encrypted-rhel)</span><span class="sxs-lookup"><span data-stu-id="e6804-695">[Deployment of RHEL 7.2 with full disk encryption] (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-full-disk-encrypted-rhel)</span></span>

### <a name="enable-encryption-on-a-new-windows-iaas-vm-with-azure-managed-disk"></a><span data-ttu-id="e6804-696">Povolit šifrování na nový virtuální počítač IaaS Windows s Azure spravované disku</span><span class="sxs-lookup"><span data-stu-id="e6804-696">Enable encryption on a new Windows IaaS VM with Azure Managed Disk</span></span>
 <span data-ttu-id="e6804-697">Pomocí šablony Azure ARM disku spravované k vytvoření nové šifrovaný IaaS virtuálního počítače s Linuxem pomocí šablony ARM nacházející se v</span><span class="sxs-lookup"><span data-stu-id="e6804-697">Use the Azure Managed Disk ARM template to create a new encrypted Linux IaaS VM using the ARM template located at</span></span>   
 <span data-ttu-id="e6804-698">[Vytvořit nové šifrovaný IaaS spravované disku virtuální počítač s Windows z Galerie obrázek] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image-managed-disks)</span><span class="sxs-lookup"><span data-stu-id="e6804-698">[Create a new encrypted Windows IaaS Managed Disk VM from gallery image] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image-managed-disks)</span></span>

  > [!NOTE]
  ><span data-ttu-id="e6804-699">Je nutné snímku nebo zálohování spravovaných disků na základě instance virtuálního počítače mimo a před povolením Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="e6804-699">It is mandatory to snapshot and/or backup a managed disk based VM instance outside of and prior to enabling Azure Disk Encryption.</span></span>  <span data-ttu-id="e6804-700">Snímek spravovaného disku můžete provést z portálu, nebo lze použít Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="e6804-700">A snapshot of the managed disk can be taken from the portal, or Azure Backup can be used.</span></span>  <span data-ttu-id="e6804-701">Zálohování zajistěte, aby byla možnost obnovení možné v případě jakékoli neočekávané selhání při šifrování.</span><span class="sxs-lookup"><span data-stu-id="e6804-701">Backups ensure that a recovery option is possible in the case of any unexpected failure during encryption.</span></span>  <span data-ttu-id="e6804-702">Po provedení zálohy rutinu Set-AzureRmVMDiskEncryptionExtension slouží k šifrování spravované disky zadáním parametru - skipVmBackup.</span><span class="sxs-lookup"><span data-stu-id="e6804-702">Once a backup is made, the Set-AzureRmVMDiskEncryptionExtension cmdlet can be used to encrypt managed disks by specifying the -skipVmBackup parameter.</span></span>  <span data-ttu-id="e6804-703">Tento příkaz se nezdaří proti spravovaných disků na základě Virtuálního počítače, dokud zálohu byl změněn a tento parametr byla zadána.</span><span class="sxs-lookup"><span data-stu-id="e6804-703">This command will fail against managed disk based VM's until a backup has been made and this parameter has been specified.</span></span>    
 
### <a name="update-encryption-settings-of-an-existing-encrypted-non-premium-vm"></a><span data-ttu-id="e6804-704">Aktualizovat nastavení šifrování pro existující virtuálního počítače šifrovaná-úrovně premium</span><span class="sxs-lookup"><span data-stu-id="e6804-704">Update encryption settings of an existing encrypted non-premium VM</span></span>
  <span data-ttu-id="e6804-705">Použít existující šifrování podporované rozhraní Azure disk pro spuštění virtuálního počítače [PS rutiny, rozhraní příkazového řádku nebo ARM šablon] aktualizovat nastavení šifrování typu klient AAD ID nebo tajný klíč šifrovacího klíče [KEK] šifrovací klíč nástroje BitLocker pro virtuální počítač s Windows nebo přístupové heslo pro virtuální počítač s Linuxem atd. Nastavení šifrování aktualizace je podporována pouze pro virtuální počítače zajišťoval neprémiové úložiště.</span><span class="sxs-lookup"><span data-stu-id="e6804-705">Use the existing Azure disk encryption supported interfaces for running VM [PS cmdlets, CLI or ARM templates] to update the encryption settings like AAD client ID/secret, Key encryption key [KEK], BitLocker encryption key for Windows VM or Passphrase for Linux VM etc. The update encryption setting is supported only for VMs backed by non-premium storage.</span></span> <span data-ttu-id="e6804-706">Je NNOT podporována u virtuálních počítačů založenou na storage úrovně premium.</span><span class="sxs-lookup"><span data-stu-id="e6804-706">It is NNOT supported for VMs backed by premium storage.</span></span>

## <a name="appendix"></a><span data-ttu-id="e6804-707">Příloha</span><span class="sxs-lookup"><span data-stu-id="e6804-707">Appendix</span></span>
### <a name="connect-to-your-subscription"></a><span data-ttu-id="e6804-708">Připojení k vašemu předplatnému</span><span class="sxs-lookup"><span data-stu-id="e6804-708">Connect to your subscription</span></span>
<span data-ttu-id="e6804-709">Než budete pokračovat, zkontrolujte *požadavky* v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="e6804-709">Before you proceed, review the *Prerequisites* section in this article.</span></span> <span data-ttu-id="e6804-710">Po zajištění, že byly splněny všechny požadavky, připojení k vašemu předplatnému následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e6804-710">After you ensure that all prerequisites have been met, connect to your subscription by doing the following:</span></span>

1. <span data-ttu-id="e6804-711">Spusťte relaci prostředí Azure PowerShell a přihlaste se k účtu Azure pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="e6804-711">Start an Azure PowerShell session, and sign in to your Azure account with the following command:</span></span>

    `Login-AzureRmAccount`

2. <span data-ttu-id="e6804-712">Pokud máte více předplatných a chcete zadat jeden použít, zadejte následující příkaz pro zobrazení předplatných pro váš účet:</span><span class="sxs-lookup"><span data-stu-id="e6804-712">If you have multiple subscriptions and want to specify one to use, type the following to see the subscriptions for your account:</span></span>

    `Get-AzureRmSubscription`

3. <span data-ttu-id="e6804-713">Chcete-li zadat odběr, který chcete použít, zadejte:</span><span class="sxs-lookup"><span data-stu-id="e6804-713">To specify the subscription you want to use, type:</span></span>

    `Select-AzureRmSubscription -SubscriptionName <Yoursubscriptionname>`

4. <span data-ttu-id="e6804-714">Chcete-li ověřit správnost předplatné nakonfigurované, zadejte:</span><span class="sxs-lookup"><span data-stu-id="e6804-714">To verify that the subscription configured is correct, type:</span></span>

    `Get-AzureRmSubscription`

5. <span data-ttu-id="e6804-715">Pokud chcete potvrdit, jestli že jsou nainstalované rutiny Azure Disk Encryption, zadejte:</span><span class="sxs-lookup"><span data-stu-id="e6804-715">To confirm the Azure Disk Encryption cmdlets are installed, type:</span></span>

    `Get-command *diskencryption*`

6. <span data-ttu-id="e6804-716">Tento výstup potvrdí instalace Azure Disk Encryption PowerShell:</span><span class="sxs-lookup"><span data-stu-id="e6804-716">The following output confirms the Azure Disk Encryption PowerShell installation:</span></span>

```
    PS C:\Windows\System32\WindowsPowerShell\v1.0> get-command *diskencryption*
    CommandType  Name                                         Source                                                             
    Cmdlet       Get-AzureRmVMDiskEncryptionStatus            AzureRM.Compute                                                    
    Cmdlet       Disable-AzureRmVMDiskEncryption              AzureRM.Compute                                                    
    Cmdlet       Set-AzureRmVMDiskEncryptionExtension         AzureRM.Compute                                                     
```

### <a name="prepare-a-pre-encrypted-windows-vhd"></a><span data-ttu-id="e6804-717">Příprava předem šifrované virtuálního pevného disku Windows</span><span class="sxs-lookup"><span data-stu-id="e6804-717">Prepare a pre-encrypted Windows VHD</span></span>
<span data-ttu-id="e6804-718">Následující části jsou nezbytné pro přípravu předem šifrované virtuálního pevného disku Windows pro nasazení jako šifrované virtuální pevný disk v Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="e6804-718">The sections that follow are necessary to prepare a pre-encrypted Windows VHD for deployment as an encrypted VHD in Azure IaaS.</span></span> <span data-ttu-id="e6804-719">Použijte informace k přípravě a spustit novou Windows virtuálních počítačů (VHD) na Azure Site Recovery nebo Azure.</span><span class="sxs-lookup"><span data-stu-id="e6804-719">Use the information to prepare and boot a fresh Windows VM (VHD) on Azure Site Recovery or Azure.</span></span>

#### <a name="update-group-policy-to-allow-non-tpm-for-os-protection"></a><span data-ttu-id="e6804-720">Aktualizace zásad skupiny umožňující bez čipu TPM k ochraně operačního systému</span><span class="sxs-lookup"><span data-stu-id="e6804-720">Update group policy to allow non-TPM for OS protection</span></span>
<span data-ttu-id="e6804-721">Konfigurace nastavení zásad skupiny pro BitLocker **nástroj BitLocker Drive Encryption**, které najdete v části **zásady místního počítače** > **konfigurace počítače**  >  **Šablony pro správu** > **součásti systému Windows**.</span><span class="sxs-lookup"><span data-stu-id="e6804-721">Configure the BitLocker Group Policy setting **BitLocker Drive Encryption**, which you'll find under **Local Computer Policy** > **Computer Configuration** > **Administrative Templates** > **Windows Components**.</span></span> <span data-ttu-id="e6804-722">Změňte toto nastavení chcete **jednotky operačního systému** > **vyžadovat další ověření při spuštění** > **povolit nástroj BitLocker bez kompatibilním čipem TPM**, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="e6804-722">Change this setting to **Operating System Drives** > **Require additional authentication at startup** > **Allow BitLocker without a compatible TPM**, as shown in the following figure:</span></span>

![Microsoft Antimalware v Azure](./media/azure-security-disk-encryption/disk-encryption-fig8.png)

#### <a name="install-bitlocker-feature-components"></a><span data-ttu-id="e6804-724">Nainstalujte komponenty funkce nástroje BitLocker</span><span class="sxs-lookup"><span data-stu-id="e6804-724">Install BitLocker feature components</span></span>
<span data-ttu-id="e6804-725">Pro Windows Server 2012 a novější použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e6804-725">For Windows Server 2012 and later, use the following command:</span></span>

    dism /online /Enable-Feature /all /FeatureName:BitLocker /quiet /norestart

<span data-ttu-id="e6804-726">Pro Windows Server 2008 R2 použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e6804-726">For Windows Server 2008 R2, use the following command:</span></span>

    ServerManagerCmd -install BitLockers

#### <a name="prepare-the-os-volume-for-bitlocker-by-using-bdehdcfg"></a><span data-ttu-id="e6804-727">Příprava svazku operačního systému pomocí nástroje BitLocker`bdehdcfg`</span><span class="sxs-lookup"><span data-stu-id="e6804-727">Prepare the OS volume for BitLocker by using `bdehdcfg`</span></span>
<span data-ttu-id="e6804-728">Pokud chcete komprimovat oddílu operačního systému a příprava počítače pro nástroj BitLocker, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="e6804-728">To compress the OS partition and prepare the machine for BitLocker, execute the following command:</span></span>

    bdehdcfg -target c: shrink -quiet

#### <a name="protect-the-os-volume-by-using-bitlocker"></a><span data-ttu-id="e6804-729">Ochrana svazku operačního systému pomocí nástroje BitLocker</span><span class="sxs-lookup"><span data-stu-id="e6804-729">Protect the OS volume by using BitLocker</span></span>
<span data-ttu-id="e6804-730">Použití [ `manage-bde` ](https://technet.microsoft.com/library/ff829849.aspx) příkaz Povolit šifrování na spouštěcím svazku pomocí externí ochranné zařízení klíče.</span><span class="sxs-lookup"><span data-stu-id="e6804-730">Use the [`manage-bde`](https://technet.microsoft.com/library/ff829849.aspx) command to enable encryption on the boot volume using an external key protector.</span></span> <span data-ttu-id="e6804-731">Také umístíte externí klíč (soubor .bek) na externím disku nebo svazku.</span><span class="sxs-lookup"><span data-stu-id="e6804-731">Also place the external key (.bek file) on the external drive or volume.</span></span> <span data-ttu-id="e6804-732">Po dalším restartování je povolené šifrování v systému nebo spouštěcí svazek.</span><span class="sxs-lookup"><span data-stu-id="e6804-732">Encryption is enabled on the system/boot volume after the next reboot.</span></span>

    manage-bde -on %systemdrive% -sk [ExternalDriveOrVolume]
    reboot

> [!NOTE]
> <span data-ttu-id="e6804-733">Příprava virtuálního počítače s samostatné dat nebo prostředků virtuálního pevného disku pro získání externí klíče pomocí nástroje BitLocker.</span><span class="sxs-lookup"><span data-stu-id="e6804-733">Prepare the VM with a separate data/resource VHD for getting the external key by using BitLocker.</span></span>

#### <a name="encrypting-an-os-drive-on-a-running-linux-vm"></a><span data-ttu-id="e6804-734">Šifrování jednotce operačního systému na spuštěný virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="e6804-734">Encrypting an OS drive on a running Linux VM</span></span>
<span data-ttu-id="e6804-735">Šifrování jednotky operačního systému na spuštěný virtuální počítač Linux je podporována v následujících rozdělení:</span><span class="sxs-lookup"><span data-stu-id="e6804-735">Encryption of an OS drive on a running Linux VM is supported on the following distributions:</span></span>

* <span data-ttu-id="e6804-736">RHEL 7.2</span><span class="sxs-lookup"><span data-stu-id="e6804-736">RHEL 7.2</span></span>
* <span data-ttu-id="e6804-737">CentOS 7.2</span><span class="sxs-lookup"><span data-stu-id="e6804-737">CentOS 7.2</span></span>
* <span data-ttu-id="e6804-738">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="e6804-738">Ubuntu 16.04</span></span>

##### <a name="prerequisites-for-os-disk-encryption"></a><span data-ttu-id="e6804-739">Požadavky pro šifrování disku operačního systému</span><span class="sxs-lookup"><span data-stu-id="e6804-739">Prerequisites for OS disk encryption</span></span>

* <span data-ttu-id="e6804-740">Virtuální počítač musí být vytvořený z Marketplace image ve službě Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="e6804-740">The VM must be created from the Marketplace image in Azure Resource Manager.</span></span>
* <span data-ttu-id="e6804-741">Virtuální počítač Azure s minimálně 4 GB paměti RAM (doporučená velikost je 7 GB).</span><span class="sxs-lookup"><span data-stu-id="e6804-741">Azure VM with at least 4 GB of RAM (recommended size is 7 GB).</span></span>
* <span data-ttu-id="e6804-742">(Pro RHEL a CentOS) Zakážete SELinux.</span><span class="sxs-lookup"><span data-stu-id="e6804-742">(For RHEL and CentOS) Disable SELinux.</span></span> <span data-ttu-id="e6804-743">Pokud chcete zakázat SELinux, najdete v části "4.4.2.</span><span class="sxs-lookup"><span data-stu-id="e6804-743">To disable SELinux, see "4.4.2.</span></span> <span data-ttu-id="e6804-744">Zakázání SELinux"v [Průvodce SELinux uživatele a správce](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="e6804-744">Disabling SELinux" in the [SELinux User's and Administrator's Guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) on the VM.</span></span>
* <span data-ttu-id="e6804-745">Po zakázání SELinux restartujte virtuální počítač alespoň jednou.</span><span class="sxs-lookup"><span data-stu-id="e6804-745">After you disable SELinux, reboot the VM at least once.</span></span>

##### <a name="steps"></a><span data-ttu-id="e6804-746">Kroky</span><span class="sxs-lookup"><span data-stu-id="e6804-746">Steps</span></span>
1. <span data-ttu-id="e6804-747">Vytvoření virtuálního počítače pomocí jednoho z rozdělení zadali dřív.</span><span class="sxs-lookup"><span data-stu-id="e6804-747">Create a VM by using one of the distributions specified previously.</span></span>

 <span data-ttu-id="e6804-748">7.2 CentOS je podporováno šifrování disku operačního systému přes speciální bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="e6804-748">For CentOS 7.2, OS disk encryption is supported via a special image.</span></span> <span data-ttu-id="e6804-749">Chcete-li použít tuto bitovou kopii, zadejte "7.2n" jako verze SKU při vytváření virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="e6804-749">To use this image, specify "7.2n" as the SKU when you create the VM:</span></span>
 ```
    Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName "OpenLogic" -Offer "CentOS" -Skus "7.2n" -Version "latest"
 ```
2. <span data-ttu-id="e6804-750">Konfigurace virtuálního počítače podle svých potřeb.</span><span class="sxs-lookup"><span data-stu-id="e6804-750">Configure the VM according to your needs.</span></span> <span data-ttu-id="e6804-751">Pokud budete k šifrování všech (operační systém + data) disky, musí být zadané a připojit z /etc/fstab datové jednotky.</span><span class="sxs-lookup"><span data-stu-id="e6804-751">If you are going to encrypt all the (OS + data) drives, the data drives need to be specified and mountable from /etc/fstab.</span></span>

 > [!NOTE]
 > <span data-ttu-id="e6804-752">Použít UUID =... k určení datových jednotkách v fstab/etc/místo zadání názvu zařízení blok (například/dev/sdb1).</span><span class="sxs-lookup"><span data-stu-id="e6804-752">Use UUID=... to specify data drives in /etc/fstab instead of specifying the block device name (for example, /dev/sdb1).</span></span> <span data-ttu-id="e6804-753">Během šifrování se změní pořadí jednotky ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="e6804-753">During encryption, the order of drives changes on the VM.</span></span> <span data-ttu-id="e6804-754">Pokud virtuálního počítače závisí na konkrétní pořadí blokovat zařízení, se nezdaří připojit je po dokončení šifrování.</span><span class="sxs-lookup"><span data-stu-id="e6804-754">If your VM relies on a specific order of block devices, it will fail to mount them after encryption.</span></span>

3. <span data-ttu-id="e6804-755">Odhlásit z relace SSH.</span><span class="sxs-lookup"><span data-stu-id="e6804-755">Sign out of the SSH sessions.</span></span>

4. <span data-ttu-id="e6804-756">K šifrování operačního systému, zadejte volumeType jako **všechny** nebo **OS** když jste [povolit šifrování](#enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure).</span><span class="sxs-lookup"><span data-stu-id="e6804-756">To encrypt the OS, specify volumeType as **All** or **OS** when you [enable encryption](#enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure).</span></span>

 > [!NOTE]
 > <span data-ttu-id="e6804-757">Všechny procesy uživatelského prostoru, které nejsou spuštěny jako `systemd` služby by měl být ukončen s `SIGKILL`.</span><span class="sxs-lookup"><span data-stu-id="e6804-757">All user-space processes that are not running as `systemd` services should be killed with a `SIGKILL`.</span></span> <span data-ttu-id="e6804-758">Restartování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e6804-758">Reboot the VM.</span></span> <span data-ttu-id="e6804-759">Pokud povolíte šifrování disku operačního systému na spuštěný virtuální počítač, naplánujte na výpadek virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e6804-759">When you enable OS disk encryption on a running VM, plan on VM downtime.</span></span>

5. <span data-ttu-id="e6804-760">Pravidelně sledovat průběh šifrování pomocí pokynů v [další části](#monitoring-os-encryption-progress).</span><span class="sxs-lookup"><span data-stu-id="e6804-760">Periodically monitor the progress of encryption by using the instructions in the [next section](#monitoring-os-encryption-progress).</span></span>

6. <span data-ttu-id="e6804-761">Po Get-AzureRmVmDiskEncryptionStatus zobrazuje "VMRestartPending", restartujte virtuální počítač po přihlášení k němu nebo pomocí portálu, prostředí PowerShell nebo rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="e6804-761">After Get-AzureRmVmDiskEncryptionStatus shows "VMRestartPending," restart your VM either by signing in to it or by using the portal, PowerShell, or CLI.</span></span>
    ```
    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : VMRestartPending
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk successfully encrypted, reboot the VM
    ```
<span data-ttu-id="e6804-762">Než restartujete, doporučujeme, abyste uložili [spouštění diagnostiky](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/) virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e6804-762">Before you reboot, we recommend that you save [boot diagnostics](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/) of the VM.</span></span>

#### <a name="monitoring-os-encryption-progress"></a><span data-ttu-id="e6804-763">Sledování průběhu šifrování operačního systému</span><span class="sxs-lookup"><span data-stu-id="e6804-763">Monitoring OS encryption progress</span></span>
<span data-ttu-id="e6804-764">Můžete sledovat průběh šifrování OS třemi způsoby:</span><span class="sxs-lookup"><span data-stu-id="e6804-764">You can monitor OS encryption progress in three ways:</span></span>

* <span data-ttu-id="e6804-765">Použití `Get-AzureRmVmDiskEncryptionStatus` rutiny a zkontrolovat ProgressMessage pole:</span><span class="sxs-lookup"><span data-stu-id="e6804-765">Use the `Get-AzureRmVmDiskEncryptionStatus` cmdlet and inspect the ProgressMessage field:</span></span>
    ```
    OsVolumeEncrypted          : EncryptionInProgress
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk encryption started
    ```
 <span data-ttu-id="e6804-766">Po virtuálního počítače dosáhne "Spuštění šifrování disku operačního systému", jak dlouho trvá asi 40 až 50 minut na storage úrovně Premium zálohovaný virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="e6804-766">After the VM reaches "OS disk encryption started," it takes about 40 to 50 minutes on a Premium-storage backed VM.</span></span>

 <span data-ttu-id="e6804-767">Z důvodu [vydání #388](https://github.com/Azure/WALinuxAgent/issues/388) v WALinuxAgent, `OsVolumeEncrypted` a `DataVolumesEncrypted` zobrazují jako `Unknown` v některých distribucích.</span><span class="sxs-lookup"><span data-stu-id="e6804-767">Because of [issue #388](https://github.com/Azure/WALinuxAgent/issues/388) in WALinuxAgent, `OsVolumeEncrypted` and `DataVolumesEncrypted` show up as `Unknown` in some distributions.</span></span> <span data-ttu-id="e6804-768">S WALinuxAgent verze 2.1.5 a novější, se tento problém vyřešen automaticky.</span><span class="sxs-lookup"><span data-stu-id="e6804-768">With WALinuxAgent version 2.1.5 and later, this issue is fixed automatically.</span></span> <span data-ttu-id="e6804-769">Pokud se zobrazí `Unknown` ve výstupu, můžete ověřit stav šifrování disku pomocí Průzkumníka prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="e6804-769">If you see `Unknown` in the output, you can verify disk-encryption status by using the Azure Resource Explorer.</span></span>

 <span data-ttu-id="e6804-770">Přejděte na [Průzkumníka prostředků Azure](https://resources.azure.com/)a potom rozbalte tuto hierarchii panelu výběr na levé straně:</span><span class="sxs-lookup"><span data-stu-id="e6804-770">Go to [Azure Resource Explorer](https://resources.azure.com/), and then expand this hierarchy in the selection panel on left:</span></span>

 ~~~~
 |-- subscriptions
     |-- [Your subscription]
          |-- resourceGroups
               |-- [Your resource group]
                    |-- providers
                         |-- Microsoft.Compute
                              |-- virtualMachines
                                   |-- [Your virtual machine]
                                        |-- InstanceView
~~~~                

 <span data-ttu-id="e6804-771">V InstanceView posuňte se dolů a zjistit stav šifrování jednotky.</span><span class="sxs-lookup"><span data-stu-id="e6804-771">In the InstanceView, scroll down to see the encryption status of your drives.</span></span>

 ![Zobrazení Instance virtuálního počítače](./media/azure-security-disk-encryption/vm-instanceview.png)

* <span data-ttu-id="e6804-773">Podívejte se na [spouštění diagnostiky](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/).</span><span class="sxs-lookup"><span data-stu-id="e6804-773">Look at [boot diagnostics](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/).</span></span> <span data-ttu-id="e6804-774">Zprávy z rozšíření ADE by měla obsahovat předponu s `[AzureDiskEncryption]`.</span><span class="sxs-lookup"><span data-stu-id="e6804-774">Messages from the ADE extension should be prefixed with `[AzureDiskEncryption]`.</span></span>

* <span data-ttu-id="e6804-775">Přihlaste se k virtuálnímu počítači pomocí protokolu SSH a získat rozšíření protokolu z:</span><span class="sxs-lookup"><span data-stu-id="e6804-775">Sign in to the VM via SSH, and get the extension log from:</span></span>

    <span data-ttu-id="e6804-776">/var/log/Azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux</span><span class="sxs-lookup"><span data-stu-id="e6804-776">/var/log/azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux</span></span>

 <span data-ttu-id="e6804-777">Doporučujeme vám, že není přihlášení k virtuálnímu počítači v průběhu šifrování operačního systému.</span><span class="sxs-lookup"><span data-stu-id="e6804-777">We recommend that you do not sign in to the VM while OS encryption is in progress.</span></span> <span data-ttu-id="e6804-778">Zkopírujte protokoly jenom v případě, že tyto dvě metody se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="e6804-778">Copy the logs only when the other two methods have failed.</span></span>

#### <a name="prepare-a-pre-encrypted-linux-vhd"></a><span data-ttu-id="e6804-779">Příprava předem šifrované Linux virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="e6804-779">Prepare a pre-encrypted Linux VHD</span></span>
##### <a name="ubuntu-16"></a><span data-ttu-id="e6804-780">Ubuntu 16</span><span class="sxs-lookup"><span data-stu-id="e6804-780">Ubuntu 16</span></span>
<span data-ttu-id="e6804-781">Konfigurace šifrování během instalace distribučního následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e6804-781">Configure encryption during the distribution installation by doing the following:</span></span>

1. <span data-ttu-id="e6804-782">Vyberte **konfigurovat šifrované svazky** při vytvoření diskových oddílů.</span><span class="sxs-lookup"><span data-stu-id="e6804-782">Select **Configure encrypted volumes** when you partition the disks.</span></span>

 ![Instalační program Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig1.png)

2. <span data-ttu-id="e6804-784">Vytvořte samostatný spouštěcí jednotka, která nesmí být zašifrovaná.</span><span class="sxs-lookup"><span data-stu-id="e6804-784">Create a separate boot drive, which must not be encrypted.</span></span> <span data-ttu-id="e6804-785">Šifrování kořenové jednotce.</span><span class="sxs-lookup"><span data-stu-id="e6804-785">Encrypt your root drive.</span></span>

 ![Instalační program Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig2.png)

3. <span data-ttu-id="e6804-787">Zadejte přístupové heslo.</span><span class="sxs-lookup"><span data-stu-id="e6804-787">Provide a passphrase.</span></span> <span data-ttu-id="e6804-788">Toto je heslo, které nahrajte do trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-788">This is the passphrase that you upload to the key vault.</span></span>

 ![Instalační program Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig3.png)

4. <span data-ttu-id="e6804-790">Dokončete vytváření oddílů.</span><span class="sxs-lookup"><span data-stu-id="e6804-790">Finish partitioning.</span></span>

 ![Instalační program Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig4.png)

5. <span data-ttu-id="e6804-792">Když spustíte virtuální počítač a vyzváni k přístupové heslo, použijte přístupové heslo, které jste zadali v kroku 3.</span><span class="sxs-lookup"><span data-stu-id="e6804-792">When you boot the VM and are asked for a passphrase, use the passphrase you provided in step 3.</span></span>

 ![Instalační program Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig5.png)

6. <span data-ttu-id="e6804-794">Příprava virtuálního počítače pro odesílání do Azure pomocí [tyto pokyny](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="e6804-794">Prepare the VM for uploading into Azure using [these instructions](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-ubuntu/).</span></span> <span data-ttu-id="e6804-795">Nespouštějte poslední krok (zrušení zřízení virtuálního počítače) ještě.</span><span class="sxs-lookup"><span data-stu-id="e6804-795">Do not run the last step (deprovisioning the VM) yet.</span></span>

<span data-ttu-id="e6804-796">Konfigurace šifrování pro práci s Azure následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="e6804-796">Configure encryption to work with Azure by doing the following:</span></span>

1. <span data-ttu-id="e6804-797">Vytvořte soubor pod /usr/local/sbin/azure_crypt_key.sh, s obsahem v následující skript.</span><span class="sxs-lookup"><span data-stu-id="e6804-797">Create a file under /usr/local/sbin/azure_crypt_key.sh, with the content in the following script.</span></span> <span data-ttu-id="e6804-798">Věnujte pozornost KeyFileName, protože je název souboru přístupové heslo používané Azure.</span><span class="sxs-lookup"><span data-stu-id="e6804-798">Pay attention to the KeyFileName, because it is the passphrase file name used by Azure.</span></span>

    ```
    #!/bin/sh
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint
    modprobe vfat >/dev/null 2>&1
    modprobe ntfs >/dev/null 2>&1
    sleep 2
    OPENED=0
    cd /sys/block
    for DEV in sd*; do

        echo "> Trying device: $DEV ..." >&2
        mount -t vfat -r /dev/${DEV}1 $MountPoint >/dev/null||
        mount -t ntfs -r /dev/${DEV}1 $MountPoint >/dev/null
        if [ -f $MountPoint/$KeyFileName ]; then
                cat $MountPoint/$KeyFileName
                umount $MountPoint 2>/dev/null
                OPENED=1
                break
        fi
        umount $MountPoint 2>/dev/null
    done

      if [ $OPENED -eq 0 ]; then
        echo "FAILED to find suitable passphrase file ..." >&2
        echo -n "Try to enter your password: " >&2
        read -s -r A </dev/console
        echo -n "$A"
     else
        echo "Success loading keyfile!" >&2
    fi
```

2. <span data-ttu-id="e6804-799">Změna konfigurace crypt v */etc/crypttab*.</span><span class="sxs-lookup"><span data-stu-id="e6804-799">Change the crypt config in */etc/crypttab*.</span></span> <span data-ttu-id="e6804-800">Mělo by to vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="e6804-800">It should look like this:</span></span>
 ```
    xxx_crypt uuid=xxxxxxxxxxxxxxxxxxxxx none luks,discard,keyscript=/usr/local/sbin/azure_crypt_key.sh
    ```

3. <span data-ttu-id="e6804-801">Pokud upravujete *azure_crypt_key.sh* v systému Windows a můžete zkopírovat do systému Linux, spusťte `dos2unix /usr/local/sbin/azure_crypt_key.sh`.</span><span class="sxs-lookup"><span data-stu-id="e6804-801">If you are editing *azure_crypt_key.sh* in Windows and you copied it to Linux, run `dos2unix /usr/local/sbin/azure_crypt_key.sh`.</span></span>

4. <span data-ttu-id="e6804-802">Přidejte oprávnění spustitelný soubor skriptu:</span><span class="sxs-lookup"><span data-stu-id="e6804-802">Add executable permissions to the script:</span></span>
 ```
    chmod +x /usr/local/sbin/azure_crypt_key.sh
 ```
5. <span data-ttu-id="e6804-803">Upravit */etc/initramfs-tools/modules* připojením řádky:</span><span class="sxs-lookup"><span data-stu-id="e6804-803">Edit */etc/initramfs-tools/modules* by appending lines:</span></span>
 ```
    vfat
    ntfs
    nls_cp437
    nls_utf8
    nls_iso8859-1
```
6. <span data-ttu-id="e6804-804">Spustit `update-initramfs -u -k all` aktualizace initramfs, aby `keyscript` vstoupila v platnost.</span><span class="sxs-lookup"><span data-stu-id="e6804-804">Run `update-initramfs -u -k all` to update the initramfs to make the `keyscript` take effect.</span></span>

7. <span data-ttu-id="e6804-805">Nyní můžete zrušení zřízení virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="e6804-805">Now you can deprovision the VM.</span></span>

 ![Instalační program Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig6.png)

8. <span data-ttu-id="e6804-807">Pokračovat k dalšímu kroku a [nahrát svůj disk VHD](#upload-encrypted-vhd-to-an-azure-storage-account) do Azure.</span><span class="sxs-lookup"><span data-stu-id="e6804-807">Continue to the next step and [upload your VHD](#upload-encrypted-vhd-to-an-azure-storage-account) into Azure.</span></span>

##### <a name="opensuse-132"></a><span data-ttu-id="e6804-808">openSUSE 13.2</span><span class="sxs-lookup"><span data-stu-id="e6804-808">openSUSE 13.2</span></span>
<span data-ttu-id="e6804-809">Pokud chcete konfigurovat šifrování během instalace distribučního, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e6804-809">To configure encryption during the distribution installation, do the following:</span></span>
1. <span data-ttu-id="e6804-810">Při vytvoření diskových oddílů, vyberte **šifrování svazku skupiny**a pak zadejte heslo.</span><span class="sxs-lookup"><span data-stu-id="e6804-810">When you partition the disks, select **Encrypt Volume Group**, and then enter a password.</span></span> <span data-ttu-id="e6804-811">Toto je heslo, které odešlete do trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-811">This is the password that you will upload to your key vault.</span></span>

 ![Instalační program openSUSE 13.2](./media/azure-security-disk-encryption/opensuse-encrypt-fig1.png)

2. <span data-ttu-id="e6804-813">Spuštění virtuálního počítače pomocí hesla.</span><span class="sxs-lookup"><span data-stu-id="e6804-813">Boot the VM using your password.</span></span>

 ![Instalační program openSUSE 13.2](./media/azure-security-disk-encryption/opensuse-encrypt-fig2.png)

3. <span data-ttu-id="e6804-815">Příprava virtuálního počítače chcete nahrát do Azure podle pokynů v [Příprava virtuálního počítače, SLES nebo openSUSE pro Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131).</span><span class="sxs-lookup"><span data-stu-id="e6804-815">Prepare the VM for uploading to Azure by following the instructions in [Prepare a SLES or openSUSE virtual machine for Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131).</span></span> <span data-ttu-id="e6804-816">Nespouštějte poslední krok (zrušení zřízení virtuálního počítače) ještě.</span><span class="sxs-lookup"><span data-stu-id="e6804-816">Do not run the last step (deprovisioning the VM) yet.</span></span>

<span data-ttu-id="e6804-817">Konfigurace šifrování pro práci s Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e6804-817">To configure encryption to work with Azure, do the following:</span></span>
1. <span data-ttu-id="e6804-818">Upravit /etc/dracut.conf a přidejte následující řádek:</span><span class="sxs-lookup"><span data-stu-id="e6804-818">Edit the /etc/dracut.conf, and add the following line:</span></span>
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```
2. <span data-ttu-id="e6804-819">Komentář tyto řádky na konci souboru /usr/lib/dracut/modules.d/90crypt/module-setup.sh:</span><span class="sxs-lookup"><span data-stu-id="e6804-819">Comment out these lines by the end of the file /usr/lib/dracut/modules.d/90crypt/module-setup.sh:</span></span>
 ```
    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator
 ```

3. <span data-ttu-id="e6804-820">Připojte následující řádek na začátek souboru /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh:</span><span class="sxs-lookup"><span data-stu-id="e6804-820">Append the following line at the beginning of the file /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh:</span></span>
 ```
    DRACUT_SYSTEMD=0
 ```
<span data-ttu-id="e6804-821">A změňte všechny výskyty:</span><span class="sxs-lookup"><span data-stu-id="e6804-821">And change all occurrences of:</span></span>
 ```
    if [ -z "$DRACUT_SYSTEMD" ]; then
 ```
<span data-ttu-id="e6804-822">na:</span><span class="sxs-lookup"><span data-stu-id="e6804-822">to:</span></span>
```
    if [ 1 ]; then
```
4. <span data-ttu-id="e6804-823">Upravit /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh a připojit k "# otevřete LUKS zařízení":</span><span class="sxs-lookup"><span data-stu-id="e6804-823">Edit /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh and append it to “# Open LUKS device”:</span></span>

    ```
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done
    ```
5. <span data-ttu-id="e6804-824">Spustit `/usr/sbin/dracut -f -v` aktualizovat initrd.</span><span class="sxs-lookup"><span data-stu-id="e6804-824">Run `/usr/sbin/dracut -f -v` to update the initrd.</span></span>

6. <span data-ttu-id="e6804-825">Teď můžete zrušení zřízení virtuálního počítače a [nahrát svůj disk VHD](#upload-encrypted-vhd-to-an-azure-storage-account) do Azure.</span><span class="sxs-lookup"><span data-stu-id="e6804-825">Now you can deprovision the VM and [upload your VHD](#upload-encrypted-vhd-to-an-azure-storage-account) into Azure.</span></span>

##### <a name="centos-7"></a><span data-ttu-id="e6804-826">CentOS 7</span><span class="sxs-lookup"><span data-stu-id="e6804-826">CentOS 7</span></span>
<span data-ttu-id="e6804-827">Pokud chcete konfigurovat šifrování během instalace distribučního, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e6804-827">To configure encryption during the distribution installation, do the following:</span></span>
1. <span data-ttu-id="e6804-828">Vyberte **šifrovat data** při oddílu disky.</span><span class="sxs-lookup"><span data-stu-id="e6804-828">Select **Encrypt my data** when you partition disks.</span></span>

 ![Instalační program centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig1.png)

2. <span data-ttu-id="e6804-830">Zajistěte, aby **šifrovat** je vybraná pro kořenový oddíl.</span><span class="sxs-lookup"><span data-stu-id="e6804-830">Make sure **Encrypt** is selected for root partition.</span></span>

 ![Instalační program centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig2.png)

3. <span data-ttu-id="e6804-832">Zadejte přístupové heslo.</span><span class="sxs-lookup"><span data-stu-id="e6804-832">Provide a passphrase.</span></span> <span data-ttu-id="e6804-833">Toto je heslo, které odešlete do trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-833">This is the passphrase that you will upload to your key vault.</span></span>

 ![Instalační program centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig3.png)

4. <span data-ttu-id="e6804-835">Když spustíte virtuální počítač a vyzváni k přístupové heslo, použijte přístupové heslo, které jste zadali v kroku 3.</span><span class="sxs-lookup"><span data-stu-id="e6804-835">When you boot the VM and are asked for a passphrase, use the passphrase you provided in step 3.</span></span>

 ![Instalační program centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig4.png)

5. <span data-ttu-id="e6804-837">Příprava virtuálního počítače pro odesílání do Azure podle pokynů "CentOS 7.0 +" v [Příprava virtuálního počítače, na základě CentOS pro Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70).</span><span class="sxs-lookup"><span data-stu-id="e6804-837">Prepare the VM for uploading into Azure by using the "CentOS 7.0+" instructions in [Prepare a CentOS-based virtual machine for Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70).</span></span> <span data-ttu-id="e6804-838">Nespouštějte poslední krok (zrušení zřízení virtuálního počítače) ještě.</span><span class="sxs-lookup"><span data-stu-id="e6804-838">Do not run the last step (deprovisioning the VM) yet.</span></span>

6. <span data-ttu-id="e6804-839">Teď můžete zrušení zřízení virtuálního počítače a [nahrát svůj disk VHD](#upload-encrypted-vhd-to-an-azure-storage-account) do Azure.</span><span class="sxs-lookup"><span data-stu-id="e6804-839">Now you can deprovision the VM and [upload your VHD](#upload-encrypted-vhd-to-an-azure-storage-account) into Azure.</span></span>

<span data-ttu-id="e6804-840">Konfigurace šifrování pro práci s Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="e6804-840">To configure encryption to work with Azure, do the following:</span></span>

1. <span data-ttu-id="e6804-841">Upravit /etc/dracut.conf a přidejte následující řádek:</span><span class="sxs-lookup"><span data-stu-id="e6804-841">Edit the /etc/dracut.conf, and add the following line:</span></span>
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```

2. <span data-ttu-id="e6804-842">Komentář tyto řádky na konci souboru /usr/lib/dracut/modules.d/90crypt/module-setup.sh:</span><span class="sxs-lookup"><span data-stu-id="e6804-842">Comment out these lines by the end of the file /usr/lib/dracut/modules.d/90crypt/module-setup.sh:</span></span>
```
    #        inst_multiple -o \
    #        $systemdutildir/system-generators/systemd-cryptsetup-generator \
    #        $systemdutildir/systemd-cryptsetup \
    #        $systemdsystemunitdir/systemd-ask-password-console.path \
    #        $systemdsystemunitdir/systemd-ask-password-console.service \
    #        $systemdsystemunitdir/cryptsetup.target \
    #        $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    #        systemd-ask-password systemd-tty-ask-password-agent
    #        inst_script "$moddir"/crypt-run-generator.sh /sbin/crypt-run-generator
```

3. <span data-ttu-id="e6804-843">Připojte následující řádek na začátek souboru /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh:</span><span class="sxs-lookup"><span data-stu-id="e6804-843">Append the following line at the beginning of the file /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh:</span></span>
```
    DRACUT_SYSTEMD=0
```
<span data-ttu-id="e6804-844">A změňte všechny výskyty:</span><span class="sxs-lookup"><span data-stu-id="e6804-844">And change all occurrences of:</span></span>
```
    if [ -z "$DRACUT_SYSTEMD" ]; then
```
<span data-ttu-id="e6804-845">na</span><span class="sxs-lookup"><span data-stu-id="e6804-845">to</span></span>
```
    if [ 1 ]; then
```
4. <span data-ttu-id="e6804-846">Upravit /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh a připojit toto po "# otevřete LUKS zařízení":</span><span class="sxs-lookup"><span data-stu-id="e6804-846">Edit /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh and append this after the “# Open LUKS device”:</span></span>
    ```
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying to get the key from disks ..." >&2
    mkdir -p $MountPoint >&2
    modprobe vfat >/dev/null >&2
    modprobe ntfs >/dev/null >&2
    for SFS in /dev/sd*; do
    echo "> Trying device:$SFS..." >&2
    mount ${SFS}1 $MountPoint -t vfat -r >&2 ||
    mount ${SFS}1 $MountPoint -t ntfs -r >&2
    if [ -f $MountPoint/$KeyFileName ]; then
        echo "> keyfile got..." >&2
        cp $MountPoint/$KeyFileName /tmp-keyfile >&2
        luksfile=/tmp-keyfile
        umount $MountPoint >&2
        break
    fi
    done
    ```    
5. <span data-ttu-id="e6804-847">Spustit "/ usr/sbin/dracut - f - v" aktualizovat initrd.</span><span class="sxs-lookup"><span data-stu-id="e6804-847">Run the “/usr/sbin/dracut -f -v” to update the initrd.</span></span>

![Instalační program centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig5.png)

### <a name="upload-encrypted-vhd-to-an-azure-storage-account"></a><span data-ttu-id="e6804-849">Nahrát šifrované virtuálního pevného disku k účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="e6804-849">Upload encrypted VHD to an Azure storage account</span></span>
<span data-ttu-id="e6804-850">Povolíte šifrování nástrojem BitLocker nebo DM-Crypt šifrování, místní šifrované virtuálního pevného disku musí být nahrán do účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="e6804-850">After BitLocker encryption or DM-Crypt encryption is enabled, the local encrypted VHD needs to be uploaded to your storage account.</span></span>

    Add-AzureRmVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>]

### <a name="upload-the-disk-encryption-secret-for-the-pre-encrypted-vm-to-your-key-vault"></a><span data-ttu-id="e6804-851">Nahrát tajný klíč šifrování disku předem šifrované virtuálního počítače k trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="e6804-851">Upload the disk-encryption secret for the pre-encrypted VM to your key vault</span></span>
<span data-ttu-id="e6804-852">Šifrování disku tajný klíč, který jste získali dříve musí být odeslán jako tajný klíč v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-852">The disk-encryption secret that you obtained previously must be uploaded as a secret in your key vault.</span></span> <span data-ttu-id="e6804-853">Trezor klíčů musí mít oprávnění povoleno pro vašeho klienta Azure AD a šifrování disku.</span><span class="sxs-lookup"><span data-stu-id="e6804-853">The key vault needs to have disk encryption and permissions enabled for your Azure AD client.</span></span>

    $AadClientId = "YourAADClientId"
    $AadClientSecret = "YourAADClientSecret"

    $key vault = New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -Location $Location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -ServicePrincipalName $AadClientId -PermissionsToKeys all -PermissionsToSecrets all
    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -EnabledForDiskEncryption


#### <a name="disk-encryption-secret-not-encrypted-with-a-kek"></a><span data-ttu-id="e6804-854">Disk encryption tajný klíč není šifrován KEK</span><span class="sxs-lookup"><span data-stu-id="e6804-854">Disk encryption secret not encrypted with a KEK</span></span>
<span data-ttu-id="e6804-855">Chcete-li nastavit tajný klíč v trezoru klíčů, použijte [Set-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret).</span><span class="sxs-lookup"><span data-stu-id="e6804-855">To set up the secret in your key vault, use [Set-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret).</span></span> <span data-ttu-id="e6804-856">Pokud máte virtuální počítače s Windows, je soubor bek kódovaný jako base64 řetězec a pak odeslat k trezoru klíčů pomocí `Set-AzureKeyVaultSecret` rutiny.</span><span class="sxs-lookup"><span data-stu-id="e6804-856">If you have a Windows virtual machine, the bek file is encoded as a base64 string and then uploaded to your key vault using the `Set-AzureKeyVaultSecret` cmdlet.</span></span> <span data-ttu-id="e6804-857">Pro Linux je heslo kódovaný jako base64 řetězec a pak odeslat do trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="e6804-857">For Linux, the passphrase is encoded as a base64 string and then uploaded to the key vault.</span></span> <span data-ttu-id="e6804-858">Kromě toho se ujistěte, že jsou při vytváření tajný klíč v trezoru klíčů nastavte následující značky.</span><span class="sxs-lookup"><span data-stu-id="e6804-858">In addition, make sure that the following tags are set when you create the secret in the key vault.</span></span>

    # This is the passphrase that was provided for encryption during the distribution installation
    $passphrase = "contoso-password"

    $tags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $secretName = [guid]::NewGuid().ToString()
    $secretValue = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($passphrase))
    $secureSecretValue = ConvertTo-SecureString $secretValue -AsPlainText -Force

    $secret = Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $secretName -SecretValue $secureSecretValue -tags $tags
    $secretUrl = $secret.Id

<span data-ttu-id="e6804-859">Použití `$secretUrl` v dalším kroku pro [připojení disk operačního systému bez použití KEK](#without-using-a-kek).</span><span class="sxs-lookup"><span data-stu-id="e6804-859">Use the `$secretUrl` in the next step for [attaching the OS disk without using KEK](#without-using-a-kek).</span></span>

#### <a name="disk-encryption-secret-encrypted-with-a-kek"></a><span data-ttu-id="e6804-860">Tajný klíč šifrování disku šifrován KEK</span><span class="sxs-lookup"><span data-stu-id="e6804-860">Disk encryption secret encrypted with a KEK</span></span>
<span data-ttu-id="e6804-861">Před nahráním tajný klíč do trezoru klíčů, můžete volitelně šifrováním pomocí klíče šifrovací klíč.</span><span class="sxs-lookup"><span data-stu-id="e6804-861">Before you upload the secret to the key vault, you can optionally encrypt it by using a key encryption key.</span></span> <span data-ttu-id="e6804-862">Použít obtékání [rozhraní API](https://msdn.microsoft.com/library/azure/dn878066.aspx) k šifrování nejprve tajného klíče pomocí klíče šifrovacího klíče.</span><span class="sxs-lookup"><span data-stu-id="e6804-862">Use the wrap [API](https://msdn.microsoft.com/library/azure/dn878066.aspx) to first encrypt the secret using the key encryption key.</span></span> <span data-ttu-id="e6804-863">Výstup této operace zalamování je řetězec ve formátu base64 kódovaná adresou URL, které potom můžete nahrát jako tajného klíče pomocí [ `Set-AzureKeyVaultSecret` ](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret) rutiny.</span><span class="sxs-lookup"><span data-stu-id="e6804-863">The output of this wrap operation is a base64 URL encoded string, which you can then upload as a secret by using the [`Set-AzureKeyVaultSecret`](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret) cmdlet.</span></span>

    # This is the passphrase that was provided for encryption during the distribution installation
    $passphrase = "contoso-password"

    Add-AzureKeyVaultKey -VaultName $KeyVaultName -Name "keyencryptionkey" -Destination Software
    $KeyEncryptionKey = Get-AzureKeyVaultKey -VaultName $KeyVault.OriginalVault.Name -Name "keyencryptionkey"

    $apiversion = "2015-06-01"

    ##############################
    # Get Auth URI
    ##############################

    $uri = $KeyVault.VaultUri + "/keys"
    $headers = @{}

    $response = try { Invoke-RestMethod -Method GET -Uri $uri -Headers $headers } catch { $_.Exception.Response }

    $authHeader = $response.Headers["www-authenticate"]
    $authUri = [regex]::match($authHeader, 'authorization="(.*?)"').Groups[1].Value

    Write-Host "Got Auth URI successfully"

    ##############################
    # Get Auth Token
    ##############################

    $uri = $authUri + "/oauth2/token"
    $body = "grant_type=client_credentials"
    $body += "&client_id=" + $AadClientId
    $body += "&client_secret=" + [Uri]::EscapeDataString($AadClientSecret)
    $body += "&resource=" + [Uri]::EscapeDataString("https://vault.azure.net")
    $headers = @{}

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $access_token = $response.access_token

    Write-Host "Got Auth Token successfully"

    ##############################
    # Get KEK info
    ##############################

    $uri = $KeyEncryptionKey.Id + "?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token}

    $response = Invoke-RestMethod -Method GET -Uri $uri -Headers $headers

    $keyid = $response.key.kid

    Write-Host "Got KEK info successfully"

    ##############################
    # Encrypt passphrase using KEK
    ##############################

    $passphraseB64 = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($Passphrase))
    $uri = $keyid + "/encrypt?api-version=" + $apiversion
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"alg" = "RSA-OAEP"; "value" = $passphraseB64}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method POST -Uri $uri -Headers $headers -Body $body

    $wrappedSecret = $response.value

    Write-Host "Encrypted passphrase successfully"

    ##############################
    # Store secret
    ##############################

    $secretName = [guid]::NewGuid().ToString()
    $uri = $KeyVault.VaultUri + "/secrets/" + $secretName + "?api-version=" + $apiversion
    $secretAttributes = @{"enabled" = $true}
    $secretTags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $headers = @{"Authorization" = "Bearer " + $access_token; "Content-Type" = "application/json"}
    $bodyObj = @{"value" = $wrappedSecret; "attributes" = $secretAttributes; "tags" = $secretTags}
    $body = $bodyObj | ConvertTo-Json

    $response = Invoke-RestMethod -Method PUT -Uri $uri -Headers $headers -Body $body

    Write-Host "Stored secret successfully"

    $secretUrl = $response.id

<span data-ttu-id="e6804-864">Použití `$KeyEncryptionKey` a `$secretUrl` v dalším kroku pro [připojení disku operačního systému pomocí KEK](#using-a-kek).</span><span class="sxs-lookup"><span data-stu-id="e6804-864">Use `$KeyEncryptionKey` and `$secretUrl` in the next step for [attaching the OS disk using KEK](#using-a-kek).</span></span>

### <a name="specify-a-secret-url-when-you-attach-an-os-disk"></a><span data-ttu-id="e6804-865">Zadejte tajný adresu URL, když připojíte disku operačního systému</span><span class="sxs-lookup"><span data-stu-id="e6804-865">Specify a secret URL when you attach an OS disk</span></span>
#### <a name="without-using-a-kek"></a><span data-ttu-id="e6804-866">Bez použití KEK</span><span class="sxs-lookup"><span data-stu-id="e6804-866">Without using a KEK</span></span>
<span data-ttu-id="e6804-867">Když připojujete disk operačního systému, je třeba předat `$secretUrl`.</span><span class="sxs-lookup"><span data-stu-id="e6804-867">While you are attaching the OS disk, you need to pass `$secretUrl`.</span></span> <span data-ttu-id="e6804-868">Adresa URL byla vygenerována v části "šifrování disku tajný klíč není šifrován KEK".</span><span class="sxs-lookup"><span data-stu-id="e6804-868">The URL was generated in the "Disk-encryption secret not encrypted with a KEK" section.</span></span>

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $VhdUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl

#### <a name="using-a-kek"></a><span data-ttu-id="e6804-869">Použití KEK</span><span class="sxs-lookup"><span data-stu-id="e6804-869">Using a KEK</span></span>
<span data-ttu-id="e6804-870">Pokud se připojíte disk operačního systému, předat `$KeyEncryptionKey` a `$secretUrl`.</span><span class="sxs-lookup"><span data-stu-id="e6804-870">When you attach the OS disk, pass `$KeyEncryptionKey` and `$secretUrl`.</span></span> <span data-ttu-id="e6804-871">Adresa URL byla vygenerována v části "šifrování disku tajný klíč není šifrován KEK".</span><span class="sxs-lookup"><span data-stu-id="e6804-871">The URL was generated in the "Disk-encryption secret not encrypted with a KEK" section.</span></span>

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $CopiedTemplateBlobUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl `
            -KeyEncryptionKeyVaultId $KeyVault.ResourceId `
            -KeyEncryptionKeyURL $KeyEncryptionKey.Id

## <a name="download-this-guide"></a><span data-ttu-id="e6804-872">Stáhněte si tento průvodce</span><span class="sxs-lookup"><span data-stu-id="e6804-872">Download this guide</span></span>
<span data-ttu-id="e6804-873">Tato příručka z si můžete stáhnout [Galerii TechNet](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).</span><span class="sxs-lookup"><span data-stu-id="e6804-873">You can download this guide from the [TechNet Gallery](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).</span></span>

## <a name="for-more-information"></a><span data-ttu-id="e6804-874">Další informace</span><span class="sxs-lookup"><span data-stu-id="e6804-874">For more information</span></span>
[<span data-ttu-id="e6804-875">Prozkoumejte Azure Disk Encryption pomocí Azure PowerShell – část 1</span><span class="sxs-lookup"><span data-stu-id="e6804-875">Explore Azure Disk Encryption with Azure PowerShell - Part 1</span></span>](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/16/explore-azure-disk-encryption-with-azure-powershell.aspx?wa=wsignin1.0)  
[<span data-ttu-id="e6804-876">Prozkoumejte Azure Disk Encryption pomocí prostředí Azure PowerShell - část 2</span><span class="sxs-lookup"><span data-stu-id="e6804-876">Explore Azure Disk Encryption with Azure PowerShell - Part 2</span></span>](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx)
