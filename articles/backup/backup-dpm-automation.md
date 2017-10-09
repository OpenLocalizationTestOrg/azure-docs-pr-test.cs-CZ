---
title: "aaaAzure zálohování - použijte PowerShell tooback až úloh DPM | Microsoft Docs"
description: "Zjistěte, jak toodeploy a spravovat Azure Backup pro Data Protection Manager (DPM) pomocí prostředí PowerShell"
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
ms.openlocfilehash: 27d2b4b3127b68c9da564697eb61dc3ccbc34b3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-backup-tooazure-for-data-protection-manager-dpm-servers-using-powershell"></a><span data-ttu-id="1bbce-103">Nasazení a správě zálohování tooAzure pro Data Protection Manager (DPM) servery pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="1bbce-103">Deploy and manage backup tooAzure for Data Protection Manager (DPM) servers using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1bbce-104">ARM</span><span class="sxs-lookup"><span data-stu-id="1bbce-104">ARM</span></span>](backup-dpm-automation.md)
> * [<span data-ttu-id="1bbce-105">Classic</span><span class="sxs-lookup"><span data-stu-id="1bbce-105">Classic</span></span>](backup-dpm-automation-classic.md)
>
>

<span data-ttu-id="1bbce-106">Tento článek ukazuje, jak toouse prostředí PowerShell toosetup Azure Backup na serveru aplikace DPM a toomanage zálohování a obnovení.</span><span class="sxs-lookup"><span data-stu-id="1bbce-106">This article shows you how toouse PowerShell toosetup Azure Backup on a DPM server, and toomanage backup and recovery.</span></span>

## <a name="setting-up-hello-powershell-environment"></a><span data-ttu-id="1bbce-107">Nastavení prostředí PowerShell hello</span><span class="sxs-lookup"><span data-stu-id="1bbce-107">Setting up hello PowerShell environment</span></span>
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

<span data-ttu-id="1bbce-108">Než budete moci použít PowerShell toomanage záloh z tooAzure Data Protection Manager, je třeba toohave hello správné prostředí v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1bbce-108">Before you can use PowerShell toomanage backups from Data Protection Manager tooAzure, you need toohave hello right environment in PowerShell.</span></span> <span data-ttu-id="1bbce-109">Při spuštění hello relace prostředí PowerShell text hello Ujistěte se, spusťte následující příkaz tooimport hello správné moduly hello a umožňují rutiny DPM hello toocorrectly odkaz:</span><span class="sxs-lookup"><span data-stu-id="1bbce-109">At hello start of hello PowerShell session, ensure that you run hello following command tooimport hello right modules and allow you toocorrectly reference hello DPM cmdlets:</span></span>

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

## <a name="setup-and-registration"></a><span data-ttu-id="1bbce-110">Instalace a registrace</span><span class="sxs-lookup"><span data-stu-id="1bbce-110">Setup and Registration</span></span>
<span data-ttu-id="1bbce-111">toobegin:</span><span class="sxs-lookup"><span data-stu-id="1bbce-111">toobegin:</span></span>

