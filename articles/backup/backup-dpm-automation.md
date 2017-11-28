---
title: "Zálohování Azure – zálohování úloh DPM pomocí Powershellu | Microsoft Docs"
description: "Zjistěte, jak nasadit a spravovat Azure Backup pro Data Protection Manager (DPM) pomocí prostředí PowerShell"
services: backup
documentationcenter: 
author: NKolli1
manager: shreeshd
editor: 
ms.assetid: e9bd223c-2398-4eb1-9bf3-50e08970fea7
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/23/2017
ms.author: adigan;anuragm;trinadhk;markgal
ms.openlocfilehash: 2e3b4a094511a59cfa02917efc2e3e053840af0c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-and-manage-backup-to-azure-for-data-protection-manager-dpm-servers-using-powershell"></a><span data-ttu-id="95771-103">Nasazení a správa zálohování do Azure pro servery DPM (Data Protection Manager) pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="95771-103">Deploy and manage backup to Azure for Data Protection Manager (DPM) servers using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="95771-104">ARM</span><span class="sxs-lookup"><span data-stu-id="95771-104">ARM</span></span>](backup-dpm-automation.md)
> * [<span data-ttu-id="95771-105">Classic</span><span class="sxs-lookup"><span data-stu-id="95771-105">Classic</span></span>](backup-dpm-automation-classic.md)
>
>

<span data-ttu-id="95771-106">Tento článek ukazuje, jak pomocí prostředí PowerShell instalační program Azure Backup na serveru DPM a ke správě zálohování a obnovení.</span><span class="sxs-lookup"><span data-stu-id="95771-106">This article shows you how to use PowerShell to setup Azure Backup on a DPM server, and to manage backup and recovery.</span></span>

## <a name="setting-up-the-powershell-environment"></a><span data-ttu-id="95771-107">Nastavení prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="95771-107">Setting up the PowerShell environment</span></span>
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

<span data-ttu-id="95771-108">Před použitím prostředí PowerShell pro správu z aplikace Data Protection Manager zálohování do Azure, musíte mít správné prostředí v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="95771-108">Before you can use PowerShell to manage backups from Data Protection Manager to Azure, you need to have the right environment in PowerShell.</span></span> <span data-ttu-id="95771-109">Na začátku relace prostředí PowerShell Ujistěte se, že spustíte následující příkaz a naimportovat moduly vpravo umožňují správně odkazovat rutiny aplikace DPM:</span><span class="sxs-lookup"><span data-stu-id="95771-109">At the start of the PowerShell session, ensure that you run the following command to import the right modules and allow you to correctly reference the DPM cmdlets:</span></span>

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

## <a name="setup-and-registration"></a><span data-ttu-id="95771-110">Instalace a registrace</span><span class="sxs-lookup"><span data-stu-id="95771-110">Setup and Registration</span></span>
<span data-ttu-id="95771-111">Chcete-li začít:</span><span class="sxs-lookup"><span data-stu-id="95771-111">To begin:</span></span>

