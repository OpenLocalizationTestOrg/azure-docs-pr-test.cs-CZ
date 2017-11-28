---
title: "výpočetní uzly clusteru HPC Pack aaaManage | Microsoft Docs"
description: "Další informace o tooadd nástroje skript prostředí PowerShell, odebrat, spuštění a zastavení výpočetní uzly clusteru HPC Pack 2012 R2 v Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 4193f03b-94e9-4704-a7ad-379abde063a9
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: 5ac1142cc5da984020779434fbb7cba5ad7c14bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hello-number-and-availability-of-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a><span data-ttu-id="8dfb8-103">Správa hello číslo a dostupnosti výpočetních uzlů v clusteru služby HPC Pack v Azure</span><span class="sxs-lookup"><span data-stu-id="8dfb8-103">Manage hello number and availability of compute nodes in an HPC Pack cluster in Azure</span></span>
<span data-ttu-id="8dfb8-104">Pokud jste vytvořili clusteru HPC Pack 2012 R2 ve virtuálních počítačích Azure, můžete chtít, že výpočetní způsoby tooeasily přidat, odebrat, (zřídit) spuštění nebo zastavení (deprovision) některé virtuální počítače uzlu v clusteru.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-104">If you created an HPC Pack 2012 R2 cluster in Azure VMs, you might want ways tooeasily add, remove, start (provision), or stop (deprovision) some compute node VMs in the cluster.</span></span> <span data-ttu-id="8dfb8-105">toodo tyto úlohy spouštět skripty prostředí Azure PowerShell, které jsou nainstalovány na hello hlavního uzlu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-105">toodo these tasks, run Azure PowerShell scripts that are installed on hello head node VM.</span></span> <span data-ttu-id="8dfb8-106">Tyto skripty pomáhá řídit hello číslo a dostupnost prostředků clusteru HPC Pack, můžete řídit náklady.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-106">These scripts help you control hello number and availability of your HPC Pack cluster resources so you can control costs.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="8dfb8-107">Tento článek se týká pouze clustery tooHPC Pack 2012 R2 v Azure vytvořit pomocí modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-107">This article applies only tooHPC Pack 2012 R2 clusters in Azure created using hello classic deployment model.</span></span> <span data-ttu-id="8dfb8-108">Společnost Microsoft doporučuje, aby většina nových nasazení používala model Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>
> <span data-ttu-id="8dfb8-109">Kromě toho nejsou k dispozici v prostředí HPC Pack 2016 hello skriptů prostředí PowerShell popsaných v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-109">In addition, hello PowerShell scripts described in this article are not available in HPC Pack 2016.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8dfb8-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8dfb8-110">Prerequisites</span></span>
* <span data-ttu-id="8dfb8-111">**Cluster HPC Pack 2012 R2 ve virtuálních počítačích Azure**: vytvoření clusteru HPC Pack 2012 R2 v modelu nasazení classic hello.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-111">**HPC Pack 2012 R2 cluster in Azure VMs**: Create an HPC Pack 2012 R2 cluster in hello classic deployment model.</span></span> <span data-ttu-id="8dfb8-112">Například můžete automatizovat nasazení hello pomocí bitové kopie virtuálních počítačů HPC Pack 2012 R2 hello v hello Azure Marketplace a skript Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-112">For example, you can automate hello deployment by using hello HPC Pack 2012 R2 VM image in hello Azure Marketplace and an Azure PowerShell script.</span></span> <span data-ttu-id="8dfb8-113">Informace a požadavky najdete v tématu [vytvoření clusteru prostředí HPC s hello skript nasazení HPC Pack IaaS](hpcpack-cluster-powershell-script.md).</span><span class="sxs-lookup"><span data-stu-id="8dfb8-113">For information and prerequisites, see [Create an HPC Cluster with hello HPC Pack IaaS deployment script](hpcpack-cluster-powershell-script.md).</span></span>
  
    <span data-ttu-id="8dfb8-114">Po nasazení najít hello uzlu správy skripty v hello % CCP\_DOMOVSKÉ složky bin % hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-114">After deployment, find hello node management scripts in hello %CCP\_HOME%bin folder on hello head node.</span></span> <span data-ttu-id="8dfb8-115">Spuštění každého ze hello skripty jako správce.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-115">Run each of hello scripts as an administrator.</span></span>