1. <span data-ttu-id="1bbce-112">[Stáhněte si nejnovější PowerShell](https://github.com/Azure/azure-powershell/releases) (minimální požadovaná verze je: 1.0.0)</span><span class="sxs-lookup"><span data-stu-id="1bbce-112">[Download latest PowerShell](https://github.com/Azure/azure-powershell/releases) (minimum version required is: 1.0.0)</span></span>
2. <span data-ttu-id="1bbce-113">Povolit rutiny Azure Backup hello přepnutím příliš*AzureResourceManager* režimu pomocí hello **Switch-AzureMode** příkaz:</span><span class="sxs-lookup"><span data-stu-id="1bbce-113">Enable hello Azure Backup commandlets by switching too*AzureResourceManager* mode by using hello **Switch-AzureMode** commandlet:</span></span>

```
PS C:\> Switch-AzureMode AzureResourceManager
```

<span data-ttu-id="1bbce-114">Hello následující nastavení a registrace úlohy je možné automatizovat pomocí prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="1bbce-114">hello following setup and registration tasks can be automated with PowerShell:</span></span>

* <span data-ttu-id="1bbce-115">Vytvoření trezoru Služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="1bbce-115">Create a Recovery Services vault</span></span>
* <span data-ttu-id="1bbce-116">Instalace agenta Azure Backup hello</span><span class="sxs-lookup"><span data-stu-id="1bbce-116">Installing hello Azure Backup agent</span></span>
* <span data-ttu-id="1bbce-117">Registrace hello služby zálohování Azure</span><span class="sxs-lookup"><span data-stu-id="1bbce-117">Registering with hello Azure Backup service</span></span>
* <span data-ttu-id="1bbce-118">Nastavení sítě</span><span class="sxs-lookup"><span data-stu-id="1bbce-118">Networking settings</span></span>
* <span data-ttu-id="1bbce-119">Nastavení šifrování</span><span class="sxs-lookup"><span data-stu-id="1bbce-119">Encryption settings</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="1bbce-120">Vytvoření trezoru služby Recovery Services</span><span class="sxs-lookup"><span data-stu-id="1bbce-120">Create a recovery services vault</span></span>
<span data-ttu-id="1bbce-121">Hello následující kroky vás provedou vytvoření trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="1bbce-121">hello following steps lead you through creating a Recovery Services vault.</span></span> <span data-ttu-id="1bbce-122">Trezor služeb zotavení se liší od úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="1bbce-122">A Recovery Services vault is different than a Backup vault.</span></span>

1. <span data-ttu-id="1bbce-123">Pokud používáte Azure Backup pro hello poprvé, musíte použít hello **Register-AzureRMResourceProvider** rutiny tooregister hello službu Azure Recovery zprostředkovatele s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="1bbce-123">If you are using Azure Backup for hello first time, you must use hello **Register-AzureRMResourceProvider** cmdlet tooregister hello Azure Recovery Service provider with your subscription.</span></span>

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
2. <span data-ttu-id="1bbce-124">Hello trezor služeb zotavení je ARM prostředek, takže je třeba tooplace ji ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="1bbce-124">hello Recovery Services vault is an ARM resource, so you need tooplace it within a Resource Group.</span></span> <span data-ttu-id="1bbce-125">Můžete použít existující skupinu prostředků nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="1bbce-125">You can use an existing resource group, or create a new one.</span></span> <span data-ttu-id="1bbce-126">Když vytváříte novou skupinu prostředků, zadejte hello název a umístění pro skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="1bbce-126">When creating a new resource group, specify hello name and location for hello resource group.</span></span>  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "West US"
    ```
3. <span data-ttu-id="1bbce-127">Použití hello **New-AzureRmRecoveryServicesVault** rutiny toocreate nový trezor.</span><span class="sxs-lookup"><span data-stu-id="1bbce-127">Use hello **New-AzureRmRecoveryServicesVault** cmdlet toocreate a new vault.</span></span> <span data-ttu-id="1bbce-128">Ujistěte se, toospecify hello stejné umístění pro hello trezoru, protože byl použit pro skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="1bbce-128">Be sure toospecify hello same location for hello vault as was used for hello resource group.</span></span>

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "West US"
    ```
4. <span data-ttu-id="1bbce-129">Zadejte typ hello toouse redundance úložiště; můžete použít [místně redundantní úložiště (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) nebo [geograficky redundantní úložiště (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span><span class="sxs-lookup"><span data-stu-id="1bbce-129">Specify hello type of storage redundancy toouse; you can use [Locally Redundant Storage (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) or [Geo Redundant Storage (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span></span> <span data-ttu-id="1bbce-130">Hello následující příklad ukazuje, že je možnost hello - BackupStorageRedundancy pro testVault nastavena tooGeoRedundant.</span><span class="sxs-lookup"><span data-stu-id="1bbce-130">hello following example shows hello -BackupStorageRedundancy option for testVault is set tooGeoRedundant.</span></span>

   > [!TIP]
   > <span data-ttu-id="1bbce-131">Mnoho rutin Azure Backup vyžadují objekt trezoru služeb zotavení hello jako vstup.</span><span class="sxs-lookup"><span data-stu-id="1bbce-131">Many Azure Backup cmdlets require hello Recovery Services vault object as an input.</span></span> <span data-ttu-id="1bbce-132">Z tohoto důvodu je vhodné toostore hello služeb zotavení zálohy trezoru objekt v proměnné.</span><span class="sxs-lookup"><span data-stu-id="1bbce-132">For this reason, it is convenient toostore hello Backup Recovery Services vault object in a variable.</span></span>
   >
   >

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testVault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-hello-vaults-in-a-subscription"></a><span data-ttu-id="1bbce-133">Zobrazení hello trezorů v předplatném.</span><span class="sxs-lookup"><span data-stu-id="1bbce-133">View hello vaults in a subscription</span></span>
<span data-ttu-id="1bbce-134">Použití **Get-AzureRmRecoveryServicesVault** tooview hello seznam všech trezorů v aktuálním předplatném hello.</span><span class="sxs-lookup"><span data-stu-id="1bbce-134">Use **Get-AzureRmRecoveryServicesVault** tooview hello list of all vaults in hello current subscription.</span></span> <span data-ttu-id="1bbce-135">Můžete použít tento příkaz toocheck, zda byl vytvořen nový trezor nebo toosee jaké trezory jsou k dispozici v rámci předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="1bbce-135">You can use this command toocheck that a new  vault was created, or toosee what vaults are available in hello subscription.</span></span>

<span data-ttu-id="1bbce-136">Spusťte příkaz hello, Get-AzureRmRecoveryServicesVault, a jsou uvedeny všechny trezorů v předplatném hello.</span><span class="sxs-lookup"><span data-stu-id="1bbce-136">Run hello command, Get-AzureRmRecoveryServicesVault, and all vaults in hello subscription are listed.</span></span>

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


## <a name="installing-hello-azure-backup-agent-on-a-dpm-server"></a><span data-ttu-id="1bbce-137">Instalace agenta Azure Backup hello na serveru aplikace DPM</span><span class="sxs-lookup"><span data-stu-id="1bbce-137">Installing hello Azure Backup agent on a DPM Server</span></span>
<span data-ttu-id="1bbce-138">Před instalací agenta Azure Backup hello, musíte instalační program toohave hello stažené a na serveru Windows hello.</span><span class="sxs-lookup"><span data-stu-id="1bbce-138">Before you install hello Azure Backup agent, you need toohave hello installer downloaded and present on hello Windows Server.</span></span> <span data-ttu-id="1bbce-139">Můžete získat nejnovější verzi instalačního programu hello hello hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) nebo z trezoru služeb zotavení hello stránka řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="1bbce-139">You can get hello latest version of hello installer from hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) or from hello Recovery Services vault's Dashboard page.</span></span> <span data-ttu-id="1bbce-140">Uložení instalačního programu hello tooan snadno dostupné místo jako * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="1bbce-140">Save hello installer tooan easily accessible location like *C:\Downloads\*.</span></span>

<span data-ttu-id="1bbce-141">tooinstall hello agenta, spusťte následující příkaz v konzolu prostředí PowerShell se zvýšenými oprávněními hello **na serveru DPM hello**:</span><span class="sxs-lookup"><span data-stu-id="1bbce-141">tooinstall hello agent, run hello following command in an elevated PowerShell console **on hello DPM server**:</span></span>

```
PS C:\> MARSAgentInstaller.exe /q
```

<span data-ttu-id="1bbce-142">Tím se nainstaluje hello agent se všemi možnostmi výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="1bbce-142">This installs hello agent with all hello default options.</span></span> <span data-ttu-id="1bbce-143">Hello instalace trvá několik minut v pozadí hello.</span><span class="sxs-lookup"><span data-stu-id="1bbce-143">hello installation takes a few minutes in hello background.</span></span> <span data-ttu-id="1bbce-144">Pokud nezadáte hello */nu* možnost hello **Windows Update** otevře se okno na konci hello hello toocheck instalace pro všechny aktualizace.</span><span class="sxs-lookup"><span data-stu-id="1bbce-144">If you do not specify hello */nu* option hello **Windows Update** window opens at hello end of hello installation toocheck for any updates.</span></span>

<span data-ttu-id="1bbce-145">Hello agent se zobrazí v seznamu nainstalovaných programů hello.</span><span class="sxs-lookup"><span data-stu-id="1bbce-145">hello agent shows up in hello list of installed programs.</span></span> <span data-ttu-id="1bbce-146">toosee hello seznamu nainstalovaných programů, přejděte příliš**ovládací panely** > **programy** > **programy a funkce**.</span><span class="sxs-lookup"><span data-stu-id="1bbce-146">toosee hello list of installed programs, go too**Control Panel** > **Programs** > **Programs and Features**.</span></span>

![Instalaci agenta](./media/backup-dpm-automation/installed-agent-listing.png)

### <a name="installation-options"></a><span data-ttu-id="1bbce-148">Možnosti instalace</span><span class="sxs-lookup"><span data-stu-id="1bbce-148">Installation options</span></span>
<span data-ttu-id="1bbce-149">toosee všechny možnosti hello k dispozici prostřednictvím příkazového řádku hello, hello použijte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="1bbce-149">toosee all hello options available via hello commandline, use hello following command:</span></span>

```
PS C:\> MARSAgentInstaller.exe /?
```

<span data-ttu-id="1bbce-150">Hello dostupné možnosti patří:</span><span class="sxs-lookup"><span data-stu-id="1bbce-150">hello available options include:</span></span>

| <span data-ttu-id="1bbce-151">Možnost</span><span class="sxs-lookup"><span data-stu-id="1bbce-151">Option</span></span> | <span data-ttu-id="1bbce-152">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="1bbce-152">Details</span></span> | <span data-ttu-id="1bbce-153">Výchozí</span><span class="sxs-lookup"><span data-stu-id="1bbce-153">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1bbce-154">/q</span><span class="sxs-lookup"><span data-stu-id="1bbce-154">/q</span></span> |<span data-ttu-id="1bbce-155">Tichá instalace</span><span class="sxs-lookup"><span data-stu-id="1bbce-155">Quiet installation</span></span> |- |
| <span data-ttu-id="1bbce-156">/ p: "umístění"</span><span class="sxs-lookup"><span data-stu-id="1bbce-156">/p:"location"</span></span> |<span data-ttu-id="1bbce-157">Cesta toohello instalační složku pro agenta Azure Backup hello.</span><span class="sxs-lookup"><span data-stu-id="1bbce-157">Path toohello installation folder for hello Azure Backup agent.</span></span> |<span data-ttu-id="1bbce-158">C:\Program Files\Microsoft Azure Recovery Services agenta</span><span class="sxs-lookup"><span data-stu-id="1bbce-158">C:\Program Files\Microsoft Azure Recovery Services Agent</span></span> |
| <span data-ttu-id="1bbce-159">/ s: "umístění"</span><span class="sxs-lookup"><span data-stu-id="1bbce-159">/s:"location"</span></span> |<span data-ttu-id="1bbce-160">Složka mezipaměti toohello cestu pro agenta Azure Backup hello.</span><span class="sxs-lookup"><span data-stu-id="1bbce-160">Path toohello cache folder for hello Azure Backup agent.</span></span> |<span data-ttu-id="1bbce-161">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span><span class="sxs-lookup"><span data-stu-id="1bbce-161">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span></span> |
| <span data-ttu-id="1bbce-162">/m</span><span class="sxs-lookup"><span data-stu-id="1bbce-162">/m</span></span> |<span data-ttu-id="1bbce-163">Výslovný souhlas tooMicrosoft aktualizace</span><span class="sxs-lookup"><span data-stu-id="1bbce-163">Opt-in tooMicrosoft Update</span></span> |- |
| <span data-ttu-id="1bbce-164">/nu</span><span class="sxs-lookup"><span data-stu-id="1bbce-164">/nu</span></span> |<span data-ttu-id="1bbce-165">Nekontrolovat aktualizace po dokončení instalace</span><span class="sxs-lookup"><span data-stu-id="1bbce-165">Do not Check for updates after installation is complete</span></span> |- |
| <span data-ttu-id="1bbce-166">/d</span><span class="sxs-lookup"><span data-stu-id="1bbce-166">/d</span></span> |<span data-ttu-id="1bbce-167">Odinstaluje Agenta Microsoft Azure Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="1bbce-167">Uninstalls Microsoft Azure Recovery Services Agent</span></span> |- |
| <span data-ttu-id="1bbce-168">/pH</span><span class="sxs-lookup"><span data-stu-id="1bbce-168">/ph</span></span> |<span data-ttu-id="1bbce-169">Adresa proxy hostitele</span><span class="sxs-lookup"><span data-stu-id="1bbce-169">Proxy Host Address</span></span> |- |
| <span data-ttu-id="1bbce-170">/Po</span><span class="sxs-lookup"><span data-stu-id="1bbce-170">/po</span></span> |<span data-ttu-id="1bbce-171">Číslo portu proxy hostitele</span><span class="sxs-lookup"><span data-stu-id="1bbce-171">Proxy Host Port Number</span></span> |- |
| <span data-ttu-id="1bbce-172">/Pu</span><span class="sxs-lookup"><span data-stu-id="1bbce-172">/pu</span></span> |<span data-ttu-id="1bbce-173">Uživatelské jméno proxy hostitele</span><span class="sxs-lookup"><span data-stu-id="1bbce-173">Proxy Host UserName</span></span> |- |
| <span data-ttu-id="1bbce-174">/pW</span><span class="sxs-lookup"><span data-stu-id="1bbce-174">/pw</span></span> |<span data-ttu-id="1bbce-175">Heslo pro proxy server</span><span class="sxs-lookup"><span data-stu-id="1bbce-175">Proxy Password</span></span> |- |

## <a name="registering-dpm-tooa-recovery-services-vault"></a><span data-ttu-id="1bbce-176">Registrace aplikace DPM tooa trezoru služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="1bbce-176">Registering DPM tooa Recovery Services Vault</span></span>
<span data-ttu-id="1bbce-177">Po vytvoření trezoru služeb zotavení hello stáhnout nejnovější verzi agenta hello a přihlašovací údaje trezoru hello a uložte ho do vhodného umístění jako C:\Downloads.</span><span class="sxs-lookup"><span data-stu-id="1bbce-177">After you created hello Recovery Services vault, download hello latest agent and hello vault credentials and store it in a convenient location like C:\Downloads.</span></span>

```
PS C:\> $credspath = "C:\downloads"
PS C:\> $credsfilename = Get-AzureRmRecoveryServicesVaultSettingsFile -Backup -Vault $vault1 -Path  $credspath
PS C:\> $credsfilename
C:\downloads\testvault\_Sun Apr 10 2016.VaultCredentials
```

<span data-ttu-id="1bbce-178">Na serveru aplikace DPM hello spusťte hello [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) rutiny tooregister hello počítač s hello trezoru.</span><span class="sxs-lookup"><span data-stu-id="1bbce-178">On hello DPM server, run hello [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) cmdlet tooregister hello machine with hello vault.</span></span>

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-OBRegistration-VaultCredentials $cred -Confirm:$false
CertThumbprint      :7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName: testvault
Region              :West US
Machine registration succeeded.
```

### <a name="initial-configuration-settings"></a><span data-ttu-id="1bbce-179">Počáteční konfigurace nastavení</span><span class="sxs-lookup"><span data-stu-id="1bbce-179">Initial configuration settings</span></span>
<span data-ttu-id="1bbce-180">Jakmile hello serveru aplikace DPM není zaregistrována hello trezor služeb zotavení, začne s výchozím nastavením odběru.</span><span class="sxs-lookup"><span data-stu-id="1bbce-180">Once hello DPM Server is registered with hello Recovery Services vault, it starts with default subscription settings.</span></span> <span data-ttu-id="1bbce-181">Tato nastavení odběru zahrnují sítě, šifrování a hello pracovní oblasti.</span><span class="sxs-lookup"><span data-stu-id="1bbce-181">These subscription settings include Networking, Encryption and hello Staging area.</span></span> <span data-ttu-id="1bbce-182">nastavení odběru toochange potřebujete toofirst získat popisovač pro stávající nastavení (výchozí) hello pomocí hello [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) rutiny:</span><span class="sxs-lookup"><span data-stu-id="1bbce-182">toochange subscription settings you need toofirst get a handle on hello existing (default) settings using hello [Get-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612793) cmdlet:</span></span>

```
$setting = Get-DPMCloudSubscriptionSetting -DPMServerName "TestingServer"
```

<span data-ttu-id="1bbce-183">Všechny změny jsou učiněna toothis místní prostředí PowerShell objekt ```$setting``` a pak je úplná objekt hello potvrdit tooDPM a toosave Azure Backup je pomocí hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) rutiny.</span><span class="sxs-lookup"><span data-stu-id="1bbce-183">All modifications are made toothis local PowerShell object ```$setting```  and then hello full object is committed tooDPM and Azure Backup toosave them using hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span></span> <span data-ttu-id="1bbce-184">Je třeba toouse hello ```–Commit``` tooensure příznak, který hello změny jsou nastavené jako trvalé.</span><span class="sxs-lookup"><span data-stu-id="1bbce-184">You need toouse hello ```–Commit``` flag tooensure that hello changes are persisted.</span></span> <span data-ttu-id="1bbce-185">Hello nastavení nebude použita a pokud potvrzené používá Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="1bbce-185">hello settings will not be applied and used by Azure Backup unless committed.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="networking"></a><span data-ttu-id="1bbce-186">Sítě</span><span class="sxs-lookup"><span data-stu-id="1bbce-186">Networking</span></span>
<span data-ttu-id="1bbce-187">Pokud připojení hello hello DPM počítač toohello služby Azure Backup na hello internet přes proxy server, měli nastavení proxy serveru hello zadat pro úspěšné zálohy.</span><span class="sxs-lookup"><span data-stu-id="1bbce-187">If hello connectivity of hello DPM machine toohello Azure Backup service on hello internet is through a proxy server, then hello proxy server settings should be provided for successful backups.</span></span> <span data-ttu-id="1bbce-188">To se provádí pomocí hello ```-ProxyServer```a ```-ProxyPort```, ```-ProxyUsername``` a hello ```ProxyPassword``` parametry s hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) rutiny.</span><span class="sxs-lookup"><span data-stu-id="1bbce-188">This is done by using hello ```-ProxyServer```and ```-ProxyPort```, ```-ProxyUsername``` and hello ```ProxyPassword``` parameters with hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet.</span></span> <span data-ttu-id="1bbce-189">V tomto příkladu není žádný proxy server, jsme jsou explicitně vymazání údaje související s proxy serverem.</span><span class="sxs-lookup"><span data-stu-id="1bbce-189">In this example, there is no proxy server so we are explicitly clearing any proxy-related information.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoProxy
```

<span data-ttu-id="1bbce-190">Využití šířky pásma se dá taky nastavit pomocí možnosti ```-WorkHourBandwidth``` a ```-NonWorkHourBandwidth``` pro danou sadu dny v týdnu hello.</span><span class="sxs-lookup"><span data-stu-id="1bbce-190">Bandwidth usage can also be controlled with options of ```-WorkHourBandwidth``` and ```-NonWorkHourBandwidth``` for a given set of days of hello week.</span></span> <span data-ttu-id="1bbce-191">V tomto příkladu jsme nejsou nastavení žádné omezení.</span><span class="sxs-lookup"><span data-stu-id="1bbce-191">In this example, we are not setting any throttling.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -NoThrottle
```

## <a name="configuring-hello-staging-area"></a><span data-ttu-id="1bbce-192">Konfigurace hello pracovní oblasti</span><span class="sxs-lookup"><span data-stu-id="1bbce-192">Configuring hello staging Area</span></span>
<span data-ttu-id="1bbce-193">agent Azure Backup Hello spuštěné na serveru DPM hello potřebuje dočasné úložiště pro data obnovená z cloudu hello (místní pracovní oblasti).</span><span class="sxs-lookup"><span data-stu-id="1bbce-193">hello Azure Backup agent running on hello DPM server needs temporary storage for data restored from hello cloud (local staging area).</span></span> <span data-ttu-id="1bbce-194">Konfigurovat pracovní oblasti hello pomocí hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) rutiny a hello ```-StagingAreaPath``` parametr.</span><span class="sxs-lookup"><span data-stu-id="1bbce-194">Configure hello staging area using hello [Set-DPMCloudSubscriptionSetting](https://technet.microsoft.com/library/jj612791) cmdlet and hello ```-StagingAreaPath``` parameter.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -StagingAreaPath "C:\StagingArea"
```

<span data-ttu-id="1bbce-195">V předchozím příkladu hello, hello pracovní oblasti se nastaví příliš*C:\StagingArea* v objektu prostředí PowerShell hello ```$setting```.</span><span class="sxs-lookup"><span data-stu-id="1bbce-195">In hello example above, hello staging area will be set too*C:\StagingArea* in hello PowerShell object ```$setting```.</span></span> <span data-ttu-id="1bbce-196">Ujistěte se, že hello zadaná složka již existuje, jinak hello konečné zápisu nastavení odběru hello nezdaří.</span><span class="sxs-lookup"><span data-stu-id="1bbce-196">Ensure that hello specified folder already exists, or else hello final commit of hello subscription settings will fail.</span></span>

### <a name="encryption-settings"></a><span data-ttu-id="1bbce-197">Nastavení šifrování</span><span class="sxs-lookup"><span data-stu-id="1bbce-197">Encryption settings</span></span>
<span data-ttu-id="1bbce-198">zálohování dat odesílaných tooAzure Hello zálohování je šifrovaný tooprotect hello důvěrnost dat hello.</span><span class="sxs-lookup"><span data-stu-id="1bbce-198">hello backup data sent tooAzure Backup is encrypted tooprotect hello confidentiality of hello data.</span></span> <span data-ttu-id="1bbce-199">šifrovací přístupové heslo Hello jsou hello "password" toodecrypt hello data v době obnovení hello.</span><span class="sxs-lookup"><span data-stu-id="1bbce-199">hello encryption passphrase is hello "password" toodecrypt hello data at hello time of restore.</span></span> <span data-ttu-id="1bbce-200">Je důležité tookeep bezpečné této informace a zabezpečení po nastavení.</span><span class="sxs-lookup"><span data-stu-id="1bbce-200">It is important tookeep this information safe and secure once it is set.</span></span>

<span data-ttu-id="1bbce-201">V příkladu hello níže, převede první příkaz hello hello řetězec ```passphrase123456789``` tooa zabezpečený řetězec a přiřadí hello zabezpečený řetězec toohello proměnné s názvem ```$Passphrase```.</span><span class="sxs-lookup"><span data-stu-id="1bbce-201">In hello example below, hello first command converts hello string ```passphrase123456789``` tooa secure string and assigns hello secure string toohello variable named ```$Passphrase```.</span></span> <span data-ttu-id="1bbce-202">druhý příkaz Hello nastaví hello zabezpečený řetězec ```$Passphrase``` jako hello heslo pro šifrování záloh.</span><span class="sxs-lookup"><span data-stu-id="1bbce-202">hello second command sets hello secure string in ```$Passphrase``` as hello password for encrypting backups.</span></span>

```
PS C:\> $Passphrase = ConvertTo-SecureString -string "passphrase123456789" -AsPlainText -Force

PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -EncryptionPassphrase $Passphrase
```

> [!IMPORTANT]
> <span data-ttu-id="1bbce-203">Zachování informací o přístupové heslo hello bezpečném, po nastavení.</span><span class="sxs-lookup"><span data-stu-id="1bbce-203">Keep hello passphrase information safe and secure once it is set.</span></span> <span data-ttu-id="1bbce-204">Nebudete moct toorestore dat z Azure bez tohoto hesla.</span><span class="sxs-lookup"><span data-stu-id="1bbce-204">You will not be able toorestore data from Azure without this passphrase.</span></span>
>
>

<span data-ttu-id="1bbce-205">V tomto okamžiku by měl provedení všech hello požadované změny toohello ```$setting``` objektu.</span><span class="sxs-lookup"><span data-stu-id="1bbce-205">At this point, you should have made all hello required changes toohello ```$setting``` object.</span></span> <span data-ttu-id="1bbce-206">Mějte na paměti, toocommit hello změny.</span><span class="sxs-lookup"><span data-stu-id="1bbce-206">Remember toocommit hello changes.</span></span>

```
PS C:\> Set-DPMCloudSubscriptionSetting -DPMServerName "TestingServer" -SubscriptionSetting $setting -Commit
```

## <a name="protect-data-tooazure-backup"></a><span data-ttu-id="1bbce-207">Ochrana dat tooAzure zálohování</span><span class="sxs-lookup"><span data-stu-id="1bbce-207">Protect data tooAzure Backup</span></span>
<span data-ttu-id="1bbce-208">V této části přidáte tooDPM produkčním serveru a poté proveďte ochranu úložiště DPM toolocal hello data a pak tooAzure zálohování.</span><span class="sxs-lookup"><span data-stu-id="1bbce-208">In this section, you will add a production server tooDPM and then protect hello data toolocal DPM storage and then tooAzure Backup.</span></span> <span data-ttu-id="1bbce-209">V příkladech hello si předvedeme jak tooback soubory a složky.</span><span class="sxs-lookup"><span data-stu-id="1bbce-209">In hello examples, we will demonstrate how tooback up files and folders.</span></span> <span data-ttu-id="1bbce-210">Hello logiky můžete snadno být rozšířené toobackup libovolný zdroj dat aplikace DPM podporováno.</span><span class="sxs-lookup"><span data-stu-id="1bbce-210">hello logic can easily be extended toobackup any DPM-supported data source.</span></span> <span data-ttu-id="1bbce-211">Všechny zálohy aplikace DPM se řídí pomocí ochranu skupiny (PG) s čtyři části:</span><span class="sxs-lookup"><span data-stu-id="1bbce-211">All your DPM backups are governed by a Protection Group (PG) with four parts:</span></span>

1. <span data-ttu-id="1bbce-212">**Členy skupiny** je seznam všech hello chránitelné objekty (také označované jako *zdrojů dat* v DPM), které chcete tooprotect v hello stejné skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="1bbce-212">**Group members** is a list of all hello protectable objects (also known as *Datasources* in DPM) that you want tooprotect in hello same protection group.</span></span> <span data-ttu-id="1bbce-213">Můžete například tooprotect produkční virtuálních počítačů na jednu skupinu ochrany a databáze systému SQL Server v jiné skupině ochrany mají různé požadavky na zálohování.</span><span class="sxs-lookup"><span data-stu-id="1bbce-213">For example, you may want tooprotect production VMs in one protection group and SQL Server databases in another protection group as they may have different backup requirements.</span></span> <span data-ttu-id="1bbce-214">Než můžete zálohovat žádný zdroj dat na provozním serveru je třeba jestli hello toomake agenta DPM na hello server je nainstalovaná a je spravovaných aplikací DPM.</span><span class="sxs-lookup"><span data-stu-id="1bbce-214">Before you can back up any datasource on a production server you need toomake sure hello DPM Agent is installed on hello server and is managed by DPM.</span></span> <span data-ttu-id="1bbce-215">Postupujte podle kroků hello pro [instalaci agenta aplikace DPM hello](https://technet.microsoft.com/library/bb870935.aspx) a jejím propojením toohello vhodné serveru aplikace DPM.</span><span class="sxs-lookup"><span data-stu-id="1bbce-215">Follow hello steps for [installing hello DPM Agent](https://technet.microsoft.com/library/bb870935.aspx) and linking it toohello appropriate DPM Server.</span></span>
2. <span data-ttu-id="1bbce-216">**Způsob ochrany dat** určuje hello cíl zálohování umístění - páska, disk a cloud.</span><span class="sxs-lookup"><span data-stu-id="1bbce-216">**Data protection method** specifies hello target backup locations - tape, disk, and cloud.</span></span> <span data-ttu-id="1bbce-217">V našem příkladu jsme bude chránit dat toohello místní disk a toohello v cloudu.</span><span class="sxs-lookup"><span data-stu-id="1bbce-217">In our example we will protect data toohello local disk and toohello cloud.</span></span>
3. <span data-ttu-id="1bbce-218">A **plán zálohování** určující při zálohování potřebovat toobe prováděné a jak často hello se mají synchronizovat data mezi hello serveru DPM a hello provozním serveru.</span><span class="sxs-lookup"><span data-stu-id="1bbce-218">A **backup schedule** that specifies when backups need toobe taken and how often hello data should be synchronized between hello DPM Server and hello production server.</span></span>
4. <span data-ttu-id="1bbce-219">A **plán uchovávání informací** určující, jak dlouho body obnovení hello tooretain v Azure.</span><span class="sxs-lookup"><span data-stu-id="1bbce-219">A **retention schedule** that specifies how long tooretain hello recovery points in Azure.</span></span>

### <a name="creating-a-protection-group"></a><span data-ttu-id="1bbce-220">Vytvoření skupiny ochrany</span><span class="sxs-lookup"><span data-stu-id="1bbce-220">Creating a protection group</span></span>
<span data-ttu-id="1bbce-221">Začněte vytvořením nové skupiny ochrany pomocí hello [New-DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) rutiny.</span><span class="sxs-lookup"><span data-stu-id="1bbce-221">Start by creating a new Protection Group using hello [New-DPMProtectionGroup](https://technet.microsoft.com/library/hh881722) cmdlet.</span></span>

```
PS C:\> $PG = New-DPMProtectionGroup -DPMServerName " TestingServer " -Name "ProtectGroup01"
```

<span data-ttu-id="1bbce-222">Hello výše rutina vytvoří skupiny ochrany s názvem *ProtectGroup01*.</span><span class="sxs-lookup"><span data-stu-id="1bbce-222">hello above cmdlet will create a Protection Group named *ProtectGroup01*.</span></span> <span data-ttu-id="1bbce-223">Existující skupiny ochrany můžete také upravit novější tooadd zálohování toohello cloudu Azure.</span><span class="sxs-lookup"><span data-stu-id="1bbce-223">An existing protection group can also be modified later tooadd backup toohello Azure cloud.</span></span> <span data-ttu-id="1bbce-224">Ale toomake změny toohello skupiny ochrany – nové nebo existující - potřebujeme tooget popisovač na *upravitelnými* objekt, který používá hello [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) rutiny.</span><span class="sxs-lookup"><span data-stu-id="1bbce-224">However, toomake any changes toohello Protection Group - new or existing - we need tooget a handle on a *modifiable* object using hello [Get-DPMModifiableProtectionGroup](https://technet.microsoft.com/library/hh881713) cmdlet.</span></span>

```
PS C:\> $MPG = Get-ModifiableProtectionGroup $PG
```

### <a name="adding-group-members-toohello-protection-group"></a><span data-ttu-id="1bbce-225">Přidání skupiny toohello členy skupiny ochrany</span><span class="sxs-lookup"><span data-stu-id="1bbce-225">Adding group members toohello Protection Group</span></span>
<span data-ttu-id="1bbce-226">Každý Agent aplikace DPM zná hello seznam zdrojů dat na hello serveru, který je nainstalovaný.</span><span class="sxs-lookup"><span data-stu-id="1bbce-226">Each DPM Agent knows hello list of datasources on hello server that it is installed on.</span></span> <span data-ttu-id="1bbce-227">tooadd toohello zdroj dat skupiny ochrany, hello agenta aplikace DPM musí toofirst odeslání seznamu hello zdrojů dat zpět toohello DPM server.</span><span class="sxs-lookup"><span data-stu-id="1bbce-227">tooadd a datasource toohello Protection Group, hello DPM Agent needs toofirst send a list of hello datasources back toohello DPM server.</span></span> <span data-ttu-id="1bbce-228">Jeden nebo více zdrojů dat jsou pak vybrali a přidat toohello skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="1bbce-228">One or more datasources are then selected and added toohello Protection Group.</span></span> <span data-ttu-id="1bbce-229">kroky PowerShell Hello potřeba tooachieve to jsou:</span><span class="sxs-lookup"><span data-stu-id="1bbce-229">hello PowerShell steps needed tooachieve this are:</span></span>

1. <span data-ttu-id="1bbce-230">Získat seznam všech serverů spravovaných aplikací DPM prostřednictvím hello agenta aplikace DPM.</span><span class="sxs-lookup"><span data-stu-id="1bbce-230">Fetch a list of all servers managed by DPM through hello DPM Agent.</span></span>
2. <span data-ttu-id="1bbce-231">Vyberte konkrétní server.</span><span class="sxs-lookup"><span data-stu-id="1bbce-231">Choose a specific server.</span></span>
3. <span data-ttu-id="1bbce-232">Získat seznam všech zdrojů dat na serveru hello.</span><span class="sxs-lookup"><span data-stu-id="1bbce-232">Fetch a list of all datasources on hello server.</span></span>
4. <span data-ttu-id="1bbce-233">Vyberte jeden nebo více zdrojů dat a přidat je toohello skupiny ochrany</span><span class="sxs-lookup"><span data-stu-id="1bbce-233">Choose one or more datasources and add them toohello Protection Group</span></span>

<span data-ttu-id="1bbce-234">Hello seznam serverů, na které hello agenta aplikace DPM je nainstalovaná a je spravován systémem hello serveru aplikace DPM je získán pomocí hello [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) rutiny.</span><span class="sxs-lookup"><span data-stu-id="1bbce-234">hello list of servers on which hello DPM Agent is installed and is being managed by hello DPM Server is acquired with hello [Get-DPMProductionServer](https://technet.microsoft.com/library/hh881600) cmdlet.</span></span> <span data-ttu-id="1bbce-235">V tomto příkladu jsme bude filtrovat a konfigurovat jenom PS s názvem *productionserver01* pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="1bbce-235">In this example we will filter and only configure PS with name *productionserver01* for backup.</span></span>

```
PS C:\> $server = Get-ProductionServer -DPMServerName "TestingServer" | where {($_.servername) –contains “productionserver01”
```

<span data-ttu-id="1bbce-236">Nyní načíst hello seznam zdrojů dat v ```$server``` pomocí hello [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) rutiny.</span><span class="sxs-lookup"><span data-stu-id="1bbce-236">Now fetch hello list of datasources on ```$server``` using hello [Get-DPMDatasource](https://technet.microsoft.com/library/hh881605) cmdlet.</span></span> <span data-ttu-id="1bbce-237">V tomto příkladu jsme pro svazek hello filtrování * D:\* chceme tooconfigure pro zálohování.</span><span class="sxs-lookup"><span data-stu-id="1bbce-237">In this example we are filtering for hello volume *D:\* that we want tooconfigure for backup.</span></span> <span data-ttu-id="1bbce-238">Tento zdroj dat se pak přidá toohello skupiny ochrany pomocí hello [přidat DPMChildDatasource](https://technet.microsoft.com/library/hh881732) rutiny.</span><span class="sxs-lookup"><span data-stu-id="1bbce-238">This datasource is then added toohello Protection Group using hello [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732) cmdlet.</span></span> <span data-ttu-id="1bbce-239">Mějte na paměti, toouse hello *upravitelnými* objektu skupiny ochrany ```$MPG``` dodatky toomake hello.</span><span class="sxs-lookup"><span data-stu-id="1bbce-239">Remember toouse hello *modifiable* protection group object ```$MPG``` toomake hello additions.</span></span>

```
PS C:\> $DS = Get-Datasource -ProductionServer $server -Inquire | where { $_.Name -contains “D:\” }

PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS
```

<span data-ttu-id="1bbce-240">Opakujte tento krok tolikrát, kolikrát podle potřeby, dokud nepřidáte všechny hello zvolené skupiny zdrojů dat toohello ochrany.</span><span class="sxs-lookup"><span data-stu-id="1bbce-240">Repeat this step as many times as required, until you have added all hello chosen datasources toohello protection group.</span></span> <span data-ttu-id="1bbce-241">Můžete začít s jenom jeden zdroj dat a dokončení hello pracovní postup pro vytváření hello skupiny ochrany a později přidat další zdroje dat toohello skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="1bbce-241">You can also start with just one datasource, and complete hello workflow for creating hello Protection Group, and at a later point add more datasources toohello Protection Group.</span></span>

### <a name="selecting-hello-data-protection-method"></a><span data-ttu-id="1bbce-242">Vyberte způsob ochrany dat hello</span><span class="sxs-lookup"><span data-stu-id="1bbce-242">Selecting hello data protection method</span></span>
<span data-ttu-id="1bbce-243">Jakmile hello zdroje dat byly přidány toohello skupiny ochrany, hello dalším krokem je způsob ochrany hello toospecify pomocí hello [Set-DPMProtectionType](https://technet.microsoft.com/library/hh881725) rutiny.</span><span class="sxs-lookup"><span data-stu-id="1bbce-243">Once hello datasources have been added toohello Protection Group, hello next step is toospecify hello protection method using hello [Set-DPMProtectionType](https://technet.microsoft.com/library/hh881725) cmdlet.</span></span> <span data-ttu-id="1bbce-244">V tomto příkladu hello skupiny ochrany je nastaven pro místního disku a zálohování.</span><span class="sxs-lookup"><span data-stu-id="1bbce-244">In this example, hello Protection Group is setup for local disk and cloud backup.</span></span> <span data-ttu-id="1bbce-245">Musíte taky toospecify hello zdroj dat, které chcete pomocí hello toocloud tooprotect [přidat DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) rutiny příznakem - Online.</span><span class="sxs-lookup"><span data-stu-id="1bbce-245">You also need toospecify hello datasource that you want tooprotect toocloud using hello [Add-DPMChildDatasource](https://technet.microsoft.com/library/hh881732.aspx) cmdlet with -Online flag.</span></span>

```
PS C:\> Set-DPMProtectionType -ProtectionGroup $MPG -ShortTerm Disk –LongTerm Online
PS C:\> Add-DPMChildDatasource -ProtectionGroup $MPG -ChildDatasource $DS –Online
```

### <a name="setting-hello-retention-range"></a><span data-ttu-id="1bbce-246">Nastavení rozsah uchování hello</span><span class="sxs-lookup"><span data-stu-id="1bbce-246">Setting hello retention range</span></span>
<span data-ttu-id="1bbce-247">Nastavit hello uchování pro body zálohy hello pomocí hello [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) rutiny.</span><span class="sxs-lookup"><span data-stu-id="1bbce-247">Set hello retention for hello backup points using hello [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span></span> <span data-ttu-id="1bbce-248">Při uchování hello liché tooset může zdát, než byla definována hello plán zálohování, pomocí hello ```Set-DPMPolicyObjective``` rutiny automaticky nastaví výchozí plán zálohování, který je pak možné upravit.</span><span class="sxs-lookup"><span data-stu-id="1bbce-248">While it might seem odd tooset hello retention before hello backup schedule has been defined, using hello ```Set-DPMPolicyObjective``` cmdlet automatically sets a default backup schedule that can then be modified.</span></span> <span data-ttu-id="1bbce-249">Je možné tooset plán zálohování hello vždy nejprve a hello zásady uchovávání informací po.</span><span class="sxs-lookup"><span data-stu-id="1bbce-249">It is always possible tooset hello backup schedule first and hello retention policy after.</span></span>

<span data-ttu-id="1bbce-250">V příkladu hello níže hello rutiny nastaví parametry uchování hello zálohování na disk.</span><span class="sxs-lookup"><span data-stu-id="1bbce-250">In hello example below, hello cmdlet sets hello retention parameters for disk backups.</span></span> <span data-ttu-id="1bbce-251">To bude uchování záloh pro 10 dnů a synchronizaci dat každých 6 hodin mezi hello provozním serveru a server aplikace DPM hello.</span><span class="sxs-lookup"><span data-stu-id="1bbce-251">This will retain backups for 10 days, and sync data every 6 hours between hello production server and hello DPM server.</span></span> <span data-ttu-id="1bbce-252">Hello ```SynchronizationFrequencyMinutes``` nedefinuje četnost zálohování je bod vytvořen, ale jak často dat je zkopírovaný toohello serveru aplikace DPM.</span><span class="sxs-lookup"><span data-stu-id="1bbce-252">hello ```SynchronizationFrequencyMinutes``` doesn't define how often a backup point is created, but how often data is copied toohello DPM server.</span></span>  <span data-ttu-id="1bbce-253">Toto nastavení zabrání zálohy příliš velká.</span><span class="sxs-lookup"><span data-stu-id="1bbce-253">This setting prevents backups from becoming too large.</span></span>

```
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -RetentionRangeInDays 10 -SynchronizationFrequencyMinutes 360
```

<span data-ttu-id="1bbce-254">Pro zálohování budete tooAzure (aplikace DPM se týká toothem jako zálohování Online) rozsahy uchovávání hello lze nakonfigurovat pro [dlouhodobé uchovávání používá schéma Dědečka. otec SYN (GFS)](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="1bbce-254">For backups going tooAzure (DPM refers toothem as Online backups) hello retention ranges can be configured for [long term retention using a Grandfather-Father-Son scheme (GFS)](backup-azure-backup-cloud-as-tape.md).</span></span> <span data-ttu-id="1bbce-255">To znamená můžete definovat zásady uchovávání informací kombinované zahrnující denní, týdenní, měsíční a roční zásady uchovávání informací.</span><span class="sxs-lookup"><span data-stu-id="1bbce-255">That is, you can define a combined retention policy involving daily, weekly, monthly and yearly retention policies.</span></span> <span data-ttu-id="1bbce-256">V tomto příkladu vytvoření představující schéma hello komplexní uchovávání, který má být pole a pak nakonfigurujte rozsah uchování hello pomocí hello [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) rutiny.</span><span class="sxs-lookup"><span data-stu-id="1bbce-256">In this example, we create an array representing hello complex retention scheme that we want, and then configure hello retention range using hello [Set-DPMPolicyObjective](https://technet.microsoft.com/library/hh881762) cmdlet.</span></span>

```
PS C:\> $RRlist = @()
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 180, Days)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 104, Weeks)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 60, Month)
PS C:\> $RRList += (New-Object -TypeName Microsoft.Internal.EnterpriseStorage.Dls.UI.ObjectModel.OMCommon.RetentionRange -ArgumentList 10, Years)
PS C:\> Set-DPMPolicyObjective –ProtectionGroup $MPG -OnlineRetentionRangeList $RRlist
```

### <a name="set-hello-backup-schedule"></a><span data-ttu-id="1bbce-257">Plán zálohování sady hello</span><span class="sxs-lookup"><span data-stu-id="1bbce-257">Set hello backup schedule</span></span>
<span data-ttu-id="1bbce-258">Aplikace DPM nastaví výchozí plán zálohování automaticky, pokud zadáte hello cíl ochrany pomocí hello ```Set-DPMPolicyObjective``` rutiny.</span><span class="sxs-lookup"><span data-stu-id="1bbce-258">DPM sets a default backup schedule automatically if you specify hello protection objective using hello ```Set-DPMPolicyObjective``` cmdlet.</span></span> <span data-ttu-id="1bbce-259">toochange hello výchozích plánů, použijte hello [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) následovanou hello [Set-DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) rutiny.</span><span class="sxs-lookup"><span data-stu-id="1bbce-259">toochange hello default schedules, use hello [Get-DPMPolicySchedule](https://technet.microsoft.com/library/hh881749) cmdlet followed by hello [Set-DPMPolicySchedule](https://technet.microsoft.com/library/hh881723) cmdlet.</span></span>

```
PS C:\> $onlineSch = Get-DPMPolicySchedule -ProtectionGroup $mpg -LongTerm Online
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[0] -TimesOfDay 02:00
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[1] -TimesOfDay 02:00 -DaysOfWeek Sa,Su –Interval 1
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[2] -TimesOfDay 02:00 -RelativeIntervals First,Third –DaysOfWeek Sa
PS C:\> Set-DPMPolicySchedule -ProtectionGroup $MPG -Schedule $onlineSch[3] -TimesOfDay 02:00 -DaysOfMonth 2,5,8,9 -Months Jan,Jul
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```

<span data-ttu-id="1bbce-260">V hello výše například ```$onlineSch``` je pole se čtyřmi prvky, které obsahuje hello existující plán ochranu online pro hello skupiny ochrany ve schématu GFS hello:</span><span class="sxs-lookup"><span data-stu-id="1bbce-260">In hello above example, ```$onlineSch``` is an array with four elements that contains hello existing online protection schedule for hello Protection Group in hello GFS scheme:</span></span>

1. <span data-ttu-id="1bbce-261">```$onlineSch[0]```obsahuje denní plán hello</span><span class="sxs-lookup"><span data-stu-id="1bbce-261">```$onlineSch[0]``` contains hello daily schedule</span></span>
2. <span data-ttu-id="1bbce-262">```$onlineSch[1]```obsahuje Týdenní plán hello</span><span class="sxs-lookup"><span data-stu-id="1bbce-262">```$onlineSch[1]``` contains hello weekly schedule</span></span>
3. <span data-ttu-id="1bbce-263">```$onlineSch[2]```obsahuje plánování měsíčně hello</span><span class="sxs-lookup"><span data-stu-id="1bbce-263">```$onlineSch[2]``` contains hello monthly schedule</span></span>
4. <span data-ttu-id="1bbce-264">```$onlineSch[3]```obsahuje roční plán hello</span><span class="sxs-lookup"><span data-stu-id="1bbce-264">```$onlineSch[3]``` contains hello yearly schedule</span></span>

<span data-ttu-id="1bbce-265">Takže pokud budete potřebovat toomodify hello týdenní plán, je třeba toorefer toohello ```$onlineSch[1]```.</span><span class="sxs-lookup"><span data-stu-id="1bbce-265">So if you need toomodify hello weekly schedule, you need toorefer toohello ```$onlineSch[1]```.</span></span>

### <a name="initial-backup"></a><span data-ttu-id="1bbce-266">Prvotní zálohování</span><span class="sxs-lookup"><span data-stu-id="1bbce-266">Initial backup</span></span>
<span data-ttu-id="1bbce-267">Při zálohování zdroje dat pro hello poprvé, aplikace DPM musí vytvoří počáteční repliky, vytvoří úplnou kopii toobe hello zdroje dat chráněné na svazku repliky DPM.</span><span class="sxs-lookup"><span data-stu-id="1bbce-267">When backing up a datasource for hello first time, DPM needs creates initial replica that creates a full copy of hello datasource toobe protected on DPM replica volume.</span></span> <span data-ttu-id="1bbce-268">Tato aktivita bude naplánována buď pro určitý čas nebo lze spustit ručně, pomocí hello [Set-DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) rutiny s parametrem hello ```-NOW```.</span><span class="sxs-lookup"><span data-stu-id="1bbce-268">This activity can either be scheduled for a specific time, or can be triggered manually, using hello [Set-DPMReplicaCreationMethod](https://technet.microsoft.com/library/hh881715) cmdlet with hello parameter ```-NOW```.</span></span>

```
PS C:\> Set-DPMReplicaCreationMethod -ProtectionGroup $MPG -NOW
```
### <a name="changing-hello-size-of-dpm-replica--recovery-point-volume"></a><span data-ttu-id="1bbce-269">Změna velikosti hello repliky aplikace DPM a svazek bodu obnovení</span><span class="sxs-lookup"><span data-stu-id="1bbce-269">Changing hello size of DPM Replica & recovery point volume</span></span>
<span data-ttu-id="1bbce-270">Můžete také změnit hello velikost svazku repliky DPM a stínové kopie svazku pomocí [Set-DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) rutiny jako hello následující ukázka: Get-DatasourceDiskAllocation - Datasource $DS Set-DatasourceDiskAllocation - Datasource $DS - ProtectionGroup $MPG – ruční - ReplicaArea (2 gb) - ShadowCopyArea (2 gb)</span><span class="sxs-lookup"><span data-stu-id="1bbce-270">You can also change hello size of DPM Replica volume and Shadow Copy volume using [Set-DPMDatasourceDiskAllocation](https://technet.microsoft.com/library/hh881618.aspx) cmdlet as in hello following example: Get-DatasourceDiskAllocation -Datasource $DS Set-DatasourceDiskAllocation -Datasource $DS -ProtectionGroup $MPG -manual -ReplicaArea (2gb) -ShadowCopyArea (2gb)</span></span>

### <a name="committing-hello-changes-toohello-protection-group"></a><span data-ttu-id="1bbce-271">Potvrzení hello změny toohello skupiny ochrany</span><span class="sxs-lookup"><span data-stu-id="1bbce-271">Committing hello changes toohello Protection Group</span></span>
<span data-ttu-id="1bbce-272">Nakonec hello změny nutné toobe potvrzené předtím, než aplikace DPM může provést zálohování hello za hello novou konfiguraci skupiny ochrany.</span><span class="sxs-lookup"><span data-stu-id="1bbce-272">Finally, hello changes need toobe committed before DPM can take hello backup per hello new Protection Group configuration.</span></span> <span data-ttu-id="1bbce-273">Toho lze dosáhnout pomocí hello [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) rutiny.</span><span class="sxs-lookup"><span data-stu-id="1bbce-273">This can be achieved using hello [Set-DPMProtectionGroup](https://technet.microsoft.com/library/hh881758) cmdlet.</span></span>

```
PS C:\> Set-DPMProtectionGroup -ProtectionGroup $MPG
```
## <a name="view-hello-backup-points"></a><span data-ttu-id="1bbce-274">Body zálohy hello zobrazení</span><span class="sxs-lookup"><span data-stu-id="1bbce-274">View hello backup points</span></span>
<span data-ttu-id="1bbce-275">Můžete použít hello [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) rutiny tooget seznam všech bodů obnovení pro zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="1bbce-275">You can use hello [Get-DPMRecoveryPoint](https://technet.microsoft.com/library/hh881746) cmdlet tooget a list of all recovery points for a datasource.</span></span> <span data-ttu-id="1bbce-276">V tomto příkladu provedeme následující:</span><span class="sxs-lookup"><span data-stu-id="1bbce-276">In this example, we will:</span></span>

* <span data-ttu-id="1bbce-277">načtení všech hello PGs na serveru DPM hello a uloženy do pole```$PG```</span><span class="sxs-lookup"><span data-stu-id="1bbce-277">fetch all hello PGs on hello DPM server and stored in an array ```$PG```</span></span>
* <span data-ttu-id="1bbce-278">získat odpovídající toohello hello zdrojů dat```$PG[0]```</span><span class="sxs-lookup"><span data-stu-id="1bbce-278">get hello datasources corresponding toohello ```$PG[0]```</span></span>
* <span data-ttu-id="1bbce-279">získání všech hello bodů obnovení pro zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="1bbce-279">get all hello recovery points for a datasource.</span></span>

```
PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online
```

## <a name="restore-data-protected-on-azure"></a><span data-ttu-id="1bbce-280">Obnovit data chráněná v Azure</span><span class="sxs-lookup"><span data-stu-id="1bbce-280">Restore data protected on Azure</span></span>
<span data-ttu-id="1bbce-281">Obnovení dat je kombinací ```RecoverableItem``` objektu a ```RecoveryOption``` objektu.</span><span class="sxs-lookup"><span data-stu-id="1bbce-281">Restoring data is a combination of a ```RecoverableItem``` object and a ```RecoveryOption``` object.</span></span> <span data-ttu-id="1bbce-282">V předchozí části hello My seznam hello body zálohy pro zdroj dat.</span><span class="sxs-lookup"><span data-stu-id="1bbce-282">In hello previous section, we got a list of hello backup points for a datasource.</span></span>

<span data-ttu-id="1bbce-283">V příkladu hello dole ukážeme, jak toorestore Hyper-V virtuální počítač z Azure Backup kombinování zálohování body s hello cíle pro obnovení.</span><span class="sxs-lookup"><span data-stu-id="1bbce-283">In hello example below, we demonstrate how toorestore a Hyper-V virtual machine from Azure Backup by combining backup points with hello target for recovery.</span></span> <span data-ttu-id="1bbce-284">Tento příklad obsahuje:</span><span class="sxs-lookup"><span data-stu-id="1bbce-284">This example includes:</span></span>

* <span data-ttu-id="1bbce-285">Vytváření možnost obnovení pomocí hello [New-DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) rutiny.</span><span class="sxs-lookup"><span data-stu-id="1bbce-285">Creating a recovery option using hello  [New-DPMRecoveryOption](https://technet.microsoft.com/library/hh881592) cmdlet.</span></span>
* <span data-ttu-id="1bbce-286">Načítání pole hello body zálohy pomocí hello ```Get-DPMRecoveryPoint``` rutiny.</span><span class="sxs-lookup"><span data-stu-id="1bbce-286">Fetching hello array of backup points using hello ```Get-DPMRecoveryPoint``` cmdlet.</span></span>
* <span data-ttu-id="1bbce-287">Výběr zálohování toorestore bodu z.</span><span class="sxs-lookup"><span data-stu-id="1bbce-287">Choosing a backup point toorestore from.</span></span>

```
PS C:\> $RecoveryOption = New-DPMRecoveryOption -HyperVDatasource -TargetServer "HVDCenter02" -RecoveryLocation AlternateHyperVServer -RecoveryType Recover -TargetLocation “C:\VMRecovery”

PS C:\> $PG = Get-DPMProtectionGroup –DPMServerName "TestingServer"
PS C:\> $DS = Get-DPMDatasource -ProtectionGroup $PG[0]
PS C:\> $RecoveryPoints = Get-DPMRecoverypoint -Datasource $DS[0] -Online

PS C:\> Restore-DPMRecoverableItem -RecoverableItem $RecoveryPoints[0] -RecoveryOption $RecoveryOption
```

<span data-ttu-id="1bbce-288">pro jakýkoli typ zdroje dat lze snadno rozšířit Hello příkazy.</span><span class="sxs-lookup"><span data-stu-id="1bbce-288">hello commands can easily be extended for any datasource type.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1bbce-289">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1bbce-289">Next steps</span></span>
* <span data-ttu-id="1bbce-290">Další informace o DPM najdete v části tooAzure zálohování [Úvod tooDPM zálohování](backup-azure-dpm-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="1bbce-290">For more information about DPM tooAzure Backup see [Introduction tooDPM Backup](backup-azure-dpm-introduction.md)</span></span>
