---
title: "Zálohování Azure: Zálohování úloh DPM pomocí Powershellu | Microsoft Docs"
description: "Zjistěte, jak nasadit a spravovat Azure Backup pro Data Protection Manager (DPM) pomocí prostředí PowerShell"
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
ms.openlocfilehash: 943a12dcba49a114d206b9dab968da332ea99926
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="deploy-and-manage-backup-to-azure-for-data-protection-manager-dpm-servers-using-powershell"></a><span data-ttu-id="8ab8a-103">Nasazení a správa zálohování do Azure pro servery DPM (Data Protection Manager) pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="8ab8a-103">Deploy and manage backup to Azure for Data Protection Manager (DPM) servers using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="8ab8a-104">ARM</span><span class="sxs-lookup"><span data-stu-id="8ab8a-104">ARM</span></span>](backup-dpm-automation.md)
> * [<span data-ttu-id="8ab8a-105">Classic</span><span class="sxs-lookup"><span data-stu-id="8ab8a-105">Classic</span></span>](backup-dpm-automation-classic.md)
>
>

<span data-ttu-id="8ab8a-106">Tento článek vysvětluje, jak pomocí prostředí PowerShell k zálohování a obnovení dat aplikace DPM z úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-106">This article explains how to use PowerShell to back up and recover DPM data from a backup vault.</span></span> <span data-ttu-id="8ab8a-107">Společnost Microsoft doporučuje používat trezory služeb zotavení pro všechna nová nasazení.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-107">Microsoft recommends using Recovery Services vaults for all new deployments.</span></span> <span data-ttu-id="8ab8a-108">Pokud jste nového uživatele Azure Backup, použijte v článku [nasadit a spravovat Data Protection Manager dat do Azure pomocí prostředí PowerShell](backup-dpm-automation.md), takže data ukládáte do trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-108">If you are a new Azure Backup user, use the article, [Deploy and manage Data Protection Manager data to Azure using PowerShell](backup-dpm-automation.md), so you store your data in a Recovery Services vault.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8ab8a-109">Nyní můžete trezory služby Backup upgradovat na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-109">You can now upgrade your Backup vaults to Recovery Services vaults.</span></span> <span data-ttu-id="8ab8a-110">Podrobnosti najdete v článku [Upgrade trezoru služby Backup na trezor služby Recovery Services](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="8ab8a-110">For details, see the article [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="8ab8a-111">Microsoft doporučuje, abyste upgradovali své trezory služby Backup na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-111">Microsoft encourages you to upgrade your Backup vaults to Recovery Services vaults.</span></span> <span data-ttu-id="8ab8a-112">Od 15. října 2017 nebude možné pomocí PowerShellu vytvářet trezory služby Backup.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-112">After October 15, 2017, you can’t use PowerShell to create Backup vaults.</span></span> <span data-ttu-id="8ab8a-113">**Do 1. listopadu 2017:**</span><span class="sxs-lookup"><span data-stu-id="8ab8a-113">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="8ab8a-114">Všechny zbývající trezory služby Backup budou automaticky upgradovány na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-114">All remaining Backup vaults will be automatically upgraded to Recovery Services vaults.</span></span>
>- <span data-ttu-id="8ab8a-115">Nebudete mít přístup k datům záloh na portálu Classic.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-115">You won't be able to access your backup data in the classic portal.</span></span> <span data-ttu-id="8ab8a-116">Pro přístup k datům záloh v trezorech služby Recovery Services místo toho použijte Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-116">Instead, use the Azure portal to access your backup data in Recovery Services vaults.</span></span>
>

## <a name="setting-up-the-powershell-environment"></a><span data-ttu-id="8ab8a-117">Nastavení prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="8ab8a-117">Setting up the PowerShell environment</span></span>
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

<span data-ttu-id="8ab8a-118">Před použitím prostředí PowerShell pro správu z aplikace Data Protection Manager zálohování do Azure, musíte mít správné prostředí v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-118">Before you can use PowerShell to manage backups from Data Protection Manager to Azure, you will need to have the right environment in PowerShell.</span></span> <span data-ttu-id="8ab8a-119">Na začátku relace prostředí PowerShell Ujistěte se, že spustíte následující příkaz a naimportovat moduly vpravo umožňují správně odkazovat rutiny aplikace DPM:</span><span class="sxs-lookup"><span data-stu-id="8ab8a-119">At the start of the PowerShell session, ensure that you run the following command to import the right modules and allow you to correctly reference the DPM cmdlets:</span></span>

```
PS C:> & "C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin\DpmCliInitScript.ps1"

Welcome to the DPM Management Shell!

Full list of cmdlets: Get-Command
Only DPM cmdlets: Get-DPMCommand
Get general help: help
Get help for a cmdlet: help <cmdlet-name> or <cmdlet-name> -?
Get definition of a cmdlet: Get-Command <cmdlet-name> -Syntax
Sample DPM scripts: Get-DPMSampleScript
```

## <a name="setup-and-registration"></a><span data-ttu-id="8ab8a-120">Instalace a registrace</span><span class="sxs-lookup"><span data-stu-id="8ab8a-120">Setup and Registration</span></span>
<span data-ttu-id="8ab8a-121">Chcete-li začít:</span><span class="sxs-lookup"><span data-stu-id="8ab8a-121">To begin:</span></span>