* <span data-ttu-id="8dfb8-116">**Certifikát správy nebo soubor nastavení publikování Azure**: budete potřebovat toodo jedna z následujících hello hello hlavního uzlu:</span><span class="sxs-lookup"><span data-stu-id="8dfb8-116">**Azure publish settings file or management certificate**: You need toodo one of hello following on hello head node:</span></span>
  
  * <span data-ttu-id="8dfb8-117">**Soubor nastavení publikování import hello Azure**.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-117">**Import hello Azure publish settings file**.</span></span> <span data-ttu-id="8dfb8-118">toodo se spuštění hello následující rutiny prostředí Azure PowerShell hello hlavního uzlu:</span><span class="sxs-lookup"><span data-stu-id="8dfb8-118">toodo this, run hello following Azure PowerShell cmdlets on hello head node:</span></span>
    
    ```PowerShell
    Get-AzurePublishSettingsFile
    
    Import-AzurePublishSettingsFile –PublishSettingsFile <publish settings file>
    ```
  * <span data-ttu-id="8dfb8-119">**Konfigurovat certifikát pro správu Azure hello hello hlavního uzlu**.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-119">**Configure hello Azure management certificate on hello head node**.</span></span> <span data-ttu-id="8dfb8-120">Pokud máte soubor .cer hello, importujte ji do úložiště certifikátů CurrentUser\My hello a pak spusťte následující rutiny Azure Powershellu pro prostředí Azure (AzureCloud nebo AzureChinaCloud) hello:</span><span class="sxs-lookup"><span data-stu-id="8dfb8-120">If you have hello .cer file, import it in hello CurrentUser\My certificate store and then run hello following Azure PowerShell cmdlet for your Azure environment (either AzureCloud or AzureChinaCloud):</span></span>
    
    ```PowerShell
    Set-AzureSubscription -SubscriptionName <Sub Name> -SubscriptionId <Sub ID> -Certificate (Get-Item Cert:\CurrentUser\My\<Cert Thrumbprint>) -Environment <AzureCloud | AzureChinaCloud>
    ```

## <a name="add-compute-node-vms"></a><span data-ttu-id="8dfb8-121">Přidat výpočetním uzlu virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="8dfb8-121">Add compute node VMs</span></span>
<span data-ttu-id="8dfb8-122">Přidat výpočetní uzly s hello **přidat HpcIaaSNode.ps1** skriptu.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-122">Add compute nodes with hello **Add-HpcIaaSNode.ps1** script.</span></span>

### <a name="syntax"></a><span data-ttu-id="8dfb8-123">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="8dfb8-123">Syntax</span></span>
```PowerShell
Add-HPCIaaSNode.ps1 [-ServiceName] <String> [-ImageName] <String>
 [-Quantity] <Int32> [-InstanceSize] <String> [-DomainUserName] <String> [[-DomainUserPassword] <String>]
 [[-NodeNameSeries] <String>] [<CommonParameters>]

```
### <a name="parameters"></a><span data-ttu-id="8dfb8-124">Parametry</span><span class="sxs-lookup"><span data-stu-id="8dfb8-124">Parameters</span></span>
* <span data-ttu-id="8dfb8-125">**ServiceName**: název hello Cloudová služba, která nový výpočet uzlu virtuální počítače jsou přidány do.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-125">**ServiceName**: Name of hello cloud service that new compute node VMs are added to.</span></span>
* <span data-ttu-id="8dfb8-126">**ImageName**: název bitové kopie virtuálního počítače Azure, který můžete získat prostřednictvím hello portál Azure classic nebo rutiny Azure Powershellu **Get-AzureVMImage**.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-126">**ImageName**: Azure VM image name, which can be obtained through hello Azure classic portal or Azure PowerShell cmdlet **Get-AzureVMImage**.</span></span> <span data-ttu-id="8dfb8-127">Obrázek Hello musí splňovat následující požadavky hello:</span><span class="sxs-lookup"><span data-stu-id="8dfb8-127">hello image must meet hello following requirements:</span></span>
  
  1. <span data-ttu-id="8dfb8-128">Musí být nainstalován operační systém Windows.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-128">A Windows operating system must be installed.</span></span>
  2. <span data-ttu-id="8dfb8-129">HPC Pack musí být nainstalován v hello výpočetní uzel roli.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-129">HPC Pack must be installed in hello compute node role.</span></span>
  3. <span data-ttu-id="8dfb8-130">Obrázek Hello musí být privátní bitové kopie v kategorii uživatelů hello, není veřejný image virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-130">hello image must be a private image in hello User category, not a public Azure VM image.</span></span>
