---
title: uzly clusteru HPC Pack aaaAutoscale | Microsoft Docs
description: "Automaticky zvětšovat a zmenšovat hello počet výpočetních uzlů clusteru HPC Pack v Azure"
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
ms.openlocfilehash: 0bdf55625d337a2bbfe05677682d645a584798d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automatically-grow-and-shrink-hello-hpc-pack-cluster-resources-in-azure-according-toohello-cluster-workload"></a><span data-ttu-id="8d26b-103">Automaticky zvětšovat a zmenšovat prostředky clusteru HPC Pack hello v Azure podle zatížení clusteru toohello</span><span class="sxs-lookup"><span data-stu-id="8d26b-103">Automatically grow and shrink hello HPC Pack cluster resources in Azure according toohello cluster workload</span></span>
<span data-ttu-id="8d26b-104">Pokud nasazujete Azure "shluků" uzly v clusteru HPC Pack nebo vytváření clusteru HPC Pack ve virtuálních počítačích Azure, můžete způsob, jak automaticky zvětšovat a zmenšovat hello prostředky clusteru jako uzly nebo jader podle hello zatížení v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="8d26b-104">If you deploy Azure “burst” nodes in your HPC Pack cluster, or you create an HPC Pack cluster in Azure VMs, you may want a way to automatically grow or shrink hello cluster resources such as nodes or cores according to hello workload on hello cluster.</span></span> <span data-ttu-id="8d26b-105">Škálování prostředky clusteru hello tímto způsobem umožňuje toouse vašich prostředků Azure efektivněji a kontrolovat náklady.</span><span class="sxs-lookup"><span data-stu-id="8d26b-105">Scaling hello cluster resources in this way allows you toouse your Azure resources more efficiently and control their costs.</span></span>

<span data-ttu-id="8d26b-106">Tento článek ukazuje dva způsoby, jak HPC Pack poskytuje tooautoscale výpočetní prostředky:</span><span class="sxs-lookup"><span data-stu-id="8d26b-106">This article shows you two ways that HPC Pack provides tooautoscale compute resources:</span></span>

* <span data-ttu-id="8d26b-107">Hello vlastnost clusteru HPC Pack **AutoGrowShrink**</span><span class="sxs-lookup"><span data-stu-id="8d26b-107">hello HPC Pack cluster property **AutoGrowShrink**</span></span>

* <span data-ttu-id="8d26b-108">Hello **AzureAutoGrowShrink.ps1** skript prostředí PowerShell HPC</span><span class="sxs-lookup"><span data-stu-id="8d26b-108">hello **AzureAutoGrowShrink.ps1** HPC PowerShell script</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="8d26b-109">Aktuálně lze pouze automaticky zvýšit nebo snížit HPC Pack výpočetní uzly, které je spuštěn operační systém Windows Server.</span><span class="sxs-lookup"><span data-stu-id="8d26b-109">Currently you can only automatically grow and shrink HPC Pack compute nodes that are running a Windows Server operating system.</span></span>


## <a name="set-hello-autogrowshrink-cluster-property"></a><span data-ttu-id="8d26b-110">Nastavte vlastnost clusteru AutoGrowShrink hello</span><span class="sxs-lookup"><span data-stu-id="8d26b-110">Set hello AutoGrowShrink cluster property</span></span>
### <a name="prerequisites"></a><span data-ttu-id="8d26b-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8d26b-111">Prerequisites</span></span>

* <span data-ttu-id="8d26b-112">**HPC Pack 2012 R2 Update 2 nebo novější clusteru** -hello hlavního uzlu clusteru může být nasazena místně nebo ve virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="8d26b-112">**HPC Pack 2012 R2 Update 2 or later cluster** - hello cluster head node can be deployed either on-premises or in an Azure VM.</span></span> <span data-ttu-id="8d26b-113">V tématu [nastavení hybridního clusteru pomocí sady HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) tooget začít s hlavnímu uzlu místní a uzly Azure "shluků".</span><span class="sxs-lookup"><span data-stu-id="8d26b-113">See [Set up a hybrid cluster with HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) tooget started with an on-premises head node and Azure "burst" nodes.</span></span> <span data-ttu-id="8d26b-114">V tématu hello [skript nasazení HPC Pack IaaS](hpcpack-cluster-powershell-script.md) tooquickly nasazení clusteru HPC Pack ve virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="8d26b-114">See hello [HPC Pack IaaS deployment script](hpcpack-cluster-powershell-script.md) tooquickly deploy an HPC Pack cluster in Azure VMs.</span></span>

