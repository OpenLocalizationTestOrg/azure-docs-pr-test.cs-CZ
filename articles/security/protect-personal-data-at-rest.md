---
title: "Ochrana osobních dat v klidovém stavu s šifrováním aaaAzure | Microsoft Docs"
description: "Tento článek je součástí série, umožňují použít Azure tooprotect osobní data"
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
ms.openlocfilehash: 9af182b4897f1d04f5f519e6671f53b85073bae1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-encryption-technologies-protect-personal-data-at-rest-with-encryption"></a><span data-ttu-id="eee89-103">Azure šifrovací technologie: ochrana osobních údajů v klidovém stavu se šifrováním</span><span class="sxs-lookup"><span data-stu-id="eee89-103">Azure encryption technologies: Protect personal data at rest with encryption</span></span>

<span data-ttu-id="eee89-104">Tento článek vám pomůže pochopit a používat Azure šifrovací technologie toosecure data v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="eee89-104">This article helps you understand and use Azure encryption technologies toosecure data at rest.</span></span>

<span data-ttu-id="eee89-105">Jako nejlepší postup tooprotect citlivý nebo osobní dat a dodržování předpisů toomeet a požadavky na ochranu dat je nezbytné šifrování dat v klidovém stavu.</span><span class="sxs-lookup"><span data-stu-id="eee89-105">Encryption of data at rest is essential as a best practice tooprotect sensitive or personal data and toomeet compliance and data privacy requirements.</span></span>
<span data-ttu-id="eee89-106">Šifrování v klidovém stavu je navrženou tooprevent hello útočník z přístup k datům hello bez šifrování zajištěním hello data se šifrují, pokud na disku.</span><span class="sxs-lookup"><span data-stu-id="eee89-106">Encryption at rest is designed tooprevent hello attacker from accessing hello unencrypted data by ensuring hello data is encrypted when on disk.</span></span>

## <a name="scenario"></a><span data-ttu-id="eee89-107">Scénář</span><span class="sxs-lookup"><span data-stu-id="eee89-107">Scenario</span></span> 

<span data-ttu-id="eee89-108">Velké výletních společnosti, centrálou v USA, hello je rozšířit jeho itineráře toooffer operace v hello Středozemního a baltský moři, jakož i hello Britské ostrovy.</span><span class="sxs-lookup"><span data-stu-id="eee89-108">A large cruise company, headquartered in hello United States, is expanding its operations toooffer itineraries in hello Mediterranean, and Baltic seas, as well as hello British Isles.</span></span> <span data-ttu-id="eee89-109">toosupport těchto úsilí získala menší výletních Víceřádkový na základě v Itálii, Německo, Dánsko a hello Spojené království</span><span class="sxs-lookup"><span data-stu-id="eee89-109">toosupport those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark, and hello U.K.</span></span>

<span data-ttu-id="eee89-110">Hello společnost používá Microsoft Azure toostore firemní data v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="eee89-110">hello company uses Microsoft Azure toostore corporate data in hello cloud.</span></span> <span data-ttu-id="eee89-111">To může zahrnovat zaměstnanců nebo zákazníků informace, jako:</span><span class="sxs-lookup"><span data-stu-id="eee89-111">This may include employee and/or customer information such as:</span></span>

- <span data-ttu-id="eee89-112">adresy</span><span class="sxs-lookup"><span data-stu-id="eee89-112">addresses</span></span>
- <span data-ttu-id="eee89-113">telefonní čísla</span><span class="sxs-lookup"><span data-stu-id="eee89-113">phone numbers</span></span>
- <span data-ttu-id="eee89-114">daň identifikační čísla</span><span class="sxs-lookup"><span data-stu-id="eee89-114">tax identification numbers</span></span>
- <span data-ttu-id="eee89-115">lékařské informace</span><span class="sxs-lookup"><span data-stu-id="eee89-115">medical information</span></span>
- <span data-ttu-id="eee89-116">informace o kreditní kartě</span><span class="sxs-lookup"><span data-stu-id="eee89-116">credit card information</span></span>

