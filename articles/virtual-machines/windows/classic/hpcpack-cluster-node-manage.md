---
title: "Spravovat výpočetní uzly clusteru HPC Pack | Microsoft Docs"
description: "Další informace o nástroje skript prostředí PowerShell pro přidání, odebrání, spuštění a zastavení výpočetní uzly clusteru HPC Pack 2012 R2 v Azure"
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
ms.openlocfilehash: dc9f354191b9e80ff6a01bd401a874c6998bda79
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="manage-the-number-and-availability-of-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a><span data-ttu-id="8209e-103">Správa počtu a dostupnosti výpočetních uzlů v clusteru HPC Pack v Azure</span><span class="sxs-lookup"><span data-stu-id="8209e-103">Manage the number and availability of compute nodes in an HPC Pack cluster in Azure</span></span>
<span data-ttu-id="8209e-104">Pokud jste vytvořili clusteru HPC Pack 2012 R2 ve virtuálních počítačích Azure, můžete chtít způsoby, jak snadno přidat, odebrat, (zřídit) spuštění nebo zastavení (deprovision) některé výpočetní uzel virtuální počítače v clusteru.</span><span class="sxs-lookup"><span data-stu-id="8209e-104">If you created an HPC Pack 2012 R2 cluster in Azure VMs, you might want ways to easily add, remove, start (provision), or stop (deprovision) some compute node VMs in the cluster.</span></span> <span data-ttu-id="8209e-105">Chcete-li provést tyto úlohy, spusťte prostředí Azure PowerShell skripty, které jsou nainstalované na hlavního uzlu virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8209e-105">To do these tasks, run Azure PowerShell scripts that are installed on the head node VM.</span></span> <span data-ttu-id="8209e-106">Tyto skripty vám pomůžou usnadnit řízení číslo a dostupnost prostředků clusteru HPC Pack, můžete řídit náklady.</span><span class="sxs-lookup"><span data-stu-id="8209e-106">These scripts help you control the number and availability of your HPC Pack cluster resources so you can control costs.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="8209e-107">Tento článek se týká pouze do clusterů HPC Pack 2012 R2 v Azure vytvořit pomocí modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="8209e-107">This article applies only to HPC Pack 2012 R2 clusters in Azure created using the classic deployment model.</span></span> <span data-ttu-id="8209e-108">Microsoft doporučuje, aby byl ve většině nových nasazení použit model Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="8209e-108">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>
> <span data-ttu-id="8209e-109">Kromě toho nejsou k dispozici v prostředí HPC Pack 2016 skriptů prostředí PowerShell popsaných v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="8209e-109">In addition, the PowerShell scripts described in this article are not available in HPC Pack 2016.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8209e-110">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8209e-110">Prerequisites</span></span>
* <span data-ttu-id="8209e-111">**Cluster HPC Pack 2012 R2 ve virtuálních počítačích Azure**: vytvoření clusteru HPC Pack 2012 R2 v modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="8209e-111">**HPC Pack 2012 R2 cluster in Azure VMs**: Create an HPC Pack 2012 R2 cluster in the classic deployment model.</span></span> <span data-ttu-id="8209e-112">Například můžete automatizovat nasazení s použitím bitové kopie prostředí HPC Pack 2012 R2 virtuálních počítačů v Azure Marketplace a skript Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8209e-112">For example, you can automate the deployment by using the HPC Pack 2012 R2 VM image in the Azure Marketplace and an Azure PowerShell script.</span></span> <span data-ttu-id="8209e-113">Informace a požadavky najdete v tématu [vytvoření clusteru prostředí HPC pomocí skriptu pro nasazení HPC Pack IaaS](hpcpack-cluster-powershell-script.md).</span><span class="sxs-lookup"><span data-stu-id="8209e-113">For information and prerequisites, see [Create an HPC Cluster with the HPC Pack IaaS deployment script](hpcpack-cluster-powershell-script.md).</span></span>
  
    <span data-ttu-id="8209e-114">Po nasazení se najít uzel správy skripty ve složce % CCP\_DOMOVSKÉ složky bin % z hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="8209e-114">After deployment, find the node management scripts in the %CCP\_HOME%bin folder on the head node.</span></span> <span data-ttu-id="8209e-115">Všechny tyto skripty spusťte jako správce.</span><span class="sxs-lookup"><span data-stu-id="8209e-115">Run each of the scripts as an administrator.</span></span>
