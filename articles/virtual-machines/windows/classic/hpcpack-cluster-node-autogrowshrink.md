---
title: "Uzly clusteru HPC Pack škálování | Microsoft Docs"
description: "Automaticky zvýšit nebo snížit počet výpočetních uzlů clusteru HPC Pack v Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: 
editor: tysonn
ms.assetid: 38762cd1-f917-464c-ae5d-b02b1eb21e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 12/08/2016
ms.author: danlep
ms.openlocfilehash: 0dc0d15c64d8951c3c457df73588c37418a3c8a4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="automatically-grow-and-shrink-the-hpc-pack-cluster-resources-in-azure-according-to-the-cluster-workload"></a><span data-ttu-id="8d9dd-103">Automaticky zvýšit nebo snížit prostředků clusteru HPC Pack v Azure podle zatížení clusteru</span><span class="sxs-lookup"><span data-stu-id="8d9dd-103">Automatically grow and shrink the HPC Pack cluster resources in Azure according to the cluster workload</span></span>
<span data-ttu-id="8d9dd-104">Pokud nasazujete Azure "shluků" uzly v clusteru HPC Pack nebo vytváření clusteru HPC Pack ve virtuálních počítačích Azure, můžete způsob, jak automaticky zvětšovat a zmenšovat prostředků clusteru jako uzly nebo jader podle zatížení v clusteru.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-104">If you deploy Azure “burst” nodes in your HPC Pack cluster, or you create an HPC Pack cluster in Azure VMs, you may want a way to automatically grow or shrink the cluster resources such as nodes or cores according to the workload on the cluster.</span></span> <span data-ttu-id="8d9dd-105">Škálování prostředků clusteru tímto způsobem umožňuje efektivněji používat vašich prostředků Azure a kontrolovat náklady.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-105">Scaling the cluster resources in this way allows you to use your Azure resources more efficiently and control their costs.</span></span>

<span data-ttu-id="8d9dd-106">Tento článek ukazuje, že dva způsoby, které poskytuje HPC Pack Chcete používat automatické škálování výpočetní prostředky:</span><span class="sxs-lookup"><span data-stu-id="8d9dd-106">This article shows you two ways that HPC Pack provides to autoscale compute resources:</span></span>

* <span data-ttu-id="8d9dd-107">Vlastnost clusteru HPC Pack **AutoGrowShrink**</span><span class="sxs-lookup"><span data-stu-id="8d9dd-107">The HPC Pack cluster property **AutoGrowShrink**</span></span>

* <span data-ttu-id="8d9dd-108">**AzureAutoGrowShrink.ps1** skript prostředí PowerShell HPC</span><span class="sxs-lookup"><span data-stu-id="8d9dd-108">The **AzureAutoGrowShrink.ps1** HPC PowerShell script</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="8d9dd-109">Aktuálně lze pouze automaticky zvýšit nebo snížit HPC Pack výpočetní uzly, které je spuštěn operační systém Windows Server.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-109">Currently you can only automatically grow and shrink HPC Pack compute nodes that are running a Windows Server operating system.</span></span>


## <a name="set-the-autogrowshrink-cluster-property"></a><span data-ttu-id="8d9dd-110">Nastavte vlastnost AutoGrowShrink clusteru</span><span class="sxs-lookup"><span data-stu-id="8d9dd-110">Set the AutoGrowShrink cluster property</span></span>
### <a name="prerequisites"></a><span data-ttu-id="8d9dd-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8d9dd-111">Prerequisites</span></span>

* <span data-ttu-id="8d9dd-112">**HPC Pack 2012 R2 Update 2 nebo novější clusteru** -hlavního uzlu clusteru může být nasazena místně nebo ve virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-112">**HPC Pack 2012 R2 Update 2 or later cluster** - The cluster head node can be deployed either on-premises or in an Azure VM.</span></span> <span data-ttu-id="8d9dd-113">V tématu [nastavení hybridního clusteru pomocí sady HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) začít pracovat se hlavnímu uzlu místní a uzly Azure "shluků".</span><span class="sxs-lookup"><span data-stu-id="8d9dd-113">See [Set up a hybrid cluster with HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) to get started with an on-premises head node and Azure "burst" nodes.</span></span> <span data-ttu-id="8d9dd-114">Najdete v článku [skript nasazení HPC Pack IaaS](hpcpack-cluster-powershell-script.md) k rychlému nasazení clusteru HPC Pack ve virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-114">See the [HPC Pack IaaS deployment script](hpcpack-cluster-powershell-script.md) to quickly deploy an HPC Pack cluster in Azure VMs.</span></span>

