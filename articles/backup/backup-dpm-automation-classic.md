---
title: "Zálohování Azure: Použijte PowerShell tooback až úloh DPM | Microsoft Docs"
description: "Zjistěte, jak toodeploy a spravovat Azure Backup pro Data Protection Manager (DPM) pomocí prostředí PowerShell"
services: backup
documentationcenter: 
author: Nkolli1
manager: shreeshd
editor: 
ms.assetid: bcbcef79-9d33-4e84-a558-9866614f2cae
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: nkolli;trinadhk;anuragm;markgal
ms.openlocfilehash: 48ebe6b520857836e89749ffb6fe83d1f14c5597
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-backup-tooazure-for-data-protection-manager-dpm-servers-using-powershell"></a><span data-ttu-id="3f65f-103">Nasazení a správě zálohování tooAzure pro Data Protection Manager (DPM) servery pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="3f65f-103">Deploy and manage backup tooAzure for Data Protection Manager (DPM) servers using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3f65f-104">ARM</span><span class="sxs-lookup"><span data-stu-id="3f65f-104">ARM</span></span>](backup-dpm-automation.md)
> * [<span data-ttu-id="3f65f-105">Classic</span><span class="sxs-lookup"><span data-stu-id="3f65f-105">Classic</span></span>](backup-dpm-automation-classic.md)
>
>

<span data-ttu-id="3f65f-106">Tento článek vysvětluje, jak toouse prostředí PowerShell tooback až a obnovení dat aplikace DPM z úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="3f65f-106">This article explains how toouse PowerShell tooback up and recover DPM data from a backup vault.</span></span> <span data-ttu-id="3f65f-107">Společnost Microsoft doporučuje používat trezory služeb zotavení pro všechna nová nasazení.</span><span class="sxs-lookup"><span data-stu-id="3f65f-107">Microsoft recommends using Recovery Services vaults for all new deployments.</span></span> <span data-ttu-id="3f65f-108">Pokud jste nového uživatele Azure Backup, pomocí článku hello [nasadit a spravovat tooAzure data Data Protection Manager pomocí prostředí PowerShell](backup-dpm-automation.md), takže data ukládáte do trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="3f65f-108">If you are a new Azure Backup user, use hello article, [Deploy and manage Data Protection Manager data tooAzure using PowerShell](backup-dpm-automation.md), so you store your data in a Recovery Services vault.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3f65f-109">Teď můžete upgradovat vaše trezory služeb tooRecovery trezory Backup.</span><span class="sxs-lookup"><span data-stu-id="3f65f-109">You can now upgrade your Backup vaults tooRecovery Services vaults.</span></span> <span data-ttu-id="3f65f-110">Podrobnosti najdete v tématu hello článku [upgradu tooa trezoru zálohování trezor služeb zotavení](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="3f65f-110">For details, see hello article [Upgrade a Backup vault tooa Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="3f65f-111">Společnost Microsoft doporučuje tooupgrade zálohování trezory tooRecovery trezory služeb.</span><span class="sxs-lookup"><span data-stu-id="3f65f-111">Microsoft encourages you tooupgrade your Backup vaults tooRecovery Services vaults.</span></span> <span data-ttu-id="3f65f-112">Po 15 říjen 2017 nemůžete použít trezory Backup toocreate prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3f65f-112">After October 15, 2017, you can’t use PowerShell toocreate Backup vaults.</span></span> <span data-ttu-id="3f65f-113">**Do 1. listopadu 2017:**</span><span class="sxs-lookup"><span data-stu-id="3f65f-113">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="3f65f-114">Všechny zbývající trezory Backup bude automaticky upgradovaný tooRecovery trezory služeb.</span><span class="sxs-lookup"><span data-stu-id="3f65f-114">All remaining Backup vaults will be automatically upgraded tooRecovery Services vaults.</span></span>
>- <span data-ttu-id="3f65f-115">Můžete nebudou moct tooaccess zálohovaných dat na portálu classic hello.</span><span class="sxs-lookup"><span data-stu-id="3f65f-115">You won't be able tooaccess your backup data in hello classic portal.</span></span> <span data-ttu-id="3f65f-116">Místo toho použijte hello Azure portálu tooaccess zálohovaných dat v trezory služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="3f65f-116">Instead, use hello Azure portal tooaccess your backup data in Recovery Services vaults.</span></span>
>

## <a name="setting-up-hello-powershell-environment"></a><span data-ttu-id="3f65f-117">Nastavení prostředí PowerShell hello</span><span class="sxs-lookup"><span data-stu-id="3f65f-117">Setting up hello PowerShell environment</span></span>
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

<span data-ttu-id="3f65f-118">Než budete moct použít PowerShell toomanage záloh z tooAzure Data Protection Manager, budete potřebovat toohave hello správné prostředí v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3f65f-118">Before you can use PowerShell toomanage backups from Data Protection Manager tooAzure, you will need toohave hello right environment in PowerShell.</span></span> <span data-ttu-id="3f65f-119">Při spuštění hello relace prostředí PowerShell text hello Ujistěte se, spusťte následující příkaz tooimport hello správné moduly hello a umožňují rutiny DPM hello toocorrectly odkaz:</span><span class="sxs-lookup"><span data-stu-id="3f65f-119">At hello start of hello PowerShell session, ensure that you run hello following command tooimport hello right modules and allow you toocorrectly reference hello DPM cmdlets:</span></span>

```
PS C:> & "C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin\DpmCliInitScript.ps1"

Welcome toohello DPM Management Shell!

Full list of cmdlets: Get-Command
Only DPM cmdlets: Get-DPMCommand
Get general help: help
Get help for a cmdlet: help <cmdlet-name> or <cmdlet-name> -?
Get definition of a cmdlet: Get-Command <cmdlet-name> -Syntax
Sample DPM scripts: Get-DPMSampleScript
```

## <a name="setup-and-registration"></a><span data-ttu-id="3f65f-120">Instalace a registrace</span><span class="sxs-lookup"><span data-stu-id="3f65f-120">Setup and Registration</span></span>
<span data-ttu-id="3f65f-121">toobegin:</span><span class="sxs-lookup"><span data-stu-id="3f65f-121">toobegin:</span></span>

