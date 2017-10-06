---
title: "aaaUse prostředí PowerShell toomanage systému Windows Server záloh v Azure | Microsoft Docs"
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
ms.openlocfilehash: 72292e510b0f059102440bd49a195be4ef700a6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-backup-tooazure-for-windows-serverwindows-client-using-powershell"></a><span data-ttu-id="b0bf0-103">Nasazení a správě zálohování tooAzure pro klienta systému Windows Server a Windows pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="b0bf0-103">Deploy and manage backup tooAzure for Windows Server/Windows Client using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b0bf0-104">ARM</span><span class="sxs-lookup"><span data-stu-id="b0bf0-104">ARM</span></span>](backup-client-automation.md)
> * [<span data-ttu-id="b0bf0-105">Classic</span><span class="sxs-lookup"><span data-stu-id="b0bf0-105">Classic</span></span>](backup-client-automation-classic.md)
>
>

<span data-ttu-id="b0bf0-106">Tento článek vysvětluje, jak zálohovat toouse tooback prostředí PowerShell systému Windows Server nebo Windows pracovní stanice data tooa trezoru.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-106">This article explains how toouse PowerShell tooback up Windows Server or Windows workstation data tooa backup vault.</span></span> <span data-ttu-id="b0bf0-107">Společnost Microsoft doporučuje používat trezory služeb zotavení pro všechna nová nasazení.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-107">Microsoft recommends using Recovery Services vaults for all new deployments.</span></span> <span data-ttu-id="b0bf0-108">Pokud jsou nového uživatele Azure Backup a nevytvořili trezor záloh v rámci vašeho předplatného, použijte hello článku [nasadit a spravovat tooAzure data Data Protection Manager pomocí prostředí PowerShell](backup-client-automation.md) tak data ukládáte do trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-108">If you are a new Azure Backup user and have not created a backup vault in your subscription, use hello article, [Deploy and manage Data Protection Manager data tooAzure using PowerShell](backup-client-automation.md) so you store your data in a Recovery Services vault.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="b0bf0-109">Teď můžete upgradovat vaše trezory služeb tooRecovery trezory Backup.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-109">You can now upgrade your Backup vaults tooRecovery Services vaults.</span></span> <span data-ttu-id="b0bf0-110">Podrobnosti najdete v tématu hello článku [upgradu tooa trezoru zálohování trezor služeb zotavení](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="b0bf0-110">For details, see hello article [Upgrade a Backup vault tooa Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="b0bf0-111">Společnost Microsoft doporučuje tooupgrade zálohování trezory tooRecovery trezory služeb.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-111">Microsoft encourages you tooupgrade your Backup vaults tooRecovery Services vaults.</span></span><br/> <span data-ttu-id="b0bf0-112">Po 15 říjen 2017 nemůžete použít trezory Backup toocreate prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-112">After October 15, 2017, you can’t use PowerShell toocreate Backup vaults.</span></span> <span data-ttu-id="b0bf0-113">**Do 1. listopadu 2017:**</span><span class="sxs-lookup"><span data-stu-id="b0bf0-113">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="b0bf0-114">Všechny zbývající trezory Backup bude automaticky upgradovaný tooRecovery trezory služeb.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-114">All remaining Backup vaults will be automatically upgraded tooRecovery Services vaults.</span></span>
>- <span data-ttu-id="b0bf0-115">Můžete nebudou moct tooaccess zálohovaných dat na portálu classic hello.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-115">You won't be able tooaccess your backup data in hello classic portal.</span></span> <span data-ttu-id="b0bf0-116">Místo toho použijte hello Azure portálu tooaccess zálohovaných dat v trezory služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-116">Instead, use hello Azure portal tooaccess your backup data in Recovery Services vaults.</span></span>
>

## <a name="install-azure-powershell"></a><span data-ttu-id="b0bf0-117">Instalace prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="b0bf0-117">Install Azure PowerShell</span></span>
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

<span data-ttu-id="b0bf0-118">Října 2015 byla vydána Azure PowerShell 1.0.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-118">In October 2015, Azure PowerShell 1.0 was released.</span></span> <span data-ttu-id="b0bf0-119">Tato verze byla úspěšná hello 0.9.8 verze a uvést do režimu o významné změny, zejména v vzoru pro pojmenovávání hello rutin hello.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-119">This release succeeded hello 0.9.8 release and brought about some significant changes, especially in hello naming pattern of hello cmdlets.</span></span> <span data-ttu-id="b0bf0-120">postupujte podle 1.0 rutiny hello formát pojmenování {sloveso}-AzureRm {podstatné jméno}; vzhledem k tomu, názvy hello 0.9.8 neobsahují **Rm** (třeba New-AzureRmResourceGroup místo New-AzureResourceGroup).</span><span class="sxs-lookup"><span data-stu-id="b0bf0-120">1.0 cmdlets follow hello naming pattern {verb}-AzureRm{noun}; whereas, hello 0.9.8 names do not include **Rm** (for example, New-AzureRmResourceGroup instead of New-AzureResourceGroup).</span></span> <span data-ttu-id="b0bf0-121">Pokud používáte prostředí Azure PowerShell 0.9.8, musíte nejdřív povolit režimu Resource Manager hello spuštěním hello **Switch-AzureMode AzureResourceManager** příkaz.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-121">When using Azure PowerShell 0.9.8, you must first enable hello Resource Manager mode by running hello **Switch-AzureMode AzureResourceManager** command.</span></span> <span data-ttu-id="b0bf0-122">Tento příkaz není nutné v 1.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-122">This command is not necessary in 1.0 or later.</span></span>

<span data-ttu-id="b0bf0-123">Pokud chcete toouse skripty napsané pro prostředí hello 0.9.8, v hello 1.0 nebo novější prostředí, měli byste pečlivě otestovat hello skripty v předprodukčním prostředí než je použijete v produkčním tooavoid neočekávané dopad.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-123">If you want toouse your scripts written for hello 0.9.8 environment, in hello 1.0 or later environment, you should carefully test hello scripts in a pre-production environment before using them in production tooavoid unexpected impact.</span></span>

<span data-ttu-id="b0bf0-124">[Stáhněte si nejnovější verzi prostředí PowerShell hello](https://github.com/Azure/azure-powershell/releases) (minimální požadovaná verze je: 1.0.0)</span><span class="sxs-lookup"><span data-stu-id="b0bf0-124">[Download hello latest PowerShell release](https://github.com/Azure/azure-powershell/releases) (minimum version required is : 1.0.0)</span></span>

[!INCLUDE [arm-getting-setup-powershell](../../includes/arm-getting-setup-powershell.md)]

## <a name="create-a-backup-vault"></a><span data-ttu-id="b0bf0-125">Vytvoření trezoru záloh</span><span class="sxs-lookup"><span data-stu-id="b0bf0-125">Create a backup vault</span></span>
> [!WARNING]
> <span data-ttu-id="b0bf0-126">Pro zákazníky používající Azure Backup pro hello poprvé budete potřebovat tooregister hello Azure Backup zprostředkovatele toobe používat s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-126">For customers using Azure Backup for hello first time, you need tooregister hello Azure Backup provider toobe used with your subscription.</span></span> <span data-ttu-id="b0bf0-127">To lze provést tak, že spustíte následující příkaz hello: Register-AzureProvider - ProviderNamespace "Microsoft.Backup"</span><span class="sxs-lookup"><span data-stu-id="b0bf0-127">This can be done by running hello following command: Register-AzureProvider -ProviderNamespace "Microsoft.Backup"</span></span>
>
>

<span data-ttu-id="b0bf0-128">Můžete vytvořit nové úložiště záloh pomocí hello **New-AzureRMBackupVault** rutiny.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-128">You can create a new backup vault using hello **New-AzureRMBackupVault** cmdlet.</span></span> <span data-ttu-id="b0bf0-129">Trezor záloh Hello je prostředek ARM, proto musíte tooplace ji ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-129">hello backup vault is an ARM resource, so you need tooplace it within a Resource Group.</span></span> <span data-ttu-id="b0bf0-130">V prostředí Azure PowerShell konzolu se zvýšenými oprávněními spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="b0bf0-130">In an elevated Azure PowerShell console, run hello following commands:</span></span>

```
PS C:\> New-AzureResourceGroup –Name “test-rg” -Region “West US”
PS C:\> $backupvault = New-AzureRMBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GeoRedundant
```

<span data-ttu-id="b0bf0-131">Použití hello **Get-AzureRMBackupVault** úložiště záloh hello toolist rutiny v předplatném.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-131">Use hello **Get-AzureRMBackupVault** cmdlet toolist hello backup vaults in a subscription.</span></span>

## <a name="installing-hello-azure-backup-agent"></a><span data-ttu-id="b0bf0-132">Instalace agenta Azure Backup hello</span><span class="sxs-lookup"><span data-stu-id="b0bf0-132">Installing hello Azure Backup agent</span></span>
<span data-ttu-id="b0bf0-133">Před instalací agenta Azure Backup hello, musíte instalační program toohave hello stažené a na serveru Windows hello.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-133">Before you install hello Azure Backup agent, you need toohave hello installer downloaded and present on hello Windows Server.</span></span> <span data-ttu-id="b0bf0-134">Můžete získat nejnovější verzi instalačního programu hello hello hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) nebo ze stránky řídicí panel hello úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-134">You can get hello latest version of hello installer from hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) or from hello backup vault's Dashboard page.</span></span> <span data-ttu-id="b0bf0-135">Uložení instalačního programu hello tooan snadno dostupné místo jako * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-135">Save hello installer tooan easily accessible location like *C:\Downloads\*.</span></span>

<span data-ttu-id="b0bf0-136">tooinstall hello agenta, spusťte následující příkaz v konzolu prostředí PowerShell se zvýšenými oprávněními hello:</span><span class="sxs-lookup"><span data-stu-id="b0bf0-136">tooinstall hello agent, run hello following command in an elevated PowerShell console:</span></span>

```
PS C:\> MARSAgentInstaller.exe /q
```

<span data-ttu-id="b0bf0-137">Tím se nainstaluje hello agent se všemi možnostmi výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-137">This installs hello agent with all hello default options.</span></span> <span data-ttu-id="b0bf0-138">Hello instalace trvá několik minut v pozadí hello.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-138">hello installation takes a few minutes in hello background.</span></span> <span data-ttu-id="b0bf0-139">Pokud nezadáte hello */nu* možnost pak hello **Windows Update** otevře se okno na konci hello hello toocheck instalace pro všechny aktualizace.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-139">If you do not specify hello */nu* option then hello **Windows Update** window will open at hello end of hello installation toocheck for any updates.</span></span> <span data-ttu-id="b0bf0-140">Po instalaci agenta hello se zobrazí v seznamu nainstalovaných programů hello.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-140">Once installed, hello agent will show in hello list of installed programs.</span></span>

<span data-ttu-id="b0bf0-141">toosee hello seznamu nainstalovaných programů, přejděte příliš**ovládací panely** > **programy** > **programy a funkce**.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-141">toosee hello list of installed programs, go too**Control Panel** > **Programs** > **Programs and Features**.</span></span>

![Instalaci agenta](./media/backup-client-automation/installed-agent-listing.png)

### <a name="installation-options"></a><span data-ttu-id="b0bf0-143">Možnosti instalace</span><span class="sxs-lookup"><span data-stu-id="b0bf0-143">Installation options</span></span>
<span data-ttu-id="b0bf0-144">toosee, které všechny možnosti, které jsou k dispozici prostřednictvím hello hello příkazového řádku, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="b0bf0-144">toosee all hello options available via hello command-line, use hello following command:</span></span>

```
PS C:\> MARSAgentInstaller.exe /?
```

<span data-ttu-id="b0bf0-145">Hello dostupné možnosti patří:</span><span class="sxs-lookup"><span data-stu-id="b0bf0-145">hello available options include:</span></span>

| <span data-ttu-id="b0bf0-146">Možnost</span><span class="sxs-lookup"><span data-stu-id="b0bf0-146">Option</span></span> | <span data-ttu-id="b0bf0-147">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="b0bf0-147">Details</span></span> | <span data-ttu-id="b0bf0-148">Výchozí</span><span class="sxs-lookup"><span data-stu-id="b0bf0-148">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b0bf0-149">/q</span><span class="sxs-lookup"><span data-stu-id="b0bf0-149">/q</span></span> |<span data-ttu-id="b0bf0-150">Tichá instalace</span><span class="sxs-lookup"><span data-stu-id="b0bf0-150">Quiet installation</span></span> |- |
| <span data-ttu-id="b0bf0-151">/ p: "umístění"</span><span class="sxs-lookup"><span data-stu-id="b0bf0-151">/p:"location"</span></span> |<span data-ttu-id="b0bf0-152">Cesta toohello instalační složku pro agenta Azure Backup hello.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-152">Path toohello installation folder for hello Azure Backup agent.</span></span> |<span data-ttu-id="b0bf0-153">C:\Program Files\Microsoft Azure Recovery Services agenta</span><span class="sxs-lookup"><span data-stu-id="b0bf0-153">C:\Program Files\Microsoft Azure Recovery Services Agent</span></span> |
| <span data-ttu-id="b0bf0-154">/ s: "umístění"</span><span class="sxs-lookup"><span data-stu-id="b0bf0-154">/s:"location"</span></span> |<span data-ttu-id="b0bf0-155">Složka mezipaměti toohello cestu pro agenta Azure Backup hello.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-155">Path toohello cache folder for hello Azure Backup agent.</span></span> |<span data-ttu-id="b0bf0-156">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span><span class="sxs-lookup"><span data-stu-id="b0bf0-156">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span></span> |
| <span data-ttu-id="b0bf0-157">/m</span><span class="sxs-lookup"><span data-stu-id="b0bf0-157">/m</span></span> |<span data-ttu-id="b0bf0-158">Výslovný souhlas tooMicrosoft aktualizace</span><span class="sxs-lookup"><span data-stu-id="b0bf0-158">Opt-in tooMicrosoft Update</span></span> |- |
| <span data-ttu-id="b0bf0-159">/nu</span><span class="sxs-lookup"><span data-stu-id="b0bf0-159">/nu</span></span> |<span data-ttu-id="b0bf0-160">Nekontrolovat aktualizace po dokončení instalace</span><span class="sxs-lookup"><span data-stu-id="b0bf0-160">Do not Check for updates after installation is complete</span></span> |- |
| <span data-ttu-id="b0bf0-161">/d</span><span class="sxs-lookup"><span data-stu-id="b0bf0-161">/d</span></span> |<span data-ttu-id="b0bf0-162">Odinstaluje Agenta Microsoft Azure Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-162">Uninstalls Microsoft Azure Recovery Services Agent</span></span> |- |
| <span data-ttu-id="b0bf0-163">/pH</span><span class="sxs-lookup"><span data-stu-id="b0bf0-163">/ph</span></span> |<span data-ttu-id="b0bf0-164">Adresa proxy hostitele</span><span class="sxs-lookup"><span data-stu-id="b0bf0-164">Proxy Host Address</span></span> |- |
| <span data-ttu-id="b0bf0-165">/Po</span><span class="sxs-lookup"><span data-stu-id="b0bf0-165">/po</span></span> |<span data-ttu-id="b0bf0-166">Číslo portu proxy hostitele</span><span class="sxs-lookup"><span data-stu-id="b0bf0-166">Proxy Host Port Number</span></span> |- |
| <span data-ttu-id="b0bf0-167">/Pu</span><span class="sxs-lookup"><span data-stu-id="b0bf0-167">/pu</span></span> |<span data-ttu-id="b0bf0-168">Uživatelské jméno proxy hostitele</span><span class="sxs-lookup"><span data-stu-id="b0bf0-168">Proxy Host UserName</span></span> |- |
| <span data-ttu-id="b0bf0-169">/pW</span><span class="sxs-lookup"><span data-stu-id="b0bf0-169">/pw</span></span> |<span data-ttu-id="b0bf0-170">Heslo pro proxy server</span><span class="sxs-lookup"><span data-stu-id="b0bf0-170">Proxy Password</span></span> |- |

## <a name="registering-with-hello-azure-backup-service"></a><span data-ttu-id="b0bf0-171">Registrace hello služby zálohování Azure</span><span class="sxs-lookup"><span data-stu-id="b0bf0-171">Registering with hello Azure Backup service</span></span>
<span data-ttu-id="b0bf0-172">Než můžete zaregistrovat s hello služby zálohování Azure, je třeba tooensure této hello [požadavky](backup-configure-vault.md) jsou splněny.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-172">Before you can register with hello Azure Backup service, you need tooensure that hello [prerequisites](backup-configure-vault.md) are met.</span></span> <span data-ttu-id="b0bf0-173">Postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="b0bf0-173">You must:</span></span>

* <span data-ttu-id="b0bf0-174">Máte platné předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="b0bf0-174">Have a valid Azure subscription</span></span>
* <span data-ttu-id="b0bf0-175">Mít úložiště záloh</span><span class="sxs-lookup"><span data-stu-id="b0bf0-175">Have a backup vault</span></span>

<span data-ttu-id="b0bf0-176">toodownload hello přihlašovací údaje trezoru, spusťte hello **Get-AzureRMBackupVaultCredentials** rutiny do konzoly Azure PowerShell a úložiště do vhodného umístění, například * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-176">toodownload hello vault credentials, run hello **Get-AzureRMBackupVaultCredentials** cmdlet in an Azure PowerShell console and store it in a convenient location like *C:\Downloads\*.</span></span>

```
PS C:\> $credspath = "C:\"
PS C:\> $credsfilename = Get-AzureRMBackupVaultCredentials -Vault $backupvault -TargetLocation $credspath
PS C:\> $credsfilename
f5303a0b-fae4-4cdb-b44d-0e4c032dde26_backuprg_backuprn_2015-08-11--06-22-35.VaultCredentials
```

<span data-ttu-id="b0bf0-177">Registrace počítače hello hello trezoru se provádí pomocí hello [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) rutiny:</span><span class="sxs-lookup"><span data-stu-id="b0bf0-177">Registering hello machine with hello vault is done using hello [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) cmdlet:</span></span>

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
> <span data-ttu-id="b0bf0-178">Nepoužívejte soubor s relativní cesty toospecify hello přihlašovacími údaji.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-178">Do not use relative paths toospecify hello vault credentials file.</span></span> <span data-ttu-id="b0bf0-179">Musíte zadat absolutní cestu jako vstupní toohello rutiny.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-179">You must provide an absolute path as an input toohello cmdlet.</span></span>
>
>

## <a name="networking-settings"></a><span data-ttu-id="b0bf0-180">Nastavení sítě</span><span class="sxs-lookup"><span data-stu-id="b0bf0-180">Networking settings</span></span>
<span data-ttu-id="b0bf0-181">Při připojení hello hello Windows počítač toohello je Internetu prostřednictvím proxy serveru, nastavení proxy serveru hello se dá zadat i toohello agenta.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-181">When hello connectivity of hello Windows machine toohello internet is through a proxy server, hello proxy settings can also be provided toohello agent.</span></span> <span data-ttu-id="b0bf0-182">V tomto příkladu neexistuje žádný proxy server, takže jsme jsou explicitně vymazání údaje související s proxy serverem.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-182">In this example, there is no proxy server, so we are explicitly clearing any proxy-related information.</span></span>

<span data-ttu-id="b0bf0-183">Využití šířky pásma také lze kontrolovat pomocí možnosti hello ```work hour bandwidth``` a ```non-work hour bandwidth``` pro danou sadu dny v týdnu hello.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-183">Bandwidth usage can also be controlled with hello options of ```work hour bandwidth``` and ```non-work hour bandwidth``` for a given set of days of hello week.</span></span>

<span data-ttu-id="b0bf0-184">Nastavení serveru proxy a šířky pásma podrobnosti hello se provádí pomocí hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) rutiny:</span><span class="sxs-lookup"><span data-stu-id="b0bf0-184">Setting hello proxy and bandwidth details is done using hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) cmdlet:</span></span>

