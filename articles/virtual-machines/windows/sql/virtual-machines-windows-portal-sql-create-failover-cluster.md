---
title: aaaSQL Server FCI - Azure Virtual Machines | Microsoft Docs
description: "Tento článek vysvětluje, jak toocreate Instance clusteru převzetí služeb při selhání SQL serveru na virtuálních počítačích Azure."
services: virtual-machines
documentationCenter: na
authors: MikeRayMSFT
manager: jhubbard
editor: monicar
tags: azure-service-management
ms.assetid: 9fc761b1-21ad-4d79-bebc-a2f094ec214d
ms.service: virtual-machines-sql
ms.devlang: na
ms.custom: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: bee3b27805c5f6cc02a43b25d480c129c254cb90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="configure-sql-server-failover-cluster-instance-on-azure-virtual-machines"></a><span data-ttu-id="d5bd0-103">Konfigurace Instance clusteru převzetí služeb při selhání systému SQL Server na virtuálních počítačích Azure</span><span class="sxs-lookup"><span data-stu-id="d5bd0-103">Configure SQL Server Failover Cluster Instance on Azure Virtual Machines</span></span>

<span data-ttu-id="d5bd0-104">Tento článek vysvětluje, jak toocreate selhání SQL serveru Cluster Instance (FCI) na virtuálních počítačích Azure v modelu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-104">This article explains how toocreate a SQL Server Failover Cluster Instance (FCI) on Azure virtual machines in Resource Manager model.</span></span> <span data-ttu-id="d5bd0-105">Toto řešení používá [Windows Server 2016 Datacenter edition prostory úložiště – přímé \(S2D\) ](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview) jako softwarová virtuální síť SAN, synchronizuje hello úložiště (datových disků) mezi hello uzlech (virtuálních počítačích Azure) v Clusteru se systémem Windows.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-105">This solution uses [Windows Server 2016 Datacenter edition Storage Spaces Direct \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview) as a software-based virtual SAN that synchronizes hello storage (data disks) between hello nodes (Azure VMs) in a Windows Cluster.</span></span> <span data-ttu-id="d5bd0-106">S2D je nového v systému Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-106">S2D is new in Windows Server 2016.</span></span>

<span data-ttu-id="d5bd0-107">Hello následující diagram znázorňuje hello kompletního řešení na virtuálních počítačích Azure:</span><span class="sxs-lookup"><span data-stu-id="d5bd0-107">hello following diagram shows hello complete solution on Azure virtual machines:</span></span>

![Skupiny dostupnosti](./media/virtual-machines-windows-portal-sql-create-failover-cluster/00-sql-fci-s2d-complete-solution.png)

<span data-ttu-id="d5bd0-109">Hello předcházející zobrazuje diagram:</span><span class="sxs-lookup"><span data-stu-id="d5bd0-109">hello preceding diagram shows:</span></span>

- <span data-ttu-id="d5bd0-110">Dva virtuální počítače Azure v clusteru s podporou převzetí služeb při selhání systému Windows.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-110">Two Azure virtual machines in a Windows Failover Cluster.</span></span> <span data-ttu-id="d5bd0-111">Pokud virtuální počítač v clusteru s podporou převzetí služeb při selhání se také označuje jako *uzlu clusteru*, nebo *uzly*.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-111">When a virtual machine is in a failover cluster it is also called a *cluster node*, or *nodes*.</span></span>
- <span data-ttu-id="d5bd0-112">Každý virtuální počítač má dva nebo více datových disků.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-112">Each virtual machine has two or more data disks.</span></span>
- <span data-ttu-id="d5bd0-113">S2D synchronizuje data hello na hello datový disk a uvede hello synchronizované úložiště jako fond úložiště.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-113">S2D synchronizes hello data on hello data disk and presents hello synchronized storage as a storage pool.</span></span>
- <span data-ttu-id="d5bd0-114">fond úložiště Hello uvede clusteru převzetí služeb při selhání toohello (CSV) pro sdílený svazek clusteru.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-114">hello storage pool presents a cluster shared volume (CSV) toohello failover cluster.</span></span>
- <span data-ttu-id="d5bd0-115">role clusteru SQL serveru FCI Hello používá hello CSV hello datových jednotek.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-115">hello SQL Server FCI cluster role uses hello CSV for hello data drives.</span></span>
- <span data-ttu-id="d5bd0-116">Zatížení Azure vyrovnávání toohold hello IP adresu pro SQL Server FCI hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-116">An Azure load balancer toohold hello IP address for hello SQL Server FCI.</span></span>
- <span data-ttu-id="d5bd0-117">Nastavení Azure dostupnosti obsahuje všechny prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-117">An Azure availability set holds all hello resources.</span></span>

   >[!NOTE]
   ><span data-ttu-id="d5bd0-118">Všechny prostředky Azure jsou v diagramu hello v hello stejnou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-118">All Azure resources are in hello diagram are in hello same resource group.</span></span>

<span data-ttu-id="d5bd0-119">Podrobnosti o S2D najdete v tématu [Windows Server 2016 Datacenter edition prostory úložiště – přímé \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview).</span><span class="sxs-lookup"><span data-stu-id="d5bd0-119">For details about S2D, see [Windows Server 2016 Datacenter edition Storage Spaces Direct \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview).</span></span>

<span data-ttu-id="d5bd0-120">S2D podporuje dva typy architektury - sblížené a hyperkonvergované.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-120">S2D supports two types of architectures - converged and hyper-converged.</span></span> <span data-ttu-id="d5bd0-121">Architektura Hello v tomto dokumentu je hyperkonvergované.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-121">hello architecture in this document is hyper-converged.</span></span> <span data-ttu-id="d5bd0-122">Úložiště hello místech hyperkonvergované infrastruktury na hello stejných serverů aplikace hello clusteru hostitele.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-122">A hyper-converged infrastructure places hello storage on hello same servers that host hello clustered application.</span></span> <span data-ttu-id="d5bd0-123">V této architektuře úložiště hello je na každém uzlu SQL serveru FCI.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-123">In this architecture, hello storage is on each SQL Server FCI node.</span></span>

### <a name="example-azure-template"></a><span data-ttu-id="d5bd0-124">Příklad šablony Azure</span><span class="sxs-lookup"><span data-stu-id="d5bd0-124">Example Azure template</span></span>

<span data-ttu-id="d5bd0-125">Hello celé řešení v Azure můžete vytvořit ze šablony.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-125">You can create hello entire solution in Azure from a template.</span></span> <span data-ttu-id="d5bd0-126">Příklad šablony je k dispozici v hello Githubu [šablon Azure rychlý Start](https://github.com/MSBrett/azure-quickstart-templates/tree/master/sql-server-2016-fci-existing-vnet-and-ad).</span><span class="sxs-lookup"><span data-stu-id="d5bd0-126">An example of a template is available in hello GitHub [Azure Quickstart Templates](https://github.com/MSBrett/azure-quickstart-templates/tree/master/sql-server-2016-fci-existing-vnet-and-ad).</span></span> <span data-ttu-id="d5bd0-127">V tomto příkladu není určená nebo testována pro všechny konkrétní úlohu.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-127">This example is not designed or tested for any specific workload.</span></span> <span data-ttu-id="d5bd0-128">Hello šablony toocreate může běžet SQL Server FCI s S2D úložiště připojené tooyour domény.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-128">You can run hello template toocreate a SQL Server FCI with S2D storage connected tooyour domain.</span></span> <span data-ttu-id="d5bd0-129">Můžete vyhodnotit hello šablony a upravit pro vaše záměry.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-129">You can evaluate hello template, and modify it for your purposes.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="d5bd0-130">Než začnete</span><span class="sxs-lookup"><span data-stu-id="d5bd0-130">Before you begin</span></span>

<span data-ttu-id="d5bd0-131">Existuje několik možností, které budete potřebovat tooknow a několik věcí, které musíte v místě, než budete pokračovat.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-131">There are a few things you need tooknow and a couple of things that you need in place before you proceed.</span></span>

### <a name="what-tooknow"></a><span data-ttu-id="d5bd0-132">Jaké tooknow</span><span class="sxs-lookup"><span data-stu-id="d5bd0-132">What tooknow</span></span>
<span data-ttu-id="d5bd0-133">Musí mít provozní znalosti o hello následující technologie:</span><span class="sxs-lookup"><span data-stu-id="d5bd0-133">You should have an operational understanding of hello following technologies:</span></span>