1. <span data-ttu-id="3f65f-122">[Stáhněte si nejnovější PowerShell](https://github.com/Azure/azure-powershell/releases) (minimální požadovaná verze je: 1.0.0)</span><span class="sxs-lookup"><span data-stu-id="3f65f-122">[Download latest PowerShell](https://github.com/Azure/azure-powershell/releases) (minimum version required is : 1.0.0)</span></span>
2. <span data-ttu-id="3f65f-123">Povolit rutiny Azure Backup hello přepnutím příliš*AzureResourceManager* režimu pomocí hello **Switch-AzureMode** příkaz:</span><span class="sxs-lookup"><span data-stu-id="3f65f-123">Enable hello Azure Backup commandlets by switching too*AzureResourceManager* mode by using hello **Switch-AzureMode** commandlet:</span></span>

```
PS C:\> Switch-AzureMode AzureResourceManager
```

<span data-ttu-id="3f65f-124">Hello následující nastavení a registrace úlohy je možné automatizovat pomocí prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="3f65f-124">hello following setup and registration tasks can be automated with PowerShell:</span></span>

* <span data-ttu-id="3f65f-125">Vytvoření trezoru záloh</span><span class="sxs-lookup"><span data-stu-id="3f65f-125">Create a backup vault</span></span>
* <span data-ttu-id="3f65f-126">Instalace agenta Azure Backup hello</span><span class="sxs-lookup"><span data-stu-id="3f65f-126">Installing hello Azure Backup agent</span></span>
* <span data-ttu-id="3f65f-127">Registrace hello služby zálohování Azure</span><span class="sxs-lookup"><span data-stu-id="3f65f-127">Registering with hello Azure Backup service</span></span>
* <span data-ttu-id="3f65f-128">Nastavení sítě</span><span class="sxs-lookup"><span data-stu-id="3f65f-128">Networking settings</span></span>
* <span data-ttu-id="3f65f-129">Nastavení šifrování</span><span class="sxs-lookup"><span data-stu-id="3f65f-129">Encryption settings</span></span>

### <a name="create-a-backup-vault"></a><span data-ttu-id="3f65f-130">Vytvoření trezoru záloh</span><span class="sxs-lookup"><span data-stu-id="3f65f-130">Create a backup vault</span></span>
> [!WARNING]
> <span data-ttu-id="3f65f-131">Pro zákazníky používající Azure Backup pro hello poprvé budete potřebovat tooregister hello Azure Backup zprostředkovatele toobe používat s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="3f65f-131">For customers using Azure Backup for hello first time, you need tooregister hello Azure Backup provider toobe used with your subscription.</span></span> <span data-ttu-id="3f65f-132">To lze provést tak, že spustíte následující příkaz hello: Register-AzureProvider - ProviderNamespace "Microsoft.Backup"</span><span class="sxs-lookup"><span data-stu-id="3f65f-132">This can be done by running hello following command: Register-AzureProvider -ProviderNamespace "Microsoft.Backup"</span></span>
>
>

<span data-ttu-id="3f65f-133">Můžete vytvořit nové úložiště záloh pomocí hello **New-AzureRMBackupVault** příkaz.</span><span class="sxs-lookup"><span data-stu-id="3f65f-133">You can create a new backup vault using hello **New-AzureRMBackupVault** commandlet.</span></span> <span data-ttu-id="3f65f-134">Trezor záloh Hello je prostředek ARM, proto musíte tooplace ji ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="3f65f-134">hello backup vault is an ARM resource, so you need tooplace it within a Resource Group.</span></span> <span data-ttu-id="3f65f-135">V prostředí Azure PowerShell konzolu se zvýšenými oprávněními spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="3f65f-135">In an elevated Azure PowerShell console, run hello following commands:</span></span>

```
PS C:\> New-AzureResourceGroup –Name “test-rg” -Region “West US”
PS C:\> $backupvault = New-AzureRMBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GRS
```

<span data-ttu-id="3f65f-136">Můžete získat seznam všech hello záloh v daném předplatném pomocí hello **Get-AzureRMBackupVault** příkaz.</span><span class="sxs-lookup"><span data-stu-id="3f65f-136">You can get a list of all hello backup vaults in a given subscription using hello **Get-AzureRMBackupVault** commandlet.</span></span>

### <a name="installing-hello-azure-backup-agent-on-a-dpm-server"></a><span data-ttu-id="3f65f-137">Instalace agenta Azure Backup hello na serveru aplikace DPM</span><span class="sxs-lookup"><span data-stu-id="3f65f-137">Installing hello Azure Backup agent on a DPM Server</span></span>
<span data-ttu-id="3f65f-138">Před instalací agenta Azure Backup hello, musíte instalační program toohave hello stažené a na serveru Windows hello.</span><span class="sxs-lookup"><span data-stu-id="3f65f-138">Before you install hello Azure Backup agent, you need toohave hello installer downloaded and present on hello Windows Server.</span></span> <span data-ttu-id="3f65f-139">Můžete získat nejnovější verzi instalačního programu hello hello hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) nebo ze stránky řídicí panel hello úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="3f65f-139">You can get hello latest version of hello installer from hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) or from hello backup vault's Dashboard page.</span></span> <span data-ttu-id="3f65f-140">Uložení instalačního programu hello tooan snadno dostupné místo jako * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="3f65f-140">Save hello installer tooan easily accessible location like *C:\Downloads\*.</span></span>

<span data-ttu-id="3f65f-141">tooinstall hello agenta, spusťte následující příkaz v konzolu prostředí PowerShell se zvýšenými oprávněními hello **na serveru DPM hello**:</span><span class="sxs-lookup"><span data-stu-id="3f65f-141">tooinstall hello agent, run hello following command in an elevated PowerShell console **on hello DPM server**:</span></span>

```
PS C:\> MARSAgentInstaller.exe /q
```

<span data-ttu-id="3f65f-142">Tím se nainstaluje hello agent se všemi možnostmi výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="3f65f-142">This installs hello agent with all hello default options.</span></span> <span data-ttu-id="3f65f-143">Hello instalace trvá několik minut v pozadí hello.</span><span class="sxs-lookup"><span data-stu-id="3f65f-143">hello installation takes a few minutes in hello background.</span></span> <span data-ttu-id="3f65f-144">Pokud nezadáte hello */nu* možnost hello **Windows Update** otevře se okno na konci hello hello toocheck instalace pro všechny aktualizace.</span><span class="sxs-lookup"><span data-stu-id="3f65f-144">If you do not specify hello */nu* option hello **Windows Update** window will open at hello end of hello installation toocheck for any updates.</span></span>

<span data-ttu-id="3f65f-145">Hello agent se zobrazí v seznamu nainstalovaných programů hello.</span><span class="sxs-lookup"><span data-stu-id="3f65f-145">hello agent will show in hello list of installed programs.</span></span> <span data-ttu-id="3f65f-146">toosee hello seznamu nainstalovaných programů, přejděte příliš**ovládací panely** > **programy** > **programy a funkce**.</span><span class="sxs-lookup"><span data-stu-id="3f65f-146">toosee hello list of installed programs, go too**Control Panel** > **Programs** > **Programs and Features**.</span></span>

![Instalaci agenta](./media/backup-dpm-automation/installed-agent-listing.png)

#### <a name="installation-options"></a><span data-ttu-id="3f65f-148">Možnosti instalace</span><span class="sxs-lookup"><span data-stu-id="3f65f-148">Installation options</span></span>
<span data-ttu-id="3f65f-149">toosee, které všechny možnosti, které jsou k dispozici prostřednictvím hello hello příkazového řádku, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="3f65f-149">toosee all hello options available via hello command-line, use hello following command:</span></span>

```
PS C:\> MARSAgentInstaller.exe /?
```

<span data-ttu-id="3f65f-150">Hello dostupné možnosti patří:</span><span class="sxs-lookup"><span data-stu-id="3f65f-150">hello available options include:</span></span>