* <span data-ttu-id="8d26b-115">**Pro cluster s hlavního uzlu v Azure (modelu nasazení Resource Manager)** – od HPC Pack 2016, ověřování pomocí certifikátu v aplikaci Azure Active Directory se používá pro automaticky rostoucí a zmenšení clusteru virtuálních počítačů nasadit pomocí Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="8d26b-115">**For a cluster with a head node in Azure (Resource Manager deployment model)** - Starting in HPC Pack 2016, certificate authentication in an Azure Active Directory application is used for automatically growing and shrinking cluster VMs deployed using Azure Resource Manager.</span></span> <span data-ttu-id="8d26b-116">Nakonfigurujte certifikát takto:</span><span class="sxs-lookup"><span data-stu-id="8d26b-116">Configure a certificate as follows:</span></span>

  1. <span data-ttu-id="8d26b-117">Po nasazení clusteru připojte pomocí vzdálené plochy tooone hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="8d26b-117">After cluster deployment, connect by Remote Desktop tooone head node.</span></span>

  2. <span data-ttu-id="8d26b-118">Nahrát hlavního uzlu tooeach hello certifikát (ve formátu PFX pomocí soukromého klíče) a nainstalujte tooCert:\LocalMachine\My a Cert: \LocalMachine\Root.</span><span class="sxs-lookup"><span data-stu-id="8d26b-118">Upload hello certificate (PFX format with private key) tooeach head node and install tooCert:\LocalMachine\My and Cert:\LocalMachine\Root.</span></span>

  3. <span data-ttu-id="8d26b-119">Otevřete prostředí Azure PowerShell jako správce a spusťte následující příkazy v jednom hlavního uzlu hello:</span><span class="sxs-lookup"><span data-stu-id="8d26b-119">Start Azure PowerShell as an administrator and run hello following commands on one head node:</span></span>

    ```powershell
        cd $env:CCP_HOME\bin

        Login-AzureRmAccount
    ```
        
    <span data-ttu-id="8d26b-120">Pokud váš účet je ve více než jednoho klienta Azure Active Directory nebo předplatné Azure, můžete spustit následující hello příkaz tooselect hello správné klienta a předplatné:</span><span class="sxs-lookup"><span data-stu-id="8d26b-120">If your account is in more than one Azure Active Directory tenant or Azure subscription, you can run hello following command tooselect hello correct tenant and subscription:</span></span>
  
    ```powershell
        Login-AzureRMAccount -TenantId <TenantId> -SubscriptionId <subscriptionId>
    ```     
       
    <span data-ttu-id="8d26b-121">Spusťte následující příkaz tooview hello hello aktuálně vybrané klienta a předplatné:</span><span class="sxs-lookup"><span data-stu-id="8d26b-121">Run hello following command tooview hello currently selected tenant and subscription:</span></span>
    
    ```powershell
        Get-AzureRMContext
    ```

  4. <span data-ttu-id="8d26b-122">Spusťte následující skript hello</span><span class="sxs-lookup"><span data-stu-id="8d26b-122">Run hello following script</span></span>

    ```powershell
        .\ConfigARMAutoGrowShrinkCert.ps1 -DisplayName “YourHpcPackAppName” -HomePage "https://YourHpcPackAppHomePage" -IdentifierUri "https://YourHpcPackAppUri" -CertificateThumbprint "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" -TenantId xxxxxxxx-xxxxx-xxxxx-xxxxx-xxxxxxxxxxxx
    ```

    <span data-ttu-id="8d26b-123">kde</span><span class="sxs-lookup"><span data-stu-id="8d26b-123">where</span></span>

    <span data-ttu-id="8d26b-124">**DisplayName** -zobrazovaný název aplikace Azure aktivní.</span><span class="sxs-lookup"><span data-stu-id="8d26b-124">**DisplayName** - Azure Active Application display name.</span></span> <span data-ttu-id="8d26b-125">Pokud aplikace hello neexistuje, vytvoří se v Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="8d26b-125">If hello application does not exist, it is created in Azure Active Directory.</span></span>

    <span data-ttu-id="8d26b-126">**Domovská stránka** -hello domovskou stránku hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="8d26b-126">**HomePage** - hello home page of hello application.</span></span> <span data-ttu-id="8d26b-127">Můžete nakonfigurovat fiktivní adresu URL, jako v předchozím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="8d26b-127">You can configure a dummy URL, as in hello preceding example.</span></span>

    <span data-ttu-id="8d26b-128">**IdentifierUri** -identifikátor aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="8d26b-128">**IdentifierUri** - Identifier of hello application.</span></span> <span data-ttu-id="8d26b-129">Můžete nakonfigurovat fiktivní adresu URL, jako v předchozím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="8d26b-129">You can configure a dummy URL, as in hello preceding example.</span></span>

    <span data-ttu-id="8d26b-130">**CertificateThumbprint** -kryptografický otisk certifikátu hello jste nainstalovali hello hlavního uzlu v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="8d26b-130">**CertificateThumbprint** - Thumbprint of hello certificate you installed on hello head node in Step 1.</span></span>

    <span data-ttu-id="8d26b-131">**TenantId** -ID služby Azure Active Directory klienta.</span><span class="sxs-lookup"><span data-stu-id="8d26b-131">**TenantId** - Tenant ID of your Azure Active Directory.</span></span> <span data-ttu-id="8d26b-132">Hello ID klienta můžete získat z portálu Azure Active Directory hello **vlastnosti** stránky.</span><span class="sxs-lookup"><span data-stu-id="8d26b-132">You can get hello Tenant ID from hello Azure Active Directory portal **Properties** page.</span></span>

    <span data-ttu-id="8d26b-133">Další podrobnosti o **ConfigARMAutoGrowShrinkCert.ps1**spusťte `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`.</span><span class="sxs-lookup"><span data-stu-id="8d26b-133">For more details about **ConfigARMAutoGrowShrinkCert.ps1**, run `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`.</span></span>