* <span data-ttu-id="8209e-116">**Certifikát správy nebo soubor nastavení publikování Azure**: je třeba provést jednu z následujících postupů z hlavního uzlu:</span><span class="sxs-lookup"><span data-stu-id="8209e-116">**Azure publish settings file or management certificate**: You need to do one of the following on the head node:</span></span>
  
  * <span data-ttu-id="8209e-117">**Soubor nastavení publikování import Azure**.</span><span class="sxs-lookup"><span data-stu-id="8209e-117">**Import the Azure publish settings file**.</span></span> <span data-ttu-id="8209e-118">Chcete-li to provést, spusťte následující rutiny prostředí Azure PowerShell z hlavního uzlu:</span><span class="sxs-lookup"><span data-stu-id="8209e-118">To do this, run the following Azure PowerShell cmdlets on the head node:</span></span>
    
    ```PowerShell
    Get-AzurePublishSettingsFile
    
    Import-AzurePublishSettingsFile –PublishSettingsFile <publish settings file>
    ```
  * <span data-ttu-id="8209e-119">**Konfigurace certifikátu pro správu Azure z hlavního uzlu**.</span><span class="sxs-lookup"><span data-stu-id="8209e-119">**Configure the Azure management certificate on the head node**.</span></span> <span data-ttu-id="8209e-120">Pokud máte soubor .cer, importujte ji do úložiště certifikátů CurrentUser\My a pak spusťte následující rutiny Azure Powershellu pro prostředí Azure (AzureCloud nebo AzureChinaCloud):</span><span class="sxs-lookup"><span data-stu-id="8209e-120">If you have the .cer file, import it in the CurrentUser\My certificate store and then run the following Azure PowerShell cmdlet for your Azure environment (either AzureCloud or AzureChinaCloud):</span></span>
    
    ```PowerShell
    Set-AzureSubscription -SubscriptionName <Sub Name> -SubscriptionId <Sub ID> -Certificate (Get-Item Cert:\CurrentUser\My\<Cert Thrumbprint>) -Environment <AzureCloud | AzureChinaCloud>
    ```

## <a name="add-compute-node-vms"></a><span data-ttu-id="8209e-121">Přidat výpočetním uzlu virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="8209e-121">Add compute node VMs</span></span>
<span data-ttu-id="8209e-122">Přidat výpočetní uzly s **přidat HpcIaaSNode.ps1** skriptu.</span><span class="sxs-lookup"><span data-stu-id="8209e-122">Add compute nodes with the **Add-HpcIaaSNode.ps1** script.</span></span>

### <a name="syntax"></a><span data-ttu-id="8209e-123">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="8209e-123">Syntax</span></span>
```PowerShell
Add-HPCIaaSNode.ps1 [-ServiceName] <String> [-ImageName] <String>
 [-Quantity] <Int32> [-InstanceSize] <String> [-DomainUserName] <String> [[-DomainUserPassword] <String>]
 [[-NodeNameSeries] <String>] [<CommonParameters>]

```
### <a name="parameters"></a><span data-ttu-id="8209e-124">Parametry</span><span class="sxs-lookup"><span data-stu-id="8209e-124">Parameters</span></span>
* <span data-ttu-id="8209e-125">**ServiceName**: název cloudové služby, která nový výpočet uzlu virtuální počítače jsou přidány do.</span><span class="sxs-lookup"><span data-stu-id="8209e-125">**ServiceName**: Name of the cloud service that new compute node VMs are added to.</span></span>
* <span data-ttu-id="8209e-126">**ImageName**: název bitové kopie virtuálního počítače Azure, který můžete získat prostřednictvím portálu Azure classic nebo rutiny Azure Powershellu **Get-AzureVMImage**.</span><span class="sxs-lookup"><span data-stu-id="8209e-126">**ImageName**: Azure VM image name, which can be obtained through the Azure classic portal or Azure PowerShell cmdlet **Get-AzureVMImage**.</span></span> <span data-ttu-id="8209e-127">Obrázek musí splňovat následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="8209e-127">The image must meet the following requirements:</span></span>
  
  1. <span data-ttu-id="8209e-128">Musí být nainstalován operační systém Windows.</span><span class="sxs-lookup"><span data-stu-id="8209e-128">A Windows operating system must be installed.</span></span>
  2. <span data-ttu-id="8209e-129">HPC Pack musí být nainstalován v uzlu výpočetní roli.</span><span class="sxs-lookup"><span data-stu-id="8209e-129">HPC Pack must be installed in the compute node role.</span></span>
  3. <span data-ttu-id="8209e-130">Obrázek musí být privátní bitové kopie v kategorii uživatelů, není veřejný image virtuálního počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="8209e-130">The image must be a private image in the User category, not a public Azure VM image.</span></span>