- [<span data-ttu-id="d5bd0-134">Technologie clusteru systému Windows</span><span class="sxs-lookup"><span data-stu-id="d5bd0-134">Windows cluster technologies</span></span>](http://technet.microsoft.com/library/hh831579.aspx)
-  <span data-ttu-id="d5bd0-135">[Instance clusteru převzetí služeb při selhání SQL serveru](http://msdn.microsoft.com/library/ms189134.aspx).</span><span class="sxs-lookup"><span data-stu-id="d5bd0-135">[SQL Server Failover Cluster Instances](http://msdn.microsoft.com/library/ms189134.aspx).</span></span>

<span data-ttu-id="d5bd0-136">Kromě toho musí mít obecné znalosti o hello následující technologie:</span><span class="sxs-lookup"><span data-stu-id="d5bd0-136">Also, you should have a general understanding of hello following technologies:</span></span>

- [<span data-ttu-id="d5bd0-137">Konvergované Hyper řešení pomocí prostory úložiště – přímé v systému Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="d5bd0-137">Hyper-converged solution using Storage Spaces Direct in Windows Server 2016</span></span>](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct)
- [<span data-ttu-id="d5bd0-138">Skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-138">Azure resource groups</span></span>](../../../azure-resource-manager/resource-group-portal.md)

### <a name="what-toohave"></a><span data-ttu-id="d5bd0-139">Jaké toohave</span><span class="sxs-lookup"><span data-stu-id="d5bd0-139">What toohave</span></span>

<span data-ttu-id="d5bd0-140">Než budete postupovat hello pokyny v tomto článku, byste již měli mít:</span><span class="sxs-lookup"><span data-stu-id="d5bd0-140">Before following hello instructions in this article, you should already have:</span></span>

- <span data-ttu-id="d5bd0-141">Předplatné Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-141">A Microsoft Azure subscription.</span></span>
- <span data-ttu-id="d5bd0-142">Domény systému Windows na virtuálních počítačích Azure.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-142">A Windows domain on Azure virtual machines.</span></span>
- <span data-ttu-id="d5bd0-143">Účet s objekty toocreate oprávnění v hello virtuální počítač Azure.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-143">An account with permission toocreate objects in hello Azure virtual machine.</span></span>
- <span data-ttu-id="d5bd0-144">Virtuální síť Azure a podsíť s dostatečný Adresní prostor IP adres pro hello následující součásti:</span><span class="sxs-lookup"><span data-stu-id="d5bd0-144">An Azure virtual network and subnet with sufficient IP address space for hello following components:</span></span>
   - <span data-ttu-id="d5bd0-145">Virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-145">Both virtual machines.</span></span>
   - <span data-ttu-id="d5bd0-146">IP adresu clusteru převzetí služeb při selhání Hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-146">hello failover cluster IP address.</span></span>
   - <span data-ttu-id="d5bd0-147">IP adresa pro každý FCI.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-147">An IP address for each FCI.</span></span>
- <span data-ttu-id="d5bd0-148">DNS nakonfigurovaný v hello síť Azure, odkazující toohello řadiče domény.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-148">DNS configured on hello Azure Network, pointing toohello domain controllers.</span></span>

<span data-ttu-id="d5bd0-149">Tyto požadavky splněny můžete pokračovat s vytvářením clusteru převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-149">With these prerequisites in place, you can proceed with building your failover cluster.</span></span> <span data-ttu-id="d5bd0-150">prvním krokem Hello je toocreate hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-150">hello first step is toocreate hello virtual machines.</span></span>

## <a name="step-1-create-virtual-machines"></a><span data-ttu-id="d5bd0-151">Krok 1: Vytvoření virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="d5bd0-151">Step 1: Create virtual machines</span></span>

1. <span data-ttu-id="d5bd0-152">Přihlaste se toohello [portál Azure](http://portal.azure.com) s vaším předplatným.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-152">Log in toohello [Azure portal](http://portal.azure.com) with your subscription.</span></span>

1. <span data-ttu-id="d5bd0-153">[Vytvořit skupinu dostupnosti Azure](../tutorial-availability-sets.md).</span><span class="sxs-lookup"><span data-stu-id="d5bd0-153">[Create an Azure availability set](../tutorial-availability-sets.md).</span></span>

   <span data-ttu-id="d5bd0-154">Hello dostupnosti v rámci domén selhání nastavit skupiny virtuálních počítačů a aktualizaci domény.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-154">hello availability set groups virtual machines across fault domains and update domains.</span></span> <span data-ttu-id="d5bd0-155">Skupina dostupnosti Hello zajišťuje, že vaše aplikace není ovlivněn jediný bod selhání, jako je síťový přepínač hello nebo jednotka power hello rack serverů.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-155">hello availability set makes sure that your application is not affected by single points of failure, like hello network switch or hello power unit of a rack of servers.</span></span>

   <span data-ttu-id="d5bd0-156">Pokud jste dosud nevytvořili hello skupinu prostředků pro virtuální počítače, když vytvoříte skupinu dostupnosti Azure ho proveďte.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-156">If you have not created hello resource group for your virtual machines, do it when you create an Azure availability set.</span></span> <span data-ttu-id="d5bd0-157">Pokud používáte skupiny dostupnosti hello Azure portálu toocreate hello, hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d5bd0-157">If you're using hello Azure portal toocreate hello availability set, do hello following steps:</span></span>

   - <span data-ttu-id="d5bd0-158">V hello portálu Azure, klikněte na  **+**  tooopen hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-158">In hello Azure portal, click **+** tooopen hello Azure Marketplace.</span></span> <span data-ttu-id="d5bd0-159">Vyhledejte **sadu dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-159">Search for **Availability set**.</span></span>
   - <span data-ttu-id="d5bd0-160">Klikněte na tlačítko **sadu dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-160">Click **Availability set**.</span></span>
   - <span data-ttu-id="d5bd0-161">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-161">Click **Create**.</span></span>
   - <span data-ttu-id="d5bd0-162">Na hello **vytvořit skupinu dostupnosti** okně sadu hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="d5bd0-162">On hello **Create availability set** blade, set hello following values:</span></span>
      - <span data-ttu-id="d5bd0-163">**Název**: název sady dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-163">**Name**: A name for hello availability set.</span></span>
      - <span data-ttu-id="d5bd0-164">**Předplatné**: Azure vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-164">**Subscription**: Your Azure subscription.</span></span>
      - <span data-ttu-id="d5bd0-165">**Skupina prostředků**: Pokud chcete, aby toouse existující skupiny, klikněte na tlačítko **použít existující** a vyberte hello skupinu z rozevíracího seznamu hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-165">**Resource group**: If you want toouse an existing group, click **Use existing** and select hello group from hello drop-down list.</span></span> <span data-ttu-id="d5bd0-166">Jinak vyberte **vytvořit nový** a zadejte název pro skupinu hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-166">Otherwise choose **Create New** and type a name for hello group.</span></span>
      - <span data-ttu-id="d5bd0-167">**Umístění**: nastavit hello umístění, kam budete toocreate virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-167">**Location**: Set hello location where you plan toocreate your virtual machines.</span></span>
      - <span data-ttu-id="d5bd0-168">**Poruch domén**: použijte výchozí hello (3).</span><span class="sxs-lookup"><span data-stu-id="d5bd0-168">**Fault domains**: Use hello default (3).</span></span>
      - <span data-ttu-id="d5bd0-169">**Aktualizovat domén**: použijte výchozí hello (5).</span><span class="sxs-lookup"><span data-stu-id="d5bd0-169">**Update domains**: Use hello default (5).</span></span>
   - <span data-ttu-id="d5bd0-170">Klikněte na tlačítko **vytvořit** skupinu dostupnosti toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-170">Click **Create** toocreate hello availability set.</span></span>

1. <span data-ttu-id="d5bd0-171">Vytvoření hello virtuálních počítačů v nastavení dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-171">Create hello virtual machines in hello availability set.</span></span>

   <span data-ttu-id="d5bd0-172">Zřídit dva virtuální počítače systému SQL Server v sadě Azure dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-172">Provision two SQL Server virtual machines in hello Azure availability set.</span></span> <span data-ttu-id="d5bd0-173">Pokyny najdete v tématu [zřízení virtuálního počítače s SQL Server v hello portál Azure](virtual-machines-windows-portal-sql-server-provision.md).</span><span class="sxs-lookup"><span data-stu-id="d5bd0-173">For instructions, see [Provision a SQL Server virtual machine in hello Azure portal](virtual-machines-windows-portal-sql-server-provision.md).</span></span>

   <span data-ttu-id="d5bd0-174">Umístíte virtuální počítače:</span><span class="sxs-lookup"><span data-stu-id="d5bd0-174">Place both virtual machines:</span></span>

   - <span data-ttu-id="d5bd0-175">V hello stejné skupiny prostředků Azure, které vaše dostupnosti je v.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-175">In hello same Azure resource group that your availability set is in.</span></span>
   - <span data-ttu-id="d5bd0-176">Na hello stejné síti jako řadiče domény.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-176">On hello same network as your domain controller.</span></span>
   - <span data-ttu-id="d5bd0-177">V podsíti s dostatkem místa IP adresu pro virtuální počítače a všech instancích Fci, které může nakonec použijete na tomto clusteru.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-177">On a subnet with sufficient IP address space for both virtual machines, and all FCIs that you may eventually use on this cluster.</span></span>
   - <span data-ttu-id="d5bd0-178">V sadě Azure dostupnosti hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-178">In hello Azure availability set.</span></span>   

      >[!IMPORTANT]
      ><span data-ttu-id="d5bd0-179">Nelze nastavit nebo změnit dostupnosti nastavit po vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-179">You cannot set or change availability set after a virtual machine has been created.</span></span>

   <span data-ttu-id="d5bd0-180">Vyberte bitovou kopii z hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-180">Choose an image from hello Azure Marketplace.</span></span> <span data-ttu-id="d5bd0-181">Můžete použít na trhu bitové kopie s, který zahrnuje Windows Server a SQL Server nebo jenom hello systému Windows Server.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-181">You can use a Marketplace image with that includes Windows Server and SQL Server, or just hello Windows Server.</span></span> <span data-ttu-id="d5bd0-182">Podrobnosti najdete v tématu [přehled systému SQL Server na virtuálních počítačích Azure](../../virtual-machines-windows-sql-server-iaas-overview.md)</span><span class="sxs-lookup"><span data-stu-id="d5bd0-182">For details, see [Overview of SQL Server on Azure Virtual Machines](../../virtual-machines-windows-sql-server-iaas-overview.md)</span></span>

   <span data-ttu-id="d5bd0-183">Hello oficiální imagí SQL serveru v Azure Gallery hello zahrnovat nainstalovaná instance systému SQL Server, plus hello systému SQL Server instalace softwaru a hello vyžadovaný klíč.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-183">hello official SQL Server images in hello Azure Gallery include an installed SQL Server instance, plus hello SQL Server installation software, and hello required key.</span></span>

   <span data-ttu-id="d5bd0-184">Vyberte obrázek vpravo hello podle toohow, které chcete toopay pro licenci na SQL Server hello:</span><span class="sxs-lookup"><span data-stu-id="d5bd0-184">Choose hello right image according toohow you want toopay for hello SQL Server license:</span></span>

   - <span data-ttu-id="d5bd0-185">**Platba za použití licencování**: náklady za minutu hello těchto bitových kopií zahrnuje hello licencování SQL serveru:</span><span class="sxs-lookup"><span data-stu-id="d5bd0-185">**Pay per usage licensing**: hello per-minute cost of these images includes hello SQL Server licensing:</span></span>
      - <span data-ttu-id="d5bd0-186">**SQL Server 2016 Enterprise na Windows Server Datacenter 2016**</span><span class="sxs-lookup"><span data-stu-id="d5bd0-186">**SQL Server 2016 Enterprise on Windows Server Datacenter 2016**</span></span>
      - <span data-ttu-id="d5bd0-187">**SQL Server 2016 Standard na Windows Server Datacenter 2016**</span><span class="sxs-lookup"><span data-stu-id="d5bd0-187">**SQL Server 2016 Standard on Windows Server Datacenter 2016**</span></span>
      - <span data-ttu-id="d5bd0-188">**SQL Server 2016 vývojáře v systému Windows Server Datacenter 2016**</span><span class="sxs-lookup"><span data-stu-id="d5bd0-188">**SQL Server 2016 Developer on Windows Server Datacenter 2016**</span></span>

   - <span data-ttu-id="d5bd0-189">**Převést – vaše – vlastní – licence (BYOL)**</span><span class="sxs-lookup"><span data-stu-id="d5bd0-189">**Bring-your-own-license (BYOL)**</span></span>

      - <span data-ttu-id="d5bd0-190">**{BYOL} SQL Server 2016 Enterprise na Windows Server Datacenter 2016**</span><span class="sxs-lookup"><span data-stu-id="d5bd0-190">**{BYOL} SQL Server 2016 Enterprise on Windows Server Datacenter 2016**</span></span>
      - <span data-ttu-id="d5bd0-191">**{BYOL} SQL Server 2016 Standard na Windows Server Datacenter 2016**</span><span class="sxs-lookup"><span data-stu-id="d5bd0-191">**{BYOL} SQL Server 2016 Standard on Windows Server Datacenter 2016**</span></span>

   >[!IMPORTANT]
   ><span data-ttu-id="d5bd0-192">Po vytvoření virtuálního počítače hello odeberte instanci systému SQL Server předem nainstalovaná samostatné hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-192">After you create hello virtual machine, remove hello pre-installed standalone SQL Server instance.</span></span> <span data-ttu-id="d5bd0-193">Po konfiguraci clusteru převzetí služeb při selhání hello a S2D použijete hello předem nainstalovaná systému SQL Server média toocreate hello SQL Server FCI.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-193">You will use hello pre-installed SQL Server media toocreate hello SQL Server FCI after you configure hello failover cluster and S2D.</span></span>

   <span data-ttu-id="d5bd0-194">Alternativně můžete použít Image Azure Marketplace s právě hello operačního systému.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-194">Alternatively, you can use Azure Marketplace images with just hello operating system.</span></span> <span data-ttu-id="d5bd0-195">Vyberte **Windows Server 2016 Datacenter** bitovou kopii a instalace hello SQL Server FCI po konfiguraci clusteru převzetí služeb při selhání hello a S2D.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-195">Choose a **Windows Server 2016 Datacenter** image and install hello SQL Server FCI after you configure hello failover cluster and S2D.</span></span> <span data-ttu-id="d5bd0-196">Tento image neobsahuje instalačního média systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-196">This image does not contain SQL Server installation media.</span></span> <span data-ttu-id="d5bd0-197">Umístíte do umístění, kde můžete spouštět hello instalace systému SQL Server pro každý server hello instalačního média.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-197">Place hello installation media in a location where you can run hello SQL Server installation for each server.</span></span>

1. <span data-ttu-id="d5bd0-198">Jakmile Azure vytvoří virtuální počítače, připojte pomocí protokolu RDP tooeach virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-198">After Azure creates your virtual machines, connect tooeach virtual machine with RDP.</span></span>

   <span data-ttu-id="d5bd0-199">Když poprvé připojíte tooa virtuálního počítače pomocí protokolu RDP, počítač hello dotáže se tooallow tento počítač toobe zjistitelný v síti hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-199">When you first connect tooa virtual machine with RDP, hello computer asks if you want tooallow this PC toobe discoverable on hello network.</span></span> <span data-ttu-id="d5bd0-200">Klikněte na **Ano**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-200">Click **Yes**.</span></span>

1. <span data-ttu-id="d5bd0-201">Pokud použijete jednu z imagí virtuálního počítače se systémem SQL Server hello, odeberte hello instance systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-201">If you are using one of hello SQL Server-based virtual machine images, remove hello SQL Server instance.</span></span>

   - <span data-ttu-id="d5bd0-202">V **programy a funkce**, klikněte pravým tlačítkem na **Microsoft SQL Server 2016 (64 bitů)** a klikněte na tlačítko **odinstalovat nebo změnit**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-202">In **Programs and Features**, right-click **Microsoft SQL Server 2016 (64-bit)** and click **Uninstall/Change**.</span></span>
   - <span data-ttu-id="d5bd0-203">Klikněte na tlačítko **odebrat**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-203">Click **Remove**.</span></span>
   - <span data-ttu-id="d5bd0-204">Vyberte výchozí instanci hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-204">Select hello default instance.</span></span>
   - <span data-ttu-id="d5bd0-205">Odeberte všechny funkce v části **služby databázového stroje**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-205">Remove all features under **Database Engine Services**.</span></span> <span data-ttu-id="d5bd0-206">Neodebírejte **sdílené součásti**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-206">Do not remove **Shared Features**.</span></span> <span data-ttu-id="d5bd0-207">Viz následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="d5bd0-207">See hello following picture:</span></span>

      ![Odebrat funkce](./media/virtual-machines-windows-portal-sql-create-failover-cluster/03-remove-features.png)

   - <span data-ttu-id="d5bd0-209">Klikněte na tlačítko **Další**a potom klikněte na **odebrat**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-209">Click **Next**, and then click **Remove**.</span></span>

1. <span data-ttu-id="d5bd0-210"><a name="ports"></a>Otevřete porty brány firewall hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-210"><a name="ports"></a>Open hello firewall ports.</span></span>

   <span data-ttu-id="d5bd0-211">Na každém virtuálním počítači otevřete hello následující porty na hello brány Windows Firewall.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-211">On each virtual machine, open hello following ports on hello Windows Firewall.</span></span>

   | <span data-ttu-id="d5bd0-212">Účel</span><span class="sxs-lookup"><span data-stu-id="d5bd0-212">Purpose</span></span> | <span data-ttu-id="d5bd0-213">TCP Port</span><span class="sxs-lookup"><span data-stu-id="d5bd0-213">TCP Port</span></span> | <span data-ttu-id="d5bd0-214">Poznámky</span><span class="sxs-lookup"><span data-stu-id="d5bd0-214">Notes</span></span>
   | ------ | ------ | ------
   | <span data-ttu-id="d5bd0-215">SQL Server</span><span class="sxs-lookup"><span data-stu-id="d5bd0-215">SQL Server</span></span> | <span data-ttu-id="d5bd0-216">1433</span><span class="sxs-lookup"><span data-stu-id="d5bd0-216">1433</span></span> | <span data-ttu-id="d5bd0-217">Normální port pro výchozí instance systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-217">Normal port for default instances of SQL Server.</span></span> <span data-ttu-id="d5bd0-218">Pokud jste použili bitovou kopii z Galerie hello, se automaticky otevře tento port.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-218">If you used an image from hello gallery, this port is automatically opened.</span></span>
   | <span data-ttu-id="d5bd0-219">Test stavu</span><span class="sxs-lookup"><span data-stu-id="d5bd0-219">Health probe</span></span> | <span data-ttu-id="d5bd0-220">59999</span><span class="sxs-lookup"><span data-stu-id="d5bd0-220">59999</span></span> | <span data-ttu-id="d5bd0-221">Žádné otevřete TCP port.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-221">Any open TCP port.</span></span> <span data-ttu-id="d5bd0-222">Později, nakonfigurujte nástroj pro vyrovnávání zatížení hello [test stavu](#probe) a hello clusteru toouse tento port.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-222">In a later step, configure hello load balancer [health probe](#probe) and hello cluster toouse this port.</span></span>  

1. <span data-ttu-id="d5bd0-223">Přidejte úložiště toohello virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-223">Add storage toohello virtual machine.</span></span> <span data-ttu-id="d5bd0-224">Podrobné informace najdete v tématu [přidejte úložiště](../../../storage/common/storage-premium-storage.md).</span><span class="sxs-lookup"><span data-stu-id="d5bd0-224">For detailed information, see [add storage](../../../storage/common/storage-premium-storage.md).</span></span>

   <span data-ttu-id="d5bd0-225">Virtuální počítače nutné aspoň dva datových disků.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-225">Both virtual machines need at least two data disks.</span></span>

   <span data-ttu-id="d5bd0-226">Připojte nezpracovaná disky - není NTFS naformátovaný disky.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-226">Attach raw disks - not NTFS formatted disks.</span></span>
      >[!NOTE]
      ><span data-ttu-id="d5bd0-227">Pokud připojíte naformátovaném systémem souborů NTFS disků, můžete povolit jenom S2D s žádná kontrola způsobilosti disku.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-227">If you attach NTFS-formatted disks, you can only enable S2D with no disk eligibility check.</span></span>  

   <span data-ttu-id="d5bd0-228">Připojte minimálně dvě tooeach Storage úrovně Premium (SSD disky) virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-228">Attach a minimum of two Premium Storage (SSD disks) tooeach VM.</span></span> <span data-ttu-id="d5bd0-229">Doporučujeme alespoň P30 disky (1 TB).</span><span class="sxs-lookup"><span data-stu-id="d5bd0-229">We recommend at least P30 (1 TB) disks.</span></span>

   <span data-ttu-id="d5bd0-230">Sada hostitele ukládání do mezipaměti příliš**jen pro čtení**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-230">Set host caching too**Read-only**.</span></span>

   <span data-ttu-id="d5bd0-231">Hello kapacitu úložiště, které použijete v produkčním prostředí závisí na velikosti pracovní zátěže.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-231">hello storage capacity you use in production environments depends on your workload.</span></span> <span data-ttu-id="d5bd0-232">Hello hodnoty popsané v tomto článku jsou pro demonstrační a testování.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-232">hello values described in this article are for demonstration and testing.</span></span>

1. <span data-ttu-id="d5bd0-233">[Přidat existující doménu tooyour hello virtuálních počítačů](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).</span><span class="sxs-lookup"><span data-stu-id="d5bd0-233">[Add hello virtual machines tooyour pre-existing domain](virtual-machines-windows-portal-sql-availability-group-prereq.md#joinDomain).</span></span>

<span data-ttu-id="d5bd0-234">Po hello virtuální počítače jsou vytvoření a konfiguraci, můžete nakonfigurovat hello převzetí služeb při selhání clusteru.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-234">After hello virtual machines are created and configured, you can configure hello failover cluster.</span></span>

## <a name="step-2-configure-hello-windows-failover-cluster-with-s2d"></a><span data-ttu-id="d5bd0-235">Krok 2: Konfigurace clusteru převzetí služeb při selhání se systémem Windows hello s S2D</span><span class="sxs-lookup"><span data-stu-id="d5bd0-235">Step 2: Configure hello Windows Failover Cluster with S2D</span></span>

<span data-ttu-id="d5bd0-236">dalším krokem Hello je tooconfigure hello převzetí služeb při selhání clusteru s S2D.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-236">hello next step is tooconfigure hello failover cluster with S2D.</span></span> <span data-ttu-id="d5bd0-237">V tomto kroku provedete hello následujících dílčích kroků:</span><span class="sxs-lookup"><span data-stu-id="d5bd0-237">In this step, you will do hello following substeps:</span></span>

1. <span data-ttu-id="d5bd0-238">Přidejte funkci Clustering převzetí služeb při selhání systému Windows</span><span class="sxs-lookup"><span data-stu-id="d5bd0-238">Add Windows Failover Clustering feature</span></span>
1. <span data-ttu-id="d5bd0-239">Ověření clusteru hello</span><span class="sxs-lookup"><span data-stu-id="d5bd0-239">Validate hello cluster</span></span>
1. <span data-ttu-id="d5bd0-240">Vytvoření clusteru převzetí služeb při selhání hello</span><span class="sxs-lookup"><span data-stu-id="d5bd0-240">Create hello failover cluster</span></span>
1. <span data-ttu-id="d5bd0-241">Vytvoření určujícího cloudu hello</span><span class="sxs-lookup"><span data-stu-id="d5bd0-241">Create hello cloud witness</span></span>
1. <span data-ttu-id="d5bd0-242">Přidání úložiště</span><span class="sxs-lookup"><span data-stu-id="d5bd0-242">Add storage</span></span>

### <a name="add-windows-failover-clustering-feature"></a><span data-ttu-id="d5bd0-243">Přidejte funkci Clustering převzetí služeb při selhání systému Windows</span><span class="sxs-lookup"><span data-stu-id="d5bd0-243">Add Windows Failover Clustering feature</span></span>

1. <span data-ttu-id="d5bd0-244">toobegin, toohello prvním virtuálním počítači připojit pomocí protokolu RDP pomocí účtu domény, který je členem místní skupiny administrators a má oprávnění toocreate objekty ve službě Active Directory.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-244">toobegin, connect toohello first virtual machine with RDP using a domain account that is a member of local administrators, and has permissions toocreate objects in Active Directory.</span></span> <span data-ttu-id="d5bd0-245">Tento účet použijte pro hello zbytek hello konfigurace.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-245">Use this account for hello rest of hello configuration.</span></span>

1. <span data-ttu-id="d5bd0-246">[Přidat Clustering převzetí služeb při selhání funkce tooeach virtuální počítač](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).</span><span class="sxs-lookup"><span data-stu-id="d5bd0-246">[Add Failover Clustering feature tooeach virtual machine](virtual-machines-windows-portal-sql-availability-group-prereq.md#add-failover-clustering-features-to-both-sql-server-vms).</span></span>

   <span data-ttu-id="d5bd0-247">funkci Clustering převzetí služeb při selhání tooinstall z hello uživatelského rozhraní, hello následující kroky na virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-247">tooinstall Failover Clustering feature from hello UI, do hello following steps on both virtual machines.</span></span>
   - <span data-ttu-id="d5bd0-248">V **správce serveru**, klikněte na tlačítko **spravovat**a potom klikněte na **přidat role a funkce**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-248">In **Server Manager**, click **Manage**, and then click **Add Roles and Features**.</span></span>
   - <span data-ttu-id="d5bd0-249">V **Průvodce přidáním rolí a funkcí**, klikněte na tlačítko **Další** dokud nezískáte příliš**vybrat funkce**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-249">In **Add Roles and Features Wizard**, click **Next** until you get too**Select Features**.</span></span>
   - <span data-ttu-id="d5bd0-250">V **vybrat funkce**, klikněte na tlačítko **Clustering převzetí služeb při selhání**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-250">In **Select Features**, click **Failover Clustering**.</span></span> <span data-ttu-id="d5bd0-251">Zahrnout všechny požadované funkce a nástroje pro správu hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-251">Include all required features and hello management tools.</span></span> <span data-ttu-id="d5bd0-252">Klikněte na tlačítko **přidat funkce**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-252">Click **Add Features**.</span></span>
   - <span data-ttu-id="d5bd0-253">Klikněte na tlačítko **Další** a pak klikněte na **Dokončit** tooinstall hello funkce.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-253">Click **Next** and then click **Finish** tooinstall hello features.</span></span>

   <span data-ttu-id="d5bd0-254">tooinstall hello převzetí služeb při selhání funkce Clustering s prostředím PowerShell, spusťte následující skript z relace prostředí PowerShell správce na jednom z virtuálních počítačů hello hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-254">tooinstall hello Failover Clustering feature with PowerShell, run hello following script from an administrator PowerShell session on one of hello virtual machines.</span></span>

   ```PowerShell
   $nodes = ("<node1>","<node2>")
   Invoke-Command  $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools}
   ```

<span data-ttu-id="d5bd0-255">Pro referenci hello další kroky, postupujte podle pokynů hello v kroku 3 [konvergované Hyper řešení pomocí prostory úložiště – přímé v systému Windows Server 2016](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-3-configure-storage-spaces-direct).</span><span class="sxs-lookup"><span data-stu-id="d5bd0-255">For reference, hello next steps follow hello instructions under Step 3 of [Hyper-converged solution using Storage Spaces Direct in Windows Server 2016](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-3-configure-storage-spaces-direct).</span></span>

### <a name="validate-hello-cluster"></a><span data-ttu-id="d5bd0-256">Ověření clusteru hello</span><span class="sxs-lookup"><span data-stu-id="d5bd0-256">Validate hello cluster</span></span>

<span data-ttu-id="d5bd0-257">Tato příručka označuje tooinstructions pod [ověření clusteru](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-31-run-cluster-validation).</span><span class="sxs-lookup"><span data-stu-id="d5bd0-257">This guide refers tooinstructions under [validate cluster](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-31-run-cluster-validation).</span></span>

<span data-ttu-id="d5bd0-258">Ověření clusteru hello v hello uživatelského rozhraní nebo pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-258">Validate hello cluster in hello UI or with PowerShell.</span></span>

<span data-ttu-id="d5bd0-259">cluster hello toovalidate s hello uživatelského rozhraní, hello postupem z jednoho hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-259">toovalidate hello cluster with hello UI, do hello following steps from one of hello virtual machines.</span></span>

1. <span data-ttu-id="d5bd0-260">V **správce serveru**, klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Správce clusteru převzetí služeb při selhání**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-260">In **Server Manager**, click **Tools**, then click **Failover Cluster Manager**.</span></span>
1. <span data-ttu-id="d5bd0-261">V **Správce clusteru převzetí služeb při selhání**, klikněte na tlačítko **akce**, pak klikněte na tlačítko **ověřit konfiguraci...** .</span><span class="sxs-lookup"><span data-stu-id="d5bd0-261">In **Failover Cluster Manager**, click **Action**, then click **Validate Configuration...**.</span></span>
1. <span data-ttu-id="d5bd0-262">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-262">Click **Next**.</span></span>
1. <span data-ttu-id="d5bd0-263">Na **vybrat servery nebo Cluster**, název typu hello i virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-263">On **Select Servers or a Cluster**, type hello name of both virtual machines.</span></span>
1. <span data-ttu-id="d5bd0-264">Na **testování možnosti**, zvolte **spustit jenom testy, vyberte**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-264">On **Testing options**, choose **Run only tests I select**.</span></span> <span data-ttu-id="d5bd0-265">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-265">Click **Next**.</span></span>
1. <span data-ttu-id="d5bd0-266">Na **testování výběr**, zahrnout všechny testy s výjimkou **úložiště**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-266">On **Test selection**, include all tests except **Storage**.</span></span> <span data-ttu-id="d5bd0-267">Viz následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="d5bd0-267">See hello following picture:</span></span>

   ![Ověřit testy](./media/virtual-machines-windows-portal-sql-create-failover-cluster/10-validate-cluster-test.png)

1. <span data-ttu-id="d5bd0-269">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-269">Click **Next**.</span></span>
1. <span data-ttu-id="d5bd0-270">Na **potvrzení**, klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-270">On **Confirmation**, click **Next**.</span></span>

<span data-ttu-id="d5bd0-271">Hello **Průvodce ověřením konfigurace** spustí hello testy pro ověření.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-271">hello **Validate a Configuration Wizard** runs hello validation tests.</span></span>

<span data-ttu-id="d5bd0-272">toovalidate hello clusteru pomocí prostředí PowerShell, spusťte následující skript z relace prostředí PowerShell správce na jednom z virtuálních počítačů hello hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-272">toovalidate hello cluster with PowerShell, run hello following script from an administrator PowerShell session on one of hello virtual machines.</span></span>

   ```PowerShell
   Test-Cluster –Node ("<node1>","<node2>") –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
   ```

<span data-ttu-id="d5bd0-273">Po ověření clusteru hello vytvořte cluster převzetí služeb při selhání hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-273">After you validate hello cluster, create hello failover cluster.</span></span>

### <a name="create-hello-failover-cluster"></a><span data-ttu-id="d5bd0-274">Vytvoření clusteru převzetí služeb při selhání hello</span><span class="sxs-lookup"><span data-stu-id="d5bd0-274">Create hello failover cluster</span></span>

<span data-ttu-id="d5bd0-275">Tato příručka označuje příliš[vytvořit hello převzetí služeb při selhání clusteru](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-32-create-a-cluster).</span><span class="sxs-lookup"><span data-stu-id="d5bd0-275">This guide refers too[Create hello failover cluster](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-32-create-a-cluster).</span></span>

<span data-ttu-id="d5bd0-276">toocreate hello převzetí služeb při selhání clusteru, je třeba:</span><span class="sxs-lookup"><span data-stu-id="d5bd0-276">toocreate hello failover cluster, you need:</span></span>
- <span data-ttu-id="d5bd0-277">názvy Hello hello virtuálních počítačů, které se stanou hello uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-277">hello names of hello virtual machines that become hello cluster nodes.</span></span>
- <span data-ttu-id="d5bd0-278">Název pro cluster převzetí služeb při selhání hello</span><span class="sxs-lookup"><span data-stu-id="d5bd0-278">A name for hello failover cluster</span></span>
- <span data-ttu-id="d5bd0-279">IP adresa pro hello převzetí služeb při selhání clusteru.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-279">An IP address for hello failover cluster.</span></span> <span data-ttu-id="d5bd0-280">Můžete použít IP adresu, která se nepoužije na hello stejnou virtuální síť Azure a podsíť jako hello uzly clusteru.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-280">You can use an IP address that is not used on hello same Azure virtual network and subnet as hello cluster nodes.</span></span>

<span data-ttu-id="d5bd0-281">Hello následující prostředí PowerShell vytváří cluster s podporou převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-281">hello following PowerShell creates a failover cluster.</span></span> <span data-ttu-id="d5bd0-282">Aktualizace hello skriptu s názvy hello hello uzlů (hello názvy virtuálních počítačů) a dostupnou IP adresu z hello virtuální sítě Azure:</span><span class="sxs-lookup"><span data-stu-id="d5bd0-282">Update hello script with hello names of hello nodes (hello virtual machine names) and an available IP address from hello Azure VNET:</span></span>

```PowerShell
New-Cluster -Name <FailoverCluster-Name> -Node ("<node1>","<node2>") –StaticAddress <n.n.n.n> -NoStorage
```   

### <a name="create-a-cloud-witness"></a><span data-ttu-id="d5bd0-283">Vytvoření určujícího cloudu</span><span class="sxs-lookup"><span data-stu-id="d5bd0-283">Create a cloud witness</span></span>

<span data-ttu-id="d5bd0-284">Cloud s kopií clusteru je nový typ určující disk kvora clusteru, která je uložená v objektu Blob úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-284">Cloud Witness is a new type of cluster quorum witness stored in an Azure Storage Blob.</span></span> <span data-ttu-id="d5bd0-285">Touto akcí odeberete hello potřebám samostatný virtuální počítač hostování určující sdílenou složku.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-285">This removes hello need of a separate VM hosting a witness share.</span></span>

1. <span data-ttu-id="d5bd0-286">[Vytvoření určujícího cloudu pro cluster převzetí služeb při selhání hello](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).</span><span class="sxs-lookup"><span data-stu-id="d5bd0-286">[Create a cloud witness for hello failover cluster](http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness).</span></span>

1. <span data-ttu-id="d5bd0-287">Vytvořte kontejner objektů blob.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-287">Create a blob container.</span></span>

1. <span data-ttu-id="d5bd0-288">Uložte hello přístupových klíčů a adresy URL kontejneru hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-288">Save hello access keys and hello container URL.</span></span>

1. <span data-ttu-id="d5bd0-289">Nakonfigurujte hello určující disk kvora clusteru převzetí služeb při selhání clusteru.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-289">Configure hello failover cluster cluster quorum witness.</span></span> <span data-ttu-id="d5bd0-290">Viz [Konfigurace hello určující disk kvora v uživatelském rozhraní hello]. (http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness#to-configure-cloud-witness-as-a-quorum-witness) v hello uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-290">See, [Configure hello quorum witness in hello user interface].(http://technet.microsoft.com/windows-server-docs/failover-clustering/deploy-cloud-witness#to-configure-cloud-witness-as-a-quorum-witness) in hello UI.</span></span>

### <a name="add-storage"></a><span data-ttu-id="d5bd0-291">Přidání úložiště</span><span class="sxs-lookup"><span data-stu-id="d5bd0-291">Add storage</span></span>

<span data-ttu-id="d5bd0-292">Hello disky pro S2D potřebovat toobe prázdný a bez oddílů nebo jiná data.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-292">hello disks for S2D need toobe empty and without partitions or other data.</span></span> <span data-ttu-id="d5bd0-293">postupujte podle tooclean disky [hello kroky v této příručce](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-34-clean-disks).</span><span class="sxs-lookup"><span data-stu-id="d5bd0-293">tooclean disks follow [hello steps in this guide](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-34-clean-disks).</span></span>

1. <span data-ttu-id="d5bd0-294">[Prostory úložiště povolení přímé \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-35-enable-storage-spaces-direct).</span><span class="sxs-lookup"><span data-stu-id="d5bd0-294">[Enable Store Spaces Direct \(S2D\)](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-35-enable-storage-spaces-direct).</span></span>

   <span data-ttu-id="d5bd0-295">Hello následující prostředí PowerShell umožňuje prostory úložiště – přímé.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-295">hello following PowerShell enables storage spaces direct.</span></span>  

   ```PowerShell
   Enable-ClusterS2D
   ```

   <span data-ttu-id="d5bd0-296">V **Správce clusteru převzetí služeb při selhání**, se zobrazí hello fondu úložiště.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-296">In **Failover Cluster Manager**, you can now see hello storage pool.</span></span>

1. <span data-ttu-id="d5bd0-297">[Vytvoření svazku](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-36-create-volumes).</span><span class="sxs-lookup"><span data-stu-id="d5bd0-297">[Create a volume](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct#step-36-create-volumes).</span></span>

   <span data-ttu-id="d5bd0-298">Jedním z hello funkce S2D je, že automaticky vytvoří fond úložiště při jeho povolení.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-298">One of hello features of S2D is that it automatically creates a storage pool when you enable it.</span></span> <span data-ttu-id="d5bd0-299">Nyní je připraven toocreate svazku.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-299">You are now ready toocreate a volume.</span></span> <span data-ttu-id="d5bd0-300">Hello prostředí PowerShell `New-Volume` automatizuje proces vytvoření svazku hello, včetně formátování, přidání toohello clusteru a vytvoření sdíleného svazku clusteru (CSV).</span><span class="sxs-lookup"><span data-stu-id="d5bd0-300">hello PowerShell commandlet `New-Volume` automates hello volume creation process, including formatting, adding toohello cluster, and creating a cluster shared volume (CSV).</span></span> <span data-ttu-id="d5bd0-301">Hello následující ukázka vytvoří 800 gigabajt (GB) sdílených svazků clusteru.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-301">hello following example creates an 800 gigabyte (GB) CSV.</span></span>

   ```PowerShell
   New-Volume -StoragePoolFriendlyName S2D* -FriendlyName VDisk01 -FileSystem CSVFS_REFS -Size 800GB
   ```   

   <span data-ttu-id="d5bd0-302">Po dokončení tohoto příkazu je připojen 800 GB svazek jako prostředku clusteru.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-302">After this command completes, an 800 GB volume is mounted as a cluster resource.</span></span> <span data-ttu-id="d5bd0-303">svazek Hello je v `C:\ClusterStorage\Volume1\`.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-303">hello volume is at `C:\ClusterStorage\Volume1\`.</span></span>

   <span data-ttu-id="d5bd0-304">Hello následující diagram znázorňuje sdílený svazek clusteru s S2D:</span><span class="sxs-lookup"><span data-stu-id="d5bd0-304">hello following diagram shows a cluster shared volume with S2D:</span></span>

   ![ClusterSharedVolume](./media/virtual-machines-windows-portal-sql-create-failover-cluster/15-cluster-shared-volume.png)

## <a name="step-3-test-failover-cluster-failover"></a><span data-ttu-id="d5bd0-306">Krok 3: Testování převzetí služeb při selhání clusteru převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="d5bd0-306">Step 3: Test failover cluster failover</span></span>

<span data-ttu-id="d5bd0-307">Ve Správci clusteru převzetí služeb při selhání ověřte, že můžete přesunout toohello prostředků úložiště hello jiného uzlu clusteru.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-307">In Failover Cluster Manager, verify that you can move hello storage resource toohello other cluster node.</span></span> <span data-ttu-id="d5bd0-308">Pokud se můžete připojit cluster převzetí služeb při selhání toohello s **Správce clusteru převzetí služeb při selhání** a přesunout úložiště hello z jednoho uzlu toohello jiné, jsou připravené tooconfigure hello FCI.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-308">If you can connect toohello failover cluster with **Failover Cluster Manager** and move hello storage from one node toohello other, you are ready tooconfigure hello FCI.</span></span>

## <a name="step-4-create-sql-server-fci"></a><span data-ttu-id="d5bd0-309">Krok 4: Vytvoření systému SQL Server FCI</span><span class="sxs-lookup"><span data-stu-id="d5bd0-309">Step 4: Create SQL Server FCI</span></span>

<span data-ttu-id="d5bd0-310">Po nakonfigurování clusteru převzetí služeb při selhání hello a všechny součásti clusteru, včetně úložiště, můžete vytvořit hello SQL Server FCI.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-310">After you have configured hello failover cluster and all cluster components including storage, you can create hello SQL Server FCI.</span></span>

1. <span data-ttu-id="d5bd0-311">Toohello prvním virtuálním počítači připojte pomocí protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-311">Connect toohello first virtual machine with RDP.</span></span>

1. <span data-ttu-id="d5bd0-312">V **Správce clusteru převzetí služeb při selhání**, zkontrolujte, zda jsou všechny základní prostředky clusteru na prvním virtuálním počítači hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-312">In **Failover Cluster Manager**, make sure all cluster core resources are on hello first virtual machine.</span></span> <span data-ttu-id="d5bd0-313">V případě potřeby přesuňte všechny prostředky toothis virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-313">If necessary, move all resources toothis virtual machine.</span></span>

1. <span data-ttu-id="d5bd0-314">Vyhledejte hello instalačního média.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-314">Locate hello installation media.</span></span> <span data-ttu-id="d5bd0-315">Pokud hello virtuální počítač používá jednu z imagí hello Azure Marketplace, se nachází na médium hello `C:\SQLServer_<version number>_Full`.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-315">If hello virtual machine uses one of hello Azure Marketplace images, hello media is located at `C:\SQLServer_<version number>_Full`.</span></span> <span data-ttu-id="d5bd0-316">Klikněte na tlačítko **instalační program**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-316">Click **Setup**.</span></span>

1. <span data-ttu-id="d5bd0-317">V hello **centrum instalace SQL serveru**, klikněte na tlačítko **instalace**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-317">In hello **SQL Server Installation Center**, click **Installation**.</span></span>

1. <span data-ttu-id="d5bd0-318">Klikněte na tlačítko **nová instalace SQL serveru převzetí služeb při selhání clusteru**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-318">Click **New SQL Server failover cluster installation**.</span></span> <span data-ttu-id="d5bd0-319">Postupujte podle pokynů hello hello Průvodce tooinstall hello SQL Server FCI.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-319">Follow hello instructions in hello wizard tooinstall hello SQL Server FCI.</span></span>

   <span data-ttu-id="d5bd0-320">Hello FCI datové adresáře potřebovat toobe na úložiště v clusteru.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-320">hello FCI data directories need toobe on clustered storage.</span></span> <span data-ttu-id="d5bd0-321">S S2D není sdíleného disku, ale svazek přípojného bodu tooa na každém serveru.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-321">With S2D, it's not a shared disk, but a mount point tooa volume on each server.</span></span> <span data-ttu-id="d5bd0-322">S2D synchronizuje hello svazku mezi obou uzlů.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-322">S2D synchronizes hello volume between both nodes.</span></span> <span data-ttu-id="d5bd0-323">svazek Hello je zobrazen toohello clusteru jako sdílený svazek clusteru.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-323">hello volume is presented toohello cluster as a cluster shared volume.</span></span> <span data-ttu-id="d5bd0-324">Použijte hello CSV přípojného bodu pro hello dat adresáře.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-324">Use hello CSV mount point for hello data directories.</span></span>

   ![DataDirectories](./media/virtual-machines-windows-portal-sql-create-failover-cluster/20-data-dicrectories.png)

1. <span data-ttu-id="d5bd0-326">Po dokončení Průvodce hello, instalační program nainstaluje SQL Server FCI hello prvního uzlu.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-326">After you complete hello wizard, Setup will install a SQL Server FCI on hello first node.</span></span>

1. <span data-ttu-id="d5bd0-327">Poté, co instalační program úspěšně nainstaluje hello FCI hello prvního uzlu, připojte pomocí protokolu RDP toohello druhého uzlu.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-327">After Setup successfully installs hello FCI on hello first node, connect toohello second node with RDP.</span></span>

1. <span data-ttu-id="d5bd0-328">Otevřete hello **centrum instalace SQL serveru**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-328">Open hello **SQL Server Installation Center**.</span></span> <span data-ttu-id="d5bd0-329">Klikněte na tlačítko **instalace**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-329">Click **Installation**.</span></span>

1. <span data-ttu-id="d5bd0-330">Klikněte na tlačítko **clusteru převzetí služeb při selhání systému SQL Server se přidat uzly tooa**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-330">Click **Add node tooa SQL Server failover cluster**.</span></span> <span data-ttu-id="d5bd0-331">Postupujte podle pokynů hello v hello Průvodce tooinstall SQL server a přidejte tento server toohello FCI.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-331">Follow hello instructions in hello wizard tooinstall SQL server and add this server toohello FCI.</span></span>

   >[!NOTE]
   ><span data-ttu-id="d5bd0-332">Pokud jste použili bitovou kopii Galerie Azure Marketplace s SQL serverem, byly součástí bitové kopie hello nástroje SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-332">If you used an Azure Marketplace gallery image with SQL Server, SQL Server tools were included with hello image.</span></span> <span data-ttu-id="d5bd0-333">Pokud jste nepoužili tuto bitovou kopii, nainstalujte nástroje SQL Server hello samostatně.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-333">If you did not use this image, install hello SQL Server tools separately.</span></span> <span data-ttu-id="d5bd0-334">V tématu [stáhnout SQL Server Management Studio (SSMS)](http://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="d5bd0-334">See [Download SQL Server Management Studio (SSMS)](http://msdn.microsoft.com/library/mt238290.aspx).</span></span>

## <a name="step-5-create-azure-load-balancer"></a><span data-ttu-id="d5bd0-335">Krok 5: Vytvoření pro vyrovnávání zatížení Azure</span><span class="sxs-lookup"><span data-stu-id="d5bd0-335">Step 5: Create Azure load balancer</span></span>

<span data-ttu-id="d5bd0-336">Na virtuálních počítačích Azure použijte clustery toohold nástroje pro vyrovnávání zatížení IP adresu, která potřebuje toobe na jednom uzlu clusteru současně.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-336">On Azure virtual machines, clusters use a load balancer toohold an IP address that needs toobe on one cluster node at a time.</span></span> <span data-ttu-id="d5bd0-337">V tomto řešení obsahuje nástroj pro vyrovnávání zatížení hello hello IP adresu pro SQL Server FCI hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-337">In this solution, hello load balancer holds hello IP address for hello SQL Server FCI.</span></span>

<span data-ttu-id="d5bd0-338">[Vytvoření a konfigurace pro vyrovnávání zatížení Azure](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="d5bd0-338">[Create and configure an Azure load balancer](virtual-machines-windows-portal-sql-availability-group-tutorial.md#configure-internal-load-balancer).</span></span>

### <a name="create-hello-load-balancer-in-hello-azure-portal"></a><span data-ttu-id="d5bd0-339">Vytvořit nástroj pro vyrovnávání zatížení hello v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="d5bd0-339">Create hello load balancer in hello Azure portal</span></span>

<span data-ttu-id="d5bd0-340">Vyrovnávání zatížení toocreate hello:</span><span class="sxs-lookup"><span data-stu-id="d5bd0-340">toocreate hello load balancer:</span></span>

1. <span data-ttu-id="d5bd0-341">V hello portálu Azure přejděte toohello skupinu prostředků s virtuálními počítači hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-341">In hello Azure portal, go toohello Resource Group with hello virtual machines.</span></span>

1. <span data-ttu-id="d5bd0-342">Klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-342">Click **+ Add**.</span></span> <span data-ttu-id="d5bd0-343">Hledání hello Marketplace pro **nástroj pro vyrovnávání zatížení**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-343">Search hello Marketplace for **Load Balancer**.</span></span> <span data-ttu-id="d5bd0-344">Klikněte na tlačítko **nástroj pro vyrovnávání zatížení**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-344">Click **Load Balancer**.</span></span>

1. <span data-ttu-id="d5bd0-345">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-345">Click **Create**.</span></span>

1. <span data-ttu-id="d5bd0-346">Konfigurace vyrovnávání zatížení hello:</span><span class="sxs-lookup"><span data-stu-id="d5bd0-346">Configure hello load balancer with:</span></span>

   - <span data-ttu-id="d5bd0-347">**Název**: název, který identifikuje hello nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-347">**Name**: A name that identifies hello load balancer.</span></span>
   - <span data-ttu-id="d5bd0-348">**Typ**: nástroj pro vyrovnávání zatížení hello může být veřejné nebo soukromé.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-348">**Type**: hello load balancer can be either public or private.</span></span> <span data-ttu-id="d5bd0-349">Nástroj pro vyrovnávání zatížení privátní je přístupná prostřednictvím hello stejnou virtuální síť.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-349">A private load balancer can be accessed from within hello same VNET.</span></span> <span data-ttu-id="d5bd0-350">Většina Azure aplikace můžete použít nástroj pro vyrovnávání zatížení privátní.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-350">Most Azure applications can use a private load balancer.</span></span> <span data-ttu-id="d5bd0-351">Pokud aplikace potřebuje přístup tooSQL serveru přímo přes hello Internetu, použijte nástroj pro vyrovnávání zatížení veřejné.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-351">If your application needs access tooSQL Server directly over hello Internet, use a public load balancer.</span></span>
   - <span data-ttu-id="d5bd0-352">**Virtuální síť**: hello stejné sítě jako hello virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-352">**Virtual Network**: hello same network as hello virtual machines.</span></span>
   - <span data-ttu-id="d5bd0-353">**Podsíť**: hello stejné podsíti jako hello virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-353">**Subnet**: hello same subnet as hello virtual machines.</span></span>
   - <span data-ttu-id="d5bd0-354">**Privátní IP adresa**: hello stejnou IP adresu, který jste přiřadili toohello SQL Server FCI clusteru síťovému prostředku.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-354">**Private IP address**: hello same IP address that you assigned toohello SQL Server FCI cluster network resource.</span></span>
   - <span data-ttu-id="d5bd0-355">**předplatné**: Azure vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-355">**subscription**: Your Azure subscription.</span></span>
   - <span data-ttu-id="d5bd0-356">**Skupina prostředků**: použití hello stejné skupině prostředků jako virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-356">**Resource Group**: Use hello same resource group as your virtual machines.</span></span>
   - <span data-ttu-id="d5bd0-357">**Umístění**: použití hello stejné umístění jako virtuální počítače Azure.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-357">**Location**: Use hello same Azure location as your virtual machines.</span></span>
   <span data-ttu-id="d5bd0-358">Viz následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="d5bd0-358">See hello following picture:</span></span>

   ![CreateLoadBalancer](./media/virtual-machines-windows-portal-sql-create-failover-cluster/30-load-balancer-create.png)

### <a name="configure-hello-load-balancer-backend-pool"></a><span data-ttu-id="d5bd0-360">Nakonfigurujte fond back-end pro vyrovnávání zatížení hello</span><span class="sxs-lookup"><span data-stu-id="d5bd0-360">Configure hello load balancer backend pool</span></span>

1. <span data-ttu-id="d5bd0-361">Vrátí toohello skupiny prostředků Azure s virtuálními počítači hello a vyhledejte hello nový nástroj pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-361">Return toohello Azure Resource Group with hello virtual machines and locate hello new load balancer.</span></span> <span data-ttu-id="d5bd0-362">Můžete mít toorefresh hello zobrazení na hello skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-362">You may have toorefresh hello view on hello Resource Group.</span></span> <span data-ttu-id="d5bd0-363">Klikněte na nástroj pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-363">Click hello load balancer.</span></span>

1. <span data-ttu-id="d5bd0-364">V okně nástroje pro vyrovnávání zatížení hello, klikněte na tlačítko **back-endové fondy**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-364">On hello load balancer blade, click **Backend pools**.</span></span>

1. <span data-ttu-id="d5bd0-365">Klikněte na tlačítko **+ přidat** tooadd fond back-end.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-365">Click **+ Add** tooadd a backend pool.</span></span>

1. <span data-ttu-id="d5bd0-366">Zadejte název pro fond back-end hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-366">Type a name for hello backend pool.</span></span>

1. <span data-ttu-id="d5bd0-367">Klikněte na tlačítko **přidat virtuální počítač**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-367">Click **Add a virtual machine**.</span></span>

1. <span data-ttu-id="d5bd0-368">Na hello **vyberte virtuální počítače** okně klikněte na tlačítko **zvolit skupinu dostupnosti**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-368">On hello **Choose virtual machines** blade, click **Choose an availability set**.</span></span>

1. <span data-ttu-id="d5bd0-369">Vyberte sadu dostupnosti hello, že jste umístili virtuální počítače systému SQL Server hello v.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-369">Choose hello availability set that you placed hello SQL Server virtual machines in.</span></span>

1. <span data-ttu-id="d5bd0-370">Na hello **vyberte virtuální počítače** okně klikněte na tlačítko **zvolte hello virtuálních počítačů**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-370">On hello **Choose virtual machines** blade, click **Choose hello virtual machines**.</span></span>

   <span data-ttu-id="d5bd0-371">Portálu Azure by měl vypadat podobně jako hello následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="d5bd0-371">Your Azure portal should look like hello following picture:</span></span>

   ![CreateLoadBalancerBackEnd](./media/virtual-machines-windows-portal-sql-create-failover-cluster/33-load-balancer-back-end.png)

1. <span data-ttu-id="d5bd0-373">Klikněte na tlačítko **vyberte** na hello **vyberte virtuální počítače** okno.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-373">Click **Select** on hello **Choose virtual machines** blade.</span></span>

1. <span data-ttu-id="d5bd0-374">Klikněte na tlačítko **OK** dvakrát.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-374">Click **OK** twice.</span></span>

### <a name="configure-a-load-balancer-health-probe"></a><span data-ttu-id="d5bd0-375">Konfigurace stavu sondu nástroje pro vyrovnávání zatížení.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-375">Configure a load balancer health probe</span></span>

1. <span data-ttu-id="d5bd0-376">V okně nástroje pro vyrovnávání zatížení hello, klikněte na tlačítko **testy stavu**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-376">On hello load balancer blade, click **Health probes**.</span></span>

1. <span data-ttu-id="d5bd0-377">Klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-377">Click **+ Add**.</span></span>

1. <span data-ttu-id="d5bd0-378">Na hello **test stavu přidat** okně <a name="probe"> </a>nastavit parametry testu stavu hello:</span><span class="sxs-lookup"><span data-stu-id="d5bd0-378">On hello **Add health probe** blade, <a name="probe"></a>Set hello health probe parameters:</span></span>

   - <span data-ttu-id="d5bd0-379">**Název**: název pro test stavu hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-379">**Name**: A name for hello health probe.</span></span>
   - <span data-ttu-id="d5bd0-380">**Protokol**: TCP.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-380">**Protocol**: TCP.</span></span>
   - <span data-ttu-id="d5bd0-381">**Port**: nastavte tooan dostupný port TCP.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-381">**Port**: Set tooan available TCP port.</span></span> <span data-ttu-id="d5bd0-382">Tento port vyžaduje k portu brány firewall otevřít.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-382">This port requires an open firewall port.</span></span> <span data-ttu-id="d5bd0-383">Použití hello [stejný port](#ports) nastavení pro test stavu hello v hello brány firewall.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-383">Use hello [same port](#ports) you set for hello health probe at hello firewall.</span></span>
   - <span data-ttu-id="d5bd0-384">**Interval**: 5 sekund.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-384">**Interval**: 5 Seconds.</span></span>
   - <span data-ttu-id="d5bd0-385">**Prahová hodnota špatného stavu**: 2 po sobě jdoucích selhání.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-385">**Unhealthy threshold**: 2 consecutive failures.</span></span>

1. <span data-ttu-id="d5bd0-386">Klikněte na tlačítko OK.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-386">Click OK.</span></span>

### <a name="set-load-balancing-rules"></a><span data-ttu-id="d5bd0-387">Nastavit pravidla Vyrovnávání zatížení</span><span class="sxs-lookup"><span data-stu-id="d5bd0-387">Set load balancing rules</span></span>

1. <span data-ttu-id="d5bd0-388">V okně nástroje pro vyrovnávání zatížení hello, klikněte na tlačítko **pravidla Vyrovnávání zatížení**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-388">On hello load balancer blade, click **Load balancing rules**.</span></span>

1. <span data-ttu-id="d5bd0-389">Klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-389">Click **+ Add**.</span></span>

1. <span data-ttu-id="d5bd0-390">Nastavte parametry pravidla vyrovnávání zátěže hello:</span><span class="sxs-lookup"><span data-stu-id="d5bd0-390">Set hello load balancing rules parameters:</span></span>

   - <span data-ttu-id="d5bd0-391">**Název**: název pravidla Vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-391">**Name**: A name for hello load balancing rules.</span></span>
   - <span data-ttu-id="d5bd0-392">**Adresa IP front-endu**: používat hello IP adresu pro hello SQL Server FCI clusteru síťovému prostředku.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-392">**Frontend IP address**: Use hello IP address for hello SQL Server FCI cluster network resource.</span></span>
   - <span data-ttu-id="d5bd0-393">**Port**: nastavení pro hello port TCP systému SQL Server FCI.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-393">**Port**: Set for hello SQL Server FCI TCP port.</span></span> <span data-ttu-id="d5bd0-394">Hello výchozí instance port je 1433.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-394">hello default instance port is 1433.</span></span>
   - <span data-ttu-id="d5bd0-395">**Back-endový port**: Tato hodnota používá hello stejný port jako hello **Port** hodnotu, pokud povolíte **plovoucí IP adresa (přímá odpověď ze serveru)**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-395">**Backend port**: This value uses hello same port as hello **Port** value when you enable **Floating IP (direct server return)**.</span></span>
   - <span data-ttu-id="d5bd0-396">**Fond back-end**: použití hello back-end fondu název, který jste dříve nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-396">**Backend pool**: Use hello backend pool name that you configured earlier.</span></span>
   - <span data-ttu-id="d5bd0-397">**Test stavu**: Test stavu hello použití, který jste nakonfigurovali dříve.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-397">**Health probe**: Use hello health probe that you configured earlier.</span></span>
   - <span data-ttu-id="d5bd0-398">**Trvalost relace**: žádné.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-398">**Session persistence**: None.</span></span>
   - <span data-ttu-id="d5bd0-399">**Časový limit (v minutách) nečinnosti**: 4.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-399">**Idle timeout (minutes)**: 4.</span></span>
   - <span data-ttu-id="d5bd0-400">**Plovoucí IP adresa (přímá odpověď ze serveru)**: povoleno</span><span class="sxs-lookup"><span data-stu-id="d5bd0-400">**Floating IP (direct server return)**: Enabled</span></span>

1. <span data-ttu-id="d5bd0-401">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-401">Click **OK**.</span></span>

## <a name="step-6-configure-cluster-for-probe"></a><span data-ttu-id="d5bd0-402">Krok 6: Konfigurace clusteru pro test paměti</span><span class="sxs-lookup"><span data-stu-id="d5bd0-402">Step 6: Configure cluster for probe</span></span>

<span data-ttu-id="d5bd0-403">Nastavit parametr port testu clusteru hello v prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-403">Set hello cluster probe port parameter in PowerShell.</span></span>

<span data-ttu-id="d5bd0-404">tooset hello parametr port testu clusteru, aktualizujte proměnné ve hello následující skript z prostředí.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-404">tooset hello cluster probe port parameter, update variables in hello following script from your environment.</span></span>

  ```PowerShell
   $ClusterNetworkName = "<Cluster Network Name>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name).
   $IPResourceName = "IP Address Resource Name" # hello IP Address cluster resource name.
   $ILBIP = "<10.0.0.x>" # hello IP Address of hello Internal Load Balancer (ILB). This is hello static IP address for hello load balancer you configured in hello Azure portal.
   [int]$ProbePort = <59999>

   Import-Module FailoverClusters

   Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```


## <a name="step-7-test-fci-failover"></a><span data-ttu-id="d5bd0-405">Krok 7: Testování převzetí služeb při selhání FCI</span><span class="sxs-lookup"><span data-stu-id="d5bd0-405">Step 7: Test FCI failover</span></span>

<span data-ttu-id="d5bd0-406">Testovací převzetí služeb při selhání hello fungování clusteru toovalidate FCI.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-406">Test failover of hello FCI toovalidate cluster functionality.</span></span> <span data-ttu-id="d5bd0-407">Hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="d5bd0-407">Do hello following steps:</span></span>

1. <span data-ttu-id="d5bd0-408">Tooone uzlů clusteru SQL serveru FCI hello připojte pomocí protokolu RDP.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-408">Connect tooone of hello SQL Server FCI cluster nodes with RDP.</span></span>

1. <span data-ttu-id="d5bd0-409">Otevřete **Správce clusteru převzetí služeb při selhání**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-409">Open **Failover Cluster Manager**.</span></span> <span data-ttu-id="d5bd0-410">Klikněte na tlačítko **role**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-410">Click **Roles**.</span></span> <span data-ttu-id="d5bd0-411">Všimněte si, který uzel vlastní role SQL serveru FCI hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-411">Notice which node owns hello SQL Server FCI role.</span></span>

1. <span data-ttu-id="d5bd0-412">Klikněte pravým tlačítkem na roli SQL serveru FCI hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-412">Right-click hello SQL Server FCI role.</span></span>

1. <span data-ttu-id="d5bd0-413">Klikněte na tlačítko **přesunout** a klikněte na tlačítko **nejvhodnějšího uzlu**.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-413">Click **Move** and click **Best Possible Node**.</span></span>

<span data-ttu-id="d5bd0-414">**Správce clusteru převzetí služeb při selhání** ukazuje hello role a její prostředky přejít do režimu offline.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-414">**Failover Cluster Manager** shows hello role and its resources go offline.</span></span> <span data-ttu-id="d5bd0-415">Hello prostředky pak přesunout a uvést do režimu online na hello jiného uzlu.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-415">hello resources then move and come online on hello other node.</span></span>

### <a name="test-connectivity"></a><span data-ttu-id="d5bd0-416">Test připojení</span><span class="sxs-lookup"><span data-stu-id="d5bd0-416">Test connectivity</span></span>

<span data-ttu-id="d5bd0-417">připojení k tootest, přihlášení tooanother virtuálního počítače v hello stejné virtuální síti.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-417">tootest connectivity, log in tooanother virtual machine in hello same virtual network.</span></span> <span data-ttu-id="d5bd0-418">Otevřete **SQL Server Management Studio** a připojte se název SQL serveru FCI toohello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-418">Open **SQL Server Management Studio** and connect toohello SQL Server FCI name.</span></span>

>[!NOTE]
><span data-ttu-id="d5bd0-419">Pokud třeba, můžete [stáhnout SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="d5bd0-419">If necessary, you can [download SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span></span>

## <a name="limitations"></a><span data-ttu-id="d5bd0-420">Omezení</span><span class="sxs-lookup"><span data-stu-id="d5bd0-420">Limitations</span></span>
<span data-ttu-id="d5bd0-421">Na virtuálních počítačích Azure Microsoft koordinátoru DTC (Distributed Transaction) není podporována v instancích Fci protože hello RPC port není podporována pro vyrovnávání zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="d5bd0-421">On Azure virtual machines, Microsoft Distributed Transaction Coordinator (DTC) is not supported on FCIs because hello RPC port is not supported by hello load balancer.</span></span>

## <a name="see-also"></a><span data-ttu-id="d5bd0-422">Viz také</span><span class="sxs-lookup"><span data-stu-id="d5bd0-422">See Also</span></span>

[<span data-ttu-id="d5bd0-423">Instalační program S2D pomocí vzdálené plochy (Azure)</span><span class="sxs-lookup"><span data-stu-id="d5bd0-423">Setup S2D with remote desktop (Azure)</span></span>](http://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-storage-spaces-direct-deployment)

<span data-ttu-id="d5bd0-424">[Konvergované Hyper řešení s prostory úložiště direct](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct).</span><span class="sxs-lookup"><span data-stu-id="d5bd0-424">[Hyper-converged solution with storage spaces direct](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct).</span></span>

[<span data-ttu-id="d5bd0-425">Prostory úložiště – přímé přehled</span><span class="sxs-lookup"><span data-stu-id="d5bd0-425">Storage Space Direct Overview</span></span>](http://technet.microsoft.com/windows-server-docs/storage/storage-spaces/storage-spaces-direct-overview)

[<span data-ttu-id="d5bd0-426">Podpora systému SQL Server pro S2D</span><span class="sxs-lookup"><span data-stu-id="d5bd0-426">SQL Server support for S2D</span></span>](https://blogs.technet.microsoft.com/dataplatforminsider/2016/09/27/sql-server-2016-now-supports-windows-server-2016-storage-spaces-direct/)
