---
title: "Zálohování Windows serveru do Azure pomocí prostředí PowerShell | Microsoft Docs"
description: "Zjistěte, jak nasadit a spravovat Azure Backup pomocí prostředí PowerShell"
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 65218095-2996-44d9-917b-8c84fc9ac415
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: saurse;markgal;jimpark;nkolli;trinadhk
ms.openlocfilehash: d3f165c749af0553c4918b33b0d24cc1e21af2a9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-and-manage-backup-to-azure-for-windows-serverwindows-client-using-powershell"></a><span data-ttu-id="6b788-103">Nasazení a správa zálohování do Azure pro servery Windows / klienty Windows pomocí PowerShellu</span><span class="sxs-lookup"><span data-stu-id="6b788-103">Deploy and manage backup to Azure for Windows Server/Windows Client using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6b788-104">ARM</span><span class="sxs-lookup"><span data-stu-id="6b788-104">ARM</span></span>](backup-client-automation.md)
> * [<span data-ttu-id="6b788-105">Classic</span><span class="sxs-lookup"><span data-stu-id="6b788-105">Classic</span></span>](backup-client-automation-classic.md)
>
>

<span data-ttu-id="6b788-106">Tento článek ukazuje, jak pomocí prostředí PowerShell pro nastavení služby Azure Backup na Windows serveru nebo klienta Windows a Správa zálohování a obnovení.</span><span class="sxs-lookup"><span data-stu-id="6b788-106">This article shows you how to use PowerShell for setting up Azure Backup on Windows Server or a Windows client, and managing backup and recovery.</span></span>

## <a name="install-azure-powershell"></a><span data-ttu-id="6b788-107">Instalace prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="6b788-107">Install Azure PowerShell</span></span>
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

<span data-ttu-id="6b788-108">Tento článek se zaměřuje na Azure Resource Manager (ARM) a rutiny MS Online zálohování Powershellu, které vám umožní používat Trezor služeb zotavení ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="6b788-108">This article focuses on the Azure Resource Manager (ARM) and the MS Online Backup PowerShell cmdlets that enable you to use a Recovery Services vault in a resource group.</span></span>

<span data-ttu-id="6b788-109">Října 2015 byla vydána Azure PowerShell 1.0.</span><span class="sxs-lookup"><span data-stu-id="6b788-109">In October 2015, Azure PowerShell 1.0 was released.</span></span> <span data-ttu-id="6b788-110">Tato verze 0.9.8 bylo úspěšně dokončeno. Uvolněte a uvést do režimu o významné změny, zejména v vzoru pro pojmenovávání rutin.</span><span class="sxs-lookup"><span data-stu-id="6b788-110">This release succeeded the 0.9.8 release and brought about some significant changes, especially in the naming pattern of the cmdlets.</span></span> <span data-ttu-id="6b788-111">Rutiny verze 1.0 dodržují formát pojmenování {sloveso}-AzureRm {podstatné jméno}, kdežto názvy ve verzi 0.9.8 neobsahují označení **Rm** (třeba New-AzureRmResourceGroup místo New-AzureResourceGroup).</span><span class="sxs-lookup"><span data-stu-id="6b788-111">1.0 cmdlets follow the naming pattern {verb}-AzureRm{noun}; whereas, the 0.9.8 names do not include **Rm** (for example, New-AzureRmResourceGroup instead of New-AzureResourceGroup).</span></span> <span data-ttu-id="6b788-112">Pokud používáte prostředí Azure PowerShell 0.9.8, musíte nejdřív spuštěním příkazu **Switch-AzureMode AzureResourceManager** spustit režim Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6b788-112">When using Azure PowerShell 0.9.8, you must first enable the Resource Manager mode by running the **Switch-AzureMode AzureResourceManager** command.</span></span> <span data-ttu-id="6b788-113">Tento příkaz není nutné v 1.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="6b788-113">This command is not necessary in 1.0 or later.</span></span>

<span data-ttu-id="6b788-114">Pokud chcete použít skripty napsané pro verzi 0.9.8 prostředí v prostředí 1.0 nebo novější, měli byste pečlivě aktualizace a testovat skripty v předprodukčním prostředí před jejich používání v produkčním prostředí předejdete neočekávané dopad.</span><span class="sxs-lookup"><span data-stu-id="6b788-114">If you want to use your scripts written for the 0.9.8 environment, in the 1.0 or later environment, you should carefully update and test the scripts in a pre-production environment before using them in production to avoid unexpected impact.</span></span>