* <span data-ttu-id="8209e-131">**Množství**: počet výpočetním uzlu virtuální počítače, který se má přidat.</span><span class="sxs-lookup"><span data-stu-id="8209e-131">**Quantity**: Number of compute node VMs to be added.</span></span>
* <span data-ttu-id="8209e-132">**InstanceSize**: velikost výpočetním uzlu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="8209e-132">**InstanceSize**: Size of the compute node VMs.</span></span>
* <span data-ttu-id="8209e-133">**DomainUserName**: uživatelské jméno domény, který se používá k připojení k nové virtuální počítače k doméně.</span><span class="sxs-lookup"><span data-stu-id="8209e-133">**DomainUserName**: Domain user name, which is used to join the new VMs to the domain.</span></span>
* <span data-ttu-id="8209e-134">**DomainUserPassword**: heslo uživatele domény.</span><span class="sxs-lookup"><span data-stu-id="8209e-134">**DomainUserPassword**: Password of the domain user.</span></span>
* <span data-ttu-id="8209e-135">**NodeNameSeries** (volitelné): pojmenování vzor pro výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="8209e-135">**NodeNameSeries** (optional): Naming pattern for the compute nodes.</span></span> <span data-ttu-id="8209e-136">Musí být ve formátu &lt; *kořenové\_název*&gt;&lt;*spustit\_číslo*&gt;%.</span><span class="sxs-lookup"><span data-stu-id="8209e-136">The format must be &lt;*Root\_Name*&gt;&lt;*Start\_Number*&gt;%.</span></span> <span data-ttu-id="8209e-137">Například MyCN % 10 % prostředků a řadu od MyCN11 názvy výpočetního uzlu.</span><span class="sxs-lookup"><span data-stu-id="8209e-137">For example, MyCN%10% means a series of the compute node names starting from MyCN11.</span></span> <span data-ttu-id="8209e-138">Pokud není zadaný, skript používá uzlu nakonfigurované pojmenování řady v clusteru HPC.</span><span class="sxs-lookup"><span data-stu-id="8209e-138">If not specified, the script uses the configured node naming series in the HPC cluster.</span></span>

### <a name="example"></a><span data-ttu-id="8209e-139">Příklad</span><span class="sxs-lookup"><span data-stu-id="8209e-139">Example</span></span>
<span data-ttu-id="8209e-140">Následující příklad přidá 20 velikost velké výpočetním uzlu virtuální počítače v cloudové službě *hpcservice1*, na bitovou kopii virtuálního počítače na bázi *hpccnimage1*.</span><span class="sxs-lookup"><span data-stu-id="8209e-140">The following example adds 20 size Large compute node VMs in the cloud service *hpcservice1*, based on the VM image *hpccnimage1*.</span></span>

```PowerShell
Add-HPCIaaSNode.ps1 –ServiceName hpcservice1 –ImageName hpccniamge1
–Quantity 20 –InstanceSize Large –DomainUserName <username>
-DomainUserPassword <password>
```