```
PS C:\> Set-OBMachineSetting -NoProxy
Server properties updated successfully.

PS C:\> Set-OBMachineSetting -NoThrottle
Server properties updated successfully.
```

## <a name="encryption-settings"></a><span data-ttu-id="b0bf0-185">Nastavení šifrování</span><span class="sxs-lookup"><span data-stu-id="b0bf0-185">Encryption settings</span></span>
<span data-ttu-id="b0bf0-186">zálohování dat odesílaných tooAzure Hello zálohování je šifrovaný tooprotect hello důvěrnost dat hello.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-186">hello backup data sent tooAzure Backup is encrypted tooprotect hello confidentiality of hello data.</span></span> <span data-ttu-id="b0bf0-187">šifrovací přístupové heslo Hello jsou hello "password" toodecrypt hello data v době obnovení hello.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-187">hello encryption passphrase is hello "password" toodecrypt hello data at hello time of restore.</span></span>

```
PS C:\> ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force | Set-OBMachineSetting
Server properties updated successfully
```

> [!IMPORTANT]
> <span data-ttu-id="b0bf0-188">Zachování informací o přístupové heslo hello bezpečném, po nastavení.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-188">Keep hello passphrase information safe and secure once it is set.</span></span> <span data-ttu-id="b0bf0-189">Nebudete moct toorestore dat z Azure bez tohoto hesla.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-189">You will not be able toorestore data from Azure without this passphrase.</span></span>
>
>