* <span data-ttu-id="8dfb8-131">**Množství**: počet výpočetních uzlů virtuální počítače toobe přidat.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-131">**Quantity**: Number of compute node VMs toobe added.</span></span>
* <span data-ttu-id="8dfb8-132">**InstanceSize**: velikost hello výpočetní uzel virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-132">**InstanceSize**: Size of hello compute node VMs.</span></span>
* <span data-ttu-id="8dfb8-133">**DomainUserName**: uživatelské jméno domény, který je použité toojoin hello nové virtuální počítače toohello domény.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-133">**DomainUserName**: Domain user name, which is used toojoin hello new VMs toohello domain.</span></span>
* <span data-ttu-id="8dfb8-134">**DomainUserPassword**: heslo uživatele domény hello.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-134">**DomainUserPassword**: Password of hello domain user.</span></span>
* <span data-ttu-id="8dfb8-135">**NodeNameSeries** (volitelné): pojmenování vzor pro hello výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-135">**NodeNameSeries** (optional): Naming pattern for hello compute nodes.</span></span> <span data-ttu-id="8dfb8-136">musí být ve formátu Hello &lt; *kořenové\_název*&gt;&lt;*spustit\_číslo*&gt;%.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-136">hello format must be &lt;*Root\_Name*&gt;&lt;*Start\_Number*&gt;%.</span></span> <span data-ttu-id="8dfb8-137">Například MyCN % 10 % znamená řadu hello výpočetní uzel názvy od MyCN11.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-137">For example, MyCN%10% means a series of hello compute node names starting from MyCN11.</span></span> <span data-ttu-id="8dfb8-138">Pokud není zadaný, nakonfigurovat hello používá skript hello pojmenování řady uzlu v clusteru HPC hello.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-138">If not specified, hello script uses hello configured node naming series in hello HPC cluster.</span></span>

### <a name="example"></a><span data-ttu-id="8dfb8-139">Příklad</span><span class="sxs-lookup"><span data-stu-id="8dfb8-139">Example</span></span>
<span data-ttu-id="8dfb8-140">Hello následující příklad přidá 20 velikost velké výpočetním uzlu virtuální počítače v cloudové službě hello *hpcservice1*, podle image virtuálního počítače hello *hpccnimage1*.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-140">hello following example adds 20 size Large compute node VMs in hello cloud service *hpcservice1*, based on hello VM image *hpccnimage1*.</span></span>

```PowerShell
Add-HPCIaaSNode.ps1 –ServiceName hpcservice1 –ImageName hpccniamge1
–Quantity 20 –InstanceSize Large –DomainUserName <username>
-DomainUserPassword <password>
```


## <a name="remove-compute-node-vms"></a><span data-ttu-id="8dfb8-141">Odebrat výpočetním uzlu virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="8dfb8-141">Remove compute node VMs</span></span>
<span data-ttu-id="8dfb8-142">Odebrat výpočetní uzly s hello **odebrat HpcIaaSNode.ps1** skriptu.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-142">Remove compute nodes with hello **Remove-HpcIaaSNode.ps1** script.</span></span>

### <a name="syntax"></a><span data-ttu-id="8dfb8-143">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="8dfb8-143">Syntax</span></span>
```PowerShell
Remove-HPCIaaSNode.ps1 -Name <String[]> [-DeleteVHD] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]

Remove-HPCIaaSNode.ps1 -Node <Object> [-DeleteVHD] [-Force] [-Confirm] [<CommonParameters>]
```