## <a name="remove-compute-node-vms"></a><span data-ttu-id="8209e-141">Odebrat výpočetním uzlu virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="8209e-141">Remove compute node VMs</span></span>
<span data-ttu-id="8209e-142">Odebrat výpočetní uzly s **odebrat HpcIaaSNode.ps1** skriptu.</span><span class="sxs-lookup"><span data-stu-id="8209e-142">Remove compute nodes with the **Remove-HpcIaaSNode.ps1** script.</span></span>

### <a name="syntax"></a><span data-ttu-id="8209e-143">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="8209e-143">Syntax</span></span>
```PowerShell
Remove-HPCIaaSNode.ps1 -Name <String[]> [-DeleteVHD] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]

Remove-HPCIaaSNode.ps1 -Node <Object> [-DeleteVHD] [-Force] [-Confirm] [<CommonParameters>]
```

### <a name="parameters"></a><span data-ttu-id="8209e-144">Parametry</span><span class="sxs-lookup"><span data-stu-id="8209e-144">Parameters</span></span>
* <span data-ttu-id="8209e-145">**Název**: názvy uzlů clusteru k odebrání.</span><span class="sxs-lookup"><span data-stu-id="8209e-145">**Name**: Names of cluster nodes to be removed.</span></span> <span data-ttu-id="8209e-146">Jsou podporovány zástupné znaky.</span><span class="sxs-lookup"><span data-stu-id="8209e-146">Wildcards are supported.</span></span> <span data-ttu-id="8209e-147">Název parametru sada je název.</span><span class="sxs-lookup"><span data-stu-id="8209e-147">The parameter set name is Name.</span></span> <span data-ttu-id="8209e-148">Nejde zadat oba seznamy **název** a **uzlu** parametry.</span><span class="sxs-lookup"><span data-stu-id="8209e-148">You can't specify both the **Name** and **Node** parameters.</span></span>
* <span data-ttu-id="8209e-149">**Uzel**: objekt HpcNode pro uzly odebrány, které lze získat pomocí rutiny prostředí HPC PowerShell [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx).</span><span class="sxs-lookup"><span data-stu-id="8209e-149">**Node**: The HpcNode object for the nodes to be removed, which can be obtained through the HPC PowerShell cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx).</span></span> <span data-ttu-id="8209e-150">Název parametru sada je uzel.</span><span class="sxs-lookup"><span data-stu-id="8209e-150">The parameter set name is Node.</span></span> <span data-ttu-id="8209e-151">Nejde zadat oba seznamy **název** a **uzlu** parametry.</span><span class="sxs-lookup"><span data-stu-id="8209e-151">You can't specify both the **Name** and **Node** parameters.</span></span>
* <span data-ttu-id="8209e-152">**DeleteVHD** (volitelné): nastavení, které chcete odstranit související disky pro virtuální počítače, které jsou odebrány.</span><span class="sxs-lookup"><span data-stu-id="8209e-152">**DeleteVHD** (optional): Setting to delete the associated disks for the VMs that are removed.</span></span>
* <span data-ttu-id="8209e-153">**Vynutit** (volitelné): nastavení přinutit HPC uzly offline před jejich odebráním.</span><span class="sxs-lookup"><span data-stu-id="8209e-153">**Force** (optional): Setting to force HPC nodes offline before removing them.</span></span>
* <span data-ttu-id="8209e-154">**Potvrďte** (volitelné): výzva k potvrzení před provedením příkazu.</span><span class="sxs-lookup"><span data-stu-id="8209e-154">**Confirm** (optional): Prompt for confirmation before executing the command.</span></span>
* <span data-ttu-id="8209e-155">**WhatIf**: nastavení popisují, co by mohlo dojít, pokud se příkaz provedl, aniž by příkaz skutečně proveden.</span><span class="sxs-lookup"><span data-stu-id="8209e-155">**WhatIf**: Setting to describe what would happen if you executed the command without actually executing the command.</span></span>

