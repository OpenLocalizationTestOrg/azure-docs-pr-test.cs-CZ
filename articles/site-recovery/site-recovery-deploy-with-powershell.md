---
title: "aaaReplicate tooAzure virtuálních počítačů Hyper-V na portálu classic hello pomocí prostředí PowerShell | Microsoft Docs"
description: "Automatizovat hello replikaci virtuálních počítačů technologie Hyper-V v cloudech VMM pomocí Site Recovery a prostředí PowerShell na portálu classic hello"
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhiag
editor: tysonn
ms.assetid: 9011f567-e0b4-4306-951a-b30da19f5db6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: d6847b46ac227209e6890de4ab603b23f827360f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-vms-tooazure-with-powershell-in-hello-classic-portal"></a><span data-ttu-id="f7090-103">Replikovat virtuální počítače Hyper-V tooAzure pomocí prostředí PowerShell na portálu classic hello</span><span class="sxs-lookup"><span data-stu-id="f7090-103">Replicate Hyper-V VMs tooAzure with PowerShell in hello classic portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f7090-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f7090-104">Azure Portal</span></span>](site-recovery-vmm-to-azure.md)
> * [<span data-ttu-id="f7090-105">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="f7090-105">PowerShell - Resource Manager</span></span>](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [<span data-ttu-id="f7090-106">Portál Classic</span><span class="sxs-lookup"><span data-stu-id="f7090-106">Classic Portal</span></span>](site-recovery-vmm-to-azure-classic.md)
> * [<span data-ttu-id="f7090-107">PowerShell – Classic</span><span class="sxs-lookup"><span data-stu-id="f7090-107">PowerShell - Classic</span></span>](site-recovery-deploy-with-powershell.md)
>
>

## <a name="overview"></a><span data-ttu-id="f7090-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="f7090-108">Overview</span></span>
<span data-ttu-id="f7090-109">Azure Site Recovery přispívá tooyour obchodní kontinuitu a po havárii (BCDR) strategii zotavení tím, že orchestruje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů v různých scénářích nasazení.</span><span class="sxs-lookup"><span data-stu-id="f7090-109">Azure Site Recovery contributes tooyour business continuity and disaster recovery (BCDR) strategy by orchestrating replication, failover and recovery of virtual machines in a number of deployment scenarios.</span></span> <span data-ttu-id="f7090-110">Úplný seznam nasazení scénářů najdete v části hello [přehled Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f7090-110">For a full list of deployment scenarios see hello [Azure Site Recovery overview](site-recovery-overview.md).</span></span>

<span data-ttu-id="f7090-111">Tento článek ukazuje, jak toouse prostředí PowerShell tooautomate běžné úlohy je nutné tooperform při nastavení Azure Site Recovery tooreplicate technologie Hyper-V virtuálních počítačů v System Center VMM cloudy tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="f7090-111">This article shows you how toouse PowerShell tooautomate common tasks you need tooperform when you set up Azure Site Recovery tooreplicate Hyper-V virtual machines in System Center VMM clouds tooAzure storage.</span></span>

<span data-ttu-id="f7090-112">Hello článek obsahuje požadavky pro hello scénář a ukazuje, jak instalovat tooset nahoru k trezoru Site Recovery zprostředkovatele Azure Site Recovery hello na zdrojovém serveru VMM hello, zaregistrujte hello server v hello trezoru, přidat účet úložiště Azure, nainstalujte hello Azure Agent služeb zotavení na hostitelských serverech technologie Hyper-V, nakonfigurovat nastavení ochrany pro cloudy VMM, které budou použité tooall chráněné virtuální počítače a pak povolte ochranu pro tyto virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="f7090-112">hello article includes prerequisites for hello scenario, and shows you how tooset up a Site Recovery vault, install hello Azure Site Recovery Provider on hello source VMM server, register hello server in hello vault, add an Azure storage account, install hello Azure Recovery Services agent on Hyper-V host servers, configure protection settings for VMM clouds that will be applied tooall protected virtual machines, and then enable protection for those virtual machines.</span></span> <span data-ttu-id="f7090-113">Nakonec testování převzetí služeb při selhání hello toomake se, že vše funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="f7090-113">Finish up by testing hello failover toomake sure everything's working as expected.</span></span>

<span data-ttu-id="f7090-114">Pokud narazíte na problémy nastavení tento scénář, zveřejněte svoje otázky na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="f7090-114">If you run into problems setting up this scenario, post your questions on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

> [!NOTE]
> <span data-ttu-id="f7090-115">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="f7090-115">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="f7090-116">Tento článek se zabývá pomocí modelu nasazení Classic hello.</span><span class="sxs-lookup"><span data-stu-id="f7090-116">This article covers using hello Classic deployment model.</span></span>
>
>

## <a name="before-you-start"></a><span data-ttu-id="f7090-117">Než začnete</span><span class="sxs-lookup"><span data-stu-id="f7090-117">Before you start</span></span>
<span data-ttu-id="f7090-118">Ujistěte se, že máte zavedenou tyto požadavky:</span><span class="sxs-lookup"><span data-stu-id="f7090-118">Make sure you have these prerequisites in place:</span></span>

### <a name="azure-prerequisites"></a><span data-ttu-id="f7090-119">Požadavky Azure</span><span class="sxs-lookup"><span data-stu-id="f7090-119">Azure prerequisites</span></span>
* <span data-ttu-id="f7090-120">Budete potřebovat účet [Microsoft Azure](https://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="f7090-120">You'll need a [Microsoft Azure](https://azure.microsoft.com/) account.</span></span> <span data-ttu-id="f7090-121">Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f7090-121">You can start with a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="f7090-122">Budete potřebovat data toostore replikovat účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="f7090-122">You'll need an Azure storage account toostore replicated data.</span></span> <span data-ttu-id="f7090-123">Hello účet potřebuje povolenou geografickou replikací.</span><span class="sxs-lookup"><span data-stu-id="f7090-123">hello account needs geo-replication enabled.</span></span> <span data-ttu-id="f7090-124">By mělo být ve stejné oblasti jako trezor Azure Site Recovery hello hello a přidružen hello stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="f7090-124">It should be in hello same region as hello Azure Site Recovery vault and be associated with hello same subscription.</span></span> <span data-ttu-id="f7090-125">[Další informace o Azure storage](../storage/common/storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f7090-125">[Learn more about Azure storage](../storage/common/storage-introduction.md).</span></span>
* <span data-ttu-id="f7090-126">Musíte mít jistotu, že virtuální počítače, které chcete tooprotect v souladu s toomake [požadavky virtuálního počítače Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span><span class="sxs-lookup"><span data-stu-id="f7090-126">You'll need toomake sure that virtual machines you want tooprotect comply with [Azure virtual machine prerequisites](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span></span>

### <a name="vmm-prerequisites"></a><span data-ttu-id="f7090-127">Požadavky VMM</span><span class="sxs-lookup"><span data-stu-id="f7090-127">VMM prerequisites</span></span>
* <span data-ttu-id="f7090-128">Budete potřebovat VMM server běžící na System Center 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="f7090-128">You'll need  VMM server running on System Center 2012 R2.</span></span>
* <span data-ttu-id="f7090-129">Budete potřebovat alespoň jeden cloud na serveru VMM má tooprotect hello.</span><span class="sxs-lookup"><span data-stu-id="f7090-129">You'll need at least one cloud on hello VMM server you want tooprotect.</span></span> <span data-ttu-id="f7090-130">Hello cloud by měl obsahovat:</span><span class="sxs-lookup"><span data-stu-id="f7090-130">hello cloud should contain:</span></span>
  * <span data-ttu-id="f7090-131">Minimálně jednu skupinu hostitelů VMM.</span><span class="sxs-lookup"><span data-stu-id="f7090-131">One or more VMM host groups.</span></span>
  * <span data-ttu-id="f7090-132">Jeden nebo více hostitelských serverů Hyper-V nebo clusterů v každé skupině hostitelů.</span><span class="sxs-lookup"><span data-stu-id="f7090-132">One or more Hyper-V host servers or clusters in each host group .</span></span>
  * <span data-ttu-id="f7090-133">Jeden nebo více virtuálních počítačů na serveru hello zdroj technologie Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="f7090-133">One or more virtual machines on hello source Hyper-V server.</span></span>

### <a name="hyper-v-prerequisites"></a><span data-ttu-id="f7090-134">Požadavky technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="f7090-134">Hyper-V prerequisites</span></span>
* <span data-ttu-id="f7090-135">Hello servery hostitele technologie Hyper-V musí běžet minimálně **systému Windows Server 2012** s rolí Hyper-V nebo **Microsoft Hyper-V Server 2012** a mít hello nainstalované nejnovější aktualizace.</span><span class="sxs-lookup"><span data-stu-id="f7090-135">hello host Hyper-V servers must be running at least **Windows Server 2012** with Hyper-V role or **Microsoft Hyper-V Server 2012** and have hello latest updates installed.</span></span>
* <span data-ttu-id="f7090-136">Pokud používáte technologii Hyper-V v clusteru, je důležité vědět, že zprostředkovatel clusteru se nevytvoří automaticky, pokud máte clustery založené na statických IP adresách.</span><span class="sxs-lookup"><span data-stu-id="f7090-136">If you're running Hyper-V in a cluster note that cluster broker isn't created automatically if you have a static IP address-based cluster.</span></span> <span data-ttu-id="f7090-137">Zprostředkovatel clusteru hello tooconfigure budete potřebovat ručně.</span><span class="sxs-lookup"><span data-stu-id="f7090-137">You'll need tooconfigure hello cluster broker manually.</span></span> <span data-ttu-id="f7090-138">toodo to, ve Správci serveru > Správce clusteru převzetí služeb při selhání, připojte toohello cluster, klikněte na tlačítko **konfigurovat roli** a vyberte **zprostředkovatele replik technologie Hyper-V** v hello **vybrat roli**obrazovka Průvodce vysokou dostupností hello.</span><span class="sxs-lookup"><span data-stu-id="f7090-138">toodo this, in Server Manager > Failover Cluster Manager, connect toohello cluster, click **Configure Role** and select **Hyper-V Replica Broker** in hello **Select Role** screen of hello High Availability wizard.</span></span>
* <span data-ttu-id="f7090-139">Libovolný server hostitele technologie Hyper-V nebo clusteru, pro které chcete toomanage ochrany musí být součástí cloudu VMM.</span><span class="sxs-lookup"><span data-stu-id="f7090-139">Any Hyper-V host server or cluster for which you want toomanage protection must be included in a VMM cloud.</span></span>

### <a name="network-mapping-prerequisites"></a><span data-ttu-id="f7090-140">Požadavky na mapování sítě</span><span class="sxs-lookup"><span data-stu-id="f7090-140">Network mapping prerequisites</span></span>
<span data-ttu-id="f7090-141">Při ochraně virtuálních počítačů v síti Azure mapování mapuje sítě virtuálních počítačů na zdrojovém serveru VMM hello a cílové sítě Azure tooenable hello následující:</span><span class="sxs-lookup"><span data-stu-id="f7090-141">When you protect virtual machines in Azure network mapping maps between VM networks on hello source VMM server and target Azure networks tooenable hello following:</span></span>

* <span data-ttu-id="f7090-142">Všechny počítače, které převzetí služeb při selhání na hello stejné tooeach jiných, bez ohledu na plánu obnovení se mohou připojit.</span><span class="sxs-lookup"><span data-stu-id="f7090-142">All machines which fail over on hello same network can connect tooeach other, irrespective of which recovery plan they are in.</span></span>
* <span data-ttu-id="f7090-143">Pokud bránu sítě je nastavena na hello cílovou síť Azure, můžete virtuální počítače připojit tooother místní virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="f7090-143">If a network gateway is setup on hello target Azure network, virtual machines can connect tooother on-premises virtual machines.</span></span>
* <span data-ttu-id="f7090-144">Pokud nenakonfigurujete sítě mapování jenom virtuální počítače, které převzetí služeb při selhání v hello stejný plán obnovení bude možné tooconnect tooeach jiných po tooAzure převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="f7090-144">If you don’t configure network mapping only virtual machines that fail over in hello same recovery plan will be able tooconnect tooeach other after failover tooAzure.</span></span>

<span data-ttu-id="f7090-145">Pokud chcete, aby mapování sítě toodeploy budete potřebovat následující hello:</span><span class="sxs-lookup"><span data-stu-id="f7090-145">If you want toodeploy network mapping you'll need hello following:</span></span>

* <span data-ttu-id="f7090-146">Hello virtuálních počítačů má tooprotect na zdrojovém serveru VMM hello by měl být připojený tooa síť virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f7090-146">hello virtual machines you want tooprotect on hello source VMM server should be connected tooa VM network.</span></span> <span data-ttu-id="f7090-147">Tuto síť by měla být propojené tooa logické sítě, který je přidružený k hello cloudu.</span><span class="sxs-lookup"><span data-stu-id="f7090-147">That network should be linked tooa logical network that is associated with hello cloud.</span></span>
* <span data-ttu-id="f7090-148">Po převzetí služeb při selhání můžete připojit síti Azure toowhich replikovat virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="f7090-148">An Azure network toowhich replicated virtual machines can connect after failover.</span></span> <span data-ttu-id="f7090-149">Tuto síť vyberete v době hello převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="f7090-149">You'll select this network at hello time of failover.</span></span> <span data-ttu-id="f7090-150">Hello síť musí být ve hello stejné oblasti jako vaše předplatné Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="f7090-150">hello network should be in hello same region as your Azure Site Recovery subscription.</span></span>

### <a name="powershell-prerequisites"></a><span data-ttu-id="f7090-151">Požadavky prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="f7090-151">PowerShell prerequisites</span></span>
<span data-ttu-id="f7090-152">Ujistěte se, že máte připravené toogo prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f7090-152">Make sure you have Azure PowerShell ready toogo.</span></span> <span data-ttu-id="f7090-153">Pokud už používáte prostředí PowerShell, budete potřebovat tooupgrade tooversion 0.8.10 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="f7090-153">If you are already using PowerShell, you'll need tooupgrade tooversion 0.8.10 or later.</span></span> <span data-ttu-id="f7090-154">Informace o nastavení prostředí PowerShell najdete v tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="f7090-154">For information about setting up PowerShell, see [How tooinstall and configure Azure PowerShell](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="f7090-155">Po nastavení a nakonfigurovat prostředí PowerShell, můžete zobrazit všechny hello dostupných rutin služby hello [zde](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f7090-155">Once you have set up and configured PowerShell, you can view all of hello available cmdlets for hello service [here](/powershell/azure/overview).</span></span>

<span data-ttu-id="f7090-156">toolearn o tipy, které vám pomohou při používání hello rutin, jako je například jak hodnoty parametrů, vstupy a výstupy jsou obvykle zpracovávány v prostředí Azure PowerShell najdete v části [Začínáme s Azure rutiny](/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="f7090-156">toolearn about tips that can help you use hello cmdlets, such as how parameter values, inputs, and outputs are typically handled in Azure PowerShell, see [Get Started with Azure Cmdlets](/powershell/azure/get-started-azureps).</span></span>

## <a name="step-1-set-hello-subscription"></a><span data-ttu-id="f7090-157">Krok 1: Nastavení odběru hello</span><span class="sxs-lookup"><span data-stu-id="f7090-157">Step 1: Set hello subscription</span></span>
<span data-ttu-id="f7090-158">V prostředí PowerShell spusťte tyto rutiny:</span><span class="sxs-lookup"><span data-stu-id="f7090-158">In PowerShell, run these cmdlets:</span></span>

```
$UserName = "<user@live.com>"
$Password = "<password>"
$AzureSubscriptionName = "prod_sub1"

$SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $securePassword
Add-AzureAccount -Credential $Cred;
$AzureSubscription = Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

```

<span data-ttu-id="f7090-159">Hello elementů v rámci hello "< >" nahraďte konkrétní informace.</span><span class="sxs-lookup"><span data-stu-id="f7090-159">Replace hello elements within hello "< >" with your specific information.</span></span>

## <a name="step-2-create-a-site-recovery-vault"></a><span data-ttu-id="f7090-160">Krok 2: Vytvoření trezoru Site Recovery</span><span class="sxs-lookup"><span data-stu-id="f7090-160">Step 2: Create a Site Recovery vault</span></span>
<span data-ttu-id="f7090-161">V prostředí PowerShell hello elementů v rámci hello "< >" nahraďte konkrétní informace a spusťte tyto příkazy:</span><span class="sxs-lookup"><span data-stu-id="f7090-161">In PowerShell, replace hello elements within hello "< >" with your specific information, and run these commands:</span></span>

```

$VaultName = "<testvault123>"
$VaultGeo  = "<Southeast Asia>"
$OutputPathForSettingsFile = "<c:\>"

```


```
New-AzureSiteRecoveryVault -Location $VaultGeo -Name $VaultName;
$vault = Get-AzureSiteRecoveryVault -Name $VaultName;

```

## <a name="step-3-generate-a-vault-registration-key"></a><span data-ttu-id="f7090-162">Krok 3: Vygenerování registračního klíče trezoru</span><span class="sxs-lookup"><span data-stu-id="f7090-162">Step 3: Generate a vault registration key</span></span>
<span data-ttu-id="f7090-163">Vygenerujte registrační klíč v trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="f7090-163">Generate a registration key in hello vault.</span></span> <span data-ttu-id="f7090-164">Po stažení hello zprostředkovatele Azure Site Recovery a nainstalujte ji na serveru VMM hello, použijete tento klíč tooregister hello VMM server v trezoru hello.</span><span class="sxs-lookup"><span data-stu-id="f7090-164">After you download hello Azure Site Recovery Provider and install it on hello VMM server, you'll use this key tooregister hello VMM server in hello vault.</span></span>

1. <span data-ttu-id="f7090-165">Soubor nastavení hello úložiště získat a nastavit kontext hello:</span><span class="sxs-lookup"><span data-stu-id="f7090-165">Get hello vault setting file and set hello context:</span></span>

   ```

   $VaultName = "<testvault123>"
   $VaultGeo  = "<Southeast Asia>"
   $OutputPathForSettingsFile = "<c:\>"

   $VaultSetingsFile = Get-AzureSiteRecoveryVaultSettingsFile -Location $VaultGeo -Name $VaultName -Path $OutputPathForSettingsFile;

   ```
2. <span data-ttu-id="f7090-166">Nastavit kontext hello trezoru spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="f7090-166">Set hello vault context by running hello following commands:</span></span>

   ```

   $VaultSettingFilePath = $vaultSetingsFile.FilePath
   $VaultContext = Import-AzureSiteRecoveryVaultSettingsFile -Path $VaultSettingFilePath -ErrorAction Stop

   ```

## <a name="step-4-install-hello-azure-site-recovery-provider"></a><span data-ttu-id="f7090-167">Krok 4: Instalace hello zprostředkovatele Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="f7090-167">Step 4: Install hello Azure Site Recovery Provider</span></span>
1. <span data-ttu-id="f7090-168">Na počítači VMM hello vytvořte adresář spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f7090-168">On hello VMM machine, create a directory by running hello following command:</span></span>

   ```

   pushd C:\ASR\

   ```
2. <span data-ttu-id="f7090-169">Extrahujte soubory hello pomocí zprostředkovatele hello stáhnout tak, že spustíte následující příkaz hello</span><span class="sxs-lookup"><span data-stu-id="f7090-169">Extract hello files using hello downloaded provider by running hello following command</span></span>

   ```

   AzureSiteRecoveryProvider.exe /x:. /q

   ```
3. <span data-ttu-id="f7090-170">Nainstalujte zprostředkovatele hello pomocí hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="f7090-170">Install hello provider using hello following commands:</span></span>

   ```

   .\SetupDr.exe /i

   ```

   ```

   $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
   do
   {
     $isNotInstalled = $true;
     if(Test-Path $installationRegPath)
     {
         $isNotInstalled = $false;
     }
   }While($isNotInstalled)

   ```

   <span data-ttu-id="f7090-171">Počkejte toofinish instalace hello.</span><span class="sxs-lookup"><span data-stu-id="f7090-171">Wait for hello installation toofinish.</span></span>
4. <span data-ttu-id="f7090-172">Hello server zaregistrujte v trezoru hello pomocí hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f7090-172">Register hello server in hello vault using hello following command:</span></span>

   ```

   $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
   pushd $BinPath
   $encryptionFilePath = "C:\temp\"
   .\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

   ```

## <a name="step-5-create-an-azure-storage-account"></a><span data-ttu-id="f7090-173">Krok 5: Vytvoření účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="f7090-173">Step 5: Create an Azure storage account</span></span>
<span data-ttu-id="f7090-174">Pokud nemáte účet úložiště Azure, vytvořte účet povolenou geografickou replikací spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f7090-174">If you don't have an Azure storage account, create a geo-replication enabled account by running hello following command:</span></span>

```

$StorageAccountName = "teststorageacc1"
$StorageAccountGeo  = "Southeast Asia"

New-AzureStorageAccount -StorageAccountName $StorageAccountName -Label $StorageAccountName -Location $StorageAccountGeo;

```

<span data-ttu-id="f7090-175">Všimněte si, že účet úložiště hello musí být ve stejné oblasti jako služba Azure Site Recovery hello hello a přidružen hello stejného předplatného.</span><span class="sxs-lookup"><span data-stu-id="f7090-175">Note that hello storage account must be in hello same region as hello Azure Site Recovery service, and be associated with hello same subscription.</span></span>

## <a name="step-6-install-hello-azure-recovery-services-agent"></a><span data-ttu-id="f7090-176">Krok 6: Instalace hello agenta služeb zotavení Azure</span><span class="sxs-lookup"><span data-stu-id="f7090-176">Step 6: Install hello Azure Recovery Services Agent</span></span>
<span data-ttu-id="f7090-177">Z hello portál Azure, nainstalujte agenta služeb zotavení Azure hello na každém serveru hostitele technologie Hyper-V v cloudech VMM hello chcete tooprotect.</span><span class="sxs-lookup"><span data-stu-id="f7090-177">From hello Azure portal, install hello Azure Recovery Services agent on each Hyper-V host server located in hello VMM clouds you want tooprotect.</span></span>

<span data-ttu-id="f7090-178">Spusťte následující příkaz na všech hostitelích VMM hello:</span><span class="sxs-lookup"><span data-stu-id="f7090-178">Run hello following command on all VMM hosts:</span></span>

```

marsagentinstaller.exe /q /nu

```


## <a name="step-7-configure-cloud-protection-settings"></a><span data-ttu-id="f7090-179">Krok 7: Konfigurace cloudu nastavení ochrany</span><span class="sxs-lookup"><span data-stu-id="f7090-179">Step 7: Configure cloud protection settings</span></span>
1. <span data-ttu-id="f7090-180">Vytvořte tooAzure profilu ochrany cloudu spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f7090-180">Create a cloud protection profile tooAzure by running hello following command:</span></span>

   ```

   $ReplicationFrequencyInSeconds = "300";
   $ProfileResult = New-AzureSiteRecoveryProtectionProfileObject -ReplicationProvider HyperVReplica -RecoveryAzureSubscription $AzureSubscriptionName -RecoveryAzureStorageAccount $StorageAccountName -ReplicationFrequencyInSeconds     $ReplicationFrequencyInSeconds;

   ```
2. <span data-ttu-id="f7090-181">Kontejner ochrany získáte spuštěním hello následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="f7090-181">Get a protection container by running hello following commands:</span></span>

   ```

   $PrimaryCloud = "testcloud"
   $protectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $PrimaryCloud;

   ```
3. <span data-ttu-id="f7090-182">Přidružení hello kontejneru ochrany hello začněte hello cloudu:</span><span class="sxs-lookup"><span data-stu-id="f7090-182">Start hello association of hello protection container with hello cloud:</span></span>

   ```

   $associationJob = Start-AzureSiteRecoveryProtectionProfileAssociationJob -ProtectionProfile $profileResult -PrimaryProtectionContainer $protectionContainer;        

   ```
4. <span data-ttu-id="f7090-183">Po dokončení úlohy hello spusťte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f7090-183">After hello job has finished, run hello following command:</span></span>

        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
        $isJobLeftForProcessing = $true;
        }


1. <span data-ttu-id="f7090-184">Po dokončení zpracování úlohy hello spusťte hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f7090-184">After hello job has finished processing, run hello following command:</span></span>

        Do
        {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

        if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)



<span data-ttu-id="f7090-185">dokončení hello toocheck hello operace, postupujte podle kroků hello v [monitorování aktivity](#monitor).</span><span class="sxs-lookup"><span data-stu-id="f7090-185">toocheck hello completion of hello operation, follow hello steps in [Monitor Activity](#monitor).</span></span>

## <a name="step-8-configure-network-mapping"></a><span data-ttu-id="f7090-186">Krok 8: Nakonfigurování mapování sítě</span><span class="sxs-lookup"><span data-stu-id="f7090-186">Step 8: Configure network mapping</span></span>
<span data-ttu-id="f7090-187">Před zahájením mapování sítě ověřte, zda virtuální počítače na zdrojovém serveru VMM hello jsou připojené tooa síť virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="f7090-187">Before you begin network mapping verify that virtual machines on hello source VMM server are connected tooa VM network.</span></span> <span data-ttu-id="f7090-188">Kromě toho můžete vytvořte jeden nebo více virtuálních sítí Azure.</span><span class="sxs-lookup"><span data-stu-id="f7090-188">In addition, create one or more Azure virtual networks.</span></span> <span data-ttu-id="f7090-189">Všimněte si, že více sítí virtuálních počítačů může být namapované tooa jednu síť Azure.</span><span class="sxs-lookup"><span data-stu-id="f7090-189">Note that multiple VM networks can be mapped tooa single Azure network.</span></span>

<span data-ttu-id="f7090-190">Všimněte si, že pokud hello Cílová síť více podsítí a jedna z těchto podsítí má hello stejný název jako podsíť, na které hello zdrojový virtuální počítač se nachází, pak bude virtuální počítač repliky hello připojené toothat cílové podsíti po převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="f7090-190">Note that if hello target network has multiple subnets and one of those subnets has hello same name as subnet on which hello source virtual machine is located, then hello replica virtual machine will be connected toothat target subnet after failover.</span></span> <span data-ttu-id="f7090-191">Pokud není cílová podsíť s odpovídajícím názvem, hello virtuální počítač bude připojený toohello první podsíť v síti hello.</span><span class="sxs-lookup"><span data-stu-id="f7090-191">If there is not a target subnet with a matching name, hello virtual machine will be connected toohello first subnet in hello network.</span></span>

<span data-ttu-id="f7090-192">První příkaz Hello získá servery pro aktuální trezoru Azure Site Recovery hello.</span><span class="sxs-lookup"><span data-stu-id="f7090-192">hello first command gets servers for hello current Azure Site Recovery vault.</span></span> <span data-ttu-id="f7090-193">příkaz Hello ukládá v proměnné pole hello $Servers hello servery Microsoft Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="f7090-193">hello command stores hello Microsoft Azure Site Recovery servers in hello $Servers array variable.</span></span>

    $Servers = Get-AzureSiteRecoveryServer


<span data-ttu-id="f7090-194">Hello druhém příkazu je získán hello síti pro obnovení lokality pro první server hello hello $Servers pole.</span><span class="sxs-lookup"><span data-stu-id="f7090-194">hello second command gets hello site recovery network for hello first server in hello $Servers array.</span></span> <span data-ttu-id="f7090-195">příkaz Hello ukládá hello sítě hello $Networks proměnné.</span><span class="sxs-lookup"><span data-stu-id="f7090-195">hello command stores hello networks in hello $Networks variable.</span></span>

    $Networks = Get-AzureSiteRecoveryNetwork -Server $Servers[0]

<span data-ttu-id="f7090-196">třetí příkaz Hello získá předplatné Azure pomocí rutiny Get-AzureSubscription hello a pak uloží tuto hodnotu v hello $Subscriptions proměnné.</span><span class="sxs-lookup"><span data-stu-id="f7090-196">hello third command gets your Azure subscriptions by using hello Get-AzureSubscription cmdlet, and then stores that value in hello $Subscriptions variable.</span></span>

    $Subscriptions = Get-AzureSubscription



<span data-ttu-id="f7090-197">příkaz čtvrtý Hello získá virtuálních sítí Azure pomocí rutiny Get-AzureVNetSite hello a potom tuto hodnotu v hello $AzureVmNetworks proměnné.</span><span class="sxs-lookup"><span data-stu-id="f7090-197">hello fourth command gets Azure virtual networks by using hello Get-AzureVNetSite cmdlet, and then that value in hello $AzureVmNetworks variable.</span></span>

    $AzureVmNetworks = Get-AzureVNetSite



<span data-ttu-id="f7090-198">Hello konečné rutina vytvoří mapování mezi primární a sítí virtuálních počítačů Azure hello hello.</span><span class="sxs-lookup"><span data-stu-id="f7090-198">hello final cmdlet creates a mapping between hello primary network and hello Azure virtual machine network.</span></span> <span data-ttu-id="f7090-199">rutiny Hello určuje hello primární síť jako první prvek $Networks hello.</span><span class="sxs-lookup"><span data-stu-id="f7090-199">hello cmdlet specifies hello primary network as hello first element of $Networks.</span></span> <span data-ttu-id="f7090-200">rutiny Hello určuje síť virtuálních počítačů jako první prvek hello $AzureVmNetworks pomocí jeho ID.</span><span class="sxs-lookup"><span data-stu-id="f7090-200">hello cmdlet specifies a virtual machine network as hello first element of $AzureVmNetworks by using its ID.</span></span> <span data-ttu-id="f7090-201">příkaz Hello obsahuje ID vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="f7090-201">hello command includes your Azure subscription ID.</span></span>

    New-AzureSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureSubscriptionId $Subscriptions[0].SubscriptionId -AzureVMNetworkId $AzureVmNetworks[0].Id


## <a name="step-9-enable-protection-for-virtual-machines"></a><span data-ttu-id="f7090-202">Krok 9: Povolení ochrany pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="f7090-202">Step 9: Enable protection for virtual machines</span></span>
<span data-ttu-id="f7090-203">Po serverů, cloudů a sítí jsou správně nakonfigurovaný, můžete povolit ochranu pro virtuální počítače v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="f7090-203">After servers, clouds, and networks are configured correctly, you can enable protection for virtual machines in hello cloud.</span></span> <span data-ttu-id="f7090-204">Vezměte na vědomí následující hello:</span><span class="sxs-lookup"><span data-stu-id="f7090-204">Note hello following:</span></span>

<span data-ttu-id="f7090-205">Virtuální počítače, musí splňovat [požadavky virtuálního počítače Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span><span class="sxs-lookup"><span data-stu-id="f7090-205">Virtual machines must meet [Azure virtual machine prerequisites](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span></span>

<span data-ttu-id="f7090-206">tooenable ochrany hello operační systém a vlastnosti disku operačního systému musí být nastavena pro virtuální počítač hello.</span><span class="sxs-lookup"><span data-stu-id="f7090-206">tooenable protection hello operating system and operating system disk properties must be set for hello virtual machine.</span></span> <span data-ttu-id="f7090-207">Při vytváření virtuálního počítače v nástroji VMM pomocí šablony virtuálního počítače můžete nastavit vlastnost hello.</span><span class="sxs-lookup"><span data-stu-id="f7090-207">When you create a virtual machine in VMM using a virtual machine template you can set hello property.</span></span> <span data-ttu-id="f7090-208">Můžete také nastavit tyto vlastnosti pro existující virtuální počítače v hello **Obecné** a **konfigurace hardwaru** karty hello vlastnosti virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="f7090-208">You can also set these properties for existing virtual machines on hello **General** and **Hardware Configuration** tabs of hello virtual machine properties.</span></span> <span data-ttu-id="f7090-209">Pokud tyto vlastnosti nenastavíte v nástroji VMM budete moct tooconfigure je na portálu Azure Site Recovery hello.</span><span class="sxs-lookup"><span data-stu-id="f7090-209">If you don't set these properties in VMM you'll be able tooconfigure them in hello Azure Site Recovery portal.</span></span>

1. <span data-ttu-id="f7090-210">Ochrana tooenable, spusťte následující příkaz tooget hello ochrany kontejneru hello:</span><span class="sxs-lookup"><span data-stu-id="f7090-210">tooenable protection, run hello following command tooget hello protection container:</span></span>

     <span data-ttu-id="f7090-211">$ProtectionContainer = get-AzureSiteRecoveryProtectionContainer-název $CloudName</span><span class="sxs-lookup"><span data-stu-id="f7090-211">$ProtectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $CloudName</span></span>
2. <span data-ttu-id="f7090-212">Entita ochrany hello (VM) získáte spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f7090-212">Get hello protection entity (VM) by running hello following command:</span></span>

        $protectionEntity = Get-AzureSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer



1. <span data-ttu-id="f7090-213">Povolte hello zotavení po Havárii pro hello virtuálních počítačů tak, že spustíte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="f7090-213">Enable hello DR for hello VM by running hello following command:</span></span>

        $jobResult = Set-AzureSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity     -Protection Enable -Force



## <a name="test-your-deployment"></a><span data-ttu-id="f7090-214">Otestujte nasazení</span><span class="sxs-lookup"><span data-stu-id="f7090-214">Test your deployment</span></span>
<span data-ttu-id="f7090-215">Plánování nasazení můžete spustit testovací převzetí služeb při selhání pro jeden virtuální počítač nebo vytvořit plán obnovení, který se skládá z více virtuálních počítačů a spustit testovací převzetí služeb pro hello tootest.</span><span class="sxs-lookup"><span data-stu-id="f7090-215">tootest your deployment you can run a test failover for a single virtual machine, or create a recovery plan consisting of multiple virtual machines and run a test failover for hello plan.</span></span> <span data-ttu-id="f7090-216">Testovací převzetí služeb při selhání simuluje váš mechanismus převzetí služeb při selhání a zotavení v izolované síti.</span><span class="sxs-lookup"><span data-stu-id="f7090-216">Test failover simulates your failover and recovery mechanism in an isolated network.</span></span> <span data-ttu-id="f7090-217">Poznámky:</span><span class="sxs-lookup"><span data-stu-id="f7090-217">Note that:</span></span>

* <span data-ttu-id="f7090-218">Pokud chcete, aby tooconnect toohello virtuálního počítače v Azure pomocí vzdálené plochy po převzetí služeb při selhání hello, povolte připojení ke vzdálené ploše na virtuálním počítači hello před spuštěním hello testovací převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="f7090-218">If you want tooconnect toohello virtual machine in Azure using Remote Desktop after hello failover, enable Remote Desktop Connection on hello virtual machine before you run hello test failover.</span></span>
* <span data-ttu-id="f7090-219">Po převzetí služeb při selhání budete používat veřejnou IP adresu tooconnect toohello virtuálního počítače v Azure pomocí vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="f7090-219">After failover, you'll use a public IP address tooconnect toohello virtual machine in Azure using Remote Desktop.</span></span> <span data-ttu-id="f7090-220">Pokud chcete, toodo to, ujistěte se, že nemáte žádné zásady domény, které vám zabrání připojování tooa virtuálnímu počítači pomocí veřejné adresy.</span><span class="sxs-lookup"><span data-stu-id="f7090-220">If you want toodo this, ensure you don't have any domain policies that prevent you from connecting tooa virtual machine using a public address.</span></span>

<span data-ttu-id="f7090-221">dokončení hello toocheck hello operace, postupujte podle kroků hello v [monitorování aktivity](#monitor).</span><span class="sxs-lookup"><span data-stu-id="f7090-221">toocheck hello completion of hello operation, follow hello steps in [Monitor Activity](#monitor).</span></span>

### <a name="create-a-recovery-plan"></a><span data-ttu-id="f7090-222">Vytvoření plánu obnovení</span><span class="sxs-lookup"><span data-stu-id="f7090-222">Create a recovery plan</span></span>
1. <span data-ttu-id="f7090-223">Vytvoření souboru XML jako šablona pro váš plán obnovení pomocí hello dat níže a pak ho uložte jako "C:\RPTemplatePath.xml".</span><span class="sxs-lookup"><span data-stu-id="f7090-223">Create an .xml file as a template for your recovery plan using hello data below, and then save it as "C:\RPTemplatePath.xml".</span></span>
2. <span data-ttu-id="f7090-224">Změnit hello RecoveryPlan uzlu Id, názvu, PrimaryServerId a SecondaryServerId.</span><span class="sxs-lookup"><span data-stu-id="f7090-224">Change hello RecoveryPlan node Id, Name, PrimaryServerId, and SecondaryServerId.</span></span>
3. <span data-ttu-id="f7090-225">Změňte hello ProtectionEntity uzlu PrimaryProtectionEntityId (vmid z nástroje VMM).</span><span class="sxs-lookup"><span data-stu-id="f7090-225">Change hello ProtectionEntity node PrimaryProtectionEntityId (vmid from VMM).</span></span>
4. <span data-ttu-id="f7090-226">Přidáním více uzlů ProtectionEntity můžete přidat další virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="f7090-226">You can add more VMs by adding more ProtectionEntity nodes.</span></span>

        <#
        <?xml version="1.0" encoding="utf-16"?>
        <RecoveryPlan Id="d0323b26-5be2-471b-addc-0a8742796610" Name="rp-test"     PrimaryServerId="9350a530-d5af-435b-9f2b-b941b5d9fcd5"     SecondaryServerId="21a9403c-6ec1-44f2-b744-b4e50b792387" Description=""     Version="V2014_07">
          <Actions />
          <ActionGroups>
            <ShutdownAllActionGroup Id="ShutdownAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </ShutdownAllActionGroup>
            <FailoverAllActionGroup Id="FailoverAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </FailoverAllActionGroup>
            <BootActionGroup Id="DefaultActionGroup">
              <PreActionSequence />
              <PostActionSequence />
              <ProtectionEntity PrimaryProtectionEntityId="d4c8ce92-a613-4c63-9b03-    cf163cc36ef8" />
            </BootActionGroup>
          </ActionGroups>
          <ActionGroupSequence>
            <ActionGroup Id="ShutdownAllActionGroup" ActionId="ShutdownAllActionGroup"     Before="FailoverAllActionGroup" />
            <ActionGroup Id="FailoverAllActionGroup" ActionId="FailoverAllActionGroup"     After="ShutdownAllActionGroup" Before="DefaultActionGroup" />
            <ActionGroup Id="DefaultActionGroup" ActionId="DefaultActionGroup" After="FailoverAllActionGroup"/>
          </ActionGroupSequence>
        </RecoveryPlan>
        #>



1. <span data-ttu-id="f7090-227">Vyplňte hello dat v šabloně hello:</span><span class="sxs-lookup"><span data-stu-id="f7090-227">Fill in hello data in hello template:</span></span>

        $TemplatePath = "C:\RPTemplatePath.xml";



1. <span data-ttu-id="f7090-228">Vytvořte hello RecoveryPlan:</span><span class="sxs-lookup"><span data-stu-id="f7090-228">Create hello RecoveryPlan:</span></span>

        $RPCreationJob = New-AzureSiteRecoveryRecoveryPlan -File $TemplatePath -WaitForCompletion;

### <a name="run-a-test-failover"></a><span data-ttu-id="f7090-229">Spuštění testovacího převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="f7090-229">Run a test failover</span></span>
1. <span data-ttu-id="f7090-230">Získejte objekt RecoveryPlan hello spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f7090-230">Get hello RecoveryPlan object by running hello following command:</span></span>

     <span data-ttu-id="f7090-231">$RPObject = get-AzureSiteRecoveryRecoveryPlan-název $RPName;</span><span class="sxs-lookup"><span data-stu-id="f7090-231">$RPObject = Get-AzureSiteRecoveryRecoveryPlan -Name $RPName;</span></span>
2. <span data-ttu-id="f7090-232">Spusťte hello testovací převzetí služeb při selhání spuštěním hello následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f7090-232">Start hello test failover by running hello following command:</span></span>

        $jobIDResult = Start-AzureSiteRecoveryTestFailoverJob -RecoveryPlan $RPObject -Direction PrimaryToRecovery;


## <span data-ttu-id="f7090-233"><a name=monitor></a>Sledování aktivity</span><span class="sxs-lookup"><span data-stu-id="f7090-233"><a name=monitor></a> Monitor Activity</span></span>
<span data-ttu-id="f7090-234">Použijte následující příkazy toomonitor hello aktivity hello.</span><span class="sxs-lookup"><span data-stu-id="f7090-234">Use hello following commands toomonitor hello activity.</span></span> <span data-ttu-id="f7090-235">Všimněte si, že máte toowait mezi úlohy pro zpracování toofinish hello.</span><span class="sxs-lookup"><span data-stu-id="f7090-235">Note that you have toowait in between jobs for hello processing toofinish.</span></span>

    Do
    {
            $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
            Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
            if($job -eq $null -or $job.StateDescription -ne "Completed")
            {
                $isJobLeftForProcessing = $true;
            }

        if($isJobLeftForProcessing)
            {
                Start-Sleep -Seconds 60
            }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a><span data-ttu-id="f7090-236">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f7090-236">Next steps</span></span>
<span data-ttu-id="f7090-237">[Další informace](/powershell/azure/overview) o rutin Powershellu pro Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="f7090-237">[Read more](/powershell/azure/overview) about Azure Site Recovery PowerShell cmdlets.</span></span> <span data-ttu-id="f7090-238"></a>.</span><span class="sxs-lookup"><span data-stu-id="f7090-238"></a>.</span></span>