### <a name="parameters"></a><span data-ttu-id="8dfb8-144">Parametry</span><span class="sxs-lookup"><span data-stu-id="8dfb8-144">Parameters</span></span>
* <span data-ttu-id="8dfb8-145">**Název**: názvy toobe uzly clusteru odstraněny.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-145">**Name**: Names of cluster nodes toobe removed.</span></span> <span data-ttu-id="8dfb8-146">Jsou podporovány zástupné znaky.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-146">Wildcards are supported.</span></span> <span data-ttu-id="8dfb8-147">Název sady parametr Hello je název.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-147">hello parameter set name is Name.</span></span> <span data-ttu-id="8dfb8-148">Nelze zadat obě hello **název** a **uzlu** parametry.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-148">You can't specify both hello **Name** and **Node** parameters.</span></span>
* <span data-ttu-id="8dfb8-149">**Uzel**: hello objekt HpcNode pro toobe hello uzly odebrány, který lze získat pomocí rutiny prostředí HPC PowerShell hello [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx).</span><span class="sxs-lookup"><span data-stu-id="8dfb8-149">**Node**: hello HpcNode object for hello nodes toobe removed, which can be obtained through hello HPC PowerShell cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx).</span></span> <span data-ttu-id="8dfb8-150">Název sady parametr Hello je uzel.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-150">hello parameter set name is Node.</span></span> <span data-ttu-id="8dfb8-151">Nelze zadat obě hello **název** a **uzlu** parametry.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-151">You can't specify both hello **Name** and **Node** parameters.</span></span>
* <span data-ttu-id="8dfb8-152">**DeleteVHD** (volitelné): nastavení toodelete hello přidružené disky pro virtuální počítače, které jsou odebrány hello.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-152">**DeleteVHD** (optional): Setting toodelete hello associated disks for hello VMs that are removed.</span></span>
* <span data-ttu-id="8dfb8-153">**Platnost** (volitelné): nastavení tooforce offline HPC uzly před jejich odebráním.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-153">**Force** (optional): Setting tooforce HPC nodes offline before removing them.</span></span>
* <span data-ttu-id="8dfb8-154">**Potvrďte** (volitelné): výzva k potvrzení před provedením příkazu hello.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-154">**Confirm** (optional): Prompt for confirmation before executing hello command.</span></span>
* <span data-ttu-id="8dfb8-155">**WhatIf**: nastavení toodescribe, co by se stalo, pokud proveden příkaz hello bez hello příkaz skutečně proveden.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-155">**WhatIf**: Setting toodescribe what would happen if you executed hello command without actually executing hello command.</span></span>

### <a name="example"></a><span data-ttu-id="8dfb8-156">Příklad</span><span class="sxs-lookup"><span data-stu-id="8dfb8-156">Example</span></span>
<span data-ttu-id="8dfb8-157">Hello následující příklad vynutí offline hello uzly s názvy od *HPCNode-CN -* a jejich odebere hello uzlů a jejich přidružené disky.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-157">hello following example forces offline hello nodes with names beginning *HPCNode-CN-* and them removes hello nodes and their associated disks.</span></span>

```PowerShell
Remove-HPCIaaSNode.ps1 –Name HPCNodeCN-* –DeleteVHD -Force
```

## <a name="start-compute-node-vms"></a><span data-ttu-id="8dfb8-158">Spuštění výpočetního uzlu virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="8dfb8-158">Start compute node VMs</span></span>
<span data-ttu-id="8dfb8-159">Spuštění výpočetní uzly s hello **Start-HpcIaaSNode.ps1** skriptu.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-159">Start compute nodes with hello **Start-HpcIaaSNode.ps1** script.</span></span>

### <a name="syntax"></a><span data-ttu-id="8dfb8-160">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="8dfb8-160">Syntax</span></span>
```PowerShell
Start-HPCIaaSNode.ps1 -Name <String[]> [<CommonParameters>]

Start-HPCIaaSNode.ps1 -Node <Object> [<CommonParameters>]
```
### <a name="parameters"></a><span data-ttu-id="8dfb8-161">Parametry</span><span class="sxs-lookup"><span data-stu-id="8dfb8-161">Parameters</span></span>
* <span data-ttu-id="8dfb8-162">**Název**: názvy toobe uzly clusteru hello spuštěna.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-162">**Name**: Names of hello cluster nodes toobe started.</span></span> <span data-ttu-id="8dfb8-163">Jsou podporovány zástupné znaky.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-163">Wildcards are supported.</span></span> <span data-ttu-id="8dfb8-164">Název sady parametr Hello je název.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-164">hello parameter set name is Name.</span></span> <span data-ttu-id="8dfb8-165">Nelze zadat obě hello **název** a **uzlu** parametry.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-165">You cannot specify both hello **Name** and **Node** parameters.</span></span>
* <span data-ttu-id="8dfb8-166">**Uzel**-hello objekt HpcNode pro toobe uzly hello spustili, který lze získat pomocí rutiny prostředí HPC PowerShell hello [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx).</span><span class="sxs-lookup"><span data-stu-id="8dfb8-166">**Node**- hello HpcNode object for hello nodes toobe started, which can be obtained through hello HPC PowerShell cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx).</span></span> <span data-ttu-id="8dfb8-167">Název sady parametr Hello je uzel.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-167">hello parameter set name is Node.</span></span> <span data-ttu-id="8dfb8-168">Nelze zadat obě hello **název** a **uzlu** parametry.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-168">You cannot specify both hello **Name** and **Node** parameters.</span></span>