<span data-ttu-id="eee89-117">Hello společnosti musí ochrany osobních údajů hello dat zaměstnanců a zákazníků při vytváření přístupné toothose oddělení dat, které je třeba ji.</span><span class="sxs-lookup"><span data-stu-id="eee89-117">hello company must protect hello privacy of employee and customer data while making data accessible toothose departments that need it.</span></span> <span data-ttu-id="eee89-118">(například oddělení mzdy a rezervace)</span><span class="sxs-lookup"><span data-stu-id="eee89-118">(such as payroll and reservations departments)</span></span>

<span data-ttu-id="eee89-119">Hello výletních řádku také udržuje velké databáze potřebu a věrnost program členů, která zahrnuje osobní údaje tootrack vztahů se zákazníky aktuální a starší.</span><span class="sxs-lookup"><span data-stu-id="eee89-119">hello cruise line also maintains a large database of reward and loyalty program members that includes personal information tootrack relationships with current and past customers.</span></span>

### <a name="problem-statement"></a><span data-ttu-id="eee89-120">Popis problému</span><span class="sxs-lookup"><span data-stu-id="eee89-120">Problem statement</span></span>

<span data-ttu-id="eee89-121">Hello společnosti musí ochrany osobních údajů hello zaměstnanců a zákazníků osobních údajů při vytváření oddělení přístupné toothose dat, které je třeba (například oddělení mzdy a rezervace).</span><span class="sxs-lookup"><span data-stu-id="eee89-121">hello company must protect hello privacy of employees’ and customers’ personal data while making data accessible toothose departments that need it (such as payroll and reservations departments).</span></span> <span data-ttu-id="eee89-122">Tato osobní data je uložený mimo hello podnikové řízenou datového centra a není pod kontrolou fyzické hello společnosti.</span><span class="sxs-lookup"><span data-stu-id="eee89-122">This personal data is stored outside of hello corporate-controlled data center and is not under hello company’s physical control.</span></span>

### <a name="company-goal"></a><span data-ttu-id="eee89-123">Cílem společnosti</span><span class="sxs-lookup"><span data-stu-id="eee89-123">Company goal</span></span>

<span data-ttu-id="eee89-124">Jako součást strategie zabezpečení víceúrovňová obrany zabezpečení je tooensure cílem společnosti, že všechny zdroje dat, které obsahují osobní data jsou zašifrovaná, včetně těch, které se nacházejí v cloudovém úložišti.</span><span class="sxs-lookup"><span data-stu-id="eee89-124">As part of a multi-layered defense-in-depth security strategy, it is a company goal tooensure that all data sources that contain personal data are encrypted, including those residing in cloud storage.</span></span> <span data-ttu-id="eee89-125">Pokud neoprávněné osoby získat přístup k toohello osobní data, musí být ve formuláři, který bude zobrazovat nečitelná.</span><span class="sxs-lookup"><span data-stu-id="eee89-125">If unauthorized persons gain access toohello personal data, it must be in a form that will render it unreadable.</span></span> <span data-ttu-id="eee89-126">Použití šifrování by měla být snadná nebo transparentní – pro uživatele a správce.</span><span class="sxs-lookup"><span data-stu-id="eee89-126">Applying encryption should be easy, or transparent – for users and administrators.</span></span>

## <a name="solutions"></a><span data-ttu-id="eee89-127">Řešení</span><span class="sxs-lookup"><span data-stu-id="eee89-127">Solutions</span></span>

<span data-ttu-id="eee89-128">Azure services poskytují několik nástrojů a technologií toohelp chránit osobní data v klidovém stavu šifrování ho.</span><span class="sxs-lookup"><span data-stu-id="eee89-128">Azure services provide multiple tools and technologies toohelp you protect personal data at rest by encrypting it.</span></span>

### <a name="azure-key-vault"></a><span data-ttu-id="eee89-129">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="eee89-129">Azure Key Vault</span></span>