| <span data-ttu-id="3f65f-151">Možnost</span><span class="sxs-lookup"><span data-stu-id="3f65f-151">Option</span></span> | <span data-ttu-id="3f65f-152">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="3f65f-152">Details</span></span> | <span data-ttu-id="3f65f-153">Výchozí</span><span class="sxs-lookup"><span data-stu-id="3f65f-153">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3f65f-154">/q</span><span class="sxs-lookup"><span data-stu-id="3f65f-154">/q</span></span> |<span data-ttu-id="3f65f-155">Tichá instalace</span><span class="sxs-lookup"><span data-stu-id="3f65f-155">Quiet installation</span></span> |- |
| <span data-ttu-id="3f65f-156">/ p: "umístění"</span><span class="sxs-lookup"><span data-stu-id="3f65f-156">/p:"location"</span></span> |<span data-ttu-id="3f65f-157">Cesta toohello instalační složku pro agenta Azure Backup hello.</span><span class="sxs-lookup"><span data-stu-id="3f65f-157">Path toohello installation folder for hello Azure Backup agent.</span></span> |<span data-ttu-id="3f65f-158">C:\Program Files\Microsoft Azure Recovery Services agenta</span><span class="sxs-lookup"><span data-stu-id="3f65f-158">C:\Program Files\Microsoft Azure Recovery Services Agent</span></span> |
| <span data-ttu-id="3f65f-159">/ s: "umístění"</span><span class="sxs-lookup"><span data-stu-id="3f65f-159">/s:"location"</span></span> |<span data-ttu-id="3f65f-160">Složka mezipaměti toohello cestu pro agenta Azure Backup hello.</span><span class="sxs-lookup"><span data-stu-id="3f65f-160">Path toohello cache folder for hello Azure Backup agent.</span></span> |<span data-ttu-id="3f65f-161">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span><span class="sxs-lookup"><span data-stu-id="3f65f-161">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span></span> |
| <span data-ttu-id="3f65f-162">/m</span><span class="sxs-lookup"><span data-stu-id="3f65f-162">/m</span></span> |<span data-ttu-id="3f65f-163">Výslovný souhlas tooMicrosoft aktualizace</span><span class="sxs-lookup"><span data-stu-id="3f65f-163">Opt-in tooMicrosoft Update</span></span> |- |
| <span data-ttu-id="3f65f-164">/nu</span><span class="sxs-lookup"><span data-stu-id="3f65f-164">/nu</span></span> |<span data-ttu-id="3f65f-165">Nekontrolovat aktualizace po dokončení instalace</span><span class="sxs-lookup"><span data-stu-id="3f65f-165">Do not Check for updates after installation is complete</span></span> |- |
| <span data-ttu-id="3f65f-166">/d</span><span class="sxs-lookup"><span data-stu-id="3f65f-166">/d</span></span> |<span data-ttu-id="3f65f-167">Odinstaluje Agenta Microsoft Azure Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="3f65f-167">Uninstalls Microsoft Azure Recovery Services Agent</span></span> |- |
| <span data-ttu-id="3f65f-168">/pH</span><span class="sxs-lookup"><span data-stu-id="3f65f-168">/ph</span></span> |<span data-ttu-id="3f65f-169">Adresa proxy hostitele</span><span class="sxs-lookup"><span data-stu-id="3f65f-169">Proxy Host Address</span></span> |- |
| <span data-ttu-id="3f65f-170">/Po</span><span class="sxs-lookup"><span data-stu-id="3f65f-170">/po</span></span> |<span data-ttu-id="3f65f-171">Číslo portu proxy hostitele</span><span class="sxs-lookup"><span data-stu-id="3f65f-171">Proxy Host Port Number</span></span> |- |
| <span data-ttu-id="3f65f-172">/Pu</span><span class="sxs-lookup"><span data-stu-id="3f65f-172">/pu</span></span> |<span data-ttu-id="3f65f-173">Uživatelské jméno proxy hostitele</span><span class="sxs-lookup"><span data-stu-id="3f65f-173">Proxy Host UserName</span></span> |- |
| <span data-ttu-id="3f65f-174">/pW</span><span class="sxs-lookup"><span data-stu-id="3f65f-174">/pw</span></span> |<span data-ttu-id="3f65f-175">Heslo pro proxy server</span><span class="sxs-lookup"><span data-stu-id="3f65f-175">Proxy Password</span></span> |- |

### <a name="registering-with-hello-azure-backup-service"></a><span data-ttu-id="3f65f-176">Registrace hello služby zálohování Azure</span><span class="sxs-lookup"><span data-stu-id="3f65f-176">Registering with hello Azure Backup service</span></span>
<span data-ttu-id="3f65f-177">Než můžete zaregistrovat s hello služby zálohování Azure, je třeba tooensure této hello [požadavky](backup-azure-dpm-introduction.md) jsou splněny.</span><span class="sxs-lookup"><span data-stu-id="3f65f-177">Before you can register with hello Azure Backup service, you need tooensure that hello [prerequisites](backup-azure-dpm-introduction.md) are met.</span></span> <span data-ttu-id="3f65f-178">Postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="3f65f-178">You must:</span></span>

* <span data-ttu-id="3f65f-179">Máte platné předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="3f65f-179">Have a valid Azure subscription</span></span>
* <span data-ttu-id="3f65f-180">Mít úložiště záloh</span><span class="sxs-lookup"><span data-stu-id="3f65f-180">Have a backup vault</span></span>

<span data-ttu-id="3f65f-181">toodownload hello přihlašovací údaje trezoru, spusťte hello **Get-AzureBackupVaultCredentials** příkaz do konzoly Azure PowerShell a úložiště do vhodného umístění, například * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="3f65f-181">toodownload hello vault credentials, run hello **Get-AzureBackupVaultCredentials** commandlet in an Azure PowerShell console and store it in a convenient location like *C:\Downloads\*.</span></span>

```
PS C:\> $credspath = "C:\"
PS C:\> $credsfilename = Get-AzureRMBackupVaultCredentials -Vault $backupvault -TargetLocation $credspath
PS C:\> $credsfilename
f5303a0b-fae4-4cdb-b44d-0e4c032dde26_backuprg_backuprn_2015-08-11--06-22-35.VaultCredentials
```

