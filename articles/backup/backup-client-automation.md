---
title: "aaaUse prostředí PowerShell tooback až Windows serveru tooAzure | Microsoft Docs"
description: "Zjistěte, jak toodeploy a spravovat Azure Backup pomocí prostředí PowerShell"
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
ms.openlocfilehash: f13224f53abd6fbd132fee4347b0b99e8f5e2678
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-backup-tooazure-for-windows-serverwindows-client-using-powershell"></a><span data-ttu-id="cef06-103">Nasazení a správě zálohování tooAzure pro klienta systému Windows Server a Windows pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="cef06-103">Deploy and manage backup tooAzure for Windows Server/Windows Client using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="cef06-104">ARM</span><span class="sxs-lookup"><span data-stu-id="cef06-104">ARM</span></span>](backup-client-automation.md)
> * [<span data-ttu-id="cef06-105">Classic</span><span class="sxs-lookup"><span data-stu-id="cef06-105">Classic</span></span>](backup-client-automation-classic.md)
>
>

<span data-ttu-id="cef06-106">Tento článek ukazuje, jak toouse prostředí PowerShell pro nastavení služby Azure Backup na Windows serveru nebo klienta Windows a Správa zálohování a obnovení.</span><span class="sxs-lookup"><span data-stu-id="cef06-106">This article shows you how toouse PowerShell for setting up Azure Backup on Windows Server or a Windows client, and managing backup and recovery.</span></span>

## <a name="install-azure-powershell"></a><span data-ttu-id="cef06-107">Instalace prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="cef06-107">Install Azure PowerShell</span></span>
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

<span data-ttu-id="cef06-108">Tento článek se zaměřuje na hello Azure Resource Manager (ARM) a hello rutiny prostředí PowerShell zálohování Online s MS, povolit toouse trezoru služeb zotavení ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="cef06-108">This article focuses on hello Azure Resource Manager (ARM) and hello MS Online Backup PowerShell cmdlets that enable you toouse a Recovery Services vault in a resource group.</span></span>

<span data-ttu-id="cef06-109">Října 2015 byla vydána Azure PowerShell 1.0.</span><span class="sxs-lookup"><span data-stu-id="cef06-109">In October 2015, Azure PowerShell 1.0 was released.</span></span> <span data-ttu-id="cef06-110">Tato verze byla úspěšná hello 0.9.8 verze a uvést do režimu o významné změny, zejména v vzoru pro pojmenovávání hello rutin hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-110">This release succeeded hello 0.9.8 release and brought about some significant changes, especially in hello naming pattern of hello cmdlets.</span></span> <span data-ttu-id="cef06-111">postupujte podle 1.0 rutiny hello formát pojmenování {sloveso}-AzureRm {podstatné jméno}; vzhledem k tomu, názvy hello 0.9.8 neobsahují **Rm** (třeba New-AzureRmResourceGroup místo New-AzureResourceGroup).</span><span class="sxs-lookup"><span data-stu-id="cef06-111">1.0 cmdlets follow hello naming pattern {verb}-AzureRm{noun}; whereas, hello 0.9.8 names do not include **Rm** (for example, New-AzureRmResourceGroup instead of New-AzureResourceGroup).</span></span> <span data-ttu-id="cef06-112">Pokud používáte prostředí Azure PowerShell 0.9.8, musíte nejdřív povolit režimu Resource Manager hello spuštěním hello **Switch-AzureMode AzureResourceManager** příkaz.</span><span class="sxs-lookup"><span data-stu-id="cef06-112">When using Azure PowerShell 0.9.8, you must first enable hello Resource Manager mode by running hello **Switch-AzureMode AzureResourceManager** command.</span></span> <span data-ttu-id="cef06-113">Tento příkaz není nutné v 1.0 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="cef06-113">This command is not necessary in 1.0 or later.</span></span>

<span data-ttu-id="cef06-114">Pokud chcete toouse skripty napsané pro prostředí hello 0.9.8, v hello 1.0 nebo novější prostředí, měli pečlivě aktualizace a testování skriptů hello v předprodukčním prostředí, než je použijete v produkčním tooavoid neočekávané dopad.</span><span class="sxs-lookup"><span data-stu-id="cef06-114">If you want toouse your scripts written for hello 0.9.8 environment, in hello 1.0 or later environment, you should carefully update and test hello scripts in a pre-production environment before using them in production tooavoid unexpected impact.</span></span>