<span data-ttu-id="eee89-130">[Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) poskytuje zabezpečené úložiště pro klíče hello používají tooencrypt data umístěná v služeb Azure a doporučuje hello klíče úložiště a správu řešení.</span><span class="sxs-lookup"><span data-stu-id="eee89-130">[Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) provides secure storage for hello keys used tooencrypt data at rest in Azure services and is hello recommended key storage and management solution.</span></span> <span data-ttu-id="eee89-131">Správa klíčů pro šifrování je nezbytné toosecuring uložená data.</span><span class="sxs-lookup"><span data-stu-id="eee89-131">Encryption key management is essential toosecuring stored data.</span></span>

#### <a name="how-do-i-use-azure-key-vault-tooprotect-keys-that-encrypt-personal-data"></a><span data-ttu-id="eee89-132">Použití Azure Key Vault tooprotect klíče, které šifrování osobní data</span><span class="sxs-lookup"><span data-stu-id="eee89-132">How do I use Azure Key Vault tooprotect keys that encrypt personal data?</span></span>

<span data-ttu-id="eee89-133">toouse Azure Key Vault, budete potřebovat předplatné tooan účet Azure.</span><span class="sxs-lookup"><span data-stu-id="eee89-133">toouse Azure Key Vault, you need a subscription tooan Azure account.</span></span> <span data-ttu-id="eee89-134">Budete také potřebovat nainstalovaný Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="eee89-134">You also need Azure PowerShell installed.</span></span> <span data-ttu-id="eee89-135">Zahrnuje následující kroky pomocí prostředí PowerShell rutiny toodo hello následující:</span><span class="sxs-lookup"><span data-stu-id="eee89-135">Steps include using PowerShell cmdlets toodo hello following:</span></span>

1. <span data-ttu-id="eee89-136">Připojit tooyour odběrů</span><span class="sxs-lookup"><span data-stu-id="eee89-136">Connect tooyour subscriptions</span></span>

2. <span data-ttu-id="eee89-137">Vytvořte trezor klíčů</span><span class="sxs-lookup"><span data-stu-id="eee89-137">Create a key vault</span></span>

3. <span data-ttu-id="eee89-138">Přidat klíč nebo tajný toohello trezoru klíčů</span><span class="sxs-lookup"><span data-stu-id="eee89-138">Add a key or secret toohello key vault</span></span>

4. <span data-ttu-id="eee89-139">Registrace aplikace, které budou používat hello trezor klíčů s Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="eee89-139">Register applications that will use hello key vault with Azure Active Directory</span></span>

5. <span data-ttu-id="eee89-140">Autorizovat hello aplikace toouse hello klíče nebo tajného klíče</span><span class="sxs-lookup"><span data-stu-id="eee89-140">Authorize hello applications toouse hello key or secret</span></span>

<span data-ttu-id="eee89-141">toocreate trezoru klíčů, použijte hello rutinu New-AzureRmKeyVault prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="eee89-141">toocreate a key vault, use hello New-AzureRmKeyVault PowerShell CmDlt.</span></span> <span data-ttu-id="eee89-142">Přiřadí název trezoru, název skupiny prostředků a zeměpisné umístění.</span><span class="sxs-lookup"><span data-stu-id="eee89-142">You will assign a vault name, resource group name, and geographic location.</span></span> <span data-ttu-id="eee89-143">Název trezoru hello budete používat při správě klíčů prostřednictvím jiné rutiny.</span><span class="sxs-lookup"><span data-stu-id="eee89-143">You’ll use hello vault name when managing keys via other Cmdlets.</span></span> <span data-ttu-id="eee89-144">Aplikace, které používají hello trezor prostřednictvím REST API hello použije hello trezoru identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="eee89-144">Applications that use hello vault through hello REST API will use hello vault URI.</span></span>

<span data-ttu-id="eee89-145">Azure Key Vault můžete zadat softwarově chráněný klíč, nebo můžete importovat existující klíč v. Soubor PFX.</span><span class="sxs-lookup"><span data-stu-id="eee89-145">Azure Key Vault can provide a software-protected key for you, or you can import an existing key in a .PFX file.</span></span> <span data-ttu-id="eee89-146">Také můžete uložit tajné klíče (hesla) v trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="eee89-146">You can also store secrets (passwords) in hello vault.</span></span>