<span data-ttu-id="3f65f-182">Registrace počítače hello hello trezoru se provádí pomocí hello [Start-DPMCloudRegistration](https://technet.microsoft.com/library/jj612787) rutiny:</span><span class="sxs-lookup"><span data-stu-id="3f65f-182">Registering hello machine with hello vault is done using hello [Start-DPMCloudRegistration](https://technet.microsoft.com/library/jj612787) cmdlet:</span></span>

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-DPMCloudRegistration -DPMServerName "TestingServer" -VaultCredentialsFilePath $cred
```

<span data-ttu-id="3f65f-183">To bude zaregistrovat hello serveru aplikace DPM s názvem "Testovacím" úložišti Microsoft Azure pomocí hello zadané přihlašovací údaje trezoru.</span><span class="sxs-lookup"><span data-stu-id="3f65f-183">This will register hello DPM Server named “TestingServer” with Microsoft Azure Vault using hello specified vault credentials.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3f65f-184">Nepoužívejte soubor s relativní cesty toospecify hello přihlašovacími údaji.</span><span class="sxs-lookup"><span data-stu-id="3f65f-184">Do not use relative paths toospecify hello vault credentials file.</span></span> <span data-ttu-id="3f65f-185">Musíte zadat absolutní cestu jako vstupní toohello rutiny.</span><span class="sxs-lookup"><span data-stu-id="3f65f-185">You must provide an absolute path as an input toohello cmdlet.</span></span>
>
>

### <a name="initial-configuration-settings"></a><span data-ttu-id="3f65f-186">Počáteční konfigurace nastavení</span><span class="sxs-lookup"><span data-stu-id="3f65f-186">Initial configuration settings</span></span>
<span data-ttu-id="3f65f-187">Jakmile hello je DPM Server registrován s úložištěm záloh Azure hello, se spustí s výchozím nastavením odběru.</span><span class="sxs-lookup"><span data-stu-id="3f65f-187">Once hello DPM Server is registered with hello Azure Backup vault, it will start with default subscription settings.</span></span> <span data-ttu-id="3f65f-188">Tato nastavení odběru zahrnují sítě, šifrování a hello pracovní oblasti.</span><span class="sxs-lookup"><span data-stu-id="3f65f-188">These subscription settings include Networking, Encryption and hello Staging area.</span></span> <span data-ttu-id="3f65f-189">toobegin změna hello nastavení odběru je třeba toofirst získat popisovač pro stávající nastavení (výchozí) hello pomocí hello [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) rutiny:</span><span class="sxs-lookup"><span data-stu-id="3f65f-189">toobegin changing hello subscription settings you need toofirst get a handle on hello existing (default) settings using hello [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) cmdlet:</span></span>

```
$setting = Get-DPMCloudSubscriptionSetting -DPMServerName "TestingServer"
```

<span data-ttu-id="3f65f-190">Všechny změny jsou učiněna toothis místní prostředí PowerShell objekt ```$setting``` a pak je úplná objekt hello potvrdit tooDPM a toosave Azure Backup je pomocí hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3f65f-190">All modifications are made toothis local PowerShell object ```$setting```  and then hello full object is committed tooDPM and Azure Backup toosave them using hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span></span> <span data-ttu-id="3f65f-191">Je třeba toouse hello ```–Commit``` tooensure příznak, který hello změny jsou nastavené jako trvalé.</span><span class="sxs-lookup"><span data-stu-id="3f65f-191">You need toouse hello ```–Commit``` flag tooensure that hello changes are persisted.</span></span> <span data-ttu-id="3f65f-192">Hello nastavení nebude použita a pokud potvrzené používá Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="3f65f-192">hello settings will not be applied and used by Azure Backup unless committed.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

### <a name="networking"></a><span data-ttu-id="3f65f-193">Sítě</span><span class="sxs-lookup"><span data-stu-id="3f65f-193">Networking</span></span>
<span data-ttu-id="3f65f-194">Pokud hello připojení hello DPM počítač toohello služby Azure Backup na hello Internetu prostřednictvím proxy serveru, by měl pro zálohování toosucceed zadat hello nastavení proxy serveru.</span><span class="sxs-lookup"><span data-stu-id="3f65f-194">If hello connectivity of hello DPM machine toohello Azure Backup service on hello internet is through a proxy server, then hello proxy server settings should be provided for backups toosucceed.</span></span> <span data-ttu-id="3f65f-195">To se provádí pomocí hello ```-ProxyServer```, ```-ProxyPort```, ```-ProxyUsername``` a hello ```ProxyPassword``` parametry s hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3f65f-195">This is done by using hello ```-ProxyServer```, ```-ProxyPort```, ```-ProxyUsername``` and hello ```ProxyPassword``` parameters with hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span></span> <span data-ttu-id="3f65f-196">V tomto příkladu není žádný proxy server, jsme jsou explicitně vymazání údaje související s proxy serverem.</span><span class="sxs-lookup"><span data-stu-id="3f65f-196">In this example, there is no proxy server so we are explicitly clearing any proxy-related information.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoProxy
```

<span data-ttu-id="3f65f-197">Využití šířky pásma se dá taky nastavit pomocí možnosti ```-WorkHourBandwidth``` a ```-NonWorkHourBandwidth``` pro danou sadu dny v týdnu hello.</span><span class="sxs-lookup"><span data-stu-id="3f65f-197">Bandwidth usage can also be controlled with options of ```-WorkHourBandwidth``` and ```-NonWorkHourBandwidth``` for a given set of days of hello week.</span></span> <span data-ttu-id="3f65f-198">V tomto příkladu jsme nejsou nastavení žádné omezení.</span><span class="sxs-lookup"><span data-stu-id="3f65f-198">In this example we are not setting any throttling.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoThrottle
```

### <a name="configuring-hello-staging-area"></a><span data-ttu-id="3f65f-199">Konfigurace hello pracovní oblasti</span><span class="sxs-lookup"><span data-stu-id="3f65f-199">Configuring hello staging Area</span></span>
<span data-ttu-id="3f65f-200">agent Azure Backup Hello spuštěné na serveru DPM hello potřebuje dočasné úložiště pro data obnovená z cloudu hello (místní pracovní oblasti).</span><span class="sxs-lookup"><span data-stu-id="3f65f-200">hello Azure Backup agent running on hello DPM server needs temporary storage for data restored from hello cloud (local staging area).</span></span> <span data-ttu-id="3f65f-201">Konfigurovat pracovní oblasti hello pomocí hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) rutiny a hello ```-StagingAreaPath``` parametr.</span><span class="sxs-lookup"><span data-stu-id="3f65f-201">Configure hello staging area using hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet and hello ```-StagingAreaPath``` parameter.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -StagingAreaPath "C:\StagingArea"
```

<span data-ttu-id="3f65f-202">V předchozím příkladu hello, hello pracovní oblasti se nastaví příliš*C:\StagingArea* v objektu prostředí PowerShell hello ```$setting```.</span><span class="sxs-lookup"><span data-stu-id="3f65f-202">In hello example above, hello staging area will be set too*C:\StagingArea* in hello PowerShell object ```$setting```.</span></span> <span data-ttu-id="3f65f-203">Ujistěte se, že hello zadaná složka již existuje, jinak hello konečné zápisu nastavení odběru hello nezdaří.</span><span class="sxs-lookup"><span data-stu-id="3f65f-203">Ensure that hello specified folder already exists, or else hello final commit of hello subscription settings will fail.</span></span>

### <a name="encryption-settings"></a><span data-ttu-id="3f65f-204">Nastavení šifrování</span><span class="sxs-lookup"><span data-stu-id="3f65f-204">Encryption settings</span></span>
<span data-ttu-id="3f65f-205">zálohování dat odesílaných tooAzure Hello zálohování je šifrovaný tooprotect hello důvěrnost dat hello.</span><span class="sxs-lookup"><span data-stu-id="3f65f-205">hello backup data sent tooAzure Backup is encrypted tooprotect hello confidentiality of hello data.</span></span> <span data-ttu-id="3f65f-206">šifrovací přístupové heslo Hello jsou hello "password" toodecrypt hello data v době obnovení hello.</span><span class="sxs-lookup"><span data-stu-id="3f65f-206">hello encryption passphrase is hello "password" toodecrypt hello data at hello time of restore.</span></span> <span data-ttu-id="3f65f-207">Je důležité tookeep bezpečné této informace a zabezpečení po nastavení.</span><span class="sxs-lookup"><span data-stu-id="3f65f-207">It is important tookeep this information safe and secure once it is set.</span></span>

<span data-ttu-id="3f65f-208">V příkladu hello níže, převede první příkaz hello hello řetězec ```passphrase123456789``` tooa zabezpečený řetězec a přiřadí hello zabezpečený řetězec toohello proměnné s názvem ```$Passphrase```.</span><span class="sxs-lookup"><span data-stu-id="3f65f-208">In hello example below, hello first command converts hello string ```passphrase123456789``` tooa secure string and assigns hello secure string toohello variable named ```$Passphrase```.</span></span> <span data-ttu-id="3f65f-209">druhý příkaz Hello nastaví hello zabezpečený řetězec ```$Passphrase``` jako hello heslo pro šifrování záloh.</span><span class="sxs-lookup"><span data-stu-id="3f65f-209">hello second command sets hello secure string in ```$Passphrase``` as hello password for encrypting backups.</span></span>

```
PS C:\> $Passphrase = ConvertTo-SecureString -string "passphrase123456789" -AsPlainText -Force

PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -EncryptionPassphrase $Passphrase
```

> [!IMPORTANT]
> <span data-ttu-id="3f65f-210">Zachování informací o přístupové heslo hello bezpečném, po nastavení.</span><span class="sxs-lookup"><span data-stu-id="3f65f-210">Keep hello passphrase information safe and secure once it is set.</span></span> <span data-ttu-id="3f65f-211">Nebudete moct toorestore dat z Azure bez tohoto hesla.</span><span class="sxs-lookup"><span data-stu-id="3f65f-211">You will not be able toorestore data from Azure without this passphrase.</span></span>
>
>

<span data-ttu-id="3f65f-212">V tomto okamžiku by měl provedení všech hello požadované změny toohello ```$setting``` objektu.</span><span class="sxs-lookup"><span data-stu-id="3f65f-212">At this point, you should have made all hello required changes toohello ```$setting``` object.</span></span> <span data-ttu-id="3f65f-213">Mějte na paměti, toocommit hello změny.</span><span class="sxs-lookup"><span data-stu-id="3f65f-213">Remember toocommit hello changes.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="protect-data-tooazure-backup"></a><span data-ttu-id="3f65f-214">Ochrana dat tooAzure zálohování</span><span class="sxs-lookup"><span data-stu-id="3f65f-214">Protect data tooAzure Backup</span></span>
<span data-ttu-id="3f65f-215">V této části přidáte tooDPM produkčním serveru a poté proveďte ochranu úložiště DPM toolocal hello data a pak tooAzure zálohování.</span><span class="sxs-lookup"><span data-stu-id="3f65f-215">In this section, you will add a production server tooDPM and then protect hello data toolocal DPM storage and then tooAzure Backup.</span></span> <span data-ttu-id="3f65f-216">V příkladech hello si předvedeme jak tooback soubory a složky.</span><span class="sxs-lookup"><span data-stu-id="3f65f-216">In hello examples we will demonstrate how tooback up files and folders.</span></span> <span data-ttu-id="3f65f-217">Hello logiky můžete snadno být rozšířené toobackup libovolný zdroj dat aplikace DPM podporováno.</span><span class="sxs-lookup"><span data-stu-id="3f65f-217">hello logic can easily be extended toobackup any DPM-supported data source.</span></span> <span data-ttu-id="3f65f-218">Všechny zálohy aplikace DPM se řídí pomocí ochranu skupiny (PG) s čtyři části:</span><span class="sxs-lookup"><span data-stu-id="3f65f-218">All your DPM backups are governed by a Protection Group (PG) with four parts:</span></span>

1. <span data-ttu-id="3f65f-219">**Členy skupiny** je seznam všech hello chránitelné objekty (také označované jako *zdrojů dat* v DPM), které chcete tooprotect v hello stejné skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="3f65f-219">**Group members** is a list of all hello protectable objects (also known as *Datasources* in DPM) that you want tooprotect in hello same protection group.</span></span> <span data-ttu-id="3f65f-220">Můžete například tooprotect produkční virtuálních počítačů na jednu skupinu ochrany a databáze systému SQL Server v jiné skupině ochrany mají různé požadavky na zálohování.</span><span class="sxs-lookup"><span data-stu-id="3f65f-220">For example, you may want tooprotect production VMs in one protection group and SQL Server databases in another protection group as they may have different backup requirements.</span></span> <span data-ttu-id="3f65f-221">Než můžete zálohovat žádný zdroj dat na provozním serveru je třeba jestli hello toomake agenta DPM na hello server je nainstalovaná a je spravovaných aplikací DPM.</span><span class="sxs-lookup"><span data-stu-id="3f65f-221">Before you can back up any datasource on a production server you need toomake sure hello DPM Agent is installed on hello server and is managed by DPM.</span></span> <span data-ttu-id="3f65f-222">Postupujte podle kroků hello pro [instalaci agenta aplikace DPM hello](https://technet.microsoft.com/library/bb870935.aspx) a jejím propojením toohello vhodné serveru aplikace DPM.</span><span class="sxs-lookup"><span data-stu-id="3f65f-222">Follow hello steps for [installing hello DPM Agent](https://technet.microsoft.com/library/bb870935.aspx) and linking it toohello appropriate DPM Server.</span></span>
2. <span data-ttu-id="3f65f-223">**Způsob ochrany dat** určuje hello cíl zálohování umístění - páska, disk a cloud.</span><span class="sxs-lookup"><span data-stu-id="3f65f-223">**Data protection method** specifies hello target backup locations - tape, disk, and cloud.</span></span> <span data-ttu-id="3f65f-224">V našem příkladu jsme bude chránit dat toohello místní disk a toohello v cloudu.</span><span class="sxs-lookup"><span data-stu-id="3f65f-224">In our example we will protect data toohello local disk and toohello cloud.</span></span>
3. <span data-ttu-id="3f65f-225">A **plán zálohování** určující při zálohování potřebovat toobe prováděné a jak často hello se mají synchronizovat data mezi hello serveru DPM a hello provozním serveru.</span><span class="sxs-lookup"><span data-stu-id="3f65f-225">A **backup schedule** that specifies when backups need toobe taken and how often hello data should be synchronized between hello DPM Server and hello production server.</span></span>
4. <span data-ttu-id="3f65f-226">A **plán uchovávání informací** určující, jak dlouho body obnovení hello tooretain v Azure.</span><span class="sxs-lookup"><span data-stu-id="3f65f-226">A **retention schedule** that specifies how long tooretain hello recovery points in Azure.</span></span>

### <a name="creating-a-protection-group"></a><span data-ttu-id="3f65f-227">Vytvoření skupiny ochrany</span><span class="sxs-lookup"><span data-stu-id="3f65f-227">Creating a protection group</span></span>
<span data-ttu-id="3f65f-228">Začněte vytvořením nové skupiny ochrany pomocí hello [New-DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3f65f-228">Start by creating a new Protection Group using hello [New-DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) cmdlet.</span></span>

```
PS C:\> $PG = New-DPMProtectionGroup -DPMServerName " TestingServer " -Name "ProtectGroup01"
```

<span data-ttu-id="3f65f-229">Hello výše rutina vytvoří skupiny ochrany s názvem *ProtectGroup01*.</span><span class="sxs-lookup"><span data-stu-id="3f65f-229">hello above cmdlet will create a Protection Group named *ProtectGroup01*.</span></span> <span data-ttu-id="3f65f-230">Existující skupiny ochrany můžete také upravit novější tooadd zálohování toohello cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="3f65f-230">An existing protection group can also be modified later tooadd backup toohello Azure cloud.</span></span> <span data-ttu-id="3f65f-231">Ale toomake změny toohello skupiny ochrany – nové nebo existující - potřebujeme tooget popisovač na *upravitelnými* objekt, který používá hello [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3f65f-231">However, toomake any changes toohello Protection Group - new or existing - we need tooget a handle on a *modifiable* object using hello [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) cmdlet.</span></span>

```
PS C:\> $MPG = Get-ModifiableProtectionGroup $PG
```

### <a name="adding-group-members-toohello-protection-group"></a><span data-ttu-id="3f65f-232">Přidání skupiny toohello členy skupiny ochrany</span><span class="sxs-lookup"><span data-stu-id="3f65f-232">Adding group members toohello Protection Group</span></span>
<span data-ttu-id="3f65f-233">Každý Agent aplikace DPM zná hello seznam zdrojů dat na hello serveru, který je nainstalovaný.</span><span class="sxs-lookup"><span data-stu-id="3f65f-233">Each DPM Agent knows hello list of datasources on hello server that it is installed on.</span></span> <span data-ttu-id="3f65f-234">tooadd toohello zdroj dat skupiny ochrany, hello agenta aplikace DPM musí toofirst odeslání seznamu hello zdrojů dat zpět toohello DPM server.</span><span class="sxs-lookup"><span data-stu-id="3f65f-234">tooadd a datasource toohello Protection Group, hello DPM Agent needs toofirst send a list of hello datasources back toohello DPM server.</span></span> <span data-ttu-id="3f65f-235">Jeden nebo více zdrojů dat jsou pak vybrali a přidat toohello skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="3f65f-235">One or more datasources are then selected and added toohello Protection Group.</span></span> <span data-ttu-id="3f65f-236">Hello prostředí PowerShell kroky potřebné tooget dosáhnout tyto:</span><span class="sxs-lookup"><span data-stu-id="3f65f-236">hello PowerShell steps needed tooget achieve this are:</span></span>

1. <span data-ttu-id="3f65f-237">Získat seznam všech serverů spravovaných aplikací DPM prostřednictvím hello agenta aplikace DPM.</span><span class="sxs-lookup"><span data-stu-id="3f65f-237">Fetch a list of all servers managed by DPM through hello DPM Agent.</span></span>
2. <span data-ttu-id="3f65f-238">Vyberte konkrétní server.</span><span class="sxs-lookup"><span data-stu-id="3f65f-238">Choose a specific server.</span></span>
3. <span data-ttu-id="3f65f-239">Získat seznam všech zdrojů dat na serveru hello.</span><span class="sxs-lookup"><span data-stu-id="3f65f-239">Fetch a list of all datasources on hello server.</span></span>
4. <span data-ttu-id="3f65f-240">Vyberte jeden nebo více zdrojů dat a přidat je toohello skupiny ochrany</span><span class="sxs-lookup"><span data-stu-id="3f65f-240">Choose one or more datasources and add them toohello Protection Group</span></span>

<span data-ttu-id="3f65f-241">Hello seznam serverů, na které hello agenta aplikace DPM je nainstalovaná a je spravován systémem hello serveru aplikace DPM je získán pomocí hello [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3f65f-241">hello list of servers on which hello DPM Agent is installed and is being managed by hello DPM Server is acquired with hello [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) cmdlet.</span></span> <span data-ttu-id="3f65f-242">V tomto příkladu jsme bude filtrovat a konfigurovat jenom PS s názvem *productionserver01* pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="3f65f-242">In this example we will filter and only configure PS with name *productionserver01* for backup.</span></span>

```
PS C:\> $server = Get-ProductionServer -DPMServerName "TestingServer" | where {($_.servername) –contains “productionserver01”
```

<span data-ttu-id="3f65f-243">Nyní načíst hello seznam zdrojů dat v ```$server``` pomocí hello [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3f65f-243">Now fetch hello list of datasources on ```$server``` using hello [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) cmdlet.</span></span> <span data-ttu-id="3f65f-244">V tomto příkladu jsme pro svazek hello filtrování * D:\* který chceme tooconfigure pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="3f65f-244">In this example we are filtering for hello volume *D:\* which we want tooconfigure for backup.</span></span> <span data-ttu-id="3f65f-245">Tento zdroj dat se pak přidá toohello skupiny ochrany pomocí hello [přidat DPMChildDatasource](https://technet.microsoft.com/library/hh881732) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3f65f-245">This datasource is then added toohello Protection Group using hello [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732) cmdlet.</span></span> <span data-ttu-id="3f65f-246">Mějte na paměti, toouse hello *modifable* objektu skupiny ochrany ```$MPG``` dodatky toomake hello.</span><span class="sxs-lookup"><span data-stu-id="3f65f-246">Remember toouse hello *modifable* protection group object ```$MPG``` toomake hello additions.</span></span>

```
PS C:\> $DS = Get-Datasource -ProductionServer $server -Inquire | where { $_.Name -contains “D:\” }

PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS
```

<span data-ttu-id="3f65f-247">Opakujte tento krok tolikrát, kolikrát podle potřeby, dokud nepřidáte všechny hello zvolené skupiny zdrojů dat toohello ochrany.</span><span class="sxs-lookup"><span data-stu-id="3f65f-247">Repeat this step as many times as required, until you have added all hello chosen datasources toohello protection group.</span></span> <span data-ttu-id="3f65f-248">Můžete začít s jenom jeden zdroj dat a dokončení hello pracovní postup pro vytváření hello skupiny ochrany a později přidat další zdroje dat toohello skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="3f65f-248">You can also start with just one datasource, and complete hello workflow for creating hello Protection Group, and at a later point add more datasources toohello Protection Group.</span></span>

### <a name="selecting-hello-data-protection-method"></a><span data-ttu-id="3f65f-249">Vyberte způsob ochrany dat hello</span><span class="sxs-lookup"><span data-stu-id="3f65f-249">Selecting hello data protection method</span></span>
<span data-ttu-id="3f65f-250">Jakmile hello zdroje dat byly přidány toohello skupiny ochrany, hello dalším krokem je způsob ochrany hello toospecify pomocí hello [Set-DPMProtectionType](https://technet.microsoft.com/library/hh881725) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3f65f-250">Once hello datasources have been added toohello Protection Group, hello next step is toospecify hello protection method using hello [Set-DPMProtectionType](https://technet.microsoft.com/library/hh881725) cmdlet.</span></span> <span data-ttu-id="3f65f-251">V tomto příkladu hello skupiny ochrany bude instalační program pro místního disku a zálohování.</span><span class="sxs-lookup"><span data-stu-id="3f65f-251">In this example, hello Protection Group will be setup for local disk and cloud backup.</span></span> <span data-ttu-id="3f65f-252">Musíte taky toospecify hello zdroj dat, které chcete pomocí hello toocloud tooprotect [přidat DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) rutiny příznakem - Online.</span><span class="sxs-lookup"><span data-stu-id="3f65f-252">You also need toospecify hello datasource that you want tooprotect toocloud using hello [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) cmdlet with -Online flag.</span></span>

```
PS C:\> Set-DPMProtectionType -ProtectionGroup $MPG -ShortTerm Disk –LongTerm Online
PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS –Online
```

### <a name="setting-hello-retention-range"></a><span data-ttu-id="3f65f-253">Nastavení rozsah uchování hello</span><span class="sxs-lookup"><span data-stu-id="3f65f-253">Setting hello retention range</span></span>
<span data-ttu-id="3f65f-254">Nastavit hello uchování pro body zálohy hello pomocí hello [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3f65f-254">Set hello retention for hello backup points using hello [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span></span> <span data-ttu-id="3f65f-255">Při uchování hello liché tooset může zdát, než byla definována hello plán zálohování, pomocí hello ```Set-DPMPolicyObjective``` rutiny automaticky nastaví výchozí plán zálohování, který je pak možné upravit.</span><span class="sxs-lookup"><span data-stu-id="3f65f-255">While it might seem odd tooset hello retention before hello backup schedule has been defined, using hello ```Set-DPMPolicyObjective``` cmdlet automatically sets a default backup schedule that can then be modified.</span></span> <span data-ttu-id="3f65f-256">Je možné tooset plán zálohování hello vždy nejprve a hello zásady uchovávání informací po.</span><span class="sxs-lookup"><span data-stu-id="3f65f-256">It is always possible tooset hello backup schedule first and hello retention policy after.</span></span>

<span data-ttu-id="3f65f-257">V příkladu hello níže hello rutiny nastaví parametry uchování hello zálohování na disk.</span><span class="sxs-lookup"><span data-stu-id="3f65f-257">In hello example below, hello cmdlet sets hello retention parameters for disk backups.</span></span> <span data-ttu-id="3f65f-258">To bude uchování záloh pro 10 dnů a synchronizaci dat každých 6 hodin mezi hello provozním serveru a server aplikace DPM hello.</span><span class="sxs-lookup"><span data-stu-id="3f65f-258">This will retain backups for 10 days, and sync data every 6 hours between hello production server and hello DPM server.</span></span> <span data-ttu-id="3f65f-259">Hello ```SynchronizationFrequencyMinutes``` nedefinuje četnost zálohování je bod vytvořen, ale jak často dat je server DPM zkopírovaný toohello; záloh zabrání příliš velká.</span><span class="sxs-lookup"><span data-stu-id="3f65f-259">hello ```SynchronizationFrequencyMinutes``` doesn't define how often a backup point is created, but how often data is copied toohello DPM server; this prevents backups from becoming too large.</span></span>

```
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -RetentionRangeInDays 10 -SynchronizationFrequencyMinutes 360
```

<span data-ttu-id="3f65f-260">Pro zálohování budete tooAzure (aplikace DPM se týká toothese jako zálohování Online) rozsahy uchovávání hello lze nakonfigurovat pro [dlouhodobé uchovávání používá schéma Dědečka. otec SYN (GFS)](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="3f65f-260">For backups going tooAzure (DPM refers toothese as Online backups) hello retention ranges can be configured for [long term retention using a Grandfather-Father-Son scheme (GFS)](backup-azure-backup-cloud-as-tape.md).</span></span> <span data-ttu-id="3f65f-261">To znamená můžete definovat zásady uchovávání informací kombinované zahrnující denní, týdenní, měsíční a roční zásady uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="3f65f-261">That is, you can define a combined retention policy involving daily, weekly, monthly and yearly retention policies.</span></span> <span data-ttu-id="3f65f-262">V tomto příkladu vytvoření představující schéma hello komplexní uchovávání, který má být pole a pak nakonfigurujte rozsah uchování hello pomocí hello [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3f65f-262">In this example, we create an array representing hello complex retention scheme that we want, and then configure hello retention range using hello [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span></span>

```
PS C:\> $RRlist = @()
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 180, Days)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 104, Weeks)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 60, Month)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 10, Years)
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -OnlineRetentionRangeList $RRlist
```

### <a name="set-hello-backup-schedule"></a><span data-ttu-id="3f65f-263">Plán zálohování sady hello</span><span class="sxs-lookup"><span data-stu-id="3f65f-263">Set hello backup schedule</span></span>
<span data-ttu-id="3f65f-264">Aplikace DPM nastaví výchozí plán zálohování automaticky, pokud zadáte hello cíl ochrany pomocí hello ```Set-DPMPolicyObjective``` rutiny.</span><span class="sxs-lookup"><span data-stu-id="3f65f-264">DPM sets a default backup schedule automatically if you specify hello protection objective using hello ```Set-DPMPolicyObjective``` cmdlet.</span></span> <span data-ttu-id="3f65f-265">toochange hello výchozích plánů, použijte hello [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) následovanou hello [Set-DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3f65f-265">toochange hello default schedules, use hello [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) cmdlet followed by hello [Set-DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) cmdlet.</span></span>

```
PS C:\> $onlineSch = Get-DPMPolicySchedule -ProtectionGroup $mpg -LongTerm Online
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[0] -TimesOfDay 02:00
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[1] -TimesOfDay 02:00 -DaysOfWeek Sa,Su –Interval 1
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[2] -TimesOfDay 02:00 -RelativeIntervals First,Third –DaysOfWeek Sa
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[3] -TimesOfDay 02:00 -DaysOfMonth 2,5,8,9 -Months Jan,Jul
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```

<span data-ttu-id="3f65f-266">Ve výše uvedeném příkladu hello ```$onlineSch``` je pole se čtyřmi prvky, které obsahuje hello existující plán ochranu online pro hello skupiny ochrany ve schématu GFS hello:</span><span class="sxs-lookup"><span data-stu-id="3f65f-266">In hello example above, ```$onlineSch``` is an array with four elements that contains hello existing online protection schedule for hello Protection Group in hello GFS scheme:</span></span>

1. <span data-ttu-id="3f65f-267">```$onlineSch[0]```bude obsahovat denní plán hello</span><span class="sxs-lookup"><span data-stu-id="3f65f-267">```$onlineSch[0]``` will contain hello daily schedule</span></span>
2. <span data-ttu-id="3f65f-268">```$onlineSch[1]```bude obsahovat týdenní plán hello</span><span class="sxs-lookup"><span data-stu-id="3f65f-268">```$onlineSch[1]``` will contain hello weekly schedule</span></span>
3. <span data-ttu-id="3f65f-269">```$onlineSch[2]```bude obsahovat plánování měsíčně hello</span><span class="sxs-lookup"><span data-stu-id="3f65f-269">```$onlineSch[2]``` will contain hello monthly schedule</span></span>
4. <span data-ttu-id="3f65f-270">```$onlineSch[3]```bude obsahovat roční plán hello</span><span class="sxs-lookup"><span data-stu-id="3f65f-270">```$onlineSch[3]``` will contain hello yearly schedule</span></span>

<span data-ttu-id="3f65f-271">Takže pokud budete potřebovat toomodify hello týdenní plán, je třeba toorefer toohello ```$onlineSch[1]```.</span><span class="sxs-lookup"><span data-stu-id="3f65f-271">So if you need toomodify hello weekly schedule, you need toorefer toohello ```$onlineSch[1]```.</span></span>

### <a name="initial-backup"></a><span data-ttu-id="3f65f-272">Prvotní zálohování</span><span class="sxs-lookup"><span data-stu-id="3f65f-272">Initial backup</span></span>
<span data-ttu-id="3f65f-273">Při zálohování zdroje dat pro hello poprvé, musí aplikace DPM toocreate počáteční repliky, která vytvoří kopii toobe hello zdroje dat chráněné na svazku repliky DPM.</span><span class="sxs-lookup"><span data-stu-id="3f65f-273">When backing up a datasource for hello first time, DPM needs toocreate an initial replica which will create a copy of hello datasource toobe protected on DPM replica volume.</span></span> <span data-ttu-id="3f65f-274">Tato aktivita bude naplánována buď pro určitý čas nebo lze spustit ručně, pomocí hello [Set-DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) rutiny s parametrem hello ```-NOW```.</span><span class="sxs-lookup"><span data-stu-id="3f65f-274">This activity can either be scheduled for a specific time, or can be triggered manually, using hello [Set-DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) cmdlet with hello parameter ```-NOW```.</span></span>

```
PS C:\> Set-DPMReplicaCreationMethod -ProtectionGroup $MPG -NOW
```
### <a name="changing-hello-size-of-dpm-replica--recovery-point-volume"></a><span data-ttu-id="3f65f-275">Změna velikosti hello repliky aplikace DPM a svazek bodu obnovení</span><span class="sxs-lookup"><span data-stu-id="3f65f-275">Changing hello size of DPM Replica & recovery point volume</span></span>
<span data-ttu-id="3f65f-276">Můžete také změnit hello velikost svazku repliky DPM, jakož i stínové kopie svazku pomocí [Set-DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) rutiny jako hello následujícím příkladu: Get-DatasourceDiskAllocation - Datasource $DS Set-DatasourceDiskAllocation - Datasource $DS - ProtectionGroup $MPG – ruční - ReplicaArea (2 gb) - ShadowCopyArea (2 gb)</span><span class="sxs-lookup"><span data-stu-id="3f65f-276">You can also change hello size of DPM Replica volume as well as Shadow Copy volume using [Set-DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) cmdlet as in hello below example: Get-DatasourceDiskAllocation -Datasource $DS Set-DatasourceDiskAllocation -Datasource $DS -ProtectionGroup $MPG -manual -ReplicaArea (2gb) -ShadowCopyArea (2gb)</span></span>

### <a name="committing-hello-changes-toohello-protection-group"></a><span data-ttu-id="3f65f-277">Potvrzení hello změny toohello skupiny ochrany</span><span class="sxs-lookup"><span data-stu-id="3f65f-277">Committing hello changes toohello Protection Group</span></span>
<span data-ttu-id="3f65f-278">Nakonec hello změny nutné toobe potvrzené předtím, než aplikace DPM může provést zálohování hello za hello novou konfiguraci skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="3f65f-278">Finally, hello changes need toobe committed before DPM can take hello backup per hello new Protection Group configuration.</span></span> <span data-ttu-id="3f65f-279">To se provádí pomocí hello [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3f65f-279">This is done using hello [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) cmdlet.</span></span>

```
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```
## <a name="view-hello-backup-points"></a><span data-ttu-id="3f65f-280">Body zálohy hello zobrazení</span><span class="sxs-lookup"><span data-stu-id="3f65f-280">View hello backup points</span></span>
<span data-ttu-id="3f65f-281">Můžete použít hello [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) rutiny tooget seznam všech bodů obnovení pro zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="3f65f-281">You can use hello [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) cmdlet tooget a list of all recovery points for a datasource.</span></span> <span data-ttu-id="3f65f-282">V tomto příkladu provedeme následující:</span><span class="sxs-lookup"><span data-stu-id="3f65f-282">In this example, we will:</span></span>

* <span data-ttu-id="3f65f-283">načtení všech hello PGs na serveru DPM hello, které se budou ukládat do pole```$PG```</span><span class="sxs-lookup"><span data-stu-id="3f65f-283">fetch all hello PGs on hello DPM server which will be stored in an array ```$PG```</span></span>
* <span data-ttu-id="3f65f-284">získat odpovídající toohello hello zdrojů dat```$PG[0]```</span><span class="sxs-lookup"><span data-stu-id="3f65f-284">get hello datasources corresponding toohello ```$PG[0]```</span></span>
* <span data-ttu-id="3f65f-285">získání všech hello bodů obnovení pro zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="3f65f-285">get all hello recovery points for a datasource.</span></span>

```
PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online
```

## <a name="restore-data-protected-on-azure"></a><span data-ttu-id="3f65f-286">Obnovit data chráněná v Azure</span><span class="sxs-lookup"><span data-stu-id="3f65f-286">Restore data protected on Azure</span></span>
<span data-ttu-id="3f65f-287">Obnovení dat je kombinací ```RecoverableItem``` objektu a ```RecoveryOption``` objektu.</span><span class="sxs-lookup"><span data-stu-id="3f65f-287">Restoring data is a combination of a ```RecoverableItem``` object and a ```RecoveryOption``` object.</span></span> <span data-ttu-id="3f65f-288">V předchozí části hello My seznam hello body zálohy pro zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="3f65f-288">In hello previous section, we got a list of hello backup points for a datasource.</span></span>

<span data-ttu-id="3f65f-289">V příkladu hello dole ukážeme, jak toorestore Hyper-V virtuální počítač z Azure Backup kombinování zálohování body s hello cíle pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="3f65f-289">In hello example below, we demonstrate how toorestore a Hyper-V virtual machine from Azure Backup by combining backup points with hello target for recovery.</span></span> <span data-ttu-id="3f65f-290">To zahrnuje následující:</span><span class="sxs-lookup"><span data-stu-id="3f65f-290">This includes:</span></span>

* <span data-ttu-id="3f65f-291">Vytváření možnost obnovení pomocí hello [New-DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) rutiny.</span><span class="sxs-lookup"><span data-stu-id="3f65f-291">Creating a recovery option using hello  [New-DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) cmdlet.</span></span>
* <span data-ttu-id="3f65f-292">Načítání pole hello body zálohy pomocí hello ```Get-DPMRecoveryPoint``` rutiny.</span><span class="sxs-lookup"><span data-stu-id="3f65f-292">Fetching hello array of backup points using hello ```Get-DPMRecoveryPoint``` cmdlet.</span></span>
* <span data-ttu-id="3f65f-293">Výběr zálohování toorestore bodu z.</span><span class="sxs-lookup"><span data-stu-id="3f65f-293">Choosing a backup point toorestore from.</span></span>

```
PS C:\> $RecoveryOption = New-DPMRecoveryOption -HyperVDatasource -TargetServer "HVDCenter02" -RecoveryLocation AlternateHyperVServer -RecoveryType Recover -TargetLocation “C:\VMRecovery”

PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online

PS C:\> Restore-DPMRecoverableItem -RecoverableItem $RecoveryPoints[0] -RecoveryOption $RecoveryOption
```

<span data-ttu-id="3f65f-294">pro jakýkoli typ zdroje dat lze snadno rozšířit Hello příkazy.</span><span class="sxs-lookup"><span data-stu-id="3f65f-294">hello commands can easily be extended for any datasource type.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3f65f-295">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3f65f-295">Next steps</span></span>
* <span data-ttu-id="3f65f-296">Další informace o zálohování Azure pro aplikaci DPM najdete v části [Úvod tooDPM zálohování](backup-azure-dpm-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="3f65f-296">For more information about Azure Backup for DPM see [Introduction tooDPM Backup](backup-azure-dpm-introduction.md)</span></span>