1. <span data-ttu-id="8ab8a-122">[Stáhněte si nejnovější PowerShell](https://github.com/Azure/azure-powershell/releases) (minimální požadovaná verze je: 1.0.0)</span><span class="sxs-lookup"><span data-stu-id="8ab8a-122">[Download latest PowerShell](https://github.com/Azure/azure-powershell/releases) (minimum version required is : 1.0.0)</span></span>
2. <span data-ttu-id="8ab8a-123">Povolte rutiny pro zálohování Azure tak, že přepnutí na *AzureResourceManager* režimu pomocí **Switch-AzureMode** příkaz:</span><span class="sxs-lookup"><span data-stu-id="8ab8a-123">Enable the Azure Backup commandlets by switching to *AzureResourceManager* mode by using the **Switch-AzureMode** commandlet:</span></span>

```
PS C:\> Switch-AzureMode AzureResourceManager
```

<span data-ttu-id="8ab8a-124">Následující instalaci a registraci úlohy je možné automatizovat pomocí prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="8ab8a-124">The following setup and registration tasks can be automated with PowerShell:</span></span>

* <span data-ttu-id="8ab8a-125">Vytvoření trezoru záloh</span><span class="sxs-lookup"><span data-stu-id="8ab8a-125">Create a backup vault</span></span>
* <span data-ttu-id="8ab8a-126">Instalace agenta Azure Backup</span><span class="sxs-lookup"><span data-stu-id="8ab8a-126">Installing the Azure Backup agent</span></span>
* <span data-ttu-id="8ab8a-127">Registrace u služby zálohování Azure</span><span class="sxs-lookup"><span data-stu-id="8ab8a-127">Registering with the Azure Backup service</span></span>
* <span data-ttu-id="8ab8a-128">Nastavení sítě</span><span class="sxs-lookup"><span data-stu-id="8ab8a-128">Networking settings</span></span>
* <span data-ttu-id="8ab8a-129">Nastavení šifrování</span><span class="sxs-lookup"><span data-stu-id="8ab8a-129">Encryption settings</span></span>

### <a name="create-a-backup-vault"></a><span data-ttu-id="8ab8a-130">Vytvoření trezoru záloh</span><span class="sxs-lookup"><span data-stu-id="8ab8a-130">Create a backup vault</span></span>
> [!WARNING]
> <span data-ttu-id="8ab8a-131">Pro zákazníky pomocí služby Azure Backup poprvé budete muset registraci poskytovatele Azure Backup pro použití s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-131">For customers using Azure Backup for the first time, you need to register the Azure Backup provider to be used with your subscription.</span></span> <span data-ttu-id="8ab8a-132">To můžete provést spuštěním následujícího příkazu: Register-AzureProvider - ProviderNamespace "Microsoft.Backup"</span><span class="sxs-lookup"><span data-stu-id="8ab8a-132">This can be done by running the following command: Register-AzureProvider -ProviderNamespace "Microsoft.Backup"</span></span>
>
>

<span data-ttu-id="8ab8a-133">Můžete vytvořit nové úložiště záloh pomocí **New-AzureRMBackupVault** příkaz.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-133">You can create a new backup vault using the **New-AzureRMBackupVault** commandlet.</span></span> <span data-ttu-id="8ab8a-134">Trezor záloh je prostředek ARM, proto musíte umístit do skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-134">The backup vault is an ARM resource, so you need to place it within a Resource Group.</span></span> <span data-ttu-id="8ab8a-135">V prostředí Azure PowerShell konzolu se zvýšenými oprávněními spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="8ab8a-135">In an elevated Azure PowerShell console, run the following commands:</span></span>

```
PS C:\> New-AzureResourceGroup –Name “test-rg” -Region “West US”
PS C:\> $backupvault = New-AzureRMBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GRS
```

<span data-ttu-id="8ab8a-136">Můžete získat seznam všech záloh v daném předplatném pomocí **Get-AzureRMBackupVault** příkaz.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-136">You can get a list of all the backup vaults in a given subscription using the **Get-AzureRMBackupVault** commandlet.</span></span>

### <a name="installing-the-azure-backup-agent-on-a-dpm-server"></a><span data-ttu-id="8ab8a-137">Instalace agenta Azure Backup na serveru aplikace DPM</span><span class="sxs-lookup"><span data-stu-id="8ab8a-137">Installing the Azure Backup agent on a DPM Server</span></span>
<span data-ttu-id="8ab8a-138">Před instalací agenta Azure Backup, musíte mít instalační program stažené a existuje v systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-138">Before you install the Azure Backup agent, you need to have the installer downloaded and present on the Windows Server.</span></span> <span data-ttu-id="8ab8a-139">Můžete získat nejnovější verzi instalačního programu z [Microsoft Download Center](http://aka.ms/azurebackup_agent) nebo ze stránky řídicího panelu trezoru záloh.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-139">You can get the latest version of the installer from the [Microsoft Download Center](http://aka.ms/azurebackup_agent) or from the backup vault's Dashboard page.</span></span> <span data-ttu-id="8ab8a-140">Instalační program uložte na snadno dostupném místě jako * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-140">Save the installer to an easily accessible location like *C:\Downloads\*.</span></span>

<span data-ttu-id="8ab8a-141">Pokud chcete nainstalovat agenta, spusťte následující příkaz v konzolu prostředí PowerShell se zvýšenými oprávněními **na serveru DPM**:</span><span class="sxs-lookup"><span data-stu-id="8ab8a-141">To install the agent, run the following command in an elevated PowerShell console **on the DPM server**:</span></span>

```
PS C:\> MARSAgentInstaller.exe /q
```

<span data-ttu-id="8ab8a-142">Tím se nainstaluje agent s výchozími možnostmi.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-142">This installs the agent with all the default options.</span></span> <span data-ttu-id="8ab8a-143">Instalace trvá několik minut na pozadí.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-143">The installation takes a few minutes in the background.</span></span> <span data-ttu-id="8ab8a-144">Pokud nezadáte */nu* možnost **Windows Update** otevře se okno na konci instalace k vyhledání všech aktualizací.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-144">If you do not specify the */nu* option the **Windows Update** window will open at the end of the installation to check for any updates.</span></span>

<span data-ttu-id="8ab8a-145">Agenta se zobrazí v seznamu nainstalovaných programů.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-145">The agent will show in the list of installed programs.</span></span> <span data-ttu-id="8ab8a-146">Chcete-li zobrazit seznam nainstalovaných programů, přejděte na **ovládací panely** > **programy** > **programy a funkce**.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-146">To see the list of installed programs, go to **Control Panel** > **Programs** > **Programs and Features**.</span></span>

![Instalaci agenta](./media/backup-dpm-automation/installed-agent-listing.png)

#### <a name="installation-options"></a><span data-ttu-id="8ab8a-148">Možnosti instalace</span><span class="sxs-lookup"><span data-stu-id="8ab8a-148">Installation options</span></span>
<span data-ttu-id="8ab8a-149">Pokud chcete zobrazit všechny možnosti, které jsou k dispozici prostřednictvím příkazového řádku, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="8ab8a-149">To see all the options available via the command-line, use the following command:</span></span>

```
PS C:\> MARSAgentInstaller.exe /?
```

<span data-ttu-id="8ab8a-150">Mezi dostupné možnosti patří:</span><span class="sxs-lookup"><span data-stu-id="8ab8a-150">The available options include:</span></span>

| <span data-ttu-id="8ab8a-151">Možnost</span><span class="sxs-lookup"><span data-stu-id="8ab8a-151">Option</span></span> | <span data-ttu-id="8ab8a-152">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="8ab8a-152">Details</span></span> | <span data-ttu-id="8ab8a-153">Výchozí</span><span class="sxs-lookup"><span data-stu-id="8ab8a-153">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8ab8a-154">/q</span><span class="sxs-lookup"><span data-stu-id="8ab8a-154">/q</span></span> |<span data-ttu-id="8ab8a-155">Tichá instalace</span><span class="sxs-lookup"><span data-stu-id="8ab8a-155">Quiet installation</span></span> |- |
| <span data-ttu-id="8ab8a-156">/ p: "umístění"</span><span class="sxs-lookup"><span data-stu-id="8ab8a-156">/p:"location"</span></span> |<span data-ttu-id="8ab8a-157">Cesta ke složce instalace pro agenta Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-157">Path to the installation folder for the Azure Backup agent.</span></span> |<span data-ttu-id="8ab8a-158">C:\Program Files\Microsoft Azure Recovery Services agenta</span><span class="sxs-lookup"><span data-stu-id="8ab8a-158">C:\Program Files\Microsoft Azure Recovery Services Agent</span></span> |
| <span data-ttu-id="8ab8a-159">/ s: "umístění"</span><span class="sxs-lookup"><span data-stu-id="8ab8a-159">/s:"location"</span></span> |<span data-ttu-id="8ab8a-160">Cesta ke složce mezipaměti pro agenta Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-160">Path to the cache folder for the Azure Backup agent.</span></span> |<span data-ttu-id="8ab8a-161">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span><span class="sxs-lookup"><span data-stu-id="8ab8a-161">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span></span> |
| <span data-ttu-id="8ab8a-162">/m</span><span class="sxs-lookup"><span data-stu-id="8ab8a-162">/m</span></span> |<span data-ttu-id="8ab8a-163">Výslovný souhlas ke službě Microsoft Update</span><span class="sxs-lookup"><span data-stu-id="8ab8a-163">Opt-in to Microsoft Update</span></span> |- |
| <span data-ttu-id="8ab8a-164">/nu</span><span class="sxs-lookup"><span data-stu-id="8ab8a-164">/nu</span></span> |<span data-ttu-id="8ab8a-165">Nekontrolovat aktualizace po dokončení instalace</span><span class="sxs-lookup"><span data-stu-id="8ab8a-165">Do not Check for updates after installation is complete</span></span> |- |
| <span data-ttu-id="8ab8a-166">/d</span><span class="sxs-lookup"><span data-stu-id="8ab8a-166">/d</span></span> |<span data-ttu-id="8ab8a-167">Odinstaluje Agenta Microsoft Azure Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-167">Uninstalls Microsoft Azure Recovery Services Agent</span></span> |- |
| <span data-ttu-id="8ab8a-168">/pH</span><span class="sxs-lookup"><span data-stu-id="8ab8a-168">/ph</span></span> |<span data-ttu-id="8ab8a-169">Adresa proxy hostitele</span><span class="sxs-lookup"><span data-stu-id="8ab8a-169">Proxy Host Address</span></span> |- |
| <span data-ttu-id="8ab8a-170">/Po</span><span class="sxs-lookup"><span data-stu-id="8ab8a-170">/po</span></span> |<span data-ttu-id="8ab8a-171">Číslo portu proxy hostitele</span><span class="sxs-lookup"><span data-stu-id="8ab8a-171">Proxy Host Port Number</span></span> |- |
| <span data-ttu-id="8ab8a-172">/Pu</span><span class="sxs-lookup"><span data-stu-id="8ab8a-172">/pu</span></span> |<span data-ttu-id="8ab8a-173">Uživatelské jméno proxy hostitele</span><span class="sxs-lookup"><span data-stu-id="8ab8a-173">Proxy Host UserName</span></span> |- |
| <span data-ttu-id="8ab8a-174">/pW</span><span class="sxs-lookup"><span data-stu-id="8ab8a-174">/pw</span></span> |<span data-ttu-id="8ab8a-175">Heslo pro proxy server</span><span class="sxs-lookup"><span data-stu-id="8ab8a-175">Proxy Password</span></span> |- |

### <a name="registering-with-the-azure-backup-service"></a><span data-ttu-id="8ab8a-176">Registrace u služby zálohování Azure</span><span class="sxs-lookup"><span data-stu-id="8ab8a-176">Registering with the Azure Backup service</span></span>
<span data-ttu-id="8ab8a-177">Než můžete zaregistrovat pomocí služby Azure Backup, musíte zajistit, aby [požadavky](backup-azure-dpm-introduction.md) jsou splněny.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-177">Before you can register with the Azure Backup service, you need to ensure that the [prerequisites](backup-azure-dpm-introduction.md) are met.</span></span> <span data-ttu-id="8ab8a-178">Postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="8ab8a-178">You must:</span></span>

* <span data-ttu-id="8ab8a-179">Máte platné předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="8ab8a-179">Have a valid Azure subscription</span></span>
* <span data-ttu-id="8ab8a-180">Mít úložiště záloh</span><span class="sxs-lookup"><span data-stu-id="8ab8a-180">Have a backup vault</span></span>

<span data-ttu-id="8ab8a-181">Chcete-li stáhnout přihlašovací údaje trezoru, spusťte **Get-AzureBackupVaultCredentials** příkaz do konzoly Azure PowerShell a úložiště do vhodného umístění, například * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-181">To download the vault credentials, run the **Get-AzureBackupVaultCredentials** commandlet in an Azure PowerShell console and store it in a convenient location like *C:\Downloads\*.</span></span>

```
PS C:\> $credspath = "C:\"
PS C:\> $credsfilename = Get-AzureRMBackupVaultCredentials -Vault $backupvault -TargetLocation $credspath
PS C:\> $credsfilename
f5303a0b-fae4-4cdb-b44d-0e4c032dde26_backuprg_backuprn_2015-08-11--06-22-35.VaultCredentials
```

<span data-ttu-id="8ab8a-182">Počítač zaregistrovat v úložišti se provádí pomocí [Start-DPMCloudRegistration](https://technet.microsoft.com/library/jj612787) rutiny:</span><span class="sxs-lookup"><span data-stu-id="8ab8a-182">Registering the machine with the vault is done using the [Start-DPMCloudRegistration](https://technet.microsoft.com/library/jj612787) cmdlet:</span></span>

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-DPMCloudRegistration -DPMServerName "TestingServer" -VaultCredentialsFilePath $cred
```

<span data-ttu-id="8ab8a-183">To bude registrace serveru DPM s názvem "Testovacím" s trezorem Microsoft Azure pomocí přihlašovacích údajů, zadaný trezor.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-183">This will register the DPM Server named “TestingServer” with Microsoft Azure Vault using the specified vault credentials.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8ab8a-184">Nepoužívejte relativní cesty k určení souboru s přihlašovacími údaji.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-184">Do not use relative paths to specify the vault credentials file.</span></span> <span data-ttu-id="8ab8a-185">Jako vstup do rutiny, musí zadat absolutní cestu.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-185">You must provide an absolute path as an input to the cmdlet.</span></span>
>
>

### <a name="initial-configuration-settings"></a><span data-ttu-id="8ab8a-186">Počáteční konfigurace nastavení</span><span class="sxs-lookup"><span data-stu-id="8ab8a-186">Initial configuration settings</span></span>
<span data-ttu-id="8ab8a-187">Jakmile je DPM Server registrován s úložištěm záloh Azure, se spustí s výchozím nastavením odběru.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-187">Once the DPM Server is registered with the Azure Backup vault, it will start with default subscription settings.</span></span> <span data-ttu-id="8ab8a-188">Tato nastavení odběru zahrnují sítě, šifrování a pracovní oblasti.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-188">These subscription settings include Networking, Encryption and the Staging area.</span></span> <span data-ttu-id="8ab8a-189">Chcete-li začít, změna nastavení přihlášení k odběru je nutné nejprve získat popisovač na existující nastavení (výchozí), pomocí [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) rutiny:</span><span class="sxs-lookup"><span data-stu-id="8ab8a-189">To begin changing the subscription settings you need to first get a handle on the existing (default) settings using the [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) cmdlet:</span></span>

```
$setting = Get-DPMCloudSubscriptionSetting -DPMServerName "TestingServer"
```

<span data-ttu-id="8ab8a-190">Všechny změny provedené tento objekt místní prostředí PowerShell ```$setting``` a pak se zaměřuje na aplikace DPM a Azure Backup, o jejich uložení úplné objekt pomocí [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) rutiny.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-190">All modifications are made to this local PowerShell object ```$setting```  and then the full object is committed to DPM and Azure Backup to save them using the [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span></span> <span data-ttu-id="8ab8a-191">Budete muset použít ```–Commit``` příznak zajistit, že jsou nastavené jako trvalé změny.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-191">You need to use the ```–Commit``` flag to ensure that the changes are persisted.</span></span> <span data-ttu-id="8ab8a-192">Nastavení nebude použita a pokud potvrzené používá Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-192">The settings will not be applied and used by Azure Backup unless committed.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

### <a name="networking"></a><span data-ttu-id="8ab8a-193">Sítě</span><span class="sxs-lookup"><span data-stu-id="8ab8a-193">Networking</span></span>
<span data-ttu-id="8ab8a-194">Pokud připojení počítače aplikace DPM do služby Azure Backup na Internetu prostřednictvím proxy serveru, by měl nastavení serveru proxy zadaný pro zálohování úspěšné.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-194">If the connectivity of the DPM machine to the Azure Backup service on the internet is through a proxy server, then the proxy server settings should be provided for backups to succeed.</span></span> <span data-ttu-id="8ab8a-195">To se provádí pomocí ```-ProxyServer```, ```-ProxyPort```, ```-ProxyUsername``` a ```ProxyPassword``` parametry se [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) rutiny.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-195">This is done by using the ```-ProxyServer```, ```-ProxyPort```, ```-ProxyUsername``` and the ```ProxyPassword``` parameters with the [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span></span> <span data-ttu-id="8ab8a-196">V tomto příkladu není žádný proxy server, jsme jsou explicitně vymazání údaje související s proxy serverem.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-196">In this example, there is no proxy server so we are explicitly clearing any proxy-related information.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoProxy
```

<span data-ttu-id="8ab8a-197">Využití šířky pásma se dá taky nastavit pomocí možnosti ```-WorkHourBandwidth``` a ```-NonWorkHourBandwidth``` pro danou sadu dny v týdnu.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-197">Bandwidth usage can also be controlled with options of ```-WorkHourBandwidth``` and ```-NonWorkHourBandwidth``` for a given set of days of the week.</span></span> <span data-ttu-id="8ab8a-198">V tomto příkladu jsme nejsou nastavení žádné omezení.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-198">In this example we are not setting any throttling.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoThrottle
```

### <a name="configuring-the-staging-area"></a><span data-ttu-id="8ab8a-199">Konfigurace pracovní oblasti</span><span class="sxs-lookup"><span data-stu-id="8ab8a-199">Configuring the staging Area</span></span>
<span data-ttu-id="8ab8a-200">Agent Azure Backup spuštěný na serveru DPM potřebuje dočasné úložiště pro data obnovená z cloudu (místní pracovní oblasti).</span><span class="sxs-lookup"><span data-stu-id="8ab8a-200">The Azure Backup agent running on the DPM server needs temporary storage for data restored from the cloud (local staging area).</span></span> <span data-ttu-id="8ab8a-201">Nakonfigurovat pracovní oblasti pomocí [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) rutiny a ```-StagingAreaPath``` parametr.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-201">Configure the staging area using the [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet and the ```-StagingAreaPath``` parameter.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -StagingAreaPath "C:\StagingArea"
```

<span data-ttu-id="8ab8a-202">V příkladu nahoře, je možnost pracovní oblasti *C:\StagingArea* v prostředí PowerShell objektu ```$setting```.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-202">In the example above, the staging area will be set to *C:\StagingArea* in the PowerShell object ```$setting```.</span></span> <span data-ttu-id="8ab8a-203">Ujistěte se, že v zadané složce již existuje, jinak se nezdaří poslední zápis nastavení přihlášení k odběru.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-203">Ensure that the specified folder already exists, or else the final commit of the subscription settings will fail.</span></span>

### <a name="encryption-settings"></a><span data-ttu-id="8ab8a-204">Nastavení šifrování</span><span class="sxs-lookup"><span data-stu-id="8ab8a-204">Encryption settings</span></span>
<span data-ttu-id="8ab8a-205">Zálohování data přenášená do služby Azure Backup je zašifrovaná chránit jejich důvěrnost dat.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-205">The backup data sent to Azure Backup is encrypted to protect the confidentiality of the data.</span></span> <span data-ttu-id="8ab8a-206">Šifrovací přístupové heslo je 'heslo' k dešifrování dat v době obnovení.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-206">The encryption passphrase is the "password" to decrypt the data at the time of restore.</span></span> <span data-ttu-id="8ab8a-207">Je důležité udržovat tento informace o bezpečném po nastavení.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-207">It is important to keep this information safe and secure once it is set.</span></span>

<span data-ttu-id="8ab8a-208">V následujícím příkladu převede první příkaz řetězec ```passphrase123456789``` zabezpečený řetězec a přiřadí zabezpečený řetězec proměnné s názvem ```$Passphrase```.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-208">In the example below, the first command converts the string ```passphrase123456789``` to a secure string and assigns the secure string to the variable named ```$Passphrase```.</span></span> <span data-ttu-id="8ab8a-209">v druhém příkazu nastaví zabezpečený řetězec v ```$Passphrase``` jako heslo pro šifrování záloh.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-209">the second command sets the secure string in ```$Passphrase``` as the password for encrypting backups.</span></span>

```
PS C:\> $Passphrase = ConvertTo-SecureString -string "passphrase123456789" -AsPlainText -Force

PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -EncryptionPassphrase $Passphrase
```

> [!IMPORTANT]
> <span data-ttu-id="8ab8a-210">Zachování informací o přístupové heslo bezpečném, po nastavení.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-210">Keep the passphrase information safe and secure once it is set.</span></span> <span data-ttu-id="8ab8a-211">Nebudete schopni obnovit data ze služby Azure bez tohoto hesla.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-211">You will not be able to restore data from Azure without this passphrase.</span></span>
>
>

<span data-ttu-id="8ab8a-212">V tomto okamžiku jste měli udělali všechny požadované změny pro ```$setting``` objektu.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-212">At this point, you should have made all the required changes to the ```$setting``` object.</span></span> <span data-ttu-id="8ab8a-213">Mějte na paměti k provedení změn.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-213">Remember to commit the changes.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="protect-data-to-azure-backup"></a><span data-ttu-id="8ab8a-214">Ochrana dat do služby Azure Backup</span><span class="sxs-lookup"><span data-stu-id="8ab8a-214">Protect data to Azure Backup</span></span>
<span data-ttu-id="8ab8a-215">V této části přidáte provozním serveru DPM a poté proveďte ochranu dat do místního úložiště DPM a pak do služby Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-215">In this section, you will add a production server to DPM and then protect the data to local DPM storage and then to Azure Backup.</span></span> <span data-ttu-id="8ab8a-216">V příkladech jsme se ukazují, jak zálohovat soubory a složky.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-216">In the examples we will demonstrate how to back up files and folders.</span></span> <span data-ttu-id="8ab8a-217">Chcete-li zálohovat všechny zdroje dat aplikace DPM podporováno lze snadno rozšířit logiku.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-217">The logic can easily be extended to backup any DPM-supported data source.</span></span> <span data-ttu-id="8ab8a-218">Všechny zálohy aplikace DPM se řídí pomocí ochranu skupiny (PG) s čtyři části:</span><span class="sxs-lookup"><span data-stu-id="8ab8a-218">All your DPM backups are governed by a Protection Group (PG) with four parts:</span></span>

1. <span data-ttu-id="8ab8a-219">**Členy skupiny** je seznam chránitelné objekty (také označované jako *zdrojů dat* v DPM), které chcete chránit ve stejné skupině ochrany.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-219">**Group members** is a list of all the protectable objects (also known as *Datasources* in DPM) that you want to protect in the same protection group.</span></span> <span data-ttu-id="8ab8a-220">Chcete třeba chránit produkční virtuálních počítačů na jednu skupinu ochrany a databáze systému SQL Server v jiné skupiny ochrany, protože mají různé požadavky na zálohování.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-220">For example, you may want to protect production VMs in one protection group and SQL Server databases in another protection group as they may have different backup requirements.</span></span> <span data-ttu-id="8ab8a-221">Předtím, než můžete zálohovat žádný zdroj dat na provozním serveru, které potřebujete, abyste měli jistotu, agenta aplikace DPM je nainstalován na serveru a je spravovaných aplikací DPM.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-221">Before you can back up any datasource on a production server you need to make sure the DPM Agent is installed on the server and is managed by DPM.</span></span> <span data-ttu-id="8ab8a-222">Postupujte podle kroků pro [instalace agenta DPM](https://technet.microsoft.com/library/bb870935.aspx) a propojení na příslušný Server aplikace DPM.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-222">Follow the steps for [installing the DPM Agent](https://technet.microsoft.com/library/bb870935.aspx) and linking it to the appropriate DPM Server.</span></span>
2. <span data-ttu-id="8ab8a-223">**Způsob ochrany dat** Určuje umístění zálohování cíl - páska, disk a cloud.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-223">**Data protection method** specifies the target backup locations - tape, disk, and cloud.</span></span> <span data-ttu-id="8ab8a-224">V našem příkladu jsme bude chránit data na místní disk a do cloudu.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-224">In our example we will protect data to the local disk and to the cloud.</span></span>
3. <span data-ttu-id="8ab8a-225">A **plán zálohování** určující, kdy je třeba přijmout zálohy a jak často se mají synchronizovat data mezi serverem DPM a na provozním serveru.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-225">A **backup schedule** that specifies when backups need to be taken and how often the data should be synchronized between the DPM Server and the production server.</span></span>
4. <span data-ttu-id="8ab8a-226">A **plán uchovávání informací** který určuje, jak dlouho chcete zachovat body obnovení v Azure.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-226">A **retention schedule** that specifies how long to retain the recovery points in Azure.</span></span>

### <a name="creating-a-protection-group"></a><span data-ttu-id="8ab8a-227">Vytvoření skupiny ochrany</span><span class="sxs-lookup"><span data-stu-id="8ab8a-227">Creating a protection group</span></span>
<span data-ttu-id="8ab8a-228">Začněte vytvořením nové skupiny ochrany pomocí [New-DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) rutiny.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-228">Start by creating a new Protection Group using the [New-DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) cmdlet.</span></span>

```
PS C:\> $PG = New-DPMProtectionGroup -DPMServerName " TestingServer " -Name "ProtectGroup01"
```

<span data-ttu-id="8ab8a-229">Výše uvedené rutina vytvoří skupiny ochrany s názvem *ProtectGroup01*.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-229">The above cmdlet will create a Protection Group named *ProtectGroup01*.</span></span> <span data-ttu-id="8ab8a-230">Existující skupiny ochrany můžete také upravit později přidat zálohování do cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-230">An existing protection group can also be modified later to add backup to the Azure cloud.</span></span> <span data-ttu-id="8ab8a-231">Ale žádné změny do skupiny ochrany – nové nebo existující - musíme získat popisovač *upravitelnými* pomocí [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) rutiny.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-231">However, to make any changes to the Protection Group - new or existing - we need to get a handle on a *modifiable* object using the [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) cmdlet.</span></span>

```
PS C:\> $MPG = Get-ModifiableProtectionGroup $PG
```

### <a name="adding-group-members-to-the-protection-group"></a><span data-ttu-id="8ab8a-232">Přidání členů skupiny do skupiny ochrany</span><span class="sxs-lookup"><span data-stu-id="8ab8a-232">Adding group members to the Protection Group</span></span>
<span data-ttu-id="8ab8a-233">Každý Agent aplikace DPM zná seznam zdrojů dat na serveru, který je nainstalovaný.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-233">Each DPM Agent knows the list of datasources on the server that it is installed on.</span></span> <span data-ttu-id="8ab8a-234">Přidat zdroje dat do skupiny ochrany, musí nejprve poslat seznam zdrojů dat zpět na DPM server agenta aplikace DPM.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-234">To add a datasource to the Protection Group, the DPM Agent needs to first send a list of the datasources back to the DPM server.</span></span> <span data-ttu-id="8ab8a-235">Jeden nebo více zdrojů dat se pak vybrali a přidají se do skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-235">One or more datasources are then selected and added to the Protection Group.</span></span> <span data-ttu-id="8ab8a-236">Prostředí PowerShell kroky potřebné k získání dosáhnout tyto:</span><span class="sxs-lookup"><span data-stu-id="8ab8a-236">The PowerShell steps needed to get achieve this are:</span></span>

1. <span data-ttu-id="8ab8a-237">Získat seznam všech serverů spravovaných aplikací DPM pomocí agenta aplikace DPM.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-237">Fetch a list of all servers managed by DPM through the DPM Agent.</span></span>
2. <span data-ttu-id="8ab8a-238">Vyberte konkrétní server.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-238">Choose a specific server.</span></span>
3. <span data-ttu-id="8ab8a-239">Získat seznam všech zdrojů dat na serveru.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-239">Fetch a list of all datasources on the server.</span></span>
4. <span data-ttu-id="8ab8a-240">Vyberte jeden nebo více zdrojů dat a přidat je do skupiny ochrany</span><span class="sxs-lookup"><span data-stu-id="8ab8a-240">Choose one or more datasources and add them to the Protection Group</span></span>

<span data-ttu-id="8ab8a-241">Seznam serverů, ve kterých je nainstalován Agent aplikace DPM a je spravován serverem aplikace DPM je získán pomocí [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) rutiny.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-241">The list of servers on which the DPM Agent is installed and is being managed by the DPM Server is acquired with the [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) cmdlet.</span></span> <span data-ttu-id="8ab8a-242">V tomto příkladu jsme bude filtrovat a konfigurovat jenom PS s názvem *productionserver01* pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-242">In this example we will filter and only configure PS with name *productionserver01* for backup.</span></span>

```
PS C:\> $server = Get-ProductionServer -DPMServerName "TestingServer" | where {($_.servername) –contains “productionserver01”
```

<span data-ttu-id="8ab8a-243">Nyní se načíst seznam zdrojů dat v ```$server``` pomocí [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) rutiny.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-243">Now fetch the list of datasources on ```$server``` using the [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) cmdlet.</span></span> <span data-ttu-id="8ab8a-244">V tomto příkladu jsme pro svazek filtrování * D:\* který chcete konfigurovat pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-244">In this example we are filtering for the volume *D:\* which we want to configure for backup.</span></span> <span data-ttu-id="8ab8a-245">Tento zdroj dat se pak přidá do skupiny ochrany pomocí [přidat DPMChildDatasource](https://technet.microsoft.com/library/hh881732) rutiny.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-245">This datasource is then added to the Protection Group using the [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732) cmdlet.</span></span> <span data-ttu-id="8ab8a-246">Nezapomeňte použít *modifable* objektu skupiny ochrany ```$MPG``` k budou přidány.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-246">Remember to use the *modifable* protection group object ```$MPG``` to make the additions.</span></span>

```
PS C:\> $DS = Get-Datasource -ProductionServer $server -Inquire | where { $_.Name -contains “D:\” }

PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS
```

<span data-ttu-id="8ab8a-247">Opakujte tento krok tolikrát, kolikrát podle potřeby, dokud nepřidáte všechny zvolené zdroje dat do skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-247">Repeat this step as many times as required, until you have added all the chosen datasources to the protection group.</span></span> <span data-ttu-id="8ab8a-248">Můžete začít s jenom jeden zdroj dat a dokončení pracovního postupu pro vytvoření skupiny ochrany a později přidat další datové zdroje do skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-248">You can also start with just one datasource, and complete the workflow for creating the Protection Group, and at a later point add more datasources to the Protection Group.</span></span>

### <a name="selecting-the-data-protection-method"></a><span data-ttu-id="8ab8a-249">Výběr způsobu ochrany dat</span><span class="sxs-lookup"><span data-stu-id="8ab8a-249">Selecting the data protection method</span></span>
<span data-ttu-id="8ab8a-250">Jakmile zdroje dat byly přidány do skupiny ochrany, dalším krokem je zadat, ochranu pomocí metody [Set-DPMProtectionType](https://technet.microsoft.com/library/hh881725) rutiny.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-250">Once the datasources have been added to the Protection Group, the next step is to specify the protection method using the [Set-DPMProtectionType](https://technet.microsoft.com/library/hh881725) cmdlet.</span></span> <span data-ttu-id="8ab8a-251">V tomto příkladu skupiny ochrany bude instalační program pro místního disku a zálohování.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-251">In this example, the Protection Group will be setup for local disk and cloud backup.</span></span> <span data-ttu-id="8ab8a-252">Budete taky muset zadat zdroj dat, který chcete chránit, do cloudu pomocí [přidat DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) rutiny příznakem - Online.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-252">You also need to specify the datasource that you want to protect to cloud using the [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) cmdlet with -Online flag.</span></span>

```
PS C:\> Set-DPMProtectionType -ProtectionGroup $MPG -ShortTerm Disk –LongTerm Online
PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS –Online
```

### <a name="setting-the-retention-range"></a><span data-ttu-id="8ab8a-253">Nastavení uchování</span><span class="sxs-lookup"><span data-stu-id="8ab8a-253">Setting the retention range</span></span>
<span data-ttu-id="8ab8a-254">Nastavení uchovávání bodů zálohování pomocí [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) rutiny.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-254">Set the retention for the backup points using the [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span></span> <span data-ttu-id="8ab8a-255">Když může být lichý nastavení uchovávání předtím, než byla definována plán zálohování, pomocí ```Set-DPMPolicyObjective``` rutiny automaticky nastaví výchozí plán zálohování, který je pak možné upravit.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-255">While it might seem odd to set the retention before the backup schedule has been defined, using the ```Set-DPMPolicyObjective``` cmdlet automatically sets a default backup schedule that can then be modified.</span></span> <span data-ttu-id="8ab8a-256">Vždy je možné sady nejdřív naplánovat zálohování a po zásady uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-256">It is always possible to set the backup schedule first and the retention policy after.</span></span>

<span data-ttu-id="8ab8a-257">V následujícím příkladu rutiny nastavuje parametry, uchování pro zálohy disku.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-257">In the example below, the cmdlet sets the retention parameters for disk backups.</span></span> <span data-ttu-id="8ab8a-258">To bude uchování záloh pro 10 dnů a synchronizaci dat každých 6 hodin mezi provozním serveru a serverem DPM.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-258">This will retain backups for 10 days, and sync data every 6 hours between the production server and the DPM server.</span></span> <span data-ttu-id="8ab8a-259">```SynchronizationFrequencyMinutes``` Nedefinuje četnost zálohování je bod vytvořen, ale jak často data se zkopírují na serveru aplikace DPM; záloh zabrání příliš velká.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-259">The ```SynchronizationFrequencyMinutes``` doesn't define how often a backup point is created, but how often data is copied to the DPM server; this prevents backups from becoming too large.</span></span>

```
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -RetentionRangeInDays 10 -SynchronizationFrequencyMinutes 360
```

<span data-ttu-id="8ab8a-260">Pro zálohování přejdete do Azure (aplikace DPM se týká jako zálohování Online) rozsahy uchovávání mohou být konfigurovány pro [dlouhodobé uchovávání používá schéma Dědečka. otec SYN (GFS)](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="8ab8a-260">For backups going to Azure (DPM refers to these as Online backups) the retention ranges can be configured for [long term retention using a Grandfather-Father-Son scheme (GFS)](backup-azure-backup-cloud-as-tape.md).</span></span> <span data-ttu-id="8ab8a-261">To znamená můžete definovat zásady uchovávání informací kombinované zahrnující denní, týdenní, měsíční a roční zásady uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-261">That is, you can define a combined retention policy involving daily, weekly, monthly and yearly retention policies.</span></span> <span data-ttu-id="8ab8a-262">V tomto příkladu vytvoření představující schéma komplexní uchovávání, který má být pole a pak nakonfigurujte pomocí rozsah uchování [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) rutiny.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-262">In this example, we create an array representing the complex retention scheme that we want, and then configure the retention range using the [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span></span>

```
PS C:\> $RRlist = @()
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 180, Days)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 104, Weeks)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 60, Month)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 10, Years)
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -OnlineRetentionRangeList $RRlist
```

### <a name="set-the-backup-schedule"></a><span data-ttu-id="8ab8a-263">Nastavte plán zálohování</span><span class="sxs-lookup"><span data-stu-id="8ab8a-263">Set the backup schedule</span></span>
<span data-ttu-id="8ab8a-264">Aplikace DPM nastaví výchozí plán zálohování automaticky, pokud zadáte cíle ochrany pomocí ```Set-DPMPolicyObjective``` rutiny.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-264">DPM sets a default backup schedule automatically if you specify the protection objective using the ```Set-DPMPolicyObjective``` cmdlet.</span></span> <span data-ttu-id="8ab8a-265">Ke změně výchozích plánů, použijte [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) následovanou [Set-DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) rutiny.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-265">To change the default schedules, use the [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) cmdlet followed by the [Set-DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) cmdlet.</span></span>

```
PS C:\> $onlineSch = Get-DPMPolicySchedule -ProtectionGroup $mpg -LongTerm Online
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[0] -TimesOfDay 02:00
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[1] -TimesOfDay 02:00 -DaysOfWeek Sa,Su –Interval 1
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[2] -TimesOfDay 02:00 -RelativeIntervals First,Third –DaysOfWeek Sa
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[3] -TimesOfDay 02:00 -DaysOfMonth 2,5,8,9 -Months Jan,Jul
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```

<span data-ttu-id="8ab8a-266">V příkladu nahoře ```$onlineSch``` je pole se čtyřmi prvky, které obsahuje existující plán ochranu online pro skupinu ochrany ve schématu GFS:</span><span class="sxs-lookup"><span data-stu-id="8ab8a-266">In the example above, ```$onlineSch``` is an array with four elements that contains the existing online protection schedule for the Protection Group in the GFS scheme:</span></span>

1. <span data-ttu-id="8ab8a-267">```$onlineSch[0]```bude obsahovat denní plán</span><span class="sxs-lookup"><span data-stu-id="8ab8a-267">```$onlineSch[0]``` will contain the daily schedule</span></span>
2. <span data-ttu-id="8ab8a-268">```$onlineSch[1]```bude obsahovat týdenní plán</span><span class="sxs-lookup"><span data-stu-id="8ab8a-268">```$onlineSch[1]``` will contain the weekly schedule</span></span>
3. <span data-ttu-id="8ab8a-269">```$onlineSch[2]```bude obsahovat plánování měsíčně</span><span class="sxs-lookup"><span data-stu-id="8ab8a-269">```$onlineSch[2]``` will contain the monthly schedule</span></span>
4. <span data-ttu-id="8ab8a-270">```$onlineSch[3]```bude obsahovat roční plán</span><span class="sxs-lookup"><span data-stu-id="8ab8a-270">```$onlineSch[3]``` will contain the yearly schedule</span></span>

<span data-ttu-id="8ab8a-271">Takže pokud budete muset upravit týdenní plán, budete muset naleznete ```$onlineSch[1]```.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-271">So if you need to modify the weekly schedule, you need to refer to the ```$onlineSch[1]```.</span></span>

### <a name="initial-backup"></a><span data-ttu-id="8ab8a-272">Prvotní zálohování</span><span class="sxs-lookup"><span data-stu-id="8ab8a-272">Initial backup</span></span>
<span data-ttu-id="8ab8a-273">Při první zálohování zdroje dat, aplikace DPM vyžaduje pro vytvoření počáteční repliky, který se vytvoří kopie zdroje dat, měly by být na svazku repliky DPM.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-273">When backing up a datasource for the first time, DPM needs to create an initial replica which will create a copy of the datasource to be protected on DPM replica volume.</span></span> <span data-ttu-id="8ab8a-274">Tato aktivita bude naplánována buď pro určitý čas nebo lze spustit ručně, pomocí [Set-DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) rutiny s parametrem ```-NOW```.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-274">This activity can either be scheduled for a specific time, or can be triggered manually, using the [Set-DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) cmdlet with the parameter ```-NOW```.</span></span>

```
PS C:\> Set-DPMReplicaCreationMethod -ProtectionGroup $MPG -NOW
```
### <a name="changing-the-size-of-dpm-replica--recovery-point-volume"></a><span data-ttu-id="8ab8a-275">Změna velikosti repliky aplikace DPM a svazek bodu obnovení</span><span class="sxs-lookup"><span data-stu-id="8ab8a-275">Changing the size of DPM Replica & recovery point volume</span></span>
<span data-ttu-id="8ab8a-276">Můžete také změnit velikost svazku repliky DPM, jakož i stínové kopie svazku pomocí [Set-DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) rutiny jako v následujícím příkladu: Get-DatasourceDiskAllocation - Datasource $DS Set-DatasourceDiskAllocation -Datasource $DS - ProtectionGroup $MPG – ruční - ReplicaArea (2 gb) - ShadowCopyArea (2 gb)</span><span class="sxs-lookup"><span data-stu-id="8ab8a-276">You can also change the size of DPM Replica volume as well as Shadow Copy volume using [Set-DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) cmdlet as in the below example: Get-DatasourceDiskAllocation -Datasource $DS Set-DatasourceDiskAllocation -Datasource $DS -ProtectionGroup $MPG -manual -ReplicaArea (2gb) -ShadowCopyArea (2gb)</span></span>

### <a name="committing-the-changes-to-the-protection-group"></a><span data-ttu-id="8ab8a-277">Potvrzení změny do skupiny ochrany</span><span class="sxs-lookup"><span data-stu-id="8ab8a-277">Committing the changes to the Protection Group</span></span>
<span data-ttu-id="8ab8a-278">Nakonec změny je nutné před provedením aplikace DPM může provést zálohování na novou konfiguraci skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-278">Finally, the changes need to be committed before DPM can take the backup per the new Protection Group configuration.</span></span> <span data-ttu-id="8ab8a-279">To se provádí pomocí [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) rutiny.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-279">This is done using the [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) cmdlet.</span></span>

```
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```
## <a name="view-the-backup-points"></a><span data-ttu-id="8ab8a-280">Zobrazení body zálohy</span><span class="sxs-lookup"><span data-stu-id="8ab8a-280">View the backup points</span></span>
<span data-ttu-id="8ab8a-281">Můžete použít [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) rutiny se získat seznam všech bodů obnovení pro zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-281">You can use the [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) cmdlet to get a list of all recovery points for a datasource.</span></span> <span data-ttu-id="8ab8a-282">V tomto příkladu provedeme následující:</span><span class="sxs-lookup"><span data-stu-id="8ab8a-282">In this example, we will:</span></span>

* <span data-ttu-id="8ab8a-283">načtení všech PGs na serveru DPM, který se uloží v matici```$PG```</span><span class="sxs-lookup"><span data-stu-id="8ab8a-283">fetch all the PGs on the DPM server which will be stored in an array ```$PG```</span></span>
* <span data-ttu-id="8ab8a-284">získání zdroje dat odpovídající```$PG[0]```</span><span class="sxs-lookup"><span data-stu-id="8ab8a-284">get the datasources corresponding to the ```$PG[0]```</span></span>
* <span data-ttu-id="8ab8a-285">získání všech bodů obnovení pro zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-285">get all the recovery points for a datasource.</span></span>

```
PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online
```

## <a name="restore-data-protected-on-azure"></a><span data-ttu-id="8ab8a-286">Obnovit data chráněná v Azure</span><span class="sxs-lookup"><span data-stu-id="8ab8a-286">Restore data protected on Azure</span></span>
<span data-ttu-id="8ab8a-287">Obnovení dat je kombinací ```RecoverableItem``` objektu a ```RecoveryOption``` objektu.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-287">Restoring data is a combination of a ```RecoverableItem``` object and a ```RecoveryOption``` object.</span></span> <span data-ttu-id="8ab8a-288">V předchozí části My seznam body zálohy pro zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-288">In the previous section, we got a list of the backup points for a datasource.</span></span>

<span data-ttu-id="8ab8a-289">V následujícím příkladu jsme ukazují, jak k obnovení virtuálního počítače technologie Hyper-V z Azure Backup kombinací body zálohy s cíle pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-289">In the example below, we demonstrate how to restore a Hyper-V virtual machine from Azure Backup by combining backup points with the target for recovery.</span></span> <span data-ttu-id="8ab8a-290">To zahrnuje následující:</span><span class="sxs-lookup"><span data-stu-id="8ab8a-290">This includes:</span></span>

* <span data-ttu-id="8ab8a-291">Vytváření možnost obnovení pomocí [New-DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) rutiny.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-291">Creating a recovery option using the  [New-DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) cmdlet.</span></span>
* <span data-ttu-id="8ab8a-292">Načítání pole body zálohy pomocí ```Get-DPMRecoveryPoint``` rutiny.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-292">Fetching the array of backup points using the ```Get-DPMRecoveryPoint``` cmdlet.</span></span>
* <span data-ttu-id="8ab8a-293">Výběr bod obnovení ze zálohy.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-293">Choosing a backup point to restore from.</span></span>

```
PS C:\> $RecoveryOption = New-DPMRecoveryOption -HyperVDatasource -TargetServer "HVDCenter02" -RecoveryLocation AlternateHyperVServer -RecoveryType Recover -TargetLocation “C:\VMRecovery”

PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online

PS C:\> Restore-DPMRecoverableItem -RecoverableItem $RecoveryPoints[0] -RecoveryOption $RecoveryOption
```

<span data-ttu-id="8ab8a-294">Příkazy můžete snadno rozšířit pro jakýkoli typ datasource.</span><span class="sxs-lookup"><span data-stu-id="8ab8a-294">The commands can easily be extended for any datasource type.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8ab8a-295">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8ab8a-295">Next steps</span></span>
* <span data-ttu-id="8ab8a-296">Další informace o zálohování Azure pro aplikaci DPM najdete v části [Úvod k zálohování aplikace DPM](backup-azure-dpm-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="8ab8a-296">For more information about Azure Backup for DPM see [Introduction to DPM Backup](backup-azure-dpm-introduction.md)</span></span>