### <a name="example"></a><span data-ttu-id="8dfb8-169">Příklad</span><span class="sxs-lookup"><span data-stu-id="8dfb8-169">Example</span></span>
<span data-ttu-id="8dfb8-170">Hello následující příklad spustí uzly s názvy od *HPCNode-CN -*.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-170">hello following example starts nodes with names beginning *HPCNode-CN-*.</span></span>

```PowerShell
Start-HPCIaaSNode.ps1 –Name HPCNodeCN-*
```

## <a name="stop-compute-node-vms"></a><span data-ttu-id="8dfb8-171">Zastavit výpočetním uzlu virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="8dfb8-171">Stop compute node VMs</span></span>
<span data-ttu-id="8dfb8-172">Zastavit výpočetní uzly s hello **Stop-HpcIaaSNode.ps1** skriptu.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-172">Stop compute nodes with hello **Stop-HpcIaaSNode.ps1** script.</span></span>

### <a name="syntax"></a><span data-ttu-id="8dfb8-173">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="8dfb8-173">Syntax</span></span>
```PowerShell
Stop-HPCIaaSNode.ps1 -Name <String[]> [-Force] [<CommonParameters>]

Stop-HPCIaaSNode.ps1 -Node <Object> [-Force] [<CommonParameters>]
```

### <a name="parameters"></a><span data-ttu-id="8dfb8-174">Parametry</span><span class="sxs-lookup"><span data-stu-id="8dfb8-174">Parameters</span></span>
* <span data-ttu-id="8dfb8-175">**Název**-názvy toobe uzly clusteru hello zastavena.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-175">**Name**- Names of hello cluster nodes toobe stopped.</span></span> <span data-ttu-id="8dfb8-176">Jsou podporovány zástupné znaky.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-176">Wildcards are supported.</span></span> <span data-ttu-id="8dfb8-177">Název sady parametr Hello je název.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-177">hello parameter set name is Name.</span></span> <span data-ttu-id="8dfb8-178">Nelze zadat obě hello **název** a **uzlu** parametry.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-178">You cannot specify both hello **Name** and **Node** parameters.</span></span>
* <span data-ttu-id="8dfb8-179">**Uzel**: hello objekt HpcNode pro toobe uzly hello zastavená, který lze získat pomocí rutiny prostředí HPC PowerShell hello [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx).</span><span class="sxs-lookup"><span data-stu-id="8dfb8-179">**Node**: hello HpcNode object for hello nodes toobe stopped, which can be obtained through hello HPC PowerShell cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx).</span></span> <span data-ttu-id="8dfb8-180">Název sady parametr Hello je uzel.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-180">hello parameter set name is Node.</span></span> <span data-ttu-id="8dfb8-181">Nelze zadat obě hello **název** a **uzlu** parametry.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-181">You cannot specify both hello **Name** and **Node** parameters.</span></span>
* <span data-ttu-id="8dfb8-182">**Platnost** (volitelné): nastavení tooforce offline HPC uzly před zastavením je.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-182">**Force** (optional): Setting tooforce HPC nodes offline before stopping them.</span></span>

### <a name="example"></a><span data-ttu-id="8dfb8-183">Příklad</span><span class="sxs-lookup"><span data-stu-id="8dfb8-183">Example</span></span>
<span data-ttu-id="8dfb8-184">Hello následující příklad vynutí offline uzly s názvy od *HPCNode-CN -* a pak zastaví hello uzly.</span><span class="sxs-lookup"><span data-stu-id="8dfb8-184">hello following example forces offline nodes with names beginning *HPCNode-CN-* and then stops hello nodes.</span></span>

```PowerShell
Stop-HPCIaaSNode.ps1 –Name HPCNodeCN-* -Force
```

## <a name="next-steps"></a><span data-ttu-id="8dfb8-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8dfb8-185">Next steps</span></span>
* <span data-ttu-id="8dfb8-186">tooautomatically zvětšit nebo zmenšit hello uzly clusteru podle aktuální zatížení hello úloh a úloh v hello clusteru najdete v tématu [automaticky zvětšovat a zmenšovat prostředky clusteru HPC Pack hello v Azure podle toohello zatížení clusteru](hpcpack-cluster-node-autogrowshrink.md).</span><span class="sxs-lookup"><span data-stu-id="8dfb8-186">tooautomatically grow or shrink hello cluster nodes according to hello current workload of jobs and tasks on hello cluster, see [Automatically grow and shrink hello HPC Pack cluster resources in Azure according toohello cluster workload](hpcpack-cluster-node-autogrowshrink.md).</span></span>

