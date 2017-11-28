---
title: "aaaRun HVĚZDIČKY – CCM + s HPC Pack na virtuální počítače s Linuxem | Microsoft Docs"
description: "Nasazení clusteru s podporou sady Microsoft HPC Pack v Azure a spuštění HVĚZDIČKOU – CCM + úlohy na několika Linux výpočetní uzly přes sítě RDMA."
services: virtual-machines-linux
documentationcenter: 
author: xpillons
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager,hpc-pack
ms.assetid: 75523406-d268-4623-ac3e-811c7b74de4b
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: big-compute
ms.date: 09/13/2016
ms.author: xpillons
ms.openlocfilehash: 8265013cb295f53d6d4354ab2f100ef20d9f4c8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-star-ccm-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a><span data-ttu-id="c4920-103">Spustit HVĚZDIČKY – CCM + pomocí sady Microsoft HPC Pack na Linux RDMA cluster v Azure</span><span class="sxs-lookup"><span data-stu-id="c4920-103">Run STAR-CCM+ with Microsoft HPC Pack on a Linux RDMA cluster in Azure</span></span>
<span data-ttu-id="c4920-104">Tento článek ukazuje, jak toodeploy sady Microsoft HPC Pack clusteru v Azure a spusťte [HVĚZDIČKY CD adapco-CCM +](http://www.cd-adapco.com/products/star-ccm%C2%AE) úlohy na několika výpočetních uzlech Linux, které jsou vzájemně propojeny InfiniBand.</span><span class="sxs-lookup"><span data-stu-id="c4920-104">This article shows you how toodeploy a Microsoft HPC Pack cluster on Azure and run a [CD-adapco STAR-CCM+](http://www.cd-adapco.com/products/star-ccm%C2%AE) job on multiple Linux compute nodes that are interconnected with InfiniBand.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="c4920-105">Microsoft HPC Pack poskytuje funkce toorun celou řadu rozsáhlé HPC a paralelní aplikace, včetně aplikací MPI, v clusterech virtuální počítače Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="c4920-105">Microsoft HPC Pack provides features toorun a variety of large-scale HPC and parallel applications, including MPI applications, on clusters of Microsoft Azure virtual machines.</span></span> <span data-ttu-id="c4920-106">HPC Pack také podporuje spuštěné aplikace prostředí HPC Linux virtuálních počítačů Linux výpočetní uzly, které jsou nasazené na clusteru HPC Pack.</span><span class="sxs-lookup"><span data-stu-id="c4920-106">HPC Pack also supports running Linux HPC applications on Linux compute-node VMs that are deployed in an HPC Pack cluster.</span></span> <span data-ttu-id="c4920-107">Úvod toousing Linux výpočetní uzly s HPC Pack, najdete v části [začít pracovat s Linux výpočetní uzly v clusteru služby HPC Pack v Azure](hpcpack-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="c4920-107">For an introduction toousing Linux compute nodes with HPC Pack, see [Get started with Linux compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster.md).</span></span>

## <a name="set-up-an-hpc-pack-cluster"></a><span data-ttu-id="c4920-108">Nastavení clusteru služby HPC Pack</span><span class="sxs-lookup"><span data-stu-id="c4920-108">Set up an HPC Pack cluster</span></span>
<span data-ttu-id="c4920-109">Stáhnout skriptů nasazení HPC Pack IaaS hello z hello [Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=44949) a extrahovat je místně.</span><span class="sxs-lookup"><span data-stu-id="c4920-109">Download hello HPC Pack IaaS deployment scripts from hello [Download Center](https://www.microsoft.com/en-us/download/details.aspx?id=44949) and extract them locally.</span></span>

<span data-ttu-id="c4920-110">Prostředí Azure PowerShell je požadována.</span><span class="sxs-lookup"><span data-stu-id="c4920-110">Azure PowerShell is a prerequisite.</span></span> <span data-ttu-id="c4920-111">Pokud PowerShell není nakonfigurovaný v místním počítači, přečtěte si článek hello [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c4920-111">If PowerShell is not configured on your local machine, please read hello article [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="c4920-112">V době psaní tohoto textu hello hello Linux Image z hello Azure Marketplace, (který obsahuje hello InfiniBand ovladače pro Azure) jsou pro SLES 12, CentOS 6.5 a CentOS 7.1.</span><span class="sxs-lookup"><span data-stu-id="c4920-112">At hello time of this writing, hello Linux images from hello Azure Marketplace (which contains hello InfiniBand drivers for Azure) are for SLES 12, CentOS 6.5, and CentOS 7.1.</span></span> <span data-ttu-id="c4920-113">Tento článek je založená na využití hello SLES 12.</span><span class="sxs-lookup"><span data-stu-id="c4920-113">This article is based on hello usage of SLES 12.</span></span> <span data-ttu-id="c4920-114">Název hello tooretrieve všechny Image Linux, které podporují prostředí HPC v hello Marketplace, můžete spustit následující příkaz prostředí PowerShell hello:</span><span class="sxs-lookup"><span data-stu-id="c4920-114">tooretrieve hello name of all Linux images that support HPC in hello Marketplace, you can run hello following PowerShell command:</span></span>

```
    get-azurevmimage | ?{$_.ImageName.Contains("hpc") -and $_.OS -eq "Linux" }
```

<span data-ttu-id="c4920-115">výstup Hello uvádí hello umístění, ve kterém se tyto Image jsou k dispozici a hello název bitové kopie (**ImageName**) toobe použitých v šabloně nasazení hello později.</span><span class="sxs-lookup"><span data-stu-id="c4920-115">hello output lists hello location in which these images are available and hello image name (**ImageName**) toobe used in hello deployment template later.</span></span>

<span data-ttu-id="c4920-116">Před nasazením hello clusteru, máte toobuild soubor HPC Pack nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="c4920-116">Before you deploy hello cluster, you have toobuild an HPC Pack deployment template file.</span></span> <span data-ttu-id="c4920-117">Protože jsme se cílení na clusteru s podporou malé, hlavního uzlu hello bude řadič domény hello a hostování místní databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="c4920-117">Because we're targeting a small cluster, hello head node will be hello domain controller and host a local SQL database.</span></span>

<span data-ttu-id="c4920-118">Hello následující šablony bude nasazení hlavního uzlu, vytvořte soubor XML s názvem **MyCluster.xml**a nahraďte hodnoty hello **SubscriptionId**, **StorageAccount**,  **Umístění**, **VMName**, a **ServiceName** s tímto počítačem.</span><span class="sxs-lookup"><span data-stu-id="c4920-118">hello following template will deploy such a head node, create an XML file named **MyCluster.xml**, and replace hello values of **SubscriptionId**, **StorageAccount**, **Location**, **VMName**, and **ServiceName** with yours.</span></span>

    <?xml version="1.0" encoding="utf-8" ?>
    <IaaSClusterConfig>
      <Subscription>
        <SubscriptionId>99999999-9999-9999-9999-999999999999</SubscriptionId>
        <StorageAccount>mystorageaccount</StorageAccount>
      </Subscription>
      <Location>North Europe</Location>
      <VNet>
        <VNetName>hpcvnetne</VNetName>
        <SubnetName>subnet-hpc</SubnetName>
      </VNet>
      <Domain>
        <DCOption>HeadNodeAsDC</DCOption>
        <DomainFQDN>hpc.local</DomainFQDN>
      </Domain>
      <Database>
        <DBOption>LocalDB</DBOption>
      </Database>
      <HeadNode>
        <VMName>myhpchn</VMName>
        <ServiceName>myhpchn</ServiceName>
        <VMSize>Standard_D4</VMSize>
      </HeadNode>
      <LinuxComputeNodes>
        <VMNamePattern>lnxcn-%0001%</VMNamePattern>
        <ServiceNamePattern>mylnxcn%01%</ServiceNamePattern>
        <MaxNodeCountPerService>20</MaxNodeCountPerService>
        <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
        <VMSize>A9</VMSize>
        <NodeCount>0</NodeCount>
        <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
      </LinuxComputeNodes>
    </IaaSClusterConfig>

<span data-ttu-id="c4920-119">Spusťte hello hlavní uzel vytvoření spuštěním hello příkaz prostředí PowerShell v příkazovém řádku se zvýšenými oprávněními:</span><span class="sxs-lookup"><span data-stu-id="c4920-119">Start hello head-node creation by running hello PowerShell command in an elevated command prompt:</span></span>

```
    .\New-HPCIaaSCluster.ps1 -ConfigFile MyCluster.xml
```

<span data-ttu-id="c4920-120">Po 20 minutách too30 hello hlavního uzlu by měl být připraven.</span><span class="sxs-lookup"><span data-stu-id="c4920-120">After 20 too30 minutes, hello head node should be ready.</span></span> <span data-ttu-id="c4920-121">Tooit můžete připojit z portálu Azure hello kliknutím hello **Connect** ikonu hello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="c4920-121">You can connect tooit from hello Azure portal by clicking hello **Connect** icon of hello virtual machine.</span></span>

<span data-ttu-id="c4920-122">Nakonec máte toofix hello, server DNS pro předávání.</span><span class="sxs-lookup"><span data-stu-id="c4920-122">You might eventually have toofix hello DNS forwarder.</span></span> <span data-ttu-id="c4920-123">toodo Ano, spusťte Správce DNS.</span><span class="sxs-lookup"><span data-stu-id="c4920-123">toodo so, start DNS Manager.</span></span>

1. <span data-ttu-id="c4920-124">Klikněte pravým tlačítkem na název serveru hello ve Správci DNS, vyberte **vlastnosti**a potom klikněte na hello **předávání** kartě.</span><span class="sxs-lookup"><span data-stu-id="c4920-124">Right-click hello server name in DNS Manager, select **Properties**, and then click hello **Forwarders** tab.</span></span>
2. <span data-ttu-id="c4920-125">Klikněte na tlačítko hello **upravit** tlačítko tooremove veškeré služby předávání a potom klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="c4920-125">Click hello **Edit** button tooremove any forwarders, and then click **OK**.</span></span>
3. <span data-ttu-id="c4920-126">Ujistěte se, že hello **pomocí odkazů na kořenový server, pokud jsou k dispozici žádné servery pro předávání** zaškrtávací políčko je vybraná a pak klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c4920-126">Make sure that hello **Use root hints if no forwarders are available** check box is selected, and then click **OK**.</span></span>

## <a name="set-up-linux-compute-nodes"></a><span data-ttu-id="c4920-127">Nastavit Linuxových výpočetních uzlů</span><span class="sxs-lookup"><span data-stu-id="c4920-127">Set up Linux compute nodes</span></span>
<span data-ttu-id="c4920-128">Nasazení hello Linux výpočetní uzly s hello stejné šablony nasazení, kterou jste použili toocreate hello hlavního uzlu.</span><span class="sxs-lookup"><span data-stu-id="c4920-128">You deploy hello Linux compute nodes by using hello same deployment template that you used toocreate hello head node.</span></span>

<span data-ttu-id="c4920-129">Kopírovat soubor hello **MyCluster.xml** z hlavního uzlu toohello místního počítače a aktualizace hello **NodeCount** značka s číslem hello uzlů, které chcete toodeploy (< = 20).</span><span class="sxs-lookup"><span data-stu-id="c4920-129">Copy hello file **MyCluster.xml** from your local machine toohello head node, and update hello **NodeCount** tag with hello number of nodes that you want toodeploy (<=20).</span></span> <span data-ttu-id="c4920-130">Být opatrní toohave dostatek dostupné jader v rámci svojí Azure kvóty, protože každá instance A9 spotřebuje 16 jader v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="c4920-130">Be careful toohave enough available cores in your Azure quota, because each A9 instance will consume 16 cores in your subscription.</span></span> <span data-ttu-id="c4920-131">A8 instancí (8 jader) můžete použít místo A9, pokud chcete toouse více virtuálních počítačů v hello stejný rozpočet.</span><span class="sxs-lookup"><span data-stu-id="c4920-131">You can use A8 instances (8 cores) instead of A9 if you want toouse more VMs in hello same budget.</span></span>

<span data-ttu-id="c4920-132">Hello hlavního uzlu zkopírujte skriptů nasazení hello HPC Pack IaaS.</span><span class="sxs-lookup"><span data-stu-id="c4920-132">On hello head node, copy hello HPC Pack IaaS deployment scripts.</span></span>

<span data-ttu-id="c4920-133">Spusťte následující příkazy prostředí Azure PowerShell v příkazovém řádku se zvýšenými hello:</span><span class="sxs-lookup"><span data-stu-id="c4920-133">Run hello following Azure PowerShell commands in an elevated command prompt:</span></span>

1. <span data-ttu-id="c4920-134">Spustit **Add-AzureAccount** tooconnect tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="c4920-134">Run **Add-AzureAccount** tooconnect tooyour Azure subscription.</span></span>
2. <span data-ttu-id="c4920-135">Pokud máte více předplatných, spusťte **Get-AzureSubscription** toolist je.</span><span class="sxs-lookup"><span data-stu-id="c4920-135">If you have multiple subscriptions, run **Get-AzureSubscription** toolist them.</span></span>
3. <span data-ttu-id="c4920-136">Nastavit výchozí předplatné spuštěním hello **Select-AzureSubscription - Název_předplatného xxxx-výchozí** příkaz.</span><span class="sxs-lookup"><span data-stu-id="c4920-136">Set a default subscription by running hello **Select-AzureSubscription -SubscriptionName xxxx -Default** command.</span></span>
4. <span data-ttu-id="c4920-137">Spustit **.\New-HPCIaaSCluster.ps1 - ConfigFile MyCluster.xml** toostart nasazení Linuxových výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="c4920-137">Run **.\New-HPCIaaSCluster.ps1 -ConfigFile MyCluster.xml** toostart deploying Linux compute nodes.</span></span>
   
   ![Nasazení hlavního uzlu v akci][hndeploy]

<span data-ttu-id="c4920-139">Otevřete nástroj Správce clusterů HPC Pack hello.</span><span class="sxs-lookup"><span data-stu-id="c4920-139">Open hello HPC Pack Cluster Manager tool.</span></span> <span data-ttu-id="c4920-140">Za několik minut bude pravidelně Linux výpočetní uzly zobrazí v seznamu výpočetních uzlů clusteru.</span><span class="sxs-lookup"><span data-stu-id="c4920-140">After few minutes, Linux compute nodes will regularly appear in list of cluster compute nodes.</span></span> <span data-ttu-id="c4920-141">V režimu nasazení classic hello se vytvoří virtuální počítače IaaS postupně.</span><span class="sxs-lookup"><span data-stu-id="c4920-141">With hello classic deployment mode, IaaS VMs are created sequentially.</span></span> <span data-ttu-id="c4920-142">Takže pokud hello počet uzlů je důležité, pak získávání všechny nasazené může trvat dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="c4920-142">So if hello number of nodes is important, getting them all deployed can take a significant amount of time.</span></span>

![Linuxové uzly ve Správci clusteru HPC Pack][clustermanager]

<span data-ttu-id="c4920-144">Teď, když jsou všechny uzly jsou spuštěny v clusteru hello, existují další infrastrukturu toomake nastavení.</span><span class="sxs-lookup"><span data-stu-id="c4920-144">Now that all nodes are up and running in hello cluster, there are additional infrastructure settings toomake.</span></span>

## <a name="set-up-an-azure-file-share-for-windows-and-linux-nodes"></a><span data-ttu-id="c4920-145">Nastavit sdílená Azure pro Windows a Linux uzly</span><span class="sxs-lookup"><span data-stu-id="c4920-145">Set up an Azure File share for Windows and Linux nodes</span></span>
<span data-ttu-id="c4920-146">Můžete použít skripty toostore služby Azure File hello, balíčky aplikací a datových souborů.</span><span class="sxs-lookup"><span data-stu-id="c4920-146">You can use hello Azure File service toostore scripts, application packages, and data files.</span></span> <span data-ttu-id="c4920-147">Azure File nabízí funkce CIFS nad úložiště objektů Blob v Azure jako trvalé úložiště.</span><span class="sxs-lookup"><span data-stu-id="c4920-147">Azure File provides CIFS capabilities on top of Azure Blob storage as a persistent store.</span></span> <span data-ttu-id="c4920-148">Uvědomte si, že to není hello nejvíce škálovatelným řešením, ale je hello nejjednodušší jeden a nevyžaduje vyhrazených virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="c4920-148">Be aware that this is not hello most scalable solution, but it is hello simplest one and doesn’t require dedicated VMs.</span></span>

<span data-ttu-id="c4920-149">Vytvoření Azure sdílené složky podle hello pokynů v článku hello [Začínáme s Azure File storage ve Windows](../../../storage/files/storage-dotnet-how-to-use-files.md).</span><span class="sxs-lookup"><span data-stu-id="c4920-149">Create an Azure File share by following hello instructions in hello article [Get started with Azure File storage on Windows](../../../storage/files/storage-dotnet-how-to-use-files.md).</span></span>

<span data-ttu-id="c4920-150">Zachovat hello název účtu úložiště jako **saname**, název sdílené složky souborů hello jako **sharename**a klíč účtu úložiště hello jako **sakey**.</span><span class="sxs-lookup"><span data-stu-id="c4920-150">Keep hello name of your storage account as **saname**, hello file share name as **sharename**, and hello storage account key as **sakey**.</span></span>

### <a name="mount-hello-azure-file-share-on-hello-head-node"></a><span data-ttu-id="c4920-151">Připojit sdílenou složku Azure File hello hello hlavního uzlu</span><span class="sxs-lookup"><span data-stu-id="c4920-151">Mount hello Azure File share on hello head node</span></span>
<span data-ttu-id="c4920-152">Otevřete příkazový řádek se zvýšenými oprávněními a spusťte následující příkaz toostore hello přihlašovací údaje v úložišti místního počítače hello hello:</span><span class="sxs-lookup"><span data-stu-id="c4920-152">Open an elevated command prompt and run hello following command toostore hello credentials in hello local machine vault:</span></span>

```
    cmdkey /add:<saname>.file.core.windows.net /user:<saname> /pass:<sakey>
```

<span data-ttu-id="c4920-153">Potom toomount hello sdílenou složku Azure File, spusťte:</span><span class="sxs-lookup"><span data-stu-id="c4920-153">Then, toomount hello Azure File share, run:</span></span>

```
    net use Z: \\<saname>.file.core.windows.net\<sharename> /persistent:yes
```

### <a name="mount-hello-azure-file-share-on-linux-compute-nodes"></a><span data-ttu-id="c4920-154">Připojit sdílenou složku Azure File hello na Linuxových výpočetních uzlů</span><span class="sxs-lookup"><span data-stu-id="c4920-154">Mount hello Azure File share on Linux compute nodes</span></span>
<span data-ttu-id="c4920-155">Užitečné nástroj, který se dodává s HPC Pack je nástroj clusrun hello.</span><span class="sxs-lookup"><span data-stu-id="c4920-155">One useful tool that comes with HPC Pack is hello clusrun tool.</span></span> <span data-ttu-id="c4920-156">Tento nástroj příkazového řádku toorun hello stejný příkaz můžete použít současně se sadou výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="c4920-156">You can use this command-line tool toorun hello same command simultaneously on a set of compute nodes.</span></span> <span data-ttu-id="c4920-157">V našem případě se používá sdílenou složku Azure File hello toomount a zachovat ji toosurvive restartování.</span><span class="sxs-lookup"><span data-stu-id="c4920-157">In our case, it's used toomount hello Azure File share and persist it toosurvive reboots.</span></span>
<span data-ttu-id="c4920-158">V příkazovém řádku se zvýšenými hello hlavního uzlu spusťte následující příkazy hello.</span><span class="sxs-lookup"><span data-stu-id="c4920-158">In an elevated command prompt on hello head node, run hello following commands.</span></span>

<span data-ttu-id="c4920-159">toocreate hello přípojného adresáře:</span><span class="sxs-lookup"><span data-stu-id="c4920-159">toocreate hello mount directory:</span></span>

```
    clusrun /nodegroup:LinuxNodes mkdir -p /hpcdata
```

<span data-ttu-id="c4920-160">toomount hello sdílenou složku Azure:</span><span class="sxs-lookup"><span data-stu-id="c4920-160">toomount hello Azure File share:</span></span>

```
    clusrun /nodegroup:LinuxNodes mount -t cifs //<saname>.file.core.windows.net/<sharename> /hpcdata -o vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777
```

<span data-ttu-id="c4920-161">toopersist hello připojení sdílené složky:</span><span class="sxs-lookup"><span data-stu-id="c4920-161">toopersist hello mount share:</span></span>

```
    clusrun /nodegroup:LinuxNodes "echo //<saname>.file.core.windows.net/<sharename> /hpcdata cifs vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777 >> /etc/fstab"
```

## <a name="install-star-ccm"></a><span data-ttu-id="c4920-162">Nainstalujte HVĚZDIČKY – CCM +</span><span class="sxs-lookup"><span data-stu-id="c4920-162">Install STAR-CCM+</span></span>
<span data-ttu-id="c4920-163">Instancemi Azure virtuální počítač A8 a A9 poskytnout podporu InfiniBand a RDMA.</span><span class="sxs-lookup"><span data-stu-id="c4920-163">Azure VM instances A8 and A9 provide InfiniBand support and RDMA capabilities.</span></span> <span data-ttu-id="c4920-164">Hello jádra ovladače, které umožňují tyto funkce jsou dostupné pro Windows Server 2012 R2, SUSE 12, CentOS 6.5 a CentOS 7.1 obrázků v hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="c4920-164">hello kernel drivers that enable those capabilities are available for Windows Server 2012 R2, SUSE 12, CentOS 6.5, and CentOS 7.1 images in hello Azure Marketplace.</span></span> <span data-ttu-id="c4920-165">Microsoft MPI a Intel MPI (verze 5.x) jsou hello dvě MPI knihovny, které podporují tyto ovladače v Azure.</span><span class="sxs-lookup"><span data-stu-id="c4920-165">Microsoft MPI and Intel MPI (release 5.x) are hello two MPI libraries that support those drivers in Azure.</span></span>

<span data-ttu-id="c4920-166">CD adapco HVĚZDIČKY – CCM + verze 11.x a později je instalován s verzí Intel MPI 5.x, tak, aby zahrnuté InfiniBand podpora pro Azure.</span><span class="sxs-lookup"><span data-stu-id="c4920-166">CD-adapco STAR-CCM+ release 11.x and later is bundled with Intel MPI version 5.x, so InfiniBand support for Azure is included.</span></span>

<span data-ttu-id="c4920-167">Získat hello Linux64 HVĚZDIČKY – CCM + balíček z hello [CD adapco portál](https://steve.cd-adapco.com).</span><span class="sxs-lookup"><span data-stu-id="c4920-167">Get hello Linux64 STAR-CCM+ package from hello [CD-adapco portal](https://steve.cd-adapco.com).</span></span> <span data-ttu-id="c4920-168">V našem případě jsme použili verze 11.02.010 ve smíšeném přesnost.</span><span class="sxs-lookup"><span data-stu-id="c4920-168">In our case, we used version 11.02.010 in mixed precision.</span></span>

<span data-ttu-id="c4920-169">Hello hlavního uzlu v hello **/hpcdata** Azure File sdílenou složku, vytvořit skript prostředí s názvem **setupstarccm.sh** s hello následující obsah.</span><span class="sxs-lookup"><span data-stu-id="c4920-169">On hello head node, in hello **/hpcdata** Azure File share, create a shell script named **setupstarccm.sh** with hello following content.</span></span> <span data-ttu-id="c4920-170">Tento skript se spustí na každý výpočetní uzel tooset až HVĚZDIČKY – CCM + místně.</span><span class="sxs-lookup"><span data-stu-id="c4920-170">This script will be run on each compute node tooset up STAR-CCM+ locally.</span></span>

#### <a name="sample-setupstarcmsh-script"></a><span data-ttu-id="c4920-171">Ukázkový skript setupstarcm.sh</span><span class="sxs-lookup"><span data-stu-id="c4920-171">Sample setupstarcm.sh script</span></span>
```
    #!/bin/bash
    # setupstarcm.sh tooset up STAR-CCM+ locally

    # Create hello CD-adapco main directory
    mkdir -p /opt/CD-adapco

    # Copy hello STAR-CCM package from hello file share toohello local directory
    cp /hpcdata/StarCCM/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz /opt/CD-adapco/

    # Extract hello package
    tar -xzf /opt/CD-adapco/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz -C /opt/CD-adapco/

    # Start a silent installation of STAR-CCM without hello FLEXlm component
    /opt/CD-adapco/starccm+_11.02.010/STAR-CCM+11.02.010_01_linux-x86_64-2.5_gnu4.8.bin -i silent -DCOMPUTE_NODE=true -DNODOC=true -DINSTALLFLEX=false

    # Update memory limits
    echo "*               hard    memlock         unlimited" >> /etc/security/limits.conf
    echo "*               soft    memlock         unlimited" >> /etc/security/limits.conf
```
<span data-ttu-id="c4920-172">Nyní, tooset až HVĚZDIČKY – CCM + na všechny Linux výpočetní uzly, otevřete příkazový řádek se zvýšenými oprávněními a spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="c4920-172">Now, tooset up STAR-CCM+ on all your Linux compute nodes, open an elevated command prompt and run hello following command:</span></span>

```
    clusrun /nodegroup:LinuxNodes bash /hpcdata/setupstarccm.sh
```

<span data-ttu-id="c4920-173">Když je spuštěný příkaz hello, můžete sledovat využití procesoru hello pomocí hello heat mapa ze Správce clusteru.</span><span class="sxs-lookup"><span data-stu-id="c4920-173">While hello command is running, you can monitor hello CPU usage by using hello heat map of Cluster Manager.</span></span> <span data-ttu-id="c4920-174">Za několik minut všechny uzly by měl být správně nastavena.</span><span class="sxs-lookup"><span data-stu-id="c4920-174">After few minutes, all nodes should be correctly set up.</span></span>

## <a name="run-star-ccm-jobs"></a><span data-ttu-id="c4920-175">Spustit HVĚZDIČKY – CCM + úlohy</span><span class="sxs-lookup"><span data-stu-id="c4920-175">Run STAR-CCM+ jobs</span></span>
<span data-ttu-id="c4920-176">HPC Pack se používá pro možnosti plánovače úloh v pořadí toorun HVĚZDIČKY – CCM + úlohy.</span><span class="sxs-lookup"><span data-stu-id="c4920-176">HPC Pack is used for its job scheduler capabilities in order toorun STAR-CCM+ jobs.</span></span> <span data-ttu-id="c4920-177">toodo tak, budeme potřebovat hello podporu několik skriptů, které jsou používané toostart hello úlohy a spusťte HVĚZDIČKY – CCM +.</span><span class="sxs-lookup"><span data-stu-id="c4920-177">toodo so, we need hello support of a few scripts that are used toostart hello job and run STAR-CCM+.</span></span> <span data-ttu-id="c4920-178">Hello vstupní data se ukládají na sdílenou složku Azure File hello první pro jednoduchost.</span><span class="sxs-lookup"><span data-stu-id="c4920-178">hello input data is kept on hello Azure File share first for simplicity.</span></span>

<span data-ttu-id="c4920-179">Následující skript prostředí PowerShell Hello je použité tooqueue HVĚZDU – CCM + úlohy.</span><span class="sxs-lookup"><span data-stu-id="c4920-179">hello following PowerShell script is used tooqueue a STAR-CCM+ job.</span></span> <span data-ttu-id="c4920-180">Trvá tři argumenty:</span><span class="sxs-lookup"><span data-stu-id="c4920-180">It takes three arguments:</span></span>

* <span data-ttu-id="c4920-181">Název modelu Hello</span><span class="sxs-lookup"><span data-stu-id="c4920-181">hello model name</span></span>
* <span data-ttu-id="c4920-182">Hello počet uzlů toobe použít</span><span class="sxs-lookup"><span data-stu-id="c4920-182">hello number of nodes toobe used</span></span>
* <span data-ttu-id="c4920-183">Hello počet jader na každý uzel toobe použít</span><span class="sxs-lookup"><span data-stu-id="c4920-183">hello number of cores on each node toobe used</span></span>

<span data-ttu-id="c4920-184">Protože HVĚZDIČKY – CCM + můžete vyplnit hello paměti šířky pásma, jeho obvykle lepší toouse menší počet jader na výpočetní uzly a přidejte nové uzly.</span><span class="sxs-lookup"><span data-stu-id="c4920-184">Because STAR-CCM+ can fill hello memory bandwidth, it's usually better toouse fewer cores per compute nodes and add new nodes.</span></span> <span data-ttu-id="c4920-185">Hello přesný počet jader na uzel, bude záviset na třídu hello procesoru a rychlost propojení hello.</span><span class="sxs-lookup"><span data-stu-id="c4920-185">hello exact number of cores per node will depend on hello processor family and hello interconnect speed.</span></span>

<span data-ttu-id="c4920-186">Hello uzly jsou přiděleny výhradně pro úlohu hello a nemohou být sdíleny s ostatními úlohami.</span><span class="sxs-lookup"><span data-stu-id="c4920-186">hello nodes are allocated exclusively for hello job and can’t be shared with other jobs.</span></span> <span data-ttu-id="c4920-187">Úloha Hello není spuštěna jako úloha MPI přímo.</span><span class="sxs-lookup"><span data-stu-id="c4920-187">hello job is not started as an MPI job directly.</span></span> <span data-ttu-id="c4920-188">Hello **runstarccm.sh** Spouštěč MPI hello se spustit skript prostředí.</span><span class="sxs-lookup"><span data-stu-id="c4920-188">hello **runstarccm.sh** shell script will start hello MPI launcher.</span></span>

<span data-ttu-id="c4920-189">Hello vstupní modelu a hello **runstarccm.sh** skriptů jsou uložené v hello **/hpcdata** sdílené složce, která byla dříve připojena.</span><span class="sxs-lookup"><span data-stu-id="c4920-189">hello input model and hello **runstarccm.sh** script are stored in hello **/hpcdata** share that was previously mounted.</span></span>

<span data-ttu-id="c4920-190">Soubory protokolu jsou pojmenované s ID úlohy hello a jsou uloženy v hello **/hpcdata sdílení**, společně s hello HVĚZDIČKY – CCM + výstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="c4920-190">Log files are named with hello job ID and are stored in hello **/hpcdata share**, along with hello STAR-CCM+ output files.</span></span>

#### <a name="sample-submitstarccmjobps1-script"></a><span data-ttu-id="c4920-191">Ukázkový skript SubmitStarccmJob.ps1</span><span class="sxs-lookup"><span data-stu-id="c4920-191">Sample SubmitStarccmJob.ps1 script</span></span>
```
    Add-PSSnapin Microsoft.HPC -ErrorAction silentlycontinue
    $scheduler="headnodename"
    $modelName=$args[0]
    $nbCoresPerNode=$args[2]
    $nbNodes=$args[1]

    #---------------------------------------------------------------------------------------------------------
    # Create a new job; this will give us hello job ID that's used tooidentify hello name of hello uploaded package in Azure
    #
    $job = New-HpcJob -Name "$modelName $nbNodes $nbCoresPerNode" -Scheduler $scheduler -NumNodes $nbNodes -NodeGroups "LinuxNodes" -FailOnTaskFailure $true -Exclusive $true
    $jobId = [String]$job.Id

    #---------------------------------------------------------------------------------------------------------
    # Submit hello job     
    $workdir =  "/hpcdata"
    $execName = "$nbCoresPerNode runner.java $modelName.sim"

    $job | Add-HpcTask -Scheduler $scheduler -Name "Compute" -stdout "$jobId.log" -stderr "$jobId.err" -Rerunnable $false -NumNodes $nbNodes -Command "runstarccm.sh $execName" -WorkDir "$workdir"


    Submit-HpcJob -Job $job -Scheduler $scheduler
```
<span data-ttu-id="c4920-192">Nahraďte **runner.java** vaše upřednostňované hvězdičkou-Spouštěče modelu CCM + Java a kód protokolování.</span><span class="sxs-lookup"><span data-stu-id="c4920-192">Replace **runner.java** with your preferred STAR-CCM+ Java model launcher and logging code.</span></span>

#### <a name="sample-runstarccmsh-script"></a><span data-ttu-id="c4920-193">Ukázkový skript runstarccm.sh</span><span class="sxs-lookup"><span data-stu-id="c4920-193">Sample runstarccm.sh script</span></span>
```
    #!/bin/bash
    echo "start"
    # hello path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    echo ${SCRIPT_PATH}
    # Set hello mpirun runtime environment
    export CDLMD_LICENSE_FILE=1999@flex.cd-adapco.com

    # mpirun command
    STARCCM=/opt/CD-adapco/STAR-CCM+11.02.010/star/bin/starccm+

    # Get node information from ENVs
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    NBCORESPERNODE=$1

    # Create hello hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$
    echo ${NODELIST_PATH}

    # Get every node name and write into hello hostfile file
    I=1
    NBNODES=0
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
        let "NBNODES=${NBNODES}+1"
    done
    let "NBCORES=${NBNODES}*${NBCORESPERNODE}"

    # Run STAR-CCM with hello hostfile argument
    #  
    ${STARCCM} -np ${NBCORES} -machinefile ${NODELIST_PATH} \
        -power -podkey "<yourkey>" -rsh ssh \
        -mpi intel -fabric UDAPL -cpubind bandwidth,v \
        -mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0" \
        -batch $2 $3
    RTNSTS=$?
    rm -f ${NODELIST_PATH}

    exit ${RTNSTS}
```

<span data-ttu-id="c4920-194">V našem testu jsme použili token licence Power na vyžádání.</span><span class="sxs-lookup"><span data-stu-id="c4920-194">In our test, we used a Power-On-Demand license token.</span></span> <span data-ttu-id="c4920-195">Token, je nutné tooset hello **$CDLMD_LICENSE_FILE** proměnnou prostředí příliš **1999@flex.cd-adapco.com**  a hello klíč v hello **- podkey** možnost hello příkazového řádku .</span><span class="sxs-lookup"><span data-stu-id="c4920-195">For that token, you have tooset hello **$CDLMD_LICENSE_FILE** environment variable too**1999@flex.cd-adapco.com** and hello key in hello **-podkey** option of hello command line.</span></span>

<span data-ttu-id="c4920-196">Po inicializaci, skript hello extrahuje--z hello **$CCP_NODES_CORES** proměnné prostředí, které HPC Pack nastavit – hello seznam uzlů toobuild hostfile, který hello MPI Spouštěč používá.</span><span class="sxs-lookup"><span data-stu-id="c4920-196">After some initialization, hello script extracts--from hello **$CCP_NODES_CORES** environment variables that HPC Pack set--hello list of nodes toobuild a hostfile that hello MPI launcher uses.</span></span> <span data-ttu-id="c4920-197">Tato hostfile bude obsahovat seznam hello výpočetní uzel názvy, které se používají pro úlohu hello, jeden název na každý řádek.</span><span class="sxs-lookup"><span data-stu-id="c4920-197">This hostfile will contain hello list of compute node names that are used for hello job, one name per line.</span></span>

<span data-ttu-id="c4920-198">Formát Hello **$CCP_NODES_CORES** následuje tento vzor:</span><span class="sxs-lookup"><span data-stu-id="c4920-198">hello format of **$CCP_NODES_CORES** follows this pattern:</span></span>

```
<Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
```

<span data-ttu-id="c4920-199">Kde:</span><span class="sxs-lookup"><span data-stu-id="c4920-199">Where:</span></span>

* <span data-ttu-id="c4920-200">`<Number of nodes>`je hello počet uzlů přidělených toothis úlohy.</span><span class="sxs-lookup"><span data-stu-id="c4920-200">`<Number of nodes>` is hello number of nodes allocated toothis job.</span></span>
* <span data-ttu-id="c4920-201">`<Name of node_n_...>`je název hello každého uzlu přidělené toothis úlohy.</span><span class="sxs-lookup"><span data-stu-id="c4920-201">`<Name of node_n_...>` is hello name of each node allocated toothis job.</span></span>
* <span data-ttu-id="c4920-202">`<Cores of node_n_...>`je hello počet jader na uzel hello přidělené toothis úlohy.</span><span class="sxs-lookup"><span data-stu-id="c4920-202">`<Cores of node_n_...>` is hello number of cores on hello node allocated toothis job.</span></span>

<span data-ttu-id="c4920-203">Hello počet jader (**$NBCORES**) je také počítané hello na základě počtu uzlů (**$NBNODES**) a hello počet jader na uzel (jako parametr **$NBCORESPERNODE**).</span><span class="sxs-lookup"><span data-stu-id="c4920-203">hello number of cores (**$NBCORES**) is also calculated based on hello number of nodes (**$NBNODES**) and hello number of cores per node (provided as parameter **$NBCORESPERNODE**).</span></span>

<span data-ttu-id="c4920-204">Možnosti MPI hello hello ty, které se používají s Intel MPI v Azure jsou:</span><span class="sxs-lookup"><span data-stu-id="c4920-204">For hello MPI options, hello ones that are used with Intel MPI on Azure are:</span></span>

* <span data-ttu-id="c4920-205">`-mpi intel`toospecify Intel MPI.</span><span class="sxs-lookup"><span data-stu-id="c4920-205">`-mpi intel` toospecify Intel MPI.</span></span>
* <span data-ttu-id="c4920-206">`-fabric UDAPL`příkazy toouse Azure InfiniBand.</span><span class="sxs-lookup"><span data-stu-id="c4920-206">`-fabric UDAPL` toouse Azure InfiniBand verbs.</span></span>
* <span data-ttu-id="c4920-207">`-cpubind bandwidth,v`toooptimize šířky pásma pro MPI s HVĚZDIČKOU – CCM +.</span><span class="sxs-lookup"><span data-stu-id="c4920-207">`-cpubind bandwidth,v` toooptimize bandwidth for MPI with STAR-CCM+.</span></span>
* <span data-ttu-id="c4920-208">`-mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0"`toomake Intel MPI pracovat s Azure InfiniBand a tooset hello požadovaný počet jader na uzel.</span><span class="sxs-lookup"><span data-stu-id="c4920-208">`-mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0"` toomake Intel MPI work with Azure InfiniBand, and tooset hello required number of cores per node.</span></span>
* <span data-ttu-id="c4920-209">`-batch`toostart HVĚZDIČKY – CCM + v dávkovém režimu s žádné uživatelské rozhraní.</span><span class="sxs-lookup"><span data-stu-id="c4920-209">`-batch` toostart STAR-CCM+ in batch mode with no UI.</span></span>

<span data-ttu-id="c4920-210">Nakonec toostart úlohu, ujistěte se, že uzly jsou spuštěny a jsou online ve Správci clusteru.</span><span class="sxs-lookup"><span data-stu-id="c4920-210">Finally, toostart a job, make sure that your nodes are up and running and are online in Cluster Manager.</span></span> <span data-ttu-id="c4920-211">Z příkazového řádku prostředí PowerShell, spusťte toto:</span><span class="sxs-lookup"><span data-stu-id="c4920-211">Then from a PowerShell command prompt, run this:</span></span>

```
    .\ SubmitStarccmJob.ps1 <model> <nbNodes> <nbCoresPerNode>
```

## <a name="stop-nodes"></a><span data-ttu-id="c4920-212">Zastavit uzly</span><span class="sxs-lookup"><span data-stu-id="c4920-212">Stop nodes</span></span>
<span data-ttu-id="c4920-213">Později na po dokončení testů, můžete použít následující příkazy toostop prostředí HPC Pack PowerShell hello a spustit uzly:</span><span class="sxs-lookup"><span data-stu-id="c4920-213">Later on, after you're done with your tests, you can use hello following HPC Pack PowerShell commands toostop and start nodes:</span></span>

```
    Stop-HPCIaaSNode.ps1 -Name <prefix>-00*
    Start-HPCIaaSNode.ps1 -Name <prefix>-00*
```

## <a name="next-steps"></a><span data-ttu-id="c4920-214">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c4920-214">Next steps</span></span>
<span data-ttu-id="c4920-215">Zkuste spuštěny další zátěže systému Linux.</span><span class="sxs-lookup"><span data-stu-id="c4920-215">Try running other Linux workloads.</span></span> <span data-ttu-id="c4920-216">Například v tématu:</span><span class="sxs-lookup"><span data-stu-id="c4920-216">For example, see:</span></span>

* [<span data-ttu-id="c4920-217">Spuštění NAMD pomocí sady Microsoft HPC Pack v systému Linux výpočetních uzlech v Azure</span><span class="sxs-lookup"><span data-stu-id="c4920-217">Run NAMD with Microsoft HPC Pack on Linux compute nodes in Azure</span></span>](hpcpack-cluster-namd.md)
* [<span data-ttu-id="c4920-218">Spustit OpenFOAM na clusteru s podporou Linux RDMA v Azure pomocí sady Microsoft HPC Pack</span><span class="sxs-lookup"><span data-stu-id="c4920-218">Run OpenFOAM with Microsoft HPC Pack on a Linux RDMA cluster in Azure</span></span>](hpcpack-cluster-openfoam.md)

<!--Image references-->
[hndeploy]:media/hpcpack-cluster-starccm/hndeploy.png
[clustermanager]:media/hpcpack-cluster-starccm/ClusterManager.png