<span data-ttu-id="cef06-115">[Stáhněte si nejnovější verzi prostředí PowerShell hello](https://github.com/Azure/azure-powershell/releases) (minimální požadovaná verze je: 1.0.0)</span><span class="sxs-lookup"><span data-stu-id="cef06-115">[Download hello latest PowerShell release](https://github.com/Azure/azure-powershell/releases) (minimum version required is : 1.0.0)</span></span>

[!INCLUDE [arm-getting-setup-powershell](../../includes/arm-getting-setup-powershell.md)]

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="cef06-116">Vytvoření trezoru služby Recovery Services</span><span class="sxs-lookup"><span data-stu-id="cef06-116">Create a recovery services vault</span></span>
<span data-ttu-id="cef06-117">Hello následující kroky vás provedou vytvoření trezoru služeb zotavení.</span><span class="sxs-lookup"><span data-stu-id="cef06-117">hello following steps lead you through creating a Recovery Services vault.</span></span> <span data-ttu-id="cef06-118">Trezor služeb zotavení se liší od úložiště záloh.</span><span class="sxs-lookup"><span data-stu-id="cef06-118">A Recovery Services vault is different than a Backup vault.</span></span>

1. <span data-ttu-id="cef06-119">Pokud používáte Azure Backup pro hello poprvé, musíte použít hello **Register-AzureRMResourceProvider** rutiny tooregister hello službu Azure Recovery zprostředkovatele s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="cef06-119">If you are using Azure Backup for hello first time, you must use hello **Register-AzureRMResourceProvider** cmdlet tooregister hello Azure Recovery Service provider with your subscription.</span></span>

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
2. <span data-ttu-id="cef06-120">Hello trezor služeb zotavení je ARM prostředek, takže je třeba tooplace ji ve skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="cef06-120">hello Recovery Services vault is an ARM resource, so you need tooplace it within a Resource Group.</span></span> <span data-ttu-id="cef06-121">Můžete použít existující skupinu prostředků nebo vytvořte novou.</span><span class="sxs-lookup"><span data-stu-id="cef06-121">You can use an existing resource group, or create a new one.</span></span> <span data-ttu-id="cef06-122">Když vytváříte novou skupinu prostředků, zadejte hello název a umístění pro skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-122">When creating a new resource group, specify hello name and location for hello resource group.</span></span>  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "WestUS"
    ```
3. <span data-ttu-id="cef06-123">Použití hello **New-AzureRmRecoveryServicesVault** rutiny toocreate hello nový trezor.</span><span class="sxs-lookup"><span data-stu-id="cef06-123">Use hello **New-AzureRmRecoveryServicesVault** cmdlet toocreate hello new vault.</span></span> <span data-ttu-id="cef06-124">Ujistěte se, toospecify hello stejné umístění pro hello trezoru, protože byl použit pro skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-124">Be sure toospecify hello same location for hello vault as was used for hello resource group.</span></span>

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "WestUS"
    ```
4. <span data-ttu-id="cef06-125">Zadejte typ hello toouse redundance úložiště; můžete použít [místně redundantní úložiště (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) nebo [geograficky redundantní úložiště (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span><span class="sxs-lookup"><span data-stu-id="cef06-125">Specify hello type of storage redundancy toouse; you can use [Locally Redundant Storage (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) or [Geo Redundant Storage (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span></span> <span data-ttu-id="cef06-126">Hello následující příklad ukazuje, že je možnost hello - BackupStorageRedundancy pro testVault nastavena tooGeoRedundant.</span><span class="sxs-lookup"><span data-stu-id="cef06-126">hello following example shows hello -BackupStorageRedundancy option for testVault is set tooGeoRedundant.</span></span>

   > [!TIP]
   > <span data-ttu-id="cef06-127">Mnoho rutin Azure Backup vyžadují objekt trezoru služeb zotavení hello jako vstup.</span><span class="sxs-lookup"><span data-stu-id="cef06-127">Many Azure Backup cmdlets require hello Recovery Services vault object as an input.</span></span> <span data-ttu-id="cef06-128">Z tohoto důvodu je vhodné toostore hello služeb zotavení zálohy trezoru objekt v proměnné.</span><span class="sxs-lookup"><span data-stu-id="cef06-128">For this reason, it is convenient toostore hello Backup Recovery Services vault object in a variable.</span></span>
   >
   >

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testVault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

## <a name="view-hello-vaults-in-a-subscription"></a><span data-ttu-id="cef06-129">Zobrazení hello trezorů v předplatném.</span><span class="sxs-lookup"><span data-stu-id="cef06-129">View hello vaults in a subscription</span></span>
<span data-ttu-id="cef06-130">Použití **Get-AzureRmRecoveryServicesVault** tooview hello seznam všech trezorů v aktuálním předplatném hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-130">Use **Get-AzureRmRecoveryServicesVault** tooview hello list of all vaults in hello current subscription.</span></span> <span data-ttu-id="cef06-131">Můžete použít tento příkaz toocheck, zda byl vytvořen nový trezor nebo toosee jaké trezory jsou k dispozici v rámci předplatného hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-131">You can use this command toocheck that a new  vault was created, or toosee what vaults are available in hello subscription.</span></span>

<span data-ttu-id="cef06-132">Spusťte příkaz hello **Get-AzureRmRecoveryServicesVault**, a jsou uvedeny všechny trezorů v předplatném hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-132">Run hello command, **Get-AzureRmRecoveryServicesVault**, and all vaults in hello subscription are listed.</span></span>

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


## <a name="installing-hello-azure-backup-agent"></a><span data-ttu-id="cef06-133">Instalace agenta Azure Backup hello</span><span class="sxs-lookup"><span data-stu-id="cef06-133">Installing hello Azure Backup agent</span></span>
<span data-ttu-id="cef06-134">Před instalací agenta Azure Backup hello, musíte instalační program toohave hello stažené a na serveru Windows hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-134">Before you install hello Azure Backup agent, you need toohave hello installer downloaded and present on hello Windows Server.</span></span> <span data-ttu-id="cef06-135">Můžete získat nejnovější verzi instalačního programu hello hello hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) nebo z trezoru služeb zotavení hello stránka řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="cef06-135">You can get hello latest version of hello installer from hello [Microsoft Download Center](http://aka.ms/azurebackup_agent) or from hello Recovery Services vault's Dashboard page.</span></span> <span data-ttu-id="cef06-136">Uložení instalačního programu hello tooan snadno dostupné místo jako * C:\Downloads\*.</span><span class="sxs-lookup"><span data-stu-id="cef06-136">Save hello installer tooan easily accessible location like *C:\Downloads\*.</span></span>

<span data-ttu-id="cef06-137">Můžete taky použijte programu hello tooget prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="cef06-137">Alternatively, use PowerShell tooget hello downloader:</span></span>
 
 ```
 $MarsAURL = 'Http://Aka.Ms/Azurebackup_Agent'
 $WC = New-Object System.Net.WebClient
 $WC.DownloadFile($MarsAURL,'C:\downloads\MARSAgentInstaller.EXE')
 C:\Downloads\MARSAgentInstaller.EXE /q
 ```

<span data-ttu-id="cef06-138">tooinstall hello agenta, spusťte následující příkaz v konzolu prostředí PowerShell se zvýšenými oprávněními hello:</span><span class="sxs-lookup"><span data-stu-id="cef06-138">tooinstall hello agent, run hello following command in an elevated PowerShell console:</span></span>

```
PS C:\> MARSAgentInstaller.exe /q
```

<span data-ttu-id="cef06-139">Tím se nainstaluje hello agent se všemi možnostmi výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-139">This installs hello agent with all hello default options.</span></span> <span data-ttu-id="cef06-140">Hello instalace trvá několik minut v pozadí hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-140">hello installation takes a few minutes in hello background.</span></span> <span data-ttu-id="cef06-141">Pokud nezadáte hello */nu* možnost pak hello **Windows Update** otevře se okno na konci hello hello toocheck instalace pro všechny aktualizace.</span><span class="sxs-lookup"><span data-stu-id="cef06-141">If you do not specify hello */nu* option then hello **Windows Update** window will open at hello end of hello installation toocheck for any updates.</span></span> <span data-ttu-id="cef06-142">Po instalaci agenta hello se zobrazí v seznamu nainstalovaných programů hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-142">Once installed, hello agent will show in hello list of installed programs.</span></span>

<span data-ttu-id="cef06-143">toosee hello seznamu nainstalovaných programů, přejděte příliš**ovládací panely** > **programy** > **programy a funkce**.</span><span class="sxs-lookup"><span data-stu-id="cef06-143">toosee hello list of installed programs, go too**Control Panel** > **Programs** > **Programs and Features**.</span></span>

![Instalaci agenta](./media/backup-client-automation/installed-agent-listing.png)

### <a name="installation-options"></a><span data-ttu-id="cef06-145">Možnosti instalace</span><span class="sxs-lookup"><span data-stu-id="cef06-145">Installation options</span></span>
<span data-ttu-id="cef06-146">toosee, které všechny možnosti, které jsou k dispozici prostřednictvím hello hello příkazového řádku, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="cef06-146">toosee all hello options available via hello command-line, use hello following command:</span></span>

```
PS C:\> MARSAgentInstaller.exe /?
```

<span data-ttu-id="cef06-147">Hello dostupné možnosti patří:</span><span class="sxs-lookup"><span data-stu-id="cef06-147">hello available options include:</span></span>

| <span data-ttu-id="cef06-148">Možnost</span><span class="sxs-lookup"><span data-stu-id="cef06-148">Option</span></span> | <span data-ttu-id="cef06-149">Podrobnosti</span><span class="sxs-lookup"><span data-stu-id="cef06-149">Details</span></span> | <span data-ttu-id="cef06-150">Výchozí</span><span class="sxs-lookup"><span data-stu-id="cef06-150">Default</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cef06-151">/q</span><span class="sxs-lookup"><span data-stu-id="cef06-151">/q</span></span> |<span data-ttu-id="cef06-152">Tichá instalace</span><span class="sxs-lookup"><span data-stu-id="cef06-152">Quiet installation</span></span> |- |
| <span data-ttu-id="cef06-153">/ p: "umístění"</span><span class="sxs-lookup"><span data-stu-id="cef06-153">/p:"location"</span></span> |<span data-ttu-id="cef06-154">Cesta toohello instalační složku pro agenta Azure Backup hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-154">Path toohello installation folder for hello Azure Backup agent.</span></span> |<span data-ttu-id="cef06-155">C:\Program Files\Microsoft Azure Recovery Services agenta</span><span class="sxs-lookup"><span data-stu-id="cef06-155">C:\Program Files\Microsoft Azure Recovery Services Agent</span></span> |
| <span data-ttu-id="cef06-156">/ s: "umístění"</span><span class="sxs-lookup"><span data-stu-id="cef06-156">/s:"location"</span></span> |<span data-ttu-id="cef06-157">Složka mezipaměti toohello cestu pro agenta Azure Backup hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-157">Path toohello cache folder for hello Azure Backup agent.</span></span> |<span data-ttu-id="cef06-158">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span><span class="sxs-lookup"><span data-stu-id="cef06-158">C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch</span></span> |
| <span data-ttu-id="cef06-159">/m</span><span class="sxs-lookup"><span data-stu-id="cef06-159">/m</span></span> |<span data-ttu-id="cef06-160">Výslovný souhlas tooMicrosoft aktualizace</span><span class="sxs-lookup"><span data-stu-id="cef06-160">Opt-in tooMicrosoft Update</span></span> |- |
| <span data-ttu-id="cef06-161">/nu</span><span class="sxs-lookup"><span data-stu-id="cef06-161">/nu</span></span> |<span data-ttu-id="cef06-162">Nekontrolovat aktualizace po dokončení instalace</span><span class="sxs-lookup"><span data-stu-id="cef06-162">Do not Check for updates after installation is complete</span></span> |- |
| <span data-ttu-id="cef06-163">/d</span><span class="sxs-lookup"><span data-stu-id="cef06-163">/d</span></span> |<span data-ttu-id="cef06-164">Odinstaluje Agenta Microsoft Azure Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="cef06-164">Uninstalls Microsoft Azure Recovery Services Agent</span></span> |- |
| <span data-ttu-id="cef06-165">/pH</span><span class="sxs-lookup"><span data-stu-id="cef06-165">/ph</span></span> |<span data-ttu-id="cef06-166">Adresa proxy hostitele</span><span class="sxs-lookup"><span data-stu-id="cef06-166">Proxy Host Address</span></span> |- |
| <span data-ttu-id="cef06-167">/Po</span><span class="sxs-lookup"><span data-stu-id="cef06-167">/po</span></span> |<span data-ttu-id="cef06-168">Číslo portu proxy hostitele</span><span class="sxs-lookup"><span data-stu-id="cef06-168">Proxy Host Port Number</span></span> |- |
| <span data-ttu-id="cef06-169">/Pu</span><span class="sxs-lookup"><span data-stu-id="cef06-169">/pu</span></span> |<span data-ttu-id="cef06-170">Uživatelské jméno proxy hostitele</span><span class="sxs-lookup"><span data-stu-id="cef06-170">Proxy Host UserName</span></span> |- |
| <span data-ttu-id="cef06-171">/pW</span><span class="sxs-lookup"><span data-stu-id="cef06-171">/pw</span></span> |<span data-ttu-id="cef06-172">Heslo pro proxy server</span><span class="sxs-lookup"><span data-stu-id="cef06-172">Proxy Password</span></span> |- |

## <a name="registering-windows-server-or-windows-client-machine-tooa-recovery-services-vault"></a><span data-ttu-id="cef06-173">Registrace systému Windows Server a Windows klientský počítač tooa trezoru služeb zotavení</span><span class="sxs-lookup"><span data-stu-id="cef06-173">Registering Windows Server or Windows client machine tooa Recovery Services Vault</span></span>
<span data-ttu-id="cef06-174">Po vytvoření trezoru služeb zotavení hello stáhnout nejnovější verzi agenta hello a přihlašovací údaje trezoru hello a uložte ho do vhodného umístění jako C:\Downloads.</span><span class="sxs-lookup"><span data-stu-id="cef06-174">After you created hello Recovery Services vault, download hello latest agent and hello vault credentials and store it in a convenient location like C:\Downloads.</span></span>

```
PS C:\> $credspath = "C:\downloads"
PS C:\> $credsfilename = Get-AzureRmRecoveryServicesVaultSettingsFile -Backup -Vault $vault1 -Path  $credspath
```

<span data-ttu-id="cef06-175">Na serveru Windows hello nebo klientský počítač systému Windows, spusťte hello [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) rutiny tooregister hello počítač s hello trezoru.</span><span class="sxs-lookup"><span data-stu-id="cef06-175">On hello Windows Server or Windows client machine, run hello [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx) cmdlet tooregister hello machine with hello vault.</span></span>
<span data-ttu-id="cef06-176">Tato a další rutiny používané pro zálohování, jsou z modulu MSONLINE hello které hello Mars AgentInstaller přidat jako součást procesu instalace hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-176">This, and other cmdlets used for backup, are from hello MSONLINE module which hello Mars AgentInstaller added as part of hello installation process.</span></span> 

<span data-ttu-id="cef06-177">Instalační program agenta Hello neaktualizuje hello $Env: PSModulePath proměnné.</span><span class="sxs-lookup"><span data-stu-id="cef06-177">hello Agent installer does not update hello $Env:PSModulePath variable.</span></span> <span data-ttu-id="cef06-178">To znamená, že automaticky načíst modul selže.</span><span class="sxs-lookup"><span data-stu-id="cef06-178">This means module auto-load fails.</span></span> <span data-ttu-id="cef06-179">tooresolve to můžete provést následující hello:</span><span class="sxs-lookup"><span data-stu-id="cef06-179">tooresolve this you can do hello following:</span></span>

```
PS C:\>  $Env:psmodulepath += ';C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules
```

<span data-ttu-id="cef06-180">Alternativně můžete ručně načíst modul hello ve vašem skriptu následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="cef06-180">Alternatively, you can manually load hello module in your script as follows:</span></span>

```
PS C:\>  Import-Module  'C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules\MSOnlineBackup'

```

<span data-ttu-id="cef06-181">Po načtení hello Online zálohování rutiny zaregistrujete přihlašovací údaje trezoru hello:</span><span class="sxs-lookup"><span data-stu-id="cef06-181">Once you load hello Online Backup cmdlets, you register hello vault credentials:</span></span>


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
> <span data-ttu-id="cef06-182">Nepoužívejte soubor s relativní cesty toospecify hello přihlašovacími údaji.</span><span class="sxs-lookup"><span data-stu-id="cef06-182">Do not use relative paths toospecify hello vault credentials file.</span></span> <span data-ttu-id="cef06-183">Musíte zadat absolutní cestu jako vstupní toohello rutiny.</span><span class="sxs-lookup"><span data-stu-id="cef06-183">You must provide an absolute path as an input toohello cmdlet.</span></span>
>
>

## <a name="networking-settings"></a><span data-ttu-id="cef06-184">Nastavení sítě</span><span class="sxs-lookup"><span data-stu-id="cef06-184">Networking settings</span></span>
<span data-ttu-id="cef06-185">Při připojení hello hello Windows počítač toohello je Internetu prostřednictvím proxy serveru, nastavení proxy serveru hello se dá zadat i toohello agenta.</span><span class="sxs-lookup"><span data-stu-id="cef06-185">When hello connectivity of hello Windows machine toohello internet is through a proxy server, hello proxy settings can also be provided toohello agent.</span></span> <span data-ttu-id="cef06-186">V tomto příkladu neexistuje žádný proxy server, takže jsme jsou explicitně vymazání údaje související s proxy serverem.</span><span class="sxs-lookup"><span data-stu-id="cef06-186">In this example, there is no proxy server, so we are explicitly clearing any proxy-related information.</span></span>

<span data-ttu-id="cef06-187">Využití šířky pásma také lze kontrolovat pomocí možnosti hello ```work hour bandwidth``` a ```non-work hour bandwidth``` pro danou sadu dny v týdnu hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-187">Bandwidth usage can also be controlled with hello options of ```work hour bandwidth``` and ```non-work hour bandwidth``` for a given set of days of hello week.</span></span>

<span data-ttu-id="cef06-188">Nastavení serveru proxy a šířky pásma podrobnosti hello se provádí pomocí hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) rutiny:</span><span class="sxs-lookup"><span data-stu-id="cef06-188">Setting hello proxy and bandwidth details is done using hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409%28v=wps.630%29.aspx) cmdlet:</span></span>

```
PS C:\> Set-OBMachineSetting -NoProxy
Server properties updated successfully.

PS C:\> Set-OBMachineSetting -NoThrottle
Server properties updated successfully.
```

## <a name="encryption-settings"></a><span data-ttu-id="cef06-189">Nastavení šifrování</span><span class="sxs-lookup"><span data-stu-id="cef06-189">Encryption settings</span></span>
<span data-ttu-id="cef06-190">zálohování dat odesílaných tooAzure Hello zálohování je šifrovaný tooprotect hello důvěrnost dat hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-190">hello backup data sent tooAzure Backup is encrypted tooprotect hello confidentiality of hello data.</span></span> <span data-ttu-id="cef06-191">šifrovací přístupové heslo Hello jsou hello "password" toodecrypt hello data v době obnovení hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-191">hello encryption passphrase is hello "password" toodecrypt hello data at hello time of restore.</span></span>

```
PS C:\> ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force | Set-OBMachineSetting
PS C:\> $PassPhrase = ConvertTo-SecureString -String "Complex!123_STRING" -AsPlainText -Force 
PS C:\> $PassCode   = 'AzureR0ckx'
PS C:\> Set-OBMachineSetting -EncryptionPassPhrase $PassPhrase
Server properties updated successfully
```

> [!IMPORTANT]
> <span data-ttu-id="cef06-192">Zachování informací o přístupové heslo hello bezpečném, po nastavení.</span><span class="sxs-lookup"><span data-stu-id="cef06-192">Keep hello passphrase information safe and secure once it is set.</span></span> <span data-ttu-id="cef06-193">Nejsou-li být schopný toorestore dat z Azure bez tohoto hesla.</span><span class="sxs-lookup"><span data-stu-id="cef06-193">You are not be able toorestore data from Azure without this passphrase.</span></span>
>
>

## <a name="back-up-files-and-folders"></a><span data-ttu-id="cef06-194">Zálohování souborů a složek</span><span class="sxs-lookup"><span data-stu-id="cef06-194">Back up files and folders</span></span>
<span data-ttu-id="cef06-195">Zásady se řídí všech záloh z Windows serverů a klientů tooAzure zálohování.</span><span class="sxs-lookup"><span data-stu-id="cef06-195">All backups from Windows Servers and clients tooAzure Backup are governed by a policy.</span></span> <span data-ttu-id="cef06-196">Hello zásad se skládá ze tří částí:</span><span class="sxs-lookup"><span data-stu-id="cef06-196">hello policy comprises three parts:</span></span>

1. <span data-ttu-id="cef06-197">A **plán zálohování** určující, kdy zálohování potřebovat toobe provést a synchronizovat se službou hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-197">A **backup schedule** that specifies when backups need toobe taken and synchronized with hello service.</span></span>
2. <span data-ttu-id="cef06-198">A **plán uchovávání informací** určující, jak dlouho body obnovení hello tooretain v Azure.</span><span class="sxs-lookup"><span data-stu-id="cef06-198">A **retention schedule** that specifies how long tooretain hello recovery points in Azure.</span></span>
3. <span data-ttu-id="cef06-199">A **zadání zahrnutí nebo vyloučení souboru** který určuje, co by měl být zálohovány.</span><span class="sxs-lookup"><span data-stu-id="cef06-199">A **file inclusion/exclusion specification** that dictates what should be backed up.</span></span>

<span data-ttu-id="cef06-200">V tomto dokumentu protože jsme se automatizaci zálohování, budete předpokládáme, že nic nebyl nakonfigurován.</span><span class="sxs-lookup"><span data-stu-id="cef06-200">In this document, since we're automating backup, we'll assume nothing has been configured.</span></span> <span data-ttu-id="cef06-201">Začneme vytvořením nové zásady zálohování pomocí hello [New-OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="cef06-201">We begin by creating a new backup policy using hello [New-OBPolicy](https://technet.microsoft.com/library/hh770416.aspx) cmdlet.</span></span>

```
PS C:\> $newpolicy = New-OBPolicy
```

<span data-ttu-id="cef06-202">V tento čas hello je prázdné zásady a jiné rutiny jsou potřebné toodefine co položky budou zahrnout nebo vyloučit, při zálohování spustí a kde hello zálohy budou uloženy.</span><span class="sxs-lookup"><span data-stu-id="cef06-202">At this time hello policy is empty and other cmdlets are needed toodefine what items will be included or excluded, when backups will run, and where hello backups will be stored.</span></span>

### <a name="configuring-hello-backup-schedule"></a><span data-ttu-id="cef06-203">Konfigurace plánu zálohování hello</span><span class="sxs-lookup"><span data-stu-id="cef06-203">Configuring hello backup schedule</span></span>
<span data-ttu-id="cef06-204">nejprve Hello hello 3 části zásady je plán zálohování hello, který je vytvořený pomocí hello [New-OBSchedule](https://technet.microsoft.com/library/hh770401) rutiny.</span><span class="sxs-lookup"><span data-stu-id="cef06-204">hello first of hello 3 parts of a policy is hello backup schedule, which is created using hello [New-OBSchedule](https://technet.microsoft.com/library/hh770401) cmdlet.</span></span> <span data-ttu-id="cef06-205">plán zálohování Hello definuje při zálohování potřebovat toobe provedena.</span><span class="sxs-lookup"><span data-stu-id="cef06-205">hello backup schedule defines when backups need toobe taken.</span></span> <span data-ttu-id="cef06-206">Při vytváření plánu je třeba toospecify 2 vstupní parametry:</span><span class="sxs-lookup"><span data-stu-id="cef06-206">When creating a schedule you need toospecify 2 input parameters:</span></span>

* <span data-ttu-id="cef06-207">**Počet dnů v týdnu hello** zálohování hello měly být spuštěny.</span><span class="sxs-lookup"><span data-stu-id="cef06-207">**Days of hello week** that hello backup should run.</span></span> <span data-ttu-id="cef06-208">Můžete spustit úlohy zálohování hello pouze jeden den nebo každý den v týdnu hello nebo libovolnou kombinaci mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="cef06-208">You can run hello backup job on just one day, or every day of hello week, or any combination in between.</span></span>
* <span data-ttu-id="cef06-209">**Hello denní dobu** spuštění zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-209">**Times of hello day** when hello backup should run.</span></span> <span data-ttu-id="cef06-210">Můžete definovat až too3 různou dobu hello, když se spustí zálohování hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-210">You can define up too3 different times of hello day when hello backup will be triggered.</span></span>

<span data-ttu-id="cef06-211">Například může nakonfigurovat zásady zálohování, který se spustí na 16: 00 každou sobotu a neděli.</span><span class="sxs-lookup"><span data-stu-id="cef06-211">For instance, you could configure a backup policy that runs at 4PM every Saturday and Sunday.</span></span>

```
PS C:\> $sched = New-OBSchedule -DaysofWeek Saturday, Sunday -TimesofDay 16:00
```

<span data-ttu-id="cef06-212">plán zálohování Hello potřebuje toobe přidružené k zásadě a toho lze dosáhnout pomocí hello [Set-OBSchedule](https://technet.microsoft.com/library/hh770407) rutiny.</span><span class="sxs-lookup"><span data-stu-id="cef06-212">hello backup schedule needs toobe associated with a policy, and this can be achieved by using hello [Set-OBSchedule](https://technet.microsoft.com/library/hh770407) cmdlet.</span></span>

```
PS C:> Set-OBSchedule -Policy $newpolicy -Schedule $sched
BackupSchedule : 4:00 PM Saturday, Sunday, Every 1 week(s) DsList : PolicyName : RetentionPolicy : State : New PolicyState : Valid
```
### <a name="configuring-a-retention-policy"></a><span data-ttu-id="cef06-213">Konfigurace zásad uchovávání</span><span class="sxs-lookup"><span data-stu-id="cef06-213">Configuring a retention policy</span></span>
<span data-ttu-id="cef06-214">zásady uchovávání informací Hello definuje, jak dlouho obnovení, které se uchovají body vytvořené z úlohy zálohování.</span><span class="sxs-lookup"><span data-stu-id="cef06-214">hello retention policy defines how long recovery points created from backup jobs are retained.</span></span> <span data-ttu-id="cef06-215">Při vytváření nových zásad uchovávání informací pomocí hello [New-OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) rutiny, můžete zadat třeba hello počet dní, které hello body obnovení záloh toobe uchovávají s Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="cef06-215">When creating a new retention policy using hello [New-OBRetentionPolicy](https://technet.microsoft.com/library/hh770425) cmdlet, you can specify hello number of days that hello backup recovery points need toobe retained with Azure Backup.</span></span> <span data-ttu-id="cef06-216">Následující příklad Hello nastaví zásady uchovávání informací 7 dní.</span><span class="sxs-lookup"><span data-stu-id="cef06-216">hello example below sets a retention policy of 7 days.</span></span>

```
PS C:\> $retentionpolicy = New-OBRetentionPolicy -RetentionDays 7
```

<span data-ttu-id="cef06-217">Hello zásady uchovávání informací musí být přidruženy k hlavní zásady hello pomocí rutiny hello [Set-OBRetentionPolicy](https://technet.microsoft.com/library/hh770405):</span><span class="sxs-lookup"><span data-stu-id="cef06-217">hello retention policy must be associated with hello main policy using hello cmdlet [Set-OBRetentionPolicy](https://technet.microsoft.com/library/hh770405):</span></span>

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
### <a name="including-and-excluding-files-toobe-backed-up"></a><span data-ttu-id="cef06-218">Zahrnutí a vyloučení souborů toobe zálohovat</span><span class="sxs-lookup"><span data-stu-id="cef06-218">Including and excluding files toobe backed up</span></span>
<span data-ttu-id="cef06-219">```OBFileSpec``` Objekt definuje hello soubory toobe zahrnout a vyloučit zálohu.</span><span class="sxs-lookup"><span data-stu-id="cef06-219">An ```OBFileSpec``` object defines hello files toobe included and excluded in a backup.</span></span> <span data-ttu-id="cef06-220">Toto je sada pravidel, že obor se hello chráněné soubory a složky na počítači.</span><span class="sxs-lookup"><span data-stu-id="cef06-220">This is a set of rules that scope out hello protected files and folders on a machine.</span></span> <span data-ttu-id="cef06-221">Můžete mít mnoho souboru pravidla zahrnutí nebo vyloučení podle potřeby a přidružovat je k zásadě.</span><span class="sxs-lookup"><span data-stu-id="cef06-221">You can have as many file inclusion or exclusion rules as required, and associate them with a policy.</span></span> <span data-ttu-id="cef06-222">Při vytváření nového objektu OBFileSpec, můžete:</span><span class="sxs-lookup"><span data-stu-id="cef06-222">When creating a new OBFileSpec object, you can:</span></span>

* <span data-ttu-id="cef06-223">Zadejte soubory a složky toobe hello zahrnuté</span><span class="sxs-lookup"><span data-stu-id="cef06-223">Specify hello files and folders toobe included</span></span>
* <span data-ttu-id="cef06-224">Zadejte soubory a složky toobe hello vyloučené</span><span class="sxs-lookup"><span data-stu-id="cef06-224">Specify hello files and folders toobe excluded</span></span>
* <span data-ttu-id="cef06-225">Zadejte rekurzivní zálohování dat v složku (nebo) zda pouze hello nejvyšší úrovně soubory v zadané složce hello by měl být zálohuje nahoru.</span><span class="sxs-lookup"><span data-stu-id="cef06-225">Specify recursive backup of data in a folder (or) whether only hello top-level files in hello specified folder should be backed up.</span></span>

<span data-ttu-id="cef06-226">Hello pozdější se dosáhne použitím příznaku - nerekurzivní hello v příkazu New-OBFileSpec hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-226">hello latter is achieved by using hello -NonRecursive flag in hello New-OBFileSpec command.</span></span>

<span data-ttu-id="cef06-227">V příkladu hello níže jsme budete zálohování svazku C: a D: a vyloučit hello binární soubory operačního systému ve složce Windows hello a všechny dočasné složky.</span><span class="sxs-lookup"><span data-stu-id="cef06-227">In hello example below, we'll back up volume C: and D: and exclude hello OS binaries in hello Windows folder and any temporary folders.</span></span> <span data-ttu-id="cef06-228">toodo, vytvoříme dvě specifikace souborů pomocí hello [New-OBFileSpec](https://technet.microsoft.com/library/hh770408) rutiny – jeden pro zahrnutí a jeden pro vyloučení.</span><span class="sxs-lookup"><span data-stu-id="cef06-228">toodo so we'll create two file specifications using hello [New-OBFileSpec](https://technet.microsoft.com/library/hh770408) cmdlet - one for inclusion and one for exclusion.</span></span> <span data-ttu-id="cef06-229">Po vytvoření specifikace hello souborů jsou přidružené zásady hello použitím hello [přidat OBFileSpec](https://technet.microsoft.com/library/hh770424) rutiny.</span><span class="sxs-lookup"><span data-stu-id="cef06-229">Once hello file specifications have been created, they're associated with hello policy using hello [Add-OBFileSpec](https://technet.microsoft.com/library/hh770424) cmdlet.</span></span>

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

### <a name="applying-hello-policy"></a><span data-ttu-id="cef06-230">Použití zásad hello</span><span class="sxs-lookup"><span data-stu-id="cef06-230">Applying hello policy</span></span>
<span data-ttu-id="cef06-231">Teď objektu zásad hello dokončení a má přidružené plánu zálohování, zásady uchovávání informací a seznam souborů zahrnutí nebo vyloučení.</span><span class="sxs-lookup"><span data-stu-id="cef06-231">Now hello policy object is complete and has an associated backup schedule, retention policy, and an inclusion/exclusion list of files.</span></span> <span data-ttu-id="cef06-232">Tato zásada může být nyní potvrdit pro toouse Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="cef06-232">This policy can now be committed for Azure Backup toouse.</span></span> <span data-ttu-id="cef06-233">Před použitím hello nově vytvořený zásad zajistěte, aby žádné existující zásady zálohování, které jsou přidružené k serveru hello pomocí hello [odebrat OBPolicy](https://technet.microsoft.com/library/hh770415) rutiny.</span><span class="sxs-lookup"><span data-stu-id="cef06-233">Before you apply hello newly created policy ensure that there are no existing backup policies associated with hello server by using hello [Remove-OBPolicy](https://technet.microsoft.com/library/hh770415) cmdlet.</span></span> <span data-ttu-id="cef06-234">Odebírání hello zásady zobrazí výzvu k potvrzení.</span><span class="sxs-lookup"><span data-stu-id="cef06-234">Removing hello policy will prompt for confirmation.</span></span> <span data-ttu-id="cef06-235">potvrzení hello tooskip použít hello ```-Confirm:$false``` příznak pomocí rutiny hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-235">tooskip hello confirmation use hello ```-Confirm:$false``` flag with hello cmdlet.</span></span>

```
PS C:> Get-OBPolicy | Remove-OBPolicy
Microsoft Azure Backup Are you sure you want tooremove this backup policy? This will delete all hello backed up data. [Y] Yes [A] Yes tooAll [N] No [L] No tooAll [S] Suspend [?] Help (default is "Y"):
```

<span data-ttu-id="cef06-236">Objekt zásad spouštění hello se provádí pomocí hello [Set-OBPolicy](https://technet.microsoft.com/library/hh770421) rutiny.</span><span class="sxs-lookup"><span data-stu-id="cef06-236">Committing hello policy object is done using hello [Set-OBPolicy](https://technet.microsoft.com/library/hh770421) cmdlet.</span></span> <span data-ttu-id="cef06-237">Také to požádá o potvrzení.</span><span class="sxs-lookup"><span data-stu-id="cef06-237">This will also ask for confirmation.</span></span> <span data-ttu-id="cef06-238">potvrzení hello tooskip použít hello ```-Confirm:$false``` příznak pomocí rutiny hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-238">tooskip hello confirmation use hello ```-Confirm:$false``` flag with hello cmdlet.</span></span>

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

<span data-ttu-id="cef06-239">Můžete zobrazit podrobnosti o hello hello existující zásady zálohování pomocí hello [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) rutiny.</span><span class="sxs-lookup"><span data-stu-id="cef06-239">You can view hello details of hello existing backup policy using hello [Get-OBPolicy](https://technet.microsoft.com/library/hh770406) cmdlet.</span></span> <span data-ttu-id="cef06-240">Vám může procházení další pomocí hello [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) rutiny pro plán zálohování hello a hello [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) rutina pro zásady uchovávání informací hello</span><span class="sxs-lookup"><span data-stu-id="cef06-240">You can drill-down further using hello [Get-OBSchedule](https://technet.microsoft.com/library/hh770423) cmdlet for hello backup schedule and hello [Get-OBRetentionPolicy](https://technet.microsoft.com/library/hh770427) cmdlet for hello retention policies</span></span>

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

### <a name="performing-an-ad-hoc-backup"></a><span data-ttu-id="cef06-241">Provádění zálohu ad-hoc</span><span class="sxs-lookup"><span data-stu-id="cef06-241">Performing an ad-hoc backup</span></span>
<span data-ttu-id="cef06-242">Jakmile byla nastavena zásada zálohování hello zálohy dojde za hello plán.</span><span class="sxs-lookup"><span data-stu-id="cef06-242">Once a backup policy has been set hello backups will occur per hello schedule.</span></span> <span data-ttu-id="cef06-243">Spuštění služby ad-hoc zálohy je také možné pomocí hello [Start-OBBackup](https://technet.microsoft.com/library/hh770426) rutiny:</span><span class="sxs-lookup"><span data-stu-id="cef06-243">Triggering an ad-hoc backup is also possible using hello [Start-OBBackup](https://technet.microsoft.com/library/hh770426) cmdlet:</span></span>

```
PS C:> Get-OBPolicy | Start-OBBackup
Initializing
Taking snapshot of volumes...
Preparing storage...
Generating backup metadata information and preparing hello metadata VHD...
Data transfer is in progress. It might take longer since it is hello first backup and all data needs toobe transferred...
Data transfer completed and all backed up data is in hello cloud. Verifying data integrity...
Data transfer completed
In progress...
Job completed.
hello backup operation completed successfully.
```

## <a name="restore-data-from-azure-backup"></a><span data-ttu-id="cef06-244">Obnovení dat z Azure Backup</span><span class="sxs-lookup"><span data-stu-id="cef06-244">Restore data from Azure Backup</span></span>
<span data-ttu-id="cef06-245">Tato část vás provede kroky hello pro automatizaci obnovení dat z Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="cef06-245">This section will guide you through hello steps for automating recovery of data from Azure Backup.</span></span> <span data-ttu-id="cef06-246">To zahrnuje tak hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="cef06-246">Doing so involves hello following steps:</span></span>

1. <span data-ttu-id="cef06-247">Vyberte zdrojový svazek hello</span><span class="sxs-lookup"><span data-stu-id="cef06-247">Pick hello source volume</span></span>
2. <span data-ttu-id="cef06-248">Vyberte zálohování toorestore bodu</span><span class="sxs-lookup"><span data-stu-id="cef06-248">Choose a backup point toorestore</span></span>
3. <span data-ttu-id="cef06-249">Zvolte toorestore položky</span><span class="sxs-lookup"><span data-stu-id="cef06-249">Choose an item toorestore</span></span>
4. <span data-ttu-id="cef06-250">Proces obnovení hello aktivační události</span><span class="sxs-lookup"><span data-stu-id="cef06-250">Trigger hello restore process</span></span>

### <a name="picking-hello-source-volume"></a><span data-ttu-id="cef06-251">Výběr hello zdrojový svazek</span><span class="sxs-lookup"><span data-stu-id="cef06-251">Picking hello source volume</span></span>
<span data-ttu-id="cef06-252">V pořadí toorestore položku z Azure Backup je nutné nejprve tooidentify hello zdroj položky hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-252">In order toorestore an item from Azure Backup, you first need tooidentify hello source of hello item.</span></span> <span data-ttu-id="cef06-253">Vzhledem k tomu, že jsme se provádění příkazů hello v kontextu hello systému Windows Server nebo klienta Windows, hello počítač je už definovaný.</span><span class="sxs-lookup"><span data-stu-id="cef06-253">Since we're executing hello commands in hello context of a Windows Server or a Windows client, hello machine is already identified.</span></span> <span data-ttu-id="cef06-254">dalším krokem Hello při identifikaci zdroje hello je tooidentify hello svazku, který ji obsahuje.</span><span class="sxs-lookup"><span data-stu-id="cef06-254">hello next step in identifying hello source is tooidentify hello volume containing it.</span></span> <span data-ttu-id="cef06-255">Seznam svazků nebo zdrojů zálohovaných z tohoto počítače se dá načíst spuštěním hello [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) rutiny.</span><span class="sxs-lookup"><span data-stu-id="cef06-255">A list of volumes or sources being backed up from this machine can be retrieved by executing hello [Get-OBRecoverableSource](https://technet.microsoft.com/library/hh770410) cmdlet.</span></span> <span data-ttu-id="cef06-256">Tento příkaz vrátí pole všech zdrojů hello zálohovat z tohoto serveru nebo klienta.</span><span class="sxs-lookup"><span data-stu-id="cef06-256">This command returns an array of all hello sources backed up from this server/client.</span></span>

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

### <a name="choosing-a-backup-point-from-which-toorestore"></a><span data-ttu-id="cef06-257">Výběr bodu zálohy z které toorestore</span><span class="sxs-lookup"><span data-stu-id="cef06-257">Choosing a backup point from which toorestore</span></span>
<span data-ttu-id="cef06-258">Načíst seznam body zálohy spuštěním hello [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) rutiny s příslušnými parametry.</span><span class="sxs-lookup"><span data-stu-id="cef06-258">You retreive a list of backup points by executing hello [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdlet with appropriate parameters.</span></span> <span data-ttu-id="cef06-259">V našem příkladu jsme vybrali hello nejnovější bod zálohy pro zdrojový svazek hello *D:* a použít ho toorecover konkrétní soubor.</span><span class="sxs-lookup"><span data-stu-id="cef06-259">In our example, we’ll choose hello latest backup point for hello source volume *D:* and use it toorecover a specific file.</span></span>

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
<span data-ttu-id="cef06-260">objekt Hello ```$rps``` je pole body zálohy.</span><span class="sxs-lookup"><span data-stu-id="cef06-260">hello object ```$rps``` is an array of backup points.</span></span> <span data-ttu-id="cef06-261">první prvek Hello je poslední bod hello a n-tou element hello je nejstarší bod hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-261">hello first element is hello latest point and hello Nth element is hello oldest point.</span></span> <span data-ttu-id="cef06-262">toochoose hello posledního bodu, použijeme ```$rps[0]```.</span><span class="sxs-lookup"><span data-stu-id="cef06-262">toochoose hello latest point, we will use ```$rps[0]```.</span></span>

### <a name="choosing-an-item-toorestore"></a><span data-ttu-id="cef06-263">Výběr toorestore položky</span><span class="sxs-lookup"><span data-stu-id="cef06-263">Choosing an item toorestore</span></span>
<span data-ttu-id="cef06-264">tooidentify hello přesný soubor nebo složku toorestore, rekurzivně použít hello [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="cef06-264">tooidentify hello exact file or folder toorestore, recursively use hello [Get-OBRecoverableItem](https://technet.microsoft.com/library/hh770399.aspx) cmdlet.</span></span> <span data-ttu-id="cef06-265">Hierarchie složky hello tento způsob, jak lze procházet výhradně pomocí hello ```Get-OBRecoverableItem```.</span><span class="sxs-lookup"><span data-stu-id="cef06-265">That way hello folder hierarchy can be browsed solely using hello ```Get-OBRecoverableItem```.</span></span>

<span data-ttu-id="cef06-266">V tomto příkladu, pokud chceme souboru hello toorestore *finances.xls* jsme můžete odkazovat pomocí tohoto objektu hello ```$filesFolders[1]```.</span><span class="sxs-lookup"><span data-stu-id="cef06-266">In this example, if we want toorestore hello file *finances.xls* we can reference that using hello object ```$filesFolders[1]```.</span></span>

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

<span data-ttu-id="cef06-267">Můžete také vyhledat položky toorestore pomocí hello ```Get-OBRecoverableItem``` rutiny.</span><span class="sxs-lookup"><span data-stu-id="cef06-267">You can also search for items toorestore using hello ```Get-OBRecoverableItem``` cmdlet.</span></span> <span data-ttu-id="cef06-268">V našem příkladu toosearch pro *finances.xls* nám může získat popisovač souboru hello spuštěním tohoto příkazu:</span><span class="sxs-lookup"><span data-stu-id="cef06-268">In our example, toosearch for *finances.xls* we could get a handle on hello file by running this command:</span></span>

```
PS C:\> $item = Get-OBRecoverableItem -RecoveryPoint $rps[0] -Location "D:\MyData" -SearchString "finance*"
```

### <a name="triggering-hello-restore-process"></a><span data-ttu-id="cef06-269">Spuštění procesu obnovení hello</span><span class="sxs-lookup"><span data-stu-id="cef06-269">Triggering hello restore process</span></span>
<span data-ttu-id="cef06-270">proces obnovení hello tootrigger, potřebujeme nejprve možnosti obnovení toospecify hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-270">tootrigger hello restore process, we first need toospecify hello recovery options.</span></span> <span data-ttu-id="cef06-271">Tento krok můžete provést pomocí hello [New-OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) rutiny.</span><span class="sxs-lookup"><span data-stu-id="cef06-271">This can be done by using hello [New-OBRecoveryOption](https://technet.microsoft.com/library/hh770417.aspx) cmdlet.</span></span> <span data-ttu-id="cef06-272">Pro tento příklad předpokládejme, že chceme, aby soubory hello toorestore příliš*C:\temp*. Také Předpokládejme, že má být tooskip soubory, které již existují na cílovou složku hello *C:\temp*. toocreate takové možnost obnovení pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="cef06-272">For this example, let's assume that we want toorestore hello files too*C:\temp*. Let's also assume that we want tooskip files that already exist on hello destination folder *C:\temp*. toocreate such a recovery option, use hello following command:</span></span>

```
PS C:\> $recovery_option = New-OBRecoveryOption -DestinationPath "C:\temp" -OverwriteType Skip
```

<span data-ttu-id="cef06-273">Teď spustit proces obnovení hello pomocí hello [Start-OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) příkaz na hello vybrané ```$item``` z hello výstup hello ```Get-OBRecoverableItem``` rutiny:</span><span class="sxs-lookup"><span data-stu-id="cef06-273">Now trigger hello restore process by using hello [Start-OBRecovery](https://technet.microsoft.com/library/hh770402.aspx) command on hello selected ```$item``` from hello output of hello ```Get-OBRecoverableItem``` cmdlet:</span></span>

```
PS C:\> Start-OBRecovery -RecoverableItem $item -RecoveryOption $recover_option
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Estimating size of backup items...
Job completed.
hello recovery operation completed successfully.
```


## <a name="uninstalling-hello-azure-backup-agent"></a><span data-ttu-id="cef06-274">Odinstalace agenta Azure Backup hello</span><span class="sxs-lookup"><span data-stu-id="cef06-274">Uninstalling hello Azure Backup agent</span></span>
<span data-ttu-id="cef06-275">Odinstalaci agenta Azure Backup hello lze provést pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="cef06-275">Uninstalling hello Azure Backup agent can be done by using hello following command:</span></span>

```
PS C:\> .\MARSAgentInstaller.exe /d /q
```

<span data-ttu-id="cef06-276">Odinstalace binárních souborů hello agenta z počítače hello má některé tooconsider důsledky:</span><span class="sxs-lookup"><span data-stu-id="cef06-276">Uninstalling hello agent binaries from hello machine has some consequences tooconsider:</span></span>

* <span data-ttu-id="cef06-277">Odebere z počítače hello soubor filtru hello a sledování změn je zastavena.</span><span class="sxs-lookup"><span data-stu-id="cef06-277">It removes hello file-filter from hello machine, and tracking of changes is stopped.</span></span>
* <span data-ttu-id="cef06-278">Všechny informace o zásadách se odebere z počítače hello, ale informace o zásadách hello pokračuje toobe uložené ve službě hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-278">All policy information is removed from hello machine, but hello policy information continues toobe stored in hello service.</span></span>
* <span data-ttu-id="cef06-279">Se odeberou všechny plány zálohování a jsou provedeny žádné další zálohy.</span><span class="sxs-lookup"><span data-stu-id="cef06-279">All backup schedules are removed, and no further backups are taken.</span></span>

<span data-ttu-id="cef06-280">Ale hello data uložená v Azure zůstane a zůstane podle nastavení zásad uchovávání hello vy.</span><span class="sxs-lookup"><span data-stu-id="cef06-280">However, hello data stored in Azure remains and is retained as per hello retention policy setup by you.</span></span> <span data-ttu-id="cef06-281">Starší body jsou automaticky stará.</span><span class="sxs-lookup"><span data-stu-id="cef06-281">Older points are automatically aged out.</span></span>

## <a name="remote-management"></a><span data-ttu-id="cef06-282">Vzdálená správa</span><span class="sxs-lookup"><span data-stu-id="cef06-282">Remote management</span></span>
<span data-ttu-id="cef06-283">Veškerá Správa hello kolem hello agenta Azure Backup, zásady a zdroje dat lze provést vzdáleně pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="cef06-283">All hello management around hello Azure Backup agent, policies, and data sources can be done remotely through PowerShell.</span></span> <span data-ttu-id="cef06-284">Hello počítač, který bude spravovat vzdáleně musí toobe připravený správně.</span><span class="sxs-lookup"><span data-stu-id="cef06-284">hello machine that will be managed remotely needs toobe prepared correctly.</span></span>

<span data-ttu-id="cef06-285">Ve výchozím nastavení je Vzdálená správa systému Windows hello nakonfigurovaná pro ruční spuštění.</span><span class="sxs-lookup"><span data-stu-id="cef06-285">By default, hello WinRM service is configured for manual startup.</span></span> <span data-ttu-id="cef06-286">musí být nastaven typ spouštění Hello příliš*automatické* a musí být spuštěna služba hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-286">hello startup type must be set too*Automatic* and hello service should be started.</span></span> <span data-ttu-id="cef06-287">tooverify, který hello Služba WinRM je spuštěná, by měly být hello hodnotu hello Vlastnost Status *systémem*.</span><span class="sxs-lookup"><span data-stu-id="cef06-287">tooverify that hello WinRM service is running, hello value of hello Status property should be *Running*.</span></span>

```
PS C:\> Get-Service WinRM

Status   Name               DisplayName
------   ----               -----------
Running  winrm              Windows Remote Management (WS-Manag...
```

<span data-ttu-id="cef06-288">Prostředí PowerShell by měl být nakonfigurovaný pro vzdálenou komunikaci.</span><span class="sxs-lookup"><span data-stu-id="cef06-288">PowerShell should be configured for remoting.</span></span>

```
PS C:\> Enable-PSRemoting -force
WinRM is already set up tooreceive requests on this computer.
WinRM has been updated for remote management.
WinRM firewall exception enabled.

PS C:\> Set-ExecutionPolicy unrestricted -force
```

<span data-ttu-id="cef06-289">počítač Hello teď můžete spravovat vzdáleně – od hello instalace agenta hello.</span><span class="sxs-lookup"><span data-stu-id="cef06-289">hello machine can now be managed remotely - starting from hello installation of hello agent.</span></span> <span data-ttu-id="cef06-290">Například následující skript hello zkopíruje hello agenta toohello vzdáleného počítače a ho nainstaluje.</span><span class="sxs-lookup"><span data-stu-id="cef06-290">For example, hello following script copies hello agent toohello remote machine and installs it.</span></span>

```
PS C:\> $dloc = "\\REMOTESERVER01\c$\Windows\Temp"
PS C:\> $agent = "\\REMOTESERVER01\c$\Windows\Temp\MARSAgentInstaller.exe"
PS C:\> $args = "/q"
PS C:\> Copy-Item "C:\Downloads\MARSAgentInstaller.exe" -Destination $dloc - force

PS C:\> $s = New-PSSession -ComputerName REMOTESERVER01
PS C:\> Invoke-Command -Session $s -Script { param($d, $a) Start-Process -FilePath $d $a -Wait } -ArgumentList $agent $args
```

## <a name="next-steps"></a><span data-ttu-id="cef06-291">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cef06-291">Next steps</span></span>
<span data-ttu-id="cef06-292">Další informace o Azure Backup pro Windows Server nebo klienta najdete v tématu</span><span class="sxs-lookup"><span data-stu-id="cef06-292">For more information about Azure Backup for Windows Server/Client see</span></span>

* [<span data-ttu-id="cef06-293">Úvod tooAzure zálohování</span><span class="sxs-lookup"><span data-stu-id="cef06-293">Introduction tooAzure Backup</span></span>](backup-introduction-to-azure-backup.md)
* [<span data-ttu-id="cef06-294">Zálohování serverů Windows</span><span class="sxs-lookup"><span data-stu-id="cef06-294">Back up Windows Servers</span></span>](backup-configure-vault.md)
