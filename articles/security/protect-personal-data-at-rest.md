---
title: "Azure ochrana osobních dat v klidovém stavu s šifrováním | Microsoft Docs"
description: "Tento článek je součástí série, umožňují použít Azure k ochraně osobních údajů"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: d0ef3ca8d48f5ba11a89c787ff59df39eeaf673b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-encryption-technologies-protect-personal-data-at-rest-with-encryption"></a><span data-ttu-id="83b1b-103">Azure šifrovací technologie: ochrana osobních údajů v klidovém stavu se šifrováním</span><span class="sxs-lookup"><span data-stu-id="83b1b-103">Azure encryption technologies: Protect personal data at rest with encryption</span></span>

<span data-ttu-id="83b1b-104">Tento článek vám pomůže pochopit a použít technologie šifrování Azure k zabezpečení dat v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="83b1b-104">This article helps you understand and use Azure encryption technologies to secure data at rest.</span></span>

<span data-ttu-id="83b1b-105">Šifrování dat v klidovém stavu je nezbytné jako osvědčený postup chránit důvěrné nebo osobní data a, aby splňoval požadavky na dodržování předpisů a data o ochraně osobních údajů.</span><span class="sxs-lookup"><span data-stu-id="83b1b-105">Encryption of data at rest is essential as a best practice to protect sensitive or personal data and to meet compliance and data privacy requirements.</span></span>
<span data-ttu-id="83b1b-106">Šifrování v klidovém stavu slouží k útočník zabránit v přístupu k nešifrovaná data zajištěním data se šifrují, pokud na disku.</span><span class="sxs-lookup"><span data-stu-id="83b1b-106">Encryption at rest is designed to prevent the attacker from accessing the unencrypted data by ensuring the data is encrypted when on disk.</span></span>

## <a name="scenario"></a><span data-ttu-id="83b1b-107">Scénář</span><span class="sxs-lookup"><span data-stu-id="83b1b-107">Scenario</span></span> 

<span data-ttu-id="83b1b-108">Velké výletních společnosti, centrálou ve Spojených státech amerických, je rozšířit jeho operací a nabídnout itineráře v Středomoří a baltský moři, jakož i Britské ostrovy.</span><span class="sxs-lookup"><span data-stu-id="83b1b-108">A large cruise company, headquartered in the United States, is expanding its operations to offer itineraries in the Mediterranean, and Baltic seas, as well as the British Isles.</span></span> <span data-ttu-id="83b1b-109">Pro podporu těchto úsilí, získala menší výletních Víceřádkový na základě v Itálii, Německo, Dánsko a Spojeném království</span><span class="sxs-lookup"><span data-stu-id="83b1b-109">To support those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark, and the U.K.</span></span>

<span data-ttu-id="83b1b-110">Společnost používá Microsoft Azure k ukládání firemních dat v cloudu.</span><span class="sxs-lookup"><span data-stu-id="83b1b-110">The company uses Microsoft Azure to store corporate data in the cloud.</span></span> <span data-ttu-id="83b1b-111">To může zahrnovat zaměstnanců nebo zákazníků informace, jako:</span><span class="sxs-lookup"><span data-stu-id="83b1b-111">This may include employee and/or customer information such as:</span></span>

- <span data-ttu-id="83b1b-112">adresy</span><span class="sxs-lookup"><span data-stu-id="83b1b-112">addresses</span></span>
- <span data-ttu-id="83b1b-113">telefonní čísla</span><span class="sxs-lookup"><span data-stu-id="83b1b-113">phone numbers</span></span>
- <span data-ttu-id="83b1b-114">daň identifikační čísla</span><span class="sxs-lookup"><span data-stu-id="83b1b-114">tax identification numbers</span></span>
- <span data-ttu-id="83b1b-115">lékařské informace</span><span class="sxs-lookup"><span data-stu-id="83b1b-115">medical information</span></span>
- <span data-ttu-id="83b1b-116">informace o kreditní kartě</span><span class="sxs-lookup"><span data-stu-id="83b1b-116">credit card information</span></span>