* <span data-ttu-id="8d26b-134">**Pro cluster s hlavního uzlu v Azure (modelu nasazení classic)** – Pokud používáte hello HPC Pack IaaS nasazení skriptu toocreate hello cluster v modelu nasazení classic hello, povolit hello **AutoGrowShrink** clusteru Vlastnost nastavením možnosti AutoGrowShrink hello v konfiguračním souboru na hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="8d26b-134">**For a cluster with a head node in Azure (classic deployment model)** - If you use hello HPC Pack IaaS deployment script toocreate hello cluster in hello classic deployment model, enable hello **AutoGrowShrink** cluster property by setting hello AutoGrowShrink option in hello cluster configuration file.</span></span> <span data-ttu-id="8d26b-135">Podrobnosti najdete v dokumentaci k hello doplňujícími hello [stahování skriptu](https://www.microsoft.com/download/details.aspx?id=44949).</span><span class="sxs-lookup"><span data-stu-id="8d26b-135">For details, see hello documentation accompanying hello [script download](https://www.microsoft.com/download/details.aspx?id=44949).</span></span>

    <span data-ttu-id="8d26b-136">Můžete taky povolit hello **AutoGrowShrink** vlastnost clusteru poté, co nasadíte hello clusteru prostředí HPC PowerShell pomocí příkazů popsané v následující části hello.</span><span class="sxs-lookup"><span data-stu-id="8d26b-136">Alternatively, enable hello **AutoGrowShrink** cluster property after you deploy hello cluster by using HPC PowerShell commands described in hello following section.</span></span> <span data-ttu-id="8d26b-137">tooprepare pro tento první dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="8d26b-137">tooprepare for this, first complete hello following steps:</span></span>

  1. <span data-ttu-id="8d26b-138">Nakonfigurujte certifikát pro správu Azure hello hlavního uzlu a v hello předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="8d26b-138">Configure an Azure management certificate on hello head node and in hello Azure subscription.</span></span> <span data-ttu-id="8d26b-139">Pro testovací nasazení můžete použít výchozí Microsoft HPC Azure hello podepsaný svým držitelem se certifikát, který nainstaluje sady HPC Pack hello hlavního uzlu a pak nahrajte tento certifikát tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="8d26b-139">For a test deployment, you can use hello Default Microsoft HPC Azure self-signed certificate that HPC Pack installs on hello head node, and then upload that certificate tooyour Azure subscription.</span></span> <span data-ttu-id="8d26b-140">Možnosti a kroky najdete v tématu hello [pokyny v knihovně TechNet](https://technet.microsoft.com/library/gg481759.aspx).</span><span class="sxs-lookup"><span data-stu-id="8d26b-140">For options and steps, see hello [TechNet Library guidance](https://technet.microsoft.com/library/gg481759.aspx).</span></span>

  2. <span data-ttu-id="8d26b-141">Spustit **regedit** hello hlavního uzlu, přejděte tooHKLM\SOFTWARE\Micorsoft\HPC\IaasInfo a přidejte hodnotu řetězce.</span><span class="sxs-lookup"><span data-stu-id="8d26b-141">Run **regedit** on hello head node, go tooHKLM\SOFTWARE\Micorsoft\HPC\IaasInfo, and add a string value.</span></span> <span data-ttu-id="8d26b-142">Nastavte název hodnoty hello příliš "Kryptografický otisk" a hodnota dat toohello kryptografický otisk certifikátu hello v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="8d26b-142">Set hello Value name too“ThumbPrint”, and Value data toohello thumbprint of hello certificate in Step 1.</span></span>

### <a name="hpc-powershell-commands-tooset-hello-autogrowshrink-property"></a><span data-ttu-id="8d26b-143">Prostředí HPC PowerShell příkazy tooset hello AutoGrowShrink vlastnost</span><span class="sxs-lookup"><span data-stu-id="8d26b-143">HPC PowerShell commands tooset hello AutoGrowShrink property</span></span>
<span data-ttu-id="8d26b-144">Následují ukázkové prostředí HPC PowerShell příkazy tooset **AutoGrowShrink** a tootune své chování s další parametry.</span><span class="sxs-lookup"><span data-stu-id="8d26b-144">Following are sample HPC PowerShell commands tooset **AutoGrowShrink** and tootune its behavior with additional parameters.</span></span> <span data-ttu-id="8d26b-145">V tématu [AutoGrowShrink parametry](#AutoGrowShrink-parameters) dále v tomto článku hello úplný seznam nastavení.</span><span class="sxs-lookup"><span data-stu-id="8d26b-145">See [AutoGrowShrink parameters](#AutoGrowShrink-parameters) later in this article for hello complete list of settings.</span></span>

<span data-ttu-id="8d26b-146">toorun tyto příkazy, spusťte prostředí HPC PowerShell hello hlavního uzlu clusteru jako správce.</span><span class="sxs-lookup"><span data-stu-id="8d26b-146">toorun these commands, start HPC PowerShell on hello cluster head node as an administrator.</span></span>

<span data-ttu-id="8d26b-147">**hello tooenable AutoGrowShrink vlastnost**</span><span class="sxs-lookup"><span data-stu-id="8d26b-147">**tooenable hello AutoGrowShrink property**</span></span>

```powershell
Set-HpcClusterProperty –EnableGrowShrink 1
```

<span data-ttu-id="8d26b-148">**hello toodisable AutoGrowShrink vlastnost**</span><span class="sxs-lookup"><span data-stu-id="8d26b-148">**toodisable hello AutoGrowShrink property**</span></span>

```powershell
Set-HpcClusterProperty –EnableGrowShrink 0
```

<span data-ttu-id="8d26b-149">**toochange hello růst interval v minutách**</span><span class="sxs-lookup"><span data-stu-id="8d26b-149">**toochange hello grow interval in minutes**</span></span>

```powershell
Set-HpcClusterProperty –GrowInterval <interval>
```

<span data-ttu-id="8d26b-150">**toochange hello zmenšit interval v minutách**</span><span class="sxs-lookup"><span data-stu-id="8d26b-150">**toochange hello shrink interval in minutes**</span></span>

```powershell
Set-HpcClusterProperty –ShrinkInterval <interval>
```

<span data-ttu-id="8d26b-151">**tooview hello aktuální konfiguraci AutoGrowShrink**</span><span class="sxs-lookup"><span data-stu-id="8d26b-151">**tooview hello current configuration of AutoGrowShrink**</span></span>

```powershell
Get-HpcClusterProperty –AutoGrowShrink
```

<span data-ttu-id="8d26b-152">**Uzel skupin tooexclude z AutoGrowShrink**</span><span class="sxs-lookup"><span data-stu-id="8d26b-152">**tooexclude node groups from AutoGrowShrink**</span></span>

```powershell
Set-HpcClusterProperty –ExcludeNodeGroups <group1,group2,group3>
```

>[!NOTE] 
> <span data-ttu-id="8d26b-153">Tento parametr je podporované počínaje HPC Pack 2016</span><span class="sxs-lookup"><span data-stu-id="8d26b-153">This parameter is supported starting in HPC Pack 2016</span></span>
>

### <a name="autogrowshrink-parameters"></a><span data-ttu-id="8d26b-154">Parametry AutoGrowShrink</span><span class="sxs-lookup"><span data-stu-id="8d26b-154">AutoGrowShrink parameters</span></span>
<span data-ttu-id="8d26b-155">Hello následují AutoGrowShrink parametry, které lze upravit pomocí hello **Set-HpcClusterProperty** příkaz.</span><span class="sxs-lookup"><span data-stu-id="8d26b-155">hello following are AutoGrowShrink parameters that you can modify by using hello **Set-HpcClusterProperty** command.</span></span>

* <span data-ttu-id="8d26b-156">**EnableGrowShrink** – přepínač tooenable nebo zakázat hello **AutoGrowShrink** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="8d26b-156">**EnableGrowShrink** - Switch tooenable or disable hello **AutoGrowShrink** property.</span></span>
* <span data-ttu-id="8d26b-157">**ParamSweepTasksPerCore** -počet čištění parametrů úlohy toogrow jednoho jádra.</span><span class="sxs-lookup"><span data-stu-id="8d26b-157">**ParamSweepTasksPerCore** - Number of parametric sweep tasks toogrow one core.</span></span> <span data-ttu-id="8d26b-158">Výchozí hodnota Hello je toogrow jednoho jádra daná úloha.</span><span class="sxs-lookup"><span data-stu-id="8d26b-158">hello default is toogrow one core per task.</span></span>

  > [!NOTE]
  > <span data-ttu-id="8d26b-159">Změny HPC Pack QFE KB3134307 **ParamSweepTasksPerCore** příliš**TasksPerResourceUnit**.</span><span class="sxs-lookup"><span data-stu-id="8d26b-159">HPC Pack QFE KB3134307 changes **ParamSweepTasksPerCore** too**TasksPerResourceUnit**.</span></span> <span data-ttu-id="8d26b-160">Ho je založený na typu prostředku hello úlohy a může být uzlu, soketu nebo jádra.</span><span class="sxs-lookup"><span data-stu-id="8d26b-160">It is based on hello job resource type and can be node, socket, or core.</span></span>
  >
  >
* <span data-ttu-id="8d26b-161">**GrowThreshold** -prahovou hodnotu automatického zvětšování tootrigger zařazených do fronty úloh.</span><span class="sxs-lookup"><span data-stu-id="8d26b-161">**GrowThreshold** - Threshold of queued tasks tootrigger automatic growth.</span></span> <span data-ttu-id="8d26b-162">Hello výchozí hodnota je 1, což znamená, že pokud neexistují 1 nebo více úloh v hello zařazených do fronty stavu, automaticky rozšiřovat uzly.</span><span class="sxs-lookup"><span data-stu-id="8d26b-162">hello default is 1, which means that if there are 1 or more tasks in hello queued state, automatically grow nodes.</span></span>
* <span data-ttu-id="8d26b-163">**GrowInterval** – Interval v minutách tootrigger automatického zvětšování.</span><span class="sxs-lookup"><span data-stu-id="8d26b-163">**GrowInterval** - Interval in minutes tootrigger automatic growth.</span></span> <span data-ttu-id="8d26b-164">Výchozí interval Hello je 5 minut.</span><span class="sxs-lookup"><span data-stu-id="8d26b-164">hello default interval is 5 minutes.</span></span>
* <span data-ttu-id="8d26b-165">**ShrinkInterval** – Interval v minutách tootrigger automatické zmenšit.</span><span class="sxs-lookup"><span data-stu-id="8d26b-165">**ShrinkInterval** - Interval in minutes tootrigger automatic shrinking.</span></span> <span data-ttu-id="8d26b-166">Výchozí interval Hello je 5 minut. |</span><span class="sxs-lookup"><span data-stu-id="8d26b-166">hello default interval is 5 minutes.|</span></span>
* <span data-ttu-id="8d26b-167">**ShrinkIdleTimes** -počet průběžné kontroly tooshrink tooindicate hello uzlů, které nejsou používána.</span><span class="sxs-lookup"><span data-stu-id="8d26b-167">**ShrinkIdleTimes** - Number of continuous checks tooshrink tooindicate hello nodes are idle.</span></span> <span data-ttu-id="8d26b-168">Hello výchozí hodnota je 3 x.</span><span class="sxs-lookup"><span data-stu-id="8d26b-168">hello default is 3 times.</span></span> <span data-ttu-id="8d26b-169">Například, pokud hello **ShrinkInterval** je 5 minut, HPC Pack zkontroluje, zda text hello uzel je nečinnosti každých 5 minut.</span><span class="sxs-lookup"><span data-stu-id="8d26b-169">For example, if hello **ShrinkInterval** is 5 minutes, HPC Pack checks whether hello node is idle every 5 minutes.</span></span> <span data-ttu-id="8d26b-170">Pokud hello uzly jsou ve stavu nečinnosti hello po 3 průběžné kontroluje (15 minut), HPC Pack zmenší tento uzel.</span><span class="sxs-lookup"><span data-stu-id="8d26b-170">If hello nodes are in hello idle state after 3 continuous checks (15 minutes), then HPC Pack shrinks that node.</span></span>
* <span data-ttu-id="8d26b-171">**ExtraNodesGrowRatio** -další procento toogrow uzlů pro úlohy Message Passing Interface (MPI).</span><span class="sxs-lookup"><span data-stu-id="8d26b-171">**ExtraNodesGrowRatio** - Additional percentage of nodes toogrow for Message Passing Interface (MPI) jobs.</span></span> <span data-ttu-id="8d26b-172">Hello výchozí hodnota je 1, což znamená zvětšování HPC Pack uzlů pro úlohy MPI 1 %.</span><span class="sxs-lookup"><span data-stu-id="8d26b-172">hello default value is 1, which means that HPC Pack grows nodes 1% for MPI jobs.</span></span>
* <span data-ttu-id="8d26b-173">**GrowByMin** – přepínač tooindicate, jestli zásady automatické zvětšování hello podle hello minimální prostředky potřebné pro úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="8d26b-173">**GrowByMin** - Switch tooindicate whether hello autogrow policy is based on hello minimum resources required for hello job.</span></span> <span data-ttu-id="8d26b-174">Hello výchozí hodnota je false, což znamená, že HPC Pack zvětšování uzlů pro úlohy podle hello maximální prostředky potřebné pro úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="8d26b-174">hello default is false, which means that HPC Pack grows nodes for jobs based on hello maximum resources required for hello jobs.</span></span>
* <span data-ttu-id="8d26b-175">**SoaJobGrowThreshold** -prahovou hodnotu příchozí SOA požadavky tootrigger hello automatické zvětšení procesu.</span><span class="sxs-lookup"><span data-stu-id="8d26b-175">**SoaJobGrowThreshold** - Threshold of incoming SOA requests tootrigger hello automatic grow process.</span></span> <span data-ttu-id="8d26b-176">Hello výchozí hodnota je 50000.</span><span class="sxs-lookup"><span data-stu-id="8d26b-176">hello default value is 50000.</span></span>

  > [!NOTE]
  > <span data-ttu-id="8d26b-177">Tento parametr je podporované počínaje HPC Pack 2012 R2 Update 3.</span><span class="sxs-lookup"><span data-stu-id="8d26b-177">This parameter is supported starting in HPC Pack 2012 R2 Update 3.</span></span>
  >
  >
* <span data-ttu-id="8d26b-178">**SoaRequestsPerCore** -počet příchozích SOA požadavků toogrow jednoho jádra.</span><span class="sxs-lookup"><span data-stu-id="8d26b-178">**SoaRequestsPerCore** -Number of incoming SOA requests toogrow one core.</span></span> <span data-ttu-id="8d26b-179">Hello výchozí hodnota je 20000.</span><span class="sxs-lookup"><span data-stu-id="8d26b-179">hello default value is 20000.</span></span>

  > [!NOTE]
  > <span data-ttu-id="8d26b-180">Tento parametr je podporované počínaje HPC Pack 2012 R2 Update 3.</span><span class="sxs-lookup"><span data-stu-id="8d26b-180">This parameter is supported starting in HPC Pack 2012 R2 Update 3.</span></span>
  >
* <span data-ttu-id="8d26b-181">**ExcludeNodeGroups** – uzel skupiny automaticky zvýšit nebo snížit zadány uzly v hello.</span><span class="sxs-lookup"><span data-stu-id="8d26b-181">**ExcludeNodeGroups** – Nodes in hello specified node groups do not automatically grow and shrink.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="8d26b-182">Tento parametr je podporované počínaje HPC Pack 2016.</span><span class="sxs-lookup"><span data-stu-id="8d26b-182">This parameter is supported starting in HPC Pack 2016.</span></span>
  >

### <a name="mpi-example"></a><span data-ttu-id="8d26b-183">Příklad MPI</span><span class="sxs-lookup"><span data-stu-id="8d26b-183">MPI example</span></span>
<span data-ttu-id="8d26b-184">Ve výchozím nastavení HPC Pack zvětšování 1 % navíc uzlů pro úlohy MPI (**ExtraNodesGrowRatio** nastavena too1).</span><span class="sxs-lookup"><span data-stu-id="8d26b-184">By default HPC Pack grows 1% extra nodes for MPI jobs (**ExtraNodesGrowRatio** is set too1).</span></span> <span data-ttu-id="8d26b-185">Hello důvodem je, že MPI může vyžadovat více uzlů a hello úlohu lze spustit pouze když jsou všechny uzly připravené.</span><span class="sxs-lookup"><span data-stu-id="8d26b-185">hello reason is that MPI may require multiple nodes, and hello job can only run when all nodes are ready.</span></span> <span data-ttu-id="8d26b-186">Při spuštění uzlů Azure příležitostně jeden uzel může být nutné další čas toostart než jiné způsobuje ostatní uzly toobe nečinnosti při čekání na tento uzel tooget připraven.</span><span class="sxs-lookup"><span data-stu-id="8d26b-186">When Azure starts nodes, occasionally one node might need more time toostart than others, causing other nodes toobe idle while waiting for that node tooget ready.</span></span> <span data-ttu-id="8d26b-187">Způsobeného nárůstem další uzly, HPC Pack snižuje dobu čekání na tento prostředek a potenciálně šetří náklady.</span><span class="sxs-lookup"><span data-stu-id="8d26b-187">By growing extra nodes, HPC Pack reduces this resource waiting time, and potentially saves costs.</span></span> <span data-ttu-id="8d26b-188">Procento hello tooincrease navíc uzlů pro úlohy MPI (například too10 %), spusťte příkaz podobný</span><span class="sxs-lookup"><span data-stu-id="8d26b-188">tooincrease hello percentage of extra nodes for MPI jobs (for example, too10%), run a command similar to</span></span>

    Set-HpcClusterProperty -ExtraNodesGrowRatio 10

### <a name="soa-example"></a><span data-ttu-id="8d26b-189">Příklad SOA</span><span class="sxs-lookup"><span data-stu-id="8d26b-189">SOA example</span></span>
<span data-ttu-id="8d26b-190">Ve výchozím nastavení **SoaJobGrowThreshold** nastavena too50000 a **SoaRequestsPerCore** too200000 nastavena.</span><span class="sxs-lookup"><span data-stu-id="8d26b-190">By default, **SoaJobGrowThreshold** is set too50000 and **SoaRequestsPerCore** is set too200000.</span></span> <span data-ttu-id="8d26b-191">Pokud odešlete jeden úlohy architektury SOA s 70000 požadavky, je ve frontě úloh a 70000 jsou příchozí požadavky.</span><span class="sxs-lookup"><span data-stu-id="8d26b-191">If you submit one SOA job with 70000 requests, there is one queued task and incoming requests are 70000.</span></span> <span data-ttu-id="8d26b-192">V takovém případě HPC Pack zvětšování 1 jádro hello zařazených do fronty úloh a pro příchozí požadavky a zvětšování (70000-50000) nebo základní 20000 = 1, takže v celkem zvětšování 2 jádra pro tuto úlohu SOA.</span><span class="sxs-lookup"><span data-stu-id="8d26b-192">In this case HPC Pack grows 1 core for hello queued task, and for incoming requests, grows (70000 - 50000)/20000 = 1 core, so in total grows 2 cores for this SOA job.</span></span>

## <a name="run-hello-azureautogrowshrinkps1-script"></a><span data-ttu-id="8d26b-193">Spusťte skript AzureAutoGrowShrink.ps1 hello</span><span class="sxs-lookup"><span data-stu-id="8d26b-193">Run hello AzureAutoGrowShrink.ps1 script</span></span>
### <a name="prerequisites"></a><span data-ttu-id="8d26b-194">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8d26b-194">Prerequisites</span></span>

* <span data-ttu-id="8d26b-195">**HPC Pack 2012 R2 Update 1 nebo novější clusteru** – hello **AzureAutoGrowShrink.ps1** skript je nainstalován do složky bin hello % CCP_HOME %.</span><span class="sxs-lookup"><span data-stu-id="8d26b-195">**HPC Pack 2012 R2 Update 1 or later cluster** - hello **AzureAutoGrowShrink.ps1** script is installed in hello %CCP_HOME%bin folder.</span></span> <span data-ttu-id="8d26b-196">Hello hlavního uzlu clusteru může být nasazena místně nebo ve virtuálním počítači Azure.</span><span class="sxs-lookup"><span data-stu-id="8d26b-196">hello cluster head node can be deployed either on-premises or in an Azure VM.</span></span> <span data-ttu-id="8d26b-197">V tématu [nastavení hybridního clusteru pomocí sady HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) tooget začít s hlavnímu uzlu místní a uzly Azure "shluků".</span><span class="sxs-lookup"><span data-stu-id="8d26b-197">See [Set up a hybrid cluster with HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) tooget started with an on-premises head node and Azure "burst" nodes.</span></span> <span data-ttu-id="8d26b-198">V tématu hello [skript nasazení HPC Pack IaaS](hpcpack-cluster-powershell-script.md) tooquickly nasazení clusteru HPC Pack ve virtuálních počítačích Azure, nebo použijte [šablony Azure rychlý Start](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/).</span><span class="sxs-lookup"><span data-stu-id="8d26b-198">See hello [HPC Pack IaaS deployment script](hpcpack-cluster-powershell-script.md) tooquickly deploy an HPC Pack cluster in Azure VMs, or use an [Azure quickstart template](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/).</span></span>
* <span data-ttu-id="8d26b-199">**Prostředí Azure PowerShell 1.4.0** -hello skriptu aktuálně závisí na konkrétní verzi prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="8d26b-199">**Azure PowerShell 1.4.0** - hello script currently depends on this specific version of Azure PowerShell.</span></span>
* <span data-ttu-id="8d26b-200">**Pro cluster s Azure burst uzly** – spusťte skript hello na klientském počítači, kde je nainstalován HPC Pack nebo hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="8d26b-200">**For a cluster with Azure burst nodes** - Run hello script on a client computer where HPC Pack is installed, or on hello head node.</span></span> <span data-ttu-id="8d26b-201">Pokud běží na klientském počítači, zajistěte, abyste nastavili hello proměnné $env: CCP_SCHEDULER toopoint toohello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="8d26b-201">If running on a client computer, ensure that you set hello variable $env:CCP_SCHEDULER toopoint toohello head node.</span></span> <span data-ttu-id="8d26b-202">uzly Azure "shluků" Hello, musí být přidaný toohello clusteru, ale může být v hello stavu nasazení není.</span><span class="sxs-lookup"><span data-stu-id="8d26b-202">hello Azure “burst” nodes must be added toohello cluster, but they may be in hello Not-Deployed state.</span></span>
* <span data-ttu-id="8d26b-203">**Pro cluster s podporou nasazené ve virtuálních počítačích Azure (modelu nasazení Resource Manager)** -pro cluster s podporou virtuálních počítačů Azure nasazené v modelu nasazení Resource Manager hello hello skriptu podporuje dvě metody pro ověřování Azure: Přihlaste tooyour účet Azure skript hello toorun pokaždé, když (spuštěním `Login-AzureRmAccount`, nebo nakonfigurovat hlavní tooauthenticate služby pomocí certifikátu.</span><span class="sxs-lookup"><span data-stu-id="8d26b-203">**For a cluster deployed in Azure VMs (Resource Manager deployment model)** - For a cluster of Azure VMs deployed in hello Resource Manager deployment model, hello script supports two methods for Azure authentication: sign in tooyour Azure account toorun hello script every time (by running `Login-AzureRmAccount`, or configure a service principal tooauthenticate with a certificate.</span></span> <span data-ttu-id="8d26b-204">HPC Pack poskytuje hello skriptu **ConfigARMAutoGrowShrinkCert.ps** toocreate hlavní název služby pomocí certifikátu.</span><span class="sxs-lookup"><span data-stu-id="8d26b-204">HPC Pack provides hello script **ConfigARMAutoGrowShrinkCert.ps** toocreate a service principal with certificate.</span></span> <span data-ttu-id="8d26b-205">Hello skript vytvoří aplikaci služby Azure Active Directory (Azure AD) a hlavní název služby a přiřadí hello Přispěvatel role toohello instanční objekt.</span><span class="sxs-lookup"><span data-stu-id="8d26b-205">hello script creates an Azure Active Directory (Azure AD) application and a service principal, and assigns hello Contributor role toohello service principal.</span></span> <span data-ttu-id="8d26b-206">toorun hello skriptu, otevřete prostředí Azure PowerShell jako správce a spusťte následující příkazy hello:</span><span class="sxs-lookup"><span data-stu-id="8d26b-206">toorun hello script, start Azure PowerShell  as administrator and run hello following commands:</span></span>

    ```powershell
    cd $env:CCP_HOME\bin

    Login-AzureRmAccount

    .\ConfigARMAutoGrowShrinkCert.ps1 -DisplayName “YourHpcPackAppName” -HomePage "https://YourHpcPackAppHomePage" -IdentifierUri "https://YourHpcPackAppUri" -PfxFile "d:\yourcertificate.pfx"
    ```

    <span data-ttu-id="8d26b-207">Další podrobnosti o **ConfigARMAutoGrowShrinkCert.ps1**spusťte `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`,</span><span class="sxs-lookup"><span data-stu-id="8d26b-207">For more details about **ConfigARMAutoGrowShrinkCert.ps1**, run `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`,</span></span>

* <span data-ttu-id="8d26b-208">**Pro cluster s podporou nasazené ve virtuálních počítačích Azure (modelu nasazení classic)** – spusťte skript hello hello hlavního uzlu virtuálního počítače, protože závisí na hello **Start-HpcIaaSNode.ps1** a **Stop-HpcIaaSNode.ps1**skripty, které jsou nainstalovány existuje.</span><span class="sxs-lookup"><span data-stu-id="8d26b-208">**For a cluster deployed in Azure VMs (classic deployment model)** - Run hello script on hello head node VM, because it depends on hello **Start-HpcIaaSNode.ps1** and **Stop-HpcIaaSNode.ps1** scripts that are installed there.</span></span> <span data-ttu-id="8d26b-209">Tyto skripty navíc vyžadují certifikát pro správu Azure nebo soubor nastavení publikování (viz [spravovat výpočetní uzly v prostředí HPC Pack clusteru v Azure](hpcpack-cluster-node-manage.md)).</span><span class="sxs-lookup"><span data-stu-id="8d26b-209">Those scripts additionally require an Azure management certificate or publish settings file (see [Manage compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster-node-manage.md)).</span></span> <span data-ttu-id="8d26b-210">Zajistěte, aby všechny hello výpočetní uzel virtuální počítače, musíte již byly přidány toohello clusteru.</span><span class="sxs-lookup"><span data-stu-id="8d26b-210">Make sure all hello compute node VMs you need are already added toohello cluster.</span></span> <span data-ttu-id="8d26b-211">Mohou být v zastaveném stavu hello.</span><span class="sxs-lookup"><span data-stu-id="8d26b-211">They may be in hello Stopped state.</span></span>



### <a name="syntax"></a><span data-ttu-id="8d26b-212">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="8d26b-212">Syntax</span></span>
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
### <a name="parameters"></a><span data-ttu-id="8d26b-213">Parametry</span><span class="sxs-lookup"><span data-stu-id="8d26b-213">Parameters</span></span>
* <span data-ttu-id="8d26b-214">**NodeTemplates** -názvů hello uzel šablony toodefine hello obor pro hello uzly toogrow a zmenšit.</span><span class="sxs-lookup"><span data-stu-id="8d26b-214">**NodeTemplates** - Names of hello node templates toodefine hello scope for hello nodes toogrow and shrink.</span></span> <span data-ttu-id="8d26b-215">Není-li zadána (hello výchozí hodnota je @()), všechny uzly v hello **AzureNodes** uzlu skupiny jsou v oboru při **NodeType** má hodnotu AzureNodes a všech uzlech v hello **ComputeNodes** uzlu skupiny jsou v oboru při **NodeType** má hodnotu ComputeNodes.</span><span class="sxs-lookup"><span data-stu-id="8d26b-215">If not specified (hello default value is @()), all nodes in hello **AzureNodes** node group are in scope when **NodeType** has a value of AzureNodes, and all nodes in hello **ComputeNodes** node group are in scope when **NodeType** has a value of ComputeNodes.</span></span>
* <span data-ttu-id="8d26b-216">**JobTemplates** -názvy hello úlohy šablony toodefine hello obor pro uzly toogrow hello.</span><span class="sxs-lookup"><span data-stu-id="8d26b-216">**JobTemplates** - Names of hello job templates toodefine hello scope for hello nodes toogrow.</span></span>
* <span data-ttu-id="8d26b-217">**Typ uzlu** – hello typ uzlu toogrow a zmenšit.</span><span class="sxs-lookup"><span data-stu-id="8d26b-217">**NodeType** - hello type of node toogrow and shrink.</span></span> <span data-ttu-id="8d26b-218">Podporované hodnoty jsou:</span><span class="sxs-lookup"><span data-stu-id="8d26b-218">Supported values are:</span></span>

  * <span data-ttu-id="8d26b-219">**AzureNodes** – u uzlů Azure PaaS (shluků) v místní nebo v clusteru Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="8d26b-219">**AzureNodes** – for Azure PaaS (burst) nodes in an on-premises or Azure IaaS cluster.</span></span>
  * <span data-ttu-id="8d26b-220">**ComputeNodes** – pro výpočetní uzel virtuální počítače pouze v clusteru služby Azure IaaS.</span><span class="sxs-lookup"><span data-stu-id="8d26b-220">**ComputeNodes** - for compute node VMs only in an Azure IaaS cluster.</span></span>

* <span data-ttu-id="8d26b-221">**NumOfQueuedJobsPerNodeToGrow** -počet úloh zařazených do fronty vyžadovaných toogrow jeden uzel.</span><span class="sxs-lookup"><span data-stu-id="8d26b-221">**NumOfQueuedJobsPerNodeToGrow** - Number of queued jobs required toogrow one node.</span></span>
* <span data-ttu-id="8d26b-222">**NumOfQueuedJobsToGrowThreshold** -hello prahová hodnota počtu úloh zařazených do fronty toostart hello růst procesu.</span><span class="sxs-lookup"><span data-stu-id="8d26b-222">**NumOfQueuedJobsToGrowThreshold** - hello threshold number of queued jobs toostart hello grow process.</span></span>
* <span data-ttu-id="8d26b-223">**NumOfActiveQueuedTasksPerNodeToGrow** -hello počet aktivních úloh zařazených do fronty vyžadovaných toogrow jeden uzel.</span><span class="sxs-lookup"><span data-stu-id="8d26b-223">**NumOfActiveQueuedTasksPerNodeToGrow** - hello number of active queued tasks required toogrow one node.</span></span> <span data-ttu-id="8d26b-224">Pokud **NumOfQueuedJobsPerNodeToGrow** je zadán s hodnotou větší než 0, tento parametr je ignorován.</span><span class="sxs-lookup"><span data-stu-id="8d26b-224">If **NumOfQueuedJobsPerNodeToGrow** is specified with a value greater than 0, this parameter is ignored.</span></span>
* <span data-ttu-id="8d26b-225">**NumOfActiveQueuedTasksToGrowThreshold** -hello prahová hodnota počtu aktivních úloh zařazených do fronty toostart hello růst procesu.</span><span class="sxs-lookup"><span data-stu-id="8d26b-225">**NumOfActiveQueuedTasksToGrowThreshold** - hello threshold number of active queued tasks toostart hello grow process.</span></span>
* <span data-ttu-id="8d26b-226">**NumOfInitialNodesToGrow** – hello počáteční minimální počet uzlů toogrow, pokud jsou všechny uzly hello v oboru **není nasazena** nebo **zastaveno (Deallocated)**.</span><span class="sxs-lookup"><span data-stu-id="8d26b-226">**NumOfInitialNodesToGrow** - hello initial minimum number of nodes toogrow if all hello nodes in scope are **Not-Deployed** or **Stopped (Deallocated)**.</span></span>
* <span data-ttu-id="8d26b-227">**GrowCheckIntervalMins** -hello interval v minutách mezi kontroluje toogrow.</span><span class="sxs-lookup"><span data-stu-id="8d26b-227">**GrowCheckIntervalMins** - hello interval in minutes between checks toogrow.</span></span>
* <span data-ttu-id="8d26b-228">**ShrinkCheckIntervalMins** -hello interval v minutách mezi kontroluje tooshrink.</span><span class="sxs-lookup"><span data-stu-id="8d26b-228">**ShrinkCheckIntervalMins** - hello interval in minutes between checks tooshrink.</span></span>
* <span data-ttu-id="8d26b-229">**ShrinkCheckIdleTimes** -hello počtu kontrol průběžné zmenšení (oddělených **ShrinkCheckIntervalMins**) jsou uzly hello tooindicate nečinnosti.</span><span class="sxs-lookup"><span data-stu-id="8d26b-229">**ShrinkCheckIdleTimes** - hello number of continuous shrink checks (separated by **ShrinkCheckIntervalMins**) tooindicate hello nodes are idle.</span></span>
* <span data-ttu-id="8d26b-230">**UseLastConfigurations** -hello předchozích konfigurací, které jsou uloženy v souboru argument hello.</span><span class="sxs-lookup"><span data-stu-id="8d26b-230">**UseLastConfigurations** - hello previous configurations saved in hello argument file.</span></span>
* <span data-ttu-id="8d26b-231">**ArgFile**– hello název hello argument soubor použitý toosave a aktualizovat hello konfigurace toorun hello skriptu.</span><span class="sxs-lookup"><span data-stu-id="8d26b-231">**ArgFile**- hello name of hello argument file used toosave and update hello configurations toorun hello script.</span></span>
* <span data-ttu-id="8d26b-232">**LogFilePrefix** -hello předpona názvu souboru protokolu hello.</span><span class="sxs-lookup"><span data-stu-id="8d26b-232">**LogFilePrefix** - hello prefix name of hello log file.</span></span> <span data-ttu-id="8d26b-233">Můžete zadat cestu.</span><span class="sxs-lookup"><span data-stu-id="8d26b-233">You can specify a path.</span></span> <span data-ttu-id="8d26b-234">Ve výchozím nastavení hello protokol je zapsaný toohello aktuální pracovní adresář.</span><span class="sxs-lookup"><span data-stu-id="8d26b-234">By default hello log is written toohello current working directory.</span></span>

### <a name="example-1"></a><span data-ttu-id="8d26b-235">Příklad 1</span><span class="sxs-lookup"><span data-stu-id="8d26b-235">Example 1</span></span>
<span data-ttu-id="8d26b-236">Hello následující příklad konfiguruje hello Azure burst uzly nasazené pomocí výchozí šablony AzureNode toogrow a zmenšit automaticky.</span><span class="sxs-lookup"><span data-stu-id="8d26b-236">hello following example configures hello Azure burst nodes deployed with the Default AzureNode Template toogrow and shrink automatically.</span></span> <span data-ttu-id="8d26b-237">Pokud jsou všechny uzly původně v hello **není nasazena** stavu, jsou spuštěny aspoň 3 uzly.</span><span class="sxs-lookup"><span data-stu-id="8d26b-237">If all the nodes are initially in hello **Not-Deployed** state, at least 3 nodes are started.</span></span> <span data-ttu-id="8d26b-238">Pokud hello počet úloh zařazených do fronty překračuje 8, hello skript spustí uzlů, dokud se jejich počet překračuje hello poměr zařazených do fronty úloh **NumOfQueuedJobsPerNodeToGrow**.</span><span class="sxs-lookup"><span data-stu-id="8d26b-238">If hello number of queued jobs exceeds 8, hello script starts nodes until their number exceeds hello ratio of queued jobs to **NumOfQueuedJobsPerNodeToGrow**.</span></span> <span data-ttu-id="8d26b-239">Pokud se uzel najde toobe nečinnosti 3 po sobě jdoucích nečinnosti časů, je zastavena.</span><span class="sxs-lookup"><span data-stu-id="8d26b-239">If a node is found toobe idle in 3 consecutive idle times, it is stopped.</span></span>

```powershell
.\AzureAutoGrowShrink.ps1 -NodeTemplates @('Default AzureNode
 Template') -NodeType AzureNodes -NumOfQueuedJobsPerNodeToGrow 5
 -NumOfQueuedJobsToGrowThreshold 8 -NumOfInitialNodesToGrow 3
 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 3
```

### <a name="example-2"></a><span data-ttu-id="8d26b-240">Příklad 2</span><span class="sxs-lookup"><span data-stu-id="8d26b-240">Example 2</span></span>
<span data-ttu-id="8d26b-241">Hello následující příklad konfiguruje hello Azure výpočetní uzel virtuální počítače nasazené pomocí výchozí šablony ComputeNode toogrow hello a zmenšit automaticky.</span><span class="sxs-lookup"><span data-stu-id="8d26b-241">hello following example configures hello Azure compute node VMs deployed with hello Default ComputeNode Template toogrow and shrink automatically.</span></span>
<span data-ttu-id="8d26b-242">úlohy Hello nakonfigurovat pomocí šablony úlohy výchozí hello definuje rozsah hello zatížení v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="8d26b-242">hello jobs configured by hello Default job template define hello scope of the workload on hello cluster.</span></span> <span data-ttu-id="8d26b-243">Pokud jsou všechny uzly hello původně zastavena, nejméně 5 uzly jsou spuštěny.</span><span class="sxs-lookup"><span data-stu-id="8d26b-243">If all hello nodes are initially stopped, at least 5 nodes are started.</span></span> <span data-ttu-id="8d26b-244">Pokud hello počet aktivních úloh zařazených do fronty překročí 15, hello skript spustí uzlů, dokud se jejich počet překračuje hello poměr aktivních úloh zařazených do fronty příliš**NumOfActiveQueuedTasksPerNodeToGrow**.</span><span class="sxs-lookup"><span data-stu-id="8d26b-244">If hello number of active queued tasks exceeds 15, hello script starts nodes until their number exceeds hello ratio of active queued tasks too**NumOfActiveQueuedTasksPerNodeToGrow**.</span></span> <span data-ttu-id="8d26b-245">Pokud se uzel najde toobe nečinnosti v 10krát po sobě jdoucích nečinnosti, je zastavena.</span><span class="sxs-lookup"><span data-stu-id="8d26b-245">If a node is found toobe idle in 10 consecutive idle times, it is stopped.</span></span>

```powershell
.\AzureAutoGrowShrink.ps1 -NodeTemplates 'Default ComputeNode Template' -JobTemplates 'Default' -NodeType ComputeNodes -NumOfActiveQueuedTasksPerNodeToGrow 10 -NumOfActiveQueuedTasksToGrowThreshold 15 -NumOfInitialNodesToGrow 5 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 10 -ArgFile 'IaaSVMComputeNodes_Arg.xml' -LogFilePrefix 'IaaSVMComputeNodes_log'
```
