---
title: "Spravovat zálohy systému Windows Server v Azure pomocí prostředí PowerShell | Microsoft Docs"
description: "Nasazení a Správa zálohování Windows serveru pomocí prostředí PowerShell."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: e7e269e2-1f11-41a9-957b-a2155de6a1e0
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: saurse;markgal;nkolli;trinadhk
ms.openlocfilehash: a8e20356ae383ee4fa2158ea544d5d0905028124
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="deploy-and-manage-backup-to-azure-for-windows-serverwindows-client-using-powershell"></a><span data-ttu-id="27316-103">Nasazení a správa zálohování do Azure pro servery Windows / klienty Windows pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="27316-103">Deploy and manage backup to Azure for Windows Server/Windows Client using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="27316-104">ARM</span><span class="sxs-lookup"><span data-stu-id="27316-104">ARM</span></span>](backup-client-automation.md)
> * [<span data-ttu-id="27316-105">Classic</span><span class="sxs-lookup"><span data-stu-id="27316-105">Classic</span></span>](backup-client-automation-classic.md)
>
>

<span data-ttu-id="27316-106">Tento článek vysvětluje, jak pomocí prostředí PowerShell pro zálohování systému Windows Server nebo Windows pracovní stanice data do úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="27316-106">This article explains how to use PowerShell to back up Windows Server or Windows workstation data to a backup vault.</span></span> <span data-ttu-id="27316-107">Společnost Microsoft doporučuje používat trezory služeb zotavení pro všechna nová nasazení.</span><span class="sxs-lookup"><span data-stu-id="27316-107">Microsoft recommends using Recovery Services vaults for all new deployments.</span></span> <span data-ttu-id="27316-108">Pokud jsou nového uživatele Azure Backup a nevytvořili trezor záloh v rámci vašeho předplatného, použijte v článku [nasadit a spravovat Data Protection Manager dat do Azure pomocí prostředí PowerShell](backup-client-automation.md) tak data ukládáte do trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="27316-108">If you are a new Azure Backup user and have not created a backup vault in your subscription, use the article, [Deploy and manage Data Protection Manager data to Azure using PowerShell](backup-client-automation.md) so you store your data in a Recovery Services vault.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="27316-109">Nyní můžete trezory služby Backup upgradovat na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="27316-109">You can now upgrade your Backup vaults to Recovery Services vaults.</span></span> <span data-ttu-id="27316-110">Podrobnosti najdete v článku [Upgrade trezoru služby Backup na trezor služby Recovery Services](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="27316-110">For details, see the article [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="27316-111">Microsoft doporučuje, abyste upgradovali své trezory služby Backup na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="27316-111">Microsoft encourages you to upgrade your Backup vaults to Recovery Services vaults.</span></span><br/> <span data-ttu-id="27316-112">Od 15. října 2017 nebude možné pomocí PowerShellu vytvářet trezory služby Backup.</span><span class="sxs-lookup"><span data-stu-id="27316-112">After October 15, 2017, you can’t use PowerShell to create Backup vaults.</span></span> <span data-ttu-id="27316-113">**Do 1. listopadu 2017:**</span><span class="sxs-lookup"><span data-stu-id="27316-113">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="27316-114">Všechny zbývající trezory služby Backup budou automaticky upgradovány na trezory služby Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="27316-114">All remaining Backup vaults will be automatically upgraded to Recovery Services vaults.</span></span>
>- <span data-ttu-id="27316-115">Nebudete mít přístup k datům záloh na portálu Classic.</span><span class="sxs-lookup"><span data-stu-id="27316-115">You won't be able to access your backup data in the classic portal.</span></span> <span data-ttu-id="27316-116">Pro přístup k datům záloh v trezorech služby Recovery Services místo toho použijte Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="27316-116">Instead, use the Azure portal to access your backup data in Recovery Services vaults.</span></span>
>

## <a name="install-azure-powershell"></a><span data-ttu-id="27316-117">Instalace prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="27316-117">Install Azure PowerShell</span></span>
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

<span data-ttu-id="27316-118">Října 2015 byla vydána Azure PowerShell 1.0.</span><span class="sxs-lookup"><span data-stu-id="27316-118">In October 2015, Azure PowerShell 1.0 was released.</span></span> <span data-ttu-id="27316-119">Tato verze 0.9.8 bylo úspěšně dokončeno. Uvolněte a uvést do režimu o významné změny, zejména v vzoru pro pojmenovávání rutin.</span><span class="sxs-lookup"><span data-stu-id="27316-119">This release succeeded the 0.9.8 release and brought about some significant changes, especially in the naming pattern of the cmdlets.</span></span> <span data-ttu-id="27316-120">Rutiny verze 1.0 dodržují formát pojmenování {sloveso}-AzureRm {podstatné jméno}, kdežto názvy ve verzi 0.9.8 neobsahují označení **Rm** (třeba New-AzureRmResourceGroup místo New-AzureResourceGroup).</span><span class="sxs-lookup"><span data-stu-id="27316-120">1.0 cmdlets follow the naming pattern {verb}-AzureRm{noun}; whereas, the 0.9.8 names do not include **Rm** (for example, New-AzureRmResourceGroup instead of New-AzureResourceGroup).</span></span> <span data-ttu-id="27316-121">Pokud používáte prostředí Azure PowerShell 0.9.8, musíte nejdřív spuštěním příkazu **Switch-AzureMode AzureResourceManager** spustit režim Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="27316-121">When using Azure PowerShell 0.9.8, you must first enable the Resource Manager mode by running the **Switch-AzureMode AzureResourceManager** command.</span></span> <span data-ttu-id="27316-122">Tento příkaz není nutné v 1.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="27316-122">This command is not necessary in 1.0 or later.</span></span>

<span data-ttu-id="27316-123">Pokud chcete použít skripty napsané pro verzi 0.9.8 prostředí, v prostředí 1.0 nebo novější, měli byste pečlivě otestovat skripty v předprodukčním prostředí před jejich používání v produkčním prostředí předejdete neočekávané dopad.</span><span class="sxs-lookup"><span data-stu-id="27316-123">If you want to use your scripts written for the 0.9.8 environment, in the 1.0 or later environment, you should carefully test the scripts in a pre-production environment before using them in production to avoid unexpected impact.</span></span>

<span data-ttu-id="27316-124">[Stáhněte si nejnovější verzi prostředí PowerShell](https://github.com/Azure/azure-powershell/releases) (minimální požadovaná verze je: 1.0.0)</span><span class="sxs-lookup"><span data-stu-id="27316-124">[Download the latest PowerShell release](https://github.com/Azure/azure-powershell/releases) (minimum version required is : 1.0.0)</span></span>

[!INCLUDE [arm-getting-setup-powershell](../../includes/arm-getting-setup-powershell.md)]

## <a name="create-a-backup-vault"></a><span data-ttu-id="27316-125">Vytvoření trezoru záloh</span><span class="sxs-lookup"><span data-stu-id="27316-125">Create a backup vault</span></span>
> [!WARNING]
> <span data-ttu-id="27316-126">Pro zákazníky pomocí služby Azure Backup poprvé budete muset registraci poskytovatele Azure Backup pro použití s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="27316-126">For customers using Azure Backup for the first time, you need to register the Azure Backup provider to be used with your subscription.</span></span> <span data-ttu-id="27316-127">To můžete provést spuštěním následujícího příkazu: Register-AzureProvider - ProviderNamespace "Microsoft.Backup"</span><span class="sxs-lookup"><span data-stu-id="27316-127">This can be done by running the following command: Register-AzureProvider -ProviderNamespace "Microsoft.Backup"</span></span>
>
>

<span data-ttu-id="27316-128">Můžete vytvořit nové úložiště záloh pomocí **New-AzureRMBackupVault** rutiny.</span><span class="sxs-lookup"><span data-stu-id="27316-128">You can create a new backup vault using the **New-AzureRMBackupVault** cmdlet.</span></span> <span data-ttu-id="27316-129">Trezor záloh je prostředek ARM, proto musíte umístit do skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="27316-129">The backup vault is an ARM resource, so you need to place it within a Resource Group.</span></span> <span data-ttu-id="27316-130">V prostředí Azure PowerShell konzolu se zvýšenými oprávněními spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="27316-130">In an elevated Azure PowerShell console, run the following commands:</span></span>

```
PS C:\> New-AzureResourceGroup –Name “test-rg” -Region “West US”
PS C:\> $backupvault = New-AzureRMBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GeoRedundant
```

<span data-ttu-id="27316-131">Použití **Get-AzureRMBackupVault** rutiny seznamu trezorů záloh v předplatném.</span><span class="sxs-lookup"><span data-stu-id="27316-131">Use the **Get-AzureRMBackupVault** cmdlet to list the backup vaults in a subscription.</span></span>

## <a name="installing-the-azure-backup-agent"></a><span data-ttu-id="27316-132">Instalace agenta Azure Backup</span><span class="sxs-lookup"><span data-stu-id="27316-132">Installing the Azure Backup agent</span></span>
<span data-ttu-id="27316-133">Před instalací agenta Azure Backup, musíte mít instalační program stažené a existuje v systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="27316-133">Before you install the Azure Backup agent, you need to have the installer downloaded and present on the Windows Server.</span></span> <span data-ttu-id="27316-134">Můžete získat nejnovější verzi instalačního programu z [Microsoft Download Center](http://aka.ms/azurebackup_agent) nebo ze stránky řídicího panelu trezoru záloh.</span><span class="sxs-lookup"><span data-stu-id="27316-134">You can get the latest version of the installer from the [Microsoft Download Center](http://aka.ms/azurebackup_agent) or from the backup vault's Dashboard page.</span></span> <span data-ttu-id="27316-135">Instalační program uložte na snadno dostupném místě jako * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="27316-135">Save the installer to an easily accessible location like *C:\Downloads\*.</span></span>

<span data-ttu-id="27316-136">Pokud chcete nainstalovat agenta, spusťte v konzolu prostředí PowerShell se zvýšenými oprávněními následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="27316-136">To install the agent, run the following command in an elevated PowerShell console:</span></span>

```
PS C:\> MARSAgentInstaller.exe /q
```

<span data-ttu-id="27316-137">Tím se nainstaluje agent s výchozími možnostmi.</span><span class="sxs-lookup"><span data-stu-id="27316-137">This installs the agent with all the default options.</span></span> <span data-ttu-id="27316-138">Instalace trvá několik minut na pozadí.</span><span class="sxs-lookup"><span data-stu-id="27316-138">The installation takes a few minutes in the background.</span></span> <span data-ttu-id="27316-139">Pokud nezadáte */nu* možnost potom **Windows Update** otevře se okno na konci instalace k vyhledání všech aktualizací.</span><span class="sxs-lookup"><span data-stu-id="27316-139">If you do not specify the */nu* option then the **Windows Update** window will open at the end of the installation to check for any updates.</span></span> <span data-ttu-id="27316-140">Po instalaci agenta se zobrazí v seznamu nainstalovaných programů.</span><span class="sxs-lookup"><span data-stu-id="27316-140">Once installed, the agent will show in the list of installed programs.</span></span>

<span data-ttu-id="27316-141">Chcete-li zobrazit seznam nainstalovaných programů, přejděte na **ovládací panely** > **programy** > **programy a funkce**.</span><span class="sxs-lookup"><span data-stu-id="27316-141">To see the list of installed programs, go to **Control Panel** > **Programs** > **Programs and Features**.</span></span>

![Instalaci agenta](./media/backup-client-automation/installed-agent-listing.png)

### <a name="installation-options"></a><span data-ttu-id="27316-143">Možnosti instalace</span><span class="sxs-lookup"><span data-stu-id="27316-143">Installation options</span></span>
<span data-ttu-id="27316-144">Pokud chcete zobrazit všechny možnosti, které jsou k dispozici prostřednictvím příkazového řádku, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="27316-144">To see all the options available via the command-line, use the following command:</span></span>

```
PS C:\> MARSAgentInstaller.exe /?
```

<span data-ttu-id="27316-145">Mezi dostupné možnosti patří:</span><span class="sxs-lookup"><span data-stu-id="27316-145">The available options include:</span></span>

| <span data-ttu-id="27316-146">Možnost</span><span class="sxs-lookup"><span data-stu-id="27316-146">Option</span></span> | <span data-ttu-id="27316-147">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="27316-147">Details</span></span> | <span data-ttu-id="27316-148">Výchozí</span><span class="sxs-lookup"><span data-stu-id="27316-148">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="27316-149">/q</span><span class="sxs-lookup"><span data-stu-id="27316-149">/q</span></span> |<span data-ttu-id="27316-150">Tichá instalace</span><span class="sxs-lookup"><span data-stu-id="27316-150">Quiet installation</span></span> |- |
| <span data-ttu-id="27316-151">/ p: "umístění"</span><span class="sxs-lookup"><span data-stu-id="27316-151">/p:"location"</span></span> |<span data-ttu-id="27316-152">Cesta ke složce instalace pro agenta Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="27316-152">Path to the installation folder for the Azure Backup agent.</span></span> |<span data-ttu-id="27316-153">C:\Program Files\Microsoft Azure Recovery Services agenta</span><span class="sxs-lookup"><span data-stu-id="27316-153">C:\Program Files\Microsoft Azure Recovery Services Agent</span></span> |
| <span data-ttu-id="27316-154">/ s: "umístění"</span><span class="sxs-lookup"><span data-stu-id="27316-154">/s:"location"</span></span> |<span data-ttu-id="27316-155">Cesta ke složce mezipaměti pro agenta Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="27316-155">Path to the cache folder for the Azure Backup agent.</span></span> |<span data-ttu-id="27316-156">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span><span class="sxs-lookup"><span data-stu-id="27316-156">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span></span> |
| <span data-ttu-id="27316-157">/m</span><span class="sxs-lookup"><span data-stu-id="27316-157">/m</span></span> |<span data-ttu-id="27316-158">Výslovný souhlas ke službě Microsoft Update</span><span class="sxs-lookup"><span data-stu-id="27316-158">Opt-in to Microsoft Update</span></span> |- |
| <span data-ttu-id="27316-159">/nu</span><span class="sxs-lookup"><span data-stu-id="27316-159">/nu</span></span> |<span data-ttu-id="27316-160">Nekontrolovat aktualizace po dokončení instalace</span><span class="sxs-lookup"><span data-stu-id="27316-160">Do not Check for updates after installation is complete</span></span> |- |
| <span data-ttu-id="27316-161">/d</span><span class="sxs-lookup"><span data-stu-id="27316-161">/d</span></span> |<span data-ttu-id="27316-162">Odinstaluje Agenta Microsoft Azure Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="27316-162">Uninstalls Microsoft Azure Recovery Services Agent</span></span> |- |
| <span data-ttu-id="27316-163">/pH</span><span class="sxs-lookup"><span data-stu-id="27316-163">/ph</span></span> |<span data-ttu-id="27316-164">Adresa proxy hostitele</span><span class="sxs-lookup"><span data-stu-id="27316-164">Proxy Host Address</span></span> |- |
| <span data-ttu-id="27316-165">/Po</span><span class="sxs-lookup"><span data-stu-id="27316-165">/po</span></span> |<span data-ttu-id="27316-166">Číslo portu proxy hostitele</span><span class="sxs-lookup"><span data-stu-id="27316-166">Proxy Host Port Number</span></span> |- |
| <span data-ttu-id="27316-167">/Pu</span><span class="sxs-lookup"><span data-stu-id="27316-167">/pu</span></span> |<span data-ttu-id="27316-168">Uživatelské jméno proxy hostitele</span><span class="sxs-lookup"><span data-stu-id="27316-168">Proxy Host UserName</span></span> |- |
| <span data-ttu-id="27316-169">/pW</span><span class="sxs-lookup"><span data-stu-id="27316-169">/pw</span></span> |<span data-ttu-id="27316-170">Heslo pro proxy server</span><span class="sxs-lookup"><span data-stu-id="27316-170">Proxy Password</span></span> |- |

## <a name="registering-with-the-azure-backup-service"></a><span data-ttu-id="27316-171">Registrace u služby zálohování Azure</span><span class="sxs-lookup"><span data-stu-id="27316-171">Registering with the Azure Backup service</span></span>
<span data-ttu-id="27316-172">Než můžete zaregistrovat pomocí služby Azure Backup, musíte zajistit, aby [požadavky](backup-configure-vault.md) jsou splněny.</span><span class="sxs-lookup"><span data-stu-id="27316-172">Before you can register with the Azure Backup service, you need to ensure that the [prerequisites](backup-configure-vault.md) are met.</span></span> <span data-ttu-id="27316-173">Postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="27316-173">You must:</span></span>

* <span data-ttu-id="27316-174">Máte platné předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="27316-174">Have a valid Azure subscription</span></span>
* <span data-ttu-id="27316-175">Mít úložiště záloh</span><span class="sxs-lookup"><span data-stu-id="27316-175">Have a backup vault</span></span>

<span data-ttu-id="27316-176">Chcete-li stáhnout přihlašovací údaje trezoru, spusťte **Get-AzureRMBackupVaultCredentials** rutiny do konzoly Azure PowerShell a úložiště do vhodného umístění, například * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="27316-176">To download the vault credentials, run the **Get-AzureRMBackupVaultCredentials** cmdlet in an Azure PowerShell console and store it in a convenient location like *C:\Downloads\*.</span></span>

```
PS C:\> $credspath = "C:\"
PS C:\> $credsfilename = Get-AzureRMBackupVaultCredentials -Vault $backupvault -TargetLocation $credspath
PS C:\> $credsfilename
f5303a0b-fae4-4cdb-b44d-0e4c032dde26_backuprg_backuprn_2015-08-11--06-22-35.VaultCredentials
```

<span data-ttu-id="27316-177">Počítač zaregistrovat v úložišti se provádí pomocí [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) rutiny:</span><span class="sxs-lookup"><span data-stu-id="27316-177">Registering the machine with the vault is done using the [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) cmdlet:</span></span>

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-OBRegistration -VaultCredentials $cred -Confirm:$false

CertThumbprint      : 7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName : test-vault
Region              : West US

Machine registration succeeded.
```

> [!IMPORTANT]
> <span data-ttu-id="27316-178">Nepoužívejte relativní cesty k určení souboru s přihlašovacími údaji.</span><span class="sxs-lookup"><span data-stu-id="27316-178">Do not use relative paths to specify the vault credentials file.</span></span> <span data-ttu-id="27316-179">Jako vstup do rutiny, musí zadat absolutní cestu.</span><span class="sxs-lookup"><span data-stu-id="27316-179">You must provide an absolute path as an input to the cmdlet.</span></span>
>
>

## <a name="networking-settings"></a><span data-ttu-id="27316-180">Nastavení sítě</span><span class="sxs-lookup"><span data-stu-id="27316-180">Networking settings</span></span>
<span data-ttu-id="27316-181">Pokud připojení Windows počítače k Internetu prostřednictvím proxy serveru, nastavení proxy serveru se dá zadat i k agentovi.</span><span class="sxs-lookup"><span data-stu-id="27316-181">When the connectivity of the Windows machine to the internet is through a proxy server, the proxy settings can also be provided to the agent.</span></span> <span data-ttu-id="27316-182">V tomto příkladu neexistuje žádný proxy server, takže jsme jsou explicitně vymazání údaje související s proxy serverem.</span><span class="sxs-lookup"><span data-stu-id="27316-182">In this example, there is no proxy server, so we are explicitly clearing any proxy-related information.</span></span>

<span data-ttu-id="27316-183">Využití šířky pásma se dá taky nastavit pomocí možnosti ```work hour bandwidth``` a ```non-work hour bandwidth``` pro danou sadu dny v týdnu.</span><span class="sxs-lookup"><span data-stu-id="27316-183">Bandwidth usage can also be controlled with the options of ```work hour bandwidth``` and ```non-work hour bandwidth``` for a given set of days of the week.</span></span>

<span data-ttu-id="27316-184">Nastavení serveru proxy a šířky pásma podrobnosti se provádí pomocí [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) rutiny:</span><span class="sxs-lookup"><span data-stu-id="27316-184">Setting the proxy and bandwidth details is done using the [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) cmdlet:</span></span>

```
PS C:\> Set-OBMachineSetting -NoProxy
Server properties updated successfully.

PS C:\> Set-OBMachineSetting -NoThrottle
Server properties updated successfully.
```

## <a name="encryption-settings"></a><span data-ttu-id="27316-185">Nastavení šifrování</span><span class="sxs-lookup"><span data-stu-id="27316-185">Encryption settings</span></span>
<span data-ttu-id="27316-186">Zálohování data přenášená do služby Azure Backup je zašifrovaná chránit jejich důvěrnost dat.</span><span class="sxs-lookup"><span data-stu-id="27316-186">The backup data sent to Azure Backup is encrypted to protect the confidentiality of the data.</span></span> <span data-ttu-id="27316-187">Šifrovací přístupové heslo je 'heslo' k dešifrování dat v době obnovení.</span><span class="sxs-lookup"><span data-stu-id="27316-187">The encryption passphrase is the "password" to decrypt the data at the time of restore.</span></span>

```
PS C:\> ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force | Set-OBMachineSetting
Server properties updated successfully
```

> [!IMPORTANT]
> <span data-ttu-id="27316-188">Zachování informací o přístupové heslo bezpečném, po nastavení.</span><span class="sxs-lookup"><span data-stu-id="27316-188">Keep the passphrase information safe and secure once it is set.</span></span> <span data-ttu-id="27316-189">Nebudete schopni obnovit data ze služby Azure bez tohoto hesla.</span><span class="sxs-lookup"><span data-stu-id="27316-189">You will not be able to restore data from Azure without this passphrase.</span></span>
>
>

## <a name="back-up-files-and-folders"></a><span data-ttu-id="27316-190">Zálohování souborů a složek</span><span class="sxs-lookup"><span data-stu-id="27316-190">Back up files and folders</span></span>
<span data-ttu-id="27316-191">Zásady se řídí všechny zálohy ze Windows serverů a klientů do služby Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="27316-191">All your backups from Windows Servers and clients to Azure Backup are governed by a policy.</span></span> <span data-ttu-id="27316-192">Zásady se skládá ze tří částí:</span><span class="sxs-lookup"><span data-stu-id="27316-192">The policy comprises three parts:</span></span>

1. <span data-ttu-id="27316-193">A **plán zálohování** určující při zálohování muset provést a synchronizovat se službou.</span><span class="sxs-lookup"><span data-stu-id="27316-193">A **backup schedule** that specifies when backups need to be taken and synchronized with the service.</span></span>
2. <span data-ttu-id="27316-194">A **plán uchovávání informací** který určuje, jak dlouho chcete zachovat body obnovení v Azure.</span><span class="sxs-lookup"><span data-stu-id="27316-194">A **retention schedule** that specifies how long to retain the recovery points in Azure.</span></span>
3. <span data-ttu-id="27316-195">A **zadání zahrnutí nebo vyloučení souboru** který určuje, co by měl být zálohovány.</span><span class="sxs-lookup"><span data-stu-id="27316-195">A **file inclusion/exclusion specification** that dictates what should be backed up.</span></span>

<span data-ttu-id="27316-196">V tomto dokumentu protože jsme se automatizaci zálohování, budete předpokládáme, že nic nebyl nakonfigurován.</span><span class="sxs-lookup"><span data-stu-id="27316-196">In this document, since we're automating backup, we'll assume nothing has been configured.</span></span> <span data-ttu-id="27316-197">Začneme vytvořením nové zásady zálohování pomocí [New-OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) rutiny a jeho použití.</span><span class="sxs-lookup"><span data-stu-id="27316-197">We begin by creating a new backup policy using the [New-OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) cmdlet and using it.</span></span>

```
PS C:\> $newpolicy = New-OBPolicy
```

<span data-ttu-id="27316-198">V tuto chvíli je prázdné zásady a ostatní rutiny jsou potřebné k definování položky, které je budou zahrnuté ani vyloučené, při zálohování se spustí, a tam, kde se uloží v zálohování.</span><span class="sxs-lookup"><span data-stu-id="27316-198">At this time the policy is empty and other cmdlets are needed to define what items will be included or excluded, when backups will run, and where the backups will be stored.</span></span>

### <a name="configuring-the-backup-schedule"></a><span data-ttu-id="27316-199">Konfigurace plánu zálohování</span><span class="sxs-lookup"><span data-stu-id="27316-199">Configuring the backup schedule</span></span>
<span data-ttu-id="27316-200">První den 3 části zásada je plán zálohování, která je vytvořena pomocí [New-OBSchedule](https://technet.microsoft.com/library/hh770401) rutiny.</span><span class="sxs-lookup"><span data-stu-id="27316-200">The first of the 3 parts of a policy is the backup schedule, which is created using the [New-OBSchedule](https://technet.microsoft.com/library/hh770401) cmdlet.</span></span> <span data-ttu-id="27316-201">Plán zálohování definuje, kdy je třeba přijmout zálohy.</span><span class="sxs-lookup"><span data-stu-id="27316-201">The backup schedule defines when backups need to be taken.</span></span> <span data-ttu-id="27316-202">Při vytváření plánu je třeba zadat 2 vstupní parametry:</span><span class="sxs-lookup"><span data-stu-id="27316-202">When creating a schedule you need to specify 2 input parameters:</span></span>

* <span data-ttu-id="27316-203">**Dny v týdnu** , má-li spustit zálohování.</span><span class="sxs-lookup"><span data-stu-id="27316-203">**Days of the week** that the backup should run.</span></span> <span data-ttu-id="27316-204">Můžete spustit úlohu zálohování na právě jeden den nebo každý den v týdnu nebo libovolnou kombinaci mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="27316-204">You can run the backup job on just one day, or every day of the week, or any combination in between.</span></span>
* <span data-ttu-id="27316-205">**Časy dne** spuštění zálohování.</span><span class="sxs-lookup"><span data-stu-id="27316-205">**Times of the day** when the backup should run.</span></span> <span data-ttu-id="27316-206">Můžete definovat až 3 různou dobu, kdy se spustí zálohování.</span><span class="sxs-lookup"><span data-stu-id="27316-206">You can define up to 3 different times of the day when the backup will be triggered.</span></span>

<span data-ttu-id="27316-207">Například může nakonfigurovat zásady zálohování, který se spustí na 16: 00 každou sobotu a neděli.</span><span class="sxs-lookup"><span data-stu-id="27316-207">For instance, you could configure a backup policy that runs at 4PM every Saturday and Sunday.</span></span>

```
PS C:\> $sched = New-OBSchedule -DaysofWeek Saturday, Sunday -TimesofDay 16:00
```

<span data-ttu-id="27316-208">Plán zálohování musí být přidružený k zásadě, a to jde dosáhnout pomocí [Set-OBSchedule](https://technet.microsoft.com/library/hh770407) rutiny.</span><span class="sxs-lookup"><span data-stu-id="27316-208">The backup schedule needs to be associated with a policy, and this can be achieved by using the [Set-OBSchedule](https://technet.microsoft.com/library/hh770407) cmdlet.</span></span>

```
PS C:> Set-OBSchedule -Policy $newpolicy -Schedule $sched
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s) DsList : PolicyName : RetentionPolicy : State : New PolicyState : Valid
```
### <a name="configuring-a-retention-policy"></a><span data-ttu-id="27316-209">Konfigurace zásad uchovávání</span><span class="sxs-lookup"><span data-stu-id="27316-209">Configuring a retention policy</span></span>
<span data-ttu-id="27316-210">Zásady uchovávání informací definuje, jak dlouho se uchovají body obnovení vytvořené z úlohy zálohování.</span><span class="sxs-lookup"><span data-stu-id="27316-210">The retention policy defines how long recovery points created from backup jobs are retained.</span></span> <span data-ttu-id="27316-211">Při vytváření nové zásady uchovávání informací pomocí [New-OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) rutiny, můžete zadat počet dnů, které je třeba body obnovení záloh pro zachování s Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="27316-211">When creating a new retention policy using the [New-OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) cmdlet, you can specify the number of days that the backup recovery points need to be retained with Azure Backup.</span></span> <span data-ttu-id="27316-212">Následující příklad nastaví zásady uchovávání informací 7 dní.</span><span class="sxs-lookup"><span data-stu-id="27316-212">The example below sets a retention policy of 7 days.</span></span>

```
PS C:\> $retentionpolicy = New-OBRetentionPolicy -RetentionDays 7
```

<span data-ttu-id="27316-213">Zásady uchovávání informací musí být přidružené hlavní zásadám, pomocí rutiny [Set-OBRetentionPolicy](https://technet.microsoft.com/library/hh770405):</span><span class="sxs-lookup"><span data-stu-id="27316-213">The retention policy must be associated with the main policy using the cmdlet [Set-OBRetentionPolicy](https://technet.microsoft.com/library/hh770405):</span></span>

```
PS C:\> Set-OBRetentionPolicy -Policy $newpolicy -RetentionPolicy $retentionpolicy

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          :
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```
### <a name="including-and-excluding-files-to-be-backed-up"></a><span data-ttu-id="27316-214">Zahrnutí a vyloučení souborů k zálohování</span><span class="sxs-lookup"><span data-stu-id="27316-214">Including and excluding files to be backed up</span></span>
<span data-ttu-id="27316-215">```OBFileSpec``` Objekt definuje soubory zahrnout a vyloučit zálohu.</span><span class="sxs-lookup"><span data-stu-id="27316-215">An ```OBFileSpec``` object defines the files to be included and excluded in a backup.</span></span> <span data-ttu-id="27316-216">To je sada pravidel, která obor chráněné soubory a složky na počítači.</span><span class="sxs-lookup"><span data-stu-id="27316-216">This is a set of rules that scope out the protected files and folders on a machine.</span></span> <span data-ttu-id="27316-217">Můžete mít mnoho souboru pravidla zahrnutí nebo vyloučení podle potřeby a přidružovat je k zásadě.</span><span class="sxs-lookup"><span data-stu-id="27316-217">You can have as many file inclusion or exclusion rules as required, and associate them with a policy.</span></span> <span data-ttu-id="27316-218">Při vytváření nového objektu OBFileSpec, můžete:</span><span class="sxs-lookup"><span data-stu-id="27316-218">When creating a new OBFileSpec object, you can:</span></span>

* <span data-ttu-id="27316-219">Zadejte soubory a složky, které mají být zahrnuty</span><span class="sxs-lookup"><span data-stu-id="27316-219">Specify the files and folders to be included</span></span>
* <span data-ttu-id="27316-220">Zadejte soubory a složky, které se mají vyloučit</span><span class="sxs-lookup"><span data-stu-id="27316-220">Specify the files and folders to be excluded</span></span>
* <span data-ttu-id="27316-221">Zadejte rekurzivní zálohování dat v složku (nebo) zda pouze nejvyšší úrovně soubory v zadané složce by měl být zálohuje nahoru.</span><span class="sxs-lookup"><span data-stu-id="27316-221">Specify recursive backup of data in a folder (or) whether only the top-level files in the specified folder should be backed up.</span></span>

<span data-ttu-id="27316-222">K tomu se dosáhne použitím příznaku - nerekurzivní v příkazu New-OBFileSpec.</span><span class="sxs-lookup"><span data-stu-id="27316-222">The latter is achieved by using the -NonRecursive flag in the New-OBFileSpec command.</span></span>

<span data-ttu-id="27316-223">V následujícím příkladu jsme budete zálohování svazku C: a D: a vyloučit binární soubory operačního systému ve složce Windows a všechny dočasné složky.</span><span class="sxs-lookup"><span data-stu-id="27316-223">In the example below, we'll back up volume C: and D: and exclude the OS binaries in the Windows folder and any temporary folders.</span></span> <span data-ttu-id="27316-224">K tomu vytvoříme dvě specifikace pomocí souboru [New-OBFileSpec](https://technet.microsoft.com/library/hh770408) rutiny – jeden pro zahrnutí a jeden pro vyloučení.</span><span class="sxs-lookup"><span data-stu-id="27316-224">To do so we'll create two file specifications using the [New-OBFileSpec](https://technet.microsoft.com/library/hh770408) cmdlet - one for inclusion and one for exclusion.</span></span> <span data-ttu-id="27316-225">Po vytvoření souboru specifikace jsou spojené s použitím zásad [přidat OBFileSpec](https://technet.microsoft.com/library/hh770424) rutiny.</span><span class="sxs-lookup"><span data-stu-id="27316-225">Once the file specifications have been created, they're associated with the policy using the [Add-OBFileSpec](https://technet.microsoft.com/library/hh770424) cmdlet.</span></span>

```
PS C:\> $inclusions = New-OBFileSpec -FileSpec @("C:\", "D:\")

PS C:\> $exclusions = New-OBFileSpec -FileSpec @("C:\windows", "C:\temp") -Exclude

PS C:\> Add-OBFileSpec -Policy $newpolicy -FileSpec $inclusions

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid


PS C:\> Add-OBFileSpec -Policy $newpolicy -FileSpec $exclusions

BackupSchedule  : 4:00 PM
                  Saturday, Sunday,
                  Every 1 week(s)
DsList          : {DataSource
                  DatasourceId:0
                  Name:C:\
                  FileSpec:FileSpec
                  FileSpec:C:\
                  IsExclude:False
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\windows
                  IsExclude:True
                  IsRecursive:True
                  ,FileSpec
                  FileSpec:C:\temp
                  IsExclude:True
                  IsRecursive:True

                  , DataSource
                  DatasourceId:0
                  Name:D:\
                  FileSpec:FileSpec
                  FileSpec:D:\
                  IsExclude:False
                  IsRecursive:True

                  }
PolicyName      :
RetentionPolicy : Retention Days : 7

                  WeeklyLTRSchedule :
                  Weekly schedule is not set

                  MonthlyLTRSchedule :
                  Monthly schedule is not set

                  YearlyLTRSchedule :
                  Yearly schedule is not set

State           : New
PolicyState     : Valid
```

### <a name="applying-the-policy"></a><span data-ttu-id="27316-226">Použití zásad</span><span class="sxs-lookup"><span data-stu-id="27316-226">Applying the policy</span></span>
<span data-ttu-id="27316-227">Objekt zásad teď dokončení a má přidružené plánu zálohování, zásady uchovávání informací a seznam souborů zahrnutí nebo vyloučení.</span><span class="sxs-lookup"><span data-stu-id="27316-227">Now the policy object is complete and has an associated backup schedule, retention policy, and an inclusion/exclusion list of files.</span></span> <span data-ttu-id="27316-228">Tato zásada může být nyní potvrdit pro zálohování Azure používat.</span><span class="sxs-lookup"><span data-stu-id="27316-228">This policy can now be committed for Azure Backup to use.</span></span> <span data-ttu-id="27316-229">Před použitím nově vytvořenou zásadu zajistěte, aby žádné existující zásady zálohování, které jsou přidružené k serveru pomocí [odebrat OBPolicy](https://technet.microsoft.com/library/hh770415) rutiny.</span><span class="sxs-lookup"><span data-stu-id="27316-229">Before you apply the newly created policy ensure that there are no existing backup policies associated with the server by using the [Remove-OBPolicy](https://technet.microsoft.com/library/hh770415) cmdlet.</span></span> <span data-ttu-id="27316-230">Odebrání zásady zobrazí výzvu k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="27316-230">Removing the policy will prompt for confirmation.</span></span> <span data-ttu-id="27316-231">Přeskočit použití potvrzení ```-Confirm:$false``` příznak pomocí rutiny.</span><span class="sxs-lookup"><span data-stu-id="27316-231">To skip the confirmation use the ```-Confirm:$false``` flag with the cmdlet.</span></span>

```
PS C:> Get-OBPolicy | Remove-OBPolicy
Microsoft Azure Backup Are you sure you want to remove this backup policy? This will delete all the backed up data. [Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Y"):
```

<span data-ttu-id="27316-232">Potvrzení objektu zásad se provádí pomocí [Set-OBPolicy](https://technet.microsoft.com/library/hh770421) rutiny.</span><span class="sxs-lookup"><span data-stu-id="27316-232">Committing the policy object is done using the [Set-OBPolicy](https://technet.microsoft.com/library/hh770421) cmdlet.</span></span> <span data-ttu-id="27316-233">Také to požádá o potvrzení.</span><span class="sxs-lookup"><span data-stu-id="27316-233">This will also ask for confirmation.</span></span> <span data-ttu-id="27316-234">Přeskočit použití potvrzení ```-Confirm:$false``` příznak pomocí rutiny.</span><span class="sxs-lookup"><span data-stu-id="27316-234">To skip the confirmation use the ```-Confirm:$false``` flag with the cmdlet.</span></span>

```
PS C:> Set-OBPolicy -Policy $newpolicy
Microsoft Azure Backup Do you want to save this backup policy ? [Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Y"):
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s)
DsList : {DataSource
         DatasourceId:4508156004108672185
         Name:C:\
         FileSpec:FileSpec
         FileSpec:C:\
         IsExclude:False
         IsRecursive:True,

         FileSpec
         FileSpec:C:\windows
         IsExclude:True
         IsRecursive:True,

         FileSpec
         FileSpec:C:\temp
         IsExclude:True
         IsRecursive:True,

         DataSource
         DatasourceId:4508156005178868542
         Name:D:\
         FileSpec:FileSpec
         FileSpec:D:\
         IsExclude:False
         IsRecursive:True
    }
PolicyName : c2eb6568-8a06-49f4-a20e-3019ae411bac
RetentionPolicy : Retention Days : 7
              WeeklyLTRSchedule :
              Weekly schedule is not set

              MonthlyLTRSchedule :
              Monthly schedule is not set

              YearlyLTRSchedule :
              Yearly schedule is not set
State : Existing PolicyState : Valid
```

<span data-ttu-id="27316-235">Můžete zobrazit podrobnosti o existující zásady zálohování pomocí [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) rutiny.</span><span class="sxs-lookup"><span data-stu-id="27316-235">You can view the details of the existing backup policy using the [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) cmdlet.</span></span> <span data-ttu-id="27316-236">Vám může procházení další pomocí [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) rutina pro plán zálohování a [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) rutina pro zásady uchovávání informací</span><span class="sxs-lookup"><span data-stu-id="27316-236">You can drill-down further using the [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) cmdlet for the backup schedule and the [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) cmdlet for the retention policies</span></span>

```
PS C:> Get-OBPolicy | Get-OBSchedule
SchedulePolicyName : 71944081-9950-4f7e-841d-32f0a0a1359a
ScheduleRunDays : {Saturday, Sunday}
ScheduleRunTimes : {16:00:00}
State : Existing

PS C:> Get-OBPolicy | Get-OBRetentionPolicy
RetentionDays : 7
RetentionPolicyName : ca3574ec-8331-46fd-a605-c01743a5265e
State : Existing

PS C:> Get-OBPolicy | Get-OBFileSpec
FileName : *
FilePath : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
FileSpec : D:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\
FileSpec : C:\
IsExclude : False
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\windows
FileSpec : C:\windows
IsExclude : True
IsRecursive : True

FileName : *
FilePath : \?\Volume{cdd41007-a22f-11e2-be6c-806e6f6e6963}\temp
FileSpec : C:\temp
IsExclude : True
IsRecursive : True
```

### <a name="performing-an-ad-hoc-backup"></a><span data-ttu-id="27316-237">Provádění zálohu ad-hoc</span><span class="sxs-lookup"><span data-stu-id="27316-237">Performing an ad-hoc backup</span></span>
<span data-ttu-id="27316-238">Jakmile byla nastavena zásada zálohování dojde k zálohování podle plánu.</span><span class="sxs-lookup"><span data-stu-id="27316-238">Once a backup policy has been set the backups will occur per the schedule.</span></span> <span data-ttu-id="27316-239">Aktivuje zálohu ad-hoc je také možné pomocí [Start-OBBackup](https://technet.microsoft.com/library/hh770426) rutiny:</span><span class="sxs-lookup"><span data-stu-id="27316-239">Triggering an ad-hoc backup is also possible using the [Start-OBBackup](https://technet.microsoft.com/library/hh770426) cmdlet:</span></span>

```
PS C:> Get-OBPolicy | Start-OBBackup
Taking snapshot of volumes...
Preparing storage...
Estimating size of backup items...
Estimating size of backup items...
Transferring data...
Verifying backup...
Job completed.
The backup operation completed successfully.
```

## <a name="restore-data-from-azure-backup"></a><span data-ttu-id="27316-240">Obnovení dat z Azure Backup</span><span class="sxs-lookup"><span data-stu-id="27316-240">Restore data from Azure Backup</span></span>
<span data-ttu-id="27316-241">Tato část vás provede kroky pro automatizaci obnovení dat z Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="27316-241">This section will guide you through the steps for automating recovery of data from Azure Backup.</span></span> <span data-ttu-id="27316-242">Díky tomu zahrnuje následující kroky:</span><span class="sxs-lookup"><span data-stu-id="27316-242">Doing so involves the following steps:</span></span>

1. <span data-ttu-id="27316-243">Vyberte zdrojový svazek</span><span class="sxs-lookup"><span data-stu-id="27316-243">Pick the source volume</span></span>
2. <span data-ttu-id="27316-244">Vyberte bod obnovení zálohy</span><span class="sxs-lookup"><span data-stu-id="27316-244">Choose a backup point to restore</span></span>
3. <span data-ttu-id="27316-245">Vyberte položku, kterou chcete obnovit</span><span class="sxs-lookup"><span data-stu-id="27316-245">Choose an item to restore</span></span>
4. <span data-ttu-id="27316-246">Aktivační událost v procesu obnovení</span><span class="sxs-lookup"><span data-stu-id="27316-246">Trigger the restore process</span></span>

### <a name="picking-the-source-volume"></a><span data-ttu-id="27316-247">Výběr zdrojovém svazku</span><span class="sxs-lookup"><span data-stu-id="27316-247">Picking the source volume</span></span>
<span data-ttu-id="27316-248">Chcete-li obnovit položky z Azure Backup, musíte nejprve k identifikaci zdroje položky.</span><span class="sxs-lookup"><span data-stu-id="27316-248">In order to restore an item from Azure Backup, you first need to identify the source of the item.</span></span> <span data-ttu-id="27316-249">Vzhledem k tomu, že jsme se provádění příkazů v kontextu systému Windows Server nebo klienta Windows, počítač je už definovaný.</span><span class="sxs-lookup"><span data-stu-id="27316-249">Since we're executing the commands in the context of a Windows Server or a Windows client, the machine is already identified.</span></span> <span data-ttu-id="27316-250">Dalším krokem při identifikaci zdroj je k identifikaci svazku, který ji obsahuje.</span><span class="sxs-lookup"><span data-stu-id="27316-250">The next step in identifying the source is to identify the volume containing it.</span></span> <span data-ttu-id="27316-251">Seznam svazků nebo zdrojů zálohovaných z tohoto počítače se dá načíst spuštěním [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) rutiny.</span><span class="sxs-lookup"><span data-stu-id="27316-251">A list of volumes or sources being backed up from this machine can be retrieved by executing the [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) cmdlet.</span></span> <span data-ttu-id="27316-252">Tento příkaz vrátí pole všech zdrojů zálohovat z tohoto serveru nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="27316-252">This command returns an array of all the sources backed up from this server/client.</span></span>

```
PS C:> $source = Get-OBRecoverableSource
PS C:> $source
FriendlyName : C:\
RecoverySourceName : C:\
ServerName : myserver.microsoft.com

FriendlyName : D:\
RecoverySourceName : D:\
ServerName : myserver.microsoft.com
```

### <a name="choosing-a-backup-point-to-restore"></a><span data-ttu-id="27316-253">Výběr bodu zálohy obnovit</span><span class="sxs-lookup"><span data-stu-id="27316-253">Choosing a backup point to restore</span></span>
<span data-ttu-id="27316-254">Seznam body zálohy můžete načíst tak, že spustíte [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) rutiny s příslušnými parametry.</span><span class="sxs-lookup"><span data-stu-id="27316-254">The list of backup points can be retrieved by executing the [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdlet with appropriate parameters.</span></span> <span data-ttu-id="27316-255">V našem příkladu jsme vybrali nejnovější bod zálohy pro zdrojový svazek *D:* a použít ho k obnovení konkrétní soubor.</span><span class="sxs-lookup"><span data-stu-id="27316-255">In our example, we’ll choose the latest backup point for the source volume *D:* and use it to recover a specific file.</span></span>

```
PS C:> $rps = Get-OBRecoverableItem -Source $source[1]
IsDir : False
ItemNameFriendly : D:\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
LocalMountPoint : D:\
MountPointName : D:\
Name : D:\
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime :

IsDir : False
ItemNameFriendly : D:\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\
LocalMountPoint : D:\
MountPointName : D:\
Name : D:\
PointInTime : 17-Jun-15 6:31:31 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime :
```
<span data-ttu-id="27316-256">Objekt ```$rps``` je pole body zálohy.</span><span class="sxs-lookup"><span data-stu-id="27316-256">The object ```$rps``` is an array of backup points.</span></span> <span data-ttu-id="27316-257">První prvek je poslední bod a n-tou elementu bude nejstarší bod.</span><span class="sxs-lookup"><span data-stu-id="27316-257">The first element is the latest point and the Nth element is the oldest point.</span></span> <span data-ttu-id="27316-258">Zvolte nejnovější bod, použijeme ```$rps[0]```.</span><span class="sxs-lookup"><span data-stu-id="27316-258">To choose the latest point, we will use ```$rps[0]```.</span></span>

### <a name="choosing-an-item-to-restore"></a><span data-ttu-id="27316-259">Vybrat položku, kterou chcete obnovit</span><span class="sxs-lookup"><span data-stu-id="27316-259">Choosing an item to restore</span></span>
<span data-ttu-id="27316-260">K identifikaci přesný soubor nebo složku pro obnovení, použijte rekurzivně [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="27316-260">To identify the exact file or folder to restore, recursively use the [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdlet.</span></span> <span data-ttu-id="27316-261">Tímto způsobem lze procházet hierarchii složek výhradně pomocí ```Get-OBRecoverableItem```.</span><span class="sxs-lookup"><span data-stu-id="27316-261">That way the folder hierarchy can be browsed solely using the ```Get-OBRecoverableItem```.</span></span>

<span data-ttu-id="27316-262">V tomto příkladu, pokud chcete obnovit soubor *finances.xls* jsme může odkazovat, který používá objekt ```$filesFolders[1]```.</span><span class="sxs-lookup"><span data-stu-id="27316-262">In this example, if we want to restore the file *finances.xls* we can reference that using the object ```$filesFolders[1]```.</span></span>

```
PS C:> $filesFolders = Get-OBRecoverableItem $rps[0]
PS C:> $filesFolders
IsDir : True
ItemNameFriendly : D:\MyData\
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\
LocalMountPoint : D:\
MountPointName : D:\
Name : MyData
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize :
ItemLastModifiedTime : 15-Jun-15 8:49:29 AM

PS C:> $filesFolders = Get-OBRecoverableItem $filesFolders[0]
PS C:> $filesFolders
IsDir : False
ItemNameFriendly : D:\MyData\screenshot.oxps
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\screenshot.oxps
LocalMountPoint : D:\
MountPointName : D:\
Name : screenshot.oxps
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize : 228313
ItemLastModifiedTime : 21-Jun-14 6:45:09 AM

IsDir : False
ItemNameFriendly : D:\MyData\finances.xls
ItemNameGuid : \?\Volume{b835d359-a1dd-11e2-be72-2016d8d89f0f}\MyData\finances.xls
LocalMountPoint : D:\
MountPointName : D:\
Name : finances.xls
PointInTime : 18-Jun-15 6:41:52 AM
ServerName : myserver.microsoft.com
ItemSize : 96256
ItemLastModifiedTime : 21-Jun-14 6:43:02 AM
```

<span data-ttu-id="27316-263">Můžete také vyhledat položky k obnovení pomocí ```Get-OBRecoverableItem``` rutiny.</span><span class="sxs-lookup"><span data-stu-id="27316-263">You can also search for items to restore using the ```Get-OBRecoverableItem``` cmdlet.</span></span> <span data-ttu-id="27316-264">V našem příkladu, k vyhledání *finances.xls* nám může získat popisovač v souboru tak, že spustíte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="27316-264">In our example, to search for *finances.xls* we could get a handle on the file by running this command:</span></span>

```
PS C:\> $item = Get-OBRecoverableItem -RecoveryPoint $rps[0] -Location "D:\MyData" -SearchString "finance*"
```

### <a name="triggering-the-restore-process"></a><span data-ttu-id="27316-265">Spuštění procesu obnovení</span><span class="sxs-lookup"><span data-stu-id="27316-265">Triggering the restore process</span></span>
<span data-ttu-id="27316-266">K aktivaci v procesu obnovení, musíme nejprve nastavte možnosti obnovení.</span><span class="sxs-lookup"><span data-stu-id="27316-266">To trigger the restore process, we first need to specify the recovery options.</span></span> <span data-ttu-id="27316-267">To můžete provést pomocí [New-OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="27316-267">This can be done by using the [New-OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) cmdlet.</span></span> <span data-ttu-id="27316-268">Pro tento příklad předpokládejme, že chceme obnovit soubory *C:\temp*.</span><span class="sxs-lookup"><span data-stu-id="27316-268">For this example, let's assume that we want to restore the files to *C:\temp*.</span></span> <span data-ttu-id="27316-269">Také Předpokládejme, že chceme přeskočit soubory, které již existují na cílovou složku *C:\temp*.</span><span class="sxs-lookup"><span data-stu-id="27316-269">Let's also assume that we want to skip files that already exist on the destination folder *C:\temp*.</span></span> <span data-ttu-id="27316-270">Pokud chcete vytvořit možnost obnovení, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="27316-270">To create such a recovery option, use the following command:</span></span>

```
PS C:\> $recovery_option = New-OBRecoveryOption -DestinationPath "C:\temp" -OverwriteType Skip
```

<span data-ttu-id="27316-271">Teď spustit obnovení pomocí [Start-OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) příkaz na vybraném ```$item``` z výstupu ```Get-OBRecoverableItem``` rutiny:</span><span class="sxs-lookup"><span data-stu-id="27316-271">Now trigger restore by using the [Start-OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) command on the selected ```$item``` from the output of the ```Get-OBRecoverableItem``` cmdlet:</span></span>

```
PS C:\> Start-OBRecovery -RecoverableItem $item -RecoveryOption $recover_option
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Job completed.
The recovery operation completed successfully.
```


## <a name="uninstalling-the-azure-backup-agent"></a><span data-ttu-id="27316-272">Odinstalace agenta Azure Backup</span><span class="sxs-lookup"><span data-stu-id="27316-272">Uninstalling the Azure Backup agent</span></span>
<span data-ttu-id="27316-273">Odinstalace agenta Azure Backup lze provést pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="27316-273">Uninstalling the Azure Backup agent can be done by using the following command:</span></span>

```
PS C:\> .\MARSAgentInstaller.exe /d /q
```

<span data-ttu-id="27316-274">Odinstalace binárních souborů agenta z počítače má některé důsledky vzít v úvahu:</span><span class="sxs-lookup"><span data-stu-id="27316-274">Uninstalling the agent binaries from the machine has some consequences to consider:</span></span>

* <span data-ttu-id="27316-275">Odebere filtr souborů z počítače a sledování změn je zastavena.</span><span class="sxs-lookup"><span data-stu-id="27316-275">It removes the file-filter from the machine, and tracking of changes is stopped.</span></span>
* <span data-ttu-id="27316-276">Všechny informace o zásadách se odebere z počítače, ale informace o zásadách dále uložena ve službě.</span><span class="sxs-lookup"><span data-stu-id="27316-276">All policy information is removed from the machine, but the policy information continues to be stored in the service.</span></span>
* <span data-ttu-id="27316-277">Se odeberou všechny plány zálohování a jsou provedeny žádné další zálohy.</span><span class="sxs-lookup"><span data-stu-id="27316-277">All backup schedules are removed, and no further backups are taken.</span></span>

<span data-ttu-id="27316-278">Však data uložená v Azure zůstane a se uchovávají podle nastavení zásad uchovávání informací, které jste.</span><span class="sxs-lookup"><span data-stu-id="27316-278">However, the data stored in Azure remains and is retained as per the retention policy setup by you.</span></span> <span data-ttu-id="27316-279">Starší body jsou automaticky stará.</span><span class="sxs-lookup"><span data-stu-id="27316-279">Older points are automatically aged out.</span></span>

## <a name="remote-management"></a><span data-ttu-id="27316-280">Vzdálená správa</span><span class="sxs-lookup"><span data-stu-id="27316-280">Remote management</span></span>
<span data-ttu-id="27316-281">Veškerá správa, kolem agenta Azure Backup, zásady a zdroje dat lze provést vzdáleně pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="27316-281">All the management around the Azure Backup agent, policies, and data sources can be done remotely through PowerShell.</span></span> <span data-ttu-id="27316-282">Na počítač, který bude spravovat vzdáleně musí být správně připravena.</span><span class="sxs-lookup"><span data-stu-id="27316-282">The machine that will be managed remotely needs to be prepared correctly.</span></span>

<span data-ttu-id="27316-283">Ve výchozím nastavení je Služba WinRM nakonfigurován pro ruční spuštění.</span><span class="sxs-lookup"><span data-stu-id="27316-283">By default, the WinRM service is configured for manual startup.</span></span> <span data-ttu-id="27316-284">Typ spuštění musí být nastavený na *automatické* a musí být spuštěna.</span><span class="sxs-lookup"><span data-stu-id="27316-284">The startup type must be set to *Automatic* and the service should be started.</span></span> <span data-ttu-id="27316-285">Pokud chcete ověřit, že Služba WinRM je spuštěná, by měla být hodnota vlastnosti stav *systémem*.</span><span class="sxs-lookup"><span data-stu-id="27316-285">To verify that the WinRM service is running, the value of the Status property should be *Running*.</span></span>

```
PS C:\> Get-Service WinRM

Status   Name               DisplayName
------   ----               -----------
Running  winrm              Windows Remote Management (WS-Manag...
```

<span data-ttu-id="27316-286">Prostředí PowerShell by měl být nakonfigurovaný pro vzdálenou komunikaci.</span><span class="sxs-lookup"><span data-stu-id="27316-286">PowerShell should be configured for remoting.</span></span>

```
PS C:\> Enable-PSRemoting -force
WinRM is already set up to receive requests on this computer.
WinRM has been updated for remote management.
WinRM firewall exception enabled.

PS C:\> Set-ExecutionPolicy unrestricted -force
```

<span data-ttu-id="27316-287">Tento počítač teď můžete spravovat vzdáleně – od instalace agenta.</span><span class="sxs-lookup"><span data-stu-id="27316-287">The machine can now be managed remotely - starting from the installation of the agent.</span></span> <span data-ttu-id="27316-288">Například následující skript zkopíruje agenta ke vzdálenému počítači a ho nainstaluje.</span><span class="sxs-lookup"><span data-stu-id="27316-288">For example, the following script copies the agent to the remote machine and installs it.</span></span>

```
PS C:\> $dloc = "\\REMOTESERVER01\c$\Windows\Temp"
PS C:\> $agent = "\\REMOTESERVER01\c$\Windows\Temp\MARSAgentInstaller.exe"
PS C:\> $args = "/q"
PS C:\> Copy-Item "C:\Downloads\MARSAgentInstaller.exe" -Destination $dloc - force

PS C:\> $s = New-PSSession -ComputerName REMOTESERVER01
PS C:\> Invoke-Command -Session $s -Script { param($d, $a) Start-Process -FilePath $d $a -Wait } -ArgumentList $agent $args
```

## <a name="next-steps"></a><span data-ttu-id="27316-289">Další kroky</span><span class="sxs-lookup"><span data-stu-id="27316-289">Next steps</span></span>
<span data-ttu-id="27316-290">Další informace o Azure Backup pro Windows Server nebo klienta najdete v tématu</span><span class="sxs-lookup"><span data-stu-id="27316-290">For more information about Azure Backup for Windows Server/Client see</span></span>

* [<span data-ttu-id="27316-291">Seznámení s Azure Backup</span><span class="sxs-lookup"><span data-stu-id="27316-291">Introduction to Azure Backup</span></span>](backup-introduction-to-azure-backup.md)
* [<span data-ttu-id="27316-292">Zálohování serverů Windows</span><span class="sxs-lookup"><span data-stu-id="27316-292">Back up Windows Servers</span></span>](backup-configure-vault.md)