<span data-ttu-id="83b1b-117">Společnosti musí při zpřístupnění dat pro tyto oddělení, které je třeba jej chránit ochrana osobních údajů zaměstnanců a zákazníků.</span><span class="sxs-lookup"><span data-stu-id="83b1b-117">The company must protect the privacy of employee and customer data while making data accessible to those departments that need it.</span></span> <span data-ttu-id="83b1b-118">(například oddělení mzdy a rezervace)</span><span class="sxs-lookup"><span data-stu-id="83b1b-118">(such as payroll and reservations departments)</span></span>

<span data-ttu-id="83b1b-119">Na řádku výletních také udržuje velké databáze potřebu a věrnost program členů, která zahrnuje osobní údaje ke sledování vztahů se zákazníky aktuální a starší.</span><span class="sxs-lookup"><span data-stu-id="83b1b-119">The cruise line also maintains a large database of reward and loyalty program members that includes personal information to track relationships with current and past customers.</span></span>

### <a name="problem-statement"></a><span data-ttu-id="83b1b-120">Popis problému</span><span class="sxs-lookup"><span data-stu-id="83b1b-120">Problem statement</span></span>

<span data-ttu-id="83b1b-121">Společnosti musí ochrany osobních údajů zaměstnanců a zákazníků osobních údajů při zpřístupnění dat pro tyto oddělení, které je třeba (například oddělení mzdy a rezervace).</span><span class="sxs-lookup"><span data-stu-id="83b1b-121">The company must protect the privacy of employees’ and customers’ personal data while making data accessible to those departments that need it (such as payroll and reservations departments).</span></span> <span data-ttu-id="83b1b-122">Tato osobní data je uložený mimo firemní řízenou datového centra a není pod kontrolou fyzické společnosti.</span><span class="sxs-lookup"><span data-stu-id="83b1b-122">This personal data is stored outside of the corporate-controlled data center and is not under the company’s physical control.</span></span>

### <a name="company-goal"></a><span data-ttu-id="83b1b-123">Cílem společnosti</span><span class="sxs-lookup"><span data-stu-id="83b1b-123">Company goal</span></span>

<span data-ttu-id="83b1b-124">Jako součást strategie zabezpečení víceúrovňová obrany zabezpečení je cílem společnosti zajistit, že všechny zdroje dat, které obsahují osobní data jsou zašifrovaná, včetně těch, které se nacházejí v cloudovém úložišti.</span><span class="sxs-lookup"><span data-stu-id="83b1b-124">As part of a multi-layered defense-in-depth security strategy, it is a company goal to ensure that all data sources that contain personal data are encrypted, including those residing in cloud storage.</span></span> <span data-ttu-id="83b1b-125">Pokud neoprávněné osoby získat přístup k osobním datům, musí být ve formuláři, který bude zobrazovat nečitelná.</span><span class="sxs-lookup"><span data-stu-id="83b1b-125">If unauthorized persons gain access to the personal data, it must be in a form that will render it unreadable.</span></span> <span data-ttu-id="83b1b-126">Použití šifrování by měla být snadná nebo transparentní – pro uživatele a správce.</span><span class="sxs-lookup"><span data-stu-id="83b1b-126">Applying encryption should be easy, or transparent – for users and administrators.</span></span>

## <a name="solutions"></a><span data-ttu-id="83b1b-127">Řešení</span><span class="sxs-lookup"><span data-stu-id="83b1b-127">Solutions</span></span>

<span data-ttu-id="83b1b-128">Azure services poskytují několik nástrojů a technologie, které vám umožní chránit osobní data v klidovém stavu šifrování ho.</span><span class="sxs-lookup"><span data-stu-id="83b1b-128">Azure services provide multiple tools and technologies to help you protect personal data at rest by encrypting it.</span></span>

### <a name="azure-key-vault"></a><span data-ttu-id="83b1b-129">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="83b1b-129">Azure Key Vault</span></span>