### <a name="example"></a><span data-ttu-id="8209e-156">Příklad</span><span class="sxs-lookup"><span data-stu-id="8209e-156">Example</span></span>
<span data-ttu-id="8209e-157">Následující příklad vynutí offline uzly s názvy od *HPCNode-CN -* a jejich odebere uzlů a jejich přidružené disky.</span><span class="sxs-lookup"><span data-stu-id="8209e-157">The following example forces offline the nodes with names beginning *HPCNode-CN-* and them removes the nodes and their associated disks.</span></span>

```PowerShell
Remove-HPCIaaSNode.ps1 –Name HPCNodeCN-* –DeleteVHD -Force
```

## <a name="start-compute-node-vms"></a><span data-ttu-id="8209e-158">Spuštění výpočetního uzlu virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="8209e-158">Start compute node VMs</span></span>
<span data-ttu-id="8209e-159">Spuštění výpočetní uzly se **Start-HpcIaaSNode.ps1** skriptu.</span><span class="sxs-lookup"><span data-stu-id="8209e-159">Start compute nodes with the **Start-HpcIaaSNode.ps1** script.</span></span>

### <a name="syntax"></a><span data-ttu-id="8209e-160">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="8209e-160">Syntax</span></span>
```PowerShell
Start-HPCIaaSNode.ps1 -Name <String[]> [<CommonParameters>]

Start-HPCIaaSNode.ps1 -Node <Object> [<CommonParameters>]
```
### <a name="parameters"></a><span data-ttu-id="8209e-161">Parametry</span><span class="sxs-lookup"><span data-stu-id="8209e-161">Parameters</span></span>
* <span data-ttu-id="8209e-162">**Název**: názvy uzlů clusteru spustit.</span><span class="sxs-lookup"><span data-stu-id="8209e-162">**Name**: Names of the cluster nodes to be started.</span></span> <span data-ttu-id="8209e-163">Jsou podporovány zástupné znaky.</span><span class="sxs-lookup"><span data-stu-id="8209e-163">Wildcards are supported.</span></span> <span data-ttu-id="8209e-164">Název parametru sada je název.</span><span class="sxs-lookup"><span data-stu-id="8209e-164">The parameter set name is Name.</span></span> <span data-ttu-id="8209e-165">Nejde zadat oba seznamy **název** a **uzlu** parametry.</span><span class="sxs-lookup"><span data-stu-id="8209e-165">You cannot specify both the **Name** and **Node** parameters.</span></span>
* <span data-ttu-id="8209e-166">**Uzel**-HpcNode objektu pro uzly má být spuštěn, které lze získat pomocí rutiny prostředí HPC PowerShell [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx).</span><span class="sxs-lookup"><span data-stu-id="8209e-166">**Node**- The HpcNode object for the nodes to be started, which can be obtained through the HPC PowerShell cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx).</span></span> <span data-ttu-id="8209e-167">Název parametru sada je uzel.</span><span class="sxs-lookup"><span data-stu-id="8209e-167">The parameter set name is Node.</span></span> <span data-ttu-id="8209e-168">Nejde zadat oba seznamy **název** a **uzlu** parametry.</span><span class="sxs-lookup"><span data-stu-id="8209e-168">You cannot specify both the **Name** and **Node** parameters.</span></span>

### <a name="example"></a><span data-ttu-id="8209e-169">Příklad</span><span class="sxs-lookup"><span data-stu-id="8209e-169">Example</span></span>
<span data-ttu-id="8209e-170">Následující příklad spustí uzly s názvy od *HPCNode-CN -*.</span><span class="sxs-lookup"><span data-stu-id="8209e-170">The following example starts nodes with names beginning *HPCNode-CN-*.</span></span>

```PowerShell
Start-HPCIaaSNode.ps1 –Name HPCNodeCN-*
```

## <a name="stop-compute-node-vms"></a><span data-ttu-id="8209e-171">Zastavit výpočetním uzlu virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="8209e-171">Stop compute node VMs</span></span>
<span data-ttu-id="8209e-172">Zastavit výpočetní uzly s **Stop-HpcIaaSNode.ps1** skriptu.</span><span class="sxs-lookup"><span data-stu-id="8209e-172">Stop compute nodes with the **Stop-HpcIaaSNode.ps1** script.</span></span>

