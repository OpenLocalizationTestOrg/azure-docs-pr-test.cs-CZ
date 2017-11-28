---
title: "aaaAzure šifrování disku pro systém Windows a virtuálních počítačů IaaS Linux | Microsoft Docs"
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
ms.openlocfilehash: b685abdcc908e66d2352ec5ac2d9996aa75af1b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-disk-encryption-for-windows-and-linux-iaas-vms"></a><span data-ttu-id="4bdec-103">Azure Disk Encryption pro systém Windows a virtuálních počítačů Linux IaaS</span><span class="sxs-lookup"><span data-stu-id="4bdec-103">Azure Disk Encryption for Windows and Linux IaaS VMs</span></span>
<span data-ttu-id="4bdec-104">Microsoft Azure je důrazně potvrdit tooensuring soukromí dat, data suverenity a umožní vám toocontrol vaše Azure hostované data prostřednictvím řadu tooencrypt pokročilé technologie, řídit a spravovat šifrovací klíče, řízení a audit přístupu k datům.</span><span class="sxs-lookup"><span data-stu-id="4bdec-104">Microsoft Azure is strongly committed tooensuring your data privacy, data sovereignty and enables you toocontrol your Azure hosted data through a range of advanced technologies tooencrypt, control and manage encryption keys, control & audit access of data.</span></span> <span data-ttu-id="4bdec-105">To poskytuje Azure zákazníkům hello flexibilitu toochoose hello řešení, které nejlépe vyhovuje potřebám své firmy.</span><span class="sxs-lookup"><span data-stu-id="4bdec-105">This provides Azure customers hello flexibility toochoose hello solution that best meets their business needs.</span></span> <span data-ttu-id="4bdec-106">V tomto dokumentu jsme vás seznámí tooa nové technologie řešení "Azure Disk Encryption pro systém Windows a Linux IaaS Virtuálního počítače" toohelp chránit a chrání vaše data toomeet organizační závazky zabezpečení a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-106">In this paper, we will introduce you tooa new technology solution “Azure Disk Encryption for Windows and Linux IaaS VM’s” toohelp protect and safeguard your data toomeet your organizational security and compliance commitments.</span></span> <span data-ttu-id="4bdec-107">Hello dokument obsahuje podrobné pokyny k jak toouse hello Azure disk encryption funkce včetně hello Podporované scénáře a hello uživatelského prostředí.</span><span class="sxs-lookup"><span data-stu-id="4bdec-107">hello paper provides detailed guidance on how toouse hello Azure disk encryption features including hello supported scenarios and hello user experiences.</span></span>

> [!NOTE]
> <span data-ttu-id="4bdec-108">Některá doporučení může zvýšit data, síťové nebo využití výpočetních prostředků, což vede k další náklady na licence nebo předplatné.</span><span class="sxs-lookup"><span data-stu-id="4bdec-108">Certain recommendations might increase data, network, or compute resource usage, resulting in additional license or subscription costs.</span></span>

## <a name="overview"></a><span data-ttu-id="4bdec-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="4bdec-109">Overview</span></span>
<span data-ttu-id="4bdec-110">Azure Disk Encryption je novou funkci, která umožňuje šifrovat disky virtuálního počítače s Windows a Linux IaaS.</span><span class="sxs-lookup"><span data-stu-id="4bdec-110">Azure Disk Encryption is a new capability that helps you encrypt your Windows and Linux IaaS virtual machine disks.</span></span> <span data-ttu-id="4bdec-111">Azure Disk Encryption využívá standardní hello [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) funkce systému Windows a hello [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) funkce Linux tooprovide svazku šifrování pro hello operačního systému a datové disky hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-111">Azure Disk Encryption leverages hello industry standard [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) feature of Windows and hello [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) feature of Linux tooprovide volume encryption for hello OS and hello data disks.</span></span> <span data-ttu-id="4bdec-112">řešení Hello je integrovaná s [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) toohelp řídit a spravovat hello šifrování disku klíče a tajné klíče v rámci vašeho předplatného trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-112">hello solution is integrated with [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) toohelp you control and manage hello disk-encryption keys and secrets in your key vault subscription.</span></span> <span data-ttu-id="4bdec-113">Hello řešení také zajistí, že jsou všechna data na disky virtuálního počítače hello zašifrovaná přinejmenším ve službě Azure storage.</span><span class="sxs-lookup"><span data-stu-id="4bdec-113">hello solution also ensures that all data on hello virtual machine disks are encrypted at rest in your Azure storage.</span></span>

<span data-ttu-id="4bdec-114">Azure disk encryption pro systém Windows a virtuálních počítačů IaaS Linux je nyní v **obecné dostupnosti** do všech veřejných oblastí Azure a oblasti AzureGov standardní virtuální počítače a virtuální počítače s storage úrovně premium.</span><span class="sxs-lookup"><span data-stu-id="4bdec-114">Azure disk encryption for Windows and Linux IaaS VMs is now in **General Availability** in all Azure public regions and AzureGov regions for Standard  VMs and VMs with premium storage.</span></span>

### <a name="encryption-scenarios"></a><span data-ttu-id="4bdec-115">Šifrování scénáře</span><span class="sxs-lookup"><span data-stu-id="4bdec-115">Encryption scenarios</span></span>
<span data-ttu-id="4bdec-116">Hello řešení Azure Disk Encryption podporuje následující scénáře zákazníka hello:</span><span class="sxs-lookup"><span data-stu-id="4bdec-116">hello Azure Disk Encryption solution supports hello following customer scenarios:</span></span>

* <span data-ttu-id="4bdec-117">Povolit šifrování na nové virtuální počítače IaaS vytvořené z předem šifrované virtuálního pevného disku a šifrovacích klíčů</span><span class="sxs-lookup"><span data-stu-id="4bdec-117">Enable encryption on new IaaS VMs created from pre-encrypted VHD and encryption keys</span></span>
* <span data-ttu-id="4bdec-118">Povolit šifrování na nové virtuální počítače IaaS vytvořené z hello podporováno obrázky z Galerie Azure</span><span class="sxs-lookup"><span data-stu-id="4bdec-118">Enable encryption on new IaaS VMs created from hello supported Azure Gallery images</span></span>
* <span data-ttu-id="4bdec-119">Povolit šifrování na existující virtuální počítače IaaS běžící v Azure</span><span class="sxs-lookup"><span data-stu-id="4bdec-119">Enable encryption on existing IaaS VMs running in Azure</span></span>
* <span data-ttu-id="4bdec-120">Vypnout šifrování u virtuálních počítačů IaaS Windows</span><span class="sxs-lookup"><span data-stu-id="4bdec-120">Disable encryption on Windows IaaS VMs</span></span>
* <span data-ttu-id="4bdec-121">Zakázat šifrování na datových jednotkách pro virtuální počítače IaaS Linux</span><span class="sxs-lookup"><span data-stu-id="4bdec-121">Disable encryption on data drives for Linux IaaS VMs</span></span>
* <span data-ttu-id="4bdec-122">Povolit šifrování spravovaných disků na virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="4bdec-122">Enable encryption of managed disk VMs</span></span>
* <span data-ttu-id="4bdec-123">Aktualizovat nastavení šifrování existující úložiště šifrované neprémiové virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="4bdec-123">Update encryption settings of an existing encrypted non-premium storage VM</span></span>
* <span data-ttu-id="4bdec-124">Zálohování a obnovení šifrovaných virtuálních počítačů, šifrovaný klíč šifrovacího klíče</span><span class="sxs-lookup"><span data-stu-id="4bdec-124">Backup and restore of encrypted VMs, encrypted with key encryption key</span></span>

<span data-ttu-id="4bdec-125">řešení Hello podporuje následující scénáře pro virtuální počítače IaaS, pokud jsou povolené v Microsoft Azure hello:</span><span class="sxs-lookup"><span data-stu-id="4bdec-125">hello solution supports hello following scenarios for IaaS VMs when they are enabled in Microsoft Azure:</span></span>