<span data-ttu-id="83b1b-130">[Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) poskytuje zabezpečené úložiště pro klíče, které slouží k šifrování dat v klidovém stavu uložených v služby Azure a je doporučenou klíče úložiště a správu řešení.</span><span class="sxs-lookup"><span data-stu-id="83b1b-130">[Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) provides secure storage for the keys used to encrypt data at rest in Azure services and is the recommended key storage and management solution.</span></span> <span data-ttu-id="83b1b-131">Správa klíčů pro šifrování je nezbytné pro zabezpečení uložená data.</span><span class="sxs-lookup"><span data-stu-id="83b1b-131">Encryption key management is essential to securing stored data.</span></span>

#### <a name="how-do-i-use-azure-key-vault-to-protect-keys-that-encrypt-personal-data"></a><span data-ttu-id="83b1b-132">Jak používat Azure Key Vault k ochraně klíče, které šifrování osobní údaje?</span><span class="sxs-lookup"><span data-stu-id="83b1b-132">How do I use Azure Key Vault to protect keys that encrypt personal data?</span></span>

<span data-ttu-id="83b1b-133">Pokud chcete používat Azure Key Vault, potřebujete předplatné účet Azure.</span><span class="sxs-lookup"><span data-stu-id="83b1b-133">To use Azure Key Vault, you need a subscription to an Azure account.</span></span> <span data-ttu-id="83b1b-134">Budete také potřebovat nainstalovaný Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="83b1b-134">You also need Azure PowerShell installed.</span></span> <span data-ttu-id="83b1b-135">Zahrnuje následující kroky pomocí rutin prostředí PowerShell provést následující akce:</span><span class="sxs-lookup"><span data-stu-id="83b1b-135">Steps include using PowerShell cmdlets to do the following:</span></span>

1. <span data-ttu-id="83b1b-136">Připojte se ke svým předplatným</span><span class="sxs-lookup"><span data-stu-id="83b1b-136">Connect to your subscriptions</span></span>

2. <span data-ttu-id="83b1b-137">Vytvořte trezor klíčů</span><span class="sxs-lookup"><span data-stu-id="83b1b-137">Create a key vault</span></span>

3. <span data-ttu-id="83b1b-138">Přidejte k trezoru klíč nebo tajný klíč</span><span class="sxs-lookup"><span data-stu-id="83b1b-138">Add a key or secret to the key vault</span></span>

4. <span data-ttu-id="83b1b-139">Registrace aplikace, které se používají trezor klíčů s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="83b1b-139">Register applications that will use the key vault with Azure Active Directory</span></span>

5. <span data-ttu-id="83b1b-140">Autorizace aplikací pro použití klíče nebo tajného klíče</span><span class="sxs-lookup"><span data-stu-id="83b1b-140">Authorize the applications to use the key or secret</span></span>

<span data-ttu-id="83b1b-141">K vytvoření trezoru klíčů, použijte rutinu prostředí PowerShell New-AzureRmKeyVault.</span><span class="sxs-lookup"><span data-stu-id="83b1b-141">To create a key vault, use the New-AzureRmKeyVault PowerShell CmDlt.</span></span> <span data-ttu-id="83b1b-142">Přiřadí název trezoru, název skupiny prostředků a zeměpisné umístění.</span><span class="sxs-lookup"><span data-stu-id="83b1b-142">You will assign a vault name, resource group name, and geographic location.</span></span> <span data-ttu-id="83b1b-143">Název trezoru budete používat při správě klíčů prostřednictvím jiné rutiny.</span><span class="sxs-lookup"><span data-stu-id="83b1b-143">You’ll use the vault name when managing keys via other Cmdlets.</span></span> <span data-ttu-id="83b1b-144">Aplikace, které používají trezor prostřednictvím REST API použije identifikátor URI trezoru.</span><span class="sxs-lookup"><span data-stu-id="83b1b-144">Applications that use the vault through the REST API will use the vault URI.</span></span>