### <a name="syntax"></a><span data-ttu-id="8209e-173">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="8209e-173">Syntax</span></span>
```PowerShell
Stop-HPCIaaSNode.ps1 -Name <String[]> [-Force] [<CommonParameters>]

Stop-HPCIaaSNode.ps1 -Node <Object> [-Force] [<CommonParameters>]
```

### <a name="parameters"></a><span data-ttu-id="8209e-174">Parametry</span><span class="sxs-lookup"><span data-stu-id="8209e-174">Parameters</span></span>
* <span data-ttu-id="8209e-175">**Název**-názvy uzlů clusteru, který má být zastaven.</span><span class="sxs-lookup"><span data-stu-id="8209e-175">**Name**- Names of the cluster nodes to be stopped.</span></span> <span data-ttu-id="8209e-176">Jsou podporovány zástupné znaky.</span><span class="sxs-lookup"><span data-stu-id="8209e-176">Wildcards are supported.</span></span> <span data-ttu-id="8209e-177">Název parametru sada je název.</span><span class="sxs-lookup"><span data-stu-id="8209e-177">The parameter set name is Name.</span></span> <span data-ttu-id="8209e-178">Nejde zadat oba seznamy **název** a **uzlu** parametry.</span><span class="sxs-lookup"><span data-stu-id="8209e-178">You cannot specify both the **Name** and **Node** parameters.</span></span>
* <span data-ttu-id="8209e-179">**Uzel**: objekt HpcNode pro uzly zastavení, které lze získat pomocí rutiny prostředí HPC PowerShell [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx).</span><span class="sxs-lookup"><span data-stu-id="8209e-179">**Node**: The HpcNode object for the nodes to be stopped, which can be obtained through the HPC PowerShell cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx).</span></span> <span data-ttu-id="8209e-180">Název parametru sada je uzel.</span><span class="sxs-lookup"><span data-stu-id="8209e-180">The parameter set name is Node.</span></span> <span data-ttu-id="8209e-181">Nejde zadat oba seznamy **název** a **uzlu** parametry.</span><span class="sxs-lookup"><span data-stu-id="8209e-181">You cannot specify both the **Name** and **Node** parameters.</span></span>
* <span data-ttu-id="8209e-182">**Vynutit** (volitelné): nastavení přinutit HPC uzly do offline režimu před zastavením je.</span><span class="sxs-lookup"><span data-stu-id="8209e-182">**Force** (optional): Setting to force HPC nodes offline before stopping them.</span></span>

### <a name="example"></a><span data-ttu-id="8209e-183">Příklad</span><span class="sxs-lookup"><span data-stu-id="8209e-183">Example</span></span>
<span data-ttu-id="8209e-184">Následující příklad vynutí offline uzly s názvy od *HPCNode-CN -* a poté se zastaví uzly.</span><span class="sxs-lookup"><span data-stu-id="8209e-184">The following example forces offline nodes with names beginning *HPCNode-CN-* and then stops the nodes.</span></span>

```PowerShell
Stop-HPCIaaSNode.ps1 –Name HPCNodeCN-* -Force
```

## <a name="next-steps"></a><span data-ttu-id="8209e-185">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8209e-185">Next steps</span></span>
* <span data-ttu-id="8209e-186">Automaticky zvětšovat a zmenšovat uzly v clusteru podle aktuální zatížení úloh a úloh v clusteru, najdete v tématu [automaticky zvýšit nebo snížit prostředků clusteru HPC Pack v Azure podle zatížení clusteru](hpcpack-cluster-node-autogrowshrink.md).</span><span class="sxs-lookup"><span data-stu-id="8209e-186">To automatically grow or shrink the cluster nodes according to the current workload of jobs and tasks on the cluster, see [Automatically grow and shrink the HPC Pack cluster resources in Azure according to the cluster workload](hpcpack-cluster-node-autogrowshrink.md).</span></span>