* <span data-ttu-id="8d9dd-115">**Pro cluster s hlavního uzlu v Azure (modelu nasazení Resource Manager)** – od HPC Pack 2016, ověřování pomocí certifikátu v aplikaci Azure Active Directory se používá pro automaticky rostoucí a zmenšení clusteru virtuálních počítačů nasadit pomocí Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-115">**For a cluster with a head node in Azure (Resource Manager deployment model)** - Starting in HPC Pack 2016, certificate authentication in an Azure Active Directory application is used for automatically growing and shrinking cluster VMs deployed using Azure Resource Manager.</span></span> <span data-ttu-id="8d9dd-116">Nakonfigurujte certifikát takto:</span><span class="sxs-lookup"><span data-stu-id="8d9dd-116">Configure a certificate as follows:</span></span>

  1. <span data-ttu-id="8d9dd-117">Po nasazení clusteru připojte pomocí vzdálené plochy na jeden hlavní uzel.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-117">After cluster deployment, connect by Remote Desktop to one head node.</span></span>

  2. <span data-ttu-id="8d9dd-118">Nahrajte certifikát (ve formátu PFX s privátním klíčem) do každé hlavního uzlu a nainstalujte Cert: \LocalMachine\My a Cert: \LocalMachine\Root.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-118">Upload the certificate (PFX format with private key) to each head node and install to Cert:\LocalMachine\My and Cert:\LocalMachine\Root.</span></span>

  3. <span data-ttu-id="8d9dd-119">Otevřete prostředí Azure PowerShell jako správce a spusťte následující příkazy v jednom hlavního uzlu:</span><span class="sxs-lookup"><span data-stu-id="8d9dd-119">Start Azure PowerShell as an administrator and run the following commands on one head node:</span></span>

    ```powershell
        cd $env:CCP_HOME\bin

        Login-AzureRmAccount
    ```
        
    <span data-ttu-id="8d9dd-120">Pokud váš účet je ve více než jednoho klienta Azure Active Directory nebo předplatné Azure, můžete spustit následující příkaz pro vybrat správný klienta a předplatné:</span><span class="sxs-lookup"><span data-stu-id="8d9dd-120">If your account is in more than one Azure Active Directory tenant or Azure subscription, you can run the following command to select the correct tenant and subscription:</span></span>
  
    ```powershell
        Login-AzureRMAccount -TenantId <TenantId> -SubscriptionId <subscriptionId>
    ```     
       
    <span data-ttu-id="8d9dd-121">Spusťte následující příkaz k zobrazení aktuálně vybraného klienta a předplatného:</span><span class="sxs-lookup"><span data-stu-id="8d9dd-121">Run the following command to view the currently selected tenant and subscription:</span></span>
    
    ```powershell
        Get-AzureRMContext
    ```

  4. <span data-ttu-id="8d9dd-122">Spusťte následující skript</span><span class="sxs-lookup"><span data-stu-id="8d9dd-122">Run the following script</span></span>

    ```powershell
        .\ConfigARMAutoGrowShrinkCert.ps1 -DisplayName “YourHpcPackAppName” -HomePage "https://YourHpcPackAppHomePage" -IdentifierUri "https://YourHpcPackAppUri" -CertificateThumbprint "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" -TenantId xxxxxxxx-xxxxx-xxxxx-xxxxx-xxxxxxxxxxxx
    ```

    <span data-ttu-id="8d9dd-123">kde</span><span class="sxs-lookup"><span data-stu-id="8d9dd-123">where</span></span>

    <span data-ttu-id="8d9dd-124">**DisplayName** -zobrazovaný název aplikace Azure aktivní.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-124">**DisplayName** - Azure Active Application display name.</span></span> <span data-ttu-id="8d9dd-125">Pokud aplikace neexistuje, vytvoří se v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-125">If the application does not exist, it is created in Azure Active Directory.</span></span>

    <span data-ttu-id="8d9dd-126">**Domovská stránka** -domovské stránce aplikace.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-126">**HomePage** - The home page of the application.</span></span> <span data-ttu-id="8d9dd-127">Můžete nakonfigurovat fiktivní adresu URL, jako v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-127">You can configure a dummy URL, as in the preceding example.</span></span>

    <span data-ttu-id="8d9dd-128">**IdentifierUri** -identifikátor aplikace.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-128">**IdentifierUri** - Identifier of the application.</span></span> <span data-ttu-id="8d9dd-129">Můžete nakonfigurovat fiktivní adresu URL, jako v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-129">You can configure a dummy URL, as in the preceding example.</span></span>

    <span data-ttu-id="8d9dd-130">**CertificateThumbprint** -kryptografický otisk certifikátu, který jste nainstalovali z hlavního uzlu v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-130">**CertificateThumbprint** - Thumbprint of the certificate you installed on the head node in Step 1.</span></span>

    <span data-ttu-id="8d9dd-131">**TenantId** -ID služby Azure Active Directory klienta.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-131">**TenantId** - Tenant ID of your Azure Active Directory.</span></span> <span data-ttu-id="8d9dd-132">ID klienta můžete získat z portálu Azure Active Directory **vlastnosti** stránky.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-132">You can get the Tenant ID from the Azure Active Directory portal **Properties** page.</span></span>

    <span data-ttu-id="8d9dd-133">Další podrobnosti o **ConfigARMAutoGrowShrinkCert.ps1**spusťte `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-133">For more details about **ConfigARMAutoGrowShrinkCert.ps1**, run `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`.</span></span>


* <span data-ttu-id="8d9dd-134">**Pro cluster s hlavního uzlu v Azure (modelu nasazení classic)** – Pokud používáte skript nasazení HPC Pack IaaS k vytvoření clusteru v modelu nasazení classic, povolte **AutoGrowShrink** vlastnost pomocí clusteru nastavení možnosti AutoGrowShrink v konfiguračním souboru na clusteru.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-134">**For a cluster with a head node in Azure (classic deployment model)** - If you use the HPC Pack IaaS deployment script to create the cluster in the classic deployment model, enable the **AutoGrowShrink** cluster property by setting the AutoGrowShrink option in the cluster configuration file.</span></span> <span data-ttu-id="8d9dd-135">Podrobnosti najdete v tématu doprovodné dokumentaci [stahování skriptu](https://www.microsoft.com/download/details.aspx?id=44949).</span><span class="sxs-lookup"><span data-stu-id="8d9dd-135">For details, see the documentation accompanying the [script download](https://www.microsoft.com/download/details.aspx?id=44949).</span></span>

    <span data-ttu-id="8d9dd-136">Můžete taky povolit **AutoGrowShrink** clusteru vlastnost po nasazení clusteru pomocí příkazů prostředí HPC PowerShell popsaných v následující části.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-136">Alternatively, enable the **AutoGrowShrink** cluster property after you deploy the cluster by using HPC PowerShell commands described in the following section.</span></span> <span data-ttu-id="8d9dd-137">Příprava na to, proveďte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8d9dd-137">To prepare for this, first complete the following steps:</span></span>

  1. <span data-ttu-id="8d9dd-138">Nakonfigurujte certifikát pro správu Azure z hlavního uzlu a v rámci předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-138">Configure an Azure management certificate on the head node and in the Azure subscription.</span></span> <span data-ttu-id="8d9dd-139">Pro testovací nasazení můžete použít výchozí Microsoft HPC Azure certifikát podepsaný svým držitelem, který nainstaluje sady HPC Pack hlavního uzlu a pak nahrajte tento certifikát do vašeho předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-139">For a test deployment, you can use the Default Microsoft HPC Azure self-signed certificate that HPC Pack installs on the head node, and then upload that certificate to your Azure subscription.</span></span> <span data-ttu-id="8d9dd-140">Možnosti a kroky najdete v tématu [pokyny v knihovně TechNet](https://technet.microsoft.com/library/gg481759.aspx).</span><span class="sxs-lookup"><span data-stu-id="8d9dd-140">For options and steps, see the [TechNet Library guidance](https://technet.microsoft.com/library/gg481759.aspx).</span></span>

  2. <span data-ttu-id="8d9dd-141">Spustit **regedit** z hlavního uzlu, přejděte na HKLM\SOFTWARE\Micorsoft\HPC\IaasInfo a přidejte hodnotu řetězce.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-141">Run **regedit** on the head node, go to HKLM\SOFTWARE\Micorsoft\HPC\IaasInfo, and add a string value.</span></span> <span data-ttu-id="8d9dd-142">Nastavte název hodnoty na "Kryptografický otisk" a hodnota dat kryptografický otisk certifikátu v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-142">Set the Value name to “ThumbPrint”, and Value data to the thumbprint of the certificate in Step 1.</span></span>

### <a name="hpc-powershell-commands-to-set-the-autogrowshrink-property"></a><span data-ttu-id="8d9dd-143">Příkazy prostředí HPC PowerShell nastavit vlastnost AutoGrowShrink</span><span class="sxs-lookup"><span data-stu-id="8d9dd-143">HPC PowerShell commands to set the AutoGrowShrink property</span></span>
<span data-ttu-id="8d9dd-144">Toto jsou příkazy prostředí HPC PowerShell ukázkový nastavit **AutoGrowShrink** a optimalizaci své chování s další parametry.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-144">Following are sample HPC PowerShell commands to set **AutoGrowShrink** and to tune its behavior with additional parameters.</span></span> <span data-ttu-id="8d9dd-145">V tématu [AutoGrowShrink parametry](#AutoGrowShrink-parameters) dále v tomto článku pro úplný seznam nastavení.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-145">See [AutoGrowShrink parameters](#AutoGrowShrink-parameters) later in this article for the complete list of settings.</span></span>

<span data-ttu-id="8d9dd-146">Ke spuštění těchto příkazů, spusťte z hlavního uzlu clusteru prostředí HPC PowerShell jako správce.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-146">To run these commands, start HPC PowerShell on the cluster head node as an administrator.</span></span>

<span data-ttu-id="8d9dd-147">**Chcete-li povolit vlastnost AutoGrowShrink**</span><span class="sxs-lookup"><span data-stu-id="8d9dd-147">**To enable the AutoGrowShrink property**</span></span>

```powershell
Set-HpcClusterProperty –EnableGrowShrink 1
```

<span data-ttu-id="8d9dd-148">**Chcete-li zakázat vlastnost AutoGrowShrink**</span><span class="sxs-lookup"><span data-stu-id="8d9dd-148">**To disable the AutoGrowShrink property**</span></span>

```powershell
Set-HpcClusterProperty –EnableGrowShrink 0
```

<span data-ttu-id="8d9dd-149">**Chcete-li změnit zvětšit interval v minutách**</span><span class="sxs-lookup"><span data-stu-id="8d9dd-149">**To change the grow interval in minutes**</span></span>

```powershell
Set-HpcClusterProperty –GrowInterval <interval>
```

<span data-ttu-id="8d9dd-150">**Chcete-li změnit zmenšení interval v minutách**</span><span class="sxs-lookup"><span data-stu-id="8d9dd-150">**To change the shrink interval in minutes**</span></span>

```powershell
Set-HpcClusterProperty –ShrinkInterval <interval>
```

<span data-ttu-id="8d9dd-151">**Chcete-li zobrazit aktuální konfiguraci AutoGrowShrink**</span><span class="sxs-lookup"><span data-stu-id="8d9dd-151">**To view the current configuration of AutoGrowShrink**</span></span>

```powershell
Get-HpcClusterProperty –AutoGrowShrink
```

<span data-ttu-id="8d9dd-152">**Chcete vyloučit z AutoGrowShrink uzel skupiny**</span><span class="sxs-lookup"><span data-stu-id="8d9dd-152">**To exclude node groups from AutoGrowShrink**</span></span>

```powershell
Set-HpcClusterProperty –ExcludeNodeGroups <group1,group2,group3>
```

>[!NOTE] 
> <span data-ttu-id="8d9dd-153">Tento parametr je podporované počínaje HPC Pack 2016</span><span class="sxs-lookup"><span data-stu-id="8d9dd-153">This parameter is supported starting in HPC Pack 2016</span></span>
>

### <a name="autogrowshrink-parameters"></a><span data-ttu-id="8d9dd-154">Parametry AutoGrowShrink</span><span class="sxs-lookup"><span data-stu-id="8d9dd-154">AutoGrowShrink parameters</span></span>
<span data-ttu-id="8d9dd-155">Toto jsou AutoGrowShrink parametry, které lze upravit pomocí **Set-HpcClusterProperty** příkaz.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-155">The following are AutoGrowShrink parameters that you can modify by using the **Set-HpcClusterProperty** command.</span></span>

* <span data-ttu-id="8d9dd-156">**EnableGrowShrink** -přepínače, které chcete povolit nebo zakázat **AutoGrowShrink** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-156">**EnableGrowShrink** - Switch to enable or disable the **AutoGrowShrink** property.</span></span>
* <span data-ttu-id="8d9dd-157">**ParamSweepTasksPerCore** -počtu úkolů čištění parametrů růst jednoho jádra.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-157">**ParamSweepTasksPerCore** - Number of parametric sweep tasks to grow one core.</span></span> <span data-ttu-id="8d9dd-158">Ve výchozím nastavení se růst jednoho jádra daná úloha.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-158">The default is to grow one core per task.</span></span>

  > [!NOTE]
  > <span data-ttu-id="8d9dd-159">Změny HPC Pack QFE KB3134307 **ParamSweepTasksPerCore** k **TasksPerResourceUnit**.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-159">HPC Pack QFE KB3134307 changes **ParamSweepTasksPerCore** to **TasksPerResourceUnit**.</span></span> <span data-ttu-id="8d9dd-160">Ho je založený na typu prostředku úlohy a může být uzlu, soketu nebo jádra.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-160">It is based on the job resource type and can be node, socket, or core.</span></span>
  >
  >
* <span data-ttu-id="8d9dd-161">**GrowThreshold** – prahová hodnota zařazených do fronty úloh pro spuštění automatického zvětšování.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-161">**GrowThreshold** - Threshold of queued tasks to trigger automatic growth.</span></span> <span data-ttu-id="8d9dd-162">Výchozí hodnota je 1, což znamená, že, pokud jsou ve stavu zařazených do fronty, 1 nebo více úloh, automaticky rozšiřovat uzly.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-162">The default is 1, which means that if there are 1 or more tasks in the queued state, automatically grow nodes.</span></span>
* <span data-ttu-id="8d9dd-163">**GrowInterval** – Interval v minutách pro spuštění automatického zvětšování.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-163">**GrowInterval** - Interval in minutes to trigger automatic growth.</span></span> <span data-ttu-id="8d9dd-164">Výchozí interval je 5 minut.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-164">The default interval is 5 minutes.</span></span>
* <span data-ttu-id="8d9dd-165">**ShrinkInterval** – Interval v minutách pro spuštění automatického zmenšit.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-165">**ShrinkInterval** - Interval in minutes to trigger automatic shrinking.</span></span> <span data-ttu-id="8d9dd-166">Výchozí interval je 5 minut. |</span><span class="sxs-lookup"><span data-stu-id="8d9dd-166">The default interval is 5 minutes.|</span></span>
* <span data-ttu-id="8d9dd-167">**ShrinkIdleTimes** -počet průběžné kontroly zmenšení udávajících uzly jsou nečinnosti.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-167">**ShrinkIdleTimes** - Number of continuous checks to shrink to indicate the nodes are idle.</span></span> <span data-ttu-id="8d9dd-168">Výchozí hodnota je 3 x.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-168">The default is 3 times.</span></span> <span data-ttu-id="8d9dd-169">Například pokud **ShrinkInterval** je 5 minut, HPC Pack zkontroluje, zda je uzel nečinnosti každých 5 minut.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-169">For example, if the **ShrinkInterval** is 5 minutes, HPC Pack checks whether the node is idle every 5 minutes.</span></span> <span data-ttu-id="8d9dd-170">Pokud uzly jsou ve stavu nečinnosti, po 3 průběžné kontroluje (15 minut), HPC Pack zmenší tento uzel.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-170">If the nodes are in the idle state after 3 continuous checks (15 minutes), then HPC Pack shrinks that node.</span></span>
* <span data-ttu-id="8d9dd-171">**ExtraNodesGrowRatio** -další procento uzly růst pro úlohy Message Passing Interface (MPI).</span><span class="sxs-lookup"><span data-stu-id="8d9dd-171">**ExtraNodesGrowRatio** - Additional percentage of nodes to grow for Message Passing Interface (MPI) jobs.</span></span> <span data-ttu-id="8d9dd-172">Výchozí hodnota je 1, což znamená zvětšování HPC Pack uzlů pro úlohy MPI 1 %.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-172">The default value is 1, which means that HPC Pack grows nodes 1% for MPI jobs.</span></span>
* <span data-ttu-id="8d9dd-173">**GrowByMin** – přepínač indikující, zda autogrow zásad je založena na minimální prostředky potřebné pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-173">**GrowByMin** - Switch to indicate whether the autogrow policy is based on the minimum resources required for the job.</span></span> <span data-ttu-id="8d9dd-174">Výchozí hodnota je nastavena hodnota false, což znamená, že HPC Pack zvětšování uzlů pro úlohy, které jsou založeny na maximální prostředky potřebné pro úlohy.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-174">The default is false, which means that HPC Pack grows nodes for jobs based on the maximum resources required for the jobs.</span></span>
* <span data-ttu-id="8d9dd-175">**SoaJobGrowThreshold** -růst prahovou hodnotu příchozí požadavky SOA pro spuštění automatického procesu.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-175">**SoaJobGrowThreshold** - Threshold of incoming SOA requests to trigger the automatic grow process.</span></span> <span data-ttu-id="8d9dd-176">Výchozí hodnota je 50000.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-176">The default value is 50000.</span></span>

  > [!NOTE]
  > <span data-ttu-id="8d9dd-177">Tento parametr je podporované počínaje HPC Pack 2012 R2 Update 3.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-177">This parameter is supported starting in HPC Pack 2012 R2 Update 3.</span></span>
  >
  >
* <span data-ttu-id="8d9dd-178">**SoaRequestsPerCore** -počet příchozích SOA požadavků růst jednoho jádra.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-178">**SoaRequestsPerCore** -Number of incoming SOA requests to grow one core.</span></span> <span data-ttu-id="8d9dd-179">Výchozí hodnota je 20000.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-179">The default value is 20000.</span></span>

  > [!NOTE]
  > <span data-ttu-id="8d9dd-180">Tento parametr je podporované počínaje HPC Pack 2012 R2 Update 3.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-180">This parameter is supported starting in HPC Pack 2012 R2 Update 3.</span></span>
  >
* <span data-ttu-id="8d9dd-181">**ExcludeNodeGroups** – uzly ve skupině zadaný uzel automaticky zvýšit nebo snížit.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-181">**ExcludeNodeGroups** – Nodes in the specified node groups do not automatically grow and shrink.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="8d9dd-182">Tento parametr je podporované počínaje HPC Pack 2016.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-182">This parameter is supported starting in HPC Pack 2016.</span></span>
  >

### <a name="mpi-example"></a><span data-ttu-id="8d9dd-183">Příklad MPI</span><span class="sxs-lookup"><span data-stu-id="8d9dd-183">MPI example</span></span>
<span data-ttu-id="8d9dd-184">Ve výchozím nastavení HPC Pack zvětšování 1 % navíc uzlů pro úlohy MPI (**ExtraNodesGrowRatio** je nastaven na hodnotu 1).</span><span class="sxs-lookup"><span data-stu-id="8d9dd-184">By default HPC Pack grows 1% extra nodes for MPI jobs (**ExtraNodesGrowRatio** is set to 1).</span></span> <span data-ttu-id="8d9dd-185">Důvodem je, že MPI může vyžadovat více uzlů, a úlohu lze spustit pouze když jsou všechny uzly připravené.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-185">The reason is that MPI may require multiple nodes, and the job can only run when all nodes are ready.</span></span> <span data-ttu-id="8d9dd-186">Při spuštění uzlů Azure příležitostně jeden uzel může potřebujete víc času spuštění než jiné způsobuje další uzly na nečinnost při čekání na tento uzel připravit.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-186">When Azure starts nodes, occasionally one node might need more time to start than others, causing other nodes to be idle while waiting for that node to get ready.</span></span> <span data-ttu-id="8d9dd-187">Způsobeného nárůstem další uzly, HPC Pack snižuje dobu čekání na tento prostředek a potenciálně šetří náklady.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-187">By growing extra nodes, HPC Pack reduces this resource waiting time, and potentially saves costs.</span></span> <span data-ttu-id="8d9dd-188">Chcete-li zvyšovat procento navíc uzlů pro úlohy MPI (například na 10 %), spusťte příkaz podobný</span><span class="sxs-lookup"><span data-stu-id="8d9dd-188">To increase the percentage of extra nodes for MPI jobs (for example, to 10%), run a command similar to</span></span>

    Set-HpcClusterProperty -ExtraNodesGrowRatio 10

### <a name="soa-example"></a><span data-ttu-id="8d9dd-189">Příklad SOA</span><span class="sxs-lookup"><span data-stu-id="8d9dd-189">SOA example</span></span>
<span data-ttu-id="8d9dd-190">Ve výchozím nastavení **SoaJobGrowThreshold** je nastaven na 50000 a **SoaRequestsPerCore** je nastaven na 200000.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-190">By default, **SoaJobGrowThreshold** is set to 50000 and **SoaRequestsPerCore** is set to 200000.</span></span> <span data-ttu-id="8d9dd-191">Pokud odešlete jeden úlohy architektury SOA s 70000 požadavky, je ve frontě úloh a 70000 jsou příchozí požadavky.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-191">If you submit one SOA job with 70000 requests, there is one queued task and incoming requests are 70000.</span></span> <span data-ttu-id="8d9dd-192">V takovém případě HPC Pack zvětšování 1 jádro pro úlohu ve frontě a pro příchozí požadavky, zvětšování (70000-50000) nebo základní 20000 = 1, takže v celkem zvětšování 2 jádra pro tuto úlohu SOA.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-192">In this case HPC Pack grows 1 core for the queued task, and for incoming requests, grows (70000 - 50000)/20000 = 1 core, so in total grows 2 cores for this SOA job.</span></span>

## <a name="run-the-azureautogrowshrinkps1-script"></a><span data-ttu-id="8d9dd-193">Spusťte skript AzureAutoGrowShrink.ps1</span><span class="sxs-lookup"><span data-stu-id="8d9dd-193">Run the AzureAutoGrowShrink.ps1 script</span></span>
### <a name="prerequisites"></a><span data-ttu-id="8d9dd-194">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8d9dd-194">Prerequisites</span></span>

* <span data-ttu-id="8d9dd-195">**HPC Pack 2012 R2 Update 1 nebo novější clusteru** – **AzureAutoGrowShrink.ps1** skriptu je nainstalován ve složce % CCP_HOME % Koš.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-195">**HPC Pack 2012 R2 Update 1 or later cluster** - The **AzureAutoGrowShrink.ps1** script is installed in the %CCP_HOME%bin folder.</span></span> <span data-ttu-id="8d9dd-196">Z hlavního uzlu clusteru může být nasazena místně nebo ve virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-196">The cluster head node can be deployed either on-premises or in an Azure VM.</span></span> <span data-ttu-id="8d9dd-197">V tématu [nastavení hybridního clusteru pomocí sady HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) začít pracovat se hlavnímu uzlu místní a uzly Azure "shluků".</span><span class="sxs-lookup"><span data-stu-id="8d9dd-197">See [Set up a hybrid cluster with HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) to get started with an on-premises head node and Azure "burst" nodes.</span></span> <span data-ttu-id="8d9dd-198">Najdete v článku [skript nasazení HPC Pack IaaS](hpcpack-cluster-powershell-script.md) rychle nasazení clusteru HPC Pack ve virtuálních počítačích Azure nebo použijte [šablony Azure rychlý Start](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/).</span><span class="sxs-lookup"><span data-stu-id="8d9dd-198">See the [HPC Pack IaaS deployment script](hpcpack-cluster-powershell-script.md) to quickly deploy an HPC Pack cluster in Azure VMs, or use an [Azure quickstart template](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/).</span></span>
* <span data-ttu-id="8d9dd-199">**Prostředí Azure PowerShell 1.4.0** -skript aktuálně závisí na konkrétní verzi prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-199">**Azure PowerShell 1.4.0** - The script currently depends on this specific version of Azure PowerShell.</span></span>
* <span data-ttu-id="8d9dd-200">**Pro cluster s Azure burst uzly** – spusťte tento skript na klientském počítači, kde je nainstalován HPC Pack nebo z hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-200">**For a cluster with Azure burst nodes** - Run the script on a client computer where HPC Pack is installed, or on the head node.</span></span> <span data-ttu-id="8d9dd-201">Pokud běží na klientském počítači, zajistěte, že nastavíte proměnnou $env: CCP_SCHEDULER tak, aby odkazoval k hlavnímu uzlu.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-201">If running on a client computer, ensure that you set the variable $env:CCP_SCHEDULER to point to the head node.</span></span> <span data-ttu-id="8d9dd-202">Uzly Azure "shluků" musí být přidaný do clusteru, ale může být ve stavu není nasazen.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-202">The Azure “burst” nodes must be added to the cluster, but they may be in the Not-Deployed state.</span></span>
* <span data-ttu-id="8d9dd-203">**Pro cluster s podporou nasazené ve virtuálních počítačích Azure (modelu nasazení Resource Manager)** -pro cluster s podporou virtuálních počítačů Azure nasazené v modelu nasazení Resource Manager, skript podporuje dvě metody pro ověřování Azure: přihlásit k účtu Azure a spustit skript pokaždé, když (spuštěním `Login-AzureRmAccount`, nebo nakonfigurovat hlavní název služby k ověřování pomocí certifikátu.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-203">**For a cluster deployed in Azure VMs (Resource Manager deployment model)** - For a cluster of Azure VMs deployed in the Resource Manager deployment model, the script supports two methods for Azure authentication: sign in to your Azure account to run the script every time (by running `Login-AzureRmAccount`, or configure a service principal to authenticate with a certificate.</span></span> <span data-ttu-id="8d9dd-204">HPC Pack poskytne skript **ConfigARMAutoGrowShrinkCert.ps** k vytvoření objektu služby pomocí certifikátu.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-204">HPC Pack provides the script **ConfigARMAutoGrowShrinkCert.ps** to create a service principal with certificate.</span></span> <span data-ttu-id="8d9dd-205">Skript vytvoří aplikaci služby Azure Active Directory (Azure AD) a hlavní název služby a přiřadí role Přispěvatel instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-205">The script creates an Azure Active Directory (Azure AD) application and a service principal, and assigns the Contributor role to the service principal.</span></span> <span data-ttu-id="8d9dd-206">Chcete-li spustit skript, otevřete prostředí Azure PowerShell jako správce a spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="8d9dd-206">To run the script, start Azure PowerShell  as administrator and run the following commands:</span></span>

    ```powershell
    cd $env:CCP_HOME\bin

    Login-AzureRmAccount

    .\ConfigARMAutoGrowShrinkCert.ps1 -DisplayName “YourHpcPackAppName” -HomePage "https://YourHpcPackAppHomePage" -IdentifierUri "https://YourHpcPackAppUri" -PfxFile "d:\yourcertificate.pfx"
    ```

    <span data-ttu-id="8d9dd-207">Další podrobnosti o **ConfigARMAutoGrowShrinkCert.ps1**spusťte `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`,</span><span class="sxs-lookup"><span data-stu-id="8d9dd-207">For more details about **ConfigARMAutoGrowShrinkCert.ps1**, run `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`,</span></span>

* <span data-ttu-id="8d9dd-208">**Pro cluster s podporou nasazené ve virtuálních počítačích Azure (modelu nasazení classic)** -spuštění skriptu na hlavního uzlu virtuálního počítače, protože závisí na **Start-HpcIaaSNode.ps1** a **Stop-HpcIaaSNode.ps1** skripty, které jsou nainstalovány existuje.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-208">**For a cluster deployed in Azure VMs (classic deployment model)** - Run the script on the head node VM, because it depends on the **Start-HpcIaaSNode.ps1** and **Stop-HpcIaaSNode.ps1** scripts that are installed there.</span></span> <span data-ttu-id="8d9dd-209">Tyto skripty navíc vyžadují certifikát pro správu Azure nebo soubor nastavení publikování (viz [spravovat výpočetní uzly v prostředí HPC Pack clusteru v Azure](hpcpack-cluster-node-manage.md)).</span><span class="sxs-lookup"><span data-stu-id="8d9dd-209">Those scripts additionally require an Azure management certificate or publish settings file (see [Manage compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster-node-manage.md)).</span></span> <span data-ttu-id="8d9dd-210">Zajistěte, aby všechny výpočetním uzlu je třeba virtuální počítače jsou již přidán do clusteru.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-210">Make sure all the compute node VMs you need are already added to the cluster.</span></span> <span data-ttu-id="8d9dd-211">Mohou být v zastaveném stavu.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-211">They may be in the Stopped state.</span></span>



### <a name="syntax"></a><span data-ttu-id="8d9dd-212">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="8d9dd-212">Syntax</span></span>
```powershell
AzureAutoGrowShrink.ps1 [-NodeTemplates <String[]>] [-JobTemplates <String[]>] [-NodeType <String>]
    -NumOfActiveQueuedTasksPerNodeToGrow <Single> [-NumOfActiveQueuedTasksToGrowThreshold <Int32>]
    [-NumOfInitialNodesToGrow <Int32>] [-GrowCheckIntervalMins <Int32>] [-ShrinkCheckIntervalMins <Int32>]
    [-ShrinkCheckIdleTimes <Int32>] [-ExtraNodesGrowRatio <Int32>] [-ArgFile <String>] [-LogFilePrefix <String>]
    [<CommonParameters>]