<span data-ttu-id="83b1b-145">Azure Key Vault můžete zadat softwarově chráněný klíč, nebo můžete importovat existující klíč v. Soubor PFX.</span><span class="sxs-lookup"><span data-stu-id="83b1b-145">Azure Key Vault can provide a software-protected key for you, or you can import an existing key in a .PFX file.</span></span> <span data-ttu-id="83b1b-146">Můžete také ukládat tajné klíče (hesla) v trezoru.</span><span class="sxs-lookup"><span data-stu-id="83b1b-146">You can also store secrets (passwords) in the vault.</span></span>

<span data-ttu-id="83b1b-147">Můžete také vygenerovat klíč v místním HSM a jeho přenesení do HSM ve službě Key Vault, aniž by klíč opustil hranice HSM.</span><span class="sxs-lookup"><span data-stu-id="83b1b-147">You can also generate a key in your local HSM and transfer it to HSMs in the Key Vault service, without the key leaving the HSM boundary.</span></span>

<span data-ttu-id="83b1b-148">Pro podrobné pokyny týkající se použití Azure Key Vault, postupujte podle kroků v [Začínáme s Azure Key Vault.](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-get-started)</span><span class="sxs-lookup"><span data-stu-id="83b1b-148">For detailed instructions on using Azure Key Vault, follow the steps in [Get Started with Azure Key Vault.](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-get-started)</span></span>

