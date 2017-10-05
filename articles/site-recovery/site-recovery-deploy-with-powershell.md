---
title: "Replikace virtuálních počítačů Hyper-V do Azure na portálu classic pomocí prostředí PowerShell | Microsoft Docs"
description: "Automatizovat replikaci virtuálních počítačů technologie Hyper-V v cloudech VMM pomocí Site Recovery a prostředí PowerShell na portálu classic"
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
ms.openlocfilehash: 581daaaa5cc0cf8be782f834c6bdb3f27ee413fb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="replicate-hyper-v-vms-to-azure-with-powershell-in-the-classic-portal"></a><span data-ttu-id="becd6-103">Replikace virtuálních počítačů Hyper-V do Azure pomocí prostředí PowerShell na portálu classic</span><span class="sxs-lookup"><span data-stu-id="becd6-103">Replicate Hyper-V VMs to Azure with PowerShell in the classic portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="becd6-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="becd6-104">Azure Portal</span></span>](site-recovery-vmm-to-azure.md)
> * [<span data-ttu-id="becd6-105">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="becd6-105">PowerShell - Resource Manager</span></span>](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [<span data-ttu-id="becd6-106">Portál Classic</span><span class="sxs-lookup"><span data-stu-id="becd6-106">Classic Portal</span></span>](site-recovery-vmm-to-azure-classic.md)
> * [<span data-ttu-id="becd6-107">PowerShell – Classic</span><span class="sxs-lookup"><span data-stu-id="becd6-107">PowerShell - Classic</span></span>](site-recovery-deploy-with-powershell.md)
>
>

## <a name="overview"></a><span data-ttu-id="becd6-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="becd6-108">Overview</span></span>
<span data-ttu-id="becd6-109">Azure Site Recovery přispívá ke strategii obchodní kontinuitu a po havárii obnovení (BCDR) tím, že orchestruje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů v různých scénářích nasazení.</span><span class="sxs-lookup"><span data-stu-id="becd6-109">Azure Site Recovery contributes to your business continuity and disaster recovery (BCDR) strategy by orchestrating replication, failover and recovery of virtual machines in a number of deployment scenarios.</span></span> <span data-ttu-id="becd6-110">Úplný seznam nasazení scénářů najdete v článku [přehled Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="becd6-110">For a full list of deployment scenarios see the [Azure Site Recovery overview](site-recovery-overview.md).</span></span>

<span data-ttu-id="becd6-111">Tento článek ukazuje, jak pomocí prostředí PowerShell pro automatizaci běžných úkolů, které je třeba provést při nastavení Azure Site Recovery replikovat virtuální počítače Hyper-V v cloudech System Center VMM do úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="becd6-111">This article shows you how to use PowerShell to automate common tasks you need to perform when you set up Azure Site Recovery to replicate Hyper-V virtual machines in System Center VMM clouds to Azure storage.</span></span>

<span data-ttu-id="becd6-112">Článek obsahuje předpoklady pro scénář a ukazuje, jak nastavit trezor Site Recovery, nainstalujte zprostředkovatele Azure Site Recovery na zdrojovém serveru VMM, zaregistrujte server v trezoru, přidat účet úložiště Azure, nainstalujte Azure Recovery Agent služeb na hostitelských serverech technologie Hyper-V, nakonfigurovat nastavení ochrany pro cloudy VMM, které se použijí na všechny chráněné virtuální počítače a pak povolte ochranu pro tyto virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="becd6-112">The article includes prerequisites for the scenario, and shows you how to set up a Site Recovery vault, install the Azure Site Recovery Provider on the source VMM server, register the server in the vault, add an Azure storage account, install the Azure Recovery Services agent on Hyper-V host servers, configure protection settings for VMM clouds that will be applied to all protected virtual machines, and then enable protection for those virtual machines.</span></span> <span data-ttu-id="becd6-113">Nakonec otestujete převzetí služeb při selhání, abyste měli jistotu, že všechno funguje podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="becd6-113">Finish up by testing the failover to make sure everything's working as expected.</span></span>

<span data-ttu-id="becd6-114">Pokud narazíte na problémy nastavení tento scénář, zveřejněte svoje otázky na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="becd6-114">If you run into problems setting up this scenario, post your questions on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

> [!NOTE]
> <span data-ttu-id="becd6-115">Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="becd6-115">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="becd6-116">Tento článek se zabývá pomocí modelu nasazení Classic.</span><span class="sxs-lookup"><span data-stu-id="becd6-116">This article covers using the Classic deployment model.</span></span>
>
>

## <a name="before-you-start"></a><span data-ttu-id="becd6-117">Než začnete</span><span class="sxs-lookup"><span data-stu-id="becd6-117">Before you start</span></span>
<span data-ttu-id="becd6-118">Ujistěte se, že máte zavedenou tyto požadavky:</span><span class="sxs-lookup"><span data-stu-id="becd6-118">Make sure you have these prerequisites in place:</span></span>

### <a name="azure-prerequisites"></a><span data-ttu-id="becd6-119">Požadavky Azure</span><span class="sxs-lookup"><span data-stu-id="becd6-119">Azure prerequisites</span></span>
* <span data-ttu-id="becd6-120">Budete potřebovat účet [Microsoft Azure](https://azure.microsoft.com/).</span><span class="sxs-lookup"><span data-stu-id="becd6-120">You'll need a [Microsoft Azure](https://azure.microsoft.com/) account.</span></span> <span data-ttu-id="becd6-121">Můžete začít s [bezplatnou zkušební verzí](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="becd6-121">You can start with a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="becd6-122">K ukládání replikovaných dat budete potřebovat účet úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="becd6-122">You'll need an Azure storage account to store replicated data.</span></span> <span data-ttu-id="becd6-123">Účet musí povolenou geografickou replikací.</span><span class="sxs-lookup"><span data-stu-id="becd6-123">The account needs geo-replication enabled.</span></span> <span data-ttu-id="becd6-124">Měl by být ve stejné oblasti jako trezor Azure Site Recovery a být přidružený ke stejnému předplatnému.</span><span class="sxs-lookup"><span data-stu-id="becd6-124">It should be in the same region as the Azure Site Recovery vault and be associated with the same subscription.</span></span> <span data-ttu-id="becd6-125">[Další informace o Azure storage](../storage/common/storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="becd6-125">[Learn more about Azure storage](../storage/common/storage-introduction.md).</span></span>
* <span data-ttu-id="becd6-126">Budete potřebovat, abyste měli jistotu, že virtuální počítače, které chcete chránit vyhovují [požadavky virtuálního počítače Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span><span class="sxs-lookup"><span data-stu-id="becd6-126">You'll need to make sure that virtual machines you want to protect comply with [Azure virtual machine prerequisites](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span></span>

### <a name="vmm-prerequisites"></a><span data-ttu-id="becd6-127">Požadavky VMM</span><span class="sxs-lookup"><span data-stu-id="becd6-127">VMM prerequisites</span></span>
* <span data-ttu-id="becd6-128">Budete potřebovat VMM server běžící na System Center 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="becd6-128">You'll need  VMM server running on System Center 2012 R2.</span></span>
* <span data-ttu-id="becd6-129">Budete potřebovat alespoň jeden cloud na serveru VMM, který chcete chránit.</span><span class="sxs-lookup"><span data-stu-id="becd6-129">You'll need at least one cloud on the VMM server you want to protect.</span></span> <span data-ttu-id="becd6-130">Cloud by měl obsahovat:</span><span class="sxs-lookup"><span data-stu-id="becd6-130">The cloud should contain:</span></span>
  * <span data-ttu-id="becd6-131">Minimálně jednu skupinu hostitelů VMM.</span><span class="sxs-lookup"><span data-stu-id="becd6-131">One or more VMM host groups.</span></span>
  * <span data-ttu-id="becd6-132">Jeden nebo více hostitelských serverů Hyper-V nebo clusterů v každé skupině hostitelů.</span><span class="sxs-lookup"><span data-stu-id="becd6-132">One or more Hyper-V host servers or clusters in each host group .</span></span>
  * <span data-ttu-id="becd6-133">Jeden nebo více virtuálních počítačů na zdrojovém technologie Hyper-V serveru.</span><span class="sxs-lookup"><span data-stu-id="becd6-133">One or more virtual machines on the source Hyper-V server.</span></span>

### <a name="hyper-v-prerequisites"></a><span data-ttu-id="becd6-134">Požadavky technologie Hyper-V</span><span class="sxs-lookup"><span data-stu-id="becd6-134">Hyper-V prerequisites</span></span>
* <span data-ttu-id="becd6-135">Servery hostitele technologie Hyper-V musí používat alespoň **systému Windows Server 2012** s rolí Hyper-V nebo **Microsoft Hyper-V Server 2012** a mít nainstalované nejnovější aktualizace.</span><span class="sxs-lookup"><span data-stu-id="becd6-135">The host Hyper-V servers must be running at least **Windows Server 2012** with Hyper-V role or **Microsoft Hyper-V Server 2012** and have the latest updates installed.</span></span>
* <span data-ttu-id="becd6-136">Pokud používáte technologii Hyper-V v clusteru, je důležité vědět, že zprostředkovatel clusteru se nevytvoří automaticky, pokud máte clustery založené na statických IP adresách.</span><span class="sxs-lookup"><span data-stu-id="becd6-136">If you're running Hyper-V in a cluster note that cluster broker isn't created automatically if you have a static IP address-based cluster.</span></span> <span data-ttu-id="becd6-137">Budete ho muset nakonfigurovat ručně.</span><span class="sxs-lookup"><span data-stu-id="becd6-137">You'll need to configure the cluster broker manually.</span></span> <span data-ttu-id="becd6-138">Chcete-li to provést ve Správci serveru > Správce clusteru převzetí služeb při selhání, připojte se ke clusteru, klikněte na tlačítko **konfigurovat roli** a vyberte **zprostředkovatele replik technologie Hyper-V** v **vybrat roli** Obrazovka Průvodce vysokou dostupností.</span><span class="sxs-lookup"><span data-stu-id="becd6-138">To do this, in Server Manager > Failover Cluster Manager, connect to the cluster, click **Configure Role** and select **Hyper-V Replica Broker** in the **Select Role** screen of the High Availability wizard.</span></span>
* <span data-ttu-id="becd6-139">Libovolný server hostitele technologie Hyper-V nebo clusteru, pro který chcete spravovat ochranu musí být součástí cloudu VMM.</span><span class="sxs-lookup"><span data-stu-id="becd6-139">Any Hyper-V host server or cluster for which you want to manage protection must be included in a VMM cloud.</span></span>

### <a name="network-mapping-prerequisites"></a><span data-ttu-id="becd6-140">Požadavky na mapování sítě</span><span class="sxs-lookup"><span data-stu-id="becd6-140">Network mapping prerequisites</span></span>
<span data-ttu-id="becd6-141">Při ochraně virtuálních počítačů v síti Azure mapování mapuje sítě virtuálních počítačů na zdrojovém serveru VMM a cílové sítě Azure, aby bylo možné následující:</span><span class="sxs-lookup"><span data-stu-id="becd6-141">When you protect virtual machines in Azure network mapping maps between VM networks on the source VMM server and target Azure networks to enable the following:</span></span>

* <span data-ttu-id="becd6-142">Všechny počítače, které převzetí služeb při selhání ve stejné síti může připojit k sobě navzájem, bez ohledu na plánu obnovení jsou v.</span><span class="sxs-lookup"><span data-stu-id="becd6-142">All machines which fail over on the same network can connect to each other, irrespective of which recovery plan they are in.</span></span>
* <span data-ttu-id="becd6-143">Pokud je v cílové síti Azure nastavená brána sítě, virtuální počítače se budou moct připojit k jiným místním virtuálním počítačům.</span><span class="sxs-lookup"><span data-stu-id="becd6-143">If a network gateway is setup on the target Azure network, virtual machines can connect to other on-premises virtual machines.</span></span>
* <span data-ttu-id="becd6-144">Pokud nenakonfigurujete mapování sítě, budou se moct po předání služeb při selhání do Azure připojit k sobě navzájem pouze virtuální počítače, u kterých dojde k převzetí služeb při selhání v rámci stejného plánu obnovení.</span><span class="sxs-lookup"><span data-stu-id="becd6-144">If you don’t configure network mapping only virtual machines that fail over in the same recovery plan will be able to connect to each other after failover to Azure.</span></span>

<span data-ttu-id="becd6-145">Pokud chcete nasadit mapování sítě, budete potřebovat následující:</span><span class="sxs-lookup"><span data-stu-id="becd6-145">If you want to deploy network mapping you'll need the following:</span></span>

* <span data-ttu-id="becd6-146">Virtuální počítače, který chcete chránit na zdrojovém serveru VMM, musí být připojené k síti virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="becd6-146">The virtual machines you want to protect on the source VMM server should be connected to a VM network.</span></span> <span data-ttu-id="becd6-147">Tato síť musí být propojená na logickou síť, která je přidružená ke cloudu.</span><span class="sxs-lookup"><span data-stu-id="becd6-147">That network should be linked to a logical network that is associated with the cloud.</span></span>
* <span data-ttu-id="becd6-148">Síť Azure, ke které se budou moct po převzetí služeb při selhání připojit replikované virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="becd6-148">An Azure network to which replicated virtual machines can connect after failover.</span></span> <span data-ttu-id="becd6-149">Tuto síť vyberete v okamžiku převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="becd6-149">You'll select this network at the time of failover.</span></span> <span data-ttu-id="becd6-150">Síť musí být ve stejné oblasti jako vaše předplatné Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="becd6-150">The network should be in the same region as your Azure Site Recovery subscription.</span></span>

### <a name="powershell-prerequisites"></a><span data-ttu-id="becd6-151">Požadavky prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="becd6-151">PowerShell prerequisites</span></span>
<span data-ttu-id="becd6-152">Ujistěte se, že máte Azure PowerShell připravené na vynucování.</span><span class="sxs-lookup"><span data-stu-id="becd6-152">Make sure you have Azure PowerShell ready to go.</span></span> <span data-ttu-id="becd6-153">Pokud už používáte prostředí PowerShell, budete muset upgradovat na verzi 0.8.10 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="becd6-153">If you are already using PowerShell, you'll need to upgrade to version 0.8.10 or later.</span></span> <span data-ttu-id="becd6-154">Informace o nastavení prostředí PowerShell najdete v tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="becd6-154">For information about setting up PowerShell, see [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="becd6-155">Po nastavení a nakonfigurovat prostředí PowerShell, můžete zobrazit všechny dostupné rutiny služby [zde](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="becd6-155">Once you have set up and configured PowerShell, you can view all of the available cmdlets for the service [here](/powershell/azure/overview).</span></span>

<span data-ttu-id="becd6-156">Další informace o tipy, které mohou pomoci pomocí rutin, jako je například jak hodnoty parametrů, vstupy a výstupy jsou obvykle zpracovávány v prostředí Azure PowerShell najdete v části [Začínáme s Azure rutiny](/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="becd6-156">To learn about tips that can help you use the cmdlets, such as how parameter values, inputs, and outputs are typically handled in Azure PowerShell, see [Get Started with Azure Cmdlets](/powershell/azure/get-started-azureps).</span></span>

## <a name="step-1-set-the-subscription"></a><span data-ttu-id="becd6-157">Krok 1: Nastavte předplatné</span><span class="sxs-lookup"><span data-stu-id="becd6-157">Step 1: Set the subscription</span></span>
<span data-ttu-id="becd6-158">V prostředí PowerShell spusťte tyto rutiny:</span><span class="sxs-lookup"><span data-stu-id="becd6-158">In PowerShell, run these cmdlets:</span></span>

```
$UserName = "<user@live.com>"
$Password = "<password>"
$AzureSubscriptionName = "prod_sub1"

$SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $securePassword
Add-AzureAccount -Credential $Cred;
$AzureSubscription = Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

```

<span data-ttu-id="becd6-159">Prvky v rámci "< >" nahraďte konkrétní informace.</span><span class="sxs-lookup"><span data-stu-id="becd6-159">Replace the elements within the "< >" with your specific information.</span></span>

## <a name="step-2-create-a-site-recovery-vault"></a><span data-ttu-id="becd6-160">Krok 2: Vytvoření trezoru Site Recovery</span><span class="sxs-lookup"><span data-stu-id="becd6-160">Step 2: Create a Site Recovery vault</span></span>
<span data-ttu-id="becd6-161">V prostředí PowerShell nahraďte elementy v rámci "< >" konkrétní informace a spusťte tyto příkazy:</span><span class="sxs-lookup"><span data-stu-id="becd6-161">In PowerShell, replace the elements within the "< >" with your specific information, and run these commands:</span></span>

```

$VaultName = "<testvault123>"
$VaultGeo  = "<Southeast Asia>"
$OutputPathForSettingsFile = "<c:\>"

```


```
New-AzureSiteRecoveryVault -Location $VaultGeo -Name $VaultName;
$vault = Get-AzureSiteRecoveryVault -Name $VaultName;

```

## <a name="step-3-generate-a-vault-registration-key"></a><span data-ttu-id="becd6-162">Krok 3: Vygenerování registračního klíče trezoru</span><span class="sxs-lookup"><span data-stu-id="becd6-162">Step 3: Generate a vault registration key</span></span>
<span data-ttu-id="becd6-163">Vygenerujte registrační klíč v trezoru.</span><span class="sxs-lookup"><span data-stu-id="becd6-163">Generate a registration key in the vault.</span></span> <span data-ttu-id="becd6-164">Po tom, co si stáhnete zprostředkovatele Azure Site Recovery a nainstalujete ho na server VMM, použijete tento klíč k registraci serveru VMM v trezoru.</span><span class="sxs-lookup"><span data-stu-id="becd6-164">After you download the Azure Site Recovery Provider and install it on the VMM server, you'll use this key to register the VMM server in the vault.</span></span>

1. <span data-ttu-id="becd6-165">Získat v souboru nastavení trezoru a nastavit kontext:</span><span class="sxs-lookup"><span data-stu-id="becd6-165">Get the vault setting file and set the context:</span></span>

   ```

   $VaultName = "<testvault123>"
   $VaultGeo  = "<Southeast Asia>"
   $OutputPathForSettingsFile = "<c:\>"

   $VaultSetingsFile = Get-AzureSiteRecoveryVaultSettingsFile -Location $VaultGeo -Name $VaultName -Path $OutputPathForSettingsFile;

   ```
2. <span data-ttu-id="becd6-166">Nastavte kontext trezoru spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="becd6-166">Set the vault context by running the following commands:</span></span>

   ```

   $VaultSettingFilePath = $vaultSetingsFile.FilePath
   $VaultContext = Import-AzureSiteRecoveryVaultSettingsFile -Path $VaultSettingFilePath -ErrorAction Stop

   ```

## <a name="step-4-install-the-azure-site-recovery-provider"></a><span data-ttu-id="becd6-167">Krok 4: Instalace nástroje Azure Site Recovery Provider</span><span class="sxs-lookup"><span data-stu-id="becd6-167">Step 4: Install the Azure Site Recovery Provider</span></span>
1. <span data-ttu-id="becd6-168">Na počítači VMM vytvořte adresář tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="becd6-168">On the VMM machine, create a directory by running the following command:</span></span>

   ```

   pushd C:\ASR\

   ```
2. <span data-ttu-id="becd6-169">Extrahujte soubory pomocí stažené poskytovatele tak, že spustíte následující příkaz</span><span class="sxs-lookup"><span data-stu-id="becd6-169">Extract the files using the downloaded provider by running the following command</span></span>

   ```

   AzureSiteRecoveryProvider.exe /x:. /q

   ```
3. <span data-ttu-id="becd6-170">Nainstalujte poskytovatele pomocí následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="becd6-170">Install the provider using the following commands:</span></span>

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

   <span data-ttu-id="becd6-171">Počkejte na dokončení instalace.</span><span class="sxs-lookup"><span data-stu-id="becd6-171">Wait for the installation to finish.</span></span>
4. <span data-ttu-id="becd6-172">Zaregistrujte server v trezoru, pomocí následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="becd6-172">Register the server in the vault using the following command:</span></span>

   ```

   $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
   pushd $BinPath
   $encryptionFilePath = "C:\temp\"
   .\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

   ```

## <a name="step-5-create-an-azure-storage-account"></a><span data-ttu-id="becd6-173">Krok 5: Vytvoření účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="becd6-173">Step 5: Create an Azure storage account</span></span>
<span data-ttu-id="becd6-174">Pokud nemáte účet úložiště Azure, vytvořte účet povolenou geografickou replikací spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="becd6-174">If you don't have an Azure storage account, create a geo-replication enabled account by running the following command:</span></span>

```

$StorageAccountName = "teststorageacc1"
$StorageAccountGeo  = "Southeast Asia"

New-AzureStorageAccount -StorageAccountName $StorageAccountName -Label $StorageAccountName -Location $StorageAccountGeo;

```

<span data-ttu-id="becd6-175">Všimněte si, že účet úložiště musí být ve stejné oblasti jako služba Azure Site Recovery a být přidružený ke stejnému předplatnému.</span><span class="sxs-lookup"><span data-stu-id="becd6-175">Note that the storage account must be in the same region as the Azure Site Recovery service, and be associated with the same subscription.</span></span>

## <a name="step-6-install-the-azure-recovery-services-agent"></a><span data-ttu-id="becd6-176">Krok 6: Instalace agenta služeb zotavení Azure</span><span class="sxs-lookup"><span data-stu-id="becd6-176">Step 6: Install the Azure Recovery Services Agent</span></span>
<span data-ttu-id="becd6-177">Z portálu Azure nainstalujte agenta služeb zotavení Azure na každém serveru hostitele technologie Hyper-V v cloudech VMM, který chcete chránit.</span><span class="sxs-lookup"><span data-stu-id="becd6-177">From the Azure portal, install the Azure Recovery Services agent on each Hyper-V host server located in the VMM clouds you want to protect.</span></span>

<span data-ttu-id="becd6-178">Na všech hostitelích VMM, spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="becd6-178">Run the following command on all VMM hosts:</span></span>

```

marsagentinstaller.exe /q /nu

```


## <a name="step-7-configure-cloud-protection-settings"></a><span data-ttu-id="becd6-179">Krok 7: Konfigurace cloudu nastavení ochrany</span><span class="sxs-lookup"><span data-stu-id="becd6-179">Step 7: Configure cloud protection settings</span></span>
1. <span data-ttu-id="becd6-180">Vytvoření profilu ochrany cloudu do Azure tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="becd6-180">Create a cloud protection profile to Azure by running the following command:</span></span>

   ```

   $ReplicationFrequencyInSeconds = "300";
   $ProfileResult = New-AzureSiteRecoveryProtectionProfileObject -ReplicationProvider HyperVReplica -RecoveryAzureSubscription $AzureSubscriptionName -RecoveryAzureStorageAccount $StorageAccountName -ReplicationFrequencyInSeconds     $ReplicationFrequencyInSeconds;

   ```
2. <span data-ttu-id="becd6-181">Kontejner ochrany získáte spuštěním následujících příkazů:</span><span class="sxs-lookup"><span data-stu-id="becd6-181">Get a protection container by running the following commands:</span></span>

   ```

   $PrimaryCloud = "testcloud"
   $protectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $PrimaryCloud;

   ```
3. <span data-ttu-id="becd6-182">Přidružení kontejner ochrany začněte cloudu:</span><span class="sxs-lookup"><span data-stu-id="becd6-182">Start the association of the protection container with the cloud:</span></span>

   ```

   $associationJob = Start-AzureSiteRecoveryProtectionProfileAssociationJob -ProtectionProfile $profileResult -PrimaryProtectionContainer $protectionContainer;        

   ```
4. <span data-ttu-id="becd6-183">Po dokončení úlohy spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="becd6-183">After the job has finished, run the following command:</span></span>

        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
        $isJobLeftForProcessing = $true;
        }


1. <span data-ttu-id="becd6-184">Po dokončení zpracování úlohy spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="becd6-184">After the job has finished processing, run the following command:</span></span>

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



<span data-ttu-id="becd6-185">Pokud chcete zkontrolovat na dokončení operace, postupujte podle kroků v [monitorování aktivity](#monitor).</span><span class="sxs-lookup"><span data-stu-id="becd6-185">To check the completion of the operation, follow the steps in [Monitor Activity](#monitor).</span></span>

## <a name="step-8-configure-network-mapping"></a><span data-ttu-id="becd6-186">Krok 8: Nakonfigurování mapování sítě</span><span class="sxs-lookup"><span data-stu-id="becd6-186">Step 8: Configure network mapping</span></span>
<span data-ttu-id="becd6-187">Před zahájením mapování sítě ověřte, jestli jsou virtuální počítače na zdrojovém serveru VMM připojené k síti virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="becd6-187">Before you begin network mapping verify that virtual machines on the source VMM server are connected to a VM network.</span></span> <span data-ttu-id="becd6-188">Kromě toho můžete vytvořte jeden nebo více virtuálních sítí Azure.</span><span class="sxs-lookup"><span data-stu-id="becd6-188">In addition, create one or more Azure virtual networks.</span></span> <span data-ttu-id="becd6-189">Na jednu síť Azure je možné namapovat více sítí virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="becd6-189">Note that multiple VM networks can be mapped to a single Azure network.</span></span>

<span data-ttu-id="becd6-190">Pamatujte, že pokud má cílová síť více podsítí a jedna z těchto podsítí má stejný název jako podsíť, ve které se nachází zdrojový virtuální počítač, bude virtuální počítač repliky po převzetí služeb při selhání připojen k této cílové podsíti.</span><span class="sxs-lookup"><span data-stu-id="becd6-190">Note that if the target network has multiple subnets and one of those subnets has the same name as subnet on which the source virtual machine is located, then the replica virtual machine will be connected to that target subnet after failover.</span></span> <span data-ttu-id="becd6-191">Pokud není cílová podsíť s odpovídajícím názvem, virtuální počítač se připojí k první podsíti v síti.</span><span class="sxs-lookup"><span data-stu-id="becd6-191">If there is not a target subnet with a matching name, the virtual machine will be connected to the first subnet in the network.</span></span>

<span data-ttu-id="becd6-192">První příkaz získá servery pro aktuální trezoru Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="becd6-192">The first command gets servers for the current Azure Site Recovery vault.</span></span> <span data-ttu-id="becd6-193">Příkaz ukládá v proměnné pole $Servers servery Microsoft Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="becd6-193">The command stores the Microsoft Azure Site Recovery servers in the $Servers array variable.</span></span>

    $Servers = Get-AzureSiteRecoveryServer


<span data-ttu-id="becd6-194">V druhém příkazu získá síť site recovery pro první server v poli $Servers.</span><span class="sxs-lookup"><span data-stu-id="becd6-194">The second command gets the site recovery network for the first server in the $Servers array.</span></span> <span data-ttu-id="becd6-195">Příkaz ukládá v proměnné $Networks sítí.</span><span class="sxs-lookup"><span data-stu-id="becd6-195">The command stores the networks in the $Networks variable.</span></span>

    $Networks = Get-AzureSiteRecoveryNetwork -Server $Servers[0]

<span data-ttu-id="becd6-196">V třetím příkazu získá předplatné Azure pomocí rutiny Get-AzureSubscription a uloží tuto hodnotu v proměnné $Subscriptions.</span><span class="sxs-lookup"><span data-stu-id="becd6-196">The third command gets your Azure subscriptions by using the Get-AzureSubscription cmdlet, and then stores that value in the $Subscriptions variable.</span></span>

    $Subscriptions = Get-AzureSubscription



<span data-ttu-id="becd6-197">Příkaz čtvrtý získá pomocí rutiny Get-AzureVNetSite a potom tuto hodnotu v proměnné $AzureVmNetworks virtuálních sítí Azure.</span><span class="sxs-lookup"><span data-stu-id="becd6-197">The fourth command gets Azure virtual networks by using the Get-AzureVNetSite cmdlet, and then that value in the $AzureVmNetworks variable.</span></span>

    $AzureVmNetworks = Get-AzureVNetSite



<span data-ttu-id="becd6-198">Poslední rutina vytvoří mapování mezi primární síť a síť virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="becd6-198">The final cmdlet creates a mapping between the primary network and the Azure virtual machine network.</span></span> <span data-ttu-id="becd6-199">Rutina určuje primární síť jako první prvek $Networks.</span><span class="sxs-lookup"><span data-stu-id="becd6-199">The cmdlet specifies the primary network as the first element of $Networks.</span></span> <span data-ttu-id="becd6-200">Rutina určuje síť virtuálních počítačů jako první prvek $AzureVmNetworks pomocí jeho ID.</span><span class="sxs-lookup"><span data-stu-id="becd6-200">The cmdlet specifies a virtual machine network as the first element of $AzureVmNetworks by using its ID.</span></span> <span data-ttu-id="becd6-201">Příkaz obsahuje ID vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="becd6-201">The command includes your Azure subscription ID.</span></span>

    New-AzureSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureSubscriptionId $Subscriptions[0].SubscriptionId -AzureVMNetworkId $AzureVmNetworks[0].Id


## <a name="step-9-enable-protection-for-virtual-machines"></a><span data-ttu-id="becd6-202">Krok 9: Povolení ochrany pro virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="becd6-202">Step 9: Enable protection for virtual machines</span></span>
<span data-ttu-id="becd6-203">Po správném nakonfigurování serverů, cloudů a sítí můžete povolit ochranu pro virtuální počítače v cloudu.</span><span class="sxs-lookup"><span data-stu-id="becd6-203">After servers, clouds, and networks are configured correctly, you can enable protection for virtual machines in the cloud.</span></span> <span data-ttu-id="becd6-204">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="becd6-204">Note the following:</span></span>

<span data-ttu-id="becd6-205">Virtuální počítače, musí splňovat [požadavky virtuálního počítače Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span><span class="sxs-lookup"><span data-stu-id="becd6-205">Virtual machines must meet [Azure virtual machine prerequisites](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span></span>

<span data-ttu-id="becd6-206">Aby bylo možné povolit ochranu, musí být pro virtuální počítač nastavené vlastnosti operačního systému a disku operačního systému.</span><span class="sxs-lookup"><span data-stu-id="becd6-206">To enable protection the operating system and operating system disk properties must be set for the virtual machine.</span></span> <span data-ttu-id="becd6-207">Vlastnost můžete nastavit při vytváření virtuálního počítače v nástroji VMM pomocí šablony virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="becd6-207">When you create a virtual machine in VMM using a virtual machine template you can set the property.</span></span> <span data-ttu-id="becd6-208">Tyto vlastnosti také můžete nastavit pro existující virtuální počítače, a to na kartách **Obecné** a **Konfigurace hardwaru** ve vlastnostech virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="becd6-208">You can also set these properties for existing virtual machines on the **General** and **Hardware Configuration** tabs of the virtual machine properties.</span></span> <span data-ttu-id="becd6-209">Pokud tyto vlastnosti nenastavíte v nástroji VMM, budete je moct nakonfigurovat na portálu Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="becd6-209">If you don't set these properties in VMM you'll be able to configure them in the Azure Site Recovery portal.</span></span>

1. <span data-ttu-id="becd6-210">Pokud chcete povolit ochranu, spusťte následující příkaz k získání kontejneru ochrany:</span><span class="sxs-lookup"><span data-stu-id="becd6-210">To enable protection, run the following command to get the protection container:</span></span>

     <span data-ttu-id="becd6-211">$ProtectionContainer = get-AzureSiteRecoveryProtectionContainer-název $CloudName</span><span class="sxs-lookup"><span data-stu-id="becd6-211">$ProtectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $CloudName</span></span>
2. <span data-ttu-id="becd6-212">Získáte entita ochrany (VM) tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="becd6-212">Get the protection entity (VM) by running the following command:</span></span>

        $protectionEntity = Get-AzureSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer



1. <span data-ttu-id="becd6-213">Povolte DR pro virtuální počítač tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="becd6-213">Enable the DR for the VM by running the following command:</span></span>

        $jobResult = Set-AzureSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity     -Protection Enable -Force



## <a name="test-your-deployment"></a><span data-ttu-id="becd6-214">Otestujte nasazení</span><span class="sxs-lookup"><span data-stu-id="becd6-214">Test your deployment</span></span>
<span data-ttu-id="becd6-215">Chcete nasazení otestovat můžete spustit testovací převzetí služeb při selhání pro jeden virtuální počítač, nebo vytvořit plán obnovení, který se skládá z více virtuálních počítačů a spustit testovací převzetí služeb pro plán.</span><span class="sxs-lookup"><span data-stu-id="becd6-215">To test your deployment you can run a test failover for a single virtual machine, or create a recovery plan consisting of multiple virtual machines and run a test failover for the plan.</span></span> <span data-ttu-id="becd6-216">Testovací převzetí služeb při selhání simuluje váš mechanismus převzetí služeb při selhání a zotavení v izolované síti.</span><span class="sxs-lookup"><span data-stu-id="becd6-216">Test failover simulates your failover and recovery mechanism in an isolated network.</span></span> <span data-ttu-id="becd6-217">Poznámky:</span><span class="sxs-lookup"><span data-stu-id="becd6-217">Note that:</span></span>

* <span data-ttu-id="becd6-218">Pokud se chcete připojit k virtuálnímu počítači v Azure pomocí Vzdálené plochy po převzetí služeb po selhání, pak před spuštěním testu převzetí služeb povolte na virtuálním počítači připojení ke Vzdálené ploše.</span><span class="sxs-lookup"><span data-stu-id="becd6-218">If you want to connect to the virtual machine in Azure using Remote Desktop after the failover, enable Remote Desktop Connection on the virtual machine before you run the test failover.</span></span>
* <span data-ttu-id="becd6-219">Po převzetí služeb při selhání budete používat veřejnou IP adresu pro připojení k virtuálnímu počítači v Azure pomocí vzdálené plochy.</span><span class="sxs-lookup"><span data-stu-id="becd6-219">After failover, you'll use a public IP address to connect to the virtual machine in Azure using Remote Desktop.</span></span> <span data-ttu-id="becd6-220">Pokud to chcete provést, zkontrolujte, že nemáte žádné zásady domény, které by vám bránily v připojení k virtuálnímu počítači pomocí veřejné adresy.</span><span class="sxs-lookup"><span data-stu-id="becd6-220">If you want to do this, ensure you don't have any domain policies that prevent you from connecting to a virtual machine using a public address.</span></span>

<span data-ttu-id="becd6-221">Pokud chcete zkontrolovat na dokončení operace, postupujte podle kroků v [monitorování aktivity](#monitor).</span><span class="sxs-lookup"><span data-stu-id="becd6-221">To check the completion of the operation, follow the steps in [Monitor Activity](#monitor).</span></span>

### <a name="create-a-recovery-plan"></a><span data-ttu-id="becd6-222">Vytvoření plánu obnovení</span><span class="sxs-lookup"><span data-stu-id="becd6-222">Create a recovery plan</span></span>
1. <span data-ttu-id="becd6-223">Vytvoření souboru XML jako šablona pro váš plán obnovení pomocí níže uvedené údaje a pak ho uložte jako "C:\RPTemplatePath.xml".</span><span class="sxs-lookup"><span data-stu-id="becd6-223">Create an .xml file as a template for your recovery plan using the data below, and then save it as "C:\RPTemplatePath.xml".</span></span>
2. <span data-ttu-id="becd6-224">Změňte RecoveryPlan uzlu Id, název, PrimaryServerId a SecondaryServerId.</span><span class="sxs-lookup"><span data-stu-id="becd6-224">Change the RecoveryPlan node Id, Name, PrimaryServerId, and SecondaryServerId.</span></span>
3. <span data-ttu-id="becd6-225">Změňte uzlu ProtectionEntity PrimaryProtectionEntityId (vmid z nástroje VMM).</span><span class="sxs-lookup"><span data-stu-id="becd6-225">Change the ProtectionEntity node PrimaryProtectionEntityId (vmid from VMM).</span></span>
4. <span data-ttu-id="becd6-226">Přidáním více uzlů ProtectionEntity můžete přidat další virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="becd6-226">You can add more VMs by adding more ProtectionEntity nodes.</span></span>

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



1. <span data-ttu-id="becd6-227">Vyplňte dat v šabloně:</span><span class="sxs-lookup"><span data-stu-id="becd6-227">Fill in the data in the template:</span></span>

        $TemplatePath = "C:\RPTemplatePath.xml";



1. <span data-ttu-id="becd6-228">Vytvořte RecoveryPlan:</span><span class="sxs-lookup"><span data-stu-id="becd6-228">Create the RecoveryPlan:</span></span>

        $RPCreationJob = New-AzureSiteRecoveryRecoveryPlan -File $TemplatePath -WaitForCompletion;

### <a name="run-a-test-failover"></a><span data-ttu-id="becd6-229">Spuštění testovacího převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="becd6-229">Run a test failover</span></span>
1. <span data-ttu-id="becd6-230">Získání objektu RecoveryPlan spuštěním následujícího příkazu:</span><span class="sxs-lookup"><span data-stu-id="becd6-230">Get the RecoveryPlan object by running the following command:</span></span>

     <span data-ttu-id="becd6-231">$RPObject = get-AzureSiteRecoveryRecoveryPlan-název $RPName;</span><span class="sxs-lookup"><span data-stu-id="becd6-231">$RPObject = Get-AzureSiteRecoveryRecoveryPlan -Name $RPName;</span></span>
2. <span data-ttu-id="becd6-232">Spusťte testovací převzetí služeb tak, že spustíte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="becd6-232">Start the test failover by running the following command:</span></span>

        $jobIDResult = Start-AzureSiteRecoveryTestFailoverJob -RecoveryPlan $RPObject -Direction PrimaryToRecovery;


## <span data-ttu-id="becd6-233"><a name=monitor></a>Sledování aktivity</span><span class="sxs-lookup"><span data-stu-id="becd6-233"><a name=monitor></a> Monitor Activity</span></span>
<span data-ttu-id="becd6-234">Použijte následující příkazy k monitorování aktivit.</span><span class="sxs-lookup"><span data-stu-id="becd6-234">Use the following commands to monitor the activity.</span></span> <span data-ttu-id="becd6-235">Všimněte si, že máte čekat mezi úlohy pro zpracování ukončíte.</span><span class="sxs-lookup"><span data-stu-id="becd6-235">Note that you have to wait in between jobs for the processing to finish.</span></span>

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



## <a name="next-steps"></a><span data-ttu-id="becd6-236">Další kroky</span><span class="sxs-lookup"><span data-stu-id="becd6-236">Next steps</span></span>
<span data-ttu-id="becd6-237">[Další informace](/powershell/azure/overview) o rutin Powershellu pro Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="becd6-237">[Read more](/powershell/azure/overview) about Azure Site Recovery PowerShell cmdlets.</span></span> <span data-ttu-id="becd6-238"></a>.</span><span class="sxs-lookup"><span data-stu-id="becd6-238"></a>.</span></span>