* <span data-ttu-id="4bdec-126">Integrace s Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="4bdec-126">Integration with Azure Key Vault</span></span>
* <span data-ttu-id="4bdec-127">Úroveň Standard virtuálních počítačů: [A, D, DS, G, GS, F a atd. řady virtuální počítače IaaS](https://azure.microsoft.com/pricing/details/virtual-machines/)</span><span class="sxs-lookup"><span data-stu-id="4bdec-127">Standard tier VMs: [A, D, DS, G, GS, F, and so forth series IaaS VMs](https://azure.microsoft.com/pricing/details/virtual-machines/)</span></span>
* <span data-ttu-id="4bdec-128">Povolit šifrování na Windows a virtuálních počítačů IaaS Linux a spravovaných disků virtuálních počítačů z hello podporováno obrázky z Galerie Azure</span><span class="sxs-lookup"><span data-stu-id="4bdec-128">Enable encryption on Windows and Linux IaaS VMs and managed disk VMs from hello supported Azure Gallery images</span></span>
* <span data-ttu-id="4bdec-129">Zakázat šifrování na jednotkách operačního systému a dat pro virtuální počítače IaaS Windows a spravovaných disků na virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="4bdec-129">Disable encryption on OS and data drives for Windows IaaS VMs and managed disk VMs</span></span>
* <span data-ttu-id="4bdec-130">Zakázat šifrování na datových jednotkách pro virtuální počítače IaaS Linux a spravovaných disků na virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="4bdec-130">Disable encryption on data drives for Linux IaaS VMs and managed disk VMs</span></span>
* <span data-ttu-id="4bdec-131">Povolit šifrování na virtuální počítače IaaS se systémem OS klienta Windows</span><span class="sxs-lookup"><span data-stu-id="4bdec-131">Enable encryption on IaaS VMs running Windows Client OS</span></span>
* <span data-ttu-id="4bdec-132">Povolit šifrování na svazcích s cestami připojení</span><span class="sxs-lookup"><span data-stu-id="4bdec-132">Enable encryption on volumes with mount paths</span></span>
* <span data-ttu-id="4bdec-133">Povolit šifrování na virtuální počítače s Linuxem nakonfigurovaný s diskem proložení (RAID) pomocí mdadm</span><span class="sxs-lookup"><span data-stu-id="4bdec-133">Enable encryption on Linux VMs configured with disk striping (RAID) using mdadm</span></span>
* <span data-ttu-id="4bdec-134">Povolit šifrování na virtuální počítače s Linuxem pomocí LVM pro datové disky</span><span class="sxs-lookup"><span data-stu-id="4bdec-134">Enable encryption on Linux VMs using LVM for data disks</span></span>
* <span data-ttu-id="4bdec-135">Povolit šifrování na virtuálních počítačích Windows nakonfigurovaný s prostory úložiště</span><span class="sxs-lookup"><span data-stu-id="4bdec-135">Enable encryption on Windows VMs configured with Storage Spaces</span></span>
* <span data-ttu-id="4bdec-136">Aktualizovat nastavení šifrování existující úložiště šifrované neprémiové virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="4bdec-136">Update encryption settings of an existing encrypted non-premium storage VM</span></span>
* <span data-ttu-id="4bdec-137">Jsou podporovány všechny veřejné Azure a AzureGov oblastí</span><span class="sxs-lookup"><span data-stu-id="4bdec-137">All Azure Public and AzureGov regions are supported</span></span>

<span data-ttu-id="4bdec-138">řešení Hello nepodporuje následující scénáře, funkce a technologie hello:</span><span class="sxs-lookup"><span data-stu-id="4bdec-138">hello solution does not support hello following scenarios, features, and technology:</span></span>

* <span data-ttu-id="4bdec-139">Úroveň Basic virtuálních počítačů IaaS</span><span class="sxs-lookup"><span data-stu-id="4bdec-139">Basic tier IaaS VMs</span></span>
* <span data-ttu-id="4bdec-140">Zakázáním šifrování na jednotce operačního systému pro virtuální počítače IaaS Linux</span><span class="sxs-lookup"><span data-stu-id="4bdec-140">Disabling encryption on an OS drive for Linux IaaS VMs</span></span>
* <span data-ttu-id="4bdec-141">Zakázáním šifrování na datová jednotka, pokud je zašifrován hello jednotky operačního systému pro virtuální počítače Iaas Linux</span><span class="sxs-lookup"><span data-stu-id="4bdec-141">Disabling encryption on a data drive if hello OS drive is encrypted for Linux Iaas VMs</span></span>
* <span data-ttu-id="4bdec-142">Virtuální počítače IaaS, vytvořené pomocí metody vytvoření virtuálního počítače classic hello</span><span class="sxs-lookup"><span data-stu-id="4bdec-142">IaaS VMs that are created by using hello classic VM creation method</span></span>
* <span data-ttu-id="4bdec-143">Povolte šifrování na systém Windows a virtuálních počítačů Linux IaaS zákazníka vlastních bitových kopií není podporován.</span><span class="sxs-lookup"><span data-stu-id="4bdec-143">Enable encryption on Windows and Linux IaaS VMs customer custom images is NOT supported.</span></span> <span data-ttu-id="4bdec-144">Povolit enccryption na operační systém Linux LVM disku aktuálně nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="4bdec-144">Enable enccryption on Linux LVM OS disk is not supported currently.</span></span> <span data-ttu-id="4bdec-145">Tato podpora bude brzy pocházet.</span><span class="sxs-lookup"><span data-stu-id="4bdec-145">This support will come soon.</span></span>
* <span data-ttu-id="4bdec-146">Integrace s vaší místní služby správy klíčů</span><span class="sxs-lookup"><span data-stu-id="4bdec-146">Integration with your on-premises Key Management Service</span></span>
* <span data-ttu-id="4bdec-147">Soubory Azure (systém souborů sdíleného), Network File System (NFS), dynamické svazky a virtuální počítače Windows, které jsou nakonfigurované s systémy na bázi softwaru diskového pole RAID</span><span class="sxs-lookup"><span data-stu-id="4bdec-147">Azure Files (shared file system), Network File System (NFS), dynamic volumes, and Windows VMs that are configured with software-based RAID systems</span></span>
* <span data-ttu-id="4bdec-148">Zálohování a obnovení šifrovaných virtuálních počítačů, šifrované bez klíče šifrovací klíč.</span><span class="sxs-lookup"><span data-stu-id="4bdec-148">Backup and restore of encrypted VMs, encrypted without key encryption key.</span></span>
* <span data-ttu-id="4bdec-149">Aktualizujte nastavení šifrování existující šifrované premium úložiště virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-149">Update encryption settings of an existing encrypted premium storage VM.</span></span>

> [!NOTE]
> <span data-ttu-id="4bdec-150">Zálohování a obnovení šifrovaných virtuálních počítačů je podporována pouze pro virtuální počítače, které jsou zašifrované s konfigurací KEK hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-150">Backup and restore of encrypted VMs is supported only for VMs that are encrypted with hello KEK configuration.</span></span> <span data-ttu-id="4bdec-151">Není podporována u virtuálních počítačů, které jsou zašifrované bez KEK.</span><span class="sxs-lookup"><span data-stu-id="4bdec-151">It is not supported on VMs that are encrypted without KEK.</span></span> <span data-ttu-id="4bdec-152">KEK je volitelný parametr, který povoluje šifrování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-152">KEK is an optional parameter that enables VM encryption.</span></span> <span data-ttu-id="4bdec-153">Tato podpora se připravuje.</span><span class="sxs-lookup"><span data-stu-id="4bdec-153">This support is coming soon.</span></span>
> <span data-ttu-id="4bdec-154">Aktualizujte nastavení šifrování virtuálního počítače nejsou podporovány existující úložiště šifrované premium Storage.</span><span class="sxs-lookup"><span data-stu-id="4bdec-154">Update encryption settings of an existing encrypted premium storage VM are not supported.</span></span> <span data-ttu-id="4bdec-155">Tato podpora se připravuje.</span><span class="sxs-lookup"><span data-stu-id="4bdec-155">This support is coming soon.</span></span>

### <a name="encryption-features"></a><span data-ttu-id="4bdec-156">Funkce šifrování</span><span class="sxs-lookup"><span data-stu-id="4bdec-156">Encryption features</span></span>
<span data-ttu-id="4bdec-157">Když povolíte a nasadíte Azure Disk Encryption pro virtuální počítače Azure IaaS, hello následující funkce jsou povolené, v závislosti na konfiguraci hello poskytuje:</span><span class="sxs-lookup"><span data-stu-id="4bdec-157">When you enable and deploy Azure Disk Encryption for Azure IaaS VMs, hello following capabilities are enabled, depending on hello configuration provided:</span></span>

* <span data-ttu-id="4bdec-158">Šifrování hello OS svazku tooprotect hello spouštěcí svazek v klidovém stavu uložených v úložišti</span><span class="sxs-lookup"><span data-stu-id="4bdec-158">Encryption of hello OS volume tooprotect hello boot volume at rest in your storage</span></span>
* <span data-ttu-id="4bdec-159">Šifrování dat svazky tooprotect hello datové svazky v klidovém stavu uložených v úložišti</span><span class="sxs-lookup"><span data-stu-id="4bdec-159">Encryption of data volumes tooprotect hello data volumes at rest in your storage</span></span>
* <span data-ttu-id="4bdec-160">Zakázáním šifrování na hello operačního systému a datové disky pro virtuální počítače IaaS Windows</span><span class="sxs-lookup"><span data-stu-id="4bdec-160">Disabling encryption on hello OS and data drives for Windows IaaS VMs</span></span>
* <span data-ttu-id="4bdec-161">Zakázáním šifrování dat hello disky pro virtuální počítače IaaS Linux (pouze v případě operačního systému je není šifrované jednotky)</span><span class="sxs-lookup"><span data-stu-id="4bdec-161">Disabling encryption on hello data drives for Linux IaaS VMs (only if OS drive IS NOT encrypted)</span></span>
* <span data-ttu-id="4bdec-162">Zabezpečení hello šifrovacích klíčů a tajných klíčů ve vašem předplatném trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="4bdec-162">Safeguarding hello encryption keys and secrets in your key vault subscription</span></span>
* <span data-ttu-id="4bdec-163">Vytváření sestav stav šifrování hello hello šifrované virtuálních počítačů IaaS</span><span class="sxs-lookup"><span data-stu-id="4bdec-163">Reporting hello encryption status of hello encrypted IaaS VM</span></span>
* <span data-ttu-id="4bdec-164">Odebrání nastavení konfigurace šifrování disku z virtuálního počítače IaaS hello</span><span class="sxs-lookup"><span data-stu-id="4bdec-164">Removal of disk-encryption configuration settings from hello IaaS virtual machine</span></span>
* <span data-ttu-id="4bdec-165">Zálohování a obnovení šifrovaných virtuálních počítačů pomocí služby Azure Backup hello</span><span class="sxs-lookup"><span data-stu-id="4bdec-165">Backup and restore of encrypted VMs by using hello Azure Backup service</span></span>

> [!NOTE]
> <span data-ttu-id="4bdec-166">Zálohování a obnovení šifrovaných virtuálních počítačů je podporována pouze pro virtuální počítače, které jsou zašifrované s konfigurací KEK hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-166">Backup and restore of encrypted VMs is supported only for VMs that are encrypted with hello KEK configuration.</span></span> <span data-ttu-id="4bdec-167">Není podporována u virtuálních počítačů, které jsou zašifrované bez KEK.</span><span class="sxs-lookup"><span data-stu-id="4bdec-167">It is not supported on VMs that are encrypted without KEK.</span></span> <span data-ttu-id="4bdec-168">KEK je volitelný parametr, který povoluje šifrování virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-168">KEK is an optional parameter that enables VM encryption.</span></span>

<span data-ttu-id="4bdec-169">Azure Disk Encryption pro virtuální počítače IaaS pro Windows a Linux řešení zahrnuje:</span><span class="sxs-lookup"><span data-stu-id="4bdec-169">Azure Disk Encryption for IaaS VMS for Windows and Linux solution includes:</span></span>

* <span data-ttu-id="4bdec-170">Hello šifrování disku rozšíření pro Windows.</span><span class="sxs-lookup"><span data-stu-id="4bdec-170">hello disk-encryption extension for Windows.</span></span>
* <span data-ttu-id="4bdec-171">Hello šifrování disku rozšíření pro Linux.</span><span class="sxs-lookup"><span data-stu-id="4bdec-171">hello disk-encryption extension for Linux.</span></span>
* <span data-ttu-id="4bdec-172">rutiny prostředí PowerShell Hello šifrování disku.</span><span class="sxs-lookup"><span data-stu-id="4bdec-172">hello disk-encryption PowerShell cmdlets.</span></span>
* <span data-ttu-id="4bdec-173">Hello šifrování disku rutiny rozhraní příkazového řádku Azure (CLI).</span><span class="sxs-lookup"><span data-stu-id="4bdec-173">hello disk-encryption Azure command-line interface (CLI) cmdlets.</span></span>
* <span data-ttu-id="4bdec-174">šablony Azure Resource Manager Hello šifrování disku.</span><span class="sxs-lookup"><span data-stu-id="4bdec-174">hello disk-encryption Azure Resource Manager templates.</span></span>

<span data-ttu-id="4bdec-175">Hello řešení Azure Disk Encryption je podporována u virtuálních počítačů IaaS, který se systémem Windows nebo operační systém Linux.</span><span class="sxs-lookup"><span data-stu-id="4bdec-175">hello Azure Disk Encryption solution is supported on IaaS VMs that are running Windows or Linux OS.</span></span> <span data-ttu-id="4bdec-176">Další informace o hello podporované operační systémy najdete v tématu požadavky"hello" oddílu.</span><span class="sxs-lookup"><span data-stu-id="4bdec-176">For more information about hello supported operating systems, see hello "Prerequisites" section.</span></span>

> [!NOTE]
> <span data-ttu-id="4bdec-177">Není k dispozici bez dalších poplatků pro šifrování disky virtuálních počítačů s Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="4bdec-177">There is no additional charge for encrypting VM disks with Azure Disk Encryption.</span></span>

### <a name="value-proposition"></a><span data-ttu-id="4bdec-178">Návrh hodnoty</span><span class="sxs-lookup"><span data-stu-id="4bdec-178">Value proposition</span></span>
<span data-ttu-id="4bdec-179">Při použití řešení hello Azure Disk Encryption správy se může stát odpovědí hello následující obchodních potřeb:</span><span class="sxs-lookup"><span data-stu-id="4bdec-179">When you apply hello Azure Disk Encryption-management solution, you can satisfy hello following business needs:</span></span>

* <span data-ttu-id="4bdec-180">Virtuální počítače IaaS jsou zabezpečeny v klidu, protože můžete použít standardní technologie tooaddress organizační zabezpečení a dodržování předpisů požadavky na šifrování.</span><span class="sxs-lookup"><span data-stu-id="4bdec-180">IaaS VMs are secured at rest, because you can use industry-standard encryption technology tooaddress organizational security and compliance requirements.</span></span>
* <span data-ttu-id="4bdec-181">Spouštění virtuálních počítačů IaaS v části klíče řídí zákazníka a zásady a auditovat jejich využití v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-181">IaaS VMs boot under customer-controlled keys and policies, and you can audit their usage in your key vault.</span></span>

### <a name="encryption-workflow"></a><span data-ttu-id="4bdec-182">Pracovní postup šifrování</span><span class="sxs-lookup"><span data-stu-id="4bdec-182">Encryption workflow</span></span>
<span data-ttu-id="4bdec-183">šifrování disku tooenable pro systém Windows a virtuální počítače s Linuxem, hello následující:</span><span class="sxs-lookup"><span data-stu-id="4bdec-183">tooenable disk encryption for Windows and Linux VMs, do hello following:</span></span>

1. <span data-ttu-id="4bdec-184">Zvolte scénářem šifrování některé z těchto hello předcházející šifrování scénáře.</span><span class="sxs-lookup"><span data-stu-id="4bdec-184">Choose an encryption scenario from among hello preceding encryption scenarios.</span></span>
2. <span data-ttu-id="4bdec-185">Vyjádřit výslovný souhlas tooenabling šifrování disku pomocí šablony Azure Disk Encryption Resource Manageru hello, rutiny prostředí PowerShell nebo rozhraní příkazového řádku příkaz a zadejte konfiguraci šifrování hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-185">Opt in tooenabling disk encryption via hello Azure Disk Encryption Resource Manager template, PowerShell cmdlets, or CLI command, and specify hello encryption configuration.</span></span>

   * <span data-ttu-id="4bdec-186">Hello šifrované zákazníka virtuálního pevného disku scénáři nahrajte účet úložiště tooyour hello šifrované virtuálního pevného disku a hello šifrovací klíče podstatným tooyour trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-186">For hello customer-encrypted VHD scenario, upload hello encrypted VHD tooyour storage account and hello encryption key material tooyour key vault.</span></span> <span data-ttu-id="4bdec-187">Potom zadejte hello šifrování konfigurace tooenable na nový virtuální počítač IaaS.</span><span class="sxs-lookup"><span data-stu-id="4bdec-187">Then, provide hello encryption configuration tooenable encryption on a new IaaS VM.</span></span>
   * <span data-ttu-id="4bdec-188">Nové virtuální počítače, které jsou vytvořené pomocí hello Marketplace a stávající virtuální počítače, které jsou již spuštěny v Azure zadejte hello šifrování konfigurace tooenable na hello virtuálních počítačů IaaS.</span><span class="sxs-lookup"><span data-stu-id="4bdec-188">For new VMs that are created from hello Marketplace and existing VMs that are already running in Azure, provide hello encryption configuration tooenable encryption on hello IaaS VM.</span></span>

3. <span data-ttu-id="4bdec-189">Udělení přístupu toohello platformy Azure tooread hello šifrovací klíč materiálu (šifrovací klíče nástroje BitLocker pro systémy Windows) a heslo pro Linux z trezoru klíčů tooenable šifrování na hello virtuálních počítačů IaaS.</span><span class="sxs-lookup"><span data-stu-id="4bdec-189">Grant access toohello Azure platform tooread hello encryption-key material (BitLocker encryption keys for Windows systems and Passphrase for Linux) from your key vault tooenable encryption on hello IaaS VM.</span></span>

4. <span data-ttu-id="4bdec-190">Zadejte hello Azure Active Directory (Azure AD) aplikace identity toowrite hello šifrovací klíče podstatným tooyour trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-190">Provide hello Azure Active Directory (Azure AD) application identity toowrite hello encryption key material tooyour key vault.</span></span> <span data-ttu-id="4bdec-191">Díky tomu povoluje šifrování na hello virtuálních počítačů IaaS pro scénáře hello uveden v kroku 2.</span><span class="sxs-lookup"><span data-stu-id="4bdec-191">Doing so enables encryption on hello IaaS VM for hello scenarios mentioned in step 2.</span></span>

5. <span data-ttu-id="4bdec-192">Azure aktualizuje hello modelu služby virtuálních počítačů pomocí šifrování a konfigurace hello trezoru klíčů a nastaví šifrované virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4bdec-192">Azure updates hello VM service model with encryption and hello key vault configuration, and sets up your encrypted VM.</span></span>

 ![Microsoft Antimalware v Azure](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

### <a name="decryption-workflow"></a><span data-ttu-id="4bdec-194">Dešifrování pracovního postupu</span><span class="sxs-lookup"><span data-stu-id="4bdec-194">Decryption workflow</span></span>
<span data-ttu-id="4bdec-195">toodisable šifrování disku pro virtuální počítače IaaS, dokončení hello následující postup vysoké úrovně:</span><span class="sxs-lookup"><span data-stu-id="4bdec-195">toodisable disk encryption for IaaS VMs, complete hello following high-level steps:</span></span>

1. <span data-ttu-id="4bdec-196">Zvolte toodisable šifrování (dešifrování) na spuštění virtuálního počítače IaaS v Azure pomocí šablony Azure Disk Encryption Resource Manageru hello nebo rutiny prostředí PowerShell a zadejte konfiguraci dešifrování hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-196">Choose toodisable encryption (decryption) on a running IaaS VM in Azure via hello Azure Disk Encryption Resource Manager template or PowerShell cmdlets, and specify hello decryption configuration.</span></span>

 <span data-ttu-id="4bdec-197">Tento krok zakazuje šifrování hello operačního systému nebo hello datový svazek nebo obojí na spuštění virtuálních počítačů IaaS Windows hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-197">This step disables encryption of hello OS or hello data volume or both on hello running Windows IaaS VM.</span></span> <span data-ttu-id="4bdec-198">Nicméně jak je uvedeno v předchozí části hello, zakázáním šifrování disku operačního systému Linux nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="4bdec-198">However, as mentioned in hello previous section, disabling OS disk encryption for Linux is not supported.</span></span> <span data-ttu-id="4bdec-199">Krok dešifrování Hello je povoleno pouze pro datové jednotky v virtuální počítače s Linuxem, dokud není šifrován hello disk operačního systému.</span><span class="sxs-lookup"><span data-stu-id="4bdec-199">hello decryption step is allowed only for data drives on Linux VMs as long as hello OS disk is not encrypted.</span></span>
2. <span data-ttu-id="4bdec-200">Azure aktualizace hello modelu služby virtuálních počítačů a virtuálních počítačů IaaS hello je označen jako dešifrovaný.</span><span class="sxs-lookup"><span data-stu-id="4bdec-200">Azure updates hello VM service model, and hello IaaS VM is marked decrypted.</span></span> <span data-ttu-id="4bdec-201">obsah Hello hello virtuálního počítače jsou již v zašifrované podobě.</span><span class="sxs-lookup"><span data-stu-id="4bdec-201">hello contents of hello VM are no longer encrypted at rest.</span></span>

> [!NOTE]
> <span data-ttu-id="4bdec-202">operace disable šifrování Hello nedojde k odstranění vašeho klíče trezoru a hello šifrování materiál klíče (šifrovací klíče nástroje BitLocker pro systémy Windows) nebo přístupové heslo pro Linux.</span><span class="sxs-lookup"><span data-stu-id="4bdec-202">hello disable-encryption operation does not delete your key vault and hello encryption key material (BitLocker encryption keys for Windows systems or Passphrase for Linux).</span></span>
 > <span data-ttu-id="4bdec-203">Zakázáním šifrování disku operačního systému Linux není podporována.</span><span class="sxs-lookup"><span data-stu-id="4bdec-203">Disabling OS disk encryption for Linux is not supported.</span></span> <span data-ttu-id="4bdec-204">Krok dešifrování Hello je povoleno pouze pro datové jednotky v virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="4bdec-204">hello decryption step is allowed only for data drives on Linux VMs.</span></span>
<span data-ttu-id="4bdec-205">Zakázání disku šifrování dat pro Linux není podporována, pokud je zašifrován hello jednotky operačního systému.</span><span class="sxs-lookup"><span data-stu-id="4bdec-205">Disabling data disk encryption for Linux is not supported if hello OS drive is encrypted.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4bdec-206">Požadavky</span><span class="sxs-lookup"><span data-stu-id="4bdec-206">Prerequisites</span></span>
<span data-ttu-id="4bdec-207">Než povolíte Azure Disk Encryption na virtuálních počítačích Azure IaaS pro hello Podporované scénáře, které bylo popsané v části "Přehled" hello, najdete v části hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="4bdec-207">Before you enable Azure Disk Encryption on Azure IaaS VMs for hello supported scenarios that were discussed in hello "Overview" section, see hello following prerequisites:</span></span>

* <span data-ttu-id="4bdec-208">V Azure v oblastech hello podporované musí mít prostředky toocreate platný aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="4bdec-208">You must have a valid active Azure subscription toocreate resources in Azure in hello supported regions.</span></span>
* <span data-ttu-id="4bdec-209">Azure Disk Encryption je podporována v následujících verzích Windows Server hello: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 a Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="4bdec-209">Azure Disk Encryption is supported on hello following Windows Server versions: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, and Windows Server 2016.</span></span>
* <span data-ttu-id="4bdec-210">Azure Disk Encryption je podporována v následujících klientských verzí Windows hello: klienta systému Windows 8 a Windows 10 klienta.</span><span class="sxs-lookup"><span data-stu-id="4bdec-210">Azure Disk Encryption is supported on hello following Windows client versions: Windows 8 client and Windows 10 client.</span></span>

> [!NOTE]
> <span data-ttu-id="4bdec-211">Pro Windows Server 2008 R2 musíte mít rozhraní .NET Framework 4.5 nainstalované dříve než povolíte šifrování v Azure.</span><span class="sxs-lookup"><span data-stu-id="4bdec-211">For Windows Server 2008 R2, you must have .NET Framework 4.5 installed before you enable encryption in Azure.</span></span> <span data-ttu-id="4bdec-212">Můžete ji nainstalovat z webu Windows Update nainstalováním hello volitelnou aktualizaci rozhraní Microsoft .NET Framework 4.5.2 na x64 systémů Windows Server 2008 R2 ([KB2901983](https://support.microsoft.com/kb/2901983)).</span><span class="sxs-lookup"><span data-stu-id="4bdec-212">You can install it from Windows Update by installing hello optional update Microsoft .NET Framework 4.5.2 for Windows Server 2008 R2 x64-based systems ([KB2901983](https://support.microsoft.com/kb/2901983)).</span></span>

* <span data-ttu-id="4bdec-213">Azure Disk Encryption se podporuje na hello následující Azure Gallery založena na Linuxových server distribucích a verzích:</span><span class="sxs-lookup"><span data-stu-id="4bdec-213">Azure Disk Encryption is supported on hello following Azure Gallery based Linux server distributions and versions:</span></span>

| <span data-ttu-id="4bdec-214">Distribuce systému Linux</span><span class="sxs-lookup"><span data-stu-id="4bdec-214">Linux Distribution</span></span> | <span data-ttu-id="4bdec-215">Verze</span><span class="sxs-lookup"><span data-stu-id="4bdec-215">Version</span></span> | <span data-ttu-id="4bdec-216">Typ svazku podporovaný pro šifrování</span><span class="sxs-lookup"><span data-stu-id="4bdec-216">Volume Type Supported for Encryption</span></span>|
| --- | --- |--- |
| <span data-ttu-id="4bdec-217">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="4bdec-217">Ubuntu</span></span> | <span data-ttu-id="4bdec-218">16.04. DENNĚ LTS</span><span class="sxs-lookup"><span data-stu-id="4bdec-218">16.04-DAILY-LTS</span></span> | <span data-ttu-id="4bdec-219">Disk operačního systému a dat</span><span class="sxs-lookup"><span data-stu-id="4bdec-219">OS and Data disk</span></span> |
| <span data-ttu-id="4bdec-220">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="4bdec-220">Ubuntu</span></span> | <span data-ttu-id="4bdec-221">14.04.5-DAILY-LTS</span><span class="sxs-lookup"><span data-stu-id="4bdec-221">14.04.5-DAILY-LTS</span></span> | <span data-ttu-id="4bdec-222">Disk operačního systému a dat</span><span class="sxs-lookup"><span data-stu-id="4bdec-222">OS and Data disk</span></span> |
| <span data-ttu-id="4bdec-223">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="4bdec-223">Ubuntu</span></span> | <span data-ttu-id="4bdec-224">12.10</span><span class="sxs-lookup"><span data-stu-id="4bdec-224">12.10</span></span> | <span data-ttu-id="4bdec-225">Datový disk</span><span class="sxs-lookup"><span data-stu-id="4bdec-225">Data disk</span></span> |
| <span data-ttu-id="4bdec-226">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="4bdec-226">Ubuntu</span></span> | <span data-ttu-id="4bdec-227">12.04</span><span class="sxs-lookup"><span data-stu-id="4bdec-227">12.04</span></span> | <span data-ttu-id="4bdec-228">Datový disk</span><span class="sxs-lookup"><span data-stu-id="4bdec-228">Data disk</span></span> |
| <span data-ttu-id="4bdec-229">RHEL</span><span class="sxs-lookup"><span data-stu-id="4bdec-229">RHEL</span></span> | <span data-ttu-id="4bdec-230">7.3</span><span class="sxs-lookup"><span data-stu-id="4bdec-230">7.3</span></span> | <span data-ttu-id="4bdec-231">Disk operačního systému a dat</span><span class="sxs-lookup"><span data-stu-id="4bdec-231">OS and Data disk</span></span> |
| <span data-ttu-id="4bdec-232">RHEL</span><span class="sxs-lookup"><span data-stu-id="4bdec-232">RHEL</span></span> | <span data-ttu-id="4bdec-233">7.2</span><span class="sxs-lookup"><span data-stu-id="4bdec-233">7.2</span></span> | <span data-ttu-id="4bdec-234">Disk operačního systému a dat</span><span class="sxs-lookup"><span data-stu-id="4bdec-234">OS and Data disk</span></span> |
| <span data-ttu-id="4bdec-235">RHEL</span><span class="sxs-lookup"><span data-stu-id="4bdec-235">RHEL</span></span> | <span data-ttu-id="4bdec-236">6.8</span><span class="sxs-lookup"><span data-stu-id="4bdec-236">6.8</span></span> | <span data-ttu-id="4bdec-237">Disk operačního systému a dat</span><span class="sxs-lookup"><span data-stu-id="4bdec-237">OS and Data disk</span></span> |
| <span data-ttu-id="4bdec-238">RHEL</span><span class="sxs-lookup"><span data-stu-id="4bdec-238">RHEL</span></span> | <span data-ttu-id="4bdec-239">6.7</span><span class="sxs-lookup"><span data-stu-id="4bdec-239">6.7</span></span> | <span data-ttu-id="4bdec-240">Datový disk</span><span class="sxs-lookup"><span data-stu-id="4bdec-240">Data disk</span></span> |
| <span data-ttu-id="4bdec-241">CentOS</span><span class="sxs-lookup"><span data-stu-id="4bdec-241">CentOS</span></span> | <span data-ttu-id="4bdec-242">7.3</span><span class="sxs-lookup"><span data-stu-id="4bdec-242">7.3</span></span> | <span data-ttu-id="4bdec-243">Disk operačního systému a dat</span><span class="sxs-lookup"><span data-stu-id="4bdec-243">OS and Data disk</span></span> |
| <span data-ttu-id="4bdec-244">CentOS</span><span class="sxs-lookup"><span data-stu-id="4bdec-244">CentOS</span></span> | <span data-ttu-id="4bdec-245">7.2N</span><span class="sxs-lookup"><span data-stu-id="4bdec-245">7.2n</span></span> | <span data-ttu-id="4bdec-246">Disk operačního systému a dat</span><span class="sxs-lookup"><span data-stu-id="4bdec-246">OS and Data disk</span></span> |
| <span data-ttu-id="4bdec-247">CentOS</span><span class="sxs-lookup"><span data-stu-id="4bdec-247">CentOS</span></span> | <span data-ttu-id="4bdec-248">6.8</span><span class="sxs-lookup"><span data-stu-id="4bdec-248">6.8</span></span> | <span data-ttu-id="4bdec-249">Disk operačního systému a dat</span><span class="sxs-lookup"><span data-stu-id="4bdec-249">OS and Data disk</span></span> |
| <span data-ttu-id="4bdec-250">CentOS</span><span class="sxs-lookup"><span data-stu-id="4bdec-250">CentOS</span></span> | <span data-ttu-id="4bdec-251">7.1</span><span class="sxs-lookup"><span data-stu-id="4bdec-251">7.1</span></span> | <span data-ttu-id="4bdec-252">Datový disk</span><span class="sxs-lookup"><span data-stu-id="4bdec-252">Data disk</span></span> |
| <span data-ttu-id="4bdec-253">CentOS</span><span class="sxs-lookup"><span data-stu-id="4bdec-253">CentOS</span></span> | <span data-ttu-id="4bdec-254">7.0</span><span class="sxs-lookup"><span data-stu-id="4bdec-254">7.0</span></span> | <span data-ttu-id="4bdec-255">Datový disk</span><span class="sxs-lookup"><span data-stu-id="4bdec-255">Data disk</span></span> |
| <span data-ttu-id="4bdec-256">CentOS</span><span class="sxs-lookup"><span data-stu-id="4bdec-256">CentOS</span></span> | <span data-ttu-id="4bdec-257">6.7</span><span class="sxs-lookup"><span data-stu-id="4bdec-257">6.7</span></span> | <span data-ttu-id="4bdec-258">Datový disk</span><span class="sxs-lookup"><span data-stu-id="4bdec-258">Data disk</span></span> |
| <span data-ttu-id="4bdec-259">CentOS</span><span class="sxs-lookup"><span data-stu-id="4bdec-259">CentOS</span></span> | <span data-ttu-id="4bdec-260">6.6</span><span class="sxs-lookup"><span data-stu-id="4bdec-260">6.6</span></span> | <span data-ttu-id="4bdec-261">Datový disk</span><span class="sxs-lookup"><span data-stu-id="4bdec-261">Data disk</span></span> |
| <span data-ttu-id="4bdec-262">CentOS</span><span class="sxs-lookup"><span data-stu-id="4bdec-262">CentOS</span></span> | <span data-ttu-id="4bdec-263">6.5</span><span class="sxs-lookup"><span data-stu-id="4bdec-263">6.5</span></span> | <span data-ttu-id="4bdec-264">Datový disk</span><span class="sxs-lookup"><span data-stu-id="4bdec-264">Data disk</span></span> |
| <span data-ttu-id="4bdec-265">openSUSE</span><span class="sxs-lookup"><span data-stu-id="4bdec-265">openSUSE</span></span> | <span data-ttu-id="4bdec-266">13.2</span><span class="sxs-lookup"><span data-stu-id="4bdec-266">13.2</span></span> | <span data-ttu-id="4bdec-267">Datový disk</span><span class="sxs-lookup"><span data-stu-id="4bdec-267">Data disk</span></span> |
| <span data-ttu-id="4bdec-268">SLES</span><span class="sxs-lookup"><span data-stu-id="4bdec-268">SLES</span></span> | <span data-ttu-id="4bdec-269">12 SP1</span><span class="sxs-lookup"><span data-stu-id="4bdec-269">12 SP1</span></span> | <span data-ttu-id="4bdec-270">Datový disk</span><span class="sxs-lookup"><span data-stu-id="4bdec-270">Data disk</span></span> |
| <span data-ttu-id="4bdec-271">SLES</span><span class="sxs-lookup"><span data-stu-id="4bdec-271">SLES</span></span> | <span data-ttu-id="4bdec-272">12-SP1 (Premium)</span><span class="sxs-lookup"><span data-stu-id="4bdec-272">12-SP1 (Premium)</span></span> | <span data-ttu-id="4bdec-273">Datový disk</span><span class="sxs-lookup"><span data-stu-id="4bdec-273">Data disk</span></span> |
| <span data-ttu-id="4bdec-274">SLES</span><span class="sxs-lookup"><span data-stu-id="4bdec-274">SLES</span></span> | <span data-ttu-id="4bdec-275">HPC 12</span><span class="sxs-lookup"><span data-stu-id="4bdec-275">HPC 12</span></span> | <span data-ttu-id="4bdec-276">Datový disk</span><span class="sxs-lookup"><span data-stu-id="4bdec-276">Data disk</span></span> |
| <span data-ttu-id="4bdec-277">SLES</span><span class="sxs-lookup"><span data-stu-id="4bdec-277">SLES</span></span> | <span data-ttu-id="4bdec-278">11-SP4 (Premium)</span><span class="sxs-lookup"><span data-stu-id="4bdec-278">11-SP4 (Premium)</span></span> | <span data-ttu-id="4bdec-279">Datový disk</span><span class="sxs-lookup"><span data-stu-id="4bdec-279">Data disk</span></span> |
| <span data-ttu-id="4bdec-280">SLES</span><span class="sxs-lookup"><span data-stu-id="4bdec-280">SLES</span></span> | <span data-ttu-id="4bdec-281">11 SP4</span><span class="sxs-lookup"><span data-stu-id="4bdec-281">11 SP4</span></span> | <span data-ttu-id="4bdec-282">Datový disk</span><span class="sxs-lookup"><span data-stu-id="4bdec-282">Data disk</span></span> |

* <span data-ttu-id="4bdec-283">Azure Disk Encryption vyžaduje, aby váš trezor klíčů a virtuální počítače jsou umístěny ve hello stejné oblasti Azure a předplatné.</span><span class="sxs-lookup"><span data-stu-id="4bdec-283">Azure Disk Encryption requires that your key vault and VMs reside in hello same Azure region and subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="4bdec-284">Konfigurace prostředků hello v oblastech způsobí selhání při povolování funkce Azure Disk Encryption hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-284">Configuring hello resources in separate regions causes a failure in enabling hello Azure Disk Encryption feature.</span></span>

* <span data-ttu-id="4bdec-285">tooset registrace a konfigurace trezoru klíčů pro Azure Disk Encryption, najdete v tématu **nastavit registrace a konfigurace trezoru klíčů pro Azure Disk Encryption** v hello *požadavky* tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="4bdec-285">tooset up and configure your key vault for Azure Disk Encryption, see section **Set up and configure your key vault for Azure Disk Encryption** in hello *Prerequisites* section of this article.</span></span>
* <span data-ttu-id="4bdec-286">tooset až a nakonfigurovat aplikace Azure AD ve službě Azure Active directory pro Azure Disk Encryption, najdete v tématu **nastavení aplikace hello Azure AD ve službě Azure Active Directory** v hello *požadavky* části tohoto článku.</span><span class="sxs-lookup"><span data-stu-id="4bdec-286">tooset up and configure Azure AD application in Azure Active directory for Azure Disk Encryption, see section **Set up hello Azure AD application in Azure Active Directory** in hello *Prerequisites* section of this article.</span></span>
* <span data-ttu-id="4bdec-287">tooset registrace a konfigurace zásad přístupu hello trezoru klíčů pro aplikaci hello Azure AD, najdete v tématu **nastavte hello trezoru klíčů zásady přístupu pro aplikace hello Azure AD** v hello *požadavky* část v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="4bdec-287">tooset up and configure hello key vault access policy for hello Azure AD application, see section **Set up hello key vault access policy for hello Azure AD application** in hello *Prerequisites* section of this article.</span></span>
* <span data-ttu-id="4bdec-288">tooprepare předem šifrované Windows virtuální pevný disk, najdete v tématu **připravit předem šifrované virtuálního pevného disku Windows** v hello *příloha*.</span><span class="sxs-lookup"><span data-stu-id="4bdec-288">tooprepare a pre-encrypted Windows VHD, see section **Prepare a pre-encrypted Windows VHD** in hello *Appendix*.</span></span>
* <span data-ttu-id="4bdec-289">tooprepare předem šifrované Linux virtuální pevný disk, najdete v tématu **připravit předem šifrované VHD Linux** v hello *příloha*.</span><span class="sxs-lookup"><span data-stu-id="4bdec-289">tooprepare a pre-encrypted Linux VHD, see section **Prepare a pre-encrypted Linux VHD** in hello *Appendix*.</span></span>
* <span data-ttu-id="4bdec-290">potřebuje přístup toohello šifrovacích klíčů nebo tajných klíčů v váš trezor klíčů toomake zprostředkovatele Hello platformy Azure je k dispozici toohello virtuální počítač, když se spustí a dešifruje svazek virtuálního počítače OS hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-290">hello Azure platform needs access toohello encryption keys or secrets in your key vault toomake them available toohello virtual machine when it boots and decrypts hello virtual machine OS volume.</span></span> <span data-ttu-id="4bdec-291">toogrant oprávnění tooAzure platformy, sada hello **EnabledForDiskEncryption** vlastnost v trezoru klíčů hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-291">toogrant permissions tooAzure platform, set hello **EnabledForDiskEncryption** property in hello key vault.</span></span> <span data-ttu-id="4bdec-292">Další informace najdete v tématu **nastavit registrace a konfigurace trezoru klíčů pro Azure Disk Encryption** v hello příloha.</span><span class="sxs-lookup"><span data-stu-id="4bdec-292">For more information, see **Set up and configure your key vault for Azure Disk Encryption** in hello Appendix.</span></span>
* <span data-ttu-id="4bdec-293">Tajný klíč trezoru klíčů a adresy URL KEK musí být verzí.</span><span class="sxs-lookup"><span data-stu-id="4bdec-293">Your key vault secret and KEK URLs must be versioned.</span></span> <span data-ttu-id="4bdec-294">Azure vynucuje toto omezení Správa verzí.</span><span class="sxs-lookup"><span data-stu-id="4bdec-294">Azure enforces this restriction of versioning.</span></span> <span data-ttu-id="4bdec-295">Platný tajný klíč a adresy URL KEK najdete v tématu hello následující příklady:</span><span class="sxs-lookup"><span data-stu-id="4bdec-295">For valid secret and KEK URLs, see hello following examples:</span></span>

  * <span data-ttu-id="4bdec-296">Příklad platnou adresu URL tajný: *https://contosovault.vault.azure.net/secrets/BitLockerEncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span><span class="sxs-lookup"><span data-stu-id="4bdec-296">Example of a valid secret URL:   *https://contosovault.vault.azure.net/secrets/BitLockerEncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span></span>
  * <span data-ttu-id="4bdec-297">Příklad platnou adresu URL KEK: *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span><span class="sxs-lookup"><span data-stu-id="4bdec-297">Example of a valid KEK URL:   *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span></span>

* <span data-ttu-id="4bdec-298">Azure Disk Encryption nepodporuje zadání čísla portu jako součást tajné klíče trezoru klíčů a adresy URL KEK.</span><span class="sxs-lookup"><span data-stu-id="4bdec-298">Azure Disk Encryption does not support specifying port numbers as part of key vault secrets and KEK URLs.</span></span> <span data-ttu-id="4bdec-299">Příklady adres URL nepodporovaný a podporované trezoru klíčů najdete v tématu hello následující:</span><span class="sxs-lookup"><span data-stu-id="4bdec-299">For examples of non-supported and supported key vault URLs, see hello following:</span></span>

  * <span data-ttu-id="4bdec-300">Adresa URL nemůže být přijata trezoru klíčů *https://contosovault.vault.azure.net:443 nebo tajných klíčů nebo contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span><span class="sxs-lookup"><span data-stu-id="4bdec-300">Unacceptable key vault URL  *https://contosovault.vault.azure.net:443/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span></span>
  * <span data-ttu-id="4bdec-301">Adresa URL přijatelné trezoru klíčů: *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span><span class="sxs-lookup"><span data-stu-id="4bdec-301">Acceptable key vault URL:   *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*</span></span>

* <span data-ttu-id="4bdec-302">Funkce Azure Disk Encryption hello tooenable, hello virtuální počítače IaaS musí splňovat následující požadavky na konfiguraci koncového bodu sítě hello:</span><span class="sxs-lookup"><span data-stu-id="4bdec-302">tooenable hello Azure Disk Encryption feature, hello IaaS VMs must meet hello following network endpoint configuration requirements:</span></span>
  * <span data-ttu-id="4bdec-303">tooget tokenu tooconnect tooyour trezoru klíčů, hello virtuálních počítačů IaaS musí být schopný tooconnect tooan Azure Active Directory koncový bod, \[login.microsoftonline.com\].</span><span class="sxs-lookup"><span data-stu-id="4bdec-303">tooget a token tooconnect tooyour key vault, hello IaaS VM must be able tooconnect tooan Azure Active Directory endpoint, \[login.microsoftonline.com\].</span></span>
  * <span data-ttu-id="4bdec-304">toowrite hello šifrovací klíče tooyour trezoru klíčů, hello virtuálních počítačů IaaS musí být schopný tooconnect toohello trezoru klíčů koncový bod.</span><span class="sxs-lookup"><span data-stu-id="4bdec-304">toowrite hello encryption keys tooyour key vault, hello IaaS VM must be able tooconnect toohello key vault endpoint.</span></span>
  * <span data-ttu-id="4bdec-305">Hello virtuálních počítačů IaaS musí být schopný tooconnect tooan úložiště Azure koncový bod, hostitelé hello úložiště rozšíření Azure a účet úložiště Azure, hostitelé hello soubory virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="4bdec-305">hello IaaS VM must be able tooconnect tooan Azure storage endpoint that hosts hello Azure extension repository and an Azure storage account that hosts hello VHD files.</span></span>

  > [!NOTE]
  > <span data-ttu-id="4bdec-306">Pokud vaše zásady zabezpečení omezuje přístup z virtuálních počítačů Azure toohello Internetu, můžete vyřešit hello předcházející URI a nakonfigurovat konkrétní pravidlo tooallow odchozí připojení toohello IP adresy.</span><span class="sxs-lookup"><span data-stu-id="4bdec-306">If your security policy limits access from Azure VMs toohello Internet, you can resolve hello preceding URI and configure a specific rule tooallow outbound connectivity toohello IPs.</span></span>
  >
  ><span data-ttu-id="4bdec-307">tooconfigure a přístup k Azure Key Vault za bránou firewall (https://docs.microsoft.com/en-us/azure/key-vault/key-vault-access-behind-firewall)</span><span class="sxs-lookup"><span data-stu-id="4bdec-307">tooconfigure and access Azure Key Vault behind a firewall(https://docs.microsoft.com/en-us/azure/key-vault/key-vault-access-behind-firewall)</span></span>

* <span data-ttu-id="4bdec-308">Použijte nejnovější verzi Azure PowerShell SDK verze tooconfigure Azure Disk Encryption hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-308">Use hello latest version of Azure PowerShell SDK version tooconfigure Azure Disk Encryption.</span></span> <span data-ttu-id="4bdec-309">Stáhněte si nejnovější verzi hello [verzi prostředí Azure PowerShell](https://github.com/Azure/azure-powershell/releases)</span><span class="sxs-lookup"><span data-stu-id="4bdec-309">Download hello latest version of [Azure PowerShell release](https://github.com/Azure/azure-powershell/releases)</span></span>

 > [!NOTE]
  > <span data-ttu-id="4bdec-310">Azure Disk Encryption není podporována na [Azure PowerShell SDK verze 1.1.0](https://github.com/Azure/azure-powershell/releases/tag/v1.1.0-January2016).</span><span class="sxs-lookup"><span data-stu-id="4bdec-310">Azure Disk Encryption is not supported on [Azure PowerShell SDK version 1.1.0](https://github.com/Azure/azure-powershell/releases/tag/v1.1.0-January2016).</span></span> <span data-ttu-id="4bdec-311">Pokud se vám zobrazuje chybu související toousing prostředí Azure PowerShell 1.1.0, najdete v části [Azure Disk Encryption chyba související tooAzure prostředí PowerShell 1.1.0](http://blogs.msdn.com/b/azuresecurity/archive/2016/02/10/azure-disk-encryption-error-related-to-azure-powershell-1-1-0.aspx).</span><span class="sxs-lookup"><span data-stu-id="4bdec-311">If you are receiving an error related toousing Azure PowerShell 1.1.0, see [Azure Disk Encryption Error Related tooAzure PowerShell 1.1.0](http://blogs.msdn.com/b/azuresecurity/archive/2016/02/10/azure-disk-encryption-error-related-to-azure-powershell-1-1-0.aspx).</span></span>

* <span data-ttu-id="4bdec-312">toorun libovolnému příkazu příkazového řádku Azure CLI a přidružit ho ke svému předplatnému Azure, je nutné nejprve nainstalovat rozhraní příkazového řádku Azure:</span><span class="sxs-lookup"><span data-stu-id="4bdec-312">toorun any Azure CLI command and associate it with your Azure subscription, you must first install Azure CLI:</span></span>
  * <span data-ttu-id="4bdec-313">tooinstall rozhraní příkazového řádku Azure a přidružit ho ke svému předplatnému Azure, najdete v části [jak tooinstall a konfigurace rozhraní příkazového řádku Azure](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="4bdec-313">tooinstall Azure CLI and associate it with your Azure subscription, see [How tooinstall and configure Azure CLI](../cli-install-nodejs.md).</span></span>
  * <span data-ttu-id="4bdec-314">toouse Azure CLI pro Mac, Linux a Windows pomocí Azure Resource Manageru, najdete v části [rozhraní příkazového řádku Azure v režimu Resource Manager](../virtual-machines/azure-cli-arm-commands.md).</span><span class="sxs-lookup"><span data-stu-id="4bdec-314">toouse Azure CLI for Mac, Linux, and Windows with Azure Resource Manager, see [Azure CLI commands in Resource Manager mode](../virtual-machines/azure-cli-arm-commands.md).</span></span>

* <span data-ttu-id="4bdec-315">Při šifrování se spravovaným diskem, je povinné požadovaných tootake snímek hello spravované disku nebo zálohy disku hello mimo Azure Disk Encryption předchozí tooenabling šifrování.</span><span class="sxs-lookup"><span data-stu-id="4bdec-315">When encrypting a managed disk, it is mandatory prerequisite tootake a snapshot of hello managed disk or a backup of hello disk outside of Azure Disk Encryption prior tooenabling encryption.</span></span>  <span data-ttu-id="4bdec-316">Bez předchozího provedení zálohy na místě jakákoli Neočekávaná chyba při šifrování může vykreslit hello disk a virtuální počítač nedostupný bez možnosti obnovení.</span><span class="sxs-lookup"><span data-stu-id="4bdec-316">Without a backup in place, any unexpected failure during encryption may render hello disk and VM inaccessible without a recovery option.</span></span>  <span data-ttu-id="4bdec-317">Set-AzureRmVMDiskEncryptionExtension není aktuálně zálohovat spravovaných disků a bude chyba, je-li použít u se spravovaným diskem, pokud byl zadán parametr - skipVmBackup hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-317">Set-AzureRmVMDiskEncryptionExtension does not currently back up managed disks and will error if used against a managed disk unless hello -skipVmBackup parameter has been specified.</span></span>  <span data-ttu-id="4bdec-318">Tento parametr je nezabezpečený toouse, není-li zálohu již byl změněn mimo Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="4bdec-318">This parameter is unsafe toouse unless a backup has already been made outside of Azure Disk Encryption.</span></span>   <span data-ttu-id="4bdec-319">Pokud je zadán parametr - skipVmBackup hello, nebude hello rutiny vytvořit záložní kopii předchozí tooencryption hello spravované disku.</span><span class="sxs-lookup"><span data-stu-id="4bdec-319">When hello -skipVmBackup parameter is specified, hello cmdlet will not make a backup of hello managed disk prior tooencryption.</span></span>  <span data-ttu-id="4bdec-320">Z tohoto důvodu bude považován za povinný požadovaných toomake se, že potřeby zálohu hello spravovaného disku virtuálního počítače je v místní předchozí tooenabling Azure Disk Encryption v případě, že je obnovení novější.</span><span class="sxs-lookup"><span data-stu-id="4bdec-320">For this reason, it is considered a mandatory prerequisite toomake sure a backup of hello managed disk VM is in place prior tooenabling Azure Disk Encryption in case recovery is later needed.</span></span>  
> [!NOTE]
 > <span data-ttu-id="4bdec-321">Parametr - skipVmBackup Hello by nikdy nepoužívali, pokud snímek nebo zálohování již byl změněn mimo Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="4bdec-321">hello -skipVmBackup parameter should never be used unless a snapshot or backup has already been made outside of Azure Disk Encryption.</span></span> 

* <span data-ttu-id="4bdec-322">Hello Azure Disk Encryption řešení používá pro virtuální počítače IaaS Windows hello externí ochrany pomocí klíče Bitlockeru.</span><span class="sxs-lookup"><span data-stu-id="4bdec-322">hello Azure Disk Encryption solution uses hello BitLocker external key protector for Windows IaaS VMs.</span></span> <span data-ttu-id="4bdec-323">Pro doménu připojené k virtuální počítače, nesmí push žádné zásady skupiny, které vynutit ochrany pomocí čipu TPM.</span><span class="sxs-lookup"><span data-stu-id="4bdec-323">For domain joined VMs, DO NOT push any group policies that enforce TPM protectors.</span></span> <span data-ttu-id="4bdec-324">Informace o zásadách skupiny hello "Povolit BitLocker bez kompatibilním čipem TPM" najdete v tématu [referenční příručka zásad skupiny Bitlockeru](https://technet.microsoft.com/library/ee706521).</span><span class="sxs-lookup"><span data-stu-id="4bdec-324">For information about hello group policy for “Allow BitLocker without a compatible TPM,” see [BitLocker Group Policy Reference](https://technet.microsoft.com/library/ee706521).</span></span>
* <span data-ttu-id="4bdec-325">Zásady nástroje BitLocker na virtuální počítače připojené k doméně pomocí zásad skupiny vlastní musí zahrnovat hello následující nastavení: `Configure user storage of bitlocker recovery information -> Allow 256-bit recovery key` Azure Disk Encryption se nezdaří, pokud nejsou kompatibilní, nastavení zásad vlastní skupiny pro Bitlocker.</span><span class="sxs-lookup"><span data-stu-id="4bdec-325">Bitlocker policy on domain joined virtual machines with custom group policy must include hello following setting: `Configure user storage of bitlocker recovery information -> Allow 256-bit recovery key`  Azure Disk Encryption will fail when custom group policy settings for Bitlocker are incompatible.</span></span> <span data-ttu-id="4bdec-326">Na počítačích, které neměl hello nastavení správné zásady, použití hello nové zásady, vynucení hello nové zásady tooupdate (gpupdate.exe/Force) a restartujte může být vyžadováno.</span><span class="sxs-lookup"><span data-stu-id="4bdec-326">On machines that did not have hello correct policy setting, applying hello new policy, forcing hello new policy tooupdate (gpupdate.exe /force), and then restarting may be required.</span></span>  
* <span data-ttu-id="4bdec-327">toocreate aplikaci Azure AD vytvoření trezoru klíčů, nebo nastavit existující trezor klíčů a povolit šifrování, najdete v části hello [požadovaných skript prostředí PowerShell Azure Disk Encryption](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).</span><span class="sxs-lookup"><span data-stu-id="4bdec-327">toocreate an Azure AD application, create a key vault, or set up an existing key vault and enable encryption, see hello [Azure Disk Encryption prerequisite PowerShell script](https://github.com/Azure/azure-powershell/blob/master/src/ResourceManager/Compute/Commands.Compute/Extension/AzureDiskEncryption/Scripts/AzureDiskEncryptionPreRequisiteSetup.ps1).</span></span>
* <span data-ttu-id="4bdec-328">požadavky tooconfigure šifrování disku pomocí hello Azure CLI, najdete v části [tento skript Bash](https://github.com/ejarvi/ade-cli-getting-started).</span><span class="sxs-lookup"><span data-stu-id="4bdec-328">tooconfigure disk-encryption prerequisites using hello Azure CLI, see [this Bash script](https://github.com/ejarvi/ade-cli-getting-started).</span></span>
* <span data-ttu-id="4bdec-329">toouse hello Azure Backup service tooback zálohu a obnovení šifrované virtuálních počítačů, když je povolené šifrování s Azure Disk Encryption zašifrovat virtuální počítače pomocí klíče konfigurace Azure Disk Encryption hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-329">toouse hello Azure Backup service tooback up and restore encrypted VMs, when encryption is enabled with Azure Disk Encryption, encrypt your VMs by using hello Azure Disk Encryption key configuration.</span></span> <span data-ttu-id="4bdec-330">Hello služby Backup podporuje virtuální počítače, které jsou šifrované pomocí pouze KEK konfigurace.</span><span class="sxs-lookup"><span data-stu-id="4bdec-330">hello Backup service supports VMs that are encrypted using KEK configuration only.</span></span> <span data-ttu-id="4bdec-331">V tématu [jak šifrované tooback zálohu a obnovení virtuálních počítačů s šifrováním Azure Backup](https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-encryption).</span><span class="sxs-lookup"><span data-stu-id="4bdec-331">See [How tooback up and restore encrypted virtual machines with Azure Backup  encryption](https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-encryption).</span></span>

* <span data-ttu-id="4bdec-332">Při šifrování svazku operačního systému Linux, je momentálně nevyžaduje na konci hello procesu hello Všimněte si, že virtuální počítač restartovat.</span><span class="sxs-lookup"><span data-stu-id="4bdec-332">When encrypting a Linux OS volume, note that a VM restart is currently required at hello end of hello process.</span></span> <span data-ttu-id="4bdec-333">To lze provést prostřednictvím portálu hello, prostředí powershell nebo rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="4bdec-333">This can be done via hello portal, powershell, or CLI.</span></span>   <span data-ttu-id="4bdec-334">průběh hello tootrack šifrování, pravidelně dotazovat vrácený Get-AzureRmVMDiskEncryptionStatus https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus hello stavovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="4bdec-334">tootrack hello progress of encryption, periodically poll hello status message returned by Get-AzureRmVMDiskEncryptionStatus https://docs.microsoft.com/en-us/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus.</span></span>  <span data-ttu-id="4bdec-335">Po dokončení šifrování bude tuto oznámí hello hello stavovou zprávu vrácený tento příkaz.</span><span class="sxs-lookup"><span data-stu-id="4bdec-335">Once encryption is complete, hello hello status message returned by this command will indicate this.</span></span>  <span data-ttu-id="4bdec-336">Například "ProgressMessage: disk operačního systému úspěšně šifrování, restartujte prosím hello virtuální počítač" na tento bod hello virtuálních počítačů může být restartován a použita.</span><span class="sxs-lookup"><span data-stu-id="4bdec-336">For example, "ProgressMessage: OS disk successfully encrypted, please reboot hello VM"  At this point hello VM can be restarted and used.</span></span>  

* <span data-ttu-id="4bdec-337">Azure Disk Encryption pro Linux vyžaduje datové disky toohave systému připojeného souboru v předchozí tooencryption Linux</span><span class="sxs-lookup"><span data-stu-id="4bdec-337">Azure Disk Encryption for Linux requires data disks toohave a mounted file system in Linux prior tooencryption</span></span>

* <span data-ttu-id="4bdec-338">Rekurzivní připojené datových disků nepodporuje hello Azure Disk Encryption pro Linux.</span><span class="sxs-lookup"><span data-stu-id="4bdec-338">Recursively mounted data disks are not supported by hello Azure Disk Encryption for Linux.</span></span> <span data-ttu-id="4bdec-339">Například pokud hello cílovém systému má připojené disku na /foo/bar a pak jiné na /foo/bar/baz hello šifrování /foo/bar/baz bude úspěšné, ale šifrování s/foo nebo se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="4bdec-339">For example, if hello target system has mounted a disk on /foo/bar and then another on /foo/bar/baz, hello encryption of /foo/bar/baz will succeed, but encryption of /foo/bar will fail.</span></span> 

* <span data-ttu-id="4bdec-340">Azure Disk Encryption je podporován pouze v galerii Azure, které jsou podporované Image, které splňují hello zmíněnými požadavky.</span><span class="sxs-lookup"><span data-stu-id="4bdec-340">Azure Disk Encryption is only supported on Azure gallery supported images that meet hello aforementioned prerequisites.</span></span> <span data-ttu-id="4bdec-341">Zákazník vlastními obrázky nejsou podporovány kvůli schématy oddílu toocustom a proces chování, které mohou existovat na těchto bitových kopií.</span><span class="sxs-lookup"><span data-stu-id="4bdec-341">Customer custom images are not supported due toocustom partition schemes and process behaviors that may exist on these images.</span></span> <span data-ttu-id="4bdec-342">Navíc založená i galerie image Virtuálního počítače, který původně splněny požadavky ale byly upraveny po vytvoření může být nekompatibilní.</span><span class="sxs-lookup"><span data-stu-id="4bdec-342">Further, even gallery image based VM's that initially met prerequisites but have been modified after creation may be incompatible.</span></span>  <span data-ttu-id="4bdec-343">Pro, důvod, proč se hello navrhované postup pro šifrování virtuálního počítače s Linuxem je toostart z Galerie vyčištění bitové kopie, šifrování hello virtuálního počítače a poté přidejte vlastní softwaru nebo data toohello virtuální počítač podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="4bdec-343">For that reason, hello suggested procedure for encrypting a Linux VM is toostart from a clean gallery image, encrypt hello VM, and then add custom software or data toohello VM as needed.</span></span>  

> [!NOTE]
> <span data-ttu-id="4bdec-344">Zálohování a obnovení šifrovaných virtuálních počítačů je podporována pouze pro virtuální počítače, které jsou zašifrované s konfigurací KEK hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-344">Backup and restore of encrypted VMs is supported only for VMs that are encrypted with hello KEK configuration.</span></span> <span data-ttu-id="4bdec-345">Není podporována u virtuálních počítačů, které jsou zašifrované bez KEK.</span><span class="sxs-lookup"><span data-stu-id="4bdec-345">It is not supported on VMs that are encrypted without KEK.</span></span> <span data-ttu-id="4bdec-346">KEK je volitelný parametr, který umožňuje virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-346">KEK is an optional parameter that enables VM.</span></span>

#### <a name="set-up-hello-azure-ad-application-in-azure-active-directory"></a><span data-ttu-id="4bdec-347">Nastavit hello aplikaci Azure AD ve službě Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4bdec-347">Set up hello Azure AD application in Azure Active Directory</span></span>
<span data-ttu-id="4bdec-348">Pokud budete potřebovat toobe šifrování povolené na spuštění virtuálního počítače v Azure, Azure Disk Encryption generuje a zapíše hello šifrovací klíče tooyour klíče trezoru.</span><span class="sxs-lookup"><span data-stu-id="4bdec-348">When you need encryption toobe enabled on a running VM in Azure, Azure Disk Encryption generates and writes hello encryption keys tooyour key vault.</span></span> <span data-ttu-id="4bdec-349">Správa šifrovacích klíčů v trezoru klíčů se vyžaduje ověřování Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4bdec-349">Managing encryption keys in your key vault requires Azure AD authentication.</span></span>

<span data-ttu-id="4bdec-350">Pro tento účel vytvořte aplikaci Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4bdec-350">For this purpose, create an Azure AD application.</span></span> <span data-ttu-id="4bdec-351">Najdete podrobné pokyny pro registraci aplikace v hello "Get Identity pro hello aplikace" v tématu hello příspěvku na blogu [Azure Key Vault - krok za krokem](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx).</span><span class="sxs-lookup"><span data-stu-id="4bdec-351">You can find detailed steps for registering an application in hello “Get an Identity for hello Application” section of hello blog post [Azure Key Vault - Step by Step](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx).</span></span> <span data-ttu-id="4bdec-352">Tento příspěvek také obsahuje řadu užitečné příklady pro nastavení a konfiguraci trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-352">This post also contains a number of helpful examples for setting up and configuring your key vault.</span></span> <span data-ttu-id="4bdec-353">Pro účely ověření můžete použít buď ověřování na základě tajný klíč klienta nebo ověřování klientů na základě certifikátů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4bdec-353">For authentication purposes, you can use either client secret-based authentication or client certificate-based Azure AD authentication.</span></span>

#### <a name="client-secret-based-authentication-for-azure-ad"></a><span data-ttu-id="4bdec-354">Ověřování na základě tajný klíč klienta pro Azure AD</span><span class="sxs-lookup"><span data-stu-id="4bdec-354">Client secret-based authentication for Azure AD</span></span>
<span data-ttu-id="4bdec-355">Hello oddíly, které následují, můžete nakonfigurovat ověřování na základě tajný klíč klienta pro Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4bdec-355">hello sections that follow can help you configure a client secret-based authentication for Azure AD.</span></span>

##### <a name="create-an-azure-ad-application-by-using-azure-powershell"></a><span data-ttu-id="4bdec-356">Vytvořit aplikaci Azure AD pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="4bdec-356">Create an Azure AD application by using Azure PowerShell</span></span>
<span data-ttu-id="4bdec-357">Použijte hello následující toocreate rutiny prostředí PowerShell aplikaci Azure AD:</span><span class="sxs-lookup"><span data-stu-id="4bdec-357">Use hello following PowerShell cmdlet toocreate an Azure AD application:</span></span>

    $aadClientSecret = "yourSecret"
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -Password $aadClientSecret
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

> [!NOTE]
> <span data-ttu-id="4bdec-358">$azureAdApplication.ApplicationId hello Azure AD ClientID a $aadClientSecret je sdílený tajný klíč klienta hello by měl použít novější tooenable Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="4bdec-358">$azureAdApplication.ApplicationId is hello Azure AD ClientID and $aadClientSecret is hello client secret that you should use later tooenable Azure Disk Encryption.</span></span> <span data-ttu-id="4bdec-359">Tajný klíč klienta Azure AD hello zabezpečit správně.</span><span class="sxs-lookup"><span data-stu-id="4bdec-359">Safeguard hello Azure AD client secret appropriately.</span></span>

##### <a name="setting-up-hello-azure-ad-client-id-and-secret-from-hello-azure-classic-portal"></a><span data-ttu-id="4bdec-360">Nastavení hello Azure AD ID klienta a tajný klíč z portálu Azure classic hello</span><span class="sxs-lookup"><span data-stu-id="4bdec-360">Setting up hello Azure AD client ID and secret from hello Azure classic portal</span></span>
<span data-ttu-id="4bdec-361">Můžete vytvořit také ID klienta Azure AD a tajný klíč pomocí hello [portál Azure classic]( https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="4bdec-361">You can also set up your Azure AD client ID and secret by using hello [Azure classic portal]( https://manage.windowsazure.com).</span></span> <span data-ttu-id="4bdec-362">tooperform tato úloha, hello následující:</span><span class="sxs-lookup"><span data-stu-id="4bdec-362">tooperform this task, do hello following:</span></span>

1. <span data-ttu-id="4bdec-363">Klikněte na tlačítko hello **služby Active Directory** kartě.</span><span class="sxs-lookup"><span data-stu-id="4bdec-363">Click hello **Active Directory** tab.</span></span>

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig3.png)

2. <span data-ttu-id="4bdec-365">Klikněte na tlačítko **přidat aplikaci**a pak zadejte název aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-365">Click **Add Application**, and then type hello application name.</span></span>

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig4.png)

3. <span data-ttu-id="4bdec-367">Klikněte na tlačítko se šipkou hello a pak nakonfigurujte vlastnosti aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-367">Click hello arrow button, and then configure hello application properties.</span></span>

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig5.png)

4. <span data-ttu-id="4bdec-369">Klikněte na tlačítko zaškrtnutí hello v dolním rohu toofinish hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-369">Click hello check mark in hello lower left corner toofinish.</span></span> <span data-ttu-id="4bdec-370">Zobrazí se stránka konfigurace aplikace Hello a hello ID klienta Azure AD se zobrazí v dolní části hello hello stránky.</span><span class="sxs-lookup"><span data-stu-id="4bdec-370">hello application configuration page appears, and hello Azure AD client ID is displayed at hello bottom of hello page.</span></span>

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig6.png)

5. <span data-ttu-id="4bdec-372">Uložit tajný klíč klienta Azure AD hello kliknutím hello **Uložit** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="4bdec-372">Save hello Azure AD client secret by clicking hello **Save** button.</span></span> <span data-ttu-id="4bdec-373">Poznámka: sdílený tajný klíč klienta Azure AD hello hello klíče textového pole.</span><span class="sxs-lookup"><span data-stu-id="4bdec-373">Note hello Azure AD client secret in hello keys text box.</span></span> <span data-ttu-id="4bdec-374">Zabezpečit správně.</span><span class="sxs-lookup"><span data-stu-id="4bdec-374">Safeguard it appropriately.</span></span>

 ![Azure Disk Encryption](./media/azure-security-disk-encryption/disk-encryption-fig7.png)

 > [!NOTE]
 > <span data-ttu-id="4bdec-376">Hello předcházející toku nepodporuje hello portál Azure classic.</span><span class="sxs-lookup"><span data-stu-id="4bdec-376">hello preceding flow is not supported on hello Azure classic portal.</span></span>

##### <a name="use-an-existing-application"></a><span data-ttu-id="4bdec-377">Použít stávající aplikaci</span><span class="sxs-lookup"><span data-stu-id="4bdec-377">Use an existing application</span></span>
<span data-ttu-id="4bdec-378">tooexecute hello následující příkazy, získání a používání hello [modulu Azure AD PowerShell](https://technet.microsoft.com/library/jj151815.aspx).</span><span class="sxs-lookup"><span data-stu-id="4bdec-378">tooexecute hello following commands, obtain and use hello [Azure AD PowerShell module](https://technet.microsoft.com/library/jj151815.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="4bdec-379">Následující příkazy Hello musí provést z nové okno prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4bdec-379">hello following commands must be executed from a new PowerShell window.</span></span> <span data-ttu-id="4bdec-380">Nepoužívejte Azure PowerShell nebo hello Azure Resource Manager okno tooexecute hello příkazů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-380">Do not use Azure PowerShell or hello Azure Resource Manager window tooexecute hello commands.</span></span> <span data-ttu-id="4bdec-381">Doporučujeme vám tento přístup, protože jsou tyto rutiny v modulu MSOnline hello nebo Azure AD PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4bdec-381">We recommend this approach because these cmdlets are in hello MSOnline module or Azure AD PowerShell.</span></span>

    $clientSecret = ‘<yourAadClientSecret>’
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type password -Value $clientSecret

#### <a name="certificate-based-authentication-for-azure-ad"></a><span data-ttu-id="4bdec-382">Ověřování pomocí certifikátů pro Azure AD</span><span class="sxs-lookup"><span data-stu-id="4bdec-382">Certificate-based authentication for Azure AD</span></span>
> [!NOTE]
> <span data-ttu-id="4bdec-383">Na virtuální počítače s Linuxem se aktuálně nepodporuje ověřování pomocí certifikátů Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4bdec-383">Azure AD certificate-based authentication is currently not supported on Linux VMs.</span></span>

<span data-ttu-id="4bdec-384">Hello oddíly, které následují zobrazit jak tooconfigure ověřování pomocí certifikátů pro Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4bdec-384">hello sections that follow show how tooconfigure a certificate-based authentication for Azure AD.</span></span>

##### <a name="create-an-azure-ad-application"></a><span data-ttu-id="4bdec-385">Vytvořit aplikaci Azure AD</span><span class="sxs-lookup"><span data-stu-id="4bdec-385">Create an Azure AD application</span></span>
<span data-ttu-id="4bdec-386">toocreate aplikaci Azure AD, spusťte následující rutiny prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="4bdec-386">toocreate an Azure AD application, execute hello following PowerShell cmdlets:</span></span>

> [!NOTE]
> <span data-ttu-id="4bdec-387">Nahraďte hello následující `yourpassword` řetězce s zabezpečeného hesla a chránit hello heslo.</span><span class="sxs-lookup"><span data-stu-id="4bdec-387">Replace hello following `yourpassword` string with your secure password, and safeguard hello password.</span></span>

    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate("C:\certificates\examplecert.pfx", "yourpassword")
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -KeyValue $keyValue -KeyType AsymmetricX509Cert
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

<span data-ttu-id="4bdec-388">Po dokončení tohoto kroku nahrát trezoru klíčů tooyour soubor PFX a povolte toodeploy zásad přístupu hello tento certifikát tooa virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-388">After you finish this step, upload a PFX file tooyour key vault and enable hello access policy needed toodeploy that certificate tooa VM.</span></span>

##### <a name="use-an-existing-azure-ad-application"></a><span data-ttu-id="4bdec-389">Použít existující aplikaci Azure AD</span><span class="sxs-lookup"><span data-stu-id="4bdec-389">Use an existing Azure AD application</span></span>
<span data-ttu-id="4bdec-390">Při konfiguraci ověřování pomocí certifikátů pro existující aplikace, použijte rutiny prostředí PowerShell hello tady uvedené.</span><span class="sxs-lookup"><span data-stu-id="4bdec-390">If you are configuring certificate-based authentication for an existing application, use hello PowerShell cmdlets shown here.</span></span> <span data-ttu-id="4bdec-391">Být jisti tooexecute jim nové okno prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4bdec-391">Be sure tooexecute them from a new PowerShell window.</span></span>

    $certLocalPath = 'C:\certs\myaadapp.cer'
    $aadClientID = '<Client ID of your Azure AD application>'
    connect-msolservice
    $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate
    $cer.Import($certLocalPath)
    $binCert = $cer.GetRawCertData()
    $credValue = [System.Convert]::ToBase64String($binCert);
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type asymmetric -Value $credValue -Usage verify

<span data-ttu-id="4bdec-392">Po dokončení tohoto kroku nahrát trezoru klíčů tooyour soubor PFX a povolte hello zásady přístupu, které je potřeba toodeploy hello certifikát tooa virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-392">After you finish this step, upload a PFX file tooyour key vault and enable hello access policy that's needed toodeploy hello certificate tooa VM.</span></span>

##### <a name="upload-a-pfx-file-tooyour-key-vault"></a><span data-ttu-id="4bdec-393">Nahrát trezoru klíčů tooyour souboru PFX</span><span class="sxs-lookup"><span data-stu-id="4bdec-393">Upload a PFX file tooyour key vault</span></span>
<span data-ttu-id="4bdec-394">Podrobné vysvětlení tohoto procesu najdete v tématu [hello oficiální Blog týmu klíč trezoru Azure](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx).</span><span class="sxs-lookup"><span data-stu-id="4bdec-394">For a detailed explanation of this process, see [hello Official Azure Key Vault Team Blog](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx).</span></span> <span data-ttu-id="4bdec-395">Hello následující rutiny prostředí PowerShell jsou však všechny, které potřebujete pro úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-395">However, hello following PowerShell cmdlets are all you need for hello task.</span></span> <span data-ttu-id="4bdec-396">Být jisti tooexecute je z konzoly Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="4bdec-396">Be sure tooexecute them from Azure PowerShell console.</span></span>

> [!NOTE]
> <span data-ttu-id="4bdec-397">Nahraďte hello následující `yourpassword` řetězce s zabezpečeného hesla a chránit hello heslo.</span><span class="sxs-lookup"><span data-stu-id="4bdec-397">Replace hello following `yourpassword` string with your secure password, and safeguard hello password.</span></span>

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

##### <a name="deploy-a-certificate-in-your-key-vault-tooan-existing-vm"></a><span data-ttu-id="4bdec-398">Nasaďte certifikát v váš trezor klíčů tooan existující virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="4bdec-398">Deploy a certificate in your key vault tooan existing VM</span></span>
<span data-ttu-id="4bdec-399">Po dokončení nahrávání hello PFX, nasaďte certifikát v tooan trezoru klíčů hello existující virtuální počítač s hello následující:</span><span class="sxs-lookup"><span data-stu-id="4bdec-399">After you finish uploading hello PFX, deploy a certificate in hello key vault tooan existing VM with hello following:</span></span>
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

#### <a name="set-up-hello-key-vault-access-policy-for-hello-azure-ad-application"></a><span data-ttu-id="4bdec-400">Nastavte zásady přístupu hello trezoru klíčů pro aplikaci hello Azure AD</span><span class="sxs-lookup"><span data-stu-id="4bdec-400">Set up hello key vault access policy for hello Azure AD application</span></span>
<span data-ttu-id="4bdec-401">Aplikace Azure AD musí práva tooaccess hello klíčů nebo tajných klíčů v trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-401">Your Azure AD application needs rights tooaccess hello keys or secrets in hello vault.</span></span> <span data-ttu-id="4bdec-402">Použití hello [ `Set-AzureKeyVaultAccessPolicy` ](/powershell/module/azure/set-azurekeyvaultaccesspolicy?view=azuresmps-3.7.0) rutiny toogrant oprávnění toohello aplikace, pomocí ID klienta hello, (který byl vygenerován při byl zaregistrován aplikace hello) jako hello _– ServicePrincipalName_ Hodnota parametru.</span><span class="sxs-lookup"><span data-stu-id="4bdec-402">Use hello [`Set-AzureKeyVaultAccessPolicy`](/powershell/module/azure/set-azurekeyvaultaccesspolicy?view=azuresmps-3.7.0) cmdlet toogrant permissions toohello application, using hello client ID (which was generated when hello application was registered) as hello _–ServicePrincipalName_ parameter value.</span></span> <span data-ttu-id="4bdec-403">toolearn více, najdete v příspěvku blogu hello [Azure Key Vault - krok za krokem](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx).</span><span class="sxs-lookup"><span data-stu-id="4bdec-403">toolearn more, see hello blog post [Azure Key Vault - Step by Step](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx).</span></span> <span data-ttu-id="4bdec-404">Tady je příklad, jak tooperform tato úloha pomocí prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="4bdec-404">Here is an example of how tooperform this task via PowerShell:</span></span>

    $keyVaultName = '<yourKeyVaultName>'
    $aadClientID = '<yourAadAppClientID>'
    $rgname = '<yourResourceGroup>'
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys 'WrapKey' -PermissionsToSecrets 'Set' -ResourceGroupName $rgname

> [!NOTE]
> <span data-ttu-id="4bdec-405">Azure Disk Encryption vyžaduje, abyste tooconfigure hello následující aplikace klienta tooyour Azure AD zásady přístupu: _WrapKey_ a _nastavit_ oprávnění.</span><span class="sxs-lookup"><span data-stu-id="4bdec-405">Azure Disk Encryption requires you tooconfigure hello following access policies tooyour Azure AD client application: _WrapKey_ and _Set_ permissions.</span></span>

## <a name="terminology"></a><span data-ttu-id="4bdec-406">Terminologie</span><span class="sxs-lookup"><span data-stu-id="4bdec-406">Terminology</span></span>
<span data-ttu-id="4bdec-407">některé běžné podmínky hello používá tuto technologii, použijte hello následující tabulka terminologie toounderstand:</span><span class="sxs-lookup"><span data-stu-id="4bdec-407">toounderstand some of hello common terms used by this technology, use hello following terminology table:</span></span>

| <span data-ttu-id="4bdec-408">Terminologie</span><span class="sxs-lookup"><span data-stu-id="4bdec-408">Terminology</span></span> | <span data-ttu-id="4bdec-409">Definice</span><span class="sxs-lookup"><span data-stu-id="4bdec-409">Definition</span></span> |
| --- | --- |
| <span data-ttu-id="4bdec-410">Azure AD</span><span class="sxs-lookup"><span data-stu-id="4bdec-410">Azure AD</span></span> | <span data-ttu-id="4bdec-411">Azure AD je [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="4bdec-411">Azure AD is [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).</span></span> <span data-ttu-id="4bdec-412">Účet Azure AD je předpokladem pro ověřování, ukládání a načítání tajné klíče z trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-412">An Azure AD account is a prerequisite for authenticating, storing, and retrieving secrets from a key vault.</span></span> |
| <span data-ttu-id="4bdec-413">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="4bdec-413">Azure Key Vault</span></span> | <span data-ttu-id="4bdec-414">Key Vault je služba kryptografické klíče správy, která je založena na moduly ověřit Federal Information Processing Standards FIPS hardwarového zabezpečení, které v zájmu ochrany kryptografické klíče a tajné klíče citlivé.</span><span class="sxs-lookup"><span data-stu-id="4bdec-414">Key Vault is a cryptographic, key management service that's based on Federal Information Processing Standards (FIPS)-validated hardware security modules, which help safeguard your cryptographic keys and sensitive secrets.</span></span> <span data-ttu-id="4bdec-415">Další informace najdete v tématu [Key Vault](https://azure.microsoft.com/services/key-vault/) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="4bdec-415">For more information, see [Key Vault](https://azure.microsoft.com/services/key-vault/) documentation.</span></span> |
| <span data-ttu-id="4bdec-416">ARM</span><span class="sxs-lookup"><span data-stu-id="4bdec-416">ARM</span></span> | <span data-ttu-id="4bdec-417">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="4bdec-417">Azure Resource Manager</span></span> |
| <span data-ttu-id="4bdec-418">Nástroj BitLocker</span><span class="sxs-lookup"><span data-stu-id="4bdec-418">BitLocker</span></span> |<span data-ttu-id="4bdec-419">[Nástroj BitLocker](https://technet.microsoft.com/library/hh831713.aspx) je rozpoznána odvětví Windows svazku šifrovací technologie, která se používá šifrování disku tooenable na virtuální počítače IaaS Windows.</span><span class="sxs-lookup"><span data-stu-id="4bdec-419">[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) is an industry-recognized Windows volume encryption technology that's used tooenable disk encryption on Windows IaaS VMs.</span></span> |
| <span data-ttu-id="4bdec-420">BEK</span><span class="sxs-lookup"><span data-stu-id="4bdec-420">BEK</span></span> | <span data-ttu-id="4bdec-421">Nástroj BitLocker šifrovací klíče jsou použité tooencrypt hello OS spouštěcí svazek a datové svazky.</span><span class="sxs-lookup"><span data-stu-id="4bdec-421">BitLocker encryption keys are used tooencrypt hello OS boot volume and data volumes.</span></span> <span data-ttu-id="4bdec-422">klíče Bitlockeru Hello chráněna jako tajných klíčů v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-422">hello BitLocker keys are safeguarded in a key vault as secrets.</span></span> |
| <span data-ttu-id="4bdec-423">Rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="4bdec-423">CLI</span></span> | <span data-ttu-id="4bdec-424">V tématu [rozhraní příkazového řádku Azure](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="4bdec-424">See [Azure command-line interface](../cli-install-nodejs.md).</span></span> |
| <span data-ttu-id="4bdec-425">DM-Crypt</span><span class="sxs-lookup"><span data-stu-id="4bdec-425">DM-Crypt</span></span> |<span data-ttu-id="4bdec-426">[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) je podsystém hello systémem Linux, transparentní-šifrování disku, který se používá šifrování disku tooenable na virtuální počítače IaaS Linux.</span><span class="sxs-lookup"><span data-stu-id="4bdec-426">[DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) is hello Linux-based, transparent disk-encryption subsystem that's used tooenable disk encryption on Linux IaaS VMs.</span></span> |
| <span data-ttu-id="4bdec-427">KEK</span><span class="sxs-lookup"><span data-stu-id="4bdec-427">KEK</span></span> | <span data-ttu-id="4bdec-428">Klíče šifrovací klíč je hello asymetrický klíč (RSA 2048), můžete použít tooprotect nebo zabalení hello tajný klíč.</span><span class="sxs-lookup"><span data-stu-id="4bdec-428">Key encryption key is hello asymmetric key (RSA 2048) that you can use tooprotect or wrap hello secret.</span></span> <span data-ttu-id="4bdec-429">Můžete zadat hardwaru zabezpečení (HSM) moduly-chráněný klíč, nebo softwarově chráněný klíč.</span><span class="sxs-lookup"><span data-stu-id="4bdec-429">You can provide a hardware security modules (HSM)-protected key or software-protected key.</span></span> <span data-ttu-id="4bdec-430">Další podrobnosti najdete v tématu [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="4bdec-430">For more details, see [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) documentation.</span></span> |
| <span data-ttu-id="4bdec-431">PS rutiny</span><span class="sxs-lookup"><span data-stu-id="4bdec-431">PS cmdlets</span></span> | <span data-ttu-id="4bdec-432">V tématu [rutin prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4bdec-432">See [Azure PowerShell cmdlets](/powershell/azure/overview).</span></span> |

### <a name="set-up-and-configure-your-key-vault-for-azure-disk-encryption"></a><span data-ttu-id="4bdec-433">Nastavení a konfigurace trezoru klíčů pro Azure Disk Encryption</span><span class="sxs-lookup"><span data-stu-id="4bdec-433">Set up and configure your key vault for Azure Disk Encryption</span></span>
<span data-ttu-id="4bdec-434">Azure Disk Encryption pomáhá chránit hello šifrování disku klíče a tajné klíče v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-434">Azure Disk Encryption helps safeguard hello disk-encryption keys and secrets in your key vault.</span></span> <span data-ttu-id="4bdec-435">tooset do trezoru klíčů pro Azure Disk Encryption, dokončení hello kroky v každé z hello následující části.</span><span class="sxs-lookup"><span data-stu-id="4bdec-435">tooset up your key vault for Azure Disk Encryption, complete hello steps in each of hello following sections.</span></span>

#### <a name="create-a-key-vault"></a><span data-ttu-id="4bdec-436">Vytvořte trezor klíčů</span><span class="sxs-lookup"><span data-stu-id="4bdec-436">Create a key vault</span></span>
<span data-ttu-id="4bdec-437">toocreate trezoru klíčů, použijte jednu z hello následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="4bdec-437">toocreate a key vault, use one of hello following options:</span></span>

* [<span data-ttu-id="4bdec-438">"101-Key-trezoru-vytvořit" šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="4bdec-438">"101-Key-Vault-Create" Resource Manager template</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create)
* [<span data-ttu-id="4bdec-439">Rutiny Azure PowerShell trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="4bdec-439">Azure PowerShell key vault cmdlets</span></span>](/powershell/module/azurerm.keyvault/#key_vault)
* <span data-ttu-id="4bdec-440">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="4bdec-440">Azure Resource Manager</span></span>
* <span data-ttu-id="4bdec-441">Jak příliš[zabezpečit váš trezor klíčů](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-secure-your-key-vault)</span><span class="sxs-lookup"><span data-stu-id="4bdec-441">How too[Secure your key vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-secure-your-key-vault)</span></span>

> [!NOTE]
> <span data-ttu-id="4bdec-442">Pokud jste již nastavili trezoru klíčů pro vaše předplatné, toohello další část přeskočte.</span><span class="sxs-lookup"><span data-stu-id="4bdec-442">If you have already set up a key vault for your subscription, skip toohello next section.</span></span>

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig1.png)

#### <a name="set-up-a-key-encryption-key-optional"></a><span data-ttu-id="4bdec-444">Nastavení klíče šifrovací klíč (volitelné)</span><span class="sxs-lookup"><span data-stu-id="4bdec-444">Set up a key encryption key (optional)</span></span>
<span data-ttu-id="4bdec-445">Pokud chcete použít pro další úroveň zabezpečení pro šifrovací klíče nástroje BitLocker hello toouse KEK, přidejte trezoru klíčů tooyour KEK.</span><span class="sxs-lookup"><span data-stu-id="4bdec-445">If you want toouse a KEK for an additional layer of security for hello BitLocker encryption keys, add a KEK tooyour key vault.</span></span> <span data-ttu-id="4bdec-446">Použití hello [ `Add-AzureKeyVaultKey` ](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) rutiny toocreate klíče šifrovací klíč v trezoru klíčů hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-446">Use hello [`Add-AzureKeyVaultKey`](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) cmdlet toocreate a key encryption key in hello key vault.</span></span> <span data-ttu-id="4bdec-447">Můžete také importovat KEK z vaší místní správy k klíče HSM.</span><span class="sxs-lookup"><span data-stu-id="4bdec-447">You can also import a KEK from your on-premises key management HSM.</span></span> <span data-ttu-id="4bdec-448">Další podrobnosti najdete v tématu [klíč trezoru dokumentaci](https://azure.microsoft.com/documentation/services/key-vault/).</span><span class="sxs-lookup"><span data-stu-id="4bdec-448">For more details, see [Key Vault Documentation](https://azure.microsoft.com/documentation/services/key-vault/).</span></span>

    Add-AzureKeyVaultKey [-VaultName] <string> [-Name] <string> -Destination <string> {HSM | Software}

<span data-ttu-id="4bdec-449">Hello KEK můžete přidat tak, že přejdete tooAzure Resource Manager nebo pomocí rozhraní váš trezor klíčů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-449">You can add hello KEK by going tooAzure Resource Manager or by using your key vault interface.</span></span>

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig2.png)

#### <a name="set-key-vault-permissions"></a><span data-ttu-id="4bdec-451">Nastavte oprávnění trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="4bdec-451">Set key vault permissions</span></span>
<span data-ttu-id="4bdec-452">potřebuje přístup toohello šifrovacích klíčů nebo tajných klíčů v váš trezor klíčů toomake zprostředkovatele Hello platformy Azure je k dispozici toohello virtuálního počítače pro spouštění a dešifrování hello svazky.</span><span class="sxs-lookup"><span data-stu-id="4bdec-452">hello Azure platform needs access toohello encryption keys or secrets in your key vault toomake them available toohello VM for booting and decrypting hello volumes.</span></span> <span data-ttu-id="4bdec-453">toogrant oprávnění toohello platformy Azure sadu hello **EnabledForDiskEncryption** vlastnost hello klíč trezoru pomocí rutiny prostředí PowerShell hello trezoru klíčů:</span><span class="sxs-lookup"><span data-stu-id="4bdec-453">toogrant permissions toohello Azure platform, set hello **EnabledForDiskEncryption** property in hello key vault by using hello key vault PowerShell cmdlet:</span></span>

    Set-AzureRmKeyVaultAccessPolicy -VaultName <yourVaultName> -ResourceGroupName <yourResourceGroup> -EnabledForDiskEncryption

<span data-ttu-id="4bdec-454">Můžete také nastavit hello **EnabledForDiskEncryption** vlastnost návštěvou hello [Průzkumníka prostředků Azure](https://resources.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4bdec-454">You can also set hello **EnabledForDiskEncryption** property by visiting hello [Azure Resource Explorer](https://resources.azure.com).</span></span>

<span data-ttu-id="4bdec-455">Jak už bylo zmíněno dříve, je nutné nastavit hello **EnabledForDiskEncryption** vlastnost v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-455">As mentioned earlier, you must set hello **EnabledForDiskEncryption** property on your key vault.</span></span> <span data-ttu-id="4bdec-456">V opačném hello nasazení se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="4bdec-456">Otherwise, hello deployment will fail.</span></span>

<span data-ttu-id="4bdec-457">Můžete nastavit pro vaši aplikaci Azure AD z rozhraní hello trezoru klíčů, zásady přístupu, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="4bdec-457">You can set up access policies for your Azure AD application from hello key vault interface, as shown here:</span></span>

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3.png)

![Azure Key Vault](./media/azure-security-disk-encryption/keyvault-portal-fig3b.png)

<span data-ttu-id="4bdec-460">Na hello **zásady rozšířené přístupu** kartě, ujistěte se, že váš trezor klíčů je povolena pro Azure Disk Encryption:</span><span class="sxs-lookup"><span data-stu-id="4bdec-460">On hello **Advanced access policies** tab, make sure that your key vault is enabled for Azure Disk Encryption:</span></span>

![Azure trezoru klíčů](./media/azure-security-disk-encryption/keyvault-portal-fig4.png)

## <a name="disk-encryption-deployment-scenarios-and-user-experiences"></a><span data-ttu-id="4bdec-462">Scénáře nasazení šifrování disku a koncových uživatelů</span><span class="sxs-lookup"><span data-stu-id="4bdec-462">Disk-encryption deployment scenarios and user experiences</span></span>
<span data-ttu-id="4bdec-463">Můžete povolit mnoho scénářů šifrování disku a hello kroky se mohou lišit podle toohello scénář.</span><span class="sxs-lookup"><span data-stu-id="4bdec-463">You can enable many disk-encryption scenarios, and hello steps may vary according toohello scenario.</span></span> <span data-ttu-id="4bdec-464">Hello následující části se věnují hello scénáře podrobněji.</span><span class="sxs-lookup"><span data-stu-id="4bdec-464">hello following sections cover hello scenarios in greater detail.</span></span>

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-hello-marketplace"></a><span data-ttu-id="4bdec-465">Povolit šifrování na nové virtuální počítače IaaS, které jsou vytvořené pomocí hello Marketplace</span><span class="sxs-lookup"><span data-stu-id="4bdec-465">Enable encryption on new IaaS VMs that are created from hello Marketplace</span></span>
<span data-ttu-id="4bdec-466">Šifrování disku na nový virtuální počítač IaaS Windows z hello v Azure Marketplace můžete povolit pomocí hello [šablony Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image).</span><span class="sxs-lookup"><span data-stu-id="4bdec-466">You can enable disk encryption on new IaaS Windows VM from hello Marketplace in Azure by using hello  [Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image).</span></span>

1. <span data-ttu-id="4bdec-467">V šabloně hello Azure rychlý start, klikněte na tlačítko **nasazení tooAzure**, zadejte hello šifrování konfigurace na hello **parametry** okna a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4bdec-467">On hello Azure quick-start template, click **Deploy tooAzure**, enter hello encryption configuration on hello **Parameters** blade, and then click **OK**.</span></span>

2. <span data-ttu-id="4bdec-468">Vyberte hello předplatné, skupinu prostředků, umístění skupiny prostředků, právní podmínky a smlouvy a pak klikněte na tlačítko **vytvořit** tooenable šifrování na nový virtuální počítač IaaS.</span><span class="sxs-lookup"><span data-stu-id="4bdec-468">Select hello subscription, resource group, resource group location, legal terms, and agreement, and then click **Create** tooenable encryption on a new IaaS VM.</span></span>

> [!NOTE]
> <span data-ttu-id="4bdec-469">Tato šablona vytvoří nový virtuální počítač šifrované Windows, která využívá image Galerie hello systému Windows Server 2012.</span><span class="sxs-lookup"><span data-stu-id="4bdec-469">This template creates a new encrypted Windows VM that uses hello Windows Server 2012 gallery image.</span></span>

<span data-ttu-id="4bdec-470">Můžete povolit šifrování disku na nový IaaS RedHat Linux 7.2 virtuální počítač s polem RAID-0 200 GB prostřednictvím tohoto [šablony Resource Manageru](https://aka.ms/fde-rhel).</span><span class="sxs-lookup"><span data-stu-id="4bdec-470">You can enable disk encryption on a new IaaS RedHat Linux 7.2 VM with a 200-GB RAID-0 array by using this [Resource Manager template](https://aka.ms/fde-rhel).</span></span> <span data-ttu-id="4bdec-471">Po nasazení šablony hello ověřit stav šifrování hello virtuálního počítače pomocí hello `Get-AzureRmVmDiskEncryptionStatus` rutiny, jak je popsáno v [OS šifrování jednotky na spuštěný virtuální počítač s Linuxem](#encrypting-os-drive-on-a-running-linux-vm).</span><span class="sxs-lookup"><span data-stu-id="4bdec-471">After you deploy hello template, verify hello VM encryption status by using hello `Get-AzureRmVmDiskEncryptionStatus` cmdlet, as described in [Encrypting OS drive on a running Linux VM](#encrypting-os-drive-on-a-running-linux-vm).</span></span> <span data-ttu-id="4bdec-472">Když počítač hello vrátí stav _VMRestartPending_, restartujte hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-472">When hello machine returns a status of _VMRestartPending_, restart hello VM.</span></span>

<span data-ttu-id="4bdec-473">Hello následující tabulka uvádí hello parametrů šablony Resource Manageru pro nové virtuální počítače z hello Marketplace scénář pomocí ID klienta Azure AD:</span><span class="sxs-lookup"><span data-stu-id="4bdec-473">hello following table lists hello Resource Manager template parameters for new VMs from hello Marketplace scenario using Azure AD client ID:</span></span>

| <span data-ttu-id="4bdec-474">Parametr</span><span class="sxs-lookup"><span data-stu-id="4bdec-474">Parameter</span></span> | <span data-ttu-id="4bdec-475">Popis</span><span class="sxs-lookup"><span data-stu-id="4bdec-475">Description</span></span> |
| --- | --- |
| <span data-ttu-id="4bdec-476">adminUserName</span><span class="sxs-lookup"><span data-stu-id="4bdec-476">adminUserName</span></span> | <span data-ttu-id="4bdec-477">Uživatelské jméno správce pro virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-477">Admin user name for hello virtual machine.</span></span> |
| <span data-ttu-id="4bdec-478">adminPassword</span><span class="sxs-lookup"><span data-stu-id="4bdec-478">adminPassword</span></span> | <span data-ttu-id="4bdec-479">Uživatelské heslo správce pro virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-479">Admin user password for hello virtual machine.</span></span> |
| <span data-ttu-id="4bdec-480">newStorageAccountName</span><span class="sxs-lookup"><span data-stu-id="4bdec-480">newStorageAccountName</span></span> | <span data-ttu-id="4bdec-481">Název hello úložiště účet toostore operačního systému a dat virtuální pevné disky.</span><span class="sxs-lookup"><span data-stu-id="4bdec-481">Name of hello storage account toostore OS and data VHDs.</span></span> |
| <span data-ttu-id="4bdec-482">vmSize</span><span class="sxs-lookup"><span data-stu-id="4bdec-482">vmSize</span></span> | <span data-ttu-id="4bdec-483">Velikost hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-483">Size of hello VM.</span></span> <span data-ttu-id="4bdec-484">V současné době jsou podporovány pouze standardní A, D a G řady.</span><span class="sxs-lookup"><span data-stu-id="4bdec-484">Currently, only Standard A, D, and G series are supported.</span></span> |
| <span data-ttu-id="4bdec-485">virtualNetworkName</span><span class="sxs-lookup"><span data-stu-id="4bdec-485">virtualNetworkName</span></span> | <span data-ttu-id="4bdec-486">Název hello virtuální síť této hello síťový adaptér virtuálního počítače by měly patřit do.</span><span class="sxs-lookup"><span data-stu-id="4bdec-486">Name of hello VNet that hello VM NIC should belong to.</span></span> |
| <span data-ttu-id="4bdec-487">subnetName</span><span class="sxs-lookup"><span data-stu-id="4bdec-487">subnetName</span></span> | <span data-ttu-id="4bdec-488">Název podsítě hello v hello virtuální síť této hello síťový adaptér virtuálního počítače by měly patřit do.</span><span class="sxs-lookup"><span data-stu-id="4bdec-488">Name of hello subnet in hello VNet that hello VM NIC should belong to.</span></span> |
| <span data-ttu-id="4bdec-489">AADClientID</span><span class="sxs-lookup"><span data-stu-id="4bdec-489">AADClientID</span></span> | <span data-ttu-id="4bdec-490">ID klienta aplikace hello Azure AD, který má oprávnění toowrite tajné klíče tooyour klíče trezoru.</span><span class="sxs-lookup"><span data-stu-id="4bdec-490">Client ID of hello Azure AD application that has permissions toowrite secrets tooyour key vault.</span></span> |
| <span data-ttu-id="4bdec-491">AADClientSecret</span><span class="sxs-lookup"><span data-stu-id="4bdec-491">AADClientSecret</span></span> | <span data-ttu-id="4bdec-492">Tajný klíč klienta aplikace hello Azure AD, který má oprávnění toowrite tajné klíče tooyour klíče trezoru.</span><span class="sxs-lookup"><span data-stu-id="4bdec-492">Client secret of hello Azure AD application that has permissions toowrite secrets tooyour key vault.</span></span> |
| <span data-ttu-id="4bdec-493">keyVaultURL</span><span class="sxs-lookup"><span data-stu-id="4bdec-493">keyVaultURL</span></span> | <span data-ttu-id="4bdec-494">Adresa URL hello klíče trezoru této hello klíč musí být nahrán do nástroje BitLocker.</span><span class="sxs-lookup"><span data-stu-id="4bdec-494">URL of hello key vault that hello BitLocker key should be uploaded to.</span></span> <span data-ttu-id="4bdec-495">Můžete ho získat pomocí rutiny hello `(Get-AzureRmKeyVault -VaultName,-ResourceGroupName ).VaultURI`.</span><span class="sxs-lookup"><span data-stu-id="4bdec-495">You can get it by using hello cmdlet `(Get-AzureRmKeyVault -VaultName,-ResourceGroupName ).VaultURI`.</span></span> |
| <span data-ttu-id="4bdec-496">keyEncryptionKeyURL</span><span class="sxs-lookup"><span data-stu-id="4bdec-496">keyEncryptionKeyURL</span></span> | <span data-ttu-id="4bdec-497">Adresa URL hello klíče šifrovací klíč, který je použité tooencrypt hello vygenerovat klíč nástroje BitLocker (volitelné).</span><span class="sxs-lookup"><span data-stu-id="4bdec-497">URL of hello key encryption key that's used tooencrypt hello generated BitLocker key (optional).</span></span> |
| <span data-ttu-id="4bdec-498">keyVaultResourceGroup</span><span class="sxs-lookup"><span data-stu-id="4bdec-498">keyVaultResourceGroup</span></span> | <span data-ttu-id="4bdec-499">Skupiny prostředků hello trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-499">Resource group of hello key vault.</span></span> |
| <span data-ttu-id="4bdec-500">vmName</span><span class="sxs-lookup"><span data-stu-id="4bdec-500">vmName</span></span> | <span data-ttu-id="4bdec-501">Název hello virtuální počítač, který hello operace šifrování je toobe na provést.</span><span class="sxs-lookup"><span data-stu-id="4bdec-501">Name of hello VM that hello encryption operation is toobe performed on.</span></span> |

> [!NOTE]
> <span data-ttu-id="4bdec-502">_KeyEncryptionKeyURL_ je volitelný parametr.</span><span class="sxs-lookup"><span data-stu-id="4bdec-502">_KeyEncryptionKeyURL_ is an optional parameter.</span></span> <span data-ttu-id="4bdec-503">V trezoru klíčů, můžete zahrnout vlastní KEK toofurther chránit hello datový šifrovací klíč (heslo tajný klíč).</span><span class="sxs-lookup"><span data-stu-id="4bdec-503">You can bring your own KEK toofurther safeguard hello data encryption key (Passphrase secret) in your key vault.</span></span>

### <a name="enable-encryption-on-new-iaas-vms-that-are-created-from-customer-encrypted-vhd-and-encryption-keys"></a><span data-ttu-id="4bdec-504">Povolit šifrování na nové virtuální počítače IaaS, vytvořený z šifrované zákazníka virtuálního pevného disku a šifrovacích klíčů</span><span class="sxs-lookup"><span data-stu-id="4bdec-504">Enable encryption on new IaaS VMs that are created from customer-encrypted VHD and encryption keys</span></span>
<span data-ttu-id="4bdec-505">V tomto scénáři můžete povolit šifrování pomocí šablony Resource Manageru hello, rutiny prostředí PowerShell nebo rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="4bdec-505">In this scenario, you can enable encrypting by using hello Resource Manager template, PowerShell cmdlets, or CLI commands.</span></span> <span data-ttu-id="4bdec-506">Hello následující části popisují větší šablony Resource Manageru hello podrobností a rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="4bdec-506">hello following sections explain in greater detail hello Resource Manager template and CLI commands.</span></span>

<span data-ttu-id="4bdec-507">Postupujte podle pokynů hello z jednoho z těchto částí pro přípravu předem šifrované bitové kopie, které lze použít v Azure.</span><span class="sxs-lookup"><span data-stu-id="4bdec-507">Follow hello instructions from one of these sections for preparing pre-encrypted images that can be used in Azure.</span></span> <span data-ttu-id="4bdec-508">Po vytvoření bitové kopie hello můžete hello kroků v další části toocreate hello šifrované virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="4bdec-508">After hello image is created, you can use hello steps in hello next section toocreate an encrypted Azure VM.</span></span>

* [<span data-ttu-id="4bdec-509">Příprava předem šifrované virtuálního pevného disku Windows</span><span class="sxs-lookup"><span data-stu-id="4bdec-509">Prepare a pre-encrypted Windows VHD</span></span>](#preparing-a-pre-encrypted-windows-vhd)
* [<span data-ttu-id="4bdec-510">Příprava předem šifrované Linux virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="4bdec-510">Prepare a pre-encrypted Linux VHD</span></span>](#preparing-a-pre-encrypted-linux-vhd)

#### <a name="using-hello-resource-manager-template"></a><span data-ttu-id="4bdec-511">Pomocí šablony Resource Manageru hello</span><span class="sxs-lookup"><span data-stu-id="4bdec-511">Using hello Resource Manager template</span></span>
<span data-ttu-id="4bdec-512">Můžete povolit šifrování disku na svůj disk VHD šifrované pomocí hello [šablony Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm).</span><span class="sxs-lookup"><span data-stu-id="4bdec-512">You can enable disk encryption on your encrypted VHD by using hello [Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm).</span></span>

1. <span data-ttu-id="4bdec-513">V šabloně hello Azure rychlý start, klikněte na tlačítko **nasazení tooAzure**, zadejte hello šifrování konfigurace na hello **parametry** okna a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4bdec-513">On hello Azure quick-start template, click **Deploy tooAzure**, enter hello encryption configuration on hello **Parameters** blade, and then click **OK**.</span></span>

2. <span data-ttu-id="4bdec-514">Vyberte hello předplatné, skupinu prostředků, umístění skupiny prostředků, právní podmínky a smlouvy a pak klikněte na tlačítko **vytvořit** hello tooenable šifrování na nový virtuální počítač IaaS.</span><span class="sxs-lookup"><span data-stu-id="4bdec-514">Select hello subscription, resource group, resource group location, legal terms, and agreement, and then click **Create** tooenable encryption on hello new IaaS VM.</span></span>

<span data-ttu-id="4bdec-515">Hello následující tabulka uvádí hello parametrů šablony Resource Manageru pro vaše virtuální pevný disk šifrovaný:</span><span class="sxs-lookup"><span data-stu-id="4bdec-515">hello following table lists hello Resource Manager template parameters for your encrypted VHD:</span></span>

| <span data-ttu-id="4bdec-516">Parametr</span><span class="sxs-lookup"><span data-stu-id="4bdec-516">Parameter</span></span> | <span data-ttu-id="4bdec-517">Popis</span><span class="sxs-lookup"><span data-stu-id="4bdec-517">Description</span></span> |
| --- | --- |
| <span data-ttu-id="4bdec-518">newStorageAccountName</span><span class="sxs-lookup"><span data-stu-id="4bdec-518">newStorageAccountName</span></span> | <span data-ttu-id="4bdec-519">Název hello úložiště účet toostore hello šifrované virtuálního pevného disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="4bdec-519">Name of hello storage account toostore hello encrypted OS VHD.</span></span> <span data-ttu-id="4bdec-520">Tento účet úložiště by měl již byla vytvořena v hello stejnou skupinu prostředků a stejné umístění jako hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-520">This storage account should already have been created in hello same resource group and same location as hello VM.</span></span> |
| <span data-ttu-id="4bdec-521">osVhdUri</span><span class="sxs-lookup"><span data-stu-id="4bdec-521">osVhdUri</span></span> | <span data-ttu-id="4bdec-522">Identifikátor URI hello virtuálního pevného disku operačního systému z účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-522">URI of hello OS VHD from hello storage account.</span></span> |
| <span data-ttu-id="4bdec-523">osType</span><span class="sxs-lookup"><span data-stu-id="4bdec-523">osType</span></span> | <span data-ttu-id="4bdec-524">Typ produktu operačního systému (Windows nebo Linuxem).</span><span class="sxs-lookup"><span data-stu-id="4bdec-524">OS product type (Windows/Linux).</span></span> |
| <span data-ttu-id="4bdec-525">virtualNetworkName</span><span class="sxs-lookup"><span data-stu-id="4bdec-525">virtualNetworkName</span></span> | <span data-ttu-id="4bdec-526">Název hello virtuální síť této hello síťový adaptér virtuálního počítače by měly patřit do.</span><span class="sxs-lookup"><span data-stu-id="4bdec-526">Name of hello VNet that hello VM NIC should belong to.</span></span> <span data-ttu-id="4bdec-527">Hello název by měl již byla vytvořena v hello stejnou skupinu prostředků a stejné umístění jako hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-527">hello name should already have been created in hello same resource group and same location as hello VM.</span></span> |
| <span data-ttu-id="4bdec-528">subnetName</span><span class="sxs-lookup"><span data-stu-id="4bdec-528">subnetName</span></span> | <span data-ttu-id="4bdec-529">Název podsítě hello na hello virtuální síť této hello síťový adaptér virtuálního počítače by měly patřit do.</span><span class="sxs-lookup"><span data-stu-id="4bdec-529">Name of hello subnet on hello VNet that hello VM NIC should belong to.</span></span> |
| <span data-ttu-id="4bdec-530">vmSize</span><span class="sxs-lookup"><span data-stu-id="4bdec-530">vmSize</span></span> | <span data-ttu-id="4bdec-531">Velikost hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-531">Size of hello VM.</span></span> <span data-ttu-id="4bdec-532">V současné době jsou podporovány pouze standardní A, D a G řady.</span><span class="sxs-lookup"><span data-stu-id="4bdec-532">Currently, only Standard A, D, and G series are supported.</span></span> |
| <span data-ttu-id="4bdec-533">keyVaultResourceID</span><span class="sxs-lookup"><span data-stu-id="4bdec-533">keyVaultResourceID</span></span> | <span data-ttu-id="4bdec-534">Hello ResourceID, který identifikuje prostředek hello trezoru klíčů ve službě Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="4bdec-534">hello ResourceID that identifies hello key vault resource in Azure Resource Manager.</span></span> <span data-ttu-id="4bdec-535">Můžete ho získat pomocí rutiny prostředí PowerShell hello `(Get-AzureRmKeyVault -VaultName &lt;yourKeyVaultName&gt; -ResourceGroupName &lt;yourResourceGroupName&gt;).ResourceId`.</span><span class="sxs-lookup"><span data-stu-id="4bdec-535">You can get it by using hello PowerShell cmdlet `(Get-AzureRmKeyVault -VaultName &lt;yourKeyVaultName&gt; -ResourceGroupName &lt;yourResourceGroupName&gt;).ResourceId`.</span></span> |
| <span data-ttu-id="4bdec-536">keyVaultSecretUrl</span><span class="sxs-lookup"><span data-stu-id="4bdec-536">keyVaultSecretUrl</span></span> | <span data-ttu-id="4bdec-537">Adresa URL hello disku šifrovací klíč, který je nastavený v trezoru klíčů hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-537">URL of hello disk-encryption key that's set up in hello key vault.</span></span> |
| <span data-ttu-id="4bdec-538">keyVaultKekUrl</span><span class="sxs-lookup"><span data-stu-id="4bdec-538">keyVaultKekUrl</span></span> | <span data-ttu-id="4bdec-539">Adresa URL hello klíče šifrovacího klíče pro šifrování hello vygenerovat klíč šifrování disku.</span><span class="sxs-lookup"><span data-stu-id="4bdec-539">URL of hello key encryption key for encrypting hello generated disk-encryption key.</span></span> |
| <span data-ttu-id="4bdec-540">vmName</span><span class="sxs-lookup"><span data-stu-id="4bdec-540">vmName</span></span> | <span data-ttu-id="4bdec-541">Název hello virtuálních počítačů IaaS.</span><span class="sxs-lookup"><span data-stu-id="4bdec-541">Name of hello IaaS VM.</span></span> |

#### <a name="using-powershell-cmdlets"></a><span data-ttu-id="4bdec-542">Pomocí rutin prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="4bdec-542">Using PowerShell cmdlets</span></span>
<span data-ttu-id="4bdec-543">Můžete povolit šifrování disku na svůj disk VHD šifrované pomocí rutiny prostředí PowerShell hello [ `Set-AzureRmVMOSDisk` ](/powershell/module/azurerm.compute/set-azurermvmosdisk).</span><span class="sxs-lookup"><span data-stu-id="4bdec-543">You can enable disk encryption on your encrypted VHD by using hello PowerShell cmdlet [`Set-AzureRmVMOSDisk`](/powershell/module/azurerm.compute/set-azurermvmosdisk).</span></span>  

#### <a name="using-cli-commands"></a><span data-ttu-id="4bdec-544">Pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="4bdec-544">Using CLI commands</span></span>
<span data-ttu-id="4bdec-545">šifrování disku tooenable pro tento scénář pomocí rozhraní příkazového řádku, hello následující:</span><span class="sxs-lookup"><span data-stu-id="4bdec-545">tooenable disk encryption for this scenario by using CLI commands, do hello following:</span></span>

1. <span data-ttu-id="4bdec-546">Nastavení zásad přístupu v trezoru klíčů:</span><span class="sxs-lookup"><span data-stu-id="4bdec-546">Set access policies in your key vault:</span></span>

   * <span data-ttu-id="4bdec-547">Sada hello **EnabledForDiskEncryption** příznak:</span><span class="sxs-lookup"><span data-stu-id="4bdec-547">Set hello **EnabledForDiskEncryption** flag:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * <span data-ttu-id="4bdec-548">Nastavte oprávnění tooAzure AD aplikace toowrite tajné klíče tooyour klíče trezoru:</span><span class="sxs-lookup"><span data-stu-id="4bdec-548">Set permissions tooAzure AD application toowrite secrets tooyour key vault:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. <span data-ttu-id="4bdec-549">šifrování tooenable ve stávající nebo spuštěném virtuálním počítači, zadejte:</span><span class="sxs-lookup"><span data-stu-id="4bdec-549">tooenable encryption on an existing or running VM, type:</span></span>

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. <span data-ttu-id="4bdec-550">Načíst stav šifrování:</span><span class="sxs-lookup"><span data-stu-id="4bdec-550">Get encryption status:</span></span>

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. <span data-ttu-id="4bdec-551">šifrování tooenable na nový virtuální počítač z vaší šifrované virtuálního pevného disku, použijte hello následující parametry s hello `azure vm create` příkaz:</span><span class="sxs-lookup"><span data-stu-id="4bdec-551">tooenable encryption on a new VM from your encrypted VHD, use hello following parameters with hello `azure vm create` command:</span></span>

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-existing-or-running-iaas-windows-vm-in-azure"></a><span data-ttu-id="4bdec-552">Povolit šifrování na existující nebo běžící virtuální počítače IaaS Windows v Azure</span><span class="sxs-lookup"><span data-stu-id="4bdec-552">Enable encryption on existing or running IaaS Windows VM in Azure</span></span>
<span data-ttu-id="4bdec-553">V tomto scénáři můžete povolit šifrování pomocí šablony Resource Manageru hello, rutiny prostředí PowerShell nebo rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="4bdec-553">In this scenario, you can enable encrypting by using hello Resource Manager template, PowerShell cmdlets, or CLI commands.</span></span> <span data-ttu-id="4bdec-554">Hello následující části popisují podrobněji jak tooenable ho pomocí hello šablony Resource Manageru a rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="4bdec-554">hello following sections explain in greater detail how tooenable it by using hello Resource Manager template and CLI commands.</span></span>

#### <a name="using-hello-resource-manager-template"></a><span data-ttu-id="4bdec-555">Pomocí šablony Resource Manageru hello</span><span class="sxs-lookup"><span data-stu-id="4bdec-555">Using hello Resource Manager template</span></span>
<span data-ttu-id="4bdec-556">Můžete povolit šifrování disku na existující nebo spuštěných virtuálních počítačů IaaS Windows Azure pomocí hello [šablony Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm).</span><span class="sxs-lookup"><span data-stu-id="4bdec-556">You can enable disk encryption on existing or running IaaS Windows VMs in Azure by using hello [Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm).</span></span>

1. <span data-ttu-id="4bdec-557">V šabloně hello Azure rychlý start, klikněte na tlačítko **nasazení tooAzure**, zadejte hello šifrování konfigurace na hello **parametry** okna a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4bdec-557">On hello Azure quick-start template, click **Deploy tooAzure**, enter hello encryption configuration on hello **Parameters** blade, and then click **OK**.</span></span>

2. <span data-ttu-id="4bdec-558">Vyberte hello předplatné, skupinu prostředků, umístění skupiny prostředků, právní podmínky a smlouvy a pak klikněte na tlačítko **vytvořit** tooenable šifrování na hello existující nebo spuštění virtuálních počítačů IaaS.</span><span class="sxs-lookup"><span data-stu-id="4bdec-558">Select hello subscription, resource group, resource group location, legal terms, and agreement, and then click **Create** tooenable encryption on hello existing or running IaaS VM.</span></span>

<span data-ttu-id="4bdec-559">Hello následující tabulka uvádí hello parametrů šablony Resource Manageru pro existující nebo spuštěných virtuálních počítačů, které používají ID klienta Azure AD:</span><span class="sxs-lookup"><span data-stu-id="4bdec-559">hello following table lists hello Resource Manager template parameters for existing or running VMs that use an Azure AD client ID:</span></span>

| <span data-ttu-id="4bdec-560">Parametr</span><span class="sxs-lookup"><span data-stu-id="4bdec-560">Parameter</span></span> | <span data-ttu-id="4bdec-561">Popis</span><span class="sxs-lookup"><span data-stu-id="4bdec-561">Description</span></span> |
| --- | --- |
| <span data-ttu-id="4bdec-562">AADClientID</span><span class="sxs-lookup"><span data-stu-id="4bdec-562">AADClientID</span></span> | <span data-ttu-id="4bdec-563">ID klienta aplikace hello Azure AD, který má oprávnění toowrite tajné klíče toohello klíče trezoru.</span><span class="sxs-lookup"><span data-stu-id="4bdec-563">Client ID of hello Azure AD application that has permissions toowrite secrets toohello key vault.</span></span> |
| <span data-ttu-id="4bdec-564">AADClientSecret</span><span class="sxs-lookup"><span data-stu-id="4bdec-564">AADClientSecret</span></span> | <span data-ttu-id="4bdec-565">Tajný klíč klienta aplikace hello Azure AD, který má oprávnění toowrite tajné klíče toohello klíče trezoru.</span><span class="sxs-lookup"><span data-stu-id="4bdec-565">Client secret of hello Azure AD application that has permissions toowrite secrets toohello key vault.</span></span> |
| <span data-ttu-id="4bdec-566">keyVaultName</span><span class="sxs-lookup"><span data-stu-id="4bdec-566">keyVaultName</span></span> | <span data-ttu-id="4bdec-567">Název klíče hello trezoru této hello klíč musí být nahrán do nástroje BitLocker.</span><span class="sxs-lookup"><span data-stu-id="4bdec-567">Name of hello key vault that hello BitLocker key should be uploaded to.</span></span> <span data-ttu-id="4bdec-568">Můžete ho získat pomocí rutiny hello `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`.</span><span class="sxs-lookup"><span data-stu-id="4bdec-568">You can get it by using hello cmdlet `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`.</span></span> |
|  <span data-ttu-id="4bdec-569">keyEncryptionKeyURL</span><span class="sxs-lookup"><span data-stu-id="4bdec-569">keyEncryptionKeyURL</span></span> | <span data-ttu-id="4bdec-570">Adresa URL hello klíče šifrovací klíč, který je použité tooencrypt hello vygenerovat klíč nástroje BitLocker.</span><span class="sxs-lookup"><span data-stu-id="4bdec-570">URL of hello key encryption key that's used tooencrypt hello generated BitLocker key.</span></span> <span data-ttu-id="4bdec-571">Tento parametr je nepovinný, pokud vyberete **nokek** hello UseExistingKek rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="4bdec-571">This parameter is optional if you select **nokek** in hello UseExistingKek drop-down list.</span></span> <span data-ttu-id="4bdec-572">Pokud vyberete **kek** hello UseExistingKek rozevíracího seznamu, je nutné zadat hello _keyEncryptionKeyURL_ hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4bdec-572">If you select **kek** in hello UseExistingKek drop-down list, you must enter hello _keyEncryptionKeyURL_ value.</span></span> |
| <span data-ttu-id="4bdec-573">volumeType</span><span class="sxs-lookup"><span data-stu-id="4bdec-573">volumeType</span></span> | <span data-ttu-id="4bdec-574">Typ svazku, který hello operace šifrování.</span><span class="sxs-lookup"><span data-stu-id="4bdec-574">Type of volume that hello encryption operation is performed on.</span></span> <span data-ttu-id="4bdec-575">Platné hodnoty jsou _OS_, _Data_, a _všechny_.</span><span class="sxs-lookup"><span data-stu-id="4bdec-575">Valid values are _OS_, _Data_, and _All_.</span></span> |
| <span data-ttu-id="4bdec-576">sequenceVersion</span><span class="sxs-lookup"><span data-stu-id="4bdec-576">sequenceVersion</span></span> | <span data-ttu-id="4bdec-577">Pořadí verze hello operace nástroje BitLocker.</span><span class="sxs-lookup"><span data-stu-id="4bdec-577">Sequence version of hello BitLocker operation.</span></span> <span data-ttu-id="4bdec-578">Pokaždé, když probíhá operace šifrování disku na hello zvýšit toto číslo verze stejného virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4bdec-578">Increment this version number every time a disk-encryption operation is performed on hello same VM.</span></span> |
| <span data-ttu-id="4bdec-579">vmName</span><span class="sxs-lookup"><span data-stu-id="4bdec-579">vmName</span></span> | <span data-ttu-id="4bdec-580">Název hello virtuální počítač, který hello operace šifrování je toobe na provést.</span><span class="sxs-lookup"><span data-stu-id="4bdec-580">Name of hello VM that hello encryption operation is toobe performed on.</span></span> |

> [!NOTE]
> <span data-ttu-id="4bdec-581">_KeyEncryptionKeyURL_ je volitelný parametr.</span><span class="sxs-lookup"><span data-stu-id="4bdec-581">_KeyEncryptionKeyURL_ is an optional parameter.</span></span> <span data-ttu-id="4bdec-582">V trezoru klíčů hello můžete zahrnout vlastní KEK toofurther chránit hello datový šifrovací klíč (šifrování nástroje BitLocker tajný klíč).</span><span class="sxs-lookup"><span data-stu-id="4bdec-582">You can bring your own KEK toofurther safeguard hello data encryption key (BitLocker encryption secret) in hello key vault.</span></span>

#### <a name="using-powershell-cmdlets"></a><span data-ttu-id="4bdec-583">Pomocí rutin prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="4bdec-583">Using PowerShell cmdlets</span></span>
<span data-ttu-id="4bdec-584">Informace o povolení šifrování s Azure Disk Encryption pomocí rutin prostředí PowerShell, najdete v příspěvcích na blogu hello [prozkoumat Azure Disk Encryption s prostředím Azure PowerShell – část 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) a [prozkoumat Azure Disk Encryption v prostředí Azure PowerShell - část 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).</span><span class="sxs-lookup"><span data-stu-id="4bdec-584">For information about enabling encryption with Azure Disk Encryption by using PowerShell cmdlets, see hello blog posts [Explore Azure Disk Encryption with Azure PowerShell - Part 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) and [Explore Azure Disk Encryption with Azure PowerShell - Part 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).</span></span>

#### <a name="using-cli-commands"></a><span data-ttu-id="4bdec-585">Pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="4bdec-585">Using CLI commands</span></span>
<span data-ttu-id="4bdec-586">šifrování tooenable na existující nebo v Azure pomocí rozhraní příkazového řádku, spuštěné virtuální počítače IaaS Windows hello následující:</span><span class="sxs-lookup"><span data-stu-id="4bdec-586">tooenable encryption on existing or running IaaS Windows VM in Azure using CLI commands, do hello following:</span></span>

1. <span data-ttu-id="4bdec-587">zásady přístupu tooset v trezoru klíčů hello:</span><span class="sxs-lookup"><span data-stu-id="4bdec-587">tooset access policies in hello key vault:</span></span>
   * <span data-ttu-id="4bdec-588">Sada hello **EnabledForDiskEncryption** příznak:</span><span class="sxs-lookup"><span data-stu-id="4bdec-588">Set hello **EnabledForDiskEncryption** flag:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
   * <span data-ttu-id="4bdec-589">Nastavte oprávnění tooAzure AD aplikace toowrite tajné klíče tooyour klíče trezoru:</span><span class="sxs-lookup"><span data-stu-id="4bdec-589">Set permissions tooAzure AD application toowrite secrets tooyour key vault:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`
2. <span data-ttu-id="4bdec-590">šifrování tooenable na stávající nebo spuštěný virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="4bdec-590">tooenable encryption on an existing or running VM:</span></span>

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`
3. <span data-ttu-id="4bdec-591">stav šifrování tooget:</span><span class="sxs-lookup"><span data-stu-id="4bdec-591">tooget encryption status:</span></span>

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`
4. <span data-ttu-id="4bdec-592">šifrování tooenable na nový virtuální počítač z vaší šifrované virtuálního pevného disku, použijte hello následující parametry s hello `azure vm create` příkaz:</span><span class="sxs-lookup"><span data-stu-id="4bdec-592">tooenable encryption on a new VM from your encrypted VHD, use hello following parameters with hello `azure vm create` command:</span></span>

 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="enable-encryption-on-an-existing-or-running-iaas-linux-vm-in-azure"></a><span data-ttu-id="4bdec-593">Povolit šifrování na existující nebo spuštěné IaaS Linux virtuální počítač v Azure</span><span class="sxs-lookup"><span data-stu-id="4bdec-593">Enable encryption on an existing or running IaaS Linux VM in Azure</span></span>
<span data-ttu-id="4bdec-594">Můžete povolit šifrování disku na existující nebo spuštěné IaaS Linux virtuální počítač v Azure pomocí hello [šablony Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm).</span><span class="sxs-lookup"><span data-stu-id="4bdec-594">You can enable disk encryption on an existing or running IaaS Linux VM in Azure by using hello [Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm).</span></span>

1. <span data-ttu-id="4bdec-595">Klikněte na tlačítko **nasazení tooAzure** v šabloně hello Azure úvodní zadejte hello šifrování konfigurace na hello **parametry** okna a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4bdec-595">Click **Deploy tooAzure** on hello Azure quick-start template, enter hello encryption configuration on hello **Parameters** blade, and then click **OK**.</span></span>

2. <span data-ttu-id="4bdec-596">Vyberte hello předplatné, skupinu prostředků, umístění skupiny prostředků, právní podmínky a smlouvy a pak klikněte na tlačítko **vytvořit** tooenable šifrování na hello existující nebo spuštění virtuálních počítačů IaaS.</span><span class="sxs-lookup"><span data-stu-id="4bdec-596">Select hello subscription, resource group, resource group location, legal terms, and agreement, and then click **Create** tooenable encryption on hello existing or running IaaS VM.</span></span>

<span data-ttu-id="4bdec-597">Hello následující tabulka obsahuje seznam parametrů šablony Resource Manageru pro existující nebo spuštěných virtuálních počítačů, které používají ID klienta Azure AD:</span><span class="sxs-lookup"><span data-stu-id="4bdec-597">hello following table lists Resource Manager template parameters for existing or running VMs that use an Azure AD client ID:</span></span>

| <span data-ttu-id="4bdec-598">Parametr</span><span class="sxs-lookup"><span data-stu-id="4bdec-598">Parameter</span></span> | <span data-ttu-id="4bdec-599">Popis</span><span class="sxs-lookup"><span data-stu-id="4bdec-599">Description</span></span> |
| --- | --- |
| <span data-ttu-id="4bdec-600">AADClientID</span><span class="sxs-lookup"><span data-stu-id="4bdec-600">AADClientID</span></span> | <span data-ttu-id="4bdec-601">ID klienta aplikace hello Azure AD, který má oprávnění toowrite tajné klíče toohello klíče trezoru.</span><span class="sxs-lookup"><span data-stu-id="4bdec-601">Client ID of hello Azure AD application that has permissions toowrite secrets toohello key vault.</span></span> |
| <span data-ttu-id="4bdec-602">AADClientSecret</span><span class="sxs-lookup"><span data-stu-id="4bdec-602">AADClientSecret</span></span> | <span data-ttu-id="4bdec-603">Tajný klíč klienta aplikace hello Azure AD, který má oprávnění toowrite tajné klíče tooyour klíče trezoru.</span><span class="sxs-lookup"><span data-stu-id="4bdec-603">Client secret of hello Azure AD application that has permissions toowrite secrets tooyour key vault.</span></span> |
| <span data-ttu-id="4bdec-604">keyVaultName</span><span class="sxs-lookup"><span data-stu-id="4bdec-604">keyVaultName</span></span> | <span data-ttu-id="4bdec-605">Název klíče hello trezoru této hello klíč musí být nahrán do nástroje BitLocker.</span><span class="sxs-lookup"><span data-stu-id="4bdec-605">Name of hello key vault that hello BitLocker key should be uploaded to.</span></span> <span data-ttu-id="4bdec-606">Můžete ho získat pomocí rutiny hello `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`.</span><span class="sxs-lookup"><span data-stu-id="4bdec-606">You can get it by using hello cmdlet `(Get-AzureRmKeyVault -ResourceGroupName <yourResourceGroupName>). Vaultname`.</span></span> |
|  <span data-ttu-id="4bdec-607">keyEncryptionKeyURL</span><span class="sxs-lookup"><span data-stu-id="4bdec-607">keyEncryptionKeyURL</span></span> | <span data-ttu-id="4bdec-608">Adresa URL hello klíče šifrovací klíč, který je použité tooencrypt hello vygenerovat klíč nástroje BitLocker.</span><span class="sxs-lookup"><span data-stu-id="4bdec-608">URL of hello key encryption key that's used tooencrypt hello generated BitLocker key.</span></span> <span data-ttu-id="4bdec-609">Tento parametr je nepovinný, pokud vyberete **nokek** hello UseExistingKek rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="4bdec-609">This parameter is optional if you select **nokek** in hello UseExistingKek drop-down list.</span></span> <span data-ttu-id="4bdec-610">Pokud vyberete **kek** hello UseExistingKek rozevíracího seznamu, je nutné zadat hello _keyEncryptionKeyURL_ hodnotu.</span><span class="sxs-lookup"><span data-stu-id="4bdec-610">If you select **kek** in hello UseExistingKek drop-down list, you must enter hello _keyEncryptionKeyURL_ value.</span></span> |
| <span data-ttu-id="4bdec-611">volumeType</span><span class="sxs-lookup"><span data-stu-id="4bdec-611">volumeType</span></span> | <span data-ttu-id="4bdec-612">Typ svazku, který hello operace šifrování.</span><span class="sxs-lookup"><span data-stu-id="4bdec-612">Type of volume that hello encryption operation is performed on.</span></span> <span data-ttu-id="4bdec-613">Platné podporované hodnoty jsou _OS_ nebo _všechny_ (pro RHEL 7.2, CentOS 7.2 a Ubuntu 16.04), a _Data_ (pro všechny ostatní distribuce).</span><span class="sxs-lookup"><span data-stu-id="4bdec-613">Valid supported values are _OS_ or _All_ (for RHEL 7.2, CentOS 7.2, and Ubuntu 16.04), and _Data_ (for all other distributions).</span></span> |
| <span data-ttu-id="4bdec-614">sequenceVersion</span><span class="sxs-lookup"><span data-stu-id="4bdec-614">sequenceVersion</span></span> | <span data-ttu-id="4bdec-615">Pořadí verze hello operace nástroje BitLocker.</span><span class="sxs-lookup"><span data-stu-id="4bdec-615">Sequence version of hello BitLocker operation.</span></span> <span data-ttu-id="4bdec-616">Pokaždé, když probíhá operace šifrování disku na hello zvýšit toto číslo verze stejného virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4bdec-616">Increment this version number every time a disk-encryption operation is performed on hello same VM.</span></span> |
| <span data-ttu-id="4bdec-617">vmName</span><span class="sxs-lookup"><span data-stu-id="4bdec-617">vmName</span></span> | <span data-ttu-id="4bdec-618">Název hello virtuální počítač, který hello operace šifrování je toobe na provést.</span><span class="sxs-lookup"><span data-stu-id="4bdec-618">Name of hello VM that hello encryption operation is toobe performed on.</span></span> |
| <span data-ttu-id="4bdec-619">přístupové heslo</span><span class="sxs-lookup"><span data-stu-id="4bdec-619">passPhrase</span></span> | <span data-ttu-id="4bdec-620">Zadejte silné heslo jako hello datový šifrovací klíč.</span><span class="sxs-lookup"><span data-stu-id="4bdec-620">Type a strong passphrase as hello data encryption key.</span></span> |

> [!NOTE]
> <span data-ttu-id="4bdec-621">_KeyEncryptionKeyURL_ je volitelný parametr.</span><span class="sxs-lookup"><span data-stu-id="4bdec-621">_KeyEncryptionKeyURL_ is an optional parameter.</span></span> <span data-ttu-id="4bdec-622">V trezoru klíčů, můžete zahrnout vlastní KEK toofurther chránit hello datový šifrovací klíč (heslo tajný klíč).</span><span class="sxs-lookup"><span data-stu-id="4bdec-622">You can bring your own KEK toofurther safeguard hello data encryption key (passphrase secret) in your key vault.</span></span>

#### <a name="cli-commands"></a><span data-ttu-id="4bdec-623">Rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="4bdec-623">CLI commands</span></span>
<span data-ttu-id="4bdec-624">Můžete povolit šifrování disku na svůj disk VHD šifrované instalací a použitím hello [rozhraní příkazového řádku příkaz](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="4bdec-624">You can enable disk encryption on your encrypted VHD by installing and using hello [CLI command](../cli-install-nodejs.md).</span></span> <span data-ttu-id="4bdec-625">šifrování tooenable na existující nebo běžící virtuální počítače IaaS Linux v Azure pomocí rozhraní příkazového řádku, hello následující:</span><span class="sxs-lookup"><span data-stu-id="4bdec-625">tooenable encryption on existing or running IaaS Linux VMs in Azure by using CLI commands, do hello following:</span></span>

1. <span data-ttu-id="4bdec-626">Nastavení zásad přístupu v trezoru klíčů hello:</span><span class="sxs-lookup"><span data-stu-id="4bdec-626">Set access policies in hello key vault:</span></span>

 * <span data-ttu-id="4bdec-627">Sada hello **EnabledForDiskEncryption** příznak:</span><span class="sxs-lookup"><span data-stu-id="4bdec-627">Set hello **EnabledForDiskEncryption** flag:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --enabled-for-disk-encryption true`
 * <span data-ttu-id="4bdec-628">Nastavte oprávnění tooAzure AD aplikace toowrite tajné klíče tooyour klíče trezoru:</span><span class="sxs-lookup"><span data-stu-id="4bdec-628">Set permissions tooAzure AD application toowrite secrets tooyour key vault:</span></span>

    `azure keyvault set-policy --vault-name <keyVaultName> --spn <aadClientID> --perms-to-keys '["wrapKey"]' --perms-to-secrets '["set"]'`

2. <span data-ttu-id="4bdec-629">šifrování tooenable na stávající nebo spuštěný virtuální počítač:</span><span class="sxs-lookup"><span data-stu-id="4bdec-629">tooenable encryption on an existing or running VM:</span></span>

 `azure vm enable-disk-encryption --resource-group <resourceGroupName> --name <vmName> --aad-client-id <aadClientId> --aad-client-secret <aadClientSecret> --disk-encryption-key-vault-url <keyVaultURL> --disk-encryption-key-vault-id <keyVaultResourceId> --volume-type [All|OS|Data]`

3. <span data-ttu-id="4bdec-630">Načíst stav šifrování:</span><span class="sxs-lookup"><span data-stu-id="4bdec-630">Get encryption status:</span></span>

 `azure vm show-disk-encryption-status --resource-group <resourceGroupName> --name <vmName> --json`

4. <span data-ttu-id="4bdec-631">šifrování tooenable na nový virtuální počítač z vaší šifrované virtuálního pevného disku, použijte hello následující parametry s hello `azure vm create` příkaz:</span><span class="sxs-lookup"><span data-stu-id="4bdec-631">tooenable encryption on a new VM from your encrypted VHD, use hello following parameters with hello `azure vm create` command:</span></span>
 ```
   * disk-encryption-key-vault-id <disk-encryption-key-vault-id>
   * disk-encryption-key-url <disk-encryption-key-url>
   * key-encryption-key-vault-id <key-encryption-key-vault-id>
   * key-encryption-key-url <key-encryption-key-url>
 ```

### <a name="get-hello-encryption-status-of-an-encrypted-iaas-vm"></a><span data-ttu-id="4bdec-632">Načíst stav šifrování hello šifrovaný IaaS virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="4bdec-632">Get hello encryption status of an encrypted IaaS VM</span></span>
<span data-ttu-id="4bdec-633">Pomocí Azure Resource Manager, můžete získat stav šifrování hello [rutiny prostředí PowerShell](/powershell/azure/overview), nebo rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="4bdec-633">You can get hello encryption status by using Azure Resource Manager, [PowerShell cmdlets](/powershell/azure/overview), or CLI commands.</span></span> <span data-ttu-id="4bdec-634">Hello následující části popisují, jak toouse hello portál Azure classic a rozhraní příkazového řádku příkazy tooget hello stav šifrování.</span><span class="sxs-lookup"><span data-stu-id="4bdec-634">hello following sections explain how toouse hello Azure classic portal and CLI commands tooget hello encryption status.</span></span>

#### <a name="get-hello-encryption-status-of-an-encrypted-windows-vm-by-using-azure-resource-manager"></a><span data-ttu-id="4bdec-635">Získat stav šifrování hello šifrované virtuální počítač Windows pomocí Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="4bdec-635">Get hello encryption status of an encrypted Windows VM by using Azure Resource Manager</span></span>
<span data-ttu-id="4bdec-636">Stav šifrování hello hello virtuálních počítačů IaaS můžete získat z Azure Resource Manager pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="4bdec-636">You can get hello encryption status of hello IaaS VM from Azure Resource Manager by doing hello following:</span></span>

1. <span data-ttu-id="4bdec-637">Přihlaste se toohello [portál Azure classic](https://portal.azure.com/)a potom klikněte na **virtuální počítače** v levém podokně toosee hello přehled hello virtuálních počítačů v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="4bdec-637">Sign in toohello [Azure classic portal](https://portal.azure.com/), and then click **Virtual machines** in hello left pane toosee a summary view of hello virtual machines in your subscription.</span></span> <span data-ttu-id="4bdec-638">Můžete filtrovat zobrazení hello virtuální počítače, vyberte název odběru hello v hello **předplatné** rozevíracího seznamu.</span><span class="sxs-lookup"><span data-stu-id="4bdec-638">You can filter hello virtual machines view by selecting hello subscription name in hello **Subscription** drop-down list.</span></span>

2. <span data-ttu-id="4bdec-639">Hello horní části hello **virtuální počítače** klikněte na tlačítko **sloupce**.</span><span class="sxs-lookup"><span data-stu-id="4bdec-639">At hello top of hello **Virtual machines** page, click **Columns**.</span></span>

3. <span data-ttu-id="4bdec-640">Na hello **vyberte sloupec** vyberte **šifrování disku**a potom klikněte na **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="4bdec-640">On hello **Choose column** blade, select **Disk Encryption**, and then click **Update**.</span></span> <span data-ttu-id="4bdec-641">Měli byste vidět stav šifrování hello šifrování disku sloupec zobrazuje hello _povoleno_ nebo _není povoleno_ pro každý virtuální počítač, jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="4bdec-641">You should see hello disk-encryption column showing hello encryption state _Enabled_ or _Not Enabled_ for each VM, as shown in hello following figure:</span></span>

 ![Microsoft Antimalware v Azure](./media/azure-security-disk-encryption/disk-encryption-fig2.png)

#### <a name="get-hello-encryption-status-of-an-encrypted-windowslinux-iaas-vm-by-using-hello-disk-encryption-powershell-cmdlet"></a><span data-ttu-id="4bdec-643">Načíst stav šifrování hello virtuálního počítače šifrovaný IaaS (Windows nebo Linuxem) pomocí rutiny prostředí PowerShell hello šifrování disku</span><span class="sxs-lookup"><span data-stu-id="4bdec-643">Get hello encryption status of an encrypted (Windows/Linux) IaaS VM by using hello disk-encryption PowerShell cmdlet</span></span>
<span data-ttu-id="4bdec-644">Stav šifrování hello hello virtuálních počítačů IaaS můžete získat z rutiny prostředí PowerShell šifrování disku hello `Get-AzureRmVMDiskEncryptionStatus`.</span><span class="sxs-lookup"><span data-stu-id="4bdec-644">You can get hello encryption status of hello IaaS VM from hello disk-encryption PowerShell cmdlet `Get-AzureRmVMDiskEncryptionStatus`.</span></span> <span data-ttu-id="4bdec-645">nastavení šifrování hello tooget pro virtuální počítač, zadejte následující hello:</span><span class="sxs-lookup"><span data-stu-id="4bdec-645">tooget hello encryption settings for your VM, enter hello following:</span></span>

    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : NotEncrypted
    DataVolumesEncrypted       : Encrypted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : https://rheltest1keyvault.vault.azure.net/secrets/bdb6bfb1-5431-4c28-af46-b18d0025ef2a/abebacb83d864a5fa729508315020f8a

<span data-ttu-id="4bdec-646">Si můžete prohlédnout výstup hello _Get-AzureRmVMDiskEncryptionStatus_ pro šifrování klíče adresy URL.</span><span class="sxs-lookup"><span data-stu-id="4bdec-646">You can inspect hello output of _Get-AzureRmVMDiskEncryptionStatus_ for encryption key URLs.</span></span>

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

<span data-ttu-id="4bdec-647">Hello OSVolumeEncrypted a DataVolumesEncrypted nastavení hodnoty jsou nastaveny too_Encrypted_, který ukazuje, že jsou oba svazky šifrované pomocí Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="4bdec-647">hello OSVolumeEncrypted and DataVolumesEncrypted settings values are set too_Encrypted_, which shows that both volumes are encrypted using Azure Disk Encryption.</span></span> <span data-ttu-id="4bdec-648">Informace o povolení šifrování s Azure Disk Encryption pomocí rutin prostředí PowerShell, najdete v příspěvcích na blogu hello [prozkoumat Azure Disk Encryption s prostředím Azure PowerShell – část 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) a [prozkoumat Azure Disk Encryption v prostředí Azure PowerShell - část 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).</span><span class="sxs-lookup"><span data-stu-id="4bdec-648">For information about enabling encryption with Azure Disk Encryption by using PowerShell cmdlets, see hello blog posts [Explore Azure Disk Encryption with Azure PowerShell - Part 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) and [Explore Azure Disk Encryption with Azure PowerShell - Part 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="4bdec-649">Na virtuální počítače s Linuxem, trvá tři toofour minut hello `Get-AzureRmVMDiskEncryptionStatus` stav šifrování hello tooreport rutiny.</span><span class="sxs-lookup"><span data-stu-id="4bdec-649">On Linux VMs, it takes three toofour minutes for hello `Get-AzureRmVMDiskEncryptionStatus` cmdlet tooreport hello encryption status.</span></span>

#### <a name="get-hello-encryption-status-of-hello-iaas-vm-from-hello-disk-encryption-cli-command"></a><span data-ttu-id="4bdec-650">Získat stav šifrování hello hello virtuálních počítačů IaaS z příkazu rozhraní příkazového řádku hello šifrování disku</span><span class="sxs-lookup"><span data-stu-id="4bdec-650">Get hello encryption status of hello IaaS VM from hello disk-encryption CLI command</span></span>
<span data-ttu-id="4bdec-651">Stav šifrování hello hello virtuálních počítačů IaaS můžete získat pomocí příkazu rozhraní příkazového řádku šifrování disku hello `azure vm show-disk-encryption-status`.</span><span class="sxs-lookup"><span data-stu-id="4bdec-651">You can get hello encryption status of hello IaaS VM by using hello disk-encryption CLI command `azure vm show-disk-encryption-status`.</span></span> <span data-ttu-id="4bdec-652">nastavení šifrování hello tooget pro virtuální počítač, zadejte relaci příkazového řádku Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="4bdec-652">tooget hello encryption settings for your VM, enter your Azure CLI session:</span></span>

    azure vm show-disk-encryption-status --resource-group <yourResourceGroupName> --name <yourVMName> --json  

#### <a name="disable-encryption-on-running-windows-iaas-vm"></a><span data-ttu-id="4bdec-653">Zakázat šifrování u spuštěných virtuálních počítačů IaaS Windows</span><span class="sxs-lookup"><span data-stu-id="4bdec-653">Disable encryption on running Windows IaaS VM</span></span>
<span data-ttu-id="4bdec-654">Můžete zakázat šifrování na spuštěný Windows nebo virtuálních počítačů IaaS Linux pomocí šablony Azure Disk Encryption Resource Manageru hello nebo rutiny prostředí PowerShell a zadejte konfiguraci dešifrování hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-654">You can disable encryption on a running Windows or Linux IaaS VM via hello Azure Disk Encryption Resource Manager template or PowerShell cmdlets and specify hello decryption configuration.</span></span>

##### <a name="windows-vm"></a><span data-ttu-id="4bdec-655">Virtuální počítač s Windows</span><span class="sxs-lookup"><span data-stu-id="4bdec-655">Windows VM</span></span>
<span data-ttu-id="4bdec-656">Hello zakázat šifrování krok zakazuje šifrování hello operačního systému, hello datový svazek nebo obojí na spuštění virtuálních počítačů IaaS Windows hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-656">hello disable-encryption step disables encryption of hello OS, hello data volume, or both on hello running Windows IaaS VM.</span></span> <span data-ttu-id="4bdec-657">Nelze zakázat hello svazku operačního systému a nechte hello datový svazek zašifrovaná.</span><span class="sxs-lookup"><span data-stu-id="4bdec-657">You cannot disable hello OS volume and leave hello data volume encrypted.</span></span> <span data-ttu-id="4bdec-658">Při provádění krok zakázat šifrování hello modelu nasazení Azure classic hello aktualizuje hello modelu služby virtuálních počítačů a virtuální počítače IaaS Windows hello je označena dešifrovaný.</span><span class="sxs-lookup"><span data-stu-id="4bdec-658">When hello disable-encryption step is performed, hello Azure classic deployment model updates hello VM service model, and hello Windows IaaS VM is marked decrypted.</span></span> <span data-ttu-id="4bdec-659">obsah Hello hello virtuálního počítače jsou již v zašifrované podobě.</span><span class="sxs-lookup"><span data-stu-id="4bdec-659">hello contents of hello VM are no longer encrypted at rest.</span></span> <span data-ttu-id="4bdec-660">dešifrování Hello nedojde k odstranění vašeho klíče trezoru a hello šifrování materiál klíče (šifrovací klíče nástroje BitLocker pro systém Windows a heslo pro Linux).</span><span class="sxs-lookup"><span data-stu-id="4bdec-660">hello decryption does not delete your key vault and hello encryption key material (BitLocker encryption keys for Windows and Passphrase for Linux).</span></span>

##### <a name="linux-vm"></a><span data-ttu-id="4bdec-661">Virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="4bdec-661">Linux VM</span></span>
<span data-ttu-id="4bdec-662">Hello zakázat šifrování krok zakazuje šifrování hello datový svazek na hello spuštění virtuálního počítače s Linuxem IaaS.</span><span class="sxs-lookup"><span data-stu-id="4bdec-662">hello disable-encryption step disables encryption of hello data volume on hello running Linux IaaS VM.</span></span> <span data-ttu-id="4bdec-663">Tento krok je funkční pouze v případě hello disk operačního systému není zašifrován.</span><span class="sxs-lookup"><span data-stu-id="4bdec-663">This step only works if hello OS disk is not encrypted.</span></span>

> [!NOTE]
> <span data-ttu-id="4bdec-664">Zakázáním šifrování na disk s operačním systémem hello není povolená na virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="4bdec-664">Disabling encryption on hello OS disk is not allowed on Linux VMs.</span></span>

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a><span data-ttu-id="4bdec-665">Zakázat šifrování na stávající nebo spuštěný virtuální počítač IaaS</span><span class="sxs-lookup"><span data-stu-id="4bdec-665">Disable encryption on an existing or running IaaS VM</span></span>
<span data-ttu-id="4bdec-666">Můžete zakázat šifrování disku na spuštěných virtuálních počítačů IaaS Windows pomocí hello [šablony Resource Manageru](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm).</span><span class="sxs-lookup"><span data-stu-id="4bdec-666">You can disable disk encryption on running Windows IaaS VMs by using hello [Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-decrypt-running-windows-vm).</span></span>

1. <span data-ttu-id="4bdec-667">V šabloně hello Azure rychlý start, klikněte na tlačítko **nasazení tooAzure**, zadejte hello dešifrování konfigurace na hello **parametry** okna a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="4bdec-667">On hello Azure quick-start template, click **Deploy tooAzure**, enter hello decryption configuration on hello **Parameters** blade, and then click **OK**.</span></span>

2. <span data-ttu-id="4bdec-668">Vyberte hello předplatné, skupinu prostředků, umístění skupiny prostředků, právní podmínky a smlouvy a pak klikněte na tlačítko **vytvořit** tooenable šifrování na nový virtuální počítač IaaS.</span><span class="sxs-lookup"><span data-stu-id="4bdec-668">Select hello subscription, resource group, resource group location, legal terms, and agreement, and then click **Create** tooenable encryption on a new IaaS VM.</span></span>

<span data-ttu-id="4bdec-669">Pro virtuální počítače s Linuxem, můžete zakázat šifrování pomocí hello [vypnout šifrování u spuštěného virtuálního počítače s Linuxem](https://aka.ms/decrypt-linuxvm) šablony.</span><span class="sxs-lookup"><span data-stu-id="4bdec-669">For Linux VMs, you can disable encryption by using hello [Disable encryption on a running Linux VM](https://aka.ms/decrypt-linuxvm) template.</span></span>

<span data-ttu-id="4bdec-670">Hello následující tabulka uvádí parametrů šablony Resource Manageru pro zakázáním šifrování na spuštěný virtuální počítač IaaS:</span><span class="sxs-lookup"><span data-stu-id="4bdec-670">hello following table lists Resource Manager template parameters for disabling encryption on a running IaaS VM:</span></span>

| <span data-ttu-id="4bdec-671">Parametr</span><span class="sxs-lookup"><span data-stu-id="4bdec-671">Parameter</span></span> | <span data-ttu-id="4bdec-672">Popis</span><span class="sxs-lookup"><span data-stu-id="4bdec-672">Description</span></span> |
| --- | --- |
| <span data-ttu-id="4bdec-673">vmName</span><span class="sxs-lookup"><span data-stu-id="4bdec-673">vmName</span></span> | <span data-ttu-id="4bdec-674">Název hello virtuální počítač, který hello operace šifrování je toobe na provést.</span><span class="sxs-lookup"><span data-stu-id="4bdec-674">Name of hello VM that hello encryption operation is toobe performed on.</span></span>
| <span data-ttu-id="4bdec-675">volumeType</span><span class="sxs-lookup"><span data-stu-id="4bdec-675">volumeType</span></span> | <span data-ttu-id="4bdec-676">Typ svazku, který na se provést operaci dešifrování.</span><span class="sxs-lookup"><span data-stu-id="4bdec-676">Type of volume that a decryption operation is performed on.</span></span> <span data-ttu-id="4bdec-677">Platné hodnoty jsou _OS_, _Data_, a _všechny_.</span><span class="sxs-lookup"><span data-stu-id="4bdec-677">Valid values are _OS_, _Data_, and _All_.</span></span> <span data-ttu-id="4bdec-678">Nelze zakázat šifrování u spuštěných virtuálních počítačů IaaS Windows operačního systému nebo spouštěcí svazek bez zakázáním šifrování na hello _Data_ svazku.</span><span class="sxs-lookup"><span data-stu-id="4bdec-678">You cannot disable encryption on running Windows IaaS VM OS/boot volume without disabling encryption on hello _Data_ volume.</span></span> <span data-ttu-id="4bdec-679">Všimněte si také, že zakázáním šifrování na disk s operačním systémem hello není povolená na virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="4bdec-679">Also note that disabling encryption on hello OS disk is not allowed on Linux VMs.</span></span> |
| <span data-ttu-id="4bdec-680">sequenceVersion</span><span class="sxs-lookup"><span data-stu-id="4bdec-680">sequenceVersion</span></span> | <span data-ttu-id="4bdec-681">Pořadí verze hello operace nástroje BitLocker.</span><span class="sxs-lookup"><span data-stu-id="4bdec-681">Sequence version of hello BitLocker operation.</span></span> <span data-ttu-id="4bdec-682">Pokaždé, když provádí operaci dešifrování disku na hello zvýšit toto číslo verze stejného virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4bdec-682">Increment this version number every time a disk decryption operation is performed on hello same VM.</span></span> |

##### <a name="disable-encryption-on-an-existing-or-running-iaas-vm"></a><span data-ttu-id="4bdec-683">Zakázat šifrování na stávající nebo spuštěný virtuální počítač IaaS</span><span class="sxs-lookup"><span data-stu-id="4bdec-683">Disable encryption on an existing or running IaaS VM</span></span>
<span data-ttu-id="4bdec-684">šifrování toodisable na existující nebo spuštěné IaaS virtuální počítač pomocí rutiny prostředí PowerShell text hello, najdete v části [ `Disable-AzureRmVMDiskEncryption` ](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption).</span><span class="sxs-lookup"><span data-stu-id="4bdec-684">toodisable encryption on an existing or running IaaS VM by using hello PowerShell cmdlet, see [`Disable-AzureRmVMDiskEncryption`](/powershell/module/azurerm.compute/disable-azurermvmdiskencryption).</span></span> <span data-ttu-id="4bdec-685">Tato rutina podporuje systém Windows a virtuální počítače s Linuxem.</span><span class="sxs-lookup"><span data-stu-id="4bdec-685">This cmdlet supports both Windows and Linux VMs.</span></span> <span data-ttu-id="4bdec-686">toodisable šifrování, nainstaluje rozšíření ve virtuálním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-686">toodisable encryption, it installs an extension on hello virtual machine.</span></span> <span data-ttu-id="4bdec-687">Pokud hello _název_ není zadán parametr, rozšíření s výchozím názvem hello _AzureDiskEncryption virtuálních počítačů pro Windows_ je vytvořena.</span><span class="sxs-lookup"><span data-stu-id="4bdec-687">If hello _Name_ parameter is not specified, an extension with hello default name _AzureDiskEncryption for Windows VMs_ is created.</span></span>

<span data-ttu-id="4bdec-688">Na virtuální počítače s Linuxem je použít hello AzureDiskEncryptionForLinux rozšíření.</span><span class="sxs-lookup"><span data-stu-id="4bdec-688">On Linux VMs, hello AzureDiskEncryptionForLinux extension is used.</span></span>

> [!NOTE]
> <span data-ttu-id="4bdec-689">Tato rutina restartuje hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="4bdec-689">This cmdlet reboots hello virtual machine.</span></span>

### <a name="enable-encryption-on-pre-encrypted-iaas-vm-with-azure-managed-disk"></a><span data-ttu-id="4bdec-690">Povolit šifrování na předem šifrované virtuálních počítačů IaaS s diskem spravované Azure</span><span class="sxs-lookup"><span data-stu-id="4bdec-690">Enable encryption on pre-encrypted IaaS VM with Azure Managed Disk</span></span>
<span data-ttu-id="4bdec-691">Použít toocreate šablony Azure ARM disku spravované hello, šifrované virtuálního počítače z disku VHD předem šifrované pomocí šablony ARM hello naleznete v</span><span class="sxs-lookup"><span data-stu-id="4bdec-691">Use hello Azure Managed Disk ARM template toocreate a encrypted VM from a pre-encrypted VHD using hello ARM template located at</span></span>   
<span data-ttu-id="4bdec-692">[Vytvořit nové šifrované spravovaným diskem z předem šifrované objektu blob virtuálního pevného disku nebo úložiště] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-create-encrypted-managed-disk)</span><span class="sxs-lookup"><span data-stu-id="4bdec-692">[Create a new encrypted managed disk from a pre-encrypted VHD/storage blob] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-create-encrypted-managed-disk)</span></span>

### <a name="enable-encryption-on-a-new-linux-iaas-vm-with-azure-managed-disk"></a><span data-ttu-id="4bdec-693">Povolit šifrování na nový virtuální počítač IaaS Linux s diskem spravované Azure</span><span class="sxs-lookup"><span data-stu-id="4bdec-693">Enable encryption on a new Linux IaaS VM with Azure Managed Disk</span></span>
<span data-ttu-id="4bdec-694">Použít toocreate šablony Azure ARM disku spravované hello nové šifrované virtuálních počítačů IaaS Linux pomocí šablony ARM hello nacházející se v</span><span class="sxs-lookup"><span data-stu-id="4bdec-694">Use hello Azure Managed Disk ARM template toocreate a new encrypted Linux IaaS VM using hello ARM template located at</span></span>   
<span data-ttu-id="4bdec-695">[Nasazení RHEL 7.2 s šifrování celého disku] (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-full-disk-encrypted-rhel)</span><span class="sxs-lookup"><span data-stu-id="4bdec-695">[Deployment of RHEL 7.2 with full disk encryption] (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-full-disk-encrypted-rhel)</span></span>

### <a name="enable-encryption-on-a-new-windows-iaas-vm-with-azure-managed-disk"></a><span data-ttu-id="4bdec-696">Povolit šifrování na nový virtuální počítač IaaS Windows s Azure spravované disku</span><span class="sxs-lookup"><span data-stu-id="4bdec-696">Enable encryption on a new Windows IaaS VM with Azure Managed Disk</span></span>
 <span data-ttu-id="4bdec-697">Použít toocreate šablony Azure ARM disku spravované hello nové šifrované virtuálních počítačů IaaS Linux pomocí šablony ARM hello nacházející se v</span><span class="sxs-lookup"><span data-stu-id="4bdec-697">Use hello Azure Managed Disk ARM template toocreate a new encrypted Linux IaaS VM using hello ARM template located at</span></span>   
 <span data-ttu-id="4bdec-698">[Vytvořit nové šifrovaný IaaS spravované disku virtuální počítač s Windows z Galerie obrázek] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image-managed-disks)</span><span class="sxs-lookup"><span data-stu-id="4bdec-698">[Create a new encrypted Windows IaaS Managed Disk VM from gallery image] (https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image-managed-disks)</span></span>

  > [!NOTE]
  ><span data-ttu-id="4bdec-699">Je povinné toosnapshot nebo zálohování spravovaných disků na základě instance virtuálního počítače mimo a předchozí tooenabling Azure Disk Encryption.</span><span class="sxs-lookup"><span data-stu-id="4bdec-699">It is mandatory toosnapshot and/or backup a managed disk based VM instance outside of and prior tooenabling Azure Disk Encryption.</span></span>  <span data-ttu-id="4bdec-700">Snímek hello spravovaných disků můžete provést z portálu hello nebo Azure Backup lze použít.</span><span class="sxs-lookup"><span data-stu-id="4bdec-700">A snapshot of hello managed disk can be taken from hello portal, or Azure Backup can be used.</span></span>  <span data-ttu-id="4bdec-701">Zálohování Ujistěte se, že možnost obnovení je možné v případě hello žádné neočekávané selhání při šifrování.</span><span class="sxs-lookup"><span data-stu-id="4bdec-701">Backups ensure that a recovery option is possible in hello case of any unexpected failure during encryption.</span></span>  <span data-ttu-id="4bdec-702">Po provedení zálohy rutiny Set-AzureRmVMDiskEncryptionExtension hello lze použít tooencrypt spravované disky zadáním parametru - skipVmBackup hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-702">Once a backup is made, hello Set-AzureRmVMDiskEncryptionExtension cmdlet can be used tooencrypt managed disks by specifying hello -skipVmBackup parameter.</span></span>  <span data-ttu-id="4bdec-703">Tento příkaz se nezdaří proti spravovaných disků na základě Virtuálního počítače, dokud zálohu byl změněn a tento parametr byla zadána.</span><span class="sxs-lookup"><span data-stu-id="4bdec-703">This command will fail against managed disk based VM's until a backup has been made and this parameter has been specified.</span></span>    
 
### <a name="update-encryption-settings-of-an-existing-encrypted-non-premium-vm"></a><span data-ttu-id="4bdec-704">Aktualizovat nastavení šifrování pro existující virtuálního počítače šifrovaná-úrovně premium</span><span class="sxs-lookup"><span data-stu-id="4bdec-704">Update encryption settings of an existing encrypted non-premium VM</span></span>
  <span data-ttu-id="4bdec-705">Použití hello stávající Azure disk rozhraní šifrování podporované pro spuštění virtuálního počítače [PS rutiny, rozhraní příkazového řádku nebo ARM šablon] tooupdate nastavení šifrování hello jako klienta AAD ID nebo tajný klíč šifrovacího klíče [KEK] šifrovací klíč nástroje BitLocker pro virtuální počítač s Windows nebo přístupové heslo pro Nastavení šifrování aktualizace hello atd virtuálních počítačů Linux je podporována pouze pro virtuální počítače zajišťoval neprémiové úložiště.</span><span class="sxs-lookup"><span data-stu-id="4bdec-705">Use hello existing Azure disk encryption supported interfaces for running VM [PS cmdlets, CLI or ARM templates] tooupdate hello encryption settings like AAD client ID/secret, Key encryption key [KEK], BitLocker encryption key for Windows VM or Passphrase for Linux VM etc. hello update encryption setting is supported only for VMs backed by non-premium storage.</span></span> <span data-ttu-id="4bdec-706">Je NNOT podporována u virtuálních počítačů založenou na storage úrovně premium.</span><span class="sxs-lookup"><span data-stu-id="4bdec-706">It is NNOT supported for VMs backed by premium storage.</span></span>

## <a name="appendix"></a><span data-ttu-id="4bdec-707">Příloha</span><span class="sxs-lookup"><span data-stu-id="4bdec-707">Appendix</span></span>
### <a name="connect-tooyour-subscription"></a><span data-ttu-id="4bdec-708">Připojení tooyour odběru</span><span class="sxs-lookup"><span data-stu-id="4bdec-708">Connect tooyour subscription</span></span>
<span data-ttu-id="4bdec-709">Než budete pokračovat, zkontrolujte hello *požadavky* v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="4bdec-709">Before you proceed, review hello *Prerequisites* section in this article.</span></span> <span data-ttu-id="4bdec-710">Po zajištění, že byly splněny všechny požadavky, připojení odběru tooyour díky hello následující:</span><span class="sxs-lookup"><span data-stu-id="4bdec-710">After you ensure that all prerequisites have been met, connect tooyour subscription by doing hello following:</span></span>

1. <span data-ttu-id="4bdec-711">Spusťte relaci prostředí Azure PowerShell a přihlaste tooyour účet Azure s hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4bdec-711">Start an Azure PowerShell session, and sign in tooyour Azure account with hello following command:</span></span>

    `Login-AzureRmAccount`

2. <span data-ttu-id="4bdec-712">Pokud máte více předplatných a chcete jeden toouse toospecify, zadejte hello následující toosee hello předplatných pro váš účet:</span><span class="sxs-lookup"><span data-stu-id="4bdec-712">If you have multiple subscriptions and want toospecify one toouse, type hello following toosee hello subscriptions for your account:</span></span>

    `Get-AzureRmSubscription`

3. <span data-ttu-id="4bdec-713">předplatné hello toospecify chcete toouse, zadejte:</span><span class="sxs-lookup"><span data-stu-id="4bdec-713">toospecify hello subscription you want toouse, type:</span></span>

    `Select-AzureRmSubscription -SubscriptionName <Yoursubscriptionname>`

4. <span data-ttu-id="4bdec-714">tooverify správnost hello předplatné nakonfigurované, typ:</span><span class="sxs-lookup"><span data-stu-id="4bdec-714">tooverify that hello subscription configured is correct, type:</span></span>

    `Get-AzureRmSubscription`

5. <span data-ttu-id="4bdec-715">tooconfirm hello Azure Disk Encryption jsou nainstalované rutiny, zadejte:</span><span class="sxs-lookup"><span data-stu-id="4bdec-715">tooconfirm hello Azure Disk Encryption cmdlets are installed, type:</span></span>

    `Get-command *diskencryption*`

6. <span data-ttu-id="4bdec-716">Následující výstup Hello potvrdí instalace Azure Disk Encryption PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="4bdec-716">hello following output confirms hello Azure Disk Encryption PowerShell installation:</span></span>

```
    PS C:\Windows\System32\WindowsPowerShell\v1.0> get-command *diskencryption*
    CommandType  Name                                         Source                                                             
    Cmdlet       Get-AzureRmVMDiskEncryptionStatus            AzureRM.Compute                                                    
    Cmdlet       Disable-AzureRmVMDiskEncryption              AzureRM.Compute                                                    
    Cmdlet       Set-AzureRmVMDiskEncryptionExtension         AzureRM.Compute                                                     
```

### <a name="prepare-a-pre-encrypted-windows-vhd"></a><span data-ttu-id="4bdec-717">Příprava předem šifrované virtuálního pevného disku Windows</span><span class="sxs-lookup"><span data-stu-id="4bdec-717">Prepare a pre-encrypted Windows VHD</span></span>
<span data-ttu-id="4bdec-718">Hello oddíly, které následují, jsou potřebné tooprepare předem šifrované Windows virtuální pevný disk pro nasazení jako šifrované virtuální pevný disk v Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="4bdec-718">hello sections that follow are necessary tooprepare a pre-encrypted Windows VHD for deployment as an encrypted VHD in Azure IaaS.</span></span> <span data-ttu-id="4bdec-719">Použijte informace tooprepare hello a spustit novou Windows virtuálních počítačů (VHD) na Azure Site Recovery nebo Azure.</span><span class="sxs-lookup"><span data-stu-id="4bdec-719">Use hello information tooprepare and boot a fresh Windows VM (VHD) on Azure Site Recovery or Azure.</span></span>

#### <a name="update-group-policy-tooallow-non-tpm-for-os-protection"></a><span data-ttu-id="4bdec-720">Aktualizace skupiny zásad tooallow bez čipu TPM k ochraně operačního systému</span><span class="sxs-lookup"><span data-stu-id="4bdec-720">Update group policy tooallow non-TPM for OS protection</span></span>
<span data-ttu-id="4bdec-721">Konfigurace nastavení zásad skupiny pro BitLocker hello **nástroj BitLocker Drive Encryption**, které najdete v části **zásady místního počítače** > **konfigurace počítače**  >  **Šablony pro správu** > **součásti systému Windows**.</span><span class="sxs-lookup"><span data-stu-id="4bdec-721">Configure hello BitLocker Group Policy setting **BitLocker Drive Encryption**, which you'll find under **Local Computer Policy** > **Computer Configuration** > **Administrative Templates** > **Windows Components**.</span></span> <span data-ttu-id="4bdec-722">Změnit toto nastavení příliš**jednotky operačního systému** > **vyžadovat další ověření při spuštění** > **povolit nástroj BitLocker bez kompatibilním čipem TPM**, jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="4bdec-722">Change this setting too**Operating System Drives** > **Require additional authentication at startup** > **Allow BitLocker without a compatible TPM**, as shown in hello following figure:</span></span>

![Microsoft Antimalware v Azure](./media/azure-security-disk-encryption/disk-encryption-fig8.png)

#### <a name="install-bitlocker-feature-components"></a><span data-ttu-id="4bdec-724">Nainstalujte komponenty funkce nástroje BitLocker</span><span class="sxs-lookup"><span data-stu-id="4bdec-724">Install BitLocker feature components</span></span>
<span data-ttu-id="4bdec-725">Pro Windows Server 2012 a novější použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="4bdec-725">For Windows Server 2012 and later, use hello following command:</span></span>

    dism /online /Enable-Feature /all /FeatureName:BitLocker /quiet /norestart

<span data-ttu-id="4bdec-726">Pro Windows Server 2008 R2 použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="4bdec-726">For Windows Server 2008 R2, use hello following command:</span></span>

    ServerManagerCmd -install BitLockers

#### <a name="prepare-hello-os-volume-for-bitlocker-by-using-bdehdcfg"></a><span data-ttu-id="4bdec-727">Přípravě hello svazku operačního systému pomocí nástroje BitLocker`bdehdcfg`</span><span class="sxs-lookup"><span data-stu-id="4bdec-727">Prepare hello OS volume for BitLocker by using `bdehdcfg`</span></span>
<span data-ttu-id="4bdec-728">toocompress hello oddílu operačního systému a připravit hello počítač pro nástroj BitLocker, spustit hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="4bdec-728">toocompress hello OS partition and prepare hello machine for BitLocker, execute hello following command:</span></span>

    bdehdcfg -target c: shrink -quiet

#### <a name="protect-hello-os-volume-by-using-bitlocker"></a><span data-ttu-id="4bdec-729">Chránit hello svazku operačního systému pomocí nástroje BitLocker</span><span class="sxs-lookup"><span data-stu-id="4bdec-729">Protect hello OS volume by using BitLocker</span></span>
<span data-ttu-id="4bdec-730">Použití hello [ `manage-bde` ](https://technet.microsoft.com/library/ff829849.aspx) příkaz tooenable šifrování na spouštěcím svazku hello pomocí externí ochranné zařízení klíče.</span><span class="sxs-lookup"><span data-stu-id="4bdec-730">Use hello [`manage-bde`](https://technet.microsoft.com/library/ff829849.aspx) command tooenable encryption on hello boot volume using an external key protector.</span></span> <span data-ttu-id="4bdec-731">Také umístíte hello externí klíč (.bek soubor) na externím disku hello nebo svazek.</span><span class="sxs-lookup"><span data-stu-id="4bdec-731">Also place hello external key (.bek file) on hello external drive or volume.</span></span> <span data-ttu-id="4bdec-732">Po dalším restartování hello je povolené šifrování na hello systémový/spouštěcí svazek.</span><span class="sxs-lookup"><span data-stu-id="4bdec-732">Encryption is enabled on hello system/boot volume after hello next reboot.</span></span>

    manage-bde -on %systemdrive% -sk [ExternalDriveOrVolume]
    reboot

> [!NOTE]
> <span data-ttu-id="4bdec-733">Připravte hello virtuálních počítačů s samostatné dat nebo prostředků virtuálního pevného disku pro získání hello externího klíče pomocí nástroje BitLocker.</span><span class="sxs-lookup"><span data-stu-id="4bdec-733">Prepare hello VM with a separate data/resource VHD for getting hello external key by using BitLocker.</span></span>

#### <a name="encrypting-an-os-drive-on-a-running-linux-vm"></a><span data-ttu-id="4bdec-734">Šifrování jednotce operačního systému na spuštěný virtuální počítač s Linuxem</span><span class="sxs-lookup"><span data-stu-id="4bdec-734">Encrypting an OS drive on a running Linux VM</span></span>
<span data-ttu-id="4bdec-735">Šifrování jednotky operačního systému na spuštěný virtuální počítač Linux je podporován v následujících distribuce hello:</span><span class="sxs-lookup"><span data-stu-id="4bdec-735">Encryption of an OS drive on a running Linux VM is supported on hello following distributions:</span></span>

* <span data-ttu-id="4bdec-736">RHEL 7.2</span><span class="sxs-lookup"><span data-stu-id="4bdec-736">RHEL 7.2</span></span>
* <span data-ttu-id="4bdec-737">CentOS 7.2</span><span class="sxs-lookup"><span data-stu-id="4bdec-737">CentOS 7.2</span></span>
* <span data-ttu-id="4bdec-738">Ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="4bdec-738">Ubuntu 16.04</span></span>

##### <a name="prerequisites-for-os-disk-encryption"></a><span data-ttu-id="4bdec-739">Požadavky pro šifrování disku operačního systému</span><span class="sxs-lookup"><span data-stu-id="4bdec-739">Prerequisites for OS disk encryption</span></span>

* <span data-ttu-id="4bdec-740">Hello virtuálního počítače musí být vytvořený z hello Marketplace image ve službě Správce prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="4bdec-740">hello VM must be created from hello Marketplace image in Azure Resource Manager.</span></span>
* <span data-ttu-id="4bdec-741">Virtuální počítač Azure s minimálně 4 GB paměti RAM (doporučená velikost je 7 GB).</span><span class="sxs-lookup"><span data-stu-id="4bdec-741">Azure VM with at least 4 GB of RAM (recommended size is 7 GB).</span></span>
* <span data-ttu-id="4bdec-742">(Pro RHEL a CentOS) Zakážete SELinux.</span><span class="sxs-lookup"><span data-stu-id="4bdec-742">(For RHEL and CentOS) Disable SELinux.</span></span> <span data-ttu-id="4bdec-743">toodisable SELinux, najdete v části "4.4.2.</span><span class="sxs-lookup"><span data-stu-id="4bdec-743">toodisable SELinux, see "4.4.2.</span></span> <span data-ttu-id="4bdec-744">Zakázání SELinux"v hello [Průvodce SELinux uživatele a správce](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-744">Disabling SELinux" in hello [SELinux User's and Administrator's Guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/SELinux_Users_and_Administrators_Guide/sect-Security-Enhanced_Linux-Working_with_SELinux-Changing_SELinux_Modes.html#sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux) on hello VM.</span></span>
* <span data-ttu-id="4bdec-745">Po zakázání SELinux restartování aspoň jednou hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-745">After you disable SELinux, reboot hello VM at least once.</span></span>

##### <a name="steps"></a><span data-ttu-id="4bdec-746">Kroky</span><span class="sxs-lookup"><span data-stu-id="4bdec-746">Steps</span></span>
1. <span data-ttu-id="4bdec-747">Vytvoření virtuálního počítače pomocí jedné z distribuce hello zadali dřív.</span><span class="sxs-lookup"><span data-stu-id="4bdec-747">Create a VM by using one of hello distributions specified previously.</span></span>

 <span data-ttu-id="4bdec-748">7.2 CentOS je podporováno šifrování disku operačního systému přes speciální bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="4bdec-748">For CentOS 7.2, OS disk encryption is supported via a special image.</span></span> <span data-ttu-id="4bdec-749">toouse to obrázku, zadejte "7.2n" jako hello SKU, když vytvoříte hello virtuálních počítačů:</span><span class="sxs-lookup"><span data-stu-id="4bdec-749">toouse this image, specify "7.2n" as hello SKU when you create hello VM:</span></span>
 ```
    Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName "OpenLogic" -Offer "CentOS" -Skus "7.2n" -Version "latest"
 ```
2. <span data-ttu-id="4bdec-750">Nakonfigurujte hello virtuálního počítače podle tooyour potřebám.</span><span class="sxs-lookup"><span data-stu-id="4bdec-750">Configure hello VM according tooyour needs.</span></span> <span data-ttu-id="4bdec-751">Chcete-li tooencrypt všech jednotkách hello (operační systém + data), hello datové jednotky potřebovat toobe zadaný a připojit z /etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="4bdec-751">If you are going tooencrypt all hello (OS + data) drives, hello data drives need toobe specified and mountable from /etc/fstab.</span></span>

 > [!NOTE]
 > <span data-ttu-id="4bdec-752">Použít UUID =... toospecify datových jednotkách v fstab/etc/místo zadání názvu zařízení bloku hello (například/dev/sdb1).</span><span class="sxs-lookup"><span data-stu-id="4bdec-752">Use UUID=... toospecify data drives in /etc/fstab instead of specifying hello block device name (for example, /dev/sdb1).</span></span> <span data-ttu-id="4bdec-753">Během šifrování se změny pořadí hello jednotek v hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-753">During encryption, hello order of drives changes on hello VM.</span></span> <span data-ttu-id="4bdec-754">Pokud virtuálního počítače závisí na konkrétní pořadí blokovat zařízení, se nezdaří, toomount je po dokončení šifrování.</span><span class="sxs-lookup"><span data-stu-id="4bdec-754">If your VM relies on a specific order of block devices, it will fail toomount them after encryption.</span></span>

3. <span data-ttu-id="4bdec-755">Odhlásit z relace SSH hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-755">Sign out of hello SSH sessions.</span></span>

4. <span data-ttu-id="4bdec-756">tooencrypt hello operačního systému, zadejte volumeType jako **všechny** nebo **OS** když jste [povolit šifrování](#enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure).</span><span class="sxs-lookup"><span data-stu-id="4bdec-756">tooencrypt hello OS, specify volumeType as **All** or **OS** when you [enable encryption](#enable-encryption-on-existing-or-running-iaas-linux-vm-in-azure).</span></span>

 > [!NOTE]
 > <span data-ttu-id="4bdec-757">Všechny procesy uživatelského prostoru, které nejsou spuštěny jako `systemd` služby by měl být ukončen s `SIGKILL`.</span><span class="sxs-lookup"><span data-stu-id="4bdec-757">All user-space processes that are not running as `systemd` services should be killed with a `SIGKILL`.</span></span> <span data-ttu-id="4bdec-758">Restartování hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-758">Reboot hello VM.</span></span> <span data-ttu-id="4bdec-759">Pokud povolíte šifrování disku operačního systému na spuštěný virtuální počítač, naplánujte na výpadek virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-759">When you enable OS disk encryption on a running VM, plan on VM downtime.</span></span>

5. <span data-ttu-id="4bdec-760">Pravidelně sledovat průběh hello šifrování podle pokynů hello v hello [další části](#monitoring-os-encryption-progress).</span><span class="sxs-lookup"><span data-stu-id="4bdec-760">Periodically monitor hello progress of encryption by using hello instructions in hello [next section](#monitoring-os-encryption-progress).</span></span>

6. <span data-ttu-id="4bdec-761">Po Get-AzureRmVmDiskEncryptionStatus zobrazuje "VMRestartPending", restartujte virtuální počítač po přihlášení tooit nebo pomocí portálu hello, prostředí PowerShell nebo rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="4bdec-761">After Get-AzureRmVmDiskEncryptionStatus shows "VMRestartPending," restart your VM either by signing in tooit or by using hello portal, PowerShell, or CLI.</span></span>
    ```
    C:\> Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $ResourceGroupName -VMName $VMName
    -ExtensionName $ExtensionName

    OsVolumeEncrypted          : VMRestartPending
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk successfully encrypted, reboot hello VM
    ```
<span data-ttu-id="4bdec-762">Než restartujete, doporučujeme, abyste uložili [spouštění diagnostiky](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/) z hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-762">Before you reboot, we recommend that you save [boot diagnostics](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/) of hello VM.</span></span>

#### <a name="monitoring-os-encryption-progress"></a><span data-ttu-id="4bdec-763">Sledování průběhu šifrování operačního systému</span><span class="sxs-lookup"><span data-stu-id="4bdec-763">Monitoring OS encryption progress</span></span>
<span data-ttu-id="4bdec-764">Můžete sledovat průběh šifrování OS třemi způsoby:</span><span class="sxs-lookup"><span data-stu-id="4bdec-764">You can monitor OS encryption progress in three ways:</span></span>

* <span data-ttu-id="4bdec-765">Použití hello `Get-AzureRmVmDiskEncryptionStatus` rutiny a zkontrolovat hello ProgressMessage pole:</span><span class="sxs-lookup"><span data-stu-id="4bdec-765">Use hello `Get-AzureRmVmDiskEncryptionStatus` cmdlet and inspect hello ProgressMessage field:</span></span>
    ```
    OsVolumeEncrypted          : EncryptionInProgress
    DataVolumesEncrypted       : NotMounted
    OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
    ProgressMessage            : OS disk encryption started
    ```
 <span data-ttu-id="4bdec-766">Po hello virtuálního počítače dosáhne "Spuštění šifrování disku operačního systému", jak dlouho trvá asi 40 minutách too50 na storage úrovně Premium zálohovaný virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-766">After hello VM reaches "OS disk encryption started," it takes about 40 too50 minutes on a Premium-storage backed VM.</span></span>

 <span data-ttu-id="4bdec-767">Z důvodu [vydání #388](https://github.com/Azure/WALinuxAgent/issues/388) v WALinuxAgent, `OsVolumeEncrypted` a `DataVolumesEncrypted` zobrazují jako `Unknown` v některých distribucích.</span><span class="sxs-lookup"><span data-stu-id="4bdec-767">Because of [issue #388](https://github.com/Azure/WALinuxAgent/issues/388) in WALinuxAgent, `OsVolumeEncrypted` and `DataVolumesEncrypted` show up as `Unknown` in some distributions.</span></span> <span data-ttu-id="4bdec-768">S WALinuxAgent verze 2.1.5 a novější, se tento problém vyřešen automaticky.</span><span class="sxs-lookup"><span data-stu-id="4bdec-768">With WALinuxAgent version 2.1.5 and later, this issue is fixed automatically.</span></span> <span data-ttu-id="4bdec-769">Pokud se zobrazí `Unknown` ve výstupu hello si můžete ověřit stav šifrování disku pomocí hello Průzkumníka prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="4bdec-769">If you see `Unknown` in hello output, you can verify disk-encryption status by using hello Azure Resource Explorer.</span></span>

 <span data-ttu-id="4bdec-770">Přejděte příliš[Průzkumníka prostředků Azure](https://resources.azure.com/)a potom rozbalte tuto hierarchii panelu hello výběr na levé straně:</span><span class="sxs-lookup"><span data-stu-id="4bdec-770">Go too[Azure Resource Explorer](https://resources.azure.com/), and then expand this hierarchy in hello selection panel on left:</span></span>

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

 <span data-ttu-id="4bdec-771">V hello InstanceView posuňte se dolů toosee hello stav šifrování jednotky.</span><span class="sxs-lookup"><span data-stu-id="4bdec-771">In hello InstanceView, scroll down toosee hello encryption status of your drives.</span></span>

 ![Zobrazení Instance virtuálního počítače](./media/azure-security-disk-encryption/vm-instanceview.png)

* <span data-ttu-id="4bdec-773">Podívejte se na [spouštění diagnostiky](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/).</span><span class="sxs-lookup"><span data-stu-id="4bdec-773">Look at [boot diagnostics](https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/).</span></span> <span data-ttu-id="4bdec-774">Zprávy ze hello ADE rozšíření by měla obsahovat předponu s `[AzureDiskEncryption]`.</span><span class="sxs-lookup"><span data-stu-id="4bdec-774">Messages from hello ADE extension should be prefixed with `[AzureDiskEncryption]`.</span></span>

* <span data-ttu-id="4bdec-775">Přihlaste se toohello virtuálních počítačů pomocí protokolu SSH a získat hello rozšíření protokolu z:</span><span class="sxs-lookup"><span data-stu-id="4bdec-775">Sign in toohello VM via SSH, and get hello extension log from:</span></span>

    <span data-ttu-id="4bdec-776">/var/log/Azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux</span><span class="sxs-lookup"><span data-stu-id="4bdec-776">/var/log/azure/Microsoft.Azure.Security.AzureDiskEncryptionForLinux</span></span>

 <span data-ttu-id="4bdec-777">Doporučujeme vám, že v toohello virtuálních počítačů nepodepíšete, když probíhá šifrování operačního systému.</span><span class="sxs-lookup"><span data-stu-id="4bdec-777">We recommend that you do not sign in toohello VM while OS encryption is in progress.</span></span> <span data-ttu-id="4bdec-778">Kopírovat protokoly hello jenom v případě, že hello další dvě metody se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="4bdec-778">Copy hello logs only when hello other two methods have failed.</span></span>

#### <a name="prepare-a-pre-encrypted-linux-vhd"></a><span data-ttu-id="4bdec-779">Příprava předem šifrované Linux virtuální pevný disk</span><span class="sxs-lookup"><span data-stu-id="4bdec-779">Prepare a pre-encrypted Linux VHD</span></span>
##### <a name="ubuntu-16"></a><span data-ttu-id="4bdec-780">Ubuntu 16</span><span class="sxs-lookup"><span data-stu-id="4bdec-780">Ubuntu 16</span></span>
<span data-ttu-id="4bdec-781">Nakonfigurujte šifrování během instalace distribučního hello pomocí hello následující:</span><span class="sxs-lookup"><span data-stu-id="4bdec-781">Configure encryption during hello distribution installation by doing hello following:</span></span>

1. <span data-ttu-id="4bdec-782">Vyberte **konfigurovat šifrované svazky** při oddílu hello disky.</span><span class="sxs-lookup"><span data-stu-id="4bdec-782">Select **Configure encrypted volumes** when you partition hello disks.</span></span>

 ![Instalační program Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig1.png)

2. <span data-ttu-id="4bdec-784">Vytvořte samostatný spouštěcí jednotka, která nesmí být zašifrovaná.</span><span class="sxs-lookup"><span data-stu-id="4bdec-784">Create a separate boot drive, which must not be encrypted.</span></span> <span data-ttu-id="4bdec-785">Šifrování kořenové jednotce.</span><span class="sxs-lookup"><span data-stu-id="4bdec-785">Encrypt your root drive.</span></span>

 ![Instalační program Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig2.png)

3. <span data-ttu-id="4bdec-787">Zadejte přístupové heslo.</span><span class="sxs-lookup"><span data-stu-id="4bdec-787">Provide a passphrase.</span></span> <span data-ttu-id="4bdec-788">Toto je heslo hello odeslat toohello trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-788">This is hello passphrase that you upload toohello key vault.</span></span>

 ![Instalační program Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig3.png)

4. <span data-ttu-id="4bdec-790">Dokončete vytváření oddílů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-790">Finish partitioning.</span></span>

 ![Instalační program Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig4.png)

5. <span data-ttu-id="4bdec-792">Když spustíte hello virtuálních počítačů a vyzváni k přístupové heslo, použijte hello přístupové heslo, které jste zadali v kroku 3.</span><span class="sxs-lookup"><span data-stu-id="4bdec-792">When you boot hello VM and are asked for a passphrase, use hello passphrase you provided in step 3.</span></span>

 ![Instalační program Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig5.png)

6. <span data-ttu-id="4bdec-794">Příprava hello virtuálního počítače pro odesílání do Azure pomocí [tyto pokyny](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-ubuntu/).</span><span class="sxs-lookup"><span data-stu-id="4bdec-794">Prepare hello VM for uploading into Azure using [these instructions](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-ubuntu/).</span></span> <span data-ttu-id="4bdec-795">Ještě nespouštějte hello poslední krok (hello zrušení zřízení virtuálního počítače).</span><span class="sxs-lookup"><span data-stu-id="4bdec-795">Do not run hello last step (deprovisioning hello VM) yet.</span></span>

<span data-ttu-id="4bdec-796">Nakonfigurujte toowork šifrování s Azure pomocí hello následující:</span><span class="sxs-lookup"><span data-stu-id="4bdec-796">Configure encryption toowork with Azure by doing hello following:</span></span>

1. <span data-ttu-id="4bdec-797">Vytvořte soubor pod /usr/local/sbin/azure_crypt_key.sh, s obsahem hello v hello následující skript.</span><span class="sxs-lookup"><span data-stu-id="4bdec-797">Create a file under /usr/local/sbin/azure_crypt_key.sh, with hello content in hello following script.</span></span> <span data-ttu-id="4bdec-798">Věnujte pozornost toohello KeyFileName, protože je název souboru hello přístupové heslo používané Azure.</span><span class="sxs-lookup"><span data-stu-id="4bdec-798">Pay attention toohello KeyFileName, because it is hello passphrase file name used by Azure.</span></span>

    ```
    #!/bin/sh
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying tooget hello key from disks ..." >&2
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
        echo "FAILED toofind suitable passphrase file ..." >&2
        echo -n "Try tooenter your password: " >&2
        read -s -r A </dev/console
        echo -n "$A"
     else
        echo "Success loading keyfile!" >&2
    fi
```

2. <span data-ttu-id="4bdec-799">Změna konfigurace crypt hello v */etc/crypttab*.</span><span class="sxs-lookup"><span data-stu-id="4bdec-799">Change hello crypt config in */etc/crypttab*.</span></span> <span data-ttu-id="4bdec-800">Mělo by to vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="4bdec-800">It should look like this:</span></span>
 ```
    xxx_crypt uuid=xxxxxxxxxxxxxxxxxxxxx none luks,discard,keyscript=/usr/local/sbin/azure_crypt_key.sh
    ```

3. <span data-ttu-id="4bdec-801">Pokud upravujete *azure_crypt_key.sh* ve Windows a zkopíruje ho tooLinux, spusťte `dos2unix /usr/local/sbin/azure_crypt_key.sh`.</span><span class="sxs-lookup"><span data-stu-id="4bdec-801">If you are editing *azure_crypt_key.sh* in Windows and you copied it tooLinux, run `dos2unix /usr/local/sbin/azure_crypt_key.sh`.</span></span>

4. <span data-ttu-id="4bdec-802">Přidejte skript toohello spustitelného souboru oprávnění:</span><span class="sxs-lookup"><span data-stu-id="4bdec-802">Add executable permissions toohello script:</span></span>
 ```
    chmod +x /usr/local/sbin/azure_crypt_key.sh
 ```
5. <span data-ttu-id="4bdec-803">Upravit */etc/initramfs-tools/modules* připojením řádky:</span><span class="sxs-lookup"><span data-stu-id="4bdec-803">Edit */etc/initramfs-tools/modules* by appending lines:</span></span>
 ```
    vfat
    ntfs
    nls_cp437
    nls_utf8
    nls_iso8859-1
```
6. <span data-ttu-id="4bdec-804">Spustit `update-initramfs -u -k all` tooupdate hello initramfs toomake hello `keyscript` vstoupí v platnost.</span><span class="sxs-lookup"><span data-stu-id="4bdec-804">Run `update-initramfs -u -k all` tooupdate hello initramfs toomake hello `keyscript` take effect.</span></span>

7. <span data-ttu-id="4bdec-805">Nyní můžete zrušení zřízení hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-805">Now you can deprovision hello VM.</span></span>

 ![Instalační program Ubuntu 16.04](./media/azure-security-disk-encryption/ubuntu-1604-preencrypted-fig6.png)

8. <span data-ttu-id="4bdec-807">Pokračovat dalším krokem toohello a [nahrát svůj disk VHD](#upload-encrypted-vhd-to-an-azure-storage-account) do Azure.</span><span class="sxs-lookup"><span data-stu-id="4bdec-807">Continue toohello next step and [upload your VHD](#upload-encrypted-vhd-to-an-azure-storage-account) into Azure.</span></span>

##### <a name="opensuse-132"></a><span data-ttu-id="4bdec-808">openSUSE 13.2</span><span class="sxs-lookup"><span data-stu-id="4bdec-808">openSUSE 13.2</span></span>
<span data-ttu-id="4bdec-809">šifrování tooconfigure během instalace distribučního hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="4bdec-809">tooconfigure encryption during hello distribution installation, do hello following:</span></span>
1. <span data-ttu-id="4bdec-810">Pokud jste oddílu hello disky, vyberte **šifrování svazku skupiny**a pak zadejte heslo.</span><span class="sxs-lookup"><span data-stu-id="4bdec-810">When you partition hello disks, select **Encrypt Volume Group**, and then enter a password.</span></span> <span data-ttu-id="4bdec-811">Toto je heslo hello trezoru klíčů tooyour odešlete.</span><span class="sxs-lookup"><span data-stu-id="4bdec-811">This is hello password that you will upload tooyour key vault.</span></span>

 ![Instalační program openSUSE 13.2](./media/azure-security-disk-encryption/opensuse-encrypt-fig1.png)

2. <span data-ttu-id="4bdec-813">Spouštěcí hello virtuálního počítače pomocí hesla.</span><span class="sxs-lookup"><span data-stu-id="4bdec-813">Boot hello VM using your password.</span></span>

 ![Instalační program openSUSE 13.2](./media/azure-security-disk-encryption/opensuse-encrypt-fig2.png)

3. <span data-ttu-id="4bdec-815">Příprava hello virtuálního počítače pro odesílání tooAzure podle pokynů hello v [Příprava virtuálního počítače, SLES nebo openSUSE pro Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131).</span><span class="sxs-lookup"><span data-stu-id="4bdec-815">Prepare hello VM for uploading tooAzure by following hello instructions in [Prepare a SLES or openSUSE virtual machine for Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-suse-create-upload-vhd/#prepare-opensuse-131).</span></span> <span data-ttu-id="4bdec-816">Ještě nespouštějte hello poslední krok (hello zrušení zřízení virtuálního počítače).</span><span class="sxs-lookup"><span data-stu-id="4bdec-816">Do not run hello last step (deprovisioning hello VM) yet.</span></span>

<span data-ttu-id="4bdec-817">šifrování toowork tooconfigure s Azure, hello následující:</span><span class="sxs-lookup"><span data-stu-id="4bdec-817">tooconfigure encryption toowork with Azure, do hello following:</span></span>
1. <span data-ttu-id="4bdec-818">Upravit hello /etc/dracut.conf a přidejte následující řádek hello:</span><span class="sxs-lookup"><span data-stu-id="4bdec-818">Edit hello /etc/dracut.conf, and add hello following line:</span></span>
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```
2. <span data-ttu-id="4bdec-819">Komentář tyto řádky hello konec hello souboru /usr/lib/dracut/modules.d/90crypt/module-setup.sh:</span><span class="sxs-lookup"><span data-stu-id="4bdec-819">Comment out these lines by hello end of hello file /usr/lib/dracut/modules.d/90crypt/module-setup.sh:</span></span>
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

3. <span data-ttu-id="4bdec-820">Připojte hello následující řádek na začátku hello /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh souboru hello:</span><span class="sxs-lookup"><span data-stu-id="4bdec-820">Append hello following line at hello beginning of hello file /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh:</span></span>
 ```
    DRACUT_SYSTEMD=0
 ```
<span data-ttu-id="4bdec-821">A změňte všechny výskyty:</span><span class="sxs-lookup"><span data-stu-id="4bdec-821">And change all occurrences of:</span></span>
 ```
    if [ -z "$DRACUT_SYSTEMD" ]; then
 ```
<span data-ttu-id="4bdec-822">na:</span><span class="sxs-lookup"><span data-stu-id="4bdec-822">to:</span></span>
```
    if [ 1 ]; then
```
4. <span data-ttu-id="4bdec-823">Upravit /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh a připojit ho příliš "# otevřete LUKS zařízení":</span><span class="sxs-lookup"><span data-stu-id="4bdec-823">Edit /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh and append it too“# Open LUKS device”:</span></span>

    ```
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying tooget hello key from disks ..." >&2
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
5. <span data-ttu-id="4bdec-824">Spustit `/usr/sbin/dracut -f -v` tooupdate hello initrd.</span><span class="sxs-lookup"><span data-stu-id="4bdec-824">Run `/usr/sbin/dracut -f -v` tooupdate hello initrd.</span></span>

6. <span data-ttu-id="4bdec-825">Teď můžete zrušení zřízení hello virtuálních počítačů a [nahrát svůj disk VHD](#upload-encrypted-vhd-to-an-azure-storage-account) do Azure.</span><span class="sxs-lookup"><span data-stu-id="4bdec-825">Now you can deprovision hello VM and [upload your VHD](#upload-encrypted-vhd-to-an-azure-storage-account) into Azure.</span></span>

##### <a name="centos-7"></a><span data-ttu-id="4bdec-826">CentOS 7</span><span class="sxs-lookup"><span data-stu-id="4bdec-826">CentOS 7</span></span>
<span data-ttu-id="4bdec-827">šifrování tooconfigure během instalace distribučního hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="4bdec-827">tooconfigure encryption during hello distribution installation, do hello following:</span></span>
1. <span data-ttu-id="4bdec-828">Vyberte **šifrovat data** při oddílu disky.</span><span class="sxs-lookup"><span data-stu-id="4bdec-828">Select **Encrypt my data** when you partition disks.</span></span>

 ![Instalační program centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig1.png)

2. <span data-ttu-id="4bdec-830">Zajistěte, aby **šifrovat** je vybraná pro kořenový oddíl.</span><span class="sxs-lookup"><span data-stu-id="4bdec-830">Make sure **Encrypt** is selected for root partition.</span></span>

 ![Instalační program centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig2.png)

3. <span data-ttu-id="4bdec-832">Zadejte přístupové heslo.</span><span class="sxs-lookup"><span data-stu-id="4bdec-832">Provide a passphrase.</span></span> <span data-ttu-id="4bdec-833">Toto je heslo hello trezoru klíčů tooyour odešlete.</span><span class="sxs-lookup"><span data-stu-id="4bdec-833">This is hello passphrase that you will upload tooyour key vault.</span></span>

 ![Instalační program centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig3.png)

4. <span data-ttu-id="4bdec-835">Když spustíte hello virtuálních počítačů a vyzváni k přístupové heslo, použijte hello přístupové heslo, které jste zadali v kroku 3.</span><span class="sxs-lookup"><span data-stu-id="4bdec-835">When you boot hello VM and are asked for a passphrase, use hello passphrase you provided in step 3.</span></span>

 ![Instalační program centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig4.png)

5. <span data-ttu-id="4bdec-837">Příprava hello virtuálního počítače pro odesílání do Azure podle pokynů "CentOS 7.0 +" hello v [Příprava virtuálního počítače, na základě CentOS pro Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70).</span><span class="sxs-lookup"><span data-stu-id="4bdec-837">Prepare hello VM for uploading into Azure by using hello "CentOS 7.0+" instructions in [Prepare a CentOS-based virtual machine for Azure](https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-create-upload-centos/#centos-70).</span></span> <span data-ttu-id="4bdec-838">Ještě nespouštějte hello poslední krok (hello zrušení zřízení virtuálního počítače).</span><span class="sxs-lookup"><span data-stu-id="4bdec-838">Do not run hello last step (deprovisioning hello VM) yet.</span></span>

6. <span data-ttu-id="4bdec-839">Teď můžete zrušení zřízení hello virtuálních počítačů a [nahrát svůj disk VHD](#upload-encrypted-vhd-to-an-azure-storage-account) do Azure.</span><span class="sxs-lookup"><span data-stu-id="4bdec-839">Now you can deprovision hello VM and [upload your VHD](#upload-encrypted-vhd-to-an-azure-storage-account) into Azure.</span></span>

<span data-ttu-id="4bdec-840">šifrování toowork tooconfigure s Azure, hello následující:</span><span class="sxs-lookup"><span data-stu-id="4bdec-840">tooconfigure encryption toowork with Azure, do hello following:</span></span>

1. <span data-ttu-id="4bdec-841">Upravit hello /etc/dracut.conf a přidejte následující řádek hello:</span><span class="sxs-lookup"><span data-stu-id="4bdec-841">Edit hello /etc/dracut.conf, and add hello following line:</span></span>
    ```
    add_drivers+=" vfat ntfs nls_cp437 nls_iso8859-1"
    ```

2. <span data-ttu-id="4bdec-842">Komentář tyto řádky hello konec hello souboru /usr/lib/dracut/modules.d/90crypt/module-setup.sh:</span><span class="sxs-lookup"><span data-stu-id="4bdec-842">Comment out these lines by hello end of hello file /usr/lib/dracut/modules.d/90crypt/module-setup.sh:</span></span>
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

3. <span data-ttu-id="4bdec-843">Připojte hello následující řádek na začátku hello /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh souboru hello:</span><span class="sxs-lookup"><span data-stu-id="4bdec-843">Append hello following line at hello beginning of hello file /usr/lib/dracut/modules.d/90crypt/parse-crypt.sh:</span></span>
```
    DRACUT_SYSTEMD=0
```
<span data-ttu-id="4bdec-844">A změňte všechny výskyty:</span><span class="sxs-lookup"><span data-stu-id="4bdec-844">And change all occurrences of:</span></span>
```
    if [ -z "$DRACUT_SYSTEMD" ]; then
```
<span data-ttu-id="4bdec-845">na</span><span class="sxs-lookup"><span data-stu-id="4bdec-845">to</span></span>
```
    if [ 1 ]; then
```
4. <span data-ttu-id="4bdec-846">Upravit /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh a připojit toto po hello "# otevřete LUKS zařízení":</span><span class="sxs-lookup"><span data-stu-id="4bdec-846">Edit /usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh and append this after hello “# Open LUKS device”:</span></span>
    ```
    MountPoint=/tmp-keydisk-mount
    KeyFileName=LinuxPassPhraseFileName
    echo "Trying tooget hello key from disks ..." >&2
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
5. <span data-ttu-id="4bdec-847">Spustit hello "/ usr/sbin/dracut - f - v" tooupdate hello initrd.</span><span class="sxs-lookup"><span data-stu-id="4bdec-847">Run hello “/usr/sbin/dracut -f -v” tooupdate hello initrd.</span></span>

![Instalační program centOS 7](./media/azure-security-disk-encryption/centos-encrypt-fig5.png)

### <a name="upload-encrypted-vhd-tooan-azure-storage-account"></a><span data-ttu-id="4bdec-849">Nahrát šifrované účtu úložiště Azure tooan virtuálního pevného disku</span><span class="sxs-lookup"><span data-stu-id="4bdec-849">Upload encrypted VHD tooan Azure storage account</span></span>
<span data-ttu-id="4bdec-850">Povolíte šifrování nástrojem BitLocker nebo DM-Crypt šifrování, šifrují hello místní účet úložiště tooyour toobe nahrán potřebám virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="4bdec-850">After BitLocker encryption or DM-Crypt encryption is enabled, hello local encrypted VHD needs toobe uploaded tooyour storage account.</span></span>

    Add-AzureRmVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>]

### <a name="upload-hello-disk-encryption-secret-for-hello-pre-encrypted-vm-tooyour-key-vault"></a><span data-ttu-id="4bdec-851">Nahrát hello šifrování disku tajný klíč pro hello předem šifrované virtuálních počítačů tooyour trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="4bdec-851">Upload hello disk-encryption secret for hello pre-encrypted VM tooyour key vault</span></span>
<span data-ttu-id="4bdec-852">tajný klíč Hello šifrování disku, který jste získali dříve musí být odeslán jako tajný klíč v trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-852">hello disk-encryption secret that you obtained previously must be uploaded as a secret in your key vault.</span></span> <span data-ttu-id="4bdec-853">Trezor klíčů Hello musí toohave šifrování disku a oprávnění, které jsou povolené pro vašeho klienta Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4bdec-853">hello key vault needs toohave disk encryption and permissions enabled for your Azure AD client.</span></span>

    $AadClientId = "YourAADClientId"
    $AadClientSecret = "YourAADClientSecret"

    $key vault = New-AzureRmKeyVault -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -Location $Location

    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -ServicePrincipalName $AadClientId -PermissionsToKeys all -PermissionsToSecrets all
    Set-AzureRmKeyVaultAccessPolicy -VaultName $KeyVaultName -ResourceGroupName $ResourceGroupName -EnabledForDiskEncryption


#### <a name="disk-encryption-secret-not-encrypted-with-a-kek"></a><span data-ttu-id="4bdec-854">Disk encryption tajný klíč není šifrován KEK</span><span class="sxs-lookup"><span data-stu-id="4bdec-854">Disk encryption secret not encrypted with a KEK</span></span>
<span data-ttu-id="4bdec-855">tooset až hello tajný klíč v trezoru klíčů, použijte [Set-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret).</span><span class="sxs-lookup"><span data-stu-id="4bdec-855">tooset up hello secret in your key vault, use [Set-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret).</span></span> <span data-ttu-id="4bdec-856">Pokud máte virtuální počítač Windows hello bek soubor je kódovaná jako řetězec base64 a pak odeslat tooyour trezoru klíčů pomocí hello `Set-AzureKeyVaultSecret` rutiny.</span><span class="sxs-lookup"><span data-stu-id="4bdec-856">If you have a Windows virtual machine, hello bek file is encoded as a base64 string and then uploaded tooyour key vault using hello `Set-AzureKeyVaultSecret` cmdlet.</span></span> <span data-ttu-id="4bdec-857">Pro systémy Linux hello heslo je kódovaná jako řetězec base64 a pak odeslat toohello trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="4bdec-857">For Linux, hello passphrase is encoded as a base64 string and then uploaded toohello key vault.</span></span> <span data-ttu-id="4bdec-858">Kromě toho Ujistěte se, že tento hello následující značky se nastavují při vytváření hello tajný klíč v trezoru klíčů hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-858">In addition, make sure that hello following tags are set when you create hello secret in hello key vault.</span></span>

    # This is hello passphrase that was provided for encryption during hello distribution installation
    $passphrase = "contoso-password"

    $tags = @{"DiskEncryptionKeyEncryptionAlgorithm" = "RSA-OAEP"; "DiskEncryptionKeyFileName" = "LinuxPassPhraseFileName"}
    $secretName = [guid]::NewGuid().ToString()
    $secretValue = [Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes($passphrase))
    $secureSecretValue = ConvertTo-SecureString $secretValue -AsPlainText -Force

    $secret = Set-AzureKeyVaultSecret -VaultName $KeyVaultName -Name $secretName -SecretValue $secureSecretValue -tags $tags
    $secretUrl = $secret.Id

<span data-ttu-id="4bdec-859">Použití hello `$secretUrl` v hello další krok pro [připojení disku hello operačního systému bez použití KEK](#without-using-a-kek).</span><span class="sxs-lookup"><span data-stu-id="4bdec-859">Use hello `$secretUrl` in hello next step for [attaching hello OS disk without using KEK](#without-using-a-kek).</span></span>

#### <a name="disk-encryption-secret-encrypted-with-a-kek"></a><span data-ttu-id="4bdec-860">Tajný klíč šifrování disku šifrován KEK</span><span class="sxs-lookup"><span data-stu-id="4bdec-860">Disk encryption secret encrypted with a KEK</span></span>
<span data-ttu-id="4bdec-861">Před nahráním hello toohello tajný klíč trezoru, můžete volitelně šifrováním pomocí klíče šifrovací klíč.</span><span class="sxs-lookup"><span data-stu-id="4bdec-861">Before you upload hello secret toohello key vault, you can optionally encrypt it by using a key encryption key.</span></span> <span data-ttu-id="4bdec-862">Použití hello wrap [rozhraní API](https://msdn.microsoft.com/library/azure/dn878066.aspx) toofirst šifrování tajného klíče hello pomocí hello klíče šifrovacího klíče.</span><span class="sxs-lookup"><span data-stu-id="4bdec-862">Use hello wrap [API](https://msdn.microsoft.com/library/azure/dn878066.aspx) toofirst encrypt hello secret using hello key encryption key.</span></span> <span data-ttu-id="4bdec-863">Hello výstup této operace zalamování je řetězec ve formátu base64 kódovaná adresou URL, které potom můžete nahrát jako tajný klíč pomocí hello [ `Set-AzureKeyVaultSecret` ](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret) rutiny.</span><span class="sxs-lookup"><span data-stu-id="4bdec-863">hello output of this wrap operation is a base64 URL encoded string, which you can then upload as a secret by using hello [`Set-AzureKeyVaultSecret`](/powershell/module/azurerm.keyvault/set-azurekeyvaultsecret) cmdlet.</span></span>

    # This is hello passphrase that was provided for encryption during hello distribution installation
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

<span data-ttu-id="4bdec-864">Použití `$KeyEncryptionKey` a `$secretUrl` v hello další krok pro [připojení disku hello operačního systému pomocí KEK](#using-a-kek).</span><span class="sxs-lookup"><span data-stu-id="4bdec-864">Use `$KeyEncryptionKey` and `$secretUrl` in hello next step for [attaching hello OS disk using KEK](#using-a-kek).</span></span>

### <a name="specify-a-secret-url-when-you-attach-an-os-disk"></a><span data-ttu-id="4bdec-865">Zadejte tajný adresu URL, když připojíte disku operačního systému</span><span class="sxs-lookup"><span data-stu-id="4bdec-865">Specify a secret URL when you attach an OS disk</span></span>
#### <a name="without-using-a-kek"></a><span data-ttu-id="4bdec-866">Bez použití KEK</span><span class="sxs-lookup"><span data-stu-id="4bdec-866">Without using a KEK</span></span>
<span data-ttu-id="4bdec-867">Při připojování disku hello operačního systému, je nutné toopass `$secretUrl`.</span><span class="sxs-lookup"><span data-stu-id="4bdec-867">While you are attaching hello OS disk, you need toopass `$secretUrl`.</span></span> <span data-ttu-id="4bdec-868">Adresa URL Hello byl vygenerován v části "šifrování disku tajný klíč není šifrován KEK" hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-868">hello URL was generated in hello "Disk-encryption secret not encrypted with a KEK" section.</span></span>

    Set-AzureRmVMOSDisk `
            -VM $VirtualMachine `
            -Name $OSDiskName `
            -SourceImageUri $VhdUri `
            -VhdUri $OSDiskUri `
            -Linux `
            -CreateOption FromImage `
            -DiskEncryptionKeyVaultId $KeyVault.ResourceId `
            -DiskEncryptionKeyUrl $SecretUrl

#### <a name="using-a-kek"></a><span data-ttu-id="4bdec-869">Použití KEK</span><span class="sxs-lookup"><span data-stu-id="4bdec-869">Using a KEK</span></span>
<span data-ttu-id="4bdec-870">Pokud se připojíte disk hello operačního systému, předat `$KeyEncryptionKey` a `$secretUrl`.</span><span class="sxs-lookup"><span data-stu-id="4bdec-870">When you attach hello OS disk, pass `$KeyEncryptionKey` and `$secretUrl`.</span></span> <span data-ttu-id="4bdec-871">Adresa URL Hello byl vygenerován v části "šifrování disku tajný klíč není šifrován KEK" hello.</span><span class="sxs-lookup"><span data-stu-id="4bdec-871">hello URL was generated in hello "Disk-encryption secret not encrypted with a KEK" section.</span></span>

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

## <a name="download-this-guide"></a><span data-ttu-id="4bdec-872">Stáhněte si tento průvodce</span><span class="sxs-lookup"><span data-stu-id="4bdec-872">Download this guide</span></span>
<span data-ttu-id="4bdec-873">Tento průvodce si můžete stáhnout z hello [Galerii TechNet](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).</span><span class="sxs-lookup"><span data-stu-id="4bdec-873">You can download this guide from hello [TechNet Gallery](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0).</span></span>

## <a name="for-more-information"></a><span data-ttu-id="4bdec-874">Další informace</span><span class="sxs-lookup"><span data-stu-id="4bdec-874">For more information</span></span>
[<span data-ttu-id="4bdec-875">Prozkoumejte Azure Disk Encryption pomocí Azure PowerShell – část 1</span><span class="sxs-lookup"><span data-stu-id="4bdec-875">Explore Azure Disk Encryption with Azure PowerShell - Part 1</span></span>](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/16/explore-azure-disk-encryption-with-azure-powershell.aspx?wa=wsignin1.0)  
[<span data-ttu-id="4bdec-876">Prozkoumejte Azure Disk Encryption pomocí prostředí Azure PowerShell - část 2</span><span class="sxs-lookup"><span data-stu-id="4bdec-876">Explore Azure Disk Encryption with Azure PowerShell - Part 2</span></span>](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx)