<span data-ttu-id="83b1b-149">Seznam rutin prostředí PowerShell použít s Azure Key Vault najdete v tématu [AzureRM.KeyVault](https://docs.microsoft.com/en-us/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).</span><span class="sxs-lookup"><span data-stu-id="83b1b-149">For a list of PowerShell Cmdlets used with Azure Key Vault, see [AzureRM.KeyVault](https://docs.microsoft.com/en-us/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).</span></span>

### <a name="azure-disk-encryption-for-windows"></a><span data-ttu-id="83b1b-150">Azure Disk Encryption pro Windows</span><span class="sxs-lookup"><span data-stu-id="83b1b-150">Azure Disk Encryption for Windows</span></span>

<span data-ttu-id="83b1b-151">[Azure Disk Encryption pro Windows a virtuálních počítačů IaaS Linux](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) chrání osobní data v klidovém stavu na virtuálních počítačích Azure a integruje se službou Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="83b1b-151">[Azure Disk Encryption for Windows and Linux IaaS VMs](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) protects personal data at rest on Azure virtual machines and integrates with Azure Key Vault.</span></span> <span data-ttu-id="83b1b-152">Používá Azure Disk Encryption [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) v systému Windows a [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) v systému Linux k šifrování operačního systému a datové disky.</span><span class="sxs-lookup"><span data-stu-id="83b1b-152">Azure Disk Encryption uses [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) in Windows and [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) in Linux to encrypt both the OS and the data disks.</span></span> <span data-ttu-id="83b1b-153">Azure Disk Encryption je podporována v systému Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016 a u klientů Windows 8 a Windows 10.</span><span class="sxs-lookup"><span data-stu-id="83b1b-153">Azure Disk Encryption is supported on Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, and on Windows 8 and Windows 10 clients.</span></span>

#### <a name="how-do-i-use-azure-disk-encryption-to-protect-personal-data"></a><span data-ttu-id="83b1b-154">Jak používat Azure Disk Encryption na ochranu osobních údajů?</span><span class="sxs-lookup"><span data-stu-id="83b1b-154">How do I use Azure Disk Encryption to protect personal data?</span></span>

<span data-ttu-id="83b1b-155">Pokud chcete používat Azure Disk Encryption, potřebujete předplatné účet Azure.</span><span class="sxs-lookup"><span data-stu-id="83b1b-155">To use Azure Disk Encryption, you need a subscription to an Azure account.</span></span> <span data-ttu-id="83b1b-156">Chcete-li Azure Disk Encryption pro systém Windows a virtuální počítače s Linuxem, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="83b1b-156">To enable Azure Disk Encryption for Windows and Linux VMs, do the following:</span></span>

1. <span data-ttu-id="83b1b-157">Pomocí šablony Azure Disk Encryption Resource Manageru, prostředí PowerShell nebo rozhraní příkazového řádku (CLI) povolte šifrování disku a zadejte nastavení šifrování.</span><span class="sxs-lookup"><span data-stu-id="83b1b-157">Use the Azure Disk Encryption Resource Manager template, PowerShell, or the command line interface (CLI) to enable disk encryption and specify the  encryption configuration.</span></span> 

2. <span data-ttu-id="83b1b-158">Udělit přístup na platformu Azure ke čtení materiálu šifrování z trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="83b1b-158">Grant access to the Azure platform to read the encryption material from your key vault.</span></span>

3. <span data-ttu-id="83b1b-159">Zadejte identity aplikací Azure Active Directory (AAD) k zápisu materiálu šifrovací klíče do trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="83b1b-159">Provide an Azure Active Directory (AAD) application identity to write the encryption key material to your key vault.</span></span>

<span data-ttu-id="83b1b-160">Azure bude aktualizace virtuálního počítače a konfigurace trezoru klíčů a nastavit šifrované virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="83b1b-160">Azure will update the VM and the key vault configuration, and set up your encrypted VM.</span></span>

<span data-ttu-id="83b1b-161">Při nastavování trezoru klíčů pro podporu Azure Disk Encryption, můžete přidat klíče šifrovací klíč (KEK) pro zvýšení zabezpečení a pro podporu zálohování šifrované virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="83b1b-161">When you set up your key vault to support Azure Disk Encryption, you can add a key encryption key (KEK) for added security and to support backup of encrypted virtual machines.</span></span>

![](media/protect-personal-data-at-rest/create-key.png)

<span data-ttu-id="83b1b-162">Podrobné pokyny pro konkrétní nasazení scénáře a uživatelského prostředí jsou uvedené v [Azure Disk Encryption pro systém Windows a virtuálních počítačů IaaS Linux.](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption)</span><span class="sxs-lookup"><span data-stu-id="83b1b-162">Detailed instructions for specific deployment scenarios and user experiences are included in [Azure Disk Encryption for Windows and Linux IaaS VMs.](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption)</span></span>

### <a name="azure-storage-service-encryption"></a><span data-ttu-id="83b1b-163">Šifrování služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="83b1b-163">Azure Storage Service Encryption</span></span>

<span data-ttu-id="83b1b-164">[Azure Storage Service šifrování (SSE) pro Data v klidovém stavu](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption) pomáhá chránit a ochranu dat, aby splňovaly vaše organizace závazky zabezpečení a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="83b1b-164">[Azure Storage Service Encryption (SSE) for Data at Rest](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption) helps you protect and safeguard your data to meet your organizational security and compliance commitments.</span></span> <span data-ttu-id="83b1b-165">Úložiště Azure automaticky šifruje vaše data pomocí šifrování AES 256 bitů před uložením do úložiště a dešifruje ji před načtení.</span><span class="sxs-lookup"><span data-stu-id="83b1b-165">Azure Storage automatically encrypts your data using 256-bit AES encryption prior to persisting to storage, and decrypts it prior to retrieval.</span></span> <span data-ttu-id="83b1b-166">Tato služba je k dispozici pro soubory a objektů BLOB služby Azure.</span><span class="sxs-lookup"><span data-stu-id="83b1b-166">This service is available for Azure Blobs and Files.</span></span>

#### <a name="how-do-i-use-storage-service-encryption-to-protect-personal-data"></a><span data-ttu-id="83b1b-167">Jak použít šifrování služby úložiště na ochranu osobních údajů?</span><span class="sxs-lookup"><span data-stu-id="83b1b-167">How do I use Storage Service Encryption to protect personal data?</span></span>

<span data-ttu-id="83b1b-168">Pokud chcete povolit šifrování služby úložiště, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="83b1b-168">To enable Storage Service Encryption, do the following:</span></span>

1. <span data-ttu-id="83b1b-169">Přihlaste se k portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="83b1b-169">Log into the Azure portal.</span></span>

2. <span data-ttu-id="83b1b-170">Vyberte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="83b1b-170">Select a storage account.</span></span>

3. <span data-ttu-id="83b1b-171">V nastavení vyberte v části služby objektů Blob šifrování.</span><span class="sxs-lookup"><span data-stu-id="83b1b-171">In Settings, under the Blob Service section, select Encryption.</span></span>

4. <span data-ttu-id="83b1b-172">V části Souborová služba vyberte šifrování.</span><span class="sxs-lookup"><span data-stu-id="83b1b-172">Under the File Service section, select Encryption.</span></span>

<span data-ttu-id="83b1b-173">Po kliknutí na tlačítko Nastavení šifrování, můžete povolit nebo zakázat šifrování služby úložiště.</span><span class="sxs-lookup"><span data-stu-id="83b1b-173">After you click the Encryption setting, you can enable or disable Storage Service Encryption.</span></span>

![](media/protect-personal-data-at-rest/storage-service-encryption.png)

<span data-ttu-id="83b1b-174">Nová data se budou šifrovat.</span><span class="sxs-lookup"><span data-stu-id="83b1b-174">New data will be encrypted.</span></span> <span data-ttu-id="83b1b-175">Data v existující soubory v rámci tohoto účtu úložiště zůstat nezašifrovaný.</span><span class="sxs-lookup"><span data-stu-id="83b1b-175">Data in existing files in this storage account will remain unencrypted.</span></span>

<span data-ttu-id="83b1b-176">Po povolení šifrování, zkopírujte data do účtu úložiště pomocí jedné z následujících metod:</span><span class="sxs-lookup"><span data-stu-id="83b1b-176">After enabling encryption, copy data to the storage account using one of the following methods:</span></span>

1. <span data-ttu-id="83b1b-177">Kopírování objektů BLOB nebo soubory s [nástroj příkazového řádku AzCopy](https://docs.microsoft.com/en-us/azure/storage/storage-use-azcopy).</span><span class="sxs-lookup"><span data-stu-id="83b1b-177">Copy blobs or files with the [AzCopy Command Line utility](https://docs.microsoft.com/en-us/azure/storage/storage-use-azcopy).</span></span>

2. <span data-ttu-id="83b1b-178">[Připojit sdílenou složku pomocí protokolu SMB](https://docs.microsoft.com/en-us/azure/storage/storage-file-how-to-use-files-windows) proto můžete použít nástroj, například Robocopy pro kopírování souborů.</span><span class="sxs-lookup"><span data-stu-id="83b1b-178">[Mount a file share using SMB](https://docs.microsoft.com/en-us/azure/storage/storage-file-how-to-use-files-windows) so you can use a utility such as Robocopy to copy files.</span></span>

3. <span data-ttu-id="83b1b-179">Zkopírujte soubor nebo objekt blob dat do a z úložiště objektů blob nebo mezi účtů úložiště pomocí [knihovny klienta úložiště, jako je například .NET](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-how-to-use-blobs).</span><span class="sxs-lookup"><span data-stu-id="83b1b-179">Copy blob or file data to and from blob storage or between storage accounts using [Storage Client Libraries such as .NET](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-how-to-use-blobs).</span></span>

4.  <span data-ttu-id="83b1b-180">Použití [Storage Explorer](https://docs.microsoft.com/en-us/azure/storage/storage-explorers) nahrát objektů BLOB do účtu úložiště s povolit šifrování.</span><span class="sxs-lookup"><span data-stu-id="83b1b-180">Use a [Storage Explorer](https://docs.microsoft.com/en-us/azure/storage/storage-explorers) to upload blobs to your storage account with encryption enabled.</span></span>

### <a name="transparent-data-encryption"></a><span data-ttu-id="83b1b-181">Transparentní šifrování dat</span><span class="sxs-lookup"><span data-stu-id="83b1b-181">Transparent Data Encryption</span></span>

<span data-ttu-id="83b1b-182">Transparentní šifrování šifrování dat (TDE) je funkce v produktech SQL Azure, pomocí kterého můžete šifrovat data na úrovni databáze a serveru.</span><span class="sxs-lookup"><span data-stu-id="83b1b-182">Transparent Data Encryption (TDE) is a feature in SQL Azure by which you can encrypt data at both the database and server levels.</span></span> <span data-ttu-id="83b1b-183">Ve výchozím nastavení na všechny nově vytvořené databáze je teď povolené šifrování TDE.</span><span class="sxs-lookup"><span data-stu-id="83b1b-183">TDE is now enabled by default on all newly created databases.</span></span> <span data-ttu-id="83b1b-184">Šifrování TDE provádí v reálném čase vstupní/výstupní šifrování a dešifrování souborů protokolu a data.</span><span class="sxs-lookup"><span data-stu-id="83b1b-184">TDE performs real-time I/O encryption and decryption of the data and log files.</span></span>

#### <a name="how-do-i-use-tde-to-protect-personal-data"></a><span data-ttu-id="83b1b-185">Jak použít šifrování TDE na ochranu osobních údajů?</span><span class="sxs-lookup"><span data-stu-id="83b1b-185">How do I use TDE to protect personal data?</span></span>

<span data-ttu-id="83b1b-186">Šifrování TDE prostřednictvím portálu Azure můžete nakonfigurovat pomocí rozhraní REST API nebo pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="83b1b-186">You can configure TDE through the Azure portal, by using the REST API, or by using PowerShell.</span></span> <span data-ttu-id="83b1b-187">Pokud chcete povolit šifrování TDE na existující databázi pomocí portálu Azure, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="83b1b-187">To enable TDE on an existing database using the Azure Portal, do the following:</span></span>

1. <span data-ttu-id="83b1b-188">Navštívit portál Azure na adrese <https://portal.azure.com> a přihlaste se pomocí účtu správce Azure nebo přispěvatelem.</span><span class="sxs-lookup"><span data-stu-id="83b1b-188">Visit the Azure portal at <https://portal.azure.com> and sign-in with your Azure Administrator or Contributor account.</span></span>

2. <span data-ttu-id="83b1b-189">V levém informační zpráva klikněte na Procházet a potom klikněte databází SQL.</span><span class="sxs-lookup"><span data-stu-id="83b1b-189">On the left banner, click to BROWSE, and then click SQL databases.</span></span>

3. <span data-ttu-id="83b1b-190">V levém podokně vybraných databází SQL klikněte na své databázi uživatelů.</span><span class="sxs-lookup"><span data-stu-id="83b1b-190">With SQL databases selected in the left pane, click your user database.</span></span>

4. <span data-ttu-id="83b1b-191">V okně databáze klikněte na tlačítko všechna nastavení.</span><span class="sxs-lookup"><span data-stu-id="83b1b-191">In the database blade, click All settings.</span></span>

5. <span data-ttu-id="83b1b-192">V okně nastavení klikněte na část šifrování transparentní dat a otevřete okno transparentní dat šifrování.</span><span class="sxs-lookup"><span data-stu-id="83b1b-192">In the Settings blade, click Transparent data encryption part to open the Transparent data encryption blade.</span></span>

6. <span data-ttu-id="83b1b-193">V okně šifrování dat přesunout tlačítko pro šifrování dat na a pak klikněte na Uložit (v horní části stránky) a použijte nastavení.</span><span class="sxs-lookup"><span data-stu-id="83b1b-193">In the Data encryption blade, move the Data encryption button to On, and then click Save (at the top of the page) to apply the setting.</span></span> <span data-ttu-id="83b1b-194">Průběh transparentní šifrování dat bude Přibližná stav šifrování.</span><span class="sxs-lookup"><span data-stu-id="83b1b-194">The Encryption status will approximate the progress of the transparent data encryption.</span></span>

![Povolení šifrování dat](media/protect-personal-data-at-rest/turn-data-encryption-on.png)

<span data-ttu-id="83b1b-196">Informace o dešifrování TDE chráněné databáze a informace a pokyny o tom, jak povolit šifrování TDE najdete v článku [transparentní šifrování dat s Azure SQL Database.](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database)</span><span class="sxs-lookup"><span data-stu-id="83b1b-196">Instructions on how to enable TDE and information on decrypting TDE-protected databases and more can be found in the article [Transparent Data Encryption with Azure SQL Database.](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database)</span></span>

## <a name="summary"></a><span data-ttu-id="83b1b-197">Souhrn</span><span class="sxs-lookup"><span data-stu-id="83b1b-197">Summary</span></span>

<span data-ttu-id="83b1b-198">Společnost můžete provést jeho cílem šifrování osobních dat uložených v cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="83b1b-198">The company can accomplish its goal of encrypting personal data stored in the Azure cloud.</span></span> <span data-ttu-id="83b1b-199">Dělají to pomocí Azure Disk Encryption k ochraně celé svazky.</span><span class="sxs-lookup"><span data-stu-id="83b1b-199">They can do this by using Azure Disk Encryption to  protect entire volumes.</span></span> <span data-ttu-id="83b1b-200">To může zahrnovat jak soubory operačního systému a datové soubory, které obsahují osobní identifikovatelné údaje a další citlivá data.</span><span class="sxs-lookup"><span data-stu-id="83b1b-200">This may include both the operating system files and data files that hold personal identifiable information and other sensitive data.</span></span> <span data-ttu-id="83b1b-201">Azure šifrování služby úložiště můžete použít k ochraně osobní data, která je uložená v souborech a objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="83b1b-201">Azure Storage Service encryption can be used to protect personal data that is stored in blobs and files.</span></span> <span data-ttu-id="83b1b-202">Transparentní šifrování dat pro data, která je uložená v databázích Azure SQL, poskytuje ochranu před neoprávněným ohrožení osobních údajů.</span><span class="sxs-lookup"><span data-stu-id="83b1b-202">For data that is stored in Azure SQL databases, Transparent Data Encryption provides protection from unauthorized exposure of personal information.</span></span>

<span data-ttu-id="83b1b-203">K ochraně klíčů, které se používají k šifrování dat v Azure, může společnost použít Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="83b1b-203">To protect the keys that are used to encrypt data in Azure, the company can use Azure Key Vault.</span></span> <span data-ttu-id="83b1b-204">To zjednodušuje proces správy klíčů a umožňuje společnosti umožní zachovat kontrolu nad klíče, které k přístupu a šifrování osobní data.</span><span class="sxs-lookup"><span data-stu-id="83b1b-204">This streamlines the key management process and enables the company to maintain control of keys that access and encrypt personal data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="83b1b-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="83b1b-205">Next steps</span></span>

- [<span data-ttu-id="83b1b-206">Průvodce odstraňováním potíží Azure Disk Encryption</span><span class="sxs-lookup"><span data-stu-id="83b1b-206">Azure Disk Encryption Troubleshooting Guide</span></span>](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption-tsg)

- [<span data-ttu-id="83b1b-207">Šifrování virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="83b1b-207">Encrypt an Azure Virtual Machine</span></span>](https://docs.microsoft.com/en-us/azure/security-center/security-center-disk-encryption?toc=%2fazure%2fsecurity%2ftoc.json)

- [<span data-ttu-id="83b1b-208">Šifrování dat v Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="83b1b-208">Encryption of data in Azure Data Lake Store</span></span>](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-encryption)

- [<span data-ttu-id="83b1b-209">Azure Cosmos DB databáze šifrování v klidovém stavu</span><span class="sxs-lookup"><span data-stu-id="83b1b-209">Azure Cosmos DB database encryption at rest</span></span>](https://docs.microsoft.com/en-us/azure/cosmos-db/database-encryption-at-rest)