## <a name="back-up-files-and-folders"></a><span data-ttu-id="b0bf0-190">Zálohování souborů a složek</span><span class="sxs-lookup"><span data-stu-id="b0bf0-190">Back up files and folders</span></span>
<span data-ttu-id="b0bf0-191">Zásady se řídí všech záloh z Windows serverů a klientů tooAzure zálohování.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-191">All your backups from Windows Servers and clients tooAzure Backup are governed by a policy.</span></span> <span data-ttu-id="b0bf0-192">Hello zásad se skládá ze tří částí:</span><span class="sxs-lookup"><span data-stu-id="b0bf0-192">hello policy comprises three parts:</span></span>

1. <span data-ttu-id="b0bf0-193">A **plán zálohování** určující, kdy zálohování potřebovat toobe provést a synchronizovat se službou hello.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-193">A **backup schedule** that specifies when backups need toobe taken and synchronized with hello service.</span></span>
2. <span data-ttu-id="b0bf0-194">A **plán uchovávání informací** určující, jak dlouho body obnovení hello tooretain v Azure.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-194">A **retention schedule** that specifies how long tooretain hello recovery points in Azure.</span></span>
3. <span data-ttu-id="b0bf0-195">A **zadání zahrnutí nebo vyloučení souboru** který určuje, co by měl být zálohovány.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-195">A **file inclusion/exclusion specification** that dictates what should be backed up.</span></span>