AzureAutoGrowShrink.ps1 [-NodeTemplates <String[]>] [-JobTemplates <String[]>] [-NodeType <String>]
    -NumOfQueuedJobsPerNodeToGrow <Single> [-NumOfQueuedJobsToGrowThreshold <Int32>] [-NumOfInitialNodesToGrow
    <Int32>] [-GrowCheckIntervalMins <Int32>] [-ShrinkCheckIntervalMins <Int32>] [-ShrinkCheckIdleTimes <Int32>]
    [-ExtraNodesGrowRatio <Int32>] [-ArgFile <String>] [-LogFilePrefix <String>] [<CommonParameters>]

AzureAutoGrowShrink.ps1 -UseLastConfigurations [-ArgFile <String>] [-LogFilePrefix <String>] [<CommonParameters>]
```
### <a name="parameters"></a><span data-ttu-id="8d9dd-213">Parametry</span><span class="sxs-lookup"><span data-stu-id="8d9dd-213">Parameters</span></span>
* <span data-ttu-id="8d9dd-214">**NodeTemplates** -názvy šablon uzel k definování oboru pro uzly zvětšovat a zmenšovat.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-214">**NodeTemplates** - Names of the node templates to define the scope for the nodes to grow and shrink.</span></span> <span data-ttu-id="8d9dd-215">Není-li zadána (výchozí hodnota je @()), všechny uzly v **AzureNodes** uzlu skupiny jsou v oboru při **NodeType** má hodnotu AzureNodes a všech uzlech v **ComputeNodes**uzlu skupiny jsou v oboru při **NodeType** má hodnotu ComputeNodes.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-215">If not specified (the default value is @()), all nodes in the **AzureNodes** node group are in scope when **NodeType** has a value of AzureNodes, and all nodes in the **ComputeNodes** node group are in scope when **NodeType** has a value of ComputeNodes.</span></span>
* <span data-ttu-id="8d9dd-216">**JobTemplates** -názvy šablon úlohy k definování oboru pro uzly zvětšovat.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-216">**JobTemplates** - Names of the job templates to define the scope for the nodes to grow.</span></span>
* <span data-ttu-id="8d9dd-217">**Typ uzlu** – typ uzlu zvětšovat a zmenšovat.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-217">**NodeType** - The type of node to grow and shrink.</span></span> <span data-ttu-id="8d9dd-218">Podporované hodnoty jsou:</span><span class="sxs-lookup"><span data-stu-id="8d9dd-218">Supported values are:</span></span>

  * <span data-ttu-id="8d9dd-219">**AzureNodes** – u uzlů Azure PaaS (shluků) v místní nebo v clusteru Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-219">**AzureNodes** – for Azure PaaS (burst) nodes in an on-premises or Azure IaaS cluster.</span></span>
  * <span data-ttu-id="8d9dd-220">**ComputeNodes** – pro výpočetní uzel virtuální počítače pouze v clusteru služby Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-220">**ComputeNodes** - for compute node VMs only in an Azure IaaS cluster.</span></span>

* <span data-ttu-id="8d9dd-221">**NumOfQueuedJobsPerNodeToGrow** -počet úloh zařazených do fronty, které jsou potřeba růst jeden uzel.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-221">**NumOfQueuedJobsPerNodeToGrow** - Number of queued jobs required to grow one node.</span></span>
* <span data-ttu-id="8d9dd-222">**NumOfQueuedJobsToGrowThreshold** – prahová hodnota počtu úloh zařazených do fronty ke spuštění procesu zvětšit.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-222">**NumOfQueuedJobsToGrowThreshold** - The threshold number of queued jobs to start the grow process.</span></span>
* <span data-ttu-id="8d9dd-223">**NumOfActiveQueuedTasksPerNodeToGrow** – počet aktivních zařazených do fronty úkoly vyžadované ke zvětšení jeden uzel.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-223">**NumOfActiveQueuedTasksPerNodeToGrow** - The number of active queued tasks required to grow one node.</span></span> <span data-ttu-id="8d9dd-224">Pokud **NumOfQueuedJobsPerNodeToGrow** je zadán s hodnotou větší než 0, tento parametr je ignorován.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-224">If **NumOfQueuedJobsPerNodeToGrow** is specified with a value greater than 0, this parameter is ignored.</span></span>
* <span data-ttu-id="8d9dd-225">**NumOfActiveQueuedTasksToGrowThreshold** – prahová hodnota počtu aktivních úloh zařazených do fronty ke spuštění procesu zvětšit.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-225">**NumOfActiveQueuedTasksToGrowThreshold** - The threshold number of active queued tasks to start the grow process.</span></span>
* <span data-ttu-id="8d9dd-226">**NumOfInitialNodesToGrow** – počáteční minimální počet uzlů roste s tím, pokud jsou všechny uzly v oboru **není nasazena** nebo **zastaveno (Deallocated)**.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-226">**NumOfInitialNodesToGrow** - The initial minimum number of nodes to grow if all the nodes in scope are **Not-Deployed** or **Stopped (Deallocated)**.</span></span>
* <span data-ttu-id="8d9dd-227">**GrowCheckIntervalMins** – interval v minutách mezi kontrolami zvětšovat.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-227">**GrowCheckIntervalMins** - The interval in minutes between checks to grow.</span></span>
* <span data-ttu-id="8d9dd-228">**ShrinkCheckIntervalMins** – interval v minutách mezi kontrolami zmenšení.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-228">**ShrinkCheckIntervalMins** - The interval in minutes between checks to shrink.</span></span>
* <span data-ttu-id="8d9dd-229">**ShrinkCheckIdleTimes** -počet průběžné zmenšit kontroly (oddělených **ShrinkCheckIntervalMins**) k označení uzly jsou nečinnosti.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-229">**ShrinkCheckIdleTimes** - The number of continuous shrink checks (separated by **ShrinkCheckIntervalMins**) to indicate the nodes are idle.</span></span>
* <span data-ttu-id="8d9dd-230">**UseLastConfigurations** -předchozí konfigurace uložené v souboru argument.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-230">**UseLastConfigurations** - The previous configurations saved in the argument file.</span></span>
* <span data-ttu-id="8d9dd-231">**ArgFile**– název souboru argument používá k uložení a aktualizovat konfiguraci pro spuštění skriptu.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-231">**ArgFile**- The name of the argument file used to save and update the configurations to run the script.</span></span>
* <span data-ttu-id="8d9dd-232">**LogFilePrefix** – předpona názvu souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-232">**LogFilePrefix** - The prefix name of the log file.</span></span> <span data-ttu-id="8d9dd-233">Můžete zadat cestu.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-233">You can specify a path.</span></span> <span data-ttu-id="8d9dd-234">Ve výchozím nastavení protokol je zapsán do aktuální pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-234">By default the log is written to the current working directory.</span></span>

### <a name="example-1"></a><span data-ttu-id="8d9dd-235">Příklad 1</span><span class="sxs-lookup"><span data-stu-id="8d9dd-235">Example 1</span></span>
<span data-ttu-id="8d9dd-236">Následující příklad konfiguruje uzlů Azure shluků nasazené pomocí výchozí šablony AzureNode zvětšovat a zmenšovat automaticky.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-236">The following example configures the Azure burst nodes deployed with the Default AzureNode Template to grow and shrink automatically.</span></span> <span data-ttu-id="8d9dd-237">Pokud jsou všechny uzly v původně **není nasazena** stavu, jsou spuštěny aspoň 3 uzly.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-237">If all the nodes are initially in the **Not-Deployed** state, at least 3 nodes are started.</span></span> <span data-ttu-id="8d9dd-238">Pokud počet úloh zařazených do fronty překračuje 8, skript spustí uzlů, dokud se jejich počet překračuje poměr zařazených do fronty úloh **NumOfQueuedJobsPerNodeToGrow**.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-238">If the number of queued jobs exceeds 8, the script starts nodes until their number exceeds the ratio of queued jobs to **NumOfQueuedJobsPerNodeToGrow**.</span></span> <span data-ttu-id="8d9dd-239">Pokud je zjištěno nečinnosti 3 po sobě jdoucích nečinnosti časů uzlu, je zastavena.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-239">If a node is found to be idle in 3 consecutive idle times, it is stopped.</span></span>

```powershell
.\AzureAutoGrowShrink.ps1 -NodeTemplates @('Default AzureNode
 Template') -NodeType AzureNodes -NumOfQueuedJobsPerNodeToGrow 5
 -NumOfQueuedJobsToGrowThreshold 8 -NumOfInitialNodesToGrow 3
 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 3
```

### <a name="example-2"></a><span data-ttu-id="8d9dd-240">Příklad 2</span><span class="sxs-lookup"><span data-stu-id="8d9dd-240">Example 2</span></span>
<span data-ttu-id="8d9dd-241">Následující příklad konfiguruje Azure výpočetním uzlu virtuální počítače nasazené pomocí výchozí šablony ComputeNode zvětšovat a zmenšovat automaticky.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-241">The following example configures the Azure compute node VMs deployed with the Default ComputeNode Template to grow and shrink automatically.</span></span>
<span data-ttu-id="8d9dd-242">Úlohy, konfigurovat tak, že výchozí šablony úlohy definovat rozsah zatížení v clusteru.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-242">The jobs configured by the Default job template define the scope of the workload on the cluster.</span></span> <span data-ttu-id="8d9dd-243">Pokud jsou všechny uzly původně zastavena, nejméně 5 uzly jsou spuštěny.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-243">If all the nodes are initially stopped, at least 5 nodes are started.</span></span> <span data-ttu-id="8d9dd-244">Pokud počet aktivních úloh zařazených do fronty překročí 15, skript spustí uzlů, dokud se jejich počet překračuje poměr aktivních úloh zařazených do fronty pro **NumOfActiveQueuedTasksPerNodeToGrow**.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-244">If the number of active queued tasks exceeds 15, the script starts nodes until their number exceeds the ratio of active queued tasks to **NumOfActiveQueuedTasksPerNodeToGrow**.</span></span> <span data-ttu-id="8d9dd-245">Pokud je zjištěno nečinnosti v 10krát po sobě jdoucích nečinnosti uzlu, je zastavena.</span><span class="sxs-lookup"><span data-stu-id="8d9dd-245">If a node is found to be idle in 10 consecutive idle times, it is stopped.</span></span>

```powershell
.\AzureAutoGrowShrink.ps1 -NodeTemplates 'Default ComputeNode Template' -JobTemplates 'Default' -NodeType ComputeNodes -NumOfActiveQueuedTasksPerNodeToGrow 10 -NumOfActiveQueuedTasksToGrowThreshold 15 -NumOfInitialNodesToGrow 5 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 10 -ArgFile 'IaaSVMComputeNodes_Arg.xml' -LogFilePrefix 'IaaSVMComputeNodes_log'
```