<span data-ttu-id="6b788-115">[Stáhněte si nejnovější verzi prostředí PowerShell](https://github.com/Azure/azure-powershell/releases) (minimální požadovaná verze je: 1.0.0)</span><span class="sxs-lookup"><span data-stu-id="6b788-115">[Download the latest PowerShell release](https://github.com/Azure/azure-powershell/releases) (minimum version required is : 1.0.0)</span></span>

[!INCLUDE [arm-getting-setup-powershell](../../includes/arm-getting-setup-powershell.md)]

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="6b788-116">Vytvoření trezoru služby Recovery Services</span><span class="sxs-lookup"><span data-stu-id="6b788-116">Create a recovery services vault</span></span>
<span data-ttu-id="6b788-117">Následující kroky vás provedou vytvoření trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="6b788-117">The following steps lead you through creating a Recovery Services vault.</span></span> <span data-ttu-id="6b788-118">Trezor služeb zotavení se liší od úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="6b788-118">A Recovery Services vault is different than a Backup vault.</span></span>

1. <span data-ttu-id="6b788-119">Pokud používáte Azure Backup poprvé, musíte použít **Register-AzureRMResourceProvider** rutiny k registraci poskytovatele služeb zotavení Azure s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="6b788-119">If you are using Azure Backup for the first time, you must use the **Register-AzureRMResourceProvider** cmdlet to register the Azure Recovery Service provider with your subscription.</span></span>

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
2. <span data-ttu-id="6b788-120">Trezor služeb zotavení je prostředek ARM, proto musíte umístit do skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="6b788-120">The Recovery Services vault is an ARM resource, so you need to place it within a Resource Group.</span></span> <span data-ttu-id="6b788-121">Můžete použít existující skupinu prostředků nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="6b788-121">You can use an existing resource group, or create a new one.</span></span> <span data-ttu-id="6b788-122">Když vytváříte novou skupinu prostředků, zadejte název a umístění pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="6b788-122">When creating a new resource group, specify the name and location for the resource group.</span></span>  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "WestUS"
    ```
3. <span data-ttu-id="6b788-123">Použití **New-AzureRmRecoveryServicesVault** vytvořte nový trezor.</span><span class="sxs-lookup"><span data-stu-id="6b788-123">Use the **New-AzureRmRecoveryServicesVault** cmdlet to create the new vault.</span></span> <span data-ttu-id="6b788-124">Ujistěte se, že zadejte stejné umístění pro úložiště, jako byl použit pro skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="6b788-124">Be sure to specify the same location for the vault as was used for the resource group.</span></span>

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "WestUS"
    ```
4. <span data-ttu-id="6b788-125">Zadejte typ redundance úložiště se použije. můžete použít [místně redundantní úložiště (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) nebo [geograficky redundantní úložiště (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span><span class="sxs-lookup"><span data-stu-id="6b788-125">Specify the type of storage redundancy to use; you can use [Locally Redundant Storage (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) or [Geo Redundant Storage (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span></span> <span data-ttu-id="6b788-126">Následující příklad ukazuje, že je možnost - BackupStorageRedundancy pro testVault nastavena na GeoRedundant.</span><span class="sxs-lookup"><span data-stu-id="6b788-126">The following example shows the -BackupStorageRedundancy option for testVault is set to GeoRedundant.</span></span>

   > [!TIP]
   > <span data-ttu-id="6b788-127">Mnoho rutin Azure Backup vyžadují objekt trezoru služeb zotavení jako vstup.</span><span class="sxs-lookup"><span data-stu-id="6b788-127">Many Azure Backup cmdlets require the Recovery Services vault object as an input.</span></span> <span data-ttu-id="6b788-128">Z tohoto důvodu je vhodné pro uložení objektu trezoru služeb zotavení zálohování v proměnné.</span><span class="sxs-lookup"><span data-stu-id="6b788-128">For this reason, it is convenient to store the Backup Recovery Services vault object in a variable.</span></span>
   >
   >

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testVault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-the-vaults-in-a-subscription"></a><span data-ttu-id="6b788-129">Zobrazit trezorů v předplatném.</span><span class="sxs-lookup"><span data-stu-id="6b788-129">View the vaults in a subscription</span></span>
<span data-ttu-id="6b788-130">Použití **Get-AzureRmRecoveryServicesVault** Chcete-li zobrazit seznam všech trezorů v aktuálním předplatném.</span><span class="sxs-lookup"><span data-stu-id="6b788-130">Use **Get-AzureRmRecoveryServicesVault** to view the list of all vaults in the current subscription.</span></span> <span data-ttu-id="6b788-131">Tento příkaz můžete použít, chcete-li zkontrolovat, zda byl vytvořen nový trezor nebo jaké trezory jsou k dispozici v rámci předplatného.</span><span class="sxs-lookup"><span data-stu-id="6b788-131">You can use this command to check that a new  vault was created, or to see what vaults are available in the subscription.</span></span>

<span data-ttu-id="6b788-132">Spusťte příkaz, **Get-AzureRmRecoveryServicesVault**, a jsou uvedeny všechny trezorů v rámci předplatného.</span><span class="sxs-lookup"><span data-stu-id="6b788-132">Run the command, **Get-AzureRmRecoveryServicesVault**, and all vaults in the subscription are listed.</span></span>

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


## <a name="installing-the-azure-backup-agent"></a><span data-ttu-id="6b788-133">Instalace agenta Azure Backup</span><span class="sxs-lookup"><span data-stu-id="6b788-133">Installing the Azure Backup agent</span></span>
<span data-ttu-id="6b788-134">Před instalací agenta Azure Backup, musíte mít instalační program stažené a existuje v systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="6b788-134">Before you install the Azure Backup agent, you need to have the installer downloaded and present on the Windows Server.</span></span> <span data-ttu-id="6b788-135">Můžete získat nejnovější verzi instalačního programu z [Microsoft Download Center](http://aka.ms/azurebackup_agent) nebo ze stránky řídicího panelu trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="6b788-135">You can get the latest version of the installer from the [Microsoft Download Center](http://aka.ms/azurebackup_agent) or from the Recovery Services vault's Dashboard page.</span></span> <span data-ttu-id="6b788-136">Instalační program uložte na snadno dostupném místě jako * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="6b788-136">Save the installer to an easily accessible location like *C:\Downloads\*.</span></span>

<span data-ttu-id="6b788-137">Chcete-li získat nástroj pro stažení programu Alternativně pomocí prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="6b788-137">Alternatively, use PowerShell to get the downloader:</span></span>
 
 ```
 $MarsAURL = 'Http://Aka.Ms/Azurebackup_Agent'
 $WC = New-Object System.Net.WebClient
 $WC.DownloadFile($MarsAURL,'C:\downloads\MARSAgentInstaller.EXE')
 C:\Downloads\MARSAgentInstaller.EXE /q
 ```

<span data-ttu-id="6b788-138">Pokud chcete nainstalovat agenta, spusťte v konzolu prostředí PowerShell se zvýšenými oprávněními následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6b788-138">To install the agent, run the following command in an elevated PowerShell console:</span></span>

```
PS C:\> MARSAgentInstaller.exe /q
```

<span data-ttu-id="6b788-139">Tím se nainstaluje agent s výchozími možnostmi.</span><span class="sxs-lookup"><span data-stu-id="6b788-139">This installs the agent with all the default options.</span></span> <span data-ttu-id="6b788-140">Instalace trvá několik minut na pozadí.</span><span class="sxs-lookup"><span data-stu-id="6b788-140">The installation takes a few minutes in the background.</span></span> <span data-ttu-id="6b788-141">Pokud nezadáte */nu* možnost potom **Windows Update** otevře se okno na konci instalace k vyhledání všech aktualizací.</span><span class="sxs-lookup"><span data-stu-id="6b788-141">If you do not specify the */nu* option then the **Windows Update** window will open at the end of the installation to check for any updates.</span></span> <span data-ttu-id="6b788-142">Po instalaci agenta se zobrazí v seznamu nainstalovaných programů.</span><span class="sxs-lookup"><span data-stu-id="6b788-142">Once installed, the agent will show in the list of installed programs.</span></span>

<span data-ttu-id="6b788-143">Chcete-li zobrazit seznam nainstalovaných programů, přejděte na **ovládací panely** > **programy** > **programy a funkce**.</span><span class="sxs-lookup"><span data-stu-id="6b788-143">To see the list of installed programs, go to **Control Panel** > **Programs** > **Programs and Features**.</span></span>

![Instalaci agenta](./media/backup-client-automation/installed-agent-listing.png)

### <a name="installation-options"></a><span data-ttu-id="6b788-145">Možnosti instalace</span><span class="sxs-lookup"><span data-stu-id="6b788-145">Installation options</span></span>
<span data-ttu-id="6b788-146">Pokud chcete zobrazit všechny možnosti, které jsou k dispozici prostřednictvím příkazového řádku, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6b788-146">To see all the options available via the command-line, use the following command:</span></span>

```
PS C:\> MARSAgentInstaller.exe /?
```

<span data-ttu-id="6b788-147">Mezi dostupné možnosti patří:</span><span class="sxs-lookup"><span data-stu-id="6b788-147">The available options include:</span></span>

| <span data-ttu-id="6b788-148">Možnost</span><span class="sxs-lookup"><span data-stu-id="6b788-148">Option</span></span> | <span data-ttu-id="6b788-149">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="6b788-149">Details</span></span> | <span data-ttu-id="6b788-150">Výchozí</span><span class="sxs-lookup"><span data-stu-id="6b788-150">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6b788-151">/q</span><span class="sxs-lookup"><span data-stu-id="6b788-151">/q</span></span> |<span data-ttu-id="6b788-152">Tichá instalace</span><span class="sxs-lookup"><span data-stu-id="6b788-152">Quiet installation</span></span> |- |
| <span data-ttu-id="6b788-153">/ p: "umístění"</span><span class="sxs-lookup"><span data-stu-id="6b788-153">/p:"location"</span></span> |<span data-ttu-id="6b788-154">Cesta ke složce instalace pro agenta Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="6b788-154">Path to the installation folder for the Azure Backup agent.</span></span> |<span data-ttu-id="6b788-155">C:\Program Files\Microsoft Azure Recovery Services agenta</span><span class="sxs-lookup"><span data-stu-id="6b788-155">C:\Program Files\Microsoft Azure Recovery Services Agent</span></span> |
| <span data-ttu-id="6b788-156">/ s: "umístění"</span><span class="sxs-lookup"><span data-stu-id="6b788-156">/s:"location"</span></span> |<span data-ttu-id="6b788-157">Cesta ke složce mezipaměti pro agenta Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="6b788-157">Path to the cache folder for the Azure Backup agent.</span></span> |<span data-ttu-id="6b788-158">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span><span class="sxs-lookup"><span data-stu-id="6b788-158">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span></span> |
| <span data-ttu-id="6b788-159">/m</span><span class="sxs-lookup"><span data-stu-id="6b788-159">/m</span></span> |<span data-ttu-id="6b788-160">Výslovný souhlas ke službě Microsoft Update</span><span class="sxs-lookup"><span data-stu-id="6b788-160">Opt-in to Microsoft Update</span></span> |- |
| <span data-ttu-id="6b788-161">/nu</span><span class="sxs-lookup"><span data-stu-id="6b788-161">/nu</span></span> |<span data-ttu-id="6b788-162">Nekontrolovat aktualizace po dokončení instalace</span><span class="sxs-lookup"><span data-stu-id="6b788-162">Do not Check for updates after installation is complete</span></span> |- |
| <span data-ttu-id="6b788-163">/d</span><span class="sxs-lookup"><span data-stu-id="6b788-163">/d</span></span> |<span data-ttu-id="6b788-164">Odinstaluje Agenta Microsoft Azure Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="6b788-164">Uninstalls Microsoft Azure Recovery Services Agent</span></span> |- |
| <span data-ttu-id="6b788-165">/pH</span><span class="sxs-lookup"><span data-stu-id="6b788-165">/ph</span></span> |<span data-ttu-id="6b788-166">Adresa proxy hostitele</span><span class="sxs-lookup"><span data-stu-id="6b788-166">Proxy Host Address</span></span> |- |
| <span data-ttu-id="6b788-167">/Po</span><span class="sxs-lookup"><span data-stu-id="6b788-167">/po</span></span> |<span data-ttu-id="6b788-168">Číslo portu proxy hostitele</span><span class="sxs-lookup"><span data-stu-id="6b788-168">Proxy Host Port Number</span></span> |- |
| <span data-ttu-id="6b788-169">/Pu</span><span class="sxs-lookup"><span data-stu-id="6b788-169">/pu</span></span> |<span data-ttu-id="6b788-170">Uživatelské jméno proxy hostitele</span><span class="sxs-lookup"><span data-stu-id="6b788-170">Proxy Host UserName</span></span> |- |
| <span data-ttu-id="6b788-171">/pW</span><span class="sxs-lookup"><span data-stu-id="6b788-171">/pw</span></span> |<span data-ttu-id="6b788-172">Heslo pro proxy server</span><span class="sxs-lookup"><span data-stu-id="6b788-172">Proxy Password</span></span> |- |

## <a name="registering-windows-server-or-windows-client-machine-to-a-recovery-services-vault"></a><span data-ttu-id="6b788-173">Registrace systému Windows Server a klientský počítač systému Windows do trezoru služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="6b788-173">Registering Windows Server or Windows client machine to a Recovery Services Vault</span></span>
<span data-ttu-id="6b788-174">Po vytvoření trezoru služeb zotavení, stáhněte si nejnovější verzi agenta a přihlašovací údaje trezoru a uložte ho do vhodného umístění jako C:\Downloads.</span><span class="sxs-lookup"><span data-stu-id="6b788-174">After you created the Recovery Services vault, download the latest agent and the vault credentials and store it in a convenient location like C:\Downloads.</span></span>

```
PS C:\> $credspath = "C:\downloads"
PS C:\> $credsfilename = Get-AzureRmRecoveryServicesVaultSettingsFile -Backup -Vault $vault1 -Path  $credspath
```

<span data-ttu-id="6b788-175">V systému Windows Server nebo klientský počítač systému Windows, spusťte [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) rutiny počítač zaregistrovat v trezoru.</span><span class="sxs-lookup"><span data-stu-id="6b788-175">On the Windows Server or Windows client machine, run the [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) cmdlet to register the machine with the vault.</span></span>
<span data-ttu-id="6b788-176">Tato a další rutiny používané pro zálohování, jsou z MSONLINE modul, který Mars AgentInstaller přidat jako součást procesu instalace.</span><span class="sxs-lookup"><span data-stu-id="6b788-176">This, and other cmdlets used for backup, are from the MSONLINE module which the Mars AgentInstaller added as part of the installation process.</span></span> 

<span data-ttu-id="6b788-177">Instalační program agenta nelze aktualizovat $Env: PSModulePath proměnné.</span><span class="sxs-lookup"><span data-stu-id="6b788-177">The Agent installer does not update the $Env:PSModulePath variable.</span></span> <span data-ttu-id="6b788-178">To znamená, že automaticky načíst modul selže.</span><span class="sxs-lookup"><span data-stu-id="6b788-178">This means module auto-load fails.</span></span> <span data-ttu-id="6b788-179">Tento problém vyřešíte tak můžete provést následující:</span><span class="sxs-lookup"><span data-stu-id="6b788-179">To resolve this you can do the following:</span></span>

```
PS C:\>  $Env:psmodulepath += ';C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules
```

<span data-ttu-id="6b788-180">Alternativně můžete ručně načíst modul ve vašem skriptu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="6b788-180">Alternatively, you can manually load the module in your script as follows:</span></span>

```
PS C:\>  Import-Module  'C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules\MSOnlineBackup'

```

<span data-ttu-id="6b788-181">Po načtení rutiny Online zálohování, zaregistrujte se přihlašovací údaje úložiště:</span><span class="sxs-lookup"><span data-stu-id="6b788-181">Once you load the Online Backup cmdlets, you register the vault credentials:</span></span>


```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-OBRegistration-VaultCredentials $cred -Confirm:$false
CertThumbprint      :7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName: testvault
Region              :WestUS
Machine registration succeeded.
```

> [!IMPORTANT]
> <span data-ttu-id="6b788-182">Nepoužívejte relativní cesty k určení souboru s přihlašovacími údaji.</span><span class="sxs-lookup"><span data-stu-id="6b788-182">Do not use relative paths to specify the vault credentials file.</span></span> <span data-ttu-id="6b788-183">Jako vstup do rutiny, musí zadat absolutní cestu.</span><span class="sxs-lookup"><span data-stu-id="6b788-183">You must provide an absolute path as an input to the cmdlet.</span></span>
>
>

## <a name="networking-settings"></a><span data-ttu-id="6b788-184">Nastavení sítě</span><span class="sxs-lookup"><span data-stu-id="6b788-184">Networking settings</span></span>
<span data-ttu-id="6b788-185">Pokud připojení Windows počítače k Internetu prostřednictvím proxy serveru, nastavení proxy serveru se dá zadat i k agentovi.</span><span class="sxs-lookup"><span data-stu-id="6b788-185">When the connectivity of the Windows machine to the internet is through a proxy server, the proxy settings can also be provided to the agent.</span></span> <span data-ttu-id="6b788-186">V tomto příkladu neexistuje žádný proxy server, takže jsme jsou explicitně vymazání údaje související s proxy serverem.</span><span class="sxs-lookup"><span data-stu-id="6b788-186">In this example, there is no proxy server, so we are explicitly clearing any proxy-related information.</span></span>

<span data-ttu-id="6b788-187">Využití šířky pásma se dá taky nastavit pomocí možnosti ```work hour bandwidth``` a ```non-work hour bandwidth``` pro danou sadu dny v týdnu.</span><span class="sxs-lookup"><span data-stu-id="6b788-187">Bandwidth usage can also be controlled with the options of ```work hour bandwidth``` and ```non-work hour bandwidth``` for a given set of days of the week.</span></span>

<span data-ttu-id="6b788-188">Nastavení serveru proxy a šířky pásma podrobnosti se provádí pomocí [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) rutiny:</span><span class="sxs-lookup"><span data-stu-id="6b788-188">Setting the proxy and bandwidth details is done using the [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) cmdlet:</span></span>

```
PS C:\> Set-OBMachineSetting -NoProxy
Server properties updated successfully.

PS C:\> Set-OBMachineSetting -NoThrottle
Server properties updated successfully.
```

## <a name="encryption-settings"></a><span data-ttu-id="6b788-189">Nastavení šifrování</span><span class="sxs-lookup"><span data-stu-id="6b788-189">Encryption settings</span></span>
<span data-ttu-id="6b788-190">Zálohování data přenášená do služby Azure Backup je zašifrovaná chránit jejich důvěrnost dat.</span><span class="sxs-lookup"><span data-stu-id="6b788-190">The backup data sent to Azure Backup is encrypted to protect the confidentiality of the data.</span></span> <span data-ttu-id="6b788-191">Šifrovací přístupové heslo je 'heslo' k dešifrování dat v době obnovení.</span><span class="sxs-lookup"><span data-stu-id="6b788-191">The encryption passphrase is the "password" to decrypt the data at the time of restore.</span></span>

```
PS C:\> ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force | Set-OBMachineSetting
PS C:\> $PassPhrase = ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force 
PS C:\> $PassCode   = 'AzureR0ckx'
PS C:\> Set-OBMachineSetting -EncryptionPassPhrase $PassPhrase
Server properties updated successfully
```

> [!IMPORTANT]
> <span data-ttu-id="6b788-192">Zachování informací o přístupové heslo bezpečném, po nastavení.</span><span class="sxs-lookup"><span data-stu-id="6b788-192">Keep the passphrase information safe and secure once it is set.</span></span> <span data-ttu-id="6b788-193">Není možné obnovit data ze služby Azure bez tohoto hesla.</span><span class="sxs-lookup"><span data-stu-id="6b788-193">You are not be able to restore data from Azure without this passphrase.</span></span>
>
>

## <a name="back-up-files-and-folders"></a><span data-ttu-id="6b788-194">Zálohování souborů a složek</span><span class="sxs-lookup"><span data-stu-id="6b788-194">Back up files and folders</span></span>
<span data-ttu-id="6b788-195">Zásady se řídí všechny zálohy ze Windows serverů a klientů do služby Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="6b788-195">All backups from Windows Servers and clients to Azure Backup are governed by a policy.</span></span> <span data-ttu-id="6b788-196">Zásady se skládá ze tří částí:</span><span class="sxs-lookup"><span data-stu-id="6b788-196">The policy comprises three parts:</span></span>

1. <span data-ttu-id="6b788-197">A **plán zálohování** určující při zálohování muset provést a synchronizovat se službou.</span><span class="sxs-lookup"><span data-stu-id="6b788-197">A **backup schedule** that specifies when backups need to be taken and synchronized with the service.</span></span>
2. <span data-ttu-id="6b788-198">A **plán uchovávání informací** který určuje, jak dlouho chcete zachovat body obnovení v Azure.</span><span class="sxs-lookup"><span data-stu-id="6b788-198">A **retention schedule** that specifies how long to retain the recovery points in Azure.</span></span>
3. <span data-ttu-id="6b788-199">A **zadání zahrnutí nebo vyloučení souboru** který určuje, co by měl být zálohovány.</span><span class="sxs-lookup"><span data-stu-id="6b788-199">A **file inclusion/exclusion specification** that dictates what should be backed up.</span></span>

<span data-ttu-id="6b788-200">V tomto dokumentu protože jsme se automatizaci zálohování, budete předpokládáme, že nic nebyl nakonfigurován.</span><span class="sxs-lookup"><span data-stu-id="6b788-200">In this document, since we're automating backup, we'll assume nothing has been configured.</span></span> <span data-ttu-id="6b788-201">Začneme vytvořením nové zásady zálohování pomocí [New-OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="6b788-201">We begin by creating a new backup policy using the [New-OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) cmdlet.</span></span>

```
PS C:\> $newpolicy = New-OBPolicy
```

<span data-ttu-id="6b788-202">V tuto chvíli je prázdné zásady a ostatní rutiny jsou potřebné k definování položky, které je budou zahrnuté ani vyloučené, při zálohování se spustí, a tam, kde se uloží v zálohování.</span><span class="sxs-lookup"><span data-stu-id="6b788-202">At this time the policy is empty and other cmdlets are needed to define what items will be included or excluded, when backups will run, and where the backups will be stored.</span></span>

### <a name="configuring-the-backup-schedule"></a><span data-ttu-id="6b788-203">Konfigurace plánu zálohování</span><span class="sxs-lookup"><span data-stu-id="6b788-203">Configuring the backup schedule</span></span>
<span data-ttu-id="6b788-204">První den 3 části zásada je plán zálohování, která je vytvořena pomocí [New-OBSchedule](https://technet.microsoft.com/library/hh770401) rutiny.</span><span class="sxs-lookup"><span data-stu-id="6b788-204">The first of the 3 parts of a policy is the backup schedule, which is created using the [New-OBSchedule](https://technet.microsoft.com/library/hh770401) cmdlet.</span></span> <span data-ttu-id="6b788-205">Plán zálohování definuje, kdy je třeba přijmout zálohy.</span><span class="sxs-lookup"><span data-stu-id="6b788-205">The backup schedule defines when backups need to be taken.</span></span> <span data-ttu-id="6b788-206">Při vytváření plánu je třeba zadat 2 vstupní parametry:</span><span class="sxs-lookup"><span data-stu-id="6b788-206">When creating a schedule you need to specify 2 input parameters:</span></span>

* <span data-ttu-id="6b788-207">**Dny v týdnu** , má-li spustit zálohování.</span><span class="sxs-lookup"><span data-stu-id="6b788-207">**Days of the week** that the backup should run.</span></span> <span data-ttu-id="6b788-208">Můžete spustit úlohu zálohování na právě jeden den nebo každý den v týdnu nebo libovolnou kombinaci mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="6b788-208">You can run the backup job on just one day, or every day of the week, or any combination in between.</span></span>
* <span data-ttu-id="6b788-209">**Časy dne** spuštění zálohování.</span><span class="sxs-lookup"><span data-stu-id="6b788-209">**Times of the day** when the backup should run.</span></span> <span data-ttu-id="6b788-210">Můžete definovat až 3 různou dobu, kdy se spustí zálohování.</span><span class="sxs-lookup"><span data-stu-id="6b788-210">You can define up to 3 different times of the day when the backup will be triggered.</span></span>

<span data-ttu-id="6b788-211">Například může nakonfigurovat zásady zálohování, který se spustí na 16: 00 každou sobotu a neděli.</span><span class="sxs-lookup"><span data-stu-id="6b788-211">For instance, you could configure a backup policy that runs at 4PM every Saturday and Sunday.</span></span>

```
PS C:\> $sched = New-OBSchedule -DaysofWeek Saturday, Sunday -TimesofDay 16:00
```

<span data-ttu-id="6b788-212">Plán zálohování musí být přidružený k zásadě, a to jde dosáhnout pomocí [Set-OBSchedule](https://technet.microsoft.com/library/hh770407) rutiny.</span><span class="sxs-lookup"><span data-stu-id="6b788-212">The backup schedule needs to be associated with a policy, and this can be achieved by using the [Set-OBSchedule](https://technet.microsoft.com/library/hh770407) cmdlet.</span></span>

```
PS C:> Set-OBSchedule -Policy $newpolicy -Schedule $sched
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s) DsList : PolicyName : RetentionPolicy : State : New PolicyState : Valid
```
### <a name="configuring-a-retention-policy"></a><span data-ttu-id="6b788-213">Konfigurace zásad uchovávání</span><span class="sxs-lookup"><span data-stu-id="6b788-213">Configuring a retention policy</span></span>
<span data-ttu-id="6b788-214">Zásady uchovávání informací definuje, jak dlouho se uchovají body obnovení vytvořené z úlohy zálohování.</span><span class="sxs-lookup"><span data-stu-id="6b788-214">The retention policy defines how long recovery points created from backup jobs are retained.</span></span> <span data-ttu-id="6b788-215">Při vytváření nové zásady uchovávání informací pomocí [New-OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) rutiny, můžete zadat počet dnů, které je třeba body obnovení záloh pro zachování s Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="6b788-215">When creating a new retention policy using the [New-OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) cmdlet, you can specify the number of days that the backup recovery points need to be retained with Azure Backup.</span></span> <span data-ttu-id="6b788-216">Následující příklad nastaví zásady uchovávání informací 7 dní.</span><span class="sxs-lookup"><span data-stu-id="6b788-216">The example below sets a retention policy of 7 days.</span></span>

```
PS C:\> $retentionpolicy = New-OBRetentionPolicy -RetentionDays 7
```

<span data-ttu-id="6b788-217">Zásady uchovávání informací musí být přidružené hlavní zásadám, pomocí rutiny [Set-OBRetentionPolicy](https://technet.microsoft.com/library/hh770405):</span><span class="sxs-lookup"><span data-stu-id="6b788-217">The retention policy must be associated with the main policy using the cmdlet [Set-OBRetentionPolicy](https://technet.microsoft.com/library/hh770405):</span></span>

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
### <a name="including-and-excluding-files-to-be-backed-up"></a><span data-ttu-id="6b788-218">Zahrnutí a vyloučení souborů k zálohování</span><span class="sxs-lookup"><span data-stu-id="6b788-218">Including and excluding files to be backed up</span></span>
<span data-ttu-id="6b788-219">```OBFileSpec``` Objekt definuje soubory zahrnout a vyloučit zálohu.</span><span class="sxs-lookup"><span data-stu-id="6b788-219">An ```OBFileSpec``` object defines the files to be included and excluded in a backup.</span></span> <span data-ttu-id="6b788-220">To je sada pravidel, která obor chráněné soubory a složky na počítači.</span><span class="sxs-lookup"><span data-stu-id="6b788-220">This is a set of rules that scope out the protected files and folders on a machine.</span></span> <span data-ttu-id="6b788-221">Můžete mít mnoho souboru pravidla zahrnutí nebo vyloučení podle potřeby a přidružovat je k zásadě.</span><span class="sxs-lookup"><span data-stu-id="6b788-221">You can have as many file inclusion or exclusion rules as required, and associate them with a policy.</span></span> <span data-ttu-id="6b788-222">Při vytváření nového objektu OBFileSpec, můžete:</span><span class="sxs-lookup"><span data-stu-id="6b788-222">When creating a new OBFileSpec object, you can:</span></span>

* <span data-ttu-id="6b788-223">Zadejte soubory a složky, které mají být zahrnuty</span><span class="sxs-lookup"><span data-stu-id="6b788-223">Specify the files and folders to be included</span></span>
* <span data-ttu-id="6b788-224">Zadejte soubory a složky, které se mají vyloučit</span><span class="sxs-lookup"><span data-stu-id="6b788-224">Specify the files and folders to be excluded</span></span>
* <span data-ttu-id="6b788-225">Zadejte rekurzivní zálohování dat v složku (nebo) zda pouze nejvyšší úrovně soubory v zadané složce by měl být zálohuje nahoru.</span><span class="sxs-lookup"><span data-stu-id="6b788-225">Specify recursive backup of data in a folder (or) whether only the top-level files in the specified folder should be backed up.</span></span>

<span data-ttu-id="6b788-226">K tomu se dosáhne použitím příznaku - nerekurzivní v příkazu New-OBFileSpec.</span><span class="sxs-lookup"><span data-stu-id="6b788-226">The latter is achieved by using the -NonRecursive flag in the New-OBFileSpec command.</span></span>

<span data-ttu-id="6b788-227">V následujícím příkladu jsme budete zálohování svazku C: a D: a vyloučit binární soubory operačního systému ve složce Windows a všechny dočasné složky.</span><span class="sxs-lookup"><span data-stu-id="6b788-227">In the example below, we'll back up volume C: and D: and exclude the OS binaries in the Windows folder and any temporary folders.</span></span> <span data-ttu-id="6b788-228">K tomu vytvoříme dvě specifikace pomocí souboru [New-OBFileSpec](https://technet.microsoft.com/library/hh770408) rutiny – jeden pro zahrnutí a jeden pro vyloučení.</span><span class="sxs-lookup"><span data-stu-id="6b788-228">To do so we'll create two file specifications using the [New-OBFileSpec](https://technet.microsoft.com/library/hh770408) cmdlet - one for inclusion and one for exclusion.</span></span> <span data-ttu-id="6b788-229">Po vytvoření souboru specifikace jsou spojené s použitím zásad [přidat OBFileSpec](https://technet.microsoft.com/library/hh770424) rutiny.</span><span class="sxs-lookup"><span data-stu-id="6b788-229">Once the file specifications have been created, they're associated with the policy using the [Add-OBFileSpec](https://technet.microsoft.com/library/hh770424) cmdlet.</span></span>

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

### <a name="applying-the-policy"></a><span data-ttu-id="6b788-230">Použití zásad</span><span class="sxs-lookup"><span data-stu-id="6b788-230">Applying the policy</span></span>
<span data-ttu-id="6b788-231">Objekt zásad teď dokončení a má přidružené plánu zálohování, zásady uchovávání informací a seznam souborů zahrnutí nebo vyloučení.</span><span class="sxs-lookup"><span data-stu-id="6b788-231">Now the policy object is complete and has an associated backup schedule, retention policy, and an inclusion/exclusion list of files.</span></span> <span data-ttu-id="6b788-232">Tato zásada může být nyní potvrdit pro zálohování Azure používat.</span><span class="sxs-lookup"><span data-stu-id="6b788-232">This policy can now be committed for Azure Backup to use.</span></span> <span data-ttu-id="6b788-233">Před použitím nově vytvořenou zásadu zajistěte, aby žádné existující zásady zálohování, které jsou přidružené k serveru pomocí [odebrat OBPolicy](https://technet.microsoft.com/library/hh770415) rutiny.</span><span class="sxs-lookup"><span data-stu-id="6b788-233">Before you apply the newly created policy ensure that there are no existing backup policies associated with the server by using the [Remove-OBPolicy](https://technet.microsoft.com/library/hh770415) cmdlet.</span></span> <span data-ttu-id="6b788-234">Odebrání zásady zobrazí výzvu k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="6b788-234">Removing the policy will prompt for confirmation.</span></span> <span data-ttu-id="6b788-235">Přeskočit použití potvrzení ```-Confirm:$false``` příznak pomocí rutiny.</span><span class="sxs-lookup"><span data-stu-id="6b788-235">To skip the confirmation use the ```-Confirm:$false``` flag with the cmdlet.</span></span>

```
PS C:> Get-OBPolicy | Remove-OBPolicy
Microsoft Azure Backup Are you sure you want to remove this backup policy? This will delete all the backed up data. [Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default is "Y"):
```

<span data-ttu-id="6b788-236">Potvrzení objektu zásad se provádí pomocí [Set-OBPolicy](https://technet.microsoft.com/library/hh770421) rutiny.</span><span class="sxs-lookup"><span data-stu-id="6b788-236">Committing the policy object is done using the [Set-OBPolicy](https://technet.microsoft.com/library/hh770421) cmdlet.</span></span> <span data-ttu-id="6b788-237">Také to požádá o potvrzení.</span><span class="sxs-lookup"><span data-stu-id="6b788-237">This will also ask for confirmation.</span></span> <span data-ttu-id="6b788-238">Přeskočit použití potvrzení ```-Confirm:$false``` příznak pomocí rutiny.</span><span class="sxs-lookup"><span data-stu-id="6b788-238">To skip the confirmation use the ```-Confirm:$false``` flag with the cmdlet.</span></span>

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

<span data-ttu-id="6b788-239">Můžete zobrazit podrobnosti o existující zásady zálohování pomocí [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) rutiny.</span><span class="sxs-lookup"><span data-stu-id="6b788-239">You can view the details of the existing backup policy using the [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) cmdlet.</span></span> <span data-ttu-id="6b788-240">Vám může procházení další pomocí [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) rutina pro plán zálohování a [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) rutina pro zásady uchovávání informací</span><span class="sxs-lookup"><span data-stu-id="6b788-240">You can drill-down further using the [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) cmdlet for the backup schedule and the [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) cmdlet for the retention policies</span></span>

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

### <a name="performing-an-ad-hoc-backup"></a><span data-ttu-id="6b788-241">Provádění zálohu ad-hoc</span><span class="sxs-lookup"><span data-stu-id="6b788-241">Performing an ad-hoc backup</span></span>
<span data-ttu-id="6b788-242">Jakmile byla nastavena zásada zálohování dojde k zálohování podle plánu.</span><span class="sxs-lookup"><span data-stu-id="6b788-242">Once a backup policy has been set the backups will occur per the schedule.</span></span> <span data-ttu-id="6b788-243">Aktivuje zálohu ad-hoc je také možné pomocí [Start-OBBackup](https://technet.microsoft.com/library/hh770426) rutiny:</span><span class="sxs-lookup"><span data-stu-id="6b788-243">Triggering an ad-hoc backup is also possible using the [Start-OBBackup](https://technet.microsoft.com/library/hh770426) cmdlet:</span></span>

```
PS C:> Get-OBPolicy | Start-OBBackup
Initializing
Taking snapshot of volumes...
Preparing storage...
Generating backup metadata information and preparing the metadata VHD...
Data transfer is in progress. It might take longer since it is the first backup and all data needs to be transferred...
Data transfer completed and all backed up data is in the cloud. Verifying data integrity...
Data transfer completed
In progress...
Job completed.
The backup operation completed successfully.
```

## <a name="restore-data-from-azure-backup"></a><span data-ttu-id="6b788-244">Obnovení dat z Azure Backup</span><span class="sxs-lookup"><span data-stu-id="6b788-244">Restore data from Azure Backup</span></span>
<span data-ttu-id="6b788-245">Tato část vás provede kroky pro automatizaci obnovení dat z Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="6b788-245">This section will guide you through the steps for automating recovery of data from Azure Backup.</span></span> <span data-ttu-id="6b788-246">Díky tomu zahrnuje následující kroky:</span><span class="sxs-lookup"><span data-stu-id="6b788-246">Doing so involves the following steps:</span></span>

1. <span data-ttu-id="6b788-247">Vyberte zdrojový svazek</span><span class="sxs-lookup"><span data-stu-id="6b788-247">Pick the source volume</span></span>
2. <span data-ttu-id="6b788-248">Vyberte bod obnovení zálohy</span><span class="sxs-lookup"><span data-stu-id="6b788-248">Choose a backup point to restore</span></span>
3. <span data-ttu-id="6b788-249">Vyberte položku, kterou chcete obnovit</span><span class="sxs-lookup"><span data-stu-id="6b788-249">Choose an item to restore</span></span>
4. <span data-ttu-id="6b788-250">Aktivační událost v procesu obnovení</span><span class="sxs-lookup"><span data-stu-id="6b788-250">Trigger the restore process</span></span>

### <a name="picking-the-source-volume"></a><span data-ttu-id="6b788-251">Výběr zdrojovém svazku</span><span class="sxs-lookup"><span data-stu-id="6b788-251">Picking the source volume</span></span>
<span data-ttu-id="6b788-252">Chcete-li obnovit položky z Azure Backup, musíte nejprve k identifikaci zdroje položky.</span><span class="sxs-lookup"><span data-stu-id="6b788-252">In order to restore an item from Azure Backup, you first need to identify the source of the item.</span></span> <span data-ttu-id="6b788-253">Vzhledem k tomu, že jsme se provádění příkazů v kontextu systému Windows Server nebo klienta Windows, počítač je už definovaný.</span><span class="sxs-lookup"><span data-stu-id="6b788-253">Since we're executing the commands in the context of a Windows Server or a Windows client, the machine is already identified.</span></span> <span data-ttu-id="6b788-254">Dalším krokem při identifikaci zdroj je k identifikaci svazku, který ji obsahuje.</span><span class="sxs-lookup"><span data-stu-id="6b788-254">The next step in identifying the source is to identify the volume containing it.</span></span> <span data-ttu-id="6b788-255">Seznam svazků nebo zdrojů zálohovaných z tohoto počítače se dá načíst spuštěním [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) rutiny.</span><span class="sxs-lookup"><span data-stu-id="6b788-255">A list of volumes or sources being backed up from this machine can be retrieved by executing the [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) cmdlet.</span></span> <span data-ttu-id="6b788-256">Tento příkaz vrátí pole všech zdrojů zálohovat z tohoto serveru nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="6b788-256">This command returns an array of all the sources backed up from this server/client.</span></span>

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

### <a name="choosing-a-backup-point-from-which-to-restore"></a><span data-ttu-id="6b788-257">Výběr bodu zálohy pro obnovení</span><span class="sxs-lookup"><span data-stu-id="6b788-257">Choosing a backup point from which to restore</span></span>
<span data-ttu-id="6b788-258">Načíst seznam body zálohy spuštěním [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) rutiny s příslušnými parametry.</span><span class="sxs-lookup"><span data-stu-id="6b788-258">You retreive a list of backup points by executing the [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdlet with appropriate parameters.</span></span> <span data-ttu-id="6b788-259">V našem příkladu jsme vybrali nejnovější bod zálohy pro zdrojový svazek *D:* a použít ho k obnovení konkrétní soubor.</span><span class="sxs-lookup"><span data-stu-id="6b788-259">In our example, we’ll choose the latest backup point for the source volume *D:* and use it to recover a specific file.</span></span>

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
<span data-ttu-id="6b788-260">Objekt ```$rps``` je pole body zálohy.</span><span class="sxs-lookup"><span data-stu-id="6b788-260">The object ```$rps``` is an array of backup points.</span></span> <span data-ttu-id="6b788-261">První prvek je poslední bod a n-tou elementu bude nejstarší bod.</span><span class="sxs-lookup"><span data-stu-id="6b788-261">The first element is the latest point and the Nth element is the oldest point.</span></span> <span data-ttu-id="6b788-262">Zvolte nejnovější bod, použijeme ```$rps[0]```.</span><span class="sxs-lookup"><span data-stu-id="6b788-262">To choose the latest point, we will use ```$rps[0]```.</span></span>

### <a name="choosing-an-item-to-restore"></a><span data-ttu-id="6b788-263">Vybrat položku, kterou chcete obnovit</span><span class="sxs-lookup"><span data-stu-id="6b788-263">Choosing an item to restore</span></span>
<span data-ttu-id="6b788-264">K identifikaci přesný soubor nebo složku pro obnovení, použijte rekurzivně [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="6b788-264">To identify the exact file or folder to restore, recursively use the [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdlet.</span></span> <span data-ttu-id="6b788-265">Tímto způsobem lze procházet hierarchii složek výhradně pomocí ```Get-OBRecoverableItem```.</span><span class="sxs-lookup"><span data-stu-id="6b788-265">That way the folder hierarchy can be browsed solely using the ```Get-OBRecoverableItem```.</span></span>

<span data-ttu-id="6b788-266">V tomto příkladu, pokud chcete obnovit soubor *finances.xls* jsme může odkazovat, který používá objekt ```$filesFolders[1]```.</span><span class="sxs-lookup"><span data-stu-id="6b788-266">In this example, if we want to restore the file *finances.xls* we can reference that using the object ```$filesFolders[1]```.</span></span>

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

<span data-ttu-id="6b788-267">Můžete také vyhledat položky k obnovení pomocí ```Get-OBRecoverableItem``` rutiny.</span><span class="sxs-lookup"><span data-stu-id="6b788-267">You can also search for items to restore using the ```Get-OBRecoverableItem``` cmdlet.</span></span> <span data-ttu-id="6b788-268">V našem příkladu, k vyhledání *finances.xls* nám může získat popisovač v souboru tak, že spustíte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="6b788-268">In our example, to search for *finances.xls* we could get a handle on the file by running this command:</span></span>

```
PS C:\> $item = Get-OBRecoverableItem -RecoveryPoint $rps[0] -Location "D:\MyData" -SearchString "finance*"
```

### <a name="triggering-the-restore-process"></a><span data-ttu-id="6b788-269">Spuštění procesu obnovení</span><span class="sxs-lookup"><span data-stu-id="6b788-269">Triggering the restore process</span></span>
<span data-ttu-id="6b788-270">K aktivaci v procesu obnovení, musíme nejprve nastavte možnosti obnovení.</span><span class="sxs-lookup"><span data-stu-id="6b788-270">To trigger the restore process, we first need to specify the recovery options.</span></span> <span data-ttu-id="6b788-271">To můžete provést pomocí [New-OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="6b788-271">This can be done by using the [New-OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) cmdlet.</span></span> <span data-ttu-id="6b788-272">Pro tento příklad předpokládejme, že chceme obnovit soubory *C:\temp*. Také Předpokládejme, že chceme přeskočit soubory, které již existují na cílovou složku *C:\temp*. Pokud chcete vytvořit možnost obnovení, použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6b788-272">For this example, let's assume that we want to restore the files to *C:\temp*. Let's also assume that we want to skip files that already exist on the destination folder *C:\temp*. To create such a recovery option, use the following command:</span></span>

```
PS C:\> $recovery_option = New-OBRecoveryOption -DestinationPath "C:\temp" -OverwriteType Skip
```

<span data-ttu-id="6b788-273">Teď spustit proces obnovení pomocí [Start-OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) příkaz na vybraném ```$item``` z výstupu ```Get-OBRecoverableItem``` rutiny:</span><span class="sxs-lookup"><span data-stu-id="6b788-273">Now trigger the restore process by using the [Start-OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) command on the selected ```$item``` from the output of the ```Get-OBRecoverableItem``` cmdlet:</span></span>

```
PS C:\> Start-OBRecovery -RecoverableItem $item -RecoveryOption $recover_option
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Job completed.
The recovery operation completed successfully.
```


## <a name="uninstalling-the-azure-backup-agent"></a><span data-ttu-id="6b788-274">Odinstalace agenta Azure Backup</span><span class="sxs-lookup"><span data-stu-id="6b788-274">Uninstalling the Azure Backup agent</span></span>
<span data-ttu-id="6b788-275">Odinstalace agenta Azure Backup lze provést pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="6b788-275">Uninstalling the Azure Backup agent can be done by using the following command:</span></span>

```
PS C:\> .\MARSAgentInstaller.exe /d /q
```

<span data-ttu-id="6b788-276">Odinstalace binárních souborů agenta z počítače má některé důsledky vzít v úvahu:</span><span class="sxs-lookup"><span data-stu-id="6b788-276">Uninstalling the agent binaries from the machine has some consequences to consider:</span></span>

* <span data-ttu-id="6b788-277">Odebere filtr souborů z počítače a sledování změn je zastavena.</span><span class="sxs-lookup"><span data-stu-id="6b788-277">It removes the file-filter from the machine, and tracking of changes is stopped.</span></span>
* <span data-ttu-id="6b788-278">Všechny informace o zásadách se odebere z počítače, ale informace o zásadách dále uložena ve službě.</span><span class="sxs-lookup"><span data-stu-id="6b788-278">All policy information is removed from the machine, but the policy information continues to be stored in the service.</span></span>
* <span data-ttu-id="6b788-279">Se odeberou všechny plány zálohování a jsou provedeny žádné další zálohy.</span><span class="sxs-lookup"><span data-stu-id="6b788-279">All backup schedules are removed, and no further backups are taken.</span></span>

<span data-ttu-id="6b788-280">Však data uložená v Azure zůstane a se uchovávají podle nastavení zásad uchovávání informací, které jste.</span><span class="sxs-lookup"><span data-stu-id="6b788-280">However, the data stored in Azure remains and is retained as per the retention policy setup by you.</span></span> <span data-ttu-id="6b788-281">Starší body jsou automaticky stará.</span><span class="sxs-lookup"><span data-stu-id="6b788-281">Older points are automatically aged out.</span></span>

## <a name="remote-management"></a><span data-ttu-id="6b788-282">Vzdálená správa</span><span class="sxs-lookup"><span data-stu-id="6b788-282">Remote management</span></span>
<span data-ttu-id="6b788-283">Veškerá správa, kolem agenta Azure Backup, zásady a zdroje dat lze provést vzdáleně pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6b788-283">All the management around the Azure Backup agent, policies, and data sources can be done remotely through PowerShell.</span></span> <span data-ttu-id="6b788-284">Na počítač, který bude spravovat vzdáleně musí být správně připravena.</span><span class="sxs-lookup"><span data-stu-id="6b788-284">The machine that will be managed remotely needs to be prepared correctly.</span></span>

<span data-ttu-id="6b788-285">Ve výchozím nastavení je Služba WinRM nakonfigurován pro ruční spuštění.</span><span class="sxs-lookup"><span data-stu-id="6b788-285">By default, the WinRM service is configured for manual startup.</span></span> <span data-ttu-id="6b788-286">Typ spuštění musí být nastavený na *automatické* a musí být spuštěna.</span><span class="sxs-lookup"><span data-stu-id="6b788-286">The startup type must be set to *Automatic* and the service should be started.</span></span> <span data-ttu-id="6b788-287">Pokud chcete ověřit, že Služba WinRM je spuštěná, by měla být hodnota vlastnosti stav *systémem*.</span><span class="sxs-lookup"><span data-stu-id="6b788-287">To verify that the WinRM service is running, the value of the Status property should be *Running*.</span></span>

```
PS C:\> Get-Service WinRM

Status   Name               DisplayName
------   ----               -----------
Running  winrm              Windows Remote Management (WS-Manag...
```

<span data-ttu-id="6b788-288">Prostředí PowerShell by měl být nakonfigurovaný pro vzdálenou komunikaci.</span><span class="sxs-lookup"><span data-stu-id="6b788-288">PowerShell should be configured for remoting.</span></span>

```
PS C:\> Enable-PSRemoting -force
WinRM is already set up to receive requests on this computer.
WinRM has been updated for remote management.
WinRM firewall exception enabled.

PS C:\> Set-ExecutionPolicy unrestricted -force
```

<span data-ttu-id="6b788-289">Tento počítač teď můžete spravovat vzdáleně – od instalace agenta.</span><span class="sxs-lookup"><span data-stu-id="6b788-289">The machine can now be managed remotely - starting from the installation of the agent.</span></span> <span data-ttu-id="6b788-290">Například následující skript zkopíruje agenta ke vzdálenému počítači a ho nainstaluje.</span><span class="sxs-lookup"><span data-stu-id="6b788-290">For example, the following script copies the agent to the remote machine and installs it.</span></span>

```
PS C:\> $dloc = "\\REMOTESERVER01\c$\Windows\Temp"
PS C:\> $agent = "\\REMOTESERVER01\c$\Windows\Temp\MARSAgentInstaller.exe"
PS C:\> $args = "/q"
PS C:\> Copy-Item "C:\Downloads\MARSAgentInstaller.exe" -Destination $dloc - force

PS C:\> $s = New-PSSession -ComputerName REMOTESERVER01
PS C:\> Invoke-Command -Session $s -Script { param($d, $a) Start-Process -FilePath $d $a -Wait } -ArgumentList $agent $args
```

## <a name="next-steps"></a><span data-ttu-id="6b788-291">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6b788-291">Next steps</span></span>
<span data-ttu-id="6b788-292">Další informace o Azure Backup pro Windows Server nebo klienta najdete v tématu</span><span class="sxs-lookup"><span data-stu-id="6b788-292">For more information about Azure Backup for Windows Server/Client see</span></span>

* [<span data-ttu-id="6b788-293">Seznámení s Azure Backup</span><span class="sxs-lookup"><span data-stu-id="6b788-293">Introduction to Azure Backup</span></span>](backup-introduction-to-azure-backup.md)
* [<span data-ttu-id="6b788-294">Zálohování serverů Windows</span><span class="sxs-lookup"><span data-stu-id="6b788-294">Back up Windows Servers</span></span>](backup-configure-vault.md)