<span data-ttu-id="b0bf0-196">V tomto dokumentu protože jsme se automatizaci zálohování, budete předpokládáme, že nic nebyl nakonfigurován.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-196">In this document, since we're automating backup, we'll assume nothing has been configured.</span></span> <span data-ttu-id="b0bf0-197">Začneme vytvořením nové zásady zálohování pomocí hello [New-OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) rutiny a jeho použití.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-197">We begin by creating a new backup policy using hello [New-OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) cmdlet and using it.</span></span>

```
PS C:\> $newpolicy = New-OBPolicy
```

<span data-ttu-id="b0bf0-198">V tento čas hello je prázdné zásady a jiné rutiny jsou potřebné toodefine co položky budou zahrnout nebo vyloučit, při zálohování spustí a kde hello zálohy budou uloženy.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-198">At this time hello policy is empty and other cmdlets are needed toodefine what items will be included or excluded, when backups will run, and where hello backups will be stored.</span></span>

### <a name="configuring-hello-backup-schedule"></a><span data-ttu-id="b0bf0-199">Konfigurace plánu zálohování hello</span><span class="sxs-lookup"><span data-stu-id="b0bf0-199">Configuring hello backup schedule</span></span>
<span data-ttu-id="b0bf0-200">nejprve Hello hello 3 části zásady je plán zálohování hello, který je vytvořený pomocí hello [New-OBSchedule](https://technet.microsoft.com/library/hh770401) rutiny.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-200">hello first of hello 3 parts of a policy is hello backup schedule, which is created using hello [New-OBSchedule](https://technet.microsoft.com/library/hh770401) cmdlet.</span></span> <span data-ttu-id="b0bf0-201">plán zálohování Hello definuje při zálohování potřebovat toobe provedena.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-201">hello backup schedule defines when backups need toobe taken.</span></span> <span data-ttu-id="b0bf0-202">Při vytváření plánu je třeba toospecify 2 vstupní parametry:</span><span class="sxs-lookup"><span data-stu-id="b0bf0-202">When creating a schedule you need toospecify 2 input parameters:</span></span>

* <span data-ttu-id="b0bf0-203">**Počet dnů v týdnu hello** zálohování hello měly být spuštěny.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-203">**Days of hello week** that hello backup should run.</span></span> <span data-ttu-id="b0bf0-204">Můžete spustit úlohy zálohování hello pouze jeden den nebo každý den v týdnu hello nebo libovolnou kombinaci mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-204">You can run hello backup job on just one day, or every day of hello week, or any combination in between.</span></span>
* <span data-ttu-id="b0bf0-205">**Hello denní dobu** spuštění zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-205">**Times of hello day** when hello backup should run.</span></span> <span data-ttu-id="b0bf0-206">Můžete definovat až too3 různou dobu hello, když se spustí zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-206">You can define up too3 different times of hello day when hello backup will be triggered.</span></span>

<span data-ttu-id="b0bf0-207">Například může nakonfigurovat zásady zálohování, který se spustí na 16: 00 každou sobotu a neděli.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-207">For instance, you could configure a backup policy that runs at 4PM every Saturday and Sunday.</span></span>

```
PS C:\> $sched = New-OBSchedule -DaysofWeek Saturday, Sunday -TimesofDay 16:00
```

<span data-ttu-id="b0bf0-208">plán zálohování Hello potřebuje toobe přidružené k zásadě a toho lze dosáhnout pomocí hello [Set-OBSchedule](https://technet.microsoft.com/library/hh770407) rutiny.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-208">hello backup schedule needs toobe associated with a policy, and this can be achieved by using hello [Set-OBSchedule](https://technet.microsoft.com/library/hh770407) cmdlet.</span></span>

```
PS C:> Set-OBSchedule -Policy $newpolicy -Schedule $sched
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s) DsList : PolicyName : RetentionPolicy : State : New PolicyState : Valid
```
### <a name="configuring-a-retention-policy"></a><span data-ttu-id="b0bf0-209">Konfigurace zásad uchovávání</span><span class="sxs-lookup"><span data-stu-id="b0bf0-209">Configuring a retention policy</span></span>
<span data-ttu-id="b0bf0-210">zásady uchovávání informací Hello definuje, jak dlouho obnovení, které se uchovají body vytvořené z úlohy zálohování.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-210">hello retention policy defines how long recovery points created from backup jobs are retained.</span></span> <span data-ttu-id="b0bf0-211">Při vytváření nových zásad uchovávání informací pomocí hello [New-OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) rutiny, můžete zadat třeba hello počet dní, které hello body obnovení záloh toobe uchovávají s Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-211">When creating a new retention policy using hello [New-OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) cmdlet, you can specify hello number of days that hello backup recovery points need toobe retained with Azure Backup.</span></span> <span data-ttu-id="b0bf0-212">Následující příklad Hello nastaví zásady uchovávání informací 7 dní.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-212">hello example below sets a retention policy of 7 days.</span></span>

```
PS C:\> $retentionpolicy = New-OBRetentionPolicy -RetentionDays 7
```

<span data-ttu-id="b0bf0-213">Hello zásady uchovávání informací musí být přidruženy k hlavní zásady hello pomocí rutiny hello [Set-OBRetentionPolicy](https://technet.microsoft.com/library/hh770405):</span><span class="sxs-lookup"><span data-stu-id="b0bf0-213">hello retention policy must be associated with hello main policy using hello cmdlet [Set-OBRetentionPolicy](https://technet.microsoft.com/library/hh770405):</span></span>

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
### <a name="including-and-excluding-files-toobe-backed-up"></a><span data-ttu-id="b0bf0-214">Zahrnutí a vyloučení souborů toobe zálohovat</span><span class="sxs-lookup"><span data-stu-id="b0bf0-214">Including and excluding files toobe backed up</span></span>
<span data-ttu-id="b0bf0-215">```OBFileSpec``` Objekt definuje hello soubory toobe zahrnout a vyloučit zálohu.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-215">An ```OBFileSpec``` object defines hello files toobe included and excluded in a backup.</span></span> <span data-ttu-id="b0bf0-216">Toto je sada pravidel, že obor se hello chráněné soubory a složky na počítači.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-216">This is a set of rules that scope out hello protected files and folders on a machine.</span></span> <span data-ttu-id="b0bf0-217">Můžete mít mnoho souboru pravidla zahrnutí nebo vyloučení podle potřeby a přidružovat je k zásadě.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-217">You can have as many file inclusion or exclusion rules as required, and associate them with a policy.</span></span> <span data-ttu-id="b0bf0-218">Při vytváření nového objektu OBFileSpec, můžete:</span><span class="sxs-lookup"><span data-stu-id="b0bf0-218">When creating a new OBFileSpec object, you can:</span></span>

* <span data-ttu-id="b0bf0-219">Zadejte soubory a složky toobe hello zahrnuté</span><span class="sxs-lookup"><span data-stu-id="b0bf0-219">Specify hello files and folders toobe included</span></span>
* <span data-ttu-id="b0bf0-220">Zadejte soubory a složky toobe hello vyloučené</span><span class="sxs-lookup"><span data-stu-id="b0bf0-220">Specify hello files and folders toobe excluded</span></span>
* <span data-ttu-id="b0bf0-221">Zadejte rekurzivní zálohování dat v složku (nebo) zda pouze hello nejvyšší úrovně soubory v zadané složce hello by měl být zálohuje nahoru.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-221">Specify recursive backup of data in a folder (or) whether only hello top-level files in hello specified folder should be backed up.</span></span>

<span data-ttu-id="b0bf0-222">Hello pozdější se dosáhne použitím příznaku - nerekurzivní hello v příkazu New-OBFileSpec hello.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-222">hello latter is achieved by using hello -NonRecursive flag in hello New-OBFileSpec command.</span></span>

<span data-ttu-id="b0bf0-223">V příkladu hello níže jsme budete zálohování svazku C: a D: a vyloučit hello binární soubory operačního systému ve složce Windows hello a všechny dočasné složky.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-223">In hello example below, we'll back up volume C: and D: and exclude hello OS binaries in hello Windows folder and any temporary folders.</span></span> <span data-ttu-id="b0bf0-224">toodo, vytvoříme dvě specifikace souborů pomocí hello [New-OBFileSpec](https://technet.microsoft.com/library/hh770408) rutiny – jeden pro zahrnutí a jeden pro vyloučení.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-224">toodo so we'll create two file specifications using hello [New-OBFileSpec](https://technet.microsoft.com/library/hh770408) cmdlet - one for inclusion and one for exclusion.</span></span> <span data-ttu-id="b0bf0-225">Po vytvoření specifikace hello souborů jsou přidružené zásady hello použitím hello [přidat OBFileSpec](https://technet.microsoft.com/library/hh770424) rutiny.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-225">Once hello file specifications have been created, they're associated with hello policy using hello [Add-OBFileSpec](https://technet.microsoft.com/library/hh770424) cmdlet.</span></span>

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

### <a name="applying-hello-policy"></a><span data-ttu-id="b0bf0-226">Použití zásad hello</span><span class="sxs-lookup"><span data-stu-id="b0bf0-226">Applying hello policy</span></span>
<span data-ttu-id="b0bf0-227">Teď objektu zásad hello dokončení a má přidružené plánu zálohování, zásady uchovávání informací a seznam souborů zahrnutí nebo vyloučení.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-227">Now hello policy object is complete and has an associated backup schedule, retention policy, and an inclusion/exclusion list of files.</span></span> <span data-ttu-id="b0bf0-228">Tato zásada může být nyní potvrdit pro toouse Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-228">This policy can now be committed for Azure Backup toouse.</span></span> <span data-ttu-id="b0bf0-229">Před použitím hello nově vytvořený zásad zajistěte, aby žádné existující zásady zálohování, které jsou přidružené k serveru hello pomocí hello [odebrat OBPolicy](https://technet.microsoft.com/library/hh770415) rutiny.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-229">Before you apply hello newly created policy ensure that there are no existing backup policies associated with hello server by using hello [Remove-OBPolicy](https://technet.microsoft.com/library/hh770415) cmdlet.</span></span> <span data-ttu-id="b0bf0-230">Odebírání hello zásady zobrazí výzvu k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-230">Removing hello policy will prompt for confirmation.</span></span> <span data-ttu-id="b0bf0-231">potvrzení hello tooskip použít hello ```-Confirm:$false``` příznak pomocí rutiny hello.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-231">tooskip hello confirmation use hello ```-Confirm:$false``` flag with hello cmdlet.</span></span>

```
PS C:> Get-OBPolicy | Remove-OBPolicy
Microsoft Azure Backup Are you sure you want tooremove this backup policy? This will delete all hello backed up data. [Y] Yes [A] Yes tooAll [N] No [L] No tooAll [S] Suspend [?] Help (default is "Y"):
```

<span data-ttu-id="b0bf0-232">Objekt zásad spouštění hello se provádí pomocí hello [Set-OBPolicy](https://technet.microsoft.com/library/hh770421) rutiny.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-232">Committing hello policy object is done using hello [Set-OBPolicy](https://technet.microsoft.com/library/hh770421) cmdlet.</span></span> <span data-ttu-id="b0bf0-233">Také to požádá o potvrzení.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-233">This will also ask for confirmation.</span></span> <span data-ttu-id="b0bf0-234">potvrzení hello tooskip použít hello ```-Confirm:$false``` příznak pomocí rutiny hello.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-234">tooskip hello confirmation use hello ```-Confirm:$false``` flag with hello cmdlet.</span></span>

```
PS C:> Set-OBPolicy -Policy $newpolicy
Microsoft Azure Backup Do you want toosave this backup policy ? [Y] Yes [A] Yes tooAll [N] No [L] No tooAll [S] Suspend [?] Help (default is "Y"):
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

<span data-ttu-id="b0bf0-235">Můžete zobrazit podrobnosti o hello hello existující zásady zálohování pomocí hello [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) rutiny.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-235">You can view hello details of hello existing backup policy using hello [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) cmdlet.</span></span> <span data-ttu-id="b0bf0-236">Vám může procházení další pomocí hello [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) rutiny pro plán zálohování hello a hello [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) rutina pro zásady uchovávání informací hello</span><span class="sxs-lookup"><span data-stu-id="b0bf0-236">You can drill-down further using hello [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) cmdlet for hello backup schedule and hello [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) cmdlet for hello retention policies</span></span>

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

### <a name="performing-an-ad-hoc-backup"></a><span data-ttu-id="b0bf0-237">Provádění zálohu ad-hoc</span><span class="sxs-lookup"><span data-stu-id="b0bf0-237">Performing an ad-hoc backup</span></span>
<span data-ttu-id="b0bf0-238">Jakmile byla nastavena zásada zálohování hello zálohy dojde za hello plán.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-238">Once a backup policy has been set hello backups will occur per hello schedule.</span></span> <span data-ttu-id="b0bf0-239">Spuštění služby ad-hoc zálohy je také možné pomocí hello [Start-OBBackup](https://technet.microsoft.com/library/hh770426) rutiny:</span><span class="sxs-lookup"><span data-stu-id="b0bf0-239">Triggering an ad-hoc backup is also possible using hello [Start-OBBackup](https://technet.microsoft.com/library/hh770426) cmdlet:</span></span>

```
PS C:> Get-OBPolicy | Start-OBBackup
Taking snapshot of volumes...
Preparing storage...
Estimating size of backup items...
Estimating size of backup items...
Transferring data...
Verifying backup...
Job completed.
hello backup operation completed successfully.
```

## <a name="restore-data-from-azure-backup"></a><span data-ttu-id="b0bf0-240">Obnovení dat z Azure Backup</span><span class="sxs-lookup"><span data-stu-id="b0bf0-240">Restore data from Azure Backup</span></span>
<span data-ttu-id="b0bf0-241">Tato část vás provede kroky hello pro automatizaci obnovení dat z Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-241">This section will guide you through hello steps for automating recovery of data from Azure Backup.</span></span> <span data-ttu-id="b0bf0-242">To zahrnuje tak hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="b0bf0-242">Doing so involves hello following steps:</span></span>

1. <span data-ttu-id="b0bf0-243">Vyberte zdrojový svazek hello</span><span class="sxs-lookup"><span data-stu-id="b0bf0-243">Pick hello source volume</span></span>
2. <span data-ttu-id="b0bf0-244">Vyberte zálohování toorestore bodu</span><span class="sxs-lookup"><span data-stu-id="b0bf0-244">Choose a backup point toorestore</span></span>
3. <span data-ttu-id="b0bf0-245">Zvolte toorestore položky</span><span class="sxs-lookup"><span data-stu-id="b0bf0-245">Choose an item toorestore</span></span>
4. <span data-ttu-id="b0bf0-246">Proces obnovení hello aktivační události</span><span class="sxs-lookup"><span data-stu-id="b0bf0-246">Trigger hello restore process</span></span>

### <a name="picking-hello-source-volume"></a><span data-ttu-id="b0bf0-247">Výběr hello zdrojový svazek</span><span class="sxs-lookup"><span data-stu-id="b0bf0-247">Picking hello source volume</span></span>
<span data-ttu-id="b0bf0-248">V pořadí toorestore položku z Azure Backup je nutné nejprve tooidentify hello zdroj položky hello.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-248">In order toorestore an item from Azure Backup, you first need tooidentify hello source of hello item.</span></span> <span data-ttu-id="b0bf0-249">Vzhledem k tomu, že jsme se provádění příkazů hello v kontextu hello systému Windows Server nebo klienta Windows, hello počítač je už definovaný.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-249">Since we're executing hello commands in hello context of a Windows Server or a Windows client, hello machine is already identified.</span></span> <span data-ttu-id="b0bf0-250">dalším krokem Hello při identifikaci zdroje hello je tooidentify hello svazku, který ji obsahuje.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-250">hello next step in identifying hello source is tooidentify hello volume containing it.</span></span> <span data-ttu-id="b0bf0-251">Seznam svazků nebo zdrojů zálohovaných z tohoto počítače se dá načíst spuštěním hello [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) rutiny.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-251">A list of volumes or sources being backed up from this machine can be retrieved by executing hello [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) cmdlet.</span></span> <span data-ttu-id="b0bf0-252">Tento příkaz vrátí pole všech zdrojů hello zálohovat z tohoto serveru nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-252">This command returns an array of all hello sources backed up from this server/client.</span></span>

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

### <a name="choosing-a-backup-point-toorestore"></a><span data-ttu-id="b0bf0-253">Výběr zálohování toorestore bodu</span><span class="sxs-lookup"><span data-stu-id="b0bf0-253">Choosing a backup point toorestore</span></span>
<span data-ttu-id="b0bf0-254">Hello body zálohy můžete načíst seznam spuštěním hello [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) rutiny s příslušnými parametry.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-254">hello list of backup points can be retrieved by executing hello [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdlet with appropriate parameters.</span></span> <span data-ttu-id="b0bf0-255">V našem příkladu jsme vybrali hello nejnovější bod zálohy pro zdrojový svazek hello *D:* a použít ho toorecover konkrétní soubor.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-255">In our example, we’ll choose hello latest backup point for hello source volume *D:* and use it toorecover a specific file.</span></span>

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
<span data-ttu-id="b0bf0-256">objekt Hello ```$rps``` je pole body zálohy.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-256">hello object ```$rps``` is an array of backup points.</span></span> <span data-ttu-id="b0bf0-257">první prvek Hello je poslední bod hello a n-tou element hello je nejstarší bod hello.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-257">hello first element is hello latest point and hello Nth element is hello oldest point.</span></span> <span data-ttu-id="b0bf0-258">toochoose hello posledního bodu, použijeme ```$rps[0]```.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-258">toochoose hello latest point, we will use ```$rps[0]```.</span></span>

### <a name="choosing-an-item-toorestore"></a><span data-ttu-id="b0bf0-259">Výběr toorestore položky</span><span class="sxs-lookup"><span data-stu-id="b0bf0-259">Choosing an item toorestore</span></span>
<span data-ttu-id="b0bf0-260">tooidentify hello přesný soubor nebo složku toorestore, rekurzivně použít hello [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-260">tooidentify hello exact file or folder toorestore, recursively use hello [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdlet.</span></span> <span data-ttu-id="b0bf0-261">Hierarchie složky hello tento způsob, jak lze procházet výhradně pomocí hello ```Get-OBRecoverableItem```.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-261">That way hello folder hierarchy can be browsed solely using hello ```Get-OBRecoverableItem```.</span></span>

<span data-ttu-id="b0bf0-262">V tomto příkladu, pokud chceme souboru hello toorestore *finances.xls* jsme můžete odkazovat pomocí tohoto objektu hello ```$filesFolders[1]```.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-262">In this example, if we want toorestore hello file *finances.xls* we can reference that using hello object ```$filesFolders[1]```.</span></span>

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

<span data-ttu-id="b0bf0-263">Můžete také vyhledat položky toorestore pomocí hello ```Get-OBRecoverableItem``` rutiny.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-263">You can also search for items toorestore using hello ```Get-OBRecoverableItem``` cmdlet.</span></span> <span data-ttu-id="b0bf0-264">V našem příkladu toosearch pro *finances.xls* nám může získat popisovač souboru hello spuštěním tohoto příkazu:</span><span class="sxs-lookup"><span data-stu-id="b0bf0-264">In our example, toosearch for *finances.xls* we could get a handle on hello file by running this command:</span></span>

```
PS C:\> $item = Get-OBRecoverableItem -RecoveryPoint $rps[0] -Location "D:\MyData" -SearchString "finance*"
```

### <a name="triggering-hello-restore-process"></a><span data-ttu-id="b0bf0-265">Spuštění procesu obnovení hello</span><span class="sxs-lookup"><span data-stu-id="b0bf0-265">Triggering hello restore process</span></span>
<span data-ttu-id="b0bf0-266">proces obnovení hello tootrigger, potřebujeme nejprve možnosti obnovení toospecify hello.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-266">tootrigger hello restore process, we first need toospecify hello recovery options.</span></span> <span data-ttu-id="b0bf0-267">Tento krok můžete provést pomocí hello [New-OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-267">This can be done by using hello [New-OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) cmdlet.</span></span> <span data-ttu-id="b0bf0-268">Pro tento příklad předpokládejme, že chceme, aby soubory hello toorestore příliš*C:\temp*. Také Předpokládejme, že má být tooskip soubory, které již existují na cílovou složku hello *C:\temp*. toocreate takové možnost obnovení pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b0bf0-268">For this example, let's assume that we want toorestore hello files too*C:\temp*. Let's also assume that we want tooskip files that already exist on hello destination folder *C:\temp*. toocreate such a recovery option, use hello following command:</span></span>

```
PS C:\> $recovery_option = New-OBRecoveryOption -DestinationPath "C:\temp" -OverwriteType Skip
```

<span data-ttu-id="b0bf0-269">Teď spustit obnovení pomocí hello [Start-OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) příkaz na hello vybrané ```$item``` z hello výstup hello ```Get-OBRecoverableItem``` rutiny:</span><span class="sxs-lookup"><span data-stu-id="b0bf0-269">Now trigger restore by using hello [Start-OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) command on hello selected ```$item``` from hello output of hello ```Get-OBRecoverableItem``` cmdlet:</span></span>

```
PS C:\> Start-OBRecovery -RecoverableItem $item -RecoveryOption $recover_option
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Job completed.
hello recovery operation completed successfully.
```


## <a name="uninstalling-hello-azure-backup-agent"></a><span data-ttu-id="b0bf0-270">Odinstalace agenta Azure Backup hello</span><span class="sxs-lookup"><span data-stu-id="b0bf0-270">Uninstalling hello Azure Backup agent</span></span>
<span data-ttu-id="b0bf0-271">Odinstalaci agenta Azure Backup hello lze provést pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="b0bf0-271">Uninstalling hello Azure Backup agent can be done by using hello following command:</span></span>

```
PS C:\> .\MARSAgentInstaller.exe /d /q
```

<span data-ttu-id="b0bf0-272">Odinstalace binárních souborů hello agenta z počítače hello má některé tooconsider důsledky:</span><span class="sxs-lookup"><span data-stu-id="b0bf0-272">Uninstalling hello agent binaries from hello machine has some consequences tooconsider:</span></span>

* <span data-ttu-id="b0bf0-273">Odebere z počítače hello soubor filtru hello a sledování změn je zastavena.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-273">It removes hello file-filter from hello machine, and tracking of changes is stopped.</span></span>
* <span data-ttu-id="b0bf0-274">Všechny informace o zásadách se odebere z počítače hello, ale informace o zásadách hello pokračuje toobe uložené ve službě hello.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-274">All policy information is removed from hello machine, but hello policy information continues toobe stored in hello service.</span></span>
* <span data-ttu-id="b0bf0-275">Se odeberou všechny plány zálohování a jsou provedeny žádné další zálohy.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-275">All backup schedules are removed, and no further backups are taken.</span></span>

<span data-ttu-id="b0bf0-276">Ale hello data uložená v Azure zůstane a zůstane podle nastavení zásad uchovávání hello vy.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-276">However, hello data stored in Azure remains and is retained as per hello retention policy setup by you.</span></span> <span data-ttu-id="b0bf0-277">Starší body jsou automaticky stará.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-277">Older points are automatically aged out.</span></span>

## <a name="remote-management"></a><span data-ttu-id="b0bf0-278">Vzdálená správa</span><span class="sxs-lookup"><span data-stu-id="b0bf0-278">Remote management</span></span>
<span data-ttu-id="b0bf0-279">Veškerá Správa hello kolem hello agenta Azure Backup, zásady a zdroje dat lze provést vzdáleně pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-279">All hello management around hello Azure Backup agent, policies, and data sources can be done remotely through PowerShell.</span></span> <span data-ttu-id="b0bf0-280">Hello počítač, který bude spravovat vzdáleně musí toobe připravený správně.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-280">hello machine that will be managed remotely needs toobe prepared correctly.</span></span>

<span data-ttu-id="b0bf0-281">Ve výchozím nastavení je Vzdálená správa systému Windows hello nakonfigurovaná pro ruční spuštění.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-281">By default, hello WinRM service is configured for manual startup.</span></span> <span data-ttu-id="b0bf0-282">musí být nastaven typ spouštění Hello příliš*automatické* a musí být spuštěna služba hello.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-282">hello startup type must be set too*Automatic* and hello service should be started.</span></span> <span data-ttu-id="b0bf0-283">tooverify, který hello Služba WinRM je spuštěná, by měly být hello hodnotu hello Vlastnost Status *systémem*.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-283">tooverify that hello WinRM service is running, hello value of hello Status property should be *Running*.</span></span>

```
PS C:\> Get-Service WinRM

Status   Name               DisplayName
------   ----               -----------
Running  winrm              Windows Remote Management (WS-Manag...
```

<span data-ttu-id="b0bf0-284">Prostředí PowerShell by měl být nakonfigurovaný pro vzdálenou komunikaci.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-284">PowerShell should be configured for remoting.</span></span>

```
PS C:\> Enable-PSRemoting -force
WinRM is already set up tooreceive requests on this computer.
WinRM has been updated for remote management.
WinRM firewall exception enabled.

PS C:\> Set-ExecutionPolicy unrestricted -force
```

<span data-ttu-id="b0bf0-285">počítač Hello teď můžete spravovat vzdáleně – od hello instalace agenta hello.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-285">hello machine can now be managed remotely - starting from hello installation of hello agent.</span></span> <span data-ttu-id="b0bf0-286">Například následující skript hello zkopíruje hello agenta toohello vzdáleného počítače a ho nainstaluje.</span><span class="sxs-lookup"><span data-stu-id="b0bf0-286">For example, hello following script copies hello agent toohello remote machine and installs it.</span></span>

```
PS C:\> $dloc = "\\REMOTESERVER01\c$\Windows\Temp"
PS C:\> $agent = "\\REMOTESERVER01\c$\Windows\Temp\MARSAgentInstaller.exe"
PS C:\> $args = "/q"
PS C:\> Copy-Item "C:\Downloads\MARSAgentInstaller.exe" -Destination $dloc - force

PS C:\> $s = New-PSSession -ComputerName REMOTESERVER01
PS C:\> Invoke-Command -Session $s -Script { param($d, $a) Start-Process -FilePath $d $a -Wait } -ArgumentList $agent $args
```

## <a name="next-steps"></a><span data-ttu-id="b0bf0-287">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b0bf0-287">Next steps</span></span>
<span data-ttu-id="b0bf0-288">Další informace o Azure Backup pro Windows Server nebo klienta najdete v tématu</span><span class="sxs-lookup"><span data-stu-id="b0bf0-288">For more information about Azure Backup for Windows Server/Client see</span></span>

* [<span data-ttu-id="b0bf0-289">Úvod tooAzure zálohování</span><span class="sxs-lookup"><span data-stu-id="b0bf0-289">Introduction tooAzure Backup</span></span>](backup-introduction-to-azure-backup.md)
* [<span data-ttu-id="b0bf0-290">Zálohování serverů Windows</span><span class="sxs-lookup"><span data-stu-id="b0bf0-290">Back up Windows Servers</span></span>](backup-configure-vault.md)