<span data-ttu-id="eee89-147">Můžete také vygenerovat klíč v místním HSM a přenést ho tooHSMs v hello služby Key Vault, bez hello klíč opustil hranice HSM hello.</span><span class="sxs-lookup"><span data-stu-id="eee89-147">You can also generate a key in your local HSM and transfer it tooHSMs in hello Key Vault service, without hello key leaving hello HSM boundary.</span></span>

<span data-ttu-id="eee89-148">Pro podrobné pokyny týkající se použití Azure Key Vault, postupujte podle kroků hello v [Začínáme s Azure Key Vault.](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-get-started)</span><span class="sxs-lookup"><span data-stu-id="eee89-148">For detailed instructions on using Azure Key Vault, follow hello steps in [Get Started with Azure Key Vault.](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-get-started)</span></span>

<span data-ttu-id="eee89-149">Seznam rutin prostředí PowerShell použít s Azure Key Vault najdete v tématu [AzureRM.KeyVault](https://docs.microsoft.com/en-us/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).</span><span class="sxs-lookup"><span data-stu-id="eee89-149">For a list of PowerShell Cmdlets used with Azure Key Vault, see [AzureRM.KeyVault](https://docs.microsoft.com/en-us/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).</span></span>

### <a name="azure-disk-encryption-for-windows"></a><span data-ttu-id="eee89-150">Azure Disk Encryption pro Windows</span><span class="sxs-lookup"><span data-stu-id="eee89-150">Azure Disk Encryption for Windows</span></span>

<span data-ttu-id="eee89-151">[Azure Disk Encryption pro Windows a virtuálních počítačů IaaS Linux](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) chrání osobní data v klidovém stavu na virtuálních počítačích Azure a integruje se službou Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="eee89-151">[Azure Disk Encryption for Windows and Linux IaaS VMs](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) protects personal data at rest on Azure virtual machines and integrates with Azure Key Vault.</span></span> <span data-ttu-id="eee89-152">Používá Azure Disk Encryption [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) v systému Windows a [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) v Linux tooencrypt obě hello operačního systému a hello datových disků.</span><span class="sxs-lookup"><span data-stu-id="eee89-152">Azure Disk Encryption uses [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) in Windows and [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) in Linux tooencrypt both hello OS and hello data disks.</span></span> <span data-ttu-id="eee89-153">Azure Disk Encryption je podporována v systému Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016 a u klientů Windows 8 a Windows 10.</span><span class="sxs-lookup"><span data-stu-id="eee89-153">Azure Disk Encryption is supported on Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, and on Windows 8 and Windows 10 clients.</span></span>

#### <a name="how-do-i-use-azure-disk-encryption-tooprotect-personal-data"></a><span data-ttu-id="eee89-154">Použití Azure Disk Encryption tooprotect osobní data</span><span class="sxs-lookup"><span data-stu-id="eee89-154">How do I use Azure Disk Encryption tooprotect personal data?</span></span>

<span data-ttu-id="eee89-155">toouse Azure Disk Encryption, budete potřebovat předplatné tooan účet Azure.</span><span class="sxs-lookup"><span data-stu-id="eee89-155">toouse Azure Disk Encryption, you need a subscription tooan Azure account.</span></span> <span data-ttu-id="eee89-156">tooenable Azure Disk Encryption pro systém Windows a virtuální počítače s Linuxem, hello následující:</span><span class="sxs-lookup"><span data-stu-id="eee89-156">tooenable Azure Disk Encryption for Windows and Linux VMs, do hello following:</span></span>

1. <span data-ttu-id="eee89-157">Pomocí šablony Azure Disk Encryption Resource Manageru hello, prostředí PowerShell nebo šifrování disku tooenable hello rozhraní příkazového řádku (CLI) a zadejte nastavení šifrování.</span><span class="sxs-lookup"><span data-stu-id="eee89-157">Use hello Azure Disk Encryption Resource Manager template, PowerShell, or hello command line interface (CLI) tooenable disk encryption and specify the  encryption configuration.</span></span> 

2. <span data-ttu-id="eee89-158">Udělení přístupu toohello platformy Azure tooread hello šifrování materiálu z trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="eee89-158">Grant access toohello Azure platform tooread hello encryption material from your key vault.</span></span>

3. <span data-ttu-id="eee89-159">Zadejte Azure Active Directory (AAD) aplikace identity toowrite hello šifrovací klíče podstatným tooyour trezoru klíčů.</span><span class="sxs-lookup"><span data-stu-id="eee89-159">Provide an Azure Active Directory (AAD) application identity toowrite hello encryption key material tooyour key vault.</span></span>

<span data-ttu-id="eee89-160">Azure bude aktualizace hello virtuálního počítače a konfigurace hello trezoru klíčů a nastavit šifrované virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="eee89-160">Azure will update hello VM and hello key vault configuration, and set up your encrypted VM.</span></span>

<span data-ttu-id="eee89-161">Při nastavování vašeho trezoru klíčů toosupport Azure Disk Encryption, můžete přidat klíče šifrovací klíč (KEK) pro zvýšení zabezpečení a toosupport zálohování šifrované virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="eee89-161">When you set up your key vault toosupport Azure Disk Encryption, you can add a key encryption key (KEK) for added security and toosupport backup of encrypted virtual machines.</span></span>

![](media/protect-personal-data-at-rest/create-key.png)

<span data-ttu-id="eee89-162">Podrobné pokyny pro konkrétní nasazení scénáře a uživatelského prostředí jsou uvedené v [Azure Disk Encryption pro systém Windows a virtuálních počítačů IaaS Linux.](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption)</span><span class="sxs-lookup"><span data-stu-id="eee89-162">Detailed instructions for specific deployment scenarios and user experiences are included in [Azure Disk Encryption for Windows and Linux IaaS VMs.](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption)</span></span>

### <a name="azure-storage-service-encryption"></a><span data-ttu-id="eee89-163">Šifrování služby Azure Storage</span><span class="sxs-lookup"><span data-stu-id="eee89-163">Azure Storage Service Encryption</span></span>

<span data-ttu-id="eee89-164">[Azure Storage Service šifrování (SSE) pro Data v klidovém stavu](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption) pomáhá chránit a chrání vaše data toomeet organizační závazky zabezpečení a dodržování předpisů.</span><span class="sxs-lookup"><span data-stu-id="eee89-164">[Azure Storage Service Encryption (SSE) for Data at Rest](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption) helps you protect and safeguard your data toomeet your organizational security and compliance commitments.</span></span> <span data-ttu-id="eee89-165">Úložiště Azure automaticky šifruje vaše data pomocí toostorage předchozí toopersisting šifrování AES 256 bitů a dešifruje ji předchozí tooretrieval.</span><span class="sxs-lookup"><span data-stu-id="eee89-165">Azure Storage automatically encrypts your data using 256-bit AES encryption prior toopersisting toostorage, and decrypts it prior tooretrieval.</span></span> <span data-ttu-id="eee89-166">Tato služba je k dispozici pro soubory a objektů BLOB služby Azure.</span><span class="sxs-lookup"><span data-stu-id="eee89-166">This service is available for Azure Blobs and Files.</span></span>

#### <a name="how-do-i-use-storage-service-encryption-tooprotect-personal-data"></a><span data-ttu-id="eee89-167">Použití osobních dat tooprotect šifrování služby úložiště</span><span class="sxs-lookup"><span data-stu-id="eee89-167">How do I use Storage Service Encryption tooprotect personal data?</span></span>

<span data-ttu-id="eee89-168">tooenable šifrování služby úložiště, hello následující:</span><span class="sxs-lookup"><span data-stu-id="eee89-168">tooenable Storage Service Encryption, do hello following:</span></span>

1. <span data-ttu-id="eee89-169">Přihlaste se k hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="eee89-169">Log into hello Azure portal.</span></span>

2. <span data-ttu-id="eee89-170">Vyberte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="eee89-170">Select a storage account.</span></span>

3. <span data-ttu-id="eee89-171">V nastavení vyberte v části služby objektů Blob hello šifrování.</span><span class="sxs-lookup"><span data-stu-id="eee89-171">In Settings, under hello Blob Service section, select Encryption.</span></span>

4. <span data-ttu-id="eee89-172">V části hello části souboru služby vyberte šifrování.</span><span class="sxs-lookup"><span data-stu-id="eee89-172">Under hello File Service section, select Encryption.</span></span>

<span data-ttu-id="eee89-173">Po kliknutí na tlačítko Nastavení šifrování hello, můžete povolit nebo zakázat šifrování služby úložiště.</span><span class="sxs-lookup"><span data-stu-id="eee89-173">After you click hello Encryption setting, you can enable or disable Storage Service Encryption.</span></span>

![](media/protect-personal-data-at-rest/storage-service-encryption.png)

<span data-ttu-id="eee89-174">Nová data se budou šifrovat.</span><span class="sxs-lookup"><span data-stu-id="eee89-174">New data will be encrypted.</span></span> <span data-ttu-id="eee89-175">Data v existující soubory v rámci tohoto účtu úložiště zůstat nezašifrovaný.</span><span class="sxs-lookup"><span data-stu-id="eee89-175">Data in existing files in this storage account will remain unencrypted.</span></span>

<span data-ttu-id="eee89-176">Po povolení šifrování, zkopírujte data toohello úložiště účtu pomocí jedné z hello následující metody:</span><span class="sxs-lookup"><span data-stu-id="eee89-176">After enabling encryption, copy data toohello storage account using one of hello following methods:</span></span>

1. <span data-ttu-id="eee89-177">Kopírování objektů BLOB nebo soubory s hello [nástroj příkazového řádku AzCopy](https://docs.microsoft.com/en-us/azure/storage/storage-use-azcopy).</span><span class="sxs-lookup"><span data-stu-id="eee89-177">Copy blobs or files with hello [AzCopy Command Line utility](https://docs.microsoft.com/en-us/azure/storage/storage-use-azcopy).</span></span>

2. <span data-ttu-id="eee89-178">[Připojit sdílenou složku pomocí protokolu SMB](https://docs.microsoft.com/en-us/azure/storage/storage-file-how-to-use-files-windows) , například Robocopy toocopy soubory můžete použít nástroj.</span><span class="sxs-lookup"><span data-stu-id="eee89-178">[Mount a file share using SMB](https://docs.microsoft.com/en-us/azure/storage/storage-file-how-to-use-files-windows) so you can use a utility such as Robocopy toocopy files.</span></span>

3. <span data-ttu-id="eee89-179">Zkopírujte soubor nebo objekt blob dat tooand z úložiště objektů blob nebo mezi účtů úložiště pomocí [knihovny klienta úložiště, jako je například .NET](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-how-to-use-blobs).</span><span class="sxs-lookup"><span data-stu-id="eee89-179">Copy blob or file data tooand from blob storage or between storage accounts using [Storage Client Libraries such as .NET](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-how-to-use-blobs).</span></span>

4.  <span data-ttu-id="eee89-180">Použití [Storage Explorer](https://docs.microsoft.com/en-us/azure/storage/storage-explorers) tooupload objekty tooyour účet úložiště BLOB s povolit šifrování.</span><span class="sxs-lookup"><span data-stu-id="eee89-180">Use a [Storage Explorer](https://docs.microsoft.com/en-us/azure/storage/storage-explorers) tooupload blobs tooyour storage account with encryption enabled.</span></span>

### <a name="transparent-data-encryption"></a><span data-ttu-id="eee89-181">Transparentní šifrování dat</span><span class="sxs-lookup"><span data-stu-id="eee89-181">Transparent Data Encryption</span></span>

<span data-ttu-id="eee89-182">Transparentní šifrování šifrování dat (TDE) je funkce v produktech SQL Azure, pomocí kterého můžete šifrovat data na obou úrovních databázi a server hello.</span><span class="sxs-lookup"><span data-stu-id="eee89-182">Transparent Data Encryption (TDE) is a feature in SQL Azure by which you can encrypt data at both hello database and server levels.</span></span> <span data-ttu-id="eee89-183">Ve výchozím nastavení na všechny nově vytvořené databáze je teď povolené šifrování TDE.</span><span class="sxs-lookup"><span data-stu-id="eee89-183">TDE is now enabled by default on all newly created databases.</span></span> <span data-ttu-id="eee89-184">Šifrování TDE provádí v reálném čase vstupní/výstupní šifrování a dešifrování hello dat a souborů protokolu.</span><span class="sxs-lookup"><span data-stu-id="eee89-184">TDE performs real-time I/O encryption and decryption of hello data and log files.</span></span>

#### <a name="how-do-i-use-tde-tooprotect-personal-data"></a><span data-ttu-id="eee89-185">Jak se používá šifrování TDE tooprotect osobní údaje?</span><span class="sxs-lookup"><span data-stu-id="eee89-185">How do I use TDE tooprotect personal data?</span></span>

<span data-ttu-id="eee89-186">Šifrování TDE prostřednictvím hello portál Azure, můžete nakonfigurovat pomocí hello REST API nebo pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="eee89-186">You can configure TDE through hello Azure portal, by using hello REST API, or by using PowerShell.</span></span> <span data-ttu-id="eee89-187">tooenable TDE na existující databázi pomocí portálu Azure, hello hello následující:</span><span class="sxs-lookup"><span data-stu-id="eee89-187">tooenable TDE on an existing database using hello Azure Portal, do hello following:</span></span>

1. <span data-ttu-id="eee89-188">Navštivte hello Azure portálu na <https://portal.azure.com> a přihlaste se pomocí účtu správce Azure nebo přispěvatelem.</span><span class="sxs-lookup"><span data-stu-id="eee89-188">Visit hello Azure portal at <https://portal.azure.com> and sign-in with your Azure Administrator or Contributor account.</span></span>

2. <span data-ttu-id="eee89-189">V levém banner hello klikněte na tlačítko tooBROWSE a potom klikněte databází SQL.</span><span class="sxs-lookup"><span data-stu-id="eee89-189">On hello left banner, click tooBROWSE, and then click SQL databases.</span></span>

3. <span data-ttu-id="eee89-190">V levém podokně hello vybraných databází SQL klikněte na své databázi uživatelů.</span><span class="sxs-lookup"><span data-stu-id="eee89-190">With SQL databases selected in hello left pane, click your user database.</span></span>

4. <span data-ttu-id="eee89-191">V okně databáze hello klikněte na tlačítko všechna nastavení.</span><span class="sxs-lookup"><span data-stu-id="eee89-191">In hello database blade, click All settings.</span></span>

5. <span data-ttu-id="eee89-192">V okně Nastavení hello klikněte na okno šifrování transparentní data tooopen transparentní dat šifrování část hello.</span><span class="sxs-lookup"><span data-stu-id="eee89-192">In hello Settings blade, click Transparent data encryption part tooopen hello Transparent data encryption blade.</span></span>

6. <span data-ttu-id="eee89-193">V okně šifrování dat hello přesunout hello dat šifrování tlačítko tooOn a potom klikněte na Uložit (v hello horní části stránky hello) tooapply hello nastavení.</span><span class="sxs-lookup"><span data-stu-id="eee89-193">In hello Data encryption blade, move hello Data encryption button tooOn, and then click Save (at hello top of hello page) tooapply hello setting.</span></span> <span data-ttu-id="eee89-194">průběh hello hello transparentní šifrování dat bude Přibližná Hello stav šifrování.</span><span class="sxs-lookup"><span data-stu-id="eee89-194">hello Encryption status will approximate hello progress of hello transparent data encryption.</span></span>

![Povolení šifrování dat](media/protect-personal-data-at-rest/turn-data-encryption-on.png)

<span data-ttu-id="eee89-196">Pokyny, jak tooenable TDE a na dešifrování TDE chráněné databáze a další informace najdete v článku hello [transparentní šifrování dat s Azure SQL Database.](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database)</span><span class="sxs-lookup"><span data-stu-id="eee89-196">Instructions on how tooenable TDE and information on decrypting TDE-protected databases and more can be found in hello article [Transparent Data Encryption with Azure SQL Database.](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database)</span></span>

## <a name="summary"></a><span data-ttu-id="eee89-197">Souhrn</span><span class="sxs-lookup"><span data-stu-id="eee89-197">Summary</span></span>

<span data-ttu-id="eee89-198">Hello společnosti můžete provést jeho cílem šifrování osobních dat uložených v cloudu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="eee89-198">hello company can accomplish its goal of encrypting personal data stored in hello Azure cloud.</span></span> <span data-ttu-id="eee89-199">Můžete to provést pomocí Azure Disk Encryption příliš chránit celou svazky.</span><span class="sxs-lookup"><span data-stu-id="eee89-199">They can do this by using Azure Disk Encryption too protect entire volumes.</span></span> <span data-ttu-id="eee89-200">To může zahrnovat hello soubory operačního systému a datové soubory, které obsahují osobní identifikovatelné údaje a další citlivá data.</span><span class="sxs-lookup"><span data-stu-id="eee89-200">This may include both hello operating system files and data files that hold personal identifiable information and other sensitive data.</span></span> <span data-ttu-id="eee89-201">Azure šifrování služby úložiště může být použité tooprotect osobní data, která je uložená v souborech a objekty BLOB.</span><span class="sxs-lookup"><span data-stu-id="eee89-201">Azure Storage Service encryption can be used tooprotect personal data that is stored in blobs and files.</span></span> <span data-ttu-id="eee89-202">Transparentní šifrování dat pro data, která je uložená v databázích Azure SQL, poskytuje ochranu před neoprávněným ohrožení osobních údajů.</span><span class="sxs-lookup"><span data-stu-id="eee89-202">For data that is stored in Azure SQL databases, Transparent Data Encryption provides protection from unauthorized exposure of personal information.</span></span>

<span data-ttu-id="eee89-203">hello společnosti tooprotect hello klíče, které jsou používané tooencrypt data v Azure, můžete použít Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="eee89-203">tooprotect hello keys that are used tooencrypt data in Azure, hello company can use Azure Key Vault.</span></span> <span data-ttu-id="eee89-204">To zjednodušuje proces správy klíčů hello a umožňuje hello společnosti toomaintain kontrolu nad klíči, které k přístupu a šifrování osobní data.</span><span class="sxs-lookup"><span data-stu-id="eee89-204">This streamlines hello key management process and enables hello company toomaintain control of keys that access and encrypt personal data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="eee89-205">Další kroky</span><span class="sxs-lookup"><span data-stu-id="eee89-205">Next steps</span></span>

- [<span data-ttu-id="eee89-206">Průvodce odstraňováním potíží Azure Disk Encryption</span><span class="sxs-lookup"><span data-stu-id="eee89-206">Azure Disk Encryption Troubleshooting Guide</span></span>](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption-tsg)

- [<span data-ttu-id="eee89-207">Šifrování virtuálního počítače Azure</span><span class="sxs-lookup"><span data-stu-id="eee89-207">Encrypt an Azure Virtual Machine</span></span>](https://docs.microsoft.com/en-us/azure/security-center/security-center-disk-encryption?toc=%2fazure%2fsecurity%2ftoc.json)

- [<span data-ttu-id="eee89-208">Šifrování dat v Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="eee89-208">Encryption of data in Azure Data Lake Store</span></span>](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-encryption)

- [<span data-ttu-id="eee89-209">Azure Cosmos DB databáze šifrování v klidovém stavu</span><span class="sxs-lookup"><span data-stu-id="eee89-209">Azure Cosmos DB database encryption at rest</span></span>](https://docs.microsoft.com/en-us/azure/cosmos-db/database-encryption-at-rest)