1. <span data-ttu-id="95771-112">[Stáhněte si nejnovější PowerShell](https://github.com/Azure/azure-powershell/releases) (minimální požadovaná verze je: 1.0.0)</span><span class="sxs-lookup"><span data-stu-id="95771-112">[Download latest PowerShell](https://github.com/Azure/azure-powershell/releases) (minimum version required is: 1.0.0)</span></span>
2. <span data-ttu-id="95771-113">Povolte rutiny pro zálohování Azure tak, že přepnutí na *AzureResourceManager* režimu pomocí **Switch-AzureMode** příkaz:</span><span class="sxs-lookup"><span data-stu-id="95771-113">Enable the Azure Backup commandlets by switching to *AzureResourceManager* mode by using the **Switch-AzureMode** commandlet:</span></span>

```
PS C:\> Switch-AzureMode AzureResourceManager
```

<span data-ttu-id="95771-114">Následující instalaci a registraci úlohy je možné automatizovat pomocí prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="95771-114">The following setup and registration tasks can be automated with PowerShell:</span></span>

* <span data-ttu-id="95771-115">Vytvoření trezoru Služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="95771-115">Create a Recovery Services vault</span></span>
* <span data-ttu-id="95771-116">Instalace agenta Azure Backup</span><span class="sxs-lookup"><span data-stu-id="95771-116">Installing the Azure Backup agent</span></span>
* <span data-ttu-id="95771-117">Registrace u služby zálohování Azure</span><span class="sxs-lookup"><span data-stu-id="95771-117">Registering with the Azure Backup service</span></span>
* <span data-ttu-id="95771-118">Nastavení sítě</span><span class="sxs-lookup"><span data-stu-id="95771-118">Networking settings</span></span>
* <span data-ttu-id="95771-119">Nastavení šifrování</span><span class="sxs-lookup"><span data-stu-id="95771-119">Encryption settings</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="95771-120">Vytvoření trezoru služby Recovery Services</span><span class="sxs-lookup"><span data-stu-id="95771-120">Create a recovery services vault</span></span>
<span data-ttu-id="95771-121">Následující kroky vás provedou vytvoření trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="95771-121">The following steps lead you through creating a Recovery Services vault.</span></span> <span data-ttu-id="95771-122">Trezor služeb zotavení se liší od úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="95771-122">A Recovery Services vault is different than a Backup vault.</span></span>

1. <span data-ttu-id="95771-123">Pokud používáte Azure Backup poprvé, musíte použít **Register-AzureRMResourceProvider** rutiny k registraci poskytovatele služeb zotavení Azure s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="95771-123">If you are using Azure Backup for the first time, you must use the **Register-AzureRMResourceProvider** cmdlet to register the Azure Recovery Service provider with your subscription.</span></span>

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
2. <span data-ttu-id="95771-124">Trezor služeb zotavení je prostředek ARM, proto musíte umístit do skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="95771-124">The Recovery Services vault is an ARM resource, so you need to place it within a Resource Group.</span></span> <span data-ttu-id="95771-125">Můžete použít existující skupinu prostředků nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="95771-125">You can use an existing resource group, or create a new one.</span></span> <span data-ttu-id="95771-126">Když vytváříte novou skupinu prostředků, zadejte název a umístění pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="95771-126">When creating a new resource group, specify the name and location for the resource group.</span></span>  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "West US"
    ```
3. <span data-ttu-id="95771-127">Použití **New-AzureRmRecoveryServicesVault** vytvořte nové úložiště.</span><span class="sxs-lookup"><span data-stu-id="95771-127">Use the **New-AzureRmRecoveryServicesVault** cmdlet to create a new vault.</span></span> <span data-ttu-id="95771-128">Ujistěte se, že zadejte stejné umístění pro úložiště, jako byl použit pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="95771-128">Be sure to specify the same location for the vault as was used for the resource group.</span></span>

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "West US"
    ```
4. <span data-ttu-id="95771-129">Zadejte typ redundance úložiště se použije. můžete použít [místně redundantní úložiště (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) nebo [geograficky redundantní úložiště (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span><span class="sxs-lookup"><span data-stu-id="95771-129">Specify the type of storage redundancy to use; you can use [Locally Redundant Storage (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) or [Geo Redundant Storage (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span></span> <span data-ttu-id="95771-130">Následující příklad ukazuje, že je možnost - BackupStorageRedundancy pro testVault nastavena na GeoRedundant.</span><span class="sxs-lookup"><span data-stu-id="95771-130">The following example shows the -BackupStorageRedundancy option for testVault is set to GeoRedundant.</span></span>

   > [!TIP]
   > <span data-ttu-id="95771-131">Mnoho rutin Azure Backup vyžadují objekt trezoru služeb zotavení jako vstup.</span><span class="sxs-lookup"><span data-stu-id="95771-131">Many Azure Backup cmdlets require the Recovery Services vault object as an input.</span></span> <span data-ttu-id="95771-132">Z tohoto důvodu je vhodné pro uložení objektu trezoru služeb zotavení zálohování v proměnné.</span><span class="sxs-lookup"><span data-stu-id="95771-132">For this reason, it is convenient to store the Backup Recovery Services vault object in a variable.</span></span>
   >
   >

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testVault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-the-vaults-in-a-subscription"></a><span data-ttu-id="95771-133">Zobrazit trezorů v předplatném.</span><span class="sxs-lookup"><span data-stu-id="95771-133">View the vaults in a subscription</span></span>
<span data-ttu-id="95771-134">Použití **Get-AzureRmRecoveryServicesVault** Chcete-li zobrazit seznam všech trezorů v aktuálním předplatném.</span><span class="sxs-lookup"><span data-stu-id="95771-134">Use **Get-AzureRmRecoveryServicesVault** to view the list of all vaults in the current subscription.</span></span> <span data-ttu-id="95771-135">Tento příkaz můžete použít, chcete-li zkontrolovat, zda byl vytvořen nový trezor nebo jaké trezory jsou k dispozici v rámci předplatného.</span><span class="sxs-lookup"><span data-stu-id="95771-135">You can use this command to check that a new  vault was created, or to see what vaults are available in the subscription.</span></span>

<span data-ttu-id="95771-136">Spusťte příkaz Get-AzureRmRecoveryServicesVault, a jsou uvedeny všechny trezorů v rámci předplatného.</span><span class="sxs-lookup"><span data-stu-id="95771-136">Run the command, Get-AzureRmRecoveryServicesVault, and all vaults in the subscription are listed.</span></span>

```
PS C:\> Get-AzureRmRecoveryServicesVault
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```


## <a name="installing-the-azure-backup-agent-on-a-dpm-server"></a><span data-ttu-id="95771-137">Instalace agenta Azure Backup na serveru aplikace DPM</span><span class="sxs-lookup"><span data-stu-id="95771-137">Installing the Azure Backup agent on a DPM Server</span></span>
<span data-ttu-id="95771-138">Před instalací agenta Azure Backup, musíte mít instalační program stažené a existuje v systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="95771-138">Before you install the Azure Backup agent, you need to have the installer downloaded and present on the Windows Server.</span></span> <span data-ttu-id="95771-139">Můžete získat nejnovější verzi instalačního programu z [Microsoft Download Center](http://aka.ms/azurebackup_agent) nebo ze stránky řídicího panelu trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="95771-139">You can get the latest version of the installer from the [Microsoft Download Center](http://aka.ms/azurebackup_agent) or from the Recovery Services vault's Dashboard page.</span></span> <span data-ttu-id="95771-140">Instalační program uložte na snadno dostupném místě jako * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="95771-140">Save the installer to an easily accessible location like *C:\Downloads\*.</span></span>

<span data-ttu-id="95771-141">Pokud chcete nainstalovat agenta, spusťte následující příkaz v konzolu prostředí PowerShell se zvýšenými oprávněními **na serveru DPM**:</span><span class="sxs-lookup"><span data-stu-id="95771-141">To install the agent, run the following command in an elevated PowerShell console **on the DPM server**:</span></span>

```
PS C:\> MARSAgentInstaller.exe /q
```

<span data-ttu-id="95771-142">Tím se nainstaluje agent s výchozími možnostmi.</span><span class="sxs-lookup"><span data-stu-id="95771-142">This installs the agent with all the default options.</span></span> <span data-ttu-id="95771-143">Instalace trvá několik minut na pozadí.</span><span class="sxs-lookup"><span data-stu-id="95771-143">The installation takes a few minutes in the background.</span></span> <span data-ttu-id="95771-144">Pokud nezadáte */nu* možnost **Windows Update** otevře se okno na konci instalace k vyhledání všech aktualizací.</span><span class="sxs-lookup"><span data-stu-id="95771-144">If you do not specify the */nu* option the **Windows Update** window opens at the end of the installation to check for any updates.</span></span>

<span data-ttu-id="95771-145">Agenta se zobrazí v seznamu nainstalovaných programů.</span><span class="sxs-lookup"><span data-stu-id="95771-145">The agent shows up in the list of installed programs.</span></span> <span data-ttu-id="95771-146">Chcete-li zobrazit seznam nainstalovaných programů, přejděte na **ovládací panely** > **programy** > **programy a funkce**.</span><span class="sxs-lookup"><span data-stu-id="95771-146">To see the list of installed programs, go to **Control Panel** > **Programs** > **Programs and Features**.</span></span>

![Instalaci agenta](./media/backup-dpm-automation/installed-agent-listing.png)

### <a name="installation-options"></a><span data-ttu-id="95771-148">Možnosti instalace</span><span class="sxs-lookup"><span data-stu-id="95771-148">Installation options</span></span>
<span data-ttu-id="95771-149">Aby se zobrazily všechny možnosti, které jsou k dispozici prostřednictvím příkazového řádku, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="95771-149">To see all the options available via the commandline, use the following command:</span></span>

```
PS C:\> MARSAgentInstaller.exe /?
```

<span data-ttu-id="95771-150">Mezi dostupné možnosti patří:</span><span class="sxs-lookup"><span data-stu-id="95771-150">The available options include:</span></span>

| <span data-ttu-id="95771-151">Možnost</span><span class="sxs-lookup"><span data-stu-id="95771-151">Option</span></span> | <span data-ttu-id="95771-152">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="95771-152">Details</span></span> | <span data-ttu-id="95771-153">Výchozí</span><span class="sxs-lookup"><span data-stu-id="95771-153">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="95771-154">/q</span><span class="sxs-lookup"><span data-stu-id="95771-154">/q</span></span> |<span data-ttu-id="95771-155">Tichá instalace</span><span class="sxs-lookup"><span data-stu-id="95771-155">Quiet installation</span></span> |- |
| <span data-ttu-id="95771-156">/ p: "umístění"</span><span class="sxs-lookup"><span data-stu-id="95771-156">/p:"location"</span></span> |<span data-ttu-id="95771-157">Cesta ke složce instalace pro agenta Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="95771-157">Path to the installation folder for the Azure Backup agent.</span></span> |<span data-ttu-id="95771-158">C:\Program Files\Microsoft Azure Recovery Services agenta</span><span class="sxs-lookup"><span data-stu-id="95771-158">C:\Program Files\Microsoft Azure Recovery Services Agent</span></span> |
| <span data-ttu-id="95771-159">/ s: "umístění"</span><span class="sxs-lookup"><span data-stu-id="95771-159">/s:"location"</span></span> |<span data-ttu-id="95771-160">Cesta ke složce mezipaměti pro agenta Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="95771-160">Path to the cache folder for the Azure Backup agent.</span></span> |<span data-ttu-id="95771-161">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span><span class="sxs-lookup"><span data-stu-id="95771-161">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span></span> |
| <span data-ttu-id="95771-162">/m</span><span class="sxs-lookup"><span data-stu-id="95771-162">/m</span></span> |<span data-ttu-id="95771-163">Výslovný souhlas ke službě Microsoft Update</span><span class="sxs-lookup"><span data-stu-id="95771-163">Opt-in to Microsoft Update</span></span> |- |
| <span data-ttu-id="95771-164">/nu</span><span class="sxs-lookup"><span data-stu-id="95771-164">/nu</span></span> |<span data-ttu-id="95771-165">Nekontrolovat aktualizace po dokončení instalace</span><span class="sxs-lookup"><span data-stu-id="95771-165">Do not Check for updates after installation is complete</span></span> |- |
| <span data-ttu-id="95771-166">/d</span><span class="sxs-lookup"><span data-stu-id="95771-166">/d</span></span> |<span data-ttu-id="95771-167">Odinstaluje Agenta Microsoft Azure Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="95771-167">Uninstalls Microsoft Azure Recovery Services Agent</span></span> |- |
| <span data-ttu-id="95771-168">/pH</span><span class="sxs-lookup"><span data-stu-id="95771-168">/ph</span></span> |<span data-ttu-id="95771-169">Adresa proxy hostitele</span><span class="sxs-lookup"><span data-stu-id="95771-169">Proxy Host Address</span></span> |- |
| <span data-ttu-id="95771-170">/Po</span><span class="sxs-lookup"><span data-stu-id="95771-170">/po</span></span> |<span data-ttu-id="95771-171">Číslo portu proxy hostitele</span><span class="sxs-lookup"><span data-stu-id="95771-171">Proxy Host Port Number</span></span> |- |
| <span data-ttu-id="95771-172">/Pu</span><span class="sxs-lookup"><span data-stu-id="95771-172">/pu</span></span> |<span data-ttu-id="95771-173">Uživatelské jméno proxy hostitele</span><span class="sxs-lookup"><span data-stu-id="95771-173">Proxy Host UserName</span></span> |- |
| <span data-ttu-id="95771-174">/pW</span><span class="sxs-lookup"><span data-stu-id="95771-174">/pw</span></span> |<span data-ttu-id="95771-175">Heslo pro proxy server</span><span class="sxs-lookup"><span data-stu-id="95771-175">Proxy Password</span></span> |- |

## <a name="registering-dpm-to-a-recovery-services-vault"></a><span data-ttu-id="95771-176">Registrace aplikace DPM k obnovení služby úložiště</span><span class="sxs-lookup"><span data-stu-id="95771-176">Registering DPM to a Recovery Services Vault</span></span>
<span data-ttu-id="95771-177">Po vytvoření trezoru služeb zotavení, stáhněte si nejnovější verzi agenta a přihlašovací údaje trezoru a uložte ho do vhodného umístění jako C:\Downloads.</span><span class="sxs-lookup"><span data-stu-id="95771-177">After you created the Recovery Services vault, download the latest agent and the vault credentials and store it in a convenient location like C:\Downloads.</span></span>

```
PS C:\> $credspath = "C:\downloads"
PS C:\> $credsfilename = Get-AzureRmRecoveryServicesVaultSettingsFile -Backup -Vault $vault1 -Path  $credspath
PS C:\> $credsfilename
C:\downloads\testvault\_Sun Apr 10 2016.VaultCredentials
```

<span data-ttu-id="95771-178">Na serveru aplikace DPM spusťte [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) rutiny počítač zaregistrovat v trezoru.</span><span class="sxs-lookup"><span data-stu-id="95771-178">On the DPM server, run the [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) cmdlet to register the machine with the vault.</span></span>

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-OBRegistration-VaultCredentials $cred -Confirm:$false
CertThumbprint      :7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName: testvault
Region              :West US
Machine registration succeeded.
```

### <a name="initial-configuration-settings"></a><span data-ttu-id="95771-179">Počáteční konfigurace nastavení</span><span class="sxs-lookup"><span data-stu-id="95771-179">Initial configuration settings</span></span>
<span data-ttu-id="95771-180">Jakmile je DPM Server registrován s trezor služeb zotavení, začíná výchozí nastavení odběru.</span><span class="sxs-lookup"><span data-stu-id="95771-180">Once the DPM Server is registered with the Recovery Services vault, it starts with default subscription settings.</span></span> <span data-ttu-id="95771-181">Tato nastavení odběru zahrnují sítě, šifrování a pracovní oblasti.</span><span class="sxs-lookup"><span data-stu-id="95771-181">These subscription settings include Networking, Encryption and the Staging area.</span></span> <span data-ttu-id="95771-182">Chcete-li změnit nastavení odběru je nutné nejprve získat popisovač na existující nastavení (výchozí), pomocí [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) rutiny:</span><span class="sxs-lookup"><span data-stu-id="95771-182">To change subscription settings you need to first get a handle on the existing (default) settings using the [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) cmdlet:</span></span>

```
$setting = Get-DPMCloudSubscriptionSetting -DPMServerName "TestingServer"
```

<span data-ttu-id="95771-183">Všechny změny provedené tento objekt místní prostředí PowerShell ```$setting``` a pak se zaměřuje na aplikace DPM a Azure Backup, o jejich uložení úplné objekt pomocí [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) rutiny.</span><span class="sxs-lookup"><span data-stu-id="95771-183">All modifications are made to this local PowerShell object ```$setting```  and then the full object is committed to DPM and Azure Backup to save them using the [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span></span> <span data-ttu-id="95771-184">Budete muset použít ```–Commit``` příznak zajistit, že jsou nastavené jako trvalé změny.</span><span class="sxs-lookup"><span data-stu-id="95771-184">You need to use the ```–Commit``` flag to ensure that the changes are persisted.</span></span> <span data-ttu-id="95771-185">Nastavení nebude použita a pokud potvrzené používá Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="95771-185">The settings will not be applied and used by Azure Backup unless committed.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="networking"></a><span data-ttu-id="95771-186">Sítě</span><span class="sxs-lookup"><span data-stu-id="95771-186">Networking</span></span>
<span data-ttu-id="95771-187">Pokud připojení počítače aplikace DPM do služby Azure Backup na internet přes proxy server, musí pro úspěšné zálohy zadat nastavení serveru proxy.</span><span class="sxs-lookup"><span data-stu-id="95771-187">If the connectivity of the DPM machine to the Azure Backup service on the internet is through a proxy server, then the proxy server settings should be provided for successful backups.</span></span> <span data-ttu-id="95771-188">To se provádí pomocí ```-ProxyServer```a ```-ProxyPort```, ```-ProxyUsername``` a ```ProxyPassword``` parametry se [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) rutiny.</span><span class="sxs-lookup"><span data-stu-id="95771-188">This is done by using the ```-ProxyServer```and ```-ProxyPort```, ```-ProxyUsername``` and the ```ProxyPassword``` parameters with the [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span></span> <span data-ttu-id="95771-189">V tomto příkladu není žádný proxy server, jsme jsou explicitně vymazání údaje související s proxy serverem.</span><span class="sxs-lookup"><span data-stu-id="95771-189">In this example, there is no proxy server so we are explicitly clearing any proxy-related information.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoProxy
```

<span data-ttu-id="95771-190">Využití šířky pásma se dá taky nastavit pomocí možnosti ```-WorkHourBandwidth``` a ```-NonWorkHourBandwidth``` pro danou sadu dny v týdnu.</span><span class="sxs-lookup"><span data-stu-id="95771-190">Bandwidth usage can also be controlled with options of ```-WorkHourBandwidth``` and ```-NonWorkHourBandwidth``` for a given set of days of the week.</span></span> <span data-ttu-id="95771-191">V tomto příkladu jsme nejsou nastavení žádné omezení.</span><span class="sxs-lookup"><span data-stu-id="95771-191">In this example, we are not setting any throttling.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoThrottle
```

## <a name="configuring-the-staging-area"></a><span data-ttu-id="95771-192">Konfigurace pracovní oblasti</span><span class="sxs-lookup"><span data-stu-id="95771-192">Configuring the staging Area</span></span>
<span data-ttu-id="95771-193">Agent Azure Backup spuštěný na serveru DPM potřebuje dočasné úložiště pro data obnovená z cloudu (místní pracovní oblasti).</span><span class="sxs-lookup"><span data-stu-id="95771-193">The Azure Backup agent running on the DPM server needs temporary storage for data restored from the cloud (local staging area).</span></span> <span data-ttu-id="95771-194">Nakonfigurovat pracovní oblasti pomocí [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) rutiny a ```-StagingAreaPath``` parametr.</span><span class="sxs-lookup"><span data-stu-id="95771-194">Configure the staging area using the [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet and the ```-StagingAreaPath``` parameter.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -StagingAreaPath "C:\StagingArea"
```

<span data-ttu-id="95771-195">V příkladu nahoře, je možnost pracovní oblasti *C:\StagingArea* v prostředí PowerShell objektu ```$setting```.</span><span class="sxs-lookup"><span data-stu-id="95771-195">In the example above, the staging area will be set to *C:\StagingArea* in the PowerShell object ```$setting```.</span></span> <span data-ttu-id="95771-196">Ujistěte se, že v zadané složce již existuje, jinak se nezdaří poslední zápis nastavení přihlášení k odběru.</span><span class="sxs-lookup"><span data-stu-id="95771-196">Ensure that the specified folder already exists, or else the final commit of the subscription settings will fail.</span></span>

### <a name="encryption-settings"></a><span data-ttu-id="95771-197">Nastavení šifrování</span><span class="sxs-lookup"><span data-stu-id="95771-197">Encryption settings</span></span>
<span data-ttu-id="95771-198">Zálohování data přenášená do služby Azure Backup je zašifrovaná chránit jejich důvěrnost dat.</span><span class="sxs-lookup"><span data-stu-id="95771-198">The backup data sent to Azure Backup is encrypted to protect the confidentiality of the data.</span></span> <span data-ttu-id="95771-199">Šifrovací přístupové heslo je 'heslo' k dešifrování dat v době obnovení.</span><span class="sxs-lookup"><span data-stu-id="95771-199">The encryption passphrase is the "password" to decrypt the data at the time of restore.</span></span> <span data-ttu-id="95771-200">Je důležité udržovat tento informace o bezpečném po nastavení.</span><span class="sxs-lookup"><span data-stu-id="95771-200">It is important to keep this information safe and secure once it is set.</span></span>

<span data-ttu-id="95771-201">V následujícím příkladu převede první příkaz řetězec ```passphrase123456789``` zabezpečený řetězec a přiřadí zabezpečený řetězec proměnné s názvem ```$Passphrase```.</span><span class="sxs-lookup"><span data-stu-id="95771-201">In the example below, the first command converts the string ```passphrase123456789``` to a secure string and assigns the secure string to the variable named ```$Passphrase```.</span></span> <span data-ttu-id="95771-202">v druhém příkazu nastaví zabezpečený řetězec v ```$Passphrase``` jako heslo pro šifrování záloh.</span><span class="sxs-lookup"><span data-stu-id="95771-202">the second command sets the secure string in ```$Passphrase``` as the password for encrypting backups.</span></span>

```
PS C:\> $Passphrase = ConvertTo-SecureString -string "passphrase123456789" -AsPlainText -Force

PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -EncryptionPassphrase $Passphrase
```

> [!IMPORTANT]
> <span data-ttu-id="95771-203">Zachování informací o přístupové heslo bezpečném, po nastavení.</span><span class="sxs-lookup"><span data-stu-id="95771-203">Keep the passphrase information safe and secure once it is set.</span></span> <span data-ttu-id="95771-204">Nebudete schopni obnovit data ze služby Azure bez tohoto hesla.</span><span class="sxs-lookup"><span data-stu-id="95771-204">You will not be able to restore data from Azure without this passphrase.</span></span>
>
>

<span data-ttu-id="95771-205">V tomto okamžiku jste měli udělali všechny požadované změny pro ```$setting``` objektu.</span><span class="sxs-lookup"><span data-stu-id="95771-205">At this point, you should have made all the required changes to the ```$setting``` object.</span></span> <span data-ttu-id="95771-206">Mějte na paměti k provedení změn.</span><span class="sxs-lookup"><span data-stu-id="95771-206">Remember to commit the changes.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="protect-data-to-azure-backup"></a><span data-ttu-id="95771-207">Ochrana dat do služby Azure Backup</span><span class="sxs-lookup"><span data-stu-id="95771-207">Protect data to Azure Backup</span></span>
<span data-ttu-id="95771-208">V této části přidáte provozním serveru DPM a poté proveďte ochranu dat do místního úložiště DPM a pak do služby Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="95771-208">In this section, you will add a production server to DPM and then protect the data to local DPM storage and then to Azure Backup.</span></span> <span data-ttu-id="95771-209">V příkladech jsme se ukazují, jak zálohovat soubory a složky.</span><span class="sxs-lookup"><span data-stu-id="95771-209">In the examples, we will demonstrate how to back up files and folders.</span></span> <span data-ttu-id="95771-210">Chcete-li zálohovat všechny zdroje dat aplikace DPM podporováno lze snadno rozšířit logiku.</span><span class="sxs-lookup"><span data-stu-id="95771-210">The logic can easily be extended to backup any DPM-supported data source.</span></span> <span data-ttu-id="95771-211">Všechny zálohy aplikace DPM se řídí pomocí ochranu skupiny (PG) s čtyři části:</span><span class="sxs-lookup"><span data-stu-id="95771-211">All your DPM backups are governed by a Protection Group (PG) with four parts:</span></span>

1. <span data-ttu-id="95771-212">**Členy skupiny** je seznam chránitelné objekty (také označované jako *zdrojů dat* v DPM), které chcete chránit ve stejné skupině ochrany.</span><span class="sxs-lookup"><span data-stu-id="95771-212">**Group members** is a list of all the protectable objects (also known as *Datasources* in DPM) that you want to protect in the same protection group.</span></span> <span data-ttu-id="95771-213">Chcete třeba chránit produkční virtuálních počítačů na jednu skupinu ochrany a databáze systému SQL Server v jiné skupiny ochrany, protože mají různé požadavky na zálohování.</span><span class="sxs-lookup"><span data-stu-id="95771-213">For example, you may want to protect production VMs in one protection group and SQL Server databases in another protection group as they may have different backup requirements.</span></span> <span data-ttu-id="95771-214">Předtím, než můžete zálohovat žádný zdroj dat na provozním serveru, které potřebujete, abyste měli jistotu, agenta aplikace DPM je nainstalován na serveru a je spravovaných aplikací DPM.</span><span class="sxs-lookup"><span data-stu-id="95771-214">Before you can back up any datasource on a production server you need to make sure the DPM Agent is installed on the server and is managed by DPM.</span></span> <span data-ttu-id="95771-215">Postupujte podle kroků pro [instalace agenta DPM](https://technet.microsoft.com/library/bb870935.aspx) a propojení na příslušný Server aplikace DPM.</span><span class="sxs-lookup"><span data-stu-id="95771-215">Follow the steps for [installing the DPM Agent](https://technet.microsoft.com/library/bb870935.aspx) and linking it to the appropriate DPM Server.</span></span>
2. <span data-ttu-id="95771-216">**Způsob ochrany dat** Určuje umístění zálohování cíl - páska, disk a cloud.</span><span class="sxs-lookup"><span data-stu-id="95771-216">**Data protection method** specifies the target backup locations - tape, disk, and cloud.</span></span> <span data-ttu-id="95771-217">V našem příkladu jsme bude chránit data na místní disk a do cloudu.</span><span class="sxs-lookup"><span data-stu-id="95771-217">In our example we will protect data to the local disk and to the cloud.</span></span>
3. <span data-ttu-id="95771-218">A **plán zálohování** určující, kdy je třeba přijmout zálohy a jak často se mají synchronizovat data mezi serverem DPM a na provozním serveru.</span><span class="sxs-lookup"><span data-stu-id="95771-218">A **backup schedule** that specifies when backups need to be taken and how often the data should be synchronized between the DPM Server and the production server.</span></span>
4. <span data-ttu-id="95771-219">A **plán uchovávání informací** který určuje, jak dlouho chcete zachovat body obnovení v Azure.</span><span class="sxs-lookup"><span data-stu-id="95771-219">A **retention schedule** that specifies how long to retain the recovery points in Azure.</span></span>

### <a name="creating-a-protection-group"></a><span data-ttu-id="95771-220">Vytvoření skupiny ochrany</span><span class="sxs-lookup"><span data-stu-id="95771-220">Creating a protection group</span></span>
<span data-ttu-id="95771-221">Začněte vytvořením nové skupiny ochrany pomocí [New-DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) rutiny.</span><span class="sxs-lookup"><span data-stu-id="95771-221">Start by creating a new Protection Group using the [New-DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) cmdlet.</span></span>

```
PS C:\> $PG = New-DPMProtectionGroup -DPMServerName " TestingServer " -Name "ProtectGroup01"
```

<span data-ttu-id="95771-222">Výše uvedené rutina vytvoří skupiny ochrany s názvem *ProtectGroup01*.</span><span class="sxs-lookup"><span data-stu-id="95771-222">The above cmdlet will create a Protection Group named *ProtectGroup01*.</span></span> <span data-ttu-id="95771-223">Existující skupiny ochrany můžete také upravit později přidat zálohování do cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="95771-223">An existing protection group can also be modified later to add backup to the Azure cloud.</span></span> <span data-ttu-id="95771-224">Ale žádné změny do skupiny ochrany – nové nebo existující - musíme získat popisovač *upravitelnými* pomocí [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) rutiny.</span><span class="sxs-lookup"><span data-stu-id="95771-224">However, to make any changes to the Protection Group - new or existing - we need to get a handle on a *modifiable* object using the [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) cmdlet.</span></span>

```
PS C:\> $MPG = Get-ModifiableProtectionGroup $PG
```

### <a name="adding-group-members-to-the-protection-group"></a><span data-ttu-id="95771-225">Přidání členů skupiny do skupiny ochrany</span><span class="sxs-lookup"><span data-stu-id="95771-225">Adding group members to the Protection Group</span></span>
<span data-ttu-id="95771-226">Každý Agent aplikace DPM zná seznam zdrojů dat na serveru, který je nainstalovaný.</span><span class="sxs-lookup"><span data-stu-id="95771-226">Each DPM Agent knows the list of datasources on the server that it is installed on.</span></span> <span data-ttu-id="95771-227">Přidat zdroje dat do skupiny ochrany, musí nejprve poslat seznam zdrojů dat zpět na DPM server agenta aplikace DPM.</span><span class="sxs-lookup"><span data-stu-id="95771-227">To add a datasource to the Protection Group, the DPM Agent needs to first send a list of the datasources back to the DPM server.</span></span> <span data-ttu-id="95771-228">Jeden nebo více zdrojů dat se pak vybrali a přidají se do skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="95771-228">One or more datasources are then selected and added to the Protection Group.</span></span> <span data-ttu-id="95771-229">Prostředí PowerShell kroky potřebné k dosažení tohoto cíle jsou:</span><span class="sxs-lookup"><span data-stu-id="95771-229">The PowerShell steps needed to achieve this are:</span></span>

1. <span data-ttu-id="95771-230">Získat seznam všech serverů spravovaných aplikací DPM pomocí agenta aplikace DPM.</span><span class="sxs-lookup"><span data-stu-id="95771-230">Fetch a list of all servers managed by DPM through the DPM Agent.</span></span>
2. <span data-ttu-id="95771-231">Vyberte konkrétní server.</span><span class="sxs-lookup"><span data-stu-id="95771-231">Choose a specific server.</span></span>
3. <span data-ttu-id="95771-232">Získat seznam všech zdrojů dat na serveru.</span><span class="sxs-lookup"><span data-stu-id="95771-232">Fetch a list of all datasources on the server.</span></span>
4. <span data-ttu-id="95771-233">Vyberte jeden nebo více zdrojů dat a přidat je do skupiny ochrany</span><span class="sxs-lookup"><span data-stu-id="95771-233">Choose one or more datasources and add them to the Protection Group</span></span>

<span data-ttu-id="95771-234">Seznam serverů, ve kterých je nainstalován Agent aplikace DPM a je spravován serverem aplikace DPM je získán pomocí [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) rutiny.</span><span class="sxs-lookup"><span data-stu-id="95771-234">The list of servers on which the DPM Agent is installed and is being managed by the DPM Server is acquired with the [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) cmdlet.</span></span> <span data-ttu-id="95771-235">V tomto příkladu jsme bude filtrovat a konfigurovat jenom PS s názvem *productionserver01* pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="95771-235">In this example we will filter and only configure PS with name *productionserver01* for backup.</span></span>

```
PS C:\> $server = Get-ProductionServer -DPMServerName "TestingServer" | where {($_.servername) –contains “productionserver01”
```

<span data-ttu-id="95771-236">Nyní se načíst seznam zdrojů dat v ```$server``` pomocí [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) rutiny.</span><span class="sxs-lookup"><span data-stu-id="95771-236">Now fetch the list of datasources on ```$server``` using the [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) cmdlet.</span></span> <span data-ttu-id="95771-237">V tomto příkladu jsme pro svazek filtrování * D:\* , jsme chcete konfigurovat pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="95771-237">In this example we are filtering for the volume *D:\* that we want to configure for backup.</span></span> <span data-ttu-id="95771-238">Tento zdroj dat se pak přidá do skupiny ochrany pomocí [přidat DPMChildDatasource](https://technet.microsoft.com/library/hh881732) rutiny.</span><span class="sxs-lookup"><span data-stu-id="95771-238">This datasource is then added to the Protection Group using the [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732) cmdlet.</span></span> <span data-ttu-id="95771-239">Nezapomeňte použít *upravitelnými* objektu skupiny ochrany ```$MPG``` k budou přidány.</span><span class="sxs-lookup"><span data-stu-id="95771-239">Remember to use the *modifiable* protection group object ```$MPG``` to make the additions.</span></span>

```
PS C:\> $DS = Get-Datasource -ProductionServer $server -Inquire | where { $_.Name -contains “D:\” }

PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS
```

<span data-ttu-id="95771-240">Opakujte tento krok tolikrát, kolikrát podle potřeby, dokud nepřidáte všechny zvolené zdroje dat do skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="95771-240">Repeat this step as many times as required, until you have added all the chosen datasources to the protection group.</span></span> <span data-ttu-id="95771-241">Můžete začít s jenom jeden zdroj dat a dokončení pracovního postupu pro vytvoření skupiny ochrany a později přidat další datové zdroje do skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="95771-241">You can also start with just one datasource, and complete the workflow for creating the Protection Group, and at a later point add more datasources to the Protection Group.</span></span>

### <a name="selecting-the-data-protection-method"></a><span data-ttu-id="95771-242">Výběr způsobu ochrany dat</span><span class="sxs-lookup"><span data-stu-id="95771-242">Selecting the data protection method</span></span>
<span data-ttu-id="95771-243">Jakmile zdroje dat byly přidány do skupiny ochrany, dalším krokem je zadat, ochranu pomocí metody [Set-DPMProtectionType](https://technet.microsoft.com/library/hh881725) rutiny.</span><span class="sxs-lookup"><span data-stu-id="95771-243">Once the datasources have been added to the Protection Group, the next step is to specify the protection method using the [Set-DPMProtectionType](https://technet.microsoft.com/library/hh881725) cmdlet.</span></span> <span data-ttu-id="95771-244">V tomto příkladu skupina ochrany je instalační program pro místního disku a zálohování.</span><span class="sxs-lookup"><span data-stu-id="95771-244">In this example, the Protection Group is setup for local disk and cloud backup.</span></span> <span data-ttu-id="95771-245">Budete taky muset zadat zdroj dat, který chcete chránit, do cloudu pomocí [přidat DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) rutiny příznakem - Online.</span><span class="sxs-lookup"><span data-stu-id="95771-245">You also need to specify the datasource that you want to protect to cloud using the [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) cmdlet with -Online flag.</span></span>

```
PS C:\> Set-DPMProtectionType -ProtectionGroup $MPG -ShortTerm Disk –LongTerm Online
PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS –Online
```

### <a name="setting-the-retention-range"></a><span data-ttu-id="95771-246">Nastavení uchování</span><span class="sxs-lookup"><span data-stu-id="95771-246">Setting the retention range</span></span>
<span data-ttu-id="95771-247">Nastavení uchovávání bodů zálohování pomocí [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) rutiny.</span><span class="sxs-lookup"><span data-stu-id="95771-247">Set the retention for the backup points using the [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span></span> <span data-ttu-id="95771-248">Když může být lichý nastavení uchovávání předtím, než byla definována plán zálohování, pomocí ```Set-DPMPolicyObjective``` rutiny automaticky nastaví výchozí plán zálohování, který je pak možné upravit.</span><span class="sxs-lookup"><span data-stu-id="95771-248">While it might seem odd to set the retention before the backup schedule has been defined, using the ```Set-DPMPolicyObjective``` cmdlet automatically sets a default backup schedule that can then be modified.</span></span> <span data-ttu-id="95771-249">Vždy je možné sady nejdřív naplánovat zálohování a po zásady uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="95771-249">It is always possible to set the backup schedule first and the retention policy after.</span></span>

<span data-ttu-id="95771-250">V následujícím příkladu rutiny nastavuje parametry, uchování pro zálohy disku.</span><span class="sxs-lookup"><span data-stu-id="95771-250">In the example below, the cmdlet sets the retention parameters for disk backups.</span></span> <span data-ttu-id="95771-251">To bude uchování záloh pro 10 dnů a synchronizaci dat každých 6 hodin mezi provozním serveru a serverem DPM.</span><span class="sxs-lookup"><span data-stu-id="95771-251">This will retain backups for 10 days, and sync data every 6 hours between the production server and the DPM server.</span></span> <span data-ttu-id="95771-252">```SynchronizationFrequencyMinutes``` Nedefinuje četnost zálohování je bod vytvořen, ale jak často data se zkopírují na serveru aplikace DPM.</span><span class="sxs-lookup"><span data-stu-id="95771-252">The ```SynchronizationFrequencyMinutes``` doesn't define how often a backup point is created, but how often data is copied to the DPM server.</span></span>  <span data-ttu-id="95771-253">Toto nastavení zabrání zálohy příliš velká.</span><span class="sxs-lookup"><span data-stu-id="95771-253">This setting prevents backups from becoming too large.</span></span>

```
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -RetentionRangeInDays 10 -SynchronizationFrequencyMinutes 360
```

<span data-ttu-id="95771-254">Pro zálohování přejdete do Azure (aplikace DPM na ně odkazuje jako zálohování Online) rozsahy uchovávání mohou být konfigurovány pro [dlouhodobé uchovávání používá schéma Dědečka. otec SYN (GFS)](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="95771-254">For backups going to Azure (DPM refers to them as Online backups) the retention ranges can be configured for [long term retention using a Grandfather-Father-Son scheme (GFS)](backup-azure-backup-cloud-as-tape.md).</span></span> <span data-ttu-id="95771-255">To znamená můžete definovat zásady uchovávání informací kombinované zahrnující denní, týdenní, měsíční a roční zásady uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="95771-255">That is, you can define a combined retention policy involving daily, weekly, monthly and yearly retention policies.</span></span> <span data-ttu-id="95771-256">V tomto příkladu vytvoření představující schéma komplexní uchovávání, který má být pole a pak nakonfigurujte pomocí rozsah uchování [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) rutiny.</span><span class="sxs-lookup"><span data-stu-id="95771-256">In this example, we create an array representing the complex retention scheme that we want, and then configure the retention range using the [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span></span>

```
PS C:\> $RRlist = @()
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 180, Days)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 104, Weeks)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 60, Month)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 10, Years)
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -OnlineRetentionRangeList $RRlist
```

### <a name="set-the-backup-schedule"></a><span data-ttu-id="95771-257">Nastavte plán zálohování</span><span class="sxs-lookup"><span data-stu-id="95771-257">Set the backup schedule</span></span>
<span data-ttu-id="95771-258">Aplikace DPM nastaví výchozí plán zálohování automaticky, pokud zadáte cíle ochrany pomocí ```Set-DPMPolicyObjective``` rutiny.</span><span class="sxs-lookup"><span data-stu-id="95771-258">DPM sets a default backup schedule automatically if you specify the protection objective using the ```Set-DPMPolicyObjective``` cmdlet.</span></span> <span data-ttu-id="95771-259">Ke změně výchozích plánů, použijte [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) následovanou [Set-DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) rutiny.</span><span class="sxs-lookup"><span data-stu-id="95771-259">To change the default schedules, use the [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) cmdlet followed by the [Set-DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) cmdlet.</span></span>

```
PS C:\> $onlineSch = Get-DPMPolicySchedule -ProtectionGroup $mpg -LongTerm Online
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[0] -TimesOfDay 02:00
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[1] -TimesOfDay 02:00 -DaysOfWeek Sa,Su –Interval 1
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[2] -TimesOfDay 02:00 -RelativeIntervals First,Third –DaysOfWeek Sa
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[3] -TimesOfDay 02:00 -DaysOfMonth 2,5,8,9 -Months Jan,Jul
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```

<span data-ttu-id="95771-260">V předchozím příkladu ```$onlineSch``` je pole se čtyřmi prvky, které obsahuje existující plán ochranu online pro skupinu ochrany ve schématu GFS:</span><span class="sxs-lookup"><span data-stu-id="95771-260">In the above example, ```$onlineSch``` is an array with four elements that contains the existing online protection schedule for the Protection Group in the GFS scheme:</span></span>

1. <span data-ttu-id="95771-261">```$onlineSch[0]```obsahuje denní plán</span><span class="sxs-lookup"><span data-stu-id="95771-261">```$onlineSch[0]``` contains the daily schedule</span></span>
2. <span data-ttu-id="95771-262">```$onlineSch[1]```obsahuje Týdenní plán</span><span class="sxs-lookup"><span data-stu-id="95771-262">```$onlineSch[1]``` contains the weekly schedule</span></span>
3. <span data-ttu-id="95771-263">```$onlineSch[2]```obsahuje plánování měsíčně</span><span class="sxs-lookup"><span data-stu-id="95771-263">```$onlineSch[2]``` contains the monthly schedule</span></span>
4. <span data-ttu-id="95771-264">```$onlineSch[3]```obsahuje roční plán</span><span class="sxs-lookup"><span data-stu-id="95771-264">```$onlineSch[3]``` contains the yearly schedule</span></span>

<span data-ttu-id="95771-265">Takže pokud budete muset upravit týdenní plán, budete muset naleznete ```$onlineSch[1]```.</span><span class="sxs-lookup"><span data-stu-id="95771-265">So if you need to modify the weekly schedule, you need to refer to the ```$onlineSch[1]```.</span></span>

### <a name="initial-backup"></a><span data-ttu-id="95771-266">Prvotní zálohování</span><span class="sxs-lookup"><span data-stu-id="95771-266">Initial backup</span></span>
<span data-ttu-id="95771-267">Při zálohování zdroje dat poprvé, musí aplikace DPM vytvoří počáteční repliky, vytvoří úplnou kopii zdroj dat, měly by být na svazku repliky DPM.</span><span class="sxs-lookup"><span data-stu-id="95771-267">When backing up a datasource for the first time, DPM needs creates initial replica that creates a full copy of the datasource to be protected on DPM replica volume.</span></span> <span data-ttu-id="95771-268">Tato aktivita bude naplánována buď pro určitý čas nebo lze spustit ručně, pomocí [Set-DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) rutiny s parametrem ```-NOW```.</span><span class="sxs-lookup"><span data-stu-id="95771-268">This activity can either be scheduled for a specific time, or can be triggered manually, using the [Set-DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) cmdlet with the parameter ```-NOW```.</span></span>

```
PS C:\> Set-DPMReplicaCreationMethod -ProtectionGroup $MPG -NOW
```
### <a name="changing-the-size-of-dpm-replica--recovery-point-volume"></a><span data-ttu-id="95771-269">Změna velikosti repliky aplikace DPM a svazek bodu obnovení</span><span class="sxs-lookup"><span data-stu-id="95771-269">Changing the size of DPM Replica & recovery point volume</span></span>
<span data-ttu-id="95771-270">Můžete také změnit velikost svazku repliky DPM a stínové kopie svazku pomocí [Set-DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) rutiny jako v následujícím příkladu: Get-DatasourceDiskAllocation - Datasource $DS Set-DatasourceDiskAllocation - Zdroj dat $DS - ProtectionGroup $MPG – ruční - ReplicaArea (2 gb) - ShadowCopyArea (2 gb)</span><span class="sxs-lookup"><span data-stu-id="95771-270">You can also change the size of DPM Replica volume and Shadow Copy volume using [Set-DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) cmdlet as in the following example: Get-DatasourceDiskAllocation -Datasource $DS Set-DatasourceDiskAllocation -Datasource $DS -ProtectionGroup $MPG -manual -ReplicaArea (2gb) -ShadowCopyArea (2gb)</span></span>

### <a name="committing-the-changes-to-the-protection-group"></a><span data-ttu-id="95771-271">Potvrzení změny do skupiny ochrany</span><span class="sxs-lookup"><span data-stu-id="95771-271">Committing the changes to the Protection Group</span></span>
<span data-ttu-id="95771-272">Nakonec změny je nutné před provedením aplikace DPM může provést zálohování na novou konfiguraci skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="95771-272">Finally, the changes need to be committed before DPM can take the backup per the new Protection Group configuration.</span></span> <span data-ttu-id="95771-273">Toho lze dosáhnout pomocí [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) rutiny.</span><span class="sxs-lookup"><span data-stu-id="95771-273">This can be achieved using the [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) cmdlet.</span></span>

```
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```
## <a name="view-the-backup-points"></a><span data-ttu-id="95771-274">Zobrazení body zálohy</span><span class="sxs-lookup"><span data-stu-id="95771-274">View the backup points</span></span>
<span data-ttu-id="95771-275">Můžete použít [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) rutiny se získat seznam všech bodů obnovení pro zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="95771-275">You can use the [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) cmdlet to get a list of all recovery points for a datasource.</span></span> <span data-ttu-id="95771-276">V tomto příkladu provedeme následující:</span><span class="sxs-lookup"><span data-stu-id="95771-276">In this example, we will:</span></span>

* <span data-ttu-id="95771-277">načtení všech PGs na serveru DPM a uloženy do pole```$PG```</span><span class="sxs-lookup"><span data-stu-id="95771-277">fetch all the PGs on the DPM server and stored in an array ```$PG```</span></span>
* <span data-ttu-id="95771-278">získání zdroje dat odpovídající```$PG[0]```</span><span class="sxs-lookup"><span data-stu-id="95771-278">get the datasources corresponding to the ```$PG[0]```</span></span>
* <span data-ttu-id="95771-279">získání všech bodů obnovení pro zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="95771-279">get all the recovery points for a datasource.</span></span>

```
PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online
```

## <a name="restore-data-protected-on-azure"></a><span data-ttu-id="95771-280">Obnovit data chráněná v Azure</span><span class="sxs-lookup"><span data-stu-id="95771-280">Restore data protected on Azure</span></span>
<span data-ttu-id="95771-281">Obnovení dat je kombinací ```RecoverableItem``` objektu a ```RecoveryOption``` objektu.</span><span class="sxs-lookup"><span data-stu-id="95771-281">Restoring data is a combination of a ```RecoverableItem``` object and a ```RecoveryOption``` object.</span></span> <span data-ttu-id="95771-282">V předchozí části My seznam body zálohy pro zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="95771-282">In the previous section, we got a list of the backup points for a datasource.</span></span>

<span data-ttu-id="95771-283">V následujícím příkladu jsme ukazují, jak k obnovení virtuálního počítače technologie Hyper-V z Azure Backup kombinací body zálohy s cíle pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="95771-283">In the example below, we demonstrate how to restore a Hyper-V virtual machine from Azure Backup by combining backup points with the target for recovery.</span></span> <span data-ttu-id="95771-284">Tento příklad obsahuje:</span><span class="sxs-lookup"><span data-stu-id="95771-284">This example includes:</span></span>

* <span data-ttu-id="95771-285">Vytváření možnost obnovení pomocí [New-DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) rutiny.</span><span class="sxs-lookup"><span data-stu-id="95771-285">Creating a recovery option using the  [New-DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) cmdlet.</span></span>
* <span data-ttu-id="95771-286">Načítání pole body zálohy pomocí ```Get-DPMRecoveryPoint``` rutiny.</span><span class="sxs-lookup"><span data-stu-id="95771-286">Fetching the array of backup points using the ```Get-DPMRecoveryPoint``` cmdlet.</span></span>
* <span data-ttu-id="95771-287">Výběr bod obnovení ze zálohy.</span><span class="sxs-lookup"><span data-stu-id="95771-287">Choosing a backup point to restore from.</span></span>

```
PS C:\> $RecoveryOption = New-DPMRecoveryOption -HyperVDatasource -TargetServer "HVDCenter02" -RecoveryLocation AlternateHyperVServer -RecoveryType Recover -TargetLocation “C:\VMRecovery”

PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online

PS C:\> Restore-DPMRecoverableItem -RecoverableItem $RecoveryPoints[0] -RecoveryOption $RecoveryOption
```

<span data-ttu-id="95771-288">Příkazy můžete snadno rozšířit pro jakýkoli typ datasource.</span><span class="sxs-lookup"><span data-stu-id="95771-288">The commands can easily be extended for any datasource type.</span></span>

## <a name="next-steps"></a><span data-ttu-id="95771-289">Další kroky</span><span class="sxs-lookup"><span data-stu-id="95771-289">Next steps</span></span>
* <span data-ttu-id="95771-290">Další informace o DPM pro zálohování Azure najdete v části [Úvod k zálohování aplikace DPM](backup-azure-dpm-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="95771-290">For more information about DPM to Azure Backup see [Introduction to DPM Backup](backup-azure-dpm-introduction.md)</span></span>
